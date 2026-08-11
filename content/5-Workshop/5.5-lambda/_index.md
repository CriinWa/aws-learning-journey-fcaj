---
title: "Lambda — Ingestion"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

# Lambda — Ingestion

AWS Lambda processes new legal documents uploaded to S3. It reads PDF/TXT, chunks text, creates embeddings, and writes vectors to RDS pgvector — triggered via SQS.

## End-to-end flow

{{< mermaid >}}
sequenceDiagram
    participant S3 as S3
    participant SQS as SQS
    participant L as Lambda
    participant B as Bedrock
    participant RDS as RDS pgvector
    S3->>SQS: ObjectCreated event
    SQS->>L: Trigger batch
    L->>S3: Read file + manifest
    L->>L: Chunk document
    L->>B: Create embeddings
    B-->>L: Vectors
    L->>RDS: Upsert legal_chunks
    Note over SQS: Failed messages → DLQ
{{< /mermaid >}}

## SQS and DLQ

| Resource | Purpose |
| --- | --- |
| **Ingestion queue** | Receives S3 event messages |
| **Dead Letter Queue (DLQ)** | Stores messages that fail after retries |

CloudFormation in `infra/foundation.yaml` can create both queues. Configure the main queue with a redrive policy pointing to the DLQ.

## Lambda handler

Entry point: **`src/rag_core/lambda_handler.py`**

Processing steps:

1. Parse S3/SQS event → get bucket, key
2. Read file from `incoming/files/` and matching manifest from `incoming/manifests/`
3. Chunk text (`chunking.py`)
4. Create embeddings (`embeddings.py` — Bedrock or local)
5. Upsert into RDS `legal_chunks` (`vector_store.py`)
6. Skip existing `chunk_id` values for safe resume
7. Report partial batch failures to SQS when enabled

## Deployment settings

| Setting | Recommendation |
| --- | --- |
| Runtime | Python 3.11+ |
| Timeout | 5–15 minutes (depends on file size) |
| Memory | 1024 MB+ |
| VPC | Same subnets as RDS; SG allows 5432 to RDS |
| IAM Role | `s3:GetObject`, `sqs:*`, `bedrock:InvokeModel`, RDS network access |

## Environment variables (Lambda)

Same database and Bedrock variables as EC2 `.env`:

```
USE_PGVECTOR=true
PGHOST=...
LLM_PROVIDER=bedrock
USE_BEDROCK_EMBEDDING=true
BEDROCK_EMBEDDING_MODEL=amazon.titan-embed-text-v2:0
LEGAL_DOCUMENTS_BUCKET=...
```

## Verification

- [ ] Upload PDF/TXT to S3 → SQS receives message
- [ ] Lambda logs show chunk + embed success
- [ ] New rows appear in `legal_chunks`
- [ ] Failed test message lands in DLQ after retries

Next: configure **Amazon Bedrock** for embedding and LLM.
