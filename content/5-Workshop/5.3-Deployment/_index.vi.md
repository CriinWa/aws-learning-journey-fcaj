---
title: "Các bước triển khai"
date: 2026-08-11
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---
Phần này hướng dẫn triển khai Vietnamese Legal RAG Chatbot trên AWS theo đúng thứ tự phụ thuộc: VPC/Network trước, sau đó RDS, S3, Lambda ingestion, Bedrock, DynamoDB, Cognito và cuối cùng là EC2 Docker Compose.

## Nội dung triển khai

1. [VPC — Network](5.3.1-vpc/) — tạo VPC, subnet, Security Group, kết nối giữa EC2/RDS/Lambda.
2. [RDS PostgreSQL + pgvector](5.3.2-rds-pgvector/) — tạo RDS instance, bật pgvector, tạo bảng và build vector index.
3. [S3 — Upload data](5.3.3-s3-upload/) — tạo bucket, cấu hình prefix, upload tài liệu và kết nối S3 Event → SQS.
4. [Lambda — Ingestion](5.3.4-lambda/) — cấu hình Lambda xử lý tài liệu: SQS trigger, chunk, embed, ghi pgvector.
5. [Cognito — Auth và RBAC](5.3.5-cognito/) — tạo User Pool, groups, tích hợp JWT vào FastAPI.
6. [Deploy Docker trên EC2](5.3.6-ec2-deploy/) — build image, Docker Compose, deploy và health check.
