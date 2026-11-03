# CyberPatriot Hardening Suite & Guides (CP19 / 2026 Edition)

Welcome to the **CyberPatriot Hardening Suite**. This repository contains OS-specific hardening scripts that automate common security controls while allowing image-specific requirements to be supplied at run time. They are a baseline, not a guarantee of any competition score or a universal fit for every image.

> **Compatibility and image-specific requirements**: The scripts target only the OS families and versions listed below; they are not intended for every CyberPatriot image or arbitrary Linux distribution. The Debian answer key was used as a source of general hardening ideas only. Image-specific usernames, passwords, forensic answers, and scenario values are not built into the scripts. Before each run, read that image's instructions and identify the authorized users/admins, required services and ports, prohibited software, and any controls that must remain unchanged. Prefer the interactive run and review the summary; `-y`/`--yes` runs modules without their individual confirmation prompts and should only be used after supplying and checking the correct scenario values.

---

## Script Overview

| Script Name | Target Operating Systems | Language / Shell | Key Features |
| :--- | :--- | :--- | :--- |
| **`UbuntuSuperScript`** | Ubuntu 20.04 / 22.04 / 24.04 LTS | Bash | Read-only `--audit-only` report; 19 modules including PAM/faillock, account and service controls, sysctl/GRUB, UFW, AppArmor, persistence scans, required-service hardening, optional desktop screen-lock/media autorun policy, managed Chromium/Chrome/Firefox policies, extended report-only persistence/package-integrity audit, browser and package updates, post-run health and final verification |
| **`MintSuperScript`** | Linux Mint 20 / 21 | Bash | Read-only `--audit-only` report; 19 modules matching Debian/Ubuntu security modules, plus Cinnamon screen-lock/media autorun policy and Mint Update Manager automatic-update settings; managed Chromium/Chrome/Firefox policies, extended report-only persistence/package-integrity audit, browser and package updates, post-run health and final verification |
| **`DebianSuperScript`** | Debian 11 (Bullseye) / 12 (Bookworm) | Bash | Read-only `--audit-only` report; 19 modules matching Ubuntu/Mint security modules, with Debian package/source safeguards; desktop policies apply only when Cinnamon or GNOME schemas are installed; managed Chromium/Chrome/Firefox policies, extended report-only persistence/package-integrity audit, browser and package updates, post-run health and final verification. No Snap refresh (Debian does not use Snap by default). |
| **`WindowsSuperScript`** | Windows 10 / 11 Workstations | PowerShell 5.1+ | Read-only `-AuditOnly` report; forensics snapshot with unique backup before replacement; SID-based User Rights Assignment via `secedit`, 40+ security registry options, UAC max, Defender & Inbound Firewall rule pruning, Adapter Bindings hardening (`-SmbRequired` / Step 0 prompt, never touches `ms_tcpip`), scenario share preservation via `-KeepShares`, OneDrive management (startup/sync disabled, no uninstall), Firefox policy preferences, 7-Zip/LibreOffice upgrades |
| **`WindowsServerSuperScript`** | Windows Server 2016 / 2019 / 2022 | PowerShell 5.1+ | Read-only `-AuditOnly` report; forensics snapshot with unique backup before replacement; Domain Controller policy, Domain Admins enforcement, LDAP signing/channel binding, DNS/IIS hardening, Windows Update policy, Server Features, Inbound Firewall rule pruning with role/port safeguards, Adapter Bindings hardening (`-SmbRequired` / Step 0 prompt, never touches `ms_tcpip`), scenario share preservation via `-KeepShares`, Firefox policy preferences, Security Registry parity (`AlwaysInstallElevated`, `AllowTelemetry`, Screen Saver, WinRM, Netlogon) |

---

