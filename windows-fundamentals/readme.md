# Windows Fundamentals Part 1, 2 & 3

## Part 1
In this section I covered the desktop, the file system, user account control, the control panel, settings, and the task manager. 

## Windows Overview and GUI Navigation

- Learned how the Windows OS is built on many layers — system files, utilities, user interfaces, and settings.
- Windows has gone through major evolution from XP to Vista to Windows 10/11, and there's a clear divide between consumer versions (Home/Pro) and server editions (like Server 2019, which this VM uses).
- Got familiar with the Windows Desktop (GUI):
  - **Start Menu**: Central hub for launching programs and managing user sessions.
  - **Taskbar & Notification Area**: Monitor and interact with open apps and background services.
  - **File Explorer, Display Settings, and Personalization**: Important for both user experience and forensic observation.

---

## File Systems, Permissions, and System Structure

- New Technology File System(NTFS) is the modern file system used in Windows. It supports:
  - File/folder permissions
  - Encryption (EFS)
  - Compression
  - Large file sizes
  - Journaling (auto repair after crashes)
- Learned how to manage NTFS permissions through the **Security tab** in file properties.
- Explored **Alternate Data Streams (ADS)** — hidden streams that can be used for both legitimate metadata and malicious data hiding.
- Understood the critical role of `C:\Windows\System32`:
  - This folder contains core executables like `cmd.exe`, `taskmgr.exe`, and more.
  - Accidental deletion or tampering can break the system.
  - `%windir%` is an environment variable that always points to the current Windows directory.

---

## User Accounts, Profiles, and Access Control

- Windows supports two main account types:
  - **Administrator**: Full control over system changes.
  - **Standard User**: Can only make changes to their own files/folders.
- Each account has a user profile stored in `C:\Users\{username}` — created at first login.
- Used `lusrmgr.msc` to manage:
  - Local users and groups
  - Group-based permission inheritance
  - Adding/removing users from permission groups

---

## User Account Control (UAC)

- UAC prevents Administrator accounts from running elevated operations automatically.
- Even Admins are prompted to confirm actions like installing software or modifying the system.
- Standard users must enter an Admin password when prompted.
- The **shield icon** on a file is a clear visual indicator that UAC will be triggered.
- UAC is a strong defense against malware that tries to escalate privileges silently.

---

## System Settings and Control Panel

- Learned the difference between the modern **Settings** menu and the legacy **Control Panel**:
  - Settings: Simplified UI for basic tasks like personalization, updates, and privacy.
  - Control Panel: More advanced and detailed tools like network adapter configuration, services, and user management.
- Some Settings options redirect to the Control Panel.
- Best practice: use the Start Menu search to find settings quickly, regardless of where they’re located.

---

## Task Manager and System Monitoring

- Task Manager gives a full view of:
  - Processes
  - Performance (CPU, RAM, disk, network)
  - Startup programs
  - User sessions
  - Services
- Opened via `Ctrl + Shift + Esc`, or right clicking the Taskbar.
- Essential for identifying suspicious activity or diagnosing performance issues.
- Malware often hides in fake processes (e.g., `svch0st.exe`), so knowing what normal looks like is important.

---

# Part 2  
In this section I covered various utilities, such as System Configuration, Computer Management, Resource Monitor, command prompts, and registory editor. 

## System Configuration & UAC

- Explored `msconfig`, the **System Configuration utility**, used for:
  - Diagnosing startup issues
  - Launching built in admin tools
  - Enabling/disabling services for testing
- Understood how the **General**, **Boot**, **Services**, **Startup**, and **Tools** tabs work
- Reviewed how to **change UAC (User Account Control) settings** using a simple slider
  - Ranges from “Always Notify” to “Never Notify”
  - Disabling UAC is dangerous and not recommended
  - UAC helps prevent malware from silently escalating privileges

---

## Computer Management

- Accessed via `compmgmt.msc` or through System Configuration > Tools
- Combines multiple powerful tools:
  - **Task Scheduler** – automate tasks or check for malicious persistence
  - **Event Viewer** – audit system logs, crashes, logins, and policy changes
  - **Shared Folders** – view open shares, remote connections, and file access
  - **Local Users and Groups** – manage accounts and permissions
  - **Performance Monitor (`perfmon`)** – analyze resource usage in real time or from logs
  - **Device Manager** – inspect and manage hardware drivers
  - **Disk Management** – configure partitions, drive letters, and volumes
  - **Services** – enable/disable Windows background services
  - **WMI Control** – manage Windows Management Instrumentation (used for remote querying)

---

## System Information

- Explored `msinfo32`, which provides a full overview of:
  - Hardware specs
  - Installed components (GPU, input devices, storage)
  - Software environment
  - Environment variables
- Environment variables such as `%WINDIR%`, `%USERPROFILE%`, and `%PATH%` are critical for OS behavior
- The search feature in `msinfo32` helps find system details (like IP address) fast

---

## Resource Monitor

