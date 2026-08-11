---
title: "Cognito — Auth & RBAC"
date: 2026-08-11
weight: 7
chapter: false
pre: " <b> 5.3.7. </b> "
---

# Cognito — Auth & RBAC

Amazon Cognito protects the FastAPI application (`api.app` with `/api/*` routes). Streamlit demo uses username/password in the app DB; Cognito is for production admin API, document upload, and chat history.

{{< mermaid >}}
sequenceDiagram
    participant C as Client
    participant CO as Cognito
    participant API as FastAPI api.app
    participant S as Services
    C->>CO: Login and get JWT
    CO-->>C: ID or Access token
    C->>API: Authorization Bearer token
    API->>API: Verify JWT via JWKS
    API->>S: Check role then process
{{< /mermaid >}}

## Cognito Groups (RBAC)

| Group | Permissions |
| --- | --- |
| **users** | Chat API, own conversations |
| **editors** | Upload documents, manage corpus |
| **admins** | User management, admin dashboard, full editor access |

## Steps

1. Deploy CloudFormation stack (creates User Pool + groups)
2. Configure `.env` with pool ID and client ID
3. Test: valid JWT → 200; missing token → 401/403
4. Verify admin routes are blocked for non-admin users

Refer to the Vietnamese version for full details.
