# Linux Networking Commands for DevOps

This guide covers essential Linux networking commands that DevOps engineers need to manage, troubleshoot, and optimize network infrastructure.

---

## 🔍 Network Interface Management

### 1. **ip** - Modern Network Configuration
**Purpose**: The modern replacement for `ifconfig` with enhanced capabilities.

```bash
# Show all network interfaces
ip addr show
# or
ip a

# Show specific interface
ip addr show eth0

# Bring interface up/down
ip link set eth0 up
ip link set eth0 down

# Add IP address
ip addr add 192.168.1.10/24 dev eth0

# Remove IP address
ip addr del 192.168.1.10/24 dev eth0

# Show routing table
ip route show
# or
ip r

# Add default route
ip route add default via 192.168.1.1

# Add static route
ip route add 10.0.0.0/24 via 192.168.1.1

# Show neighbor table (ARP)
ip neigh show

# Flush ARP cache
ip neigh flush all

# Show network statistics
ip -s link show eth0
```

### 2. **ifconfig** - Legacy Interface Configuration
**Purpose**: Traditional network interface configuration (being phased out but still common).

```bash
# Show all interfaces
ifconfig

# Show specific interface
ifconfig eth0

# Configure IP address
ifconfig eth0 192.168.1.10 netmask 255.255.255.0

# Bring interface up/down
ifconfig eth0 up
ifconfig eth0 down

# Set MTU size
ifconfig eth0 mtu 1500
```

---

## 🌐 Network Connectivity Testing

### 3. **ping** - Test Network Connectivity
**Purpose**: Test reachability of network hosts and measure latency.

```bash
# Basic ping
ping google.com

# Send specific number of packets
ping -c 4 google.com

# Continuous ping with timestamp
ping -D google.com

# Flood ping (use with caution)
ping -f google.com

# Specify interface
ping -I eth0 google.com

# Ping without waiting for reply
ping -q -c 1 google.com

# Set packet size
ping -s 1500 google.com
```

### 4. **traceroute** - Trace Network Path
**Purpose**: Display the route packets take to a network host.

```bash
# Basic traceroute
traceroute google.com

# Use ICMP instead of UDP
traceroute -I google.com

# Set maximum hops
traceroute -m 15 google.com

# Don't resolve hostnames
traceroute -n google.com

# Set packet size
traceroute google.com 1500
```

### 5. **tracepath** - Simple Path Tracer
**Purpose**: Similar to traceroute but doesn't require special privileges.

```bash
# Basic tracepath
tracepath google.com

# Set maximum hops
tracepath -m 15 google.com
```

### 6. **mtr** - Network Diagnostic Tool
**Purpose**: Combines ping and traceroute functionality in a single tool.

```bash
# Basic mtr
mtr google.com

# Use report mode
mtr --report google.com

# Set packet size
mtr --report --psize=1500 google.com

# Show numeric IPs only
mtr --no-dns google.com

# Set interval between pings
mtr --interval=0.5 google.com
```

---

## 📊 Network Statistics and Monitoring

### 7. **netstat** - Network Statistics
**Purpose**: Display network connections, routing tables, interface statistics, masquerade connections, and multicast memberships.

```bash
# Show all connections
netstat -a

# Show TCP connections
netstat -t

# Show UDP connections
netstat -u

# Show listening ports
netstat -l

# Show numeric addresses (no DNS resolution)
netstat -n

# Show PID/program name
netstat -p

# Show routing table
netstat -r

# Show interface statistics
netstat -i

# Show multicast group memberships
netstat -g

# Show all TCP/UDP connections with numeric addresses and PIDs
netstat -tunp

# Show listening TCP ports with PIDs
netstat -tlnp
```

### 8. **ss** - Socket Statistics
**Purpose**: Modern replacement for netstat with more features and better performance.

```bash
# Show all connections
ss -a

# Show TCP connections
ss -t

# Show UDP connections
ss -u

# Show listening sockets
ss -l

# Show process information
ss -p

# Show numeric addresses
ss -n

# Show summary statistics
ss -s

# Show all TCP sockets with process info
ss -tp

# Show listening TCP ports with process info
ss -tlnp

# Show socket memory usage
ss -m

# Show timer information
ss -o

# Filter by state
ss state established
ss state time-wait
```

### 9. **nload** - Network Traffic Monitor
**Purpose**: Monitor network traffic and bandwidth usage in real-time.

```bash
# Monitor all interfaces
nload

# Monitor specific interface
nload eth0

# Update interval in milliseconds
nload -t 500

# Set units (bits, bytes, kbits, kbytes)
nload -u m
```

