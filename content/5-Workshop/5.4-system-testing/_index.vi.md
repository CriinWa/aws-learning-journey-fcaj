---
title: "Kiểm thử hệ thống"
date: 2026-08-11
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# Kiểm thử hệ thống

Kiểm thử theo từng lớp, đảm bảo hệ thống hoạt động end-to-end trước khi đưa vào vận hành.

{{< mermaid >}}
graph TB;
    T1["Frontend"] --> T2["Backend RAG"]
    T2 --> T3["AWS Services"]
    T3 --> T4["Docker EC2 Benchmark"]
{{< /mermaid >}}

## 5.11.1. Kiểm thử Frontend

| Test case | Kiểm tra |
| --- | --- |
| Màn hình login | Form đăng nhập, chặn user Inactive |
| Màn hình register | Validate username/email/password |
| Màn hình chatbot | Gửi câu hỏi, nhận answer + sources |
| Màn hình admin | Dashboard KPI, quản lý user |
| Session state | Phiên chat được lưu và khôi phục đúng |
| Hiển thị nguồn | Expander sources hiển thị title, snippet, score |

## 5.11.2. Kiểm thử FastAPI

| Endpoint | Kiểm tra |
| --- | --- |
| `GET /` | Health check trả 200 |
| `POST /ask` | Request `{question, top_k}` → response `{answer, sources}` |
| Lỗi QAService | Trả lỗi rõ ràng khi service chưa sẵn sàng |

## 5.11.3. Kiểm thử RAG Pipeline

Luồng kiểm thử từng bước:

1. **Đọc dữ liệu** — corpus PDF/TXT load đúng
2. **Chunking** — đoạn cắt có overlap, không mất ngữ cảnh
3. **Embedding** — vector có dimension đúng model
4. **Retrieval** — top-k chunk liên quan với câu hỏi mẫu
5. **Prompt** — ngữ cảnh luật được ghép đúng format
6. **Generation** — LLM trả lời dựa trên context, không hallucinate
7. **Câu trả lời cuối** — answer + sources nhất quán

## 5.11.4. Kiểm thử RDS pgvector

- Kết nối database thành công
- Bảng `legal_chunks` tồn tại với cột vector
- Insert embedding hoạt động
- Similarity search trả kết quả hợp lý
- Timeout query được cấu hình
- Fallback exact search khi không dùng HNSW/IVFFlat

## 5.11.5. Kiểm thử S3/SQS/Lambda Ingestion

1. Upload file PDF/TXT lên S3 prefix `incoming/files`
2. Kiểm tra manifest trong `incoming/manifests`
3. SQS nhận event từ S3
4. Lambda xử lý file → chunk + embed
5. Chunk/embedding được lưu vào RDS
6. Message lỗi được chuyển sang DLQ

## 5.11.6. Kiểm thử Bedrock

- Gọi Bedrock embedding thành công
- Gọi Bedrock LLM sinh câu trả lời
- Timeout được xử lý đúng
- Model ID cấu hình đúng region
- So sánh chất lượng với provider dev (Gemini/local) nếu cần

## 5.11.7. Kiểm thử DynamoDB Chat History

- Tạo conversation mới
- Lưu message user/assistant
- Lấy danh sách conversation theo user (GSI)
- Lấy message theo conversation ID
- Xóa conversation
- TTL tự động xóa dữ liệu cũ

## 5.11.8. Kiểm thử Cognito/RBAC

| Nhóm | Kiểm tra |
| --- | --- |
| users | Truy cập chat API |
| editors | Upload tài liệu |
| admins | Quản lý user, xem log |
| Không token | API bị chặn 401/403 |
| Admin service | List/enable/disable user, gán group |

## 5.11.9. Kiểm thử Docker/EC2

- Build Docker image thành công
- `docker compose up` chạy cả 2 container
- Container `api` health check pass
- Container `streamlit` truy cập port 8501
- Streamlit gọi được FastAPI qua Docker network
- EC2 kết nối RDS và Bedrock

## 5.11.10. Kiểm thử hiệu năng

Chạy benchmark bằng `scripts/benchmark_qa.py`:

| Metric | Mô tả |
| --- | --- |
| Latency tổng | Thời gian từ gửi câu hỏi đến nhận answer |
| Embedding time | Thời gian tạo vector câu hỏi |
| DB search time | Thời gian similarity search trên pgvector |
| LLM time | Thời gian sinh câu trả lời |
| p50 / p95 / max | Percentile latency trên môi trường test |
