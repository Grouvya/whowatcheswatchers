# 🛡️ Who Watches Watchers? (Antigravity Shield)

*An advanced, system-wide Anonymity Engine and Fingerprint Shield designed for extreme privacy.*

**Who Watches Watchers?** is an all-in-one transparent proxy and system lockdown tool. It forces 100% of your operating system's network traffic through the Tor network, spoofs your hardware footprint, and intercepts browser tracking APIs to make your machine indistinguishable from millions of others.

---

## 🌟 Key Functionality

1. **System-Wide Transparent Proxy**
   - Automatically intercepts **all** outbound TCP/UDP traffic and forces it through the Tor network (`TransPort` and `DNSPort`).
   - Completely blocks IPv6 to prevent DNS and IP leaks.
   - Includes a strict **Kill Switch**: If Tor drops or crashes, the firewall locks down the machine, air-gapping the OS until Tor reconnects.

2. **Advanced Snowflake Integration**
   - Connects to Tor via the **Snowflake** anti-censorship bridge, disguising Tor traffic as standard WebRTC video-calling traffic to bypass Deep Packet Inspection (DPI) and strict firewalls.

3. **Exhaustive Fingerprint Shield (Mitmproxy)**
   - Deploys an invisible Man-in-the-Middle (MITM) proxy to inject a massive JavaScript anti-fingerprinting payload into every unencrypted webpage.
   - Spoofs **57+ advanced browser tracking APIs**, including Canvas, WebGL, Audio Oscillators, Battery API, Sensors (Gyroscope), Media Formats, Timezone, User Agent, and more.
   - Bypasses advanced trackers (like AmiUnique) by returning plausible, randomized data instead of your real hardware telemetry.

4. **Hardware MAC Spoofing & PCIe Resets**
   - Automatically randomizes your network interface MAC addresses before connecting to any network.
   - Performs a **Raw PCIe Bus Reset** on network cards to clear zombie states and ensure MAC changes persist safely across router reconnections.
   - Supports automated periodic MAC rotation.

5. **Decoy Traffic Generator**
   - Automatically generates randomized, harmless background web traffic through Tor to throw off ISP traffic analysis and prevent Tor circuits from going into Dormant mode.

6. **DNS Blackholing & Anti-Telemetry**
   - Implements the StevenBlack hosts file to natively sinkhole telemetry, tracking, and ad domains at the system level.

---

## 🛠️ Installation

### Prerequisites
- Debian-based OS (Parrot OS, Kali, Ubuntu, Debian)
- `sudo` privileges

### Setup Instructions
1. Navigate to the directory containing the installer script.
2. Run the automated installer:
   ```bash
   sudo ./install.sh
   ```
3. The script will automatically resolve dependencies, install Tor, generate local Certificates for the Fingerprint Shield, and configure the firewall.
4. Once completed, a graphical dashboard (`anonshield`) will be available.

---

## 🚀 Usage

You can launch the dashboard anywhere by opening a terminal and typing:
```bash
sudo anonshield
```
From the GUI, you can:
- **Start Shield:** Bootstraps Tor, spoofs MACs, and applies the transparent proxy firewall.
- **Stop Shield:** Removes the firewall, un-spoofs MACs, and restores direct internet access.
- **Rotate Identity:** Forces Tor to build a brand new circuit, changing your external IP.
- **Panic Button:** Immediately shreds the cache and memory, and violently kills network interfaces in case of emergency.

---

## 💻 Using in a Virtual Machine (VM)

If you are running Who Watches Watchers? inside a Virtual Machine (like VirtualBox, VMware, or KVM), you must be aware of the following network configurations:

1. **MAC Spoofing Restrictions:**
   - Hypervisors often strictly enforce the MAC addresses of virtual adapters. 
   - **VirtualBox:** Go to Settings -> Network -> Advanced -> **Promiscuous Mode**, and set it to **Allow All**. If this is not set, MAC spoofing will disconnect you from the internet because the hypervisor will drop packets from the spoofed MAC.
   - **VMware:** You may need to edit the `.vmx` file or adjust vSwitch security policies to allow MAC address changes, otherwise the virtual adapter will silently drop traffic.

2. **Network Adapter Types:**
   - **NAT (Recommended):** Uses the host machine's connection securely. The VM's traffic is forced through Tor *before* leaving the VM.
   - **Bridged:** Gives the VM its own IP on the local network. MAC spoofing is more effective here but requires Promiscuous Mode enabled.

3. **Time Sync:**
   - The script disables system time-sync while active to prevent timing attacks. VM hypervisors sometimes forcefully sync the guest clock with the host. Ensure "Time Synchronization" via VMware Tools / VBox Guest Additions is **disabled** while the shield is active for maximum anonymity.

---

## 🗑️ Uninstallation

If you wish to completely remove Who Watches Watchers?, restore your original DNS, revert all firewall configurations, and delete the background services, simply run:

```bash
sudo ./install.sh --uninstall
```

This will safely roll back the entire system to its default state and delete the application.
