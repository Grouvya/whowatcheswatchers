# Who Watches Watchers? (AnonShield)

> [!IMPORTANT]
> **Who Watches Watchers?** is a highly advanced, system-wide automated setup and configuration framework designed to enforce extreme network privacy, anonymity, and security on Linux systems. 

This application goes beyond a simple VPN or Tor proxy. It routes all system TCP and DNS traffic through the Tor network, forcefully hardens the host operating system, and provides multiple layers of defense against advanced forensic analysis, deanonymization attacks, physical hardware seizures, and browser fingerprinting.

---

## ⚠️ Legal Disclaimer
*Who Watches Watchers?* is developed solely for educational and privacy-enhancing purposes. The author (Grouvya) assumes absolutely no responsibility or liability for how this tool is utilized. Any misuse of this application for malicious, unauthorized, or illegal activities is strictly prohibited and is done entirely at the user's own risk.

---

## 🛡️ Core Features & Defense Layers

The application provides a multi-layered defense strategy, broken down into Network, Hardware, Operational Security, and Isolation modules.

### 1. Advanced Network Obfuscation & Proxying
- **Transparent Tor Proxying:** All system TCP and DNS traffic is forcibly routed through the Tor network using `nftables` firewalls and `systemd-resolved` integration. Any traffic not passing through Tor is dropped by the kernel.
- **Tor Snowflake Bridges:** Evades ISP censorship and deep packet inspection (DPI) by disguising Tor traffic as innocent WebRTC video/audio calls using Snowflake bridges.
- **Decoy Traffic Generation:** To disrupt statistical traffic correlation attacks (where an adversary analyzes packet sizes and timings), an `anonshield-decoy` daemon runs in the background. It continuously requests random popular Clearnet websites (Wikipedia, Weather) through the proxy to create baseline network noise ("chaff").
- **DNS Blackholing & Encryption:** Uses `dnsmasq` paired with the StevenBlack blacklist to sinkhole telemetry and ads before they even hit the Tor network. Fallback DNS queries are routed securely through `dnscrypt-proxy`.
- **I2P Multiplexing:** Includes the Invisible Internet Project (`i2pd`) daemon for accessing the alternate I2P darknet securely.

### 2. Deep OS & Hardware Spoofing
- **Hardware MAC Address Spoofing:** Randomizes network interface MAC addresses on startup using `macchanger` and performs raw PCIe bus resets to ensure hardware-level identities are completely wiped from LAN broadcasts.
- **Hostname & Timezone Masking:** Upon activation, the system hostname is dynamically rewritten to a generic `amnesic` label, and the system timezone is aggressively forced to `UTC` to prevent local time leaks in timestamps.
- **Keystroke Biometric Scrambling:** Integrates `kloak` to obfuscate typing patterns and micro-delays between keystrokes, defeating behavioral biometrics and ML-based typist identification.
- **Physical USB Lockdown:** Leverages `USBGuard` to generate an aggressive policy on startup. Any new physical USB devices plugged into the machine while the proxy is active are instantly blocked, preventing "Rubber Ducky" or physical extraction attacks.

### 3. Extreme Anti-Forensics & Fingerprint Scrubbing
- **Browser Fingerprint Scrubbing:** Routes unencrypted system traffic through a `Privoxy` middleman that violently strips and standardizes `User-Agent`, `Accept-Language`, and `Referer` headers, flattening your fingerprint to look like millions of other machines.
- **AmiUnique Integration:** A built-in GUI button directly launches `https://amiunique.org/fingerprint` allowing users to instantly audit their browser's canvas and fingerprint anonymity score.
- **Cryptographic RAM Wiping & Swap Encryption:** Uses `sdmem` to cryptographically wipe physical RAM upon shutdown. Additionally, any active swap partitions are dynamically secured using single-use AES-XTS keys via `/dev/urandom`.
- **Amnesic Shells:** The `amnesic` subcommand drops the user into a root shell where all Bash history logging (`HISTFILE`) is explicitly disabled. Commands executed here will never be written to the disk.
- **Hardware Panic Wiping:** The `panic` subcommand acts as a software kill-switch. It instantly destroys all network routing, flashes extreme drop-all firewall rules, kills the Tor/Privoxy services, shreds the state files holding the original MAC addresses, and aggressively flushes all RAM caches via `sysrq` triggers.

