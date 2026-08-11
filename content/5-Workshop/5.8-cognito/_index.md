---
title: "Cognito — Auth and RBAC"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

# Cognito — Auth and RBAC

Amazon Cognito secures the full FastAPI app (`api.app` with `/api/*` routes). Streamlit demo uses username/password in the app DB; Cognito applies to admin API, document upload, and production chat history.

## Auth flow

{{< mermaid >}}
sequenceDiagram
    participant C as Client
    participant CO as Cognito
    participant API as FastAPI api.app
    participant S as Services
    C->>CO: Login get JWT
    CO-->>C: ID or Access token
    C->>API: Authorization Bearer token
    API->>API: Verify JWT JWKS
    API->>S: Check role then execute
{{< /mermaid >}}

## Cognito groups (RBAC)

| Group | Permissions |
| --- | --- |
| **users** | Chat API, own conversations |
| **editors** | Upload documents, manage corpus |
| **admins** | User management, admin dashboards, all editor rights |

Module **`src/api/auth.py`**: verify JWT against Cognito JWKS, extract groups, enforce `require_roles`.

Admin operations: **`src/services/cognito_admin.py`** — list users, enable/disable, assign groups.

## CloudFormation setup

`infra/foundation.yaml` creates:

- Cognito User Pool
- App Client
- Groups: `users`, `editors`, `admins`

Deploy stack and copy outputs to `.env`:

```
COGNITO_USER_POOL_ID=ap-southeast-1_xxxxx
COGNITO_APP_CLIENT_ID=xxxxxxxx
AUTH_DISABLED=false
```

{{% notice warning %}}
`AUTH_DISABLED=true` creates a synthetic admin user — **Streamlit compose/dev only**, not for production.
{{% /notice %}}

## Protected vs open endpoints

| Path | Auth |
| --- | --- |
| `POST /ask` (api.main) | No Cognito — Streamlit demo path |
| `/api/*` (api.app) | Cognito JWT required |
| `/api/admin/*` | Cognito + `admins` group |

## Admin API examples

| Endpoint | Action |
| --- | --- |
| `GET /api/admin/users` | List Cognito users |
| `POST /api/admin/users/{username}/disable` | Disable user |
| `POST /api/admin/users/{username}/enable` | Enable user |
| `POST /api/admin/users/{username}/group` | Assign group |
| `POST /api/admin/documents/upload-url` | Presigned S3 upload |

## Verification

- [ ] User Pool and groups exist
- [ ] Valid JWT → `/api/chat` returns 200
- [ ] Missing token → 401/403
- [ ] Non-admin blocked from `/api/admin/*`
- [ ] Admin can list and manage users

Next: configure **VPC and network** for EC2, RDS, and Lambda.
