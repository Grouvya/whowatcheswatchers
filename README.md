# 🛡️ Who Watches Watchers? (Antigravity Shield)

*An advanced, system-wide Anonymity Engine, Transparent Proxy, and Fingerprint Shield designed for extreme privacy.*

**Created with ❤ by Grouvya (ofinilet)**

**Who Watches Watchers?** is an all-in-one privacy framework. Unlike standard Tor browsers that only protect web traffic within a single window, this application implements a **system-wide transparent proxy**. It forces 100% of your operating system's network traffic through the Tor network, violently spoofs your hardware footprint, and intercepts tracking APIs to make your machine completely untraceable.

---

## 🌟 Core Architecture & Functionality

### 1. System-Wide Transparent Proxy (nftables)
- **100% Traffic Redirection:** The engine dynamically compiles and applies `nftables` rules to intercept **all** outbound TCP and UDP traffic across your entire OS, forcing it through Tor's `TransPort` (Port 9040) and `DNSPort` (Port 9053). 
- **IPv6 Blackholing:** IPv6 traffic is completely dropped at the kernel level (`net.ipv6.conf.all.disable_ipv6=1` and `nftables` reject rules) to prevent IP leakage, which is a common vulnerability in standard VPN setups.
- **Air-Gapped Kill Switch:** A background Python thread constantly monitors the health of the Tor daemon (`systemctl is-active tor`). If Tor crashes, stalls, or drops the connection, the Kill Switch instantly triggers. It leaves the strict firewall rules in place, effectively air-gapping the system and preventing any clearnet traffic from leaking out.

### 2. Advanced Snowflake Integration (Anti-Censorship)
- Instead of connecting directly to Tor entry guards (which are easily identifiable and blocked by authoritarian regimes or strict corporate firewalls), the engine connects via the **Snowflake** client plugin.
- Snowflake disguises your Tor traffic as standard WebRTC video-calling traffic, bouncing it through a constantly shifting pool of temporary volunteer proxies. This easily defeats Deep Packet Inspection (DPI).

### 3. Exhaustive Fingerprint Shield (Mitmproxy)
This is the crown jewel of the anonymity engine. The shield deploys an invisible Man-in-the-Middle (MITM) proxy using `mitmdump`. The firewall dynamically redirects all HTTP/HTTPS traffic (Ports 80 & 443) to this proxy before it enters the Tor network. The proxy injects a massive, highly sophisticated JavaScript payload into every webpage you visit.

**Intercepted & Spoofed APIs (57+ vectors covered):**
- **Canvas & WebGL:** Spoofs `HTMLCanvasElement.getContext`, `toDataURL`, `getImageData`, and adds microscopic noise to image rendering. WebGL vendor strings, shader precision (`getShaderPrecisionFormat`), and supported extensions are randomized to mask your real GPU hardware.
- **AudioContext & Formats:** Randomizes oscillator wave generation and spoof checks for obscure media formats (`flac`, `vp9`) via `HTMLMediaElement.canPlayType`.
- **Hardware Sensors & Battery:** Intercepts `navigator.getBattery()` with a fake, static Promise. `DeviceMotionEvent` and `DeviceOrientationEvent` are silenced to prevent physical gyroscope profiling.
- **Media Devices & Network:** `navigator.mediaDevices.enumerateDevices` returns generic, fake hardware. `navigator.connection` is spoofed to report randomized 4G/WiFi data.
- **Navigator & Timezone:** User Agent, Platform, Device Memory, Hardware Concurrency, and Timezone offsets are overridden and held static per session to prevent rapid-fire detection.

### 4. Hardware MAC Spoofing & Persistence
- Automatically randomizes the MAC addresses of all physical and wireless network interfaces (e.g., `eth0`, `wlan0`) using `macchanger` before any network connection is established.
- **Raw PCIe Bus Reset:** Standard MAC spoofing often leaves Wi-Fi cards in a "zombie" state where they refuse to authenticate with routers. This engine dynamically locates the PCI address of your network cards and triggers a hardware-level bus reset via `sysfs` (`/sys/bus/pci/devices/.../reset`), ensuring the spoofed MAC persists flawlessly.

