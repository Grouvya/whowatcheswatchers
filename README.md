# Who Watches Watchers? (AnonShield)

**Who Watches Watchers?** is a sophisticated, system-wide automated setup and configuration framework designed to enforce extreme network privacy, anonymity, and security. It routes all system TCP and DNS traffic through the Tor network, hardens the host operating system, and provides multiple layers of defense against forensic analysis and deanonymization attacks.

## ⚠️ Legal Disclaimer
*Who Watches Watchers?* is developed solely for educational and privacy-enhancing purposes. The author assumes absolutely no responsibility or liability for how this tool is utilized. Any misuse of this application for malicious, unauthorized, or illegal activities is strictly prohibited and is done entirely at the user's own risk.

---

## 🛡️ Core Features

- **Transparent Tor Proxying:** All system TCP and DNS traffic is forced through the Tor network using `nftables` firewalls and `systemd-resolved` integration.
- **Hardware MAC Address Spoofing:** Randomizes network interface MAC addresses on startup to protect hardware identities.
- **Keystroke Biometric Scrambling:** Integrates `kloak` to obfuscate typing behavior and defeat keystroke biometrics.
- **Cryptographic RAM Wiping:** Uses `sdmem` to securely wipe RAM during shutdown/reboot, mitigating cold-boot attacks.
- **MicroVM Sandboxing:** Downloads an Alpine Linux ISO to run specific, high-risk commands isolated within a virtualized Tor namespace using `qemu`.
- **NTP Location Scrubbing:** Disables standard NTP and uses `htpdate` routed through Tor to prevent time-based location deanonymization.
- **Split Tunneling:** Allows specific users or commands (via the `anonshield-bypass` group) to bypass Tor and connect directly to the Clearnet when necessary.
- **Kernel & AppArmor Hardening:** Applies aggressive `sysctl` kernel hardening profiles (e.g., restricted `ptrace`, disabled unprivileged BPF) and strict `AppArmor` confinement for its binaries.
- **Kill Switch Monitor:** Continuously monitors the Tor service; if it crashes, network interfaces remain locked down to prevent Clearnet leaks.
- **Rich CLI & GUI:** Provides a beautiful Command Line Interface (using `rich`) and a modern Graphical User Interface (using `customtkinter`) for easy management.

---

## ⚙️ Installation

The installer script automatically handles downloading dependencies, creating virtual environments, compiling necessary tools, and configuring system services.

### Requirements
- Root (`sudo`) privileges
- Supported Package Managers: `apt-get`, `dnf`, or `pacman` (Debian/Ubuntu, Fedora/RHEL, Arch Linux).

### Running the Setup
1. Download or clone the script: `install.sh`
2. Run the script with root privileges:
   ```bash
   chmod +x install.sh
   sudo ./install.sh --install
   ```
3. The script will handle the setup process in 7 stages, including dependency installation, directory creation, Tor configuration, and bundled resource extraction.

---

## 💻 Usage

Once installed, you can manage the application via the terminal or the graphical interface.

### Command Line Interface (CLI)
The `anonshield` command requires root privileges.

```bash
# Secure all outbound traffic, start proxy, and spoof MACs
sudo anonshield start

# Flush firewall rules, restore system DNS, and reset MACs
sudo anonshield stop

# View current routing details, Tor circuits, and IP information
sudo anonshield status

# Request a new Tor circuit to change your public IP address
sudo anonshield newid

# Manually change or reset hardware MAC addresses
sudo anonshield mac

# Run a specific command isolated in a Tor namespace MicroVM
sudo anonshield sandbox <command>

# Run a specific command that bypasses Tor (Clearnet)
sudo anonshield bypass <command>
```

### Graphical User Interface (GUI)
You can launch the GUI using Polkit from any terminal or via the desktop shortcut created during installation:
```bash
pkexec anonshield-gui
```

### Systemd Service Integration
During installation, a systemd service (`anonshield.service`) is created. It ensures that the transparent proxy and MAC spoofing are initiated at boot time.

---

## 🗑️ Uninstallation

To completely remove the software, firewall rules, user groups, and revert Tor configurations to their original state:

```bash
sudo ./install.sh --uninstall
```

---

## 📂 File Architecture
- `/opt/anonshield/`: Application binary, virtual environment, configuration fallback, and MicroVM ISO.
- `/etc/anonshield/`: Primary configuration files and Privoxy configurations.
- `/var/lib/anonshield/`: Secure state directory for storing original MAC addresses and application state.
- `/usr/local/bin/anonshield`: Symlink to the main CLI executable.
- `/etc/apparmor.d/opt.anonshield.anonshield`: AppArmor strict confinement policy.
- `/etc/sysctl.d/99-anonshield-paranoid.conf`: Aggressive kernel hardening parameters.
