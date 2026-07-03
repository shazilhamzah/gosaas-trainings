# Linux: Operating System Basics

## 1. Introduction to Linux & Core Concepts
### Operating System Components
* **Linux Kernel**: The core of the operating system, originally created by Linus Torvalds. It directly interacts with the hardware, managing CPU, memory, and devices.
* **GNU Utilities**: (GNU stands for "GNU's Not Unix"). A suite of essential system tools, libraries, and shell applications that provide the user interface to communicate with the kernel (hence the term **GNU/Linux**).
* **Kernel**: The low-level translation layer between hardware and software.

### Linux Distributions (Distros)
Distros package the Linux kernel with GNU tools, package managers, and graphical desktop environments tailored for different audiences:
* **Ubuntu**: User-friendly, widely used for desktop, server, and IoT (Ubuntu Core).
* **Debian**: Renowned for stability and open-source compliance; the upstream parent of Ubuntu.
* **Kali Linux**: Pre-configured with security and penetration testing utilities.
* **Red Hat Enterprise Linux (RHEL)**: Commercial distribution designed for business environments.

### Desktop Environments (DE)
A Desktop Environment provides the graphical user interface (GUI) on top of the OS, containing window managers, menus, and system widgets. Common examples include GNOME, KDE Plasma, XFCE, and MATE.

---

## 2. The Terminal Prompt & Directory Structure
### Shell Prompt Structure
When you open a terminal, you see a prompt like `useraccount@systemname:~$`
* `useraccount`: The name of the currently logged-in user.
* `systemname`: The hostname of the computer.
* `~`: Represents the current user's home directory (e.g., `/home/useraccount`).
* `$`: Indicates a standard, non-root user shell. A `#` prompt indicates a root (administrator) shell.
* `/`: The root directory, the base of the entire Linux file system.

### Pathing
* **Absolute Path**: The full path starting from the root directory `/` (e.g., `/home/username/test/file.txt`).
* **Relative Path**: A path relative to the current working directory, acting as a shortcut (e.g., `test/file.txt`).

### Hidden Files
* Prefixing a file or directory name with a dot `.` (e.g., `.file.txt`) makes it hidden.

### Basic Commands
* `man <command>`: Displays the system manual pages for a command.
* `pwd` (Print Working Directory): Shows the absolute path of your current directory.
* `ls -a`: Lists all files, including hidden ones.
* `ls -l`: Long listing format showing file permissions, owner, group, size, and modification date.
* `ls -la` / `ls -lah`: Lists all files in long format, with `-h` formatting sizes in human-readable units (K, M, G).
* `mkdir -p <dir>`: Creates directories, including parent directories if they don't exist. Does not raise an error if the directory already exists.
* `rmdir -p <dir>`: Removes empty directories and their parent directories if they become empty.
  > [!WARNING]
  > `rmdir` can only delete **empty** directories. To remove a directory with contents, use `rm -r`.

---

## 3. Working with Files
### File System Rules
* **Case Sensitivity**: Linux is case-sensitive; `readme.txt` and `README.txt` are distinct files.
* **Everything is a File**: Directories, devices, keyboards, and network cards are represented as files.
* **Special Directory References**:
  * `.`: Refers to the current directory.
  * `..`: Refers to the parent directory.

### Commands for Managing Files
* `file <filename>`: Identifies the file type (text, image, binary, etc.) and properties.
* `touch <filename>`: Creates an empty file or updates the timestamps of an existing file.
* `rm -i <files>`: Prompts for confirmation before removing each file.
* `rm -rf <path>`: Recursively (`-r`) and forcefully (`-f`) removes files and directories. **Warning:** Cannot be easily undone.
* `cp <source> <dest>`: Copies a file.
* `cp -r <source_dir> <dest_dir>`: Copies a directory and its contents recursively.
* `mv <source> <dest>`: Renames a file or moves it to a new location.
  * `mv file1.txt file2.txt` (Rename)
  * `mv file1.txt test/file2.txt` (Move and Rename)

---

