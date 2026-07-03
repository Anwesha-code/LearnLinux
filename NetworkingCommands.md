# 🌐 Linux Networking Cheat Sheet

> A quick reference for the most commonly used Linux networking commands.


---

# 📚 Command Categories

## 🌍 Network Interfaces

| Command | Purpose |
|----------|---------|
| `ip` | Modern utility for managing interfaces, routes, neighbors, and addresses |
| `ifconfig` | Legacy network interface configuration |
| `iw` | Configure wireless interfaces |
| `iwconfig` | Legacy wireless configuration |
| `nmcli` | NetworkManager command-line interface |

---

## 📡 Connectivity Testing

| Command | Purpose |
|----------|---------|
| `ping` | Test connectivity using ICMP |
| `traceroute` | Show the path packets take to a destination |
| `tracepath` | Lightweight traceroute |
| `mtr` | Combines ping and traceroute with live statistics |

---

## 🌐 DNS

| Command | Purpose |
|----------|---------|
| `dig` | Advanced DNS lookup utility |
| `nslookup` | Query DNS servers |
| `host` | Resolve hostnames and IP addresses |
| `resolvectl` | View and manage system DNS settings |

---

## 🛣️ Routing

| Command | Purpose |
|----------|---------|
| `ip route` | View and modify routing tables |
| `route` | Legacy routing utility |

---

## 🔗 ARP & Neighbor Discovery

| Command | Purpose |
|----------|---------|
| `arp` | View or modify the ARP cache |
| `ip neigh` | View neighbor table (modern replacement for arp) |

---

## 🔌 Network Connections

| Command | Purpose |
|----------|---------|
| `ss` | Display socket statistics (modern replacement for netstat) |
| `netstat` | Display network connections and routing information |
| `lsof -i` | Show processes using network ports |

---

## 📥 HTTP & Downloads

| Command | Purpose |
|----------|---------|
| `curl` | Transfer data using HTTP, HTTPS, FTP, etc. |
| `wget` | Download files from the Internet |

---

## 🔐 Remote Access

| Command | Purpose |
|----------|---------|
| `ssh` | Secure remote login |
| `scp` | Secure file transfer |
| `sftp` | Secure interactive file transfer |
| `rsync` | Efficient file synchronization |

---

## 🔍 Port Scanning & Diagnostics

| Command | Purpose |
|----------|---------|
| `nmap` | Discover hosts and scan ports |
| `nc` (Netcat) | TCP/UDP connections, debugging, file transfer |
| `telnet` | Test TCP connectivity (legacy) |

---

## 📦 Packet Capture & Analysis

| Command | Purpose |
|----------|---------|
| `tcpdump` | Capture packets from the command line |
| `tshark` | Command-line Wireshark |
| `Wireshark` | GUI packet analyzer |

---

## 🔥 Firewall

| Command | Purpose |
|----------|---------|
| `iptables` | Linux firewall management |
| `nft` | Modern replacement for iptables |
| `ufw` | Simple firewall frontend |

---

## ⚙️ Network Services

| Command | Purpose |
|----------|---------|
| `systemctl` | Manage networking services |
| `service` | Legacy service manager |

---

## 📊 Monitoring

| Command | Purpose |
|----------|---------|
| `iftop` | Live bandwidth usage |
| `bmon` | Bandwidth monitor |
| `iperf3` | Network throughput testing |
| `vnstat` | Traffic statistics |
| `sar -n` | Network performance statistics |

---

# ⭐ Most Important Commands

If you're just starting Linux networking, learn these first:

| Priority | Command | Purpose |
|----------|---------|---------|
| ⭐⭐⭐ | `ip` | Interfaces, IP addresses, routing |
| ⭐⭐⭐ | `ping` | Connectivity testing |
| ⭐⭐⭐ | `ss` | Active TCP/UDP connections |
| ⭐⭐⭐ | `dig` | DNS troubleshooting |
| ⭐⭐⭐ | `traceroute` | Trace packet path |
| ⭐⭐⭐ | `ssh` | Remote login |
| ⭐⭐⭐ | `scp` | Secure file transfer |
| ⭐⭐⭐ | `curl` | HTTP requests & API testing |
| ⭐⭐⭐ | `nmap` | Port scanning |
| ⭐⭐⭐ | `tcpdump` | Packet capture |
| ⭐⭐⭐ | `iptables` | Firewall |
| ⭐⭐ | `ip neigh` | ARP / neighbor table |
| ⭐⭐ | `route` | Legacy routing |
| ⭐⭐ | `netstat` | Legacy socket information |
| ⭐⭐ | `wget` | File downloads |
| ⭐⭐ | `mtr` | Network diagnostics |
| ⭐⭐ | `Wireshark` | Packet analysis |

---

# 🚀 Frequently Used Options

| Option | Meaning |
|---------|---------|
| `-a` | Show all |
| `-v` | Verbose output |
| `-vv` | More verbose |
| `-vvv` | Maximum verbosity |
| `-h` | Help / Human-readable (command dependent) |
| `-n` | Numeric output (don't resolve hostnames) |
| `-p` | Show process / Specify port (depends on command) |
| `-l` | Listening sockets |
| `-t` | TCP |
| `-u` | UDP |
| `-i` | Interface / Interval |
| `-s` | Statistics / Packet size |
| `-c` | Count |
| `-r` | Routing / Recursive |
| `-4` | Force IPv4 |
| `-6` | Force IPv6 |

---

# 📝 Learning Order

1. IP Addressing
2. `ip`
3. `ping`
4. `ss`
5. `traceroute`
6. `dig`
7. `curl`
8. `ssh`
9. `scp`
10. `nmap`
11. `tcpdump`
12. Wireshark
13. `iptables`
14. `ufw`
15. Network Troubleshooting

---

# Author

Anwesha Singh  
B.Tech (Computer Science Engineering)  
Manipal University Jaipur
