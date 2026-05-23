# 🛡️ Who Watches Watchers? (AnonShield)

**Who Watches Watchers?** (also referred to as *AnonShield*) is an advanced, automated anonymity and privacy-enhancing framework for Linux systems. It acts as a transparent proxy, securely routing all operating system traffic through the Tor network, while seamlessly offering multiple layers of security to thwart tracking, telemetry, and fingerprinting.

---

## ✨ Key Features

1. **Transparent Tor Proxying:** All system TCP traffic is transparently routed through the Tor network. DNS requests are securely resolved via Tor's DNS port, preventing DNS leaks. IPv6 is forcefully disabled to prevent Deanonymization leaks.
2. **🎭 Fingerprint Shield (System-Wide JS Interceptor):** A built-in Man-in-the-Middle (MITM) proxy seamlessly intercepts HTTP/HTTPS traffic and injects anti-tracking JavaScript into every webpage. This completely masks your browser footprint (Canvas, WebGL, Audio Context, Screen Resolution, User-Agent, and Timezone) for *any* browser on the system.
3. **📦 Ephemeral microVM Sandbox:** Run untrusted applications or malware inside a lightweight, highly restrictive QEMU-based Alpine Linux microVM. The VM is ephemeral (wiped on exit) and routed exclusively through Tor.
4. **🔓 Split Tunneling (Bypass Tor):** Sometimes you need to access clear-net services (like local banking or services that block Tor). The Split Tunnel feature allows you to launch specific applications outside the Tor network securely.
5. **🔄 Automatic MAC Address Spoofing:** Spoofs the MAC addresses of your network interfaces upon connection and supports automatic periodic rotation to prevent physical tracking across networks.
6. **🔒 System Hardening & Security:**
   - **Kill Switch:** Immediately cuts off all network connectivity if the Tor daemon crashes.
   - **DNS Blackholing:** Blocks telemetry, ads, and known tracking domains at the system DNS level.
   - **Single-Use Encrypted Swap:** Re-encrypts your swap partition with a one-time random AES key on startup.
   - **USBGuard:** Blocks rogue USB devices (BadUSB attacks) while the shield is active.

---

## 🚀 Installation

The application comes bundled into a single installation script. It supports Debian/Ubuntu (`apt`), Fedora (`dnf`), and Arch Linux (`pacman`).

```bash
# Clone the repository or download install.sh
chmod +x install.sh

# Run the installer with root privileges
sudo ./install.sh
```

The script will:
- Install all necessary dependencies (Tor, QEMU, mitmproxy, nftables, etc.).
- Set up a dedicated Python virtual environment.
- Create Desktop shortcuts and GUI launchers.
- Generate the microVM Alpine ISO.

---

## 💻 Usage

You can control *Who Watches Watchers?* using either the graphical interface (GUI) or the command-line interface (CLI).

### 🖥️ Graphical User Interface (GUI)
Simply search for **AnonShield** in your application menu, or double-click the **AnonShield** icon on your Desktop. 
The GUI provides easy toggles to:
- Start/Stop the Anonymity Engine
- Request a New Identity (Rotate Tor IP)
- Enable/Disable the **Fingerprint Shield**
- Launch an application bypassing Tor
- Launch the Ephemeral microVM

### ⌨️ Command Line Interface (CLI)
You can interact with the daemon directly from the terminal. Note: Most commands require `sudo`.

```bash
# Start the Anonymity Engine (Transparent Proxy)
sudo anonshield start

# Stop the Anonymity Engine and restore default settings
sudo anonshield stop

# Check current connection status and public IP
anonshield status

# Request a new Tor circuit (New IP)
sudo anonshield newid
```

### 🎭 Fingerprint Shield
The Fingerprint Shield randomizes your digital footprint.
```bash
sudo anonshield fingerprint start
sudo anonshield fingerprint stop
anonshield fingerprint status
```

### 📦 Sandbox & Bypass
```bash
# Launch a command inside the ephemeral microVM
sudo anonshield sandbox <command>

# Launch a command completely bypassing Tor (Clear-net)
sudo anonshield bypass <command>
```

### 🔄 MAC Spoofing
```bash
# Randomize a specific interface
sudo anonshield mac wlan0 random

# Restore permanent MAC address
sudo anonshield mac wlan0 reset
```

---

## 🛑 Uninstallation

If you wish to completely remove the application and restore all system configurations:

```bash
sudo ./install.sh uninstall
```

This will automatically flush all firewall rules, stop all proxies, delete certificates, remove system groups, and clean up the application directories.

---

## ⚠️ Legal Disclaimer

*Who Watches Watchers?* is developed solely for educational and privacy-enhancing purposes. The developers hold no responsibility for any misuse, illegal activities, or damages caused by the use of this software. Always comply with the laws of your jurisdiction.
