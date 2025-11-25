---
title: "Shell Scripting for DevOps Engineer — With Practical Scripts You Must Know"
datePublished: Tue Nov 25 2025 16:40:50 GMT+0000 (Coordinated Universal Time)
cuid: cmiesyc8d000002l6fmey9hzb
slug: shell-scripting-for-devops-engineer-with-practical-scripts-you-must-know
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1764088786231/b4e78e3f-6277-4551-b0fa-bec165597d61.png
tags: linux, bash, shell, shell-script, bash-script, 90daysofdevops, trainwithshubham

---

Shell scripting is one of the most essential skills for every DevOps engineer. Whether you’re automating deployments, cleaning logs, creating backups, or monitoring system health — shell scripts save time, reduce manual work, and make your operations reliable.

In this blog, I will walk you through:

* **What shell scripting is**
    
* **Why DevOps engineers use it**
    
* **Basic syntax**
    
* **Useful real-world scripts with clear explanations**
    

## Let’s begin.

# **What is Shell Scripting?**

A **shell script** is a simple text file that contains commands that you normally type in a Linux terminal.  
Instead of running each command manually, you write them in a script → run the script → automation happens.

# **Why Do We Use Shell Scripts?**

Here’s why shell scripting is a superpower for DevOps:

### 1\. Automation

Automate repetitive tasks like:

* Creating users
    
* Cleaning logs
    
* Taking backups
    
* Checking CPU, Disk, Memory
    

### 2\. Save Time

A task that takes 10 minutes manually can run in 1 second using a script.

### 3\. Reduce Human Errors

Automation avoids manual mistakes.

### 4\. Easily Schedule with Cron

You can run scripts automatically at any time using `crontab`.

### 5\. Integrate with DevOps Tools

Shell scripts are widely used in:

* CI/CD pipelines
    
* Docker image builds
    
* Terraform provisioning
    
* Kubernetes automation
    
* Jenkins jobs
    

# **Basic Structure of a Shell Script**

Every shell script begins with a **shebang**:

```plaintext
#!/bin/bash
```

It tells Linux: “Use **bash** interpreter to run this script.”

After that, you write commands just like in the terminal.

You save the script (example: [`script.sh`](http://script.sh)) and run:

```plaintext
chmod +x script.sh   # give execute permission
./script.sh          # run the script
```

# **Let’s Go Through Practical Shell Scripts — Explained Line by Line**

Below are real-world scripts commonly used in system administration & DevOps.

````plaintext
### Simple User Creation Script  
This script creates a new Linux user and assigns a default password.

```bash
#!/bin/bash

read -p "Enter username: " USERNAME

PASSWORD="Password@123"

sudo useradd -m -s /bin/bash $USERNAME

echo "$USERNAME:$PASSWORD" | sudo chpasswd

echo "User $USERNAME created with password: $PASSWORD"
```

<details>
<summary><strong>Show / Hide Script Explanation</strong></summary>

<br>

```bash
# !/bin/bash → Shebang telling Linux to use Bash.
# read -p → asks the user to enter a username.
# PASSWORD → stores a default password.
# useradd -m -s → creates a user + home directory + bash shell.
# chpasswd → sets the password for the new user.
# final echo → prints a confirmation message.
```

</details>
````

# **1\.** Automation

### When to Use This Script?

* When onboarding new users
    
* Creating multiple accounts in training environments
    
* Quick user creation on servers
    

# **2\. Check System Uptime**

Displays how long the system has been running.

```plaintext
#!/bin/bash            # Use bash shell to run the script

echo "System Uptime:"  # Display a message

uptime                 # Shows system running time, load, and active users
```

### Why It’s Useful?

* Helps monitor server health
    
* Checks if reboot is needed
    
* Used in troubleshooting
    

# **3\. Check Disk Space Usage**

```plaintext
#!/bin/bash              # Bash interpreter

echo "Disk Space Usage:" # Message to user

df -h                    # Shows disk usage in human-readable format (GB/MB)
```

### Why It’s Important?

* Prevents server from getting full
    
* Helps plan storage
    
* Useful for monitoring scripts
    

# **4\. Check CPU & Memory Usage**

```plaintext
#!/bin/bash

echo "CPU Usage:"                           # Message
top -b -n1 | grep "Cpu(s)"                  # Extracts CPU usage from top command in batch mode

echo "Memory Usage:"                        # Message
free -m                                     # Shows RAM usage in MB
```

### When to Use?

* Debugging performance issues
    
* Checking resource utilization
    
* Monitoring servers
    

# **5\. Find Top 5 Largest Files**

This helps identify what’s consuming storage.

```plaintext
#!/bin/bash

echo "Top 5 largest files in the system:"         # Message

find / -type f -exec du -h {} + 2>/dev/null \; \  # Lists file sizes under root folder
| sort -rh                                         # Sorts in reverse (largest first)
| head -n 5                                        # Shows only top 5
```

### Use Cases

* Cleaning up disk
    
* Finding log files or backups consuming space
    
* Troubleshooting storage issues
    

# **6\. Show Active SSH Sessions**

```plaintext
#!/bin/bash

echo "Active SSH Sessions:"   # Message

who | grep "pts"              # Shows who is logged in through SSH terminals (pts)
```

### When It's Useful?

* Checking who is logged in
    
* Security monitoring
    
* Detecting unauthorized access
    

# **7\. Automated Log Cleanup Script**

Clears logs older than X days (default: 30 days).

```plaintext
#!/bin/bash

LOG_DIR="/var/log"                      # Directory containing log files
DAYS=30                                 # Delete logs older than 30 days

echo "Cleaning logs older than $DAYS days in $LOG_DIR..."  # Message

find $LOG_DIR -type f -name "*.log" -mtime +$DAYS -exec rm -f {} \;  
# -type f      → search only files
# -name "*.log" → select log files
# -mtime +30   → older than 30 days
# -exec rm -f {} \; → delete each file

echo "Log cleanup completed."           # Confirmation message
```

### Why It Matters?

* Prevents disk from filling up
    
* Keeps logs clean
    
* Very useful on production servers
    

# **8\. Backup a Directory (with date)**

This script creates a `.tar.gz` backup with timestamp.

````plaintext
### Directory Backup Script  
This script creates a backup of a directory with today's date.

```bash
#!/bin/bash

SRC_DIR="/home/user/data"
BACKUP_DIR="/home/user/backup"
BACKUP_FILE="$BACKUP_DIR/backup-$(date +%F).tar.gz"

mkdir -p $BACKUP_DIR

tar -czvf $BACKUP_FILE $SRC_DIR

echo "Backup completed: $BACKUP_FILE"
```

<details>
<summary><strong>Show / Hide Explanation</strong></summary>

<br>

**Explanation of Each Line:**

- `SRC_DIR="/home/user/data"` → Directory you want to back up  
- `BACKUP_DIR="/home/user/backup"` → Location where backup will be stored  
- `BACKUP_FILE="backup-<date>.tar.gz"` → Creates file name with current date  
- `mkdir -p` → Creates backup folder if not present  
- `tar -czvf` → Creates a compressed backup  
- `echo` → Prints the final backup path  

</details>
````

### Why This Script Is Important?

* Automates daily backups
    
* Useful for DevOps CI/CD
    
* Protects from accidental deletion
    
* Essential for production and staging environments
    

# **Final Thoughts**

Shell scripting is one of the core skills for DevOps and Linux system administration.  
With automation, you become faster, more efficient, and more reliable in managing servers.