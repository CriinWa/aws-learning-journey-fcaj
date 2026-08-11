---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Workshop: Vietnamese Legal RAG Chatbot

## Overview

This workshop presents the **Law-Chatbot** project — a Vietnamese legal Q&A system using **RAG (Retrieval-Augmented Generation)** on AWS, based on the **vietnamese-legal-llmops** codebase.

**Problem:** The Vietnamese legal corpus is large and fragmented (laws, decrees, circulars…). Users struggle to find the right articles and clauses.

**RAG solution:** Retrieve relevant passages → augment the prompt with context → generate answers **grounded in legal citations**, with source references.

**Core features:**

- Legal Q&A
- Retrieve related document sources
- Manage legal documents
- User management
- Conversation history

**Demo:** [http://18.143.187.153:8501/](http://18.143.187.153:8501/)

## AWS architecture

![Vietnamese Legal RAG Chatbot architecture](/images/2-Proposal/legal_chatbot_architecture.png)

**AWS services used:** EC2, ALB, VPC/Subnet/Security Group, S3, SQS + DLQ, Lambda, Bedrock, RDS PostgreSQL + pgvector, DynamoDB, Cognito, CloudFormation, IAM, CloudWatch/SNS.

## Three main flows

### 1. Ingestion flow

Admin uploads PDF/TXT to S3 → SQS receives event → Lambda chunks + embeds → stores in RDS pgvector.

{{< mermaid >}}
graph LR;
    A["Admin upload"] --> B["S3 bucket"]
    B --> C["Manifest"]
    B -->|Event| D["SQS"]
    D --> E["Lambda"]
    E --> F["Chunk + Embed"]
    F --> G[("RDS pgvector")]
    D -.->|Failed| H["DLQ"]
{{< /mermaid >}}

### 2. Q&A flow

User asks via Streamlit → FastAPI embeds question → similarity search on pgvector → build prompt → Bedrock LLM → return answer + sources.

{{< mermaid >}}
sequenceDiagram
    participant U as Streamlit
    participant API as FastAPI
    participant QA as QAService
    participant EMB as BedrockEmbed
    participant PG as RDS
    participant LLM as BedrockLLM
    U->>API: POST question
    API->>QA: ask
    QA->>EMB: embed question
    EMB-->>QA: vector
    QA->>PG: similarity search
    PG-->>QA: top-k chunks
    QA->>LLM: generate answer
    LLM-->>QA: answer
    QA-->>API: answer and sources
    API-->>U: JSON response
{{< /mermaid >}}

### 3. Operations flow

CloudFormation creates foundation resources → Docker Compose deploys on EC2 → Cognito secures admin API → CloudWatch monitors.

## Workshop contents

| Section | Content |
| --- | --- |
| [5.1 Overview — Project architecture](5.1-Workshop-overview/) | Introduction, Frontend/Backend/AWS architecture, functional spec |
| ↳ [5.1.1 Frontend](5.1-Workshop-overview/5.1.1-frontend/) | Streamlit UI, views, API connection |
| ↳ [5.1.2 Backend](5.1-Workshop-overview/5.1.2-backend/) | FastAPI, RAG Core, processing flows |
| [5.2 Prerequisites](5.2-Prerequisite/) | Source code, data, IAM, RDS, EC2/Docker |
| [5.3 S3 — Upload documents](5.3-s3-upload/) | Bucket, presigned upload, incoming prefix |
| [5.4 RDS — PostgreSQL + pgvector](5.4-rds-pgvector/) | Vector store, legal_chunks table |
| [5.5 Lambda — Ingestion](5.5-lambda/) | Process new documents from S3/SQS |
| [5.6 Bedrock — Embedding and LLM](5.6-bedrock/) | Embedding and answer generation models |
| [5.7 DynamoDB — Chat history](5.7-dynamodb/) | Conversation storage |
| [5.8 Cognito — Auth and RBAC](5.8-cognito/) | JWT, users/editors/admins groups |
| [5.9 VPC — Network](5.9-vpc/) | Subnet, Security Group, endpoints |
| [5.10 EC2 — Docker deploy](5.10-ec2-deploy/) | Docker Compose, api + streamlit containers |
| [5.11 System testing](5.11-system-testing/) | Test Frontend, RAG, AWS services, performance |
| [5.12 Summary](5.12-summary/) | Results, limitations, cost, next steps |

## Tech stack

| Layer | Choice |
| --- | --- |
| UI | Streamlit, Chainlit |
| API | FastAPI + uvicorn |
| RAG | Custom Python in `src/rag_core` |
| Embedding | Local Vietnamese model or Bedrock Titan |
| LLM | Gemini or Amazon Bedrock Converse |
| Vector store | PostgreSQL + pgvector |
| UI auth | Username/password in app DB |
| API auth | Cognito JWT |
| Deploy | Docker Compose on EC2 |
| IaC | `infra/foundation.yaml`, `migrations/*.sql` |
