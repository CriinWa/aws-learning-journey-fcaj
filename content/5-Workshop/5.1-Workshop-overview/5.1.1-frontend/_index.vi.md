---
title: "Frontend"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1.1 </b> "
---

# Frontend

Phần Frontend của Law-Chatbot có **hai giao diện**. Trên demo EC2, UI chính là **Streamlit**. **Chainlit** là phương án chat RAG thay thế, gọi QAService trực tiếp trong process.

| UI | Vai trò trong workshop | Kết nối Backend |
| --- | --- | --- |
| **Streamlit** | UI sản phẩm chính, demo EC2 cổng **8501** | HTTP **POST /ask** tới api.main |
| **Chainlit** | UI RAG tương thích / thử nghiệm | In-process QAService |

## Streamlit — UI chính

### Entry và cấu hình

- File entry: **streamlit_app.py**
- Tiêu đề trang: Trợ lý Pháp luật Việt Nam; layout wide; CSS **assets/style.css**
- Khi khởi động: initialize_database(), khởi tạo session state
- Router: chưa đăng nhập → register hoặc login; đã đăng nhập + role admin → views.admin; còn lại → views.chatbot
- Cổng mặc định **8501** (.streamlit/config.toml và Docker Compose)
- Chạy local: streamlit run streamlit_app.py
- Docker: APP_MODE=streamlit trong deploy/entrypoint.sh

### Các view

| View | File | Trách nhiệm |
| --- | --- | --- |
| Login | views/login.py | Form đăng nhập → authenticate_user; chặn user Inactive; gán role |
| Register | views/register.py | Validate username/email/password → create_user với role user |
| Chatbot | views/chatbot.py | Phiên chat, gợi ý câu hỏi, gọi API, hiển thị nguồn, feedback |
| Admin | views/admin.py | Dashboard KPI, quản lý user, log CSV, cấu hình top_k / temperature / model |

### Cách gọi API

Trong **views/chatbot.py**:

- Biến môi trường **API_URL** — mặc định hướng tới **http://127.0.0.1:8000/ask**; tự nối **/ask** nếu thiếu
- Trên Compose: API_URL=http://api:8000/ask
- Hàm process_user_query gửi POST JSON với question và top_k
- Kỳ vọng JSON trả về: answer, sources với title / snippet / score
- **Lịch sử chat không đi qua API** — lưu qua src/storage trên SQLite hoặc Postgres

### Hành trình người dùng

**Người dùng cuối:** Đăng ký / Đăng nhập → Chatbot → tạo hoặc chọn phiên → gửi câu hỏi → nhận trả lời + expander nguồn → thích/không thích → đăng xuất.

**Admin:** Đăng nhập role admin → Admin Dashboard → xem KPI, quản lý user, log, settings → đăng xuất. Admin **không** đi vào chatbot view mặc định.

## Chainlit — UI thay thế

- Entry: **app.py**
- Chạy: chainlit run app.py hoặc scripts/run_chainlit.py; Docker APP_MODE=chainlit
- Dùng chung **QAService** với backend RAG
- Có thể ghi lịch sử lên DynamoDB khi ENABLE_CHAT_HISTORY=true
- Auth local chỉ bật khi set đủ CHAINLIT_DEV_USERNAME và CHAINLIT_DEV_PASSWORD; cần CHAINLIT_AUTH_SECRET
- Trong code hiện tại, UI **cập nhật sau khi ask xong** — chưa token-stream từng chữ bằng stream_token

## Biến môi trường liên quan Frontend

| Biến | Mục đích |
| --- | --- |
| API_URL | Endpoint **POST /ask** cho Streamlit |
| APP_DB_BACKEND | postgres hoặc sqlite cho users/chats |
| USE_PGVECTOR | Ảnh hưởng chọn backend app DB khi không set APP_DB_BACKEND |
| PGHOST, PGPORT, PGDATABASE, PGUSER, PGPASSWORD | Kết nối Postgres app DB |
| CHAINLIT_DEV_USERNAME, CHAINLIT_DEV_PASSWORD, CHAINLIT_AUTH_SECRET | Auth local Chainlit |
| QA_TOP_K, ENABLE_CHAT_HISTORY, DYNAMODB_* | Cấu hình Chainlit / lịch sử |

API_URL có thể không nằm trong .env.sample nhưng **bắt buộc** khi chạy Streamlit với FastAPI tách process.
