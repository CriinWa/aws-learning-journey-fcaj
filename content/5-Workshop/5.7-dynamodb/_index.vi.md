---
title: "DynamoDB — Chat history"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

# DynamoDB — Chat history

DynamoDB lưu metadata hội thoại và message cho API có Cognito (`/api/chat`, `/api/conversations`). Lịch sử trên Streamlit demo dùng Postgres/SQLite trong app DB.

## Mô hình dữ liệu

Module: **`src/services/chat_history.py`**

Bảng: **`LegalChatbotHistory`** (tạo bởi CloudFormation hoặc thủ công)

{{< mermaid >}}
graph TB;
    U["User"] --> C["Conversation"]
    C --> M1["Message user"]
    C --> M2["Message assistant"]
    C --> GSI1["GSI theo user"]
    C --> GSI2["GSI theo ngay admin"]
    C --> TTL["TTL expiry"]
{{< /mermaid >}}

| Loại item | Key pattern | Nội dung |
| --- | --- | --- |
| Conversation | `PK=USER#<id>`, `SK=CONV#<conv_id>` | Tiêu đề, timestamp |
| Message | `SK=MSG#<timestamp>` | Role, nội dung, metadata |

**GSI:** theo user (list conversation) và theo ngày (admin dashboard).

**TTL:** tự xóa hội thoại cũ khi cấu hình.

## Bật trong `.env`

```
ENABLE_CHAT_HISTORY=true
DYNAMODB_TABLE_NAME=LegalChatbotHistory
AWS_DEFAULT_REGION=ap-southeast-1
```

Output CloudFormation từ `infra/foundation.yaml` cung cấp tên bảng.

## API dùng lịch sử

| Endpoint | Hành vi |
| --- | --- |
| `POST /api/chat` | Hỏi đáp + lưu message (tùy cấu hình) |
| `GET /api/conversations` | Danh sách hội thoại của user |
| `GET /api/conversations/{id}` | Lấy message trong hội thoại |
| `DELETE /api/conversations/{id}` | Xóa hội thoại |
| `GET /api/admin/conversations` | Admin xem theo ngày (GSI) |

Cần Cognito JWT — xem [5.8 Cognito](5.8-cognito/).

## Kiểm tra

- [ ] Bảng `LegalChatbotHistory` tồn tại
- [ ] `POST /api/chat` tạo conversation + message
- [ ] `GET /api/conversations` trả session của user
- [ ] TTL attribute được set (nếu bật)

Tiếp theo: cấu hình **Cognito** cho xác thực và phân quyền.
