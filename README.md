# CyberPatriot Hardening Suite & Guides (CP19 / 2026 Edition)

Welcome to the **CyberPatriot Hardening Suite**. This repository contains five OS-specific, interactive hardening scripts designed to achieve **85–100 points** in CyberPatriot competition rounds with maximum speed, accuracy, and compliance with image-specific README requirements.

> **Note on Operating System Compatibility**: The scripts in this repository are developed and tested specifically for **Ubuntu 20.04 / 22.04 / 24.04 LTS**, **Linux Mint 20 / 21**, **Debian 11 (Bullseye) / 12 (Bookworm)**, **Windows 10 / 11 Workstations**, and **Windows Server 2016 / 2019 / 2022**.

---

## 📜 Script Overview

| Script Name | Target Operating Systems | Language / Shell | Key Features |
| :--- | :--- | :--- | :--- |
| **`UbuntuSuperScript`** | Ubuntu 20.04 / 22.04 / 24.04 LTS | Bash | 17 modules: Step 0 prompts, PAM faillock, `su` restriction, apt dependency guard, sysctl, GRUB boot hardening (`chmod 600 /boot/grub/grub.cfg`, `GRUB_CMDLINE_LINUX` sanitization, `update-grub`), UFW, AppArmor enforce (skipping kept service families), AST/tokenizer backdoor sweep (crontabs & user/system units), service family map, rootkit/antivirus scanners (`rkhunter`, `chkrootkit`, `clamscan`), `snap refresh`, required service hardening (Apache, vsftpd with `allow_writeable_chroot` & port 21 check, Samba, MySQL/MariaDB, PHP isolated syntax linting `php -n -c`, Nginx with drop-in config rollback), defenses pre-install & auditd rules, system upgrades with unattended-upgrades pre-install & reconfigure, post-run health check, final verification printout |
| **`MintSuperScript`** | Linux Mint 20 / 21 | Bash | 17 modules: Step 0 prompts, Mint Update Manager dconf database, `su` restriction, apt dependency guard, sysctl, GRUB boot hardening (`chmod 600 /boot/grub/grub.cfg`, `GRUB_CMDLINE_LINUX` sanitization, `update-grub`), UFW, AppArmor enforce (skipping kept service families), AST/tokenizer backdoor sweep (crontabs & user/system units), rootkit/antivirus scanners (`rkhunter`, `chkrootkit`, `clamscan`), required service hardening (Apache, vsftpd with `allow_writeable_chroot` & port 21 check, Samba, MySQL/MariaDB, PHP isolated syntax linting `php -n -c`, Nginx with drop-in config rollback), defenses pre-install & auditd rules, system upgrades with unattended-upgrades pre-install & reconfigure, post-run health check, final verification printout |
| **`DebianSuperScript`** | Debian 11 (Bullseye) / 12 (Bookworm) | Bash | 17 modules: Step 0 prompts, `su` restriction, apt dependency guard (Debian meta-packages protected), sysctl, GRUB boot hardening, UFW, AppArmor enforce (skipping kept service families), AST/tokenizer backdoor sweep (crontabs & user/system units), rootkit/antivirus scanners (`rkhunter`, `chkrootkit`, `clamscan`), required service hardening (Apache, vsftpd with `allow_writeable_chroot` & port 21 check, Samba, MySQL/MariaDB, PHP isolated syntax linting `php -n -c`, Nginx with drop-in config rollback), defenses pre-install & auditd rules, system upgrades with unattended-upgrades pre-install & reconfigure, post-run health check, final verification printout. No `snap refresh` (Debian does not use snap by default). Apt sources filtered against official Debian mirrors (`deb.debian.org`, `security.debian.org`). |
| **`WindowsSuperScript`** | Windows 10 / 11 Workstations | PowerShell 5.1+ | Forensics snapshot (`CP-Evidence.txt`), SID-based User Rights Assignment via `secedit`, 40+ security registry options, UAC max, Defender & Inbound Firewall rule pruning, Adapter Bindings hardening (`-SmbRequired` / Step 0 prompt, never touches `ms_tcpip`), OneDrive management (startup/sync disabled, no uninstall), 7-Zip/LibreOffice upgrades |
| **`WindowsServerSuperScript`** | Windows Server 2016 / 2019 / 2022 | PowerShell 5.1+ | Domain Controller policy, Domain Admins enforcement, LDAP signing/channel binding, DNS/IIS hardening, Windows Update policy, Server Features, Inbound Firewall rule pruning with role/port safeguards, Adapter Bindings hardening (`-SmbRequired` / Step 0 prompt, never touches `ms_tcpip`), Security Registry parity (`AlwaysInstallElevated`, `AllowTelemetry`, Screen Saver, WinRM, Netlogon) |

