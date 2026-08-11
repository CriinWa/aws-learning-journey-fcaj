---
title: "Bedrock — Embedding and LLM"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

# Bedrock — Embedding and LLM

Amazon Bedrock provides managed embedding and LLM inference for the RAG pipeline on AWS — replacing local models or Gemini in cloud deployments.

## Two Bedrock roles

{{< mermaid >}}
graph LR;
    T["Legal text / question"] --> E["Bedrock Embedding"]
    E --> V["Vector"]
    V --> R["pgvector search"]
    R --> C["Retrieved chunks"]
    C --> L["Bedrock LLM"]
    L --> A["Grounded answer"]
{{< /mermaid >}}

| Role | When used | Example model |
| --- | --- | --- |
| **Embedding** | Index build, query embedding | `amazon.titan-embed-text-v2:0` |
| **LLM** | Answer generation from prompt | Claude / Titan via Converse API |

## Step 1 — Enable model access

1. Open **Amazon Bedrock** → **Model access**
2. Request access for embedding and chat models in **ap-southeast-1**
3. Wait for approval (usually minutes)

## Step 2 — Configure `.env`

```
LLM_PROVIDER=bedrock
BEDROCK_LLM_MODEL_ID=anthropic.claude-3-haiku-20240307-v1:0
USE_BEDROCK_EMBEDDING=true
BEDROCK_EMBEDDING_MODEL=amazon.titan-embed-text-v2:0
AWS_DEFAULT_REGION=ap-southeast-1
```

## Step 3 — IAM permissions

EC2 instance role or Lambda role needs:

```json
{
  "Effect": "Allow",
  "Action": ["bedrock:InvokeModel", "bedrock:InvokeModelWithResponseStream"],
  "Resource": "*"
}
```

Scope `Resource` to specific model ARNs in production.

## Code integration

| Module | Bedrock usage |
| --- | --- |
| `src/rag_core/embeddings.py` | Titan embeddings for chunks and questions |
| `src/rag_core/generator.py` | Converse API for Vietnamese legal answers |
| `src/rag_core/qa_service.py` | Orchestrates embed → retrieve → generate |

{{% notice info %}}
For local dev, set `LLM_PROVIDER=gemini` and `GEMINI_API_KEY` — Bedrock is the recommended cloud path on EC2 with an IAM role.
{{% /notice %}}

## Verification

- [ ] Model access enabled in console
- [ ] Test embedding call returns vector with expected dimension
- [ ] Test LLM call returns an answer from a sample prompt
- [ ] `POST /ask` returns grounded answer with sources on EC2

Next: set up **DynamoDB** for chat history.
