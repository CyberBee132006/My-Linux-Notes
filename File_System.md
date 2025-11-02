# Linux File Systems - Detailed Notes

## Overview

- Linux uses a hierarchical file system structure governed by the Filesystem Hierarchy Standard (FHS), maintained by the Linux Foundation.
- The entire file system starts from a single root directory `/`.
- All files, directories, devices, and partitions reside under this root directory, forming a tree-like structure.
- Unlike Windows drive letters, Linux uses mount points to integrate various device file systems within this single structure.
- Only the root user has permission to modify contents inside the root directory.

## Important Directories and Their Roles

| Directory  | Description                                                                                      | Importance                                                    |
|------------|--------------------------------------------------------------------------------------------------|---------------------------------------------------------------|
| `/`        | Top-most root directory; all other directories branch from here.                                | Foundation of entire filesystem; only root can modify directly here. |
| `/bin`     | Essential binary executables for system startup, repair, and general commands.                   | Contains critical commands like `ls`, `cp`, `rm`.             |
| `/sbin`    | System binaries for administrative tasks, mostly run by root.                                  | Commands like `mount`, `fsck`, `ifconfig`.                    |
| `/boot`    | Kernel and bootloader files necessary for OS startup (`vmlinuz`, `initrd`).                     | Critical for system boot processes.                           |
| `/dev`     | Device files representing hardware and virtual devices.                                        | Interface for hardware access via files.                      |
| `/etc`     | System-wide configuration files for settings and services.                                    | Stores config files like `/etc/passwd`, `/etc/fstab`.         |
| `/home`    | User home directories storing personal files and configs.                                     | Location for user data storage.                               |
| `/lib`     | Shared libraries for binaries in `/bin` and `/sbin`.                                          | Essential for command execution; like DLLs in Windows.         |
| `/media`   | Mount points for removable media devices (USB drives, CD-ROMs).                               | Temporary mount locations for external devices.               |
| `/mnt`     | Temporary mount point for filesystems during maintenance.                                     | Used by sysadmins for mounting during system maintenance.     |
| `/opt`     | Optional software packages installed manually outside the package management system.          | Typically for third-party software installations.             |
| `/proc`    | Virtual filesystem providing information about running processes and kernel.                   | Dynamic info accessed as files, like kernel and process stats. |
| `/root`    | Home directory of the root user (system administrator).                                       | Separate from `/home` for security reasons.                    |
| `/run`     | Runtime data and system information since the last boot.                                      | Includes PID files, sockets, runtime state info.              |
| `/srv`     | Data for services provided by the system (e.g., web server files).                            | Location for site-specific service data.                      |
| `/tmp`     | Temporary files storage, usually cleared upon reboot.                                         | Scratch space for applications and users.                     |
| `/usr`     | Secondary hierarchy for read-only user data, user utilities, and applications.                | Contains most user binaries, libraries, documentation.       |
| `/var`     | Variable data like logs, caches, spool files.                                                | Storage for frequently changing files like `/var/log`.       |

## File Types in Linux Filesystem

- Regular files: Text, executable binaries, media.
- Directories: Containers for files and other directories.
- Symbolic Links: References or shortcuts pointing to other files or directories.
- Device Files: Files representing hardware devices.
- Sockets and Pipes: Used for inter-process communication.

## Mounting and Devices

- File systems and devices are accessed via mount points.
- `mount` command attaches file systems to directories; `umount` detaches them.
- Devices like hard drives appear as special files in `/dev`.
- All files and partitions reside in a cohesive directory hierarchy.

## File System Permissions and Ownership

- Permissions: Read (r), write (w), execute (x) are set for user, group, and others.
- Ownership defines who can access or modify files.
- Commands to manage permissions: `chmod`, `chown`, `chgrp`.
- Permissions are essential to prevent unauthorized access or alterations.

## Special Features

- Virtual filesystems `/proc` and `/sys` provide kernel and process information dynamically.
- Symbolic links (`ln -s`) allow flexible referencing and file organization.
- Hard links (`ln`) provide multiple directory entries for the same file inode.

## Common Linux File System Types

- **ext4**: Most widely used, journaling enabled, reliable.
- **xfs**: High-performance journaling filesystem.
- **btrfs**: Advanced; supports snapshots, checksums.
- **vfat/ntfs**: Windows-compatible formats.
- **tmpfs**: In-memory temporary file system.

## Importance in Cybersecurity and System Administration

- Mastery of Linux file system hierarchy aids system auditing and security hardening.
- Sensitive directories (`/etc`, `/root`, `/bin`) must be secured to prevent unauthorized changes.
- Logs in `/var/log` are critical for detecting system and security events.
- Proper mount and permission configurations reduce unauthorized privilege escalations.
- Incident response relies on knowledge of config files and system locations.

---





# Linux File System Commands Reference

## Navigation & Directory Operations

| Command | Options | Description |
|---------|---------|-------------|
| `cd` | `~` | Change to home directory |
| | `..` | Move up one directory |
| | `-` | Return to previous directory |
| `pwd` | `-P` | Print physical working directory (resolve symlinks) |
| `ls` | `-l` | Long format listing with details |
| | `-a` | Show hidden files (starting with .) |
| | `-h` | Human-readable file sizes |
| | `-R` | Recursive listing |
| | `-t` | Sort by modification time |
| | `-S` | Sort by file size |
| `tree` | `-L n` | Limit depth to n levels |
| | `-a` | Show hidden files |
| | `-d` | Directories only |