## Quick Start & Execution

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
& ([scriptblock]::Create((Get-Content .\WindowsSuperScript -Raw))) -Force -SmbRequired -Users "alice,bob" -Admins "alice" -Keep "Spooler,sshd" -KeepShares "Public" -Ports "22,80" -Password (Read-Host -AsSecureString "Enter a unique password")
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
& ([scriptblock]::Create((Get-Content .\WindowsServerSuperScript -Raw))) -Force -SmbRequired -Users "alice,bob" -Admins "alice" -Keep "DNS,DHCP,W3SVC" -KeepShares "Public" -Ports "53,80" -Password (Read-Host -AsSecureString "Enter a unique password")
```
*Self-Test Dry Run (safe, non-interactive, dry-run only, makes no changes):*
```powershell
& ([scriptblock]::Create((Get-Content .\WindowsServerSuperScript -Raw))) -SelfTest -SmbRequired
```

## Read-only Audit Mode

Use audit mode before hardening to capture the guide's baseline information without changing system settings or entering remediation modules. Linux writes a uniquely named report in `/tmp` with owner-only permissions; Windows writes a uniquely named `CP-Audit-*.txt` file to the Desktop (or `%TEMP%` if no Desktop is available). Reports include account and privilege inventories, services and listeners, share permissions, firewall/audit policy, installed software or packages, persistence leads, and platform-specific security status. Non-root Linux runs may not see protected details. Windows scripts still require an elevated PowerShell session. Reports may contain usernames, paths, services, and network details; keep them local. A normal Windows run preserves any existing `CP-Evidence.txt` to a unique backup before replacing it, and stops if that backup cannot be created.

```bash
./UbuntuSuperScript --audit-only
./MintSuperScript --audit-only
./DebianSuperScript --audit-only
```

```powershell
& ([scriptblock]::Create((Get-Content .\WindowsSuperScript -Raw))) -AuditOnly
& ([scriptblock]::Create((Get-Content .\WindowsServerSuperScript -Raw))) -AuditOnly
```

`--self-test` / `-SelfTest` remains the code-path dry run; audit mode inspects the actual host. Review audit output against the image README before running a remediation mode.

## What the Scripts Automate and What Still Needs Manual Work

The scripts automate general system controls, but they cannot infer image-specific answers or decide whether a service, share, account, file, or application is required. Read the image README and answer forensic questions before remediation. The scripts do not contain answer-key usernames, passwords, or forensic values.

### Windows

The Windows scripts apply the selected account, password, audit, user-rights, UAC, anonymous-access, Defender, firewall, SMB, service, and remote-access controls. Both configure Chrome/Edge managed security policies and Firefox profile preferences; restart browsers and verify their policies/settings afterward. The workstation script scans for prohibited software/files and attempts 7-Zip, LibreOffice, and detected Firefox upgrades through `winget` or Chocolatey. These package-manager attempts are not proof that the latest version installed.

Windows Update is not fully installed or verified by the scripts: they configure automatic-update policy and request a scan, but do not confirm that updates downloaded, installed, or completed a required restart. Open **Settings > Windows Update** (or **Settings > Update & Security > Windows Update** on older releases), select **Check for updates**, install applicable updates, restart when prompted, then check again. On Server, coordinate patching and restarts with required roles and the image README. If `winget` or Chocolatey is unavailable, or a browser update attempt fails, update the software using approved installers or the competition's software center. Do not install third-party scanners or cleanup utilities unless the competition explicitly permits them.

Before share cleanup, enter every required non-default share in the interactive prompt or pass it as `-KeepShares "share1,share2"`. Domain controllers retain `SYSVOL` and `NETLOGON` automatically. Only built-in administrative shares are retained by default on workstations and member servers; a scenario share omitted from `-KeepShares` can be removed. Required services still belong in `-Keep`.

Windows interactive runs request a unique password as masked input. Account creation and password-reset actions are skipped when no password is supplied; this is expected in `-Force` mode unless a `SecureString` is provided, for example `-Password (Read-Host -AsSecureString "Enter a unique password")`. Do not pass a literal password in a command line or store one in the README. Newly created accounts must change their password at first logon.

### Debian, Ubuntu, and Linux Mint

All three Linux scripts now share the same 19-module hardening set: account and privilege review/remediation, password and PAM policy, sudoers, kernel settings, SSH, package/service removal, selected firewall rules, required-service hardening, desktop login protections, managed browser policies, broader persistence/package-integrity review, and package upgrades. Browser extensions and integrity findings are report-only; the scripts do not remove extensions or guess at scenario-specific accounts/files. Installed Firefox and Chromium are updated through their detected APT, Snap, or Flatpak packages where supported. Mint additionally configures Linux Mint Update Manager settings. These actions depend on working repositories, the image's package format, and the scenario's required services; review the log and final health check. The [manual update and UFW guide](#manual-debianubuntumint-updates-and-ufw) explains how to recover from failed repository/package operations and verify updates.

### Manual tasks on every platform

1. **Forensics questions and application-specific answers:** inspect the named evidence and application, record the requested answer in the required file and format, and submit it yourself. A localhost database or driver-license value cannot be safely guessed or derived by a generic hardening script. Preserve evidence before removing accounts, files, keys, or software.
2. **Scenario decisions:** compare users, administrators, services, shares, ports, packages, and prohibited files with the image README. Review audit reports and suspicious scanner findings; a search match is a lead, not proof. Do not remove all SUID files, keys, media, or scheduled jobs indiscriminately.
3. **Functional verification:** log in with the intended accounts; test required websites, shares, remote access, services, DNS, and application workflows. Confirm firewall rules permit required traffic and that policy changes have not broken the image.
4. **Updates and restarts:** verify that updates actually installed and restart only when appropriate. For Windows, use Windows Update as described above. For Debian-family systems, use the manual update guide if the script reports failed repositories, held packages, or pending updates.
5. **Other platforms and GUI-only settings:** these scripts do not configure FreeBSD. Use the official FreeBSD Handbook and image-specific instructions for FreeBSD practice. After browser policy changes, restart browsers and verify Chrome/Edge policies in `chrome://policy` / `edge://policy`, Firefox settings in `about:policies`, and the Linux desktop lock/autorun settings in the active session. If a setting is unavailable through a supported policy or user profile, configure it in that application's UI and verify it manually.

