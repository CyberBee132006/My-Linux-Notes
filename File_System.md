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