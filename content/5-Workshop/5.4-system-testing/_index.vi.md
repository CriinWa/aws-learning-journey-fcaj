---
title: "Kiểm thử hệ thống"
date: 2026-08-11
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# Kiểm thử hệ thống

Sau khi triển khai, chạy các kịch bản kiểm thử cơ bản để xác nhận hệ thống hoạt động end-to-end trước khi demo hoặc vận hành thử.

## 1. Kiểm thử Frontend

| Kịch bản | Kết quả mong đợi |
| --- | --- |
| Đăng ký tài khoản mới | Cognito tạo user thành công, vào được màn hình chat |
| Đăng nhập đúng / sai mật khẩu | Đúng → vào app; sai → báo lỗi rõ ràng |
| Gửi câu hỏi trên chatbot | Nhận `answer` + danh sách `sources` |
| User thường mở trang Admin | Bị từ chối hoặc không thấy chức năng admin |

## 2. Kiểm thử Backend

| Kịch bản | Kết quả mong đợi |
| --- | --- |
| `GET /` | Trả `200` (health check) |
| `POST /ask` với `{question, top_k}` | Trả `{answer, sources}` hợp lệ |
| Gọi API không có token | Bị chặn `401` / `403` |
| Câu hỏi rỗng hoặc thiếu field | Trả lỗi validate, không crash |

## 3. Kiểm thử RAG end-to-end

Dùng vài câu hỏi mẫu:

- *"Điều kiện kết hôn theo pháp luật Việt Nam là gì?"*
- *"Quyền và nghĩa vụ của người lao động theo Bộ luật Lao động?"*

| Tiêu chí | Kết quả mong đợi |
| --- | --- |
| Retrieval | Sources liên quan đến chủ đề câu hỏi |
| Generation | Câu trả lời dựa trên context, không bịa lung tung |
| Latency | Thời gian phản hồi chấp nhận được cho demo |
