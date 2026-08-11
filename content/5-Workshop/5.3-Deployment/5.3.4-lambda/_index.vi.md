---
title: "Lambda — Ingestion"
date: 2026-08-11
weight: 4
chapter: false
pre: " <b> 5.3.4. </b> "
---
# Lambda

## Tổng quan Sơ đồ Kiến trúc

![1786475183451](image/_index.vi/1786475183451.png)

## Khởi tạo Lambda

![]()![1786473446421](image/_index.vi/1786473446421.png)

Runtime chọn Python 3.14 -> Create function

![]()![1786473453617](image/_index.vi/1786473453617.png)

Ấn vào Role name -> Search AWSLambdaBasicExecutionRole -> Add Permissions

![1786473467091](image/_index.vi/1786473467091.png)

![1786473483756](image/_index.vi/1786473483756.png)

Ấn Add trigger

![]()![1786473489619](image/_index.vi/1786473489619.png)

Chọn SQS -> Chọn SQS Queue nãy vừa tạo (Main Queue) ->
Create

Quay lại Bucket -> Ấn qua tab Properties

![1786473502978](image/_index.vi/1786473502978.png)

Tìm **Event notifications -> Create event notification
-> Đặt tên -> Prefix là folder uploads/ -> All object create events**

![1786473553617](image/_index.vi/1786473553617.png)

**Mục Desination chọn -> SQS Queue -> Enter SQS queue
ARN -> Nhập link ARN Queue vào -> Save changes**
