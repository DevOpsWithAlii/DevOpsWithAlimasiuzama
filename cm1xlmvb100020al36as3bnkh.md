---
title: "Linux"
datePublished: Fri Oct 04 2024 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cm1xlmvb100020al36as3bnkh
slug: linux-day-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1728219882408/fb0343c6-2b83-40e9-8183-d850ab776b78.jpeg
tags: linux, aws, devops

---

# What is Linux ?

When it comes to operating systems, most people think of Windows or macOS. But have you ever heard of **Linux**? It’s an incredibly powerful and flexible operating system that many people love—and for good reason! In this post, we’ll dive into what Linux is all about, its cool features, and why you might want to give it a try.

## So, What Exactly is Linux?

Linux is an open-source operating system that first hit the scene in 1991, thanks to Linus Torvalds. What does "open-sources" mean? Simply put, it means anyone can download it, use it, and even modify it if they want to. Unlike Windows or macOS, which you have to pay for, Linux is free!

At its core, Linux is built around the **Linux kernel**—the part of the operating system that manages your computer's hardware. Around this kernel, there are many different **distributions** or "distros," each with its own twist. Some of the popular ones include:

* **Ubuntu**: Great for beginners with a user-friendly interface.
    
* **Debian**: A solid choice for more advanced users who want stability.
    
* **Fedora**: Always up-to-date with the latest features.
    
* **CentOS**: Perfect for servers and enterprise use.
    

## What Makes Linux Stand Out?

### 1\. **It’s Free!**

You can download and use Linux without spending a dime. This is a huge advantage for students or anyone on a budget.

### 2\. **It’s Stable and Reliable**

Linux systems are known for being rock-solid. Many web servers run on Linux because it can handle heavy loads without crashing. If you’re looking for an OS that won’t let you down, Linux is a great pick.

### 3\. **Enhanced Security**

Linux is often considered more secure than other operating systems. Its design includes permission settings that help prevent malware and unauthorized access. You’ll find that many hackers prefer Linux due to its robust security features.

### 4\. **Customization Galore**

One of the coolest things about Linux is how customizable it is. You can change almost anything—from the desktop environment to the applications you use. Want your computer to look and act a certain way? Go for it!

### 5\. **A Supportive Community**

There’s a huge community of Linux users out there who are always willing to help. If you run into problems, you can find support through forums, documentation, and tutorials. It’s a friendly environment where everyone helps each other out.

### 6\. **Tons of Software Options**

Linux has a vast library of software available for almost any task you can think of, whether you need a word processor, a graphics editor, or programming tools. Best of all, many of these applications are also free!

### 7\. **Multi-User Capabilities**

Linux is designed for multiple users. This means you can have several people using the same computer without stepping on each other’s toes. Each user can have their own files and settings.

# Why Should You Consider Using Linux?

### 1\. **Cost-Effective**

Since Linux is free, it’s a great option for anyone looking to save money. This can be especially appealing for businesses that want to cut software costs.

### 2\. **Performance on Older Hardware**

Linux can run smoothly on older machines that might struggle with newer versions of Windows or macOS. If you have an old laptop lying around, try installing Linux on it—it might breathe new life into the device!

### 3\. **Great for Learning and Development**

If you’re interested in programming or IT, Linux is a fantastic platform to learn on. It supports many programming languages and tools, making it easier to develop and deploy applications.

### 4\. **Control and Transparency**

With Linux, you have more control over your system. You can see how it works and what’s running on it, which isn’t always the case with proprietary operating systems. This transparency is a big plus for many users.

### 5\. **Community Engagement**

Using Linux means joining a global community of like-minded individuals. You can contribute to projects, attend meetups, and learn from others. It’s a great way to connect with people who share your interests.

### **Basic Linux Shell Scripting for DevOps Engineers**

As a DevOps engineer, learning Linux shell scripting is key to making your work more efficient. It allows you to automate tasks, handle system operations, and make it easier for users to interact with the operating system. Let's break down the basics of Linux, shell scripting, and go over some tasks that will get you started.

---

### What is a Kernel?