### 4. Advanced Sandboxing & Hardening
- **Ephemeral MicroVMs:** Includes a `sandbox` command that launches an Alpine Linux ISO via `qemu-system-x86_64` using the `-snapshot` flag. This creates a highly isolated, ephemeral virtual machine. Everything done inside the VM evaporates from memory upon closure, never touching the host hard drive.
- **Aggressive Kernel Hardening:** Applies paranoid `sysctl` profiles restricting `ptrace` (process tracing), disabling unprivileged BPF, and restricting `dmesg` outputs.
- **AppArmor Confinement:** The application binary itself runs under strict `AppArmor` confinement to ensure even if the Python GUI is compromised, the attacker cannot escape to the host system.

---

## ⚙️ Installation

The installer script is fully automated. It handles downloading dependencies across multiple package managers (`apt`, `dnf`, `pacman`), creating isolated Python virtual environments, compiling necessary C tools (`kloak`), downloading the MicroVM ISO, and configuring system services.

### Requirements
- Root (`sudo`) privileges
- Supported Package Managers: Debian/Ubuntu (`apt`), Fedora/RHEL (`dnf`), or Arch Linux (`pacman`).

### Running the Setup
1. Clone or download `install.sh`.
2. Make it executable and run it with root privileges:
   ```bash
   chmod +x install.sh
   sudo ./install.sh --install
   ```
3. The script will handle the setup process in 7 stages. **Note:** It may take a few minutes as it compiles software and downloads the Alpine ISO.

---

## 💻 Usage

Once installed, you can manage the framework via the terminal or the graphical interface.

### Command Line Interface (CLI)
The `anonshield` command is deployed globally but requires root privileges.

```bash
# Secure all outbound traffic, start proxy, spoof MACs, mask Hostname, and lock down USBs
sudo anonshield start

# Flush firewall rules, restore system DNS, and reset hardware states
sudo anonshield stop

# View current routing details, Tor circuits, and IP information
sudo anonshield status

# Request a new Tor circuit to change your public IP address
sudo anonshield newid

# Drop into a fully amnesic shell (no bash history recorded)
sudo anonshield amnesic

# HARD PANIC: Instantly kill Tor, firewall everything, and wipe RAM caches
sudo anonshield panic

# Run a specific command isolated in an ephemeral Tor MicroVM
sudo anonshield sandbox

# Run a specific command that bypasses Tor (Clearnet Split-Tunneling)
sudo anonshield bypass <command>
```

### Graphical User Interface (GUI)
You can launch the GUI using Polkit from any terminal or via the desktop shortcut created during installation. The GUI provides a modern, dark-themed dashboard to toggle Tor routing, view bandwidth graphs in real-time, launch the MicroVM, and audit fingerprinting.
```bash
pkexec anonshield-gui
```

### Systemd Integration
During installation, the `anonshield.service` is generated as a `oneshot` service. It ensures that the transparent proxy and MAC spoofing are initiated seamlessly at system boot time.

---

## 🗑️ Uninstallation

To completely and safely remove the software, firewall rules, custom user groups, and revert all Tor configurations back to their original state:

```bash
sudo ./install.sh --uninstall
```

---

## 📂 Architecture Overview
- `/opt/anonshield/`: Application binary, Python virtual environment, configuration fallback, and the Alpine MicroVM ISO.
- `/etc/anonshield/`: Primary configuration files, DNS blacklists, and Privoxy setups.
- `/var/lib/anonshield/`: Secure state directory for storing original MAC addresses (temporarily) and application state.
- `/usr/local/bin/anonshield`: Symlink to the main CLI executable.
- `/usr/local/bin/decoy-traffic.sh`: The background script generating Tor traffic chaff.
- `/etc/apparmor.d/opt.anonshield.anonshield`: AppArmor strict confinement policy.
- `/etc/sysctl.d/99-anonshield-paranoid.conf`: Aggressive kernel hardening parameters.
