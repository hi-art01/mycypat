# CyberPatriot Hardening Suite & Guides (CP19 / 2026 Edition)

Welcome to the **CyberPatriot Hardening Suite**. This repository contains four OS-specific, interactive hardening scripts designed to achieve **85–100 points** in CyberPatriot competition rounds with maximum speed, accuracy, and compliance with image-specific README requirements.

---

## 📜 Script Overview

| Script Name | Target Operating Systems | Language / Shell | Key Features |
| :--- | :--- | :--- | :--- |
| **`UbuntuSuperScript`** | Ubuntu 20.04 / 22.04 / 24.04 LTS | Bash | Step 0 prompts, PAM faillock, `su` restriction, apt dependency guard, sysctl, UFW, AppArmor, backdoor sweep, service family map |
| **`MintSuperScript`** | Linux Mint 20 / 21 | Bash | Step 0 prompts, Mint Update Manager dconf database, `su` restriction, apt dependency guard, UFW, AppArmor, backdoor sweep |
| **`WindowsSuperScript`** | Windows 10 / 11 Workstations | PowerShell 5.1+ | Forensics snapshot (`CP-Evidence.txt`), SID-based User Rights Assignment via `secedit`, 40+ security registry options, UAC max, Defender & Firewall |
| **`WindowsServerSuperScript`** | Windows Server 2016 / 2019 / 2022 | PowerShell 5.1+ | Domain Controller policy, Domain Admins enforcement, LDAP signing/channel binding, DNS/IIS hardening, Windows Update policy, Server Features |

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
*Self-Test Dry Run:*
```bash
sudo ./UbuntuSuperScript --self-test
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
*Self-Test Dry Run:*
```bash
sudo ./MintSuperScript --self-test
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
| **Forensics Gate & Evidence Snapshot** | Yes | Yes | Yes (`CP-Evidence.txt`) | Yes (`CP-Evidence.txt`) |
| **Step 0 Interactive Specifics Prompts** | Yes | Yes | Yes | Yes |
| **FAST MODE (Timed Judgment Prompts)** | Yes (10s auto-default) | Yes (10s auto-default) | Yes (10s auto-default) | Yes (10s auto-default) |
| **Self-Test Dry-Run Flag** | `--self-test` | `--self-test` | `-SelfTest` | `-SelfTest` |
| **User & Group Remediation** | Yes | Yes | Yes | Yes (AD & Local) |
| **Password Aging & Complexity** | Min 10, Max 90, Remember 5 | Min 10, Max 90, Remember 5 | Min 14, Max 90, Remember 24 | Min 14, Max 90, Remember 24 |
| **Account Lockout Policy** | `pam_faillock` (5 attempts / 15 min) | `pam_faillock` (5 attempts / 15 min) | `net accounts` (10 attempts / 30 min) | `net accounts` / AD Domain Policy (10 / 30 min) |
| **`su` Privilege Restriction** | `pam_wheel.so` (group=sudo/admin) | `pam_wheel.so` (group=sudo/admin) | N/A | N/A |
| **Sudoers ALL Rule & NOPASSWD Cleanup** | Yes (`visudo -cf` check) | Yes (`visudo -cf` check) | N/A | N/A |
| **User Rights Assignment (SIDs)** | N/A | N/A | Full `secedit` (30+ SIDs) | Full `secedit` (30+ SIDs) |
| **Security Registry Options (40+)** | N/A | N/A | UAC Max, CAD, LSA, WDigest | UAC Max, CAD, LSA, LDAP Signing/Binding |
| **Sysctl Kernel Hardening** | IPv4 + IPv6 | IPv4 + IPv6 | N/A | N/A |
| **Firewall Configuration** | UFW Default Deny | UFW Default Deny | Defender Firewall (All Profiles) | Defender Firewall (All Profiles) |
| **Backdoor & Persistence Sweep** | Netcat `dpkg -S`, Cron, Systemd | Netcat `dpkg -S`, Cron, Systemd | IFEO, WMI, Tasks, Run Keys, Startup Folders | IFEO, WMI, Tasks, Run Keys, Startup Folders |
| **Refined File Deletion Scope** | Non-system Media, Installers | Non-system Media, Installers | Targeted User Folders | Targeted User Folders |
| **Apt Package Family Kept Map** | Yes (APACHE, NGINX, PHP, MYSQL, SAMBA, FTP) | Yes (APACHE, NGINX, PHP, MYSQL, SAMBA, FTP) | N/A | N/A |
| **Apt Dependency Guard** | `apt-get -s purge` simulation | `apt-get -s purge` simulation | N/A | N/A |
| **Required Service Configuration Validation** | `sshd -t`, `nginx -t`, `testparm`, `apache2ctl` | `sshd -t`, `nginx -t`, `testparm`, `apache2ctl` | Service Status Check | Service Status Check |

