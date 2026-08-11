---
title: "Overview"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# Overview — Kiến trúc dự án Law-Chatbot

## 5.1.1. Tổng quan đề tài

**Law-Chatbot** là hệ thống hỏi đáp pháp luật Việt Nam dựa trên **RAG** trong codebase **`vietnamese-legal-llmops`**. Hệ thống tập trung vào việc nhận câu hỏi pháp lý, truy xuất các đoạn văn bản liên quan trong kho dữ liệu, rồi tổng hợp câu trả lời có nguồn tham khảo.

**Bài toán:** Corpus luật dài, phân mảnh (Luật, Nghị định, Thông tư…); tìm đúng đoạn liên quan tốn thời gian và dễ bỏ sót ngữ cảnh.

**Lý do sử dụng RAG:** LLM thuần có thể trả lời thiếu căn cứ. RAG giúp hệ thống bám vào dữ liệu đã truy xuất trước khi sinh câu trả lời.

**Mục tiêu hệ thống theo repo hiện tại:**

- Tra cứu pháp luật qua giao diện chat
- Trả lời dựa trên ngữ cảnh retrieve được từ kho dữ liệu
- Quản lý user, phiên chat và feedback trong giao diện Streamlit
- Có cấu trúc code đủ rõ để chạy local hoặc Docker

**Phạm vi chức năng:**

| Chức năng | Mô tả |
| --- | --- |
| Hỏi đáp pháp luật | Gửi câu hỏi và nhận câu trả lời từ pipeline RAG |
| Truy xuất nguồn | Hiển thị đoạn luật liên quan và điểm tương đồng |
| Quản lý người dùng | Đăng ký, đăng nhập, khóa/mở user trong app DB |
| Quản lý phiên chat | Tạo, đổi tiêu đề, xóa và lưu lịch sử hội thoại |
| Quản trị cơ bản | Dashboard, log gần đây, cấu hình tham số ở mức UI |

## 5.1.2. Kiến trúc Frontend

Repo hiện có **hai giao diện**:

- **Streamlit**: giao diện chính cho người dùng và admin
- **Chainlit**: giao diện chat thay thế, gọi `QAService` trực tiếp trong process

**Các màn hình:**

| Màn hình / UI | File | Chức năng |
| --- | --- | --- |
| Login | `views/login.py` | Đăng nhập bằng tài khoản trong app DB |
| Register | `views/register.py` | Tạo tài khoản người dùng mới |
| Chatbot | `views/chatbot.py` | Gửi câu hỏi tới `API_URL`, hiển thị answer và sources |
| Admin | `views/admin.py` | KPI, quản lý user, logs, cấu hình tham số |
| Chainlit | `app.py` | Chat UI thay thế, gọi `QAService` trực tiếp |

{{< mermaid >}}
graph LR;
    A["streamlit_app.py"] --> B{"Dang nhap?"}
    B -->|Khong| C["Login / Register"]
    B -->|Co| D{"Role?"}
    D -->|user| E["Chatbot view"]
    D -->|admin| F["Admin view"]
    E -->|POST /ask| G["src/api/main.py"]
    H["app.py (Chainlit)"] --> I["QAService"]
{{< /mermaid >}}

**Giao diện Chatbot:** `views/chatbot.py` lấy `API_URL`, gửi `POST /ask`, sau đó hiển thị `answer` và `sources`. Lịch sử phiên chat được lưu qua `src/storage`.

**Giao diện Admin:** `views/admin.py` có các tab Dashboard, Quản lý người dùng, Logs và Cài đặt hệ thống.

Chi tiết: [Frontend](5.1.1-frontend/)

## 5.1.3. Kiến trúc Backend

Backend sử dụng **FastAPI** và có hai hướng chạy chính:

- `src/api/main.py`: API mỏng cho Streamlit, mở endpoint **`POST /ask`**
- `src/api/app.py` + `src/api/routes.py`: API đầy đủ với prefix **`/api`**

**Cấu trúc module:**

```
src/
├── api/          → API, routes, schema, auth
├── rag_core/     → dataset, chunking, embedding, retrieval, generation
├── services/     → chat history, ingestion, Cognito admin
├── storage/      → user, chat session, feedback
└── monitoring/   → logging, feedback store
```

