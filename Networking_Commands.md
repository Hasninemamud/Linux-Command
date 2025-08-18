

# Linux Networking Commands: Detailed Usage Guide

## 1. ping
**Purpose**: Tests connectivity between your system and a remote host.
**When to use**: 
- To check if a remote host is reachable
- To measure round-trip time for packets
- To test network connectivity issues

**Common usage**:
```bash
ping google.com                    # Basic ping to a domain
ping 8.8.8.8                       # Ping an IP address
ping -c 4 google.com               # Send exactly 4 packets
ping -i 2 google.com               # Wait 2 seconds between each packet
ping -s 1024 google.com            # Send packets of 1024 bytes
ping -t 10 google.com              # Timeout after 10 seconds
```

## 2. netstat
**Purpose**: Network statistics utility that displays network connections, routing tables, interface statistics, etc.
**When to use**:
- To monitor network connections
- To check which ports are open/listening
- To view network interface statistics

**Common usage**:
```bash
netstat -a                         # Show all connections
netstat -t                         # Show TCP connections
netstat -u                         # Show UDP connections
netstat -l                         # Show only listening ports
netstat -p                         # Show PID/Program name
netstat -n                         # Show numeric addresses (no DNS resolution)
netstat -r                         # Display routing table
netstat -i                         # Show network interface statistics
netstat -s                         # Show network statistics
netstat -tulnp                     # Show all listening TCP/UDP ports with PIDs
```

## 3. ifconfig
**Purpose**: Interface configurator - used to initialize, configure, and display network interfaces.
**When to use**:
- To view network interface configuration
- To assign IP addresses to interfaces
- To enable or disable interfaces

**Common usage**:
```bash
ifconfig                           # Display all active interfaces
ifconfig eth0                      # Display specific interface
ifconfig eth0 up                   # Enable interface
ifconfig eth0 down                 # Disable interface
ifconfig eth0 192.168.1.100        # Assign IP address
ifconfig eth0 netmask 255.255.255.0 # Assign subnet mask
ifconfig eth0 broadcast 192.168.1.255 # Assign broadcast address
ifconfig eth0 mtu 1500             # Set Maximum Transmission Unit
```

## 4. traceroute vs tracepath
**Purpose**: Both commands trace the network path to a destination, showing intermediate hops.

### traceroute
**When to use**:
- To discover the route packets take to a network host
- To identify network bottlenecks or failures

**Common usage**:
```bash
traceroute google.com              # Trace route to domain
traceroute -n google.com           # Don't resolve hostnames
traceroute -I google.com           # Use ICMP ECHO for probes
traceroute -T google.com           # Use TCP SYN for probes
traceroute -w 2 google.com         # Wait 2 seconds for response
```

### tracepath
**When to use**:
- Similar to traceroute but doesn't require special privileges
- When traceroute is not available

**Common usage**:
```bash
tracepath google.com               # Trace path to domain
tracepath -n google.com            # Don't resolve hostnames
tracepath -b google.com            # Show both hop address and hostname
```

## 5. mtr
**Purpose**: "My Traceroute" - combines traceroute and ping functionality in a single network diagnostic tool.
**When to use**:
- For continuous network monitoring
- To identify packet loss at specific hops
- When you need more detailed statistics than traceroute

**Common usage**:
```bash
mtr google.com                     # Run mtr in interactive mode
mtr -n google.com                  # Don't resolve hostnames
mtr -c 10 google.com               # Send 10 packets to each hop
mtr -r google.com                  # Report mode (single run)
mtr -w google.com                  # Use wide mode for long hostnames
mtr --tcp google.com               # Use TCP instead of ICMP
```

## 6. nslookup
**Purpose**: Name Server Lookup - queries DNS servers to obtain domain name or IP address mapping.
**When to use**:
- To query DNS information
- To troubleshoot DNS resolution issues
- To find MX records, NS records, etc.

**Common usage**:
```bash
nslookup google.com                # Basic DNS query
nslookup 8.8.8.8                   # Reverse DNS lookup
nslookup -type=MX google.com       # Query MX records
nslookup -type=NS google.com       # Query NS records
nslookup -type=A google.com        # Query A records
nslookup -type=ANY google.com      # Query all records
nslookup -debug google.com         # Show debug information
```

## 7. telnet
**Purpose**: Teletype Network - used for bidirectional interactive text-oriented communication over a network.
**When to use**:
- To test connectivity to a specific port
- To connect to remote systems (less secure than SSH)
- For basic network troubleshooting

**Common usage**:
```bash
telnet example.com                 # Connect to default telnet port (23)
telnet example.com 80              # Connect to specific port (HTTP)
telnet 192.168.1.1                 # Connect to IP address
telnet -l user example.com         # Specify username
```

## 8. hostname
**Purpose**: Display or set the system's host name.
**When to use**:
- To check the current hostname of the system
- To temporarily change the hostname
- To view domain information

