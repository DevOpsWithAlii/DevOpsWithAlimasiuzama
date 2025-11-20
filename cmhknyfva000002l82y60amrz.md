---
title: "Deploy a Two-Tier Flask & MySQL Application Using Jenkins Pipeline on AWS EC2"
datePublished: Tue Nov 04 2025 14:27:51 GMT+0000 (Coordinated Universal Time)
cuid: cmhknyfva000002l82y60amrz
slug: deploy-a-two-tier-flask-and-mysql-application-using-jenkins-pipeline-on-aws-ec2
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1762264642103/c270a343-ca72-4df2-a116-5ace28f99172.png
tags: google, devops, jenkins, cicd-cjy1vtdk2005kjjs17n8couc3, jenkins-devops, 90daysofdevops

---

## Table of Contents

1. Introduction
    
2. What is a Two-Tier Application?
    
3. Architecture Overview
    
4. Prerequisites
    
5. Step 1: Launch EC2 Instance
    
6. Step 2: Install Jenkins, Docker, and Dependencies
    
7. Step 3: Setup GitHub Repository
    
8. Step 4: Create Jenkins Pipeline
    
9. Step 5: Write the Jenkinsfile (Groovy Pipeline Script)
    
10. Step 6: Build and Deploy Flask Application
    
11. Step 7: Verify Application
    
12. Step 8: View Build Results in Blue Ocean
    
13. Common Errors & Fixes
    
14. Conclusion
    

## **1\. Introduction**

In this project, we have deployed a **Two-Tier Flask Application** connected to a **MySQL database**, using **Jenkins Pipeline** for automation and **Docker** for containerization — all hosted on **AWS EC2.**.

## **2\. What is a Two-Tier Application?**

A **two-tier architecture** separates the application into:

* **Tier 1 – Frontend/Application Layer:** Flask web app (Python)
    
* **Tier 2 – Database Layer:** MySQL database
    

This design improves scalability, maintainability, and performance.

## **3\. Architecture Overview**

**Flow:**

1. Developer pushes code to **GitHub**.
    
2. **Jenkins** automatically pulls the code.
    
3. Jenkins builds the **Docker image** and runs **docker-compose**.
    
4. Flask container communicates with the **MySQL container**.
    
5. App is hosted and accessible through the EC2 public IP.
    

## **4\. Prerequisites**

* AWS Account
    
* EC2 Instance (Ubuntu 24.04, t2.medium for better performance)
    
* Installed:
    
    * Jenkins
        
    * Docker & Docker Compose
        
    * Git
        
* GitHub Repository
    

## **5\. Step 1: Launch EC2 Instance**

* AMI: **Ubuntu 24.04**
    
* Type: **t2.medium**
    
* Add ports in Security Group:
    
    * 8080 → Jenkins
        
    * 5000 → Flask App
        
    * 22 → SSH
        
* Connect to instance via EC2 Instance Connect or SSH.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1762264992038/e9093df2-0701-43bf-b276-41508c997ae9.jpeg align="center")

## **6\. Step 2: Install Jenkins, Docker, and Dependencies**

```plaintext
sudo apt update
sudo apt install openjdk-17-jdk -y
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install jenkins -y
sudo systemctl enable jenkins
sudo systemctl start jenkins
```

Then install **Docker**:

```plaintext
sudo apt install docker.io docker-compose -y
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1762265488336/3384a1dd-8d78-4b3b-9d2f-f0f18a51378f.jpeg align="center")

## **7\. Step 3: Setup GitHub Repository**

Create a new repo, e.g.  
🔗 [`https://github.com/DevOpsWithAlii/two-tier-flask-app`](https://github.com/DevOpsWithAlii/two-tier-flask-app)

Include:

* [`app.py`](http://app.py)
    
* `Dockerfile`
    
* `docker-compose.yml`
    
* `requirements.txt`
    

## **8\. Step 4: Create Jenkins Pipeline**

* Open Jenkins → **New Item → Pipeline**
    
* Name it: `flaskapp-pipeline`
    
* Tick “GitHub Project” and paste repo URL
    
* Scroll down → Pipeline script section → add your code.
    

## 9\. Step 5: Write the Jenkinsfile

```plaintext
pipeline {
    agent any;
    
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/DevOpsWithAlii/two-tier-flask-app.git", branch: "master"
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t flaskapp ."
            }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
```

## **10\. Step 6: Build and Deploy Flask Application**

Click **Build Now** in Jenkins.  
It will:

* Clone GitHub code
    
* Build Docker image
    
* Deploy Flask + MySQL containers
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1762265847486/c7e4df0f-6f28-4578-9dd5-709e52fa5f03.jpeg align="center")

## **11\. Step 7: Verify Application**

Visit:  
👉 `http://<EC2-Public-IP>:5000`  
Example:  
[`http://18.234.86.181:5000`](http://18.234.86.181:5000)

You should see your Flask app (as in your screenshot: *“Hello Dosto, Let’s make a 2 Tier Application with Flask and MySQL”*).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1762265933521/9bcd3c95-0062-43ab-a0cb-c37fd0959358.jpeg align="center")

## **12\. Step 8: View in Blue Ocean**

* Install **Blue Ocean Plugin**
    
* Open Jenkins → Blue Ocean → visualize pipeline stages (Code → Build → Deploy)
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1762266028250/26dcdc4e-88e8-483d-9fd8-c149dec83759.jpeg align="center")

## **13\. Common Errors & Fixes**

| Issue | Fix |
| --- | --- |
| Jenkins can’t run Docker commands | Add Jenkins user to Docker group |
| Port already in use | Stop existing container or change ports |
| MySQL connection refused | Update Flask DB credentials or network |
| Pipeline fails on “docker build” | Check Dockerfile path and syntax |

## **14\. Conclusion**

You’ve successfully automated deployment of a **Two-Tier Flask + MySQL Application** using a **Jenkins CI/CD pipeline** hosted on **AWS EC2**.  
This setup demonstrates your practical DevOps knowledge — perfect for your **portfolio and interviews**.