### 10. **iftop** - Interface Bandwidth Monitor
**Purpose**: Display bandwidth usage on an interface by host.

```bash
# Monitor default interface
iftop

# Monitor specific interface
iftop -i eth0

# Don't show hostname lookups
iftop -n

# Show port numbers
iftop -P

# Use specific network mask
iftop -F 192.168.1.0/24
```

### 11. **nethogs** - Network Bandwidth Monitoring by Process
**Purpose**: Show network bandwidth usage by process.

```bash
# Monitor all interfaces
nethogs

# Monitor specific interface
nethogs eth0

# Show in KB/s
nethogs -k

# Show in MB/s
nethogs -m

# Show in bits/s
nethogs -b

# Refresh delay in seconds
nethogs -t 3
```

---

## 🔒 Network Security

### 12. **iptables** - Packet Filtering and NAT
**Purpose**: Configure Linux kernel firewall (IPv4).

```bash
# Show all rules
iptables -L -v -n

# Show rules for specific chain
iptables -L INPUT -v -n

# Allow incoming SSH
iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Allow HTTP/HTTPS
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# Allow established connections
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# Allow loopback
iptables -A INPUT -i lo -j ACCEPT

# Block all other incoming
iptables -A INPUT -j DROP

# Save rules (Debian/Ubuntu)
iptables-save > /etc/iptables/rules.v4

# Save rules (RHEL/CentOS)
service iptables save

# NAT configuration
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

# Port forwarding
iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080

# Delete rule
iptables -D INPUT -p tcp --dport 22 -j ACCEPT

# Flush all rules
iptables -F
iptables -t nat -F
```

### 13. **nftables** - Modern Packet Filtering Framework
**Purpose**: Modern replacement for iptables with simplified syntax.

```bash
# Show all rules
nft list ruleset

# Create a table
nft add table inet filter

# Create a chain
nft add chain inet filter input { type filter hook input priority 0 \; }

# Add rules
nft add rule inet filter input tcp dport 22 accept
nft add rule inet filter input tcp dport { 80, 443 } accept
nft add rule inet filter input iif "lo" accept
nft add rule inet filter input ct state established,related accept
nft add rule inet filter input drop

# Save rules
nft list ruleset > /etc/nftables.conf

# Load rules
nft -f /etc/nftables.conf
```

### 14. **ufw** - Uncomplicated Firewall
**Purpose**: Simplified firewall management (Ubuntu default).

```bash
# Enable firewall
sudo ufw enable

# Disable firewall
sudo ufw disable

# Show status
sudo ufw status

# Allow SSH
sudo ufw allow ssh

# Allow specific port
sudo ufw allow 8080/tcp

# Allow service by name
sudo ufw allow http
sudo ufw allow https

# Allow IP range
sudo ufw allow from 192.168.1.0/24

# Deny specific port
sudo ufw deny 25

# Delete rule
sudo ufw delete allow 8080/tcp

# Reset to default
sudo ufw reset

# Enable logging
sudo ufw logging on

# Show verbose status
sudo ufw status verbose
```

### 15. **firewalld** - Dynamic Firewall Manager
**Purpose**: Dynamic firewall management (RHEL/CentOS default).

```bash
# Start and enable firewalld
sudo systemctl start firewalld
sudo systemctl enable firewalld

# Check status
sudo firewall-cmd --state

# Show active zones
sudo firewall-cmd --get-active-zones

# Show default zone
sudo firewall-cmd --get-default-zone

# Show configuration
sudo firewall-cmd --list-all

# Add permanent rule
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-port=8080/tcp

# Reload configuration
sudo firewall-cmd --reload

# Add temporary rule (lost on reload)
sudo firewall-cmd --add-service=https

# Remove service
sudo firewall-cmd --permanent --remove-service=http

# Add port forwarding
sudo firewall-cmd --permanent --add-forward-port=port=80:proto=tcp:toport=8080

# Add rich rule
sudo firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="192.168.1.0/24" service name="ssh" accept'

# Panic mode (block all traffic)
sudo firewall-cmd --panic-on
sudo firewall-cmd --panic-off
```

---

## 🔍 Network Troubleshooting

### 16. **nslookup** - DNS Query Tool
**Purpose**: Query DNS servers for information.

```bash
# Basic DNS query
nslookup google.com

# Query specific DNS server
nslookup google.com 8.8.8.8

# Reverse DNS lookup
nslookup 8.8.8.8

# Query MX records
nslookup -type=mx google.com

# Query NS records
nslookup -type=ns google.com

# Query SOA records
nslookup -type=soa google.com

# Query all records
nslookup -type=any google.com
```

