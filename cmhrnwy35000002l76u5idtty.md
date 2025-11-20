---
title: "Docker Networking Explained with MySQL and Nginx Example"
datePublished: Sun Nov 09 2025 12:01:04 GMT+0000 (Coordinated Universal Time)
cuid: cmhrnwy35000002l76u5idtty
slug: docker-networking-explained-with-mysql-and-nginx-example
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1762689560097/7ff5d41a-28e8-4541-b579-2904ae92af57.png
tags: docker, google, devops, docker-network, 90daysofdevops

---

If you’ve ever wondered how Docker containers talk to each other, then this blog is for you!  
Docker Networking allows containers to **communicate securely and efficiently**, just like systems in a real network.

In this blog, we’ll create a **custom Docker network**, connect multiple containers (MySQL and Nginx) to it, and verify that they can communicate….

## **Table of Contents**

1. **Introduction to Docker Networking**
    
2. **Step 1 — What is Docker Network?**
    
3. **Step 2 — Create a Custom Network**
    
4. **Step 3 — Run Containers on the Same Network**
    
5. **Step 4 — Verify Network Connections**
    
6. **Step 5 — Why Custom Networks Matter**
    
7. **Conclusion & Key Takeaways**
    

## **1\. Introduction to Docker Networking**

Every Docker container is isolated by default — it has its own filesystem and network stack.  
But when containers need to share data or communicate, Docker networking comes into play.

Docker provides different types of networks like:

* **bridge** (default)
    
* user defined bridge
    
* **host**
    
* **none**
    
* **overlay**
    
* **macvlan**
    
* ip
    

In this example, we’ll use a **bridge network** — a simple and commonly used type for single-host setups.

## **2\. Step 1 — What is a Docker Network?**

A Docker network acts as a **virtual bridge** that connects multiple containers so they can communicate using container names instead of IPs.

When containers are in the same network:

* They can communicate using container names (e.g., `mysql`).
    
* They remain isolated from other networks for better security.
    

## **3\. Step 2 — Create a Custom Network**

Let’s create a custom network named `my-network`.

```bash
docker network create my-network
```

### Output Example:

```plaintext
b3a5a2d12c0a1c0eacb124a1f77e4f9cf2e67cb3b9cc83e7e4f7dc245a9cfd7b
```

### Verify the network:

```bash
docker network ls
```

| NETWORK ID | NAME | DRIVER | SCOPE |
| --- | --- | --- | --- |
| b3a5a2d12c0a | bridge | bridge | local |
| my-network | bridge | bridge | local |

Now, we have successfully created a **custom bridge network**.

## **4\. Step 3 — Run Containers on the Same Network**

### Run MySQL Container:

```bash
docker run -d --name mysql -e MYSQL_ROOT_PASSWORD=root --network=my-network mysql:latest
```

### Run Nginx Container:

```bash
docker run -d -v mysql-data:/var -p 80:80 --network=my-network nginx:latest
```

### Explanation:

| Option | Description |
| --- | --- |
| `--network=my-network` | Connects the container to your custom network |
| `--name` | Gives a readable name to the container |
| `-p 80:80` | Maps host port 80 to container port 80 |
| `-v mysql-data:/var` | Creates a named volume for data storage |
| `-e` | Sets an environment variable (in MySQL’s case, the root password) |

## **5\. Step 4 — Verify Network Connections**

Now, let’s check if both containers are connected to the same network.

### Command:

```bash
docker network inspect my-network
```

### Example Output:

```json
[
    {
        "Name": "my-network",
        "Id": "b3a5a2d12c0a1c0eacb124a1f77e4f9cf2e67cb3b9cc83e7e4f7dc245a9cfd7b",
        "Containers": {
            "2be185b15a7e": {
                "Name": "mysql",
                "IPv4Address": "172.18.0.2/16"
            },
            "9fd87cba456e": {
                "Name": "nginx",
                "IPv4Address": "172.18.0.3/16"
            }
        }
    }
]
```

✅ **Result:** Both containers are listed under the same network — `my-network`.  
This means they can communicate using container names like:

```bash
ping mysql
```

or

```bash
ping nginx
```

## **6\. Step 5 — Why Custom Networks Matter**

| Advantage | Description |
| --- | --- |
| **Container-to-Container Communication** | Allows containers to talk to each other using names instead of IPs |
| **Isolation** | Containers on different networks cannot access each other |
| **Security** | Provides better control and privacy |
| **Flexibility** | You can connect or disconnect containers from networks anytime |

## **7\. Conclusion & Key Takeaways**

* Docker networks connect containers just like a local network.
    
* Containers in the same network can communicate easily.
    
* Use custom networks for better **security**, **organization**, and **flexibility**.
    
* You can inspect, manage, and monitor these networks anytime using Docker commands.
    

### **Final Note**

Now you’ve learned how to connect multiple containers using Docker networks — a must-know concept for any DevOps engineer!  
Keep experimenting with other network types like **host**, **overlay**, and **macvlan** to level up your Docker skills.