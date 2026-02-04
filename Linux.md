                                             
**What is Linux?**
Linux is a free and **open-source,** Unix like operating system (OS) that manages a computer’s hardware and software.
- Linux technically is a kernel, but not a complete OS.
- It is based on Unix principles meaning it is stable, secure and designed for multi-tasking and multi-user environments.
- Kernal + GNU = Linux OS. (GNU: Set of utilities + tools like GNU editors, GNU debugger, GNU compiler, GNU core utilities like ls, cp, mv, rm, etc.).
In 1991, Linus Torvalds developed this Linux. 
Why was Linux Created?
Unix was powerful but expensive and closed source. Linux filled the gaps: free, stable, secure, customizable environment and available to everyone. 
Before Linux? - History:
- 1969: Ken Thompson and Dennis Ritche created UNIX OS, later written in C for portability.
- 1980: Richard Stallman began a GNU project to create a free UNIX-like OS, namely MINIX, BSD etc.
- 1991: MINIX is designed for teaching purpose and not feasible for large scale use. BSD kernel is strong, but tied up with few AT & T UNIX legal issues.
- Solving these, LINUX was created.
- 1992: LINUX Kernel was released under GNU general public license.
**Architecture of Linux:**
It’s a **layered **architecture.

<img width="589" height="589" alt="image" src="https://github.com/user-attachments/assets/3c65e42a-1a53-47c3-91fb-4603eb8b27ac" />

Like any operating system, Linux consists of software, computer programs, documentation, and hardware.
The main components of Linux operating system are: Application, Shell, Kernel, Hardware, Utilities
- Hardware Layer: Physical components like CPU, RAM, storage, and devices.
- Kernel: Core part of Linux managing hardware, memory, and processes.
- System Libraries: Bridge between user applications and the kernel (e.g., GNU C Library).
- Shell: Command interface that passes user instructions to the kernel.
- Applications: User programs like browsers, editors, and utilities running on top.
**Distributions of Linux:**
A Linux system package, known as a distribution, consists of multiple Linux distributions available for different computing needs. Linux distribution is developed using a set of software based on compatibility with the Linux core kernel, using which Linux-based operations in different systems, such as personal systems, embedded systems, etc. There are around 600 distributions available.
Each distribution has specialized packages installed to support specific tasks.
Few of them are as follows:
- Debian: Stable and community driven. Debian is a Linux-based operating system for a wide range of devices including laptops, desktops and servers.
- Ubuntu: Ubuntu is better for beginners and general desktop users, as it offers a more user-friendly, out-of-the-box experience with modern features and easier installation.
- Red Hat Enterprise Linux: A stable, high-performance Linux platform with built-in security and management features to help you run critical workloads anywhere in the hybrid cloud. Enterprise- focused.
- Fedora: Linux distribution developed by the Fedora Project and has cutting-edge features. It was originally developed in 2003 as a continuation of the Red Hat Linux project.
- Cent OS: CentOS is a stable, secure, open-source Linux distribution derived from RHEL, known for its stability and long-term support.
- Arch Linux: A simple, lightweight distribution driven 100% by community.
- Kali Linux: An Advanced Penetration Testing Linux distribution used for Penetration Testing, Ethical Hacking and network security assessments.
**Navigation Basics:**
Linux is a hierarchical file system. Understanding navigation helps in exploring, configuring, and maintaining the system effectively.
**Core Commands:**

| Command | Description | Example |
| --- | --- | --- |
| pwd | Displays current directory path | pwd |
| ls | Lists directory contents | ls -l /home |
| cd | Changes directory | cd /var/log |
| tree | Displays directory structure graphically | tree /etc |
| find | Searches files, directories | find / -name nginx.conf |
| df | Shows file system usage | df -h |

Basic/ Options of Listing Commands:
- ls –l: Provides information in a detailed “long listing” format.
- ls –a: Displays all files including hidden files.
- ls –s: Sorts by file size, largest first.
- ls –t: Sorts by modification time, latest time first.
**Editing Files:**
Editing files is one of the most common **administrative tasks** and used especially for **modifying configurations** or scripts.
**Popular Editors**
- Vim (Advanced Users)
Launch: vim /etc/hosts
Modes: i (Insert), Esc (Command), v (Visual)
Save & Quit: :wq, Quit without saving: :q!
- Nano (Beginner-Friendly)
Launch: nano file.txt
Save: Ctrl + O
Exit: Ctrl + X
**Moving Files/Directories:**
The mv command is a command used for **renaming and moving** files and directories within a filesystem.
The main command is:
mv [options] <source> <destination>

| Option | Description |
| --- | --- |
| -i | Interactive mode, asks before overwriting files |
| -f | Force move, overwrite without asking |
| -n | No overwrite – skip existing files |
| -v | Verbose – shows what’s being moved |

**Basic Usage:**
***Move a file to another directory:***
mv file1.txt /home/user/Documents/   (Moves file1.txt to the Documents folder.)
***Rename a file:***
mv oldname.txt newname.txt (Renames oldname.txt to newname.txt.)
***Moving Directories***
Move an entire directory:
mv dir1 /home/user/Projects/   (Moves dir1 to the Projects directory.)
***You can move multiple files at once:***
mv file1.txt file2.txt folder/
**Creating & Deleting Files/Directories:**
**Creating:**
**Using touch**
Command:
touch <filename>

**Purpose:***** ***Creates an empty file (or updates timestamp if file exists).
**Examples:**
touch notes.txt                  # Creates a single file
touch file1.txt file2.txt        # Creates multiple files

**Using Redirection Operators**
Command:
> filename

or
cat > filename

**Purpose:***** ***Creates a new file and allows immediate writing.
**Examples:**
> report.txt           # Creates empty file
cat > todo.txt       # Opens editor to type; press Ctrl + D to save & exit, 
cat: command also displays the content of the file.
**Using Editors**
**Examples:**
nano file.txt
vi file.txt

Opens text editors to create or edit files.
echo “statement/content” > filename: Without opening editor, it pushes the file content either in new file or existing file as we mention.
**Deleting:**
**Command:**
rm  <Options> <filename>

**Examples:**
rm file.txt                   # Deletes one file
rm file1 file2 file3      # Deletes multiple files
**Options:**

