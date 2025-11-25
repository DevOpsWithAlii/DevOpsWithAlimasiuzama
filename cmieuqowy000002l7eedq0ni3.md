---
title: "Shell Scripting Real-World  AWS, Jenkins, Docker & Kubernetes Automation Scripts (With Explanations)"
datePublished: Tue Nov 25 2025 17:30:52 GMT+0000 (Coordinated Universal Time)
cuid: cmieuqowy000002l7eedq0ni3
slug: shell-scripting-real-world-aws-jenkins-docker-and-kubernetes-automation-scripts-with-explanations
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1764090598739/4ac67636-6088-4433-9a1b-2671f8f209af.png
tags: linux, shell, scripting, shell-script, 90daysofdevops

---

Automation is the heart of DevOps. And one of the most powerful automation tools is **Shell Scripting**. In this guide, you’ll learn **real DevOps-focused shell scripts**, each explained line-by-line, tool-by-tool:

* AWS (S3, EC2)
    
* Jenkins (Trigger jobs, get status)
    
* Docker (backup, cleanup, listing containers)
    
* Kubernetes (pods, nodes, health checks)
    

# **Table of Contents**

1. Why DevOps Engineers Need Shell Scripting
    
2. AWS Automation Scripts
    
    * S3 Sync Backup
        
    * List EC2 Instances
        
3. Linux Service Monitoring Script
    
4. Jenkins Automation
    
    * Trigger Jenkins Job
        
    * Check Jenkins Job Status
        
5. Docker Automation Scripts
    
    * List Running Containers
        
    * Backup Containers & Images
        
    * Delete Unused Images
        
6. Kubernetes Automation Scripts
    
    * Watch Live Pods
        
    * Pod Health Check
        
    * Restart All Pods
        
    * Node Status Check
        
7. Final Thoughts
    

# **Why Shell Scripting Matters in DevOps**

DevOps automation = repeatable, predictable, error-free processes.

Shell scripting helps you automate:

* Server provisioning
    
* AWS CLI tasks
    
* Monitoring
    
* CI/CD workflows
    
* Docker operations
    
* Kubernetes management
    

Every DevOps engineer must know how to build scripts that run cloud commands automatically.

# **1\. AWS Automation Scripts**

## **1.1 Sync Local Directory to AWS S3 (Automated Backup)**

This script uploads a local directory (like logs or DB dumps) to an S3 bucket.

### **📌 Script**

```bash
#!/bin/bash                                 # Use Bash shell

BUCKET_NAME="my-backup-bucket"               # S3 bucket name
SOURCE_DIR="/backup"                         # Local directory to sync

echo "Syncing $SOURCE_DIR to S3 bucket $BUCKET_NAME..."   # Show progress

aws s3 sync $SOURCE_DIR s3://$BUCKET_NAME --delete        
# aws s3 sync = Copies files
# $SOURCE_DIR = Local folder
# s3://$BUCKET_NAME = Destination bucket
# --delete = Removes files from bucket if deleted locally

echo "Sync Completed!"                       # Print completion message
```

## **1.2 List AWS EC2 Instances with Public IPs**

Useful when managing multiple servers.

### **📌 Script**

```bash
#!/bin/bash

echo "Fetching EC2 Instances..."             # Progress message

aws ec2 describe-instances \
--query "Reservations[*].Instances[*].[InstanceId, PublicIpAddress]" \
--output table
# --query = Filters output using JMESPath
# Shows Instance ID + Public IP only
# --output table = Makes it readable
```

# **2\. Check if a Linux Service is Running**

Example: restart Nginx/MySQL automatically if it stops.

### **📌 Script**

```bash
#!/bin/bash

SERVICE="nginx"                                # Service name to monitor

if systemctl is-active --quiet $SERVICE; then  # Check if running
    echo "$SERVICE is running"                 # OK
else
    echo "$SERVICE is NOT running, restarting..."  # Error detected
    systemctl.restart $SERVICE                 # Restart service
fi
```

# **3\. Jenkins Automation Scripts**

