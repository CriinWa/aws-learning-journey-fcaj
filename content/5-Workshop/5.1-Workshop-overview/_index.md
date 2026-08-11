---
title: "Workshop Overview"
date: 2026-08-11
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---
## Context

**Vietnamese Legal RAG Chatbot** is an intelligent question-answering system specialized for Vietnamese legal documents. The system enables users to ask legal questions in Vietnamese, retrieves relevant legal provisions from a vector database, and generates accurate answers using Large Language Models (LLM).

The system serves three main user groups: citizens seeking quick access to legal information; law students and researchers needing specific legal provisions; and administrators managing legal documents, system quality, and user accounts. The application comprises a FastAPI backend, Streamlit/Chainlit frontend, and PostgreSQL database with pgvector extension.

## Problem Statement

Looking up Vietnamese legal documents currently faces significant challenges: large volume of documents scattered across multiple sources, complex legal language inaccessible to ordinary citizens, and lack of semantic search tools that understand question context. Technically, traditional keyword-based chatbot solutions cannot capture the deep meaning of legal queries, resulting in inaccurate responses.

**Vietnamese Legal RAG Chatbot** addresses this by applying RAG techniques: converting legal texts into vector embeddings, storing them in a vector database (pgvector), and combining semantic retrieval with LLM to generate cited answers.

## Architecture Overview

The system uses **serverless ingestion** combined with **containerized application** on AWS in the `ap-southeast-1` Region. Legal documents are uploaded via S3, processed asynchronously through SQS and Lambda; the application runs on EC2 with Docker Compose; vector data resides on private RDS PostgreSQL.

### Overall Architecture Diagram

{{< mermaid >}}
graph TB
    subgraph "User Layer"
        U["User"]
    end

    subgraph "Application Layer (EC2)"
        ST["Streamlit :8501"]
        CL["Chainlit"]
        API["FastAPI :8000"]
    end

    subgraph "RAG Pipeline"
        EMB["Embedding Service"]
        RET["Vector Search"]
        REK["Reranker"]
        GEN["LLM Generator"]
    end

    subgraph "AWS Services"
        RDS[("RDS PostgreSQL<br/>+ pgvector")]
        S3["S3 Documents"]
        SQS["SQS Queue"]
        LAM["Lambda"]
        BED["Bedrock"]
        COG["Cognito"]
        DDB["DynamoDB"]
    end

    U --> ST
    U --> CL
    ST --> API
    CL --> API
    API --> EMB
    EMB --> RET
    RET --> RDS
    RET --> REK
    REK --> GEN
    GEN --> BED

    API --> COG
    API --> DDB

    S3 --> SQS
    SQS --> LAM
    LAM --> RDS
{{< /mermaid >}}

### Query Processing Flow

{{< mermaid >}}
sequenceDiagram
    participant U as User
    participant FE as Streamlit
    participant BE as FastAPI
    participant E as Embedding
    participant DB as pgvector (RDS)
    participant R as Reranker
    participant L as LLM (Gemini/Bedrock)

    U->>FE: Ask legal question
    FE->>BE: POST /ask
    BE->>E: Embed query
    E-->>BE: Vector (768d)
    BE->>DB: Cosine similarity search
    DB-->>BE: Top-K chunks
    BE->>R: Rerank chunks
    R-->>BE: Top-N relevant
    BE->>L: Prompt + Context
    L-->>BE: Generated answer
    BE-->>FE: Answer + Sources + Timings
    FE-->>U: Display result
{{< /mermaid >}}

### Five Architecture Layers

| Layer | Components | Primary Role |
| --- | --- | --- |
| Ingestion | Amazon S3, Amazon SQS, AWS Lambda, DLQ | Receive legal documents, async processing, auto chunking and embedding. |
| Embedding & Vector Store | SentenceTransformers, RDS PostgreSQL + pgvector | Convert text to vector representations and store with cosine similarity search. |
| Retrieval & Generation | Cosine Search, Cross-encoder Reranker, Gemini / Bedrock | Retrieve relevant context, rerank, and generate cited legal answers. |
| Application | FastAPI, Streamlit, Chainlit, Docker Compose, EC2 | Provide API endpoints, chat interface, and admin dashboard. |
| Auth & Session | Amazon Cognito (JWT + RBAC), Amazon DynamoDB | Authenticate users by role groups, store conversation history with TTL. |

## Tech Stack

| Layer | Technologies | Role |
| --- | --- | --- |
| Frontend | Streamlit, Chainlit | Chat UI, login/register, admin dashboard |
| Backend | FastAPI, Python 3.11, Gunicorn | API endpoints, RAG business logic, JWT auth |
| Embedding | SentenceTransformers (AITeamVN/Vietnamese_Embedding), Bedrock Titan | Vectorize legal texts and user queries |
| LLM | Google Gemini 2.5 Flash, Amazon Bedrock (Claude 3 / Llama 3) | Generate answers based on retrieved context |
| Vector DB | Amazon RDS PostgreSQL + pgvector (HNSW/IVFFlat) | Store and search vector embeddings at scale |
| Auth | Amazon Cognito (users/editors/admins groups) | JWT authentication, three-tier RBAC |
| Session | Amazon DynamoDB | Conversation history with auto TTL |
| Ingestion | Amazon S3, SQS + DLQ, Lambda | Async document processing pipeline |
| IaC | AWS CloudFormation | Auto-provision Cognito, DynamoDB, S3, SQS |
| Container | Docker, Docker Compose | Package and deploy on EC2 |

## AWS Services Used

| Service | Purpose |
| --- | --- |
| Cognito | User authentication with User Pool, Groups, and JWT tokens. |
| DynamoDB | Conversation history storage with auto TTL. |
| S3 | Legal document storage, vector store backup, presigned upload. |
| SQS + DLQ | Async ingestion queue with at-least-once delivery. |
| Lambda | Serverless ingestion: chunking, embedding, vector insert. |
| RDS PostgreSQL | Managed database with pgvector for ANN search. |
| CloudFormation | Infrastructure as Code provisioning. |
| CloudWatch | Application logs and metrics collection. |
| Bedrock | Managed LLM service (Claude 3, Llama 3, Titan Embeddings). |
| EC2 | Host Docker Compose application for dev/demo. |
