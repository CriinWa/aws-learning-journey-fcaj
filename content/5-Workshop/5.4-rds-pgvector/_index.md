---
title: "RDS PostgreSQL + pgvector"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# RDS — PostgreSQL + pgvector

RDS PostgreSQL with the **pgvector** extension is the vector store for legal text chunks. QAService embeds questions and runs similarity search to retrieve relevant passages.

## Architecture role

{{< mermaid >}}
graph TB;
    Q["Question"] --> E["Embedding"]
    E --> S["Similarity search"]
    S --> PG[("RDS legal_chunks")]
    PG --> C["Top-k chunks"]
    C --> P["Prompt + LLM"]
{{< /mermaid >}}

## Step 1 — Create RDS instance

1. **Amazon RDS** → **Create database** → **PostgreSQL**
2. Engine: PostgreSQL 15+ (supports pgvector)
3. Instance: `db.t3.micro` or `db.t3.small` for lab
4. Storage: 20 GB+ gp3
5. VPC: same as EC2; **not publicly accessible** unless required for lab
6. Security Group: inbound **5432** from EC2 SG only

## Step 2 — Enable pgvector and create table

Connect with psql or a SQL client:

```sql
CREATE EXTENSION IF NOT EXISTS vector;

CREATE TABLE IF NOT EXISTS legal_chunks (
    chunk_id    TEXT PRIMARY KEY,
    doc_id      TEXT,
    title       TEXT,
    content     TEXT,
    embedding   vector(1024),  -- dimension matches embedding model
    metadata    JSONB,
    created_at  TIMESTAMPTZ DEFAULT NOW()
);
```

Run migrations in `migrations/` if provided by the project.

{{% notice note %}}
HNSW / IVFFlat indexes are optional optimizations for large corpora — not required for the workshop lab.
{{% /notice %}}

## Step 3 — Configure `.env`

```
USE_PGVECTOR=true
PGHOST=<rds-endpoint>.ap-southeast-1.rds.amazonaws.com
PGPORT=5432
PGDATABASE=legalchatbot
PGUSER=postgres
PGPASSWORD=<password>
```

## Step 4 — Build index

**Local / EC2 one-shot:**

```bash
python scripts/build_index.py
```

**Cloud ingestion:** Lambda writes chunks after S3 upload (see [5.5 Lambda](5.5-lambda/)).

## Query behavior

| Setting | Purpose |
| --- | --- |
| `TOP_K` | Number of chunks returned per question |
| Query timeout | Prevents long-running similarity searches |
| Exact search fallback | Used when HNSW/IVFFlat index is not created |

## Verification

- [ ] EC2 can connect to RDS on port 5432
- [ ] `CREATE EXTENSION vector` succeeds
- [ ] Table `legal_chunks` exists with rows after index build
- [ ] Sample similarity query returns relevant chunks

Next: configure **Lambda ingestion** for automatic document processing.
