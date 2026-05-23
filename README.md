# 🛡️ Who Watches Watchers?

**Who Watches Watchers?** is a premium, automated transparent proxy and anonymity system for Linux. It routes all system traffic through the Tor network, providing robust privacy, security, and an arsenal of advanced networking features wrapped in a sleek GUI and CLI.

## ✨ Features

- **Transparent Tor Proxying:** All system traffic (TCP/DNS) is automatically routed through Tor using `nftables`.
- **System Kill Switch:** Monitors the Tor daemon and immediately blocks all network traffic if the connection drops, preventing accidental IP leaks.
- **Auto MAC Spoofing & Rotation:** Automatically changes your hardware MAC address upon startup and can rotate it continuously at a set interval (default 15m) to thwart local tracking.
- **Split Tunneling (Clearnet Bypass):** Run specific applications outside of the Tor network using the `anonshield-bypass` group.
- **Isolated App Sandbox:** Launch applications in a secure, isolated network namespace.
- **Advanced Tor Controls:**
  - Easily request new Tor identities (circuits).
  - Force Exit Nodes to specific geographic regions (e.g., US, DE).
  - Enable **Obfs4 Bridges** or custom bridges to bypass deep packet inspection and Tor censorship.
  - Ephemeral Hidden Service hosting (expose a local port as an `.onion` address).
- **Premium GUI Dashboard:** Built with `customtkinter`, featuring real-time bandwidth graphs, active Tor circuit routing paths, and system status indicators.
- **Automated Network Checks:** Warns you if you attempt to start the proxy without an active internet connection.

## 🚀 Installation

This tool provides a fully automated setup script that works on Debian/Ubuntu (`apt`), Fedora/RHEL (`dnf`), and Arch Linux (`pacman`). 

To install the application and all necessary dependencies:

```bash
chmod +x install.sh
sudo ./install.sh
```

During installation, the script will:
1. Install system dependencies (`tor`, `macchanger`, `nftables`, `obfs4proxy`, etc.)
2. Create an isolated Python virtual environment and install UI dependencies.
3. Configure your `/etc/tor/torrc` for transparent proxying.
4. Set up the `anonshield-bypass` group for split-tunneling.
5. Create desktop shortcuts and a system tray icon.

## 💻 Usage

After installation, you can launch the application either via your desktop application menu (**Who Watches Watchers?**) or from the command line:

### Graphical Interface (GUI)
Simply run `anonshield-gui` from your terminal or click the application icon. The GUI provides a full dashboard to start/stop the shield, monitor traffic, and configure advanced settings like bridges and exit nodes.

### Command Line Interface (CLI)
You can also control the system directly from the command line using the `anonshield` command:

```bash
# Start the transparent proxy and anonymize traffic
sudo anonshield start

# Stop the proxy and restore normal networking
sudo anonshield stop

# Check current status, public IP, and active Tor circuit
sudo anonshield status

# Rotate to a new Tor identity (new circuit)
sudo anonshield new-id

# Start MAC auto-rotator (rotates every 15 minutes)
sudo anonshield start-mac-rotator

# Launch an application in the sandbox
sudo anonshield sandbox <command>

# Launch an application bypassing Tor (Clearnet)
sudo anonshield bypass <command>
```

## ⚠️ Uninstallation

To completely remove the application, its configuration files, and restore your system to its original state, run the install script with the `uninstall` argument:

```bash
sudo ./install.sh uninstall
```

## ⚖️ Disclaimer

This tool is designed to enhance privacy and security by leveraging the Tor network. However, no software can guarantee 100% anonymity. Use this tool responsibly and at your own risk. The developers are not responsible for any misuse, data leaks, or legal consequences resulting from the use of this software.