---

## Linux Module Guide

The following guide applies to `DebianSuperScript`, `UbuntuSuperScript`, and `MintSuperScript`. Module numbers are the same in all three scripts. Step 0 gathers scenario-specific information before the modules run.

| Module | What it does |
| :--- | :--- |
| **1. User Accounts & Groups** | Reviews local accounts and administrator privileges. In interactive mode, asks separately whether to delete each account and whether to remove admin privileges; both default to **No**. It can add a space- or comma-separated list of users, apply requested `user:group` memberships, and lock empty-password accounts. Non-interactive account deletion requires an explicit `--users` list. |
| **2. Password Security & Aging** | Applies password aging settings to local accounts and prompts for new passwords only for selected accounts. Non-interactive resets use `--reset-password-users` and `--password`; no built-in default password is used. |
| **3. PAM, Login Policy & `su` Restriction** | Sets password length/complexity and history, configures failed-login lockout where supported, removes password `nullok` options, and limits `su` access to the administrative group. |
| **4. Sudoers & Privilege Delegation** | Audits `/etc/sudoers` and its drop-ins, removes `NOPASSWD` and `!authenticate`, comments out unauthorized broad `ALL` grants, validates syntax with `visudo`, and enables sudo session logging and pseudo-terminal use. **This module does not configure IPv4.** |
| **5. Sysctl Kernel Network Hardening** | Configures kernel network and process protections. This is where **IPv4 TCP SYN cookies are enabled** (`net.ipv4.tcp_syncookies=1`) and **IPv4 forwarding is disabled** (`net.ipv4.ip_forward=0`), alongside IPv4/IPv6 redirect and source-route protections, address randomization, and core-dump protections. It persists the requested IPv4 values in both `/etc/sysctl.d/99-cp-hardening.conf` and `/etc/sysctl.conf`, applies the settings, and verifies the runtime values. |
| **6. File Permissions & Home Directories** | Tightens permissions on account, sudo, cron, SSH, and GRUB files; sanitizes risky GRUB boot parameters; restricts home-directory access; removes world-writable permissions where appropriate; and records SUID files and file capabilities for review. |
| **7. SSH Server Hardening & Authorized Keys** | If SSH is installed, audits authorized key files and hardens the SSH daemon (for example, disables root/empty-password login and X11 forwarding, limits authentication attempts, and optionally restricts logins to configured users). It validates the configuration and rolls it back if `sshd -t` fails. |
| **8. Prohibited Software & Service Removal** | Finds configured prohibited tools and unrequired server packages, checks whether removal would affect protected or required software, and purges candidates that pass the dependency guard. Required services/packages can be declared in setup prompts. |
| **9. Backdoor & Persistence Sweep** | Checks for netcat/socat processes, suspicious cron and systemd persistence entries, and executables or command patterns associated with persistence. Suspicious findings are surfaced for review; package-owned system files are protected from automatic cleanup. |
| **10. Prohibited Media & Archive Search** | Searches selected non-system locations for prohibited media, torrents, archives, and installers, preserving README/forensics files and protected application/service paths. Review the competition image’s rules before approving deletion. |
| **11. APT Sources, Hosts, DNS & UFW Firewall** | Filters package sources to the distribution’s configured official repositories, validates APT metadata (including partial repository failures), preserves the active machine hostname in `/etc/hosts`, configures DNS, refreshes APT before installing UFW, and configures deny-incoming/allow-outgoing defaults and requested ports. It enables UFW and checks that its status is active; metadata, installation, SSH-rule, and firewall failures are reported. **Review custom repositories, DNS requirements, SSH access, and required ports before accepting these changes.** |
| **12. Rootkit & Malware Scanners** | Optionally installs/runs `rkhunter`, `chkrootkit`, and ClamAV scans with timeouts. Results are logged for review; scanner findings are report-only and are not automatically treated as proof of infection. |
| **13. Required Service Hardening** | Applies configuration hardening only to services identified as required (Apache, Nginx, FTP, Samba, MySQL/MariaDB, or PHP). It uses service-specific configuration checks and restores backups when validation fails. |
| **14. Defenses, Logging & Access Controls** | Enables or configures AppArmor, auditd rules, fail2ban, and rsyslog where available; disables desktop guest/automatic login; blocks selected removable-storage/expansion modules; masks Ctrl-Alt-Del reboot; disables core dumps; and preserves VM guest integration services. |
| **15. Package Upgrades & Security Updates** | Uses a successful APT metadata refresh—not an ICMP ping—to detect repository access, then installs all available package upgrades (including published security updates), performs a distribution upgrade, protects important packages during guarded autoremove, configures unattended upgrades, and targets installed Firefox, Chromium, and OpenSSH packages for updates. Browser package sources are refreshed through APT, Snap, or Flatpak where installed. Mint also configures Update Manager automatic updates. Update success depends on working repositories and is reported in the log. |
| **16. Extended Persistence & Integrity Audit (Report Only)** | Reviews shell and desktop startup, authorized SSH keys, inetd/xinetd, systemd timers, scheduled jobs, remote-access packages, APT hooks, PAM bypass indicators, local command shadowing, package-integrity differences, and suspicious Snap/Flatpak applications. Matches are leads for manual review and are never automatically deleted by this module. |
| **17. Browser Policies & Extension Inventory** | Adds managed pop-up, notification, Safe Browsing, password-manager, autofill, sync, HTTPS-only, and related policies for installed Chromium/Chrome and Firefox browsers. Existing Firefox policies are preserved; Chromium extensions are inventoried only. Restart browsers and inspect their policy pages to verify settings. |
| **18. Post-Run System Health Check** | Checks required services and ports, SSH and sudoers syntax, configured users/admins, and whether UFW is active; reports pass/fail/skip results. |
| **19. Final System Verification Report** | Reports failed systemd units, listening ports, administrator/UID-0 accounts, IPv4 SYN-cookie/forwarding values, UFW status, effective SSH settings, remaining upgradable packages, and whether a reboot is pending. |

