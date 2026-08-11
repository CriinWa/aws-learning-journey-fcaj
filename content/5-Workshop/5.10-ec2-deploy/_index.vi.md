---
title: "Deploy Docker trên EC2"
date: 2024-01-01
weight: 10
chapter: false
pre: " <b> 5.10. </b> "
---

# EC2 — Deploy Docker

Demo production chạy hai container trên EC2 qua Docker Compose: **FastAPI** (port 8000) và **Streamlit** (port 8501). Streamlit gọi API qua Docker internal network.

![Deploy EC2](/images/5-Workshop/5.2-Prerequisite/ec2.png)

## Kiến trúc container

{{< mermaid >}}
graph LR;
    U["Trinh duyet"] -->|8501| ST["streamlit container"]
    ST -->|http://api:8000/ask| API["api container"]
    API --> RDS[("RDS")]
    API --> BR["Bedrock"]
    API --> S3["S3"]
{{< /mermaid >}}

| Container | Image / build | Port | Vai trò |
| --- | --- | --- | --- |
| **api** | `deploy/Dockerfile` | 8000 | FastAPI + QAService |
| **streamlit** | Cùng image, `APP_MODE=streamlit` | 8501 | Giao diện Streamlit |

File chính:

- `deploy/Dockerfile` — image Python app
- `deploy/docker-compose.yml` — định nghĩa service
- `deploy/entrypoint.sh` — chuyển `APP_MODE` (api / streamlit / chainlit)

## Các bước deploy

```bash
# Trên EC2
git clone <your-repo-url> vietnamese-legal-llmops
cd vietnamese-legal-llmops
cp .env.sample .env
# Sua .env: RDS, Bedrock, S3, Cognito outputs...

cd deploy
docker compose build
docker compose up -d
docker compose ps
docker compose logs -f api
```

## Biến `.env` quan trọng cho Compose

```
API_URL=http://api:8000/ask
USE_PGVECTOR=true
PGHOST=<rds-endpoint>
LLM_PROVIDER=bedrock
LEGAL_DOCUMENTS_BUCKET=<bucket-name>
AWS_DEFAULT_REGION=ap-southeast-1
```

EC2 nên dùng **IAM Instance Role** (không dùng Access Key) cho Bedrock và S3.

## Health check

| Kiểm tra | Lệnh / URL |
| --- | --- |
| API sống | `curl http://localhost:8000/` |
| Streamlit UI | `http://<ec2-public-ip>:8501` |
| End-to-end | Hỏi câu pháp luật trên chatbot |
| Log container | `docker compose logs api streamlit` |

![Hoàn tất deploy](/images/5-Workshop/5.2-Prerequisite/complete.png)

## IAM trên EC2

Gắn role với policy:

- `bedrock:InvokeModel`
- `s3:GetObject`, `s3:PutObject` (nếu app ghi S3)
- `dynamodb:*` (nếu bật chat history)

## Kiểm tra

- [ ] Cả hai container đang chạy (`docker compose ps`)
- [ ] Streamlit mở được :8501
- [ ] Câu hỏi chat trả answer + sources
- [ ] Log API kết nối Bedrock và RDS OK
- [ ] Không hard-code credential trong image/compose

Tiếp theo: **kiểm thử hệ thống** toàn bộ các lớp.
