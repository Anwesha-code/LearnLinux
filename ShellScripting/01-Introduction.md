# 🐚 Introduction to Shell Scripting

> "Why do the same task again and again when your computer can do it for you?"

Shell scripting is all about **automation**. It allows you to combine multiple Linux commands into a single file and execute them together.

Instead of typing the same commands repeatedly in the terminal, you can write them once in a script and run that script whenever you need it.

---

# 📖 What is a Shell?

A **Shell** is a program that acts as an interface between the **user** and the **Linux operating system (Kernel)**.

Whenever you type a command in the terminal:

```bash
ls
```

the shell:

1. Reads the command.
2. Understands what you want.
3. Passes the command to the Linux Kernel.
4. Displays the result on your screen.

```text
             User
               │
               ▼
          Linux Shell
               │
               ▼
            Kernel
               │
               ▼
          Hardware
```

Think of the shell as a **translator** between you and the operating system.

---

# 🤔 What is Shell Scripting?

A **Shell Script** is simply a text file containing multiple Linux commands.

Instead of running commands one by one:

```bash
mkdir Project

cd Project

touch app.py

touch README.md

ls
```

You can write them inside a file:

```bash
#!/bin/bash

mkdir Project

cd Project

touch app.py

touch README.md

ls
```

Save it as:

```text
setup.sh
```

Run it once:

```bash
bash setup.sh
```

All the commands execute automatically.

---

# 🎯 Why Do We Need Shell Scripting?

Imagine you perform these tasks every morning:

- Update your system
- Pull the latest code from GitHub
- Start Docker
- Run your application
- Open logs

Doing this manually every day is time-consuming.

Instead, create a script:

```bash
#!/bin/bash

sudo apt update

git pull

docker compose up -d

python3 app.py
```

Now everything runs with a single command.

That's the power of shell scripting.

---

# 🚀 Real World Uses

Shell scripting is used almost everywhere in Linux.

## 🖥️ System Administration

- Create users
- Delete users
- Change passwords
- Install software
- Restart services

---

## 📁 File Management

- Rename files
- Copy folders
- Move files
- Delete temporary files
- Create backups

---

## 🌐 Networking

- Check internet connectivity
- Test servers
- Scan ports
- Collect network information

---

## ☁️ DevOps

- Deploy applications
- Build projects
- Configure servers
- Run CI/CD pipelines

---

## 🔐 Cybersecurity

- Run security scans
- Analyse log files
- Automate penetration testing tasks
- Monitor suspicious activity

---

## 📊 Monitoring

Automatically check:

- CPU Usage
- RAM Usage
- Disk Space
- Running Processes

---

# 🐚 Popular Linux Shells

Linux supports multiple shells.

| Shell | Description |
|--------|-------------|
| Bash | Most popular Linux shell |
| Zsh | Modern shell with powerful features |
| Fish | Beginner-friendly interactive shell |
| Ksh | Korn Shell |
| Sh | Original Unix Shell |

In this guide, we'll use **Bash** because it is the default shell on most Linux distributions.

---

# 🧐 What is Bash?

**Bash** stands for:

> **Bourne Again SHell**

It is an improved version of the original Bourne Shell (`sh`) and is the most commonly used shell in Linux.

Check your Bash version:

```bash
bash --version
```

Check your current shell:

```bash
echo $SHELL
```

Example output:

```text
/bin/bash
```

---

# 📄 What is a `.sh` File?

A shell script is usually saved with the **`.sh`** extension.

Examples:

```text
backup.sh

install.sh

deploy.sh

hello.sh
```

The `.sh` extension isn't compulsory, but it makes it easy to identify shell scripts.

---

# 📝 Writing Your First Shell Script

Create a file:

```bash
nano hello.sh
```

Write:

```bash
#!/bin/bash

echo "Hello, World!"
```

Save the file.

---

# 🔍 What is `#!/bin/bash`?

The first line:

```bash
#!/bin/bash
```

is called the **Shebang** (or **Hashbang**).

It tells Linux:

> "Run this script using the Bash interpreter."

Without the shebang, Linux may try to execute the script using a different shell.

---

# ▶️ Running a Shell Script

### Method 1

Run using Bash:

```bash
bash hello.sh
```

---

### Method 2

Make the script executable:

```bash
chmod +x hello.sh
```

Run it:

```bash
./hello.sh
```

---

# 📌 Important Commands You'll Use

| Command | Purpose |
|----------|---------|
| `bash script.sh` | Execute a script |
| `chmod +x script.sh` | Make a script executable |
| `./script.sh` | Run an executable script |
| `echo` | Print text |
| `nano` | Create/edit scripts |
| `cat` | View script contents |

---

# 📂 How Shell Scripts Execute

Suppose your script contains:

```bash
echo "Hello"

pwd

date

whoami
```

Linux executes the commands **one after another**.

```text
Script Starts
      │
      ▼
echo "Hello"
      │
      ▼
pwd
      │
      ▼
date
      │
      ▼
whoami
      │
      ▼
Script Ends
```

---

# 💡 Advantages of Shell Scripting

✅ Saves time

✅ Automates repetitive tasks

✅ Easy to write

✅ Lightweight

✅ No compilation required

✅ Comes pre-installed with Linux

✅ Useful for DevOps, Networking and System Administration

---

# ⚠️ Limitations

Shell scripting is great for automation, but it isn't the right choice for every task.

- Not ideal for large software projects
- Limited support for complex data structures
- Slower than compiled languages like C or C++
- Difficult to maintain if scripts become very large

For automation and system tasks, however, it's one of the best tools available.

---

# 🎯 Where You'll Use Shell Scripting

As you continue learning Linux, you'll find shell scripts being used for tasks like:

- Installing software
- Creating backups
- Scheduling jobs with Cron
- Monitoring servers
- Configuring networks
- Deploying applications
- Running Docker containers
- Managing cloud infrastructure

If you're interested in Linux, Networking, DevOps, Cybersecurity or Cloud Computing, shell scripting is a skill you'll use regularly.

---


