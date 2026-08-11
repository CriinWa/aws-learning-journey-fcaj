---
title: "RDS PostgreSQL + pgvector"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# RDS — PostgreSQL + pgvector

RDS PostgreSQL kết hợp extension **pgvector** là vector store cho các chunk văn bản luật. QAService embed câu hỏi và chạy similarity search để lấy đoạn văn liên quan.

## Vai trò trong kiến trúc

{{< mermaid >}}
graph TB;
    Q["Cau hoi"] --> E["Embedding"]
    E --> S["Similarity search"]
    S --> PG[("RDS legal_chunks")]
    PG --> C["Top-k chunks"]
    C --> P["Prompt + LLM"]
{{< /mermaid >}}

## Bước 1 — Tạo RDS instance

1. **Amazon RDS** → **Create database** → **PostgreSQL**
2. Engine: PostgreSQL 15+ (hỗ trợ pgvector)
3. Instance: `db.t3.micro` hoặc `db.t3.small` cho lab
4. Storage: 20 GB+ gp3
5. VPC: cùng VPC với EC2; **không public** trừ khi lab yêu cầu
6. Security Group: inbound **5432** chỉ từ SG của EC2

## Bước 2 — Bật pgvector và tạo bảng

Kết nối bằng psql hoặc SQL client:

```sql
CREATE EXTENSION IF NOT EXISTS vector;

CREATE TABLE IF NOT EXISTS legal_chunks (
    chunk_id    TEXT PRIMARY KEY,
    doc_id      TEXT,
    title       TEXT,
    content     TEXT,
    embedding   vector(1024),  -- dimension khớp embedding model
    metadata    JSONB,
    created_at  TIMESTAMPTZ DEFAULT NOW()
);
```

Chạy migration trong `migrations/` nếu project cung cấp.

{{% notice note %}}
Index HNSW / IVFFlat chỉ là tối ưu tùy chọn khi corpus lớn — không bắt buộc cho lab workshop.
{{% /notice %}}

## Bước 3 — Cấu hình `.env`

```
USE_PGVECTOR=true
PGHOST=<rds-endpoint>.ap-southeast-1.rds.amazonaws.com
PGPORT=5432
PGDATABASE=legalchatbot
PGUSER=postgres
PGPASSWORD=<password>
```

## Bước 4 — Build index

**Local / EC2 một lần:**

```bash
python scripts/build_index.py
```

**Ingestion cloud:** Lambda ghi chunk sau khi upload S3 (xem [5.5 Lambda](5.5-lambda/)).

## Hành vi truy vấn

| Cấu hình | Mục đích |
| --- | --- |
| `TOP_K` | Số chunk trả về mỗi câu hỏi |
| Query timeout | Tránh truy vấn similarity quá lâu |
| Fallback exact search | Dùng khi chưa tạo index HNSW/IVFFlat |

## Kiểm tra

- [ ] EC2 kết nối RDS port 5432 thành công
- [ ] `CREATE EXTENSION vector` chạy OK
- [ ] Bảng `legal_chunks` có dữ liệu sau build index
- [ ] Truy vấn similarity mẫu trả chunk hợp lý

Tiếp theo: cấu hình **Lambda ingestion** để xử lý tài liệu tự động.