| Options | Description |
| --- | --- |
| -i | Interactive, asks before delete. |
| -f | Force delete(no confirmation) |
| -v | Verbose, shows what is deleted. |

**Deleting Directories**
***a) If Empty:***
rmdir directory_name      #Deletes only empty directories.
***b) If Not Empty:***
rm -r directory_name       #Deletes directory and its contents recursively.
**Options:**

| Option | Description |
| --- | --- |
| -r | Recursive delete |
| -rf | Force delete recursively (no confirmation) |

**Viewing Content**

| Command | Content | Default Lines | Common Use |
| --- | --- | --- | --- |
| cat | View entire file | All | Small files |
| less | Page-by-page view | - | Large files |
| large | Page-by-page view (basic) | - | Simple Viewing |
| head | View beginning of file | 10 | Preview start |
| tail | View end of file | 10 | Monitor logs |

**Directory Hierarchy:**

<img width="618" height="377" alt="image" src="https://github.com/user-attachments/assets/a1212971-0638-43eb-8df1-ba24e993f556" />

**Text Processing Commands**
Text processing commands are used to **search, filter, transform,** and format text files or command outputs.
They are essential for shell scripting, log analysis, and data manipulation.
**Common Text Processing Commands**
**a)cat: **View or Concatenate Files
- Displays content of files.
- cat file.txt
- Combine multiple files:
- cat file1.txt file2.txt > combined.txt
**b)sort:** Sort Lines in a File
- Sorts text alphabetically or numerically.
- sort file.txt
**Options:**

| Option | Description |
| --- | --- |
| -r | Reverse order |
| -n | Numeric sort |
| -u | Removes dup. |

Example:
sort -nr marks.txt      # Sort numerically in reverse order
**c)uniq:** Remove Duplicate Lines
- Works best with sorted files.
- uniq file.txt
**Options:**

| Option | Description |
| --- | --- |
| -c | Count occurrences |
| -d | Show only duplicates |
| -u | Show only unique lines |

Example:
sort names.txt | uniq -c
**d)wc:** Word, Line, and Character Count
- Displays line, word, and byte/character count.
- wc file.txt
**Options:**

| Option | Description |
| --- | --- |
| -l | Count lines |
| -w | Count words |
| -c | Count characters/bytes |

Example:
wc -l file.txt      # Count number of lines
**e)grep:** Search for Patterns
- Searches for a specific word or pattern in files.
- grep "error" logfile.txt
**Options:**

| Option | Description |
| --- | --- |
| -i | Case-insensitive search |
| -n | Show line numbers |
| -v | Show lines not matching the pattern |
| -r | Search recursively in directories |

Examples:
grep -i "warning" /var/log/syslog
grep -v "pass" results.txt
**f)cut:** Extract Specific Columns
- Extracts text from specific positions or fields.
- cut -d " " -f2 names.txt
**Options:**

| Option | Description |
| --- | --- |
| -d | Specify delimiter |
| -f | Specify field number |
| -c | Specify character position |

Example:
cut -c 1-5 data.txt     # First 5 characters of each line
**g)tr: **Translate or Delete Characters
- Replaces or removes characters.
- tr a-z A-Z < file.txt     # Convert lowercase to uppercase
tr -d " " < file.txt        # Delete spaces
**h)awk:** Field-Based Text Processing
- Processes text by pattern and action.
- awk '{print $1, $3}' file.txt
***Features:***
$1, $2, … → columns (fields)
NR → line number
NF → number of fields in current line
Example:
awk '/error/ {print NR, $0}' logfile.txt
**i)sed:** Stream Editor
- Performs substitution, deletion, or insertion in files.
- sed 's/old/new/' file.txt
**Options:**

| Option | Description |
| --- | --- |
| -s | Substitute text |
| -i | Edit file in-place |
| -d | Delete lines matching pattern |


Examples:
sed 's/linux/Unix/g' notes.txt     # Replace all 'linux' with 'Unix'
sed '/error/d' logfile.txt         # Delete lines containing 'error'
**File Permissions and Ownership**
Every file and directory in Linux has **permissions and ownership** that control who can **read, write, or execute** them.
- This is crucial for system security and user access control.
**File Ownership**

<img width="1200" height="628" alt="image" src="https://github.com/user-attachments/assets/7050bc9b-19d9-45c7-be7a-f5b460ea4c7b" />

Each file or directory has:
**User (Owner):** The creator of the file.
**Group:** A set of users who share certain permissions.
**Others**: All other users on the system.
**File Permission Types**

<img width="801" height="381" alt="image" src="https://github.com/user-attachments/assets/a622a963-43c6-482e-b30d-817651cc5049" />


<img width="800" height="381" alt="image" src="https://github.com/user-attachments/assets/b066bd44-61c6-459f-9875-59a9095de753" />

**Viewing Permissions**
**Command:**
ls -l
Example Output:
-rwxr-xr-- 1 user group 1024 Nov 6 report.txt
**Breakdown:**

| Field | Meaning |
| --- | --- |
| - | File type (- for file, d for directory) |
| rwx | Owner permissions |
| r-x | Group permissions |
| r-- | Others’ permissions |
| user | Owner name |
| group | Group name |
| report.txt | File name |

**Changing Permissions: chmod**
**Command:**
chmod [options] mode filename

**a) Symbolic Mode**
chmod u+x file.txt                 # Add execute permission for user
chmod g-w file.txt                # Remove write permission for group
chmod o+r file.txt                # Add read permission for others
chmod a+r file.txt                # Add read permission for all

| Symbol | Meaning |
| --- | --- |
| u | User (owner) |
| g | Group |
| o | Others |
| a | All (u+g+o) |
| + | Add permission |
| - | Remove permission |
| = | Set exact permission |

**b) Numeric (Octal) Mode**
Each permission (r, w, x) has a number: **r=4, w=2, x=1**
Add them for each user type.

| Permission | Octal Value |
| --- | --- |
| rwx | 7 |
| rw- | 6 |
| r-x | 5 |
| r-- | 4 |

**Example:**
chmod 755 script.sh
Owner: 7 (rwx)
Group: 5 (r-x)
Others: 5 (r-x)
**Changing Ownership: chown**
**Command:**
chown new_owner file.txt       #Changes the file owner.
**Examples:**
chown root file.txt                                  # Change owner to root
chown user1:group1 file.txt              # Change owner and group
**Changing Group Ownership: chgrp**
**Command:**
chgrp groupname file.txt                       #Assigns the file to a different group.
**Example:**
chgrp developers project.txt

