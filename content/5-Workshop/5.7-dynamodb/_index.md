---
title: "DynamoDB — Chat history"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

# DynamoDB — Chat history

DynamoDB stores conversation metadata and messages for the Cognito-protected API (`/api/chat`, `/api/conversations`). Streamlit demo history uses the app Postgres/SQLite store instead.

## Data model

Module: **`src/services/chat_history.py`**

Table: **`LegalChatbotHistory`** (created by CloudFormation or manually)

{{< mermaid >}}
graph TB;
    U["User"] --> C["Conversation"]
    C --> M1["Message user"]
    C --> M2["Message assistant"]
    C --> GSI1["GSI by user"]
    C --> GSI2["GSI by date admin"]
    C --> TTL["TTL expiry"]
{{< /mermaid >}}

| Item type | Key pattern | Content |
| --- | --- | --- |
| Conversation | `PK=USER#<id>`, `SK=CONV#<conv_id>` | Title, timestamps |
| Message | `SK=MSG#<timestamp>` | Role, content, metadata |

**GSI:** by user (list conversations) and by date (admin dashboard).

**TTL:** auto-delete old conversations when configured.

## Enable in `.env`

```
ENABLE_CHAT_HISTORY=true
DYNAMODB_TABLE_NAME=LegalChatbotHistory
AWS_DEFAULT_REGION=ap-southeast-1
```

CloudFormation outputs from `infra/foundation.yaml` provide the table name.

## API endpoints using history

| Endpoint | Behavior |
| --- | --- |
| `POST /api/chat` | Ask + optionally persist messages |
| `GET /api/conversations` | List user conversations |
| `GET /api/conversations/{id}` | Get messages in a conversation |
| `DELETE /api/conversations/{id}` | Remove a conversation |
| `GET /api/admin/conversations` | Admin view by day (GSI) |

Requires Cognito JWT — see [5.8 Cognito](5.8-cognito/).

## Verification

- [ ] Table `LegalChatbotHistory` exists
- [ ] `POST /api/chat` creates conversation + messages
- [ ] `GET /api/conversations` returns the user's sessions
- [ ] TTL attribute set on items (if enabled)

Next: configure **Cognito** for authentication and RBAC.
