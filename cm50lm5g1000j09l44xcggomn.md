---
title: "Terraform - Data Sources in terraform resources"
datePublished: Mon Dec 23 2024 05:30:27 GMT+0000 (Coordinated Universal Time)
cuid: cm50lm5g1000j09l44xcggomn
slug: terraform-data-sources-in-terraform-resources
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1760986904107/ffca8247-189b-4061-935c-118bbb02fdc5.png
tags: devops, terraform, 90daysofdevops

---

## **Table of contents**

* [Use of Data source in terraform code](https://namg.hashnode.dev/terraform-data-sources-in-terraform-resources#heading-use-of-data-source-in-terraform-code)
    
* [Data block:](https://namg.hashnode.dev/terraform-data-sources-in-terraform-resources#heading-data-block)
    
* [Use case 1: Use of manually created golden ami in the data block](https://namg.hashnode.dev/terraform-data-sources-in-terraform-resources#heading-use-case-1-use-of-manually-created-golden-ami-in-the-data-block)
    
* [Steps to Create Golden Ami](https://namg.hashnode.dev/terraform-data-sources-in-terraform-resources#heading-steps-to-create-golden-ami)
    
* [Task:](https://namg.hashnode.dev/terraform-data-sources-in-terraform-resources#heading-task)
    
    * [Write code to create an EC2 instance by use of existing Golden AMI and Terraform state-file.](https://namg.hashnode.dev/terraform-data-sources-in-terraform-resources#heading-write-code-to-create-an-ec2-instance-by-use-of-existing-golden-ami-and-terraform-state-file)
        
    * [Run terraform project:](https://namg.hashnode.dev/terraform-data-sources-in-terraform-resources#heading-run-terraform-project)
        
    * [Validate Output:](https://namg.hashnode.dev/terraform-data-sources-in-terraform-resources#heading-validate-output)
        

### Use of Data source in terraform code

**Terraform Data Source**:

* The data sources allow terraform to use the information defined outside of terraform, defined by another separate terraform configuration or directly created in provider resources using another function.
    
* Using data source is accessed via a special kind of resource known as a '**data** **resource'**, declared using a **data block**.
    

### **Data block:**

* A `data` block in Terraform allows you to retrieve information about a resource. For example, you can look up the latest Amazon Machine Image (AMI) you’ve created:
    

```plaintext
data "aws_ami" "example" {
  most_recent = true
  owners      = ["self"]
  tags = {
    Name   = "app-server"
    Tested = "true"
  }
}
```

This block finds your most recent AMI in AWS that matches your custom tags.

### Use case 1: Use of manually created golden ami in the data block

## Use Case 1: Referencing a Manually Created Golden AMI

A **Golden AMI** is a custom Amazon Machine Image pre-installed with packages (like Apache) and configurations that you need on all your servers. Creating one involves:

1. Launching an EC2 instance.
    
2. Logging in and installing software such as Apache.
    
3. Verifying its status.
    
4. Going to Instance Actions → Image and templates → Create Image.
    
5. Viewing your new AMI under "Images" in AWS.
    

Whenever you launch a new instance using the Golden AMI, it’s already configured, saving you time and effort.

### Steps to Create Golden Ami

1. Create a normal ec2 instance
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694480132589/615af349-5521-4b53-bbaa-3636a24197eb.png?auto=compress,format&format=webp align="left")
    
2. Login to web-server
    
3. Install Apache over there
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694518848980/cecc2ff7-d6f7-4d5a-97bd-8bc3ab85862e.png?auto=compress,format&format=webp align="left")
    
4. Check the status of apache2
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694519013305/594ef22e-4b7d-46e0-b5b2-92315303e36d.png?auto=compress,format&format=webp align="left")
    
5. Go to Created Instance Actions&gt; Image and templates &gt; Create Image
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694480230251/3af07402-5f16-472f-ab31-2401b34e6ef8.png?auto=compress,format&format=webp align="left")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694480485898/b35aec1c-2612-478f-a8a1-8bf5b437436f.png?auto=compress,format&format=webp align="left")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694480527642/382f2c07-9314-49a1-bee0-44fd400b3441.png?auto=compress,format&format=webp align="left")
    
6. You can check the created AMI at 'Images'
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694480695531/66c7407b-d91d-405e-b84b-58a1f1c1c38c.png?auto=compress,format&format=webp align="left")
    
7. The custom ami you have created is pre-loaded with default configurations that you are expecting to be available on every instance in my project. This AMI called <mark>Golden AMI</mark>
    
8. Now, go to the launch instance and create an instance by using custom ami.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694481255344/a77ce019-4d00-4d45-a8e5-5132bb9f4c22.png?auto=compress,format&format=webp align="left")
    
9. Launch the instance and see all created instance
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694481434547/65bbcc24-a66b-4cfa-97ae-df5296752ee4.png?auto=compress,format&format=webp align="left")
    
