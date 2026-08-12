---
title: "Summary"
date: 2026-08-11
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---
# Workshop Summary

## 5.5.1. Results

After completing the workshop, Law-Chatbot includes:

- A legal Q&A chatbot built on **RAG**
- **Streamlit** frontend with Login, Register, Chatbot, and Admin screens
- **FastAPI** backend with `/ask` and full `/api/*` routes
- **RDS PostgreSQL + pgvector** for vector search
- **Amazon Bedrock** for cloud embedding/LLM
- Document upload and ingestion through **S3 → SQS → Lambda**
- Conversation history in **RDS PostgreSQL**
- **Cognito** authentication/authorization for admin APIs
- A **CloudFormation** template for foundation resources
- Production-ready deploy with **Docker Compose** on EC2

## 5.5.2. Actual usage cost

The table below summarizes AWS cost for Law-Chatbot over one month (12/07/2026 to 11/08/2026).

*(Note: charges vary with instance uptime and query volume. Some services stay within Free Tier and incur no charge.)*

| Service | Cost (USD) | Notes |
| :------------------------- | :---------------- | :--------------------------------------------------------------------------------------------- |
| **Amazon EC2** | $ 8.50 | Running a `t3.micro` instance (billed by actual hours). |
| **Amazon RDS** | $ 16.20 | Highest lab cost. Includes `db.t4.micro` and 20GB gp3 storage. |
| **Amazon Bedrock** | $ 1.45 | Billed by input/output tokens for Claude 3 Sonnet and Titan Embedding. |
| **Amazon S3** | $ 0.05 | Storage for PDF/TXT files plus PUT/GET requests. |
| **AWS Lambda** | $ 0.00 | Within Free Tier (1 million requests/month). |
| **Amazon SQS / SNS** | $ 0.00 | Test traffic stayed within Free Tier. |
| **Amazon Cognito** | $ 0.00 | Free for the first 50,000 MAU. |
| **Amazon VPC** | $ 0.00 | No NAT Gateway, so no expensive network charges. |
| **Total** | **$ 26.20** | *(From AWS Billing & Cost Management Dashboard)* |

---

## 5.5.3. Cleanup guide

To avoid unexpected charges after the workshop, delete resources. **Important rule:** delete from the outside in to avoid dependency errors.

**Step 1: Clean up the server (EC2)**

* Open **EC2 Console** → choose **Instances** → select instance **law-chatbot-key** → **Instance state** → **Terminate instance**.

![1786478840396](image/_index.vi/1786478840396.png)

**Step 2: Delete the database (RDS)**

* Open **RDS Console** → choose **Databases** → select `vector-db-server` → **Actions** → **Delete**.

![1786479013493](image/_index.vi/1786479013493.png)

**Step 3: Delete accounts and permissions (IAM)**

* Open **IAM Console** → **Users** → select the user you created (for example `Dat`) → **Delete**.

![1786480145480](image/_index.vi/1786480145480.png)

* Open **User groups** → select the project group → **Delete**.

![1786480172675](image/_index.vi/1786480172675.png)

**Step 4: Remove network and security (VPC & Security Groups)**

* Open **VPC Console** → **Security groups** → delete the security groups you created.

![1786480267634](image/_index.vi/1786480267634.png)

* Open **Your VPCs** → select `law-chatbot-vpc` → **Actions** → **Delete VPC**. This also removes the related subnets, route tables, and Internet Gateway.

![1786480441898](image/_index.vi/1786480441898.png)

## 5.5.4. Next steps

- Deploy the full **CloudFormation stack** on a real AWS account
- Finish **Lambda ingestion** for production
- Enable **Cognito** fully in production (do not use AUTH_DISABLED)
- Add **CloudWatch logs/alarms** and SNS notifications
- Move secrets to **AWS Secrets Manager**
- Consider **RDS Proxy** if there are many concurrent connections
- Add **HTTPS/domain/WAF** before public production
- Further improve retrieval quality and latency