---

## ⚡ Quick Start & Execution

### 1. Ubuntu SuperScript
```bash
sudo chmod +x UbuntuSuperScript
sudo ./UbuntuSuperScript
```
*Non-Interactive Mode:*
```bash
sudo ./UbuntuSuperScript -y --users "alice bob" --admins "alice" --keep "ssh apache2" --ports "22/tcp 80/tcp"
```
*Self-Test Dry Run (safe, non-interactive, dry-run only, writes to /tmp, works as non-root):*
```bash
./UbuntuSuperScript --self-test
```
To reset selected accounts' passwords in non-interactive mode, add `--reset-password-users "alice"` and provide a new password with `--password "your-new-password"`. To add required supplementary group memberships, use `--group-memberships "alice:developers"`. Interactive mode prompts for these selections; passwords are never set to a built-in default.
In interactive mode, the script reviews each existing user and administrator with separate yes/no prompts for account deletion and admin-privilege removal. To add accounts, answer yes when asked and enter usernames separated by spaces or commas.

### 2. Linux Mint SuperScript
```bash
sudo chmod +x MintSuperScript
sudo ./MintSuperScript
```
*Non-Interactive Mode:*
```bash
sudo ./MintSuperScript -y --users "mint alice" --admins "mint" --keep "ssh" --ports "22/tcp"
```
*Self-Test Dry Run (safe, non-interactive, dry-run only, writes to /tmp, works as non-root):*
```bash
./MintSuperScript --self-test
```
To reset selected accounts' passwords in non-interactive mode, add `--reset-password-users "alice"` and provide a new password with `--password "your-new-password"`. To add required supplementary group memberships, use `--group-memberships "alice:developers"`. Interactive mode prompts for these selections; passwords are never set to a built-in default.
In interactive mode, the script reviews each existing user and administrator with separate yes/no prompts for account deletion and admin-privilege removal. To add accounts, answer yes when asked and enter usernames separated by spaces or commas.

### 3. Debian SuperScript
```bash
sudo chmod +x DebianSuperScript
sudo ./DebianSuperScript
```
*Non-Interactive Mode:*
```bash
sudo ./DebianSuperScript -y --users "debian alice" --admins "debian" --keep "ssh" --ports "22/tcp"
```
*Self-Test Dry Run (safe, non-interactive, dry-run only, writes to /tmp, works as non-root):*
```bash
./DebianSuperScript --self-test
```
To reset selected accounts' passwords in non-interactive mode, add `--reset-password-users "alice"` and provide a new password with `--password "your-new-password"`. To add required supplementary group memberships, use `--group-memberships "alice:developers"`. Interactive mode prompts for these selections; passwords are never set to a built-in default.
In interactive mode, the script reviews each existing user and administrator with separate yes/no prompts for account deletion and admin-privilege removal. To add accounts, answer yes when asked and enter usernames separated by spaces or commas.

