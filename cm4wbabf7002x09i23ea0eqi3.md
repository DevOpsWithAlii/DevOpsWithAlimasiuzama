---
title: "Terraform (IaaC)"
datePublished: Fri Dec 20 2024 05:30:14 GMT+0000 (Coordinated Universal Time)
cuid: cm4wbabf7002x09i23ea0eqi3
slug: terraform-iaac-01-day-51
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1760985691621/33c7aba8-b0c5-4056-bcb6-6c7093ff4b2e.png
tags: linux, aws, google, devops, hashnode, terraform, 90daysofdevops, tws

---

## **Table of contents**

* [What is Iaac (Infrastructure as a Code)?](https://namg.hashnode.dev/terraform-iaac#heading-what-is-iaac-infrastructure-as-a-code)
    
* [What is Terraform?](https://namg.hashnode.dev/terraform-iaac#heading-what-is-terraform)
    
* [Why do we use terraform?](https://namg.hashnode.dev/terraform-iaac#heading-why-do-we-use-terraform)
    
* [What is a Provider?](https://namg.hashnode.dev/terraform-iaac#heading-what-is-a-provider)
    
* [What is a Resource?](https://namg.hashnode.dev/terraform-iaac#heading-what-is-a-resource)
    
* [What is a State file in Terraform? What’s the importance of it?](https://namg.hashnode.dev/terraform-iaac#heading-what-is-a-state-file-in-terraform-whats-the-importance-of-it)
    
* [What is the Desired and Current State?](https://namg.hashnode.dev/terraform-iaac#heading-what-is-the-desired-and-current-state)
    
* [Task:](https://namg.hashnode.dev/terraform-iaac#heading-task)
    
    * [Technology Stacks on local ( pre-requisites)](https://namg.hashnode.dev/terraform-iaac#heading-technology-stacks-on-local-pre-requisites)
        
    * [Connect to AWS Cloud](https://namg.hashnode.dev/terraform-iaac#heading-connect-to-aws-cloud)
        
    * [Write Terraform code](https://namg.hashnode.dev/terraform-iaac#heading-write-terraform-code)
        

### What is Iaac (Infrastructure as a Code)?

**Infrastructure as Code (IaC)** is the concept of managing and provisioning infrastructure through **code files** rather than manual processes.

With IaC:

* You can deploy infrastructure **faster** and **error-free**.
    
* Your infrastructure setup becomes **repeatable** and **version-controlled**.
    
* Any changes can be tracked through **Git repositories**, ensuring full transparency.
    

In short, **IaC = Speed + Consistency + Control**.

### What is Terraform?

**Terraform** is an open-source **Infrastructure as Code tool** developed by **HashiCorp**.  
It allows you to **define, provision, and manage infrastructure** across multiple cloud providers using a simple declarative syntax known as **HCL (HashiCorp Configuration Language)**.

Terraform works with AWS, Azure, GCP, Oracle Cloud, and many others — all using the same unified workflow.

### Why do we use terraform?

Terraform **<mark>makes it easy to automate infrastructure provisioning on any cloud or data center</mark>**. It also tracks all changes in a statefile. The file can be committed to a version control system (VCS) and acts as a source of truth for the infrastructure. It thus provides more transparency and prevents unauthorized changes.

## Why Use Terraform?

Here’s why Terraform is so widely used in DevOps:

* **Multi-Cloud Support:** Manage AWS, Azure, and GCP from a single tool.
    
* **Automation:** Define once, deploy anywhere — no manual setup.
    
* **State Management:** Tracks infrastructure with a *state file* for consistency.
    
* **Version Control:** Store your Terraform code in GitHub or any VCS.
    
* **Collaboration:** Share infrastructure blueprints across teams easily.
    

Terraform simplifies large-scale cloud provisioning with reliability and repeatability.

### What is a Provider?

A **Provider** in Terraform is a plugin that allows Terraform to interact with an **external API** (like AWS or Azure).

Each provider knows how to:

* Authenticate to the platform
    
* Manage specific resources (e.g., EC2, VPC, S3)
    
* Translate Terraform code into real infrastructure actions
    

Provider: AWS

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694235720759/7b9834bb-b71d-42ef-bf5e-c35acf8e89c1.png?auto=compress,format&format=webp align="left")

### What is a Resource?

A **Resource** represents a component of your infrastructure — such as a **VPC**, **EC2 instance**, or **S3 bucket**.

I am creating VPC in AWS. So resource type is "aws\_vpc" and giving the resource name as "tws-vpc" as an identity.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694236044217/4bfb70e4-fd09-4790-ac91-40e23917846d.png?auto=compress,format&format=webp align="left")

Resources define the actual cloud entities that Terraform will create, update, or destroy.

### What is a State file in Terraform? What’s the importance of it?

The **Terraform State File (**`terraform.tfstate`) is like Terraform’s memory.

It stores:

* The **current configuration** of your infrastructure
    
* Resource metadata and properties
    
* Mappings between Terraform resources and real-world infrastructure
    

### Importance:

* Helps Terraform **compare current vs desired state**
    
* Ensures **idempotency** (no duplicate creation)
    
* Enables **team collaboration** using remote backends like S3
    

By default, it’s stored locally, but for production, it’s recommended to store it in a **remote backend** such as **AWS S3** with DynamoDB locking.

* Ex:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694236657802/976af591-5396-459a-b2f9-f445f9c6de0a.png?auto=compress,format&format=webp align="left")
    

### What is the Desired and Current State?

In Terraform, <mark>the desired state</mark> is the state that you want your infrastructure to be as defined in your Terraform Configuration files.

<mark>The current state</mark> is the actual state of the infrastructure as represented in the Terraform state file

# Task:

### Technology Stacks on local ( pre-requisites)

1. Install Visual Studio Code  
    <mark>Download vs code latest version from</mark>[**here**](https://code.visualstudio.com/download)<mark>and install it.</mark>
    
2. Install AWS CLI  
    <mark>Download AWSCLI latest version from</mark>[**here**](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)<mark>and install it.</mark>
    
    or you can run the below command in powershell or the command prompt
    
3. Install Terraform
    
    <mark>Download Terraform the latest version from</mark>[**here**](https://developer.hashicorp.com/terraform/downloads)
    
    Setup environment variable click on start --&gt; search "edit the environment variables" and click on it  
    Under the advanced tab, choose "Environment variables" --&gt; under the system variables select path variable and add terraform location in the path variable. system variables --&gt; select path add new --&gt; terraform\_Path
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694237333333/bc72c190-919b-4041-9810-eca7b2073309.png?auto=compress,format&format=webp align="left")
    
4. Run the below command to validate the Terraform version the output should be something like below
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694237517266/c9c273f7-221c-4f7a-a706-032a06c62132.png?auto=compress,format&format=webp align="left")
    

### Connect to AWS Cloud

You must have an AWS account to proceed with the below steps.

1. Create IAM user  
    Go to IAM &gt; create user and add "**<mark>AdministratorAccess</mark>**" policy
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1710611108304/623d5a48-11df-466e-8804-87b69fa8010b.png?auto=compress,format&format=webp align="left")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1710610985232/9b85ac52-57b4-4bd7-a949-251393725cc6.png?auto=compress,format&format=webp align="left")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1710611012023/13fddc11-ec59-4fad-8dbc-307f127bb547.png?auto=compress,format&format=webp align="left")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1710611043138/6bb8d568-2f62-472f-aea5-5351e062afb3.png?auto=compress,format&format=webp align="left")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1710611051925/d321ce0c-a4a7-4169-a116-14fb4937fc84.png?auto=compress,format&format=webp align="left")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1710611059995/a85593dc-3d19-45b6-bbc5-7601b6dd36fb.png?auto=compress,format&format=webp align="left")
    
2. Login to aws cli
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694237774095/3d842849-f51c-448d-931c-59f9af15879c.png?auto=compress,format&format=webp align="left")
    

### Write Terraform code

1. Write the First Terraform code
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694238064324/54b424c7-6904-4b10-b0be-4607d3067f6b.png?auto=compress,format&format=webp align="left")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694238086347/7b4d921c-f471-4d44-8df9-50dbb59d4d3b.png?auto=compress,format&format=webp align="left")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694238128316/5d2b6820-a185-4c0f-ba0e-99709700ef8f.png?auto=compress,format&format=webp align="left")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694238144995/da5cffe2-c753-4ca1-927e-5b7e44f13d29.png?auto=compress,format&format=webp align="left")
    
2. Run terraform command &gt; <mark>terrafrom init</mark> &gt; <mark>terraform validate</mark> &gt; <mark>terraform plan</mark>
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694174648634/5ed7b2a1-a122-48e8-9212-fb5944a27a06.png?auto=compress,format&format=webp align="left")
    
3. Before running the '<mark>terraform apply</mark>' command check the AWS console
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694174692210/2e1c1d7d-120f-4b74-b078-e89a9ce06d82.png?auto=compress,format&format=webp align="left")
    
4. Run <mark>terraform apply</mark>
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694238243435/6fb77a23-1998-4608-944c-b9f5fc5d1010.png?auto=compress,format&format=webp align="left")
    
5. EC2 instance created
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694175154563/301f7409-ad0a-476f-ae5f-ad2510c5f963.png?auto=compress,format&format=webp align="left")
    
6. To destroy created infrastructure run '<mark>terraform destroy</mark>'
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694238275566/e391864c-c58f-4533-ae4f-af5eefb91273.png?auto=compress,format&format=webp align="left")
    

***EC2 instance created, validated, applied and at last destroyed through Terraform code successfully.***

> ***"Thank you for reading my blog! Happy Learning!!!😊***