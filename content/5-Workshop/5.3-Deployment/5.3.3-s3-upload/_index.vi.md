---
title: "S3 — Tạo Bucket"
date: 2026-08-11
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---
# S3 — Tạo bucket

## Tổng quan Sơ đồ Kiến trúc

![1786475100342](image/_index.vi/1786475100342.png)

## Tạo các bucket của dự án

1. Đăng nhập AWS Console tại Region ap-southeast-1, tìm Amazon S3 và chọn Buckets → Create bucket.
2. Đặt tên

![1786472818649](image/_index.vi/1786472818649.png)

![1786472832913](image/_index.vi/1786472832913.png)

3. Giữ nguyên các cài đặt
4. Ấn nút Tạo

![1786472896121](image/_index.vi/1786472896121.png)

Sau khi tạo xong thì ta ấn vào Bucket vừa tạo
Ấn vào tab Properties -> Tìm Bucket Versioning -> Edit -> Enable -> Save changes

![1786472922395](image/_index.vi/1786472922395.png)

![1786472929548](image/_index.vi/1786472929548.png)

Tạo DLQ

Vào **AWS Console** → **SQS** → **Queues** → **Create queue**.
Type: Standard

![1786472957161](image/_index.vi/1786472957161.png)

Tạo Main queue
Set Visibility Timeout: 60s

![1786472998924](image/_index.vi/1786472998924.png)

Tìm phần Dead-letter-queue
Chọn Enable -> Chọn DLQ nãy vừa tạo

![1786473010263](image/_index.vi/1786473010263.png)

Sau đó lấy ARN của main queue

![1786473020049](image/_index.vi/1786473020049.png)