### 4. Windows SuperScript (Windows 10 / 11)
Launch PowerShell as **Administrator**:
```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force
# Direct execution (as-is from repository root):
& ([scriptblock]::Create((Get-Content .\WindowsSuperScript -Raw)))

# Or copy/rename with .ps1 extension:
Copy-Item .\WindowsSuperScript .\WindowsSuperScript.ps1
.\WindowsSuperScript.ps1
```
*Non-Interactive Mode:*
```powershell
& ([scriptblock]::Create((Get-Content .\WindowsSuperScript -Raw))) -Force -SmbRequired -Users "alice,bob" -Admins "alice" -Keep "Spooler,sshd" -Ports "22,80" -Password "Cyb3r-Pat!2026-xQz"
```
*Self-Test Dry Run (safe, non-interactive, dry-run only, makes no changes):*
```powershell
& ([scriptblock]::Create((Get-Content .\WindowsSuperScript -Raw))) -SelfTest -SmbRequired
```

### 5. Windows Server SuperScript (Server 2016/2019/2022)
Launch PowerShell as **Administrator**:
```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force
# Direct execution (as-is from repository root):
& ([scriptblock]::Create((Get-Content .\WindowsServerSuperScript -Raw)))

# Or copy/rename with .ps1 extension:
Copy-Item .\WindowsServerSuperScript .\WindowsServerSuperScript.ps1
.\WindowsServerSuperScript.ps1
```
*Non-Interactive Mode:*
```powershell
& ([scriptblock]::Create((Get-Content .\WindowsServerSuperScript -Raw))) -Force -SmbRequired -Users "alice,bob" -Admins "alice" -Keep "DNS,DHCP,W3SVC" -Ports "53,80" -Password "Cyb3r-Pat!2026-xQz"
```
*Self-Test Dry Run (safe, non-interactive, dry-run only, makes no changes):*
```powershell
& ([scriptblock]::Create((Get-Content .\WindowsServerSuperScript -Raw))) -SelfTest -SmbRequired
```

---

## 🐧 Linux Module Guide

The following guide applies to `DebianSuperScript`, `UbuntuSuperScript`, and `MintSuperScript`. Module numbers are the same in all three scripts. Step 0 gathers scenario-specific information before the modules run.

