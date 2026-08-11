---
title: "S3 — Upload data"
date: 2026-08-11
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

# S3 — Create bucket and upload data

Amazon S3 stores legal documents and manifest files. When a new file is uploaded, S3 event triggers the downstream processing (SQS → Lambda → RDS).

{{< mermaid >}}
graph LR
    A["Admin"] -->|presigned URL| B["S3 bucket"]
    B --> C["incoming/files"]
    B --> D["incoming/manifests"]
    B -->|ObjectCreated| E["SQS"]
    E --> F["Lambda"]
    F --> G[("RDS pgvector")]
{{< /mermaid >}}

## Steps

1. Create S3 bucket in `ap-southeast-1`
2. Configure `.env` with bucket name
3. Upload via admin API or CLI
4. Configure S3 event notification → SQS

Refer to the Vietnamese version for full details.
