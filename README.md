# CyberPatriot Hardening Suite & Guides (CP19 / 2026 Edition)

Welcome to the **CyberPatriot Hardening Suite**. This repository contains four OS-specific, interactive hardening scripts designed to achieve **85–100 points** in CyberPatriot competition rounds with maximum speed, accuracy, and compliance with image-specific README requirements.

> **Note on Operating System Compatibility**: The scripts in this repository are developed and tested specifically for **Ubuntu 20.04 / 22.04 / 24.04 LTS**, **Linux Mint 20 / 21**, **Windows 10 / 11 Workstations**, and **Windows Server 2016 / 2019 / 2022**. They are not officially supported for Debian or other non-targeted Linux distributions.

---

## 📜 Script Overview

| Script Name | Target Operating Systems | Language / Shell | Key Features |
| :--- | :--- | :--- | :--- |
| **`UbuntuSuperScript`** | Ubuntu 20.04 / 22.04 / 24.04 LTS | Bash | Step 0 prompts, PAM faillock, `su` restriction, apt dependency guard, sysctl, UFW, AppArmor, backdoor sweep, service family map, rootkit/antivirus scanners, snap refresh |
| **`MintSuperScript`** | Linux Mint 20 / 21 | Bash | Step 0 prompts, Mint Update Manager dconf database, `su` restriction, apt dependency guard, UFW, AppArmor, backdoor sweep, rootkit/antivirus scanners |
| **`WindowsSuperScript`** | Windows 10 / 11 Workstations | PowerShell 5.1+ | Forensics snapshot (`CP-Evidence.txt`), SID-based User Rights Assignment via `secedit`, 40+ security registry options, UAC max, Defender & Firewall, OneDrive & App Removal |
| **`WindowsServerSuperScript`** | Windows Server 2016 / 2019 / 2022 | PowerShell 5.1+ | Domain Controller policy, Domain Admins enforcement, LDAP signing/channel binding, DNS/IIS hardening, Windows Update policy, Server Features, Printer driver policy, Wecsvc |

---

## ⚡ Quick Start & Execution

### 1. Ubuntu SuperScript
```bash
sudo chmod +x UbuntuSuperScript
sudo ./UbuntuSuperScript
```
*Non-Interactive Mode:*
```bash
sudo ./UbuntuSuperScript -y --users "alice bob" --admins "alice" --keep "ssh apache2" --ports "22/tcp 80/tcp" --password "Cyb3r-Pat!2026-xQz"
```
*Self-Test Dry Run (safe, non-interactive, dry-run only, writes to /tmp, works as non-root):*
```bash
./UbuntuSuperScript --self-test
```

### 2. Linux Mint SuperScript
```bash
sudo chmod +x MintSuperScript
sudo ./MintSuperScript
```
*Non-Interactive Mode:*
```bash
sudo ./MintSuperScript -y --users "mint alice" --admins "mint" --keep "ssh" --ports "22/tcp" --password "Cyb3r-Pat!2026-xQz"
```
*Self-Test Dry Run (safe, non-interactive, dry-run only, writes to /tmp, works as non-root):*
```bash
./MintSuperScript --self-test
```

### 3. Windows SuperScript (Windows 10 / 11)
Launch PowerShell as **Administrator**:
```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force
.\WindowsSuperScript.ps1
```
*Non-Interactive Mode:*
```powershell
.\WindowsSuperScript.ps1 -Force -Users "alice,bob" -Admins "alice" -Keep "Spooler,sshd" -Ports "22,80" -Password "Cyb3r-Pat!2026-xQz"
```
*Self-Test Dry Run:*
```powershell
.\WindowsSuperScript.ps1 -SelfTest
```

### 4. Windows Server SuperScript (Server 2016/2019/2022)
Launch PowerShell as **Administrator**:
```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force
.\WindowsServerSuperScript.ps1
```
*Non-Interactive Mode:*
```powershell
.\WindowsServerSuperScript.ps1 -Force -Users "alice,bob" -Admins "alice" -Keep "DNS,DHCP,W3SVC" -Ports "53,80" -Password "Cyb3r-Pat!2026-xQz"
```
*Self-Test Dry Run:*
```powershell
.\WindowsServerSuperScript.ps1 -SelfTest
```

---

## 🛡️ Feature & Capability Matrix

