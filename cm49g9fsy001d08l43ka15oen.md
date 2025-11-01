---
title: "Docker Cheatsheet — All Essential Commands for DevOps Engineers"
datePublished: Thu Oct 16 2025 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cm49g9fsy001d08l43ka15oen
slug: docker-cheatsheet-all-essential-commands-for-devops-engineers
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1761245842998/b0ead7f5-79e1-4f62-95b9-d3f34201fd8f.png
tags: linux, docker, 90daysofdevops, 90daysofdevopschallenge

---

Table of Contents

1. [Install & Setup Docker](https://chatgpt.com/c/68fa6e7d-5d68-8322-b818-c35b94cef815#install--setup-docker)
    
2. [Docker Basics](https://chatgpt.com/c/68fa6e7d-5d68-8322-b818-c35b94cef815#docker-basics)
    
3. [Container Management](https://chatgpt.com/c/68fa6e7d-5d68-8322-b818-c35b94cef815#container-management)
    
4. [Image Management](https://chatgpt.com/c/68fa6e7d-5d68-8322-b818-c35b94cef815#image-management)
    
5. [Dockerfile Commands](https://chatgpt.com/c/68fa6e7d-5d68-8322-b818-c35b94cef815#dockerfile-commands)
    
6. [Volumes & Data Management](https://chatgpt.com/c/68fa6e7d-5d68-8322-b818-c35b94cef815#volumes--data-management)
    
7. [Networking in Docker](https://chatgpt.com/c/68fa6e7d-5d68-8322-b818-c35b94cef815#networking-in-docker)
    
8. [Docker Compose](https://chatgpt.com/c/68fa6e7d-5d68-8322-b818-c35b94cef815#docker-compose)
    
9. [Docker Logs & Inspect](https://chatgpt.com/c/68fa6e7d-5d68-8322-b818-c35b94cef815#docker-logs--inspect)
    
10. [System Cleanup & Info](https://chatgpt.com/c/68fa6e7d-5d68-8322-b818-c35b94cef815#system-cleanup--info)
    
11. [Bonus: Docker Useful Shortcuts](https://chatgpt.com/c/68fa6e7d-5d68-8322-b818-c35b94cef815#bonus-docker-useful-shortcuts)
    

---

## ⚙️ Install & Setup Docker

```bash
# Install Docker on Ubuntu
sudo apt update
sudo apt install docker.io -y

# Start and enable Docker service
sudo systemctl start docker
sudo systemctl enable docker

# Check version
docker --version

# Check Docker service status
sudo systemctl status docker

# Run hello-world container to test
sudo docker run hello-world
```

---

## 🐋 Docker Basics

```bash
docker version              # Show Docker version
docker info                 # Display system-wide information
docker help                 # Show all Docker commands
```

---

## 📦 Container Management

```bash
# Run a new container
docker run -d nginx                         # Run in background
docker run -it ubuntu bash                  # Run interactively
docker run --name myapp -d nginx            # Assign a custom name
docker run -p 8080:80 nginx                 # Map ports (host:container)
docker run -v /host/data:/container/data nginx  # Mount volume
docker run -e VAR=value nginx               # Set environment variable

# List containers
docker ps                                   # List running containers
docker ps -a                                # List all containers

# Container operations
docker stop container_id                    # Stop container
docker start container_id                   # Start stopped container
docker restart container_id                 # Restart container
docker rm container_id                      # Remove container
docker kill container_id                    # Kill running container
docker pause container_id                   # Pause container
docker unpause container_id                 # Resume container
docker rename old_name new_name             # Rename container
docker attach container_id                  # Attach to running container
docker exec -it container_id bash           # Run command inside container
docker logs container_id                    # Show container logs
docker inspect container_id                 # Detailed info about container

# Export & import container
docker export container_id > mycontainer.tar
docker import mycontainer.tar myimage:latest
```

---

## 🧱 Image Management

```bash
# List and pull images
docker images                               # List local images
docker pull nginx                           # Download image
docker search ubuntu                        # Search image from Docker Hub

# Build and tag image
docker build -t myimage:1.0 .               # Build image from Dockerfile
docker tag myimage:1.0 myrepo/myimage:latest  # Tag image

# Remove & clean images
docker rmi image_id                         # Remove image
docker rmi -f image_id                      # Force remove
docker image prune                          # Remove unused images
docker image inspect image_id               # Show image details

# Save & load images
docker save -o nginx.tar nginx:latest       # Save image as tar
docker load -i nginx.tar                    # Load image from tar
```

---

## 🧾 Dockerfile Commands

> Below are the main Dockerfile instructions used to build custom images.

```Dockerfile
# Dockerfile Example
FROM ubuntu:20.04
MAINTAINER ali_masiu_zama

RUN apt update && apt install -y nginx
COPY index.html /var/www/html/
WORKDIR /app
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### 🔹 Common Dockerfile Instructions

| Command | Description |
| --- | --- |
| `FROM` | Base image |
| `MAINTAINER` | Author name |
| `RUN` | Execute command during build |
| `COPY` | Copy files from host to container |
| `ADD` | Copy files (with URL/tar support) |
| `WORKDIR` | Set working directory |
| `EXPOSE` | Expose a port |
| `ENV` | Set environment variables |
| `CMD` | Define default command |
| `ENTRYPOINT` | Define executable command |

---

## 📂 Volumes & Data Management

```bash
# Create volume
docker volume create myvolume

# List volumes
docker volume ls

# Inspect a volume
docker volume inspect myvolume

# Run container with volume
docker run -d -v myvolume:/data nginx

# Remove unused volumes
docker volume prune
```

---

## 🌐 Networking in Docker

```bash
# List networks
docker network ls

# Inspect network
docker network inspect bridge

# Create custom network
docker network create mynetwork

# Connect/Disconnect container
docker network connect mynetwork container_id
docker network disconnect mynetwork container_id

# Run container in specific network
docker run -d --name app1 --network mynetwork nginx
```

---

## 🧩 Docker Compose

> Use `docker-compose.yml` to manage multi-container applications.

```bash
# Start containers in background
docker-compose up -d

# Stop containers
docker-compose down

# Restart containers
docker-compose restart

# View logs
docker-compose logs -f

# List running services
docker-compose ps
```

**Example** `docker-compose.yml`:

```yaml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "8080:80"
  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: root
```

---

## 🧾 Docker Logs & Inspect

```bash
docker logs container_id            # View logs
docker logs -f container_id         # Follow live logs
docker inspect container_id         # Inspect details
docker events                       # Show real-time Docker events
docker stats                        # Monitor resource usage
```

---

## 🧹 System Cleanup & Info

```bash
docker system df                    # Show disk usage
docker system prune                 # Remove unused containers, networks, images
docker system prune -a              # Remove everything not in use
docker info                         # Display system information
docker version                      # Show Docker version details
```

---

## ⚡ Bonus: Docker Useful Shortcuts

```bash
# Remove all stopped containers
docker rm $(docker ps -aq)

# Remove all unused images
docker rmi $(docker images -q)

# Stop all running containers
docker stop $(docker ps -aq)

# Remove all volumes
docker volume rm $(docker volume ls -q)
```

---

Docker simplifies containerization — and this cheatsheet gives you a **one-stop reference** to all major Docker commands used by DevOps engineers.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1670863735841/r6xdXpsap.png?auto=compress,format&format=webp&auto=compress,format&format=webp align="left")

> ***Thank you for reading. Happy Learning 😊!!!***