| Module | What it does |
| :--- | :--- |
| **1. User Accounts & Groups** | Reviews local accounts and administrator privileges. In interactive mode, asks separately whether to delete each account and whether to remove admin privileges; both default to **No**. It can add a space- or comma-separated list of users, apply requested `user:group` memberships, and lock empty-password accounts. Non-interactive account deletion requires an explicit `--users` list. |
| **2. Password Security & Aging** | Applies password aging settings to local accounts and prompts for new passwords only for selected accounts. Non-interactive resets use `--reset-password-users` and `--password`; no built-in default password is used. |
| **3. PAM, Login Policy & `su` Restriction** | Sets password length/complexity and history, configures failed-login lockout where supported, removes password `nullok` options, and limits `su` access to the administrative group. |
| **4. Sudoers & Privilege Delegation** | Audits `/etc/sudoers` and its drop-ins, removes `NOPASSWD` and `!authenticate`, comments out unauthorized broad `ALL` grants, validates syntax with `visudo`, and enables sudo session logging and pseudo-terminal use. **This module does not configure IPv4.** |
| **5. Sysctl Kernel Network Hardening** | Configures kernel network and process protections. This is where **IPv4 TCP SYN cookies are enabled** (`net.ipv4.tcp_syncookies=1`) and **IPv4 forwarding is disabled** (`net.ipv4.ip_forward=0`), alongside IPv4/IPv6 redirect and source-route protections, address randomization, and core-dump protections. It applies the settings and verifies the two requested IPv4 values. |
| **6. File Permissions & Home Directories** | Tightens permissions on account, sudo, cron, SSH, and GRUB files; sanitizes risky GRUB boot parameters; restricts home-directory access; removes world-writable permissions where appropriate; and records SUID files and file capabilities for review. |
| **7. SSH Server Hardening & Authorized Keys** | If SSH is installed, audits authorized key files and hardens the SSH daemon (for example, disables root/empty-password login and X11 forwarding, limits authentication attempts, and optionally restricts logins to configured users). It validates the configuration and rolls it back if `sshd -t` fails. |
| **8. Prohibited Software & Service Removal** | Finds configured prohibited tools and unrequired server packages, checks whether removal would affect protected or required software, and purges candidates that pass the dependency guard. Required services/packages can be declared in setup prompts. |
| **9. Backdoor & Persistence Sweep** | Checks for netcat/socat processes, suspicious cron and systemd persistence entries, and executables or command patterns associated with persistence. Suspicious findings are surfaced for review; package-owned system files are protected from automatic cleanup. |
| **10. Prohibited Media & Archive Search** | Searches selected non-system locations for prohibited media, torrents, archives, and installers, preserving README/forensics files and protected application/service paths. Review the competition image’s rules before approving deletion. |
| **11. APT Sources, Hosts, DNS & UFW Firewall** | Filters package sources to the distribution’s configured official repositories, checks repository signatures, preserves the active machine hostname in `/etc/hosts`, configures DNS, and installs/configures UFW with deny-incoming/allow-outgoing defaults and requested ports. It enables UFW and checks that its status is active. **Review custom repositories, DNS requirements, SSH access, and required ports before accepting these changes.** |
| **12. Rootkit & Malware Scanners** | Optionally installs/runs `rkhunter`, `chkrootkit`, and ClamAV scans with timeouts. Results are logged for review; scanner findings are report-only and are not automatically treated as proof of infection. |
| **13. Required Service Hardening** | Applies configuration hardening only to services identified as required (Apache, Nginx, FTP, Samba, MySQL/MariaDB, or PHP). It uses service-specific configuration checks and restores backups when validation fails. |
| **14. Defenses, Logging & Access Controls** | Enables or configures AppArmor, auditd rules, fail2ban, and rsyslog where available; disables desktop guest/automatic login; blocks selected removable-storage/expansion modules; masks Ctrl-Alt-Del reboot; disables core dumps; and preserves VM guest integration services. |
| **15. Package Upgrades & Security Updates** | Refreshes APT metadata, installs available package/security updates, performs a distribution upgrade, protects important packages during guarded autoremove, configures unattended upgrades, and targets installed Chromium and OpenSSH packages for updates. Ubuntu/Mint also refresh Chromium via Snap when installed; Mint configures Update Manager automatic updates. Update success depends on network and repository availability and is reported in the log. |
| **16. Post-Run System Health Check** | Checks required services and ports, SSH and sudoers syntax, configured users/admins, and whether UFW is active; reports pass/fail/skip results. |
| **17. Final System Verification Report** | Reports failed systemd units, listening ports, administrator/UID-0 accounts, IPv4 SYN-cookie/forwarding values, UFW status, effective SSH settings, remaining upgradable packages, and whether a reboot is pending. |

---

## 🛡️ Feature & Capability Matrix