**Luồng xử lý backend:**

1. Nhận câu hỏi từ Streamlit hoặc Chainlit
2. `QAService` gọi retriever để embed câu hỏi
3. Tìm các chunk liên quan trong vector store
4. Ghép prompt từ ngữ cảnh retrieve được
5. Gọi model sinh câu trả lời
6. Trả về `answer`, `results` hoặc `sources`

Chi tiết: [Backend](5.1.2-backend/)

## 5.1.4. Kiến trúc theo codebase

| Thành phần | Dấu vết trong codebase | Vai trò |
| --- | --- |
| **Streamlit UI** | `streamlit_app.py`, `views/` | Giao diện chính |
| **Chainlit UI** | `app.py`, `scripts/run_chainlit.py` | Giao diện chat thay thế |
| **FastAPI** | `src/api/main.py`, `src/api/app.py`, `src/api/routes.py` | Nhận request và expose API |
| **QAService** | `src/rag_core/qa_service.py` | Điều phối pipeline hỏi đáp |
| **Retriever / Generator** | `src/rag_core/retriever.py`, `generator.py`, `prompt.py` | Tìm kiếm và sinh câu trả lời |
| **Vector store** | `src/rag_core/vector_store.py` | Lưu và truy xuất embedding |
| **App storage** | `src/storage/` | User, session chat, feedback |
| **Docker deploy** | `deploy/Dockerfile`, `deploy/docker-compose.yml` | Chạy container hóa |

![Kiến trúc Vietnamese Legal RAG Chatbot](/images/2-Proposal/legal_chatbot_architecture.png)

Luồng thể hiện rõ nhất trong repo là: **Streamlit :8501** → **`POST /ask`** ở `src/api/main.py` → **`QAService`** → **vector store / model** → trả **answer + sources**.

## 5.1.5. Các dịch vụ AWS được sử dụng

| Dịch vụ | Mục đích |
| --- | --- |
| Amazon RDS PostgreSQL + pgvector | Đường chạy chính trong `.env.sample` và Compose |
| Amazon Bedrock | Có biến cấu hình cho embedding / LLM cloud |
| Amazon S3 | Có script đồng bộ và service ingestion |
| Amazon DynamoDB | Có `ChatHistoryStore` cho lịch sử chat |
| Amazon Cognito | Có lớp verify JWT và admin service |
| AWS CloudFormation | Có file `infra/foundation.yaml` |
| IAM / AWS SDK | Có biến môi trường và service tương ứng |

## 5.1.6. Đặc tả chức năng hệ thống

| Chức năng | Mô tả ngắn |
| --- | --- |
| Đăng ký/đăng nhập | Tài khoản local trong app DB cho Streamlit |
| Hỏi đáp pháp luật bằng RAG | `question` → retrieve → generate |
| Hiển thị nguồn tài liệu | Trả về `title`, `snippet`, `score` |
| Quản lý session/hội thoại | Tạo, đổi tên, xóa phiên chat |
| Thu thập feedback | Like / dislike trên câu trả lời |
| Quản lý người dùng | Admin tạo user, khóa / mở user |
| API bảo vệ theo vai trò | `users`, `editors`, `admins` trên `/api/*` |

## Cấu trúc repository

| Path | Vai trò |
| --- | --- |
| `streamlit_app.py` | Entry Streamlit |
| `app.py` | Entry Chainlit |
| `views/` | login, register, chatbot, admin |
| `.env.sample` | Mẫu biến môi trường |
| `src/api/` | `main.py`, `app.py`, `routes.py`, `auth.py` |
| `src/rag_core/` | `dataset_reader`, `chunking`, `embeddings`, `retriever`, `generator`, `qa_service` |
| `src/storage/` | `sqlite_store.py`, `postgres_store.py` |
| `src/services/` | `chat_history.py`, `ingestion.py`, `cognito_admin.py` |
| `scripts/` | `build_index.py`, `run_api.py`, `run_chainlit.py`, `sync_to_s3.py` |
| `deploy/` | `Dockerfile`, `docker-compose.yml`, `entrypoint.sh` |
| `infra/` | `foundation.yaml` |

## Các phần kiến trúc chi tiết

1. [Frontend](5.1.1-frontend/)
2. [Backend](5.1.2-backend/)
