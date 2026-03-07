# 1. The Linux Filesystem Hierarchy (The "Tree")

* In linux "`Everything is a File`"
* Unlike Windows, where you there is a `C:` or `D:` drive, Linux uses a single unified tree structure like a giant tree
* The very bottom (or start) is called the **Root**, represented by a single forward slash: `/`
* Every single file, folder, and even your hardware (like hard drives and USBs) exists somewhere under this root
* The Filesystem Hierarchy Standard (FHS) defines the directory structure and directory contents in Unix-like operating systems

<br>

Breakdown of the major directories, based on the architecture of a standard `Linux system`

<img src="linux/assets/images/linux-fs-hierarchy.gif" width="800px" alt="linux-fs-hierarchy">

| Directory | Name / Purpose          | Industrial Cloud Use Case                                                |
| :-------- | :---------------------- | :-------------------------------- :------------------------------------- |
| `/`       | **Root**                | The base of the entire system. Every file resides here.                  |
| `/bin`    | **User Binaries**       | Essential command binaries (`ls`, `cp`, `ping`) for all users.           |
| `/boot`   | **Static Boot Files**   | Contains the Linux kernel and bootloader files to start the system.      |
| `/dev`    | **Device Files**        | Hardware (HDD, USB) represented as files (e.g., `/dev/sda`).             |
| `/etc`    | **Configuration Files** | Critical folder. Contains system-wide configs like `nginx` or `passwd`.  |
| `/home`   | **User Directories**    | Personal space for users (e.g., `/home/ubuntu`) for settings/files.      |
| `/lib`    | **System Libraries**    | Shared library files required by binaries in `/bin` and `/sbin`.         |
| `/media`  | **Removable Media**     | Temporary mount point for removable devices like USB sticks.             |
| `/mnt`    | **Mount Directory**     | Used by admins to mount external filesystems like AWS EBS volumes.       |
| `/opt`    | **Optional Add-ons**    | Third-party software packages (Google Chrome, enterprise apps).          |
| `/proc`   | **Process Information** | Virtual filesystem containing system resource and process info.          |
| `/root`   | **Root Home**           | The private home directory for the System Admin (`root user`).           |
| `/run`    | **Runtime Data**        | Stores system info since last boot (e.g., logged-in users).              |
| `/sbin`   | **System Binaries**     | Admin-only commands (e.g., `iptables`, `reboot`, `fdisk`).               |
| `/tmp`    | **Temporary Files**     | A "scratchpad" for the system. Usually deleted upon reboot.              |
| `/usr`    | **User Resources**      | Contains the majority of user utilities and applications.                |
| `/var`    | **Variable Data**       | Log Central. Contains files that change frequently (`/var/log`).         |

Pro-Tip for Cloud Engineers
When working with AWS or Azure, you'll spend 90% of your troubleshooting time in:

/etc (to fix a broken Nginx/Apache config)

/var/log (to see why a service crashed)

/mnt or /dev (when attaching new storage volumes)

<br>

# 2. Navigating the System: Key Commands

Fundamental commands to move through the linux filesystem hierarchy

## 1. `pwd` (Print Working Directory)

* **Purpose**: Tells you exactly where you are in the filesystem tree
* **Industry Context**: Before running a "delete" command or editing a config, always run pwd to ensure you aren't in the wrong directory
* **Industrial Use Case**: When you SSH into a remote server, you might feel lost. Running pwd confirms your current location before you run a delete command

`example`
```bash
pwd  # Returns: /home/ubuntu (home irectory after ssh)
```

## 2. `ls` (List)

* Purpose: Shows the contents of a directory
* Professional Flags:

| **Command**   | **What it does ?**                                                                       |      
|---------------|------------------------------------------------------------------------------------------|
| `ls -l`       | (Long Listing) Shows permissions, owner, size, and timestamp                             |
| `ls -a`       | (All) Shows hidden files (those starting with a .)                                       |
| `ls -h`       | (Human Readable) Converts bytes to KB, MB, or GB                                         |
| `ls -t`       | Sorts files by modification time (newest first)                                          |
| `ls -r`       | Shows contents of current directory recursively and also its subdirectories if available |

`example`

