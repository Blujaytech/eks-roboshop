# Project: RoboShop E-Commerce Platform on AWS EKS with DevSecOps

### Project Overview

RoboShop is a cloud-native, microservices-based e-commerce application deployed on Amazon EKS (Elastic Kubernetes Service). The platform simulates a real-world online shopping application where users can browse products, manage carts, place orders, and process payments. Each business capability is implemented as an independent microservice, enabling scalability, fault isolation, and independent deployments. The application includes multiple backend databases and messaging components to support distributed transactions and asynchronous communication.

# Tech Stack
1. Aws (EC2-Servers-1)
2. AWS Cli + IAM (Role) + Docker + EKS.
3. Database: mysql,mongodb,redis,rabbitmq.
4. Frontend-server..Nginx Package
5. Backend-server..Nodejs,maven,Golanguage,python.

# roboshop-documentation

Below is the communication between components and dependency. This dependency comes from **Development team**. Architects decide that, DevOps has no scope in this.

<img width="731" height="561" alt="image" src="https://github.com/user-attachments/assets/c4a46e79-03f9-4f3e-815d-3d2391a1961f" />

🚀 RoboShop Deployment on Amazon EKS — A Practical, Production-Style Walkthrough
Deploying a microservices-based application like RoboShop on Kubernetes is one of the best ways to demonstrate real-world DevOps capability—especially on Amazon Web Services using Amazon EKS.
This guide walks you through a clean, dependency-aware deployment using Kubernetes manifests—exactly how you’d approach it in a professional environment.

🧠 Understanding What You’re Deploying
RoboShop is a microservices e-commerce application, where each component runs independently but depends on shared backend services like databases, caches, and messaging systems.
Start by pulling the deployment manifests:
```
git clone https://github.com/

cd k8-roboshop/
```
📦 Step 1: Create a Dedicated Namespace

In Kubernetes, namespaces help isolate environments and organize resources logically.

```
kubectl apply -f 01-namespace.yaml
```

This creates a roboshop namespace where all application components will live.

🧱 Step 2: Deploy Backend Dependencies (Critical Order)

Before deploying application services, you must bring up the foundational components they rely on.
```
kubectl apply -f mongodb/manifest.yaml
kubectl apply -f redis/manifest.yaml
kubectl apply -f rabbitmq/manifest.yaml
kubectl apply -f mysql/manifest.yaml
```
These services act as the backbone:

MongoDB → Catalog data
Redis → Caching layer
RabbitMQ → Message broker
MySQL → Transactional data

⚠️ Order matters here. If these aren’t ready, your application pods will fail or crash-loop.

⚙️ Step 3: Deploy Application Microservices

Once dependencies are up, deploy the actual business services:
```
kubectl apply -f catalogue/manifest.yaml
kubectl apply -f user/manifest.yaml
kubectl apply -f cart/manifest.yaml
kubectl apply -f shipping/manifest.yaml
kubectl apply -f payment/manifest.yaml
kubectl apply -f frontend/manifest.yaml
```
Each service represents a specific domain in the application:

Catalogue → Product listing
User → Authentication & profiles
Cart → Session-based cart logic
Shipping & Payment → Order processing
Frontend → UI layer exposed to users

🔍 Step 4: Verify Deployment Health

Check whether all pods are successfully scheduled and running:
```
kubectl get pods -A
```
You’re aiming for:

STATUS: Running
Minimal or no restarts

If something is off, inspect logs:
```
kubectl logs <pod-name>
```
🌐 Step 5: Identify Access Endpoint

To access the application, retrieve the service details:
```
kubectl get svc -A
```
Look for the LoadBalancer service associated with the frontend. This will expose an external URL via AWS Elastic Load Balancer.

⚡ Step 6: Improve Workflow with Default Namespace

Avoid repeatedly specifying the namespace:
```
kubectl config set-context --current --namespace=roboshop
```
Now you can run simplified commands:
```
kubectl get pods
kubectl get svc
```
🌍 Application Access

Once everything is up, your application is live and accessible:
```
http://a690ac2db9d9544d7bf2a2c801446d0f-2116354069.ap-south-1.elb.amazonaws.com
```



![alt text](roboshop.jpg)

### WEB TIER:
* Usually web tier is the one which has frontend technologies like HTML, CSS, Java Script (React/Angular/Node).
* We use web server to deploy these kind of applications.
* Earlier Apache Server was popular, Now Nginx is the most popular web server.

### APP TIER:
* APP/API Tier is the one which has backend technologies like Java, .NET, Python, Go, Php, etc.
* Earlier Backend technologies had servers like tomcat, Jboss, IIS, etc.
* Now all backend technologies are coming up with in built servers.
* Usually API tier should not opened through internet, it should be only accessible through web tier.

### DB TIER:
* Storage of the applications will be here like user data, products, orders data, etc.
* We can use RDBMS like MySQL, MSSQL, Postgress, etc for row and column based data.
* We can use NoSQL databases like MongoDB for storing the product information.
* We can use Cache servers like Redis to access the data with lightening speed.
* We can use MQ Servers like RabbitMQ, ActiveMQ, Kafka, etc for asynchronous communication.
