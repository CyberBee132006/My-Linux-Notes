# 🐧 Linux: Complete Notes — Basic to Advanced
### The Most Comprehensive Linux Reference Guide

---

> **How to Use This Document:**
> This guide covers every major Linux topic from absolute basics to advanced internals. Each section builds on the previous. Commands are shown with real examples. The **Professional Cheat Sheet** is at the end.

---

## Table of Contents

1. [Introduction to Linux](#1-introduction-to-linux)
2. [Linux Distributions](#2-linux-distributions)
3. [Linux Architecture & Kernel](#3-linux-architecture--kernel)
4. [Installation & Boot Process](#4-installation--boot-process)
5. [The Linux Filesystem Hierarchy](#5-the-linux-filesystem-hierarchy)
6. [Terminal & Shell Basics](#6-terminal--shell-basics)
7. [File & Directory Management](#7-file--directory-management)
8. [File Permissions & Ownership](#8-file-permissions--ownership)
9. [Users & Groups Management](#9-users--groups-management)
10. [Text Processing & Editors](#10-text-processing--editors)
11. [Processes & Job Control](#11-processes--job-control)
12. [Package Management](#12-package-management)
13. [Disk Management & Storage](#13-disk-management--storage)
14. [Networking](#14-networking)
15. [Shell Scripting (Bash)](#15-shell-scripting-bash)
16. [Environment Variables & Configuration](#16-environment-variables--configuration)
17. [System Services & Systemd](#17-system-services--systemd)
18. [Logging & Monitoring](#18-logging--monitoring)
19. [Archiving, Compression & Backup](#19-archiving-compression--backup)
20. [SSH & Remote Access](#20-ssh--remote-access)
21. [Security & Firewall](#21-security--firewall)
22. [Advanced File Operations](#22-advanced-file-operations)
23. [Kernel & System Internals](#23-kernel--system-internals)
24. [Virtualization & Containers](#24-virtualization--containers)
25. [Cron Jobs & Task Scheduling](#25-cron-jobs--task-scheduling)
26. [Advanced Networking](#26-advanced-networking)
27. [Performance Tuning & Optimization](#27-performance-tuning--optimization)
28. [Troubleshooting & Debugging](#28-troubleshooting--debugging)
29. [Linux for Developers](#29-linux-for-developers)
30. [Professional Cheat Sheet](#30-professional-cheat-sheet)

---

## 1. Introduction to Linux

### What is Linux?

Linux is a free, open-source, Unix-like **operating system kernel** created by **Linus Torvalds** in **1991** as a hobby project while he was a student at the University of Helsinki, Finland. It was inspired by MINIX (a Unix-like teaching OS) and the GNU Project (which provided many of the surrounding tools, compilers, and utilities).

The **full OS** is technically called **GNU/Linux** because it combines:
- The **Linux kernel** (the core that talks to hardware)
- **GNU tools** (compilers, shell, coreutils, etc.)
- Various **system libraries and daemons**

### Why Linux?

| Feature | Explanation |
|---|---|
| **Free & Open Source** | Anyone can read, modify, and redistribute the source code (GPL license) |
| **Stability** | Linux servers routinely run for years without rebooting |
| **Security** | Strong permission model, active security community, fewer viruses |
| **Performance** | Powers 100% of the world's top 500 supercomputers |
| **Flexibility** | Runs on everything from smartwatches to mainframes |
| **Community** | Massive global developer and user community |
| **Servers** | ~96% of the world's web servers run Linux |

### Key Milestones

- **1969**: Unix created at Bell Labs (AT&T)
- **1983**: GNU Project started by Richard Stallman
- **1991**: Linus Torvalds releases Linux 0.01
- **1994**: Linux kernel 1.0 released
- **1996**: Tux the penguin becomes the Linux mascot
- **2003**: Red Hat Enterprise Linux becomes dominant in enterprise
- **2008**: Android (Linux-based) launches
- **2019**: Microsoft adds Linux kernel to Windows (WSL2)
- **Present**: Linux kernel has ~30 million lines of code with thousands of contributors

---

## 2. Linux Distributions

A **distribution (distro)** is a packaged combination of the Linux kernel, GNU tools, a package manager, desktop environment (optional), and additional software.

### Major Distribution Families

#### Debian Family
- **Debian** — The "universal OS", extremely stable, foundation for many others
- **Ubuntu** — Most popular desktop Linux, beginner-friendly, backed by Canonical
- **Ubuntu LTS** — Long-Term Support versions (5 years support), e.g., 22.04 LTS
- **Linux Mint** — Based on Ubuntu, Windows-like interface, great for beginners
- **Kali Linux** — Security/penetration testing distro
- **Raspberry Pi OS** — Optimized for Raspberry Pi hardware
- **Pop!_OS** — System76's Ubuntu-based distro, popular for developers
- **Elementary OS** — macOS-inspired aesthetics

Package manager: **APT** (`apt`, `dpkg`)
Package format: `.deb`

#### Red Hat Family
- **Red Hat Enterprise Linux (RHEL)** — Enterprise standard, paid support
- **CentOS Stream** — Community RHEL (CentOS 8 end-of-life in 2021)
- **Rocky Linux** — Community RHEL replacement after CentOS EOL
- **AlmaLinux** — Another RHEL clone
- **Fedora** — Red Hat's upstream testing distro, cutting-edge features
- **openSUSE** — German enterprise-quality distro

Package manager: **YUM/DNF** (`yum`, `dnf`, `rpm`)
Package format: `.rpm`

#### Arch Family
- **Arch Linux** — Rolling release, highly customizable, "build it yourself"
- **Manjaro** — Arch-based, more user-friendly with GUI installer
- **EndeavourOS** — Arch-based, terminal-centric but easier setup

Package manager: **Pacman** (`pacman`)
Package format: custom

#### Other Notable Distros
- **Gentoo** — Source-based distro, everything compiled from source
- **Slackware** — Oldest active Linux distro (1993), UNIX-like philosophy
- **Alpine Linux** — Tiny (5MB), security-focused, popular in Docker containers
- **NixOS** — Declarative configuration, reproducible builds
- **Void Linux** — Independent distro, musl libc option, runit init

### Choosing a Distribution

| Use Case | Recommended Distro |
|---|---|
| Beginner / Desktop | Ubuntu LTS, Linux Mint |
| Developer workstation | Ubuntu, Fedora, Pop!_OS |
| Server / Production | Ubuntu Server, RHEL, Rocky Linux |
| Security testing | Kali Linux, Parrot OS |
| Embedded / IoT | Alpine, Raspberry Pi OS |
| Cutting-edge packages | Arch, Fedora |
| Stability matters most | Debian, CentOS |
| Learning Linux deeply | Arch, Gentoo, LFS (Linux From Scratch) |

---

## 3. Linux Architecture & Kernel

### Layered Architecture

```
┌─────────────────────────────────────────────┐
│           User Applications                 │  ← Firefox, bash, vim...
├─────────────────────────────────────────────┤
│        System Libraries (glibc)             │  ← libc, libm, libpthread...
├─────────────────────────────────────────────┤
│         System Call Interface               │  ← open(), read(), fork()...
├─────────────────────────────────────────────┤
│              Linux Kernel                   │
│  ┌──────────┬───────────┬────────────────┐  │
│  │ Process  │  Memory   │   File System  │  │
│  │ Mgmt     │  Mgmt     │   VFS Layer    │  │
│  ├──────────┼───────────┼────────────────┤  │
│  │ Network  │  Device   │   IPC          │  │
│  │ Stack    │  Drivers  │   Subsystem    │  │
│  └──────────┴───────────┴────────────────┘  │
├─────────────────────────────────────────────┤
│              Hardware                       │  ← CPU, RAM, Disk, NIC...
└─────────────────────────────────────────────┘
```

### Kernel Modes

**Kernel Space** — Unrestricted access to hardware, all memory; runs kernel code
**User Space** — Restricted access, processes run here, must use system calls to access hardware

### System Calls

System calls are the interface between user space and kernel space. Common ones:

| System Call | Purpose |
|---|---|
| `open()` | Open a file |
| `read()` | Read data |
| `write()` | Write data |
| `fork()` | Create a new process |
| `exec()` | Execute a program |
| `exit()` | Terminate a process |
| `wait()` | Wait for child process |
| `mmap()` | Map memory |
| `socket()` | Create network socket |
| `ioctl()` | Device-specific operations |

### Kernel Subsystems

**Process Management** — Scheduling, forking, signals, IPC
**Memory Management** — Virtual memory, paging, swapping, OOM killer
**Virtual File System (VFS)** — Abstraction layer over ext4, xfs, btrfs, etc.
**Network Stack** — TCP/IP implementation, socket layer
**Device Drivers** — Modules that interface with hardware
**IPC** — Pipes, signals, message queues, shared memory, semaphores

### Monolithic vs Microkernel

Linux uses a **monolithic kernel** (with loadable modules) — all kernel code runs in kernel space.
This is different from microkernels (like Mach/GNU Hurd) where most services run in user space.

The monolithic design provides **better performance** but less fault isolation.

---

## 4. Installation & Boot Process

### The Linux Boot Process (BIOS/UEFI → Login)

Understanding boot is essential for troubleshooting.

```
Power On
    │
    ▼
BIOS / UEFI
  - POST (Power-On Self Test)
  - Hardware initialization
  - Finds boot device
    │
    ▼
Bootloader (GRUB2 or UEFI stub)
  - Loads kernel image (vmlinuz)
  - Loads initial RAM disk (initrd/initramfs)
    │
    ▼
Linux Kernel Initialization
  - Decompresses itself
  - Initializes CPU, memory, devices
  - Mounts initramfs as temporary root
    │
    ▼
Init System (systemd / SysVinit / OpenRC)
  - PID 1 — the first process
  - Starts all system services
    │
    ▼
Login (getty / display manager)
```

### GRUB2 (Grand Unified Bootloader)

GRUB2 is the most common Linux bootloader.

**GRUB configuration files:**
```bash
/boot/grub/grub.cfg          # Main config (auto-generated, don't edit directly)
/etc/default/grub            # User-editable settings
/etc/grub.d/                 # Scripts that generate grub.cfg
```

**Common /etc/default/grub settings:**
```bash
GRUB_DEFAULT=0               # Default menu entry (0 = first)
GRUB_TIMEOUT=5               # Seconds to wait before booting
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"   # Kernel parameters
GRUB_CMDLINE_LINUX=""        # Extra parameters for all modes
```

**Regenerate GRUB config:**
```bash
sudo update-grub             # Debian/Ubuntu
sudo grub2-mkconfig -o /boot/grub2/grub.cfg  # RHEL/Fedora
```

**Common GRUB rescue commands** (when GRUB shell appears):
```bash
ls                           # List devices
ls (hd0,gpt2)/               # List filesystem on partition
set root=(hd0,gpt2)          # Set root device
linux /boot/vmlinuz root=/dev/sda2  # Set kernel
initrd /boot/initrd.img      # Set initrd
boot                         # Boot
```

### Kernel Parameters

Passed at boot via GRUB:

```bash
root=/dev/sda1               # Root filesystem device
ro                           # Mount root read-only initially
quiet                        # Suppress most boot messages
splash                       # Show splash screen
init=/bin/bash               # Boot directly to bash (recovery)
single                       # Single-user mode
nomodeset                    # Disable kernel mode setting (GPU issues)
mem=4G                       # Limit memory
elevator=deadline            # Set I/O scheduler
```

### initramfs (Initial RAM Filesystem)

`initramfs` is a temporary root filesystem loaded into RAM at boot.
It contains just enough to mount the real root filesystem (LVM, LUKS, NFS, etc.).

```bash
# View contents of initramfs
mkdir /tmp/initramfs
cd /tmp/initramfs
zcat /boot/initrd.img-$(uname -r) | cpio -idmv

# Rebuild initramfs (Debian/Ubuntu)
sudo update-initramfs -u

# Rebuild initramfs (RHEL/Fedora)
sudo dracut -f
```

### Systemd Targets (Runlevels)

Modern Linux uses **systemd targets** instead of old SysVinit runlevels:

| Target | Old Runlevel | Description |
|---|---|---|
| `poweroff.target` | 0 | Halt/Power off |
| `rescue.target` | 1 | Single-user mode |
| `multi-user.target` | 3 | Multi-user, no GUI |
| `graphical.target` | 5 | Multi-user with GUI |
| `reboot.target` | 6 | Reboot |

```bash
# Check current target
systemctl get-default

# Set default target
sudo systemctl set-default multi-user.target

# Switch to target immediately
sudo systemctl isolate rescue.target
```

---

## 5. The Linux Filesystem Hierarchy

Linux follows the **Filesystem Hierarchy Standard (FHS)**. Everything is a file — hardware devices, network sockets, processes — all accessed through the filesystem.

### Root Directory Structure

```
/
├── bin/      → Essential user binaries (ls, cp, bash) — symlink to /usr/bin on modern systems
├── boot/     → Boot loader files, kernel images, initramfs
├── dev/      → Device files (virtual filesystem)
├── etc/      → System-wide configuration files
├── home/     → User home directories (/home/alice, /home/bob)
├── lib/      → Essential shared libraries and kernel modules
├── lib64/    → 64-bit shared libraries
├── media/    → Mount points for removable media (USB, CD)
├── mnt/      → Temporary mount points
├── opt/      → Optional/third-party software packages
├── proc/     → Virtual filesystem exposing kernel/process info
├── root/     → Home directory of root user
├── run/      → Runtime data (PIDs, sockets) — cleared on reboot
├── sbin/     → System administration binaries — symlink to /usr/sbin
├── srv/      → Data for services (web server files, FTP data)
├── sys/      → Virtual filesystem for kernel/hardware info
├── tmp/      → Temporary files (cleared on reboot or periodically)
├── usr/      → User utilities and applications (read-only data)
│   ├── bin/          → User commands
│   ├── include/      → Header files for C programming
│   ├── lib/          → Libraries
│   ├── local/        → Locally installed software (not managed by package manager)
│   ├── sbin/         → System admin commands
│   └── share/        → Architecture-independent data (man pages, docs)
└── var/      → Variable data (logs, spools, databases)
    ├── log/          → Log files
    ├── mail/         → Mail spool
    ├── spool/        → Print/cron spools
    ├── tmp/          → Temp files persisted across reboots
    └── www/          → Web server document root (often)
```

### Important Directories in Detail

#### /proc — Process Information Filesystem

`/proc` is a virtual filesystem (procfs) that exposes kernel data structures as files.

```bash
/proc/cpuinfo          # CPU information
/proc/meminfo          # Memory usage details
/proc/version          # Kernel version
/proc/uptime           # System uptime
/proc/loadavg          # System load averages
/proc/filesystems      # Supported filesystems
/proc/mounts           # Currently mounted filesystems
/proc/net/             # Network statistics
/proc/sys/             # Kernel parameters (tunable via sysctl)
/proc/1/               # PID 1 (init/systemd) information
/proc/1/cmdline        # Command line of PID 1
/proc/1/status         # Status of PID 1
/proc/1/maps           # Memory maps of PID 1
/proc/self/            # Symlink to current process's /proc entry
```

#### /sys — Sysfs

Exposes kernel objects (devices, drivers, buses) as files:

```bash
/sys/block/            # Block devices
/sys/bus/              # System buses (PCI, USB, etc.)
/sys/class/            # Device classes (net, block, etc.)
/sys/devices/          # Device tree
/sys/kernel/           # Kernel objects
/sys/module/           # Loaded kernel modules
```

#### /dev — Device Files

```bash
/dev/sda               # First SATA/SCSI disk
/dev/sda1              # First partition of first disk
/dev/nvme0n1           # First NVMe disk
/dev/tty               # Current terminal
/dev/tty1              # First virtual terminal
/dev/pts/0             # First pseudo-terminal (SSH, gnome-terminal)
/dev/null              # Null device (discards all writes)
/dev/zero              # Produces infinite null bytes
/dev/urandom           # Non-blocking random number generator
/dev/random            # Blocking true random number generator
/dev/full              # Always returns "no space left" on write
/dev/loop0             # First loop device (for mounting images)
/dev/mem               # Physical memory
/dev/stdin             # Standard input
/dev/stdout            # Standard output
/dev/stderr            # Standard error
```

---

## 6. Terminal & Shell Basics

### What is a Shell?

A **shell** is a command-line interpreter that takes commands from the user, passes them to the OS, and returns output. It is both an interactive tool and a scripting language.

### Shell Types

| Shell | Description |
|---|---|
| **bash** | Bourne Again SHell — default on most Linux distros |
| **sh** | Original Bourne shell (often symlink to dash/bash) |
| **zsh** | Z Shell — extended bash, popular with devs (default on macOS) |
| **fish** | Friendly Interactive Shell — auto-suggest, colorful |
| **dash** | Debian Almquist Shell — minimal, fast, POSIX-compliant |
| **ksh** | Korn Shell — traditional Unix shell |
| **tcsh/csh** | C Shell — C-like syntax, used on BSD systems |

```bash
# Check current shell
echo $SHELL
echo $0

# List all available shells
cat /etc/shells

# Change your default shell
chsh -s /bin/zsh

# Run a command in a specific shell
bash -c "echo Hello"
zsh -c "echo Hello"
```

### Terminal Emulators

- **gnome-terminal** — GNOME default
- **konsole** — KDE default
- **xterm** — Classic, minimal
- **alacritty** — GPU-accelerated, fast
- **kitty** — GPU-accelerated, feature-rich
- **tmux/screen** — Terminal multiplexers (multiple panes in one terminal)

### The Shell Prompt

```bash
[username@hostname current_directory]$
user@machine:~$          # Typical bash prompt
root@machine:/etc#       # Root prompt (# instead of $)
```

Prompt components defined in `$PS1` variable:
```bash
\u   = username
\h   = hostname (short)
\H   = hostname (full)
\w   = current working directory (full path)
\W   = current working directory (basename only)
\$   = $ for user, # for root
\t   = time (HH:MM:SS)
\d   = date
\n   = newline
\[...\] = non-printing characters (for colors)
```

Example custom prompt:
```bash
export PS1="\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ "
```

### Essential Navigation Commands

```bash
pwd                      # Print working directory (current location)
cd /path/to/dir          # Change to absolute path
cd relative/path         # Change to relative path
cd ~                     # Go to home directory
cd                       # Go to home directory (same as cd ~)
cd ..                    # Go up one level
cd ../..                 # Go up two levels
cd -                     # Go to previous directory
cd /                     # Go to root directory

ls                       # List files
ls -l                    # Long format (permissions, size, date)
ls -a                    # Show hidden files (starting with .)
ls -la                   # Long format + hidden files
ls -lh                   # Human-readable file sizes (KB, MB, GB)
ls -lt                   # Sort by modification time (newest first)
ls -ltr                  # Sort by modification time (oldest first)
ls -lS                   # Sort by file size
ls -R                    # Recursive listing
ls --color=auto          # Colored output
ls -i                    # Show inode numbers
ls -1                    # One file per line
ls -d */                 # List only directories
```

### Shell Shortcuts & Keyboard Commands

```bash
# Navigation
Ctrl+A          # Move cursor to beginning of line
Ctrl+E          # Move cursor to end of line
Ctrl+B          # Move cursor back one character
Ctrl+F          # Move cursor forward one character
Alt+B           # Move cursor back one word
Alt+F           # Move cursor forward one word

# Editing
Ctrl+K          # Cut (kill) from cursor to end of line
Ctrl+U          # Cut from cursor to beginning of line
Ctrl+Y          # Paste (yank) cut text
Ctrl+W          # Cut previous word
Alt+D           # Cut next word
Ctrl+T          # Transpose characters
Ctrl+_          # Undo last edit

# History
Ctrl+R          # Reverse search command history
Ctrl+G          # Escape history search
Ctrl+P          # Previous command (same as Up arrow)
Ctrl+N          # Next command (same as Down arrow)
!!              # Repeat last command
!n              # Run command number n from history
!string         # Run last command starting with 'string'
!$              # Last argument of previous command
!*              # All arguments of previous command
Alt+.           # Insert last argument of previous command

# Control
Ctrl+C          # Interrupt (kill) current process
Ctrl+Z          # Suspend current process (send to background)
Ctrl+D          # EOF (logout/exit if at prompt)
Ctrl+L          # Clear screen (same as 'clear')
Ctrl+S          # Pause terminal output
Ctrl+Q          # Resume terminal output
```

### Command History

```bash
history                  # Show command history
history 20               # Show last 20 commands
history -c               # Clear history
history -d 42            # Delete history entry 42
!42                      # Run history entry 42
!!                       # Run last command
sudo !!                  # Run last command as root

# History configuration
HISTSIZE=10000           # Number of commands in memory
HISTFILESIZE=20000       # Number of commands in history file
HISTFILE=~/.bash_history # Location of history file
HISTCONTROL=ignoredups   # Don't store duplicate commands
HISTCONTROL=ignorespace  # Don't store commands starting with space
HISTTIMEFORMAT="%F %T "  # Add timestamps to history

# Search history
Ctrl+R, then type        # Reverse incremental search
grep "docker" ~/.bash_history  # Search history file
```

### Shell Globbing (Wildcards)

```bash
*               # Match any number of any characters
?               # Match exactly one character
[abc]           # Match a, b, or c
[a-z]           # Match any lowercase letter
[A-Z]           # Match any uppercase letter
[0-9]           # Match any digit
[!abc]          # Match any character NOT a, b, or c
{a,b,c}         # Brace expansion: a, b, c (not a glob, shell expansion)
{1..10}         # Brace expansion: 1 2 3 ... 10
{a..z}          # Brace expansion: a b c ... z

# Examples
ls *.txt                 # All .txt files
ls file?.log             # file1.log, file2.log, etc.
ls [Rr]eadme*            # README, readme, Readme, etc.
ls {*.jpg,*.png}         # All jpg and png files
mkdir dir{1..5}          # Create dir1, dir2, dir3, dir4, dir5
touch file{a,b,c}.txt    # Create filea.txt, fileb.txt, filec.txt
```

---

## 7. File & Directory Management

### Creating Files and Directories

```bash
# Directories
mkdir mydir                    # Create single directory
mkdir -p a/b/c/d               # Create nested directories (parent dirs as needed)
mkdir -m 755 mydir             # Create with specific permissions
mkdir dir1 dir2 dir3           # Create multiple directories at once

# Files
touch file.txt                 # Create empty file OR update timestamps
touch -a file.txt              # Update only access time
touch -m file.txt              # Update only modification time
touch -t 202301151200 file.txt # Set specific timestamp (YYYYMMDDhhmm)

# Other ways to create files
echo "content" > file.txt      # Create file with content (overwrite)
echo "content" >> file.txt     # Append to file
cat > file.txt                 # Type content, Ctrl+D to save
printf "line1\nline2\n" > file.txt
tee file.txt <<< "content"     # Write content using tee
```

### Copying Files and Directories

```bash
cp source dest                 # Copy file
cp source1 source2 dest_dir/   # Copy multiple files to directory
cp -r source_dir dest_dir      # Copy directory recursively
cp -p file.txt backup/         # Preserve permissions, timestamps, owner
cp -a source dest              # Archive mode: recursive + preserve all attributes
cp -i source dest              # Interactive (prompt before overwrite)
cp -n source dest              # No-clobber (don't overwrite existing)
cp -u source dest              # Update only (copy only if newer)
cp -v source dest              # Verbose output
cp -l source dest              # Hard link instead of copy
cp -s source dest              # Symbolic link instead of copy

# Examples
cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bak
cp -r /var/www/html/ /backup/html_$(date +%Y%m%d)/
cp -rp /home/user1/ /home/user2/  # Copy home dir preserving permissions
```

### Moving and Renaming

```bash
mv source dest                 # Move or rename
mv old.txt new.txt             # Rename file
mv file.txt /another/dir/      # Move to another directory
mv file.txt /another/dir/newname.txt  # Move and rename
mv -i source dest              # Prompt before overwrite
mv -n source dest              # Don't overwrite existing
mv -v source dest              # Verbose
mv -u source dest              # Move only if source is newer
mv *.log /var/log/archived/    # Move all .log files
```

### Deleting Files and Directories

```bash
rm file.txt                    # Delete file
rm file1 file2 file3           # Delete multiple files
rm -r directory/               # Delete directory recursively
rm -rf directory/              # Force recursive delete (no prompts) — DANGEROUS
rm -i file.txt                 # Interactive (confirm each deletion)
rm -v file.txt                 # Verbose
rm -f file.txt                 # Force (no error if file doesn't exist)
rmdir emptydir/                # Delete empty directory only

# Safe deletion practices
rm -ri directory/              # Interactive recursive delete
ls directory/                  # Always verify what you're deleting first
rm -- -filename                # Delete file starting with hyphen (use --)
```

> ⚠️ **WARNING**: `rm -rf /` or `rm -rf /*` will destroy your entire system. Always double-check paths before running `rm -rf`.

### Finding Files

```bash
# find command — searches filesystem
find /path -name "filename"          # Find by exact name
find /path -name "*.txt"             # Find by pattern
find /path -iname "*.TXT"            # Case-insensitive name search
find /path -type f                   # Find files only
find /path -type d                   # Find directories only
find /path -type l                   # Find symbolic links
find /path -size +100M               # Files larger than 100MB
find /path -size -1k                 # Files smaller than 1KB
find /path -size 50c                 # Files exactly 50 bytes
find /path -mtime -7                 # Modified in last 7 days
find /path -mtime +30                # Modified more than 30 days ago
find /path -mmin -60                 # Modified in last 60 minutes
find /path -newer reference_file     # Newer than reference file
find /path -user alice               # Files owned by alice
find /path -group developers         # Files owned by group developers
find /path -perm 755                 # Exact permission match
find /path -perm -644                # Has at least these permissions
find /path -perm /111                # Has any execute bit set
find /path -empty                    # Empty files and directories
find /path -maxdepth 2               # Limit search depth
find /path -mindepth 1               # Minimum depth

# Actions with find
find /path -name "*.log" -delete     # Delete found files
find /path -name "*.txt" -print      # Print paths (default)
find /path -name "*.bak" -exec rm {} \;   # Execute command on each
find /path -name "*.bak" -exec rm {} +    # More efficient: batch
find /path -name "*.conf" -exec grep -l "localhost" {} \;
find /tmp -mtime +7 -delete          # Clean files older than 7 days

# Combining conditions
find /path -name "*.log" -and -size +10M    # Both conditions
find /path -name "*.log" -or -name "*.txt"  # Either condition
find /path -not -name "*.log"               # Negation

# locate command — uses database (faster but not real-time)
locate filename                      # Search database
locate -i filename                   # Case-insensitive
locate -c filename                   # Count matches
sudo updatedb                        # Update locate database

# which, whereis, type
which ls                             # Full path of executable
which -a python                      # All instances in PATH
whereis ls                           # Binary, source, man page locations
type ls                              # Type of command (builtin/alias/file)
type -a ls                           # All definitions of command
```

### Links

Linux has two types of links:

**Hard Link** — Another name (directory entry) for the same inode (same physical data).
- Cannot span filesystems
- Cannot link to directories
- File persists until ALL hard links are deleted
- Same inode number as original

**Symbolic Link (Symlink)** — A file that contains a path to another file.
- Can span filesystems
- Can link to directories
- If original is deleted, symlink becomes broken (dangling)
- Different inode from original

```bash
# Hard links
ln source hardlink               # Create hard link
ls -li file hardlink             # Verify same inode number

# Symbolic links
ln -s target linkname            # Create symbolic link
ln -s /absolute/path linkname    # Absolute symlink (preferred)
ln -s ../relative/path linkname  # Relative symlink
ln -sf target linkname           # Force (overwrite existing link)
readlink linkname                # Show target of symlink
readlink -f linkname             # Show fully resolved path
ls -la linkname                  # Shows: linkname -> target

# Practical examples
ln -s /usr/bin/python3 /usr/local/bin/python  # python → python3
ln -s /var/www/html /home/alice/www           # Quick access to web dir
ln -s /path/to/config.conf ~/.config          # Link config file
```

---

## 8. File Permissions & Ownership

### Understanding Permission Bits

Every file has three sets of permissions for three categories of users:

```
-rwxr-xr--  1  alice  developers  4096  Jan 15  file.txt
│││││││││
││││││││└── Other: read=yes, write=no, execute=no (r--)
│││││││└─── Other: write
││││││└──── Other: read
│││││└───── Group: execute (x)
││││└────── Group: write (-)
│││└─────── Group: read (r)
││└──────── Owner: execute (x)
│└───────── Owner: write (w)
└────────── Owner: read (r)

└─┬─┘
  └── File type: - (file), d (dir), l (link), c (char dev), b (block dev), p (pipe), s (socket)
```

### Permission Values

| Symbol | Name | Octal | Effect on File | Effect on Directory |
|---|---|---|---|---|
| `r` | Read | 4 | Read file contents | List directory contents |
| `w` | Write | 2 | Modify/delete file | Create/delete files in dir |
| `x` | Execute | 1 | Execute as program | Enter directory (`cd`) |
| `-` | None | 0 | No permission | No permission |

### chmod — Change Mode

```bash
# Symbolic mode
chmod u+x file.txt              # Add execute for owner (user)
chmod g+w file.txt              # Add write for group
chmod o-r file.txt              # Remove read for others
chmod a+x script.sh             # Add execute for all (a = ugo)
chmod u+x,g-w,o=r file.txt      # Multiple changes at once
chmod ug=rwx,o= file.txt        # Set exactly: owner/group=rwx, other=none

# Octal mode
chmod 755 file.txt              # rwxr-xr-x (owner: rwx, group: r-x, other: r-x)
chmod 644 file.txt              # rw-r--r-- (owner: rw, group: r, other: r)
chmod 600 file.txt              # rw------- (owner: rw only — private files)
chmod 700 dir/                  # rwx------ (owner access only)
chmod 777 dir/                  # rwxrwxrwx (everyone — avoid in production)
chmod 000 file.txt              # No permissions for anyone
chmod 750 script.sh             # rwxr-x--- (owner: all, group: rx, other: none)

# Recursive
chmod -R 755 directory/         # Apply recursively
chmod -R a-x,a+X directory/     # Remove execute from files, keep on directories

# Common permission patterns
chmod 644 config.txt            # Regular file (readable by all, writable by owner)
chmod 755 script.sh             # Executable script
chmod 600 ~/.ssh/id_rsa         # Private SSH key (required by SSH)
chmod 644 ~/.ssh/id_rsa.pub     # Public SSH key
chmod 700 ~/.ssh/               # SSH directory
chmod 1777 /tmp                 # Sticky bit + rwx for all (like /tmp)
```

### Special Permission Bits

```bash
# SetUID (SUID) — bit 4
# When set on executable: runs as file OWNER regardless of who executes
# Example: /usr/bin/passwd runs as root even when called by normal user
chmod u+s /path/to/file
chmod 4755 file                 # 4 = SUID + 755
ls -l /usr/bin/passwd
# -rwsr-xr-x (note 's' in owner execute position)

# SetGID (SGID) — bit 2
# On executable: runs as file GROUP
# On directory: new files inherit the directory's group
chmod g+s /shared/directory
chmod 2775 /shared/directory    # 2 = SGID + 775
ls -la /shared/
# drwxrwsr-x (note 's' in group execute position)

# Sticky Bit — bit 1
# On directory: users can only delete their OWN files
# Classic use: /tmp directory
chmod +t /tmp
chmod 1777 /tmp                 # 1 = sticky + 777
ls -la /
# drwxrwxrwt tmp (note 't' in other execute position)

# All special bits
chmod 4755 file                 # SUID
chmod 2755 file                 # SGID
chmod 1755 file                 # Sticky
chmod 7777 file                 # All three (SUID+SGID+Sticky+rwx)
```

### chown — Change Ownership

```bash
chown alice file.txt             # Change owner to alice
chown alice:developers file.txt  # Change owner and group
chown :developers file.txt       # Change group only (note colon)
chown -R alice:alice /home/alice/ # Recursive ownership change
chown --reference=reffile file.txt # Copy ownership from reference file
```

### chgrp — Change Group

```bash
chgrp developers file.txt        # Change group
chgrp -R developers /shared/     # Recursive group change
```

### Default Permissions — umask

`umask` defines default permissions for new files/directories.

```bash
umask                            # Show current umask (typically 022 or 002)
umask 022                        # Set umask

# How umask works:
# Base permissions for files:   666 (rw-rw-rw-)
# Base permissions for dirs:    777 (rwxrwxrwx)
# New permissions = base - umask
# umask 022:
#   files:  666 - 022 = 644 (rw-r--r--)
#   dirs:   777 - 022 = 755 (rwxr-xr-x)
# umask 002:
#   files:  666 - 002 = 664 (rw-rw-r--)
#   dirs:   777 - 002 = 775 (rwxrwxr-x)

# Set in ~/.bashrc for persistent change
echo "umask 027" >> ~/.bashrc
```

### Access Control Lists (ACL)

ACLs provide more granular permissions beyond owner/group/other.

```bash
# Install (if needed)
sudo apt install acl

# View ACL
getfacl file.txt

# Set ACL
setfacl -m u:bob:rw file.txt         # Give bob read/write
setfacl -m g:devteam:rx directory/   # Give group rx
setfacl -m o::- file.txt             # Remove other permissions
setfacl -x u:bob file.txt            # Remove ACL entry for bob
setfacl -b file.txt                  # Remove all ACL entries

# Default ACLs (inherited by new files in directory)
setfacl -d -m u:bob:rw directory/    # Default ACL for new files
setfacl -R -m u:bob:rwX directory/   # Recursive (X = execute only if dir)

# Copy ACL from one file to another
getfacl file1 | setfacl --set-file=- file2
```

---

## 9. Users & Groups Management

### User Account Files

```bash
/etc/passwd       # User account info (colon-separated)
/etc/shadow       # Encrypted passwords (root-readable only)
/etc/group        # Group definitions
/etc/gshadow      # Secure group info
/etc/skel/        # Template for new user home directories
/etc/login.defs   # Default values for user creation
/etc/adduser.conf # adduser configuration (Debian)
```

**Format of /etc/passwd:**
```
username:password:UID:GID:GECOS:home_dir:shell
alice:x:1001:1001:Alice Smith:/home/alice:/bin/bash
     ^ x = password in /etc/shadow
```

**Format of /etc/shadow:**
```
username:encrypted_pwd:lastchg:min:max:warn:inactive:expire
alice:$6$salt$hashedpassword:19000:0:99999:7:::
      ^ $6$ = SHA-512 hash
```

**Format of /etc/group:**
```
groupname:password:GID:members
developers:x:1002:alice,bob,charlie
```

### Creating and Managing Users

```bash
# useradd — low-level user creation
sudo useradd username                      # Create user (minimal)
sudo useradd -m username                   # Create with home directory
sudo useradd -m -s /bin/bash username      # With bash shell
sudo useradd -m -s /bin/bash -G sudo,docker username  # With groups
sudo useradd -m -u 1500 -g developers username        # Specify UID and group
sudo useradd -c "Full Name" username       # Add GECOS/comment field
sudo useradd -e 2025-12-31 username        # Account expiry date
sudo useradd -r servicename               # System account (no home, low UID)

# adduser — higher-level, interactive (Debian/Ubuntu)
sudo adduser username                      # Interactive user creation
sudo adduser username sudo                 # Add user to sudo group

# Set password
sudo passwd username                       # Set/change password interactively
echo "username:password" | sudo chpasswd  # Set password non-interactively
sudo passwd -l username                    # Lock account
sudo passwd -u username                    # Unlock account
sudo passwd -e username                    # Force password change at next login
sudo passwd -n 7 username                  # Minimum days between changes
sudo passwd -x 90 username                 # Maximum days until change required
sudo passwd -w 7 username                  # Warn 7 days before expiry

# usermod — modify user
sudo usermod -aG sudo username             # Add to supplementary group (KEEP -a!)
sudo usermod -aG docker,sudo username      # Add to multiple groups
sudo usermod -g newgroup username          # Change primary group
sudo usermod -s /bin/zsh username          # Change shell
sudo usermod -d /newhome username          # Change home directory
sudo usermod -l newname oldname            # Rename user
sudo usermod -L username                   # Lock account
sudo usermod -U username                   # Unlock account
sudo usermod -e 2025-12-31 username        # Set expiry date
sudo usermod -e "" username                # Remove expiry date

# userdel — delete user
sudo userdel username                      # Delete user (keep home)
sudo userdel -r username                   # Delete user AND home directory

# id — show user identity
id                                         # Current user info
id username                                # Specific user info
id -u username                             # Just UID
id -g username                             # Just primary GID
id -G username                             # All GIDs
id -n -u                                   # Username as name

# User information commands
whoami                                     # Current username
who                                        # Who is logged in
w                                          # Who is logged in + what they're doing
last                                       # Login history
lastlog                                    # Last login for all users
finger username                            # User info (if installed)
getent passwd username                     # Get user from passwd database
```

### Creating and Managing Groups

```bash
# groupadd
sudo groupadd groupname                    # Create group
sudo groupadd -g 1500 groupname            # Specify GID
sudo groupadd -r servicegroup              # System group

# groupmod
sudo groupmod -n newname oldname           # Rename group
sudo groupmod -g 1600 groupname            # Change GID

# groupdel
sudo groupdel groupname                    # Delete group (only if not primary for any user)

# gpasswd — manage group membership
sudo gpasswd -a username groupname         # Add user to group
sudo gpasswd -d username groupname         # Remove user from group
sudo gpasswd -A alice groupname            # Make alice group administrator
sudo gpasswd -M alice,bob groupname        # Set group members (replaces existing)

# newgrp — switch active group
newgrp developers                          # Start new shell with developers as effective group

# Group info
groups                                     # Show groups current user belongs to
groups username                            # Groups of specific user
getent group groupname                     # Get group info
cat /etc/group | grep username             # Manual search
```

### Sudo Configuration

```bash
# Edit sudoers file safely (never edit directly)
sudo visudo                                # Opens /etc/sudoers in editor safely

# sudoers syntax:
# user  host=(run_as_user:group)  commands
alice   ALL=(ALL:ALL) ALL                  # Full sudo access
bob     ALL=(ALL) /usr/bin/apt,/sbin/reboot  # Limited commands
%sudo   ALL=(ALL:ALL) ALL                  # Group sudo can do everything
%wheel  ALL=(ALL) ALL                      # wheel group (RHEL style)
alice   ALL=(ALL) NOPASSWD: ALL            # No password required (be careful!)
alice   ALL=(ALL) NOPASSWD: /usr/bin/apt   # No password for specific command

# sudoers.d — drop-in files (preferred)
sudo visudo -f /etc/sudoers.d/alice        # Create/edit per-user file
# Content:
alice ALL=(ALL) ALL

# Check sudo access
sudo -l                                    # List what current user can sudo
sudo -l -U username                        # List what specific user can sudo
sudo -v                                    # Validate/refresh sudo timestamp
sudo -k                                    # Expire sudo timestamp immediately

# Run as different user
sudo command                               # Run as root
sudo -u bob command                        # Run as bob
sudo -u bob -i                             # Login shell as bob
sudo su -                                  # Switch to root with environment
sudo su - alice                            # Switch to alice
su username                                # Switch user (need their password)
su -                                       # Switch to root
```

### Password Aging and Account Policies

```bash
# chage — change user password expiry
sudo chage username                        # Interactive
sudo chage -l username                     # List current settings
sudo chage -d 0 username                   # Force password change at next login
sudo chage -M 90 username                  # Max 90 days before change required
sudo chage -m 7 username                   # Min 7 days between changes
sudo chage -W 14 username                  # Warn 14 days before expiry
sudo chage -I 30 username                  # Account inactive 30 days after password expires
sudo chage -E 2025-12-31 username          # Account expires on date
sudo chage -E -1 username                  # Never expire

# pam_pwquality — password quality
# Configure in /etc/security/pwquality.conf
minlen = 12                                # Minimum length
minclass = 3                               # Minimum character classes
maxrepeat = 2                              # Max consecutive same character
dcredit = -1                               # Require at least 1 digit
ucredit = -1                               # Require at least 1 uppercase
lcredit = -1                               # Require at least 1 lowercase
ocredit = -1                               # Require at least 1 special char
```

---

## 10. Text Processing & Editors

### Viewing File Contents

```bash
cat file.txt                    # Display entire file
cat -n file.txt                 # Display with line numbers
cat -A file.txt                 # Show all chars including non-printing (^ for ctrl, $ for newline)
cat -s file.txt                 # Squeeze multiple blank lines into one
cat file1 file2                 # Concatenate and display
cat file1 file2 > combined.txt  # Concatenate to new file

tac file.txt                    # Display in reverse (last line first)

less file.txt                   # Page through file (recommended for large files)
# less navigation:
# Space / f    = page forward
# b            = page backward
# /pattern     = search forward
# ?pattern     = search backward
# n            = next match
# N            = previous match
# g            = go to start
# G            = go to end
# q            = quit
# h            = help

more file.txt                   # Simpler pager (forward only)

head file.txt                   # First 10 lines
head -n 20 file.txt             # First 20 lines
head -c 100 file.txt            # First 100 bytes

tail file.txt                   # Last 10 lines
tail -n 20 file.txt             # Last 20 lines
tail -f file.txt                # Follow — watch file for new content (great for logs)
tail -F file.txt                # Follow by filename (handles log rotation)
tail -f -n 50 /var/log/syslog   # Last 50 lines, then follow

strings file.bin                # Print printable strings from binary file
xxd file.bin                    # Hexdump + ASCII
od -x file.bin                  # Octal dump (hex format)
od -c file.txt                  # Octal dump (char format — shows \n, \t, etc.)
hexdump -C file.bin             # Hex + ASCII dump
```

### grep — Global Regular Expression Print

```bash
grep "pattern" file.txt         # Search for pattern in file
grep -i "pattern" file.txt      # Case-insensitive search
grep -r "pattern" directory/    # Recursive search
grep -l "pattern" *.txt         # List files containing pattern (filename only)
grep -L "pattern" *.txt         # List files NOT containing pattern
grep -n "pattern" file.txt      # Show line numbers
grep -c "pattern" file.txt      # Count matching lines
grep -v "pattern" file.txt      # Invert — show lines NOT matching
grep -w "word" file.txt         # Match whole words only
grep -x "exact line" file.txt   # Match entire lines only
grep -A 3 "pattern" file.txt    # Show 3 lines AFTER match
grep -B 3 "pattern" file.txt    # Show 3 lines BEFORE match
grep -C 3 "pattern" file.txt    # Show 3 lines before AND after (context)
grep -o "pattern" file.txt      # Print only matching part
grep -q "pattern" file.txt      # Quiet mode (just exit code, 0=found)
grep -m 5 "pattern" file.txt    # Stop after 5 matches
grep -f patterns.txt file.txt   # Read patterns from file
grep --color=auto "pattern" file.txt  # Highlight matches

# Extended grep (regex)
grep -E "pattern1|pattern2" file.txt  # OR
grep -E "^start" file.txt             # Starts with
grep -E "end$" file.txt               # Ends with
grep -E "[0-9]+" file.txt             # One or more digits
grep -E "[a-zA-Z]{3,}" file.txt       # 3+ letters

egrep "pattern"                  # Same as grep -E
fgrep "pattern"                  # Fixed string (no regex, faster)

# Practical examples
grep "error" /var/log/syslog                      # Find errors in log
grep -r "TODO" /home/alice/project/ --include="*.py"  # Find TODOs in Python files
grep -rn "password" /etc/ 2>/dev/null             # Search configs for password
ps aux | grep nginx                               # Find nginx processes
grep -E "^[0-9]{1,3}\.[0-9]{1,3}" access.log     # Find IP addresses
```

### sed — Stream Editor

`sed` is a powerful tool for text transformation.

```bash
# Basic syntax: sed [options] 'command' file

# Substitution: s/find/replace/flags
sed 's/old/new/' file.txt         # Replace first occurrence per line
sed 's/old/new/g' file.txt        # Replace ALL occurrences (global)
sed 's/old/new/2' file.txt        # Replace 2nd occurrence per line
sed 's/old/new/gi' file.txt       # Case-insensitive global replace
sed 's/old/new/gI' file.txt       # Same (GNU sed)
sed -i 's/old/new/g' file.txt     # In-place edit (modifies file)
sed -i.bak 's/old/new/g' file.txt # In-place edit with backup (.bak)

# Deletion
sed '3d' file.txt                 # Delete line 3
sed '3,7d' file.txt               # Delete lines 3 through 7
sed '/pattern/d' file.txt         # Delete lines matching pattern
sed '/^$/d' file.txt              # Delete empty lines
sed '/^#/d' file.txt              # Delete comment lines

# Printing (usually with -n to suppress default output)
sed -n '5p' file.txt              # Print line 5
sed -n '5,10p' file.txt           # Print lines 5-10
sed -n '/pattern/p' file.txt      # Print matching lines

# Insertion and appending
sed '3i\new line' file.txt        # Insert before line 3
sed '3a\new line' file.txt        # Append after line 3
sed '/pattern/i\new line' file.txt  # Insert before matching line
sed '/pattern/a\new line' file.txt  # Append after matching line

# Multiple commands
sed -e 's/foo/bar/g' -e 's/baz/qux/g' file.txt
sed 's/foo/bar/g; s/baz/qux/g' file.txt

# Address ranges
sed '5,10s/old/new/g' file.txt    # Only lines 5-10
sed '1~2s/old/new/g' file.txt     # Every other line starting at 1
sed '$d' file.txt                 # Delete last line

# Practical examples
sed -i 's/localhost/production.server.com/g' config.conf
sed '/^#/d; /^$/d' config.conf    # Remove comments and blank lines
sed 's/\s*$//' file.txt           # Remove trailing whitespace
sed 's/^/  /' file.txt            # Add 2-space indent to each line
```

### awk — Pattern Scanning and Processing Language

`awk` is a complete programming language for text processing.

```bash
# Basic syntax: awk 'pattern { action }' file

# Field separator is whitespace by default
awk '{print $1}' file.txt         # Print first field
awk '{print $NF}' file.txt        # Print last field
awk '{print $1, $3}' file.txt     # Print fields 1 and 3
awk 'NR==5' file.txt              # Print line 5 (NR = row number)
awk 'NR>=5 && NR<=10' file.txt    # Print lines 5-10
awk '/pattern/' file.txt          # Print lines matching pattern
awk '!/pattern/' file.txt         # Print lines NOT matching

# Custom field separator
awk -F: '{print $1}' /etc/passwd         # Use : as separator
awk -F, '{print $2}' data.csv            # CSV processing
awk -F'\t' '{print $1}' data.tsv         # Tab-separated

# Built-in variables
# NR  = current record (line) number
# NF  = number of fields in current line
# FS  = field separator (default: whitespace)
# RS  = record separator (default: newline)
# OFS = output field separator
# ORS = output record separator
# FILENAME = current filename

# Calculations
awk '{sum += $1} END {print sum}' numbers.txt       # Sum first column
awk '{sum += $1; count++} END {print sum/count}' n  # Average
awk 'NR%2==0' file.txt                              # Even lines only
awk '{print NF}' file.txt                           # Print field count per line

# Formatted output
awk '{printf "%-20s %5d\n", $1, $2}' file.txt       # Formatted columns
awk -v OFS=, '{print $1, $2, $3}' file.txt          # Custom output separator

# BEGIN and END blocks
awk 'BEGIN{print "Header"} {print} END{print "Footer"}' file.txt
awk 'BEGIN{FS=":"} {print $1, $5}' /etc/passwd      # Set FS in BEGIN

# Conditionals
awk '$3 > 100 {print $1}' file.txt          # Conditional print
awk '{if ($1=="error") print $0}' log.txt   # If statement
awk '$2 == "ERROR" || $2 == "FATAL"' log   # OR condition

# Practical examples
awk -F: '$3 >= 1000 {print $1, $3}' /etc/passwd     # Non-system users
df -h | awk 'NR>1 {print $1, $5}'                   # Disk usage per partition
ps aux | awk 'NR>1 {print $1, $11}' | sort -u        # Unique process owners
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head -10  # Top IPs
```

### sort, uniq, cut, tr, wc

```bash
# sort
sort file.txt                    # Alphabetical sort
sort -r file.txt                 # Reverse sort
sort -n file.txt                 # Numerical sort
sort -k2 file.txt                # Sort by field 2
sort -k2 -n file.txt             # Sort by field 2 numerically
sort -t: -k3 -n /etc/passwd      # Sort passwd by UID (field 3, colon-separated)
sort -u file.txt                 # Sort and remove duplicates
sort -f file.txt                 # Case-insensitive
sort -h file.txt                 # Human-readable number sort (1K, 2M, 3G)
sort -R file.txt                 # Random shuffle
sort file1 file2 > sorted.txt    # Sort multiple files

# uniq (works on sorted input)
uniq file.txt                    # Remove consecutive duplicates
uniq -c file.txt                 # Count occurrences
uniq -d file.txt                 # Show only duplicates
uniq -u file.txt                 # Show only unique (non-duplicated) lines
uniq -i file.txt                 # Case-insensitive comparison

# Common pattern: sort | uniq -c | sort -rn (frequency count)
sort access.log | uniq -c | sort -rn | head -20

# cut
cut -d: -f1 /etc/passwd          # Cut field 1 with : delimiter
cut -d, -f2,4 data.csv           # Fields 2 and 4 from CSV
cut -c1-10 file.txt              # First 10 characters
cut -c5- file.txt                # Characters 5 onwards
cut -c1-5,10-15 file.txt         # Multiple ranges

# tr — translate characters
tr 'a-z' 'A-Z' < file.txt        # Uppercase
tr 'A-Z' 'a-z' < file.txt        # Lowercase
tr -d '\n' < file.txt            # Remove newlines
tr -s ' ' < file.txt             # Squeeze multiple spaces
tr -d '[:punct:]' < file.txt     # Remove punctuation
tr ' ' '\n' < file.txt           # Words to lines
tr -dc '[:print:]' < file.bin    # Delete non-printable chars
echo "hello" | tr 'a-z' 'n-za-m' # ROT13

# wc — word count
wc file.txt                      # Lines, words, bytes
wc -l file.txt                   # Lines only
wc -w file.txt                   # Words only
wc -c file.txt                   # Bytes
wc -m file.txt                   # Characters (respects multibyte)
ls | wc -l                       # Count files in directory
```

### Pipes and Redirection

```bash
# Redirection
command > file.txt               # Redirect stdout to file (overwrite)
command >> file.txt              # Append stdout to file
command 2> error.log             # Redirect stderr to file
command 2>> error.log            # Append stderr to file
command &> all.log               # Redirect both stdout and stderr
command > out.log 2> err.log     # Redirect to separate files
command > /dev/null 2>&1         # Discard all output
command 2>&1                     # Redirect stderr to stdout
command < input.txt              # Redirect file to stdin
command << EOF                   # Here document
  content
EOF
command <<< "string"             # Here string

# Pipes
cmd1 | cmd2                      # Pipe stdout of cmd1 to stdin of cmd2
cmd1 |& cmd2                     # Pipe stdout AND stderr
cmd1 | cmd2 | cmd3               # Chained pipes
cmd1 | tee file.txt | cmd2       # Tee: write to file AND pass to next command
cmd1 | tee -a file.txt | cmd2    # Tee with append

# Process substitution
diff <(ls dir1/) <(ls dir2/)     # Compare output of two commands
while read line; do ...; done < <(find . -name "*.txt")

# Named pipes (FIFO)
mkfifo /tmp/mypipe               # Create named pipe
producer > /tmp/mypipe &         # Write to pipe
consumer < /tmp/mypipe           # Read from pipe

# Command grouping
(cmd1; cmd2; cmd3) > output.txt  # Group in subshell
{ cmd1; cmd2; cmd3; } > out.txt  # Group in current shell
```

### Vim Text Editor

Vim is the most powerful terminal text editor. It has modes:

```
NORMAL mode → default, for navigation and commands
INSERT mode → for typing text (press i, a, o, etc.)
VISUAL mode → for selecting text (press v, V, Ctrl+V)
COMMAND mode → for : commands (press :)
```

```bash
# Opening vim
vim file.txt                     # Open file
vim +42 file.txt                 # Open at line 42
vim +/pattern file.txt           # Open at first match of pattern
view file.txt                    # Open read-only (same as vim -R)
vimdiff file1 file2              # Diff view

# Mode switching
i          # Insert before cursor
I          # Insert at beginning of line
a          # Append after cursor
A          # Append at end of line
o          # Open new line below and insert
O          # Open new line above and insert
R          # Replace mode
Esc        # Return to Normal mode

# Navigation (Normal mode)
h j k l    # Left down up right
w          # Next word start
b          # Previous word start
e          # End of word
W B E      # Same but WORD (whitespace-separated)
0          # Beginning of line
^          # First non-whitespace of line
$          # End of line
gg         # Beginning of file
G          # End of file
:42        # Go to line 42
42G        # Go to line 42
Ctrl+F     # Page forward
Ctrl+B     # Page backward
Ctrl+D     # Half page down
Ctrl+U     # Half page up
%          # Jump to matching bracket
*          # Search for word under cursor (forward)
#          # Search for word under cursor (backward)

# Editing (Normal mode)
x          # Delete character under cursor
X          # Delete character before cursor
dd         # Delete line
D          # Delete from cursor to end of line
dw         # Delete word
d$         # Delete to end of line
d0         # Delete to beginning of line
yy         # Yank (copy) line
yw         # Yank word
y$         # Yank to end of line
p          # Paste after cursor
P          # Paste before cursor
u          # Undo
Ctrl+R     # Redo
.          # Repeat last command
r          # Replace single character
cw         # Change word (delete + insert mode)
cc         # Change entire line
C          # Change from cursor to end of line
~          # Toggle case of character
>>         # Indent line
<<         # Un-indent line
==         # Auto-indent line

# Search (Normal mode)
/pattern   # Search forward
?pattern   # Search backward
n          # Next match (same direction)
N          # Previous match (reverse direction)
:noh       # Clear search highlighting

# Visual mode
v          # Character visual
V          # Line visual
Ctrl+V     # Block visual
y          # Yank selection
d          # Delete selection
>          # Indent selection
<          # Un-indent
~          # Toggle case

# Command mode (:)
:w                # Save
:q                # Quit (fails if unsaved changes)
:wq               # Save and quit
:q!               # Quit without saving (force)
:wq!              # Force save and quit
:x                # Save and quit (like :wq)
ZZ                # Save and quit (normal mode)
ZQ                # Quit without saving

:s/old/new/       # Substitute on current line
:s/old/new/g      # Substitute all on current line
:%s/old/new/g     # Substitute all in file
:%s/old/new/gc    # With confirmation
:3,10s/old/new/g  # Lines 3-10
:g/pattern/d      # Delete all matching lines
:v/pattern/d      # Delete all non-matching lines

:set number        # Show line numbers
:set nonumber      # Hide line numbers
:set hlsearch      # Highlight search results
:set ignorecase    # Case-insensitive search
:set autoindent    # Auto-indent
:set tabstop=4     # Tab width 4
:set expandtab     # Convert tabs to spaces
:syntax on         # Enable syntax highlighting

:r file.txt        # Read and insert file
:e newfile.txt     # Edit different file
:sp file.txt       # Split horizontal + open file
:vsp file.txt      # Split vertical + open file
Ctrl+W+W           # Switch between splits
Ctrl+W+hjkl        # Navigate splits

:!command          # Execute shell command
:shell             # Open shell
```

### nano Text Editor

```bash
nano file.txt           # Open file

# Key shortcuts (shown at bottom)
Ctrl+O    # Save (WriteOut)
Ctrl+X    # Exit
Ctrl+W    # Search (Where is)
Ctrl+\    # Search and replace
Ctrl+K    # Cut line
Ctrl+U    # Paste (Uncut)
Ctrl+G    # Help
Ctrl+A    # Beginning of line
Ctrl+E    # End of line
Alt+G     # Go to line number
Ctrl+C    # Show cursor position
```

---

## 11. Processes & Job Control

### What is a Process?

A **process** is an instance of a running program. Each process has:
- **PID** (Process ID) — unique identifier
- **PPID** (Parent Process ID)
- **UID/GID** — owner and group
- **State** (Running, Sleeping, Stopped, Zombie)
- **Memory space** (code, data, stack, heap)
- **File descriptors**

### Process States

| State | Symbol | Description |
|---|---|---|
| Running | R | Actively executing on CPU |
| Sleeping (Interruptible) | S | Waiting for event, can be interrupted |
| Sleeping (Uninterruptible) | D | Waiting for I/O, cannot be interrupted |
| Stopped | T | Suspended (e.g., Ctrl+Z) |
| Zombie | Z | Finished but parent hasn't called wait() |
| Dead | X | Should never be seen |

### Viewing Processes

```bash
ps                              # Processes in current terminal
ps aux                          # ALL processes, detailed format
ps -ef                          # All processes, full format
ps -u username                  # Processes of specific user
ps -p 1234                      # Specific PID
ps aux | grep nginx             # Filter by name
ps -eo pid,ppid,user,%cpu,%mem,comm  # Custom output format
ps auxf                         # With ASCII tree (forest)
ps --sort=-%cpu | head -10      # Top 10 CPU consumers
ps --sort=-%mem | head -10      # Top 10 memory consumers

# ps column meanings
# USER   = process owner
# PID    = process ID
# %CPU   = CPU usage
# %MEM   = memory usage %
# VSZ    = virtual memory size (KB)
# RSS    = resident set size (physical RAM used, KB)
# TTY    = terminal
# STAT   = process state
# START  = start time
# TIME   = cumulative CPU time
# COMMAND = command name

pstree                          # Processes in tree format
pstree -p                       # With PIDs
pstree alice                    # Tree for specific user
pstree -a                       # Show command line arguments
```

### top and htop

```bash
top                             # Interactive process viewer
# top keyboard commands:
# q    = quit
# k    = kill process (enter PID)
# r    = renice (change priority)
# u    = filter by user
# M    = sort by memory
# P    = sort by CPU
# 1    = show per-CPU stats
# h    = help
# f    = manage fields
# d    = change refresh delay
# z    = color mode
# Shift+H = show threads

# top load average: 3 numbers (1 min, 5 min, 15 min averages)
# 1.0 on single-core = 100% utilized
# 4.0 on quad-core = 100% utilized

htop                            # Enhanced top (install separately)
# Features: mouse support, color, scrolling, F-keys for actions
# F3/F4 = search/filter
# F5 = tree view
# F6 = sort
# F9 = kill
# F10 = quit
```

### Signals

Signals are software interrupts sent to processes.

```bash
kill -l                         # List all signals

# Common signals
# SIGHUP  (1)  — Hangup: reload config or terminate daemon
# SIGINT  (2)  — Interrupt: Ctrl+C
# SIGQUIT (3)  — Quit: Ctrl+\ (with core dump)
# SIGKILL (9)  — Kill: CANNOT be caught or ignored, immediate termination
# SIGTERM (15) — Terminate: graceful shutdown (default for kill)
# SIGSTOP (19) — Stop: Ctrl+Z, cannot be caught (like SIGKILL)
# SIGCONT (18) — Continue: resume stopped process
# SIGUSR1 (10) — User-defined signal 1
# SIGUSR2 (12) — User-defined signal 2
# SIGCHLD (17) — Child status changed

kill PID                        # Send SIGTERM to process (graceful)
kill -9 PID                     # Send SIGKILL (force kill)
kill -SIGTERM PID               # By signal name
kill -15 PID                    # By signal number
kill -HUP PID                   # Reload (useful for daemons)
kill -0 PID                     # Check if process exists (no signal sent)

killall nginx                   # Kill all processes named nginx
killall -9 nginx                # Force kill by name
pkill nginx                     # Kill by pattern
pkill -u alice                  # Kill all processes of user
pkill -TERM -P 1234             # Kill children of PID 1234

# Signal to current process from keyboard
Ctrl+C       # SIGINT
Ctrl+Z       # SIGSTOP (suspend)
Ctrl+\       # SIGQUIT
```

### Job Control

```bash
# Running in background
command &                       # Run command in background
nohup command &                 # Run immune to hangup (survives terminal close)
nohup command > output.log 2>&1 &  # With output saved

# Job control
jobs                            # List background jobs
jobs -l                         # With PIDs
fg                              # Bring most recent job to foreground
fg %1                           # Bring job 1 to foreground
bg                              # Resume most recent stopped job in background
bg %2                           # Resume job 2 in background
kill %1                         # Kill job 1

# Workflow
sleep 100       # Start a command
Ctrl+Z          # Suspend it
bg              # Resume in background
jobs            # Check it's running
fg %1           # Bring back to foreground

# disown — remove from shell's job table
disown %1                       # Detach job from shell (won't receive SIGHUP)
disown -h %1                    # Mark job to ignore SIGHUP but keep in job table
```

### Process Priority (Nice)

```bash
# Nice values: -20 (highest priority) to +19 (lowest priority)
# Default is 0, normal users can only decrease priority (increase nice value)
# Root can increase priority (decrease nice value)

nice -n 10 command              # Start with nice 10 (lower priority)
nice -n -10 command             # Start with nice -10 (higher priority, root only)

renice 10 -p 1234               # Change priority of running process
renice -5 -p 1234               # Increase priority (root only)
renice 10 -u alice              # Renice all alice's processes

ps -eo pid,nice,comm | head -20 # Show nice values
top -d 1                        # See priority (NI column) in top
```

### Process Limits

```bash
ulimit -a                       # Show all limits
ulimit -n                       # Max open files
ulimit -u                       # Max processes
ulimit -m                       # Max memory
ulimit -s                       # Stack size
ulimit -c                       # Core dump size (0 = disabled)

ulimit -n 65536                 # Set max open files (current session)
ulimit -c unlimited             # Enable core dumps

# Persistent limits in /etc/security/limits.conf
alice     soft    nofile    10240
alice     hard    nofile    65536
*         soft    nproc     4096
@devs     soft    memlock   unlimited
```

### /proc and Process Information

```bash
ls /proc/1234/                  # All info about PID 1234
cat /proc/1234/status           # Status, memory, UID, etc.
cat /proc/1234/cmdline          # Command line (null-separated)
cat /proc/1234/environ          # Environment variables
cat /proc/1234/maps             # Memory mappings
ls -la /proc/1234/fd/           # Open file descriptors
cat /proc/1234/net/tcp          # Network connections
cat /proc/1234/io               # I/O statistics

# Useful commands using /proc
cat /proc/loadavg               # System load
cat /proc/uptime                # Uptime in seconds
cat /proc/meminfo               # Memory info
cat /proc/cpuinfo               # CPU details
cat /proc/version               # Kernel version
cat /proc/partitions            # Disk partitions
```

---

## 12. Package Management

### APT (Advanced Package Tool) — Debian/Ubuntu

```bash
# Update package lists
sudo apt update                          # Refresh package index

# Search
apt search keyword                       # Search packages
apt-cache search keyword                 # Same
apt show packagename                     # Show package details
apt-cache show packagename               # Same
apt list --installed                     # List installed packages
apt list --installed | grep nginx        # Check if installed
dpkg -l                                  # All installed packages
dpkg -l | grep nginx                     # Check specific
dpkg -s packagename                      # Package status

# Install
sudo apt install packagename             # Install package
sudo apt install pkg1 pkg2 pkg3         # Install multiple
sudo apt install packagename=1.2.3      # Specific version
sudo apt install -y packagename          # Yes to all prompts
sudo apt install --no-install-recommends packagename  # No recommended extras
sudo apt install ./local-package.deb     # Install local .deb file

# Upgrade
sudo apt upgrade                         # Upgrade all packages
sudo apt full-upgrade                    # Upgrade + remove obsolete
sudo apt dist-upgrade                    # Distribution upgrade

# Remove
sudo apt remove packagename              # Remove (keep config)
sudo apt purge packagename               # Remove + delete config
sudo apt autoremove                      # Remove unneeded dependencies
sudo apt clean                           # Clear package cache
sudo apt autoclean                       # Remove outdated cache files

# dpkg — low-level package management
sudo dpkg -i package.deb                # Install .deb file
sudo dpkg -r packagename                # Remove package
sudo dpkg -P packagename                # Purge package
dpkg -l                                 # List all installed
dpkg -L packagename                     # List files installed by package
dpkg -S /path/to/file                   # Which package owns this file
dpkg --get-selections                   # Get selection states
dpkg-reconfigure packagename            # Reconfigure package

# apt-get (older but still valid)
sudo apt-get update && sudo apt-get upgrade
sudo apt-get install packagename
sudo apt-get remove packagename
sudo apt-get autoremove && sudo apt-get autoclean

# Adding repositories
sudo add-apt-repository ppa:user/ppa-name    # Add PPA (Ubuntu)
sudo add-apt-repository universe             # Enable universe repo
sudo add-apt-repository multiverse

# Manually add repo
echo "deb http://repo.example.com/ubuntu focal main" | sudo tee /etc/apt/sources.list.d/example.list
curl -fsSL https://example.com/gpg.key | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/example.gpg
sudo apt update

# Pin/hold packages
sudo apt-mark hold packagename           # Hold back from upgrades
sudo apt-mark unhold packagename         # Resume upgrades
apt-mark showhold                        # Show held packages
```

### DNF/YUM — RHEL/Fedora/CentOS

```bash
# DNF (modern, Fedora/RHEL 8+)
sudo dnf update                         # Update all packages
sudo dnf upgrade                        # Same as update
sudo dnf install packagename            # Install
sudo dnf remove packagename             # Remove
sudo dnf search keyword                 # Search
sudo dnf info packagename               # Package info
sudo dnf list installed                 # List installed
sudo dnf list available                 # Available packages
sudo dnf list updates                   # Available updates
sudo dnf autoremove                     # Remove unused deps
sudo dnf clean all                      # Clean cache
sudo dnf history                        # Transaction history
sudo dnf history undo last             # Undo last transaction

# Groups
sudo dnf grouplist                      # List package groups
sudo dnf groupinstall "Development Tools"
sudo dnf groupremove "Development Tools"

# YUM (older, still on some systems)
sudo yum update
sudo yum install packagename
sudo yum remove packagename
sudo yum search keyword
sudo yum info packagename

# RPM — low-level
sudo rpm -ivh package.rpm               # Install with verbose + hash
sudo rpm -Uvh package.rpm               # Upgrade
sudo rpm -evv packagename               # Erase (remove)
rpm -qa                                 # Query all installed
rpm -qi packagename                     # Info about package
rpm -ql packagename                     # Files in package
rpm -qf /path/to/file                   # Which package owns file
rpm -qp package.rpm                     # Query uninstalled package file

# Repositories
sudo dnf config-manager --add-repo URL
sudo dnf config-manager --set-enabled repo-id
sudo dnf config-manager --set-disabled repo-id
ls /etc/yum.repos.d/                    # Repo files
```

### Pacman — Arch Linux

```bash
sudo pacman -Syu                        # Sync + upgrade all
sudo pacman -S packagename              # Install
sudo pacman -R packagename              # Remove
sudo pacman -Rs packagename             # Remove + dependencies
sudo pacman -Rns packagename            # Remove + deps + config
sudo pacman -Ss keyword                 # Search
sudo pacman -Si packagename             # Package info (remote)
sudo pacman -Qi packagename             # Package info (installed)
sudo pacman -Ql packagename             # Files in installed package
sudo pacman -Qo /path/to/file           # Which package owns file
sudo pacman -Q                          # List installed packages
sudo pacman -Qe                         # Explicitly installed
sudo pacman -Qdt                        # Orphaned packages
sudo pacman -Sc                         # Clean cache

# AUR (Arch User Repository) with yay
yay -Syu                                # Update + AUR packages
yay -S packagename                      # Install from AUR
yay -Ss keyword                         # Search AUR
```

### Snap & Flatpak

```bash
# Snap (Ubuntu, cross-distro)
snap find keyword                       # Search
snap install packagename                # Install
snap install packagename --classic      # Classic confinement
snap remove packagename                 # Remove
snap list                               # List installed
snap refresh                            # Update all snaps
snap info packagename                   # Package info
snap run packagename                    # Run snap app

# Flatpak (cross-distro, sandboxed)
flatpak search keyword                  # Search
flatpak install flathub app.id          # Install from Flathub
flatpak remove app.id                   # Remove
flatpak list                            # List installed
flatpak update                          # Update all
flatpak run app.id                      # Run
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

---

## 13. Disk Management & Storage

### Disk Partitioning

```bash
# List block devices
lsblk                           # Tree view of block devices
lsblk -f                        # With filesystem info
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINT
fdisk -l                        # Partition tables (all disks)
fdisk -l /dev/sda               # Specific disk
parted -l                       # GNU parted list
blkid                           # Block device IDs and UUIDs

# fdisk — MBR/DOS partition tool
sudo fdisk /dev/sda
# p = print partition table
# n = new partition
# d = delete partition
# t = change type
# w = write changes
# q = quit without saving
# l = list partition types
# m = help

# gdisk — GPT partition tool (recommended for modern systems)
sudo gdisk /dev/sda
# Commands similar to fdisk but for GPT

# parted — supports both MBR and GPT
sudo parted /dev/sda print      # Print partition table
sudo parted /dev/sda mklabel gpt   # Create GPT label
sudo parted /dev/sda mklabel msdos # Create MBR label
sudo parted /dev/sda mkpart primary ext4 1MiB 50GiB
sudo parted /dev/sda resizepart 1 100GiB
sudo parted /dev/sda rm 2       # Remove partition 2

# cfdisk — ncurses-based, easier to use
sudo cfdisk /dev/sda
```

### Filesystems

```bash
# Create filesystems (mkfs)
sudo mkfs.ext4 /dev/sda1                    # ext4 (most common on Linux)
sudo mkfs.ext4 -L "MyDisk" /dev/sda1        # With label
sudo mkfs.ext4 -b 4096 /dev/sda1            # Block size 4096
sudo mkfs.xfs /dev/sda1                     # XFS (good for large files)
sudo mkfs.btrfs /dev/sda1                   # Btrfs (snapshots, checksums)
sudo mkfs.vfat /dev/sda1                    # FAT32
sudo mkfs.ntfs /dev/sda1                    # NTFS
sudo mkfs.exfat /dev/sda1                   # exFAT

# Filesystem check and repair
sudo fsck /dev/sda1                         # Check filesystem
sudo fsck -f /dev/sda1                      # Force check
sudo fsck -y /dev/sda1                      # Auto-yes (fix all errors)
sudo e2fsck -f /dev/sda1                    # ext2/3/4 check
sudo xfs_repair /dev/sda1                   # XFS repair
# IMPORTANT: always unmount before fsck!

# Filesystem info
sudo tune2fs -l /dev/sda1                   # ext4 superblock info
sudo xfs_info /dev/sda1                     # XFS info
sudo dumpe2fs /dev/sda1                     # Detailed ext4 info

# Resize filesystems
sudo resize2fs /dev/sda1                    # Resize ext4 (after partition resize)
sudo resize2fs /dev/sda1 50G               # Resize to 50G
# XFS can only grow, not shrink
sudo xfs_growfs /mountpoint                 # Grow XFS

# Labels and UUIDs
sudo e2label /dev/sda1 "MyData"            # Set ext4 label
sudo tune2fs -L "NewLabel" /dev/sda1       # Another way
sudo blkid /dev/sda1                       # Show UUID and label
sudo tune2fs -U random /dev/sda1           # Generate new UUID
```

### Mounting and Unmounting

```bash
# Mount
sudo mount /dev/sda1 /mnt/data              # Mount device to mountpoint
sudo mount -t ext4 /dev/sda1 /mnt/data     # Specify filesystem type
sudo mount -o ro /dev/sda1 /mnt/data       # Mount read-only
sudo mount -o rw,noexec /dev/sda1 /mnt     # Multiple options
sudo mount -o remount,rw /mnt/data         # Remount with different options
sudo mount UUID="abc-123-..." /mnt/data    # Mount by UUID
sudo mount LABEL="MyData" /mnt/data        # Mount by label
sudo mount -a                               # Mount all in /etc/fstab

# Unmount
sudo umount /mnt/data                       # Unmount by mountpoint
sudo umount /dev/sda1                       # Unmount by device
sudo umount -l /mnt/data                   # Lazy unmount (when busy)
sudo umount -f /mnt/data                   # Force unmount (NFS)

# Check what's using a mounted filesystem
lsof /mnt/data                             # Open files on mount
fuser -m /mnt/data                         # Processes using mount
fuser -km /mnt/data                        # Kill processes using mount

# Bind mounts
sudo mount --bind /source /dest            # Bind mount directory
sudo mount --rbind /source /dest           # Recursive bind mount
sudo mount --make-shared /mnt             # Make mount shared

# Mount loop devices (ISO, disk images)
sudo mount -o loop disk.iso /mnt/iso       # Mount ISO
sudo mount -t iso9660 -o loop image.iso /mnt
sudo losetup -f disk.img                   # Associate loop device
sudo losetup -a                            # List loop devices

# /etc/fstab — persistent mounts
# Format: device  mountpoint  fstype  options  dump  fsck_order
UUID=abc-123  /data     ext4    defaults         0  2
UUID=def-456  /home     ext4    defaults         0  2
/dev/sda1     /mnt      ext4    defaults         0  2
tmpfs         /tmp      tmpfs   defaults,size=1G 0  0
//server/share /mnt/smb cifs   credentials=/etc/samba/cred 0 0

# fstab options
# defaults = rw,suid,dev,exec,auto,nouser,async
# ro = read-only
# rw = read-write
# noexec = don't execute binaries
# nosuid = ignore SUID/SGID bits
# nodev = don't interpret device files
# auto = mount with -a
# noauto = don't mount with -a
# user = allow regular users to mount
# owner = allow device owner to mount
# nofail = don't report error if device not present
```

### Disk Usage

```bash
df                              # Disk free space (all filesystems)
df -h                           # Human-readable (KB, MB, GB)
df -T                           # Include filesystem type
df -i                           # Inode usage instead of block usage
df /home                        # Specific mountpoint
df -x tmpfs                     # Exclude tmpfs

du                              # Disk usage of current directory
du -h                           # Human-readable
du -s directory/                # Summary (total only)
du -sh *                        # Summary of each item in current dir
du -sh /home/*                  # Space used by each user
du -h --max-depth=2             # Limit depth
du -a                           # All files, not just directories
du -k                           # In kilobytes
du --exclude=*.log directory/   # Exclude pattern
du -sh * | sort -rh | head -10  # Top 10 space consumers

# ncdu — ncurses disk usage (install separately)
sudo apt install ncdu
ncdu /home                      # Interactive disk usage browser
```

### LVM — Logical Volume Manager

LVM provides flexible disk management with volume groups and logical volumes.

```bash
# LVM concepts:
# PV (Physical Volume) = actual disk/partition
# VG (Volume Group) = pool of PVs
# LV (Logical Volume) = like a partition but flexible

# Physical Volumes
sudo pvcreate /dev/sdb /dev/sdc    # Initialize PVs
pvdisplay                           # Display PV info
pvs                                 # Summary
pvscan                              # Scan for PVs

# Volume Groups
sudo vgcreate myvg /dev/sdb /dev/sdc  # Create VG
vgdisplay                              # Display VG info
vgs                                    # Summary
sudo vgextend myvg /dev/sdd            # Add PV to VG
sudo vgreduce myvg /dev/sdb            # Remove PV from VG

# Logical Volumes
sudo lvcreate -L 10G -n mylv myvg      # Create 10G LV
sudo lvcreate -l 100%FREE -n data myvg # Use all free space
sudo lvcreate -L 20G -n lvhome myvg    # Home partition LV
lvdisplay                               # Display LV info
lvs                                     # Summary

# Use LV like any partition
sudo mkfs.ext4 /dev/myvg/mylv
sudo mount /dev/myvg/mylv /mnt/data

# Resize LVs
sudo lvextend -L +5G /dev/myvg/mylv        # Add 5G to LV
sudo lvextend -l +100%FREE /dev/myvg/mylv  # Use all free space
sudo resize2fs /dev/myvg/mylv              # Resize filesystem to fill LV

sudo lvreduce -L -5G /dev/myvg/mylv        # Shrink LV (filesystem first!)
sudo e2fsck -f /dev/myvg/mylv              # Check first
sudo resize2fs /dev/myvg/mylv 15G          # Shrink filesystem first
sudo lvreduce -L 15G /dev/myvg/mylv        # Then shrink LV

# Snapshots
sudo lvcreate -L 5G -s -n snap /dev/myvg/mylv   # Create snapshot
sudo mount /dev/myvg/snap /mnt/snapshot          # Mount snapshot
sudo lvremove /dev/myvg/snap                     # Remove snapshot
```

### RAID

```bash
# Software RAID with mdadm
sudo apt install mdadm

# Create RAID arrays
sudo mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdb /dev/sdc  # RAID 1 (mirror)
sudo mdadm --create /dev/md0 --level=5 --raid-devices=3 /dev/sdb /dev/sdc /dev/sdd  # RAID 5
sudo mdadm --create /dev/md0 --level=0 --raid-devices=2 /dev/sdb /dev/sdc  # RAID 0 (stripe)

# View RAID status
cat /proc/mdstat                    # RAID status
sudo mdadm --detail /dev/md0        # Detailed info

# Manage RAID
sudo mdadm --add /dev/md0 /dev/sde  # Add spare drive
sudo mdadm --fail /dev/md0 /dev/sdb # Mark drive as failed
sudo mdadm --remove /dev/md0 /dev/sdb  # Remove failed drive

# Save RAID config
sudo mdadm --detail --scan >> /etc/mdadm/mdadm.conf
```

### Swap

```bash
# Create swap file
sudo fallocate -l 4G /swapfile     # Create 4GB file
sudo chmod 600 /swapfile           # Secure it
sudo mkswap /swapfile              # Format as swap
sudo swapon /swapfile              # Enable
sudo swapon --show                 # Show active swap

# Create swap partition
sudo mkswap /dev/sda2              # Format partition as swap
sudo swapon /dev/sda2              # Enable

sudo swapoff /swapfile             # Disable swap
sudo swapoff -a                    # Disable all swap

# Add to /etc/fstab for persistence
/swapfile none swap sw 0 0
UUID=xxx none swap sw 0 0

# Swappiness (0-100, default 60)
cat /proc/sys/vm/swappiness        # Current value
sudo sysctl vm.swappiness=10       # Set to 10 (less swapping)
echo "vm.swappiness=10" | sudo tee -a /etc/sysctl.conf  # Persistent
```

---

## 14. Networking

### Network Configuration

```bash
# ip command (modern — replaces ifconfig, route, etc.)
ip addr                          # Show IP addresses
ip addr show                     # Same
ip addr show eth0                # Specific interface
ip addr add 192.168.1.100/24 dev eth0    # Add IP
ip addr del 192.168.1.100/24 dev eth0   # Delete IP

ip link                          # Show link status
ip link show eth0                # Specific interface
ip link set eth0 up              # Bring interface up
ip link set eth0 down            # Bring interface down
ip link set eth0 mtu 9000        # Set MTU
ip link set eth0 promisc on      # Promiscuous mode

ip route                         # Show routing table
ip route show                    # Same
ip route add default via 192.168.1.1     # Default gateway
ip route add 10.0.0.0/8 via 192.168.1.1 # Static route
ip route del 10.0.0.0/8                  # Delete route
ip route get 8.8.8.8             # Which route to reach IP

ip neigh                         # ARP table (neighbors)
ip neigh show                    # ARP cache
ip neigh flush dev eth0          # Flush ARP cache

# ifconfig (legacy, still common)
ifconfig                         # Show all interfaces
ifconfig eth0                    # Specific interface
ifconfig eth0 192.168.1.100 netmask 255.255.255.0  # Set IP
ifconfig eth0 up                 # Up
ifconfig eth0 down               # Down
ifconfig eth0:0 192.168.1.200    # Virtual interface

# nmcli (NetworkManager command line)
nmcli device                     # Show devices
nmcli device status              # Device status
nmcli connection show            # Show connections
nmcli connection show --active   # Active connections
nmcli connection up "Connection Name"   # Activate connection
nmcli connection down "Connection Name" # Deactivate
nmcli device connect eth0        # Connect device
nmcli device disconnect eth0     # Disconnect

# Add static IP via nmcli
nmcli connection add type ethernet con-name "static" ifname eth0 \
    ip4 192.168.1.100/24 gw4 192.168.1.1

# Modify connection
nmcli connection modify "static" ipv4.dns "8.8.8.8 8.8.4.4"
nmcli connection modify "static" ipv4.method manual
nmcli connection reload
```

### Network Configuration Files

```bash
# Debian/Ubuntu (Netplan — modern)
/etc/netplan/*.yaml
# Example netplan config:
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      dhcp4: false
      addresses: [192.168.1.100/24]
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]

sudo netplan apply               # Apply netplan config
sudo netplan try                 # Test config (auto-reverts)
sudo netplan generate            # Generate backend configs

# RHEL/CentOS (ifcfg files)
/etc/sysconfig/network-scripts/ifcfg-eth0
# Content:
TYPE=Ethernet
BOOTPROTO=none
IPADDR=192.168.1.100
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
DNS1=8.8.8.8
ONBOOT=yes
NAME=eth0

# DNS configuration
/etc/resolv.conf                 # DNS resolver config
nameserver 8.8.8.8
nameserver 8.8.4.4
search example.com
domain example.com

# /etc/hosts — static hostname resolution
127.0.0.1   localhost
127.0.1.1   myhostname
192.168.1.50  server1 server1.example.com
```

### Network Diagnostics

```bash
# Connectivity tests
ping google.com                  # ICMP ping
ping -c 4 8.8.8.8               # 4 packets
ping -i 0.2 google.com          # 0.2 second interval
ping -s 1472 google.com         # Packet size (test MTU)
ping6 ipv6.google.com           # IPv6 ping

# Traceroute
traceroute google.com            # Trace path to host (UDP)
traceroute -T google.com         # TCP traceroute
traceroute -I google.com         # ICMP traceroute
tracepath google.com             # Simpler, no root needed
mtr google.com                   # My traceroute (real-time)
mtr --report google.com          # Report mode

# DNS lookup
nslookup google.com              # DNS query (older)
dig google.com                   # DNS query (detailed)
dig google.com A                 # Query A record
dig google.com MX                # Mail exchange records
dig google.com NS                # Name server records
dig google.com AAAA              # IPv6 records
dig google.com ANY               # All records
dig @8.8.8.8 google.com          # Use specific DNS server
dig +short google.com            # Short output
dig -x 8.8.8.8                   # Reverse DNS lookup
host google.com                  # Simple DNS lookup
host -t MX google.com            # MX records
whois google.com                 # WHOIS lookup

# Port scanning
nmap google.com                  # Scan common ports
nmap -p 22,80,443 host           # Specific ports
nmap -p 1-1000 host              # Port range
nmap -sV host                    # Service version detection
nmap -O host                     # OS detection
nmap -A host                     # Aggressive scan (OS, version, scripts)
nmap -sn 192.168.1.0/24         # Ping sweep (no port scan)
nmap --open host                 # Only open ports
sudo nmap -sS host               # SYN scan (stealth, root needed)

# Connection info
netstat -tuln                    # Listening ports (TCP/UDP)
netstat -tulnp                   # With PID/program
netstat -an                      # All connections
netstat -r                       # Routing table
ss -tuln                         # Modern replacement for netstat
ss -tulnp                        # With process info
ss -s                            # Summary statistics
ss -tp                           # TCP connections with processes
lsof -i                          # All network connections
lsof -i :80                      # What's using port 80
lsof -i tcp:443                  # TCP port 443

# Bandwidth tools
iftop                            # Interface traffic (like top for network)
nethogs                          # Per-process bandwidth
iperf3 -s                        # Start iperf server
iperf3 -c server_ip              # Test bandwidth to server
bmon                             # Bandwidth monitor
vnstat                           # Long-term stats

# curl and wget
curl https://example.com                    # Fetch URL
curl -o output.html https://example.com     # Save to file
curl -O https://example.com/file.tar.gz     # Save with original name
curl -L https://example.com                 # Follow redirects
curl -I https://example.com                 # Headers only
curl -v https://example.com                 # Verbose
curl -u user:pass https://api.example.com   # Basic auth
curl -H "Authorization: Bearer TOKEN" URL   # Custom header
curl -X POST -d '{"key":"val"}' -H "Content-Type: application/json" URL
curl --proxy http://proxy:8080 https://example.com
curl -k https://self-signed.example.com     # Skip SSL verification

wget https://example.com/file.tar.gz        # Download file
wget -O newname.tar.gz URL                  # Custom output name
wget -c URL                                 # Continue interrupted download
wget -r -l2 https://example.com/           # Recursive download (depth 2)
wget --mirror https://example.com/          # Mirror site
wget -q URL                                 # Quiet mode
wget --limit-rate=1M URL                    # Limit download speed
```

---

## 15. Shell Scripting (Bash)

### Script Structure and Basics

```bash
#!/bin/bash
# Shebang line — tells kernel which interpreter to use
# Common shebangs:
#!/bin/bash       # Bash
#!/bin/sh         # POSIX sh
#!/usr/bin/env bash  # Portable bash (searches PATH)
#!/usr/bin/python3   # Python 3
#!/usr/bin/perl      # Perl

# Make script executable
chmod +x script.sh
./script.sh       # Run

# Run without making executable
bash script.sh
sh script.sh
```

### Variables

```bash
# Variable assignment (NO spaces around =)
name="Alice"
age=30
count=0
PI=3.14159

# Using variables (prefix with $)
echo $name
echo ${name}          # Braces are clearer and safer
echo "${name} is ${age} years old"

# Command substitution
today=$(date +%Y-%m-%d)
files=$(ls *.txt)
user=$(whoami)

# Arithmetic
result=$((5 + 3))
result=$((10 * 2 - 5))
result=$(echo "scale=2; 10/3" | bc)   # Floating point with bc

# String operations
str="Hello World"
echo ${#str}            # Length: 11
echo ${str:0:5}         # Substring: Hello (offset 0, length 5)
echo ${str:6}           # From position 6: World
echo ${str/World/Linux} # Replace first: Hello Linux
echo ${str//l/L}        # Replace all: HeLLo WorLd
echo ${str^^}           # Uppercase: HELLO WORLD
echo ${str,,}           # Lowercase: hello world
echo ${str^}            # Capitalize first letter

# Default values
echo ${var:-"default"}      # Use default if var is unset or empty
echo ${var:="default"}      # Assign default if var is unset or empty
echo ${var:+"exists"}       # Use "exists" if var IS set
echo ${var:?"error msg"}    # Error if var is unset

# Array variables
fruits=("apple" "banana" "cherry")
echo ${fruits[0]}           # First element: apple
echo ${fruits[1]}           # Second element: banana
echo ${fruits[@]}           # All elements
echo ${#fruits[@]}          # Array length: 3
fruits+=("date")            # Append
fruits[1]="blueberry"       # Modify
unset fruits[2]             # Delete element

# Associative arrays (dictionaries)
declare -A ages
ages["alice"]=30
ages["bob"]=25
echo ${ages["alice"]}
echo ${!ages[@]}            # All keys
echo ${ages[@]}             # All values

# Environment variables
export MYVAR="value"        # Export to child processes
unset MYVAR                 # Remove variable
declare -r CONST="value"    # Read-only variable
declare -i number=42        # Integer variable
declare -l lower="HELLO"    # Auto-lowercase (stored as hello)
declare -u upper="hello"    # Auto-uppercase (stored as HELLO)
```

### Control Flow

```bash
# if / elif / else
if [ condition ]; then
    commands
elif [ other_condition ]; then
    other_commands
else
    fallback_commands
fi

# Test conditions
# File tests
[ -f file ]        # File exists and is regular file
[ -d dir ]         # Directory exists
[ -e path ]        # Path exists (file or directory)
[ -L link ]        # Is a symbolic link
[ -r file ]        # Readable
[ -w file ]        # Writable
[ -x file ]        # Executable
[ -s file ]        # Non-empty file
[ -z file ]        # Zero size

# String tests
[ -z "$str" ]      # String is empty
[ -n "$str" ]      # String is non-empty
[ "$a" = "$b" ]    # Strings are equal
[ "$a" != "$b" ]   # Strings are not equal
[ "$a" < "$b" ]    # Lexicographically less (in [[ ]])
[[ "$str" =~ ^[0-9]+$ ]]  # Regex match (bash only, needs [[]])

# Integer comparisons
[ "$a" -eq "$b" ]  # Equal
[ "$a" -ne "$b" ]  # Not equal
[ "$a" -lt "$b" ]  # Less than
[ "$a" -le "$b" ]  # Less or equal
[ "$a" -gt "$b" ]  # Greater than
[ "$a" -ge "$b" ]  # Greater or equal

# Logical operators
[ cond1 ] && [ cond2 ]    # AND (in single brackets)
[ cond1 ] || [ cond2 ]    # OR
[[ cond1 && cond2 ]]      # AND (in double brackets)
[[ cond1 || cond2 ]]      # OR
! [ condition ]           # NOT

# [[ ]] vs [ ]
# [[ ]] is a bash-specific keyword (more powerful)
# [ ] is a POSIX command (more portable)
# [[ ]] supports &&, ||, =~, no word splitting

# Practical examples
if [ -f "/etc/passwd" ]; then
    echo "File exists"
fi

if [ "$USER" = "root" ]; then
    echo "Running as root"
elif [ "$EUID" -eq 0 ]; then
    echo "Effective root"
else
    echo "Regular user: $USER"
fi

# case statement
case "$variable" in
    pattern1)
        commands1
        ;;
    pattern2|pattern3)
        commands2
        ;;
    *.txt)
        echo "Text file"
        ;;
    *)
        default_commands
        ;;
esac

# Practical case example
case "$1" in
    start)  systemctl start nginx ;;
    stop)   systemctl stop nginx ;;
    restart) systemctl restart nginx ;;
    status) systemctl status nginx ;;
    *)      echo "Usage: $0 {start|stop|restart|status}" ;;
esac
```

### Loops

```bash
# for loop
for i in 1 2 3 4 5; do
    echo "Number: $i"
done

for i in {1..10}; do
    echo "$i"
done

for i in {0..100..5}; do    # 0 5 10 ... 100
    echo "$i"
done

for file in *.txt; do
    echo "Processing: $file"
    cat "$file"
done

for user in alice bob charlie; do
    useradd -m "$user"
done

# C-style for loop
for ((i=0; i<10; i++)); do
    echo "$i"
done

# while loop
count=0
while [ $count -lt 5 ]; do
    echo "Count: $count"
    ((count++))
done

# Read file line by line
while IFS= read -r line; do
    echo "Line: $line"
done < file.txt

# Process command output
while read -r pid; do
    kill -9 "$pid"
done < <(pgrep nginx)

# until loop (opposite of while)
count=10
until [ $count -le 0 ]; do
    echo "Countdown: $count"
    ((count--))
done

# Loop control
break       # Exit loop
continue    # Skip to next iteration
break 2     # Break out of 2 nested loops

for i in {1..10}; do
    if [ $i -eq 5 ]; then
        continue    # Skip 5
    fi
    if [ $i -eq 8 ]; then
        break       # Stop at 8
    fi
    echo "$i"
done
```

### Functions

```bash
# Function definition
function greet() {
    echo "Hello, $1!"
}

# Alternative syntax (POSIX-compatible)
greet() {
    echo "Hello, $1!"
}

# Calling functions
greet "World"
greet "Alice"

# Return values
# Functions return exit codes (0=success, 1-255=error)
is_even() {
    if (( $1 % 2 == 0 )); then
        return 0   # Success/true
    else
        return 1   # Failure/false
    fi
}

if is_even 4; then
    echo "4 is even"
fi

# Return strings via echo
get_date() {
    echo "$(date +%Y-%m-%d)"
}

today=$(get_date)
echo "Today is: $today"

# Function parameters
$0      # Script name
$1, $2  # Parameters 1, 2
$@      # All parameters (as separate words)
$*      # All parameters (as single string)
$#      # Number of parameters
$?      # Exit status of last command
$$      # Current script PID
$!      # PID of last background job

# Local variables (inside functions)
my_function() {
    local local_var="I'm local"
    global_var="I'm global"
    echo "$local_var"
}
my_function
echo "$global_var"    # Works
echo "$local_var"     # Empty (local to function)
```

### Error Handling

```bash
# Exit codes
command && echo "Success" || echo "Failed"
command; if [ $? -ne 0 ]; then echo "Failed"; fi

# set options
set -e              # Exit immediately on error (errexit)
set -u              # Treat unset variables as errors (nounset)
set -o pipefail     # Pipe fails if any command fails
set -x              # Print each command before executing (xtrace — debug)
set -E              # ERR traps inherited by functions

# Best practice shebang + set
#!/bin/bash
set -euo pipefail

# trap — execute code on signals/exit
trap "echo 'Cleaning up...'; rm -f /tmp/tempfile" EXIT
trap "echo 'Interrupted'; exit 1" INT TERM
trap 'echo "Error on line $LINENO"' ERR

# Error checking patterns
command || { echo "Command failed"; exit 1; }

die() {
    echo "ERROR: $*" >&2
    exit 1
}

# Usage: command || die "Failed to do X"
mkdir /tmp/workdir || die "Cannot create work directory"
```

### Practical Script Examples

```bash
#!/bin/bash
# Backup script
set -euo pipefail

BACKUP_DIR="/backup"
SOURCE_DIR="/home"
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_FILE="${BACKUP_DIR}/home_backup_${DATE}.tar.gz"

log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $*" | tee -a /var/log/backup.log
}

log "Starting backup of $SOURCE_DIR"

if [ ! -d "$BACKUP_DIR" ]; then
    mkdir -p "$BACKUP_DIR" || { log "Cannot create backup dir"; exit 1; }
fi

tar -czf "$BACKUP_FILE" "$SOURCE_DIR" 2>/dev/null
log "Backup created: $BACKUP_FILE ($(du -sh "$BACKUP_FILE" | cut -f1))"

# Remove backups older than 30 days
find "$BACKUP_DIR" -name "home_backup_*.tar.gz" -mtime +30 -delete
log "Old backups cleaned up"

log "Backup complete"
```

```bash
#!/bin/bash
# System health check script
set -euo pipefail

check_disk() {
    local threshold=90
    while IFS= read -r line; do
        local usage partition
        usage=$(echo "$line" | awk '{print $5}' | tr -d '%')
        partition=$(echo "$line" | awk '{print $6}')
        if [ "$usage" -gt "$threshold" ]; then
            echo "WARNING: $partition is ${usage}% full"
        fi
    done < <(df -h | tail -n +2)
}

check_memory() {
    local used total percent
    used=$(free | awk '/^Mem:/{print $3}')
    total=$(free | awk '/^Mem:/{print $2}')
    percent=$((used * 100 / total))
    echo "Memory usage: ${percent}%"
    [ "$percent" -gt 90 ] && echo "WARNING: High memory usage!"
}

check_services() {
    local services=("nginx" "sshd" "cron")
    for service in "${services[@]}"; do
        if systemctl is-active --quiet "$service"; then
            echo "OK: $service is running"
        else
            echo "FAIL: $service is not running"
        fi
    done
}

echo "=== System Health Check $(date) ==="
echo "--- Disk ---"
check_disk
echo "--- Memory ---"
check_memory
echo "--- Services ---"
check_services
```

---

## 16. Environment Variables & Configuration

### Understanding Environment Variables

Environment variables are key-value pairs that affect process behavior. They are inherited by child processes.

```bash
# View variables
env                              # All environment variables
printenv                         # Same as env
printenv HOME                    # Specific variable
echo $HOME                       # Print variable
set                              # All shell variables including local ones

# Set variables
MYVAR="value"                    # Shell variable (not exported)
export MYVAR="value"             # Environment variable (exported to children)
export MYVAR                     # Export existing variable
unset MYVAR                      # Remove variable

# Important system variables
HOME         # User's home directory (/home/alice)
USER         # Current username
SHELL        # Current shell (/bin/bash)
PATH         # Command search path (colon-separated)
PWD          # Current directory
OLDPWD       # Previous directory
LANG         # System language/locale
LC_ALL       # Override all locale settings
TERM         # Terminal type (xterm-256color, etc.)
EDITOR       # Default text editor
VISUAL       # Visual editor
PAGER        # Default pager (less, more)
LOGNAME      # Login name
HOSTNAME     # System hostname
HISTSIZE     # History size in memory
HISTFILE     # History file location
TMPDIR       # Temporary directory (/tmp)
UID          # Current user ID
EUID         # Effective user ID
GROUPS       # User's groups
IFS          # Internal Field Separator (default: space, tab, newline)
PS1          # Primary prompt
PS2          # Secondary prompt (continuation)
CDPATH       # Search path for cd command
MANPATH      # Path for man pages
LD_LIBRARY_PATH  # Library search path

# Modify PATH
export PATH="$PATH:/new/directory"           # Append
export PATH="/new/directory:$PATH"           # Prepend
export PATH="/opt/myapp/bin:$HOME/bin:$PATH" # Multiple additions
```

### Shell Configuration Files

These files are sourced (executed) to configure the shell:

```bash
# Bash startup order:
# Login shell: /etc/profile → ~/.bash_profile → ~/.bash_login → ~/.profile
# Interactive non-login: /etc/bash.bashrc → ~/.bashrc
# Non-interactive: $BASH_ENV file

~/.bashrc                    # Per-user bash configuration (interactive shells)
~/.bash_profile              # Login shell config (typically sources .bashrc)
~/.profile                   # Shell-agnostic login config (sh, bash, zsh)
~/.bash_login                # Sourced if .bash_profile doesn't exist
~/.bash_logout               # Executed on logout
~/.bash_aliases              # Alias definitions (sourced from .bashrc)
~/.bash_history              # Command history

/etc/profile                 # System-wide login profile
/etc/profile.d/*.sh          # Drop-in scripts sourced by /etc/profile
/etc/bash.bashrc             # System-wide bashrc (Debian)
/etc/bashrc                  # System-wide bashrc (RHEL)
/etc/environment             # System-wide environment variables (PAM)
/etc/skel/                   # Template files for new users

# Apply changes without logging out
source ~/.bashrc             # Source (execute) a file in current shell
. ~/.bashrc                  # Same (. is POSIX alias for source)
exec bash                    # Replace current shell with new bash
```

### Aliases

```bash
# Define aliases
alias ll='ls -la'
alias la='ls -A'
alias l='ls -CF'
alias grep='grep --color=auto'
alias ..='cd ..'
alias ...='cd ../..'
alias cls='clear'
alias h='history'
alias ports='ss -tulnp'
alias myip='curl -s https://checkip.amazonaws.com'
alias update='sudo apt update && sudo apt upgrade -y'
alias backup='tar -czf backup_$(date +%Y%m%d).tar.gz'
alias rm='rm -i'           # Safety alias
alias cp='cp -i'           # Safety alias
alias mv='mv -i'           # Safety alias

# View all aliases
alias

# Remove alias
unalias ll

# Make persistent — add to ~/.bashrc
echo "alias ll='ls -la'" >> ~/.bashrc

# Bypass alias
\ls          # Run ls without alias
command ls   # Same
'ls'         # Same
```

### Locale and Language Settings

```bash
locale                       # Show current locale settings
locale -a                    # All available locales
localectl status             # System locale status
localectl list-locales        # Available locales
sudo localectl set-locale LANG=en_US.UTF-8   # Set locale

# Generate locale (if missing)
sudo locale-gen en_US.UTF-8
sudo update-locale LANG=en_US.UTF-8

# Manual locale settings
export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8
export LC_TIME=en_GB.UTF-8    # Override just time format

# Timezone
timedatectl                   # Show current time/timezone
timedatectl list-timezones    # Available timezones
sudo timedatectl set-timezone Asia/Kolkata
sudo timedatectl set-timezone UTC

date                          # Current date/time
date +"%Y-%m-%d %H:%M:%S"    # Formatted
date -d "yesterday"           # Relative dates
date -d "+3 days"
date -d "2025-01-01"
date -s "2025-01-01 12:00:00" # Set date (root)
hwclock --show                # Hardware clock
hwclock --hctosys             # Sync hardware → system
hwclock --systohc             # Sync system → hardware
```

---

## 17. System Services & Systemd

### Systemd Overview

**Systemd** is the init system and service manager used by most modern Linux distributions. It replaces SysVinit and Upstart. PID 1 is always systemd.

Key concepts:
- **Units** — configuration files describing resources
- **Services** — daemon processes
- **Targets** — groups of units (similar to runlevels)
- **Sockets** — socket-based activation
- **Timers** — cron-like scheduling

### systemctl — Service Management

```bash
# Service control
sudo systemctl start nginx         # Start service
sudo systemctl stop nginx          # Stop service
sudo systemctl restart nginx       # Stop + Start
sudo systemctl reload nginx        # Reload config without stopping
sudo systemctl reload-or-restart nginx  # Reload if supported, else restart

# Enable/disable (boot persistence)
sudo systemctl enable nginx        # Enable at boot
sudo systemctl disable nginx       # Disable at boot
sudo systemctl enable --now nginx  # Enable + start immediately
sudo systemctl disable --now nginx # Disable + stop immediately
sudo systemctl reenable nginx      # Disable then enable (fix symlinks)

# Status
systemctl status nginx             # Detailed status + last log lines
systemctl is-active nginx          # Is it running? (active/inactive)
systemctl is-enabled nginx         # Is it enabled? (enabled/disabled)
systemctl is-failed nginx          # Did it fail?
systemctl status nginx --lines=50  # More log lines

# List units
systemctl list-units               # All active units
systemctl list-units --type=service  # Just services
systemctl list-units --state=failed  # Failed units
systemctl list-unit-files           # All unit files + enabled state
systemctl list-unit-files --type=service
systemctl list-timers              # Active timers

# Targets
systemctl get-default              # Default target
sudo systemctl set-default graphical.target
sudo systemctl isolate multi-user.target  # Switch now
sudo systemctl rescue              # Enter rescue mode
sudo systemctl poweroff            # Power off
sudo systemctl reboot              # Reboot
sudo systemctl halt                # Halt (no power off)
sudo systemctl hibernate           # Hibernate

# Masking — completely prevent starting
sudo systemctl mask nginx          # Mask (cannot start at all)
sudo systemctl unmask nginx        # Unmask

# Daemon reload (after modifying unit files)
sudo systemctl daemon-reload       # Reload unit files

# Reset failed state
sudo systemctl reset-failed nginx
sudo systemctl reset-failed        # Reset all failed
```

### Creating Systemd Service Units

```bash
# Unit file location
/etc/systemd/system/myapp.service    # Custom/override (preferred)
/lib/systemd/system/                 # Package-provided
/usr/lib/systemd/system/             # Package-provided
~/.config/systemd/user/              # User services

# Basic service unit
# /etc/systemd/system/myapp.service
[Unit]
Description=My Application Service
Documentation=https://example.com/docs
After=network.target network-online.target
Requires=network-online.target
Wants=postgresql.service
BindsTo=postgresql.service

[Service]
Type=simple              # simple, forking, oneshot, notify, dbus, idle
ExecStart=/usr/bin/myapp --config /etc/myapp/config.conf
ExecStop=/usr/bin/myapp stop
ExecReload=/bin/kill -HUP $MAINPID
Restart=always           # always, on-failure, on-abnormal, on-abort, on-watchdog, no
RestartSec=5             # Wait 5 seconds before restart
User=myappuser
Group=myappgroup
WorkingDirectory=/var/lib/myapp
Environment="NODE_ENV=production" "PORT=3000"
EnvironmentFile=/etc/myapp/environment
StandardOutput=journal
StandardError=journal
SyslogIdentifier=myapp
PIDFile=/run/myapp/myapp.pid
LimitNOFILE=65536        # Increase file descriptor limit

# Security hardening
PrivateTmp=true          # Private /tmp
ProtectSystem=strict     # /usr, /boot, /etc read-only
ProtectHome=true         # Deny access to /home
NoNewPrivileges=true     # Cannot gain new privileges
CapabilityBoundingSet=CAP_NET_BIND_SERVICE  # Only allow specific capabilities
ReadWritePaths=/var/lib/myapp /var/log/myapp

[Install]
WantedBy=multi-user.target

# Service Types:
# simple    = Main process is ExecStart (default)
# forking   = Process forks into background (traditional daemon)
# oneshot   = Short-lived, systemd waits for it to exit
# notify    = Process sends sd_notify() when ready
# dbus      = Waits for D-Bus name
# idle      = Wait for all jobs to finish first

# After creating/modifying:
sudo systemctl daemon-reload
sudo systemctl enable myapp.service
sudo systemctl start myapp.service
```

### Override Unit Files (drop-ins)

```bash
# Override specific settings without copying entire unit
sudo systemctl edit nginx                # Creates override file
# Opens editor with /etc/systemd/system/nginx.service.d/override.conf
# Add your overrides:
[Service]
Restart=always
RestartSec=10

# Or full override (replaces entire unit file)
sudo systemctl edit --full nginx

# Drop-in file location
/etc/systemd/system/nginx.service.d/override.conf
```

### journalctl — Log Viewing

```bash
journalctl                         # All logs (most recent at bottom)
journalctl -r                      # Reverse (newest first)
journalctl -f                      # Follow (like tail -f)
journalctl -n 100                  # Last 100 lines
journalctl -u nginx                # Specific unit (service)
journalctl -u nginx -f             # Follow specific service
journalctl -u nginx --since today  # Today's logs
journalctl --since "2025-01-01" --until "2025-01-31"
journalctl --since "1 hour ago"
journalctl --since "10 minutes ago"
journalctl -p err                  # Priority: emerg,alert,crit,err,warning,notice,info,debug
journalctl -p err -u nginx        # Errors from nginx
journalctl -k                      # Kernel messages (dmesg equivalent)
journalctl -b                      # This boot
journalctl -b -1                   # Previous boot
journalctl --list-boots            # All boots
journalctl --disk-usage            # Disk space used by journals
journalctl --vacuum-size=500M      # Keep only 500MB of logs
journalctl --vacuum-time=30d       # Keep only last 30 days
journalctl -o json                 # JSON output
journalctl -o json-pretty          # Pretty JSON
journalctl --no-pager              # Don't use pager

# Search within journal
journalctl -u nginx | grep error
journalctl -u nginx --grep="upstream"  # Built-in grep
journalctl -u nginx --output=cat   # Just message text, no metadata
```

---

## 18. Logging & Monitoring

### Syslog / rsyslog

```bash
# Traditional log files in /var/log/
/var/log/syslog            # General system messages (Debian)
/var/log/messages          # General system messages (RHEL)
/var/log/auth.log          # Authentication (Debian)
/var/log/secure            # Authentication (RHEL)
/var/log/kern.log          # Kernel messages
/var/log/dmesg             # Kernel ring buffer (boot messages)
/var/log/boot.log          # Boot messages
/var/log/apt/              # APT package manager logs
/var/log/dpkg.log          # dpkg package operations
/var/log/nginx/access.log  # Nginx access log
/var/log/nginx/error.log   # Nginx error log
/var/log/apache2/          # Apache logs
/var/log/mysql/            # MySQL logs
/var/log/cron              # Cron job logs
/var/log/mail.log          # Mail logs
/var/log/wtmp              # Login/logout history (binary — use last)
/var/log/btmp              # Failed logins (binary — use lastb)
/var/log/utmp              # Currently logged in (binary — use w, who)

# View logs
tail -f /var/log/syslog              # Follow syslog
tail -f /var/log/auth.log            # Follow auth log
grep "Failed" /var/log/auth.log      # Find failures
zcat /var/log/syslog.1.gz            # View compressed old log
zgrep "error" /var/log/syslog*.gz    # Search in compressed logs

# rsyslog configuration
/etc/rsyslog.conf                    # Main config
/etc/rsyslog.d/*.conf                # Drop-in configs
sudo systemctl restart rsyslog

# logger — send messages to syslog
logger "Test message"
logger -t myapp "Application started"
logger -p user.error "Critical error occurred"
logger -p local0.notice "Custom facility message"
```

### dmesg — Kernel Ring Buffer

```bash
dmesg                        # All kernel messages
dmesg -H                     # Human-readable with timestamps
dmesg -T                     # Human-readable timestamps
dmesg | tail -50             # Last 50 messages
dmesg | grep -i error        # Errors
dmesg | grep -i usb          # USB events
dmesg | grep -i "disk\|sda"  # Disk events
dmesg -w                     # Watch/follow (kernel version 3.5+)
dmesg --level=err,crit       # Filter by level
sudo dmesg -c                # Print and clear ring buffer
```

### System Monitoring

```bash
# top / htop — already covered

# vmstat — virtual memory statistics
vmstat                       # Summary
vmstat 2                     # Update every 2 seconds
vmstat 2 5                   # 5 reports, 2 seconds apart
vmstat -m                    # Slab allocation info
vmstat -s                    # Summary statistics
# Columns: r=run queue, b=blocked, swpd=swap, free=free, 
#          buff=buffers, cache=cache, si=swap in, so=swap out,
#          bi=blocks in, bo=blocks out, in=interrupts, cs=context switches,
#          us=user, sy=system, id=idle, wa=wait I/O, st=stolen

# iostat — I/O statistics
iostat                       # Basic I/O stats
iostat -x                    # Extended stats
iostat -x 2                  # Every 2 seconds
iostat -p sda                # Specific disk
iostat -h                    # Human-readable

# sar — system activity reporter
sar                          # Today's stats
sar -u                       # CPU utilization
sar -r                       # Memory usage
sar -b                       # I/O stats
sar -n DEV                   # Network interface stats
sar -q                       # Load average, run queue
sar -u 2 5                   # CPU: 5 reports, 2 sec apart
sar -f /var/log/sa/sa01      # Specific day file

# free — memory usage
free                         # Memory stats
free -h                      # Human-readable
free -s 2                    # Update every 2 seconds
free -m                      # In megabytes
free -g                      # In gigabytes

# uptime
uptime                       # Current uptime and load averages
cat /proc/uptime             # Uptime in seconds

# watch — run command repeatedly
watch -n 2 df -h             # df every 2 seconds
watch -n 1 'ps aux | head -20'
watch -d -n 1 netstat -tuln  # Highlight changes
watch --color df -h          # Preserve colors

# lsof — list open files
lsof                         # All open files (very verbose)
lsof -p PID                  # Files opened by process
lsof -u alice                # Files opened by user
lsof /var/log/syslog         # Processes using this file
lsof -i                      # Network connections
lsof -i :80                  # Port 80
lsof -i tcp                  # All TCP connections
lsof +D /var/log             # Open files in directory
```

### System Information

```bash
# Hardware info
uname -a                     # Kernel + system info
uname -r                     # Kernel version
uname -m                     # Architecture (x86_64, aarch64)
hostname                     # System hostname
hostname -I                  # All IP addresses
hostnamectl                  # System hostname + info
hostnamectl set-hostname newhostname

cat /proc/cpuinfo            # CPU details
lscpu                        # CPU architecture info
lscpu -e                     # Extended table
nproc                        # Number of processors
lsmem                        # Memory info
lshw                         # Detailed hardware list
lshw -short                  # Short summary
lshw -c network              # Network hardware
lspci                        # PCI devices
lspci -v                     # Verbose
lsusb                        # USB devices
lsusb -v                     # Verbose
dmidecode                    # DMI/SMBIOS info (root)
dmidecode -t memory          # Memory info
dmidecode -t processor       # CPU info
dmidecode -t bios            # BIOS info

# OS info
cat /etc/os-release          # OS name and version
cat /etc/issue               # Login banner (usually shows OS)
lsb_release -a               # LSB info
cat /proc/version            # Kernel version string
```

---

## 19. Archiving, Compression & Backup

### tar — Tape Archive

```bash
# tar options:
# c = create, x = extract, t = list, u = update, r = append
# v = verbose, f = file, z = gzip, j = bzip2, J = xz, a = auto-detect

# Create archives
tar -cvf archive.tar directory/          # Create uncompressed
tar -czvf archive.tar.gz directory/     # Create with gzip compression
tar -cjvf archive.tar.bz2 directory/   # Create with bzip2
tar -cJvf archive.tar.xz directory/    # Create with xz
tar -cavf archive.tar.zst directory/   # Create with zstd (auto-detect)
tar -czvf archive.tar.gz file1 file2 dir/  # Multiple sources

# Extract archives
tar -xvf archive.tar                    # Extract to current dir
tar -xzvf archive.tar.gz               # Extract gzip
tar -xjvf archive.tar.bz2              # Extract bzip2
tar -xJvf archive.tar.xz               # Extract xz
tar -xvf archive.tar -C /target/dir/  # Extract to specific directory
tar -xvf archive.tar file.txt          # Extract specific file
tar -xvf archive.tar --strip-components=1  # Remove top directory

# List/inspect
tar -tvf archive.tar                    # List contents
tar -tvzf archive.tar.gz               # List gzip archive

# Update/append
tar -rvf archive.tar newfile.txt        # Append file to archive
tar -uvf archive.tar directory/         # Update changed files

# Exclude patterns
tar -czvf archive.tar.gz dir/ --exclude="*.log"
tar -czvf backup.tar.gz /home/ --exclude="/home/*/.cache"
tar -czvf backup.tar.gz dir/ --exclude-from=exclude.list

# Split large archives
tar -czvf - directory/ | split -b 1G - archive.tar.gz.
cat archive.tar.gz.* | tar -xzvf -    # Reassemble and extract

# Incremental backup
tar -czvf full_backup.tar.gz --listed-incremental=snapshot.snar /data/
tar -czvf inc_backup.tar.gz --listed-incremental=snapshot.snar /data/
```

### Compression Tools

```bash
# gzip
gzip file.txt                   # Compress → file.txt.gz (original removed)
gzip -k file.txt                # Keep original
gzip -d file.txt.gz             # Decompress
gunzip file.txt.gz              # Same as gzip -d
gzip -v file.txt                # Verbose (show compression ratio)
gzip -9 file.txt                # Maximum compression
gzip -1 file.txt                # Fastest compression
gzip -l file.txt.gz             # List compressed info
zcat file.txt.gz                # View without decompressing
zgrep "pattern" file.txt.gz     # Grep in compressed file
zdiff file1.gz file2.gz         # Diff compressed files

# bzip2 (slower but better compression than gzip)
bzip2 file.txt                  # Compress → file.txt.bz2
bzip2 -d file.txt.bz2           # Decompress
bunzip2 file.txt.bz2            # Same
bzip2 -k file.txt               # Keep original
bzcat file.txt.bz2              # View without decompressing
bzgrep "pattern" file.bz2       # Grep in bzip2 file

# xz (best compression ratio, slower)
xz file.txt                     # Compress → file.txt.xz
xz -d file.txt.xz               # Decompress
unxz file.txt.xz                # Same
xz -k file.txt                  # Keep original
xz -9 file.txt                  # Maximum compression
xzcat file.txt.xz               # View without decompressing

# zstd (fast compression with good ratio — modern choice)
zstd file.txt                   # Compress → file.txt.zst
zstd -d file.txt.zst            # Decompress
zstd -19 file.txt               # Maximum level
zstd -T4 file.txt               # Use 4 threads

# zip/unzip
zip archive.zip file1 file2     # Create zip
zip -r archive.zip directory/   # Recursive
zip -e archive.zip file.txt     # Encrypted zip
unzip archive.zip               # Extract
unzip archive.zip -d /target/   # Extract to directory
unzip -l archive.zip            # List contents
unzip -p archive.zip file.txt   # Print specific file to stdout

# 7zip
7z a archive.7z directory/      # Create 7z archive
7z x archive.7z                 # Extract
7z l archive.7z                 # List
7z t archive.7z                 # Test integrity
7z a -p archive.7z dir/         # Password protected
```

### rsync — Remote/Local Sync

```bash
# rsync is the gold standard for file sync and backup
# Syntax: rsync [options] source destination

# Basic local sync
rsync -av source/ destination/         # Archive + verbose
rsync -avh source/ dest/               # Human-readable sizes
rsync -av --delete source/ dest/       # Delete files in dest not in source
rsync -av --dry-run source/ dest/      # Simulate (don't actually sync)
rsync -av -n source/ dest/             # Same (short form of --dry-run)

# Remote sync (uses SSH)
rsync -avz user@remote:/path/ /local/  # Remote → local
rsync -avz /local/ user@remote:/path/  # Local → remote
rsync -avze "ssh -p 2222" user@remote:/path/ /local/  # Custom SSH port
rsync -avz --progress source/ dest/    # Show progress
rsync -avz --partial source/ dest/     # Keep partial files (for large files)
rsync -avzP source/ dest/              # --partial --progress combined

# Advanced options
rsync -av --exclude="*.log" src/ dst/  # Exclude pattern
rsync -av --exclude={".git","*.bak"} src/ dst/  # Multiple excludes
rsync -av --exclude-from=exclude.txt src/ dst/  # Exclude list file
rsync -av --include="*.conf" --exclude="*" src/ dst/  # Include only
rsync -av --bwlimit=1000 src/ dst/     # Bandwidth limit (KB/s)
rsync -av --checksum src/ dst/         # Check by checksum, not mtime
rsync -av --link-dest=/previous/backup/ src/ new_backup/  # Hardlink unchanged files (efficient)

# Backup with rsync
rsync -avz --delete \
    --exclude=".cache/" \
    --exclude="*.tmp" \
    --log-file=/var/log/rsync.log \
    /home/alice/ backup@server:/backups/alice/
```

---

## 20. SSH & Remote Access

### SSH Basics

SSH (Secure Shell) provides encrypted remote access.

```bash
# Connect to remote host
ssh user@hostname               # Basic connection
ssh user@192.168.1.100          # By IP
ssh -p 2222 user@hostname       # Custom port
ssh -i ~/.ssh/mykey user@host   # Specific key
ssh -v user@hostname            # Verbose (debug)
ssh -vvv user@hostname          # Very verbose

# Run command without interactive shell
ssh user@host "ls -la /var/log/"
ssh user@host "sudo systemctl restart nginx"
ssh user@host "bash -s" < local_script.sh

# X11 forwarding (run GUI apps remotely)
ssh -X user@host                # X11 forwarding
ssh -Y user@host                # Trusted X11 forwarding
```

### SSH Key Authentication

```bash
# Generate key pair
ssh-keygen -t rsa -b 4096 -C "alice@example.com"    # RSA 4096-bit
ssh-keygen -t ed25519 -C "alice@example.com"          # Ed25519 (recommended, modern)
ssh-keygen -t ecdsa -b 521 -C "alice@example.com"    # ECDSA
# Files: ~/.ssh/id_ed25519 (private), ~/.ssh/id_ed25519.pub (public)

# Copy public key to remote host
ssh-copy-id user@hostname                # Copy default key
ssh-copy-id -i ~/.ssh/mykey.pub user@host  # Specific key
ssh-copy-id -p 2222 user@hostname        # Custom port

# Manually add public key
cat ~/.ssh/id_ed25519.pub | ssh user@host "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"

# Key permissions (SSH is strict about this)
chmod 700 ~/.ssh/
chmod 600 ~/.ssh/id_ed25519      # Private key
chmod 644 ~/.ssh/id_ed25519.pub  # Public key
chmod 600 ~/.ssh/authorized_keys
chmod 600 ~/.ssh/config
chmod 644 ~/.ssh/known_hosts
```

### SSH Config File

```bash
# ~/.ssh/config — makes SSH connections easier
Host myserver
    HostName 192.168.1.100
    User alice
    Port 2222
    IdentityFile ~/.ssh/my_server_key
    ServerAliveInterval 60
    ServerAliveCountMax 3

Host bastion
    HostName bastion.example.com
    User alice
    ForwardAgent yes

Host internal-server
    HostName 10.0.0.50
    User root
    ProxyJump bastion          # Hop through bastion

Host *
    ServerAliveInterval 60
    ControlMaster auto
    ControlPath ~/.ssh/sockets/%r@%h-%p
    ControlPersist 600         # Keep connection alive 600 seconds

# Now connect with:
ssh myserver                   # Uses all the settings above
```

### SSH Server Configuration

```bash
# /etc/ssh/sshd_config
Port 22                        # Listening port
ListenAddress 0.0.0.0          # Listen on all interfaces
PermitRootLogin no             # Disable root login (security!)
PermitRootLogin prohibit-password  # Root with key only
PasswordAuthentication no      # Disable password auth (key only — recommended!)
PubkeyAuthentication yes       # Enable key auth
AuthorizedKeysFile .ssh/authorized_keys
MaxAuthTries 3                 # Max auth attempts
LoginGraceTime 30              # Seconds to complete login
X11Forwarding yes              # Allow X11 forwarding
AllowUsers alice bob           # Whitelist specific users
AllowGroups sshusers           # Whitelist groups
DenyUsers hacker               # Blacklist users
MaxSessions 10                 # Max sessions per connection
Banner /etc/ssh/banner.txt     # Login banner
ClientAliveInterval 300        # Ping client every 300s
ClientAliveCountMax 2          # Disconnect after 2 missed pings

# Apply changes
sudo systemctl restart sshd
sudo sshd -t                   # Test config syntax
```

### SSH Tunneling

```bash
# Local port forwarding: forward local port to remote service
ssh -L 8080:localhost:80 user@remote       # remote:80 → local:8080
ssh -L 5432:db-server:5432 user@bastion   # db:5432 → local:5432 via bastion
ssh -L 8080:internal-web:80 user@gateway  # Access internal site

# Remote port forwarding: expose local service to remote
ssh -R 8080:localhost:3000 user@remote    # local:3000 → remote:8080

# Dynamic port forwarding (SOCKS proxy)
ssh -D 1080 user@remote                   # Creates SOCKS5 proxy on port 1080
# Configure browser to use SOCKS5 proxy at localhost:1080

# SSH tunneling options
ssh -N -f -L 8080:localhost:80 user@host  # -N=no command, -f=background

# Jump hosts / ProxyJump
ssh -J bastion user@internal-server       # Jump through bastion
ssh -J user@bastion,user@hop2 user@final  # Multiple hops
```

### SCP and SFTP

```bash
# scp — secure copy
scp file.txt user@host:/remote/path/       # Local → remote
scp user@host:/remote/file.txt /local/     # Remote → local
scp -r directory/ user@host:/remote/       # Recursive directory
scp -P 2222 file.txt user@host:/path/      # Custom port
scp -i ~/.ssh/key file.txt user@host:/     # Specific key
scp -C file.txt user@host:/path/           # Compress during transfer

# sftp — interactive secure file transfer
sftp user@host                             # Connect
# sftp commands:
# ls         = list remote
# lls        = list local
# cd         = change remote dir
# lcd        = change local dir
# get file   = download file
# put file   = upload file
# rm file    = delete remote file
# mkdir dir  = create remote dir
# bye/exit   = disconnect
# ? / help   = help

# Non-interactive sftp
echo "get /remote/file.txt" | sftp user@host
sftp user@host <<EOF
cd /var/www/html
put index.html
EOF
```

---

## 21. Security & Firewall

### iptables

iptables is the traditional Linux firewall (packet filtering).

```bash
# View rules
sudo iptables -L                 # List all rules
sudo iptables -L -n -v           # Verbose with packet counts
sudo iptables -L -n -v --line-numbers  # With line numbers
sudo iptables -L INPUT           # Specific chain
sudo iptables -t nat -L          # NAT table

# Tables: filter (default), nat, mangle, raw, security
# Chains: INPUT (incoming), OUTPUT (outgoing), FORWARD (routing through)
# Targets: ACCEPT, DROP, REJECT, LOG, RETURN, MASQUERADE, DNAT, SNAT

# Basic rules
sudo iptables -P INPUT DROP           # Default policy: drop incoming
sudo iptables -P FORWARD DROP         # Default policy: drop forwarded
sudo iptables -P OUTPUT ACCEPT        # Default policy: accept outgoing

# Allow established connections
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Allow loopback
sudo iptables -A INPUT -i lo -j ACCEPT

# Allow SSH
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Allow HTTP/HTTPS
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# Allow from specific IP
sudo iptables -A INPUT -s 192.168.1.0/24 -j ACCEPT

# Drop from specific IP
sudo iptables -A INPUT -s 10.0.0.5 -j DROP

# Allow ping
sudo iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT

# Insert rule at specific position
sudo iptables -I INPUT 1 -p tcp --dport 80 -j ACCEPT

# Delete rules
sudo iptables -D INPUT -p tcp --dport 80 -j ACCEPT  # By specification
sudo iptables -D INPUT 3                            # By line number
sudo iptables -F                                    # Flush (delete all) rules
sudo iptables -F INPUT                              # Flush specific chain
sudo iptables -X                                    # Delete custom chains

# Save and restore
sudo iptables-save > /etc/iptables/rules.v4         # Save
sudo iptables-restore < /etc/iptables/rules.v4      # Restore
sudo apt install iptables-persistent                # Auto-load on boot
sudo netfilter-persistent save                      # Save current rules

# NAT — masquerading (IP sharing)
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
sudo sysctl net.ipv4.ip_forward=1    # Enable IP forwarding
```

### UFW — Uncomplicated Firewall (Ubuntu/Debian)

```bash
sudo ufw status                  # Status
sudo ufw status verbose          # Verbose status
sudo ufw enable                  # Enable
sudo ufw disable                 # Disable
sudo ufw reset                   # Reset all rules

# Default policies
sudo ufw default deny incoming   # Block all incoming
sudo ufw default allow outgoing  # Allow all outgoing

# Allow/deny
sudo ufw allow ssh               # Allow SSH (port 22)
sudo ufw allow 22/tcp            # Same, explicit
sudo ufw allow 80/tcp            # HTTP
sudo ufw allow 443/tcp           # HTTPS
sudo ufw allow 8080              # Custom port
sudo ufw allow 9000:9100/tcp     # Port range
sudo ufw allow from 192.168.1.0/24  # From subnet
sudo ufw allow from 10.0.0.5 to any port 22  # From specific IP to port
sudo ufw deny 23                 # Deny telnet
sudo ufw delete allow 80/tcp     # Delete rule
sudo ufw delete 3                # Delete rule by number

# Named services (from /etc/services)
sudo ufw allow 'Nginx Full'      # Nginx HTTP + HTTPS
sudo ufw allow 'OpenSSH'
sudo ufw app list                # List application profiles

# Numbered rules
sudo ufw status numbered         # Show with numbers
sudo ufw delete 3                # Delete rule 3

# Logging
sudo ufw logging on
sudo ufw logging low/medium/high/full
```

### firewalld — Dynamic Firewall (RHEL/Fedora)

```bash
sudo firewall-cmd --state                    # Status
sudo firewall-cmd --list-all                 # Show active zone rules
sudo firewall-cmd --list-all-zones           # All zones
sudo firewall-cmd --get-active-zones         # Active zones

# Add services
sudo firewall-cmd --add-service=http         # Temporary (until reload)
sudo firewall-cmd --add-service=https
sudo firewall-cmd --add-service=ssh --permanent  # Permanent
sudo firewall-cmd --remove-service=http --permanent

# Add ports
sudo firewall-cmd --add-port=8080/tcp --permanent
sudo firewall-cmd --add-port=9000-9100/tcp --permanent
sudo firewall-cmd --remove-port=8080/tcp --permanent

# Zones
sudo firewall-cmd --set-default-zone=home
sudo firewall-cmd --zone=public --add-service=http --permanent
sudo firewall-cmd --zone=trusted --add-source=192.168.1.0/24 --permanent

# Reload to apply permanent changes
sudo firewall-cmd --reload

# NAT / Masquerading
sudo firewall-cmd --add-masquerade --permanent
sudo firewall-cmd --reload
```

### fail2ban — Brute Force Protection

```bash
sudo apt install fail2ban

# Config: /etc/fail2ban/jail.conf (don't edit)
# Override: /etc/fail2ban/jail.local

# Example jail.local
[DEFAULT]
bantime = 3600      # Ban for 1 hour
findtime = 600      # Within 10 minutes
maxretry = 5        # After 5 failures

[sshd]
enabled = true
port = ssh
logpath = /var/log/auth.log
maxretry = 3
bantime = 86400     # 24 hours for SSH

# Commands
sudo systemctl start fail2ban
sudo fail2ban-client status          # Overall status
sudo fail2ban-client status sshd     # SSH jail status
sudo fail2ban-client set sshd unbanip 1.2.3.4  # Unban IP
sudo fail2ban-client get sshd banned  # List banned IPs

# View logs
sudo tail -f /var/log/fail2ban.log
```

### SELinux (Security-Enhanced Linux) — RHEL/Fedora

```bash
# SELinux status
getenforce                       # Enforcing / Permissive / Disabled
sestatus                         # Detailed status
sestatus -v                      # Verbose

# Modes
sudo setenforce 1                # Enforcing (blocks violations)
sudo setenforce 0                # Permissive (logs but doesn't block)
# Persistent: edit /etc/selinux/config
SELINUX=enforcing   # OR permissive OR disabled

# File contexts
ls -Z file.txt                   # Show SELinux context
ls -Z /var/www/html/             # Directory context
stat -c %C file.txt              # Context via stat

# Change context
chcon -t httpd_sys_content_t /var/www/html/file.html  # Temporary
restorecon -v /var/www/html/     # Restore default context
restorecon -Rv /var/www/html/    # Recursive

# Manage file contexts persistently
semanage fcontext -l             # List contexts
semanage fcontext -a -t httpd_sys_content_t "/mywebroot(/.*)?"
restorecon -Rv /mywebroot/

# Booleans
getsebool -a                     # All booleans
getsebool httpd_can_network_connect  # Specific
setsebool httpd_can_network_connect 1   # Temporary
setsebool -P httpd_can_network_connect 1  # Persistent

# Audit logs
ausearch -m avc -ts recent       # Recent AVC denials
audit2why < /var/log/audit/audit.log  # Explain denials
audit2allow -a                   # Generate allow rules from denials
```

### AppArmor — Mandatory Access Control (Ubuntu/Debian)

```bash
sudo systemctl status apparmor   # Status
apparmor_status                  # Profile status
aa-status                        # Same

# Modes
aa-complain /path/to/binary      # Complain mode (log only)
aa-enforce /path/to/binary       # Enforce mode (block)
aa-disable /etc/apparmor.d/usr.bin.nginx  # Disable profile

# Profiles location
/etc/apparmor.d/                 # Profile files

# Logs
sudo dmesg | grep apparmor
sudo tail -f /var/log/syslog | grep apparmor
```

---

## 22. Advanced File Operations

### Find with Execute

```bash
# Find and execute commands
find /var/log -name "*.log" -exec gzip {} \;    # Compress all log files
find /home -name "*.bak" -exec rm -f {} \;     # Delete all .bak files
find . -name "*.py" -exec chmod 644 {} \;      # Set permissions
find . -newer reference_file -exec ls -la {} \; # Files newer than reference

# xargs — build and execute commands from stdin
find . -name "*.txt" | xargs grep "TODO"        # Grep in all .txt files
find . -name "*.log" | xargs -I {} cp {} /backup/  # Copy with -I
find . -name "*.jpg" | xargs -P 4 -I {} convert {} {}.png  # Parallel
ls *.txt | xargs rm                             # Remove listed files
echo "file1 file2 file3" | xargs touch          # Create files

# Handle filenames with spaces
find . -name "*.txt" -print0 | xargs -0 grep "TODO"  # Null-terminated
find . -name "*.txt" -exec grep -l "TODO" {} +       # More efficient
```

### File Descriptors and Redirection

```bash
# File descriptor numbers
0 = stdin
1 = stdout
2 = stderr

# Redirect stderr
command 2>/dev/null              # Discard errors
command 2>&1 > file.txt         # stderr to stdout, both to file
command > file.txt 2>&1         # Both to file (more common order)
command &> file.txt             # Both to file (bash shortcut)
command 2>&1 | tee log.txt      # Both to file and screen

# Create and use custom file descriptors
exec 3> output.txt              # Open fd 3 for writing
exec 4< input.txt               # Open fd 4 for reading
echo "hello" >&3                # Write to fd 3
read line <&4                   # Read from fd 4
exec 3>&-                       # Close fd 3
exec 4<&-                       # Close fd 4
```

### Inodes and Hard Links

```bash
stat file.txt                   # All file metadata including inode
ls -i file.txt                  # Show inode number
ls -li                          # Long listing with inodes
find / -inum 12345              # Find file by inode number
find / -samefile file.txt       # Find all hard links to same file
find / -links +1 -type f        # Files with multiple hard links

# Inode information
df -i                           # Inode usage per filesystem
tune2fs -l /dev/sda1 | grep -i inode   # ext4 inode info
```

### File Attributes (chattr/lsattr)

```bash
lsattr file.txt                 # Show file attributes
lsattr -R directory/            # Recursive
chattr +i file.txt              # Set immutable (cannot modify/delete/rename)
chattr -i file.txt              # Remove immutable
chattr +a file.txt              # Append-only
chattr +e file.txt              # Extent format
chattr +u file.txt              # Undeletable

# Common attributes
# i = immutable
# a = append only
# A = no atime update
# d = no dump
# s = secure deletion (zero blocks)
# S = synchronous writes
# u = undeletable
# e = extent format
# C = no copy-on-write (btrfs)

# Useful: protect important files
sudo chattr +i /etc/passwd      # Protect passwd
sudo chattr +a /var/log/secure  # Append-only log
```

### dd — Disk Dump/Convert

```bash
# dd: if=input file, of=output file, bs=block size, count=blocks

# Copy disk/partition
sudo dd if=/dev/sda of=/dev/sdb bs=4M status=progress  # Disk clone
sudo dd if=/dev/sda of=disk.img bs=4M status=progress  # Create disk image
sudo dd if=disk.img of=/dev/sdb bs=4M status=progress  # Restore image

# Create files
dd if=/dev/zero of=1GB.file bs=1M count=1024     # Create 1GB zeroed file
dd if=/dev/urandom of=random.bin bs=1M count=100 # 100MB random data

# Write ISO to USB
sudo dd if=ubuntu.iso of=/dev/sdb bs=4M status=progress oflag=sync

# Test disk speed
sudo dd if=/dev/zero of=/tmp/test bs=1M count=1000 status=progress  # Write speed
sudo dd if=/tmp/test of=/dev/null bs=1M status=progress              # Read speed

# Backup and restore MBR
sudo dd if=/dev/sda of=mbr.bin bs=512 count=1    # Backup MBR
sudo dd if=mbr.bin of=/dev/sda bs=512 count=1    # Restore MBR

# Wipe disk
sudo dd if=/dev/zero of=/dev/sdb bs=1M status=progress   # Overwrite with zeros
sudo dd if=/dev/urandom of=/dev/sdb bs=1M status=progress # Overwrite with random (secure)
```

---

## 23. Kernel & System Internals

### Kernel Modules

```bash
# List modules
lsmod                            # Currently loaded modules
modinfo module_name              # Module information
modinfo ext4                     # Info about ext4 module

# Load/unload modules
sudo modprobe module_name        # Load module (with dependencies)
sudo modprobe -r module_name     # Remove module
sudo insmod /path/to/module.ko   # Insert module (no deps)
sudo rmmod module_name           # Remove module

# Persistent module loading
echo "module_name" | sudo tee /etc/modules-load.d/module_name.conf

# Module options
sudo modprobe module_name param=value
# Persistent options:
echo "options module_name param=value" | sudo tee /etc/modprobe.d/module_name.conf

# Blacklist modules
echo "blacklist nouveau" | sudo tee /etc/modprobe.d/blacklist.conf
```

### sysctl — Kernel Parameter Tuning

```bash
sysctl -a                        # All kernel parameters
sysctl net.ipv4.ip_forward       # Read specific parameter
sudo sysctl -w net.ipv4.ip_forward=1  # Set parameter (temporary)
sudo sysctl -p                   # Apply /etc/sysctl.conf
sudo sysctl -p /etc/sysctl.d/99-custom.conf  # Apply specific file

# Persistent settings: /etc/sysctl.conf or /etc/sysctl.d/*.conf
# Common tuning parameters:
net.ipv4.ip_forward = 1                    # Enable IP routing
net.ipv4.tcp_syncookies = 1               # SYN flood protection
net.ipv4.conf.all.accept_redirects = 0    # Disable ICMP redirects
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.rp_filter = 1      # Reverse path filtering
net.core.somaxconn = 65535                # Max socket backlog
net.ipv4.tcp_max_syn_backlog = 65535     # SYN backlog
vm.swappiness = 10                        # Use swap less
vm.dirty_ratio = 40                       # % RAM for dirty pages before sync
vm.dirty_background_ratio = 10            # % RAM for background sync
kernel.panic = 60                         # Reboot after 60s kernel panic
kernel.sysrq = 1                          # Enable SysRq key
fs.file-max = 2097152                     # Max open files system-wide
net.core.rmem_max = 16777216             # Max receive buffer
net.core.wmem_max = 16777216             # Max send buffer
```

### Kernel Compilation (Overview)

```bash
# Get kernel source
wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.x.tar.xz
tar -xJf linux-6.x.tar.xz
cd linux-6.x

# Configure
make menuconfig          # ncurses GUI configuration
make xconfig             # Qt GUI configuration
make oldconfig           # Use existing .config with prompts for new options
make localmodconfig      # Config based on currently loaded modules

# Build
make -j$(nproc)          # Compile (use all CPUs)
make -j$(nproc) modules  # Compile modules

# Install
sudo make modules_install
sudo make install        # Installs to /boot
sudo update-grub         # Update bootloader
```

### Virtual Memory System

```bash
# Memory info
free -h                          # Memory summary
cat /proc/meminfo                # Detailed memory info
vmstat -s                        # Statistics

# Page cache
sudo sync                        # Flush filesystem buffers
sudo echo 1 > /proc/sys/vm/drop_caches  # Drop page cache
sudo echo 2 > /proc/sys/vm/drop_caches  # Drop dentries and inodes
sudo echo 3 > /proc/sys/vm/drop_caches  # Drop all caches

# OOM Killer
cat /proc/1234/oom_score         # OOM score of process
cat /proc/1234/oom_adj           # OOM adjustment
echo -17 > /proc/1234/oom_adj   # Protect process from OOM killer
dmesg | grep -i "out of memory" # OOM killer events
dmesg | grep -i "oom"           # OOM events
```

---

## 24. Virtualization & Containers

### KVM/QEMU Virtualization

```bash
# Check KVM support
egrep -c '(vmx|svm)' /proc/cpuinfo    # >0 means supported
kvm-ok                                  # Ubuntu tool

# Install KVM
sudo apt install qemu-kvm libvirt-daemon-system virtinst bridge-utils
sudo usermod -aG libvirt,kvm $USER

# virsh — manage VMs
virsh list                        # Running VMs
virsh list --all                  # All VMs
virsh start vm-name               # Start VM
virsh shutdown vm-name            # Graceful shutdown
virsh destroy vm-name             # Force stop
virsh reboot vm-name              # Reboot
virsh console vm-name             # Connect to console
virsh dominfo vm-name             # VM info
virsh domstats vm-name            # Statistics

# Create VM
virt-install \
  --name ubuntu-vm \
  --ram 2048 \
  --vcpus 2 \
  --disk path=/var/lib/libvirt/images/ubuntu.qcow2,size=20 \
  --os-variant ubuntu20.04 \
  --cdrom /path/to/ubuntu.iso \
  --network default \
  --graphics vnc

# Snapshots
virsh snapshot-create-as vm-name snapshot1 "Before update"
virsh snapshot-list vm-name
virsh snapshot-revert vm-name snapshot1
virsh snapshot-delete vm-name snapshot1
```

### Docker

```bash
# Installation
curl -fsSL https://get.docker.com | sh
sudo usermod -aG docker $USER

# Images
docker images                    # List images
docker pull ubuntu:22.04         # Download image
docker pull nginx:latest         # Latest nginx
docker rmi image_id              # Remove image
docker rmi $(docker images -q)   # Remove all images
docker build -t myapp:1.0 .      # Build from Dockerfile
docker push myapp:1.0            # Push to registry
docker tag source target         # Tag image
docker history image_id          # Image layers
docker inspect image_id          # Detailed info
docker search nginx              # Search Docker Hub

# Containers
docker ps                        # Running containers
docker ps -a                     # All containers
docker run ubuntu echo "Hello"   # Run one-off command
docker run -it ubuntu bash       # Interactive terminal
docker run -d nginx              # Detached (background)
docker run -d -p 8080:80 nginx   # Port mapping (host:container)
docker run -d -v /host/path:/container/path nginx  # Volume mount
docker run -d --name mycontainer nginx  # Named container
docker run --rm ubuntu echo "clean up after"  # Auto-remove on exit
docker run -e ENV_VAR=value nginx   # Environment variable
docker run --network=host nginx     # Host network

docker start container_id        # Start stopped container
docker stop container_id         # Graceful stop
docker kill container_id         # Force stop
docker rm container_id           # Remove container
docker rm $(docker ps -aq)       # Remove all stopped
docker restart container_id      # Restart
docker exec -it container bash   # Open shell in running container
docker exec container ls /var    # Run command in container
docker logs container            # View logs
docker logs -f container         # Follow logs
docker stats                     # Real-time stats
docker inspect container         # Detailed info
docker top container             # Running processes
docker cp container:/path/file . # Copy from container
docker cp file container:/path/  # Copy to container

# Dockerfile
# Example Dockerfile
FROM ubuntu:22.04
LABEL maintainer="alice@example.com"
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y \
    nginx \
    curl \
    && rm -rf /var/lib/apt/lists/*
COPY ./html/ /var/www/html/
COPY ./nginx.conf /etc/nginx/nginx.conf
EXPOSE 80 443
HEALTHCHECK --interval=30s --timeout=3s CMD curl -f http://localhost/ || exit 1
CMD ["nginx", "-g", "daemon off;"]

# Docker Compose
docker-compose up                # Start services
docker-compose up -d             # Detached
docker-compose down              # Stop and remove
docker-compose down -v           # Also remove volumes
docker-compose ps                # Status
docker-compose logs              # All logs
docker-compose logs -f service   # Follow specific service
docker-compose build             # Build images
docker-compose restart service   # Restart service

# Networks
docker network ls                # List networks
docker network create mynet      # Create network
docker network inspect mynet     # Inspect
docker run --network mynet nginx # Use network

# Volumes
docker volume ls                 # List volumes
docker volume create myvol       # Create
docker volume inspect myvol      # Inspect
docker volume rm myvol           # Remove
docker run -v myvol:/data nginx  # Use named volume
docker volume prune              # Remove unused volumes

# Cleanup
docker system prune              # Remove unused resources
docker system prune -a           # Remove ALL unused (including images)
docker system df                 # Disk usage
```

---

## 25. Cron Jobs & Task Scheduling

### Cron

```bash
# Crontab format:
# * * * * * command
# │ │ │ │ │
# │ │ │ │ └── Day of week (0-7, both 0 and 7 = Sunday)
# │ │ │ └──── Month (1-12)
# │ │ └────── Day of month (1-31)
# │ └──────── Hour (0-23)
# └────────── Minute (0-59)

# Special values:
# * = any/every
# 5 = exact value 5
# 5,10,15 = 5, 10, and 15
# 5-10 = 5 through 10
# */5 = every 5 (5,10,15,20...)
# @reboot = at startup
# @daily = once a day (0 0 * * *)
# @weekly = once a week (0 0 * * 0)
# @monthly = once a month (0 0 1 * *)
# @yearly = once a year (0 0 1 1 *)
# @hourly = every hour (0 * * * *)

# Edit crontab
crontab -e                       # Edit current user's crontab
sudo crontab -e -u alice         # Edit alice's crontab
crontab -l                       # List crontab
crontab -r                       # Remove crontab (BE CAREFUL)
crontab -l | crontab -           # Backup and reload

# Example crontab entries
# Run at 2:30 AM daily
30 2 * * * /usr/local/bin/backup.sh

# Run every 5 minutes
*/5 * * * * /usr/local/bin/health_check.sh

# Run every Monday at 9 AM
0 9 * * 1 /usr/local/bin/weekly_report.sh

# Run on 1st of every month at midnight
0 0 1 * * /usr/local/bin/monthly_cleanup.sh

# Run at reboot
@reboot /usr/local/bin/startup_tasks.sh

# Run Mon-Fri at 8 AM and 5 PM
0 8,17 * * 1-5 /usr/local/bin/notify.sh

# With logging
0 2 * * * /usr/local/bin/backup.sh >> /var/log/backup.log 2>&1

# With email notification (set MAILTO)
MAILTO=admin@example.com
*/10 * * * * /usr/local/bin/check.sh

# Disable email
MAILTO=""
*/10 * * * * /usr/local/bin/check.sh

# System-wide cron directories
/etc/cron.d/            # Drop-in crontab files
/etc/cron.daily/        # Scripts run daily (run-parts)
/etc/cron.weekly/       # Scripts run weekly
/etc/cron.monthly/      # Scripts run monthly
/etc/cron.hourly/       # Scripts run hourly
/etc/crontab            # System crontab (has username field)

# System crontab format (extra username field)
# * * * * * username command
0 2 * * * root /usr/local/bin/backup.sh
```

### at — One-time Scheduled Tasks

```bash
at 2pm tomorrow                  # Schedule for 2pm tomorrow
at 14:00 2025-01-15             # Specific date and time
at now + 1 hour                  # 1 hour from now
at now + 30 minutes
at next monday
# Then type commands, Ctrl+D to save

at -l                            # List pending jobs (same as atq)
atq                              # List pending jobs
at -r 3                          # Remove job 3 (same as atrm)
atrm 3                           # Remove job 3

# Non-interactive
echo "/usr/local/bin/backup.sh" | at 2am tomorrow
```

### Systemd Timers

Systemd timers are a more flexible cron alternative.

```bash
# List timers
systemctl list-timers            # Active timers
systemctl list-timers --all      # All timers

# Timer unit file: /etc/systemd/system/myapp.timer
[Unit]
Description=Run myapp daily

[Timer]
OnCalendar=daily
OnCalendar=*-*-* 02:00:00       # Every day at 2 AM
OnCalendar=Mon 09:00:00         # Every Monday at 9 AM
OnCalendar=*-01-01 00:00:00     # Every New Year
OnBootSec=15min                  # 15 min after boot
OnUnitActiveSec=1h               # Every hour after last run
Persistent=true                  # Run if missed (was offline)
Unit=myapp.service               # Which service to trigger

[Install]
WantedBy=timers.target

# Corresponding service unit: /etc/systemd/system/myapp.service
[Unit]
Description=My Application Task

[Service]
Type=oneshot
ExecStart=/usr/local/bin/myapp-task.sh
User=myappuser

# Enable and start
sudo systemctl daemon-reload
sudo systemctl enable myapp.timer
sudo systemctl start myapp.timer
```

---

## 26. Advanced Networking

### Network Namespaces

```bash
# Create namespace
sudo ip netns add myns

# List namespaces
ip netns list

# Run command in namespace
sudo ip netns exec myns ip addr
sudo ip netns exec myns bash    # Shell in namespace

# Create veth pair
sudo ip link add veth0 type veth peer name veth1

# Move interface to namespace
sudo ip link set veth1 netns myns

# Configure interface in namespace
sudo ip netns exec myns ip addr add 10.0.0.2/24 dev veth1
sudo ip netns exec myns ip link set veth1 up
sudo ip addr add 10.0.0.1/24 dev veth0
sudo ip link set veth0 up

# Delete namespace
sudo ip netns delete myns
```

### Network Bonding and Bridging

```bash
# Network bridge
sudo ip link add name br0 type bridge
sudo ip link set br0 up
sudo ip link set eth0 master br0
sudo ip addr add 192.168.1.100/24 dev br0
sudo ip route add default via 192.168.1.1 dev br0

# With nmcli
nmcli connection add type bridge con-name br0 ifname br0
nmcli connection add type bridge-slave con-name br0-port1 ifname eth0 master br0
nmcli connection modify br0 ipv4.addresses 192.168.1.100/24
nmcli connection modify br0 ipv4.gateway 192.168.1.1
nmcli connection modify br0 ipv4.method manual
nmcli connection up br0
```

### tc — Traffic Control (QoS)

```bash
# View existing queuing discipline
tc qdisc show dev eth0

# Limit bandwidth (token bucket filter)
sudo tc qdisc add dev eth0 root tbf rate 10mbit burst 32kbit latency 400ms

# Delete qdisc
sudo tc qdisc del dev eth0 root

# Simulate network conditions (netem)
sudo tc qdisc add dev eth0 root netem delay 100ms       # Add 100ms latency
sudo tc qdisc add dev eth0 root netem loss 1%           # 1% packet loss
sudo tc qdisc add dev eth0 root netem delay 100ms loss 0.1%  # Combined
sudo tc qdisc del dev eth0 root                         # Remove
```

### VLAN Configuration

```bash
sudo apt install vlan

# Create VLAN interface
sudo ip link add link eth0 name eth0.100 type vlan id 100
sudo ip addr add 192.168.100.1/24 dev eth0.100
sudo ip link set eth0.100 up

# Persistent (Netplan)
network:
  version: 2
  vlans:
    eth0.100:
      id: 100
      link: eth0
      addresses: [192.168.100.1/24]
```

### Network Troubleshooting Advanced

```bash
# Packet capture
sudo tcpdump -i eth0                          # Capture all traffic
sudo tcpdump -i eth0 port 80                  # HTTP traffic
sudo tcpdump -i eth0 host 192.168.1.5        # Specific host
sudo tcpdump -i eth0 -w capture.pcap          # Write to file
sudo tcpdump -r capture.pcap                  # Read from file
sudo tcpdump -i eth0 'tcp port 443 and host google.com'
sudo tcpdump -i eth0 -n -c 100               # No DNS, 100 packets
sudo tcpdump -i any port 22                  # All interfaces

# Wireshark (GUI)
wireshark &                                   # Open GUI
tshark -i eth0                               # Command-line Wireshark
tshark -r capture.pcap -Y "http"            # Filter display

# ARP
arp -n                                        # ARP table
arp -d 192.168.1.1                           # Delete ARP entry
arping 192.168.1.1                           # ARP ping
ip neigh show                                # Modern equivalent
ip neigh flush all                           # Flush ARP cache
```

---

## 27. Performance Tuning & Optimization

### CPU Performance

```bash
# CPU frequency scaling
cpupower frequency-info                       # Current CPU freq info
sudo cpupower frequency-set -g performance   # Set performance governor
sudo cpupower frequency-set -g powersave     # Set powersave governor

# Governors: performance, powersave, ondemand, conservative, schedutil
cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor

# CPU affinity (bind process to specific CPUs)
taskset -c 0,1 command                       # Run on CPUs 0 and 1
taskset -p 0x3 PID                          # Set mask for running process
taskset -c 2-5 -p PID                       # CPUs 2-5 for running process

# CPU isolation (prevent kernel from using CPUs)
# Kernel parameter: isolcpus=2,3            # Isolate CPUs 2 and 3

# NUMA topology
numactl --hardware                           # NUMA hardware info
numactl --membind=0 --cpunodebind=0 command # Run on NUMA node 0
numastat                                     # NUMA statistics
```

### Memory Performance

```bash
# Huge pages
cat /proc/meminfo | grep HugePages           # Huge page info
echo 256 | sudo tee /proc/sys/vm/nr_hugepages  # Set 256 huge pages
sudo sysctl vm.nr_hugepages=256               # Same via sysctl

# Transparent huge pages
cat /sys/kernel/mm/transparent_hugepage/enabled
echo always | sudo tee /sys/kernel/mm/transparent_hugepage/enabled
echo madvise | sudo tee /sys/kernel/mm/transparent_hugepage/enabled
echo never | sudo tee /sys/kernel/mm/transparent_hugepage/enabled

# Memory pressure testing
stress-ng --vm 2 --vm-bytes 1G --vm-method all --verify -t 30  # Memory stress
```

### Disk I/O Performance

```bash
# I/O schedulers
cat /sys/block/sda/queue/scheduler           # Current scheduler
# Schedulers: none, mq-deadline, kyber, bfq
echo mq-deadline | sudo tee /sys/block/sda/queue/scheduler  # Change

# Disk benchmarking
sudo hdparm -tT /dev/sda                    # Read speed test
sudo hdparm -I /dev/sda                     # Disk information
fio --name=write_test --rw=write --bs=4k --size=1G --numjobs=4 --runtime=60 --iodepth=64 --direct=1

# I/O priority (ionice)
ionice -c 3 command                          # Idle class
ionice -c 2 -n 0 command                    # Best-effort, highest priority
ionice -c 1 -n 4 command                    # Real-time class
ionice -p PID                               # Show priority of process
```

### Network Performance

```bash
# TCP tuning (add to /etc/sysctl.conf)
net.core.rmem_max = 67108864
net.core.wmem_max = 67108864
net.ipv4.tcp_rmem = 4096 87380 67108864
net.ipv4.tcp_wmem = 4096 65536 67108864
net.core.netdev_max_backlog = 250000
net.ipv4.tcp_max_syn_backlog = 8192
net.ipv4.tcp_tw_reuse = 1
net.ipv4.ip_local_port_range = 10000 65000

# ethtool — network interface settings
ethtool eth0                                 # Interface info
ethtool -i eth0                             # Driver info
ethtool -S eth0                             # Statistics
ethtool -g eth0                             # Ring buffer sizes
sudo ethtool -G eth0 rx 4096 tx 4096       # Set ring buffer sizes
sudo ethtool -K eth0 tso on gso on gro on  # Enable offloading
sudo ethtool -s eth0 speed 1000 duplex full # Force speed/duplex
sudo ethtool -A eth0 rx on tx on           # Enable flow control
```

### perf — Performance Analysis

```bash
sudo perf top                               # Real-time top events
sudo perf stat command                      # Stats for command
sudo perf record command                    # Record events
sudo perf report                            # Analyze recorded
sudo perf record -g -p PID                 # Record with call graphs
sudo perf list                              # Available events
```

---

## 28. Troubleshooting & Debugging

### Boot Troubleshooting

```bash
# Access rescue mode
# 1. At GRUB menu, press 'e' to edit
# 2. Find the linux line, append: systemd.unit=rescue.target
# 3. Press Ctrl+X to boot

# Emergency mode (even more minimal)
# Append: systemd.unit=emergency.target

# Boot to single user mode
# Append: single   OR   init=/bin/bash

# Repair filesystem
# When system won't boot due to filesystem error:
# 1. Boot with init=/bin/bash
# 2. mount -o remount,rw /
# 3. fsck -y /dev/sda1
# 4. mount -a
# 5. reboot -f

# View boot logs
journalctl -b              # Current boot
journalctl -b -1           # Previous boot
journalctl -b -p err       # Boot errors
dmesg | less               # Kernel ring buffer
```

### System Debugging Tools

```bash
# strace — trace system calls
strace command                               # Trace all syscalls
strace -p PID                               # Attach to running process
strace -e trace=open,read,write command     # Specific syscalls
strace -o strace.log command                # Output to file
strace -c command                           # Count and summarize
strace -f command                           # Follow forks

# ltrace — trace library calls
ltrace command                              # Trace library calls

# gdb — GNU debugger
gdb program                                 # Debug program
gdb program core                            # Analyze core dump
gdb -p PID                                 # Attach to process
# GDB commands: run, break, continue, next, step, print, backtrace, quit

# valgrind — memory debugging
valgrind command                            # Memory check
valgrind --leak-check=full command          # Full leak check
valgrind --tool=massif command             # Memory profiler

# Core dumps
ulimit -c unlimited                         # Enable core dumps
cat /proc/sys/kernel/core_pattern          # Core dump location
echo "/tmp/core.%e.%p" | sudo tee /proc/sys/kernel/core_pattern
gdb ./program ./core                        # Analyze core dump
```

### Network Troubleshooting

```bash
# Connectivity debugging checklist
1. ping 127.0.0.1             # Is networking stack working?
2. ping 192.168.1.1           # Can we reach the gateway?
3. ping 8.8.8.8               # Can we reach the internet?
4. ping google.com            # Is DNS working?

# If step 1 fails: kernel network stack issue
# If step 2 fails: local network/interface issue
# If step 3 fails: gateway/routing issue
# If step 4 fails: DNS issue

# Network diagnostic
ip addr                       # Check IPs
ip route                      # Check routes
cat /etc/resolv.conf          # Check DNS config
ss -tulnp                     # Check listening ports
iptables -L -n -v             # Check firewall

# DNS troubleshooting
dig google.com                # Query DNS
dig @8.8.8.8 google.com       # Query specific server
dig +trace google.com          # Trace DNS resolution
nslookup -debug google.com    # Debug DNS
cat /etc/nsswitch.conf        # Name resolution order
```

### Disk Troubleshooting

```bash
# Check disk errors
sudo dmesg | grep -iE "error|fault|fail|disk|sda"
sudo smartctl -a /dev/sda                    # SMART data
sudo smartctl -t short /dev/sda             # Run short test
sudo badblocks -v /dev/sda                  # Check for bad blocks
sudo fsck -f /dev/sda1                      # Filesystem check

# Recover deleted files
sudo apt install testdisk
sudo photorec /dev/sda                       # File recovery
sudo testdisk /dev/sda                      # Partition recovery

# Check I/O performance issues
iostat -x 2 10                              # Extended I/O stats
iotop                                        # Per-process I/O (like top)
atop                                         # Advanced system monitor
```

---

## 29. Linux for Developers

### Build Tools and Compilers

```bash
# GCC — GNU Compiler Collection
gcc hello.c -o hello                         # Compile C
gcc -o hello hello.c -Wall -Wextra          # With warnings
gcc -O2 hello.c -o hello                    # Optimization level 2
gcc -g hello.c -o hello                     # With debug info
gcc -shared -fPIC lib.c -o libmy.so         # Shared library
g++ hello.cpp -o hello                       # C++
g++ -std=c++17 hello.cpp -o hello           # C++17 standard
gfortran hello.f90 -o hello                 # Fortran

# Make
make                                         # Build (reads Makefile)
make clean                                   # Clean build artifacts
make install                                 # Install
make -j$(nproc)                             # Parallel build
make -f custom.makefile                     # Custom makefile
make CFLAGS="-O3" all                       # Pass variables

# CMake
mkdir build && cd build
cmake ..                                     # Configure
cmake .. -DCMAKE_BUILD_TYPE=Release
cmake --build .                             # Build
cmake --install . --prefix /usr/local       # Install
```

### Programming Language Tools

```bash
# Python
python3 --version
python3 script.py
pip3 install package
pip3 install -r requirements.txt
python3 -m venv myenv                       # Virtual environment
source myenv/bin/activate                   # Activate venv
deactivate                                  # Deactivate venv

# Node.js
node script.js
npm install
npm install packagename
npm install -g packagename                  # Global install
npx create-react-app myapp

# Java
javac Hello.java                            # Compile
java Hello                                  # Run
mvn package                                 # Maven build
gradle build                                # Gradle build

# Ruby
ruby script.rb
gem install bundler
bundle install

# Go
go run main.go
go build .
go test ./...
go mod init mymodule

# Rust
cargo new myproject
cargo build
cargo run
cargo test
```

### Version Control (Git)

```bash
# Git basics
git init                                    # Initialize repo
git clone URL                               # Clone repo
git status                                  # Working tree status
git add file.txt                            # Stage file
git add .                                   # Stage all
git add -p                                  # Interactive staging
git commit -m "Message"                     # Commit
git commit -am "Message"                    # Stage all tracked + commit

# Branches
git branch                                  # List branches
git branch feature-x                        # Create branch
git checkout feature-x                      # Switch branch
git checkout -b feature-x                   # Create + switch
git switch -c feature-x                     # Modern way
git merge feature-x                         # Merge branch
git rebase main                             # Rebase onto main
git branch -d feature-x                     # Delete merged branch
git branch -D feature-x                     # Force delete

# Remote
git remote -v                               # List remotes
git remote add origin URL                   # Add remote
git push origin main                        # Push
git push -u origin main                     # Push + set upstream
git pull                                    # Pull (fetch + merge)
git fetch                                   # Fetch only
git pull --rebase                           # Pull with rebase

# History
git log                                     # Commit history
git log --oneline --graph --all             # Compact tree view
git show commit_hash                        # Show specific commit
git diff                                    # Unstaged changes
git diff --staged                           # Staged changes
git diff HEAD~3 HEAD                       # Last 3 commits
git blame file.txt                          # Who changed each line

# Undo
git restore file.txt                        # Discard unstaged changes
git restore --staged file.txt               # Unstage file
git reset HEAD~1                            # Undo last commit (keep changes)
git reset --hard HEAD~1                    # Undo last commit (discard changes)
git revert commit_hash                      # Reverse a commit (safe)
git stash                                   # Stash changes
git stash pop                               # Apply stash
git stash list                              # List stashes

# Tags
git tag v1.0.0                              # Lightweight tag
git tag -a v1.0.0 -m "Release 1.0"        # Annotated tag
git push origin --tags                      # Push tags
```

### System Calls and IPC

```bash
# Shared memory
ipcs                                        # List IPC resources
ipcrm -m shmid                              # Remove shared memory segment

# Message queues
ipcs -q                                     # List message queues
ipcrm -q msqid                              # Remove queue

# Semaphores
ipcs -s                                     # List semaphores

# Sockets
ss -lnt                                     # Listening TCP sockets
ss -lnu                                     # Listening UDP sockets
ss -lnx                                     # Unix domain sockets
ls /tmp/*.sock /var/run/*.sock 2>/dev/null  # Socket files
```

---

## 30. Professional Cheat Sheet

---

```
╔══════════════════════════════════════════════════════════════════════════════╗
║           LINUX PROFESSIONAL CHEAT SHEET — MASTER REFERENCE               ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

---

### NAVIGATION & FILES

| Command | Description |
|---|---|
| `pwd` | Current directory |
| `ls -lah` | List all files (long + hidden + human sizes) |
| `ls -lt` | Sort by time (newest first) |
| `cd -` | Previous directory |
| `find / -name "*.conf" 2>/dev/null` | Find files system-wide |
| `find . -type f -mtime -7` | Files modified in last 7 days |
| `find . -size +100M` | Files larger than 100MB |
| `locate filename` | Fast file search (database) |
| `which command` | Location of executable |
| `type command` | Type of command |

---

### FILE OPERATIONS

| Command | Description |
|---|---|
| `cp -rp src/ dst/` | Copy recursively preserving attributes |
| `mv -iv src dst` | Move interactively verbose |
| `rm -ri dir/` | Remove interactively recursive |
| `ln -s target linkname` | Create symbolic link |
| `ln target hardlink` | Create hard link |
| `readlink -f link` | Fully resolved symlink path |
| `stat file` | File metadata (size, inode, times) |
| `file filename` | Identify file type |
| `wc -l file` | Count lines |
| `diff file1 file2` | Show differences |
| `diff -u file1 file2` | Unified diff (patch format) |
| `patch file < patch.diff` | Apply patch |
| `split -b 1G bigfile part_` | Split file into 1GB parts |

---

### TEXT PROCESSING

| Command | Description |
|---|---|
| `cat -n file` | Show file with line numbers |
| `head -n 20 file` | First 20 lines |
| `tail -f log` | Follow log in real time |
| `grep -rn "pattern" .` | Recursive search with line numbers |
| `grep -v "exclude"` | Show non-matching lines |
| `sed 's/old/new/g' file` | Replace all occurrences |
| `sed -i.bak 's/a/b/g' file` | In-place with backup |
| `sed '/^#/d; /^$/d' file` | Remove comments and blanks |
| `awk '{print $2}' file` | Print second field |
| `awk -F: '{print $1}' /etc/passwd` | Extract first field |
| `awk '{sum+=$1}END{print sum}'` | Sum a column |
| `sort \| uniq -c \| sort -rn` | Frequency count (pipe chain) |
| `cut -d: -f1,3 /etc/passwd` | Cut specific fields |
| `tr 'a-z' 'A-Z'` | Convert to uppercase |
| `strings binary` | Print readable strings from binary |

---

### PERMISSIONS

| Command | Description |
|---|---|
| `chmod 755 file` | rwxr-xr-x |
| `chmod 644 file` | rw-r--r-- |
| `chmod 600 file` | rw------- (private) |
| `chmod -R 750 dir/` | Recursive |
| `chmod u+x,g-w file` | Symbolic mode |
| `chown alice:group file` | Change owner and group |
| `chown -R alice /home/alice` | Recursive ownership |
| `umask 022` | Default permission mask |
| `getfacl file` | Show ACL |
| `setfacl -m u:bob:rw file` | Add ACL entry |
| `lsattr file` | Show immutable attributes |
| `chattr +i file` | Make file immutable |

---

### USERS & GROUPS

| Command | Description |
|---|---|
| `useradd -m -s /bin/bash user` | Create user with home + bash |
| `passwd user` | Set password |
| `usermod -aG sudo user` | Add user to sudo group |
| `userdel -r user` | Delete user and home |
| `groupadd groupname` | Create group |
| `gpasswd -a user group` | Add user to group |
| `id user` | Show UID, GID, groups |
| `who` | Logged-in users |
| `w` | Logged-in users + activity |
| `last` | Login history |
| `sudo -l` | What can current user sudo |
| `su - user` | Switch to user |
| `chage -l user` | Password expiry info |
| `chage -M 90 user` | Set 90-day password max |

---

### PROCESSES

| Command | Description |
|---|---|
| `ps aux` | All processes (BSD style) |
| `ps -ef` | All processes (UNIX style) |
| `ps auxf` | Process tree |
| `top` | Interactive process viewer |
| `htop` | Enhanced interactive viewer |
| `kill PID` | SIGTERM (graceful) |
| `kill -9 PID` | SIGKILL (force) |
| `killall nginx` | Kill by name |
| `pkill -u alice` | Kill all user's processes |
| `nice -n 10 cmd` | Run with lower priority |
| `renice 5 -p PID` | Change priority |
| `jobs` | Background jobs |
| `fg %1` | Foreground job 1 |
| `bg %1` | Resume job 1 in background |
| `cmd &` | Run in background |
| `nohup cmd &` | Run immune to hangup |
| `disown %1` | Detach from shell |

---

### DISK & STORAGE

| Command | Description |
|---|---|
| `df -h` | Disk free space (human) |
| `df -i` | Inode usage |
| `du -sh *` | Directory sizes summary |
| `du -sh * \| sort -rh` | Sorted by size |
| `lsblk -f` | Block devices with filesystem |
| `fdisk -l` | Partition tables |
| `blkid` | Block device UUIDs |
| `sudo mount /dev/sdb1 /mnt` | Mount device |
| `sudo umount /mnt` | Unmount |
| `sudo mkfs.ext4 /dev/sdb1` | Format as ext4 |
| `sudo fsck -f /dev/sdb1` | Check filesystem |
| `sudo tune2fs -l /dev/sda1` | ext4 filesystem info |
| `sudo lvdisplay` | LVM logical volumes |
| `sudo lvextend -L +10G /dev/vg/lv` | Extend LV |
| `sudo resize2fs /dev/vg/lv` | Resize filesystem |

---

### NETWORKING

| Command | Description |
|---|---|
| `ip addr` | IP addresses |
| `ip route` | Routing table |
| `ip neigh` | ARP table |
| `ss -tulnp` | Listening sockets with PID |
| `ss -s` | Socket statistics |
| `ping -c 4 host` | 4 ICMP pings |
| `traceroute host` | Trace path |
| `mtr host` | Real-time traceroute |
| `dig domain` | DNS lookup |
| `dig +short domain` | Quick DNS lookup |
| `nmap -sV host` | Port scan with version |
| `curl -I URL` | HTTP headers only |
| `curl -L -o file URL` | Download following redirects |
| `wget -c URL` | Continue interrupted download |
| `netstat -r` | Routing table (legacy) |
| `lsof -i :80` | Who's using port 80 |

---

### PACKAGE MANAGEMENT

| Debian/Ubuntu | RHEL/Fedora | Description |
|---|---|---|
| `apt update` | `dnf check-update` | Update package lists |
| `apt upgrade` | `dnf upgrade` | Upgrade packages |
| `apt install pkg` | `dnf install pkg` | Install |
| `apt remove pkg` | `dnf remove pkg` | Remove |
| `apt purge pkg` | `dnf remove pkg` | Remove + config |
| `apt autoremove` | `dnf autoremove` | Remove orphans |
| `apt search name` | `dnf search name` | Search |
| `apt show pkg` | `dnf info pkg` | Package info |
| `dpkg -l` | `rpm -qa` | List installed |
| `dpkg -L pkg` | `rpm -ql pkg` | Files in package |
| `dpkg -S /path` | `rpm -qf /path` | Which package owns file |
| `dpkg -i pkg.deb` | `rpm -ivh pkg.rpm` | Install local file |

---

### SYSTEMD

| Command | Description |
|---|---|
| `systemctl status svc` | Service status |
| `systemctl start svc` | Start |
| `systemctl stop svc` | Stop |
| `systemctl restart svc` | Restart |
| `systemctl reload svc` | Reload config |
| `systemctl enable svc` | Enable at boot |
| `systemctl disable svc` | Disable at boot |
| `systemctl enable --now svc` | Enable + start |
| `systemctl list-units --failed` | Failed units |
| `systemctl list-unit-files` | All unit files |
| `systemctl daemon-reload` | Reload unit files |
| `systemctl mask svc` | Completely prevent starting |
| `journalctl -u svc -f` | Follow service logs |
| `journalctl -p err -b` | Boot errors |
| `journalctl --since "1h ago"` | Last hour |

---

### SSH

| Command | Description |
|---|---|
| `ssh user@host` | Connect |
| `ssh -p 2222 user@host` | Custom port |
| `ssh -i ~/.ssh/key user@host` | Specific key |
| `ssh -J bastion user@internal` | Jump host |
| `ssh -L 8080:localhost:80 user@host` | Local forward |
| `ssh -D 1080 user@host` | SOCKS proxy |
| `ssh-keygen -t ed25519` | Generate Ed25519 key |
| `ssh-copy-id user@host` | Copy key to server |
| `scp -r dir/ user@host:/path/` | Copy directory |
| `rsync -avz src/ user@host:dst/` | Sync files |
| `rsync -avz --delete src/ dst/` | Sync with delete |

---

### ARCHIVING & COMPRESSION

| Command | Description |
|---|---|
| `tar -czvf arch.tar.gz dir/` | Create gzip archive |
| `tar -cjvf arch.tar.bz2 dir/` | Create bzip2 archive |
| `tar -cJvf arch.tar.xz dir/` | Create xz archive |
| `tar -xzvf arch.tar.gz` | Extract gzip |
| `tar -xvf arch.tar -C /dest/` | Extract to directory |
| `tar -tvf arch.tar` | List contents |
| `tar --exclude="*.log" -czvf a.tgz d/` | Exclude pattern |
| `gzip file` | Compress (gzip) |
| `gunzip file.gz` | Decompress |
| `zip -r arch.zip dir/` | Create zip |
| `unzip arch.zip -d /dest/` | Extract zip |
| `dd if=/dev/sda of=disk.img bs=4M status=progress` | Disk image |

---

### FIREWALL — UFW

| Command | Description |
|---|---|
| `ufw status verbose` | Status |
| `ufw enable` | Enable |
| `ufw default deny incoming` | Default deny |
| `ufw allow ssh` | Allow SSH |
| `ufw allow 80/tcp` | Allow HTTP |
| `ufw allow from 192.168.1.0/24` | Allow subnet |
| `ufw deny 23/tcp` | Deny telnet |
| `ufw status numbered` | Show rule numbers |
| `ufw delete 3` | Delete rule 3 |
| `ufw logging on` | Enable logging |

---

### PERFORMANCE MONITORING

| Command | Description |
|---|---|
| `top` | Process viewer |
| `htop` | Enhanced process viewer |
| `vmstat 2` | VM stats every 2 seconds |
| `iostat -x 2` | Extended I/O stats |
| `free -h` | Memory usage |
| `sar -u 2 5` | CPU utilization |
| `sar -r 2 5` | Memory utilization |
| `sar -n DEV 2 5` | Network stats |
| `iotop` | Per-process I/O |
| `nethogs` | Per-process network |
| `atop` | Advanced system monitor |
| `perf top` | System-wide performance |
| `watch -n 1 'ss -s'` | Watch socket stats |

---

### KERNEL & SYSTEM

| Command | Description |
|---|---|
| `uname -r` | Kernel version |
| `uname -a` | All system info |
| `lscpu` | CPU info |
| `lsmem` | Memory info |
| `lshw -short` | Hardware summary |
| `lspci` | PCI devices |
| `lsusb` | USB devices |
| `dmesg -T \| tail` | Recent kernel messages |
| `sysctl -a` | All kernel parameters |
| `sysctl -w vm.swappiness=10` | Set kernel param |
| `lsmod` | Loaded kernel modules |
| `modprobe module` | Load module |
| `modprobe -r module` | Remove module |
| `cat /proc/meminfo` | Memory details |
| `cat /proc/cpuinfo` | CPU details |

---

### CRON REFERENCE

```
┌─────────── minute (0-59)
│ ┌───────── hour (0-23)
│ │ ┌─────── day of month (1-31)
│ │ │ ┌───── month (1-12)
│ │ │ │ ┌─── day of week (0-7, Sun=0 or 7)
│ │ │ │ │
* * * * *  command

*/5 * * * *         Every 5 minutes
0 */2 * * *         Every 2 hours
30 2 * * *          2:30 AM daily
0 9 * * 1           9 AM every Monday
0 0 1 * *           Midnight, 1st of month
0 0 1 1 *           Midnight, January 1st
@reboot             At system boot
@daily              Once a day
@weekly             Once a week
@monthly            Once a month
```

---

### SPECIAL CHARACTERS REFERENCE

| Character | Meaning |
|---|---|
| `~` | Home directory |
| `.` | Current directory |
| `..` | Parent directory |
| `/` | Root directory / path separator |
| `*` | Wildcard: any characters |
| `?` | Wildcard: single character |
| `[abc]` | Character class |
| `{a,b}` | Brace expansion |
| `$var` | Variable expansion |
| `$(cmd)` | Command substitution |
| `` `cmd` `` | Command substitution (legacy) |
| `>` | Redirect stdout (overwrite) |
| `>>` | Redirect stdout (append) |
| `2>` | Redirect stderr |
| `2>&1` | Redirect stderr to stdout |
| `\|` | Pipe |
| `&&` | Run if previous succeeded |
| `\|\|` | Run if previous failed |
| `;` | Run sequentially |
| `!` | Negate or history |
| `#` | Comment in script |
| `\` | Escape character / line continuation |
| `"..."` | Preserve spaces, expand variables |
| `'...'` | Literal (no expansion) |
| `&` | Run in background |

---

### EXIT CODE REFERENCE

| Code | Meaning |
|---|---|
| `0` | Success |
| `1` | General error |
| `2` | Misuse of shell builtins |
| `126` | Permission denied (command not executable) |
| `127` | Command not found |
| `128` | Invalid argument to exit |
| `128+N` | Fatal error signal N (e.g., 130 = Ctrl+C = SIGINT) |
| `130` | Killed with Ctrl+C (SIGINT, signal 2) |
| `137` | Killed with kill -9 (SIGKILL, signal 9) |
| `255` | Exit status out of range |

```bash
$?      # Exit code of last command
echo $? # Print exit code
```

---

### IMPORTANT FILES REFERENCE

| File | Purpose |
|---|---|
| `/etc/passwd` | User accounts |
| `/etc/shadow` | Encrypted passwords |
| `/etc/group` | Group definitions |
| `/etc/sudoers` | Sudo configuration |
| `/etc/fstab` | Filesystem mount table |
| `/etc/hosts` | Static hostname resolution |
| `/etc/resolv.conf` | DNS server configuration |
| `/etc/hostname` | System hostname |
| `/etc/os-release` | OS name/version |
| `/etc/crontab` | System-wide cron |
| `/etc/ssh/sshd_config` | SSH server config |
| `/etc/ssh/ssh_config` | SSH client config |
| `/etc/nsswitch.conf` | Name service switch |
| `/etc/environment` | System-wide environment vars |
| `/etc/profile` | Login shell initialization |
| `/etc/sysctl.conf` | Kernel parameter config |
| `/etc/modules-load.d/` | Modules to load at boot |
| `/etc/modprobe.d/` | Module options/blacklists |
| `/etc/systemd/system/` | Custom systemd units |
| `/etc/logrotate.conf` | Log rotation config |
| `/proc/sys/` | Kernel parameter interface |
| `/proc/meminfo` | Memory information |
| `/proc/cpuinfo` | CPU information |
| `/proc/net/` | Network information |
| `~/.bashrc` | Bash config (interactive) |
| `~/.bash_profile` | Bash login config |
| `~/.ssh/config` | SSH client config |
| `~/.ssh/authorized_keys` | Authorized SSH keys |

---

### GREP QUICK REFERENCE

```bash
grep "word"          # Basic search
grep -i "word"       # Case insensitive
grep -r "word" dir/  # Recursive
grep -v "word"       # Invert (non-matching)
grep -n "word"       # With line numbers
grep -c "word"       # Count matches
grep -l "word" *     # Filenames only
grep -w "word"       # Whole word
grep -A 3 "word"     # 3 lines after
grep -B 3 "word"     # 3 lines before
grep -C 3 "word"     # 3 lines context
grep -o "word"       # Only matching part
grep -E "a|b"        # Extended regex (OR)
grep -P "\d+"        # Perl regex
grep "^start"        # Lines starting with
grep "end$"          # Lines ending with
grep "^$"            # Empty lines
grep -v "^$"         # Non-empty lines
grep -v "^#"         # Non-comment lines
```

---

### SED QUICK REFERENCE

```bash
sed 's/old/new/'        # Replace first per line
sed 's/old/new/g'       # Replace all
sed 's/old/new/gi'      # Case-insensitive global
sed -i 's/a/b/g' file   # In-place edit
sed -i.bak 's/a/b/g'    # In-place + backup
sed '5d'                # Delete line 5
sed '5,10d'             # Delete lines 5-10
sed '/pattern/d'        # Delete matching lines
sed '/^$/d'             # Delete empty lines
sed '/^#/d'             # Delete comments
sed -n '5p'             # Print line 5
sed -n '5,10p'          # Print lines 5-10
sed '5i\new line'       # Insert before line 5
sed '5a\new line'       # Append after line 5
sed 's/\s*$//'          # Remove trailing spaces
sed 's/^/  /'           # Add indent
sed 's/  */,/g'         # Replace spaces with comma
```

---

### AWK QUICK REFERENCE

```bash
awk '{print $1}'              # Print field 1
awk '{print $NF}'             # Print last field
awk '{print NR, $0}'          # With line numbers
awk 'NR==5'                   # Print line 5
awk 'NR>=5 && NR<=10'         # Lines 5-10
awk '/pattern/'               # Lines matching pattern
awk '!/pattern/'              # Lines NOT matching
awk -F: '{print $1}'          # Custom delimiter
awk '{sum+=$1}END{print sum}' # Sum column
awk '{print $2,$1}'           # Reorder fields
awk 'NF>3'                    # Lines with >3 fields
awk 'length($0)>80'           # Lines longer than 80
awk '$3>100{print $1}'        # Conditional print
awk 'BEGIN{FS=":"}NR>1'       # Skip header
awk '{printf "%-10s %5d\n",$1,$2}' # Formatted output
awk 'END{print NR}'           # Count lines
awk '!seen[$0]++'             # Remove duplicates
```

---

### KEYBOARD SHORTCUTS

```bash
Ctrl+A    Move to start of line
Ctrl+E    Move to end of line
Ctrl+K    Cut to end of line
Ctrl+U    Cut to start of line
Ctrl+Y    Paste
Ctrl+W    Cut previous word
Ctrl+R    Reverse history search
Ctrl+L    Clear screen
Ctrl+C    Interrupt/kill process
Ctrl+Z    Suspend process
Ctrl+D    EOF/logout
Alt+.     Last argument of previous command
!!        Repeat last command
!$        Last argument
!string   Last command starting with string
!?string  Last command containing string
```

---

### NETWORKING QUICK COMMANDS

```bash
# Find your IPs
hostname -I
ip addr show | grep "inet "

# Find default gateway
ip route show default
route -n

# DNS servers
cat /etc/resolv.conf

# Who's listening on what port
ss -tulnp
netstat -tulnp

# See all TCP connections
ss -tnp

# Test port connectivity
nc -zv hostname 80
telnet hostname 80
curl -s telnet://hostname:80

# Check firewall
iptables -L -n -v --line-numbers
ufw status numbered

# Capture traffic
sudo tcpdump -i eth0 -n port 80
sudo tcpdump -i any -w /tmp/capture.pcap

# Quick HTTP server for sharing files
python3 -m http.server 8080

# Check SSL certificate
openssl s_client -connect google.com:443
openssl s_client -connect host:443 </dev/null | openssl x509 -noout -dates
```

---

### PERFORMANCE ONE-LINERS

```bash
# Top 10 memory-consuming processes
ps aux --sort=-%mem | head -11

# Top 10 CPU-consuming processes
ps aux --sort=-%cpu | head -11

# Largest directories in /var
du -sh /var/*/ 2>/dev/null | sort -rh | head -10

# Largest files in filesystem
find / -type f -size +100M -exec ls -lh {} \; 2>/dev/null | sort -k5 -rh

# Open files by process
lsof -p PID

# Count connections by state
ss -tan | awk '{print $1}' | sort | uniq -c | sort -rn

# Who's listening and on what
ss -tulnp | column -t

# Check if port is open on remote host
timeout 3 bash -c "</dev/tcp/hostname/80" && echo "Open" || echo "Closed"

# Watch disk I/O
watch -n 1 'iostat -x | tail -20'

# Memory usage trend
while true; do free | awk '/Mem/{print strftime("%H:%M:%S"), $3/$2*100"%"}'; sleep 5; done
```

---

### SECURITY ONE-LINERS

```bash
# Find SUID files
find / -perm -4000 -type f 2>/dev/null

# Find SGID files
find / -perm -2000 -type f 2>/dev/null

# Find world-writable files
find / -perm -0002 -type f 2>/dev/null

# Find files with no owner
find / -nouser 2>/dev/null

# List all listening ports
ss -tulnp

# Failed SSH login attempts
grep "Failed password" /var/log/auth.log | tail -20

# Successful SSH logins
grep "Accepted" /var/log/auth.log | tail -20

# Last 10 logins
last -10

# Check sudoers
cat /etc/sudoers | grep -v "^#" | grep -v "^$"

# Active network connections
ss -tnp state established

# Check for rootkits (need rkhunter)
sudo rkhunter --check --skip-keypress
```

---

### BASH SCRIPTING TEMPLATES

```bash
#!/bin/bash
# === Script Header Template ===
set -euo pipefail
readonly SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
readonly SCRIPT_NAME="$(basename "${BASH_SOURCE[0]}")"
readonly LOG_FILE="/var/log/${SCRIPT_NAME%.sh}.log"

log()  { echo "[$(date '+%Y-%m-%d %H:%M:%S')] INFO:  $*" | tee -a "$LOG_FILE"; }
warn() { echo "[$(date '+%Y-%m-%d %H:%M:%S')] WARN:  $*" | tee -a "$LOG_FILE" >&2; }
die()  { echo "[$(date '+%Y-%m-%d %H:%M:%S')] ERROR: $*" | tee -a "$LOG_FILE" >&2; exit 1; }

cleanup() { log "Cleanup called"; rm -f /tmp/tempfile_$$; }
trap cleanup EXIT
trap 'die "Error on line $LINENO"' ERR

# === Check root ===
[[ $EUID -eq 0 ]] || die "Must run as root"

# === Check dependencies ===
for cmd in curl jq awk; do
    command -v "$cmd" &>/dev/null || die "Required command not found: $cmd"
done

# === Main ===
main() {
    log "Starting $SCRIPT_NAME"
    # ... your code here ...
    log "Done"
}

main "$@"
```

---

### SYSTEM RESCUE COMMANDS

```bash
# Boot to bash without password (emergency)
# At GRUB: edit linux line, add init=/bin/bash
mount -o remount,rw /          # Make root read-write
passwd root                     # Reset root password
exec /sbin/init                 # Start normal boot

# Fix broken apt
sudo dpkg --configure -a
sudo apt install -f
sudo apt clean && sudo apt update

# Recover deleted file (if still open by process)
lsof | grep deleted            # Find deleted-but-open files
cp /proc/PID/fd/FD /recovered_file  # Recover via /proc

# Reset root password from live CD
mount /dev/sda1 /mnt
for d in dev proc sys run; do
    mount --bind /$d /mnt/$d
done
chroot /mnt
passwd root
exit
for d in dev proc sys run; do umount /mnt/$d; done
umount /mnt
reboot

# Fix read-only filesystem
mount -o remount,rw /
dmesg | grep -i "i/o error"    # Check for hardware errors
fsck /dev/sda1                 # After unmounting
```

---

*Last updated for Linux Kernel 6.x | GNU/Linux Reference Guide*
*Created for System Administrators, DevOps Engineers, and Developers*
