---
title: "Mastering Docker Compose: Networks, Volumes, and Container Communication"
datePublished: Mon Nov 10 2025 19:22:47 GMT+0000 (Coordinated Universal Time)
cuid: cmhtj4uhx000402jlbow7d08i
slug: mastering-docker-compose-networks-volumes-and-container-communication
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1762802391916/73f068e2-e2a2-487c-83e7-7e78afc97537.png
tags: docker, devops, docker-network, docker-volume, 90daysofdevops

---

## Table of Contents

1. **Introduction**
    
2. **What is Docker Compose?**
    
3. **Setup and Prerequisites**
    
4. **Building a Docker Compose File**
    
5. Starting the Containers
    
6. **Testing Container -to- Container Communication**
    
7. **Understanding Volumes (Shared Data Between Containers)**
    
8. **Accessing Shared Files from the Host Machine**
    
9. Stop and Remove Containers
    
10. **Summary and Real-World Use Case**
    
11. Docker Compose Cleanup Summary
    

## 1\. Introduction

Docker Compose is one of the most powerful tools in the DevOps world — it helps you **define, manage, and run multi-container applications** effortlessly.

In this blog, we’ll walk through a **real example** where:

* We connect two containers: **Nginx** and **MySQL**
    
* Share data between them using **volumes**
    
* Enable communication between containers through **networks**
    
* Access that shared data from both inside and outside Docker!
    

By the end, you’ll have a solid understanding of how to build a production-style setup using `docker-compose`.

## 2\. What is Docker Compose?

Docker Compose allows you to define and run multiple containers using a single file — `docker-compose.yml`.  
Instead of running many `docker run` commands, you just define everything in YAML format and start all containers together.

## 3\. Setup and Prerequisites

Make sure you have Docker and Docker Compose installed.

```bash
sudo apt update
sudo apt install docker.io -y
sudo apt install docker-compose -y
```

Check the versions:

```bash
docker --version
docker compose version
```

Create a working directory:

```bash
mkdir ~/compose && cd ~/compose
```

## 4\. Building a Docker Compose File

Let’s create a file named `docker-compose.yml`:

```bash
vim docker-compose.yml
```

Paste this content 👇

```yaml
services:

  nginx:
    image: nginx:latest
    container_name: nginx
    ports:
      - 80:80
    volumes:
      - shared-data:/shared
    networks:
      - docker-network

  mysql:
    image: mysql:latest
    container_name: mysql
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: my_db
      MYSQL_USER: myuser
      MYSQL_PASSWORD: user
    volumes:
      - shared-data:/shared
    networks:
      - docker-network

volumes:
  shared-data:

networks:
  docker-network:
```

## 5\. Starting the Containers

Now, run the following command:

```bash
docker compose up -d
```

Check if containers are running:

```bash
docker ps
```

## 6\. Testing Container-to-Container Communication

You can test network connectivity using `ping`.

First, install ping inside Nginx container:

```bash
docker exec -it nginx bash
apt update && apt install -y iputils-ping   #iputils-ping is the package that contains the ping command
```

Then test:

```bash
ping mysql
```

Expected output:

```plaintext
PING mysql (172.18.0.3) 56(84) bytes of data.
64 bytes from mysql.compose_docker-network (172.18.0.3): icmp_seq=1 ttl=64 time=0.04 ms
```

This means **Nginx successfully communicates with MySQL** via Docker network.

## 7\. Understanding Volumes (Shared Data Between Containers)

Both containers share the same volume named **shared-data**.  
That means — if you create a file inside `/shared` in one container,  
you can read it from the other!

Enter inside the **Nginx container**:

```bash
docker exec -it nginx bash
```

Create a file:

```bash
echo "Hello from Nginx!" > /shared/test.txt    # Create a File {test.txt} inside shared Folder
cat /shared/test.txt
Hello from Nginx!
Exit                           #Exit from nginx Container
```

Now go inside **MySQL container**:

```bash
docker exec -it mysql bash           #Enter inside mysql container
```

Read the same file:

```bash
cat /shared/test.txt                 #See the mess
```

You’ll see:

```plaintext
Hello from Nginx!   
```

This confirms both containers share the same data.

## 8\. Accessing Shared Files from the Host Machine

Docker stores named volumes on the host system under:

```plaintext
Docker volume inspect compose_shared-data       #See docker volume mountpoint path

"Mountpoint": "/var/lib/docker/volumes/compose_shared-data/_data",  
```

Let’s locate our shared-data volume:

```bash
cd /var/lib/docker/volumes/compose_shared-data/_data 
ls
cat test.txt
```

You’ll see the same file and message:

```plaintext
hello from docker
```

That means — the same data is now visible:

* Inside **Nginx**
    
* Inside **MySQL**
    
* And on your **Ubuntu host**
    

## 9\. Stop and Remove Containers

If you want to stop everything:

```bash
docker compose down
```

This removes containers and networks but keeps the volume (data safe).

If you want to remove volumes too:

```bash
docker compose down -v
```

## 10\. Summary

You learned how to:

* Create and use **Docker networks**
    
* Create and share **volumes** between containers
    
* Connect containers and test communication
    
* Access data from both **containers** and **host**
    

## 11\. Docker Compose Cleanup Summary

When you’re done experimenting with your containers, you’ll often need to clean things up.  
Here’s what each `docker compose down` option does 👇

| Command | What It Removes | Images Deleted? |
| --- | --- | --- |
| `docker compose down` | Stops and removes **containers** and **networks** created by Compose | ❌ No |
| `docker compose down --volumes` | Removes **containers**, **networks**, and **volumes** | ❌ No |
| `docker compose down --rmi local` | Removes everything above **plus locally built images** (from `build:` in Compose) | ⚠️ Yes (only built images) |
| `docker compose down --rmi all` | Removes **containers**, **networks**, **volumes**, and **all images** | ✅ Yes (all images) |