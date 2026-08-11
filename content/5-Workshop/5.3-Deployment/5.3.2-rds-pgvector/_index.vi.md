---
title: "RDS PostgreSQL + pgvector"
date: 2026-08-11
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---
# Triển khai Amazon RDS PostgreSQL

**Mục tiêu:** Khởi tạo cơ sở dữ liệu quan hệ Amazon RDS PostgreSQL có tích hợp extension `pgvector`. RDS sẽ đóng vai trò là vector store để lưu trữ các chunk văn bản luật, cho phép QAService thực hiện embedding và chạy similarity search để truy xuất các đoạn văn liên quan nhất.

## Tổng quan Sơ đồ Kiến trúc

![1786474957256](image/_index.vi/1786474957256.png)

---

## Bước 1: Khởi tạo RDS instance

Quá trình này thiết lập một database instance an toàn, nằm gọn trong mạng nội bộ (Private Subnet) và chỉ cho phép kết nối từ hệ thống của dự án.

* Truy cập vào AWS Management Console, tìm kiếm dịch vụ **RDS** và chọn **Databases** ở menu bên trái.
* Nhấn vào nút màu cam **Create database**.

![1786468170065](image/_index.vi/1786468170065.png)

* **Engine options** chọn **PostgreSQL**.

![1786468416661](image/_index.vi/1786468416661.png)

* Chọn phương pháp tạo cơ sở dữ liệu là **Full configuration**

![1786468631195](image/_index.vi/1786468631195.png)

* Trong phần **Settings**:

  * **DB instance identifier**: Nhập `vector-db-server`.
  * **Master username**: Nhập `postgres`.
  * **Master password**: Nhập`aws-law-chatbot`.

![1786469931116](image/_index.vi/1786469931116.png)

![1786469064842](image/_index.vi/1786469064842.png)

* Tại mục **Instance configuration**, chọn **Burstable classes** và cấu hình kích thước là `db.t4.micro`.
* Tại mục **Storage**, chọn loại **General Purpose SSD (gp3)** và dung lượng Allocated storage là **20 GB**.

![1786469135983](image/_index.vi/1786469135983.png)

* Trong phần **Connectivity**:
  * **Virtual private cloud (VPC)**: Chọn `law-chatbot-vpc` đã tạo ở bài trước.
  * **Public access**: Chọn **No** (Database không được phép truy cập từ Internet).
  * **VPC security group (firewall)**: Chọn **Choose existing** và chọn `law-chatbot-rds-sg`.

![1786469301809](image/_index.vi/1786469301809.png)

![1786469346246](image/_index.vi/1786469346246.png)

* Ở mục **Additional configuration** (phần Database options), nhập tên cho cơ sở dữ liệu khởi tạo ban đầu là `postgres`.

![1786469691414](image/_index.vi/1786469691414.png)

* Cuộn xuống cuối trang và nhấn **Create database**. Hệ thống sẽ mất khoảng 5-10 phút để khởi tạo.

![1786469714926](image/_index.vi/1786469714926.png)

* Khi trạng thái của database chuyển sang **Available**, sao chép giá trị ở mục **Endpoint** trong tab *Connectivity & security*.

![1786470046580](image/_index.vi/1786470046580.png)

---

## Bước 2: Bật pgvector và tạo bảng cấu trúc

* Cài đặt extension Database Clinent trên VSCode, bấm Add Conection và điền các thông số ở mục Endpoint.

![1786470519839](image/_index.vi/1786470519839.png)

* Sau khi kết nối thành công, mở cửa sổ Query (SQL Editor) và dán đoạn mã khởi tạo sau:

```sql
CREATE EXTENSION IF NOT EXISTS vector;

CREATE TABLE IF NOT EXISTS legal_chunks (
    chunk_id   TEXT PRIMARY KEY,
    doc_id     TEXT,
    title      TEXT,
    content    TEXT,
    embedding  vector(1024),  -- dimension khớp embedding model
    metadata   JSONB,
    created_at TIMESTAMPTZ DEFAULT NOW()
);
```

## Bước 3: Cấu hình biến môi trường (`.env`)

Điền các thông số vào file .env

USE_PGVECTOR=true

PGHOST=vector-db-server.c3cec066gyal.ap-southeast-1.rds.amazonaws.com

PGPORT=5432

PGDATABASE=postgres

PGUSER=postgres

PGPASSWORD=aws-law-chatbot

PGSSLMODE=require

## Bước 4: Build index (Đưa dữ liệu vào Database)

Sau khi cấu hình xong, tiến hành chạy kịch bản nhúng dữ liệu vào vector store.

* Mở Terminal (Command Prompt) tại thư mục gốc của dự án trên local.
* Gõ và thực thi lệnh sau:

```
python scripts/build_index.py
```

Chờ hệ thống thực hiện nhúng (embedding) và đẩy dữ liệu lên bảng `legal_chunks`.
