# Users & Groups Management in Linux

## 🧑‍💻 User Management Commands

### 1. **Viewing User Information**

#### List all users
```bash
cat /etc/passwd          # View all user accounts
cut -d: -f1 /etc/passwd  # List just usernames
```

#### View user details
```bash
id username              # Show UID, GID, and groups
finger username          # Display user information
getent passwd username   # Get user details from all sources
```

#### Check logged-in users
```bash
who                      # Show currently logged-in users
w                        # Show users and their processes
last                     # Show last login history
lastlog                  # Show last login for all users
```

### 2. **Creating Users**

#### Basic user creation
```bash
sudo useradd username    # Create user (no home directory)
sudo useradd -m username # Create with home directory
sudo useradd -m -s /bin/bash username  # With specific shell
```

#### Create user with custom settings
```bash
sudo useradd \
  -m \                    # Create home directory
  -s /bin/bash \          # Set shell
  -c "Full Name" \        # Set comment/real name
  -G sudo,developers \    # Add to supplementary groups
  -e 2023-12-31 \        # Set expiration date
  username
```

#### Set password
```bash
sudo passwd username      # Set password interactively
echo "username:password" | sudo chpasswd  # Set password non-interactively
```

### 3. **Modifying Users**

#### Change user properties
```bash
sudo usermod -l newname oldname    # Rename user
sudo usermod -d /new/home username # Change home directory
sudo usermod -s /bin/zsh username  # Change default shell
sudo usermod -c "New Name" username # Change comment
sudo usermod -e 2024-12-31 username # Change expiration date
```

#### Group management
```bash
sudo usermod -aG groupname username  # Add user to group (without removing existing groups)
sudo usermod -G groupname username   # Replace user's supplementary groups
sudo usermod -g groupname username   # Change primary group
```

#### Lock/unlock accounts
```bash
sudo usermod -L username      # Lock user account
sudo usermod -U username      # Unlock user account
sudo passwd -l username       # Alternative lock method
sudo passwd -u username       # Alternative unlock method
```

### 4. **Deleting Users**

#### Basic user deletion
```bash
sudo userdel username        # Delete user but keep home directory
sudo userdel -r username     # Delete user and home directory
sudo userdel -rf username    # Force delete even if files exist elsewhere
```

#### Remove user from groups
```bash
sudo gpasswd -d username groupname  # Remove user from specific group
```

---

## 👥 Group Management Commands

### 1. **Viewing Groups**

#### List all groups
```bash
cat /etc/group           # View all groups
cut -d: -f1 /etc/group   # List just group names
getent group             # Show groups from all sources
```

#### View group members
```bash
getent group groupname   # Show group and its members
groups username          # Show groups a user belongs to
```

### 2. **Creating Groups**

#### Basic group creation
```bash
sudo groupadd groupname  # Create group
sudo groupadd -g 1500 groupname  # Create with specific GID
```

#### System groups
```bash
sudo groupadd -r groupname  # Create system group (low GID)
```

### 3. **Modifying Groups**

#### Change group properties
```bash
sudo groupmod -n newname oldname  # Rename group
sudo groupmod -g 2000 groupname   # Change GID
```

#### Set group password
```bash
sudo gpasswd groupname    # Set group password
```

### 4. **Deleting Groups**
```bash
sudo groupdel groupname   # Delete group
```

### 5. **Group Administration**

#### Add users to groups
```bash
sudo gpasswd -a username groupname  # Add user to group
sudo gpasswd -M user1,user2 groupname  # Set group members (replaces existing)
```

#### Remove users from groups
```bash
sudo gpasswd -d username groupname  # Remove user from group
```

#### Group administrators
```bash
sudo gpasswd -A admin1,admin2 groupname  # Set group administrators
```

---

## 🔐 Permission Management

### 1. **File Permissions**

#### Basic permissions
```bash
chmod 755 file.txt      # rwxr-xr-x
chmod 644 file.txt      # rw-r--r--
chmod u+x script.sh     # Add execute for owner
chmod go-w file.txt     # Remove write for group/others
```

#### Special permissions
```bash
chmod u+s file.txt      # Set SUID (run as owner)
chmod g+s directory/    # Set SGID (inherit group)
chmod +t directory/     # Set sticky bit (restrict deletion)
```

### 2. **Ownership Management**

#### Change file ownership
```bash
sudo chown user:group file.txt    # Change owner and group
sudo chown user file.txt          # Change owner only
sudo chown :group file.txt        # Change group only
```

#### Recursive ownership changes
```bash
sudo chown -R user:group directory/  # Recursive change
```

### 3. **Default Permissions**

#### Set default ACL
```bash
sudo setfacl -d -m u:user:rwx directory/  # Default permissions for new files
sudo setfacl -R -m u:user:rwx directory/  # Apply to existing files
```

#### View ACLs
```bash
getfacl file.txt         # View file ACLs
getfacl directory/       # View directory ACLs
```

---

## 🛠️ Advanced User Management

### 1. **User Environment Management**

#### User shell management
```bash
chsh -s /bin/zsh username  # Change user's shell
chsh -l                   # List available shells
```

