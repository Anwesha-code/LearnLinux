# NFTables Cheat Sheet

`nftables` is the modern Linux packet filtering framework that replaces `iptables`, `ip6tables`, `ebtables`, and `arptables`.

It provides a unified firewall framework with improved performance, simpler syntax, and better scalability.

---

# Installation

Ubuntu

```bash
sudo apt install nftables
```

Enable

```bash
sudo systemctl enable nftables
```

Start

```bash
sudo systemctl start nftables
```

Status

```bash
systemctl status nftables
```

---

# Basic Concepts

| Component | Description |
|-----------|-------------|
| Table | Container for chains |
| Chain | Collection of rules |
| Rule | Match + Action |
| Set | Group of addresses or ports |
| Map | Key-value lookup |
| Hook | Packet processing point |

---

# Hooks

```
PREROUTING

INPUT

FORWARD

OUTPUT

POSTROUTING
```

---

# Families

```
ip

ip6

inet

arp

bridge

netdev
```

Most administrators use

```
inet
```

because it supports both IPv4 and IPv6.

---

# View Rules

```bash
sudo nft list ruleset
```

---

# Flush Rules

```bash
sudo nft flush ruleset
```

---

# Create Table

```bash
sudo nft add table inet filter
```

---

# Create Chain

```bash
sudo nft add chain inet filter input \
'{ type filter hook input priority 0 ; policy drop ; }'
```

---

# Allow Established Connections

```bash
sudo nft add rule inet filter input \
ct state established,related accept
```

---

# Allow Loopback

```bash
sudo nft add rule inet filter input \
iif lo accept
```

---

# Allow SSH

```bash
sudo nft add rule inet filter input \
tcp dport 22 accept
```

---

# Allow HTTP

```bash
sudo nft add rule inet filter input \
tcp dport 80 accept
```

---

# Allow HTTPS

```bash
sudo nft add rule inet filter input \
tcp dport 443 accept
```

---

# Allow ICMP

```bash
sudo nft add rule inet filter input \
icmp type echo-request accept
```

---

# Block IP

```bash
sudo nft add rule inet filter input \
ip saddr 192.168.1.100 drop
```

---

# Delete Rule

View handles

```bash
sudo nft -a list ruleset
```

Delete

```bash
sudo nft delete rule inet filter input handle 12
```

---

# Logging

```bash
sudo nft add rule inet filter input \
log prefix "NFT: "
```

---

# NAT

Create NAT table

```bash
sudo nft add table ip nat
```

Create chain

```bash
sudo nft add chain ip nat postrouting \
'{ type nat hook postrouting priority 100; }'
```

Masquerade

```bash
sudo nft add rule ip nat postrouting \
oif enp0s3 masquerade
```

---

# Port Forwarding

```bash
sudo nft add rule ip nat prerouting \
tcp dport 80 dnat to 192.168.1.20:80
```

---

# Sets

Create

```bash
sudo nft add set inet filter blacklist \
'{ type ipv4_addr; }'
```

Add IP

```bash
sudo nft add element inet filter blacklist \
{192.168.1.100}
```

Use

```bash
sudo nft add rule inet filter input \
ip saddr @blacklist drop
```

---

# Save Configuration

```bash
sudo nft list ruleset > /etc/nftables.conf
```

Reload

```bash
sudo nft -f /etc/nftables.conf
```

---

# Example nftables Script

```nft
#!/usr/sbin/nft -f

flush ruleset

table inet filter {

    chain input {

        type filter hook input priority 0;

        policy drop;

        iif lo accept

        ct state established,related accept

        tcp dport 22 accept

        tcp dport 80 accept

        tcp dport 443 accept

        ip protocol icmp accept
    }

    chain forward {

        type filter hook forward priority 0;

        policy drop;
    }

    chain output {

        type filter hook output priority 0;

        policy accept;
    }
}
```

Save as

```
/etc/nftables.conf
```

Load

```bash
sudo nft -f /etc/nftables.conf
```

---

# Example NAT Script

```nft
table ip nat {

    chain prerouting {

        type nat hook prerouting priority -100;
    }

    chain postrouting {

        type nat hook postrouting priority 100;

        oifname "enp0s3" masquerade
    }
}
```

---

# Best Practices

- Use the `inet` family whenever possible.
- Set default policies to `drop` and explicitly allow required traffic.
- Keep configuration in `/etc/nftables.conf`.
- Group IPs using **sets** instead of writing many individual rules.
- Test rules before enabling them permanently.
- Comment complex configurations for easier maintenance.

---

# Quick Reference

```bash
# Show rules
sudo nft list ruleset

# Flush everything
sudo nft flush ruleset

# Add table
sudo nft add table inet filter

# Add chain
sudo nft add chain inet filter input '{ type filter hook input priority 0; policy drop; }'

# Allow SSH
sudo nft add rule inet filter input tcp dport 22 accept

# Allow HTTP
sudo nft add rule inet filter input tcp dport 80 accept

# Save
sudo nft list ruleset > /etc/nftables.conf

# Load
sudo nft -f /etc/nftables.conf
```