| Feature / Hardening Control | `UbuntuSuperScript` | `MintSuperScript` | `DebianSuperScript` | `WindowsSuperScript` | `WindowsServerSuperScript` |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Forensics Gate & Read-Only Network Scan** | Yes | Yes | Yes | Yes (`CP-Evidence.txt`) | Yes (`CP-Evidence.txt`) |
| **Step 0 Interactive Specifics Prompts** | Yes | Yes | Yes | Yes | Yes |
| **FAST MODE (Timed Judgment Prompts)** | Yes (10s auto-default) | Yes (10s auto-default) | Yes (10s auto-default) | Yes (10s auto-default) | Yes (10s auto-default) |
| **Self-Test Dry-Run Flag** | `--self-test` | `--self-test` | `--self-test` | `-SelfTest` | `-SelfTest` |
| **User & Group Remediation (Module 1 / Sec 1)** | Per-account delete/admin prompts; add-user list | Per-account delete/admin prompts; add-user list | Per-account delete/admin prompts; add-user list | Yes | Yes (AD & Local) |
| **Password Aging & Complexity (Module 2 / Sec 2)** | Min 12, Max 90, Remember 5 (`pam_pwhistory`) | Min 12, Max 90, Remember 5 (`pam_pwhistory`) | Min 12, Max 90, Remember 5 (`pam_pwhistory`) | Min 14, Max 90, Remember 24 | Min 14, Max 90, Remember 24 |
| **Account Lockout Policy (Module 3 / Sec 2)** | `pam_faillock` (5 attempts / 15 min) | `pam_faillock` (5 attempts / 15 min) | `pam_faillock` (5 attempts / 15 min) | `net accounts` (10 attempts / 30 min) | `net accounts` / AD Domain Policy (10 / 30 min) |
| **`su` Privilege Restriction (Module 3)** | `pam_wheel.so` (group=sudo/admin) | `pam_wheel.so` (group=sudo/admin) | `pam_wheel.so` (group=sudo/admin) | N/A | N/A |
| **Sudoers ALL Rule & NOPASSWD Cleanup (Module 4)** | Yes (`visudo -cf` check) | Yes (`visudo -cf` check) | Yes (`visudo -cf` check) | N/A | N/A |
| **User Rights Assignment (SIDs) (Sec 3)** | N/A | N/A | N/A | Full `secedit` (33+ SIDs) | Full `secedit` (33+ SIDs, DC-aware) |
| **Security Registry Options (40+) (Sec 4)** | N/A | N/A | N/A | UAC Max, CAD, LSA, WDigest, AutoAdminLogon=0, AlwaysInstallElevated=0, AllowTelemetry=0 | UAC Max, CAD, LSA, LDAP Signing, Printer Driver Policy, Wecsvc, AlwaysInstallElevated=0, AllowTelemetry=0, Screen Saver, WinRM, Netlogon |
| **System File Permissions & GRUB Hardening (Module 6)** | `chmod 600 /boot/grub/grub.cfg`, remove `init=`, `single`, `rw init` from `GRUB_CMDLINE_LINUX*`, `update-grub`, shadow/passwd/sudoers perms | `chmod 600 /boot/grub/grub.cfg`, remove `init=`, `single`, `rw init` from `GRUB_CMDLINE_LINUX*`, `update-grub`, shadow/passwd/sudoers perms | Same as Ubuntu | N/A | N/A |
| **Sysctl Kernel Hardening (Module 5)** | IPv4 TCP SYN cookies enabled, IPv4 forwarding disabled; IPv4 + IPv6 hardening verified | Same as Ubuntu | Same as Ubuntu | N/A | N/A |
| **Firewall Configuration (Module 11 / Sec 5)** | UFW installed, default deny, enabled & status verified; rules pruned | Same as Ubuntu | Same as Ubuntu | Defender Firewall (App Rules Pruned) | Defender Firewall (App Rules Pruned, Role/Port Safeguards) |
| **Adapter Bindings Hardening (Sec 6)** | N/A | N/A | N/A | Disables bindings; preserves `ms_server`/`ms_msclient` only if `-SmbRequired`; never touches `ms_tcpip` | Disables bindings; preserves `ms_server`/`ms_msclient` only if `-SmbRequired`; never touches `ms_tcpip` |
| **Backdoor & Persistence Sweep (Module 9 / Sec 7)** | Netcat `dpkg -S`, Tokenizer/AST Cron & Systemd Sweep (/root & /home user units) | Netcat `dpkg -S`, Tokenizer/AST Cron & Systemd Sweep (/root & /home user units) | Same as Ubuntu | IFEO, WMI, Tasks, Run Keys, Startup Folders | IFEO, WMI, Tasks, Run Keys, Startup Folders |
| **Prohibited Software Removal (Module 8 / Sec 8)** | Purges prohibited & unrequired services | Purges prohibited & unrequired services | Purges prohibited & unrequired services | CCleaner, WebCompanion, OpenTFTP, BitTorrent, Wireshark | CCleaner, WebCompanion, OpenTFTP, BitTorrent, Wireshark |
| **Software Version Upgrades (Module 15 / Sec 9)** | `apt-get upgrade`, `dist-upgrade`, guarded `autoremove` | `apt-get upgrade`, `dist-upgrade`, guarded `autoremove` | `apt-get upgrade`, `dist-upgrade`, guarded `autoremove` | 7-Zip, LibreOffice (`winget`/`choco`, deduplicated) | 7-Zip, LibreOffice (`winget`/`choco`, deduplicated) |
| **Rootkit & Malware Scanning (Module 12)** | Module 12: `rkhunter`, `chkrootkit`, `clamscan` (600s timeouts, report-only), `snap refresh` | Module 12: `rkhunter`, `chkrootkit`, `clamscan` (600s timeouts, report-only) | Module 12: `rkhunter`, `chkrootkit`, `clamscan` (600s timeouts, report-only, no snap) | N/A | N/A |
| **Required Service Hardening (Module 13)** | Module 13: Apache (`TraceEnable Off`), vsftpd (`anonymous_enable=NO`, `chroot_local_user=YES`, `allow_writeable_chroot=YES`, active/port 21 test), Samba, MySQL/MariaDB (`bind-address 127.0.0.1`), PHP (`php -n -c` linting), Nginx (`server_tokens` drop-in rollback) | Module 13: Apache (`TraceEnable Off`), vsftpd (`anonymous_enable=NO`, `chroot_local_user=YES`, `allow_writeable_chroot=YES`, active/port 21 test), Samba, MySQL/MariaDB (`bind-address 127.0.0.1`), PHP (`php -n -c` linting), Nginx (`server_tokens` drop-in rollback) | Same as Ubuntu | N/A | DNS, IIS, Wecsvc hardening |
| **Defenses, Logging & Access Controls (Module 14)** | Module 14: Pre-install defense pkgs, AppArmor (`aa-enforce` skipping kept family profiles), auditd rules, fail2ban, rsyslog, LightDM/GDM guest/autologin off, `usb-storage`/firewire/thunderbolt blacklist, coredump off | Module 14: Pre-install defense pkgs, AppArmor (`aa-enforce` skipping kept family profiles), auditd rules, fail2ban, rsyslog, LightDM/GDM guest/autologin off, `usb-storage`/firewire/thunderbolt blacklist, coredump off | Same as Ubuntu | Exploit Guard, Defender Realtime/Cloud/MAPS, LSA Protection | Exploit Guard, Defender Realtime/Cloud/MAPS, LSA Protection, LDAP Signing/Enforcement |
| **Package Upgrades & Policies (Module 15)** | Security/repository upgrades; installed Chromium (APT or snap) and OpenSSH targeted updates; `dist-upgrade`, guarded `autoremove`, unattended-upgrades | Same as Ubuntu plus Mint Update Manager auto-refresh/auto-apply | Security/repository upgrades; installed Chromium and OpenSSH targeted updates; `dist-upgrade`, guarded `autoremove`, unattended-upgrades |
| **Post-Run System Health Check (Module 16 / Sec 10)** | Module 16: Required services active, required ports listening, `sshd -t`, `visudo -c`, users exist, admin sudo/su sanity, UFW active | Module 16: Required services active, required ports listening, `sshd -t`, `visudo -c`, users exist, admin sudo/su sanity, UFW active | Same as Ubuntu | Section 10: Automatic Health Check (Firewall, Defender, RDP, Admins) | Section 10: Automatic Health Check (Firewall, Defender, RDP, Domain Admins) |
| **Final System Verification Report (Module 17 / Sec 11)** | Failed units, listening ports, sudo/UID 0 accounts, IPv4 sysctls, active UFW, effective `sshd -T`, upgradable packages, pending reboot | Same as Ubuntu | Same as Ubuntu | Section 11: Final Verification Report (Shares, Firewall profiles) | Section 11: Final Verification Report (Shares, Firewall profiles) |
| **VM Integration & Snapshot Safety** | Open-VM-Tools, VBoxGuest, Qemu-GA & Hyper-V daemons protected by dependency guard; snapshot integration active | Open-VM-Tools, VBoxGuest, Qemu-GA & Hyper-V daemons protected by dependency guard; snapshot integration active | Open-VM-Tools, VBoxGuest, Qemu-GA & Hyper-V daemons protected by dependency guard; snapshot integration active | Volume Shadow Copy (VSS) & swprv set to Manual/Healthy; VMTools & VBoxService protected; SeServiceLogonRight grants Local/Network Service | Volume Shadow Copy (VSS) & swprv set to Manual/Healthy; VMTools & VBoxService protected; SeServiceLogonRight grants Local/Network Service |

