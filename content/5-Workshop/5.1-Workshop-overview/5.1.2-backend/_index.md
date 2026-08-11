---
title: "Backend"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.1.2 </b> "
---

# Backend

The Law-Chatbot backend is **FastAPI** plus a **RAG Core**. There are **two FastAPI apps** that share QAService but serve different scenarios.

## Two FastAPI apps

| | **api.main** | **api.app + routes** |
| --- | --- | --- |
| Files | src/api/main.py | src/api/app.py, src/api/routes.py |
| Purpose | Thin API for Streamlit / EC2 demo | Full Cognito-protected API |
| Prefix | None | **/api** |
| Auth | None on **/ask** | Cognito JWT; AUTH_DISABLED → fake admin user |
| Docs | Default Swagger | **/docs** only if ENABLE_API_DOCS=true |
| When to use | Primary Compose deploy path | Override API_MODULE=api.app:app |

Entrypoint: uvicorn with API_MODULE defaulting to api.main:app; PYTHONPATH includes /app and /app/src.

**EC2 demo path:** api.main on port **8000**; Streamlit calls **POST /ask**.

## Endpoints

The table covers **api.main** and **api.app**. Auth column: ✓ = Cognito JWT required; — = no auth.

| Endpoint | Method | Auth | Desc |
| --- | --- | --- | --- |
| **/** | GET | — | api.main health check |
| **/ask** | POST | — | RAG Q&A; body question, optional top_k |
| **/api/health** | GET | — | api.app health check |
| **/api/chat** | POST | ✓ | RAG Q&A; optional DynamoDB history |
| **/api/conversations** | GET | ✓ | List the user’s conversations |
| **/api/conversations/{id}** | GET | ✓ | Get one conversation |
| **/api/conversations/{id}** | DELETE | ✓ | Delete a conversation |
| **/api/admin/conversations** | GET | ✓ | Conversations by day |
| **/api/admin/users** | GET | ✓ | List Cognito users |
| **/api/admin/users/{username}/disable** | POST | ✓ | Disable a user |
| **/api/admin/users/{username}/enable** | POST | ✓ | Enable a user |
| **/api/admin/users/{username}/group** | POST | ✓ | Assign Cognito group |
| **/api/admin/documents/upload-url** | POST | ✓ | S3 presigned upload URL |
| **/api/admin/documents** | GET | ✓ | List documents |
| **/api/admin/documents/{id}** | PATCH | ✓ | Update document metadata |
| **/api/admin/documents/{id}** | DELETE | ✓ | Soft-delete a document |

## Backend Architecture Diagram

{{< mermaid >}}
graph TB
    subgraph "Docker Compose (EC2)"
        ST["Streamlit :8501"] -->|POST /ask| API["FastAPI :8000"]
        API --> QA["QAService"]
    end

    QA --> EMB["Embedding<br/>(SentenceTransformer / Bedrock Titan)"]
    EMB --> VS["pgvector Search<br/>(RDS PostgreSQL)"]
    VS --> RR["Reranker<br/>(Cross-encoder)"]
    RR --> PR["Prompt Builder"]
    PR --> LLM["LLM<br/>(Gemini / Bedrock)"]
    LLM --> RES["Response + Sources"]

    API -->|JWT verify| COG["Cognito JWKS"]
    API -->|Chat history| DDB["DynamoDB"]
{{< /mermaid >}}

## Ingestion Flow Diagram

{{< mermaid >}}
sequenceDiagram
    participant Admin
    participant API as FastAPI
    participant S3 as S3
    participant SQS as SQS
    participant Lambda as Lambda
    participant RDS as RDS pgvector

    Admin->>API: POST /api/admin/documents/upload-url
    API-->>Admin: Presigned URL
    Admin->>S3: PUT file
    S3->>SQS: ObjectCreated event
    SQS->>Lambda: Trigger
    Lambda->>S3: Download file
    Lambda->>Lambda: Chunk + Embed
    Lambda->>RDS: Upsert vectors
    Note over SQS: Failure → DLQ
{{< /mermaid >}}

## Backend flows

### Flow 1 — Demo Q&A

Primary path: Streamlit → api.main → QAService.

1. Client sends **POST /ask** with question and top_k
2. FastAPI receives the request and calls QAService.ask
3. Normalize the question; optional rewrite / query variants
4. Retriever embeds the question → search on RDS pgvector
5. If no chunks → return the fixed “no info in DB” answer
6. Rerank when USE_RERANKER is enabled
7. build_prompt merges legal context + question
8. Generator calls Gemini or Bedrock
9. Return JSON: answer + sources

