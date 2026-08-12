---
title: "VPC — Network"
date: 2026-08-11
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---
# Deploy the Network Infrastructure (VPC)

**Goal:** Build a secure network for the Law-Chatbot project (Vietnamese Legal LLMOps). Isolate RDS PostgreSQL (pgvector) and Lambda in private subnets, while allowing EC2 (Streamlit and FastAPI) to serve users from a public subnet.

![1786457559140](image/_index.vi/1786457559140.png)

### Communication flow (Network & Security Groups)

![1786474843338](image/_index.vi/1786474843338.png)

---

## 1. Create the core VPC (VPC and more)

This wizard automatically creates the VPC, subnets, route tables, and Internet Gateway.

* In the AWS services menu, search for and open the **VPC Dashboard**, then choose **Create VPC**.

![1786456192770](image/_index.vi/1786456192770.png)

* Under **Resources to create**, choose **VPC and more** so AWS generates the related components.
* In **Name tag auto-generation**, enter `law-chatbot`.
* Under **IPv4 CIDR block**, keep the default `10.0.0.0/16`.

![1786457003752](image/_index.vi/1786457003752.png)

* Under **Number of Availability Zones (AZs)**, choose **2** for high availability.
* Under **Number of public subnets**, choose **2** (for EC2 and ALB later).
* Under **Number of private subnets**, choose **2** (for RDS and Lambda).

![1786457055669](image/_index.vi/1786457055669.png)

* Under **NAT gateways ($)**, choose **None** to reduce cost (VPC Endpoints can be used later).
* Under **VPC endpoints**, choose **S3 Gateway** so resources in private subnets can reach S3 without going to the Internet.

![1786457107099](image/_index.vi/1786457107099.png)

* On the right-hand preview, review the network that will be created.
* Scroll to the bottom and choose **Create VPC**.

![1786457331449](image/_index.vi/1786457331449.png)

* Wait until creation finishes and the console reports success.

![1786457367494](image/_index.vi/1786457367494.png)

---

## 2. Configure Security Groups (3-layer firewall)

Security Groups follow least privilege: each component only accepts traffic on the ports it needs.

### 2.1. Create the EC2 Security Group (`law-chatbot-ec2-sg`)

* In the left menu of the VPC Dashboard, choose **Security Groups**, then **Create security group**.

![1786457709037](image/_index.vi/1786457709037.png)

* Enter `law-chatbot-ec2-sg` as **Security group name** and add a matching description.
* Under **VPC**, remove the default VPC and select `law-chatbot-vpc`.

![1786458009881](image/_index.vi/1786458009881.png)

* Under **Inbound rules**, choose **Add rule** and add three rules: port `8501` (Custom TCP) for Streamlit, port `8000` (Custom TCP) for FastAPI, and port `22` (SSH) from the admin IP.

![1786467181298](image/_index.vi/1786467181298.png)

* Choose **Create security group**.

![1786458261997](image/_index.vi/1786458261997.png)

### 2.2. Create the Lambda Security Group (`law-chatbot-lambda-sg`)

* Return to Security Groups and choose **Create security group** again.

![1786467284205](image/_index.vi/1786467284205.png)

* Enter `law-chatbot-lambda-sg` and select VPC `law-chatbot-vpc`.
* Skip inbound rules (add none) because Lambda does not accept inbound connections. Keep **Outbound rules** as the default All traffic.

![1786467354605](image/_index.vi/1786467354605.png)

* Choose **Create security group**.

![1786463859232](image/_index.vi/1786463859232.png)

![1786467430802](image/_index.vi/1786467430802.png)

### 2.3. Create the RDS Security Group (`law-chatbot-rds-sg`)

* Choose **Create security group**.

![1786461235842](image/_index.vi/1786461235842.png)

* Enter `law-chatbot-rds-sg` and select VPC `law-chatbot-vpc`.
* Under **Inbound rules**, choose type PostgreSQL (port 5432). For **Source**, choose **Custom** and allow `law-chatbot-ec2-sg` and `law-chatbot-lambda-sg`.

![1786467628217](image/_index.vi/1786467628217.png)

* Choose **Create security group** to finish the security layout.

![1786467688049](image/_index.vi/1786467688049.png)

---

## 3. VPC Endpoints

In the current lab deployment, the team has not configured VPC Endpoints. The system uses a simplified setup where EC2 can reach the Internet directly and call AWS services such as S3, Bedrock, or DynamoDB through the AWS SDK. VPC Endpoints are therefore not required for the system to run at this stage.

VPC Endpoints become relevant when you move to a fully private network. If EC2, Lambda, or workers sit in private subnets without a NAT Gateway, they cannot reach AWS public endpoints. You then create VPC Endpoints so traffic stays on the AWS private network instead of going to the Internet.

Skipping VPC Endpoints does not change application logic. The code calls AWS through `boto3`, for example `boto3.client("s3")` or `boto3.client("bedrock-runtime")`. A VPC Endpoint is a network setting, not an application module. Once private DNS and route tables are correct, SDK calls go through the endpoint with no code change.

**Conclusion:** For this project, VPC Endpoints are a production-oriented security and networking improvement. They reduce dependence on the Internet/NAT Gateway, keep traffic inside AWS, and support a fully isolated layout for RDS, Lambda, and the backend.
