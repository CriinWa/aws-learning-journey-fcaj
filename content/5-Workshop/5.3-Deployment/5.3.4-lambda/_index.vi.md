---
title: "Lambda — Ingestion"
date: 2026-08-11
weight: 4
chapter: false
pre: " <b> 5.3.4. </b> "
---
# Triển khai Lambda

## Tổng quan Sơ đồ Kiến trúc

![1786475183451](image/_index.vi/1786475183451.png)

## Các bước tạo Lambda

![]()![1786473446421](image/_index.vi/1786473446421.png)
**Bước 1**
- Đặt tên cho Lambda function, chọn runtime là Python 3.14 rồi nhấn **Create function** để tạo.

![]()![1786473453617](image/_index.vi/1786473453617.png)

**Bước 2**
- Sau khi tạo xong function, hãy nhấn vào phần **Role name**, tìm kiếm **AWSLambdaBasicExecutionRole** và chọn **Add Permissions** để gán quyền cho function.

![1786473467091](image/_index.vi/1786473467091.png)

![1786473483756](image/_index.vi/1786473483756.png)

**Bước 3**
- Tiếp theo, nhấn **Add trigger** để thêm trigger cho Lambda.

![]()![1786473489619](image/_index.vi/1786473489619.png)

**Bước 4**
- Chọn **SQS**, sau đó chọn queue SQS vừa tạo (Main Queue), rồi nhấn **Create** để hoàn tất.


**Bước 5**
- Quay lại bucket S3, chuyển sang tab **Properties**.

![1786473502978](image/_index.vi/1786473502978.png)

- Tìm tới **Event notifications**, bấm **Create event notification**. Đặt tên thông báo, chọn **Prefix** là `uploads/` và chọn loại sự kiện là **All object create events**.

![1786473553617](image/_index.vi/1786473553617.png)

- Ở mục **Destination**, chọn **SQS Queue**, sau đó nhập **ARN** của queue SQS mà bạn muốn nhận sự kiện, rồi lưu thay đổi bằng cách nhấn **Save changes**.
