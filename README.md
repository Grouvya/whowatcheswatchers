# 🛡️ Who Watches Watchers?

**Who Watches Watchers?** is a premium, automated transparent proxy and state-of-the-art anonymity system for Linux. It routes all system traffic through the Tor network and provides an arsenal of paranoia-level network defenses, physical security lock-downs, and anti-forensic tools, all wrapped in a sleek GUI and CLI.

## ✨ Extreme Anonymity Features

### 📡 Network Defense
- **Transparent Tor Proxying:** All system traffic (TCP/DNS) is automatically forced through Tor using strict `nftables` firewall rules.
- **Protocol Scrubbing (Privoxy):** Seamlessly strips application-level HTTP/HTTPS tracking headers and cookies before traffic even hits the Tor network.
- **DNS Leak Prevention:** Forwards standard port 53 DNS to Tor, and actively **blocks** outgoing DoT (Port 853) to prevent rogue apps from bypassing the proxy.
- **IPv6 Disabling:** Aggressively disables IPv6 at the kernel level (`sysctl`) when the shield is active to prevent subtle routing leaks.
- **Traffic Padding:** Continuously generates Tor dummy packets to destroy Website Traffic Fingerprinting attempts by deep-packet inspection.

### 🛑 Anti-Forensics & System Lockdown
- **Emergency Panic Mode:** A dedicated button that instantly drops all network traffic, terminates the proxy, drops system RAM caches (to defeat cold-boot memory forensics), and shreds your original MAC address state files.
- **Physical Lockdown (`USBGuard`):** Automatically whitelists currently connected devices and **blocks all new USB devices** when the shield is active, thwarting physical BadUSB or hardware keylogger attacks.
- **Mandatory Access Control (`AppArmor`):** Enforces a dynamically generated, strict AppArmor profile to physically prevent the anonymizer backend from accessing unauthorized files.
- **System Kill Switch:** Monitors the Tor daemon and drops all packets instantly if the connection is lost.

### 🎭 Hardware & Identity Spoofing
- **MAC Spoofing & Rotation:** Automatically changes your hardware MAC address on startup and can seamlessly rotate it in the background every 15 minutes.
- **Hostname Spoofing:** Temporarily rewrites your machine's hostname to `amnesic` to prevent local network leaks.
- **Strict Isolated Sandbox (`bwrap`):** Launch sensitive apps in an isolated namespace where hardware identifiers (CPU, Motherboard) and personal files (`/home`) are completely hidden via `tmpfs` mounts.
- **Time-Correlation Protection:** Apps launched in the sandbox are injected with `faketime`, slightly skewing their clock to defeat millisecond-level time tracking.

### 🌉 Advanced Tor Controls
- **Disguised Traffic (Snowflake & Obfs4):** Bypass severe censorship by disguising your Tor traffic as WebRTC video calls (Snowflake) or using obfuscated bridges (Obfs4).
- **Exit Node Selection:** Force your traffic to exit from specific geographic regions (e.g., US, DE).
- **Split Tunneling:** Run specific trusted applications outside of the Tor network (Clearnet) using the `anonshield-bypass` group.
- **Ephemeral Hidden Services:** Easily expose a local port as an anonymous `.onion` address.

## 🚀 Installation

The automated setup script installs all extreme-level dependencies and applies the system hardening rules. It supports Debian/Ubuntu (`apt`), Fedora/RHEL (`dnf`), and Arch Linux (`pacman`).

```bash
chmod +x install.sh
sudo ./install.sh
```

During installation, the script will:
1. Install Tor, Bubblewrap, Privoxy, Snowflake, USBGuard, AppArmor, and Python dependencies.
2. Generate strict Tor configurations (Padding enabled).
3. Generate and enforce AppArmor profiles.
4. Set up the split-tunneling group.

## 💻 Usage

Launch the application via your desktop menu (**Who Watches Watchers?**) or run the GUI/CLI from the terminal.

### Graphical Interface (GUI)
Run `anonshield-gui` to open the premium dashboard. From here, you can enable Snowflake, trigger a Panic Wipe, rotate your identity, and monitor active Tor circuits.

### Command Line Interface (CLI)
```bash
# Start the hardened proxy
sudo anonshield start

# Stop the proxy and restore system defaults (Networking, USB, Hostname)
sudo anonshield stop

# Check status and public IP
sudo anonshield status

# Rotate to a new Tor identity
sudo anonshield new-id

# Launch an application in the extremely secure sandbox
sudo anonshield sandbox <command>

# Launch an application bypassing Tor (Clearnet)
sudo anonshield bypass <command>
```

## ⚠️ Uninstallation

To completely remove the application, AppArmor profiles, and custom configurations:
```bash
sudo ./install.sh uninstall
```

## ⚖️ Disclaimer

This tool employs extreme measures to protect user privacy. However, no software can guarantee absolute 100% anonymity against all threats. Understand the tools you are using, practice strong operational security (OpSec), and use this software responsibly and at your own risk. The developers are not responsible for any consequences resulting from its use.
