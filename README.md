# Who Watches Watchers? 🛡️

**Who Watches Watchers?** is a state-of-the-art, system-wide, fail-closed Transparent Tor Anonymizer for Linux. It aggressively reroutes all network traffic originating from your machine through the Tor network while blocking leaks natively at the firewall level.

Designed for journalists, whistleblowers, and privacy advocates operating in high-risk environments, this tool features an array of deep-cover operational security functions seamlessly controlled via a sleek graphical interface.

---

## 🌟 Key Features

### 🛡️ Core Defenses
*   **Transparent Proxying**: Captures and routes 100% of TCP traffic through Tor automatically.
*   **Fail-Close Kill Switch**: If the Tor daemon crashes or the connection drops, the firewall locks down the system. Not a single byte of traffic can leak onto the clearnet.
*   **DNS Blackholing**: Intercepts DNS queries natively and scrubs them against the StevenBlack blocklist using `dnsmasq`, destroying ads and telemetry requests before routing legitimate domains through Tor.
*   **IPv6 Disablement**: Hard-disables IPv6 kernel-wide to prevent routing leaks and tunnel bypassing.
*   **Time Fuzzing**: Protects against clock-skew fingerprinting by using `faketime` to slightly alter timestamps.

### 🧬 Anonymity & Anti-Forensics
*   **Tor Stream Compartmentalization**: Actively forces `IsolateDestAddr` and `IsolateDestPort`, instructing Tor to build distinct circuits for every website you visit, making traffic correlation impossible.
*   **Smart MAC Address Blending**: Automatically clones legitimate hardware manufacturer prefixes (Apple, Intel, Samsung) during MAC spoofing to blend perfectly into coffee shop networks.
*   **TCP Header Mangling**: Defeats deep-packet inspection (DPI) by overwriting the Linux TTL signature (64) to mimic a standard Windows 10 machine (128).
*   **Protocol Scrubbing**: Uses Privoxy to scrub HTTP headers of identifying telemetry before passing traffic to Tor.
*   **Disguised Traffic**: Built-in support for Obfs4 bridges and Snowflake proxies to bypass strict ISP-level Tor blocks.

### 📦 Sandboxing & Hardening
*   **Ephemeral microVM Sandbox**: Boot a fully functioning, nestable Alpine Linux Virtual Machine on-the-fly (`qemu`). When closed, the microVM is vaporized from memory, leaving zero forensic trace.
*   **AppArmor Confinement**: Core application components are strictly confined to prevent malicious takeover.
*   **Physical Lockdown**: USBGuard integration automatically blocks the insertion of rogue USB devices (Rubber Duckies, badUSBs).

### 🚨 Emergency Functions
*   **Emergency Panic Wipe**: Instantly drops kernel memory caches, shreds temporary state files, violently kills network connections, and blocks all future network access until the machine is hard-rebooted.

---

## 🚀 Installation & Usage

1. **Install the Application:**
   Run the interactive install script with `sudo`:
   ```bash
   sudo ./install.sh
   ```
   *The script automatically detects your package manager, downloads dependencies (including the Alpine microVM ISO), and installs the GUI wrapper.*

2. **Launch the Interface:**
   You can run the application directly from your desktop launcher or the terminal:
   ```bash
   anonshield-gui
   ```
   *(Note: You will be prompted to authenticate via polkit as the core daemon requires root access to manage network namespaces and nftables).*

3. **Using the Sandbox:**
   To use the microVM Sandbox, launch the GUI, ensure the shield is active, and click **"Launch microVM Sandbox"**.

---

## ⚠️ Important Considerations

*   **Clearnet Bypass**: The *Split Tunneling* feature allows you to bypass Tor entirely. Use this **only** for trusted applications that explicitly ban Tor nodes (e.g., banking apps or specific games).
*   **Speed**: Due to Stream Compartmentalization, your initial page loads may be slower as Tor is forced to aggressively construct new circuits.
*   **MicroVM**: To use the microVM sandbox, your hardware must support nested virtualization if you are already inside a VM.

## 🤝 Support & Contributions
Built with paranoia, for paranoia. If you find a security hole, consider it a zero-day and fix it.