## 4. Working with File Content
* `head -n <lines> <file>`: Displays the first N lines of a file (default is 10 lines).
* `tail -n <lines> <file>`: Displays the last N lines of a file (default is 10 lines).
* `cat <file>`: Outputs the entire contents of a file to the terminal.
* `echo "text" > file.txt`: Overwrites or creates `file.txt` with "text" (standard output redirection).
* `cat > file1.txt`: Creates `file1.txt` and accepts terminal keyboard input. Press `Ctrl + D` to save and exit.
* `cat file1.txt > file2.txt`: Copies the content of `file1.txt` to `file2.txt`.
* `more <file>`: Displays file contents page-by-page.
* `less <file>`: More advanced than `more`; reads file contents line-by-line, allows backward navigation, and is faster for loading large files.

---

## 5. Linux Directory Structure
* `/bin`: Essential user command binaries (e.g., `cd`, `ls`, `mkdir`).
* `/boot`: Files needed to boot the system (the kernel and bootloader files).
  * **GRUB (Grand Unified Bootloader)**: The default bootloader that allows selecting kernels or booting recovery mode.
* `/dev`: Device nodes representing hardware components (e.g., disks `/dev/sda`, CD-ROMs).
* `/etc`: System-wide configuration files (e.g., configurations for network interfaces, Apache, MySQL).
* `/home`: User home folders (e.g., `/home/alice`).
* `/lib`: Shared libraries required by binaries in `/bin` and `/sbin`.
* `/media`: Automatic mount points for removable media (e.g., USB drives).
* `/mnt`: Temporary manual mount points for filesystems.
* `/opt`: Optional third-party software packages.
* `/proc`: Virtual filesystem containing system resource information and process details.
* `/root`: Home directory for the superuser (root).
* `/run`: Volatile runtime data containing system information since last boot.
* `/sbin`: Essential system binaries used by root for maintenance (e.g., `fdisk`, `reboot`).
* `/tmp`: Temporary files, often cleared automatically on reboot.
* `/usr`: Shared user utilities, libraries, read-only binaries, and documentation.
* `/var`: Variable data files that change dynamically, such as logs (`/var/log`) and mail spools.
* `/swapfile`: Virtual memory space allocated on the hard drive. Used when physical RAM is full.

---

## 6. System Information & Troubleshooting
* `uptime`: Shows system runtime, logged-in users, and load averages.
* `free`: Displays free and used physical memory (RAM) and swap memory.
* `ps`: Returns a snapshot of currently running processes.
* `df -h`: Shows disk space usage of mounted filesystems in human-readable format.
* `sudo fdisk -l`: Lists partition tables and disk sizes.
* `lsblk`: Displays block storage devices (disks and partitions) in a tree layout.
* `top`: Live, interactive process viewer showing CPU/Memory consumption.
* `htop`: Improved, interactive, color-coded version of `top`.

---

## 7. Networking & Package Management
### Networking Commands
* `ifconfig`: Legacy tool to view and configure network interfaces (requires `net-tools`).
* `ip a` (or `ip address`): Modern replacement for `ifconfig` to inspect network interfaces.

### APT Package Manager
The Advanced Package Tool (APT) is the package manager for Debian and Ubuntu:
* `sudo apt update`: Updates the local index database of packages (checks for updates).
* `sudo apt upgrade`: Upgrades all installed packages to their latest versions.
* `sudo apt search <package>`: Searches repositories for available software (e.g., `gimp`).
* `sudo apt install <package>`: Downloads and installs the package.
* `sudo apt remove <package>`: Uninstalls the software but preserves configuration files.
* `sudo apt purge <package>`: Uninstalls the software and deletes all its configuration files.

### Text Editors
* Command-line text editors include **`nano`** (easy to learn, basic) and **`nvim`** (Neovim, advanced and highly customizable).

---

## Summary
This note covers:
* The definition of Linux, GNU utilities, kernel functions, and Linux distributions.
* Shell prompt components, path structures, and standard system navigation.
* File management commands (`mkdir`, `rm`, `cp`, `mv`).
* File content operations (`head`, `tail`, `cat`, output redirection, `less`).
* The purpose of each standard root directory (e.g., `/etc`, `/bin`, `/var`) and swap files.
* System diagnostics commands (`uptime`, `free`, `df`, `ps`, `htop`).
* Network interface commands and the `apt` package manager commands.
