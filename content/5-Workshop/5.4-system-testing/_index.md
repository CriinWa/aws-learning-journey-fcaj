---
title: "System Testing"
date: 2026-08-11
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# System Testing

After deployment, run a short set of end-to-end scenarios before demo.

## 1. Frontend (Streamlit)

| Scenario | Expected |
| --- | --- |
| Register a new account | Cognito user created; chat screen opens |
| Login with valid / invalid password | Valid → app; invalid → clear error |
| Ask a question in chatbot | Returns `answer` + `sources` |
| Open source expander | Shows title / snippet / score |
| Regular user opens Admin | Denied or admin UI hidden |
| Admin opens Dashboard | KPI / user list visible |

## 2. Backend (FastAPI)

| Scenario | Expected |
| --- | --- |
| `GET /` | `200` health check |
| `POST /ask` with `{question, top_k}` | Valid `{answer, sources}` |
| Call API without token | Blocked with `401` / `403` |
| Empty or invalid payload | Validation error, no crash |

## 3. RAG end-to-end

Sample questions: marriage conditions under Vietnamese law; employee rights under the Labor Code.

| Criterion | Pass when |
| --- | --- |
| Retrieval | Sources match the question topic |
| Generation | Answer stays grounded in context |
| Latency | Response time is acceptable for a demo |
