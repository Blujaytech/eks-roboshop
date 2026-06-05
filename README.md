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
