---
title: "VPC — Network"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---

# VPC — Network

VPC cô lập EC2, RDS và Lambda. Security Group kiểm soát port mà từng thành phần được phép truy cập — cần thiết cho deploy Law-Chatbot an toàn.

![Tổng quan VPC](/images/5-Workshop/5.3-S3-vpc/vpc.png)

## Sơ đồ network

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

| Thành phần | Subnet | Ghi chú |
| --- | --- | --- |
| **EC2** | Public subnet (lab) hoặc private + ALB | Chạy Streamlit :8501 và FastAPI :8000 |
| **RDS** | Private subnet | Production không public |
| **Lambda** | Private subnet (cùng VPC RDS) | Cần NAT hoặc VPC endpoint cho S3/Bedrock |

## Security Group

| Resource | Inbound | Outbound |
| --- | --- | --- |
| **EC2 SG** | 8501 từ IP bạn / ALB; 22 từ IP admin (tùy chọn) | All (hoặc giới hạn 443) |
| **RDS SG** | 5432 từ EC2 SG và Lambda SG | — |
| **Lambda SG** | — | 5432 tới RDS; 443 tới AWS API |

![Sơ đồ network](/images/5-Workshop/5.3-S3-vpc/diagram2.png)

## Tùy chọn: VPC endpoint

Subnet private không có NAT Gateway:

| Endpoint | Loại | Mục đích |
| --- | --- | --- |
| S3 | Gateway | EC2/Lambda đọc tài liệu không qua Internet |
| Bedrock | Interface | Gọi LLM/embedding trong VPC |
| SQS | Interface | Lambda poll queue nội bộ |

Demo lab thường dùng EC2 public subnet + Internet Gateway — đơn giản hơn nhưng kém hardened production.

## ALB (tùy chọn production)

- Application Load Balancer port 443 → target group EC2:8501
- Terminate TLS tại ALB
- EC2 SG chỉ nhận từ ALB SG (đóng 8501 trực tiếp từ Internet)

## Kiểm tra

- [ ] EC2 và RDS cùng VPC
- [ ] RDS SG chỉ mở 5432 từ EC2/Lambda SG
- [ ] EC2 gọi được Bedrock và S3
- [ ] Lambda trong VPC kết nối RDS (nếu dùng serverless ingestion)
- [ ] Streamlit truy cập được :8501 từ IP cho phép

Tiếp theo: **deploy Docker Compose trên EC2**.
