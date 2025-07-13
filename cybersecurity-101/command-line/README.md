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

# Part 2 - Windows Power Shell