## **3.1 Trigger Jenkins Job Without UI**

Perfect for CI/CD triggers, Git hooks, webhooks.

### **📌 Script**

```plaintext
#!/bin/bash

# ================================
# Jenkins Connection Information
# ================================

JENKINS_URL="http://localhost:8080"      # URL of your Jenkins server
JOB_NAME="MyJob"                         # Name of the Jenkins job you want to trigger
USER="admin"                             # Jenkins username
API_TOKEN="your-api-token"               # API token for authentication (NOT your password)

# ================================
# Display a friendly message
# ================================

echo "Triggering Jenkins job: $JOB_NAME..."   # Inform the user what's happening

# ================================
# Trigger Jenkins Job using curl
# ================================

curl -X POST "$JENKINS_URL/job/$JOB_NAME/build" \   # -X POST means send a POST request to trigger the job
  --user "$USER:$API_TOKEN"                         # Send username + API token for authentication
                                                     # Without this, Jenkins will reject the request

# Important Notes:
# /job/<JOB_NAME>/build → this Jenkins API endpoint starts a new build
# curl sends the request directly to Jenkins API
# No need to open Jenkins UI manually

# ================================
# Confirmation message
# ================================

echo "Job triggered successfully!"   # Final message after triggering the job
```

## What This Script Does

* Triggers a Jenkins job **from the terminal**
    
* No need to open the Jenkins web UI
    
* Useful in automation (Git hooks, cron jobs, CI/CD pipelines)
    
* Uses **API token** for secure authentication
    
* Works for freestyle, pipeline, and multibranch jobs
    

## **3.2 Check Jenkins Job Status**

Shows whether last build = SUCCESS / FAILURE.

### **📌 Script**

```plaintext
#!/bin/bash

# ================================
# Jenkins Login Details
# ================================

JENKINS_URL="http://localhost:8080"    # Jenkins server URL
JOB_NAME="MyJob"                       # Job you want to check
USER="admin"                           # Jenkins username
API_TOKEN="your-api-token"             # Jenkins API token (NOT password)


# ================================
# Tell the user what is happening
# ================================

echo "Checking last build status for: $JOB_NAME..."


# ================================
# Call Jenkins REST API
# ================================

curl -s "$JENKINS_URL/job/$JOB_NAME/lastBuild/api/json" \   # -s = silent output (no progress bar)
  --user "$USER:$API_TOKEN" \                                # Login using username + API token
  | jq -r '.result'                                          # Extract only the build result (SUCCESS, FAILURE, etc.)
# jq -r → raw output without quotes
# .result → field in JSON that stores the build status

# ================================
# Possible Results Explained
# ================================
# SUCCESS   → Build completed successfully
# FAILURE   → Build failed
# ABORTED   → Someone cancelled the build
# UNSTABLE  → Tests failed or warnings found
# null      → Build is still running (no result yet)
```

# **4\. Docker Automation Scripts**

## **4.1 List All Running Containers**

### **📌 Script**

```bash
#!/bin/bash

echo "Running Docker containers (ID + Name):"    # Header

docker ps --format "{{.ID}}  {{.Names}}"         # Show only ID and container name
# {{.ID}}    → container ID
# {{.Names}} → container name
# Very simple and clean output
```

## **4.2 Backup Docker Containers & Images**

Used during migrations, upgrades, or disaster recovery.

### **📌 Script**

```bash
#!/bin/bash

# =============================
# Create a backup folder
# =============================

BACKUP_DIR="/backup/docker"      # Location to store backups
mkdir -p "$BACKUP_DIR"           # Create folder if it does not exist

echo "Backing up running containers..."

# =============================
# Backup all RUNNING containers
# =============================

for container in $(docker ps -q); do          # Loop through container IDs

    docker commit "$container" "${container}-backup"
    # docker commit → takes a snapshot of the running container

    docker save -o "$BACKUP_DIR/${container}.tar" "${container}-backup"
    # docker save → save that snapshot as a .tar file
done

echo "Backing up all Docker images..."

# =============================
# Backup ALL Docker images
# =============================

docker images -q | xargs -I {} docker save -o "$BACKUP_DIR/{}.tar" {}
# docker images -q → list only image IDs
# xargs → passes each ID into docker save
# docker save → exports image as a tar file

echo "Backup completed successfully!"
echo "Files saved in: $BACKUP_DIR"
```

