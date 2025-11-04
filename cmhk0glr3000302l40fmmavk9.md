---
title: "Exploring Kubernetes Architecture"
datePublished: Tue Nov 04 2025 03:30:08 GMT+0000 (Coordinated Universal Time)
cuid: cmhk0glr3000302l40fmmavk9
slug: exploring-kubernetes-architecture
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1762007797820/158fd02c-dae9-4bb0-8152-54a50f6d0d1e.png
tags: kubernetes, k8s, 90daysofdevops, trainwithshubham

---

# **Table of contents**

* [**Kubernetes Overview**](https://namg.hashnode.dev/kubernetes-architecture-day30#heading-kubernetes-overview)
    

### Kubernetes Overview

**What is Kubernetes?**

**<mark>Kubernetes</mark>** also known as **<mark>K8s</mark>** was built by Google based on their experience running containers in production. It is now an open-source project and is arguably one of the best and most popular container orchestration technologies out there.

To understand Kubernetes, we must first understand two things – **<mark>Container and Orchestration</mark>**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692708223123/612a17f8-fbda-4f44-b9f1-1b60e95f0968.png?auto=compress,format&format=webp&auto=compress,format&format=webp&auto=compress,format&format=webp align="left")

**<mark>Containers:</mark>**  
Containers are completely isolated environments, as in they can have their own processes or services, their own network interfaces, and their own mounts, just like Virtual machines, except that they all share the same OS kernel.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692707549494/534f9d28-936e-4e9b-b83f-9eceff2aaaca.png?auto=compress,format&format=webp&auto=compress,format&format=webp&auto=compress,format&format=webp align="left")

**<mark><br>Orchestration:</mark>** It is a technique that refers to the automated management and deployment of containerized applications across a clustered environment.

**<mark>Kubernetes:</mark>**

Kubernetes is known for its powerful orchestration capabilities, which enable the efficient operation of containerized workloads in a distributed environment.

**<mark>What benefits of using k8s:</mark>**

1. Automated Container deployments with scaling, load balancing, High Availability capabilities
    
2. Automated Application Deployments
    
3. Service discovery and load balancing
    
4. Automated rollouts and rollbacks
    
5. Self-healing
    
6. Secret and configuration management
    

\-Application is highly available as hardware failures do not bring the application down because you have multiple instances of your application running on different nodes.

\-The user traffic is load balanced across the various containers. When demand increases, deploy more instances of the application seamlessly and within a matter of seconds.

\-When we run out of hardware resources, scale the number of nodes up/down without having to take down the application.

**<mark>Explain the architecture of Kubernetes:</mark>**

![](https://miro.medium.com/v2/resize:fit:875/1*6uhBDhkNaniQoubPN51yUA.jpeg align="left")

When you install Kubernetes on a System, you are installing the following components.  
`An API Server`  
`An ETCD service`  
`A kubelet service`  
`A Container Runtime, Controllers and Schedulers`.

<table><tbody><tr><td colspan="1" rowspan="1"><p>Node</p></td><td colspan="1" rowspan="1"><p>A node is a machine – physical or virtual – on which Kubernetes is installed.</p></td></tr><tr><td colspan="1" rowspan="1"><p>Cluster</p></td><td colspan="1" rowspan="1"><p>A cluster is a set of nodes grouped together.</p></td></tr><tr><td colspan="1" rowspan="1"><p>Pods</p></td><td colspan="1" rowspan="1"><p>A POD is a single instance of an application. A POD is the smallest object, that you can create in Kubernetes.</p></td></tr><tr><td colspan="1" rowspan="1"><p>Master</p></td><td colspan="1" rowspan="1"><p>The master node manages the entire cluster through the API server and is responsible for assigning tasks to worker nodes.</p></td></tr><tr><td colspan="1" rowspan="1"><p>API Server</p></td><td colspan="1" rowspan="1"><p>The API server acts as the front end for kubernetes. The users, management devices and Command line interfaces all talk to the API server to interact with the Kubernetes cluster.</p></td></tr><tr><td colspan="1" rowspan="1"><p>Scheduler</p></td><td colspan="1" rowspan="1"><p>The scheduler is responsible for distributing work or containers across multiple nodes. It looks for newly created containers and assigns them to Nodes.</p></td></tr><tr><td colspan="1" rowspan="1"><p>Controller</p></td><td colspan="1" rowspan="1"><p>The controllers are the brain behind orchestration. They are responsible for noticing and responding when nodes, containers or endpoints goes down. The controllers make decisions to bring up new containers in such cases.</p></td></tr><tr><td colspan="1" rowspan="1"><p>etcd</p></td><td colspan="1" rowspan="1"><p>ETCD is a distributed reliable key-value store used by Kubernetes to store all data used to manage the cluster. This component stores a dictionary of data. It is a Kubernetes database that stores all cluster data.</p></td></tr><tr><td colspan="1" rowspan="1"><p>Container-runtime</p></td><td colspan="1" rowspan="1"><p>The container runtime is the underlying software that is used to run containers. In our case it happens to be Docker.</p></td></tr><tr><td colspan="1" rowspan="1"><p>kubelet</p></td><td colspan="1" rowspan="1"><p>kubelet is the agent that runs on each node in the cluster. The agent is responsible for making sure that the containers are running on the nodes as expected.</p></td></tr><tr><td colspan="1" rowspan="1"><p>Kube-proxy</p></td><td colspan="1" rowspan="1"><p>This component manages the network and the ports exposed outside the cluster for users.</p></td></tr><tr><td colspan="1" rowspan="1"><p></p></td><td colspan="1" rowspan="1"><p></p></td></tr></tbody></table>

**<mark>What is Control Plane?</mark>**

The control plane manages the worker nodes and the Pods in the cluster.

**<mark>Write the difference between kubectl and kubelets.</mark>**

`kubectl` is the command-line interface (CLI) tool for working with a Kubernetes cluster.

`Kubelet` is the technology that applies, creates, updates, and destroys containers on a Kubernetes node.

**<mark>Explain the role of the API server.</mark>**

The API server acts as the front-end for Kubernetes. The users, management devices and Command line interfaces all talk to the API server to interact with the Kubernetes cluster.

> ***"Thank you for reading my blog! Happy Learning!!!😊***