**Common usage**:
```bash
hostname                           # Show current hostname
hostname -f                        # Show fully qualified domain name
hostname -i                        # Show IP address of the host
hostname -d                        # Show domain name
hostname new-hostname              # Set temporary hostname
hostname -b new-hostname           # Set hostname permanently (on some systems)
```

## 9. ip
**Purpose**: Show/manipulate routing, network devices, interfaces, and tunnels (modern replacement for ifconfig).
**When to use**:
- To view network interface information
- To manage IP addresses
- To manage routing tables
- For modern network configuration

**Common usage**:
```bash
ip addr show                       # Display all interfaces and addresses
ip link show                       # Display network interfaces
ip link set eth0 up                # Bring interface up
ip link set eth0 down              # Bring interface down
ip addr add 192.168.1.100/24 dev eth0 # Add IP address to interface
ip addr del 192.168.1.100/24 dev eth0 # Remove IP address
ip route show                      # Display routing table
ip route add default via 192.168.1.1 # Add default route
ip neigh show                      # Show ARP table
```

## 10. iwconfig
**Purpose**: Configure wireless network interfaces.
**When to use**:
- To view or configure wireless network parameters
- To set wireless network modes
- To manage wireless connections

**Common usage**:
```bash
iwconfig                           # Show all wireless interfaces
iwconfig wlan0                     # Show specific wireless interface
iwconfig wlan0 essid "NetworkName" # Connect to network by ESSID
iwconfig wlan0 mode managed        # Set mode to managed
iwconfig wlan0 key s:password      # Set WEP key
iwconfig wlan0 channel 6           # Set channel
iwconfig wlan0 rate 54M            # Set data rate
```

## 11. ss
**Purpose**: Socket Statistics - utility to investigate sockets (replacement for netstat).
**When to use**:
- To display socket statistics
- To monitor network connections
- For faster and more detailed output than netstat

**Common usage**:
```bash
ss -t                              # Show TCP sockets
ss -u                              # Show UDP sockets
ss -a                              # Show all sockets
ss -l                              # Show listening sockets
ss -n                              # Show numeric addresses (no DNS)
ss -p                              # Show process using socket
ss -t -a                           # Show all TCP sockets
ss -u -a                           # Show all UDP sockets
ss -t -n -p                        # Show TCP sockets with numeric addresses and processes
```

## 12. arp
**Purpose**: Address Resolution Protocol - manipulates or displays the kernel's ARP cache.
**When to use**:
- To view the ARP cache (IP to MAC address mappings)
- To manually add or remove ARP entries
- To troubleshoot ARP-related issues

**Common usage**:
```bash
arp -a                             # Show all ARP entries
arp -v                             # Show verbose information
arp -n                             # Show numeric addresses (no DNS)
arp -i eth0                        # Show ARP entries for specific interface
arp -s 192.168.1.100 00:11:22:33:44:55 # Add static ARP entry
arp -d 192.168.1.100               # Delete ARP entry
```

## 13. dig
**Purpose**: Domain Information Groper - DNS lookup utility (more detailed than nslookup).
**When to use**:
- For detailed DNS queries
- To troubleshoot DNS issues
- To view complete DNS response information

**Common usage**:
```bash
dig google.com                     # Basic DNS query
dig google.com ANY                 # Query all records
dig google.com MX                  # Query MX records
dig -x 8.8.8.8                     # Reverse DNS lookup
dig +short google.com              # Show only answer section
dig +trace google.com              # Trace the DNS path
dig @8.8.8.8 google.com            # Use specific DNS server
```

## 14. nc (netcat)
**Purpose**: Network utility for reading from and writing to network connections using TCP or UDP.
**When to use**:
- To create network connections
- To transfer files between systems
- To port scan
- For network debugging

**Common usage**:
```bash
nc -l -p 1234                      # Listen on port 1234
nc example.com 80                  # Connect to port 80 on example.com
nc -z -v example.com 80            # Scan if port 80 is open
nc -l -p 1234 > received_file.txt  # Receive a file
nc example.com 1234 < file.txt     # Send a file
nc -u -l -p 1234                   # Listen on UDP port 1234
nc -v -w 2 -z example.com 20-30    # Port scan ports 20-30
```

## 15. whois
**Purpose**: Client for the whois directory service.
**When to use**:
- To look up domain registration information
- To find contact information for a domain
- To check domain availability

**Common usage**:
```bash
whois google.com                   # Basic whois query
whois 8.8.8.8                      # Whois for IP address
whois -h whois.example.com domain.com # Use specific whois server
whois -H domain.com                # Hide legal disclaimers
```

## 16. ifplugstatus
**Purpose**: Detects link status in network interfaces.
**When to use**:
- To check if a network cable is plugged in
- To verify interface link status
- For automated network configuration scripts

**Common usage**:
```bash
ifplugstatus                       # Check status of all interfaces
ifplugstatus eth0                  # Check status of specific interface
ifplugstatus -a                    # Check all interfaces
ifplugstatus -q                    # Quiet mode (exit code indicates status)
```