Think of the kernel as the brain of your computer. It's the core part of the operating system that manages everything—memory, CPU, input/output devices, and more. The kernel makes sure all parts of your system are working together smoothly.

---

### What is a Shell?

The shell is like a translator between you (the user) and the kernel (the system's brain). It takes the commands you type and translates them into instructions that the system can understand and execute. When you open a terminal in Linux, you’re working with the shell to interact with the system directly by typing commands. It’s your main tool to control the system.

---

### What is Linux Shell Scripting?

Linux shell scripting is the process of writing a series of commands (a script) that the shell can execute. These scripts help automate repetitive tasks, handle system administration, and make managing systems easier and more efficient. Shell scripting is super important in DevOps because it allows you to automate deployments, monitor systems, manage backups, and so much more.

---

### Tasks

#### 1\. Shell Scripting for DevOps: In Simple Terms

In DevOps, shell scripting is all about **automation**. As a DevOps engineer, you’ll often deal with multiple servers, services, and environments. Manually performing tasks on each one can be time-consuming and prone to errors. Shell scripts allow you to automate these tasks—saving you time and ensuring everything runs consistently.

For example, instead of manually updating software on 10 servers, you can write a shell script to do it automatically. Other common tasks you can automate include deploying code, monitoring system performance, and backing up data.

---

#### 2\. What is `#!/bin/bash`? Can We Write `#!/bin/sh` Instead?

`#!/bin/bash` is the **shebang** line at the start of a shell script. It tells the system which interpreter (shell) to use to run the script. In this case, it’s using the **Bash** shell (Bourne Again Shell), which is one of the most common shells in Linux.

Yes, you can write `#!/bin/sh` instead. The difference is that `sh` refers to the **Bourne shell**, which is simpler and has fewer features than `bash`. While most Linux systems have `sh` linked to `bash`, for advanced scripting it’s better to stick with `bash`.

---

#### 3\. Shell Script to Print "I will complete #90DaysOfDevOps challenge"

Here’s a basic shell script that prints out the message:

**Copy**

**Copy**

```plaintext
#!/bin/bash
echo "I will complete #90DaysOfDevOps challenge"
```

To run this:

1. Save the script in a file called [`challenge.sh`](http://challenge.sh/).
    
2. Give it permission to execute by running `chmod +x` [`challenge.sh`](http://challenge.sh/).
    
3. Run the script with `./`[`challenge.sh`](http://challenge.sh/).
    

---

#### 4\. Shell Script to Take User Input and Print Variables

This script takes input directly from the user and from arguments passed in when you run the script:

**Copy**

**Copy**

```plaintext
#!/bin/bash

# Take user input
echo "What's your name?"
read name

# Take arguments from the command line
arg1=$1
arg2=$2

# Print out the results
echo "Hello, $name!"
echo "Argument 1: $arg1"
echo "Argument 2: $arg2"
```

To run this script:

1. Save it as `input_`[`script.sh`](http://script.sh/).
    
2. Make it executable (`chmod +x input_`[`script.sh`](http://script.sh/)).
    
3. Run the script by passing arguments like this:
    
    **Copy**
    
    **Copy**
    
    ```plaintext
     ./input_script.sh DevOpsEngineer Coding
    ```
    

It will greet the user by name and print out the two arguments you passed in.

---

#### 5\. Example of an If-Else Statement in Shell Scripting

Here’s an example script that compares two numbers and prints out which one is greater:

**Copy**

**Copy**

```plaintext
#!/bin/bash

# Get numbers from the user
echo "Enter the first number:"
read num1
echo "Enter the second number:"
read num2

# Compare the numbers
if [ $num1 -gt $num2 ]; then
    echo "$num1 is greater than $num2"
elif [ $num1 -lt $num2 ]; then
    echo "$num1 is less than $num2"
else
    echo "Both numbers are equal"
fi
```

This script will prompt the user for two numbers, then compare them and let you know which one is bigger, smaller, or if they are equal.

---

### Wrapping Up

Shell scripting is a powerful tool for DevOps engineers, allowing you to automate tasks and make your workflows more efficient. Whether it’s deploying code, backing up files, or monitoring systems, writing and executing shell scripts will save you time and make your operations run smoothly.