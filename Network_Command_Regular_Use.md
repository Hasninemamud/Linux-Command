# Linux Networking Commands: Regular Day Use

## 1. route

**Purpose**: The `route` command is used to display and manipulate the IP routing table in Linux. It shows how network traffic is directed to different destinations.

**When to use**:
- When you need to view the current routing table
- To add static routes for specific networks
- To troubleshoot network connectivity issues
- When setting up complex network configurations

**Common usage examples**:

```bash
# Display the current routing table
route -n

# Add a default gateway
route add default gw 192.168.1.1

# Add a route to a specific network
route add -net 192.168.2.0 netmask 255.255.255.0 gw 192.168.1.1

# Delete a route
route del -net 192.168.2.0 netmask 255.255.255.0

# Add a route for a specific host
route add -host 10.0.0.5 gw 192.168.1.1

# Reject traffic to a specific network
route add -net 192.168.100.0 netmask 255.255.255.0 reject
```

**Note**: The `route` command is considered legacy in many modern Linux distributions, with `ip route` being the preferred alternative.

## 2. nmap

**Purpose**: Network Mapper (nmap) is a powerful network scanning and discovery tool used to discover hosts and services on a computer network.

**When to use**:
- For network discovery and security auditing
- To identify open ports and services on a network
- To detect operating systems and versions
- For vulnerability scanning
- When managing large networks and inventorying devices

**Common usage examples**:

```bash
# Basic scan of a target
nmap 192.168.1.1

# Scan a specific port
nmap -p 22 192.168.1.1

# Scan a range of ports
nmap -p 1-100 192.168.1.1

# Service version detection
nmap -sV 192.168.1.1

# OS detection
nmap -O 192.168.1.1

# Aggressive scan (enables OS detection, version detection, etc.)
nmap -A 192.168.1.1

# Scan using TCP SYN (stealth scan)
nmap -sS 192.168.1.1

# UDP scan
nmap -sU 192.168.1.1

# Scan an entire network
nmap 192.168.1.0/24

# Save output to a file
nmap -oN scan_results.txt 192.168.1.1

# Script scanning with NSE (Nmap Scripting Engine)
nmap --script vuln 192.168.1.1
```

## 3. Wget

**Purpose**: Wget is a command-line utility for retrieving files from the web using HTTP, HTTPS, and FTP protocols.

**When to use**:
- To download files from the internet
- For mirroring websites
- To resume interrupted downloads
- For automated downloading in scripts
- When you need non-interactive downloading

**Common usage examples**:

```bash
# Download a file
wget https://example.com/file.zip

# Download with a different filename
wget -O newfile.zip https://example.com/file.zip

# Download in the background
wget -b https://example.com/file.zip

# Resume an interrupted download
wget -c https://example.com/file.zip

# Limit download speed
wget --limit-rate=100k https://example.com/file.zip

# Download multiple files from a file containing URLs
wget -i urls.txt

# Mirror a website
wget --mirror --convert-links --adjust-extension https://example.com

# Download files from FTP
wget ftp://example.com/file.zip

# Download with username and password
wget --user=username --password=password ftp://example.com/file.zip

# Retry failed downloads
wget -t 10 https://example.com/file.zip

# Download only specific file types
wget -A pdf,jpg https://example.com/files/
```

## 4. watch

**Purpose**: The `watch` command runs a specified command repeatedly, displaying its output and errors. This allows you to monitor the output of a command over time.

**When to use**:
- To monitor system resources or logs
- To watch for changes in file contents
- For tracking network connections or traffic
- When debugging intermittent issues
- To observe the progress of a process

**Common usage examples**:

```bash
# Monitor system load every 2 seconds (default)
watch uptime

# Monitor disk space usage
watch df -h

# Monitor active network connections
watch netstat -an

# Monitor a log file for changes
watch tail -n 20 /var/log/syslog

# Update every 5 seconds
watch -n 5 uptime

# Highlight differences between updates
watch -d uptime

# Exit if command output changes
watch -g ping -c 1 example.com

# Monitor processes
watch "ps aux | grep python"

# Monitor directory contents
watch ls -l

# Monitor system memory usage
watch free -m

# Monitor network interface statistics
watch cat /proc/net/dev
```

## 5. iptables

**Purpose**: iptables is a user-space utility program that allows a system administrator to configure the IP packet filter rules of the Linux kernel firewall.

**When to use**:
- To set up a firewall for your system
- To filter network traffic
- For Network Address Translation (NAT)
- To implement port forwarding
- When securing a server or network

**Common usage examples**:

