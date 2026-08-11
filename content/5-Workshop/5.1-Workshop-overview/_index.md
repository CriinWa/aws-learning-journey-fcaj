---
title: "Overview"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# Overview — Law-Chatbot project architecture

## 5.1.1. Project introduction

**Law-Chatbot** (Vietnamese Legal Assistant) is a **RAG** Q&A system in the **vietnamese-legal-llmops** codebase. It helps users look up Vietnamese legal documents and get answers **grounded in article/clause citations**, instead of relying only on the LLM's internal knowledge.

**Problem:** Legal corpora are long and fragmented (laws, decrees, circulars…); finding the right passages is slow and easy to miss context.

**Why RAG:** A plain LLM can hallucinate or lack legal basis. RAG forces the model to answer **only from retrieved context**, with cited sources.

**System goals:**

- Fast, accurate legal lookup
- Answers with specific article/clause references
- Corpus and user management
- Cost-effective AWS deployment

**Functional scope:**

| Feature | Description |
| --- | --- |
| Legal Q&A | End-to-end RAG pipeline |
| Source retrieval | Related legal passages with scores |
| Document management | Upload, automatic ingestion |
| User management | Register, login, role-based access |
| Chat history | Session storage on DynamoDB |

## 5.1.2. Frontend architecture

Primary UI: **Streamlit**, container port **8501**.

| Screen | File | Function |
| --- | --- | --- |
| Login | `views/login.py` | Auth, block Inactive users |
| Register | `views/register.py` | Create new account |
| Chatbot | `views/chatbot.py` | Q&A, display sources |
| Admin | `views/admin.py` | User/doc management, KPI |

{{< mermaid >}}
graph LR;
    A["streamlit_app"] --> B{"Logged in?"}
    B -->|No| C["Login Register"]
    B -->|Yes| D{"Role?"}
    D -->|User| E["Chatbot"]
    D -->|Admin| F["Admin"]
    E -->|POST ask| G["FastAPI"]
    F --> G
{{< /mermaid >}}

**Chatbot UI:** enter question → POST to FastAPI `/ask` → show answer and sources → manage chat sessions.

**Admin UI:** user management, document management, system monitoring.

Details: [Frontend](5.1.1-frontend/)

## 5.1.3. Backend architecture

Backend: **FastAPI**, main Q&A endpoint **`POST /ask`**.

**Module structure:**

```
src/
├── api/          → API, schema, routes, auth
├── rag_core/     → RAG processing
├── services/     → ingestion, chat history, Cognito admin
├── storage/      → app data storage
└── monitoring/   → logging and feedback
```

**Backend flow:**

1. Receive question from Streamlit
2. Embed the question
3. Query RDS PostgreSQL pgvector
4. Build prompt with context
5. Call LLM (Bedrock/Gemini)
6. Return answer and sources to frontend

Details: [Backend](5.1.2-backend/)

## 5.1.4. Overall AWS architecture

| Component | Role |
| --- | --- |
| **EC2** | Docker Compose (FastAPI :8000 + Streamlit :8501) |
| **RDS PostgreSQL + pgvector** | Legal text vectors, similarity search |
| **Amazon Bedrock** | Embedding and LLM in cloud config |
| **Amazon S3** | Legal documents and manifests |
| **Amazon SQS + DLQ** | Ingestion events from S3 |
| **AWS Lambda** | Process new documents (chunk + embed) |
| **Amazon DynamoDB** | Chat history |
| **Amazon Cognito** | Auth/RBAC for admin API |
| **CloudFormation** | Foundation resources (`infra/foundation.yaml`) |

![Vietnamese Legal RAG Chatbot architecture](/images/2-Proposal/legal_chatbot_architecture.png)

**Demo path:** Streamlit **8501** → **POST /ask** → FastAPI **8000** → QAService → RDS pgvector + Bedrock. Demo instance: **EC2 t3a.small**.

## 5.1.5. AWS services used

| Service | Purpose |
| --- | --- |
| Amazon EC2 | Host Docker Compose |
| Application Load Balancer | Traffic distribution (optional production) |
| Amazon VPC, Subnet, Security Group | Network isolation |
| Amazon S3 | Legal document storage |
| Amazon SQS and DLQ | Event-driven ingestion |
| AWS Lambda | Serverless ingestion |
| Amazon Bedrock | Embedding + LLM |
| Amazon RDS PostgreSQL + pgvector | Vector store |
| Amazon DynamoDB | Chat history |
| Amazon Cognito | Auth & RBAC |
| AWS CloudFormation | Infrastructure as Code |
| IAM/IAM Role | Least-privilege access |
| CloudWatch/SNS | Monitoring and alerts |

## 5.1.6. Functional specification

| Function | Summary |
| --- | --- |
| Register / login | App DB (Streamlit); Cognito JWT for API |
| Legal Q&A via RAG | Embed → retrieve → prompt → generate |
| Display sources | Title, snippet, score of related chunks |
| Session management | Create, select, delete chat sessions |
| Document management | Admin upload, list, soft-delete |
| S3 upload | Presigned URL for admin |
| Auto document processing | Lambda chunk/embed to RDS |
| Chat history | DynamoDB with user/date GSI, TTL |
| User/admin roles | Cognito groups: users, editors, admins |

## Repository layout

| Path | Role |
| --- | --- |
| `streamlit_app.py` | Streamlit entry |
| `views/` | login, register, chatbot, admin |
| `.env.sample` | Environment variable template |
| `src/api/` | `main.py` (POST /ask) and `app.py` (/api/*) |
| `src/rag_core/` | chunk, embed, retrieve, prompt, generate, lambda |
| `src/services/` | cognito, ingestion, chat_history |
| `scripts/` | build_index, sync_to_s3, benchmark_qa |
| `deploy/` | Dockerfile, docker-compose |
| `infra/` | CloudFormation template |

## Detailed architecture sections

1. [Frontend](5.1.1-frontend/)
2. [Backend](5.1.2-backend/)