## Manual Debian/Ubuntu/Mint Updates and UFW

Use these steps if Module 11 or Module 15 cannot update packages or enable UFW in the VM. They are manual alternatives for the Debian-family images supported by the Linux scripts; they are not commands to run blindly on every CyberPatriot image. Complete the image's forensics questions first, read its desktop README, and confirm the OS and required services/ports before changing anything.

### 1. Check the OS and package repositories

```bash
cat /etc/os-release
```

APT needs working network access and valid repositories for the installed OS release. The attached Debian 12 answer key shows Debian Bookworm repository examples, but **do not replace `/etc/apt/sources.list` with those lines unless the VM is actually Debian 12**. Ubuntu uses Ubuntu release repositories; Linux Mint uses Mint repositories together with repositories for its Ubuntu base. Newer images may store repositories in `.sources` files under `/etc/apt/sources.list.d/`.

Inspect the configured sources and refresh the package lists:

```bash
grep -RhsE '^[[:space:]]*(deb[[:space:]]|URIs:|Suites:|Components:)' \
  /etc/apt/sources.list /etc/apt/sources.list.d 2>/dev/null
sudo apt update
```

Read the complete `apt update` output. Resolve DNS/network problems, incorrect release names, disabled security repositories, expired signing keys, or other reported repository errors before proceeding. Do not ignore signature errors or mix Debian, Ubuntu, and Mint repositories.