## **4.3 Delete Old Unused Docker Images**

Very useful for freeing disk space.

### **📌 Script**

```bash
#!/bin/bash

echo "Deleting unused Docker images..."    # Info
docker image prune -a -f                  # Remove unused images
echo "Cleanup done!"                      # Completed
```

# **5\. Kubernetes Automation Scripts**

## **5.1 Monitor Pod Status (Live Watch)**

This is used during deployments.

### **📌 Script**

```bash
#!/bin/bash

# =============================
# Select Kubernetes Namespace
# =============================

NAMESPACE="default"      # Namespace you want to monitor (e.g., default, dev, prod)

echo "Monitoring pods in namespace: $NAMESPACE..."   # Display message

# =============================
# Live Pod Watch Command
# =============================

kubectl get pods -n $NAMESPACE --watch
# kubectl get pods → list all pods
# -n $NAMESPACE   → check pods inside the chosen namespace
# --watch         → keeps the command running & updates live
# Useful during deployments or troubleshooting
```

## **5.2 Kubernetes Pod Health Check**

Alerts if any pod is not in Running state.

### **📌 Script**

```bash
#!/bin/bash

# =============================
# Select Namespace to Check
# =============================

NAMESPACE="default"     # Namespace you want to check (example: default, dev, prod)

echo "Checking Kubernetes Pod Health..."   # Message before running check

# =============================
# Find Pods That Are NOT Running
# =============================

kubectl get pods -n $NAMESPACE --no-headers | \        # List all pods without header row
awk '$3 != "Running" {                                  # If column 3 (STATUS) is NOT "Running"
    print "⚠️ Pod "$1" is in state "$3                  # Print alert with pod name + its state
}'
# Column meanings from 'kubectl get pods':
# $1 = Pod Name
# $2 = Ready containers
# $3 = STATUS (Running, Pending, CrashLoopBackOff, Error)

echo "Health Check Completed."   # Final message
```

## **5.3 Restart All Pods in a Namespace**

Used after config updates, DNS issues, or deployments.

### **📌 Script**

```bash
#!/bin/bash

# =============================
# Choose Namespace
# =============================

NAMESPACE="default"     # Namespace where pods need to be restarted

echo "Restarting all pods in $NAMESPACE..."     # Display message before restart

# =============================
# Delete All Pods → K8s Recreates Them Automatically
# =============================

kubectl delete pods --all -n $NAMESPACE --grace-period=0 --force
# kubectl delete pods --all → deletes every pod in the namespace
# -n $NAMESPACE            → applies to the selected namespace
# --grace-period=0         → immediate kill, no waiting time
# --force                  → force delete even if pods are stuck
# Kubernetes automatically re-creates pods through Deployments/ReplicaSets

# =============================
# Completion Message
# =============================

echo "All pods restarted!"        # Inform user that process is done
```

\- Deletes all pods in the namespace  
\- Kubernetes automatically recreates them (because Deployments/ReplicaSets manage them)  
\- Useful when:

* Pods are stuck
    
* Pods are in CrashLoopBackOff
    
* ConfigMaps/Secrets updated & need restart
    
* Deployment not refreshing
    

## **5.4 Check Kubernetes Node Status**

Useful for cluster health.

### **📌 Script**

```bash
#!/bin/bash

echo "Checking Kubernetes Nodes..."

kubectl get nodes | grep -v "Ready"
# Show nodes NOT in Ready state
```

## **Final Thoughts**

Shell scripting is one of the **most powerful weapons** in a DevOps engineer’s toolbox.  
With AWS CLI, Docker CLI, Kubernetes CLI, and Jenkins CLI — automation becomes easy.