# Linux Commands: When to Use Each

## File & Directory Operations

### `ls` - List directory contents
**When to use:**
- Exploring directory structure
- Checking file permissions and sizes
- Verifying file existence

**Examples:**
```bash
ls -l    # Detailed view with permissions, size, owner
ls -a    # Show hidden files (starting with .)
ls -lh   # Human-readable sizes (KB, MB, GB)
ls -t    # Sort by modification time (newest first)
```

### `cd` - Change directory
**When to use:**
- Navigating between directories
- Moving to home or parent directories
- Switching to previous working directory

**Examples:**
```bash
cd /var/log       # Navigate to absolute path
cd ..             # Move up one directory level
cd ~              # Go to home directory
cd -              # Return to previous directory
```

### `pwd` - Print working directory
**When to use:**
- Confirming current location
- Saving current path for later use
- Verifying directory after complex navigation

**Example:**
```bash
pwd    # Outputs: /home/user/documents
```

### `mkdir` - Create directory
**When to use:**
- Setting up project structures
- Creating temporary directories
- Organizing files

**Examples:**
```bash
mkdir new_folder              # Single directory
mkdir -p path/to/nested/folder  # Create parent directories as needed
```

### `touch` - Create empty file
**When to use:**
- Creating placeholder files
- Updating file timestamps
- Testing file permissions

**Examples:**
```bash
touch report.txt             # Create empty file
touch -t 202301011200 file.txt  # Set specific timestamp
```

### `cp` - Copy files/directories
**When to use:**
- Backing up files
- Duplicating project files
- Moving files across filesystems

**Examples:**
```bash
cp file.txt backup.txt       # Copy file
cp -r folder/ new_location/  # Copy entire directory
cp -u source.txt dest.txt    # Update only if source is newer
```

### `mv` - Move/rename files
**When to use:**
- Renaming files
- Moving files to different directories
- Reorganizing directory structures

**Examples:**
```bash
mv old.txt new.txt           # Rename file
mv file.txt /path/to/destination/  # Move file
mv *.jpg images/             # Move all JPG files to images directory
```

### `rm` - Remove files
**When to use:**
- Deleting unnecessary files
- Cleaning up temporary files
- Removing entire directory trees

**Examples:**
```bash
rm file.txt                 # Remove file
rm -r folder/               # Remove directory and contents
rm -f file.txt              # Force remove (ignore errors)
rm -rf folder/              # Force remove directory and contents (use with caution!)
```

### `ln` - Create links
**When to use:**
- Creating shortcuts to frequently accessed files
- Sharing files between users
- Maintaining multiple versions of files

**Examples:**
```bash
ln source.txt hardlink.txt      # Create hard link
ln -s source.txt symlink.txt    # Create symbolic link
ln -s /path/to/long/named/file ~/shortcut  # Create shortcut to long path
```

## Text Processing

### `cat` - Concatenate and display files
**When to use:**
- Viewing short files
- Combining multiple files
- Creating simple files

**Examples:**
```bash
cat file.txt                 # Display file content
cat file1.txt file2.txt > combined.txt  # Combine files
cat > newfile.txt            # Create file from keyboard input
```

### `less`/`more` - View files page by page
**When to use:**
- Viewing large files
- Reading logs
- Examining files without editing

**Examples:**
```bash
less large_file.log          # View file with navigation
more large_file.log          # Simpler pager (less features)
grep "error" logfile.txt | less  # View filtered results
```

### `head`/`tail` - View file beginnings/ends
**When to use:**
- Checking file headers
- Monitoring log updates
- Previewing file content

**Examples:**
```bash
head -n 20 file.txt          # First 20 lines
tail -f /var/log/syslog      # Follow log updates in real-time
tail -n +5 file.txt          # Skip first 4 lines
head -c 100 file.txt         # First 100 bytes
```

### `grep` - Search text patterns
**When to use:**
- Finding specific content in files
- Filtering command output
- Searching for patterns across multiple files

**Examples:**
```bash
grep "error" logfile.txt     # Find lines containing "error"
grep -r "192.168.1" /etc/    # Recursive search in directory
grep -i "warning" file.txt   # Case-insensitive search
grep -v "debug" file.txt    # Show lines not containing "debug"
```

### `sed` - Stream editor
**When to use:**
- Find and replace text
- Editing files in place
- Performing text transformations

