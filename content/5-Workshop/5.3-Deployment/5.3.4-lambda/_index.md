---
title: "Lambda — Ingestion"
date: 2026-08-11
weight: 4
chapter: false
pre: " <b> 5.3.4. </b> "
---

# Lambda — Ingestion

AWS Lambda processes new legal documents uploaded to S3: reads PDF/TXT, chunks, creates embeddings, and writes vectors to RDS pgvector — triggered via SQS.

{{< mermaid >}}
sequenceDiagram
    participant S3 as S3
    participant SQS as SQS
    participant L as Lambda
    participant B as Bedrock
    participant RDS as RDS pgvector
    S3->>SQS: ObjectCreated event
    SQS->>L: Trigger batch
    L->>S3: Download file + manifest
    L->>L: Chunk document
    L->>B: Create embeddings
    B-->>L: Vectors
    L->>RDS: Upsert legal_chunks
    Note over SQS: Failed message → DLQ
{{< /mermaid >}}

## Steps

1. Create SQS queue + Dead Letter Queue
2. Deploy Lambda function with VPC access to RDS
3. Configure S3 event notification → SQS
4. Test end-to-end: upload PDF → verify vectors in RDS

Refer to the Vietnamese version for full details.
