---
title: "Virtual Private Cloud (VPC)- (04)  - Day 20"
datePublished: Sat Oct 26 2024 08:01:12 GMT+0000 (Coordinated Universal Time)
cuid: cm2pvglt6000009mk64dz4ihw
slug: virtual-private-cloud-vpc-04-day-20
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1760872992867/f956788d-1b72-4d83-ac96-79f95452d22f.jpeg
tags: aws, google, devops, aws-vpc, 90daysofdevops, trainwithshubham

---

Sure! I can help you **modify and structure your content** to make it cleaner, more readable, and professional—like a polished blog or article. Here’s a revised version:

---

# **Table of Contents**

1. [What is Amazon VPC?](https://chatgpt.com/c/68f4b799-b594-8322-aee5-91fe2bd283ea#what-is-amazon-vpc)
    
2. [Types of Amazon VPC](https://chatgpt.com/c/68f4b799-b594-8322-aee5-91fe2bd283ea#types-of-amazon-vpc)
    
    * Default VPC
        
    * Custom VPC
        
    * VPC Peering
        
    * Transit Gateway
        
3. [When to Use Each Type](https://chatgpt.com/c/68f4b799-b594-8322-aee5-91fe2bd283ea#when-to-use-each-type)
    
4. [Subnet and Its Utility](https://chatgpt.com/c/68f4b799-b594-8322-aee5-91fe2bd283ea#subnet-and-its-utility)
    
    * Resource Isolation
        
    * Availability Zones (AZs)
        
    * Network Segmentation
        
5. [What is a Route Table?](https://chatgpt.com/c/68f4b799-b594-8322-aee5-91fe2bd283ea#what-is-route-table)
    
    * Routing Decisions
        
    * Custom Routing
        
    * Network Segmentation
        
6. [What is an Internet Gateway?](https://chatgpt.com/c/68f4b799-b594-8322-aee5-91fe2bd283ea#what-is-internet-gateway)
    
    * Internet Connectivity
        
    * Egress and Ingress
        
    * Default for Public Subnets
        

---

## **What is Amazon VPC?**

Amazon Virtual Private Cloud (VPC) is a logically isolated section of the AWS Cloud where you can launch AWS resources, such as EC2 instances and RDS databases, within a defined network.

VPC provides **complete control over IP address ranges, subnets, route tables, and network security**, enabling you to build a secure and private environment in the AWS cloud.

---

## **Types of Amazon VPC**

### **1\. Default VPC**

When you create a new AWS account, a default VPC is automatically created in each region. It comes with preconfigured resources like a main route table, security group, and network ACL.

While convenient, default VPCs may not be the most secure, as they allow certain traffic by default. However, you can modify the configuration to match your security requirements.

### **2\. Custom VPC**

A custom VPC is fully created and configured by you. You control the IP ranges, subnet structure, and routing rules, making it ideal for projects or organizations that require a **custom network environment** aligned with security and application needs.

### **3\. VPC Peering**

VPC peering enables **private communication between two VPCs**, whether in the same or different regions. It is useful when resources in separate VPCs need to communicate while remaining isolated.

### **4\. Transit Gateway**

For complex networking, **Transit Gateway** connects multiple VPCs, on-premises data centers, and remote offices in a hub-and-spoke model. It simplifies network management and ensures secure, low-latency connections for large-scale enterprises.

---

## **When to Use Each Type**

* **Default VPC:** Best for beginners or quick setups. Ensure to review security settings.
    
* **Custom VPC:** Ideal for projects with unique network requirements.
    
* **VPC Peering:** Use when resources in multiple VPCs need secure communication.
    
* **Transit Gateway:** Recommended for large organizations with multiple VPCs and on-premises networks.
    

---

## **Subnet and Its Utility**

A **subnet** is a logical partition of a VPC’s IP address range. It helps organize and isolate resources.

### **1\. Resource Isolation**

Place different resources in separate subnets (e.g., web servers in one, database servers in another) to enhance security.

### **2\. Availability Zones (AZs)**

Subnets are tied to specific AZs. This allows high availability, as resources can be distributed across multiple AZs.

### **3\. Network Segmentation**

You can apply different security rules to subnets using **security groups** and **network ACLs**, controlling traffic flow and access.

---

## **What is a Route Table?**

A **route table** defines where network traffic is directed within your VPC. Each subnet is associated with a route table.

### **1\. Routing Decisions**

Route tables specify destinations for network traffic. Example: directing internet-bound traffic through an Internet Gateway.

### **2\. Custom Routing**

Create custom route tables for specific routing rules to control traffic flow within your VPC.

### **3\. Network Segmentation**

By controlling subnet communication with route tables, you enhance security and prevent unauthorized access.

---

## **What is an Internet Gateway?**

An **Internet Gateway (IGW)** enables communication between your VPC and the public internet.

### **1\. Internet Connectivity**

The IGW serves as the entry and exit point for internet-bound traffic.

### **2\. Egress and Ingress**

All outgoing traffic passes through the IGW, and incoming traffic first arrives at the IGW.

### **3\. Default for Public Subnets**

Public subnets attach to the IGW to allow resources like web servers to communicate with the internet.