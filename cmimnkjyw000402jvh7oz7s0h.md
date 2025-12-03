---
title: "My Kubernetes Journey — Day 1"
datePublished: Mon Dec 01 2025 04:32:18 GMT+0000 (Coordinated Universal Time)
cuid: cmimnkjyw000402jvh7oz7s0h
slug: my-kubernetes-journey-day-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1764561470899/4a8dd8e6-532c-477c-9ce2-02b020acf53a.jpeg
tags: kubernetes, k8s, 90daysofdevops, pods, k8s-commands

---

### Understanding Kubernetes + Full Architecture + Hands-On Setup (Kind, kubectl, Pods)

Today I officially started my journey into Kubernetes (K8s). I installed the tools, created my first cluster using **Kind**, interacted with it using **kubectl**, and understood the complete Kubernetes architecture from the Control Plane to Worker Nodes.

This blog is my Day-1 learning summary — written simply so any beginner can understand.

# **What is Kubernetes? (K8s explained simply)**

Kubernetes is an **open-source container orchestration platform** used to run, scale, and manage containerized applications automatically.

### 🔥 Why companies use Kubernetes?

* Auto-scaling
    
* Self-healing (restart failed containers)
    
* Zero-downtime deployments
    
* Load balancing built-in
    
* Best for microservices
    
* Works on any cloud (AWS, GCP, Azure)
    

In short: **Docker runs containers. Kubernetes runs containers at scale.**

# Why Do We Need Kubernetes?

Containers are great for packaging applications — but in **production**, you need something to **manage** those containers:

* What if a container crashes?
    
* What if traffic increases suddenly?
    
* What if you want zero-downtime deployments?
    

Managing all this manually is impossible.  
**That’s where Kubernetes comes in.**

Kubernetes gives you a **powerful framework** to run applications across multiple machines with **auto-scaling, self-healing, load balancing, and safe deployments**.

# What Kubernetes Can Do ?

### 1\. **Service Discovery & Load Balancing**

Kubernetes can expose containers using:

* DNS names
    
* Stable IPs
    

If traffic increases, it automatically load-balances across Pods.

### 2\. **Storage Orchestration**

You can automatically attach storage from:

* Local server
    
* AWS/GCP/Azure
    
* NFS, Ceph, etc.
    

No manual setup required.

### 3\. **Automated Rollouts & Rollbacks**

You define the **desired state** (e.g., 3 pods running version 2).  
Kubernetes automatically:

* Creates new pods
    
* Removes old pods
    
* Rolls back if something fails
    

Zero downtime deployments.

### 4\. **Automatic Bin Packing**

You tell Kubernetes how much CPU/RAM a container needs.  
It will **smartly place pods** on nodes to maximize resource usage.

### 5\. **Self-Healing**

Kubernetes automatically:

* Restarts failed containers
    
* Replaces unhealthy pods
    
* Waits until containers are ready (health checks)
    
* Removes broken pods from load balancers
    

Your app stays up even if containers crash.

### 6\. **Secrets & Configuration Management**

You can store sensitive data like:

* passwords
    
* tokens
    
* SSH keys
    

Without rebuilding your container images.

### 7\. **Batch & CI Jobs**

Kubernetes can run:

* Scheduled jobs
    
* One-time jobs
    
* CI workloads  
    and restart them if they fail.
    

### 8\. **Horizontal Scaling**

Scale up or down:

* with a command
    
* with an auto-scaler
    
* based on CPU/memory usage
    

### 9\. **Dual-stack IPv4/IPv6 Support**

Pods and Services can use both IPv4 and IPv6.

### 10\. **Designed for Extensibility**

You can add new features (operators, CRDs) without modifying Kubernetes itself.

# What Kubernetes Is **NOT ?**

Many beginners think Kubernetes is a full PaaS platform — it’s not.  
Kubernetes gives **building blocks**, not a complete platform.

Here’s what Kubernetes does **NOT** do:

### ❌ Kubernetes does *not* limit what apps you run

Stateless, stateful, big data — anything that runs in a container works.

### ❌ Kubernetes does *not* build your application