**Examples:**
```bash
sed 's/old/new/g' file.txt    # Replace all occurrences of "old" with "new"
sed -i 's/foo/bar/g' *.txt    # Edit files in place
sed '3d' file.txt             # Delete 3rd line
sed -n '5,10p' file.txt       # Print lines 5-10
```

### `awk` - Text processing utility
**When to use:**
- Extracting columns from structured text
- Performing calculations on text data
- Processing CSV or tabular data

**Examples:**
```bash
awk '{print $1}' file.txt     # Print first column
awk -F: '{print $1}' /etc/passwd  # Print usernames from /etc/passwd
awk '{sum+=$1} END {print sum}' file.txt  # Sum values in first column
awk '$3 > 100 {print $0}' file.txt  # Print lines where third column > 100
```

### `sort` - Sort lines
**When to use:**
- Organizing text data
- Preparing data for further processing
- Removing duplicate lines (with uniq)

**Examples:**
```bash
sort file.txt               # Sort lines alphabetically
sort -n file.txt           # Numeric sort
sort -r file.txt           # Reverse order
sort -k 2 file.txt         # Sort by second column
sort -u file.txt           # Remove duplicate lines
```

### `uniq` - Remove duplicate lines
**When to use:**
- Removing duplicate entries
- Counting occurrences of lines
- Finding unique values in data

**Examples:**
```bash
sort file.txt | uniq       # Remove duplicate lines
uniq -c file.txt           # Count occurrences of each line
uniq -d file.txt           # Show only duplicate lines
uniq -u file.txt           # Show only unique lines
```

### `wc` - Word count
**When to use:**
- Counting lines in files
- Analyzing text statistics
- Verifying file sizes

**Examples:**
```bash
wc file.txt                # Count lines, words, and bytes
wc -l file.txt             # Count lines only
wc -w file.txt             # Count words only
wc -c file.txt             # Count bytes only
wc -m file.txt             # Count characters only
```

## 🧑‍💻 User Management in Linux

### 1. **User Accounts**
#### Create a new user
```bash
sudo useradd -m -s /bin/bash newuser  # Create with home directory and bash shell
sudo passwd newuser                   # Set password
```

#### Delete a user
```bash
sudo userdel -r newuser  # Delete user and home directory
```

#### Modify user properties
```bash
sudo usermod -l newname oldname    # Rename user
sudo usermod -aG sudo newuser      # Add to sudo group
sudo usermod -s /bin/zsh newuser   # Change default shell
```

### 2. **Group Management**
#### Create/delete groups
```bash
sudo groupadd developers
sudo groupdel developers
```

#### Manage group membership
```bash
sudo usermod -aG developers newuser  # Add user to group
sudo gpasswd -d newuser developers   # Remove user from group
```

### 3. **User Information**
```bash
id newuser              # Show user/group IDs
finger newuser          # Show user details
last newuser           # Show login history
who                    # Show logged-in users
w                      # Show active users with processes
```

### 4. **Switching Users**
```bash
su - newuser           # Switch to user with environment
sudo -i                # Switch to root
sudo -u newuser command  # Run command as another user
```

### 5. **Password Management**
```bash
sudo passwd newuser    # Change user password
sudo chage -l newuser  # Show password expiration info
sudo chage -M 90 newuser  # Set password to expire in 90 days
```

---

## 📁 File Management in Linux

### 1. **File Operations**
#### Create files
```bash
touch file.txt         # Create empty file
echo "content" > file.txt  # Create with content
nano file.txt          # Create with text editor
```

#### Copy files
```bash
cp file.txt backup.txt              # Copy file
cp -r source_dir/ dest_dir/         # Copy directory
cp -u source.txt dest.txt           # Update only if newer
```

#### Move/Rename files
```bash
mv old.txt new.txt      # Rename file
mv file.txt /path/to/   # Move file
mv *.txt documents/     # Move multiple files
```

#### Delete files
```bash
rm file.txt             # Delete file
rm -r directory/        # Delete directory
rm -f file.txt          # Force delete
rm -i file.txt          # Interactive delete (confirm)
```

### 2. **Directory Operations**
```bash
mkdir new_dir           # Create directory
mkdir -p path/to/dir   # Create nested directories
rmdir empty_dir        # Remove empty directory
```

### 3. **File Viewing**
```bash
cat file.txt           # View entire file
less file.txt          # View page by page
head -n 20 file.txt    # View first 20 lines
tail -f file.txt       # Follow file updates (logs)
```

