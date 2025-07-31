Here's a comprehensive Linux command cheat sheet in Markdown format. You can convert this to PDF using tools like [Pandoc](https://pandoc.org/), [Typora](https://typora.io/), or online converters:

```markdown
# Essential Linux Commands Cheat Sheet

## Table of Contents
1. [File & Directory Operations](#file--directory-operations)
2. [Text Processing](#text-processing)
3. [System Information](#system-information)
4. [Process Management](#process-management)
5. [Networking](#networking)
6. [Package Management](#package-management)
7. [Disk & Storage](#disk--storage)
8. [Permissions & Ownership](#permissions--ownership)
9. [Archiving & Compression](#archiving--compression)
10. [Search & Find](#search--find)

---

## File & Directory Operations

### `ls` - List directory contents
```bash
ls -l    # Long format with permissions
ls -a    # Show hidden files
ls -lh   # Human-readable sizes
ls -t    # Sort by modification time
```

### `cd` - Change directory
```bash
cd /var/log       # Absolute path
cd ..             # Parent directory
cd ~              # Home directory
cd -              # Previous directory
```

### `pwd` - Print working directory
```bash
pwd    # /home/user/documents
```

### `mkdir` - Create directory
```bash
mkdir new_folder
mkdir -p path/to/nested/folder  # Create parent directories
```

### `touch` - Create empty file
```bash
touch report.txt
```

### `cp` - Copy files/directories
```bash
cp file.txt backup.txt
cp -r folder/ new_location/     # Recursive copy
cp -u source.txt dest.txt       # Update only
```

### `mv` - Move/rename files
```bash
mv old.txt new.txt
mv file.txt /path/to/destination/
```

### `rm` - Remove files
```bash
rm file.txt
rm -r folder/       # Recursive delete
rm -f file.txt      # Force delete
rm -rf folder/      # Dangerous! Force recursive delete
```

### `ln` - Create links
```bash
ln source.txt hardlink.txt      # Hard link
ln -s source.txt symlink.txt    # Symbolic link
```

---

## Text Processing

### `cat` - Concatenate and display files
```bash
cat file.txt
cat file1.txt file2.txt > combined.txt
```

### `less`/`more` - View files page by page
```bash
less large_file.log
```

### `head`/`tail` - View file beginnings/ends
```bash
head -n 20 file.txt    # First 20 lines
tail -f /var/log/syslog  # Follow log updates
tail -n +5 file.txt    # Skip first 4 lines
```

### `grep` - Search text patterns
```bash
grep "error" logfile.txt
grep -r "192.168.1" /etc/  # Recursive search
grep -i "warning" file.txt # Case-insensitive
grep -v "debug" file.txt  # Invert match
```

### `sed` - Stream editor
```bash
sed 's/old/new/g' file.txt    # Replace text
sed -i 's/foo/bar/g' *.txt    # In-place edit
sed '3d' file.txt             # Delete 3rd line
```

### `awk` - Text processing utility
```bash
awk '{print $1}' file.txt     # Print first column
awk -F: '{print $1}' /etc/passwd  # Colon delimiter
awk '{sum+=$1} END {print sum}' file.txt  # Sum column
```

### `sort` - Sort lines
```bash
sort file.txt
sort -n file.txt     # Numeric sort
sort -r file.txt     # Reverse order
sort -k 2 file.txt   # Sort by 2nd column
```

### `uniq` - Remove duplicate lines
```bash
sort file.txt | uniq
uniq -c file.txt     # Count occurrences
uniq -d file.txt     # Show only duplicates
```

### `wc` - Word count
```bash
wc file.txt          # Lines, words, bytes
wc -l file.txt       # Line count only
wc -w file.txt       # Word count only
```

---

## System Information

### `uname` - System information
```bash
uname -a             # All information
uname -r             # Kernel version
```

### `top`/`htop` - Process monitor
```bash
top
htop                 # Enhanced version
```

### `df` - Disk space usage
```bash
df -h                # Human-readable
df -T               # Show filesystem types
```

### `du` - Directory space usage
```bash
du -sh /path/to/dir  # Summary in human-readable
du -h --max-depth=1  # Show immediate subdirectories
```

### `free` - Memory usage
```bash
free -h              # Human-readable
free -m              # Megabytes
```

### `lscpu` - CPU information
```bash
lscpu
```

### `uptime` - System uptime
```bash
uptime
```

---

## Process Management

### `ps` - Process status
```bash
ps aux               # All processes
ps -ef               # All processes in full format
ps aux | grep nginx  # Find specific process
```

### `kill` - Terminate processes
```bash
kill 1234            # PID
kill -9 1234         # Force kill
killall nginx        # Kill by name
```

### `jobs`/`fg`/`bg` - Job control
```bash
jobs                 # List background jobs
fg %1                # Bring job to foreground
bg %1                # Send to background
```

### `nohup` - Run command immune to hangups
```bash
nohup long_script.sh &
```

---

## Networking

### `ip` - Network configuration
```bash
ip addr show         # Show interfaces
ip route show        # Show routing table
```

### `ping` - Network connectivity test
```bash
ping google.com
ping -c 4 8.8.8.8    # Send 4 packets
```

### `netstat`/`ss` - Network statistics
```bash
ss -tuln             # Listening ports
netstat -an          # All connections
```

### `curl`/`wget` - File transfer
```bash
curl -O https://example.com/file.zip
wget https://example.com/file.zip
```

### `ssh` - Secure shell
```bash
ssh user@remote_host
ssh -i key.pem user@host
```

### `scp` - Secure copy
```bash
scp file.txt user@remote:/path/
scp -r folder/ user@remote:/path/
```

---

## Package Management

### Debian/Ubuntu (`apt`)
```bash
sudo apt update               # Update package lists
sudo apt install package_name # Install package
sudo apt remove package_name  # Remove package
apt search keyword            # Search packages
apt list --installed          # Show installed packages
```

### RHEL/CentOS (`dnf`/`yum`)
```bash
sudo dnf install package_name
sudo dnf remove package_name
dnf search keyword
```

### Arch (`pacman`)
```bash
sudo pacman -S package_name
sudo pacman -R package_name
pacman -Ss keyword
```

---

## Disk & Storage

### `fdisk`/`parted` - Partitioning
```bash
sudo fdisk -l       # List partitions
sudo parted /dev/sdb
```

### `mkfs` - Create filesystem
```bash
sudo mkfs.ext4 /dev/sdb1
```

### `mount`/`umount` - Mount filesystems
```bash
mount /dev/sdb1 /mnt/data
umount /mnt/data
```

### `lsblk` - List block devices
```bash
lsblk
```

### `dd` - Disk cloning/convert
```bash
dd if=/dev/sda of=backup.img bs=4M
dd if=backup.img of=/dev/sdb bs=4M
```

---

## Permissions & Ownership

### `chmod` - Change permissions
```bash
chmod 755 script.sh      # rwxr-xr-x
chmod u+x script.sh      # Add execute for owner
chmod g-w file.txt       # Remove write for group
```

### `chown` - Change ownership
```bash
sudo chown user:group file.txt
sudo chown -R user /path/to/dir
```

### `chgrp` - Change group ownership
```bash
sudo chgrp developers file.txt
```

---

## Archiving & Compression

### `tar` - Tape archiver
```bash
tar -cvf archive.tar file1 file2/  # Create archive
tar -xvf archive.tar               # Extract
tar -czvf archive.tar.gz folder/   # Create gzip
tar -xzvf archive.tar.gz           # Extract gzip
tar -cjvf archive.tar.bz2 folder/  # Create bzip2
```

### `zip`/`unzip`
```bash
zip -r archive.zip folder/
unzip archive.zip
```

### `gzip`/`gunzip`
```bash
gzip file.txt          # Creates file.txt.gz
gunzip file.txt.gz
```

---

## Search & Find

### `find` - Search files
```bash
find /path -name "*.log"       # By name
find /path -type f -mtime +7   # Files modified >7 days ago
find /path -size +100M         # Files larger than 100MB
find /path -exec rm {} \;      # Execute command on results
```

### `locate` - Quick file search
```bash
locate config.ini
sudo updatedb                 # Update database
```

### `which`/`whereis` - Locate commands
```bash
which python3
whereis ls
```

---

## Tips & Tricks
1. **Command Chaining**:
   ```bash
   command1 && command2    # Run if command1 succeeds
   command1 || command2    # Run if command1 fails
   command1 ; command2     # Run sequentially
   ```

2. **Redirection**:
   ```bash
   command > file.txt      # Overwrite output
   command >> file.txt     # Append output
   command 2> error.log    # Redirect stderr
   command &> output.log   # Redirect stdout and stderr
   ```

3. **Pipes**:
   ```bash
   command1 | command2     # Pass output as input
   ```

4. **History**:
   ```bash
   history                 # Show command history
   !50                     # Execute 50th command
   !!                      # Execute last command
   ```

5. **Help**:
   ```bash
   man command             # Manual page
   command --help          # Quick help
   whatis command          # Brief description
   ```
```

### To Convert to PDF:
1. Save as `linux_commands.md`
2. Use one of these methods:

**Option 1: Pandoc (Recommended)**
```bash
pandoc linux_commands.md -o linux_commands.pdf --pdf-engine=xelatex -V geometry:margin=1in
```