**Directory Permissions**
**Example:**
chmod 750 myfolder

Owner: full access
Group: read + execute
Others: no access
**Shell and Other Basics**

**What is a Shell?**
- A Shell is a command-line interpreter that allows users to interact with the Linux operating system.
- It accepts commands, interprets them, and passes them to the kernel for execution.
**Example:**
 When you type ls, the shell tells the kernel to list the directory contents.
**Types of Shells**

<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/eef6c5f9-d86e-4a57-a13b-e9f53aaeff18" />

**To see all shells available:**
cat /etc/shells
**Shell Prompt Symbols**

| Symbol | Description |
| --- | --- |
| $ | Normal user prompt |
| # | Root(superuser) prompt |
| ~ | Home directory |
| / | Root directory |

**Shell Commands Categories**


| Type | Description | Example |
| --- | --- | --- |
| Internal (Built-in) | Executed directly by shell | cd, echo, pwd, set |
| External | Located in system directories like /bin or /usr/bin | ls, cp, grep, cat |

**Variables in Shell Scripting**
- Used to store data temporarily.

<img width="715" height="375" alt="image" src="https://github.com/user-attachments/assets/325bfe50-8f5f-4edb-acc7-851f14070ef3" />


**Example:**
name="Jashwanth"
echo $name

Output → Jashwanth
In shell scripting there are three main types of variables are present. They are:
- Local Variables
- Global Variables or Environment Variables
- Shell Variables or System Variables
**Local Variable**
A local variable is a special type of variable which has its scope only within a specific function or block of code. Local variables can override the same variable name in the larger scope.
**Global Variables**
A global variable is a variable with global scope. It is accessible throughout the program. Global variables are declared outside any block of code or function.
**Shell Variables**
These are special types of variables. They are created and maintained by Linux Shell itself. These variables are required by the shell to function properly They are defined in Capital letters and to see all of them, we can use set / env / printenv command.
A Shell Script is a text file containing a sequence of shell commands.
**Steps:**
***Create a file:***
nano script.sh

***Add commands:***
#!/bin/bash
echo "Hello, Linux!"

***Make executable:***
chmod +x script.sh

***Run it:***
./script.sh

**Process Management**
**What is a Process?**
- A process is a running instance of a program.
- Each process has a unique Process ID (PID).
- Created when a command or program is executed.
**Process States**

<img width="960" height="720" alt="image" src="https://github.com/user-attachments/assets/90c4ee27-ca26-4460-83c8-ee66779740ca" />

***Running:*** Currently executing.
***Sleeping:*** Waiting for resources.
***Stopped:*** Suspended processes.
***Zombie: ***Complete but not yet cleared by parent class.
**Process Management Tasks**

| Category | Description |
| --- | --- |
| Listing Processes | View active processes using ps, top, or htop |
| Finding Processes | Use pgrep or pidof |
| Killing Processes | Terminate using kill or killall |
| Process Priorities | Adjust using nice or renice |
| Foreground/Background Processes | Manage with &, fg, bg, and jobs |
| Process Forking | Parent process creates a child using fork() system call |

**Common Commands**
- Display all running processes
- ps aux
- Monitor system processes in real-time
- top
- Terminate process
- kill <PID>
- Start process with priority
- nice -n <value> <cmd>
- Nice value range from -20 (highest priority) to 19 (lowest priority)
- List background processes
- jobs
- Bring job to foreground
- fg %1
**Service Management**
Linux services are a background process that provides essential system functionality.
Essential functions are like network connectivity, security, and system administration.
**What is systemd?**
- systemd is the system and service manager for modern Linux distributions.
- It is responsible for booting the system, managing services, and handling background daemons.
- Replaces older systems like SysVinit and Upstart.
**Key Features of systemd:**
- Parallel Startup
- Dependency Management
- Logging (journald): Collects and manages logs efficiently
- On-Demand Activation
- Socket/Device-based Activation
**Common systemd Commands**
- Start a service
- systemctl start <service>
- Stop a service
- systemctl stop <service>
- Restart a service
- systemctl restart <service>
- View service status
- systemctl status <service>
- Enable service at boot
- systemctl enable <service>
- Disable service at boot
- systemctl disable <service>
- List active services
- systemctl list-units --type=service
- View service logs
- journalctl -u <service>
**systemd Unit Files**
Located in:
- /etc/systemd/system/ → Custom user-created services
- /lib/systemd/system/ → Default system services
Example of a Unit File (myapp.service):
[Unit]
Description=My Sample Application
After=network.target

[Service]
ExecStart=/usr/bin/python3 /opt/myapp/app.py
Restart=always

[Install]
WantedBy=multi-user.target

**Targets in systemd**

| Target | Purpose |
| --- | --- |
| graphical.target | GUI login environment |
| multi-user.target | Non-GUI, multi-user environment |
| rescue.target | Single-user (maintenance) mode |
| emergency.target | Minimal recovery shell |

***Check current target:***
systemctl get-default

***Change target:***
systemctl set-default multi-user.target
**journalctl common commands: **journalctl is the **log viewer** for **systemd’s journald service**.

| Task | Command | Description |
| --- | --- | --- |
| View all logs | journalctl | Shows complete system logs |
| Follow logs live | journalctl -f | Real-time log updates |
| Logs for a service | journalctl -u ssh.service | Filter by service |
| Logs since specific time | journalctl --since "2025-11-07" | Filter by date/time |
| Kernel logs only | journalctl -k | Show kernel messages |
| Logs from current boot | journalctl -b | Only current boot logs |
| Filter by priority | journalctl -p err | Only errors or warnings |
| Limit entries | journalctl -n 10 | Show last 10 logs |

**File Systems**
- The Linux filesystem organizes and manages data storage on disks.
- It involves disks, partitions, inodes, mounting, and volume management.
**Inodes**
- Inode (Index Node) stores metadata about a file, not the file content itself.
- Each file has one unique inode number.
**Inode contains:**
- File type (regular file, directory, etc.)
- Permissions (rwx)
- Owner and group
- File size
- Timestamps (created, modified, accessed)
- Pointers to data blocks on disk

