# Part 1 - Windows Command Line

## Basic System Information

I learned how to get basic system details quickly:

- `set` shows environment variables, including the system’s executable path.
- `ver` displays the Windows OS version.
- `systeminfo` gives an overview of the machine, including OS build, CPU, memory, and more.
- Long outputs like `systeminfo` or `driverquery` can be piped into `more` to read them page by page.
- `help` lists available commands, and `cls` clears the screen.

---

## Network Troubleshooting

This part was about checking and diagnosing network configurations.

- `ipconfig` shows your IP address, subnet mask, and default gateway.
- `ipconfig /all` gives more detail like DNS servers and whether DHCP is enabled.
- `ping example.com` tests if a host is reachable.
- `tracert example.com` maps the route packets take to reach a host.
- `nslookup example.com` looks up the IP address of a domain name.
- `netstat` shows active connections and ports. Adding flags like `-abon` reveals the process ID (PID) and associated program (like `sshd.exe`), which helps identify what’s using which port.

---

## File and Disk Management

This section covered how to navigate the file system and manage files without a GUI:

- `cd` shows your current directory, or lets you move into one.
- `dir` lists files and folders in the current directory. Adding `/a` shows hidden files; `/s` includes subdirectories.
- `tree` gives a visual structure of folder hierarchy.
- `mkdir` creates a folder. `rmdir` removes one.
- `type file.txt` shows the contents of a text file.
- `more file.txt` lets you read long files one page at a time.
- `copy`, `move`, and `del` (or `erase`) let you duplicate, move, or delete files.
- Wildcards like `*.md` help you manage groups of files at once.

---

## Task and Process Management

This task was about monitoring and controlling running programs from the command line:

- `tasklist` lists all active processes.
- `tasklist /FI "imagename eq process.exe"` filters the list to a specific process.
- `taskkill /PID <PID>` lets you terminate a process by its ID.
  
These commands offer command line control similar to what you'd get from Task Manager, but without needing the GUI.

---

## Extra Tools

- `chkdsk` to check disk health,
- `driverquery` to list drivers,
- `sfc /scannow` to scan and fix system files.

You can use `/?”` with nearly any command to pull up its help page. Also, the `more` command is versatile, it works both for viewing long files and for paginating long command output.

`shutdown /s` shuts down the system, `shutdown /r` restarts it. 

# Part 2 - Windows PowerShell

## Core Commands 

### What PowerShell Actually Is
- It’s a shell and scripting language built on .NET
- It works across platforms, not just Windows
- Unlike cmd, it returns objects instead of just text
- You can automate system tasks and dig deep into Windows with just a few commands

---

### Working With Files and Folders

| Task                  | Command                                                   |
|-----------------------|------------------------------------------------------------|
| Show contents of a folder | `Get-ChildItem -Path C:\Users`                          |
| Move into a folder        | `Set-Location -Path .\Documents`                        |
| Create a folder           | `New-Item -Path .\folder -ItemType Directory`           |
| Create a file             | `New-Item -Path .\folder\file.txt -ItemType File`       |
| Delete a file or folder   | `Remove-Item -Path .\file.txt`                          |
| Copy or move files        | `Copy-Item`, `Move-Item`                                |
| View file contents        | `Get-Content -Path .\file.txt`                          |

PowerShell made it easy to jump around the file system and manage files with a few simple cmdlets.


---

### Piping, Filtering, Sorting

| Task | Command |
|------|---------|
| Sort files by size | `Get-ChildItem | Sort-Object Length` |
| Filter files over 100 bytes | `Where-Object -Property Length -gt 100` |
| Match names that start with "ship" | `Where-Object -Property Name -like ship*` |
| Only show name and size | `Select-Object Name, Length` |
| Search inside a file | `Select-String -Path .\file.txt -Pattern hat` |

PowerShell pipes real objects, not just raw text. That means you can sort, filter, and pull out details way more precisely.

---

### Checking What’s Running

| Task | Command |
|------|---------|
| View running processes | `Get-Process` |
| Check service status | `Get-Service` |
| See active network connections | `Get-NetTCPConnection` |
| Generate file hash | `Get-FileHash -Path .\file.txt` |

I used these commands to check which processes were running, which services were active or stopped, and what network connections were open. Useful for monitoring or spotting anything strange.

---

### Local Users and Suspicious Services

| Task | Command |
|------|---------|
| View all users | `Get-LocalUser` |
| Check user descriptions | `Get-LocalUser | Select-Object Name, Description` |
| Scan for modified services | `Get-Service` filtered by `DisplayName` |

One of the tasks was about finding a tampered service that had a weird motto in the display name. I matched it with a user’s description using filtering.

---

### Running Commands on Other Machines

| Task | Command |
|------|---------|
| Run something on another computer | `Invoke-Command -ComputerName RoyalFortune -ScriptBlock { Get-Service }` |
| Run a full script remotely | `Invoke-Command -FilePath .\script.ps1 -ComputerName Server01` |

No need to log in manually. These commands let you run things remotely which is powerful for both IT management and red teaming.

---

# Part 3 - Linux Shells

## Introduction to Linux Shells

- Most users interact with OSs using GUIs, but Linux power users often prefer the CLI (Command Line Interface) for efficiency and control.
- The **Shell** is the interpreter that reads commands from users and executes them in the OS.
- Using the CLI is like cooking in the kitchen yourself (vs. being served at a restaurant via the GUI).
- Learning the Linux shell gives users more flexibility, power, and automation capabilities.

---

## How to Interact With a Shell

- Most distributions use **Bash** as the default shell.
- Common shell commands:
  - `pwd` – shows the current working directory
  - `cd <directory>` – changes the working directory
  - `ls` – lists directory contents
  - `cat <file>` – displays the contents of a file
  - `grep <pattern> <file>` – searches for a pattern in a file

---

## Types of Linux Shells

- `echo $SHELL` - view your current shell
- `cat /etc/shells` - see all installed shells
- command shell types - Bash, Fish, ZSH

| Feature             | Bash               | Fish                       | Zsh                            |
|---------------------|--------------------|-----------------------------|--------------------------------|
| **Full Name**       | Bourne Again Shell | Friendly Interactive Shell | Z Shell                        |
| **Scripting**       | Extensive support  | Limited scripting features | Strong, modern scripting       |
| **Tab Completion**  | Basic              | Advanced and contextual     | Extensible via plugins         |
| **Customization**   | Basic              | Good via interactive tools | Advanced (`oh-my-zsh`, themes) |
| **User Friendliness** | Traditional      | Very beginner-friendly     | High with proper setup         |
| **Syntax Highlighting** | Not built-in  | Built-in                   | Available via plugins          |


## Shell Scripting and components
Shell scripting automates tasks by grouping commands into .sh files. Key concepts:

- `#!/bin/bash` - tells the system to use the bash interpreter (shebang)
- `chmod +x script.sh` - gives execute permissions. used after the script
- `./script.sh` - running the script
