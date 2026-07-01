
# Linux Commands Cheat Sheet

A quick reference guide for commonly used Linux commands with syntax and examples.

---

## 📂 File & Directory Commands

### `ls` - List files and directories
```bash
ls
```

---

### `cd` - Change the current directory
```bash
cd directory_name
```

Example:
```bash
cd Documents
```

---

### `pwd` - Print the current working directory
```bash
pwd
```

---

### `mkdir` - Create a new directory
```bash
mkdir rohan
```

---

### `rmdir` - Remove an empty directory
```bash
rmdir rohan
```

---

### `touch` - Create a new empty file
```bash
touch shayan.txt
```

---

### `cp` - Copy files or directories
```bash
cp example.txt backup/
```

---

### `mv` - Move or rename files/directories
```bash
mv example.txt backup/
```

---

### `rm` - Remove files
```bash
rm example.txt
```

---

### `cat` - Display file contents
```bash
cat example.txt
```

---

## 📖 Help

### `man` - Display the manual page for a command
```bash
man ls
```

---

## 🔒 Permissions & Ownership

### `chmod` - Change file permissions

Permission values:

| Number | Permission |
|---------|------------|
| 0 | No permission |
| 1 | Execute |
| 2 | Write |
| 3 | Write + Execute |
| 4 | Read |
| 5 | Read + Execute |
| 6 | Read + Write |
| 7 | Read + Write + Execute |

Example:

```bash
chmod 700 file.txt
```

Where:

- First digit → Owner
- Second digit → Group
- Third digit → Others

---

### `chown` - Change file owner
```bash
chown new_owner example.txt
```

---

## 📦 Compression & Archives

### Create a TAR archive
```bash
tar cf archive.tar file1 file2 file3
```

### Extract a TAR archive
```bash
tar xf archive.tar
```

### Compress using Gzip
```bash
gzip file.txt
```

### Decompress
```bash
gunzip file.txt.gz
```

---

## 🌐 Networking

### SSH into a remote server
```bash
ssh username@server_address
```

---

### Securely copy files
```bash
scp myfile.txt user@remotehost:/home/user/
```

---

### Ping a server
```bash
ping 8.8.8.8
```

---

### Display network interfaces
```bash
ifconfig
```

---

### Display network connections
```bash
netstat
```

---

### View routing table
```bash
route
```

---

## ⚙️ Process Management

### Interactive process viewer
```bash
htop
```

---

### Display running processes
```bash
ps aux
```

---

### Display system processes
```bash
top
```

---

### Kill a process
```bash
kill PID
```

Example:

```bash
kill 1234
```

---

## 🖥️ System Services

### Using systemctl

Start a service:

```bash
systemctl start nginx
```

Check status:

```bash
systemctl status nginx
```

Stop a service:

```bash
systemctl stop nginx
```

---

### Using service

```bash
service apache2 start
```

---

## 👤 User Management

### Add a user
```bash
useradd harry
```

---

### Change user password
```bash
passwd harry
```

---

### Delete a user
```bash
userdel harry
```

---

### Switch user
```bash
su john
```

---

### Execute commands as root
```bash
sudo command
```

---

## 💾 Disk Management

### Display disk usage
```bash
df
```

---

### Display directory size
```bash
du
```

---

### Mount a filesystem
```bash
sudo mount /dev/sdb1 /mnt/usb
```

---

### Unmount a filesystem
```bash
sudo umount /mnt/usb
```

---

## ℹ️ System Information

### Display system uptime
```bash
uptime
```

---

### Display current date & time
```bash
date
```

---

### Display current username
```bash
whoami
```

---

### Locate a command
```bash
which ls
```

---

### Display user information
```bash
finger harry
```

---

### Display system information
```bash
uname
```

Detailed information:

```bash
uname -a
```

---

### Command history
```bash
history
```

---

## 📄 Text Processing

### Print text
```bash
echo "I need Tshirt from Codeswear!"
```

---

### Write output to terminal and file
```bash
ls | tee file.txt
```

---

### Locate files
```bash
locate file.txt
```

---

### Sort file contents

Input:

```text
banana
orange
apple
```

Command:

```bash
sort file.txt
```

Output:

```text
apple
banana
orange
```

---

### Remove duplicate lines

Input:

```text
apple
orange
banana
apple
banana
```

Command:

```bash
uniq file.txt
```

---

### Display first few lines
```bash
head file.txt
```

---

### Display last few lines
```bash
tail file.txt
```

---

## 🏷️ Tags

- Linux
- Ubuntu
- Terminal
- Shell
- Bash
- CLI
- System Administration
- Networking
- File Management
- Commands
