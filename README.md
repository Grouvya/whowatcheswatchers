# Who Watches Watchers? (AnonShield)

Who Watches Watchers? (AnonShield) is an advanced, system-wide Tor anonymizer GUI and security hardening suite for Linux. It completely locks down your system's network connections, routing everything securely through the Tor network while deploying multiple layers of anti-forensic countermeasures.

## 🛡️ Functionalities and Capabilities

The suite comes packed with advanced privacy, security, and anti-forensic features:

* **System-Wide Tor Routing:** Forces all system network traffic through Tor transparently using `nftables`.
* **Fingerprint Shield (MITM Proxy):** Uses `mitmproxy` to intercept secure web traffic and strip tracking headers to obfuscate your unique browser fingerprint.
* **Keystroke Biometric Scrambling:** Integrates `kloak` to randomize the timing of your keystrokes, defeating stylometry and behavioral identification.
* **Cryptographic RAM Wiping:** Uses `sdmem` to securely overwrite your system RAM during shutdown to prevent cold-boot attacks.
* **Single-Use Swap Encryption:** Automatically encrypts your swap space using a single-use key to prevent data recovery.
* **MAC Address Spoofing:** Randomizes your network interface MAC addresses on activation using `macchanger` and restores them upon deactivation.
* **Hostname Spoofing:** Temporarily changes your computer's hostname to `amnesic` while active.
* **Extreme Kernel Hardening:** Deploys aggressive `sysctl` rules to restrict kernel pointers (`kptr_restrict`), kernel logs (`dmesg_restrict`), `ptrace` scope, and unprivileged BPF.
* **Secure DNS & NTP:** Routes your system time synchronization (NTP) over Tor using `htpdate`, and secures your DNS requests via Tor and `dnscrypt-proxy`.
* **IPv6 Disablement:** Prevents IP leaks by completely disabling IPv6.
* **Ephemeral microVM Sandbox:** A built-in graphical QEMU microVM running Alpine Linux for completely isolated tasks.

---

## 📦 Ephemeral microVM Sandbox

AnonShield includes a built-in isolated micro-Virtual Machine (microVM) running Alpine Linux. This allows you to open potentially dangerous files or browse securely in an environment that is completely wiped the moment it is closed.

* **Default Login User:** `root`
* **Default Password:** *(none, just press Enter)*

*Note: The microVM sandbox routes all of its traffic through the host's Tor connection automatically.*

---

## 🚀 Walkthrough

### 1. Activating the Shield
To open the AnonShield Graphical Interface, run the following command in any terminal:
```bash
pkexec anonshield-gui
```
From the GUI, you can click **"Activate Shield"** to lock down the system. The GUI will guide you through the process, setting up Tor routing and applying all security measures.

### 2. Using the microVM
Once the shield is active, you can launch the isolated Alpine Linux environment directly from the GUI by clicking **"Launch microVM Sandbox"**.

---

## ⚠️ Important: Brave Browser & "HSTS" Errors

Because AnonShield includes a **"Fingerprint Shield"** that scrubs your traffic, you might encounter a security error in browsers like **Brave** or **Chrome** when visiting secure websites (like `github.com`). The error will say:

> "The website sent back unusual and incorrect credentials... You cannot visit github.com right now because the website uses HSTS."

### Why this happens:
To scrub identifying information from secure (HTTPS) websites, AnonShield's `mitmproxy` has to generate a custom "Man-in-the-Middle" security certificate and install it on your system. Because `github.com` strictly enforces HSTS (HTTP Strict Transport Security), Brave detects this custom certificate as an unrecognized interception and blocks the connection.

### How to fix it:
1. **Restart your browser:** If Brave was already running when you activated the shield, it hasn't loaded the newly injected certificates yet. Fully close Brave and open it again.
2. **Manually import the certificate:** Modern Chromium browsers sometimes ignore local Linux system certificates. You can manually tell Brave to trust AnonShield's proxy:
   * Open Brave and navigate to: `brave://settings/certificates`
   * Click on the **Authorities** tab.
   * Click **Import**.
   * Navigate to `/usr/local/share/ca-certificates/` and select the `anonshield-mitm.crt` file.
   * Check the boxes to trust the certificate for identifying websites, and click **OK**.

---

## ⚙️ Installation Instructions

To install the application and all of its dependencies, simply run the setup script with root privileges:

```bash
sudo ./install.sh --install
```
*(Alternatively, running `sudo ./install.sh` without arguments will default to installing it).*

The script will handle installing required packages, generating the python virtual environment, downloading the microVM ISO, and compiling necessary tools like `kloak`.

---

## 🗑️ Uninstallation Instructions

To completely remove AnonShield, stop all its background services, restore your MAC addresses, and clean up all application data, run:

```bash
sudo ./install.sh --uninstall
```
This will completely revert your system's network routing, firewall rules, and DNS resolution back to their original defaults.
