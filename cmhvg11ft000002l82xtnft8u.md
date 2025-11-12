---
title: "AWS Cloud Formation"
datePublished: Wed Nov 12 2025 03:31:23 GMT+0000 (Coordinated Universal Time)
cuid: cmhvg11ft000002l82xtnft8u
slug: aws-cloud-formation
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1762009578319/7a1e18ee-3cc5-4d1b-a8dd-5ddb119e9b3f.png
tags: aws, google, search-engine-optimization, aws-cloudformation, 90daysofdevops

---

## **Table of contents**

* [**AWS CloudFormation?**](https://bhavyabojanapalli.hashnode.dev/aws-cloud-formation-day-9#heading-aws-cloudformation)
    
* [**Benefits of AWS CloudFormation:**](https://bhavyabojanapalli.hashnode.dev/aws-cloud-formation-day-9#heading-benefits-of-aws-cloudformation)
    
    * [**1\. Automation and Consistency:**](https://bhavyabojanapalli.hashnode.dev/aws-cloud-formation-day-9#heading-1-automation-and-consistency)
        
    * [**2\. Version Control:**](https://bhavyabojanapalli.hashnode.dev/aws-cloud-formation-day-9#heading-2-version-control)
        
    * [**3\. Scalability and Flexibility:**](https://bhavyabojanapalli.hashnode.dev/aws-cloud-formation-day-9#heading-3-scalability-and-flexibility)
        
* [**Basic Units:**](https://bhavyabojanapalli.hashnode.dev/aws-cloud-formation-day-9#heading-basic-units)
    
* [**Example:**](https://bhavyabojanapalli.hashnode.dev/aws-cloud-formation-day-9#heading-example)
    

![AWS Cloud Formation — Day 9](https://cdn.hashnode.com/res/hashnode/image/upload/v1700550650125/22b673b9-1e25-4af7-8d36-f741372e16fe.png?w=1600&h=840&fit=crop&crop=entropy&auto=compress,format&format=webp&auto=compress,format&format=webp align="left")

### AWS CloudFormation?

AWS CloudFormation is like a magic recipe book for building your digital world on Amazon Web Services (AWS). It's a tool that helps you create and manage all the different pieces of your cloud setup—like virtual servers, databases, storage, and more—without manually clicking around in the AWS console every time you need something new.

### Benefits of AWS CloudFormation:

#### 1\. Automation and Consistency:

CloudFormation automates the process of provisioning and updating infrastructure, reducing the manual effort required. This automation ensures that infrastructure is consistent across different environments, mitigating the risk of configuration drift.

#### 2\. Version Control:

By treating infrastructure as code, CloudFormation templates can be version-controlled using standard version-control systems like Git. This facilitates collaboration among team members and provides a history of changes, making it easier to track and roll back if necessary.

#### 3\. Scalability and Flexibility:

CloudFormation supports the definition of scalable and flexible architectures. Dynamic parameters, conditionals, and the ability to reuse code through nested stacks contribute to the creation of versatile and scalable infrastructure designs.

### Basic Units:

* **Templates:** These are the special documents you write in CloudFormation's language. They're like blueprints that tell AWS what resources you want, like servers, databases, networks, etc.
    
* **Stacks:** When you give CloudFormation a template, it creates a "stack" for you. A stack is like a container that holds all the resources you asked for, managed as a single unit. It's the package that keeps things neat and organized.
    

### Example:

```plaintext
AWSTemplateFormatVersion: '2010-09-09'
Resources:
  MyEC2Instance:
    Type: 'AWS::EC2::Instance'
    Properties:
      ImageId: 'ami-12345678'  # Replace with your desired AMI ID
      InstanceType: 't2.micro' # Replace with your desired instance type
      KeyName: 'my-key-pair'   # Replace with your key pair name
```

**Explanation of the template**:

* `AWSTemplateFormatVersion`: Indicates the CloudFormation template version.
    
* `Resources`: Defines the resources to be created. In this case, it's an EC2 instance named `MyEC2Instance`.
    
    * `Type`: Specifies the resource type, in this case, `AWS::EC2::Instance`.
        
    * `Properties`: Contains the configuration properties for the EC2 instance.
        
        * `ImageId`: The ID of the Amazon Machine Image (AMI) that the instance will use.
            
        * `InstanceType`: Specifies the instance type, such as `t2.micro`.
            
        * `KeyName`: The name of the EC2 key pair that allows you to connect to the instance.
            

Once you have your CloudFormation template ready, follow these steps to create the EC2 instance using the AWS Management Console:

1. **Access AWS CloudFormation**:
    
    * Sign in to the AWS Management Console.
        
    * Go to the AWS CloudFormation service.
        
2. **Create a Stack**:
    
    * Click on the "Create stack" button.
        
    * Choose "Template is ready" and select "Upload a template file".
        
    * Upload or copy-paste your CloudFormation template into the editor.
        
3. **Configure Stack**:
    
    * Enter a stack name.
        
    * Set parameters if your template includes them.
        
    * Click "Next".
        
4. **Options**:
    
    * You can set tags or leave this section as default.
        
    * Click "Next".
        
5. **Review**:
    
    * Review your stack details.
        
    * Check the acknowledgment box.
        
    * Click "Create stack" to initiate the creation process.
        

AWS CloudFormation will then start creating the EC2 instance based on the provided template. Once the stack creation is complete, you'll find the provisioned EC2 instance in your AWS account.

Please ensure you have appropriate permissions and that the settings in your template, such as the AMI ID and key pair, are valid and accessible in your AWS account. Adjust the template properties according to your specific requirements before creating the stack.