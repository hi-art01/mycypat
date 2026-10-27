# CyberPatriot Round-Ready SuperScripts

This repository contains four specialized, interactive CyberPatriot hardening scripts designed to obtain 85–100 points quickly in competition rounds across Ubuntu, Linux Mint, Windows 10/11 Workstations, and Windows Server environments.

---

## Script Overview

| Script Name | Target Operating System | Language | Description |
| :--- | :--- | :--- | :--- |
| **`UbuntuSuperScript`** | Ubuntu 20.04 / 22.04 / 24.04 LTS | Bash | Interactive Linux hardening script tailored for Ubuntu. |
| **`MintSuperScript`** | Linux Mint 20 / 21 | Bash | Interactive Linux hardening script tailored for Linux Mint (includes Update Manager auto-refresh settings). |
| **`WindowsSuperScript`** | Windows 10 / Windows 11 | PowerShell | Interactive Windows Desktop hardening script. |
| **`WindowsServerSuperScript`** | Windows Server 2016 / 2019 / 2022 | PowerShell | Interactive Windows Server hardening script (supports Active Directory Domain Controllers & Member Servers). |

---

## CyberPatriot Practice Images & Training Resources

Practice on virtual machines using snapshots before competing! Use these official and community resources to download CyberPatriot practice images:

1. **Official US CyberPatriot Training Materials & Practice Images:**
   - [US CyberPatriot Official Training Materials](https://www.uscyberpatriot.org/competition/training-materials)
   - [Official Linux Training Page](https://www.uscyberpatriot.org/competition/training-materials/linux)
   - [Official Windows Training Page](https://www.uscyberpatriot.org/competition/training-materials/windows)

2. **Community Practice Images & Repositories:**
   - [CyberPanther Practice Images Archive](https://github.com/CyberPanthers)
   - [CyberPatriot Practice Image Catalog (CyberPatriot Wiki)](https://github-wiki-see.page/m/Wes-Bl/Cyberpatriot-Wiki/wiki)
   - [LinuxVox CyberPatriot Guide & Exercises](https://linuxvox.com/blog/cyberpatriot-linux-checklist/)

---

## How to Run in a Competition Round

### 1. Pre-Run Prerequisites (First 2 Minutes of the Round)
1. **Read the Competition README:** Open and read the image's README carefully before running any scripts.
2. **Note Down Specifics:** Keep track of Authorized Users, Authorized Administrators, Critical / Required Services (e.g., SSH, Apache, IIS, FTP, Remote Desktop), Extra Allowed Firewall Ports, and Password requirements.
3. **Answer Forensics Questions First:** Complete and save your answers for any Forensics Questions on the Desktop *before* running remediation tools.

---

### 2. Execution Methods

#### **A. Linux Systems (`UbuntuSuperScript` / `MintSuperScript`)**

1. **Make the script executable:**
   ```bash
   chmod +x UbuntuSuperScript
   # OR for Mint:
   chmod +x MintSuperScript
   ```

2. **Run the script with `sudo` privileges:**
   ```bash
   sudo ./UbuntuSuperScript
   # OR for Mint:
   sudo ./MintSuperScript
   ```

3. **Non-Interactive Flag Execution (Repeatable Runs):**
   ```bash
   sudo ./UbuntuSuperScript -y --users "alice bob" --admins "alice" --keep "ssh apache2" --ports "22/tcp 80/tcp" --password "Cyb3r-Pat!2026-xQz"
   ```

---

#### **B. Windows Systems (`WindowsSuperScript` / `WindowsServerSuperScript`)**

1. **Open PowerShell as Administrator:**
   - Click Start, type `powershell`, right-click **Windows PowerShell**, and select **Run as administrator**.

2. **Bypass Execution Policy for the session:**
   ```powershell
   Set-ExecutionPolicy -Scope Process Bypass -Force
   ```

3. **Execute the script:**
   ```powershell
   .\WindowsSuperScript.ps1
   # OR for Windows Server / Domain Controller:
   .\WindowsServerSuperScript.ps1
   ```

4. **Non-Interactive Flag Execution:**
   ```powershell
   .\WindowsSuperScript.ps1 -Force -Users "alice,bob" -Admins "alice" -Keep "Spooler,sshd" -Ports "22,80" -Password "Cyb3r-Pat!2026-xQz"
   ```

---

## Interactive Prompts and Execution Modes

1. **Forensics Gate:** Every interactive run requires typing `YES` to confirm Forensics Questions are answered before running any changes.
2. **Step 0 README Inputs:** Prompts for Authorized Users, Authorized Admins, Services to Keep, Firewall Ports to Allow, Default Password, and Required Roles.
3. **Run Modes:**
   - **FAST_MODE (Default Y):** Bulk silent execution of routine items without per-item prompts. Shows a 10-second auto-Y timed prompt for unknown listening processes or unclassified items.
   - **ALL_MODULES (Default N):** Executes all hardening modules automatically without section-level Y/N stops.

---

## Automated vs. Report-Only vs. Manual Capabilities Matrix

| Feature / Hardening Area | `UbuntuSuperScript` | `MintSuperScript` | `WindowsSuperScript` | `WindowsServerSuperScript` |
| :--- | :---: | :---: | :---: | :---: |
| **Forensics Question Answers** | Manual | Manual | Manual | Manual |
| **Read-Only Forensics Scanner** | Automated | Automated | Automated | Automated |
| **User Account Audit & Removal** | Automated | Automated | Automated | Automated |
| **Admin Group Membership Enforce** | Automated | Automated | Automated | Automated |
| **Password Resets & Expiration** | Automated | Automated | Automated | Automated |
| **PAM Policy / Secedit User Rights** | Automated | Automated | Automated | Automated |
| **Sysctl Kernel / Registry Options** | Automated | Automated | Automated | Automated |
| **UAC Max / SmartScreen / CAD** | N/A | N/A | Automated | Automated |
| **RDP NLA / SSL / Firewall Rules** | N/A | N/A | Automated | Automated |
| **AD Domain Controller Policy** | N/A | N/A | N/A | Automated |
| **IFEO & WMI Hijack Cleanup** | N/A | N/A | Automated | Automated |
| **Prohibited Software Uninstallation** | Automated | Automated | Automated | Automated |
| **Backdoor Process Kill & dpkg Purge** | Automated | Automated | Automated | Automated |
| **Media File Search & Deletion** | Automated | Automated | Automated | Automated |
| **UFW / Defender Firewall Enforce** | Automated | Automated | Automated | Automated |
| **Service Hardening (Apache/IIS/FTP)** | Automated | Automated | Automated | Automated |
| **Automatic Health Check Report** | Automated | Automated | Automated | Automated |
| **Final Verification Summary** | Automated | Automated | Automated | Automated |

---

## Competition Best Practices

1. **Keep Log Files:** All script outputs are logged (`/var/log/cp_*_hardening.log` and `/root/cp-notes/summary.txt` on Linux; `C:\Windows\Temp\CP-*-Hardening-Log.txt` and `C:\CP-Notes\summary.txt` on Windows).
2. **Never Blindly Disable Required Services:** Always include required services in `KEEP_SERVICES` during Step 0 prompts.
3. **Check Scoring Engine:** After running the script, check the CyberPatriot Scoring Report to verify points gained.
