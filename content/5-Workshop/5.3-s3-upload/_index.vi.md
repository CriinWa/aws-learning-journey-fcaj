---
title: "S3 bucket và upload data"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

# S3 — Tạo bucket và upload data

Amazon S3 lưu tài liệu pháp luật và manifest ingestion. Khi có file mới, S3 event kích hoạt luồng xử lý phía sau (SQS → Lambda → RDS).

![Tổng quan S3](/images/5-Workshop/5.3-S3-vpc/overview.png)

## Vai trò trong hệ thống

| Prefix | Mục đích |
| --- | --- |
| `incoming/files/` | PDF/TXT do admin upload |
| `incoming/manifests/` | JSON metadata cho từng tài liệu |

{{< mermaid >}}
graph LR;
    A["Admin"] -->|presigned URL| B["S3 bucket"]
    B --> C["incoming/files"]
    B --> D["incoming/manifests"]
    B -->|ObjectCreated| E["SQS"]
    E --> F["Lambda"]
    F --> G[("RDS pgvector")]
{{< /mermaid >}}

## Bước 1 — Tạo bucket

1. Mở **Amazon S3** → **Create bucket**
2. Tên: ví dụ `legal-documents-<account-id>-ap-southeast-1`
3. Region: **ap-southeast-1** (cùng region với các tài nguyên khác)
4. Block public access: **bật**
5. Bật versioning (tùy chọn, khuyến nghị cho production)

![Tạo bucket](/images/5-Workshop/5.3-S3-vpc/create-button.png)

## Bước 2 — Cấu hình `.env`

```
USE_S3=true
LEGAL_DOCUMENTS_BUCKET=legal-documents-<account-id>-ap-southeast-1
AWS_DEFAULT_REGION=ap-southeast-1
```

## Bước 3 — Upload qua admin API hoặc CLI

**Luồng production:** admin gọi `POST /api/admin/documents/upload-url` → client PUT file lên S3.

**Lab / đồng bộ:** dùng `scripts/sync_to_s3.py`:

```bash
python scripts/sync_to_s3.py --bucket $LEGAL_DOCUMENTS_BUCKET
```

**Ví dụ CLI thủ công:**

```bash
aws s3 cp data_demo/sample.pdf s3://$LEGAL_DOCUMENTS_BUCKET/incoming/files/sample.pdf
aws s3 cp data_demo/sample.manifest.json s3://$LEGAL_DOCUMENTS_BUCKET/incoming/manifests/sample.manifest.json
```

## Bước 4 — Kết nối S3 → SQS (xem trước)

Cấu hình S3 event notification trên prefix `incoming/files/` gửi message tới ingestion queue (chi tiết ở [5.5 Lambda](5.5-lambda/)).

{{% notice info %}}
Mỗi file upload nên có manifest tương ứng trong `incoming/manifests/` để Lambda biết tiêu đề, loại tài liệu và metadata.
{{% /notice %}}

## Kiểm tra

- [ ] Bucket tồn tại đúng region
- [ ] Object test xuất hiện dưới `incoming/files/`
- [ ] IAM role/user có quyền `s3:PutObject` và `s3:GetObject`
- [ ] `LEGAL_DOCUMENTS_BUCKET` đã set trong `.env`

Tiếp theo: cấu hình **RDS PostgreSQL + pgvector** để lưu vector.
