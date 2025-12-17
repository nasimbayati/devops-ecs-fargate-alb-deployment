# DevOps TIO2 — ECS Fargate Deployment with Load Balancer & Auto-Healing

## Overview
This project demonstrates a **production-style container deployment on AWS** using **Amazon ECS with Fargate**, fronted by an **Application Load Balancer (ALB)** and backed by **auto-healing services**.

The application is a **Spring Boot REST service** packaged as a Docker image and stored in **Amazon ECR**, then deployed to a **high-availability ECS service** running **two Fargate tasks**.

This lab builds directly on TIO1 (containerization & ECR) and focuses on **orchestration, scalability, and resiliency**.

---

## What This Project Demonstrates
- Container orchestration using **Amazon ECS (Fargate)**
- **Serverless containers** (no EC2 management)
- **Application Load Balancer** integration
- **High availability** with multiple tasks
- **Auto-healing** via ECS service desired count
- Secure image storage in **Amazon ECR**
- Real production traffic flow (not direct container access)

---

## Architecture

![ECS Fargate Architecture](docs/architecture-tio2-ecs-fargate.png)


## Technologies Used
- **AWS ECS (Fargate)**
- **Application Load Balancer (ALB)**
- **Amazon ECR**
- **Docker**
- **Spring Boot**
- **IAM (Task Role & Execution Role)**
- **Security Groups & Target Group Health Checks**

---

## Deployment Details

### Container Image
- **Repository:** `helloworld`
- **Image URI:** 207813898654.dkr.ecr.us-east-1.amazonaws.com/helloworld:1.0

---

### ECS Configuration
- **Cluster:** `devopscluster`
- **Task Definition:** `devopstask:1`
- **Service:** `devopsservice`
- **Desired Tasks:** `2`
- **Launch Type:** Fargate

---

### Networking
- **Load Balancer:** `devopslb`
- **Target Group:** `Devopstg`
- **Container Port:** `8080`
- **Inbound Rules:**
- **80/tcp** — Public HTTP traffic to the ALB
- **8080/tcp** — ALB → ECS tasks (health checks + app traffic)

---

## Application Access
The application is accessed **only via the Application Load Balancer**, not directly via containers or an EC2 public IP.

**ALB DNS Name:** http://devopslb-656375846.us-east-1.elb.amazonaws.com/

---

**Expected Response:** Hello from Spring Boot on EC2 - DevOps TIO1!
---

## Health Checks & Auto-Healing
- **Target Group health checks** confirm both tasks are reachable and healthy.
- The **ECS Service maintains Desired Count = 2**.
- When a task is stopped manually, ECS automatically launches a replacement to return to **2 running tasks**.

---

## 📸 Deployment Evidence

All screenshots are organized in `/screenshots/tio2` and demonstrate:
- ECS cluster created and **ACTIVE**
- Task definition `devopstask:1` created
- Service `devopsservice` running with **2 tasks**
- Security group inbound rules showing **80** and **8080**
- Target group `Devopstg` showing **2 healthy targets**
- Load balancer `devopslb` DNS name visible
- Browser proof of app response via ALB DNS
- Auto-healing proof after stopping one task

---

## Resume-Ready Highlights
- Deployed a containerized Spring Boot application using **AWS ECS with Fargate**
- Implemented **load-balanced high availability** with an Application Load Balancer and **2 running tasks**
- Stored and deployed images from **Amazon ECR**, configured IAM roles, and enforced network access via security groups
- Validated **health checks** and demonstrated **auto-healing** by replacing failed/stopped tasks

---

## Next Steps
- Proceed to **TIO3**: mutable vs immutable deployments
- Roll out new versions using **task definition revisions**
- Add CI/CD using **CodeCommit / CodeBuild / CodeDeploy**

---

## Author
Nasim Bayati  
DevOps & Cloud Engineering (AWS)