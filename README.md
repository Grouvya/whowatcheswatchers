# Who Watches Watchers? 🛡️

**Who Watches Watchers?** is an extreme-edge, system-wide Transparent Tor Anonymizer designed for Linux. It aggressively reroutes all network traffic originating from your machine through the Tor network, blocks leaks natively at the firewall level, and provides God-Tier physical and digital anti-forensic capabilities.

Built for individuals operating in the most hostile environments imaginable, this tool incorporates defenses normally only seen in paranoid, specialized operating systems like Qubes OS or Tails—seamlessly controlled via a sleek graphical interface.

---

## 🌟 Key Features

### 🛡️ Core Network Defenses
*   **Transparent Proxying**: Captures and routes 100% of TCP traffic through Tor automatically.
*   **Fail-Close Kill Switch**: If the Tor daemon crashes or the connection drops, the firewall locks down the system. Not a single byte of traffic can leak onto the clearnet.
*   **DNS Blackholing & Double Encryption**: DNS queries are scrubbed against the StevenBlack telemetry blocklist using `dnsmasq`, and then double-encrypted using **DNSCrypt-Proxy** *before* being routed through Tor.
*   **IPv6 Disablement**: Hard-disables IPv6 kernel-wide to prevent routing leaks and tunnel bypassing.
*   **NTP Location Scrubbing**: Replaces standard, unencrypted UDP time syncing with `htpdate`, which fetches the time securely over Tor using TCP HTTP headers to hide your uptime and rough timezone.

### 🧬 God-Tier Anonymity & Anti-Forensics
*   **Keystroke Biometric Scrambling**: Integrates `kloak` to intercept and mangle your physical keyboard input at the kernel level, completely destroying your biometric typing signature.
*   **Tor Stream Compartmentalization**: Forces Tor to build distinct circuits for every website you visit, making traffic correlation mathematically impossible.
*   **Polymorphic Hardware Spoofing**: The network interfaces undergo a raw PCIe bus reset on boot, and MAC addresses are dynamically cloned to mimic trusted hardware prefixes (Apple, Intel).
*   **TCP Header Mangling**: Defeats deep-packet inspection (DPI) by overwriting the Linux TTL signature (64) to mimic a standard Windows 10 machine (128).
*   **Cryptographic RAM Wiping**: A dedicated systemd hook (`sdmem`) forcefully overwrites all physical RAM chips with random data on shutdown or reboot, neutralizing Cold Boot hardware attacks.
*   **Single-Use Encrypted Swap**: Every time the shield activates, your Swap partition is wiped and re-encrypted with a randomized AES key that is destroyed when the computer loses power.

### 📦 Sandboxing & Hardening
*   **Polymorphic microVM Sandbox**: Boot a fully functioning, nestable Alpine Linux Virtual Machine on-the-fly (`qemu`). Every boot dynamically generates a fake RAM allocation, random CPU cores, fake MAC address, and a random SMBIOS UUID to break browser fingerprinting.
*   **Aggressive Kernel Lockdown**: Injects extreme `sysctl` rules to disable `ptrace`, restrict kernel logs, and turn off unused network protocols to prevent zero-day privilege escalations.
*   **AppArmor Confinement**: Core application components are strictly confined.
*   **Physical Lockdown**: USBGuard integration automatically blocks the insertion of rogue USB devices (Rubber Duckies, badUSBs).

### 🚨 Emergency Functions
*   **Emergency Panic Wipe**: Instantly drops kernel memory caches, shreds temporary state files, violently kills network connections, and blocks all future network access until the machine is hard-rebooted.
*   **Dead-Man's Poison Pill**: A silent `<Control-Shift-Delete>` hotkey in the GUI that permanently shreds the application source code, deletes the microVM ISO, drops memory caches, and hard-crashes the desktop environment to instantly lock out an adversary. **Use with extreme caution.**

---

## 🚀 Installation & Usage

1. **Install the Application:**
   Run the interactive install script with `sudo`. (Note: The script will automatically compile tools like `kloak` from source and configure kernel parameters).
   ```bash
   sudo ./install.sh
   ```

2. **Launch the Interface:**
   You can run the application directly from your desktop launcher or the terminal:
   ```bash
   anonshield-gui
   ```

3. **Using the Sandbox:**
   To use the microVM Sandbox, launch the GUI, ensure the shield is active, and click **"Launch microVM Sandbox"**.
   - When the QEMU window opens and the Alpine Linux boot finishes, you will be prompted to log in.
   - **Username:** `root`
   - **Password:** *(Leave blank and press Enter)*
   - Any changes made inside the microVM are ephemeral and will be destroyed completely when you close the window.

---

## ⚠️ Important Considerations

*   **Speed**: Due to Stream Compartmentalization and DNSCrypt, your initial page loads may be slower as Tor is forced to aggressively construct new circuits.
*   **Typing Feel**: Because of the `kloak` biometric scrambler, you may feel an extremely tiny latency when typing rapidly. This is intentional.
*   **Hibernation Disabled**: Due to Single-Use Encrypted Swap, **Hibernation / Suspend-to-Disk is completely unsupported and will destroy your session data.**
*   **Poison Pill Danger**: The `Ctrl + Shift + Delete` shortcut has no confirmation dialog. If you press it, the installation will self-destruct immediately.

## 🤝 Support & Contributions
Built with paranoia, for paranoia. If you find a security hole, consider it a zero-day and fix it.