<img width="1200" height="628" alt="image" src="https://github.com/user-attachments/assets/4d54b733-273b-4dd4-bb14-cf7302c173c2" />

***View inode info:***
ls -i filename
stat filename
**Mounting and Unmounting**
**Mounting**
- To use a disk or partition, it must be mounted to a directory.
- Mount point: the location where filesystem is attached.
**Commands:**
mount /dev/sdb1 /mnt/data

**Unmounting**
- Safely remove or detach a filesystem.
umount /mnt/data

***View mounted filesystems:***
df -h
mount | column –t

## Disk Management
***Check available disks:***
lsblk
fdisk -l

***View filesystem usage:***
df -Th

***Check disk I/O performance:***
iostat


## Adding a New Disk
**Steps:**
- Identify disk:
lsblk

- Partition disk:
fdisk /dev/sdb

- Create filesystem:
mkfs.ext4 /dev/sdb1

- Create mount point:
mkdir /data

- Mount it:
mount /dev/sdb1 /data

- Make it permanent:
 Add entry in /etc/fstab
/dev/sdb1   /data   ext4   defaults   0 0
**LVM (Logical Volume Management)**
In Linux, Logical Volume Manager (LVM) is a device mapper framework that provides logical volume management for the Linux kernel.
**Purpose:**
Manages storage flexibly by combining multiple disks into logical volumes.
**LVM Structure:**

<img width="602" height="623" alt="image" src="https://github.com/user-attachments/assets/cc54e01e-d2ff-464f-9548-e8cccf5dcd9c" />

**Physical Volume (PV):**
A Physical Volume is the base layer in LVM.
It represents a physical storage device such as:
- a disk (/dev/sdb),
- a disk partition (/dev/sdb1), or
- a RAID array.
**Commands:**

| Task | Command |
| --- | --- |
| Create PV | pvcreate /dev/sdb1 |
| List PVs | pvs |
| Display PV details | pvdisplay |


**Volume Group (VG)**
- A Volume Group is created by combining one or more Physical Volumes.
- It’s like VG as a pool of storage space from which logical volumes can be created.
- Each VG can host multiple Logical Volumes (LVs).
Commands

| Task | Command |
| --- | --- |
| Create LV | lvcreate -L 10G -n lv_docs vg_data |
| List LVs | lvs |
| Display LV details | lvdisplay |
| Extend LV | lvextend -L +5G /dev/vg_data/lv_docs |
| Resize filesystem | resize2fs /dev/vg_data/lv_docs |


**Swap Space**
Swap acts as an overflow for RAM, used when physical memory is full.
Located either as:
- A swap partition, or
- A swap file.
***Check swap usage:***
swapon --show
free -h

***Create swap file (if needed):***
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

***Make it permanent:***
 Add to /etc/fstab:
/swapfile   none   swap   sw   0  0
**Common Disk Commands**

| Command | Description |
| --- | --- |
| df -h | Show disk space usage |
| du -sh * | Show size of files/folders |
| lsblk | List block devices |
| fdisk -l | Show partitions |
| mkfs.ext4 | Format a partition |
| mount / umount | Attach or detach filesystems |
| pvcreate, vgcreate, lvcreate | LVM setup |
| swapon, swapoff | Enable/disable swap |

**Package Management**
**Packages:**
Bundle of files like programs, libraries, configurations needed for software.
**Package Management**
The system that handles installing, updating, configuring, and removing software.
- Each package contains:
- Program binaries
- Configuration files
- Dependencies
- Metadata
Managed through a **Package Manager**, which automates installation and dependency handling.

## What is a Package Manager?
A **Package Manager** is a tool that:
- Fetches software from repositories.
- Handles dependencies automatically.
- Keeps track of installed software.
- Supports updates, upgrades, and removal.
**Example:**
 apt, yum, dnf, zypper, pacman, snap.
**Types of Package Managers**

| Package Manager | Distribution | Package Format | Example Commands |
| --- | --- | --- | --- |
| APT (Advanced Package Tool) | Debian, Ubuntu | .deb | sudo apt install nginx |
| YUM / DNF | RedHat, CentOS, Fedora | .rpm | sudo dnf install nginx |
| Zypper | openSUSE | .rpm | sudo zypper install nginx |
| Pacman | Arch Linux | .pkg.tar.zst | sudo pacman -S nginx |
| Snap | Universal (Ubuntu-led) | .snap | sudo snap install vlc |
| Flatpak | Universal | .flatpak | flatpak install flathub org.gimp.GIMP |


## Finding Packages

### APT (Debian/Ubuntu)
- apt search package-name

### YUM/DNF (RHEL/Fedora)
- dnf search package-name

### Snap
- snap find package-name

## Installing Packages

| Package Manager | Command Example |
| --- | --- |
| APT | sudo apt install git |
| YUM/DNF | sudo dnf install git |
| Zypper | sudo zypper install git |
| Pacman | sudo pacman -S git |
| Snap | sudo snap install vlc |


## Listing Installed Packages

| Package Manager | Command |
| --- | --- |
| APT | apt list --installed |
| YUM/DNF | dnf list installed |
| Zypper | zypper se --installed-only |
| Pacman | pacman -Q |
| Snap | snap list |


## Updating and Upgrading

| Task | Command (APT Example) | Description |
| --- | --- | --- |
| Update Package List | sudo apt update | Fetches latest info from repositories |
| Upgrade Packages | sudo apt upgrade | Installs newer versions |
| Full Upgrade | sudo apt full-upgrade | Updates and handles dependency changes |

**Other Examples:**
- YUM/DNF → sudo dnf update
- Pacman → sudo pacman -Syu
- Snap → sudo snap refresh

## Removing Packages

| Package Manager | Command |
| --- | --- |
| APT | sudo apt remove package-name |
| APT (with config) | sudo apt purge package-name |
| YUM/DNF | sudo dnf remove package-name |
| Zypper | sudo zypper remove package-name |
| Pacman | sudo pacman -R package-name |
| Snap | sudo snap remove package-name |


## Snap Packages (Modern Universal Format)

### What is Snap?
- Snap is a containerized package system developed by Canonical (Ubuntu).
- Works on any Linux distribution.
- Packages run in isolated sandboxes, making them secure and self-contained.

