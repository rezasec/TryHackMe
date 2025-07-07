# Linux Fundamentals: Part 1, 2 & 3 


## Part 1: The Basics

### Connecting to a Machine with SSH
  - `ssh username@ip address`
  - SSH is used to securely log into remote systems. SSH lets you securely send commands from your computer (AttackBox) to a remote machine (Target Machine). 

###  Navigating the Filesystem
- `pwd` – show the full path of your current directory.
- `ls` – List files/folders in the current directory
- `ls -l` – Show detailed file info (permissions, owner, size).
- `cd foldername` – Move into a folder.
- `cd ..` – Go up one level.
- `echo` - prints text to the screen. 
- `whoami` - tells you which user your currently logged in as (used to verify session identity).
- `cat` - contents of the file
  

### Viewing File Content
- `cat file` – View all contents at once.
- `less file` – View contents page by page.
- `head file` – First 10 lines.
- `tail file` – Last 10 lines.

### Finding Files and Getting Help (search what we need fast and efficiently) 
-`find` - Used to search the filesystem for files and directories (use it when you dont know where a file is). 
- `find . -name filename` – Search for a file starting from current dir.
- `man command` – Open the manual for a command.
- `grep` - Use to search inside files for specific words or values.

### Shell Operators
- Symbols that let you do more advanced stuff with Linux commands like running things in the background, chaining commands, or redirecting output to files.
- `&` - run a command in the background so your terminal isn’t stuck waiting (cp bigfile.zip backup/ &). 
- `&&` - lets you chain commands, but the second one only runs if the first one works. cd test will only run if the mkdir test command succeeds. (mkdir test && cd test) 
- `>`- take the output of a command and save it into a file. If the file exists, it will be overwritten (echo hey > welcome) (cat welcome = hey).
- `>>` - Same as >, but this one appends to the file instead of replacing the contents ( echo hello >> welcome) (cat welcome = hey hello)


---

##  Part 2: Interacting with Files

- `flags` - modifiers you can attach to commands to change how they behave ( - or --). 
- ` ls -a` - list all files, including hidden ones
- ` ls --help` - show all possible flags/switches and what they do
  
###  Creating and Managing Files
- `touch filename` – Create an empty file.
- `mkdir foldername` – Make a new folder.
- `rm file` – Delete file.
- `rm -r folder` – Delete folder recursively.
- `cp file1 file2` – Copy file.
- `mv file1 file2` – Move or rename file.

### What Kind of File Is This?
- `file filename` – Show what type of file it is (text, binary, image, etc.).

### Permissions and Ownership
- Run `ls -l` to see file permissions 
- Permissions are split into **owner**, **group**, and **others**:
  - r = read, w = write, x = execute (-rwxr-xr--)
  - rw- : user owner has read, write, and execute.
  - r-x : group has read and execute.
  - r-- : others have read only.
- `su username` – switch to another user.

### Important Directories
| Directory | Purpose |
|----------|---------|
| `/etc` | Stores essential system config files. Config files like sudoers, passwd | 
| `/var` | Contains variable data like logs and service data. Logs, backups, runtime data. Useful for analysis or investigation |
| `/root` | Home directory for the root user. |
| `/tmp` | A writable directory for temporary file storage (wiped on reboot). |

---

##  Part 3: Practical System Use

### Terminal Text Editors
Efficiently editing files from the terminal is essential in system administration, scripting, automation, and troubleshooting.
- `nano filename` – Easy editor. Ctrl+O to save, Ctrl+X to exit, Ctrl+W to search, Ctrl+k/Ctril+u to cut/paste.
  - Use nano to quickly modify config files, write scripts, or troubleshoot logs on remote systems via SSH.
- `vim filename` – Advanced and customizable editor

### Downloading Files
Transferring files is fundamental for scripting, backups, CTFs, and pentesting. Knowing how to download from the internet, copy across machines, or serve own files is vital for speed and flexibility in Linux environments.
- `wget http://example.com/file.txt`
- use to download files from the internet via HTTP/HTTP (web downloading). 

### Copying Files Between Systems
`scp` - secure copy from SSH. Securely transfers files between local ↔ remote systems.
- From local → remote: scp myfile.txt user@ip:/path/to/destination
- From remote → local: scp user@ip:/path/file.txt ./localname.txt
  
### Hosting Files with Python (Quick Server)
- `python3 -m http.server` – Quickly share files in a directory by running a web server. Serve files in current directory on port 8000. 
- Access it via browser or `wget http://ip:8000/filename`

### Processes and Performance
- `ps` – View processes for current user.
- `ps aux` – View all system processes.
- `top` – Real time process monitoring.
- `kill PID` – Kill process.
- Common signals:
  - `SIGTERM` – Graceful stop
  - `SIGKILL` – Force stop
  - `SIGSTOP` – Suspend

### Background & Foreground Jobs
- Add `&` to run something in background.
- `Ctrl + Z` – Suspend job
- `fg` – Resume job in foreground

### Services & Startup
- `systemctl start|stop|enable|disable service`
  - `start` – Start service now
  - `enable` – Start on boot

### Cron Jobs
- View/edit crontab: `crontab -e`
Format:
- `MIN` - Minute (0–59) ex = 0
- `HOUR` - Hour (0–23)	ex = */12
- `DOM` - Day of Month (1–31)	ex = *
- `MON` - Month (1–12)	ex =*
- `DOW` - Day of Week (0–7, Sun=0/7)	ex = *
- `CMD` - Command to execute	ex = cp ...

- Example: Run every 12 hours
 0 */12 * * * cp -R /home/user/Documents /backup/
---

## Package Management (APT)

### Search and Install
- `apt update` – Update package list
- `apt install packagename`
- `apt remove packagename`

### Adding Repos Manually
- Add GPG key: wget -qO - https://... | sudo apt-key add -
 
- Add source: sudo nano /etc/apt/sources.list.d/custom.list

- Then `apt update` and install.

---

## Logs and Monitoring

- Most logs are in `/var/log`
- Examples:
  - `/var/log/auth.log` – Login attempts
  - `/var/log/apache2/access.log` – Website requests
  - `/var/log/apache2/error.log` – Web server errors

Logs are crucial for monitoring attacks, debugging issues, and seeing what happened on your system.

---