#### User limits
```bash
sudo vi /etc/security/limits.conf  # Edit user limits
ulimit -a                   # View current limits
```

### 2. **Password Management**

#### Password policies
```bash
sudo chage -l username     # View password aging info
sudo chage -M 90 username  # Set max password age
sudo chage -W 7 username   # Set warning days
sudo chage -E 2023-12-31 username  # Set account expiration
```

#### Force password change
```bash
sudo chage -d 0 username  # Force password change on next login
```

### 3. **User Templates**

#### Skeleton directory
```bash
ls /etc/skel/             # View template files
sudo cp /etc/skel/.bashrc /home/newuser/  # Copy template to existing user
```

### 4. **Batch User Management**

#### Create multiple users
```bash
# Create users from file
while read username; do
  sudo useradd -m "$username"
  echo "$username:TempPass123!" | sudo chpasswd
done < users.txt
```

#### Add multiple users to group
```bash
sudo gpasswd -M user1,user2,user3 groupname
```

---

## 📊 Auditing and Monitoring

### 1. **User Activity Monitoring**

#### Login monitoring
```bash
last username            # Show user's login history
lastb                   # Show failed login attempts
sudo journalctl -u sshd  # View SSH logs
```

#### Process monitoring
```bash
ps -u username          # Show user's processes
top -u username         # Monitor user's processes
```

### 2. **Security Auditing**

#### Check for users with UID 0
```bash
awk -F: '($3 == 0) { print $1 }' /etc/passwd
```

#### Check for users without passwords
```bash
sudo awk -F: '($2 == "") { print $1 }' /etc/shadow
```

#### Check for duplicate UIDs
```bash
cut -d: -f3 /etc/passwd | sort -n | uniq -d
```

### 3. **File System Auditing**

#### Find files with SUID/SGID
```bash
sudo find / -perm -4000 -type f  # Find SUID files
sudo find / -perm -2000 -type f  # Find SGID files
```

#### Find world-writable files
```bash
sudo find / -perm -0002 -type f  # Find world-writable files
```

---

## 🚀 DevOps Use Cases

### 1. **User Provisioning Script**
```bash
#!/bin/bash
# Create developer user with proper setup
username=$1
sudo useradd -m -s /bin/bash -G sudo,developers,docker "$username"
echo "$username:TempPass123!" | sudo chpasswd
sudo chage -d 0 "$username"
sudo -u "$username" ssh-keygen -t rsa -b 4096 -f /home/"$username"/.ssh/id_rsa -N ""
```

### 2. **Service Account Setup**
```bash
#!/bin/bash
# Create service account for application
appuser="app-service"
sudo groupadd "$appuser"
sudo useradd -r -s /bin/false -g "$appuser" "$appuser"
sudo mkdir -p /opt/app
sudo chown -R "$appuser:$appuser" /opt/app
```

### 3. **User Cleanup Script**
```bash
#!/bin/bash
# Disable inactive users
inactive_days=90
sudo lastlog -b "$inactive_days" | grep -v "Username" | awk '{print $1}' | while read user; do
  sudo usermod -L "$user"
  echo "User $user locked due to inactivity"
done
```

### 4. **Permission Fix Script**
```bash
#!/bin/bash
# Fix web directory permissions
sudo chown -R www-data:www-data /var/www
sudo find /var/www -type d -exec chmod 755 {} \;
sudo find /var/www -type f -exec chmod 644 {} \;
```

---

## 📚 Best Practices

1. **Security**
   - Use strong passwords and enforce rotation
   - Lock inactive accounts
   - Regularly audit user accounts and permissions
   - Use `sudo` instead of root login

2. **Organization**
   - Create meaningful group names
   - Use groups to manage permissions
   - Document user account purposes
   - Implement naming conventions

3. **Automation**
   - Use configuration management tools (Ansible, Puppet) for user management
   - Automate user provisioning and deprovisioning
   - Implement centralized authentication (LDAP, SSSD) for large environments

4. **Monitoring**
   - Regularly review user access
   - Monitor for suspicious activity
   - Implement logging for all user management actions
   - Use tools like `auditd` for security auditing

---

## 🔍 Troubleshooting

### Common Issues and Solutions:

#### 1. "User is not in sudoers file"
```bash
# Fix by adding user to sudo group
sudo usermod -aG sudo username
# Or edit sudoers file
sudo visudo
# Add: username ALL=(ALL) ALL
```

#### 2. "Permission denied" after file creation
```bash
# Check ownership and permissions
ls -l file.txt
# Fix ownership
sudo chown user:group file.txt
# Fix permissions
chmod 644 file.txt
```

#### 3. User can't login
```bash
# Check if shell exists
grep username /etc/passwd
# Check if account is locked
sudo passwd -S username
# Unlock if needed
sudo usermod -U username
```

#### 4. Group changes not taking effect
```bash
# User needs to logout/login or run
newgrp groupname
# Or start new shell
su - username
```

---


Always test commands in safe environments before applying to production systems, and implement proper change management procedures for user and permission changes.