It does not:

* deploy source code
    
* run CI/CD pipelines
    

You use tools like GitHub Actions, Jenkins, ArgoCD for that.

### ❌ Kubernetes is not a database, cache, or storage system

It doesn’t include:

* MySQL
    
* Redis
    
* RabbitMQ
    
* Spark
    
* Ceph
    

But you can **run** these on Kubernetes.

# **Why Kubernetes Exists — The Problem It Solves**

Before Kubernetes:

* Servers were configured manually
    
* Scaling required adding new servers by hand
    
* Deploying new versions caused downtime
    
* If a container crashed → website down
    

With Kubernetes:

* Pods restart automatically
    
* Apps scale automatically
    
* Traffic is load balanced
    
* Deployments happen with zero downtime
    
* Entire infra is declared in YAML (IaC)
    

This is why DevOps engineers must know Kubernetes.

# **Kubernetes Architecture (COMPLETE Explained)**

Here is the simplest explanation of all components inside a Kubernetes cluster — from API → Pods.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1764560443811/390735c7-468b-48ad-98b0-ce6a652ecbfc.png align="center")

# **CONTROL PLANE (Master Node)**

The **Control Plane** is the *brain* of Kubernetes.  
It makes all high-level decisions for the entire cluster — like scheduling pods, monitoring health, and ensuring the cluster always matches the **desired state**.

When something changes (like a pod crashes), the control plane reacts and fixes it automatically.

## What the Control Plane Does ?

* Makes **global decisions** about the cluster  
    Example: Which node should run a new Pod?
    
* **Detects problems** and reacts  
    Example: If a Deployment needs 3 replicas but only 2 pods are running → create 1 more
    
* Continuously works to match **desired state → actual state**
    

This is the “intelligence layer” of Kubernetes.

## There are 5 main components:

## **1\. API Server (**`kube-apiserver`) - The Gateway of Kubernetes

The **API Server** is the front door of Kubernetes.  
Every command (`kubectl`) and every internal request hits the API server first.

* * Exposes the **Kubernetes API**, which is how every tool, script, or component talks to Kubernetes.
        
        * Handles **authentication**, **authorization**, and **admission control**.
            
        * Validates every YAML file before storing it in etcd.
            
        * Converts your YAML into an internal Kubernetes JSON structure.
            
        
        ### **Why it exists**
        
        Kubernetes is a fully API-driven system.  
        Nothing happens without passing through the API Server.
        
        ### **How it works internally**
        
        * Multiple API servers can run behind a load balancer → **horizontal scaling**.
            
        * When you run:
            
        
        ```bash
        kubectl apply -f deploy.yaml
        ```
        
        this happens:
        
        1. kubectl sends REST request → API server
            
        2. API server validates your YAML
            
        3. API server stores the desired state into etcd
            
        4. Controllers & Scheduler react to this new state
            
        
        ### **One-liner**
        
        **The API Server is the communicator, validator, and entry point of the Kubernetes brain.**
        

Think of it as: **the receptionist + traffic manager of Kubernetes.**

## **2\. ETCD -** The Database / Brain Memory

etcd is the **database** of Kubernetes.

* Stores all cluster data (pods, nodes, configs, secrets, etc.)
    
* Highly available, consistent key-value store
    
* Must be backed up — losing etcd = losing cluster state
    

Simple summary: **If Kubernetes was a brain, etcd is its memory.**

## **3\. Kube Scheduler -** Pod Placement Intelligence

* Watches for **new pods without assigned nodes**, and decides where to place them.
    
    ### **How it makes decisions**
    
    It considers dozens of factors:
    
    * Node CPU & memory availability
        
    * Pod resource requests/limits
        
    * Pod affinity rules
        
    * Node labels & taints
        
    * Pod tolerations
        
    * Storage/data locality
        
    * Custom scheduling plugins
        
    
    ### **Internal scheduling process**
    
    1. Pod is created but unassigned
        
    2. Scheduler filters nodes → picks all nodes *capable*
        
    3. Ranks nodes → scores them based on rules
        
    4. Assigns Pod → API server updates etcd
        

