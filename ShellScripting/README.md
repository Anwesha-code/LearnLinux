# 🐚 Shell Scripting


Shell scripting is one of the most useful skills you can learn if you're working with Linux. Whether you're a student, developer, system administrator, or someone just exploring Linux, shell scripting helps you automate repetitive tasks and get more done with less effort.

This folder contains my notes as I learn Shell Scripting from the basics. The aim is to keep everything beginner-friendly, practical, and easy to understand.

---

# 📖 What is Shell Scripting?

A **shell script** is simply a text file containing a sequence of Linux commands that are executed one after another.

Instead of typing the same commands repeatedly in the terminal, you can write them once in a script and run the script whenever needed.

Think of it as creating your own mini-program to automate tasks.

---

# 🎯 Why Learn Shell Scripting?

Shell scripting is used everywhere in Linux.

Some common use cases include:

- Automating repetitive tasks
- Running backups
- Managing files and folders
- Monitoring CPU, RAM and disk usage
- Managing users and permissions
- Starting or stopping services
- Automating software installation
- Writing deployment scripts
- Scheduling tasks using Cron
- DevOps and Cloud Automation

If you plan to work with Linux, Networking, Cybersecurity, DevOps or Cloud Computing, shell scripting is a must-have skill.


---


# 🚀 How to Run a Shell Script

Create a script:

```bash
nano hello.sh

bash hello.sh

./hello.sh               --> works if file is executable
```

Write your script:

```bash
#!/bin/bash                  --> shebang

echo "Hello, World!"
```

Make it executable:

```bash
chmod +x hello.sh
```

Run the script:

```bash
./hello.sh
```

Or execute it directly using Bash:

```bash
bash hello.sh
```


---

# Author

Anwesha Singh  
B.Tech (Computer Science Engineering)  
Manipal University Jaipur