**Summary:** Question → Embed → Retrieve → Prompt → LLM → Grounded answer

### Flow 2 — Build index

Via scripts/build_index.py or pipeline.build_index_pipeline.

1. Load HF_DATASET_NAME / LOCAL_DEMO_PATH and connect to the vector store
2. Load existing chunk IDs to skip already indexed content
3. Iterate legal documents from the dataset
4. chunk_document creates overlapping passages
5. EmbeddingService embeds in batches
6. Insert into pgvector / FAISS
7. Commit periodically via INDEX_COMMIT_INTERVAL

**Summary:** Corpus → Chunk → Embed → Upsert vector store

### Flow 3 — Serverless ingestion

When an admin uploads a file to S3.

1. Admin calls **POST /api/admin/documents/upload-url** → receives upload URL
2. Client PUT/POST the file to S3
3. S3 event pushes a message to SQS
4. Lambda lambda_handler consumes the event
5. Read file → chunk → embed → write RDS pgvector
6. Skip existing chunk IDs for safe resume

**Summary:** Upload URL → S3 → SQS → Lambda → RDS

### Flow 4 — Cognito-protected API

api.app path with authentication enabled.

1. Client calls **/api/*** with Authorization Bearer header
2. auth.py verifies the JWT against Cognito JWKS
3. If AUTH_DISABLED=true → use a synthetic user with admins/editors/users
4. require_roles checks groups before admin routes
5. Handler calls QAService or admin/ingestion services
6. For **/api/chat**, history may be written to DynamoDB when ENABLE_CHAT_HISTORY is on

**Summary:** Bearer token → Verify Cognito → Check role → Business logic

## RAG Core

Main modules under **src/rag_core/**:

| Module | Role |
| --- | --- |
| config.py | Load settings from .env |
| dataset_reader.py | Read HuggingFace legal corpus |
| chunking.py | Overlapping character chunks |
| embeddings.py | Local or Bedrock Titan embeddings |
| vector_store.py | pgvector RDS or FAISS + SQLite |
| pipeline.py | build_index_pipeline and ask_pipeline |
| retriever.py | Embed query → vector search |
| reranker.py | Optional CrossEncoder |
| prompt.py | Grounded Vietnamese legal system prompt |
| generator.py | Gemini or Bedrock converse |
| qa_service.py | End-to-end ask orchestration |
| document_manager.py | PDF → chunk → embed → store |
| lambda_handler.py | S3/SQS ingestion worker |

### Query and index pipelines

Step-by-step detail is in **Backend flows** above. The modules in the table below are invoked by QAService and the pipeline in that order.

## Auth

- **Streamlit:** hashed username/password in the app DB — Cognito is not used on the demo UI
- **/api/*:** Cognito JWT via src/api/auth.py; Authorization Bearer header
- **AUTH_DISABLED=true:** synthetic user with admins/editors/users groups — Streamlit compose/dev only
- Cognito admin helpers: src/services/cognito_admin.py

## Configuration groups

Do not paste secrets into the report — document **variable names** only:

| Group | Example variables |
| --- | --- |
| Data / chunk | HF_DATASET_NAME, LOCAL_DEMO_PATH, CHUNK_SIZE_CHARS, CHUNK_OVERLAP_CHARS |
| Embedding | EMBEDDING_MODEL_NAME, USE_BEDROCK_EMBEDDING, BEDROCK_EMBEDDING_MODEL, TOP_K |
| LLM | LLM_PROVIDER, GEMINI_API_KEY, GEMINI_MODEL_NAME, BEDROCK_LLM_MODEL_ID |
| Postgres | USE_PGVECTOR, PGHOST, PGPORT, PGDATABASE, PGUSER, PGPASSWORD |
| AWS / S3 | AWS_DEFAULT_REGION, USE_S3, LEGAL_DOCUMENTS_BUCKET, VECTOR_S3_* |
| API / Cognito | AUTH_DISABLED, ENABLE_API_DOCS, COGNITO_USER_POOL_ID, COGNITO_APP_CLIENT_ID |
| DynamoDB / Chainlit | DYNAMODB_*, ENABLE_CHAT_HISTORY, CHAINLIT_DEV_* |
| Runtime | API_URL, APP_DB_BACKEND, API_MODULE, APP_MODE, PORT |
