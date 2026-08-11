---
title: "Prerequisites"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

# Prerequisites

Before setting up AWS services in the following sections, complete the preparation steps below.

## 5.2.1. Source code

- Clone the repository to your local machine or EC2
- Verify folder layout (`src/`, `views/`, `deploy/`, `infra/`)
- Create a virtual environment and install dependencies
- Copy `.env.sample` to `.env`

```bash
git clone <your-repo-url> vietnamese-legal-llmops
cd vietnamese-legal-llmops
cp .env.sample .env
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

{{% notice warning %}}
Do not commit a `.env` file containing RDS passwords, API keys, or Access Keys to Git.
{{% /notice %}}

## 5.2.2. Data

- Prepare legal corpus files (PDF/TXT demo)
- Validate input files before building the index
- Configure data paths:
  - Local: `LOCAL_DEMO_PATH=data_demo/`
  - Cloud: `LEGAL_DOCUMENTS_BUCKET` on S3

## 5.2.3. AWS account

- Choose a deployment region (recommended: **ap-southeast-1**)
- Enable MFA for IAM user / root
- Confirm access to required services:

| Service | Required access |
| --- | --- |
| EC2 | Launch, describe instances |
| S3 | Create bucket, put/get objects |
| SQS | Create queue, send/receive messages |
| Lambda | Create function, invoke |
| Bedrock | InvokeModel (enable model access) |
| RDS | Create DB instance |
| DynamoDB | Create table, read/write |
| Cognito | Create user pool |
| CloudFormation | Create/update stack |

## 5.2.4. IAM User / IAM Role

- **Local dev:** create an IAM User and run `aws configure`
- **EC2 / Lambda:** prefer an **IAM Role** over long-lived Access Keys
- Grant **least privilege** per service

## 5.2.5. RDS PostgreSQL

- Create an Amazon RDS PostgreSQL instance
- Security Group: port **5432** only from the EC2 security group
- Configure `.env`:

```
USE_PGVECTOR=true
PGHOST=<rds-endpoint>
PGPORT=5432
PGDATABASE=legalchatbot
PGUSER=postgres
PGPASSWORD=<password>
```

- Enable pgvector: `CREATE EXTENSION vector;`
- Prepare the `legal_chunks` table for chunks and vectors

## 5.2.6. EC2 / Docker environment

- Create an EC2 instance (recommended: **t3a.small**)
- Install Docker and Docker Compose
- Clone the project and create a production `.env`
- Verify EC2 can reach RDS and Bedrock

![EC2 instance](images/5-Workshop/5.2-Prerequisite/ec2.png)

```bash
sudo yum install -y docker git
sudo systemctl enable --now docker
sudo usermod -aG docker $USER
docker --version
git --version
```

## Minimum security groups

| Direction | Port | Notes |
| --- | --- | --- |
| Inbound EC2 | **8501** | Streamlit UI — open only from your IP or ALB |
| Inbound EC2 | **8000** | FastAPI — usually Docker-internal only |
| Inbound RDS | **5432** | Allow only from the EC2 security group |

## Checklist before section 5.3

- [ ] Can sign in to AWS Console / CLI
- [ ] EC2 is ready; SSH or Session Manager works
- [ ] Docker and Git work on EC2
- [ ] Repo is cloned with a `.env` file
- [ ] RDS pgvector is ready; `vector` extension enabled
- [ ] Security groups and IAM role are configured
- [ ] Bedrock or Gemini is chosen for LLM

Next: create an **S3 bucket** and upload data.