### Common Snap Commands:

| Command | Description |
| --- | --- |
| snap find <pkg> | Search for a snap package |
| sudo snap install <pkg> | Install a snap |
| snap list | List installed snaps |
| sudo snap refresh | Update all snaps |
| sudo snap remove <pkg> | Remove snap package |
| snap info <pkg> | View details about a snap |

**Booting Process: Logs & Boot Loaders**

## What is the Booting Process?
- Booting is the process of starting the computer and loading the Linux operating system into memory.
- It involves multiple stages, from powering on the system to getting the login prompt or desktop.
**Stages of the Linux Boot Process**

<img width="1024" height="1024" alt="image" src="https://github.com/user-attachments/assets/3d45a407-02f9-48ff-b861-4e41b781db6d" />


## Stage 1: BIOS / UEFI

### Function:
- Performs Power-On Self Test (POST), checks hardware (CPU, RAM, drives).
- Locates the bootable disk and loads the boot loader (first sector of disk).

### Key Points:
- BIOS = Legacy firmware
- UEFI = Modern replacement (faster boot, supports larger disks)

## Stage 2: Boot Loader (GRUB)

### What is a Boot Loader?
- A small program that loads the Linux kernel into memory.
- GRUB (GRand Unified Bootloader) is the most commonly used boot loader in Linux.

### Functions of GRUB:
- Displays a boot menu with OS options.
- Loads the selected kernel and its initramfs (initial RAM filesystem).
- Passes control to the kernel.
**Commands:**
grub-install /dev/sda               # Install GRUB to disk
update-grub                                # Regenerate GRUB configuration

## Stage 3: Kernel Initialization

### Function:
- The kernel is loaded into memory by GRUB.
- It:
- Detects hardware.
- Mounts the root filesystem.
- Starts the init (or systemd) process (PID 1).
***View kernel messages:***
dmesg | less


## Stage 4: Init / systemd

### Function:
- The first user-space process started by the kernel.
- Responsible for starting all background services and system targets.

### Commands:
systemctl list-units --type=service
systemctl list-dependencies multi-user.target


## Stage 5: Runlevel / Target & Login
- Defines the system state — whether the system boots into:
- GUI (graphical.target)
- Multi-user (multi-user.target)
- Rescue or Maintenance mode
***View Current Target:***
systemctl get-default

***Change Boot Target:***
sudo systemctl set-default graphical.target


## Boot Logs and Troubleshooting

### View Boot Logs
journalctl -b
- Shows logs from the current boot.
***Previous Boot Logs:***
journalctl -b -1
***View Kernel Logs:***
dmesg

### System Boot Timing
systemd-analyze
systemd-analyze blame
Displays how long each service took to start.
**Networking**
***Network:*** Two or more connected devices like computer or phone that can communicate from each other and share information or resources. 
***Internet:*** Globl network that connects devices to share information. 
The main types of networks are classified by their geographical scope:
By geographical scope
**PAN (Personal Area Network): **Connects devices within an individual's personal space, like a mouse to a computer via Bluetooth.
**LAN (Local Area Network): **Covers a small, localized area, such as a home, office, or school.
**WLAN (Wireless Local Area Network): **A LAN that uses wireless communication instead of cables, such as a home Wi-Fi network.
**CAN (Campus Area Network): **Connects multiple LANs within a limited geographical area like a university campus or corporate park.
**MAN (Metropolitan Area Network): **A network that spans a city or a large metropolitan area, connecting multiple LANs across different locations.
**WAN (Wide Area Network):** A network that covers a large geographical area, such as a country or the entire globe, and is made up of interconnected LANs and MANs. The internet is an example of a WAN. 
***Other types***
**VPN (Virtual Private Network): **A network that creates a secure, encrypted connection over a public network, like the internet, allowing you to access a private network remotely.
**SAN (Storage Area Network): **A dedicated, high-speed network for connecting storage devices to servers, separate from the regular LAN.
**TCP/IP Stack**
The TCP/IP model is a framework that is used to model the communication in a network. It is mainly a collection of network protocols and organization of these protocols in different layers for modeling the network.
Role of TCP/IP:

<img width="520" height="347" alt="image" src="https://github.com/user-attachments/assets/d9c91ef9-931c-4575-a7bf-6546d25c57d1" />

- The TCP/IP stack defines how data travels across networks.
- It has 5 layers each handling specific functions.
Physical layer: Transfers raw bits over hardware
Data Link layer: Handles MAC addressing & framing
Network layer: Logical addressing, routing
Transport layer: Reliable or fast data delivery
Application layer: User applications and services
Role of TCP/IP model:

<img width="1024" height="1024" alt="image" src="https://github.com/user-attachments/assets/a3ba093f-08b4-4f33-87b8-dae070c92248" />

**Common TCP/IP Commands**
**Application Layer Commands:**

| Command | Purpose | Example |
| --- | --- | --- |
| ping | Check connectivity to a host | ping google.com |
| curl | Fetch web data / test APIs | curl |
| wget | Download files from web | wget |
| ssh | Secure remote login | ssh |
| ftp | File transfer via FTP protocol | ftp 192.168.1.20 |
| telnet | Test connectivity to a specific port | telnet 192.168.1.1 80 |
| dig | Query DNS servers | dig google.com |
| nslookup | Resolve domain names | nslookup github.com |
| nc (netcat) | Test open ports / send TCP or UDP data | nc -zv 192.168.1.1 22 |

**Transport Layer Commands:**

| Command | Purpose | Example |
| --- | --- | --- |
| netstat | Show active TCP/UDP connections | netstat -tuln |
| ss | Modern replacement for netstat | ss -tulnp |
| nc | Open TCP/UDP connection for testing | nc -l 8080 |
| lsof -i | List processes using network ports | sudo lsof -i :80 |
| traceroute | Track path packets take | traceroute 8.8.8.8 |

**Network Layer Commands:**

| Command | Purpose | Example |
| --- | --- | --- |
| ip addr show | View IP addresses of interfaces | ip addr show eth0 |
| ip route show | Display routing table | ip route |
| ip route add | Add static route | sudo ip route add 10.0.0.0/24 via 192.168.1.1 |
| ping | Check IP connectivity | ping 8.8.8.8 |
| traceroute | Display route to host | traceroute google.com |
| arp | View IP-to-MAC mapping | arp -n |
| ip neigh | Modern ARP view | ip neigh show |
| hostname -I | Show system IPs | hostname -I |

