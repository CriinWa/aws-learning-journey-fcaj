---
title: "Project Proposal"
date: 2026-07-31
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
# NeonFoodMap – Vinh Khanh Food Street Automated Audio Guide

## 1. Project Overview

This project builds an automated audio-guide platform for visitors to **Vinh Khanh Food Street, District 4, Ho Chi Minh City**. It helps visitors explore food and cultural locations through multimedia content triggered by location or QR codes.

| Criteria           | Value                                                                                               |
| ------------------ | --------------------------------------------------------------------------------------------------- |
| Project type       | Automated audio-guide and digital tourism platform                                                  |
| Deployment area    | Vinh Khanh Food Street, District 4, Ho Chi Minh City                                                |
| Users              | Visitors, local business partners, and administrators                                               |
| Technologies       | Frontend on S3/CloudFront; backend containers on Amazon ECS Fargate; Amazon RDS MySQL and Amazon S3 |
| AWS infrastructure | VPC across two Availability Zones, ECS Auto Scaling, and RDS Multi-AZ                               |
| Operations         | Docker, GitHub Actions, Amazon ECR, CloudWatch, and Amazon SNS                                      |

### Project Background

This proposal presents a solution for deploying the NeonFoodMap system on the Amazon Web Services (AWS) platform using a Cloud-Native architecture that meets requirements for scalability, high availability, security, and automated software release. The goal is to build a reusable infrastructure that supports repeatable deployment and standardizes DevOps operations for a Production environment.

NeonFoodMap is a food map website that allows users to search, explore, and evaluate dining locations in real time. The system integrates geographic POI search, GPS positioning, route display, location review, and Text-to-Speech for describing content, improving the experience of discovering food. The near real-time data processing and support for many concurrent users require a flexible infrastructure with high availability and ease of maintenance.

The proposed solution uses Docker and Amazon ECS Fargate; GitHub, GitHub Actions, and OpenID Connect (OIDC) to automate the Build–Test–Deploy workflow; Amazon ECR to store Docker images; Amazon RDS in a Private Subnet to protect data; Amazon S3 for static assets; and Amazon CloudWatch for monitoring. This architecture establishes a unified, secure, and scalable deployment process for subsequent development stages.

## 2. Objectives

### 2.1 Project Objectives

| # | Objective                                                                | Measure                                                                              |
| - | ------------------------------------------------------------------------ | ------------------------------------------------------------------------------------ |
| 1 | Provide automated audio guides through location or QR codes.             | Users can access POIs and play multimedia content.                                   |
| 2 | Manage POIs, audio, images, menus, and partner information in one place. | Data is managed through APIs and an admin interface.                                 |
| 3 | Deploy a scalable, secure, and monitored AWS system.                     | ECS Auto Scaling, RDS Multi-AZ, private subnets, CloudWatch, and SNS are configured. |
| 4 | Automate releases.                                                       | Images are built, pushed to ECR, and deployed to ECS through GitHub Actions.         |

### 2.2 Benefits

- **Better visitor experience:** flexible and easy-to-access multimedia guides.
- **Local business support:** a digital channel for menus, promotions, and service information.
- **Reliable operations:** separated application, data, and network layers with monitoring and alerts.
- **Easy scaling:** containerized architecture across multiple Availability Zones.

## 3. Problems to Solve

**Problem 1 — High staffing cost:** Manual guiding and content updates need staff, are hard to provide continuously, and cannot easily support multiple languages.

**Problem 2 — Infrastructure cost and complexity:** A multi-user platform needs scaling, media storage, backups, monitoring, and security. Poor design can increase operating cost or reduce performance during busy periods.

**Problem 3 — Fragmented content management:** POIs, guides, images, audio, menus, and promotions can be stored separately, making updates and quality control difficult.

### 3.1 Functional Scope

- Display maps and POIs in Vinh Khanh Food Street.
- Trigger guides with geofencing or QR codes, including audio and images.
- Support multilingual content and synchronized user history.
- Let partners update menus and promotions.
- Provide administration for POIs, content, users, and system health.

