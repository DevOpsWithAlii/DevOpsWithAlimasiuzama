---
title: "What Are Terraform Blocks And Terraform State Management  ?"
datePublished: Sun Nov 23 2025 07:59:58 GMT+0000 (Coordinated Universal Time)
cuid: cmibfgsta000402ky42ni4mbt
slug: what-are-terraform-blocks-and-terraform-state-management
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1763884723463/82d259b9-0958-4be9-9d2a-ad5afd8cc845.png
tags: terraform, terraform-variables

---

Terraform is built around **blocks**.  
A block is like a “section” in your configuration where you tell Terraform what to do.

There are four important blocks you'll use daily:

**1.Terraform Block**  
2.**Variable Block**  
3.**Data Block**  
4.**Output Block**

Let’s understand all of them in a beginner-friendly way.

# **Terraform Block**

This block tells Terraform **which providers** you want to use and **what version** you want.

Think of it like setting up the *environment* before doing anything.

### ✔️ Example:

```plaintext
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }

  required_version = ">= 1.5"
}
```

### ✔️ Simple Explanation:

* You’re telling Terraform:  
    “I want to use the AWS provider of version 5.x.”
    
* And:  
    “My Terraform version must be at least 1.5.”
    

## Terraform Configuration Blocks

Terraform uses different blocks in `.tf` files:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1763883258990/bc7ecc0a-122c-46a7-abeb-d9fdd1ba6308.png align="center")

| Block Name | Purpose (Short Explanation) |
| --- | --- |
| **terraform** | Configures Terraform settings like required providers and versions. |
| **provider** | Tells Terraform which cloud/platform to connect with (AWS, Azure, GCP, GitHub, etc.). |
| **resource** | Creates actual infrastructure (EC2, S3, VPC, IAM, etc.). |
| **variable** | Stores input values to make configuration reusable and dynamic. |
| **output** | Prints important values after deployment (IP addresses, IDs). |
| **module** | Calls reusable sets of Terraform code. |
| **data** | Reads existing data/resources from the cloud (without creating anything). |
| **locals** | Stores local values/expressions inside the config (like small helper variables). |

# 1\. Variable Block — Simple Explanation + Examples

Variables in Terraform help you **avoid hardcoding values**.  
They make your configuration **clean**, **reusable**, and **easy to update**.

You define variables in a separate file like:

```plaintext
variables.tf
```

And you use them in your main Terraform code:

```plaintext
terraform.tf
```

## Example 1:

### [variables.tf](http://variables.tf)

```plaintext
variable "instance_type" {
  type        = string
  description = "EC2 instance type"
  default     = "t2.micro"
}
```

### [terraform.tf](http://terraform.tf)

```plaintext
resource "aws_instance" "web" {
  instance_type = var.instance_type
}
```

## Example 2:

### [variables.tf](http://variables.tf)

```plaintext
variable "instance_count" {
  type    = number
  default = 2
}
```

### [terraform.tf](http://terraform.tf)

```plaintext
resource "aws_instance" "web" {
  count = var.instance_count
}
```

## Example 3:

### [variables.tf](http://variables.tf)

```plaintext
variable "availability_zones" {
  type    = list(string)
  default = ["ap-south-1a", "ap-south-1b"]
}
```

### [terraform.tf](http://terraform.tf)

```plaintext
resource "aws_subnet" "subnets" {
  for_each          = toset(var.availability_zones)
  availability_zone = each.value
}
```

When you run:

```plaintext
terraform validate
terraform plan
terraform apply
```

# 2\. Data Block — Simple Explanation + Examples

A **data block** is used to **read** or **fetch** existing information from your cloud provider.

It **does NOT create** anything.  
It **only reads** something that already exists.

Examples of what you can fetch:

* Existing VPC
    
* Existing AMI ID
    
* Existing Security Group
    
* Existing Subnet
    
* Existing IAM Role  
    …basically anything already present in AWS.
    

## Example 1 — Fetch Latest Amazon Linux AMI

You don’t want to hard-code AMI IDs.  
So you use a **data block** to get the latest AMI automatically.

### **data.tf**

```plaintext
data "aws_ami" "amazon_linux" {
  most_recent = true

  owners = ["amazon"]

  filter {
    name   = "name"
    values = ["amzn2-ami-hvm-*-x86_64-gp2"]
  }
}
```

### main.tf

```plaintext
resource "aws_instance" "server" {
  ami           = data.aws_ami.amazon_linux.id
  instance_type = "t2.micro"
}
```

Terraform will read the latest AMI → use it in EC2.  
You don’t need to update AMI manually.

## Example 2 — Fetch Existing VPC

You already created a VPC manually or with other tools.

### data.tf

```plaintext
data "aws_vpc" "main" {
  filter {
    name   = "tag:Name"
    values = ["my-vpc"]
  }
}
```

### main.tf

```plaintext
resource "aws_subnet" "subnet1" {
  vpc_id     = data.aws_vpc.main.id
  cidr_block = "10.0.1.0/24"
}
```

