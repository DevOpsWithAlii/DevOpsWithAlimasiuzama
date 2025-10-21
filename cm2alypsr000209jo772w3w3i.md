---
title: "Linux Interview Questions Part- 1"
datePublished: Tue Oct 15 2024 15:38:48 GMT+0000 (Coordinated Universal Time)
cuid: cm2alypsr000209jo772w3w3i
slug: linux-interview-questions-part-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1761059531312/cd0b2cc8-3a04-4642-9949-e1030f6a2509.jpeg
tags: linux, aws, devops, linux-basics, 90daysofdevops, trainwithshubham

---

### **Top Linux Interview Questions for Learners.**

#### **1\. What is Linux?**

Explain the difference between Linux, a kernel, and an operating system.

#### **2\. Explain the File System Hierarchy in Linux.**

Discuss the structure and purpose of directories like `/bin`, `/home`, `/etc`, `/var`, `/usr`, and others.

#### **3\. What are the basic file permissions in Linux?**

* How does the permission model work for files and directories in Linux?
    
* Explain the significance of `r`, `w`, and `x` for users, groups, and others.
    

#### **4\. How do you change file permissions?**

* Command to change file permissions: `chmod`
    
* Command to change ownership: `chown`
    
* What are symbolic and numeric modes for permissions?
    

#### **5\. What are the differences between** `sudo` and `su`?

* Describe when and why you would use `sudo` over `su` and vice versa.
    

#### **6\. What is a shell?**

* Explain the concept of a shell in Linux.
    
* Name different types of shells, like Bash, Zsh, etc.
    
* How do you change the default shell?
    

#### **7\. Explain the purpose of environment variables in Linux.**

* What are some common environment variables like `PATH`, `HOME`, and `USER`?
    
* How do you create and manage environment variables?
    

#### **8\. How do you check the current running processes?**

* Discuss commands like `ps`, `top`, `htop`, and `pgrep`.
    
* How would you kill a process using `kill`, `killall`, or `pkill`?
    

#### **9\. What is the difference between hard links and soft (symbolic) links?**

* Describe how hard links and symbolic links are created.
    
* When should you use each type?
    

#### **10\. Explain package management in Linux.**

* What are some package managers used in Linux, like `apt`, `yum`, `dnf`, and `pacman`?
    
* How do you install, update, and remove packages?
    

#### **11\. How do you search for files in Linux?**

* Explain the use of `find` and `locate` commands.
    
* How do you search for a specific text within files using `grep`?
    

#### **12\. How do you view the content of a file?**

* Explain the use of commands like `cat`, `less`, `more`, `head`, and `tail`.
    
* How do you follow a log file in real-time using `tail -f`?
    

#### **13\. What is** `cron` and how do you schedule jobs using it?

* Explain the format of a cron job.
    
* How do you view, add, or remove cron jobs for a user?
    

#### **14\. What are system logs and where are they stored?**

* Explain the purpose of logs and the location of system logs in `/var/log`.
    
* How do you use `journalctl` to view system logs?
    

#### **15\. What is the difference between** `df` and `du` commands?

* How do you check disk usage and available space?
    
* What are some useful flags for these commands?
    

#### **16\. How do you manage services in Linux?**

* How do you start, stop, restart, and check the status of services using `systemctl` and `service` commands?
    

#### **17\. What is the difference between a terminal emulator and a shell?**

* Describe the relationship between terminal emulators (like GNOME Terminal, Konsole) and shells (like Bash, Zsh).
    

#### **18\. How do you compress and extract files in Linux?**

* Explain how to use commands like `tar`, `gzip`, `bzip2`, `zip`, and `unzip`.
    
* What’s the difference between `gzip` and `bzip2`?
    

#### **19\. How do you redirect output in Linux?**

* Explain input/output redirection using `>`, `>>`, and `2>` for standard output and error streams.
    
* How do you use pipes (`|`) to chain commands together?
    

#### **20\. What is SSH and how do you use it to connect to a remote server?**

* How do you set up an SSH connection and copy files using `scp` or `rsync`?
    

---

### **Advanced Linux Interview Questions (Optional)**

#### **21\. Explain how networking works in Linux.**

* What commands are used to check network interfaces (`ip`, `ifconfig`) and troubleshoot network issues (`ping`, `netstat`, `traceroute`)?
    

#### **22\. What is** `iptables` and how is it used in Linux?

* Explain the role of `iptables` in firewall configuration.
    

#### **23\. What is a Linux kernel module?**

* How do you list, load, and unload kernel modules using commands like `lsmod`, `modprobe`, and `rmmod`?
    

#### **24\. What is LVM (Logical Volume Management)?**

* Explain how LVM is used to manage disk storage dynamically and how to create, extend, and reduce LVM volumes.
    

#### **25\. How do you monitor system performance in Linux?**

* Discuss tools like `vmstat`, `iostat`, `sar`, and `free`.
    
* How do you track CPU, memory, and disk performance?
    

# **Top Linux Interview Questions with Answers**

#### **1\. What is Linux?**

**Answer:**  
Linux is an open-source operating system based on the Unix kernel. It is the core software that manages hardware resources and provides essential services to applications. The term "Linux" often refers to the entire Linux OS (kernel + software), though technically, Linux is just the kernel. The operating system includes utilities, graphical environments, and other software.

#### **2\. Explain the File System Hierarchy in Linux.**

**Answer:**  
The Linux file system is organized in a hierarchical structure, with `/` as the root directory. Common directories include:

* `/bin`: Essential user command binaries (e.g., `ls`, `cp`).
    
* `/home`: Users' home directories.
    
* `/etc`: Configuration files.
    
* `/var`: Variable data like logs.
    
* `/usr`: User programs and system utilities.
    