| Feature / Hardening Control | `UbuntuSuperScript` | `MintSuperScript` | `WindowsSuperScript` | `WindowsServerSuperScript` |
| :--- | :---: | :---: | :---: | :---: |
| **Forensics Gate & Read-Only Network Scan** | Yes | Yes | Yes (`CP-Evidence.txt`) | Yes (`CP-Evidence.txt`) |
| **Step 0 Interactive Specifics Prompts** | Yes | Yes | Yes | Yes |
| **FAST MODE (Timed Judgment Prompts)** | Yes (10s auto-default) | Yes (10s auto-default) | Yes (10s auto-default) | Yes (10s auto-default) |
| **Self-Test Dry-Run Flag** | `--self-test` | `--self-test` | `-SelfTest` | `-SelfTest` |
| **User & Group Remediation** | Yes | Yes | Yes | Yes (AD & Local) |
| **Password Aging & Complexity** | Min 10, Max 90, Remember 5 (`pam_pwhistory`) | Min 10, Max 90, Remember 5 (`pam_pwhistory`) | Min 14, Max 90, Remember 24 | Min 14, Max 90, Remember 24 |
| **Account Lockout Policy** | `pam_faillock` (5 attempts / 15 min) | `pam_faillock` (5 attempts / 15 min) | `net accounts` (10 attempts / 30 min) | `net accounts` / AD Domain Policy (10 / 30 min) |
| **`su` Privilege Restriction** | `pam_wheel.so` (group=sudo/admin) | `pam_wheel.so` (group=sudo/admin) | N/A | N/A |
| **Sudoers ALL Rule & NOPASSWD Cleanup** | Yes (`visudo -cf` check) | Yes (`visudo -cf` check) | N/A | N/A |
| **User Rights Assignment (SIDs)** | N/A | N/A | Full `secedit` (33+ SIDs) | Full `secedit` (33+ SIDs, DC-aware) |
| **Security Registry Options (40+)** | N/A | N/A | UAC Max, CAD, LSA, WDigest, AutoAdminLogon=0 | UAC Max, CAD, LSA, LDAP Signing, Printer Driver Policy, Wecsvc |
| **Sysctl Kernel Hardening** | IPv4 + IPv6 | IPv4 + IPv6 | N/A | N/A |
| **Firewall Configuration** | UFW Default Deny & Pruning | UFW Default Deny & Pruning | Defender Firewall (App Rules Disabled) | Defender Firewall (App Rules Disabled) |
| **Backdoor & Persistence Sweep** | Netcat `dpkg -S`, Cron, Systemd | Netcat `dpkg -S`, Cron, Systemd | IFEO, WMI, Tasks, Run Keys, Startup Folders | IFEO, WMI, Tasks, Run Keys, Startup Folders |
| **Prohibited Software Removal** | Purges prohibited & unrequired services | Purges prohibited & unrequired services | CCleaner, WebCompanion, OpenTFTP, BitTorrent, Wireshark | CCleaner, WebCompanion, OpenTFTP, BitTorrent, Wireshark |
| **Software Version Audit & Upgrades** | `apt-get upgrade` | `apt-get upgrade` | 7-Zip, LibreOffice (`winget`/`choco`) | 7-Zip, LibreOffice (`winget`/`choco`) |
| **Rootkit & Malware Scanning** | `rkhunter`, `chkrootkit`, `clamscan`, `snap refresh` | `rkhunter`, `chkrootkit`, `clamscan` | N/A | N/A |

---

## 🔍 Scored Competition Item Coverage Matrix (Automated / Report-Only / Manual)

| Category / Scored Item | Windows (`WindowsSuperScript` & `Server`) | Linux (`UbuntuSuperScript` & `Mint`) | Status |
| :--- | :--- | :--- | :--- |
| **Forensics Questions** | Discovers Forensics Q files on Desktop | Discovers Forensics Q files on Desktop | **Manual** |
| **Unauthorized Users / Admins** | Audit & remove non-authorized users/admins | Audit & remove non-authorized users/admins | **Automated** |
| **User Passwords & Complexity** | Enforces complexity, min age, lockout, no null | Enforces complexity, aging, `pam_pwhistory`, `no nullok` | **Automated** |
| **Audit Policies** | Credential validation, logon failure, all categories | `auditd` enabled and configured | **Automated** |
| **UAC & Security Policies** | UAC secure desktop, CTRL+ALT+DEL, hide last user | `pam_wheel` for `su`, sudoers NOPASSWD cleanup | **Automated** |
| **Firewall & Network Security** | Defender Firewall enabled, app rules pruned | UFW default deny, pruned unallowed rules | **Automated** |
| **Remote Access (RDP / SSH)** | RDP disabled or NLA+SSL required per README | SSH root login disabled, protocol 2, AllowUsers | **Automated** |
| **Insecure Services & Shares** | SMB1 off, W3SVC/FTP/SNMP/Telnet disabled | Unauthorized services purged, family map respected | **Automated** |
| **Prohibited Software Removal** | CCleaner, WebCompanion, OpenTFTP, BitTorrent, Wireshark | Prohibited tools & unneeded service packages purged | **Automated** |
| **Prohibited File Search** | Non-system MP3s, installers, archives deleted | Non-system MP3s, installers, archives deleted | **Automated** |
| **Software Upgrades** | 7-Zip, LibreOffice, Firefox updated via winget/choco | APT package upgrade simulation & execution | **Automated** |
| **Malware & Backdoor Sweeps** | IFEO, WMI, Tasks, Run keys swept | Netcat purged, anchored cron/systemd sweep, `rkhunter`/`chkrootkit`/`clamscan` | **Automated / Report-Only** |
| **Localhost Driver License & Perl Backdoor Hunt** | N/A | Manual investigation required | **Manual** |