Think of it like assigning tasks to the best-fit employee.

## **4\. Kube Controller Manager -** The Autopilot System

Kubernetes has many “controllers”, each watching the cluster and fixing issues.

Examples of internal controllers:

### **Node Controller**

* Detects when nodes go offline
    
* Marks nodes as “NotReady”
    
* Evicts pods from dead nodes
    

### **Deployment Controller**

* Ensures the right number of pod replicas exist
    
* Creates new ReplicaSets on rollout
    
* Deletes old ones
    

### **Job Controller**

* Runs one-time tasks
    
* Ensures completion even after failures
    

### **EndpointSlice Controller**

* Maintains the mapping between **Services → Pods**
    

### **How controllers work internally**

* They constantly compare:  
    **Desired state** (from etcd)  
    **Actual state** (from nodes)
    
* If mismatch → take action
    

### **Why they exist**

Kubernetes is *self-healing* only because controllers run continuously.

### **One-liner**

**Controllers watch everything and fix problems automatically.**

## **5\. cloud-controller-manager — Cloud Integration Brain**

Used only in cloud platforms (AWS, Azure, GCP).

### **Why it exists**

Controls resources *outside* the Kubernetes cluster:

* Cloud load balancers
    
* Cloud routes
    
* Cloud node lifecycle
    

### **Example**

When you create a Service of type LoadBalancer:

* Cloud controller talks to AWS/GCP API
    
* Creates an external LB
    
* Connects LB → NodePorts
    
* Updates Kubernetes API with LB IP
    

### **One-liner**

**This component lets Kubernetes talk to your cloud provider.**

# **WORKER NODES (Where Apps Run)**

Worker nodes run all application containers (Pods).

### There are 3 essential node components:

## **1\. Kubelet -** The Node’s Local Manager

* **What it does ?**
    
    * Talks to API server
        
    * Watches for assigned pods
        
    * Starts containers via runtime
        
    * Performs health checks
        
    * Restarts failed containers
        
    * Reports back node & pod status
        
    
    ### **Important**
    
    Kubelet **only manages containers Kubernetes created**.
    
    ### **One-liner**
    
    \*\*kubelet ensures containers run exactly as Kubernetes wants.\*\*ime
    

## **2\. Kube-proxy -** Kubernetes Networking Engine

### **What it does ?**

* Implements Service networking
    
* Maintains IP tables / IPVS rules
    
* Routes external/internal traffic to Pods
    
* Handles load balancing inside the cluster
    

### **Modern cluster note**

Some CNI plugins (Cilium, Calico eBPF) can replace kube-proxy entirely by providing faster datapaths.

### **One-liner**

**kube-proxy makes sure traffic reaches the correct Pod.**

## **3\. Container Runtime -** The Engine That Runs Containers

A container runtime is **<mark>the software that runs containers, handling everything from pulling images and managing their lifecycle (starting, stopping, restarting) to isolating resources like CPU and memory</mark>**.

# **What is a Pod in Kubernetes?**

A **Pod is the smallest deployable unit in Kubernetes.**  
It is **NOT** a container — instead, a Pod is a *wrapper* around one or more containers.

Think of a Pod as:

> **A small environment where your container(s) live, share the same network, and work together.**

# Why Pods Exist (Why not run containers directly?)

Kubernetes could have scheduled containers directly, but Pods solve three problems:

**1\. Shared networking**

All containers inside a Pod share:

* one IP address
    
* one network namespace
    
* same ports
    

This makes multi-container applications easy.

### **2\. Shared storage**

Containers can share volumes inside the Pod.

Example:

* Container A downloads files
    
* Container B processes them  
    Both use the same volume.
    

### **3\. Sidecar pattern**

Many microservices need helper containers, such as:

* logging agent
    
* monitoring agent
    
* proxy (Istio, Envoy)
    
* file synchronization service
    

Pods allow these helpers to run *beside* the main app.

# Pod Structure (Deep Internal View)

Inside a Pod:

