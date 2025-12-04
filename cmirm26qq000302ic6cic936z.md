---
title: "What is Ansible Playbook?"
datePublished: Thu Dec 04 2025 15:48:52 GMT+0000 (Coordinated Universal Time)
cuid: cmirm26qq000302ic6cic936z
slug: what-is-ansible-playbook
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1764863292971/821c6e20-0af7-432e-9254-2b1c472dfb9d.png
tags: ansible, ansible-playbook

---

An Ansible Playbook is a YAML file where you define the automation tasks to achieve a desired state on managed nodes.

* **Example: Playbook for Patching a Server**:
    

```bash
- name: Patch and update servers
  hosts: servers
  become: yes
  tasks:
    - name: Update the package manager cache
      apt:
        update_cache: yes
      when: ansible_os_family == "Debian"

    - name: Update all packages to their latest version (Debian-based)
      apt:
        upgrade: dist
      when: ansible_os_family == "Debian"

    - name: Update the package manager cache (RHEL-based)
      yum:
        name: '*'
        state: latest
      when: ansible_os_family == "RedHat"

    - name: Reboot the server if a kernel update was installed
      reboot:
        reboot_timeout: 300
```

* **Executing the Playbook**:
    

Step 1: Prepare the Inventory - Create an inventory file named hosts to define the servers.

```bash
  [servers]
  node1 ansible_host=3.142.79.205
  node2 ansible_host=3.143.210.190

  [all:vars]
  ansible_python_interpreter=/usr/bin/python3
  ansible_user=ubuntu
  ansible_ssh_private_key_file=/home/ubuntu/keys/ansible-master-key.pem
```

Step 2: Execute the playbook using the ansible-playbook command.

```bash
ansible-playbook -i hosts patch_server.yml
```

## **What are Ansible Roles?**

Roles are a way to organize playbooks into reusable components. A role typically includes tasks, templates, files, handlers, and variables.

Ansible roles are a way to organize playbooks into reusable components. They help break down a playbook into smaller, more manageable pieces, making it easier to scale and maintain. Each role is essentially a directory structure with predefined files and folders that Ansible recognizes and uses to perform specific tasks.

### **Role Directory Structure?**

When you create a role, Ansible expects a standard directory layout.

```bash
roles/
  <role_name>/
    tasks/         # Defines the main tasks for the role (main.yml is mandatory).
    handlers/      # Defines handlers (e.g., restart services).
    templates/     # Stores Jinja2 templates for configuration files.
    files/         # Contains static files to be copied to the target machine.
    vars/          # Stores variables specific to the role.
    defaults/      # Stores default variables (overridden by other variable sources).
    meta/          # Metadata about the role, including dependencies.
```

### **How to Create a Role?**

To create a role named webserver, use the command.

```bash
ansible-galaxy init webserver
```

This creates the directory structure for the webserver role.

* **Example: Role Directory Structure**:
    

```bash
roles/
  webserver/
    tasks/
      main.yml
    templates/
      index.html.j2
    vars/
      main.yml
    handlers/
      main.yml
```