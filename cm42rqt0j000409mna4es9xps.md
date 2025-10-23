---
title: "Docker"
datePublished: Fri Nov 29 2024 13:17:52 GMT+0000 (Coordinated Universal Time)
cuid: cm42rqt0j000409mna4es9xps
slug: docker
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1761245336019/73593592-743b-467f-9120-6d21f4fbfcd3.png
tags: linux, docker, devops, 90daysofdevops

---

## **Table of contents**

* [Docker:](https://namg.hashnode.dev/docker-day16-of-90daysofdevops#heading-docker)
    
* [Docker installation](https://namg.hashnode.dev/docker-day16-of-90daysofdevops#heading-docker-installation)
    

### Docker:

Docker is a platform and tool that enables you to develop, deploy, and run applications inside containers.

Containers are lightweight, standalone, and executable software packages that include everything needed to run a piece of software, including the code, runtime, system tools, system libraries, and settings.

**Container contains:**

1. `Application/webserver Software`
    
2. `Minimal OS`
    
3. `Dependency Libraries and Binaries`
    
4. `Application Code`
    
5. `Supported configuration data`
    

**You can see Docker architecture in below picture:**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691244936949/327edab2-e77c-40b5-8b78-ad24c5908a55.png?auto=compress,format&format=webp align="left")

### **Docker installation**

* Create a new ec2 instance. I have used Amazon-Linux-image as AMI and my instance name is: builder-server
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691240757684/32db5c3c-dba9-4f4f-8da2-15cc7449b44a.png?auto=compress,format&format=webp align="left")
    
* Install docker over there
    
    command to install docker : `yum install docker -y`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691240868655/4bdbaaa8-d855-4b75-8833-ac1d6ed4434d.png?auto=compress,format&format=webp align="left")
    
* check the docker service status
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691241074076/ea99ce76-9aa6-4759-9ae1-a2ccbae53884.png?auto=compress,format&format=webp align="left")
    
* Start docker service
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691241225062/a1c03ad3-0a8e-420b-b1cd-5d5d083d6373.png?auto=compress,format&format=webp align="left")
    

📌Tasks

1. As you have already installed Docker through the above steps, now is the time to run Docker commands.
    
    * Use the `docker run` command to start a new container and interact with it through the command line.
        
        `docker run hello-world`
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691241462505/37010bd0-3232-454b-87b0-a67717fe0639.png?auto=compress,format&format=webp align="left")
        
    * Use the `docker inspect` command to view detailed information about a container or image.
        

\-*To see all the running containers the command is:* `docker ps -a`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691242325347/2d2d4a17-e550-4f76-8d1c-0d40e017f677.png?auto=compress,format&format=webp align="left")

\-*We can use the inspect command to get low-level configuration information about various Docker objects like images, containers, volumes, networks, nodes, etc.  
To see, for example, on which node a container is deployed or what ports it listens to.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691242437405/335a76d1-1264-481d-b6dc-29566f1e77ef.png?auto=compress,format&format=webp align="left")

* Use the `docker port` command to list the port mappings for a container.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691242895855/9638000f-b300-46bb-a206-5d15a51e1acf.png?auto=compress,format&format=webp align="left")
    
* Use the `docker stats` command to view resource usage statistics for one or more containers.
    
    `docker stats` *command is used to see* ***live stream a container's runtime metrics\****. The command supports CPU, memory usage, memory limit, and network IO metrics.\*
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691243016353/f9cc0560-c1f5-4563-95b6-6ca3960a38c9.png?auto=compress,format&format=webp align="left")
    
* Use the `docker top` command to view the processes running inside a container.  
    `docker top <container ID >` *command allows users to display the ps output for the main process of a given container ID or name.*
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691243237774/93b81f00-03ca-448a-8b5f-2e117b19e777.png?auto=compress,format&format=webp align="left")