**Data Link Layer Commands:**

| Command | Purpose | Example |
| --- | --- | --- |
| ip link show | Show all interfaces | ip link show |
| ifconfig | Display interface configuration | ifconfig eth0 |
| ethtool | Display Ethernet interface stats | sudo ethtool eth0 |
| ip -s link | Show interface statistics | ip -s link show eth0 |
| arp -n | Show MAC-to-IP mapping | arp -n |

**Subnetting**

## What is Subnetting?
- Subnetting is the process of dividing a large network into smaller subnetworks (subnets).
- It helps in:
- Efficient IP address utilization
- Better network management
- Improved performance and security
- IP Address: Unique address assigned to a device (e.g., 192.168.1.10)
- Subnet Mask: Defines which portion of IP is network vs host (e.g., 255.255.255.0)
- Network ID: Identifies the subnet (e.g., 192.168.1.0)
- Broadcast Address: Used to send data to all hosts in a subnet (e.g., 192.168.1.255)
- Host Range: Range of usable IP addresses for devices
***IP Address?***
An IP Address (Internet Protocol Address) is a unique numerical label assigned to each device connected to a computer network that uses the Internet Protocol for communication. 
It serves two main purposes:
- Identifying a device on the network.
- Locating the device to enable communication with other devices over a network like the Internet.

<img width="1200" height="659" alt="image" src="https://github.com/user-attachments/assets/c17b719a-5b23-43c7-b707-d5d28d3da26d" />

## Components of an IP Address
- Network Portion: Identifies the network to which the device belongs.
- Host Portion: Identifies the individual device on the network.
- Subnet Mask (for IPv4): Defines which part of the IP is network and which part is host.
***Example:**** IP 192.168.1.10 with subnet mask 255.255.255.0*
***Network ID:**** 192.168.1.0*
***Host ID****: 10*
**Classes of IP Address (IP V4):**

<img width="867" height="800" alt="image" src="https://github.com/user-attachments/assets/f0bfa10b-550c-46a3-ac0e-5cb5ccb20ebe" />
- Class A: For large networks; the first octet ranges from 1-126.
- Class B: For medium-sized networks; the first octet ranges from 128-191.
- Class C: For small networks; the first octet ranges from 192-223.
- Class D: Reserved for multicasting; the first octet ranges from 224-239.
- Class E: Reserved for experimental use; the first octet ranges from 240-254.
**Classless Inter Domain Routing (CIDR)**
**One of the most frequently used andpreferred subnetting techniques.**
CIDR(Classless Inter Domain Routing) is a method of IP address allocation and routing that allows more efficient use of IP addresses. Unlike traditional class based addressing, CIDR allocates IP addresses based on a network prefix rather than a fixed class (A, B, or C).
**Notation:** a.b.c.d/n
- n = number of bits in the network prefix.
- Example: 192.168.1.0/24 first 24 bits are network, remaining 8 bits are host ID.

### Why CIDR?
Classful addressing wastes IP addresses:

| Class | IPs Available | Hosts | Example Wastage |
| --- | --- | --- | --- |
| A | 2²⁴ | 2²⁴ - 2 | Too large for small orgs |
| B | 2¹⁶ | 2¹⁶ - 2 | Wastes 49,150 hosts for 214 needed |
| C | 2⁸ | 2⁸ - 2 | Small networks only |

Formula:
Number of Subnets = 2^borrowed_bits
Hosts per Subnet = 2^remaining_bits - 2

## Example Calculation
**Network:** 192.168.1.0/24
 **Subnet Mask:** 255.255.255.0
 If we divide into 4 subnets → need 2 bits borrowed (since 2² = 4 subnets).

| Subnet | Network Address | Range | Broadcast |
| --- | --- | --- | --- |
| 1 | 192.168.1.0/26 | 192.168.1.1 – 192.168.1.62 | 192.168.1.63 |
| 2 | 192.168.1.64/26 | 192.168.1.65 – 192.168.1.126 | 192.168.1.127 |
| 3 | 192.168.1.128/26 | 192.168.1.129 – 192.168.1.190 | 192.168.1.191 |
| 4 | 192.168.1.192/26 | 192.168.1.193 – 192.168.1.254 | 192.168.1.255 |

Tools for calculating Subnets:    Online Subnet Calculations,
                                                                    Command line tools like ipcalc
**Commands for Subnetting**
***Check IP and Subnet***
- ip addr show
***Check Network and Broadcast***
- ip -br -4 addr
***Display Routing Table***
- ip route show
***Calculate Subnet Info***
- ipcalc 192.168.1.0/26
***Assign a subnet manually***
- sudo ip addr add 192.168.10.5/24 dev eth0
***Add route for subnet:***
- sudo ip route add 192.168.20.0/24 via 192.168.10.1
**IP Address & Routing**
Linux provides wide range of network configuration tools from traditional ones like ifconfig to modern ones like ip, nmcli, and, nmtui.
**Configuring IP Address in Linux**
***Display current configurtion***
- Ip addr show
***Assign a static IP address***
- Ip addr add <your IP address> dev eth0
***Remove an IP address***
- Ip addr del <your IP address> dev eth0
**nmcli for NetworkManager provides a user-friendly approach.**
***Display Connection Details***
- nmcli connection show
***Display NetworkManager general status and operations***
- nmcli general status
***Set a Static IP address***
- Nmcli conmod “Connection Name” ipv4.address “<IP address> ipv4.method manual
Here, it permanently sets a static IP address using Network Manager.
***Activate a connection***
- nmcli con up “Connection Up”
**Configuring Routes**

## What is Routing?
- Routing is the process of directing network traffic from one network to another.
- A route tells Linux where to send packets based on their destination IP address.
- Linux uses a routing table to make these decisions.
**Types of Routing:**

| Type | Description |
| --- | --- |
| Direct / Connected Route | Automatically added when an interface has an IP address. |
| Static Route | Manually configured by the user. |
| Default Route | Used when no other route matches. |
| Dynamic Route | Managed by routing protocols (e.g., RIP, OSPF, BGP). |


## View Current Routing Table

