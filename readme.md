# CloudFront with Application Load Balancer (AWS Free Tier Project)

## Project Overview
This project demonstrates a real-world AWS web architecture where Amazon CloudFront is used as a Content Delivery Network (CDN) in front of an Application Load Balancer (ALB), which routes traffic to an EC2 instance running an Nginx web server.

The project is designed and implemented strictly within AWS Free Tier limits and follows production-style security and networking practices.

---

## Architecture
Traffic flow:

User → CloudFront → Application Load Balancer → EC2 (Nginx)

CloudFront improves performance by caching content at edge locations, while the ALB provides scalable and secure traffic routing to backend compute resources.

[Screenshot:](screenshots\architecture.png)

---

## AWS Services Used
- Amazon EC2 (t2.micro)
- Application Load Balancer
- Amazon CloudFront
- Target Groups
- Security Groups
- IAM (default roles)

---

## Implementation Details

### 1. EC2 Setup
- Launched an Ubuntu EC2 instance
- Installed and configured Nginx web server
- Verified application availability on port 80

![Screenshot:](screenshots\ec2instance.png)
![EC2 Target Group:](screenshots\targetgroup.png)

---

### 2. Application Load Balancer
- Created an internet-facing Application Load Balancer
- Configured a target group with EC2 instance
- Health check configured with path `/`
- Confirmed target status as **Healthy**

![Application Load Balancer:](screenshots\ALBmapping.png)
![Load Balancer:](screenshots\loadbalancer.png)

---

### 3. Security Configuration
Security was implemented using least-privilege principles:

**ALB Security Group**
- Allows HTTP (port 80) from the internet

**EC2 Security Group**
- Allows HTTP (port 80) only from ALB security group
- SSH (port 22) restricted to personal IP

Direct public access to EC2 is blocked.

![Screenshot:](screenshots\ec2instance.png)

---

### 4. CloudFront Distribution
- Created CloudFront distribution
- Origin set to ALB DNS name
- Origin protocol configured as **HTTP only**
- No custom domain or ACM certificate used
- Default CloudFront domain utilized

![CloudFront Console:](screenshots\cloudfrontconsole.png)

---

### 5. Testing and Validation
Validation steps performed:
- Direct EC2 public IP access blocked
- ALB DNS accessible and serving content
- CloudFront distribution accessible
- Verified CloudFront caching headers (`X-Cache`)

![CloudFront Domain:](screenshots\cloudfrontdomain.png)

---

## Issue Faced and Resolution

### Problem
CloudFront returned **504 Gateway Timeout** error.

### Root Cause
CloudFront origin protocol was set to **HTTPS only**, while the ALB was serving traffic over HTTP. CloudFront attempted to connect to port 443, which was not configured on the ALB.

### Resolution
- Changed CloudFront origin protocol to **HTTP only**
- Invalidated CloudFront cache

After correction, CloudFront successfully connected to the ALB.

![CloudFront Final:](screenshots\cloudfrontconsole.png)

---

## Cost Management
- All resources used fall under AWS Free Tier
- EC2 instance stopped when not in use
- No paid plans or premium services enabled

---

## Learnings
- Difference between viewer protocol and origin protocol in CloudFront
- Secure ALB-to-EC2 communication using security groups
- Common causes of CloudFront 504 Gateway Timeout errors
- CDN caching behavior and request flow
- Importance of protocol alignment between services

---

## Final Outcome
The project successfully demonstrates a scalable, secure, and production-style AWS architecture using CloudFront and Application Load Balancer. It reflects practical cloud design, troubleshooting ability, and cost awareness.