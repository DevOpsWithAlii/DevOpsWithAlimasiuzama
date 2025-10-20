---
title: "The Power of Terraform: A Step-by-Step Guide to Validate, Format, and Deploy"
datePublished: Wed Dec 25 2024 05:30:50 GMT+0000 (Coordinated Universal Time)
cuid: cm53gid0m000009jocdo2gvta
slug: the-power-of-terraform-a-step-by-step-guide-to-validate-format-and-deploy
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1760988035286/88f712e6-8ab8-488b-b70d-1312f80fc326.png
tags: aws, devops, terraform, 90daysofdevops, tws

---

## "From Code to Cloud: Terraform's Five Command Checklist for Infrastructure Success"

## **Table of contents**

* * Introduction
        
    * Task 1: Create a Terraform Configuration File
        
    * Task 2: Validate and Manage Terraform State
        
    * Task 3: Add Provisioners to Configure Resources
        
    * Task 4: Implement Lifecycle Management
        
    * Conclusion
        

### Introduction

In the world of **Infrastructure as Code (IaC)**, precision and automation are key.  
Terraform, developed by HashiCorp, allows DevOps engineers to **build, change, and version infrastructure safely and efficiently**.  
In this guide, you’ll learn how to validate, format, and deploy Terraform configurations using real AWS examples — along with key lifecycle and state management commands.

## **Task 1: Create a Terraform Configuration File**

Let’s start by defining infrastructure resources using Terraform — such as an **AWS EC2 instance** (you can try Azure or GCP as well).

### Step 1: Create an Ubuntu Machine on AWS

```plaintext
sudo su
mkdir terraform_aws
cd terraform_aws
```

### 🧩 Step 2: Install Terraform

```plaintext
sudo apt update
sudo apt install wget unzip
wget https://releases.hashicorp.com/terraform/<latest-version>/terraform_<version>_linux_amd64.zip
unzip terraform_<version>_linux_amd64.zip
sudo mv terraform /usr/local/bin/
terraform -version
```

**STEP 3: Create an IAM user:**

* Go to **AWS IAM Console**
    
* Create a new user with **AdministratorAccess**
    
* Generate **Access Key & Secret Key**
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694549528032/5b5c5bd0-c97d-4435-b881-cae0d8564834.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694549541002/67c1d11b-9c94-4e1a-8e03-41fd5a07fc6f.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694549566403/9ffd4cc3-8449-4e98-a550-736b14774683.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694549579166/c8c853b6-d293-46f5-83ac-7896bff03d67.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694549590353/34743448-fffb-4187-9b53-b76d6e3809fe.png?auto=compress,format&format=webp align="left")

**STEP 4: Login as IAM User and Create an ENV Variable.**

sudo apt install awscli -y

aws configure

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694549646703/07e46e88-9308-492f-93f7-19a0ecf15d97.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694549809771/43ecf98b-d3ab-4e86-8550-163c017f0027.png?auto=compress,format&format=webp align="left")

STEP 5: Create a [**variable.tf**](http://variable.tf/) file for AMI (Amazon Machine Image)ID. As per your need, you can select OS and respected AMI ID.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694593571489/9cbd9285-0c50-45b0-9d90-ffa22f08509b.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694593606172/5bf41413-bb2e-447d-b8db-d23c257feda3.png?auto=compress,format&format=webp align="left")

STEP 6: We will set SSH keygen before the configuration of VPC, security groups.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694593945870/a0c0a839-d349-4a70-a522-e958c7599df5.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694594033098/767ba6b4-e1b9-467c-9da4-08928a53a4b0.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694594046994/fac051fe-d66f-409d-a82d-12edb00cd55f.png?auto=compress,format&format=webp align="left")

