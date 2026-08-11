---
title: "Cognito — Auth và RBAC"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

# Cognito — Auth và RBAC

Amazon Cognito bảo vệ FastAPI đầy đủ (`api.app` với route `/api/*`). Demo Streamlit dùng username/password trong app DB; Cognito áp dụng cho API quản trị, upload tài liệu và lịch sử chat production.

## Luồng xác thực

{{< mermaid >}}
sequenceDiagram
    participant C as Client
    participant CO as Cognito
    participant API as FastAPI api.app
    participant S as Services
    C->>CO: Dang nhap lay JWT
    CO-->>C: ID hoac Access token
    C->>API: Authorization Bearer token
    API->>API: Verify JWT JWKS
    API->>S: Kiem tra role roi xu ly
{{< /mermaid >}}

## Nhóm Cognito (RBAC)

| Nhóm | Quyền |
| --- | --- |
| **users** | Chat API, hội thoại của mình |
| **editors** | Upload tài liệu, quản lý corpus |
| **admins** | Quản lý user, dashboard admin, toàn quyền editor |

Module **`src/api/auth.py`**: verify JWT qua Cognito JWKS, lấy groups, enforce `require_roles`.

Thao tác admin: **`src/services/cognito_admin.py`** — list user, enable/disable, gán group.

## Cấu hình CloudFormation

`infra/foundation.yaml` tạo:

- Cognito User Pool
- App Client
- Groups: `users`, `editors`, `admins`

Deploy stack và copy output vào `.env`:

```
COGNITO_USER_POOL_ID=ap-southeast-1_xxxxx
COGNITO_APP_CLIENT_ID=xxxxxxxx
AUTH_DISABLED=false
```

{{% notice warning %}}
`AUTH_DISABLED=true` tạo user admin giả — **chỉ dùng Streamlit compose/dev**, không dùng production.
{{% /notice %}}

## Endpoint mở vs bảo vệ

| Path | Auth |
| --- | --- |
| `POST /ask` (api.main) | Không Cognito — luồng demo Streamlit |
| `/api/*` (api.app) | Bắt buộc Cognito JWT |
| `/api/admin/*` | Cognito + nhóm `admins` |

## API admin ví dụ

| Endpoint | Hành động |
| --- | --- |
| `GET /api/admin/users` | Liệt kê user Cognito |
| `POST /api/admin/users/{username}/disable` | Vô hiệu hóa user |
| `POST /api/admin/users/{username}/enable` | Kích hoạt lại user |
| `POST /api/admin/users/{username}/group` | Gán group |
| `POST /api/admin/documents/upload-url` | Presigned upload S3 |

## Kiểm tra

- [ ] User Pool và groups đã tạo
- [ ] JWT hợp lệ → `/api/chat` trả 200
- [ ] Thiếu token → 401/403
- [ ] User không phải admin bị chặn `/api/admin/*`
- [ ] Admin list và quản lý user được

Tiếp theo: cấu hình **VPC và network** cho EC2, RDS, Lambda.