### 2. Install available security and other package updates

```bash
sudo apt upgrade
```

Review APT's proposed changes and confirm when ready. This installs available updates from the enabled repositories, including security updates when the correct security repository is configured. If APT reports packages kept back, inspect the proposed changes before considering:

```bash
sudo apt full-upgrade
```

`full-upgrade` may remove packages to resolve dependencies; do not approve removals that conflict with the image README or required services. If `apt update` failed, fix that first—an upgrade cannot retrieve current updates from broken or stale repository metadata.

### 3. Update Firefox, Chromium, and OpenSSH if installed

First check which Chromium delivery method and packages are present:

```bash
dpkg-query -W -f='${db:Status-Abbrev} ${binary:Package}\n' chromium chromium-browser 2>/dev/null
dpkg-query -W -f='${db:Status-Abbrev} ${binary:Package}\n' firefox 2>/dev/null
```

Use the update command that matches the installed browser package:

```bash
# Debian/Ubuntu/Mint APT package (run only for a package shown as installed)
sudo apt install --only-upgrade chromium
sudo apt install --only-upgrade chromium-browser

# Firefox APT package (run only when installed from APT)
sudo apt install --only-upgrade firefox

# Chromium Snap (common on Ubuntu and some Mint images)
sudo snap refresh chromium

# Firefox Snap (only when `snap list firefox` shows it installed)
sudo snap refresh firefox

# Chromium Flatpak (only if this application is installed as a Flatpak)
sudo flatpak update org.chromium.Chromium

# Firefox Flatpak (only if this application is installed as a Flatpak)
sudo flatpak update org.mozilla.firefox
```

An error such as “package is not installed” or “snap not found” means that command does not match the image's installation method; do not install a browser solely to clear the error. Check `snap list` or `flatpak list --app` to identify the installed source. Keep browsers named in the image README.

Update the installed OpenSSH packages from APT:

```bash
sudo apt install --only-upgrade openssh-server openssh-client
```

If either package is not installed, rerun the command with only the installed package name. Review prompts about replacing SSH configuration before answering; keeping the local configuration is usually safer for preserving the image's required SSH access. You can check installed versions with:

```bash
ssh -V
dpkg-query -W -f='${Version}\n' openssh-server openssh-client 2>/dev/null
```

### 4. Install and enable UFW

Check the image README and listening services first. If SSH access is required, add its actual port **before** enabling the firewall; the common default is `22/tcp`. Add every other required service port as well.

```bash
sudo apt update
sudo apt install ufw
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp       # only if SSH is required on port 22
sudo ufw enable
sudo ufw status verbose
```

Run an additional `sudo ufw allow <port>/<protocol>` for each required port (for example, `80/tcp` for a required HTTP service). Do not use the example port list without checking the image instructions. Enabling UFW can disconnect a remote session or block required services; use the VM console where possible, and verify the final status says `Status: active` and the required ports are allowed.

### 5. Confirm the result

```bash
sudo apt update
apt list --upgradable
sudo ufw status verbose
```

An empty upgradable-package list means APT currently reports no upgrades from the configured repositories; it does not prove that the correct repositories are configured. If a command fails, keep its full error output and check the matching script log in `/var/log/cp_debian_hardening.log`, `/var/log/cp_ubuntu_hardening.log`, or `/var/log/cp_mint_hardening.log`.

---