---

## 🔍 Scored Competition Item Coverage Matrix (Automated / Report-Only / Manual)

| Category / Scored Item | Windows (`WindowsSuperScript` & `Server`) | Linux (`UbuntuSuperScript`, `MintSuperScript` & `DebianSuperScript`) | Status |
| :--- | :--- | :--- | :--- |
| **Forensics Questions** | Discovers Forensics Q files on Desktop | Discovers Forensics Q files on Desktop | **Manual** |
| **Unauthorized Users / Admins** | Audit & remove non-authorized users/admins | Audit & remove non-authorized users/admins (Module 1) | **Automated** |
| **User Passwords & Complexity** | Enforces complexity, min age, lockout, no null | Enforces complexity, aging, `pam_pwhistory`, `no nullok` (Modules 2, 3) | **Automated** |
| **Audit Policies** | Credential validation, logon failure, all categories | `auditd` enabled and configured for identity/sudoers/logins/time (Module 14) | **Automated** |
| **UAC & Security Policies** | UAC secure desktop, CTRL+ALT+DEL, hide last user | `pam_wheel` for `su`, sudoers NOPASSWD cleanup (Modules 3, 4) | **Automated** |
| **Firewall & Network Security** | Defender Firewall enabled, consumer app rules pruned (Server protects required roles/ports), Adapter Bindings hardened | UFW default deny, pruned unallowed rules (Module 11) | **Automated** |
| **Remote Access (RDP / SSH)** | RDP disabled or NLA+SSL required per README | SSH root login disabled, protocol 2, AllowUsers (Module 7) | **Automated** |
| **Insecure Services & Shares** | SMB1 off, W3SVC/FTP/SNMP/Telnet disabled | Unauthorized services purged, family map respected (Module 8) | **Automated** |
| **Required Service Hardening** | DNS, IIS, Wecsvc hardened | Apache, vsftpd (`chroot_local_user=YES`, `allow_writeable_chroot=YES`, active & port 21 check), Samba, MySQL/MariaDB, PHP (`php -n -c` linting), Nginx (drop-in config rollback) (Module 13) | **Automated** |
| **System Defenses & Access** | Exploit Guard, Defender, LSA Protection | Pre-install defense packages, AppArmor enforce (skips kept family profiles), fail2ban, rsyslog, guest/autologin off, kernel blacklists, coredumps off (Module 14) | **Automated** |
| **Prohibited Software Removal** | CCleaner, WebCompanion, OpenTFTP, BitTorrent, Wireshark | Prohibited tools & unneeded service packages purged (Module 8) | **Automated** |
| **Prohibited File Search** | Non-system MP3s, installers, archives deleted | Non-system MP3s, installers, archives deleted (Module 10) | **Automated** |
| **Software Upgrades** | 7-Zip, LibreOffice, Firefox updated via winget/choco (deduplicated, native exit codes) | `apt-get upgrade`, `dist-upgrade`, guarded `autoremove`, pre-install & reconfigure `unattended-upgrades`, `snap refresh` (Module 15) | **Automated** |
| **Malware & Backdoor Sweeps** | IFEO, WMI, Tasks, Run keys swept | Netcat purged, AST/tokenizer cron & user/system units sweep, `rkhunter`/`chkrootkit`/`clamscan` (Modules 9, 12) | **Automated / Report-Only** |
| **Health Check & Verification** | Health check (Section 10) and Verification (Section 11) | Service health check (Module 16) and system verification report (Module 17) | **Automated** |
| **Localhost Driver License & Perl Backdoor Hunt** | N/A | Manual investigation required | **Manual** |

