---
title: "Summary"
date: 2024-01-01
weight: 12
chapter: false
pre: " <b> 5.12. </b> "
---

# Workshop Summary

## 5.12.1. Results achieved

After completing the workshop, the Law-Chatbot system delivers:

- Full **RAG** legal Q&A chatbot
- **Streamlit** frontend with Login, Register, Chatbot, Admin screens
- **FastAPI** backend with `/ask` and full `/api/*` routes
- **RDS PostgreSQL + pgvector** for vector search
- **Amazon Bedrock** for cloud embedding/LLM
- Document upload and ingestion via **S3 → SQS → Lambda**
- Chat history on **DynamoDB**
- **Cognito** auth/RBAC for admin API
- **CloudFormation** template for foundation resources
- Production-ready **Docker Compose** deploy on EC2

{{< mermaid >}}
graph LR;
    A1["Data ingest"] --> A2["RAG Q&A"]
    A2 --> A3["AWS operations"]
{{< /mermaid >}}

## 5.12.2. Current limitations

| Limitation | Detail |
| --- | --- |
| AWS resources | Some resources exist as code/templates — full deploy needed to validate |
| Cognito on `/ask` | Not required for Streamlit endpoint in dev |
| Monitoring | CloudWatch/SNS at guidance level only |
| Admin UI | Room for more features |
| Benchmark | Needs measurement on real AWS environment |

## 5.12.3. Cost and cleanup

Track costs after the lab:

| Service | Notes |
| --- | --- |
| EC2 | Stop instance when not in use |
| RDS | Highest cost — delete if not needed |
| Bedrock | Billed per embedding/LLM token |
| S3 | Delete test files after lab |
| Lambda | Usually within free tier |
| DynamoDB | On-demand or delete test table |

**Cleanup checklist:**

- [ ] Stop or terminate EC2 instance
- [ ] Delete RDS instance if no longer needed
- [ ] Delete test files in S3 bucket
- [ ] Delete SQS queue, DLQ, DynamoDB test table
- [ ] Delete CloudFormation stack if no longer needed
- [ ] Review AWS Billing Dashboard

## 5.12.4. Future development

- Deploy full **CloudFormation stack** on real AWS
- Complete **Lambda ingestion** on production
- Enable full **Cognito** for production (no AUTH_DISABLED)
- Add **CloudWatch logs/alarms** and SNS notifications
- Move secrets to **AWS Secrets Manager**
- Consider **RDS Proxy** for many concurrent connections
- Add **HTTPS/domain/WAF** for public production
- Further optimize retrieval quality and latency

## 5.12.5. References

| Document | Description |
| --- | --- |
| `README.md` | Project overview |
| `docs/IMPLEMENTATION_PLAN.md` | Detailed implementation plan |
| `deploy/README.md` | Docker deploy guide |
| `infra/foundation.yaml` | CloudFormation template |
| [Amazon EC2](https://docs.aws.amazon.com/ec2/) | EC2 documentation |
| [Amazon S3](https://docs.aws.amazon.com/s3/) | S3 documentation |
| [Amazon Bedrock](https://docs.aws.amazon.com/bedrock/) | Bedrock documentation |
| [pgvector](https://github.com/pgvector/pgvector) | PostgreSQL vector extension |
| [Amazon Cognito](https://docs.aws.amazon.com/cognito/) | Cognito documentation |
