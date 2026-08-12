---
title: "S3 — Create Bucket"
date: 2026-08-11
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---
# Deploy S3 Bucket

## Architecture overview

![1786475100342](image/_index.vi/1786475100342.png)

## Create the project buckets
**Step 1: Create the bucket**
- Sign in to the AWS Console in Region `ap-southeast-1`, search for **Amazon S3**, then choose **Buckets → Create bucket.**
- Set the bucket name

![1786472818649](image/_index.vi/1786472818649.png)

![1786472832913](image/_index.vi/1786472832913.png)

- **Keep** the remaining settings
- Choose **Create**

![1786472896121](image/_index.vi/1786472896121.png)

**Step 2**
- Open the bucket you just created
- Go to the **Properties** tab → find **Bucket Versioning → Edit → Enable → Save changes**

![1786472922395](image/_index.vi/1786472922395.png)

![1786472929548](image/_index.vi/1786472929548.png)

**Step 3**
- Next, create the DLQ (Dead Letter Queue)
- Open **AWS Console** → **SQS** → **Queues** → **Create queue**. Choose
Type: **Standard**

![1786472957161](image/_index.vi/1786472957161.png)
**Step 4**
- Then create the **Main Queue**. Under **Configuration**, set **Visibility Timeout: 60s**

![1786472998924](image/_index.vi/1786472998924.png)

- Find **Dead-letter queue → Enable → select the DLQ you just created**

![1786473010263](image/_index.vi/1786473010263.png)

- Copy the ARN of the **Main Queue**

![1786473020049](image/_index.vi/1786473020049.png)
