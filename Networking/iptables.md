# IPTables Cheat Sheet

`iptables` is a command-line utility in Linux used to configure the **Netfilter Firewall** in the Linux kernel. It allows administrators to control incoming, outgoing, and forwarded network traffic by defining rules.

> **Note:** Newer Linux distributions may use **nftables** or **iptables-nft** internally, but `iptables` is still widely used and commonly encountered in networking, cybersecurity, and system administration.

---

# Table of Contents

1. What is IPTables?
2. How IPTables Works
3. Packet Flow
4. Tables
5. Chains
6. Targets
7. Rule Syntax
8. Basic Commands
9. Viewing Rules
10. Managing Rules
11. Default Policies
12. Common Examples
13. Matching Options
14. Stateful Firewall
15. NAT
16. Port Forwarding
17. Logging
18. Saving Rules
19. Common Scenarios
20. Best Practices
21. Troubleshooting
22. Quick Reference

---

# What is IPTables?

`iptables` is the userspace utility used to configure the Linux kernel firewall.

It can:

- Allow network traffic
- Block traffic
- Filter packets
- Perform NAT (Network Address Translation)
- Forward ports
- Log packets
- Protect against unwanted connections

---

# Basic Terminology

| Term | Meaning |
|------|---------|
| Packet | Unit of data travelling over a network |
| Rule | Condition + action |
| Chain | Collection of rules |
| Table | Collection of chains |
| Target | Action taken when rule matches |
| Policy | Default action when no rule matches |

---

# Packet Flow

```
Incoming Packet
       │
       ▼
   PREROUTING
       │
       ▼
Routing Decision
   │          │
   │          │
Local       Forwarded
   │          │
   ▼          ▼
INPUT      FORWARD
   │          │
   ▼          ▼
Process    POSTROUTING
   │
   ▼
OUTPUT
   │
   ▼
POSTROUTING
```

Understanding packet flow is essential for writing firewall rules.

---

# IPTables Tables

There are several tables, each designed for different purposes.

| Table | Purpose |
|--------|----------|
| filter | Packet filtering (default) |
| nat | Network Address Translation |
| mangle | Packet modification |
| raw | Connection tracking exceptions |
| security | SELinux security rules |

Most users mainly work with:

- filter
- nat

---

# Filter Table

Default table.

Contains:

- INPUT
- OUTPUT
- FORWARD

Example:

```bash
sudo iptables -L
```

---

# NAT Table

Used for:

- Port forwarding
- Masquerading
- Source NAT
- Destination NAT

Contains:

- PREROUTING
- POSTROUTING
- OUTPUT

---

# Chains

A chain is a list of firewall rules.

## INPUT

Controls packets coming **into the local machine**.

```
Internet
   │
   ▼
Computer
```

---

## OUTPUT

Controls packets leaving your machine.

```
Computer
   │
   ▼
Internet
```

---

## FORWARD

Controls packets passing **through** your system.

Example:

Linux router

```
PC1 ---- Linux Router ---- Internet
```

Packets are forwarded.

---

# Targets

Targets specify what happens when a rule matches.

| Target | Meaning |
|---------|---------|
| ACCEPT | Allow packet |
| DROP | Silently discard packet |
| REJECT | Reject and send error |
| LOG | Log packet |
| RETURN | Return to previous chain |

---

# Command Syntax

```
iptables [table] command chain matching target
```

General format:

```bash
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

---

# Rule Operations

| Option | Meaning |
|---------|----------|
| -A | Append rule |
| -I | Insert rule |
| -D | Delete rule |
| -R | Replace rule |
| -L | List rules |
| -F | Flush rules |
| -N | Create new chain |
| -X | Delete chain |

---

# Viewing Rules

List rules

```bash
sudo iptables -L
```

Verbose output

```bash
sudo iptables -L -v
```

Numeric output

```bash
sudo iptables -L -n
```

Show line numbers

```bash
sudo iptables -L --line-numbers
```

View NAT rules

```bash
sudo iptables -t nat -L
```

---

# Append Rule

Allow SSH

```bash
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

---

# Insert Rule

Insert at top

```bash
sudo iptables -I INPUT 1 -p tcp --dport 22 -j ACCEPT
```

---

# Delete Rule

Delete by number

```bash
sudo iptables -L --line-numbers

sudo iptables -D INPUT 2
```

Delete by specification

```bash
sudo iptables -D INPUT -p tcp --dport 22 -j ACCEPT
```

---

# Flush Rules

Delete every rule

```bash
sudo iptables -F
```

Flush only NAT table

```bash
sudo iptables -t nat -F
```

---

# Default Policies

View policies

```bash
sudo iptables -L
```

Allow everything

```bash
sudo iptables -P INPUT ACCEPT
```

Block everything

```bash
sudo iptables -P INPUT DROP
```

---

# Matching Options

## Protocol

TCP

```bash
-p tcp
```

UDP

```bash
-p udp
```

ICMP

```bash
-p icmp
```

---

## Source Address

```bash
-s 192.168.1.10
```

Subnet

```bash
-s 192.168.1.0/24
```

---

## Destination Address

```bash
-d 8.8.8.8
```

---

## Source Port

```bash
--sport 443
```

---

## Destination Port

```bash
--dport 80
```

Multiple ports

```bash
-m multiport --dports 22,80,443
```

---

# Interface Matching

Incoming interface

```bash
-i eth0
```

Outgoing interface

```bash
-o eth0
```

---

# Stateful Firewall

