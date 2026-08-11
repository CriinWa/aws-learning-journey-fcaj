---
title: "VPC — Network"
date: 2026-08-11
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

# VPC — Network

VPC isolates EC2, RDS, and Lambda. Security Groups control port access for each component.

{{< mermaid >}}
graph TB
    I["Internet"] --> EC2["EC2 Docker Compose"]
    EC2 -->|5432| RDS[("RDS PostgreSQL")]
    EC2 -->|443| BR["Bedrock API"]
    EC2 -->|443| S3["S3"]
    L["Lambda VPC"] -->|5432| RDS
    L --> S3
    S3 --> SQS["SQS"]
    SQS --> L
{{< /mermaid >}}

| Component | Subnet | Note |
| --- | --- | --- |
| **EC2** | Public subnet (lab) or private + ALB | Runs Streamlit :8501 and FastAPI :8000 |
| **RDS** | Private subnet | Not publicly accessible |
| **Lambda** | Private subnet (same VPC as RDS) | Needs NAT or VPC endpoint for S3/Bedrock |

## Security Groups

| Resource | Inbound | Outbound |
| --- | --- | --- |
| **EC2 SG** | 8501 from your IP / ALB; 22 from admin IP (optional) | All (or restrict to 443) |
| **RDS SG** | 5432 from EC2 SG and Lambda SG | — |
| **Lambda SG** | — | 5432 to RDS; 443 to AWS APIs |

Refer to the Vietnamese version for full step-by-step instructions.