## 4. AWS Deployment Architecture

The system uses a **multi-tier architecture** on **Amazon Web Services (AWS)** and follows the **AWS Well-Architected Framework**. All infrastructure runs in **Amazon VPC (10.0.0.0/16)** in **ap-southeast-1 (Singapore)** across two Availability Zones for high availability and fault tolerance.

Static frontend content is stored in **Amazon S3 Static Website** and delivered through **Amazon CloudFront**. API requests go to an **Application Load Balancer**, then to backend containers on **Amazon ECS Fargate** in private subnets. Business data is stored in **Amazon RDS MySQL Multi-AZ**. GitHub Actions, Amazon ECR, CloudWatch, and SNS support automated deployment, monitoring, and alerts.

### 4.1 Architecture Diagrams

#### Platform Architecture

{{< event-image src="images/2-Proposal/platform_architecture.jpg" alt="AWS platform architecture" >}}

#### Edge and Service Connectivity

{{< event-image src="images/2-Proposal/edge_architecture.jpg" alt="AWS edge and service connectivity architecture" >}}

### 4.2 Architecture Components

| Layer             | Service                                    | Role                                                                           |
| ----------------- | ------------------------------------------ | ------------------------------------------------------------------------------ |
| Edge              | Amazon CloudFront                          | Delivers static content from S3 and forwards API requests to the ALB.          |
| Frontend          | Amazon S3 Static Website                   | Stores static application assets.                                              |
| Compute           | Application Load Balancer                  | Receives API requests, performs health checks, and routes traffic to ECS.      |
| Compute           | Amazon ECS Fargate                         | Runs backend containers in private subnets and scales as needed.               |
| Service discovery | AWS Cloud Map and Amazon Route 53          | Supports internal service discovery in the ECS cluster.                        |
| CI/CD             | GitHub Actions, AWS STS, Amazon ECR        | Authenticates with OIDC, builds images, stores them, and deploys new versions. |
| Data              | Amazon RDS MySQL Multi-AZ                  | Stores business data with a synchronized standby database.                     |
| Media             | Amazon S3 and VPC Endpoint for S3          | Stores media and provides private access from the application.                 |
| Security          | AWS IAM and AWS Secrets Manager            | Manages permissions and sensitive configuration.                               |
| Observability     | Amazon CloudWatch, Amazon SNS, and S3 Logs | Collects logs and metrics, sends alerts, and stores logs long term.            |

### 4.3 Main Processing Flow

1. Users access the application through CloudFront.
2. CloudFront serves the static frontend from S3.
3. Dynamic API requests go through the ALB to backend containers on ECS Fargate.
4. The backend processes requests, uses RDS MySQL, and accesses media through the S3 endpoint.
5. CloudWatch collects logs and metrics; SNS sends alerts.
6. GitHub Actions builds and pushes images to ECR, then updates the ECS service.

### 4.4 AWS Well-Architected Framework

| Pillar                 | Applied Solution                                                                 |
| ---------------------- | -------------------------------------------------------------------------------- |
| Operational Excellence | GitHub Actions CI/CD, CloudFormation, CloudWatch Logs, and SNS alerts.           |
| Security               | IAM roles with least privilege, Secrets Manager, and private subnets.            |
| Reliability            | ALB, ECS Auto Scaling, RDS Multi-AZ, two Availability Zones, and health checks.  |
| Performance Efficiency | CloudFront, ECS Fargate scaling, and S3 for media.                               |
| Cost Optimization      | ECS Auto Scaling and S3 Lifecycle.                                               |
| Sustainability         | Scale resources based on demand and stop dev environments outside working hours. |

## 5. Timeline