```bash
# List all rules
iptables -L

# List rules with line numbers
iptables -L --line-numbers

# List rules for a specific chain
iptables -L INPUT

# Allow incoming SSH connections
iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Allow incoming HTTP and HTTPS traffic
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# Allow traffic from a specific IP
iptables -A INPUT -s 192.168.1.100 -j ACCEPT

# Block traffic from a specific IP
iptables -A INPUT -s 192.168.1.200 -j DROP

# Block outgoing traffic to a specific port
iptables -A OUTPUT -p tcp --dport 25 -j DROP

# Set up port forwarding
iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080

# Enable NAT for a subnet
iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -o eth0 -j MASQUERADE

# Delete a rule by line number
iptables -D INPUT 3

# Insert a rule at a specific position
iptables -I INPUT 2 -p tcp --dport 22 -j ACCEPT

# Save iptables rules (Debian/Ubuntu)
iptables-save > /etc/iptables/rules.v4

# Save iptables rules (RHEL/CentOS)
service iptables save

# Flush all rules (delete all rules)
iptables -F

# Set default policies
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT ACCEPT
```

**Note**: On modern systems using systemd, `iptables` is being replaced by `nftables` as the default firewall framework.

## 6. traceroute

**Purpose**: traceroute is a network diagnostic tool that displays the route (path) and measures transit delays of packets across a network.

**When to use**:
- To diagnose network connectivity issues
- To identify where packets are being dropped
- To determine the network path to a destination
- When troubleshooting slow network connections
- To identify routing loops or misconfigurations

**Common usage examples**:

```bash
# Basic traceroute to a host
traceroute google.com

# Use ICMP instead of UDP
traceroute -I google.com

# Use TCP SYN for traceroute
traceroute -T google.com

# Specify the maximum number of hops
traceroute -m 15 google.com

# Wait a specific amount of time for a response
traceroute -w 2 google.com

# Don't resolve hostnames
traceroute -n google.com

# Set the source interface
traceroute -i eth0 google.com

# Set the destination port
traceroute -p 80 google.com

# Set the packet size
traceroute 1400 google.com

# Use IPv6
traceroute -6 ipv6.google.com
```

**Note**: Some networks may block traceroute packets, which can result in incomplete or failed traces. In such cases, alternative tools like `mtr` or `tracepath` might be useful.

## 7. curl vs wget

Both `curl` and `wget` are command-line tools for transferring data with URLs, but they have different strengths and use cases.

### curl

**Purpose**: curl is a tool for transferring data from or to a server using URLs, supporting many protocols including HTTP, HTTPS, FTP, FTPS, SCP, SFTP, TFTP, LDAP, and more.

**When to use**:
- When you need to interact with APIs
- For testing REST endpoints
- When you need more control over HTTP requests
- To upload files to servers
- When working with multiple protocols beyond HTTP/FTP
- For complex authentication scenarios

**Common usage examples**:

```bash
# Download a file
curl -O https://example.com/file.zip

# Download with a different filename
curl -o newfile.zip https://example.com/file.zip

# Follow redirects
curl -L https://example.com

# Show HTTP headers
curl -I https://example.com

# Send POST request
curl -X POST -d "param1=value1&param2=value2" https://example.com/resource

# Send JSON data
curl -X POST -H "Content-Type: application/json" -d '{"key":"value"}' https://example.com/api

# Include HTTP headers in output
curl -i https://example.com

# Set a custom header
curl -H "Authorization: Bearer token" https://example.com/api

# Use basic authentication
curl -u username:password https://example.com/protected

# Upload a file
curl -T file.txt ftp://ftp.example.com/

# Limit download speed
curl --limit-rate 100K https://example.com/file.zip

# Resume a download
curl -C - -O https://example.com/file.zip

# Use a proxy
curl -x proxy.example.com:8080 https://example.com

# Save cookies to a file
curl -c cookies.txt https://example.com

# Load cookies from a file
curl -b cookies.txt https://example.com
```

### wget vs curl: Key Differences

1. **Scope**:
   - wget is designed for downloading files and website mirroring
   - curl is designed for data transfer and API interaction

2. **Protocols**:
   - wget supports HTTP, HTTPS, and FTP
   - curl supports many more protocols including DICT, FILE, FTP, FTPS, Gopher, HTTP, HTTPS, IMAP, IMAPS, LDAP, LDAPS, POP3, POP3S, RTMP, RTSP, SCP, SFTP, SMB, SMBS, SMTP, SMTPS, Telnet, and TFTP

3. **Recursive downloads**:
   - wget has built-in support for recursive downloads
   - curl does not support recursive downloads natively

4. **Libraries**:
   - curl uses the libcurl library, making it easy to integrate into other applications
   - wget is a standalone application

5. **Output**:
   - wget saves output to files by default
   - curl outputs to stdout by default

6. **POST requests**:
   - curl has more sophisticated support for POST requests and form data
   - wget has limited POST capabilities

**When to choose wget**:
- For simple downloading tasks
- When you need to mirror websites
- For recursive downloading
- In scripts where simplicity is key

**When to choose curl**:
- When working with APIs
- For more control over HTTP requests
- When you need to upload files
- When working with multiple protocols
- For complex authentication scenarios

Both tools are powerful and have their place in a Linux user's toolkit. The choice between them depends on the specific requirements of your task.