### 17. **dig** - DNS Information Groper
**Purpose**: Advanced DNS query tool with more features than nslookup.

```bash
# Basic DNS query
dig google.com

# Query specific record type
dig google.com A
dig google.com MX
dig google.com NS

# Reverse DNS lookup
dig -x 8.8.8.8

# Query specific DNS server
dig @8.8.8.8 google.com

# Short answer
dig +short google.com

# Show all DNS records
dig google.com ANY

# Trace DNS delegation
dig +trace google.com

# Query with DNSSEC
dig +dnssec google.com

# Show only answer section
dig +noall +answer google.com
```

### 18. **host** - DNS Lookup Utility
**Purpose**: Simple DNS lookup utility.

```bash
# Basic lookup
host google.com

# Lookup specific record type
host -t MX google.com

# Reverse lookup
host 8.8.8.8

# Use specific DNS server
host google.com 8.8.8.8

# Show all information
host -a google.com

# Verbose output
host -v google.com
```

### 19. **tcpdump** - Packet Analyzer
**Purpose**: Capture and analyze network traffic.

```bash
# Capture on default interface
tcpdump

# Capture on specific interface
tcpdump -i eth0

# Save capture to file
tcpdump -w capture.pcap

# Read capture file
tcpdump -r capture.pcap

# Show human-readable timestamps
tcpdump -tttt

# Capture specific port
tcpdump port 80

# Capture specific host
tcpdump host 192.168.1.1

# Capture specific protocol
tcpdump icmp

# Capture with filter expression
tcpdump "src host 192.168.1.1 and dst port 80"

# Capture in hex and ASCII
tcpdump -X

# Limit number of packets
tcpdump -c 100

# Capture without DNS resolution
tcpdump -n
```

### 20. **wireshark** - Network Protocol Analyzer
**Purpose**: GUI-based network protocol analyzer (can also use tshark for CLI).

```bash
# Command-line version (tshark)
# Capture on interface
tshark -i eth0

# Capture to file
tshark -i eth0 -w capture.pcap

# Read capture file
tshark -r capture.pcap

# Show only HTTP traffic
tshark -i eth0 -Y "http"

# Show only specific host
tshark -i eth0 -Y "host 192.168.1.1"

# Display filter
tshark -r capture.pcap -Y "tcp.port == 80"

# Show statistics
tshark -z conv,tcp
```

### 21. **nc** / **netcat** - Network Swiss Army Knife
**Purpose**: Read from and write to network connections using TCP or UDP.

```bash
# Listen on TCP port
nc -l -p 8080

# Connect to TCP port
nc 192.168.1.1 8080

# Listen on UDP port
nc -u -l -p 8080

# Connect to UDP port
nc -u 192.168.1.1 8080

# Port scan
nc -z -v 192.168.1.1 1-100

# Transfer file (receiver)
nc -l -p 8080 > received_file.txt

# Transfer file (sender)
nc 192.168.1.1 8080 < file_to_send.txt

# Chat server
nc -l -p 8080

# Chat client
nc 192.168.1.1 8080

# Simple HTTP server
echo -e "HTTP/1.1 200 OK\r\n\r\nHello World" | nc -l -p 8080
```

### 22. **socat** - Multipurpose Relay
**Purpose**: More advanced version of netcat with bidirectional data streams.

```bash
# TCP port forward
socat TCP-LISTEN:8080,fork TCP:192.168.1.1:80

# Serial port over TCP
socat TCP-LISTEN:8080,fork FILE:/dev/ttyS0,b115200,raw

# Connect to serial port
socat TCP:192.168.1.1:8080 FILE:/dev/ttyUSB0,b115200,raw

# UDP broadcast
socat UDP-DATAGRAM:192.168.1.255:8080,broadcast,source-port=8080 -

# Create virtual network interface
socat TUN:192.168.100.1/24,up TUN:192.168.100.2/24,up
```

---

## 📡 Network Configuration Files

### 23. **Network Configuration Management**

#### Debian/Ubuntu (Netplan)
```bash
# View Netplan configuration
cat /etc/netplan/*.yaml

# Apply Netplan configuration
sudo netplan apply

# Test Netplan configuration
sudo netplan try

# Generate configuration
sudo netplan generate
```