### 4. **File Permissions**
#### Symbolic mode
```bash
chmod u+x script.sh    # Add execute for owner
chmod go-w file.txt    # Remove write for group/others
chmod a=r file.txt     # Set read-only for all
```

#### Numeric mode
```bash
chmod 755 script.sh    # rwxr-xr-x
chmod 644 file.txt     # rw-r--r--
chmod 700 ~/.ssh/      # Restrict SSH directory
```

#### Ownership
```bash
sudo chown user:group file.txt  # Change owner and group
sudo chown -R user /path/       # Recursive ownership change
```

### 5. **File Searching**
```bash
find /path -name "*.log"        # Find by name
find /path -type f -mtime +7    # Find files modified >7 days ago
find /path -size +100M          # Find files >100MB
locate config.ini               # Quick name search
```

### 6. **File Comparison**
```bash
diff file1.txt file2.txt        # Show differences
sdiff file1.txt file2.txt       # Side-by-side comparison
vimdiff file1.txt file2.txt     # Visual diff in vim
```

### 7. **File Content Manipulation**
```bash
grep "error" logfile.txt        # Search text pattern
sed 's/old/new/g' file.txt     # Find and replace
awk '{print $1}' file.txt       # Extract first column
sort file.txt                   # Sort lines
uniq file.txt                   # Remove duplicate lines
```

### 8. **File Compression**
```bash
tar -czvf archive.tar.gz dir/   # Create compressed archive
tar -xzvf archive.tar.gz       # Extract archive
zip -r archive.zip dir/        # Create ZIP archive
unzip archive.zip              # Extract ZIP
```

### 9. **File System Management**
```bash
df -h                         # Show disk usage
du -sh /path/to/dir           # Show directory size
lsblk                         # List block devices
mount /dev/sdb1 /mnt/data    # Mount filesystem
umount /mnt/data              # Unmount filesystem
```

### 10. **Special Files**
```bash
ln -s source.txt link.txt     # Create symbolic link
ln source.txt hardlink.txt    # Create hard link
mkfifo mypipe                 # Create named pipe
```

---

## 🔐 Security Best Practices

1. **User Security**
   ```bash
   sudo passwd -l newuser     # Lock user account
   sudo usermod -L newuser    # Alternative lock method
   sudo chage -E 2023-12-31 newuser  # Set account expiration
   ```

2. **File Security**
   ```bash
   chmod 600 ~/.ssh/id_rsa   # Restrict private key
   chmod 644 ~/.ssh/id_rsa.pub # Public key permissions
   sudo chmod 4755 /bin/sudo  # Set SUID bit (if needed)
   ```

3. **Audit Files**
   ```bash
   sudo auditctl -w /etc/passwd -p wa -k passwd_changes  # Monitor changes
   sudo ausearch -k passwd_changes  # View audit logs
   ```


## 🛠️ DevOps Use Cases

### 1. **User Provisioning Script**
```bash
#!/bin/bash
# Add new developer user
username=$1
sudo useradd -m -s /bin/bash -G developers $username
echo "$username:TempPass123!" | sudo chpasswd
sudo chage -d 0 $username  # Force password change on first login
```

### 2. **File Cleanup Automation**
```bash
#!/bin/bash
# Clean up old logs
find /var/log -name "*.log" -type f -mtime +30 -exec rm {} \;
find /tmp -type f -atime +7 -delete
```

### 3. **Permission Fix Script**
```bash
#!/bin/bash
# Fix web directory permissions
sudo chown -R www-data:www-data /var/www/html
sudo find /var/www/html -type d -exec chmod 755 {} \;
sudo find /var/www/html -type f -exec chmod 644 {} \;
```

### 4. **Backup Strategy**
```bash
#!/bin/bash
# Create backup with proper permissions
tar -czvf /backup/$(date +%Y%m%d).tar.gz /important/data
chmod 600 /backup/*.tar.gz
chown backup:backup /backup/*.tar.gz
```

### `uname` - System information
**When to use:**
- Checking kernel version
- Identifying system architecture
- Verifying system type

**Examples:**
```bash
uname -a                   # Show all system information
uname -r                   # Show kernel version
uname -m                   # Show machine architecture
```

### `top`/`htop` - Process monitor
**When to use:**
- Monitoring system resources
- Identifying resource-intensive processes
- Checking system performance

