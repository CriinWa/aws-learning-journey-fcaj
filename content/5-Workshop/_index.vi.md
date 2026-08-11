---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Workshop: Vietnamese Legal RAG Chatbot

## Tổng quan

Phần Workshop này chỉ trình bày **đến mục 5.2.2**, bám trực tiếp theo codebase **`vietnamese-legal-llmops`** hiện có trong workspace. Trọng tâm là:

- Tổng quan hệ thống Law-Chatbot
- Kiến trúc `Frontend` và `Backend`
- Các thành phần repo đang có sẵn
- Chuẩn bị source code và dữ liệu demo trước khi chạy

**Bài toán:** văn bản pháp luật Việt Nam dài, phân mảnh và khó tra cứu thủ công.

**Giải pháp trong repo:** dùng pipeline **RAG** để đọc dữ liệu, chia chunk, tạo embedding, truy xuất đoạn liên quan rồi sinh câu trả lời có nguồn tham khảo.

## Luồng chính trong codebase

{{< mermaid >}}
graph LR;
    A["Nguoi dung"] --> B["Streamlit / Chainlit"]
    B --> C["FastAPI / QAService"]
    C --> D["Retriever"]
    D --> E[("pgvector / vector store")]
    E --> F["Prompt + Generator"]
    F --> B
{{< /mermaid >}}

## Nội dung Workshop

| Mục | Nội dung |
| --- | --- |
| [5.1 Overview — Kiến trúc dự án](5.1-Workshop-overview/) | Giới thiệu hệ thống, Frontend, Backend, cấu trúc repo |
| ↳ [5.1.1 Frontend](5.1-Workshop-overview/5.1.1-frontend/) | Streamlit UI, các view, kết nối API |
| ↳ [5.1.2 Backend](5.1-Workshop-overview/5.1.2-backend/) | FastAPI, RAG Core, các flow xử lý |
| [5.2 Prerequisites — Chuẩn bị](5.2-Prerequisite/) | Chuẩn bị source code và dữ liệu demo để chạy repo |

## Tech stack

| Lớp | Theo repo hiện tại |
| --- | --- |
| UI | Streamlit, Chainlit |
| API | FastAPI + uvicorn |
| RAG | Python tự viết trong `src/rag_core` |
| Embedding | `AITeamVN/Vietnamese_Embedding` hoặc Bedrock embedding |
| LLM | Gemini mặc định, Bedrock là tùy chọn |
| Vector store | PostgreSQL + pgvector là đường chạy chính trong `.env.sample` |
| Auth UI | Username/password trong app DB |
| Auth API | Có sẵn lớp Cognito auth cho `/api/*` |
| Chạy local | Python local hoặc Docker Compose |