### 5. DNS Blackholing & Traffic Obfuscation
- **StevenBlack Hosts:** Native system-level ad, tracking, and malware domain blocking via the comprehensive StevenBlack host list.
- **Decoy Traffic Generator:** A background bash daemon occasionally curls random popular Wikipedia and news articles through a hidden SOCKS5 proxy (`127.0.0.1:9050`). This adds noise to your traffic pattern, disrupting ISP traffic analysis and preventing Tor circuits from going Dormant.

---

## 🛠️ Installation & Setup

### Prerequisites
- A Debian-based Linux distribution (Parrot OS, Kali Linux, Ubuntu, Debian).
- `sudo` privileges (Root access is strictly required to modify kernel parameters and `nftables`).

### Quick Install
1. Open a terminal in the directory containing the installer.
2. Run the automated deployment script:
   ```bash
   sudo ./install.sh
   ```
3. The script will automatically install Tor, python3-stem, mitmproxy, generate local CA Certificates, and extract the GUI dashboard.

---

## 🚀 Using the Dashboard

Launch the application globally by opening a terminal and running:
```bash
sudo anonshield
```

**Dashboard Controls:**
- **Start Shield:** Begins the sequence: MAC Spoofing -> PCIe Reset -> Tor Bootstrap -> Fingerprint Proxy Initialization -> Firewall Lockdown.
- **Stop Shield:** Carefully dismantles the firewall, restores your original MAC addresses, and reverts your DNS settings to their default clearnet configuration.
- **Rotate Identity:** Sends a `NEWNYM` signal to the Tor control port, forcing Tor to shred the current circuit and issue you a brand new external IP address.
- **Panic Wipe:** An emergency button that instantly shreds memory caches, drops all network connections, and leaves the firewall in a permanent lockdown state until you reboot the system.

---

## 💻 Virtual Machine (VM) specific guidelines

If you are deploying "Who Watches Watchers?" inside a Virtual Machine (VirtualBox, VMware, KVM/QEMU), the hypervisor's virtual networking stack introduces complications. **You must follow these rules:**

1. **MAC Spoofing Restrictions (Crucial):**
   - Hypervisors usually enforce strict MAC address policies to prevent ARP spoofing on the host network.
   - **VirtualBox:** You must go to VM Settings -> Network -> Advanced -> **Promiscuous Mode**, and set it to **Allow All**. If you do not do this, MAC spoofing will succeed in Linux, but the hypervisor will silently drop all packets leaving the VM, resulting in zero internet connectivity.
   - **VMware:** You may need to edit your virtual switch security settings to allow "MAC Address Changes" and "Forged Transmits".

2. **Network Adapter Types:**
   - **NAT (Network Address Translation):** This is the **recommended** mode. The VM acts as an isolated sandbox. The application forces traffic into Tor, and the host machine only sees encrypted Tor traffic.
   - **Bridged Adapter:** Exposes the VM directly to your local router. MAC spoofing is highly effective here, but hypervisor Promiscuous Mode is absolutely required.

3. **Time Synchronization Leaks:**
   - The application intentionally disables `systemd-timesyncd` because hyper-accurate system clocks can be used for timing attacks.
   - However, VM software (like VMware Tools or VirtualBox Guest Additions) often forces the VM clock to sync with the host OS in the background. **Disable Host-to-Guest time synchronization** in your hypervisor settings to maximize anonymity.

---

## 🗑️ Complete Uninstallation

The application is designed to be cleanly removable without permanently damaging your network configuration. 

To completely uninstall the proxy, restore your original DNS resolvers, revert all `nftables` firewall rules, delete the CA certificates, and remove all background decoy services, execute:

```bash
sudo ./install.sh --uninstall
```

*Note: You do not need to manually fix your network after uninstallation; the `--uninstall` flag handles the complete rollback of all modified kernel and system parameters.*
