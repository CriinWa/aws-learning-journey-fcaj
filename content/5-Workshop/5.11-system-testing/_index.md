---
title: "System Testing"
date: 2024-01-01
weight: 11
chapter: false
pre: " <b> 5.11. </b> "
---

# System Testing

Test layer by layer to confirm the system works end-to-end before going live.

{{< mermaid >}}
graph TB;
    T1["Frontend"] --> T2["Backend RAG"]
    T2 --> T3["AWS Services"]
    T3 --> T4["Docker EC2 Benchmark"]
{{< /mermaid >}}

## 5.11.1. Frontend testing

| Test case | Check |
| --- | --- |
| Login screen | Form works; Inactive users blocked |
| Register screen | Username/email/password validation |
| Chatbot screen | Send question → receive answer + sources |
| Admin screen | KPI dashboard, user management |
| Session state | Chat sessions persist and restore correctly |
| Source display | Expander shows title, snippet, score |

## 5.11.2. FastAPI testing

| Endpoint | Check |
| --- | --- |
| `GET /` | Health check returns 200 |
| `POST /ask` | Request `{question, top_k}` → response `{answer, sources}` |
| QAService error | Clear error when service not ready |

## 5.11.3. RAG pipeline testing

Step-by-step flow:

1. **Read data** — PDF/TXT corpus loads correctly
2. **Chunking** — chunks have overlap, context preserved
3. **Embedding** — vector dimension matches model
4. **Retrieval** — top-k chunks relevant to sample questions
5. **Prompt** — legal context merged in correct format
6. **Generation** — LLM answers from context, minimal hallucination
7. **Final answer** — answer + sources consistent

## 5.11.4. RDS pgvector testing

- Database connection succeeds
- Table `legal_chunks` exists with vector column
- Insert embedding works
- Similarity search returns sensible results
- Query timeout configured
- Exact search fallback when HNSW/IVFFlat not used

## 5.11.5. S3/SQS/Lambda ingestion testing

1. Upload PDF/TXT to S3 prefix `incoming/files`
2. Verify manifest in `incoming/manifests`
3. SQS receives S3 event
4. Lambda processes file → chunk + embed
5. Chunk/embedding saved to RDS
6. Failed messages routed to DLQ

## 5.11.6. Bedrock testing

- Bedrock embedding call succeeds
- Bedrock LLM generates answer
- Timeout handled correctly
- Model ID matches region configuration
- Compare quality with dev provider (Gemini/local) if needed

## 5.11.7. DynamoDB chat history testing

- Create new conversation
- Save user/assistant messages
- List conversations by user (GSI)
- Get messages by conversation ID
- Delete conversation
- TTL auto-expires old data

## 5.11.8. Cognito/RBAC testing

| Group | Check |
| --- | --- |
| users | Access chat API |
| editors | Upload documents |
| admins | Manage users, view logs |
| No token | API returns 401/403 |
| Admin service | List/enable/disable user, assign group |

## 5.11.9. Docker/EC2 testing

- Docker image builds successfully
- `docker compose up` runs both containers
- Container `api` health check passes
- Container `streamlit` reachable on port 8501
- Streamlit calls FastAPI via Docker network
- EC2 connects to RDS and Bedrock

## 5.11.10. Performance testing

Run benchmark with `scripts/benchmark_qa.py`:

| Metric | Description |
| --- | --- |
| Total latency | Time from question to answer |
| Embedding time | Question vector creation |
| DB search time | pgvector similarity search |
| LLM time | Answer generation |
| p50 / p95 / max | Latency percentiles on test environment |

Next: [Workshop summary](5.12-summary/).