#### RHEL/CentOS (NetworkManager)
```bash
# Show NetworkManager status
nmcli general status

# Show all connections
nmcli connection show

# Show active connections
nmcli connection show --active

# Show device status
nmcli device status

# Configure connection
nmcli connection add type ethernet con-name "eth0" ifname eth0 ip4 192.168.1.10/24 gw4 192.168.1.1

# Set DNS
nmcli connection modify eth0 ipv4.dns "8.8.8.8 8.8.4.4"

# Bring connection up/down
nmcli connection up eth0
nmcli connection down eth0

# Show connection details
nmcli connection show eth0
```

#### Static Configuration Files
```bash
# View interfaces file (Debian/Ubuntu)
cat /etc/network/interfaces

# View ifcfg files (RHEL/CentOS)
cat /etc/sysconfig/network-scripts/ifcfg-eth0

# View resolv.conf
cat /etc/resolv.conf

# View hosts file
cat /etc/hosts
```

---

## 🌐 Network Services

### 24. **systemd-networkd** - Network Configuration Service
**Purpose**: System network configuration service for systemd-based systems.

```bash
# Enable and start systemd-networkd
sudo systemctl enable systemd-networkd
sudo systemctl start systemd-networkd

# Check status
sudo systemctl status systemd-networkd

# View network configuration
networkctl

# View link status
networkctl status

# View all links
networkctl list

# Test DNS resolution
systemd-resolve google.com

# View DNS configuration
systemd-resolve --status
```

### 25. **Network Time Protocol (NTP)**
**Purpose**: Synchronize system time across network.

```bash
# Check NTP status
timedatectl status

# Enable NTP synchronization
sudo timedatectl set-ntp true

# List NTP peers
ntpq -p

# Force time sync
sudo ntpd -gq

# Check time server
chronyc sources
```

---

## 🚀 DevOps Networking Use Cases

### 1. **Network Configuration Script**
```bash
#!/bin/bash
# Configure network interface
IP="192.168.1.10"
NETMASK="24"
GATEWAY="192.168.1.1"
DNS="8.8.8.8 8.8.4.4"

# Set IP address
sudo ip addr add $IP/$NETMASK dev eth0

# Set default route
sudo ip route add default via $GATEWAY

# Set DNS
echo "nameserver $DNS" | sudo tee /etc/resolv.conf

# Bring interface up
sudo ip link set eth0 up

# Verify configuration
ip addr show eth0
ip route show
```

### 2. **Port Monitoring Script**
```bash
#!/bin/bash
# Monitor critical ports
HOST="localhost"
PORTS=(22 80 443 8080)

for port in "${PORTS[@]}"; do
  if nc -z $HOST $port; then
    echo "Port $port is open"
  else
    echo "Port $port is closed"
  fi
done
```

### 3. **Network Traffic Capture Script**
```bash
#!/bin/bash
# Capture traffic for specific host
HOST="192.168.1.100"
INTERFACE="eth0"
DURATION="60"  # seconds

sudo tcpdump -i $INTERFACE -w capture_$HOST.pcap host $HOST &
TCPDUMP_PID=$!

sleep $DURATION
sudo kill $TCPDUMP_PID

echo "Capture saved to capture_$HOST.pcap"
```

### 4. **Firewall Configuration Script**
```bash
#!/bin/bash
# Configure basic firewall
# Flush existing rules
sudo iptables -F
sudo iptables -t nat -F

# Allow loopback
sudo iptables -A INPUT -i lo -j ACCEPT

# Allow established connections
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# Allow SSH
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Allow HTTP/HTTPS
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# Block all other incoming
sudo iptables -A INPUT -j DROP

# Save rules
sudo iptables-save > /etc/iptables/rules.v4

echo "Firewall configured successfully"
```

## 🔍 Troubleshooting Tips

### Common Network Issues and Solutions:

#### 1. No Network Connectivity
```bash
# Check interface status
ip addr show

# Check link status
ip link show

# Check routing
ip route show

# Test DNS resolution
nslookup google.com

# Test connectivity
ping 8.8.8.8
```

#### 2. Port Not Accessible
```bash
# Check if service is listening
ss -tlnp | grep :port

# Check firewall rules
sudo iptables -L -v -n

# Test port locally
nc -z localhost port

# Test port remotely
nc -z host port
```

#### 3. Slow Network Performance
```bash
# Check interface errors
ip -s link show

# Check network utilization
iftop

# Check for packet loss
ping -c 100 host | grep "packet loss"

# Check network statistics
netstat -i
```

#### 4. DNS Resolution Issues
```bash
# Check DNS configuration
cat /etc/resolv.conf

# Test DNS resolution
nslookup google.com
dig google.com

# Test with different DNS server
nslookup google.com 8.8.8.8

# Check DNS service status
sudo systemctl status systemd-resolved
```

---

