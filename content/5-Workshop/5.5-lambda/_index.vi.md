---
title: "Lambda — Ingestion"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

# Lambda — Ingestion

AWS Lambda xử lý tài liệu pháp luật mới upload lên S3: đọc PDF/TXT, chunk, tạo embedding và ghi vector vào RDS pgvector — được kích hoạt qua SQS.

## Luồng end-to-end

{{< mermaid >}}
sequenceDiagram
    participant S3 as S3
    participant SQS as SQS
    participant L as Lambda
    participant B as Bedrock
    participant RDS as RDS pgvector
    S3->>SQS: ObjectCreated event
    SQS->>L: Trigger batch
    L->>S3: Doc file + manifest
    L->>L: Chunk document
    L->>B: Create embeddings
    B-->>L: Vectors
    L->>RDS: Upsert legal_chunks
    Note over SQS: Message loi → DLQ
{{< /mermaid >}}

## SQS và DLQ

| Tài nguyên | Mục đích |
| --- | --- |
| **Ingestion queue** | Nhận message từ S3 event |
| **Dead Letter Queue (DLQ)** | Lưu message xử lý thất bại sau retry |

CloudFormation trong `infra/foundation.yaml` có thể tạo cả hai queue. Cấu hình redrive policy từ queue chính sang DLQ.

## Lambda handler

Entry point: **`src/rag_core/lambda_handler.py`**

Các bước xử lý:

1. Parse S3/SQS event → lấy bucket, key
2. Đọc file từ `incoming/files/` và manifest từ `incoming/manifests/`
3. Chunk văn bản (`chunking.py`)
4. Tạo embedding (`embeddings.py` — Bedrock hoặc local)
5. Ghi vào RDS `legal_chunks` (`vector_store.py`)
6. Bỏ qua `chunk_id` đã tồn tại để resume an toàn
7. Báo partial batch failure cho SQS khi bật

## Cấu hình deploy

| Thiết lập | Khuyến nghị |
| --- | --- |
| Runtime | Python 3.11+ |
| Timeout | 5–15 phút (tùy kích thước file) |
| Memory | 1024 MB+ |
| VPC | Cùng subnet với RDS; SG cho phép 5432 tới RDS |
| IAM Role | `s3:GetObject`, `sqs:*`, `bedrock:InvokeModel`, truy cập RDS |

## Biến môi trường (Lambda)

Dùng cùng biến database và Bedrock như `.env` trên EC2:

```
USE_PGVECTOR=true
PGHOST=...
LLM_PROVIDER=bedrock
USE_BEDROCK_EMBEDDING=true
BEDROCK_EMBEDDING_MODEL=amazon.titan-embed-text-v2:0
LEGAL_DOCUMENTS_BUCKET=...
```

## Kiểm tra

- [ ] Upload PDF/TXT lên S3 → SQS nhận message
- [ ] Log Lambda hiển thị chunk + embed thành công
- [ ] Dòng mới xuất hiện trong `legal_chunks`
- [ ] Message lỗi test chuyển sang DLQ sau retry

Tiếp theo: cấu hình **Amazon Bedrock** cho embedding và LLM.
