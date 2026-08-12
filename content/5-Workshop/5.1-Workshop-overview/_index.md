---
title: "Workshop Overview"
date: 2026-08-11
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---
## Context

**Vietnamese Legal RAG Chatbot** is an intelligent question-answering system specialized for Vietnamese legal documents. It allows users to ask legal questions in Vietnamese, retrieves relevant legal clauses from a vector database, and generates accurate answers using Large Language Models (LLM).

The system serves three main user groups: citizens who need quick legal lookup, law students and researchers who need specific legal references, and administrators who manage legal documents, monitor system quality, and manage users. The application includes a FastAPI backend, a Streamlit frontend, and a PostgreSQL database with the pgvector extension.

**Repository:** [github.com/KhanhKoy/vietnamese-legal-llmops](https://github.com/KhanhKoy/vietnamese-legal-llmops)

## Problem Statement

Looking up Vietnamese legal documents currently has major challenges: a large volume of documents spread across many sources, complex legal language that is difficult for ordinary citizens, and the lack of semantic search tools that understand question context. From a technical perspective, traditional keyword-matching chatbot solutions cannot capture the deeper meaning of legal questions, which leads to inaccurate responses.

**Vietnamese Legal RAG Chatbot** addresses this with RAG techniques: converting legal text into vector embeddings, storing them in a vector database (pgvector), and combining semantic retrieval with LLM generation to produce answers with citations. The system architecture is organized into two main flows: **Ingestion Pipeline** and **Query Processing.**

## Architecture Overview

Vietnamese Legal RAG Chatbot uses a **serverless ingestion** architecture combined with a **containerized application** on AWS in Region `ap-southeast-1`. Legal documents are uploaded through S3, processed asynchronously through SQS and Lambda; the application runs on EC2 with Docker Compose; vector data is stored in private RDS PostgreSQL.

### Five Architecture Layers

| Layer | Components | Primary Role |
| --- | --- | --- |
| Ingestion | Amazon S3, Amazon SQS, AWS Lambda, Dead Letter Queue | Receive legal documents, asynchronous processing, automatic chunking and embedding. |
| Embedding & Vector Store | SentenceTransformers (AITeamVN/Vietnamese_Embedding), Amazon RDS PostgreSQL + pgvector | Convert text into vector representations and store them with cosine similarity search. |
| Retrieval & Generation | Cosine Search, Google Gemini / Amazon Bedrock | Retrieve relevant context, rerank, and generate legal answers with citations. |
| Application | FastAPI, Streamlit, Docker Compose, EC2 | Provide API endpoints, chat interface, and admin dashboard. |
| Auth & Session | Amazon Cognito (JWT + RBAC), RDS (chat history) | Authenticate users by role groups, store conversation history with TTL. |

### Overall Architecture Diagram

![1786492866559](image/_index.vi/1786492866559.png)

### Main Processing Flow

![1786492900339](image/_index.vi/1786492900339.png)

## Tech Stack

| Layer | Technologies | Role |
| --- | --- | --- |
| Frontend | Streamlit | Chat UI, login/register, admin dashboard |
| Backend | FastAPI, Python 3.11, Gunicorn | API endpoints, RAG business logic, JWT auth |
| Embedding | SentenceTransformers (AITeamVN/Vietnamese_Embedding), Bedrock Titan | Vectorize legal texts and user queries |
| LLM | Google Gemini 2.5 Flash, Amazon Bedrock (Claude 3 / Llama 3) | Generate answers based on retrieved context |
| Vector Database | Amazon RDS PostgreSQL + pgvector (HNSW/IVFFlat index) | Store and search vector embeddings with high performance |
| Auth | Amazon Cognito (User Pool, Groups: users/editors/admins) | JWT authentication, three-tier RBAC |
| Session Storage | Amazon RDS | Store conversation history with automatic TTL |
| Ingestion | Amazon S3, Amazon SQS + DLQ, AWS Lambda | Asynchronous, fault-tolerant document processing pipeline |
| Containerization | Docker, Docker Compose | Package and deploy the application on EC2 |

## Legal Question Processing Flow

When a user asks a legal question, the system performs the following steps:

1. **User sends a question** -> **FastAPI receives the request** and validates JWT token via Cognito.
2. Backend **embeds the question** into a vector using a Vietnamese-specialized SentenceTransformer model.
3. Perform **cosine similarity search** on pgvector to retrieve the top-k relevant legal text chunks.
4. Build a **prompt** with legal context and send it to LLM (Gemini or Bedrock) to **generate an answer**.
5. Return the response with **citations** (legal sources) and **timings_ms** (processing time for each stage).

Simplified flow:

```text
User Question -> JWT Auth -> Embed Query -> pgvector Search
             -> Prompt Construction -> LLM Generation -> Response + Citations
```

## Two Main Processing Pipelines

### Ingestion Flow (Document Pipeline)

When an admin uploads new legal documents, the system processes them asynchronously so it does not affect active user chat experience. Documents are uploaded to **Amazon S3** via presigned URL; S3 event notifications push messages to **Amazon SQS**; an **AWS Lambda** consumer reads the message, downloads the file, performs text chunking with overlapping windows, embeds each chunk, and inserts vectors into **pgvector**. If processing fails, the message is sent to a **Dead Letter Queue** for retry or investigation.

```text
Admin Upload -> S3 Presigned URL -> S3 Bucket -> SQS Queue -> Lambda
           -> Download + Chunk + Embed -> pgvector Insert
           (failure) -> Dead Letter Queue -> Retry/Alert
```

### Query Flow (User Request Flow)

Users access the system through the **Streamlit** UI. The request is sent to the **FastAPI** backend and authenticated using **Cognito** JWT tokens. The backend executes the RAG pipeline: embed query, search pgvector, generate answer. Conversation history is stored in **RDS** with TTL to automatically delete old sessions and reduce storage cost.

```text
User -> Streamlit -> FastAPI (Cognito JWT Auth)
     -> RAG Pipeline (Embed -> Search -> Generate)
     -> RDS (Save conversation history)
     -> Response to User
```

## AWS Services Used

| Service | Purpose |
| --- | --- |
| Cognito | User authentication with User Pool, Groups (users/editors/admins), and JWT tokens. |
| RDS | Store conversation history with automatic TTL; on-demand throughput suitable for uneven workloads. |
| S3 | Store original legal documents, vector store backups, and presigned upload for ingestion. |
| SQS + DLQ | Message queue for asynchronous ingestion pipeline, ensuring at-least-once delivery and fault tolerance. |
| Lambda | Serverless ingestion processing: chunking, embedding, and vector insert into RDS. |
| RDS PostgreSQL | Managed database with pgvector extension, supporting HNSW/IVFFlat index for approximate nearest neighbor search. |
| Bedrock | Managed LLM service (Claude 3, Llama 3, Titan Embeddings) without GPU infrastructure management. |
| EC2 | Host Docker Compose application (FastAPI + Streamlit) for development and demo. |

## Backend Architecture

### Backend Source Structure

```text
src/
├── api/
│   ├── main.py          # Simple FastAPI app (POST /ask, no auth)
│   ├── app.py           # Full FastAPI app (CORS, Cognito JWT, /api/*)
│   ├── routes.py        # /api/chat, /api/conversations, /api/admin/*
│   ├── auth.py          # Cognito JWT verification & RBAC
│   └── schemas.py       # Pydantic request/response models
│
├── rag_core/            # Core RAG pipeline
│   ├── config.py        # Settings from .env + YAML
│   ├── dataset_reader.py# Read HuggingFace datasets / local parquet
│   ├── chunking.py      # Overlapping character-based text chunking
│   ├── embeddings.py    # SentenceTransformer or Bedrock Titan
│   ├── vector_store.py  # pgvector storage & cosine search
│   ├── retriever.py     # Retrieval orchestration
│   ├── reranker.py      # Cross-encoder reranking
│   ├── qa_service.py    # End-to-end ask() pipeline
│   ├── generator.py     # LLM generation (Gemini or Bedrock)
│   ├── prompt.py        # Prompt template construction
│   └── lambda_handler.py# AWS Lambda ingestion handler
│
├── services/
│   ├── chat_history.py  # DynamoDB conversation store
│   ├── cognito_admin.py # Cognito user management
│   ├── document_admin.py# Document CRUD (soft delete)
│   └── ingestion.py     # S3 presigned upload + manifest
│
├── storage/
│   ├── postgres_store.py# PostgreSQL app tables
│   └── sqlite_store.py  # SQLite fallback for local dev
│
└── evaluation/
    ├── eval_runner.py   # Offline evaluation
    └── metrics.py       # Recall@k, MRR
```

### QAService Processing Flow

`src/rag_core/qa_service.py` is the orchestration center of the RAG pipeline:

1. Receive question from API layer
2. Call `embeddings.py` to embed the question into a 768/1024-dimension vector
3. Call `vector_store.py` to run cosine similarity search on pgvector
4. Call `reranker.py` (if enabled) to cross-encoder rerank top-k -> top-n
5. Call `prompt.py` to build a prompt with legal context
6. Call `generator.py` to send prompt to LLM and receive answer
7. Return response with sources and timings

## Results Achieved

- Completed an end-to-end RAG pipeline for Vietnamese legal documents with high-quality retrieval using Vietnamese-specialized embeddings.
- Deployed the application with Docker Compose on EC2, with Streamlit.
- Delivered dual LLM providers (Google Gemini and Amazon Bedrock) with flexible switching through environment variables.
