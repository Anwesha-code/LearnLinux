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

---

# Manual vs Automatic MAC Changing

During installation, `macchanger` asks:

```
Change MAC automatically?
<Yes>   <No>
```

### Choose **No** if you want manual control.

This installs `macchanger` but **does not** automatically change your MAC address whenever the network interface is enabled.

You can change it whenever you want by running the appropriate commands.

### Choose **Yes** if you want automatic changing.

Every time the interface comes up (for example, after rebooting, reconnecting Wi-Fi, or plugging in an Ethernet cable), `macchanger` will automatically assign a new MAC address.

For learning, networking labs, and troubleshooting, **manual mode is usually recommended** because it gives you complete control.

---

# Automatic Configuration

The configuration file is:

```bash
sudo nano /etc/default/macchanger
```

Example configuration:

```text
ENABLE_ON_PRE_UP=true
ENABLE_ON_POST_UP_DOWN=false
ENABLE_ON_POST_DOWN=false
```

### Meaning of each option

| Option | Description |
|---------|-------------|
| `ENABLE_ON_PRE_UP` | Change the MAC before the interface is brought up. This is the most commonly used setting. |
| `ENABLE_ON_POST_UP_DOWN` | Change the MAC whenever the interface goes up or down. |
| `ENABLE_ON_POST_DOWN` | Change the MAC after the interface has been brought down. |

To disable automatic changing:

```text
ENABLE_ON_PRE_UP=false
ENABLE_ON_POST_UP_DOWN=false
ENABLE_ON_POST_DOWN=false
```

Save the file and reconnect the interface (or reboot) for the changes to take effect.

---

# Changing the MAC Periodically

Sometimes you may want to change the MAC address every few minutes while performing experiments or testing.

This can be done using a simple Bash script.

## Example Script

```bash
#!/bin/bash

INTERFACE="your_interface_name"
INTERVAL=300     # Time in seconds

while true
do
    echo "[$(date)] Changing MAC..."

    sudo ip link set "$INTERFACE" down
    sudo macchanger -r "$INTERFACE"
    sudo ip link set "$INTERFACE" up

    echo "Sleeping for $INTERVAL seconds..."
    sleep "$INTERVAL"
done
```

### Make it executable

```bash
chmod +x mac_randomizer.sh
```

### Run

```bash
./mac_randomizer.sh
```

or

```bash
bash mac_randomizer.sh
```

### Stop

Press

```
Ctrl + C
```

to terminate the script.

---

# Common Bash Mistakes

When writing shell scripts, these are very common mistakes.

## Incorrect Shebang

❌

```bash
#!bin/bash
```

✅

```bash
#!/bin/bash
```

---

## Spaces Around '='

Bash variable assignments **must not contain spaces**.

❌

```bash
INTERFACE = "your_interface"
INTERVAL = 300
```

✅

```bash
INTERFACE="your_interface"
INTERVAL=300
```

---

## Incorrect Interface Name

Always verify the interface name before using it.

```bash
ip link
```

Then use the exact interface name returned by the command.

---

# Understanding the Script

```bash
#!/bin/bash
```

Uses the Bash shell to execute the script.

---

```bash
INTERFACE="your_interface_name"
```

Stores the network interface that will have its MAC address changed.

---

```bash
INTERVAL=300
```

Wait time (in seconds) between MAC address changes.

---

```bash
while true
```

Creates an infinite loop.

The script keeps running until it is manually stopped.

---

```bash
sudo ip link set "$INTERFACE" down
```

Disables the network interface.

Most Linux drivers require the interface to be down before changing the MAC address.

---

```bash
sudo macchanger -r "$INTERFACE"
```

Generates and assigns a random MAC address.

---

```bash
sudo ip link set "$INTERFACE" up
```

Re-enables the interface.

The operating system will usually request a new DHCP lease after this.

---

```bash
sleep "$INTERVAL"
```

Pauses execution before repeating the process.

---

# Limitations of Periodic MAC Randomization

Changing the MAC address repeatedly is possible, but there are some practical limitations.

- The network interface must be temporarily disabled.
- Network connectivity is interrupted during each MAC change.
- DHCP usually assigns a new IP address after each change.
- Existing SSH sessions, downloads, or network connections may disconnect.
- Some enterprise or campus networks may block frequent MAC changes.

For these reasons, periodic MAC randomization is mainly useful for:

- Networking labs
- Virtual machines
- Security testing
- Privacy experiments

It is generally **not recommended on production systems**.

---

# Recommended Automation Methods

| Method | Best For |
|---------|----------|
| Manual commands | Learning and testing |
| `while` loop | Shell scripting practice |
| `cron` | Scheduled execution at fixed times |
| `systemd` timer | Long-term Linux automation (recommended) |

---

# Tips

- Always verify the interface name using `ip link`.
- Restore the permanent MAC before joining trusted or production networks.
- Use manual mode while learning networking concepts.
- If you accidentally lose network connectivity, restore the original MAC using:

```bash
sudo ip link set <interface> down
sudo macchanger -p <interface>
sudo ip link set <interface> up
```

---