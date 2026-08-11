---
title: "Deploy Docker on EC2"
date: 2026-08-11
weight: 8
chapter: false
pre: " <b> 5.3.8. </b> "
---

# EC2 — Deploy Docker

Production demo runs two containers on EC2 via Docker Compose: **FastAPI** (port 8000) and **Streamlit** (port 8501). Streamlit calls the API via Docker internal network.

{{< mermaid >}}
graph LR
    U["Browser"] -->|8501| ST["streamlit container"]
    ST -->|"http://api:8000/ask"| API["api container"]
    API --> RDS[("RDS")]
    API --> LLM["Gemini"]
    API --> S3["S3 (optional)"]
{{< /mermaid >}}

| Container | Port | Role |
| --- | --- | --- |
| **api** | 8000 | FastAPI + QAService |
| **streamlit** | 8501 | Streamlit UI |

## Steps

1. Launch EC2 instance (t3.medium or t2.medium, Ubuntu 22.04)
2. Install Docker and Docker Compose
3. Clone repo, configure `.env`
4. `docker compose build && docker compose up -d`
5. Verify: health check, Streamlit UI, end-to-end Q&A

Refer to the Vietnamese version for full details.