Terraform **reads** the VPC ID  
Then uses it in new resources

## Example 3 — Fetch Existing Security Group

```plaintext
data "aws_security_group" "sg" {
  name = "default"
}
```

main.tf

```plaintext
resource "aws_instance" "app" {
  ami           = "ami-123456"
  instance_type = "t2.micro"
  vpc_security_group_ids = [data.aws_security_group.sg.id]
}
```

## Why Do We Use Data Blocks?

To avoid hard-coding things  
To fetch existing resources  
To read values we didn't create in this Terraform project  
To maintain real-time correct values  
To connect new infrastructure with old/existing infra

# 3\. Output Block?

Think of **output** as a way to *print important information* after Terraform finishes creating resources.

Example:  
You create an EC2 instance → Terraform will give you the **instance ID**, **public IP**, etc., only if you define them as output.

## Simple Example: 1

### **project structure**

```plaintext
main.tf
outputs.tf
```

## [**main.tf**](http://main.tf)

Let’s say you create an AWS EC2 instance:

```plaintext
resource "aws_instance" "myserver" {
  ami           = "ami-123456789"
  instance_type = "t2.micro"
}
```

## [**outputs.tf**](http://outputs.tf)

Now you want Terraform to show the EC2 **public IP** after creation:

```plaintext
output "server_public_ip" {
  value = aws_instance.myserver.public_ip
}
```

### What happens after “terraform apply”?

You will see:

```plaintext
server_public_ip = "18.212.55.101"
```

So you don’t need to search manually. Terraform prints it for you!

## Example: 2

### Get instance ID:

```plaintext
output "instance_id" {
  value = aws_instance.myserver.id
}
```

### Get instance type:

```plaintext
output "instance_type" {
  value = aws_instance.myserver.instance_type
}
```

## Why output block is useful?

Shows important information after deployment  
Helps in debugging  
Can be reused by other Terraform configurations  
Useful in automation (CI/CD)

# **2\. What Is Terraform State Management?**

Terraform needs a way to **remember** what it created.  
So it keeps a memory file called **terraform.tfstate**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1763888527730/aa477ca7-a706-4c6f-814b-2726f2d8e702.jpeg align="center")

### Think of it like this:

**Terraform state = Terraform’s memory**

Without this memory, Terraform won’t know:

* * What resources it created
        
        * IDs
            
        * IP addresses
            
        * Configuration values
            
        * Current status
            

So… state management means **managing Terraform’s memory properly**.

# What Is State Management?

**State Management** means:

How Terraform stores the state  
How Terraform updates the state  
How we keep the state safe, correct, and shared

It simply means:  
**Taking care of terraform.tfstate — because it is the heart of Terraform.**

# **Why Is State Important?**

Imagine you tell Terraform:

➡️ “Create one EC2 instance.”

Terraform creates it and saves details in **state file**:

```plaintext
aws_instance.myserver
ID: i-12345
type: t2.micro
public_ip: 3.89.12.22
```

Next time you run:

```plaintext
terraform apply
```

Terraform checks the **state file** and decides:

* Should it create?
    
* Should it update?
    
* Should it delete?
    
* Or do nothing?
    

Without the state file, Terraform becomes **BLIND** 👀❌

## **Real-Life Example**

### Example: Creating EC2

1. You run:
    

```plaintext
terraform apply
```

2. Terraform creates an EC2 and stores details in **terraform.tfstate**
    
3. If you manually delete EC2 from AWS console  
    Terraform still thinks the EC2 exists (because of state)
    

So your next run shows:

❌ Drift detected (difference between real infra vs state)

This is why state matters.

## **Where Is State Stored?**

By default:

📄 Local file:

```plaintext
terraform.tfstate
```

In real companies:

☁️ Remote backend (S3 bucket + DynamoDB lock)

# **Basic Commands Should Know**

| Command | Meaning |
| --- | --- |
| `terraform show` | Show whole state info |
| `terraform state list` | Show all resources in state |
| `terraform state show <resource>` | Show one resource info |
| `terraform state rm <resource>` | Remove something from state (without deleting in AWS) |
| `terraform state mv` | Rename or move resources |

## **Why Do Companies Use Remote State?**

Because when multiple people work on the same project:

* Local state causes conflicts
    
* Two people may overwrite each other’s infra  
    Solution:  
    S3 + DynamoDB = safe, locked state.
    

## ⭐ Here are **5 most important Terraform terminologies** explained in a very simple, fresher-friendly way with examples.

| Terminology | Meaning | Example |
| --- | --- | --- |
| **Resource** | Creates something | EC2 instance |
| **Provider** | Platform plugin | AWS |
| **Variable** | Input values | instance\_type |
| **State** | Terraform’s memory | terraform.tfstate |
| **Data block** | Read existing info | AWS AMI lookup |
| **Output** | Print values | public IP |
| **Module** | Reusable code | EC2 module |