## File Operations

| Command | Options | Description |
|---------|---------|-------------|
| `cp` | `-r` | Copy directories recursively |
| | `-i` | Interactive (prompt before overwrite) |
| | `-v` | Verbose output |
| | `-p` | Preserve permissions and timestamps |
| | `-u` | Copy only when source is newer |
| `mv` | `-i` | Interactive mode |
| | `-v` | Verbose output |
| | `-n` | No overwrite |
| `rm` | `-r` | Remove directories recursively |
| | `-f` | Force removal without prompts |
| | `-i` | Interactive confirmation |
| | `-v` | Verbose output |
| `mkdir` | `-p` | Create parent directories as needed |
| | `-m` | Set permissions |
| | `-v` | Verbose output |
| `rmdir` | `-p` | Remove parent directories if empty |
| `touch` | `-a` | Change only access time |
| | `-m` | Change only modification time |
| | `-t` | Use specific timestamp |

## File Viewing & Searching

| Command | Options | Description |
|---------|---------|-------------|
| `cat` | `-n` | Number all output lines |
| | `-b` | Number non-empty lines |
| | `-A` | Show all non-printing characters |
| `less` | `-N` | Show line numbers |
| | `-S` | Don't wrap long lines |
| | `+F` | Follow mode (like tail -f) |
| `head` | `-n N` | Show first N lines (default 10) |
| | `-c N` | Show first N bytes |
| `tail` | `-n N` | Show last N lines (default 10) |
| | `-f` | Follow file updates in real-time |
| | `-F` | Follow with retry (if file recreated) |
| `grep` | `-r` | Search recursively in directories |
| | `-i` | Case-insensitive search |
| | `-n` | Show line numbers |
| | `-v` | Invert match (show non-matching) |
| | `-c` | Count matching lines |
| | `-l` | Show only filenames with matches |
| `find` | `-name` | Search by filename pattern |
| | `-type f/d` | Search for files/directories |
| | `-size +/-N` | Find by size |
| | `-mtime N` | Modified N days ago |
| | `-exec` | Execute command on results |

## Permissions & Ownership

| Command | Options | Description |
|---------|---------|-------------|
| `chmod` | `-R` | Change permissions recursively |
| | `-v` | Verbose output |
| | `u+x` | Add execute for user |
| | `755` | rwxr-xr-x permissions |
| `chown` | `-R` | Change ownership recursively |
| | `-v` | Verbose output |
| | `user:group` | Set user and group |
| `chgrp` | `-R` | Change group recursively |
| | `-v` | Verbose output |

## Disk Usage & Information

| Command | Options | Description |
|---------|---------|-------------|
| `df` | `-h` | Human-readable sizes |
| | `-T` | Show filesystem type |
| | `-i` | Show inode information |
| `du` | `-h` | Human-readable sizes |
| | `-s` | Summary (total only) |
| | `-a` | Show all files, not just directories |
| | `-c` | Grand total at the end |
| | `--max-depth=N` | Limit depth of directory tree |
| `stat` | `-f` | Display filesystem status |
| | `-c` | Custom format |

## File Compression & Archiving

| Command | Options | Description |
|---------|---------|-------------|
| `tar` | `-c` | Create archive |
| | `-x` | Extract archive |
| | `-v` | Verbose output |
| | `-f` | Specify filename |
| | `-z` | Compress with gzip |
| | `-j` | Compress with bzip2 |
| | `-t` | List contents |
| `gzip` | `-d` | Decompress |
| | `-k` | Keep original file |
| | `-r` | Recursive |
| `zip` | `-r` | Recursive (include directories) |
| | `-e` | Encrypt with password |
| `unzip` | `-l` | List contents |
| | `-d` | Extract to directory |

## Links & File Info

| Command | Options | Description |
|---------|---------|-------------|
| `ln` | `-s` | Create symbolic link |
| | `-f` | Force (overwrite existing) |
| | `-v` | Verbose output |
| `file` | `-b` | Brief mode (no filename) |
| | `-i` | Show MIME type |
| `wc` | `-l` | Count lines |
| | `-w` | Count words |
| | `-c` | Count bytes |

## Common Command Combinations

```bash
# Find and delete files
find . -name "*.tmp" -type f -delete

# Copy with progress
cp -rv source/ destination/

# Archive and compress
tar -czf archive.tar.gz directory/

# Extract archive
tar -xzf archive.tar.gz

# Find large files
find / -type f -size +100M -exec ls -lh {} \;

# Disk usage sorted by size
du -sh */ | sort -hr

# Search in files
grep -rni "search_term" /path/

# Monitor log file
tail -f /var/log/syslog
```

## Permission Notation Quick Reference

| Notation | Binary | Decimal | Meaning |
|----------|--------|---------|---------|
| `rwx` | 111 | 7 | Read, write, execute |
| `rw-` | 110 | 6 | Read, write |
| `r-x` | 101 | 5 | Read, execute |
| `r--` | 100 | 4 | Read only |
| `---` | 000 | 0 | No permissions |
