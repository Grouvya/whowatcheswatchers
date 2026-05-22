# 🛡️ Who Watches Watchers?

**Who Watches Watchers?** is an advanced, premium system-wide privacy suite that routes your entire Linux operating system's TCP and DNS traffic through the Tor anonymity network. It features a stunning Dark Mode dashboard, real-time circuit visualization, network fail-safes, and 1-click Dark Web hosting.

## 🚀 Key Features

*   **System-Wide Anonymity:** Transparently forces all system TCP and DNS traffic through Tor using advanced `nftables` firewall rules, preventing IP leaks and bypassing local network surveillance.
*   **Stunning Graphical Dashboard:** A beautifully designed, modern dashboard that displays your live bandwidth, active Tor circuits, spoofed MAC hardware addresses, and connection status in real-time.
*   **Network Kill Switch (Fail-Close):** A background daemon constantly monitors your connection. If Tor crashes or drops unexpectedly, the firewall instantly airgaps your machine, guaranteeing your true IP is never leaked to the Clearnet.
*   **Split Tunneling (Bypass):** Need to access a local device or play a latency-sensitive game? Use the built-in Split Tunneling to launch specific applications directly over the Clearnet while the rest of your system remains shielded.
*   **1-Click Hidden Services:** Instantly map any local port on your machine to a globally accessible Dark Web `.onion` domain with the press of a single button.
*   **Automated MAC Spoofing:** Periodically rotates your physical hardware MAC address to evade local network tracking and WiFi profiling.

## 📦 Installation

**Who Watches Watchers?** is distributed as a highly portable, self-extracting payload.

1. Download the `install.sh` file.
2. Open a terminal and grant it execute permissions: `chmod +x install.sh`
3. Run the automated setup: `sudo ./install.sh`

The installer will automatically detect your Linux distribution, install all required dependencies (`tor`, `macchanger`, `nftables`, etc.), unpack the core binaries, configure the firewall, and deploy a system-wide application shortcut to your Start Menu!

## 🖥️ Usage

You can manage the anonymizer using the **Command Line** or the **Graphical Interface**.

**Graphical Interface:**
Search for "Who Watches Watchers?" in your Start Menu / App Grid, or launch it from the terminal using:
```bash
pkexec anonshield-gui
```
*(The GUI requires your administrative password to securely manipulate system firewall rules)*

**Command Line:**
```bash
sudo anonshield start    # Initiate the transparent proxy and secure your connection
sudo anonshield status   # View current routing details and IP information
sudo anonshield newid    # Request a new anonymous circuit and external IP
sudo anonshield stop     # Safely revert your network and disable the proxy
```

## 🗑️ Uninstallation

If you ever want to completely remove the software and restore your system to its default state, simply run the installer with the uninstall flag:
```bash
sudo ./install.sh --uninstall
```

## ⚠️ Legal Disclaimer
**Who Watches Watchers?** is developed solely for educational and privacy-enhancing purposes. The author assumes absolutely no responsibility or liability for how this tool is utilized. Any misuse of this application for malicious, unauthorized, or illegal activities is strictly prohibited and is done entirely at the user's own risk. Stay safe and respect the law.
