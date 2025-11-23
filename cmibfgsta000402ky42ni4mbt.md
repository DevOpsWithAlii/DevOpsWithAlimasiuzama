---
title: "What Are Terraform Blocks? (With Simple Examples)"
datePublished: Sun Nov 23 2025 07:59:58 GMT+0000 (Coordinated Universal Time)
cuid: cmibfgsta000402ky42ni4mbt
slug: what-are-terraform-blocks-with-simple-examples
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

---

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

A **data block** is used to **fetch existing resources**.

Important:  
➡️ It *does not create* anything.  
➡️ It *reads* existing resources.

## **Example 1: Fetch Latest Ubuntu AMI**

### [**variable.tf**](http://variable.tf)

```plaintext
variable "ubuntu_owner" {
  description = "Canonical AWS Account ID"
  type        = string
  default     = "099720109477"
}

variable "ubuntu_name_filter" {
  description = "Filter for Ubuntu AMIs"
  type        = string
  default     = "ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"
}
```

### [**data.tf**](http://data.tf)

```plaintext
data "aws_ami" "ubuntu" {
  owners      = [var.ubuntu_owner]
  most_recent = true

  filter {
    name   = "name"
    values = [var.ubuntu_name_filter]
  }
}
```

### [**terraform.tf**](http://terraform.tf) **(main file)**

```plaintext
resource "aws_instance" "server" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t2.micro"
}

output "ubuntu_ami_id" {
  value = data.aws_ami.ubuntu.id
}
```

### Simple Explanation:

Terraform will look inside AWS and bring the **latest Ubuntu AMI ID**.  
Then you can use it like this:

```plaintext
resource "aws_instance" "server" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t2.micro"
}
```