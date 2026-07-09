# MAC Changer Cheat Sheet

`macchanger` is a Linux utility used to **view** and **change** the MAC (Media Access Control) address of a network interface.

> **Note:** Changing the MAC address is temporary and lasts until the interface is reset or the system is rebooted (unless configured otherwise).

---

# Installation

### Debian/Ubuntu

```bash
sudo apt update
sudo apt install macchanger
```

### Verify Installation

```bash
macchanger --version
```

---

# What is a MAC Address?

A **MAC (Media Access Control) address** is a unique 48-bit hardware identifier assigned to every network interface.

Example:

```
08:00:27:12:34:56
```

It is used for communication within a local network (Layer 2 of the OSI model).

---

# Find Your Network Interface

List all interfaces:

```bash
ip link
```

Example output:

```text
1: lo
2: enp0s3
3: wlp2s0
```

Common interface names:

| Interface | Description |
|-----------|-------------|
| `lo` | Loopback interface |
| `enp0s3` | Ethernet |
| `eth0` | Ethernet (older naming) |
| `wlp2s0` | Wi-Fi |
| `wlan0` | Wi-Fi (older naming) |

---

# Basic Syntax

```bash
sudo macchanger [OPTION] <interface>
```

Example:

```bash
sudo macchanger -r enp0s3
```

---

# Important: Bring Interface Down First

Most changes require the interface to be disabled first.

```bash
sudo ip link set enp0s3 down
```

After changing the MAC:

```bash
sudo ip link set enp0s3 up
```

---

# Common Commands

## Show Current MAC Address

```bash
macchanger -s enp0s3
```

or

```bash
macchanger --show enp0s3
```

Example:

```
Current MAC: 08:00:27:12:34:56
Permanent MAC: 08:00:27:12:34:56
```

---

## Set a Completely Random MAC

```bash
sudo ip link set enp0s3 down
sudo macchanger -r enp0s3
sudo ip link set enp0s3 up
```

Option:

```
-r
--random
```

---

## Random MAC from Same Vendor

Keeps the manufacturer the same while changing the device portion.

```bash
sudo macchanger -a enp0s3
```

Option:

```
-a
--another
```

---

## Random MAC from Any Vendor

Generates a MAC with a randomly selected vendor.

```bash
sudo macchanger -A enp0s3
```

Option:

```
-A
```

---

## Keep Vendor Bytes Unchanged

Only changes the last three bytes.

```bash
sudo macchanger -e enp0s3
```

Option:

```
-e
--ending
```

---

## Restore Original MAC

Returns the interface to its factory-assigned MAC.

```bash
sudo ip link set enp0s3 down
sudo macchanger -p enp0s3
sudo ip link set enp0s3 up
```

Option:

```
-p
--permanent
```

---

# Check the New MAC Address

Using macchanger:

```bash
macchanger -s enp0s3
```

Using ip:

```bash
ip link show enp0s3
```

---

# Complete Workflow

Randomize:

```bash
sudo ip link set enp0s3 down
sudo macchanger -r enp0s3
sudo ip link set enp0s3 up
macchanger -s enp0s3
```

Restore:

```bash
sudo ip link set enp0s3 down
sudo macchanger -p enp0s3
sudo ip link set enp0s3 up
```

---

# Useful Options

| Option | Long Form | Description |
|---------|-----------|-------------|
| `-h` | `--help` | Display help |
| `-V` | `--version` | Show version |
| `-s` | `--show` | Show current MAC |
| `-r` | `--random` | Set a fully random MAC |
| `-a` | `--another` | Random MAC with same vendor |
| `-A` | — | Random MAC from any vendor |
| `-e` | `--ending` | Change only device-specific bytes |
| `-p` | `--permanent` | Restore original MAC |

---

# Enable/Disable Automatic MAC Changing

Configuration file:

```bash
sudo nano /etc/default/macchanger
```

Typical settings:

```text
ENABLE_ON_PRE_UP=true
ENABLE_ON_POST_UP_DOWN=false
ENABLE_ON_POST_DOWN=false
```

Set values to `true` or `false` depending on whether you want MAC addresses to change automatically.

---

# Best Practices

- Always bring the interface **down** before changing the MAC.
- Bring the interface **up** after changing it.
- Verify the new MAC using `macchanger -s` or `ip link`.
- Restore the permanent MAC before connecting to trusted or production networks.
- Manual mode is recommended for labs and learning so you have full control over when the MAC changes.

---

# Quick Reference

```bash
# Show MAC
macchanger -s enp0s3

# Random MAC
sudo ip link set enp0s3 down
sudo macchanger -r enp0s3
sudo ip link set enp0s3 up

# Random MAC (same vendor)
sudo macchanger -a enp0s3

# Random vendor
sudo macchanger -A enp0s3

# Keep vendor bytes
sudo macchanger -e enp0s3

# Restore original MAC
sudo ip link set enp0s3 down
sudo macchanger -p enp0s3
sudo ip link set enp0s3 up

# List interfaces
ip link

# Show interface details
ip link show enp0s3
```

---

# Related Commands

| Command | Purpose |
|---------|---------|
| `ip link` | View/manage network interfaces |
| `ip addr` | View IP addresses |
| `ifconfig` | Legacy interface management |
| `nmcli` | NetworkManager CLI |
| `ethtool` | Interface information |
| `arp` / `ip neigh` | View ARP cache |
| `ping` | Test network connectivity |