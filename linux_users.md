# 🐧 Linux Notes — From Beginner to Intermediate

> A complete, hands-on guide to using Linux as a normal user. No prior experience needed.

---

## Table of Contents

1. [What is Linux?](#1-what-is-linux)
2. [How Linux is Structured](#2-how-linux-is-structured)
3. [The Terminal — Your Best Friend](#3-the-terminal--your-best-friend)
4. [Navigating the File System](#4-navigating-the-file-system)
5. [Working with Files and Directories](#5-working-with-files-and-directories)
6. [Viewing and Editing Files](#6-viewing-and-editing-files)
7. [Users, Groups and Permissions](#7-users-groups-and-permissions)
8. [Installing and Managing Software](#8-installing-and-managing-software)
9. [Processes and System Monitoring](#9-processes-and-system-monitoring)
10. [Networking Basics](#10-networking-basics)
11. [Pipes, Redirection and Filters](#11-pipes-redirection-and-filters)
12. [Environment Variables and Shell Config](#12-environment-variables-and-shell-config)
13. [Shell Scripting Basics](#13-shell-scripting-basics)
14. [Archiving and Compression](#14-archiving-and-compression)
15. [Finding Files](#15-finding-files)
16. [Disk and Storage Management](#16-disk-and-storage-management)
17. [Useful Everyday Commands](#17-useful-everyday-commands)
18. [Linux Cheat Sheet](#18-linux-cheat-sheet)

---

## 1. What is Linux?

Linux is a **free, open-source operating system kernel** created by Linus Torvalds in 1991. It powers everything from your Android phone to web servers, supercomputers, and even smart TVs.

What most people call "Linux" is actually a **Linux distribution (distro)** — a complete OS package built around the Linux kernel. Popular distros include:

| Distro | Best For |
|--------|----------|
| Ubuntu | Beginners, general use |
| Fedora | Developers, cutting-edge packages |
| Debian | Stability, servers |
| Arch Linux | Advanced users who want full control |
| Linux Mint | Windows switchers |
| Kali Linux | Security and penetration testing |

### Why Learn Linux?

- It's used on **90%+ of servers** on the internet
- It's **free** and **open source**
- It's **stable**, **secure**, and **fast**
- It gives you **full control** over your machine
- It's essential for **development, DevOps, and cloud** careers

---

## 2. How Linux is Structured

### The Kernel

The kernel is the **core** of Linux. It talks to your hardware — CPU, RAM, disk — and lets software use those resources. You never interact with the kernel directly; the shell does that for you.

### The Shell

The shell is a **program that takes your commands** and sends them to the kernel. Think of it as a translator between you and the machine. The most popular shell is **Bash** (Bourne Again Shell).

### The File System Hierarchy

Linux organizes everything in a single tree starting from `/` (called "root"). There are no C: or D: drives like Windows.

```
/                   ← Root of the entire file system
├── bin/            ← Essential commands (ls, cp, mv, etc.)
├── boot/           ← Files needed to boot the system
├── dev/            ← Device files (hard drives, USB, etc.)
├── etc/            ← System-wide configuration files
├── home/           ← User home directories (/home/john, /home/sara)
├── lib/            ← Libraries needed by programs
├── media/          ← Mounted drives (USB, CD)
├── mnt/            ← Temporary mount points
├── opt/            ← Optional/third-party software
├── proc/           ← Virtual filesystem (running processes info)
├── root/           ← Home directory of the root (admin) user
├── sbin/           ← System admin commands
├── tmp/            ← Temporary files (cleared on reboot)
├── usr/            ← User programs, documentation
└── var/            ← Variable data: logs, cache, mail
```

**Key rule:** In Linux, **everything is a file** — even your mouse, keyboard, and network card appear as files in `/dev/`.

---

## 3. The Terminal — Your Best Friend

The **terminal** (also called the command line or console) is where you type commands. On a desktop Linux, you can open it by pressing `Ctrl + Alt + T` or searching for "Terminal" in your apps.

### The Prompt

When you open a terminal, you see something like this:

```
john@ubuntu:~$
```

Breaking it down:
- `john` → your username
- `ubuntu` → your computer's hostname (name)
- `~` → your current directory (`~` means home directory)
- `$` → means you are a regular user (not root)
- `#` → means you are root (admin/superuser)

### Running Your First Commands

```bash
whoami          # prints your username
date            # shows current date and time
cal             # shows a calendar
uptime          # how long the system has been running
hostname        # shows your machine's name
echo "Hello"    # prints Hello to the screen
clear           # clears the terminal screen (Ctrl+L also works)
```

### Getting Help

```bash
man ls          # opens the manual page for 'ls' command
ls --help       # shows quick help for 'ls'
info ls         # detailed info (alternative to man)
whatis ls       # one-line description of a command
```

**Tip:** In `man` pages, press `q` to quit, arrow keys or `j/k` to scroll, and `/word` to search.

---

## 4. Navigating the File System

### Essential Navigation Commands

```bash
pwd             # Print Working Directory — where am I right now?
ls              # List files in current directory
ls -l           # Long listing (shows permissions, size, date)
ls -a           # Show hidden files too (files starting with .)
ls -la          # Combine both: long listing + hidden files
ls -lh          # Human-readable file sizes (KB, MB, GB)
cd /home        # Change directory to /home
cd ~            # Go to your home directory
cd ..           # Go up one level (parent directory)
cd -            # Go back to previous directory
cd /            # Go to root directory
```

### Understanding Paths

There are two types of paths:

**Absolute path** — starts from root `/`, works from anywhere:
```bash
cd /home/john/Documents
```

**Relative path** — starts from where you currently are:
```bash
cd Documents       # go into Documents (inside current dir)
cd ../Downloads    # go up one level, then into Downloads
```

### Special Symbols

| Symbol | Meaning |
|--------|---------|
| `~` | Your home directory (`/home/username`) |
| `.` | Current directory |
| `..` | Parent directory |
| `/` | Root directory |
| `-` | Previous directory (used with `cd`) |

---

## 5. Working with Files and Directories

### Creating Files and Directories

```bash
touch file.txt          # Create an empty file
touch a.txt b.txt c.txt # Create multiple files at once
mkdir myfolder          # Create a directory
mkdir -p a/b/c          # Create nested directories all at once
```

### Copying Files and Directories

```bash
cp file.txt backup.txt           # Copy file to backup.txt
cp file.txt /home/john/          # Copy to another directory
cp -r folder1/ folder2/          # Copy entire directory (-r = recursive)
cp -i file.txt dest.txt          # Ask before overwriting (-i = interactive)
```

### Moving and Renaming

```bash
mv file.txt newname.txt          # Rename a file
mv file.txt /home/john/          # Move file to another directory
mv folder1/ /tmp/                # Move entire folder
```

> **Note:** `mv` is both rename and move in Linux. If the destination is in the same directory, it renames. If it's a different path, it moves.

### Deleting Files and Directories

```bash
rm file.txt              # Delete a file
rm -i file.txt           # Ask before deleting
rm -r folder/            # Delete a folder and everything inside it
rm -rf folder/           # Force delete without asking (BE CAREFUL!)
rmdir emptyfolder/       # Delete an empty directory
```

> ⚠️ **Warning:** Linux has no Recycle Bin in the terminal. Once deleted with `rm`, it's gone. Always double-check before using `rm -rf`.

### Creating Symbolic Links (Shortcuts)

```bash
ln -s /original/path /link/path   # Create a soft/symbolic link
```

---

## 6. Viewing and Editing Files

### Viewing File Contents

```bash
cat file.txt            # Print entire file to screen
cat -n file.txt         # Print with line numbers
less file.txt           # View file page by page (press q to quit)
more file.txt           # Similar to less, older tool
head file.txt           # Show first 10 lines
head -n 20 file.txt     # Show first 20 lines
tail file.txt           # Show last 10 lines
tail -n 20 file.txt     # Show last 20 lines
tail -f logfile.log     # Follow a file live (great for logs!)
```

### Searching Inside Files

```bash
grep "error" logfile.txt         # Find lines containing "error"
grep -i "error" logfile.txt      # Case-insensitive search
grep -r "TODO" ~/projects/       # Search recursively in a folder
grep -n "error" file.txt         # Show line numbers
grep -v "error" file.txt         # Show lines that do NOT contain "error"
grep -c "error" file.txt         # Count matching lines
```

### Text Editors in Terminal

**nano** — Simple and beginner-friendly:
```bash
nano file.txt
```
Key shortcuts in nano:
- `Ctrl+O` → Save
- `Ctrl+X` → Exit
- `Ctrl+W` → Search
- `Ctrl+K` → Cut line
- `Ctrl+U` → Paste line

**vim** — Powerful but has a learning curve:
```bash
vim file.txt
```
Basic vim workflow:
- Open file with `vim filename`
- Press `i` to enter Insert mode (now you can type)
- Press `Esc` to go back to Normal mode
- Type `:w` and Enter to save
- Type `:q` and Enter to quit
- Type `:wq` to save and quit
- Type `:q!` to quit without saving

**VS Code (GUI)** — If you have VS Code installed:
```bash
code file.txt
```

---

## 7. Users, Groups and Permissions

### Understanding Users

Linux is a **multi-user operating system**. There are three types of users:

- **Root (superuser)** — Has complete control over the system. Username is `root`.
- **Regular users** — Normal accounts with limited permissions.
- **System users** — Used by services (like `www-data` for Apache web server).

```bash
whoami              # who are you right now?
id                  # shows your user ID, group ID, and all groups
cat /etc/passwd     # list of all users on the system
cat /etc/group      # list of all groups
```

### sudo — Run Commands as Root

Instead of logging in as root, you use `sudo` (super user do) to run a single command with admin privileges:

```bash
sudo apt update          # run 'apt update' as root
sudo -i                  # open a root shell (be careful!)
su - username            # switch to another user
```

### File Permissions

Every file in Linux has three sets of permissions:

```
-rwxr-xr--  1  john  staff  4096  Jun 1  file.txt
│└──┴──┴──┘     │      │
│  │  │  │      │      └─ group
│  │  │  │      └─ owner
│  │  │  └─ others (everyone else)
│  │  └─ group permissions
│  └─ owner permissions
└─ file type (- = file, d = directory, l = symlink)
```

Each permission group has three characters:

| Character | Meaning | Value |
|-----------|---------|-------|
| `r` | Read | 4 |
| `w` | Write | 2 |
| `x` | Execute | 1 |
| `-` | No permission | 0 |

So `rwxr-xr--` means:
- **Owner (john):** rwx = read, write, execute (7)
- **Group (staff):** r-x = read, execute (5)
- **Others:** r-- = read only (4)

### Changing Permissions with chmod

```bash
chmod 755 script.sh       # rwxr-xr-x (owner: full, group: r+x, others: r+x)
chmod 644 file.txt        # rw-r--r-- (owner: r+w, group: r, others: r)
chmod 600 secret.txt      # rw------- (only owner can read/write)
chmod 777 file.txt        # rwxrwxrwx (everyone has full access — avoid this)
chmod +x script.sh        # Add execute permission for everyone
chmod -w file.txt         # Remove write permission
chmod u+x script.sh       # Add execute only for the owner (u = user/owner)
chmod g+w file.txt        # Add write for the group
chmod o-r file.txt        # Remove read from others
```

**Quick permission cheat:**
- `755` → standard for executables and directories
- `644` → standard for regular files
- `600` → private files (SSH keys, secrets)
- `700` → private directories

### Changing Ownership with chown

```bash
chown john file.txt            # Change owner to john
chown john:staff file.txt      # Change owner to john, group to staff
chown -R john folder/          # Change recursively (all files in folder)
sudo chown root file.txt       # Only root can give ownership to root
```

---

## 8. Installing and Managing Software

### Package Managers

Linux uses package managers to install, update, and remove software. The package manager depends on your distro:

| Distro | Package Manager | Command Prefix |
|--------|----------------|----------------|
| Ubuntu/Debian | APT | `apt` |
| Fedora/RHEL | DNF/YUM | `dnf` or `yum` |
| Arch Linux | Pacman | `pacman` |

### APT (Ubuntu/Debian)

```bash
sudo apt update                  # Refresh package list from internet
sudo apt upgrade                 # Upgrade all installed packages
sudo apt install firefox         # Install a package
sudo apt install vim git curl    # Install multiple packages at once
sudo apt remove firefox          # Remove a package (keeps config files)
sudo apt purge firefox           # Remove package + all its config files
sudo apt autoremove              # Remove packages no longer needed
sudo apt search vim              # Search for a package by name
apt show vim                     # Show info about a package
dpkg -l                          # List all installed packages
dpkg -l | grep vim               # Check if vim is installed
```

### Installing from a .deb File (like an .exe for Ubuntu)

```bash
sudo dpkg -i package.deb         # Install downloaded .deb file
sudo apt install -f              # Fix any dependency issues
```

### Snap Packages (Universal, Sandboxed)

```bash
sudo snap install vlc            # Install VLC via snap
snap list                        # List installed snaps
sudo snap remove vlc             # Remove a snap
```

### Flatpak (Another Universal Format)

```bash
flatpak install flathub org.gimp.GIMP   # Install GIMP from Flathub
flatpak list                             # List installed flatpaks
flatpak update                           # Update all flatpaks
```

---

## 9. Processes and System Monitoring

### Viewing Running Processes

```bash
ps                    # Show processes in current terminal
ps aux                # Show ALL processes from ALL users
ps aux | grep firefox # Find the firefox process
top                   # Live process monitor (press q to quit)
htop                  # Better version of top (install: sudo apt install htop)
```

### Understanding top/htop

When you run `top`, you see:
- **PID** — Process ID (unique number for each process)
- **USER** — Who owns the process
- **%CPU** — CPU usage percentage
- **%MEM** — Memory usage percentage
- **COMMAND** — The program name

### Managing Processes

```bash
kill 1234              # Kill a process by PID
kill -9 1234           # Force kill (when normal kill doesn't work)
killall firefox        # Kill all processes named "firefox"
pkill firefox          # Similar to killall

# Running in background
sleep 100 &            # Run in background (& at the end)
jobs                   # List background jobs
fg                     # Bring background job to foreground
bg                     # Send a paused job to background
Ctrl+C                 # Stop a running program
Ctrl+Z                 # Pause (suspend) a running program
```

### System Resource Information

```bash
free -h                 # Show RAM usage (human readable)
df -h                   # Show disk space usage
df -h /                 # Show disk usage of root partition
du -sh folder/          # Show size of a folder
du -sh *                # Show size of each item in current dir
lscpu                   # CPU information
lsblk                   # List block devices (disks, partitions)
uname -a                # Kernel version and system info
cat /proc/meminfo       # Detailed memory information
```

### System Logs

```bash
journalctl               # View system logs (systemd)
journalctl -f            # Follow logs live
journalctl -u nginx      # Logs for a specific service
sudo dmesg               # Kernel messages (hardware events)
cat /var/log/syslog      # System log file (Ubuntu/Debian)
```

### Managing Services (systemd)

Most modern Linux systems use **systemd** to manage services (background programs):

```bash
sudo systemctl start nginx      # Start a service
sudo systemctl stop nginx       # Stop a service
sudo systemctl restart nginx    # Restart a service
sudo systemctl status nginx     # Check if service is running
sudo systemctl enable nginx     # Start automatically at boot
sudo systemctl disable nginx    # Don't start at boot
systemctl list-units --type=service   # List all services
```

---

## 10. Networking Basics

### Checking Network Information

```bash
ip addr                  # Show IP addresses (modern way)
ip addr show eth0        # Show info for specific interface
ifconfig                 # Old way (install: sudo apt install net-tools)
hostname -I              # Quick way to see your IP
ip route                 # Show routing table (shows default gateway)
cat /etc/resolv.conf     # See DNS servers
```

### Testing Connectivity

```bash
ping google.com           # Test internet connectivity
ping -c 4 google.com      # Send only 4 packets
traceroute google.com     # Trace path to a host
nslookup google.com       # DNS lookup
dig google.com            # Detailed DNS query
curl https://example.com  # Fetch a webpage
wget https://example.com/file.zip   # Download a file
```

### SSH — Secure Remote Access

```bash
ssh john@192.168.1.10        # Connect to a remote machine
ssh -p 2222 john@server.com  # Connect on a different port
ssh-keygen                   # Generate SSH key pair
ssh-copy-id john@server.com  # Copy your public key to server (passwordless login)
scp file.txt john@server.com:/home/john/    # Copy file to remote machine
scp john@server.com:/file.txt .             # Copy file from remote machine
```

### Firewall Basics (UFW)

UFW = Uncomplicated Firewall, the easy way to manage firewall rules on Ubuntu:

```bash
sudo ufw status              # Check firewall status
sudo ufw enable              # Turn on firewall
sudo ufw disable             # Turn off firewall
sudo ufw allow ssh           # Allow SSH connections
sudo ufw allow 80/tcp        # Allow HTTP traffic
sudo ufw allow 443/tcp       # Allow HTTPS traffic
sudo ufw deny 23             # Block Telnet
sudo ufw delete allow 80/tcp # Remove a rule
```

---

## 11. Pipes, Redirection and Filters

This is where Linux becomes incredibly powerful. You can connect commands together like building blocks.

### Output Redirection

```bash
echo "Hello" > file.txt        # Write to file (overwrites!)
echo "World" >> file.txt       # Append to file (adds to end)
ls /nonexistent 2> error.txt   # Redirect error output to file
ls > output.txt 2> error.txt   # Redirect both normal and error output
ls > output.txt 2>&1           # Redirect everything to one file
```

### Input Redirection

```bash
sort < names.txt               # Read input from file instead of keyboard
```

### Pipes — Connecting Commands

The `|` (pipe) symbol sends the output of one command as input to the next:

```bash
ls -l | less                   # View long listing page by page
ps aux | grep firefox          # Find firefox process
cat file.txt | wc -l           # Count lines in a file
ls | sort                      # List files in sorted order
ls | sort -r                   # List files in reverse sorted order
cat /etc/passwd | cut -d: -f1  # Get just usernames from passwd file
history | tail -20             # Show last 20 commands
```

### Useful Filter Commands

```bash
# wc — Word Count
wc file.txt          # Lines, words, characters
wc -l file.txt       # Just line count
wc -w file.txt       # Just word count

# sort — Sort lines
sort file.txt        # Sort alphabetically
sort -n file.txt     # Sort numerically
sort -r file.txt     # Reverse sort
sort -u file.txt     # Sort and remove duplicates

# uniq — Remove duplicates (file must be sorted first)
sort file.txt | uniq           # Remove duplicate lines
sort file.txt | uniq -c        # Count occurrences of each line

# cut — Extract columns
cut -d, -f1 data.csv           # Get first column of CSV file
cut -d: -f1,3 /etc/passwd      # Get columns 1 and 3 (colon-separated)

# tr — Translate/replace characters
echo "hello" | tr 'a-z' 'A-Z'  # Convert to uppercase
cat file.txt | tr -d '\n'       # Remove newlines

# awk — Powerful text processing
awk '{print $1}' file.txt       # Print first word of each line
awk -F: '{print $1}' /etc/passwd  # Print first field (colon-separated)

# sed — Stream editor
sed 's/old/new/' file.txt       # Replace first occurrence on each line
sed 's/old/new/g' file.txt      # Replace all occurrences
sed -i 's/old/new/g' file.txt   # Edit file in place
sed '2d' file.txt               # Delete line 2
```

### Chaining with && and ||

```bash
mkdir project && cd project     # Run second command only if first succeeds
cat file.txt || echo "File not found"  # Run second only if first fails
```

---

## 12. Environment Variables and Shell Config

### What are Environment Variables?

Environment variables are **named values** that programs can read. They store things like your home directory, preferred editor, system PATH, etc.

```bash
echo $HOME          # Your home directory
echo $USER          # Your username
echo $PATH          # Directories where Linux looks for commands
echo $SHELL         # Your current shell
echo $PWD           # Current working directory
env                 # Show all environment variables
printenv HOME       # Print a specific variable
```

### Setting Variables

```bash
MY_NAME="John"          # Set a variable (only in current session)
export MY_NAME="John"   # Export it so child processes can see it
echo $MY_NAME           # Use it

unset MY_NAME           # Remove a variable
```

### Making Variables Permanent — Shell Config Files

Variables set in the terminal are lost when you close it. To make them permanent, add them to your shell config file:

- **Bash:** `~/.bashrc` (runs for each new terminal session)
- **Bash:** `~/.bash_profile` (runs at login)
- **Zsh:** `~/.zshrc`

```bash
nano ~/.bashrc
```

Add a line like:
```bash
export MY_VAR="hello"
alias ll='ls -la'
```

Then reload the config:
```bash
source ~/.bashrc
# or
. ~/.bashrc
```

### Aliases — Create Your Own Shortcuts

```bash
alias ll='ls -la'           # Define in terminal (temporary)
alias update='sudo apt update && sudo apt upgrade'
alias cls='clear'

# Add these to ~/.bashrc to make them permanent
```

### PATH — Where Linux Finds Commands

When you type a command, Linux searches directories listed in `$PATH`:

```bash
echo $PATH
# /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin

# Add a new directory to PATH:
export PATH="$PATH:/home/john/scripts"
# Add this line to ~/.bashrc to make it permanent
```

---

## 13. Shell Scripting Basics

A shell script is a **text file containing commands** that run in sequence. It's a way to automate repetitive tasks.

### Your First Script

```bash
nano hello.sh
```

```bash
#!/bin/bash
# This is a comment
# #!/bin/bash is called the shebang — tells the system which shell to use

echo "Hello, World!"
echo "Today is: $(date)"
echo "You are: $USER"
```

Make it executable and run it:
```bash
chmod +x hello.sh
./hello.sh
```

### Variables in Scripts

```bash
#!/bin/bash
NAME="Linux"
echo "I am learning $NAME"

# Read user input
echo "Enter your name:"
read USER_NAME
echo "Hello, $USER_NAME!"
```

### If Statements

```bash
#!/bin/bash
AGE=20

if [ $AGE -ge 18 ]; then
    echo "You are an adult"
elif [ $AGE -ge 13 ]; then
    echo "You are a teenager"
else
    echo "You are a child"
fi
```

Comparison operators for numbers:
| Operator | Meaning |
|----------|---------|
| `-eq` | equal |
| `-ne` | not equal |
| `-gt` | greater than |
| `-lt` | less than |
| `-ge` | greater than or equal |
| `-le` | less than or equal |

For strings:
```bash
if [ "$NAME" = "Linux" ]; then echo "yes"; fi
if [ "$NAME" != "Windows" ]; then echo "not windows"; fi
if [ -z "$NAME" ]; then echo "string is empty"; fi
if [ -n "$NAME" ]; then echo "string is not empty"; fi
```

For files:
```bash
if [ -f "file.txt" ]; then echo "file exists"; fi
if [ -d "folder" ]; then echo "directory exists"; fi
if [ -e "path" ]; then echo "path exists (file or dir)"; fi
```

### Loops

**For loop:**
```bash
#!/bin/bash
for i in 1 2 3 4 5; do
    echo "Number: $i"
done

# Loop over files
for file in *.txt; do
    echo "Processing: $file"
done
```

**While loop:**
```bash
#!/bin/bash
COUNT=1
while [ $COUNT -le 5 ]; do
    echo "Count: $COUNT"
    COUNT=$((COUNT + 1))
done
```

### Functions

```bash
#!/bin/bash
greet() {
    echo "Hello, $1!"   # $1 is the first argument passed to the function
}

greet "World"
greet "Linux"
```

### Script Arguments

When you call a script with arguments:
```bash
./myscript.sh one two three
```

Inside the script:
```bash
echo $0    # Script name
echo $1    # First argument: "one"
echo $2    # Second argument: "two"
echo $3    # Third argument: "three"
echo $#    # Number of arguments: 3
echo $@    # All arguments: "one two three"
```

---

## 14. Archiving and Compression

### tar — The Swiss Army Knife of Archiving

`tar` creates and extracts archives (.tar files).

```bash
# Create archives
tar -cvf archive.tar folder/            # Create archive (no compression)
tar -czvf archive.tar.gz folder/        # Create with gzip compression (.tar.gz)
tar -cjvf archive.tar.bz2 folder/       # Create with bzip2 compression

# Extract archives
tar -xvf archive.tar                    # Extract
tar -xzvf archive.tar.gz               # Extract gzip archive
tar -xvf archive.tar.gz -C /tmp/       # Extract to specific directory

# List contents without extracting
tar -tvf archive.tar.gz
```

Remember the flags: **c**reate, e**x**tract, **v**erbose, **f**ilename, **z** (gzip), **j** (bzip2).

### zip/unzip

```bash
zip archive.zip file1.txt file2.txt    # Create zip
zip -r archive.zip folder/             # Zip a folder
unzip archive.zip                      # Extract zip
unzip archive.zip -d /tmp/             # Extract to specific directory
unzip -l archive.zip                   # List contents without extracting
```

### gzip / gunzip (for single files)

```bash
gzip file.txt            # Compress file → file.txt.gz (original is removed)
gunzip file.txt.gz       # Decompress (restores original)
gzip -k file.txt         # Compress but keep original
```

---

## 15. Finding Files

### find — The Powerful File Finder

```bash
find / -name "file.txt"              # Find by name (search everywhere)
find ~ -name "*.pdf"                 # Find all PDFs in home directory
find . -name "*.log" -type f         # Find files only (not dirs)
find . -type d -name "node_modules"  # Find directories named node_modules
find . -size +100M                   # Find files larger than 100MB
find . -size -1M                     # Find files smaller than 1MB
find . -newer file.txt               # Find files newer than file.txt
find . -mtime -7                     # Find files modified in last 7 days
find . -name "*.log" -delete         # Find and delete .log files

# Run a command on each found file
find . -name "*.txt" -exec wc -l {} \;
```

### locate — Faster but Uses a Database

```bash
locate file.txt           # Fast search using pre-built database
sudo updatedb             # Update the database (run if locate misses recent files)
```

### which / whereis — Find Command Locations

```bash
which python3             # Where is the python3 command?
which ls                  # /usr/bin/ls
whereis python3           # Shows binary, source, and man page locations
```

---

## 16. Disk and Storage Management

```bash
lsblk                     # List all disks and partitions
df -h                     # Disk space usage for all mounted filesystems
df -h /home               # Disk usage for specific directory
du -sh /var/log           # Size of /var/log directory
du -sh * | sort -rh       # Sort all items in current dir by size
du -sh ~                  # Size of your home directory
```

### Mounting and Unmounting

When you plug in a USB drive, Linux mounts it (makes it accessible):

```bash
lsblk                     # See all drives and where they're mounted
mount                     # See what's currently mounted
sudo mount /dev/sdb1 /mnt/usb      # Manually mount a drive
sudo umount /mnt/usb               # Unmount before unplugging
```

Modern desktop systems (GNOME, KDE) auto-mount USB drives and show them in the file manager.

---

## 17. Useful Everyday Commands

### History and Shortcuts

```bash
history                   # Show command history
history | tail -20        # Show last 20 commands
!!                        # Repeat last command
!150                      # Run command #150 from history
!apt                      # Run last command starting with "apt"
Ctrl+R                    # Search through command history interactively
```

### Keyboard Shortcuts in Terminal

| Shortcut | Action |
|----------|--------|
| `Ctrl+C` | Cancel running command |
| `Ctrl+Z` | Suspend running command |
| `Ctrl+D` | Exit terminal / end input |
| `Ctrl+L` | Clear screen |
| `Ctrl+A` | Move cursor to beginning of line |
| `Ctrl+E` | Move cursor to end of line |
| `Ctrl+U` | Delete everything before cursor |
| `Ctrl+K` | Delete everything after cursor |
| `Tab` | Auto-complete command or filename |
| `Tab Tab` | Show all possible completions |
| `↑ / ↓` | Navigate command history |

### Miscellaneous Useful Commands

```bash
# Time a command
time ls -la /usr/bin

# Run a command and ignore hangup (runs even after terminal closes)
nohup long_script.sh &

# Schedule commands with cron
crontab -e         # Edit your cron jobs
crontab -l         # List your cron jobs

# Cron format: minute hour day month weekday command
# Example: run backup every day at 2am
# 0 2 * * * /home/john/backup.sh

# Difference between two files
diff file1.txt file2.txt

# See hardware info
lshw                       # All hardware details (sudo for full output)
lsusb                      # USB devices connected
lspci                      # PCI devices (graphics card, network card, etc.)

# Clipboard from terminal (needs xclip)
echo "hello" | xclip -selection clipboard
cat file.txt | xclip -selection clipboard

# Check Linux version
cat /etc/os-release
lsb_release -a

# Shutdown and reboot
sudo shutdown now           # Shutdown immediately
sudo shutdown -h +10        # Shutdown in 10 minutes
sudo reboot                 # Reboot
```

---

## 18. Linux Cheat Sheet

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
                    🐧 LINUX CHEAT SHEET
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📁 NAVIGATION
  pwd               Print current directory
  ls                List files
  ls -la            List all (including hidden) with details
  cd /path          Change directory
  cd ~              Go to home
  cd ..             Go up one level
  cd -              Go back to previous directory

📄 FILES & DIRECTORIES
  touch file.txt    Create empty file
  mkdir dir         Create directory
  mkdir -p a/b/c    Create nested directories
  cp src dest       Copy file
  cp -r src dest    Copy directory
  mv src dest       Move or rename
  rm file           Delete file
  rm -rf dir        Delete directory and contents
  ln -s src link    Create symbolic link

👀 VIEWING FILES
  cat file          Print file
  less file         View page by page
  head -n 20 file   First 20 lines
  tail -n 20 file   Last 20 lines
  tail -f file      Follow file live

🔍 SEARCHING
  grep "text" file        Search in file
  grep -r "text" dir      Search recursively
  grep -i "text" file     Case-insensitive search
  find . -name "*.txt"    Find files by name
  find . -size +100M      Find files over 100MB
  locate filename         Quick database search

🔐 PERMISSIONS
  chmod 755 file    rwxr-xr-x
  chmod 644 file    rw-r--r--
  chmod 600 file    rw-------
  chmod +x file     Add execute permission
  chown user file   Change owner
  chown user:grp f  Change owner and group

👤 USERS
  whoami            Current user
  id                User ID, group info
  sudo command      Run as root
  su - user         Switch to user
  passwd            Change password

📦 APT PACKAGES (Ubuntu/Debian)
  sudo apt update            Refresh package list
  sudo apt upgrade           Upgrade all packages
  sudo apt install pkg       Install a package
  sudo apt remove pkg        Remove a package
  sudo apt autoremove        Remove unused packages
  apt search pkg             Search for a package

⚙️ PROCESSES
  ps aux            List all processes
  top               Live process monitor
  htop              Better live monitor
  kill PID          Kill by process ID
  kill -9 PID       Force kill
  killall name      Kill all by name
  command &         Run in background
  jobs              List background jobs
  fg                Bring job to foreground
  Ctrl+C            Stop current program
  Ctrl+Z            Suspend current program

🌐 NETWORK
  ip addr           Show IP addresses
  ping host         Test connectivity
  curl URL          Fetch URL
  wget URL          Download file
  ssh user@host     Connect via SSH
  scp file u@h:path Copy file to remote

📊 SYSTEM INFO
  uname -a          Kernel info
  df -h             Disk space usage
  free -h           RAM usage
  du -sh dir        Directory size
  top / htop        CPU and memory live
  uptime            System uptime
  lscpu             CPU info

🔧 SERVICES (systemd)
  sudo systemctl start svc    Start service
  sudo systemctl stop svc     Stop service
  sudo systemctl restart svc  Restart service
  sudo systemctl status svc   Check status
  sudo systemctl enable svc   Auto-start on boot
  sudo systemctl disable svc  Disable auto-start

📦 ARCHIVING
  tar -czvf out.tar.gz dir/   Compress directory
  tar -xzvf file.tar.gz       Extract
  tar -tvf file.tar.gz        List contents
  zip -r out.zip dir/         Zip a directory
  unzip file.zip              Unzip

↔️ PIPES & REDIRECTION
  cmd1 | cmd2       Pipe output to next command
  cmd > file        Redirect output to file (overwrite)
  cmd >> file       Append output to file
  cmd 2> file       Redirect errors to file
  cmd1 && cmd2      Run cmd2 only if cmd1 succeeds
  cmd1 || cmd2      Run cmd2 only if cmd1 fails

⌨️ KEYBOARD SHORTCUTS
  Ctrl+C    Cancel running command
  Ctrl+Z    Suspend command
  Ctrl+D    Exit shell
  Ctrl+L    Clear screen
  Ctrl+A    Go to beginning of line
  Ctrl+E    Go to end of line
  Ctrl+R    Search history
  Tab       Auto-complete
  ↑ / ↓    Navigate history

🗒️ MISC
  echo "text"        Print text
  date               Current date/time
  cal                Show calendar
  history            Command history
  man command        Manual for command
  which command      Find command path
  clear              Clear screen
  exit               Close terminal
  sudo reboot        Restart system
  sudo shutdown now  Shutdown immediately

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

💡 PERMISSION NUMBERS QUICK REFERENCE
  7 = rwx   4 = r--   0 = ---
  6 = rw-   5 = r-x
  3 = -wx   2 = -w-   1 = --x

  Common combos:
  755 → rwxr-xr-x  (programs, directories)
  644 → rw-r--r--  (regular files)
  600 → rw-------  (private/secret files)
  777 → rwxrwxrwx  (avoid — everyone has full access)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

> **Keep practicing!** The best way to learn Linux is to use it daily. Break things, fix them, and don't be afraid of the terminal. The more you type, the faster and more confident you'll become.
>
> **Recommended next steps:**
> - Try every command in this guide on a real terminal
> - Set up a free Linux VM with VirtualBox or try Ubuntu on WSL (Windows Subsystem for Linux)
> - Explore [linuxcommand.org](https://linuxcommand.org) and [tldr.sh](https://tldr.sh) for quick command references
> - Practice shell scripting by automating something you do repeatedly

---

*Notes compiled for beginners and intermediate Linux users. Last updated: June 2026.*
