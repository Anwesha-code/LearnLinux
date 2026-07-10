# Cron Quick Reference Cheat Sheet

`cron` is a Linux utility used to **schedule commands or scripts to run automatically** at specified times and intervals.

---

# Basic Syntax

```text
* * * * * command_to_execute
│ │ │ │ │
│ │ │ │ └── Day of Week (0–7) (0 and 7 = Sunday)
│ │ │ └──── Month (1–12)
│ │ └────── Day of Month (1–31)
│ └──────── Hour (0–23)
└────────── Minute (0–59)
```

---

# Special Characters

| Symbol | Meaning |
|---------|---------|
| `*` | Every value |
| `,` | Multiple values |
| `-` | Range |
| `/` | Step value |
| `@` | Special shortcuts |

Example:

```text
*/10 * * * * command
```

Runs every **10 minutes**.

---

# Managing Cron Jobs

Edit current user's crontab:

```bash
crontab -e
```

List cron jobs:

```bash
crontab -l
```

Remove all cron jobs:

```bash
crontab -r
```

Edit another user's crontab (root):

```bash
sudo crontab -u username -e
```

---

# Common Scheduling Examples

| Schedule | Expression |
|----------|------------|
| Every minute | `* * * * *` |
| Every 5 minutes | `*/5 * * * *` |
| Every 10 minutes | `*/10 * * * *` |
| Every hour | `0 * * * *` |
| Every 2 hours | `0 */2 * * *` |
| Every day at midnight | `0 0 * * *` |
| Every day at 2:30 AM | `30 2 * * *` |
| Every Sunday | `0 0 * * 0` |
| Every Monday | `0 0 * * 1` |
| Every weekday | `0 9 * * 1-5` |
| First day of every month | `0 0 1 * *` |
| Every January 1st | `0 0 1 1 *` |

---

# Special Time Shortcuts

| Shortcut | Meaning |
|----------|---------|
| `@reboot` | Run once after boot |
| `@yearly` | Once a year |
| `@annually` | Same as yearly |
| `@monthly` | Once a month |
| `@weekly` | Once a week |
| `@daily` | Once a day |
| `@midnight` | Every midnight |
| `@hourly` | Every hour |

Example:

```bash
@reboot /home/user/startup.sh
```

---

# Running Scripts

Run a shell script every day at 9 AM:

```text
0 9 * * * /home/user/backup.sh
```

Run a Python script every hour:

```text
0 * * * * /usr/bin/python3 /home/user/script.py
```

---

# Logging Output

Save output to a log file:

```text
0 * * * * /home/user/script.sh >> /home/user/script.log 2>&1
```

Discard all output:

```text
0 * * * * /home/user/script.sh > /dev/null 2>&1
```

---

# Useful Directories

| Directory | Purpose |
|-----------|---------|
| `/etc/crontab` | System-wide cron configuration |
| `/etc/cron.d/` | Additional cron jobs |
| `/etc/cron.daily/` | Daily jobs |
| `/etc/cron.weekly/` | Weekly jobs |
| `/etc/cron.monthly/` | Monthly jobs |
| `/var/log/syslog` | Cron logs (Ubuntu) |
| `/var/log/cron` | Cron logs (RHEL/CentOS) |

---

# Cron Service

Check status:

```bash
systemctl status cron
```

Start:

```bash
sudo systemctl start cron
```

Enable on boot:

```bash
sudo systemctl enable cron
```

Restart:

```bash
sudo systemctl restart cron
```

---

# Common Examples

Run every 15 minutes:

```text
*/15 * * * * /home/user/script.sh
```

Run every 30 minutes:

```text
*/30 * * * * /home/user/script.sh
```

Run every Sunday at 6 PM:

```text
0 18 * * 0 /home/user/script.sh
```

Run every weekday at 8 AM:

```text
0 8 * * 1-5 /home/user/script.sh
```

Run every month on the 15th:

```text
0 0 15 * * /home/user/script.sh
```

---

# Quick Reference

```bash
# Edit cron jobs
crontab -e

# List cron jobs
crontab -l

# Delete cron jobs
crontab -r

# Check cron service
systemctl status cron

# Restart cron
sudo systemctl restart cron
```

---

# Cron Time Format

```text
┌──────── Minute (0-59)
│ ┌────── Hour (0-23)
│ │ ┌──── Day of Month (1-31)
│ │ │ ┌── Month (1-12)
│ │ │ │ ┌─ Day of Week (0-7)
│ │ │ │ │
* * * * * command
```

---

# Tips

- Always use **absolute paths** for scripts and executables.
- Ensure the script has execute permission (`chmod +x script.sh`).
- Redirect output to a log file for easier debugging.
- Verify the cron service is running before troubleshooting scheduled tasks.
- Test your command manually before adding it to the crontab.