### Commands:
- ip route show
or
- route -n
**Example Output:**
default via 192.168.1.1 dev eth0
192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.10

**Explanation:**
- default via 192.168.1.1 → All traffic goes to 192.168.1.1 if no specific route exists.
- 192.168.1.0/24 dev eth0 → Traffic for the local network goes via eth0.

## Adding a Static Route

### Command Syntax:
- sudo ip route add <network>/<mask> via <gateway> dev <interface>
**Example:**
sudo ip route add 10.0.0.0/24 via 192.168.1.1 dev eth0
Sends packets destined for 10.0.0.0/24 through gateway 192.168.1.1.

## Adding a Default Route (Gateway)

### Command:
- sudo ip route add default via 192.168.1.1 dev eth0
All traffic not matching another route goes through 192.168.1.1.
**Verify:**
- ip route

## Deleting a Route

### Command:
- sudo ip route del <network>/<mask> via <gateway>
**Example:**
sudo ip route del 10.0.0.0/24 via 192.168.1.1

### View Routing Table
- ip route show

### Check Connectivity
- ping 10.0.0.1

### Trace Route Path
- traceroute 8.8.8.8

### View Network Interfaces
- ip addr show
**ARP (Address Resolution Protocol)**
- It is used to map an IP address to a MAC address on a local network (LAN).
- Works at the Data Link Layer (Layer 2) of the OSI model.
- Essential for communication between devices in the same subnet.

## How ARP Works?

### Step-by-Step Process:
- Host A wants to send data to Host B (192.168.1.10).
- It checks its ARP cache for the MAC address of 192.168.1.10.
- If not found → Host A broadcasts an ARP Request:
 “Who has 192.168.1.10?”
- Host B responds with an ARP Reply:
 “192.168.1.10 is at 00:11:22:33:44:55”
- The sender stores this mapping in its ARP table for future use.
**Common Commands:**
***Show ARP cache table***
- arp  –n               #A table of IP address and corresponding MAC addresses your system recently communicated with.
***Modern way to view ARP table***
- ip neigh show
***Clear your ARP cache***
- sudo ip neigh flush all
***Generate new ARP requests***
- ping <IP address>
**DHCP**
**DHCP (Dynamic Host Configuration Protocol)** is a network management protocol used to **automatically assign IP addresses and other network configuration parameters** (like gateway, subnet mask, and DNS) to devices on a network.
 It eliminates the need for manually assigning IP addresses to each device.

## How DHCP Works
When a Linux system connects to a network:
- DHCP Discover:
 The client broadcasts a request (DHCPDISCOVER) asking for an IP address.
- DHCP Offer:
 The DHCP server responds with an offer (DHCPOFFER) containing an available IP.
- DHCP Request:
 The client sends a DHCPREQUEST message to confirm it wants the offered IP.
- DHCP Acknowledgment:
 The server sends a DHCPACK, confirming the IP and other details.
 The client can now use that IP for communication.

<img width="1457" height="1127" alt="image" src="https://github.com/user-attachments/assets/96689890-9a32-4fab-9d0d-71c5c2e170bb" />

**DHCP – DORA principle:**

The DHCP DORA process is the four-step sequence a client uses to dynamically obtain an IP address from a DHCP server: Discover, Offer, Request, and Acknowledge. First, the client sends a "Discover" broadcast to find servers. Servers then respond with an "Offer" of an IP address and configuration details. The client sends a "Request" to accept an offer, which is then confirmed by the server's "Acknowledge" message.  
**1. Discover**
- A new client (or one that needs an IP address) sends a broadcast "DHCP Discover" message to the network.
- This message doesn't have a source IP address since it hasn't been assigned one yet, so it's sent to the broadcast address (255.255.255.255).
- The message includes the client's unique , which identifies it to any DHCP servers on the network.
**2. Offer**
- Any DHCP servers on the network that receive the Discover message respond with a "DHCP Offer" message.
- This offer includes a potential IP address, the , and other network configuration details like the default gateway and DNS server.
**3. Request**
- The client receives one or more offers and chooses one it wants to use.
- It then sends a broadcast "DHCP Request" message to the network, stating which IP address it is requesting from which server.
- This broadcast also informs other servers that it will not be using their offers.
**4. Acknowledge**
- The server that the client selected responds with a final "DHCP Acknowledge" (ACK) message.
- This message confirms that the IP address lease is granted, and the server provides the client with all the necessary information to join the network. The client can now use the IP address and other settings.
**DHCP Configuration Files in Linux**

| File | Purpose |
| --- | --- |
| /etc/dhcp/dhcpd.conf | Main configuration file for the DHCP server |
| /etc/default/isc-dhcp-server | Defines which network interfaces DHCP will listen on |
| /var/lib/dhcp/dhcpd.leases | Stores the current IP lease information |


## DHCP Server Installation & Configuration (Ubuntu/Debian)

### Step 1: Install DHCP Server
sudo apt update
sudo apt install isc-dhcp-server -y


### Step 2: Configure the DHCP Server
Edit the configuration file:
sudo nano /etc/dhcp/dhcpd.conf

### Step 3: Specify Network Interface
Edit: sudo nano /etc/default/isc-dhcp-server
Set the interface: INTERFACESv4="eth0"

### Step 4: Restart the Service
sudo systemctl restart isc-dhcp-server
sudo systemctl enable isc-dhcp-server

### Step 5: Verify Status
sudo systemctl status isc-dhcp-server
Most Linux systems use a **DHCP client daemon** (like dhclient) to obtain an IP address automatically.
***To request a new IP:***
sudo dhclient -v eth0

***To release an IP:***
sudo dhclient -r eth0

***To check current IP details:***
ip addr show eth0 or
ifconfig eth0
**DNS Resolution**
**DNS (Domain Name System)** is the service that translates **domain names** (like www.google.com) into **IP addresses** (like 142.250.190.68) that computers use to identify each other on the network.
 This process is called **DNS Resolution**.

## Why DNS Is Needed?
Humans remember names easily (like example.com),
 but computers communicate using IP addresses.
 So DNS acts as a **phonebook** for the internet, converting names to IPs automatically.

## Useful DNS Commands in Linux