---

## 📦 Kept Package Family Map (Linux Scripts)

When a service is marked as required during Step 0 prompts, the script automatically expands the selection to preserve its entire package family:

- **Apache (`REQ_APACHE`)**: `apache2*`, `libapache2-mod-*` (and `libapache2-mod-php*` if PHP requested)
- **Nginx (`REQ_NGINX`)**: `nginx*` (and `php*-fpm` if PHP requested)
- **PHP (`REQ_PHP`)**: `php*`
- **FTP (`REQ_FTP`)**: `vsftpd`, `proftpd*`, `pure-ftpd*`, `ftpd`, `tftpd-hpa`
- **Samba (`REQ_SAMBA`)**: `samba*`, `smbd`, `smbclient`, `libsmbclient*`, `cifs-utils`, `winbind`
- **MySQL / MariaDB (`REQ_MYSQL`)**: `mysql-*`, `mariadb-*`, `libmysqlclient*`, `libmariadb*`, `default-mysql-*`

---

## 🔍 Automated vs. Report-Only vs. Manual Task Matrix

| Task Category | Automated by Scripts | Report-Only in Log / Evidence File | Requires Manual Attention |
| :--- | :--- | :--- | :--- |
| **Forensics Questions** | None (Gate enforces user completion) | Discovers Forensics Q files on Desktop | Answering and saving Forensics answer files |
| **User & Group Auditing** | Removes unauthorized users, enforces admins, resets passwords | Logs removed users & group changes | Verifying authorized users against README |
| **Local Security Policies** | Applies password aging, lockout, UAC, audit policies, sysctl | Logs policy values and registry keys | Verifying domain-specific GPO overrides |
| **Firewall & Network** | Enables Defender/UFW, applies default deny, opens Step 0 ports | Logs active listening ports & firewall state | Verifying custom application port requirements |
| **Backdoor Remediation** | Kills netcat/socat, purges binaries, cleans cron/tasks/run keys | Logs killed PIDs and deleted files | Investigating unknown listening process trees |
| **File Cleanup** | Purges non-system media, archives, and installers | Logs deleted file paths | Inspecting user Documents for non-media work files |
| **Service Hardening** | Hardens SSH, Apache, Nginx, VSFTPD, Samba, MySQL | Logs config test results & active units | Custom application configuration files |

---

## 🌐 CyberPatriot Practice Image Resources

Practice and train on official and community CyberPatriot images:
- **CyberPatriot Training Materials**: [https://www.uscyberpatriot.org/competition/training-materials](https://www.uscyberpatriot.org/competition/training-materials)
- **Ubuntu Server Security Guide**: [https://ubuntu.com/server/docs/how-to/security/](https://ubuntu.com/server/docs/how-to/security/)
- **Debian Hardening Manual**: [https://www.debian.org/doc/manuals/securing-debian-manual/](https://www.debian.org/doc/manuals/securing-debian-manual/)
- **CyberPatriot Linux Checklist**: [https://linuxvox.com/blog/cyberpatriot-linux-checklist/](https://linuxvox.com/blog/cyberpatriot-linux-checklist/)
