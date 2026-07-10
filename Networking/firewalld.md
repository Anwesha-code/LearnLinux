# Firewalld Cheat Sheet

`firewalld` is a dynamic firewall management tool for Linux that provides an easier interface to Netfilter (`iptables`/`nftables`).

Unlike `iptables`, rules can be added or removed without restarting the firewall.

> Modern versions of `firewalld` use **nftables** as the backend by default.

---

# Table of Contents

1. What is Firewalld?
2. Architecture
3. Zones
4. Runtime vs Permanent Configuration
5. Managing the Service
6. Firewall Zones
7. Services
8. Ports
9. Rich Rules
10. Source IP Rules
11. NAT & Masquerading
12. Port Forwarding
13. Logging
14. Lockdown Mode
15. Best Practices
16. Troubleshooting
17. Quick Reference

---

# Installation

Ubuntu

```bash
sudo apt install firewalld
```

Fedora / RHEL

```bash
sudo dnf install firewalld
```

---

# Start Firewalld

```bash
sudo systemctl start firewalld
```

Enable at boot

```bash
sudo systemctl enable firewalld
```

Stop

```bash
sudo systemctl stop firewalld
```

Restart

```bash
sudo systemctl restart firewalld
```

Reload configuration

```bash
sudo firewall-cmd --reload
```

---

# Check Status

```bash
sudo firewall-cmd --state
```

Example

```
running
```

---

# Runtime vs Permanent Configuration

Firewalld has two configurations.

## Runtime

Changes disappear after reboot.

Example

```bash
sudo firewall-cmd --add-service=http
```

---

## Permanent

Survives reboot.

```bash
sudo firewall-cmd --permanent --add-service=http
```

Apply changes

```bash
sudo firewall-cmd --reload
```

---

# Firewall Zones

A zone represents a trust level.

List zones

```bash
firewall-cmd --get-zones
```

Example

```
public
home
work
trusted
drop
block
internal
external
dmz
```

---

# Active Zone

```bash
firewall-cmd --get-active-zones
```

---

# Default Zone

View

```bash
firewall-cmd --get-default-zone
```

Set

```bash
sudo firewall-cmd --set-default-zone=home
```

---

# Assign Interface to Zone

```bash
sudo firewall-cmd \
--zone=home \
--change-interface=enp0s3
```

Permanent

```bash
sudo firewall-cmd \
--permanent \
--zone=home \
--change-interface=enp0s3
```

---

# List Everything

```bash
firewall-cmd --list-all
```

---

# List All Zones

```bash
firewall-cmd --list-all-zones
```

---

# Services

List available

```bash
firewall-cmd --get-services
```

Allow SSH

```bash
sudo firewall-cmd \
--add-service=ssh
```

Permanent

```bash
sudo firewall-cmd \
--permanent \
--add-service=ssh
```

Remove

```bash
sudo firewall-cmd \
--remove-service=ssh
```

---

# Ports

Open HTTP

```bash
sudo firewall-cmd \
--add-port=80/tcp
```

Open HTTPS

```bash
sudo firewall-cmd \
--add-port=443/tcp
```

UDP

```bash
sudo firewall-cmd \
--add-port=53/udp
```

Permanent

```bash
sudo firewall-cmd \
--permanent \
--add-port=8080/tcp
```

Remove

```bash
sudo firewall-cmd \
--remove-port=8080/tcp
```

---

# Rich Rules

Allow IP

```bash
sudo firewall-cmd \
--add-rich-rule='rule family="ipv4" source address="192.168.1.10" accept'
```

Block IP

```bash
sudo firewall-cmd \
--add-rich-rule='rule family="ipv4" source address="192.168.1.10" drop'
```

Limit SSH

```bash
sudo firewall-cmd \
--add-rich-rule='rule service name="ssh" limit value="5/m" accept'
```

---

# Source Address Rules

Allow subnet

```bash
sudo firewall-cmd \
--zone=trusted \
--add-source=192.168.1.0/24
```

Remove

```bash
sudo firewall-cmd \
--remove-source=192.168.1.0/24
```

---

# NAT

Enable masquerading

```bash
sudo firewall-cmd \
--add-masquerade
```

Permanent

```bash
sudo firewall-cmd \
--permanent \
--add-masquerade
```

---

# Port Forwarding

Forward port 80 to 8080

```bash
sudo firewall-cmd \
--add-forward-port=port=80:proto=tcp:toport=8080
```

Forward to another machine

```bash
sudo firewall-cmd \
--add-forward-port=port=80:proto=tcp:toaddr=192.168.1.20
```

---

# Logging

Log denied packets

```bash
sudo firewall-cmd \
--set-log-denied=all
```

View logs

```bash
journalctl -f
```

---

# Lockdown Mode

Enable

```bash
sudo firewall-cmd --lockdown-on
```

Disable

```bash
sudo firewall-cmd --lockdown-off
```

Status

```bash
firewall-cmd --query-lockdown
```

---

# Reload Configuration

```bash
sudo firewall-cmd --reload
```

Complete reload

```bash
sudo firewall-cmd --complete-reload
```

---

# Panic Mode

Enable

```bash
sudo firewall-cmd --panic-on
```

Disable

```bash
sudo firewall-cmd --panic-off
```

During panic mode, all incoming and outgoing traffic is blocked.

---

# Troubleshooting

Show configuration

```bash
firewall-cmd --list-all
```

Check service

```bash
systemctl status firewalld
```

Reload

```bash
sudo firewall-cmd --reload
```

---

# Best Practices

- Prefer services over opening individual ports.
- Use permanent rules only after testing runtime rules.
- Use zones to separate trusted and untrusted networks.
- Regularly review rich rules.
- Enable logging for troubleshooting.
- Backup firewall configuration before making major changes.

---

# Quick Reference

```bash
# Status
firewall-cmd --state

# Reload
sudo firewall-cmd --reload

# List rules
firewall-cmd --list-all

# Open SSH
sudo firewall-cmd --add-service=ssh

# Open HTTP
sudo firewall-cmd --add-service=http

# Open HTTPS
sudo firewall-cmd --add-service=https

# Open custom port
sudo firewall-cmd --add-port=8080/tcp

# Enable NAT
sudo firewall-cmd --add-masquerade

# List zones
firewall-cmd --get-zones
```