* `/tmp`: Temporary files created by system processes and users.
    

#### **3\. What are the basic file permissions in Linux?**

**Answer:**  
Linux permissions are divided into three types: `r` (read), `w` (write), and `x` (execute), and are set for three classes: user (owner), group, and others. For example, `rwxr-xr--` means the owner has full permission, the group can read and execute, and others can only read.

#### **4\. How do you change file permissions?**

**Answer:**

* Use `chmod` to change file permissions. For example:
    
    ```plaintext
      chmod 755 file.txt
    ```
    
    This grants the owner full permissions (7), the group read and execute (5), and others read and execute (5).
    
* To change the ownership of a file, use `chown`:
    
    ```plaintext
      chown user:group file.txt
    ```
    
    This assigns ownership of `file.txt` to the specified user and group.
    

#### **5\. What are the differences between** `sudo` and `su`?

**Answer:**

* `sudo`: Allows a permitted user to execute a command as the superuser or another user. It’s temporary and doesn't change the current user’s session.
    
* `su`: Switches to the superuser (or another user) and starts a new shell session as that user.
    

#### **6\. What is a shell?**

**Answer:**  
A shell is a command-line interface that allows users to interact with the operating system. Common shells include:

* **Bash**: Default on most Linux distributions.
    
* **Zsh**: An extended shell with more features.
    

To change the default shell, use the `chsh` command:

```plaintext
chsh -s /bin/zsh
```

#### **7\. Explain the purpose of environment variables in Linux.**

**Answer:**  
Environment variables store information that can be used by the shell or applications. Common variables:

* `PATH`: Directories the shell searches for executable files.
    
* `HOME`: The current user's home directory.
    
* `USER`: The current username.
    

To create an environment variable:

```plaintext
export MY_VAR="Hello"
```

#### **8\. How do you check the current running processes?**

**Answer:**  
To view running processes:

* `ps`: Displays active processes.
    
    ```plaintext
      ps aux
    ```
    
* `top`: Displays a dynamic real-time view of system processes.
    
* `htop`: An interactive version of `top` (needs to be installed).
    
* `pgrep`: Search for a specific process by name.
    

#### **9\. What is the difference between hard links and soft (symbolic) links?**

**Answer:**

* **Hard link**: Directly points to the inode (data) of the file. Changes in the original file reflect in the hard link, and both share the same data.
    
* **Symbolic (soft) link**: Points to the filename. It’s more like a shortcut. If the original file is deleted, the symlink becomes broken.
    

To create:

* **Hard link**:
    
    ```plaintext
      ln original.txt hardlink.txt
    ```
    
* **Symbolic link**:
    
    ```plaintext
      ln -s original.txt symlink.txt
    ```
    

#### **10\. Explain package management in Linux.**

**Answer:**  
Linux uses package managers to install, update, and remove software:

* **Debian-based (Ubuntu, etc.):** `apt`
    
    ```plaintext
      sudo apt install package_name
      sudo apt update && sudo apt upgrade
      sudo apt remove package_name
    ```
    
* **Red Hat-based (CentOS, etc.):** `yum` or `dnf`
    
    ```plaintext
      sudo yum install package_name
      sudo yum update
    ```
    

#### **11\. How do you search for files in Linux?**

**Answer:**

* `find`: Searches for files based on various criteria (name, size, etc.).
    
    ```plaintext
      find /home/user -name "*.txt"
    ```
    
* `locate`: Searches an indexed database of files.
    
    ```plaintext
      locate filename
    ```
    

#### **12\. How do you view the content of a file?**

**Answer:**

* `cat`: Concatenates and displays the contents of a file.
    
* `less` and `more`: Pagers to view large files one screen at a time.
    
* `head`: Displays the first few lines.
    
* `tail`: Displays the last few lines. Use `tail -f` to follow a file in real time.
    
    ```plaintext
      tail -f /var/log/syslog
    ```
    

#### **13\. What is** `cron` and how do you schedule jobs using it?

**Answer:**  
`cron` is used to schedule recurring tasks in Linux. A cron job consists of five fields (minute, hour, day, month, day of the week) followed by the command:

```plaintext
30 2 * * * /path/to/script.sh
```

This runs [`script.sh`](http://script.sh/) every day at 2:30 AM.

To manage cron jobs:

* **View jobs**: `crontab -l`
    
* **Edit jobs**: `crontab -e`
    
* **Remove jobs**: `crontab -r`
    

#### **14\. What are system logs and where are they stored?**

**Answer:** System logs are stored in the `/var/log/` directory. Common log files include:

* `/var/log/syslog` or `/var/log/messages`: General system log.
    
* `/var/log/auth.log`: Authentication logs.
    

Use `journalctl` to view systemd logs:

```plaintext
journalctl -xe
```

#### **15\. What is the difference between** `df` and `du` commands?

**Answer:**

* `df`: Displays the available disk space on file systems.
    
    ```plaintext
      df -h
    ```
    
    The `-h` flag shows human-readable sizes (e.g., MB, GB).
    
* `du`: Shows the disk usage of files and directories.
    
    ```plaintext
      du -sh /path/to/directory
    ```
    

#### **16\. How do you manage services in Linux?**

**Answer:**  
Use `systemctl` for managing system services (on `systemd` systems):

```plaintext
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
sudo systemctl status nginx
```

For older systems without `systemd`, you may use `service`:

```plaintext
sudo service nginx start
```

#### **17\. What is the difference between a terminal emulator and a shell?**

**Answer:**

* A **terminal emulator** (like GNOME Terminal, Konsole) is a program that emulates a physical terminal and allows users to interact with the shell.
    
* A **shell** (like Bash or Zsh) is a command-line interpreter that runs commands.