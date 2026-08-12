---
title: "Deployment Steps"
date: 2026-08-11
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---
This section deploys Vietnamese Legal RAG Chatbot on AWS in dependency order: VPC/Network first, then RDS, S3, Lambda ingestion, Cognito, and finally EC2 Docker Compose.

## Deployment contents

1. [VPC — Network](5.3.1-vpc/) — create VPC, subnets, Security Groups, and connectivity between EC2/RDS/Lambda.
2. [RDS PostgreSQL + pgvector](5.3.2-rds-pgvector/) — create the RDS instance, enable pgvector, create tables, and build the vector index.
3. [S3 — Upload data](5.3.3-s3-upload/) — create the bucket, configure prefixes, upload documents, and connect S3 Event → SQS.
4. [Lambda — Ingestion](5.3.4-lambda/) — configure Lambda to process documents: SQS trigger, chunk, embed, write to pgvector.
5. [Cognito — Auth & RBAC](5.3.5-cognito/) — create the User Pool, groups, and JWT integration with FastAPI.
6. [Deploy Docker on EC2](5.3.6-ec2-deploy/) — build images, run Docker Compose, deploy, and health-check.
