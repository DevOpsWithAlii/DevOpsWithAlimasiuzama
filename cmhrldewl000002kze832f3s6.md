---
title: "Docker Volumes Explained with MySQL Example"
datePublished: Sun Nov 09 2025 10:49:54 GMT+0000 (Coordinated Universal Time)
cuid: cmhrldewl000002kze832f3s6
slug: docker-volumes-explained-with-mysql-example
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1762684339707/95c6edbe-ca64-4605-a3d9-034cdd2bea3a.png
tags: docker, aws, google, devops, devops-articles, docker-volume, 90daysofdevops

---

If you’ve ever deleted a Docker container and lost your database data — don’t worry, it’s a common mistake!  
In this post, we’ll explore how **Docker Volumes** can help persist your data even after containers are removed.

Let’s go through a practical example using **MySQL on Docker**.

## **Table of Contents**

1. **Introduction to Docker Volumes**
    
2. **Step 1 — Create Directory for MySQL Volume**
    
3. **Step 2 — Run MySQL Container with Volume Mount**
    
4. **Step 3 — Access MySQL and Create Database**
    
5. **Step 4 — Insert Data into the Database**
    
6. **Step 5 — Delete Container and Test Data Persistence**
    
7. **Step 6 — Recreate Container and Verify Stored Data**
    
8. **Why Docker Volumes Are Important**
    

## What is a Docker Volume?

A **Docker Volume** is a storage mechanism that allows data to be stored **outside the container’s writable layer**, ensuring that data remains even after containers are deleted or recreated.

It’s the preferred way to handle **persistent data** in Docker — especially for databases like MySQL or PostgreSQL.

## Step 1: Create a Directory for the Volume

We’ll first create a local directory that will store MySQL data permanently.

```plaintext
mkdir mysql-volume
cd mysql-volume
pwd
```

The output shows your path (for example):

```plaintext
/home/ubuntu/mysql-volume
```

## Step 2: Run a MySQL Container with Volume Mount

Now, let’s run a MySQL container and mount our created directory using the `-v` flag.

```plaintext
docker run -d -v /home/ubuntu/mysql-volume:/var/lib/mysql --name mysql -e MYSQL_ROOT_PASSWORD=root
mysql:latest
```

### Explanation:

| Option | Description |
| --- | --- |
| `-d` | Run container in detached mode |
| `-v` | Mounts our local folder to the container path `/var/lib/mysql` |
| `--name` | Assigns a name to our container (`mysql`) |
| `-e MYSQL_ROOT_PASSWORD=root` | Sets the root password for MySQL |
| `mysql:latest` | Image used to run the container |

## Step 3: Access the MySQL Container

List running containers:

```plaintext
docker ps
```

Example output:

| CONTAINER ID | IMAGE | STATUS | NAMES |
| --- | --- | --- | --- |
| 2467d65eab12 | mysql:latest | Up 10 seconds | mysql |

Now enter into the container:

```plaintext
docker exec -it 2467d65eab12 mysql -u root -p
```

Enter the password (`root`), and you’ll enter the MySQL shell.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1762684544975/c116da15-3915-4671-9476-1622063ba815.png align="center")

## Step 4: Create Database and Table

Now let’s create a database and table inside MySQL.

```plaintext
show databases;

create database college;

use college;

create table students(
  id int,
  name varchar(50)
);

insert into students values(101, "Ankit");

select * from students;
```

### Output:

| id | name |
| --- | --- |
| 101 | Ankit |

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1762684665913/e8b5855f-7187-4c46-8ad7-4bf3fdf2a531.png align="center")

## Step 5: Delete the Container

Let’s now delete the container to test if our data is really persisted.

```plaintext
docker ps
docker stop 2be
docker rm 2be
docker ps
```

Output:

```plaintext
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

No containers are running — meaning the MySQL container is fully deleted.

## Step 6: Recreate the Container

Now, create a new MySQL container **with the same volume mount path**:

```plaintext
docker run -d -v /home/ubuntu/mysql-volume:/var/lib/mysql --name mysql -e MYSQL_ROOT_PASSWORD=root
mysql:latest
```

Then access MySQL again:

```plaintext
docker exec -it <new-container-id> mysql -u root -p
```

Now check your databases:

```plaintext
show databases;
use college;
select * from students;
```

### Output:

| id | name |
| --- | --- |
| 101 | Ankit |

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1762684866242/fe34d75d-a2cf-42b7-9481-e1798f46c3b2.png align="center")

**Your data is still there!**  
That’s the magic of Docker Volumes — even after deleting containers, data remains safe.

## Why Docker Volumes Are Important

| Benefit | Description |
| --- | --- |
| **Persistence** | Data survives container removal or crashes |
| **Portability** | Easy to back up or move to another host |
| **Performance** | Optimized for Docker engine’s file system |
| **Sharing** | Can be shared between multiple containers |

## Conclusion !!!

Using **Docker Volumes** is one of the most reliable ways to handle **persistent storage** in containerized environments.  
This simple MySQL demo proves that even if containers are deleted, your data remains safe.