# 🛡️ Who Watches Watchers? (AnonShield)

**Who Watches Watchers?** (also known as AnonShield) is an advanced automated setup system designed to route all your system traffic transparently through the Tor network while applying extreme operational security measures.

## 🌟 Features

*   **Transparent Tor Routing:** Forces all system network traffic through Tor using `nftables` and `Tor`'s transparent proxy feature.
*   **Split Tunneling:** Allows specific users (added to `anonshield-bypass` group) or local LAN traffic to bypass the Tor network.
*   **Hardware Anonymization:** Automatically spoofs and rotates your network interface MAC addresses on startup.
*   **Keystroke Biometric Scrambling:** Integrates `kloak` to obfuscate typing patterns and prevent stylometric identification.
*   **Cryptographic RAM Wiping:** Uses `sdmem` to securely wipe system memory on shutdown/reboot, mitigating cold-boot attacks.
*   **Secure DNS & Time Sync:** Uses `dnscrypt-proxy` and Tor-routed `htpdate` to prevent DNS leaks and timezone-based tracking.
*   **Kernel Hardening:** Applies aggressive `sysctl` security parameters (e.g., `ptrace_scope`, `kptr_restrict`, restricting `dmesg`).
*   **AppArmor Confinement:** Enforces strict AppArmor profiles to sandbox the application.
*   **I2P Integration:** Automatically starts `i2pd` in the background for `.i2p` hidden service access alongside standard Tor routing.

## 🚀 Installation

> [!WARNING]
> This script modifies core system network configurations, firewall rules, and kernel parameters. Please ensure you understand the changes before running.

1. Ensure the installation script is executable:
   ```bash
   chmod +x install.sh
   ```

2. Run the script with root privileges:
   ```bash
   sudo ./install.sh
   ```

The script will automatically detect your package manager (APT, DNF, or Pacman) and install all required dependencies.

## 🗑️ Uninstallation

If you wish to completely remove AnonShield and restore your default network configurations, you can do so by editing the script to run the uninstallation routine or by manually reverting the changes listed in `do_uninstall()` within `install.sh`.

## ⚙️ Configuration

Application configurations and state are stored in:
*   **Config Directory:** `/etc/anonshield/`
*   **State Directory:** `/var/lib/anonshield/`

Tor configurations are appended to `/etc/tor/torrc`.

## 📝 License
This tool is provided as-is. Use responsibly and ensure compliance with your local laws regarding encryption and anonymizing networks.
