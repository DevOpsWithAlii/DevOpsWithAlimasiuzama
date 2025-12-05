---
title: "Hands-On: Installing Kubernetes Tools"
datePublished: Wed Dec 03 2025 16:00:24 GMT+0000 (Coordinated Universal Time)
cuid: cmiq715qc000102l8gcft6g4h
slug: hands-on-installing-kubernetes-tools
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1764777543031/2f60ce10-9937-4415-8860-76167069e1c9.png
tags: kubernetes, k8s, kind

---

## Installed & Configured everything step-by-step.

## **Install Docker**

```bash
# Install docker 
sudo apt install docker.io
sudo usermod -aG docker $USER newgrp docker
```

## **Install Kind**

```bash
# For AMD64 / x86_64
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.30.0/kind-linux-amd64

chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind

# kind version check
kind --version
```

## **Install kubectl**

```bash
# X86-64
   curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

# Validate the binary (optional)
# Download the kubectl checksum file
   curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"

# Validate the kubectl binary against the checksum file:
echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check

# If valid, the output is:
kubectl: OK

# Install Kubectl now 
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# If you do not have root access on the target system,
chmod +x kubectl
mkdir -p ~/.local/bin
mv ./kubectl ~/.local/bin/kubectl
```

# **Creating Kubernetes Cluster Using Kind**

```bash
# make a working directory where u will execute all the work in k8s
mkdir k8s-prectice
cd k8s-prectice
```

Default cluster / config.yml

```bash
# vim config.yml

kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker

# create clusters
kind create cluster --name=my-cluster  --config=config.yml
```

```bash
# Check clusters
kind get clusters

# Check Kubernetes nodes
kubectl get nodes

# Delete cluster
kind delete cluster --name=my-cluster

# Check internal Docker containers
docker ps
```

# Create a Namespace.yml file

```bash
# vim namesapce.yml
apiVersion: v1
kind: Namespace
metadata:
  name: ngin-n
  labels:
    environment: dev

# making namespace
kubectl apply -f namespace.yml

# Check namespaces
kubectl get ns -n nginx-n

# delete ns
kubectl delete ns namespace.yml -n nginx-n
```

# **Create Pod**

```bash
# vim my-pod.yml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx-container
    image: nginx:latest
    ports:
    - containerPort: 80

# create a pods
kubectl apply -f my-pod.yml -n nginx-n 

# check pods 
kubectl gets pod -n nginx-n

# delete pods
kubectl delete pod {pod name} -n nginx-n
```

# Create Deployment file

```bash
# vim deployment.yml
apiVersion: apps/v1
kind: Deployment
metadata:      
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80

# metadata= Information to help uniquely identify the object, including a name and labels
# spec= The desired state of the Deployment [1].
# replicas=	The number of desired pods to maintain [1].
# selector=	Defines how the Deployment finds which pods to manage (must match the template's labels) [1].
# template=	The blueprint for the pods that will be created [1].
# spec.containers= A list of containers that will run inside the pod [1].

# execute deployment.yml file
kubectl apply -f deployment.yml -n nginx-n

# check pods/replicas
kubectl get pods -n nginx-n

# delete pods
kubectl delete pod {pod name} -n nginx-n
```

## **Scale the Pods/replicasets**

You can manually scale the number of pods in a Deployment using two methods:

```bash
# methods 1: Using the kubectl scale command
kubectl scale deployment/nginx-deployment -n nginx-n --replicas=5 

# check pods/replicas
kubectl get pods -n nginx-n     # -n nginx-n  = namespace

# Editing the YAML file and reapplying
# Edit the nginx-deployment.yaml file to set 'replicas: 5'
kubectl apply -f nginx-deployment.yaml
```

## **Managing Rollouts (Updates)**

**Updating the container image (triggering a new rollout):**  
You can update the image version directly using a command, which is one way to trigger an update without editing the YAML file directly.

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.16.1 --record=true

# Monitoring the rollout status
kubectl rollout status deployment/nginx-deployment
# Viewing rollout history
kubectl rollout history deployment/nginx-deployment
```

## **Performing a Rollback**

```bash
# rollback
kubectl rollout undo deployment/myapp-deployment
```

# **Service**

Expose an application running in your cluster behind a single outward-facing endpoint, even when the workload is split across multiple backends.

In Kubernetes, a Service is a method for exposing a network application that is running as one or more [Pods](https://kubernetes.io/docs/concepts/workloads/pods/) in your cluster.

```bash
apiVersion: v1
kind: Service
metadata:
  name: my-service
  namespace: nginx-n
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80

# give To disable all privileged ports, set this to 0.
sudo sysctl -w net.ipv4.ip_unprivileged_port_start=0   # only for nginx will work.

# Now Expose on Browser ports
kubectl port-forward service/nginx-service -n nginx-n 80:80 --address=0.0.0.0
```