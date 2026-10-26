# CyberPatriot Round-Ready SuperScripts

This repository contains four specialized, interactive CyberPatriot hardening scripts designed to get 85–100 points quickly in competition rounds across Ubuntu, Linux Mint, Windows 10/11 Workstations, and Windows Server environments.

---

## Script Overview

| Script Name | Target Operating System | Language | Description |
| :--- | :--- | :--- | :--- |
| **`UbuntuSuperScript`** | Ubuntu 20.04 / 22.04 / 24.04 | Bash | Interactive Linux hardening script tailored for Ubuntu. |
| **`MintSuperScript`** | Linux Mint 20 / 21 | Bash | Interactive Linux hardening script tailored for Linux Mint (includes Update Manager auto-refresh settings). |
| **`WindowsSuperScript`** | Windows 10 / Windows 11 | PowerShell | Interactive Windows Desktop hardening script. |
| **`WindowsServerSuperScript`** | Windows Server 2016 / 2019 / 2022 | PowerShell | Interactive Windows Server hardening script (supports Active Directory Domain Controllers & Member Servers). |

---

## How to Run in a Competition Round

### 1. Pre-Run Prerequisites (First 2 Minutes of the Round)
1. **Read the Competition README:** Open and read the image's README carefully before running any scripts.
2. **Note Down Specifics:** Keep track of:
   - Authorized Users
   - Authorized Administrators
   - Critical / Required Services (e.g., SSH, Apache, IIS, FTP, Remote Desktop)
   - Extra Allowed Firewall Ports
   - Password requirements
3. **Answer Forensics Questions First:** Always complete and save your answers for any Forensics Questions on the Desktop *before* running remediation tools.

---

### 2. Running Linux Scripts (`UbuntuSuperScript` / `MintSuperScript`)

1. Open a terminal on the image.
2. Grant execution permissions:
   ```bash
   chmod +x UbuntuSuperScript
   # OR for Mint:
   chmod +x MintSuperScript
   ```
3. Run the script with root privileges:
   ```bash
   sudo ./UbuntuSuperScript
   # OR for Mint:
   sudo ./MintSuperScript
   ```
4. **Interactive Prompts:**
   - **Step 0 (README Details):** Enter space-separated lists when prompted for Authorized Users, Authorized Administrators, Services to Keep, Firewall Ports to Allow, and default password.
   - **Hardening Steps:** Answer `Y` or `N` to confirm each aggressive hardening section (User deletion, Password reset, PAM policy, Sysctl rules, SSH hardening, Package/Service purging, Backdoor removal, Media file cleanup, UFW Firewall, and System updates).

---

### 3. Running Windows Scripts (`WindowsSuperScript` / `WindowsServerSuperScript`)

1. Open **PowerShell as Administrator** (Right-click PowerShell -> *Run as administrator*).
2. Set execution policy for the current session:
   ```powershell
   Set-ExecutionPolicy -Scope Process Bypass -Force
   ```
3. Execute the script:
   ```powershell
   .\WindowsSuperScript.ps1
   # OR for Windows Server / Domain Controller:
   .\WindowsServerSuperScript.ps1
   ```
4. **Interactive Prompts:**
   - **Step 0 (README Details):** Enter comma-separated lists for Authorized Users, Authorized Admins, Required Services to Keep, Default Password, and confirm if RDP/IIS/FTP are required.
   - **Hardening Steps:** Answer `Y` or `N` to confirm each aggressive hardening section (User/Admin cleanup, Password/Account lockout policy, Full Auditpol logging, UAC/LSA/SMBGhost registry options, Defender & Firewall activation, Service/SMBv1 disabling, Backdoor process termination, MP3/Media file deletion, and Automatic Windows Update policy).

---

## What the Scripts Do (Hardening Modules)

### Linux (`UbuntuSuperScript` & `MintSuperScript`)
- **User & Group Security:** Audits and removes unauthorized local accounts; enforces `sudo`/`admin` group memberships; locks direct `root` login and empty-password accounts.
- **Password & PAM Policy:** Enforces minimum password length (10+), password complexity (`pwquality`), password history (remembers last 5 passwords via `opasswd`), and account lockout (`pam_faillock`).
- **Network Kernel Hardening (`sysctl`):** Disables IP forwarding, ICMP redirects, and source routing; enables TCP SYN cookies (`net.ipv4.tcp_syncookies = 1`) and martian logging.
- **File & System Permissions:** Restricts permissions on `/etc/shadow`, `/etc/gshadow`, `/etc/passwd`, `/etc/group`, and `/etc/sudoers`.
- **SSH Hardening:** Disables root SSH login (`PermitRootLogin no`), empty passwords, and X11 forwarding.
- **Service & Package Purging:** Disables and purges prohibited hacking tools (`nmap`, `netcat`, `wireshark`, `john`, `hydra`, `aircrack-ng`, etc.) and unauthorized server packages (`nginx`, `vsftpd`, `telnetd`, `smbd`, `snmpd`, `cups`, `avahi`).
- **Backdoor & Persistence Cleanup:** Kills running netcat/socat processes, deletes netcat binaries, and restricts cron/at to `root`.
- **Media Cleanup:** Deletes prohibited `.mp3`, `.mp4`, `.avi`, `.mkv`, `.mov`, and `.flac` files in home and temp directories.
- **Host Firewall (UFW):** Sets default-deny incoming and default-allow outgoing policies; allows SSH and user-specified ports.
- **System Audit & Updates:** Configures `unattended-upgrades`, `auditd`, `apparmor`, and `fail2ban`; applies `dconf` auto-refresh settings for Linux Mint Update Manager.

### Windows (`WindowsSuperScript` & `WindowsServerSuperScript`)
- **Account & Privileges:** Audits and removes unauthorized local or Active Directory domain accounts; enforces `Administrators` or `Domain Admins` group membership; resets passwords for authorized users; enforces Active Directory Default Domain Password Policy on DCs.
- **Local Security & Account Policies:** Sets minimum password length (14), maximum password age (90 days), password history (24), and account lockout threshold (10 invalid attempts via `net accounts` and `secedit`).
- **Audit Logging:** Configures full Auditpol logging (`success:enable /failure:enable` across all categories).
- **Security Options & Registry Tweaks:** Enforces maximum UAC prompts, SmartScreen warnings, LSA blank-password restrictions, Anonymous SAM restrictions, and SMBGhost compression fixes.
- **Defender & Firewall:** Enables Windows Defender Real-Time Protection and Windows Defender Firewall on Domain, Public, and Private profiles with default-inbound block rules.
- **Services & Server Roles:** Disables SMBv1, Telnet, FTP, IIS, SNMP, UPnP, and non-standard SMB shares (`USERS$` or custom drive shares); hardens DNS zone transfers and LDAP signing/channel binding on Domain Controllers.
- **Prohibited Software & Media:** Kills backdoor processes (`nc`, `ncat`, `netcat`, `GooseDesktop`) and deletes prohibited MP3/media files under `C:\Users`.
- **Windows Updates:** Enforces automatic Windows Update policy settings (`AUOptions = 4`).

---

## Competition Best Practices

1. **Keep Log Files:** All scripts output logs (`/var/log/cp_*_hardening.log` on Linux, `C:\Windows\Temp\CP-*-Hardening-Log.txt` on Windows).
2. **Double-Check README Requirements:** Never blindly answer `Y` to disable a service (e.g., IIS, FTP, SSH, Apache, RDP) if the README lists it as critical or required.
3. **Re-Check Scoring Engine:** After running the script, check the CyberPatriot Scoring Report to verify points gained and review any penalties.
