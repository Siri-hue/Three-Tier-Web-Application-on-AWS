## Three-Tier-Web-Application-on-AWS

This project walks through how I built and deployed a complete three-tier architecture—web, application, and database—on AWS. The goal was to create a secure, scalable setup using real-world best practices such as private subnets, load balancing, and managed databases.

# Overview
The application is split into three layers:

- Web Layer
This layer sits in public subnets and is exposed to the internet through an Application Load Balancer (ALB). All external traffic enters the architecture through the ALB.


- Application Layer
The core application runs on two EC2 instances placed in private subnets. These servers run Apache and PHP, and can only receive traffic from the ALB, not directly from the internet.

- Database Layer
The data is stored in an Amazon RDS MySQL instance inside private DB subnets. It’s isolated from the outside world and only accessible to the application layer.

- Services Used:
Custom VPC
Public, private app, and private DB subnets
Route tables
Internet Gateway and NAT Gateway
Security groups
EC2 (Jump Server + App Servers)
Application Load Balancer (ALB)
Target Groups and health checks
Amazon RDS (MySQL)
phpMyAdmin connected to RDS


# How the Deployment Works

- Setting Up the Network
I started with a custom VPC and created separate public, private app, and private DB subnets. After that, I added the necessary route tables and connected an Internet Gateway for public access.
A NAT Gateway was added so the private servers could install packages without exposing them to the internet.


- Configuring the Servers
A small EC2 instance in a public subnet acts as the jump server for SSH access.
Two more EC2 instances were launched in private subnets as application servers. These servers run Apache and PHP.
To test load balancing, I created simple index.html files with different messages on each server.


- Setting Up the Load Balancer
The Application Load Balancer sits in the public subnets and forwards traffic to both app servers.
Health checks were enabled to make sure the ALB only sends traffic to healthy instances.


- Database Setup
I created an RDS MySQL instance using a DB subnet group. Its security group allows access only from the app servers, not from the internet.
phpMyAdmin on the app servers was configured to connect to this RDS instance.


- Session Stickiness
phpMyAdmin requires session consistency, so ALB cookie-based stickiness was enabled.
This keeps each user’s session tied to one app server unless a failure occurs.


# How to Access the Application
Once everything is running, the application can be accessed using the DNS name of the ALB.
If phpMyAdmin is configured, it can be opened at /phpmyadmin through the same ALB endpoint.

# Highlights
- Fully isolated application and database layers
- Secure traffic flow through load balancer and jump server
- Private subnets with NAT for safe outbound access
- Clean three-tier separation that mirrors real industry setups
- Multi-AZ architecture for better availability


# Possible Improvements
- Add Auto Scaling for the application layer
- Introduce CloudFront and Route 53
- Containerize the application using ECS/EKS
- Move toward serverless options where possible

  