## Feature & Capability Matrix

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
| **Browser Security Policies** | Chromium/Chrome managed policies; Firefox enterprise policy where installed | Same as Ubuntu | Same as Ubuntu | Chrome/Edge policies; Firefox profile hardening | Chrome/Edge policies; Firefox profile hardening |
| **System File Permissions & GRUB Hardening (Module 6)** | `chmod 600 /boot/grub/grub.cfg`, remove `init=`, `single`, `rw init` from `GRUB_CMDLINE_LINUX*`, `update-grub`, shadow/passwd/sudoers perms | `chmod 600 /boot/grub/grub.cfg`, remove `init=`, `single`, `rw init` from `GRUB_CMDLINE_LINUX*`, `update-grub`, shadow/passwd/sudoers perms | Same as Ubuntu | N/A | N/A |
| **Sysctl Kernel Hardening (Module 5)** | IPv4 TCP SYN cookies enabled, IPv4 forwarding disabled and persisted in sysctl drop-in and `/etc/sysctl.conf`; runtime values verified | Same as Ubuntu | Same as Ubuntu | N/A | N/A |
| **Firewall Configuration (Module 11 / Sec 5)** | APT refreshed, UFW installed, default deny, enabled & status verified; rules pruned | Same as Ubuntu | Same as Ubuntu | Defender Firewall (App Rules Pruned) | Defender Firewall (App Rules Pruned, Role/Port Safeguards) |
| **Adapter Bindings Hardening (Sec 6)** | N/A | N/A | N/A | Disables bindings; preserves `ms_server`/`ms_msclient` only if `-SmbRequired`; never touches `ms_tcpip` | Disables bindings; preserves `ms_server`/`ms_msclient` only if `-SmbRequired`; never touches `ms_tcpip` |
| **Backdoor & Persistence Sweep (Module 9 / Sec 7)** | Netcat `dpkg -S`, Tokenizer/AST Cron & Systemd Sweep (/root & /home user units) | Netcat `dpkg -S`, Tokenizer/AST Cron & Systemd Sweep (/root & /home user units) | Same as Ubuntu | IFEO, WMI, Tasks, Run Keys, Startup Folders | IFEO, WMI, Tasks, Run Keys, Startup Folders |
| **Prohibited Software Removal (Module 8 / Sec 8)** | Purges prohibited & unrequired services | Purges prohibited & unrequired services | Purges prohibited & unrequired services | CCleaner, WebCompanion, OpenTFTP, BitTorrent, Wireshark | CCleaner, WebCompanion, OpenTFTP, BitTorrent, Wireshark |
| **Software Version Upgrades (Module 15 / Sec 9)** | Firefox/Chromium where installed; `apt-get upgrade`, `dist-upgrade`, guarded `autoremove` | Firefox/Chromium where installed; `apt-get upgrade`, `dist-upgrade`, guarded `autoremove` | Firefox/Chromium where installed; `apt-get upgrade`, `dist-upgrade`, guarded `autoremove` | 7-Zip, LibreOffice, Firefox attempts via `winget`/Chocolatey | 7-Zip, LibreOffice and package-manager-wide upgrades |
| **Rootkit & Malware Scanning (Module 12)** | Module 12: `rkhunter`, `chkrootkit`, `clamscan` (600s timeouts, report-only), `snap refresh` | Module 12: `rkhunter`, `chkrootkit`, `clamscan` (600s timeouts, report-only) | Module 12: `rkhunter`, `chkrootkit`, `clamscan` (600s timeouts, report-only, no snap) | N/A | N/A |
| **Required Service Hardening (Module 13)** | Module 13: Apache (`TraceEnable Off`), vsftpd (`anonymous_enable=NO`, `chroot_local_user=YES`, `allow_writeable_chroot=YES`, active/port 21 test), Samba, MySQL/MariaDB (`bind-address 127.0.0.1`), PHP (`php -n -c` linting), Nginx (`server_tokens` drop-in rollback) | Module 13: Apache (`TraceEnable Off`), vsftpd (`anonymous_enable=NO`, `chroot_local_user=YES`, `allow_writeable_chroot=YES`, active/port 21 test), Samba, MySQL/MariaDB (`bind-address 127.0.0.1`), PHP (`php -n -c` linting), Nginx (`server_tokens` drop-in rollback) | Same as Ubuntu | N/A | DNS, IIS, Wecsvc hardening |
| **Defenses, Logging & Access Controls (Module 14)** | Module 14: Pre-install defense pkgs, AppArmor (`aa-enforce` skipping kept family profiles), auditd rules, fail2ban, rsyslog, LightDM/GDM guest/autologin off, conditional Cinnamon/GNOME 5-minute lock and media autorun policy, `usb-storage`/firewire/thunderbolt blacklist, coredumps off | Same as Ubuntu plus Mint Cinnamon settings | Same as Ubuntu with available desktop schema detection | Exploit Guard, Defender Realtime/Cloud/MAPS, LSA Protection | Exploit Guard, Defender Realtime/Cloud/MAPS, LSA Protection, LDAP Signing/Enforcement |
| **Package Upgrades & Policies (Module 15)** | Security/repository upgrades; installed Firefox, Chromium (APT, Snap, or Flatpak), and OpenSSH targeted updates; `dist-upgrade`, guarded `autoremove`, unattended-upgrades | Same as Ubuntu plus Mint Update Manager auto-refresh/auto-apply | Security/repository upgrades; installed Firefox, Chromium (APT or Flatpak), and OpenSSH targeted updates; `dist-upgrade`, guarded `autoremove`, unattended-upgrades |
| **Extended Audit & Browser Policies (Modules 16-17)** | Report-only startup/integrity audit; managed Chromium/Chrome/Firefox policies and extension inventory | Same as Ubuntu | Same as Ubuntu | Chrome/Edge managed policies, Firefox profile preferences | Chrome/Edge managed policies, Firefox profile preferences |
| **Post-Run System Health Check (Module 18 / Sec 10)** | Required services active, required ports listening, `sshd -t`, `visudo -c`, users/admins, UFW active | Same as Ubuntu | Same as Ubuntu | Section 10: Automatic Health Check (Firewall, Defender, RDP, Admins) | Section 10: Automatic Health Check (Firewall, Defender, RDP, Domain Admins) |
| **Final System Verification Report (Module 19 / Sec 11)** | Failed units, listening ports, sudo/UID 0 accounts, IPv4 sysctls, active UFW, effective `sshd -T`, upgradable packages, pending reboot | Same as Ubuntu | Same as Ubuntu | Section 11: Final Verification Report (Shares, Firewall profiles) | Section 11: Final Verification Report (Shares, Firewall profiles) |
| **VM Integration & Snapshot Safety** | Open-VM-Tools, VBoxGuest, Qemu-GA & Hyper-V daemons protected by dependency guard; snapshot integration active | Open-VM-Tools, VBoxGuest, Qemu-GA & Hyper-V daemons protected by dependency guard; snapshot integration active | Open-VM-Tools, VBoxGuest, Qemu-GA & Hyper-V daemons protected by dependency guard; snapshot integration active | Volume Shadow Copy (VSS) & swprv set to Manual/Healthy; VMTools & VBoxService protected; SeServiceLogonRight grants Local/Network Service | Volume Shadow Copy (VSS) & swprv set to Manual/Healthy; VMTools & VBoxService protected; SeServiceLogonRight grants Local/Network Service |

