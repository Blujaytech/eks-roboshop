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