```bash
+-------------------------------------------------+
|                    Pod                          |
|   IP: 10.244.1.5                                |
|                                                 |
|   +---------------+   +----------------------+  |
|   |  Container 1  |   |   Container 2 (sidecar) |
|   |   App logic   |   |   Logging agent       |
|   +---------------+   +----------------------+  |
|       Shared network, storage, IPC               |
+-------------------------------------------------+
```

### A Pod contains:

* **1+ containers**
    
* **shared network namespace**
    
* **shared IP**
    
* **shared volumes**
    
* **PodSpec** (YAML definition)
    
* **init containers** (optional)
    
* **container lifecycle rules**
    

# How Pods Are Created Internally

When you run:

```bash
kubectl run nginx --image=nginx
```

This happens:

1. **kubectl → API Server**  
    It sends PodSpec request.
    
2. **API Server stores the PodSpec in etcd**
    
3. **Scheduler picks a node** for the Pod.
    
4. **Kubelet on that node pulls the image**  
    (via containerd/Docker)
    
5. **Kubelet starts containers inside the Pod**
    
6. Pod becomes **Running**
    

# Pod Lifecycle (Deep but easy)

A Pod has these phases:

1. **Pending** → created, waiting for images
    
2. **ContainerCreating** → runtime pulling images
    
3. **Running** → containers running
    
4. **Succeeded** → job finished successfully
    
5. **Failed** → job/pod failed
    
6. **CrashLoopBackOff** → container keeps crashing
    
7. **Evicted** → removed due to low resources
    
8. **Terminating** → being stopped manually
    

# What Happens If a Pod Dies?

**Pods do NOT heal themselves.**

If a Pod crashes:

* kubelet may restart containers **inside the Pod**
    
* but if the *Pod itself* disappears, Kubernetes does nothing  
    (because Pods are meant to be ephemeral)
    

That is why we use:

* **Deployments**
    
* **ReplicaSets**
    

These controllers recreate Pods automatically.

# Pod vs Container (Clear Comparison)

| Feature | Container | Pod |
| --- | --- | --- |
| Smallest unit in Docker | ✔ | ✖ |
| Smallest unit in Kubernetes | ✖ | ✔ |
| Has its own network | ✔ | Pod shares network for all containers |
| Can run alone | ✔ | No, pod is the wrapper |
| Supports sidecar pattern | ✖ | ✔ |
| Managed by K8s controllers | ✖ | ✔ |

# Types of Pods

### **1\. Single-container Pod**

Most common.  
One app container runs inside.

Example: Nginx server, backend app.

### **2\. Multi-container Pod**

Two or more containers working tightly together.

Common use cases:

* Sidecar container
    
* Logging agent
    
* Proxy container
    
* Helper scripts
    

### **3\. Static Pod**

Managed directly by kubelet, not by API server.

Used mainly for:

* Bootstrapping control plane components  
    (ETCD, API server, controller-manager run as static pods)
    

### **4\. Init Containers**

Special containers that:

* run **before** main containers
    
* ensure conditions are met
    
* always run sequentially
    

Example: Download config before app starts.

# How Pods Communicate

### **Inside the same Pod**

* Containers share [localhost](http://localhost)
    
* Communicate using [`localhost`](http://localhost)`:port`
    

### **Between Pods**

* Communicate using their Pod IP
    
* Or Service abstraction for stability
    

# YAML Example (Beginner-Friendly)

```bash
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
    - name: nginx
      image: nginx
```

This creates a simple Pod running Nginx.

Keeps a specific number of Pods running.

### **Service**

Provides stable networking for Pods

* ClusterIP
    
* NodePort
    
* LoadBalancer
    

### **Namespace**

Logical grouping (dev / stage / prod).

### **ConfigMap & Secret**

Store configuration values.

# **Working With Namespaces**

Check names

This was my first real workload inside a Kubernetes cluster.

# 🎯 **Day-1 Summary**

Today I learned:

* What is Kubernetes, why companies use it & Why Do We Need Kubernetes?
    
* Full Kubernetes Architecture (API → Pods)
    
* Installed Docker, Kind, kubectl
    

Kubernetes is huge but extremely interesting — excited for Day 2!