---

## Scored Competition Item Coverage Matrix (Automated / Report-Only / Manual)

| Category / Scored Item | Windows (`WindowsSuperScript` & `Server`) | Linux (`UbuntuSuperScript`, `MintSuperScript` & `DebianSuperScript`) | Status |
| :--- | :--- | :--- | :--- |
| **Pre-hardening baseline inventory** | Unique audit report with users/groups, share permissions, listeners, services, tasks, audit/firewall policy, Defender, updates, optional features, and RDP settings | Unique owner-only report with accounts, privilege rules, services, sockets, firewall, packages, SSH, persistence, permissions, and audit status | **Report-only** |
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
| **Software Upgrades** | 7-Zip, LibreOffice, and detected Firefox attempted via `winget`/Chocolatey; Windows Update installation/restart verification remains manual | APT package upgrades plus installed Firefox/Chromium updates by package source; verify repository results | **Partially automated; Windows Update installation and restart are manual** |
| **Malware & Backdoor Sweeps** | IFEO, WMI, Tasks, Run keys swept | Netcat purged, AST/tokenizer cron & user/system units sweep, `rkhunter`/`chkrootkit`/`clamscan` (Modules 9, 12) | **Automated / Report-Only** |
| **Health Check & Verification** | Health check (Section 10) and Verification (Section 11) | Service health check (Module 18) and system verification report (Module 19) | **Automated** |
| **Localhost Driver License & Perl Backdoor Hunt** | N/A | Manual investigation required | **Manual** |

---

## Quality Assurance & Test Verification

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
