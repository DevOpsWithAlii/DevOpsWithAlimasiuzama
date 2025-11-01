---
title: "Ansible - 1 (Write your first playbook)"
datePublished: Tue Sep 09 2025 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cm69b4ybo000q09i39hvydupv
slug: ansible-1-write-your-first-playbook
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1737635167278/e938680f-d06d-4ad7-9c62-94b6fe26b727.avif
tags: ansible, google, 90daysofdevops, trainwithshubham

---

[**Ansible Basics - Getting Started**](https://bhavyabojanapalli.hashnode.dev/ansible-day-1-write-your-first-playbook#heading-ansible-basics-getting-started)

* [**Introduction to Ansible**](https://bhavyabojanapalli.hashnode.dev/ansible-day-1-write-your-first-playbook#heading-introduction-to-ansible)
    
* [**Setting Up Your Environment**](https://bhavyabojanapalli.hashnode.dev/ansible-day-1-write-your-first-playbook#heading-setting-up-your-environment)
    
* [**Writing Your First Playbook**](https://bhavyabojanapalli.hashnode.dev/ansible-day-1-write-your-first-playbook#heading-writing-your-first-playbook)
    

### **Ansible Basics - Getting Started**

**What is Ansible?**

Ansible is a powerful open-source automation tool that can help you manage and configure your servers, applications, and infrastructure effortlessly. Whether you're a seasoned pro or a beginner in the world of DevOps, Ansible is here to simplify your life.

**Why Use Ansible?**

* **Simplicity**: Ansible uses plain English and YAML files for configuration, making it accessible to everyone.
    
* **Agentless**: You don't need to install agents on target servers. Ansible works over SSH or WinRM, which most systems support by default.
    
* **Versatility**: It can handle a wide range of tasks, from simple server configuration to complex application deployments.
    

### **Introduction to Ansible**

**Ansible Components**

* **Control Node**: This is your Ansible machine, where you write your playbooks and manage everything from.
    
* **Managed Nodes**: These are the servers or devices you want to automate. Ansible communicates with them via SSH (Linux) or WinRM (Windows).
    
* **Playbooks**: Think of them as recipes. Playbooks define the tasks you want to perform on your managed nodes.
    
* **Modules**: These are Ansible's building blocks. Modules are used to execute tasks, such as installing software or creating files.
    

### **Setting Up Your Environment**

**Step 1: Install Ansible**

On your control node (your laptop or a dedicated server):

```plaintext
sudo apt-get install ansible  # For Ubuntu
```

**Step 2: Inventory File**

Create an inventory file (e.g., `inventory`) where you list the IP addresses or hostnames of your managed nodes:

```plaintext
[web]
192.168.1.10
[database]
192.168.1.11
```

**Step 3: SSH Key**

Make sure you have SSH access to your managed nodes. If not, generate an SSH key on your control node and copy the public key to your managed nodes.

```plaintext
ssh-keygen  # Generate SSH key
ssh-copy-id <username>@<managed_node_ip>  # Copy your SSH key to the managed node
```

**Step 4: Test the Connection**

You can test the connection to your managed nodes using the following command:

```plaintext
ansible -m ping -i inventory all
```

### **Writing Your First Playbook**

Now, it's time to put Ansible to work. Create your first playbook (e.g., `my_first_playbook.yml`) using a text editor:

```plaintext
---
- name: Install Nginx
  hosts: web
  become: yes  # Run tasks with sudo
  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes

    - name: Install Nginx
      apt:
        name: nginx
        state: present

    - name: Start Nginx
      service:
        name: nginx
        state: started
```

In this simple playbook, we:

1. Update the package cache.
    
2. Install Nginx.
    
3. Start the Nginx service.
    

To run your playbook, use the following command:

```plaintext
ansible-playbook -i inventory my_first_playbook.yml
```

Ansible will take care of installing Nginx on your web server.