**Examples:**
```bash
top                       # Basic process monitor
htop                      # Enhanced process monitor with colors
top -p 1234               # Monitor specific process
```

### `df` - Disk space usage
**When to use:**
- Checking available disk space
- Identifying full filesystems
- Monitoring storage usage

**Examples:**
```bash
df -h                     # Human-readable sizes
df -T                     # Show filesystem types
df -i                     # Show inode usage
df -h /dev/sda1           # Check specific filesystem
```

### `du` - Directory space usage
**When to use:**
- Finding large directories
- Identifying disk space hogs
- Cleaning up disk space

**Examples:**
```bash
du -sh /path/to/dir       # Summary in human-readable format
du -h --max-depth=1       # Show immediate subdirectories
du -h --sort=-r           # Sort by size (largest first)
```

### `free` - Memory usage
**When to use:**
- Checking available memory
- Monitoring memory usage
- Identifying memory issues

**Examples:**
```bash
free -h                   # Human-readable sizes
free -m                   # Show in megabytes
free -t                   # Show total memory
free -s 5                 # Update every 5 seconds
```

### `lscpu` - CPU information
**When to use:**
- Checking CPU specifications
- Identifying processor capabilities
- Verifying system architecture

**Example:**
```bash
lscpu                     # Display CPU information
```

### `uptime` - System uptime
**When to use:**
- Checking how long system has been running
- Monitoring system load
- Verifying system stability

**Example:**
```bash
uptime                    # Show uptime and load averages
```

## Process Management

### `ps` - Process status
**When to use:**
- Listing running processes
- Finding specific processes
- Checking process details

**Examples:**
```bash
ps aux                    # List all processes in detail
ps -ef                    # Alternative format for all processes
ps aux | grep nginx       # Find specific process
ps -p 1234               # Show details for specific PID
```

### `kill` - Terminate processes
**When to use:**
- Stopping unresponsive processes
- Terminating background processes
- Managing system resources

**Examples:**
```bash
kill 1234                 # Terminate process with PID 1234
kill -9 1234              # Force kill process
killall nginx             # Kill all processes named nginx
kill -l                   # List all signal types
```

### `jobs`/`fg`/`bg` - Job control
**When to use:**
- Managing background processes
- Switching between foreground and background
- Checking running jobs

**Examples:**
```bash
jobs                      # List background jobs
fg %1                     # Bring job 1 to foreground
bg %1                     # Send job 1 to background
sleep 100 &               # Start command in background
```

### `nohup` - Run command immune to hangups
**When to use:**
- Running long processes after logout
- Starting services that should persist
- Executing background tasks

**Examples:**
```bash
nohup long_script.sh &    # Run script in background after logout
nohup python app.py > app.log 2>&1 &  # Redirect output to log file
```

## Networking

### `ip` - Network configuration
**When to use:**
- Configuring network interfaces
- Viewing network settings
- Troubleshooting network issues

**Examples:**
```bash
ip addr show              # Show all network interfaces
ip route show             # Show routing table
ip link set eth0 up       # Bring interface up
ip addr add 192.168.1.10/24 dev eth0  # Add IP address
```

### `ping` - Network connectivity test
**When to use:**
- Testing network connectivity
- Checking remote host availability
- Diagnosing network latency

**Examples:**
```bash
ping google.com           # Test connectivity to google.com
ping -c 4 8.8.8.8         # Send 4 packets
ping -i 2 google.com      # Wait 2 seconds between packets
```

### `netstat`/`ss` - Network statistics
**When to use:**
- Viewing network connections
- Monitoring listening ports
- Troubleshooting network issues

**Examples:**
```bash
ss -tuln                  # Show all listening ports
netstat -an               # Show all connections
ss -tp                    # Show processes using sockets
netstat -s               # Show network statistics
```

### `curl`/`wget` - File transfer
**When to use:**
- Downloading files from internet
- Testing web services
- Transferring data via URLs

**Examples:**
```bash
curl -O https://example.com/file.zip  # Download file
curl -I https://example.com   # Get HTTP headers only
wget https://example.com/file.zip  # Download file
wget -r https://example.com/dir/  # Recursively download directory
```

### `ssh` - Secure shell
**When to use:**
- Connecting to remote servers
- Executing commands remotely
- Secure file transfers

**Examples:**
```bash
ssh user@remote_host      # Connect to remote host
ssh -i key.pem user@host  # Connect using SSH key
ssh user@host "command"   # Execute command remotely
ssh -L 8080:localhost:80 user@host  # Create SSH tunnel
```

