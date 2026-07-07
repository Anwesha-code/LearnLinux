# 🔐 Access Control Lists (ACL) Cheat Sheet

> A quick reference guide for configuring Access Control Lists (ACLs) on routers and firewalls.

---

# 📖 What is an ACL?

An **Access Control List (ACL)** is a set of rules used to **allow or deny network traffic** based on specific conditions.

ACLs help improve security by filtering packets based on:

- Source IP Address
- Destination IP Address
- Protocol
- Port Number
- Direction of Traffic

Think of an ACL as a security guard checking every packet before allowing it through.

```
Incoming Packet
        │
        ▼
   Access Control List
   ┌──────────────┐
   │ Permit?      │────► Allow
   │ Deny?        │────► Drop
   └──────────────┘
```

---

# Why Use ACLs?

- Restrict unauthorized access
- Protect internal networks
- Block malicious traffic
- Permit only required services
- Improve network security

---

# Types of ACLs

| Type | Filters Based On |
|------|------------------|
| Standard ACL | Source IP Address |
| Extended ACL | Source IP, Destination IP, Protocol, Port |
| Named ACL | ACL identified using a name instead of a number |

---

# Standard ACL

Filters traffic using **only the source IP address**.

Example:

```text
Permit 192.168.1.10
Deny everyone else
```

Cisco Example:

```bash
access-list 10 permit 192.168.1.10
access-list 10 deny any
```

---

# Extended ACL

Can filter using:

- Source IP
- Destination IP
- TCP
- UDP
- ICMP
- Port Numbers

Example:

Allow HTTP traffic only

```bash
access-list 100 permit tcp any any eq 80
```

Allow HTTPS

```bash
access-list 100 permit tcp any any eq 443
```

Block Telnet

```bash
access-list 100 deny tcp any any eq 23
```

---

# Named ACL

Instead of numbers:

```bash
ip access-list extended WEB_FILTER
```

Example

```bash
ip access-list extended WEB_FILTER

permit tcp any any eq 80

permit tcp any any eq 443

deny ip any any
```

---

# Applying an ACL

Inbound

```bash
interface GigabitEthernet0/0

ip access-group 100 in
```

Outbound

```bash
interface GigabitEthernet0/0

ip access-group 100 out
```

---

# ACL Directions

```
Incoming Traffic

Internet
    │
    ▼
Router Interface

Inbound ACL

Router Processing

Outbound ACL

    ▼
LAN
```

---

# Common Keywords

| Keyword | Meaning |
|----------|---------|
| permit | Allow traffic |
| deny | Block traffic |
| any | Any IP Address |
| host | Single IP Address |
| eq | Equal to port |
| gt | Greater than |
| lt | Less than |
| neq | Not equal |

---

# Common Ports

| Service | Port |
|----------|------|
| HTTP | 80 |
| HTTPS | 443 |
| FTP | 21 |
| SSH | 22 |
| Telnet | 23 |
| SMTP | 25 |
| DNS | 53 |
| DHCP | 67/68 |
| POP3 | 110 |
| IMAP | 143 |
| SNMP | 161 |
| LDAP | 389 |

---

# Examples

## Permit one host

```bash
access-list 10 permit host 192.168.1.100
```

---

## Permit a network

```bash
access-list 10 permit 192.168.1.0 0.0.0.255
```

---

## Block one host

```bash
access-list 10 deny host 192.168.1.50
```

---

## Block Telnet

```bash
access-list 100 deny tcp any any eq 23
```

---

## Allow SSH

```bash
access-list 100 permit tcp any any eq 22
```

---

## Block an entire subnet

```bash
access-list 100 deny ip 192.168.10.0 0.0.0.255 any
```

---

# Wildcard Mask

Wildcard masks are the inverse of subnet masks.

| Subnet Mask | Wildcard Mask |
|-------------|---------------|
| 255.255.255.0 | 0.0.0.255 |
| 255.255.0.0 | 0.0.255.255 |
| 255.0.0.0 | 0.255.255.255 |

---

# Verify ACLs

Show configured ACLs

```bash
show access-lists
```

Show interface ACLs

```bash
show ip interface
```

Show running configuration

```bash
show running-config
```

---

# Linux Firewall (iptables)

Allow SSH

```bash
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

Allow HTTP

```bash
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

Allow HTTPS

```bash
iptables -A INPUT -p tcp --dport 443 -j ACCEPT
```

Drop all incoming traffic

```bash
iptables -P INPUT DROP
```

Allow loopback

```bash
iptables -A INPUT -i lo -j ACCEPT
```

Allow established connections

```bash
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
```

List rules

```bash
iptables -L -v -n
```

Delete a rule

```bash
iptables -D INPUT 1
```

Flush all rules

```bash
iptables -F
```

---

# UFW Examples

Enable firewall

```bash
sudo ufw enable
```

Allow SSH

```bash
sudo ufw allow 22
```

Allow HTTP

```bash
sudo ufw allow 80
```

Allow HTTPS

```bash
sudo ufw allow 443
```

Deny Telnet

```bash
sudo ufw deny 23
```

Check status

```bash
sudo ufw status verbose
```

Disable firewall

```bash
sudo ufw disable
```

---

# Best Practices

- Follow the principle of least privilege.
- Allow only the services that are required.
- Place more specific rules before general ones.
- Remember that Cisco ACLs have an **implicit `deny any`** at the end.
- Regularly review and remove unused rules.
- Test ACL changes in a controlled environment before applying them in production.

---

# Quick Revision

✅ Standard ACL → Source IP only

✅ Extended ACL → Source, Destination, Protocol, Ports

✅ Named ACL → Easier to manage

✅ `permit` → Allow

✅ `deny` → Block

✅ `show access-lists` → View ACLs

✅ `iptables -L` → View Linux firewall rules

✅ `ufw status` → Check UFW firewall status

```
Packet
   │
   ▼
ACL
   │
 ┌─┴─────────┐
 │ Permit    │──► Forward
 │ Deny      │──► Drop
 └───────────┘
```