---

## 🧪 Quality Assurance & Test Verification

All scripts in this repository undergo automated verification checks prior to release:

1. **Bash Syntax & Shellcheck Verification**:
   - `UbuntuSuperScript`, `MintSuperScript`, and `DebianSuperScript` all pass `bash -n` checks with zero syntax errors.
   - All three Linux scripts pass `shellcheck -S error` with zero errors.
   ```bash
   bash -n UbuntuSuperScript
   bash -n MintSuperScript
   bash -n DebianSuperScript
   shellcheck -S error UbuntuSuperScript
   shellcheck -S error MintSuperScript
   shellcheck -S error DebianSuperScript
   ```
2. **Tokenizer & Persistence Integrity**:
   - `check_suspicious_file` and `clean_suspicious_file` use POSIX `awk` tokenizers to parse command paths rather than relying on brittle substring matching.
   - Accurately matches single-argument script interpreters (`python -c`, `python3 -c`, `perl -e`, `ruby -e`, `php -r`), reverse shells (`nc -e`, `bash -i >& /dev/tcp/`), download pipes (`curl ... | sh`), and `ExecStart` binaries under `/tmp`, `/dev/shm`, and `/var/tmp`.
   - Preserves legitimate commands (`# run once a day`, `ExecStart=/bin/sync`, `ExecStart=/usr/bin/python3 /usr/lib/ubuntu-advantage/x.py`, `0 3 * * * root rm -rf /tmp/cache`, `17 * * * * root find /tmp/ -mtime +7 -delete`).
   - Recursively sweeps user systemd units under both `/root/.config/systemd/user` and `/home/*/.config/systemd/user`.
