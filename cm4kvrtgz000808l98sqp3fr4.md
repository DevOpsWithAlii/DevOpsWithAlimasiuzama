---
title: "Jenkins Declarative Pipeline"
datePublished: Sun Oct 26 2025 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cm4kvrtgz000808l98sqp3fr4
slug: jenkins-declarative-pipeline
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1762007109045/be1162db-b768-432e-94bb-a70023447e34.png
tags: devops, jenkins, 90daysofdevops

---

## **Table of contents**

* [Some terms for your Knowledge](https://namg.hashnode.dev/jenkins-declarative-pipeline-part-1-day26#heading-some-terms-for-your-knowledge)
    
* [Why you should have a Pipeline?](https://namg.hashnode.dev/jenkins-declarative-pipeline-part-1-day26#heading-why-you-should-have-a-pipeline)
    
* [Pipeline syntax](https://namg.hashnode.dev/jenkins-declarative-pipeline-part-1-day26#heading-pipeline-syntax)
    
* [Task-01: Jenkins "Hello World" Declarative Pipeline](https://namg.hashnode.dev/jenkins-declarative-pipeline-part-1-day26#heading-task-01-jenkins-hello-world-declarative-pipeline)
    

One of the most important parts of your DevOps and CICD journey is a Declarative Pipeline Syntax of Jenkins

### **Some terms for your Knowledge**

**<mark>What is a Pipeline</mark> -** A pipeline is a collection of steps or jobs interlinked in a sequence.

**<mark>Declarative</mark>:** Declarative is a more recent and advanced implementation of a pipeline as a code.

**<mark>Scripted</mark>:** Scripted was the first and most traditional implementation of the pipeline as a code in Jenkins. It was designed as a general-purpose DSL (Domain Specific Language) built with Groovy.

### **Why you should have a Pipeline?**

The definition of a Jenkins Pipeline is written into a text file (called a [`Jenkinsfile`](https://www.jenkins.io/doc/book/pipeline/jenkinsfile)) which in turn can be committed to a project’s source control repository.  
This is the foundation of "Pipeline-as-code"; treating the CD pipeline as a part of the application to be versioned and reviewed like any other code.

**Creating a** `Jenkinsfile` and committing it to source control provides several immediate benefits:

* Automatically creates a Pipeline build process for all branches and pull requests.
    
* Code review/iteration on the Pipeline (along with the remaining source code).
    

### Pipeline syntax

```plaintext
pipeline {
    agent any 
    stages {
        stage('Build') { 
            steps {
                // 
            }
        }
        stage('Test') { 
            steps {
                // 
            }
        }
        stage('Deploy') { 
            steps {
                // 
            }
        }
    }
}
```

## **Task-01: Jenkins "Hello World" Declarative Pipeline**

* Create a New Job, this time select Pipeline instead of Freestyle Project.
    

1. Click on "New Item" to create a new Jenkins job.
    
2. **Enter Job Details:**
    
    * Provide a name for the job (e.g., "Hello-World").
        
    * Choose "Pipeline" as the job type.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692420331590/26b5d097-287f-42a1-96b9-3f73a9294b90.png?auto=compress,format&format=webp align="left")
        
3. Add a description of this project
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692422042118/37f29434-bc80-45bd-9c54-e44da2a3d083.png?auto=compress,format&format=webp align="left")
    
4. Configure Pipeline: In the pipeline configuration section: Write declarative pipeline code
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692421507678/387cd9a1-f132-4e23-9c42-d3bc58010ad4.png?auto=compress,format&format=webp align="left")
    
5. Save pipeline configurations and run the pipeline job by clicking on "Build Now."
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692422507380/6c3f617a-454b-451e-9db6-4e0d48288d0e.png?auto=compress,format&format=webp align="left")
    
6. See the console output
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692423031496/af8d3ab4-42e3-454a-9a72-fb492b2f57a2.png?auto=compress,format&format=webp align="left")
    

> ***"Thank you for reading my blog! Happy Learning!!!😊***