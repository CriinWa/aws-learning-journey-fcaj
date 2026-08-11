---
title: "VPC — Network"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---

# VPC — Network

The VPC isolates EC2, RDS, and Lambda. Security Groups control which ports each component can reach — essential for a secure Law-Chatbot deployment.

![VPC overview](/images/5-Workshop/5.3-S3-vpc/vpc.png)

## Network layout

{{< mermaid >}}
graph TB;
    I["Internet"] --> EC2["EC2 Docker Compose"]
    EC2 -->|5432| RDS[("RDS PostgreSQL")]
    EC2 -->|443| BR["Bedrock API"]
    EC2 -->|443| S3["S3"]
    L["Lambda VPC"] -->|5432| RDS
    L --> S3
    S3 --> SQS["SQS"]
    SQS --> L
{{< /mermaid >}}

| Component | Subnet | Notes |
| --- | --- | --- |
| **EC2** | Public subnet (lab) or private + ALB | Hosts Streamlit :8501 and FastAPI :8000 |
| **RDS** | Private subnet | No public access in production |
| **Lambda** | Private subnet (same VPC as RDS) | Needs NAT or VPC endpoints for S3/Bedrock |

## Security Groups

| Resource | Inbound | Outbound |
| --- | --- | --- |
| **EC2 SG** | 8501 from your IP / ALB; 22 from admin IP (optional) | All (or restrict to 443) |
| **RDS SG** | 5432 from EC2 SG and Lambda SG | — |
| **Lambda SG** | — | 5432 to RDS; 443 to AWS APIs |

![Network diagram](/images/5-Workshop/5.3-S3-vpc/diagram2.png)

## Optional: VPC endpoints

For private subnets without NAT Gateway:

| Endpoint | Type | Purpose |
| --- | --- | --- |
| S3 | Gateway | Lambda/EC2 read documents without Internet |
| Bedrock | Interface | LLM/embedding calls stay in VPC |
| SQS | Interface | Lambda poll queue privately |

Lab demo often uses a public subnet EC2 with Internet Gateway — simpler but less production-hardened.

## ALB (optional production)

- Application Load Balancer on port 443 → target group EC2:8501
- Terminate TLS at ALB
- Restrict EC2 SG to ALB SG only (close direct 8501 from Internet)

## Verification

- [ ] EC2 and RDS in the same VPC
- [ ] RDS SG allows 5432 only from EC2/Lambda SG
- [ ] EC2 can `curl` Bedrock and reach S3
- [ ] Lambda in VPC connects to RDS (if using serverless ingestion)
- [ ] Streamlit accessible on :8501 from allowed IP

Next: **deploy Docker Compose on EC2**.