| Command | Description |
| --- | --- |
| nslookup domain.com | Basic DNS query tool |
| dig domain.com | Detailed DNS query |
| host domain.com | Simple lookup showing IP |
| systemd-resolve --status | Show current DNS settings |
| ping domain.com | Tests DNS resolution and connectivity |


<img width="308" height="164" alt="image" src="https://github.com/user-attachments/assets/53cb4189-6894-435e-81c4-5dd21ffc32ec" />

## The Role of DNS Resolver in Address Resolution
DNS operates as client-server application. A host that needs to map a domain name to an IP address (or vice versa) calls a DNS client called a **resolver**. The resolver accesses the nearest DNS server with a mapping request. Here's how the process works:
**Step 1**: The resolver sends a query to the nearest DNS server.
**Step 2**: If the server has the required information, it returns the result. If not, it refers the resolver to other DNS servers.
**Step 3**: The resolver checks if the response is correct and delivers it to the requesting application or user.
**Netfilter:**
In Linux, net filter is a kernel-level framework that allows various networking operations such as;
Packet Filtering
Network Address Translation (NAT)
Packet Mangling (Modifying headers)
Connectivity Tracking
**Packet Filtering**
Packet filtering means controlling network traffic (like incoming, outgoing, or forwarded) based on rules.
It checks each packet’s header (like Source IP, Destination IP, Port, or Protocol) and decides whether to:
- ACCEPT (allow)
DROP (silently discard)
REJECT (send error back)
Packet filtering is essential for networking security.
3 main tools for packet filtering in Linux,
- ip tables,
- nf tables,
- Firewalld.
**ip Tables:**
- A command-line tool to configure the Netfilter firewall in Linux.
- Works by checking packets against rules and performing an action when a rule matches.

### Key Terms
- Table: Collection of chains.
- Chain: Collection of rules.
- Rule: Condition to match packets.
- Target: Action taken (ACCEPT, DROP, QUEUE).
- Policy: Default action if no rule matches.
***Syntax:*** iptables -t TABLE -A CHAIN <conditions> -j TARGET

### Main Tables (5 total, mostly 4 used)

#### filter table → default table for packet filtering
Contains these chains:
- INPUT → incoming packets to the host
- OUTPUT → outgoing packets from the host
- FORWARD → packets passing through the host
- PREROUTING → packets arriving before routing
- POSTROUTING → packets leaving after routing

### Important iptables Commands

#### Add rule
- iptables -t TABLE -A CHAIN <conditions> -j TARGET

Delete rule
- iptables -t TABLE -D CHAIN <rule_number>

#### Check rule
- iptables -t TABLE -C CHAIN <conditions>

### Matching Options
- -p protocol → tcp, udp, icmp
- -s source address
- -d destination address
- -i input interface
- -o output interface
- -j target action

### Flush Rules
sudo iptables --flush


### Save Rules
sudo iptables-save

### Other iptables Tables

#### 2. raw table
- Used to mark packets that should skip connection tracking.
- Chains: PREROUTING, OUTPUT

#### 3. nat table
- Handles Network Address Translation (source and destination IP modification).
- Chains: PREROUTING, POSTROUTING

#### 4. mangle table
- Used for packet modification (TOS, TTL, MARK).
- Chains: PREROUTING, OUTPUT

#### 5. security table
- Used for Mandatory Access Control (MAC).
- Supports user-defined chains.
**nftables:**
- Advanced version of iptables.
- Uses Netfilter infrastructure with new packet classification.
- nftables use a new compact syntax and inspired by tcpdump.
 (tcpdump is a CLI network packet analyzer that intercepts and displays network traffic on a computer)
- Tables and chains are fully configurable and no pre-defined tables or chains.
- A single nftables rule can make multiple actions.
- Supports maps configurations such as maps, verdict maps and intervals to obtain performance-oriented packet classification.
- Supports concatenations. Supports dynamic ruleset updates.
- Supports new protocols without kernel update.
**Common nft Commands**
***1. Check version and existing rules***
nft --version
sudo nft list ruleset

***2. Create table, chain, and block port 8080***
sudo nft add table inet myfilter
sudo nft add chain inet myfilter input { type filter hook input priority 0; }
sudo nft add rule inet myfilter input tcp dport 8080 drop

***3. Delete table***
sudo nft delete table inet myfilter


### Firewalld
- A modern firewall management tool replacing manual iptables/nftables handling.
- Provides a user-friendly interface with dynamic updates (no restart needed).
- Uses zones to group network interfaces based on trust level.

### Default in:
- RHEL 7+, CentOS 7+, Fedora 18+, SUSE 15+, OpenSUSE 15+

### Firewalld Features
- Zone-based filtering:
 Each zone defines allowed ports & services:
- public
- private
- trusted
- Dynamic rules: Apply instantly without reboot.
- Persistent rules: Use --permanent to save across reboots.
- Uses nftables as backend.

### Common firewalld commands

| Task | Command |
| --- | --- |
| Enable & start firewalld | sudo systemctl enable firewalld
sudo systemctl start firewalld |
| Check status | systemctl status firewalld |
| Reload firewall | firewall-cmd --reload |
| List rules | firewall-cmd --list-all |
| Open port permanently | firewall-cmd --permanent --add-port=22/tcp |
| Enable service permanently | firewall-cmd --permanent --add-service=ssh |

**SSH (Secure Shell):**
 SSH provides a secure channel or tunnel over an unsecured network by using client-server architecture.
- Secure Data Exchange:
 SSH ensures integrity and confidentiality of data transferred over an insecure network by encrypting the data.
- Authentication:
 SSH uses various authentication methods such as:
- password
- public key
- Remote Shell Access:
 Allow secure remote login to another computer or system over the network.
- Port (Default):
 SSH uses port = 22

### How to Connect:
- ssh username@IP_address
Example:
ssh 

### Generate SSH Keys
- ssh-keygen
Creates:
- id_rsa → private key (keep safe)
- id_rsa.pub → public key (share with server)

### SSH Key Pair Steps

#### 1. Generate SSH Key
ssh-keygen

This creates:
- Private key → stays in client (~/.ssh/id_rsa)
- Public key → copied to server (~/.ssh/id_rsa.pub)

#### 2. Copy Public Key to Remote Server
ssh-copy-id user@remote_host

This enables **passwordless login**.

#### 3. Connect to Server
ssh user@remote_host

Example:
ssh 