### `scp` - Secure copy
**When to use:**
- Securely transferring files between systems
- Copying files to/from remote servers
- Automating secure file transfers

**Examples:**
```bash
scp file.txt user@remote:/path/  # Copy file to remote host
scp user@remote:/path/file.txt .  # Copy file from remote host
scp -r folder/ user@remote:/path/  # Copy directory recursively
scp -P 2222 file.txt user@remote:/path/  # Use custom port
```

## Package Management

### Debian/Ubuntu (`apt`)
**When to use:**
- Installing software on Debian-based systems
- Managing system updates
- Handling software dependencies

**Examples:**
```bash
sudo apt update           # Update package lists
sudo apt install package_name  # Install package
sudo apt remove package_name   # Remove package
apt search keyword        # Search for packages
apt list --installed      # Show installed packages
sudo apt upgrade          # Upgrade all packages
```

### RHEL/CentOS (`dnf`/`yum`)
**When to use:**
- Managing software on Red Hat-based systems
- Installing enterprise software
- Handling system updates

**Examples:**
```bash
sudo dnf install package_name  # Install package
sudo dnf remove package_name   # Remove package
dnf search keyword        # Search for packages
dnf list installed        # Show installed packages
sudo dnf upgrade          # Upgrade all packages
```

### Arch (`pacman`)
**When to use:**
- Managing software on Arch Linux
- Installing bleeding-edge software
- Maintaining a rolling-release system

**Examples:**
```bash
sudo pacman -S package_name  # Install package
sudo pacman -R package_name   # Remove package
pacman -Ss keyword       # Search for packages
pacman -Q               # Show installed packages
sudo pacman -Syu        # Upgrade system
```

## Disk & Storage

### `fdisk`/`parted` - Partitioning
**When to use:**
- Creating disk partitions
- Modifying partition tables
- Preparing disks for formatting

**Examples:**
```bash
sudo fdisk -l            # List all partitions
sudo fdisk /dev/sdb      # Start partitioning tool
sudo parted /dev/sdb     # Start GNU parted
parted /dev/sdb mklabel gpt  # Create GPT partition table
```

### `mkfs` - Create filesystem
**When to use:**
- Formatting partitions
- Preparing new disks for use
- Creating filesystems on block devices

**Examples:**
```bash
sudo mkfs.ext4 /dev/sdb1  # Create ext4 filesystem
sudo mkfs.xfs /dev/sdb2   # Create XFS filesystem
sudo mkfs.vfat -F 32 /dev/sdb3  # Create FAT32 filesystem
```

### `mount`/`umount` - Mount filesystems
**When to use:**
- Making filesystems accessible
- Accessing external drives
- Working with network shares

**Examples:**
```bash
mount /dev/sdb1 /mnt/data  # Mount filesystem
mount -t ntfs /dev/sdb1 /mnt/windows  # Mount with specific type
umount /mnt/data          # Unmount filesystem
mount -a                 # Mount all filesystems in /etc/fstab
```

### `lsblk` - List block devices
**When to use:**
- Identifying available storage devices
- Checking disk partitioning
- Finding device names for mounting

**Examples:**
```bash
lsblk                    # List block devices
lsblk -f                 # Show filesystem information
lsblk -d                 # Show only disks (not partitions)
```

### `dd` - Disk cloning/convert
**When to use:**
- Creating disk images
- Cloning disks
- Converting file formats
- Wiping disks securely

**Examples:**
```bash
dd if=/dev/sda of=backup.img bs=4M  # Create disk image
dd if=backup.img of=/dev/sdb bs=4M  # Restore from image
dd if=/dev/zero of=/dev/sdb bs=1M count=100  # Wipe first 100MB
dd if=/dev/urandom of=file.txt bs=1M count=10  # Create 10MB file with random data
```

## Permissions & Ownership

### `chmod` - Change permissions
**When to use:**
- Setting file permissions
- Making files executable
- Restricting access to files

**Examples:**
```bash
chmod 755 script.sh      # Set rwxr-xr-x permissions
chmod u+x script.sh      # Add execute permission for owner
chmod g-w file.txt       # Remove write permission for group
chmod o=r file.txt       # Set read-only for others
chmod -R 644 directory/  # Recursively set permissions
```

### `chown` - Change ownership
**When to use:**
- Transferring file ownership
- Setting correct permissions for web servers
- Managing shared files

