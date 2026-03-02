# Highly-Available-Web-Application-EC2-ALB-ASG-
Highly Available Web Application on AWS
This project demonstrates how to deploy a Highly Available and Auto-Scaling Web Application using core AWS services.

The goal was to simulate a real-world production architecture where:

The application remains available even if one server fails

Traffic is distributed across multiple instances

Infrastructure automatically scales based on demand

Servers are secured inside private subnets

🎯 Why This Project Exists

Modern applications must:

Handle unpredictable traffic

Avoid single points of failure

Remain online during AZ outages

Scale horizontally

Follow security best practices

This project implements those principles using AWS infrastructure components.

Internet
   ↓
Application Load Balancer (Public Subnets)
   ↓
Target Group
   ↓
Auto Scaling Group
   ↓
EC2 Instances (Private Subnets, Multi-AZ)
Infrastructure Components
1️⃣ VPC (Virtual Private Cloud)

Purpose:
Provides network isolation and full control over IP addressing and routing.

Configuration:

CIDR: 10.0.0.0/16

DNS hostnames enabled

2️⃣ Subnet Design
Public Subnets (for ALB)
Subnet	CIDR	AZ
Public-1	10.0.1.0/24	AZ1
Public-2	10.0.2.0/24	AZ2

Auto-assign Public IP enabled

Private Subnets (for EC2)
Subnet	CIDR	AZ
Private-1	10.0.3.0/24	AZ1
Private-2	10.0.4.0/24	AZ2

Why?

Public resources exposed to internet

Application servers protected inside private network

3️⃣ Internet Gateway (IGW)

Purpose:
Allows public subnets to communicate with the internet.

Route Table (Public):

0.0.0.0/0 → Internet Gateway
4️⃣ NAT Gateway

Purpose:
Allows private EC2 instances to access internet (for updates) without being publicly exposed.

Route Table (Private):

0.0.0.0/0 → NAT Gateway
5️⃣ Security Groups (Firewall Layer)
ALB Security Group
Rule	Value
Inbound	HTTP 80 from 0.0.0.0/0

Allows internet traffic to Load Balancer.

EC2 Security Group
Rule	Value
Inbound	HTTP 80 from ALB Security Group

Prevents direct access to EC2 from internet.

6️⃣ Launch Template

Standardized EC2 configuration:

AMI: Amazon Linux 2023

Instance Type: t3.micro

Security Group: ec2-sg

User Data Script:

#!/bin/bash
dnf install -y httpd
systemctl start httpd
systemctl enable httpd
echo "Healthy from $(hostname)" > /var/www/html/index.html

Purpose:
Automates web server installation at launch.

7️⃣ Auto Scaling Group (ASG)

Configuration:

Desired Capacity: 2

Minimum: 2

Maximum: 4

Multi-AZ deployment

Attached to Target Group

Purpose:

Ensures high availability

Replaces unhealthy instances

Scales automatically during traffic spikes

8️⃣ Application Load Balancer (ALB)

Configuration:

Internet-facing

Listener: HTTP (Port 80)

Forwarding to Target Group

Purpose:

Distributes traffic evenly

Performs health checks

Routes only to healthy instances

🧪 Validation

Accessed ALB DNS and confirmed:

Healthy from ip-10-0-141-33

Refreshing the page returned different hostnames, confirming:

Load balancing working

Multi-AZ deployment active

🔥 High Availability Features

✔ Multi-AZ deployment
✔ No single point of failure
✔ Auto-healing via ASG
✔ Horizontal scaling
✔ Secure private backend servers

📚 Key Concepts Demonstrated

VPC Networking

Public vs Private Subnets

Route Tables & NAT

Security Groups

Launch Templates

Auto Scaling Groups

Application Load Balancer

Multi-AZ High Availability Architecture

🚀 Production Principles Applied

Infrastructure Isolation

Defense-in-Depth Security

Elastic Scaling

Fault Tolerance

Automated Provisioning

🏁 Outcome

Successfully deployed a scalable, secure, and highly available web application architecture using AWS best practices.

This project represents real-world cloud infrastructure design used in production environments.

<img width="737" height="487" alt="image" src="https://github.com/user-attachments/assets/7bd72733-04af-410f-a5e5-52e887a1f5a8" />
<img width="601" height="484" alt="image" src="https://github.com/user-attachments/assets/529597c1-413d-4451-b99b-8937ba93d871" />
<img width="626" height="493" alt="image" src="https://github.com/user-attachments/assets/b36d8374-b0eb-4c6e-815b-423e8b8393b6" />
<img width="695" height="486" alt="image" src="https://github.com/user-attachments/assets/3d22dfc7-f6f2-42af-930a-3d98cd93a86f" />









