---
title: "Backend"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.1.2 </b> "
---
# Backend


## Endpoints

| Endpoint                                      | Method | Auth | Desc                                               |
| --------------------------------------------- | ------ | ---- | -------------------------------------------------- |
| **/**                                   | GET    | —   | Health check api.main                              |
| **/ask**                                | POST   | —   | Hỏi đáp RAG; body question, top_k tùy chọn    |
| **/api/health**                         | GET    | —   | Health check api.app                               |
| **/api/chat**                           | POST   | ✓   | Hỏi đáp RAG; tùy chọn lưu lịch sử DynamoDB |
| **/api/conversations**                  | GET    | ✓   | Liệt kê hội thoại của user                    |
| **/api/conversations/{id}**             | GET    | ✓   | Chi tiết một hội thoại                         |
| **/api/conversations/{id}**             | DELETE | ✓   | Xóa hội thoại                                   |
| **/api/admin/conversations**            | GET    | ✓   | Lịch sử theo ngày                               |
| **/api/admin/users**                    | GET    | ✓   | Danh sách user Cognito                            |
| **/api/admin/users/{username}/disable** | POST   | ✓   | Vô hiệu hóa user                                |
| **/api/admin/users/{username}/enable**  | POST   | ✓   | Kích hoạt user                                   |
| **/api/admin/users/{username}/group**   | POST   | ✓   | Gán group Cognito                                 |
| **/api/admin/documents/upload-url**     | POST   | ✓   | Presigned S3 upload                                |
| **/api/admin/documents**                | GET    | ✓   | Danh sách tài liệu                              |
| **/api/admin/documents/{id}**           | PATCH  | ✓   | Cập nhật metadata tài liệu                     |
| **/api/admin/documents/{id}**           | DELETE | ✓   | Soft-delete tài liệu                             |

## Sơ đồ kiến trúc Backend

![1786491165710](image/_index.vi/1786491165710.png)

## Sơ đồ luồng Ingestion

![1786490883735](image/_index.vi/1786490883735.png)

## Các flow Backend

### Flow 1 — Hỏi đáp demo

Đường chính Streamlit → api.main → QAService.

1. Client gửi **POST /ask** với question và top_k
2. FastAPI nhận request, gọi QAService.ask
3. Chuẩn hóa câu hỏi; tùy chọn rewrite / mở rộng biến thể truy vấn
4. Retriever embed câu hỏi → search trên RDS pgvector
5. Nếu không có chunk → trả câu “không có thông tin trong DB”
6. Rerank nếu bật USE_RERANKER
7. build_prompt ghép ngữ cảnh luật + câu hỏi
8. Generator gọi Gemini hoặc Bedrock
9. Trả JSON: answer + sources

![1786490993207](image/_index.vi/1786490993207.png)

### Flow 2 — Build index

Chạy qua scripts/build_index.py hoặc pipeline.build_index_pipeline.

1. Đọc cấu hình HF_DATASET_NAME / LOCAL_DEMO_PATH và kết nối vector store
2. Lấy danh sách chunk ID đã có để bỏ qua phần đã index
3. Đọc từng văn bản pháp luật từ dataset
4. chunk_document cắt đoạn chồng lấp
5. EmbeddingService tạo vector theo batch
6. Insert vào pgvector / FAISS
7. Commit định kỳ theo INDEX_COMMIT_INTERVAL

![1786491027197](image/_index.vi/1786491027197.png)

### Flow 3 — Ingestion serverless

Khi admin upload file lên S3.

1. Admin gọi **POST /api/admin/documents/upload-url** → nhận URL upload
2. Client PUT/POST file lên S3
3. S3 event đẩy message vào SQS
4. Lambda lambda_handler nhận event
5. Đọc file → chunk → embed → ghi RDS pgvector
6. Bỏ qua chunk ID đã tồn tại để resume an toàn

![1786491049949](image/_index.vi/1786491049949.png)

### Flow 4 — API có Cognito

Đường api.app khi bật xác thực.

1. Client gửi request **/api/*** kèm header Authorization Bearer
2. auth.py xác thực JWT với Cognito JWKS
3. Nếu AUTH_DISABLED=true → dùng user giả có group admins/editors/users
4. require_roles kiểm tra group trước khi vào admin routes
5. Handler gọi QAService hoặc service admin/ingestion
6. Với **/api/chat**, có thể ghi lịch sử lên DynamoDB nếu ENABLE_CHAT_HISTORY

![1786491064764](image/_index.vi/1786491064764.png)

## RAG Core

Module chính trong **src/rag_core/**:

| Module              | Vai trò                                |
| ------------------- | --------------------------------------- |
| config.py           | Đọc settings từ .env                 |
| dataset_reader.py   | Đọc corpus HuggingFace                |
| chunking.py         | Cắt đoạn chồng lấp                 |
| embeddings.py       | Embedding local hoặc Bedrock Titan     |
| vector_store.py     | pgvector RDS hoặc FAISS + SQLite       |
| pipeline.py         | build_index_pipeline và ask_pipeline   |
| retriever.py        | Embed câu hỏi → search vector        |
| reranker.py         | CrossEncoder tùy chọn                 |
| prompt.py           | System prompt pháp luật có grounding |
| generator.py        | Gemini hoặc Bedrock converse           |
| qa_service.py       | Điều phối end-to-end ask             |
| document_manager.py | PDF → chunk → embed → store          |
| lambda_handler.py   | Worker ingestion S3/SQS                 |

## Auth

- **Streamlit:** username/password băm trong app DB — không dùng Cognito trên UI demo
- **/api/*:** Cognito JWT qua src/api/auth.py; header Authorization Bearer
- **AUTH_DISABLED=true:** tạo user giả có đủ group admins/editors/users — chỉ cho compose/dev gắn Streamlit
- Cognito admin: src/services/cognito_admin.py