```bash
ls -lah /var/log  # List all logs with detailed info from /var/log directory
```
```bash
ls -ltr # list all files from current directory
```
<br>

## 3. `cd` (Change Directory)

* Purpose: Moves your terminal session from one folder to another
* Essential Shortcuts:

| Command                     | Action / Purpose                        | Practical "Why"                                |
| :-------------------------- | :-------------------------------------- | :--------------------------------------------- |
| `cd /`                      | Go to the very top (Root).              | Access system-wide configs in /etc.            |
| `cd ~` or `cd`              | Jump back to your Home directory.       | Quick reset to your personal workspace.        |
| `cd ..`                     | Move up one level.                      | Back out of a subdirectory.                    |
| `cd ../../`                 | Move up two levels.                     | Jump from /var/log/nginx to /var.              |
| `cd -`                      | Move to the previous directory.         | Like a "back" button between two folders.      |
| `cd ../another_directory`   | Move up and then into a sibling folder. | Switch projects without going to Root.         |
| `cd "My Documents"`         | Change to directory with spaces.        | Handles folders with spaces safely (`Quotes`). |
| `cd My\ Documents`          | Change to directory with spaces.        | Manual way to handle spaces (`Escape char`).   |
| `cd /var/l<Tab>`            | Use tab completion to auto-complete.    | Expands to /var/log; saves typing time.        |
| `cd /path && ls`            | Chain commands to move and list.        | Only lists if the directory change succeeds.   |

<br>

# 3. Understanding Paths: Absolute vs. Relative

* In Linux, a "path" is the address of a file or folder
* **Absolute Path**:
  * **Definition**: The path starting from the Root (/). It is a complete address that works from anywhere in the system
  * **Starts with**: `/`
  * **Example**: `/var/log/nginx/error.log`
  * **When to use**: Inside scripts or automation (like Crontabs) to ensure the system always finds the file regardless of where the script is run
* **Relative Path**:
  * **Definition**: The path relative to your current location (`pwd`)
  * **Starts with**: A folder name, `.` (current), or `..` (parent)
  * **Example**: If you are in `/var/log`, the relative path to the error log is `nginx/error.log`
  * **When to use**: Quick manual navigation in the terminal to save typing

`Absolute vs. Relative Path Cheat Sheet`

| Feature             | Absolute Path (Complete Address)             | Relative Path (Current Context)              |
| :--------------     | :------------------------------------------- | :------------------------------------------- |
| **Definition**      | Path starting from the Root (`/`).           | Path relative to your current location.      |
| **Starts With**     | `/` (The forward slash)                      | Folder name, `.` (current), or `..` (parent) |
| **Example**         | `/var/log/nginx/error.log`                   | `nginx/error.log` (if you are in `/var/log`) |
| **Best Used For**   | Scripts, Automation, Crontabs, Systemd       | Manual terminal navigation, local testing    |
| **Reliability**     | Works from anywhere in the system.           | Only works if your current location is right.|

## Use Cases

The same goal is achieved using both `Absolute` or `Relative Path`, depending on your "**Starting Point**" (`pwd`)

### Scenario 1: You are in home directory `/home/ubuntu`. You want to see the system logs in `/var/log`
* **Target**: `/var/log`
* **Absolute**: `cd /var/log` (Direct jump)
* **Relative**: `cd ../../var/log` (Moving up twice, then down)

### Scenario 2: Accessing Web Server Logs
* **Target**: `/var/log/nginx/access.log`
* If you are in `/home/ubuntu` (home directory)
  * **Absolute**: `cat /var/log/nginx/access.log`
  * **Relative**: `cat ../../var/log/nginx/access.log`

### Scenario 3: Running a Local Script
* **Target**: A script named `deploy.sh` inside your current folder
* **Absolute**: `/home/user/projects/website/deploy.sh`
* **Relative**: `./deploy.sh`

### Scenario 4: Writing a Cron Job (Automation)
* **Target**: Backing up `/etc/nginx` to `/backup`
* **The Command**: `cp -r /etc/nginx /backup`
* **Verdict**: Always use Absolute in scripts.
  * Since you don't always know which directory the system is "standing in" when a background task runs
  * Absolute paths prevent the script from failing or creating files in the wrong place
