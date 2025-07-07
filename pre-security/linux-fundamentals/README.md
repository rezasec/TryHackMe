# Linux Fundamentals: Part 1, 2 & 3 summary


## Part 1: The Basics

### Connecting to a Machine with SSH
  - `ssh username@ip address`
  - SSH is used to securely log into remote systems.

###  Navigating the Filesystem
- `pwd` – Print current directory.
- `ls` – List files.
- `ls -l` – Show detailed file info (permissions, owner, size).
- `cd foldername` – Move into a folder.
- `cd ..` – Go up one level.

### Viewing File Content
- `cat file` – View all contents at once.
- `less file` – View contents page by page.
- `head file` – First 10 lines.
- `tail file` – Last 10 lines.

### Finding Files and Getting Help
- `find . -name filename` – Search for a file starting from current dir.
- `man command` – Open the manual for a command.

---

##  Part 2: Interacting with Files

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
- Run `ls -l` to see file permissions (e.g., `-rw-r--r--`).
- Permissions are split into **owner**, **group**, and **others**:
  - r = read, w = write, x = execute
- `su username` or `su - username` – Switch to another user.

### Important Directories
| Directory | Purpose |
|----------|---------|
| `/etc` | Config files like sudoers, passwd |
| `/var` | Logs, backups, runtime data |
| `/root` | Home for root user |
| `/tmp` | Temporary storage (wiped on reboot) |

---

##  Part 3: Practical System Use

### Terminal Text Editors
- `nano filename` – Easy editor. Ctrl+O to save, Ctrl+X to exit.
- `vim filename` – Advanced editor. (Not covered deeply here.)

### Downloading Files
- `wget http://example.com/file.txt`
- Starts a file download from a URL.

### Copying Files Between Systems
- From local → remote:
  ```bash
  scp myfile.txt user@ip:/path/to/destination
  ```
- From remote → local:
  ```bash
  scp user@ip:/path/file.txt ./localname.txt
  ```

### Hosting Files with Python (Quick Server)
- `python3 -m http.server` – Serve files in current directory on port 8000.
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
- Format:
  ```
  MIN HOUR DOM MON DOW CMD

  ```
- Example: Run every 12 hours
  ```
  0 */12 * * * cp -R /home/user/Documents /backup/
  ```

---

## Package Management (APT)

### Search and Install
- `apt update` – Update package list
- `apt install packagename`
- `apt remove packagename`

### Adding Repos Manually
- Add GPG key:
  ```bash
  wget -qO - https://... | sudo apt-key add -
  ```
- Add source:
  ```bash
  sudo nano /etc/apt/sources.list.d/custom.list
  ```
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
