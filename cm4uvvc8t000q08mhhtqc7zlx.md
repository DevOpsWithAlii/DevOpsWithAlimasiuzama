---
title: "Microservices-k8's flask-app Deployment on kubeadm"
datePublished: Tue Aug 19 2025 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cm4uvvc8t000q08mhhtqc7zlx
slug: microservices-k8s-flask-app-deployment-on-kubeadm
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1760877875214/f39a9d0c-0236-49cd-9896-4b74d61fea3d.png
tags: kubernetes, devops, 90daysofdevops

---

## **Table of contents**

* [Setting up Kubernetes Cluster using Kubeadm](https://namg.hashnode.dev/microservices-k8s-flask-app-deployment-on-kubeadm#heading-setting-up-kubernetes-cluster-using-kubeadm)
    
* [Building image of flask-app](https://namg.hashnode.dev/microservices-k8s-flask-app-deployment-on-kubeadm#heading-building-image-of-flask-app)
    
* [Deployment of MongoDB in Kubernetes Cluster](https://namg.hashnode.dev/microservices-k8s-flask-app-deployment-on-kubeadm#heading-deployment-of-mongodb-in-kubernetes-cluster)
    
* [Deployment of taskmaster in kubernetes cluster](https://namg.hashnode.dev/microservices-k8s-flask-app-deployment-on-kubeadm#heading-deployment-of-taskmaster-in-kubernetes-cluster)
    

### Setting up Kubernetes Cluster using Kubeadm

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693217815309/ba77e48a-a6a2-409a-8d07-0bf90eabd820.png?auto=compress,format&format=webp align="left")

Master and Worker nodes created

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693218080931/d38cc7b6-e0b7-478e-8e5a-f1bf472c066d.png?auto=compress,format&format=webp align="left")

You can verify the same on the worker node You will see taskmaster container is running

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693219004316/869b9d27-b919-43b6-84f2-ce2b9c1424e5.png?auto=compress,format&format=webp align="left")

### Building image of flask-app

clone microservices k8's app: [**https://github.com/LondheShubham153/microservices-k8s.git**](https://github.com/LondheShubham153/microservices-k8s.git)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694004365967/db69b0ca-a1ca-4e0a-9eed-ca75cd447c64.png?auto=compress,format&format=webp align="left")

Building image from Dockerfile

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694004262843/18ab1157-88a0-4577-893f-b015701c846d.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694004471154/54290718-a489-462a-8671-973acef66167.png?auto=compress,format&format=webp align="left")

Login to Docker Hub:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694004739602/f64c5289-0f5b-46d8-bbce-9cecfa1b92c0.png?auto=compress,format&format=webp align="left")

Pushing image to DockerHub:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694004833059/c1ea8662-3b08-48de-98c7-4d095eebea3a.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694004897768/57754a9e-ab57-4adc-bd02-b6036c0dea29.png?auto=compress,format&format=webp align="left")

Create a namespace for this project and deploy manifest files under this namespace one by one:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694005398739/40e6f68d-3eb8-4020-9063-7fb0987b31bc.png?auto=compress,format&format=webp align="left")

### Deployment of MongoDB in Kubernetes Cluster

\-Integrate Persistent volume and Persistent volume claim  
Create Persistent volume:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694051881198/ab635f8f-b13c-4b20-b1a4-12a4d4444400.png?auto=compress,format&format=webp align="left")

Create a folder on Workernode at specified path: /home/ubuntu/mongodata before creating a mongo persistent volume:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694051809572/9b2a5d13-b0e5-447e-bebf-3f37da36dc7a.png?auto=compress,format&format=webp align="left")

Claim Persistent volume:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694007002177/e6366f6a-68a2-4bde-a5fc-0651605bf9a9.png?auto=compress,format&format=webp align="left")

Deploy mongo-db

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694007053415/ece1ce70-22b2-406d-a27f-7389d6987174.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694007250588/204e9270-7b4b-4e02-9022-b8f584758a18.png?auto=compress,format&format=webp align="left")

The mongo-db pod is running:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694007368525/ad42121c-f437-411c-b733-9392e3ed28ec.png?auto=compress,format&format=webp align="left")

Mongo-svc file:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694007610003/6a6b5ba2-5d0b-4670-952d-e0a6fa207d3f.png?auto=compress,format&format=webp align="left")

Open port 30007 on the worker node in the security group:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694008068777/48948b36-c1a0-4ede-881f-fe5560f5fd2b.png?auto=compress,format&format=webp align="left")

### Deployment of taskmaster in kubernetes cluster

taskmaster.yml file is the deployment file.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694005231421/88dd50c9-19a2-446d-9113-74e396cddfce.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694005613232/034c5ae7-3abc-4968-9e60-95dcbf953447.png?auto=compress,format&format=webp align="left")

taskmaster-svc.yml

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694005681036/b732cadd-78d1-4ac5-8149-ff29b0e3e7a2.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694005809653/caafb798-2bca-40fc-82cb-b503c4f61556.png?auto=compress,format&format=webp align="left")

Open port 30007 on the worker node in the security group:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694005911708/4ae42363-9393-4a57-9ad7-bd1552d1feae.png?auto=compress,format&format=webp align="left")

You can take the worker node IP and check taskmaster has deployed successfully. taskmaster microservice has deployed successfully in the Kubeadm cluster:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694006064711/1ac4a17c-7042-440e-b398-aa65f02188a2.png?auto=compress,format&format=webp align="left")

Testing my flask app

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694053101865/acce6b52-bc9d-46a4-a5d5-55e5c46b31cc.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694051618902/9a962d91-b9c9-400a-afc1-077abe0c24e5.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694008218773/c39024d8-94bd-459f-a02b-9d6c2e481718.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694051587166/74d161b8-a7ae-4275-9e4d-f690bdc83fbd.png?auto=compress,format&format=webp align="left")

**You have successfully Deployed Flask app with MongoDB on kubeadm Kubernetes cluster.💥**

> ***"Thank you for reading my blog! Happy Learning!!!😊***