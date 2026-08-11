---
title: "RDS PostgreSQL + pgvector"
date: 2026-08-11
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

# RDS — PostgreSQL + pgvector

RDS PostgreSQL with **pgvector** extension serves as the vector store. QAService embeds the question and runs similarity search to retrieve relevant legal passages.

{{< mermaid >}}
graph TB
    Q["Question"] --> E["Embedding"]
    E --> S["Similarity search"]
    S --> PG[("RDS legal_chunks")]
    PG --> C["Top-k chunks"]
    C --> P["Prompt + LLM"]
{{< /mermaid >}}

## Steps

1. Create RDS PostgreSQL 15+ instance in private subnet
2. Enable pgvector extension and create tables
3. Configure `.env` with connection details
4. Build vector index via `scripts/build_index.py`

Refer to the Vietnamese version for full details.
