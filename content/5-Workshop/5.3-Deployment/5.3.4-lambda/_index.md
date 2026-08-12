---
title: "Lambda — Ingestion"
date: 2026-08-11
weight: 4
chapter: false
pre: " <b> 5.3.4. </b> "
---
# Deploy Lambda

## Architecture overview

![1786475183451](image/_index.vi/1786475183451.png)

## Create the Lambda function

![]()![1786473446421](image/_index.vi/1786473446421.png)
**Step 1**
- Name the Lambda function, choose runtime Python 3.14, then choose **Create function**.

![]()![1786473453617](image/_index.vi/1786473453617.png)

**Step 2**
- After the function is created, open **Role name**, find **AWSLambdaBasicExecutionRole**, and choose **Add Permissions**.

![1786473467091](image/_index.vi/1786473467091.png)

![1786473483756](image/_index.vi/1786473483756.png)

**Step 3**
- Choose **Add trigger** to attach a trigger to Lambda.

![]()![1786473489619](image/_index.vi/1786473489619.png)

**Step 4**
- Choose **SQS**, select the Main Queue created earlier, then choose **Create**.


**Step 5**
- Go back to the S3 bucket and open the **Properties** tab.

![1786473502978](image/_index.vi/1786473502978.png)

- Find **Event notifications**, choose **Create event notification**. Name the notification, set **Prefix** to `uploads/`, and choose **All object create events**.

![1786473553617](image/_index.vi/1786473553617.png)

- Under **Destination**, choose **SQS Queue**, enter the **ARN** of the queue that should receive events, then choose **Save changes**.
