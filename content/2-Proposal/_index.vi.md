---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---


# Vietnamese Legal RAG Chatbot
## Giải pháp hỏi đáp pháp luật Việt Nam trên AWS

### 1. Tóm tắt điều hành

Vietnamese Legal RAG Chatbot là hệ thống cho phép người dùng đặt câu hỏi bằng tiếng Việt về các văn bản pháp luật (Luật, Nghị định, Thông tư...) và nhận câu trả lời có trích dẫn nguồn. Hệ thống sử dụng pipeline RAG: truy xuất các đoạn văn bản luật liên quan từ cơ sở vector, sau đó gửi sang mô hình ngôn ngữ lớn (LLM) trên **Amazon Bedrock** để sinh câu trả lời chính xác, tránh hallucination so với chatbot thuần LLM.

Giải pháp hướng tới quy mô nội bộ (phòng ban pháp chế, trung tâm nghiên cứu, sinh viên luật) với khoảng 10–50 người dùng đồng thời, corpus ban đầu từ dataset HuggingFace NguyenKH/clean_legal_knowledge và khả năng bổ sung văn bản mới qua kênh admin upload.

**Demo triển khai:** [http://18.143.187.153:8501/](http://18.143.187.153:8501/) (Streamlit trên EC2, ap-southeast-1)

### 2. Tuyên bố vấn đề

**Vấn đề hiện tại**

* Tra cứu văn bản pháp luật Việt Nam thường thủ công, tốn thời gian tìm điều/khoản liên quan.
* Chatbot LLM thuần túy dễ trả lời sai hoặc bịa điều luật không tồn tại.
* Thiếu hệ thống tập trung cho phép upload, index và quản lý vòng đời văn bản pháp luật mới.

**Giải pháp**

Xây dựng chatbot RAG trên AWS với ba luồng chính:

1. **Luồng nạp dữ liệu (Ingestion):** Admin upload PDF/TXT lên **Amazon S3** → sự kiện kích hoạt **Amazon SQS** → **AWS Lambda** chunking, gọi **Amazon Bedrock Titan Embeddings**, lưu vector vào **Amazon RDS PostgreSQL (pgvector)**.
2. **Luồng hỏi đáp (RAG Query):** Người dùng gửi câu hỏi qua giao diện **Chainlit/FastAPI** trên **Amazon EC2** (sau **Application Load Balancer**) → embed câu hỏi → tìm kiếm vector trên RDS → ghép prompt → **Amazon Bedrock LLM** (Claude 3 / Llama 3) sinh câu trả lời → stream về UI.
3. **Luồng vận hành (Observability):** Log/metric lên **Amazon CloudWatch** → **Amazon SNS** gửi email cảnh báo khi lỗi hoặc chi phí vượt ngưỡng.

Xác thực người dùng qua **Amazon Cognito** (nhóm users/editors/admins). Lịch sử hội thoại lưu trên **Amazon DynamoDB** với TTL và GSI phục vụ admin.

**Lợi ích**

* Câu trả lời có căn cứ từ văn bản luật thực tế, kèm metadata trích dẫn.
* Pipeline ingestion tự động khi admin upload văn bản mới.
* Kiến trúc cloud-native, có thể mở rộng corpus và số người dùng.
* Tận dụng managed services AWS, giảm vận hành hạ tầng so với self-host toàn bộ.

### 3. Kiến trúc giải pháp

![Kiến trúc Vietnamese Legal RAG Chatbot](/images/2-Proposal/legal_chatbot_architecture.png)

**Dịch vụ AWS sử dụng**

| Dịch vụ | Vai trò |
| --- | --- |
| **Amazon EC2** | Host ứng dụng FastAPI + Chainlit trong private subnet |
| **Application Load Balancer (ALB)** | Điểm vào HTTPS, phân tải tới EC2/ECS |
| **Amazon RDS (PostgreSQL + pgvector)** | Vector database, lưu chunk văn bản luật và embedding |
| **Amazon Bedrock** | Titan Embeddings + LLM (Claude 3, Llama 3) |
| **Amazon S3** | Lưu văn bản gốc, manifest upload, vector store artefact |
| **AWS Lambda** | Xử lý ingestion: đọc S3, chunk, embed, ghi RDS |
| **Amazon SQS + DLQ** | Hàng đợi ingestion, retry và dead-letter |
| **Amazon DynamoDB** | Lưu lịch sử chat, conversation metadata |
| **Amazon Cognito** | Xác thực JWT, RBAC users/editors/admins |
| **Amazon VPC** | Public/private/isolated subnet, security group |
| **VPC Endpoints** | Truy cập S3, Bedrock, DynamoDB không qua Internet |
| **Amazon CloudWatch + SNS** | Logging, metric, alarm, thông báo email |
| **AWS CloudFormation** | IaC deploy foundation stack (Cognito, DynamoDB, S3, SQS) |
| **AWS Secrets Manager** | Quản lý RDS password, API key (production) |

**Thiết kế thành phần**

* **Frontend/Chat UI:** Chainlit (demo/UAT) hoặc web app tích hợp Cognito (production).
* **API Layer:** FastAPI với **/api/chat**, **/api/admin/***, JWT middleware.
* **RAG Core:** QAService — retriever pgvector, prompt builder, Bedrock generator.
* **Ingestion Worker:** Lambda handler đọc S3/SQS, hỗ trợ PDF/TXT, partial batch failure.
* **Admin:** Presigned S3 upload, quản lý user Cognito, soft delete văn bản.

### 4. Triển khai kỹ thuật

**Các giai đoạn triển khai**

| Giai đoạn | Nội dung | Thời gian |
| --- | --- | --- |
| 1. Nghiên cứu & prototype local | RAG pipeline, SQLite vector store, FastAPI | Tuần 4–5 |
| 2. Tích hợp AWS cơ bản | S3 sync, Docker, Chainlit | Tuần 6–7 |
| 3. Production data layer | RDS pgvector, Bedrock | Tuần 7 |
| 4. Auth & foundation IaC | Cognito, DynamoDB, CloudFormation stack | Tuần 8 |
| 5. Ingestion serverless | S3 → SQS → Lambda → RDS | Tuần 8 |
| 6. Tối ưu & báo cáo | Benchmark, CloudWatch, hoàn thiện báo cáo | Tuần 8 |

**Yêu cầu kỹ thuật**

* Python 3.11+, FastAPI, Sentence-Transformers / Bedrock Embeddings.
* PostgreSQL 15+ với extension pgvector.
* Docker container cho EC2/ECS deployment.
* IAM least privilege; không commit credential vào git.
* Region đề xuất: **ap-southeast-1** (Singapore).

**Liên kết triển khai**

| Loại | Liên kết |
| --- | --- |
| **Repository** | [github.com/KhanhKoy/vietnamese-legal-llmops](https://github.com/KhanhKoy/vietnamese-legal-llmops) |
| **Production (demo)** | [http://18.143.187.153:8501/](http://18.143.187.153:8501/) |

Mã nguồn gồm src/rag_core/, src/api/, infra/foundation.yaml, deploy/Dockerfile. Môi trường demo chạy trên EC2 (ap-southeast-1) với giao diện Streamlit, kết nối RDS pgvector và Bedrock.

### 5. Lộ trình & Mốc triển khai

* **Tuần 1–2 (22/06 – 03/07):** AWS fundamentals, S3/IAM.
* **Tuần 3–4 (06/07 – 17/07):** VPC workshop, nghiên cứu RAG và dataset pháp luật.
* **Tuần 5–6 (20/07 – 31/07):** Prototype local, FastAPI, Docker, Chainlit.
* **Tuần 7–8 (03/08 – 14/08):** Bedrock, RDS pgvector, Cognito, Lambda ingestion, benchmark, báo cáo.
* **Sau thực tập:** HA deployment, WAF, frontend admin, chuyển toàn bộ embedding sang Bedrock Titan.

### 6. Ước tính ngân sách (dev/staging, ap-southeast-1)

| Hạng mục | Chi phí ước tính/tháng |
| --- | --- |
| EC2 t3a.small | ~14 USD |
| RDS db.t3.micro PostgreSQL | ~15 USD |
| Amazon Bedrock (embed + LLM, ~10K query) | ~5–20 USD |
| S3 Standard (~10 GB) | ~0.25 USD |
| DynamoDB on-demand | ~1 USD |
| Lambda + SQS | ~1 USD |
| Cognito | Free tier (< 50K MAU) |
| CloudWatch + SNS | ~2 USD |
| **Tổng ước tính** | **~40–55 USD/tháng** |

*Ghi chú:* Chi phí production với ALB, 2 EC2, RDS Multi-AZ sẽ cao hơn. Có thể giảm bằng ECS Fargate Spot, RDS Reserved Instance, hoặc tắt instance ngoài giờ lab.

### 7. Đánh giá rủi ro

| Rủi ro | Mức ảnh hưởng | Giảm thiểu |
| --- | --- | --- |
| LLM hallucination | Cao | RAG bắt buộc trích dẫn context; prompt từ chối khi thiếu dữ liệu |
| Chi phí Bedrock vượt ngân sách | Trung bình | CloudWatch alarm, giới hạn token, cache câu hỏi phổ biến |
| Corpus pháp luật lỗi thời | Cao | Pipeline upload admin, versioning S3, soft delete |
| Latency cao khi corpus lớn | Trung bình | pgvector index (IVFFlat/HNSW), RDS Proxy, benchmark định kỳ |
| Lộ credential | Cao | Secrets Manager, IAM role cho EC2/Lambda, không hard-code key |

### 8. Kết quả kỳ vọng

* Prototype chatbot trả lời câu hỏi pháp luật tiếng Việt với nguồn trích dẫn.
* Pipeline ingestion tự động cho văn bản PDF/TXT mới.
* Nền tảng mở rộng cho nghiên cứu NLP pháp luật, đánh giá RAG metrics (Recall@k, MRR).
* Kiến thức thực hành AWS: EC2, S3, RDS, Lambda, Bedrock, Cognito, DynamoDB, VPC, CloudFormation.
