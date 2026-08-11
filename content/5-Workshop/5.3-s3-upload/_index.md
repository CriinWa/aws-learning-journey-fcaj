---
title: "S3 bucket and upload data"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

# S3 — Create bucket and upload data

Amazon S3 stores legal documents and ingestion manifests. When a new file arrives, S3 events trigger the downstream ingestion pipeline (SQS → Lambda → RDS).

![S3 overview](/images/5-Workshop/5.3-S3-vpc/overview.png)

## Role in the system

| Prefix | Purpose |
| --- | --- |
| `incoming/files/` | PDF/TXT uploaded by admin |
| `incoming/manifests/` | Metadata JSON for each document |

{{< mermaid >}}
graph LR;
    A["Admin"] -->|presigned URL| B["S3 bucket"]
    B --> C["incoming/files"]
    B --> D["incoming/manifests"]
    B -->|ObjectCreated| E["SQS"]
    E --> F["Lambda"]
    F --> G[("RDS pgvector")]
{{< /mermaid >}}

## Step 1 — Create bucket

1. Open **Amazon S3** → **Create bucket**
2. Name: e.g. `legal-documents-<account-id>-ap-southeast-1`
3. Region: **ap-southeast-1** (same as other resources)
4. Block public access: **enabled**
5. Enable versioning (optional, recommended for production)

![Create bucket](/images/5-Workshop/5.3-S3-vpc/create-button.png)

## Step 2 — Configure `.env`

```
USE_S3=true
LEGAL_DOCUMENTS_BUCKET=legal-documents-<account-id>-ap-southeast-1
AWS_DEFAULT_REGION=ap-southeast-1
```

## Step 3 — Upload via admin API or CLI

**Production path:** admin calls `POST /api/admin/documents/upload-url` → client PUTs the file to S3.

**Lab / sync path:** use `scripts/sync_to_s3.py` to upload demo corpus:

```bash
python scripts/sync_to_s3.py --bucket $LEGAL_DOCUMENTS_BUCKET
```

**Manual CLI example:**

```bash
aws s3 cp data_demo/sample.pdf s3://$LEGAL_DOCUMENTS_BUCKET/incoming/files/sample.pdf
aws s3 cp data_demo/sample.manifest.json s3://$LEGAL_DOCUMENTS_BUCKET/incoming/manifests/sample.manifest.json
```

## Step 4 — Wire S3 → SQS (preview)

Configure an S3 event notification on `incoming/files/` to send messages to the ingestion queue (detailed in [5.5 Lambda](5.5-lambda/)).

{{% notice info %}}
Each uploaded file should have a matching manifest under `incoming/manifests/` so Lambda knows document title, type, and metadata.
{{% /notice %}}

## Verification

- [ ] Bucket exists in the correct region
- [ ] Test object appears under `incoming/files/`
- [ ] IAM role / user can `s3:PutObject` and `s3:GetObject`
- [ ] `LEGAL_DOCUMENTS_BUCKET` is set in `.env`

Next: set up **RDS PostgreSQL + pgvector** for vector storage.
