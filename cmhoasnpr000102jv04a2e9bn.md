---
title: "AWS Lambda"
datePublished: Fri Nov 07 2025 03:30:31 GMT+0000 (Coordinated Universal Time)
cuid: cmhoasnpr000102jv04a2e9bn
slug: aws-lambda
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1762008903588/4275b084-de47-4fc8-82de-525a9d713ebd.png
tags: google, aws-lambda, 90daysofdevops

---

## **Table of contents**

* [**How does AWS Lambda work?**](https://bhavyabojanapalli.hashnode.dev/aws-lambda-day-6#heading-how-does-aws-lambda-work)
    
* [**AWS Lambda Main Features**](https://bhavyabojanapalli.hashnode.dev/aws-lambda-day-6#heading-aws-lambda-main-features)
    
* [**AWS Lambda Function Code**](https://bhavyabojanapalli.hashnode.dev/aws-lambda-day-6#heading-aws-lambda-function-code)
    
    * [**Use cases of Lambda:**](https://bhavyabojanapalli.hashnode.dev/aws-lambda-day-6#heading-use-cases-of-lambda)
        
    * [**Real-Time File Processing in Media Streaming:**](https://bhavyabojanapalli.hashnode.dev/aws-lambda-day-6#heading-real-time-file-processing-in-media-streaming)
        
    * [**Dynamic Image Resizing in E-commerce:**](https://bhavyabojanapalli.hashnode.dev/aws-lambda-day-6#heading-dynamic-image-resizing-in-e-commerce)
        
    * [**Real-Time Analytics for IoT Devices:**](https://bhavyabojanapalli.hashnode.dev/aws-lambda-day-6#heading-real-time-analytics-for-iot-devices)
        
    * [**Serverless Backend for Mobile Apps:**](https://bhavyabojanapalli.hashnode.dev/aws-lambda-day-6#heading-serverless-backend-for-mobile-apps)
        
    * [**Chatbot Integration in Messaging Applications:**](https://bhavyabojanapalli.hashnode.dev/aws-lambda-day-6#heading-chatbot-integration-in-messaging-applications)
        

AWS Lambda is a serverless computing service provided by Amazon Web Services (AWS). At its core, serverless computing eliminates the need for developers to manage server infrastructure, allowing them to focus solely on writing code and deploying applications. AWS Lambda takes this concept a step further by enabling developers to run code in response to events without provisioning or managing servers..

**Key Features:**

1. **Event-Driven Execution:** AWS Lambda allows developers to trigger functions in response to various events such as changes to data in an Amazon S3 bucket, updates to a DynamoDB table, or an HTTP request via Amazon API Gateway.
    
2. **Scalability:** Lambda automatically scales to handle the number of requests, ensuring optimal performance without the need for manual intervention. It can handle a few requests per day to thousands of requests per second.
    
3. **Pay-Per-Use Pricing Model:** With AWS Lambda, you only pay for the compute time that you consume. There are no upfront costs or charges when your code is not running.
    
4. **Multilanguage Support:** Lambda supports multiple programming languages, including Node.js, Python, Java, C#, and more, giving developers the flexibility to choose the language that best suits their application.
    
5. **Integrated Security:** AWS Lambda integrates with AWS Identity and Access Management (IAM) to control access to functions and resources. Developers can define IAM roles and permissions to manage who can invoke functions and what resources they can access.
    

### **How does AWS Lambda work?**

We said that AWS Lambda fits into the event-driven architectures. So, As you can see in the image AWS Lambda triggers by event and executes the function code.

![](https://miro.medium.com/v2/resize:fit:1400/1*N-0DBILWoiNUZsTcWQPmFg.png align="left")

Each Lambda function runs in its container. You can think of every lambda function as a standalone docker container. When a function is created, Lambda packages it into a new container and then executes that container on a multi-region cloud cluster of servers managed by AWS.

Before the functions start running, each function’s container is allocated its necessary RAM and CPU capacity that parameters are configurable in aws lambda.

When the functions finish running, there is a calculation; the allocated RAM and the function execution time are multiplied and the calculated charged cost is to the customer. So that means customers are charged based on the allocated memory and the amount of execution time the function finished.

AWS Lambda’s entire infrastructure layer is managed by AWS. Clients don’t get a lot of visibility into how the system is running, but they’re also don’t need to know about underlying machines, network contention, etc. They don’t have to worry about things like; AWS handles this itself.

Using AWS Lambda can save you time on operational tasks because the service is fully managed.

When there is no infrastructure to maintain, you can spend more time on application code and your actual business logic; that means it gives up flexibility about your infrastructure.

### **AWS Lambda Main Features**

As you remember we said that Lambda is a compute service that lets you run code without provisioning or managing any servers.

Now let's focus on AWS Lambda Main Features.

![](https://miro.medium.com/v2/resize:fit:1400/1*JUIVrWomojb0IZ1B0JF06A.png align="left")

**Cost Saving with Pay-as-you-go model**  
Customers are charged based on the allocated memory and the amount of execution time the function finished. You only pay for the compute time and there is no charge when your code is not running.

**Event-driven Architecture with Lambda**  
Lambda is an on-demand computing service that runs custom code in response to events. Most AWS services generate events, and many can act as an event source for Lambda.

**Scalability and Availability**  
With Lambda, you can run code for virtually any type of application or backend services all with zero administration. Upload your code run your code and scale your code automatically with high availability.

**Supports Multiple Languages and Frameworks**  
Lambda has native support for many programming languages including Java, Go, PowerShell, Node.js, C#, Python, and Ruby code, and provides a Runtime API that lets you use any additional programming language to write your functions.

# AWS Lambda Function Code

Lambda runs instances of your function to process events. You can invoke your function directly using the Lambda API, or you can configure an AWS service or resource to invoke your function. You can find the image below, which is a Lambda function with the Node.js version.

![](https://miro.medium.com/v2/resize:fit:1400/1*uttVOuykAG9OcM0Fg6Jinw.png align="left")

[**serverless-stack.com/chapters/what-is-aws-l..**](https://serverless-stack.com/chapters/what-is-aws-lambda.html)

As you can see we have a lambda function name that we export in the handler method. We have defined a function that has event, context and callback parameters. With these parameters, we develop our actual business function and after that perform some callback operations that can return a response or not respond for async invocations.

So Lambda function has code to process the events that you pass into the function or that other AWS services send to the function with an event JSON object. The event object contains all the information about the event that triggered this Lambda. For example, if lambda invokes from API gateway, then an HTTP request will be the information of the event.

The context object contains info about the runtime our Lambda function is executing in. After we do all the work inside our Lambda function, we simply call the callback function with the results or the error and AWS will respond to the HTTP request with it.

### **Use cases of Lambda:**

### **Real-Time File Processing in Media Streaming:**

Consider a media streaming platform that allows users to upload videos. AWS Lambda can be employed to process these video uploads in real-time. As soon as a user uploads a video file to an S3 bucket, a Lambda function is triggered. This function can automatically transcode the video into various formats suitable for different devices, generate thumbnails, and update the database with the necessary information. The entire process occurs seamlessly and without manual intervention, ensuring that users can start streaming their content across devices almost immediately.

### **Dynamic Image Resizing in E-commerce:**

In an e-commerce application, product images of varying sizes are often required for different parts of the website or mobile app. AWS Lambda can be utilized to dynamically resize and optimize images on the fly. When a new image is uploaded to an S3 bucket, a Lambda function triggered by the S3 event can resize the image to different dimensions and store the resized versions back in the bucket. This ensures that the application always serves appropriately sized images, optimizing both user experience and bandwidth usage.

### **Real-Time Analytics for IoT Devices:**

For an Internet of Things (IoT) application, where devices generate a continuous stream of data, AWS Lambda can be used to perform real-time analytics. Imagine a fleet of IoT devices collecting sensor data. Lambda functions can process this data as it arrives, performing analytics to identify patterns, anomalies, or trigger alerts based on specific conditions. This real-time processing capability ensures that timely insights are derived from the data, enabling proactive decision-making.

### Serverless Backend for Mobile Apps:

Developing and maintaining a scalable backend for a mobile app can be challenging. AWS Lambda, in conjunction with Amazon API Gateway, can serve as a serverless backend for mobile applications. Lambda functions can handle user authentication, process database queries, and execute business logic. This architecture allows developers to focus on building the front end of the app, while AWS Lambda takes care of the backend operations. Additionally, the automatic scaling of Lambda ensures that the backend can handle varying loads, providing a seamless experience for users.

### **Chatbot Integration in Messaging Applications:**

In a messaging application, handling incoming messages and triggering appropriate actions can be achieved using AWS Lambda. When a user sends a message, a Lambda function can be invoked to analyze the message, perform relevant actions, and send responses. This serverless approach enables the application to scale effortlessly with the number of users, as Lambda functions automatically scale based on demand. Whether it's natural language processing, user authentication, or content moderation, Lambda can execute the required functionality in real time.