STEP 7: Now create a file [**main.tf**](http://main.tf/) to configure keypair, vpc and security groups. We will define all these in resource blocks.

**1\. Resource Block (aws\_instance):**  
• Creates an EC2 instance resource.  
• Uses the specified AMI ID to launch the instance.  
• Sets the instance type as t2.micro.  
• Associates the instance with a security group defined by aws\_security\_groups  
• Specifies the key pair name for SSH access.  
• Adds a tag to the instance to identify it.

**2\. Resource Block (aws\_security\_group):**  
• Creates a security group resource and sets the name of the security group.  
•Defines an ingress rule to allow SSH traffic on port 22 from any IP.

**3\. Resource Block (aws\_key\_pair):**  
• Creates an AWS key pair resource and sets the key pair name.  
• Associates the public key from the (home/ubuntu/.ssh/[**terra-key.pub**](http://terra-key.pub/)) resource with the key pair.

**4\. Resource Block (aws\_default\_vpc):**  
• Assign a default vpc to the instance.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694594336190/437b5a64-a652-4d51-a5d6-8bdcab904bd6.png?auto=compress,format&format=webp align="left")

### Task 2: Check state files before running the plan and apply commands & Use the validate command to validate your tf file for errors and provide the Output generated by each command.

Run the below commands in the same directory.

`terraform init`

`terraform plan`

`terraform apply`

Terraform will download the necessary plugins and then create the EC2 instance based on the defined configuration. You can customize the configuration according to your specific requirements.

The state files contain information about the current state of your infrastructure, and Terraform uses this information to determine what changes need to be made. If the state files are not up-to-date, Terraform may make incorrect changes to your infrastructure.

**Check State Files:** To check the state files, you can use the terraform state list command. It lists all the resources managed by Terraform and their current state.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694596163900/c4c730b8-afc0-4aa4-9c95-d83c53ac2767.png?auto=compress,format&format=webp align="left")

**Validate Configuration File:** To validate the configuration file for errors, you can use the terraform validate command. It checks the syntax and structure of the Terraform files and reports any errors or warnings.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694596199331/2312c150-36db-4f30-a053-80599b4f4bc6.png?auto=compress,format&format=webp align="left")

These commands help you check the state files, validate the configuration, and get insights into the changes that Terraform will make to your infrastructure before actually applying them.

### Task 3: Add a provisioner to the configuration file to configure the resource after it is created and use Terraform commands to apply for changes and destroy to remove resources.

To configure a resource after it is created, you can use provisioners in Terraform. Provisioners allow you to run scripts or execute commands on the resource during creation or destruction.

**Provider Block:**  
• Sets the provider as AWS and specifies the region as “eu-west-2”. Here’s an example of adding a provisioner to an AWS EC2 instance resource:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694596611651/ed1f8447-37bf-4038-b1bc-51404421d517.png?auto=compress,format&format=webp align="left")

To apply changes and provision the resources, you can use the following command:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694596748342/9999f9aa-52d1-448e-9e6a-512767ee5648.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694596762924/4bbe851b-8b80-43e4-8eac-534f1df20223.png?auto=compress,format&format=webp align="left")

Check the instance and click on the connect button, your instance will be running successfully.

**Output:**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694599434398/6e4ae53b-69b6-42b8-9c12-224b6620aeb4.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694599459766/d2be9d40-9c7f-4530-a246-2f0a5beb3bdb.png?auto=compress,format&format=webp align="left")

To destroy the resources created by the Terraform configuration, you can use the following command:

`terraform destroy`

This command will remove the EC2 instance and any associated resources.

### Task 3: Add lifecycle management configurations to the configuration file to control the creation, modification, and deletion of the resource and use Terraform commands to apply the changes.

To control the lifecycle management of resources in Terraform, you can use lifecycle blocks. These blocks allow you to define specific behaviour for resource creation, modification, and deletion. Here’s an example of adding lifecycle management configurations to an AWS EC2 instance resource:

**Example:1**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694598839201/a6c59324-6f32-4b5f-a144-1baf323eed1f.png?auto=compress,format&format=webp align="left")

The prevent\_destroy parameter is set to true, which will prevent destroying instances accidentally.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694598965994/1f7017f2-400a-4ee1-9b46-ddc83f402d04.png?auto=compress,format&format=webp align="left")

**Example:2**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694598286495/ead97522-f814-422b-afbb-5be1cbce8be2.png?auto=compress,format&format=webp align="left")

In the above example, a lifecycle block is added to the AWS EC2 instance resource. The prevent\_destroy parameter is set to true, which means that Terraform will create a new instance before destroying the old one during updates. The prevent\_destroy parameter is set to false, which allows the instance to be destroyed using the terraform destroy command

To apply changes and create or update the resource with the lifecycle management configurations, you can use the following command:

`terraform apply`

`terraform destroy`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694599233035/341d1b1f-f416-41ee-aeed-345e72a966fe.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694599356811/11a79068-1cb2-4032-97d7-c06234675412.png?auto=compress,format&format=webp align="left")

**Output:**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694599603426/3be97033-599e-41b8-abf9-fb53470e8fd9.png?auto=compress,format&format=webp align="left")