---
title: "Tổng kết"
date: 2026-08-11
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

# Tổng kết Workshop

## 5.12.1. Kết quả đạt được

Sau khi hoàn thành Workshop, hệ thống Law-Chatbot đạt được:

- Chatbot hỏi đáp pháp luật hoàn chỉnh theo kiến trúc **RAG**
- Frontend **Streamlit** với các màn hình Login, Register, Chatbot, Admin
- Backend **FastAPI** với endpoint `/ask` và API đầy đủ `/api/*`
- Tích hợp **RDS PostgreSQL + pgvector** cho vector search
- Tích hợp **Amazon Bedrock** cho embedding/LLM trên cloud
- Luồng upload và ingestion tài liệu qua **S3 → SQS → Lambda**
- Lưu lịch sử hội thoại bằng **DynamoDB**
- Xác thực/phân quyền bằng **Cognito** cho API quản trị
- **CloudFormation** template cho tài nguyên nền
- Deploy production-ready bằng **Docker Compose** trên EC2

{{< mermaid >}}
graph LR;
    A1["Nap du lieu"] --> A2["Hoi dap RAG"]
    A2 --> A3["Van hanh AWS"]
{{< /mermaid >}}

## 5.12.2. Hạn chế hiện tại

| Hạn chế | Chi tiết |
| --- | --- |
| Tài nguyên AWS | Một số tài nguyên ở mức code/template, cần deploy thật để kiểm chứng đầy đủ |
| Cognito trên `/ask` | Chưa bắt buộc cho endpoint Streamlit trong môi trường dev |
| Giám sát | CloudWatch/SNS mới ở mức định hướng vận hành |
| Admin UI | Còn có thể mở rộng thêm tính năng |
| Benchmark | Cần đo thêm trên môi trường AWS thật |

## 5.12.3. Chi phí và dọn dẹp tài nguyên

Theo dõi chi phí các dịch vụ sau khi hoàn thành lab:

| Dịch vụ | Ghi chú |
| --- | --- |
| EC2 | Dừng instance khi không sử dụng |
| RDS | Chi phí cao nhất — xóa nếu không cần |
| Bedrock | Tính theo token embedding/LLM |
| S3 | Xóa file test sau lab |
| Lambda | Thường trong free tier |
| DynamoDB | On-demand hoặc xóa table test |

**Checklist dọn dẹp:**

- [ ] Dừng hoặc terminate EC2 instance
- [ ] Xóa RDS instance nếu không còn dùng
- [ ] Xóa file test trong S3 bucket
- [ ] Xóa SQS queue, DLQ, DynamoDB table test
- [ ] Xóa CloudFormation stack nếu không còn cần
- [ ] Kiểm tra AWS Billing Dashboard

## 5.12.4. Hướng phát triển tiếp theo

- Deploy đầy đủ **CloudFormation stack** trên AWS thật
- Hoàn thiện **Lambda ingestion** trên production
- Bật **Cognito** đầy đủ cho production (không dùng AUTH_DISABLED)
- Bổ sung **CloudWatch logs/alarms** và SNS notification
- Chuyển secret sang **AWS Secrets Manager**
- Cân nhắc **RDS Proxy** nếu nhiều kết nối đồng thời
- Bổ sung **HTTPS/domain/WAF** khi public production
- Tối ưu thêm chất lượng retrieval và latency

## 5.12.5. Tài liệu tham khảo

| Tài liệu | Mô tả |
| --- | --- |
| `README.md` | Hướng dẫn chung dự án |
| `docs/IMPLEMENTATION_PLAN.md` | Kế hoạch triển khai chi tiết |
| `deploy/README.md` | Hướng dẫn deploy Docker |
| `infra/foundation.yaml` | CloudFormation template |
| [Amazon EC2](https://docs.aws.amazon.com/ec2/) | EC2 documentation |
| [Amazon S3](https://docs.aws.amazon.com/s3/) | S3 documentation |
| [Amazon Bedrock](https://docs.aws.amazon.com/bedrock/) | Bedrock documentation |
| [pgvector](https://github.com/pgvector/pgvector) | PostgreSQL vector extension |
| [Amazon Cognito](https://docs.aws.amazon.com/cognito/) | Cognito documentation |