3. **Linux Dry-Run Verification**:
   - Verified via `./UbuntuSuperScript --self-test`, `./MintSuperScript --self-test`, and `./DebianSuperScript --self-test` as non-root with zero destructive actions.
   - All modules report `"skipped: no network/changes in self-test"` during dry-run testing.
4. **Configuration Helper Verification (`set_kv` & `set_ini`)**:
   - Verified across tab-separated keys (`PASS_MAX_DAYS 90`), commented keys (`# minlen = 8`), key-value assignment (`anonymous_enable=NO`), idempotent re-runs, complex values with backslashes and special characters, and section isolation (`[global]` vs `[share]`, `[mysqld]` vs `[client]`).
5. **PowerShell AST Parser Validation**:
   - Both `WindowsSuperScript` and `WindowsServerSuperScript` are validated against `[System.Management.Automation.Language.Parser]::ParseFile` on Windows PowerShell 5.1 with zero syntax errors.
   - *(Note: PSScriptAnalyzer module is not installed in the testing environment and is marked as untested; AST parser validation and -SelfTest execution were directly verified.)*
6. **Deduplication & Safety Auditing**:
   - Verified that OneDrive management blocks and 7-Zip/LibreOffice upgrade blocks have an exact occurrence count of 1.
   - Confirmed OneDrive handling terminates processes and disables startup/sync policies without triggering breaking uninstalls.
   - Standardized native CLI calls (`winget`, `choco`, `UsoClient`) without PowerShell `-ErrorAction` flags and with `$LASTEXITCODE` validation.
   - Network adapter bindings properly skip `ms_server` and `ms_msclient` when `-SmbRequired` is set; never modifies `ms_tcpip`.
7. **Windows Dry-Run Verification**:
   - Verified via `-SelfTest` (with and without `-SmbRequired`) on both `WindowsSuperScript` and `WindowsServerSuperScript`, generating review output without modifying system configurations.