10. When I logged in to the tws-webserver and checked the status of apche2, it was running fine.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694519861648/1b231ff2-b374-4ef4-80e0-5ed47a80901c.png?auto=compress,format&format=webp align="left")
    

Use case2: Use of vpc resource from Terraform created state file

* Below is the example data source block to get the explored variables from another terraform project remote state file.
    

data "terraform\_remote\_state" "vpc" {  
backend = "s3"  
config = {  
bucket = "ed-k8s-config-data"  
key = "dft/vpc/terraform.tfstate"  
region = "us-east-1"  
}  
}

## Task:

### Write code to create an EC2 instance by use of existing Golden AMI and Terraform state-file.

`Terraform Resources created:`

<mark>S3-Backend-project</mark>

1. dynamodb
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694523294747/75beef5d-a8a4-4220-abc7-c786e739152f.png?auto=compress,format&format=webp align="left")
    
2. [**s3.tf**](http://s3.tf/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694523334345/c4ffae17-9f53-4e8c-9ddb-24c230c390fd.png?auto=compress,format&format=webp align="left")
    

<mark>vpc-project</mark>

1. [**backend.tf**](http://backend.tf/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694523483195/9816f192-002e-451a-94b1-5f71a775a6cb.png?auto=compress,format&format=webp align="left")
    
2. [**igw.tf**](http://igw.tf/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694523521626/5d9a49d4-8b7e-436b-a69b-ca20ebc35bab.png?auto=compress,format&format=webp align="left")
    
3. [**output.tf**](http://output.tf/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694523573226/2fd2f017-6b26-4155-a324-d9e438b100fb.png?auto=compress,format&format=webp align="left")
    
4. [**priv-subnet.tf**](http://priv-subnet.tf/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694523628104/2d786067-347b-44bf-9c43-62b1df8b58c9.png?auto=compress,format&format=webp align="left")
    
5. [**pub-subnet.tf**](http://pub-subnet.tf/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694523697468/a35fbcab-4637-4c35-a00f-0af13e26324b.png?auto=compress,format&format=webp align="left")
    
6. [**rt.tf**](http://rt.tf/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694523734139/304e8ae1-ea5e-4418-8147-d9ec1c420d6a.png?auto=compress,format&format=webp align="left")
    
7. terraform.tfvars
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694523759984/0378bbf5-ce07-4eef-ab06-cc26e46f667c.png?auto=compress,format&format=webp align="left")
    
8. [**variables.tf**](http://variables.tf/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694523779096/7bcb08a0-03ef-4058-8baf-af5fef9fd10e.png?auto=compress,format&format=webp align="left")
    
9. [**vpc.tf**](http://vpc.tf/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694523802954/011a588e-8709-42b1-8272-51f5f265101f.png?auto=compress,format&format=webp align="left")
    

<mark>EC2-project</mark>

1. [**backend.tf**](http://backend.tf/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694523904974/d66d338b-00c5-451a-a8be-a20a49e605a9.png?auto=compress,format&format=webp align="left")
    
2. [**ec2.tf**](http://ec2.tf/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694524005998/4c78697d-dcb7-48ef-a125-851b48fa3600.png?auto=compress,format&format=webp align="left")
    
3. terraform.tfvars
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694524025161/f99744fb-6d83-4d88-a170-b7474c9e7826.png?auto=compress,format&format=webp align="left")
    
4. [**variables.tf**](http://variables.tf/)
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694524042540/d526beb4-230d-4971-a2b0-eff44420d789.png?auto=compress,format&format=webp align="left")

### Run terraform project:

1. S3-backend-project
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694524317259/969b72c7-b5f7-46c7-b8ac-6966d4fcf820.png?auto=compress,format&format=webp align="left")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694524258416/1d917a02-cf52-4ff0-9661-e6014cf5bcc0.png?auto=compress,format&format=webp align="left")
    
2. vpc-project
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694524392142/b73c34c1-8381-4756-bbc5-251d93f708f4.png?auto=compress,format&format=webp align="left")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694524490823/859773c6-6b6c-43ec-8f68-cae264639d25.png?auto=compress,format&format=webp align="left")
    
3. EC2-project
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694524555970/6a9958bd-37fb-44db-8424-b2ff00461f1d.png?auto=compress,format&format=webp align="left")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694524707277/35de5dda-4879-4076-a2da-2e851db0655b.png?auto=compress,format&format=webp align="left")
    

### Validate Output:

1. s3 backend
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694525051238/c3a042d6-60c5-4896-8b91-4730c875767a.png?auto=compress,format&format=webp align="left")
    
2. VPC
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694524947443/7c7a56b7-e5c8-4e87-bc66-872abaaf152d.png?auto=compress,format&format=webp align="left")
    
3. EC2
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694525108923/76d02a3e-73f5-44c0-ac59-cf0b0a4f5acf.png?auto=compress,format&format=webp align="left")
    

> ***Thank you for reading. Happy Learning😊***