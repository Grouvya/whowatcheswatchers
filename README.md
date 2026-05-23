# Who Watches Watchers? (AnonShield)

**Advanced System-Wide Anonymizer and Privacy Toolkit**

"Who Watches Watchers?" (also known as AnonShield) is a highly aggressive, system-wide network anonymization and security tool designed for Linux. It forces all of your operating system's traffic through the Tor network via a transparent proxy and hardened firewall rules, ensuring absolutely no IP leaks. 

In addition to routing traffic through Tor, it provides an array of advanced privacy countermeasures, including hardware address spoofing, biometric keystroke obfuscation, browser fingerprint randomization, and an isolated MicroVM sandbox.

---

## 🌟 Key Functionality

- **System-Wide Tor Transparent Proxy:** All TCP and DNS traffic from any application is forcefully routed through the Tor network using `nftables`. Traffic that cannot be routed (e.g., UDP, ICMP) is strictly blocked to prevent leaks.
- **Hardware MAC Address Spoofing:** Automatically rotates and spoofs your network interfaces' MAC addresses on startup, protecting your hardware identity from local network administrators.
- **Fingerprint Shield (mitmproxy):** A local interception proxy that injects dynamic noise into your web traffic, effectively randomizing your browser fingerprint to defeat advanced tracking techniques (like Canvas, WebGL, and AudioContext fingerprinting).
- **Alpine MicroVM Sandbox:** Launch untrusted applications or files within an isolated Alpine Linux QEMU MicroVM. The VM is confined to its own network namespace and destroys all session data upon exit.
- **Clearnet Split-Tunneling (Bypass):** Need to access a website that blocks Tor? You can run specific applications outside the Tor tunnel using the dedicated `anonshield-bypass` group.
- **Biometric Keystroke Anonymization:** Utilizes `kloak` to scramble the timing of your keystrokes, defeating biometric identification based on typing cadence.
- **Decoy Traffic Generator:** Continuously generates background noise traffic to popular websites, obfuscating your actual browsing patterns from ISP traffic analysis.
- **Aggressive Kernel Hardening:** Applies restrictive `sysctl` security configurations (disabling ptrace, restricting dmesg, and disabling TCP timestamps) to protect the host OS.
- **Cryptographic RAM Wiping:** Hooks into the shutdown process to securely wipe system memory (using `sdmem`), mitigating cold-boot attacks.

---

## 🛠 Installation

The application comes with a fully automated installer script that handles dependencies, virtual environments, systemd services, and desktop shortcuts.

1. Open a terminal and navigate to the directory containing the installer.
2. Run the installer script with `sudo`:
   ```bash
   sudo ./install.sh
   ```
3. The script will automatically detect your package manager (`apt`, `dnf`, or `pacman`) and install required system packages, download the MicroVM image, and set up the Python environment.
4. Once completed, a desktop shortcut will be placed on your desktop and in your application menu.

---

## 🚀 Usage

You can manage the shield either through the sleek Graphical User Interface (GUI) or the Command Line Interface (CLI).

### Using the GUI
You can launch the GUI by searching for **"Who Watches Watchers?"** in your application menu, using the Desktop shortcut, or running the following command:
```bash
pkexec anonshield-gui
```
The GUI allows you to:
- Toggle the Tor transparent proxy on and off.
- Monitor real-time Rx/Tx bandwidth and view security logs.
- Start or stop the Fingerprint Randomizer.
- Request a new Tor identity (IP address).
- Launch applications directly into the MicroVM or the Clearnet Bypass.

### Using the CLI
For power users, the `anonshield` command provides full control:
- `sudo anonshield start` — Activate the transparent proxy and MAC spoofing.
- `sudo anonshield stop` — Deactivate the proxy and restore normal networking.
- `sudo anonshield status` — View your current external IP and Tor circuit status.
- `sudo anonshield newid` — Request a new Tor identity.
- `sudo anonshield bypass <command>` — Run a command over the normal network (bypassing Tor).

---

## 📦 Using the MicroVM Sandbox

The MicroVM Sandbox is a powerful feature designed to handle untrusted or potentially malicious files/applications without risking your host operating system.

When you launch an application in the sandbox, it runs inside a lightweight Alpine Linux virtual machine (QEMU). The VM has no access to your host filesystem and runs inside a locked-down network namespace.

**To use the sandbox:**
1. **Via GUI:** Click the **"MicroVM Sandbox"** button in the graphical interface. A prompt will ask you which application or command you wish to sandbox.
2. **Via CLI:** Run the following command:
   ```bash
   sudo anonshield sandbox "firefox"
   ```
   *(Replace `firefox` with the command you wish to run).*

*Note: The MicroVM is amnesic. Once the sandbox window is closed, the VM is instantly destroyed and all changes made inside it are permanently lost.*

---

## 🗑 Uninstallation

If you wish to remove the application and restore your system to its default state, the installer script includes an automated cleanup sequence.

1. Navigate to the directory containing `install.sh`.
2. Run the uninstallation command:
   ```bash
   sudo ./install.sh --uninstall
   ```

**The uninstaller will safely:**
- Stop all background anonymization services.
- Flush all firewall (`nftables`) proxy rules.
- Restore your original hardware MAC addresses from the saved state.
- Remove injected CA certificates from your browser trust stores (Firefox, Chromium, Brave).
- Restore your system's DNS and time synchronization settings.
- Delete all application files, virtual environments, and desktop shortcuts.

---
*Created with ❤ by Grouvya!*