| Phase                                      | Worklog Schedule | Activities                                                                                                |
| ------------------------------------------ | ---------------- | --------------------------------------------------------------------------------------------------------- |
| Foundation and design                      | Weeks 1–3       | AWS account, IAM, Budgets, networking, EC2, S3, RDS, monitoring, backup, and architecture design.         |
| Development and infrastructure preparation | Week 4           | Agile planning, backend, RDS schema, Dockerfiles, CloudFormation, IAM, and security setup.                |
| Containerization and staging               | Weeks 5–6       | Frontend, backend, APIs, ECR, ECS Fargate, ALB, RDS, Auto Scaling, monitoring, and staging tests.         |
| Deployment automation                      | Week 7           | GitHub Actions OIDC, automated image build/push, task-definition updates, and rollout monitoring.         |
| Content delivery and completion            | Weeks 7–8       | CloudFront, DNS, access checks, cost and security review, testing, documentation, and final architecture. |

## 6. Budget

### 6.1 Current and Maximum Estimated Cost

| Service                            | Current Architecture Configuration                                                                          |                                    Current Monthly Cost | Maximum Estimated Monthly Cost |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------- | ------------------------------------------------------: | -----------------------------: |
| Amazon ECS (Fargate)               | Backend containers; production uses two tasks across two AZs with Auto Scaling.                             |                                        $9.86 | ~$20–35 |                                |
| Amazon RDS MySQL                   | Production Multi-AZ with primary and standby databases.                                                     |                                           $11.78 | ~$50 |                                |
| NAT Gateway, ALB, and Amazon VPC   | Two NAT Gateways and an ALB for production; the dashboard groups part of the cost under EC2–Other and VPC. |                                      $32.80* | ~$82–84 |                                |
| Amazon CloudFront                  | Static delivery from S3 and API routing.                                                                    | $0.00 (Free Tier for 1 TB) | $0.00 (Free Tier for 1 TB) |                                |
| Amazon S3                          | Static website, media, and logs.                                                                            |                                               ~$2 | ~$2 |                                |
| Amazon CloudWatch and SNS          | Logs, metrics, alarms, and email alerts.                                                                    |                                          $5.61 | ~$5–6 |                                |
| AWS Secrets Manager and Amazon ECR | Secrets and container images.                                                                               |                                               ~$2 | ~$3 |                                |
| **Total monthly cost**       | Current dashboard cost and complete production architecture.                                                |                 **$64.05** | **~$166–184** |                                |

### 6.2 Cost Optimization Strategy

- Set AWS Budgets and SNS alerts at 50%, 80%, and 100% of the monthly budget.
- Monitor NAT Gateway, ECS Fargate, RDS, and CloudWatch costs.
- Use Auto Scaling and remove unused staging resources.
- Use CloudFront caching and S3 Lifecycle rules as media and logs grow.

## 7. Risk Assessment

### 7.1 Risk Matrix

| Risk                          | Likelihood | Impact    |
| ----------------------------- | ---------- | --------- |
| AWS cost exceeds forecast     | Medium     | Medium    |
| ECS task or container failure | Medium     | Medium    |
| Database failure              | Low        | High      |
| Sensitive-data exposure       | Low        | Very high |
| Sudden traffic increase       | Medium     | Medium    |
| Incomplete logs or alerts     | Medium     | Medium    |
| Deployment failure            | Medium     | Medium    |

### 7.2 Response Plan

- Check Budgets, Cost Explorer, and SNS alerts when costs reach thresholds.
- Check CloudWatch Logs, ALB health checks, and ECS task definitions when APIs or containers fail.
- Protect and restore data through a tested backup and restore process.
- Rotate exposed secrets and review IAM permissions and deployment history.

## 9. Expected Results

* **Technical improvement:** Digitize guides and POI management, replacing manual information delivery with a multimedia platform that can be monitored, scaled, and deployed automatically on AWS.
* **Long-term value:** Create reusable content and data for other tourism locations, future user-behavior analysis, multilingual content, and local-business partnerships.

### References

- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [The First Cloud Journey](https://cloudjourney.awsstudygroup.com/)
- [AWS Documentation](https://docs.aws.amazon.com/)
