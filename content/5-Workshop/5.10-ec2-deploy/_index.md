---
title: "Docker deploy on EC2"
date: 2024-01-01
weight: 10
chapter: false
pre: " <b> 5.10. </b> "
---

# EC2 — Docker deploy

Production demo runs two containers on EC2 via Docker Compose: **FastAPI** (port 8000) and **Streamlit** (port 8501). Streamlit calls the API over the Docker internal network.

![EC2 deploy](/images/5-Workshop/5.2-Prerequisite/ec2.png)

## Container architecture

{{< mermaid >}}
graph LR;
    U["User browser"] -->|8501| ST["streamlit container"]
    ST -->|http://api:8000/ask| API["api container"]
    API --> RDS[("RDS")]
    API --> BR["Bedrock"]
    API --> S3["S3"]
{{< /mermaid >}}

| Container | Image / build | Port | Role |
| --- | --- | --- | --- |
| **api** | `deploy/Dockerfile` | 8000 | FastAPI + QAService |
| **streamlit** | Same image, `APP_MODE=streamlit` | 8501 | Streamlit UI |

Key files:

- `deploy/Dockerfile` — Python app image
- `deploy/docker-compose.yml` — service definitions
- `deploy/entrypoint.sh` — switches `APP_MODE` (api / streamlit / chainlit)

## Deploy steps

```bash
# On EC2
git clone <your-repo-url> vietnamese-legal-llmops
cd vietnamese-legal-llmops
cp .env.sample .env
# Edit .env: RDS, Bedrock, S3, Cognito outputs...

cd deploy
docker compose build
docker compose up -d
docker compose ps
docker compose logs -f api
```

## Important `.env` values for Compose

```
API_URL=http://api:8000/ask
USE_PGVECTOR=true
PGHOST=<rds-endpoint>
LLM_PROVIDER=bedrock
LEGAL_DOCUMENTS_BUCKET=<bucket-name>
AWS_DEFAULT_REGION=ap-southeast-1
```

EC2 should use an **IAM Instance Role** (not Access Keys) for Bedrock and S3.

## Health checks

| Check | Command / URL |
| --- | --- |
| API alive | `curl http://localhost:8000/` |
| Streamlit UI | `http://<ec2-public-ip>:8501` |
| End-to-end | Ask a legal question in chatbot UI |
| Container logs | `docker compose logs api streamlit` |

![Deploy complete](/images/5-Workshop/5.2-Prerequisite/complete.png)

## IAM on EC2

Attach a role with policies for:

- `bedrock:InvokeModel`
- `s3:GetObject`, `s3:PutObject` (if app writes to S3)
- `dynamodb:*` (if chat history enabled)

## Verification

- [ ] Both containers running (`docker compose ps`)
- [ ] Streamlit loads on :8501
- [ ] Chat question returns answer + sources
- [ ] API logs show Bedrock and RDS connections OK
- [ ] No credentials hard-coded in image or compose file

Next: run **system testing** across all layers.
