---
title: "Terraform (IaaC) - S3 backend"
datePublished: Sat Dec 21 2024 05:30:51 GMT+0000 (Coordinated Universal Time)
cuid: cm4xqqz7i000l09mn62ow9llq
slug: terraform-iaac-s3-backend
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1760986314715/4dc8a31b-cdc1-4ef8-b233-20235ce89bb1.png
tags: devops, terraform, 90daysofdevops

---

## How to Use the S3 Backend with Locking in Terraform for Better Collaboration

When working on infrastructure in a team, managing the Terraform state file becomes crucial. Keeping this file locally can lead to conflicts or accidental overwrites when multiple people apply changes. That’s why Terraform supports using **remote backends** — and one of the most common is **AWS S3**.

Using an S3 backend along with **DynamoDB state locking** makes collaboration smoother and prevents conflicting changes. Let’s walk through how this setup works.

## **Table of contents**

* [Terraform Variables](https://namg.hashnode.dev/terraform-iaac-s3-backend#heading-terraform-variables)
    
* [Terraform output](https://namg.hashnode.dev/terraform-iaac-s3-backend#heading-terraform-output)
    
* [Terraform S3 backend](https://namg.hashnode.dev/terraform-iaac-s3-backend#heading-terraform-s3-backend)
    
* [Terraform state lock](https://namg.hashnode.dev/terraform-iaac-s3-backend#heading-terraform-state-lock)
    
* [Force Unlock:](https://namg.hashnode.dev/terraform-iaac-s3-backend#heading-force-unlock)
    
* [Task 1- Create S3 backend](https://namg.hashnode.dev/terraform-iaac-s3-backend#heading-task-1-create-s3-backend)
    
    * [Task 2- Store the terraform state file in the S3 backend](https://namg.hashnode.dev/terraform-iaac-s3-backend#heading-task-2-store-the-terraform-state-file-in-the-s3-backend)
        
    * [Conclusion:](https://namg.hashnode.dev/terraform-iaac-s3-backend#heading-conclusion)
        

### **Terraform Variables**

As a best practice, you should never hardcode values directly in your Terraform configuration. Terraform provides **variables** to help you store values in one place and reuse them across multiple files.

You can define variables in a [`variables.tf`](http://variables.tf) file. For example:

```plaintext
textvariable "region" {
  description = "Please provide region details"
  default     = "us-west-1"
  type        = string
}

variable "vpc_cidr" {
  default = "192.168.0.0/16"
}
```

To assign values to these variables, use `terraform.tfvars`:

```plaintext
textregion   = "us-west-1"
vpc_cidr = "192.168.0.0/16"
```

If you don’t provide values in `terraform.tfvars`, Terraform will prompt you for them during `apply`.

**Note:**  
Avoid placing sensitive values directly in the `.tfvars` file since they are visible in plaintext. You can also provide variables via the command line:

```plaintext
bashterraform apply -var="vpc_cidr=192.168.0.0/16"
```

You can explore supported da[ta t](https://developer.hashicorp.com/terraform/language/expressions/types)ypes [**here**](https://developer.hashicorp.com/terraform/language/expressions/types).

### **Terraform output**

Outputs make it easier to retrieve resource information after deployment. For example:

```plaintext
textoutput "vpc_name" {
  value = aws_vpc.vpc1.id
}
```

After running `terraform apply`, you can display outputs using:

```plaintext
bashterraform output
```

### **Terraform S3 backend**

Terraform backend is the concept of keeping the state file safe in the remote location rather than keeping it in the local system.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694325224604/959cdee8-0ed5-4d7c-bd34-7a23e4e74a15.png?auto=compress,format&format=webp align="left")

<mark>Follow the</mark> [**LINK**](https://developer.hashicorp.com/terraform/language/settings/backends/configuration) <mark>to learn more about available backends. If your infrastructure provider is AWS then S3 backend is the opted one to store the state file.&nbsp;</mark> 

* Stores the state as a given key in a given bucket on S3.
    
* This backend also supports state locking and consistency checking via Dynamo DB, which can be enabled by setting the dynamodb\_table field to an existing DynamoDB table name.
    
* A single DynamoDB table can be used to lock multiple remote state files. Terraform generates key names that include the values of the bucket and key variables.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694325463761/133f33f5-cd2a-42d9-a619-4e6a37522005.png?auto=compress,format&format=webp align="left")

* This setup:
    
    * Stores the Terraform state file in an S3 bucket.
        
    * Enables **state locking** and **consistency checks** with DynamoDB.
        
    * Ensures that only one person can modify infrastructure at a time.
        
    
    During `terraform apply`, Terraform:
    
    1. Checks if a lock exists in DynamoDB.
        
    2. Acquires the lock if available.
        
    3. Compares the current state with desired configurations.
        
    4. Deploys the plan if approved.
        
    5. Updates the state file in S3 and releases the lock.
        

### **Terraform state lock**

If your backend supports it, Terraform automatically **locks the state file** when performing changes. This prevents multiple users from updating resources simultaneously and avoids corruption.

If locking fails, Terraform will stop the operation. You can disable locking with the `-lock` flag (not recommended).

If locking takes too long, Terraform will notify you. Otherwise, the process continues silently in the background.

### **Force Unlock:**

**Be very careful with this command.**

If you unlock the state when someone else is holding the lock it could cause multiple writers. Force unlock should only be used to unlock your own lock in a situation where automatic unlocking fails.

To protect you, the force-unlock command requires a unique lock ID. Terraform will output this lock ID if unlocking fails. This lock ID acts as a nonce(a **nonce** is an arbitrary number that can be used just once in a cryptographic communication), ensuring that locks and unlocks target the correct lock.

## **Task 1- Create S3 backend**

To create S3 bucket through terraform code i have written below terraform files:

1. [**provider.tf**](http://provider.tf/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694327799646/6bf52c65-32e9-41b1-934c-e362deae2b8f.png?auto=compress,format&format=webp align="left")
    
2. [**variables.tf**](http://variables.tf/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694327857257/156fb866-8bd4-424c-acb9-729b9e700b25.png?auto=compress,format&format=webp align="left")
    
3. terraform.tfvars
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694327829038/4f802dae-3fa9-4f28-bae3-830a04694da0.png?auto=compress,format&format=webp align="left")
    
4. [**s3.tf**](http://s3.tf/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694327696769/cdf29e8d-b1bd-4e9d-8416-d0e60704b596.png?auto=compress,format&format=webp align="left")
    
5. [**dynamodb.tf**](http://dynamodb.tf/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694327767216/c33baefc-fd68-4ee4-87a7-be6bdd01a69a.png?auto=compress,format&format=webp align="left")
    
6. After writing the code lets create infrastructure <mark>terrafrom init</mark>
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694327123727/462ec32a-62d5-4e95-9e2c-b04cfaa22695.png?auto=compress,format&format=webp align="left")

1. <mark>terraform validate</mark>
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694327160963/2db3f28a-d174-4de1-b1dc-0ab79346e20e.png?auto=compress,format&format=webp align="left")

1. <mark>terraform plan</mark> : lock will create once I'll run 'terraform apply' <mark>'aws_dynamodb_table.terraform-lock will be created'</mark>
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694327230802/97abff37-a3de-401b-83a0-a0be32fcf215.png?auto=compress,format&format=webp align="left")

After running 'terrafrom apply' dynamo db, s3 bucket and lock on dynamo db created.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694327398987/21fab27d-5ff2-4b57-95a0-9f8475fb4301.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694327463530/36876233-aaea-4094-a9f2-26d45e44b714.png?auto=compress,format&format=webp align="left")

1. Verifying S3 bucket from AWS console after creation of infrastructure=-  
    namg-tws-terraform-state bucket created.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694319623738/00197820-d437-4a8b-9d41-ed3c639b6683.png?auto=compress,format&format=webp align="left")

1. Dynamo DB with 'tws\_table' created.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694319680588/55e1f39b-dc56-4f90-94fd-f3b0f13383ce.png?auto=compress,format&format=webp align="left")

**The purpose of above created S3 bucket and dynamo db is to store terraform state files separately instead of any version control system like Git/Bitbucket to protect the sensitive data from other users.**

### **Task 2- Store the terraform state file in the S3 backend**

1. I have written code to create custom vpc through terraform code.
    
2. The task is to:
    
    Create VPC Create Internet Gateway Create Public subnet Create Private Subnet Create Public route table Create Private route table  
    [**provider.tf**](http://provider.tf/)
    
    [**Variables.tf**](http://variables.tf/)  
    terraform.tfvars  
    [**backend.tf**](http://backend.tf/)
    
3. [**provider.tf**](http://provider.tf/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694328931682/f53c72d1-c879-40a0-9d08-539665c569c2.png?auto=compress,format&format=webp align="left")
    
4. [**variables.tf**](http://variables.tf/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694328971942/da7afe61-d4c1-47a3-84c5-7c55c1340250.png?auto=compress,format&format=webp align="left")
    
5. terraform.tfvars
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694329033372/75fe1201-e3fa-4568-9ac5-3086acfb237d.png?auto=compress,format&format=webp align="left")
    
6. [**vpc.tf**](http://vpc.tf/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694329054812/213195bb-c393-44b8-a4fb-770e4a552bec.png?auto=compress,format&format=webp align="left")
    
7. [**public-subnet.tf**](http://public-subnet.tf/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694329176693/edfdf72d-6025-4720-93d0-85bd1c65e31c.png?auto=compress,format&format=webp align="left")
    
8. [**private-subnet.tf**](http://private-subnet.tf/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694329209042/b322fb12-fb01-4242-be51-a6681ee0f6f2.png?auto=compress,format&format=webp align="left")
    
9. [**route-table.tf**](http://route-table.tf/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694329845697/fd083844-d348-4052-b165-3c9848de912c.png?auto=compress,format&format=webp align="left")
    
10. [**Internet-gateway.tf**](http://internet-gateway.tf/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694329919541/aac59887-bcd9-4f09-b92c-ca4c83b4220e.png?auto=compress,format&format=webp align="left")
    
11. [**output.tf**](http://output.tf/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694329966601/c7dc2cbc-5805-43f6-b97b-b63597ebcf8c.png?auto=compress,format&format=webp align="left")
    
12. [**backedn.tf**](http://backedn.tf/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694330073613/26a8b963-cec1-4118-be65-b008dd876b20.png?auto=compress,format&format=webp align="left")
    
13. terraform init
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694330168240/460bc6f6-e0d1-42c0-b573-4791af342835.png?auto=compress,format&format=webp align="left")
    
14. terraform validate
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694330197268/3bd1f11d-c852-40b5-afce-f623fd1d0963.png?auto=compress,format&format=webp align="left")
    
15. terraform plan
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694330243376/2dfcdb97-b126-4fc8-a53d-20851a4e485c.png?auto=compress,format&format=webp align="left")
    
16. terraform apply
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694330275868/5920d00f-783b-41cc-9bc9-2a5dff478838.png?auto=compress,format&format=webp align="left")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694330307673/d18db600-f4fa-478f-8bed-f7de81661fde.png?auto=compress,format&format=webp align="left")
    
17. After the creation of vpc my 'terraformtf.statefile' is not showing locally
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694330416126/f9572a13-1892-44f7-ab99-c3b5213c7183.png?auto=compress,format&format=webp align="left")
    
18. 'terraformtf.statefile' has stored in s3 bucket
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694330531594/9528023d-d4a1-42ce-bad9-fe0a99724e98.png?auto=compress,format&format=webp align="left")
    
19. custom vpc created
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694330614253/cc116299-e968-44b8-ad05-510865793749.png?auto=compress,format&format=webp align="left")
    
20. I have destroyed vpc infrastructure through terrafrom destroy
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694330715280/6881d051-f340-4dc5-a07d-d22d5e0f0cf3.png?auto=compress,format&format=webp align="left")
    
21. Destroyed s3 bucket through terraform destroy, before that make sure you delete the terraform state file, delete s3 bucket and then destroy s3.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694331074507/642577df-b0b5-4442-910e-7f9306dddb01.png?auto=compress,format&format=webp align="left")

> ***"Thank you for reading my blog! Happy Learning!!!😊***