- Accessed via `resmon`, Resource Monitor gives deeper insight than Task Manager
- Real time data is split into 4 main tabs:
  - **CPU** – see which processes are using the CPU and threads
  - **Memory** – identify memory leaks or high RAM usage
  - **Disk** – find which processes are reading/writing to disk
  - **Network** – trace bandwidth use per process
- Useful for catching:
  - Malware connections
  - Unresponsive apps
  - Hidden resource hogs

---

## Command Prompt Basics

- Reviewed core command line tools in `cmd.exe`, including:
  - `hostname`, `whoami`, `ipconfig`, `cls`, `netstat`
- `netstat` shows active connections and can detect suspicious network activity
- Learned `net` command and its sub commands:
  - `net user`, `net localgroup`, `net share`, `net session`
  - Use `net help <subcommand>` to access detailed help
- Understanding CLI basics is essential for automation, diagnostics, and forensic analysis

---

## Registry Editor

- Accessed via `regedit`, the Windows Registry is the **central config database** for the OS
- Stores data on:
  - Installed apps
  - User profiles
  - System behavior
  - Hardware and drivers
- Highly sensitive — incorrect edits can crash the system
- Key registry areas to watch in cybersecurity:
  - `HKCU\...\Run` – malware persistence
  - `HKLM\SYSTEM` – services and drivers
  - `HKLM\Software\Policies\Microsoft\Windows Defender` – security tool configurations
- Used during investigations to:
  - Identify hidden startup programs
  - Detect altered security settings
  - Audit software installs and user activity

---

# Part 3
In this section I learned about the built in Microsoft tools that help keep the device secure, such as Windows Updates, Windows Security, BitLocker and vss. 

---

## Windows Updates

Windows Update is a service that delivers security updates, bug fixes, and feature enhancements for the operating system and Microsoft products like Defender.

**Patch Tuesday**  
Updates are usually released on the second Tuesday of each month.

**Urgent Fixes**  
Critical updates may be pushed outside of Patch Tuesday if needed.

**Ways to Open Windows Update**
- Settings > Update & Security > Windows Update
- Run: `control /name Microsoft.WindowsUpdate`

---

## Windows Security

Windows Security centralizes tools to help protect your device and data.

**Access**  
Settings > Update & Security > Windows Security

### Protection Areas
- Virus & threat protection
- Firewall & network protection
- App & browser control
- Device security

### Status Icons
- Green: Fully protected
- Yellow: Review recommended settings
- Red: Immediate attention required

---

## Virus & Threat Protection

### Current Threats

**Scan Options**
- Quick Scan
- Full Scan
- Custom Scan

**Threat History**
- Last scan results
- Quarantined threats
- Allowed threats

### Protection Settings

**Manage Settings**
- Real time protection
- Cloud-delivered protection
- Automatic sample submission
- Controlled folder access
- Exclusions
- Notifications

**Defender Updates**
- Check manually for definition updates

**Ransomware Protection**
- Controlled folder access must be enabled
- Real time protection must be on

Tip: You can right click any file or folder and select "Scan with Microsoft Defender" to manually scan it.

---

## Firewall & Network Protection

A firewall controls what network traffic is allowed to enter or leave your device through network ports.

**Access**  
Settings > Update & Security > Windows Security > Firewall & Network Protection  
Run: `WF.msc`

### Profiles
- Domain: For authenticated enterprise environments
- Private: For home or trusted networks
- Public: For open networks like airports and cafes

Each profile allows:
- Firewall on or off
- Blocking all incoming connections

**Allow an App Through Firewall**
- View and adjust which apps are allowed through the firewall in public or private profiles

**Advanced Settings**
- For experienced users who want to configure rules or create custom firewall policies

---

## App & Browser Control

This section controls Microsoft Defender SmartScreen.

**SmartScreen Features**
- Protects against phishing, malware, and suspicious downloads

**Check Apps and Files**
- Warns users before running unknown files from the web

**Exploit Protection**
- Built-in mitigation against memory and system attacks

> Leave default settings enabled unless you fully understand what you are changing.

---

## Device Security

This section shows system level protections available on your machine.

### Core Isolation

**Memory Integrity**
- Prevents malicious code from interfering with secure processes

### Security Processor (TPM)

**TPM (Trusted Platform Module)**
- A secure hardware chip used for encryption and system integrity
- Used by features like BitLocker
- Found in most modern systems. 

---

## BitLocker

BitLocker is a drive encryption tool built into Windows.

**Function**
- Protects data from theft or unauthorized access

**Best Used With**
- TPM 1.2 or newer hardware chip

---

## Volume Shadow Copy Service (VSS)

Volume Shadow Copy Service allows for point in time snapshots of disk volumes.

**Functionality**
- Create restore points
- Configure or delete restore settings
- Restore system state

**Storage Location**
- Stored in System Volume Information on each protected drive

**Security Consideration**
- Some malware attempts to delete shadow copies to block recovery

---

