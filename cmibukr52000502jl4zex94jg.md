---
title: "Ansible  — A Complete Beginner Guide"
datePublished: Sun Nov 23 2025 15:02:56 GMT+0000 (Coordinated Universal Time)
cuid: cmibukr52000502jl4zex94jg
slug: ansible-a-complete-beginner-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1763910122785/268476d8-8387-45bd-925e-098fe2507550.png
tags: ansible, 90daysofdevops, ansible-playbook

---

Ansible is one of the most powerful automation tools every DevOps engineer should know. Whether you want to install software on 100+ servers, configure systems, manage deployments, or automate daily tasks — Ansible makes everything super fast and simple.

### You now will understand:

* What is Ansible
    
* Why use Ansible
    
* Control node & managed nodes
    
* Inventory file and common fields
    
* Ad-hoc commands and examples
    
* Modules and when to use them
    

### **What is Ansible?** → **Why to use it?** → **Architecture** → **Installation** → **Inventory** → **Ad-Hoc Commands**, and finally **hands-on examples**.

# What is Ansible?

Ansible is an **open-source automation tool** used for:

* Installing packages
    
* Managing configurations
    
* Running commands on multiple servers
    
* Deploying applications
    
* Orchestrating DevOps pipelines
    

Think of Ansible as a **remote control for your servers**. You write instructions → Ansible executes them on all your machines.

# Why Ansible?

Here’s why it’s loved in DevOps:

* No agent required (uses SSH)
    
* Easy to learn (human-readable YAML)
    
* Works everywhere (cloud, containers, on-prem)
    
* Scales from small to very large
    
* Free & open-source
    

# Ansible Architecture (Simple Explanation)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1763909820240/7af9d20c-24ee-4868-b101-c1a6c99f3704.png align="center")

Ansible has only **two components**:

* **Control Node (Master)** — The machine where you install Ansible and run commands/playbooks.
    
* **Managed Nodes (Clients)** — The servers Ansible controls (via SSH).
    

**Flow:** You → Ansible CLI → SSH → Target Server → Action executed

# Ansible Setup on Ubuntu (Control Node)

### 1\. nstall Ansible

```bash
sudo add-apt-repository ppa:ansible/ansible
sudo apt update -y
sudo apt install ansible -y
```

### 2\. Setup SSH Key

```bash
cd ~/.ssh
vim ansible_key
chmod 700 ~/.ssh
chmod 600 ~/.ssh/ansible_key
```

Add the same public key to your server’s `authorized_keys`.

### 3\. Test SSH connection

```bash
ssh -i ~/.ssh/ansible_key ubuntu@<server-ip>

# Fix errore if not connect servers
export ANSIBLE_HOST_KEY_CHECKING=False
```

# Inventory File (Very Important)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1763910043123/0d0ac686-a212-4ef8-9bb8-cfefb5594447.png align="center")

Default inventory location:

```plaintext
/etc/ansible/hosts
```

Example `hosts` file:

```plaintext
[servers]
server1 ansible_host=3.145.29.59
server2 ansible_host=3.150.114.25

[all:vars]
ansible_user=ubuntu
ansible_python_interpreter=/usr/bin/python3
ansible_ssh_private_key_file=/home/ubuntu/.ssh/ansible_key
```

### Inventory fields quick reference

| Field | Meaning | Example |
| --- | --- | --- |
| `ansible_host` | IP or hostname used to connect | `ansible_host=3.145.29.59` |
| `ansible_user` | Remote user to login as | `ansible_user=ubuntu` |
| `ansible_ssh_private_key_file` | Path to private key on control node | `/home/ubuntu/.ssh/ansible_key` |
| `ansible_port` | SSH port if not 22 | `ansible_port=2222` |
| group names | Logical grouping of hosts | `[webservers]`, `[db]` |

### Verify inventory

```bash
ansible-inventory --list -i hosts
```

