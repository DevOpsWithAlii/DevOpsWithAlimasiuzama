---
title: "Ansible - 02 (Ansible Playbooks & Modules)"
datePublished: Wed Feb 05 2025 10:54:53 GMT+0000 (Coordinated Universal Time)
cuid: cm6rskv00001409l4eopv9xr5
slug: ansible-02-ansible-playbooks-modules
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1760984776732/cb766a4b-a9fd-4a79-a3e3-bd69e3a052cd.png
tags: linux, docker, aws, ansible, devops

---

## Understanding Ansible Playbooks and Modules (With Real-World Example)

### Table of Contents

* [What is a Playbook?](https://chatgpt.com/c/68f67b22-fc30-8324-952c-e49cb693e19e#what-is-a-playbook)
    
* [What are Ansible Modules?](https://chatgpt.com/c/68f67b22-fc30-8324-952c-e49cb693e19e#what-are-ansible-modules)
    
* [Building a Real-World Playbook: MySQL on AWS EC2](https://chatgpt.com/c/68f67b22-fc30-8324-952c-e49cb693e19e#building-real-world-playbook)
    

In the [previous post of this Ansible series](https://chatgpt.com/c/68f67b22-fc30-8324-952c-e49cb693e19e#), you learned the fundamentals of **Ansible** — what it is, how it works, and why it’s so widely used in DevOps automation.

In this article, let’s take a step further and explore two of Ansible’s most powerful building blocks:  
👉 **Playbooks** and **Modules**.

And to make it practical, we’ll also build a **real-world Ansible Playbook** that installs and configures **MySQL on an AWS EC2 instance**.

## What is a Playbook?

Ansible **Playbooks** are YAML files that define a series of **tasks and configurations** to be executed on remote systems.  
Think of a Playbook as a **recipe** that tells Ansible *what to do and in what order*.

A playbook can:

* Install software
    
* Configure servers
    
* Start or stop services
    
* Deploy complete applications
    

They are also version-controllable, shareable, and reusable — which makes them perfect for **infrastructure automation**.

### How Do Playbooks Work?

A Playbook consists of one or more **plays**, and each play defines:

* The **hosts** (servers) where it should run
    
* The **tasks** to execute
    
* Optional **variables**, **handlers**, and **conditions**
    

Ansible connects to the target servers (via SSH) and executes each task **sequentially**.

Here’s a simple example 👇

```yaml
---
- name: Configure Web Servers
  hosts: webservers
  tasks:
    - name: Install Apache
      apt:
        name: apache2
        state: present

    - name: Start Apache Service
      service:
        name: apache2
        state: started
```

🔍 **Explanation:**  
This playbook installs and starts Apache on all servers listed under the *webservers* inventory group.

---

## 🧠 What are Ansible Modules?

**Modules** are the real workhorses of Ansible.  
They are **standalone scripts** that Ansible uses to perform specific actions like installing a package, copying a file, creating a user, or managing services.

Modules are designed to be **idempotent**, meaning you can run them multiple times safely without changing the system’s state unnecessarily.

---

### 📦 Commonly Used Ansible Modules

| Module | Purpose |
| --- | --- |
| `apt`, `yum` | Manage software packages on Linux systems |
| `copy`, `template` | Manage files and templates |
| `user`, `group` | Manage users and groups |
| `service`, `systemd` | Control services (start/stop/restart) |
| `shell`, `command` | Execute shell commands |

To explore all available modules:  
👉 [Ansible Module Index (official docs)](https://docs.ansible.com/ansible/latest/collections/index_module.html)

---

## 💡 Building Real-World Playbooks

### Installing and Configuring MySQL on an EC2 Instance

Now that you know what Playbooks and Modules are, let’s combine them to automate a **real-world task** — setting up a **MySQL database on AWS EC2**.

---

### 🪜 Step 1: Prerequisites

Before you begin, ensure:

* You have an **AWS EC2 instance** (Ubuntu/CentOS) with SSH access.
    
* **Ansible** is installed on your local machine.
    

### 🪶 Step 2: Create an Ansible Playbook

Create a file named `mysql_installation.yml` and add the following content:

```yaml
---
- name: Install and Configure MySQL on EC2
  hosts: your_ec2_instance
  become: yes
  vars:
    mysql_root_password: your_root_password

  tasks:
    - name: Update APT cache (for Ubuntu)
      apt:
        update_cache: yes
      when: ansible_os_family == 'Debian'
      tags: [update_cache]

    - name: Install MySQL Server (Ubuntu)
      apt:
        name: mysql-server
        state: present
      when: ansible_os_family == 'Debian'
      tags: [install_mysql]

    - name: Install MySQL Server (CentOS)
      yum:
        name: mysql-server
        state: present
      when: ansible_os_family == 'RedHat'
      tags: [install_mysql]

    - name: Start MySQL service
      service:
        name: mysql
        state: started
      tags: [start_mysql]

    - name: Secure MySQL installation
      mysql_user:
        name: root
        password: "{{ mysql_root_password }}"
        host: localhost
      tags: [secure_mysql]
```

### 🧩 Step 3: Understanding the Playbook

* `become: yes` → runs tasks with admin privileges.
    
* `mysql_root_password` → define your MySQL root password.
    
* Tasks include:
    
    * Updating package cache
        
    * Installing MySQL
        
    * Starting the MySQL service
        
    * Securing the MySQL root user
        

### ▶️ Step 4: Run the Playbook

Execute the playbook using:

```bash
ansible-playbook mysql_installation.yml -i "your_ec2_instance_ip," --private-key=/path/to/key.pem
```

Replace:

* `your_ec2_instance_ip` → with your EC2 public IP
    
* `/path/to/key.pem` → with your private key path
    

Once complete, MySQL will be fully installed and configured on your EC2 instance.

## 🚀 Wrapping Up

You just automated a **real-world database setup** using Ansible!

In this post, you learned:

* What Playbooks and Modules are
    
* How they work together
    
* How to create a production-like playbook for MySQL setup
    

In the **next part**, we’ll dive deeper into **Ansible Roles** — a powerful way to organize large-scale automation projects.