**Examples:**
```bash
sudo chown user:group file.txt  # Change owner and group
sudo chown user file.txt     # Change owner only
sudo chown :group file.txt    # Change group only
sudo chown -R user /path/to/dir  # Recursively change ownership
```

### `chgrp` - Change group ownership
**When to use:**
- Setting group permissions
- Managing shared resources
- Collaborative file management

**Examples:**
```bash
sudo chgrp developers file.txt  # Change group to developers
sudo chgrp -R staff /path/to/dir  # Recursively change group
```

## Archiving & Compression

### `tar` - Tape archiver
**When to use:**
- Creating backups
- Bundling multiple files
- Preserving directory structures

**Examples:**
```bash
tar -cvf archive.tar file1 file2/  # Create uncompressed archive
tar -xvf archive.tar               # Extract archive
tar -czvf archive.tar.gz folder/   # Create gzip-compressed archive
tar -xzvf archive.tar.gz           # Extract gzip-compressed archive
tar -cjvf archive.tar.bz2 folder/  # Create bzip2-compressed archive
tar -tjf archive.tar.bz2           # List contents of bzip2 archive
```

### `zip`/`unzip`
**When to use:**
- Creating Windows-compatible archives
- Compressing files for email
- Extracting ZIP archives

**Examples:**
```bash
zip -r archive.zip folder/  # Create ZIP archive
unzip archive.zip           # Extract ZIP archive
unzip -l archive.zip        # List contents without extracting
zip -e archive.zip file.txt # Create encrypted ZIP
```

### `gzip`/`gunzip`
**When to use:**
- Compressing single files
- Reducing file sizes for storage
- Preparing files for transfer

**Examples:**
```bash
gzip file.txt              # Compress file (creates file.txt.gz)
gunzip file.txt.gz         # Decompress file
gzip -r directory/         # Compress all files in directory
gzip -d file.txt.gz        # Alternative to gunzip
```

## Search & Find

### `find` - Search files
**When to use:**
- Locating files by name or properties
- Finding files based on content
- Performing actions on found files

**Examples:**
```bash
find /path -name "*.log"       # Find files by name pattern
find /path -type f -mtime +7   # Find files modified >7 days ago
find /path -size +100M         # Find files larger than 100MB
find /path -exec rm {} \;      # Delete found files
find /path -user john          # Find files owned by user john
find /path -perm 755           # Find files with specific permissions
```

### `locate` - Quick file search
**When to use:**
- Quickly finding files by name
- Searching system files
- When speed is more important than recency

**Examples:**
```bash
locate config.ini            # Find all files named config.ini
locate -r '\.conf$'          # Find files ending with .conf
sudo updatedb                # Update file database
```

### `which`/`whereis` - Locate commands
**When to use:**
- Finding executable locations
- Identifying command paths
- Checking which command will be executed

**Examples:**
```bash
which python3                # Show path to python3 executable
whereis ls                   # Show locations for ls (binary, source, man)
whereis -b python            # Show only binary location
```

## Tips & Tricks

### Command Chaining
**When to use:**
- Combining multiple commands
- Creating efficient workflows
- Building complex operations

**Examples:**
```bash
command1 && command2    # Run command2 only if command1 succeeds
command1 || command2    # Run command2 only if command1 fails
command1 ; command2     # Run commands sequentially
command1 | command2     # Pipe output of command1 to command2
```

### Redirection
**When to use:**
- Saving command output
- Hiding error messages
- Combining outputs

**Examples:**
```bash
command > file.txt      # Overwrite output to file
command >> file.txt     # Append output to file
command 2> error.log    # Redirect stderr to file
command 2>&1            # Redirect stderr to stdout
command &> output.log   # Redirect both stdout and stderr
command < input.txt     # Use file as input
```

### History Management
**When to use:**
- Reusing previous commands
- Finding past commands
- Avoiding retyping

**Examples:**
```bash
history                  # Show command history
!50                      # Execute 50th command in history
!!                       # Execute last command
!text                    # Execute last command starting with "text"
Ctrl+R                   # Search command history
history | grep command   # Find commands in history
```

### Getting Help
**When to use:**
- Learning new commands
- Understanding command options
- Troubleshooting command issues

**Examples:**
```bash
man command              # Display manual page
command --help           # Show brief help
whatis command           # Show short description
info command             # Show info documentation
apropos keyword          # Search man pages for keyword
```