Modern firewalls track connection state.

Allow established connections

```bash
sudo iptables -A INPUT \
-m conntrack \
--ctstate ESTABLISHED,RELATED \
-j ACCEPT
```

Allow new SSH connections

```bash
sudo iptables -A INPUT \
-p tcp \
--dport 22 \
-m conntrack \
--ctstate NEW \
-j ACCEPT
```

Connection states:

| State | Meaning |
|---------|---------|
| NEW | New connection |
| ESTABLISHED | Existing connection |
| RELATED | Related connection |
| INVALID | Invalid packet |

---

# Blocking Traffic

Block an IP

```bash
sudo iptables -A INPUT -s 10.0.0.15 -j DROP
```

Block subnet

```bash
sudo iptables -A INPUT -s 10.0.0.0/24 -j DROP
```

Block website IP

```bash
sudo iptables -A OUTPUT -d 8.8.8.8 -j DROP
```

---

# Allowing Traffic

Allow HTTP

```bash
sudo iptables -A INPUT \
-p tcp \
--dport 80 \
-j ACCEPT
```

Allow HTTPS

```bash
sudo iptables -A INPUT \
-p tcp \
--dport 443 \
-j ACCEPT
```

Allow Ping

```bash
sudo iptables -A INPUT \
-p icmp \
-j ACCEPT
```

---

# Reject vs Drop

DROP

```
Packet disappears.

No reply.
```

REJECT

```
Packet rejected.

Sender receives an error.
```

---

# Logging Packets

```bash
sudo iptables \
-A INPUT \
-j LOG \
--log-prefix "IPTables: "
```

View logs

```bash
journalctl -f
```

or

```bash
dmesg
```

---

# NAT (Masquerading)

Enable IP forwarding

```bash
sudo sysctl -w net.ipv4.ip_forward=1
```

Masquerade traffic

```bash
sudo iptables \
-t nat \
-A POSTROUTING \
-o eth0 \
-j MASQUERADE
```

---

# Port Forwarding

Forward port 80

```bash
sudo iptables \
-t nat \
-A PREROUTING \
-p tcp \
--dport 80 \
-j DNAT \
--to-destination 192.168.1.20:80
```

---

# Save Rules

Ubuntu/Debian

Install

```bash
sudo apt install iptables-persistent
```

Save

```bash
sudo netfilter-persistent save
```

Reload

```bash
sudo netfilter-persistent reload
```

---

# Backup Rules

Save

```bash
sudo iptables-save > firewall.rules
```

Restore

```bash
sudo iptables-restore < firewall.rules
```

---

# Common Firewall Examples

## Allow SSH only

```bash
sudo iptables -P INPUT DROP

sudo iptables \
-A INPUT \
-m conntrack \
--ctstate ESTABLISHED,RELATED \
-j ACCEPT

sudo iptables \
-A INPUT \
-p tcp \
--dport 22 \
-j ACCEPT
```

---

## Block Ping

```bash
sudo iptables \
-A INPUT \
-p icmp \
-j DROP
```

---

## Block HTTP

```bash
sudo iptables \
-A OUTPUT \
-p tcp \
--dport 80 \
-j DROP
```

---

## Allow Localhost

```bash
sudo iptables \
-A INPUT \
-i lo \
-j ACCEPT
```

---

## Allow DNS

UDP

```bash
sudo iptables \
-A OUTPUT \
-p udp \
--dport 53 \
-j ACCEPT
```

TCP

```bash
sudo iptables \
-A OUTPUT \
-p tcp \
--dport 53 \
-j ACCEPT
```

---

# Troubleshooting

View counters

```bash
sudo iptables -L -v
```

Reset counters

```bash
sudo iptables -Z
```

List NAT

```bash
sudo iptables -t nat -L -v
```

List mangle

```bash
sudo iptables -t mangle -L
```

---

# Best Practices

- Allow `ESTABLISHED,RELATED` traffic first.
- Test rules before making them permanent.
- Avoid locking yourself out of SSH.
- Use `DROP` for stealth, `REJECT` when clients should receive an immediate response.
- Save working rules with `iptables-save`.
- Keep backups of firewall configurations.
- Use comments (`-m comment --comment "Allow SSH"`) to document complex rules.

---

# Quick Reference

```bash
# List rules
sudo iptables -L

# List with numbers
sudo iptables -L --line-numbers

# Verbose
sudo iptables -L -v -n

# Allow SSH
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Allow HTTP
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT

# Allow HTTPS
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# Allow Ping
sudo iptables -A INPUT -p icmp -j ACCEPT

# Drop an IP
sudo iptables -A INPUT -s 192.168.1.100 -j DROP

# Delete rule
sudo iptables -D INPUT 2

# Flush rules
sudo iptables -F

# Set default policy
sudo iptables -P INPUT DROP

# Save rules
sudo iptables-save > firewall.rules

# Restore rules
sudo iptables-restore < firewall.rules
```

---

# Related Commands

| Command | Purpose |
|----------|---------|
| `iptables` | Configure firewall rules |
| `iptables-save` | Save current rules |
| `iptables-restore` | Restore rules |
| `ip` | View network interfaces and routes |
| `ss` | View active sockets |
| `netstat` | Legacy socket information |
| `tcpdump` | Capture network packets |
| `nmap` | Scan ports and hosts |
| `ufw` | Simplified firewall frontend |
| `firewalld` | Dynamic firewall manager |
| `nft` | Configure nftables (modern replacement for iptables) |