# Test Connection with Ping Module

```bash
ansible all -m ping -i hosts
```

If you see `pong`, your setup is perfect. 🎉

# Ansible Ad-Hoc Commands (Cheat Sheet)

Ad-hoc commands let you run quick commands without playbooks.

## Common ad-hoc tasks table

| Task | Command example | Notes |
| --- | --- | --- |
| Memory usage | `ansible all -a "free -h" -i hosts` | Quick check across hosts |
| Disk usage | `ansible all -a "df -h" -i hosts` | Useful before installs |
| Uptime | `ansible all -a "uptime" -i hosts` | See CPU/load averages |
| Install package | `ansible server1 -b -a "apt-get install nginx -y" -i hosts` | Use `-b` to sudo |
| Service state | `ansible all -m service -a "name=nginx state=started" -b -i hosts` | Manage services with module |

# Install Nginx (Examples)

```bash
ansible server1 -b -a "apt-get update" -i hosts
ansible server1 -b -a "apt-get install nginx -y" -i hosts
ansible server1 -b -a "systemctl stop nginx" -i hosts
ansible server1 -b -a "apt-get remove nginx -y" -i hosts
```

# Install Docker (Examples)

```bash
ansible server1 -b -a "apt-get update" -i hosts
ansible server1 -b -a "apt-get install docker.io -y" -i hosts
ansible server1 -b -a "systemctl status docker" -i hosts
ansible server1 -b -a "apt-get remove docker.io -y" -i hosts
```

# Package Management (Module vs Shell)

**Using module (recommended):**

```bash
ansible all -m apt -a "name=curl,git state=present update_cache=yes" -b -i hosts
```

**Ad-hoc shell (less ideal):**

```bash
ansible all -a "apt-get install -y curl git" -b -i hosts
```

### Quick comparison table: Module vs Ad-hoc shell

| Factor | Module (e.g., `apt`) | Ad-hoc shell (`-a`) |
| --- | --- | --- |
| Idempotence | ✅ Ensures desired state | ❌ May not be idempotent |
| Readability | ✅ Structured arguments | ⚠️ Free-form commands |
| Error handling | ✅ Better (Ansible knows failure) | ⚠️ Harder to parse failures |
| Best use | Playbooks & automation | Quick one-off tasks |

# Service Management (Module examples)

```bash
ansible all -m ansible.builtin.service -a "name=nginx state=started" -b -i hosts
ansible all -m ansible.builtin.service -a "name=nginx state=restarted" -b -i hosts
ansible all -a "systemctl is-active nginx" -b -i hosts
```

# File & Log Commands

```bash
ansible all -a "du -sh /home/*" -b -i hosts
ansible all -a "tail -n 50 /var/log/syslog" -b -i hosts
```

# Common Ansible Modules (Handy Table)

| Module | Purpose | Short example |
| --- | --- | --- |
| `apt` | Manage apt packages (Debian/Ubuntu) | `-m apt -a "name=nginx state=present"` |
| `yum` | Manage yum packages (RHEL/CentOS) | `-m yum -a "name=httpd state=present"` |
| `service` | Start/stop/restart services | `-m service -a "name=nginx state=started"` |
| `copy` | Copy files to remote hosts | `-m copy -a "src=local dest=/tmp/"` |
| `template` | Jinja2 templates rendering | `-m template -a "src=site.j2 dest=/etc/nginx/conf.d/site.conf"` |
| `file` | Manage file attributes & permissions | `-m file -a "path=/tmp/foo state=touch mode=0644"` |
| `command` | Run commands (no shell) | `-m command -a "/usr/bin/uptime"` |
| `shell` | Run shell commands (with shell) | `-m shell -a "echo $HOME"` |

# Notes & Best Practices

* Always use `-b` when you need root privileges.
    
* Prefer modules over raw shell commands.
    
* Keep inventory organized with groups.
    
* Test changes on a staging group before production.