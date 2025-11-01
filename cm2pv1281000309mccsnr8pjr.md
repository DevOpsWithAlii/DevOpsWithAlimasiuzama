---
title: "AWS IAM & Its Features-"
datePublished: Tue Oct 21 2025 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cm2pv1281000309mccsnr8pjr
slug: aws-iam-and-its-features
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1760868331343/efc5b561-db64-4dfd-894d-75ae2161ded1.png
tags: linux, aws, google, devops, aws-iam, 90daysofdevops, trainwithshubham, tws

---

## AWS IAM

8 min read

## **Table of content**[**s**](https://hashnode.com/@BhavyaBojanapalli)

* [Features of IAM:](https://bhavyabojanapalli.hashnode.dev/aws-iam-its-features-day-1#heading-features-of-iam)
    
* [IAM Users:](https://bhavyabojanapalli.hashnode.dev/aws-iam-its-features-day-1#heading-iam-users)
    
* [IAM Groups:](https://bhavyabojanapalli.hashnode.dev/aws-iam-its-features-day-1#heading-iam-groups)
    
* [IAM Roles:](https://bhavyabojanapalli.hashnode.dev/aws-iam-its-features-day-1#heading-iam-roles)
    
* [Important Terms associated with IAM Roles:](https://bhavyabojanapalli.hashnode.dev/aws-iam-its-features-day-1#heading-important-terms-associated-with-iam-roles)
    

IAM stands for Identity and Access Management. It is like a central manager that helps you control who can enter your cloud, access AWS resources and what they can do once inside.

* **Identity:** IAM helps you manage the identities (like usernames or service accounts) that can interact with your AWS resources.
    
* **Access:** It determines what actions (like reading, writing, or deleting data) each identity is allowed to perform on AWS services.
    

## Key Features of IAM

**1\. Control Who Does What**  
IAM lets you decide which users, groups, or services can access AWS resources, and what actions they can perform.

**2\. Extra Security with MFA (Multi-Factor Authentication)**  
You can add an extra layer of security by requiring a second verification method, such as a code from a mobile app, in addition to a password.

**3\. Simplify Access for AWS Services**  
AWS services like EC2 or Lambda often need access to other AWS services such as S3. IAM allows this securely, without using long-term access keys.

**4\. Fine-Tuned Permissions**  
You can define exactly what each user or group can do — for example, allowing them to read S3 data but not delete it.

**5\. Access Advisor Insights**  
IAM provides visibility into which permissions are being used, helping you optimize and remove unnecessary access.

## IAM Identities

There are **three main types of IAM identities:**

1. **IAM Users**
    
2. **IAM Groups**
    
3. **IAM Roles**
    

### IAM Users

IAM Users represent individual people, applications, or services that need access to AWS resources.

**Key Points:**

* Each user has a unique username (e.g., *Ali*, *DevUser*).
    
* Users can access AWS through passwords (console) or access keys (CLI, SDK).
    
* Policies attached to a user define their allowed actions.
    
* MFA can be enabled for additional security.
    

**Example:**  
Create a user named `developer1` who can start EC2 instances but cannot delete S3 buckets.

### IAM Groups

IAM Groups are collections of users that share the same permissions.

**Why They’re Useful:**

* You can attach one policy to a group instead of repeating it for every user.
    
* All users in the group automatically get the same permissions.
    
* Changing a group’s policy updates access for everyone in that group.
    

**Example:**  
A group named `Developers` has permissions to deploy applications. All developers in that group inherit those permissions.

### IAM Roles

IAM Roles are not tied to specific users. Instead, they are used when AWS **services, external users, or other AWS accounts** need temporary access to your resources.

**How Roles Work:**

* Roles have **trust relationships** that define who can assume them.
    
* When assumed, they provide **temporary security credentials**.
    
* Common use cases: EC2 accessing S3, Lambda accessing DynamoDB, or cross-account access.
    

**Example:**  
An EC2 instance assumes a role to read data from a private S3 bucket — no need for permanent credentials.

## Key Concepts in IAM Roles

### 1\. Delegation

Delegation means allowing another AWS account or service to access your resources.

* The **trusted account** owns the resources.
    
* The **trusting account** is the one requesting access.
    

Delegation types include:

* Same account
    
* Two accounts under the same organization
    
* Two accounts from different organizations
    

### 2\. Policies in IAM Roles

Two types of policies are attached to IAM Roles:

#### Permission Policy

Specifies **what actions** are allowed or denied.

* Attached directly to the IAM Role.
    

### 3\. Other Important Terms

* **Federation:** Establishing trust between an external identity provider (like Google Workspace or Okta) and AWS.
    
* **Permission Boundary:** Sets the maximum permissions that an IAM user or role can have.
    
* **Principal:** The entity (user, role, or AWS account) receiving the permissions.
    
* **Cross-Account Access:** Allows one AWS account to securely access resources in another account.
    

## IAM Roles — Two Ways to Use Them

**1\. IAM Console:**  
A user can assume a role temporarily while working in the AWS console. When they exit the role, their original permissions return.

**2\. Programmatic Access:**  
AWS services (like EC2 or Lambda) assume roles automatically to get temporary credentials for secure operations.

## Summary

| IAM Concept | Description |
| --- | --- |
| **User** | Represents an individual person or application with credentials. |
| **Group** | A collection of users sharing common permissions. |
| **Role** | Grants temporary access to AWS services or external entities. |
| **MFA** | Adds a second authentication layer for stronger security. |
| **Policies** | Define allowed or denied actions on AWS resources. |

An IAM Role can be used in the following ways:

1. IAM User
    
2. Applications and services
    
3. Federated Users.
    

Following are the cases of Roles:

![](https://miro.medium.com/v2/resize:fit:1400/1*sGtY53AuPP-0Kwc2SjhlyQ.png align="left")

1. Switch to a role as an IAM user in one AWS account to access another account that you own.
    
2. Providing access to an AWS Service.
    
3. Providing access to external authenticated users. (sometimes users have identities outside of AWS such as corporate directory, if such users want to work with AWS resources then they should have security credentials. In such a situation role is to specify the permission for third-party identity providers\[IDP\]).
    

* **Web-Identity federation —** Users do not require any custom sign-in or user identities. Users can use any external identity provider (Facebook, amazon, google..), after login users get **an authentication token**, and they can **exchange an authentication token for receiving temporary security credentials**.
    
* \*\*SAML—\*\*based Federation **(Security Assertion Markup language)** is an open framework that many identity providers use, which provides a single sign-on to access the AWS Management console.
    

4\. Providing access to third parties: (when third party want to access the AWS resources then they can use roles to delegate access to them without sharing security credentials).

**Creating IAM Roles:**

Step : 1

![](https://miro.medium.com/v2/resize:fit:1400/1*A_HgEwaxzHR2tUtYksr7mA.png align="left")

Step : 2

![](https://miro.medium.com/v2/resize:fit:5760/1*J2M1MFIdkpeAk-si9P522Q.png align="left")

![](https://miro.medium.com/v2/resize:fit:5760/1*2JzW4Y_9m9dMp-O0eKEfXQ.png align="left")

Attach policies if required (JSON format Trust , Permission Policy)

![](https://miro.medium.com/v2/resize:fit:1400/1*2tx-W7N2vUiQ2R1oP-T6kw.png align="left")

Step : 3: Add Role Name , Description … and create role will create.

![](https://miro.medium.com/v2/resize:fit:1400/1*sWVWi2-Jm6EM4yup3_kyqA.png align="left")

Step :4 Creating IAM roles for an IAM User. (sample where you can do same)

![](https://miro.medium.com/v2/resize:fit:1400/1*ISYp1xPoYZRecWu1E4laFA.png align="left")

Hope the basic things of IAM are clear,in next part let see about EC2 (Elastic Compute Cloud).

## **Subscribe to my newsletter**