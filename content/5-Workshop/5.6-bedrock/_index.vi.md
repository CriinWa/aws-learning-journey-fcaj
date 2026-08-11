---
title: "Bedrock — Embedding và LLM"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

# Bedrock — Embedding và LLM

Amazon Bedrock cung cấp embedding và LLM managed cho pipeline RAG trên AWS — thay thế model local hoặc Gemini khi deploy cloud.

## Hai vai trò của Bedrock

{{< mermaid >}}
graph LR;
    T["Van ban / cau hoi"] --> E["Bedrock Embedding"]
    E --> V["Vector"]
    V --> R["pgvector search"]
    R --> C["Retrieved chunks"]
    C --> L["Bedrock LLM"]
    L --> A["Cau tra loi"]
{{< /mermaid >}}

| Vai trò | Khi dùng | Model ví dụ |
| --- | --- | --- |
| **Embedding** | Build index, embed câu hỏi | `amazon.titan-embed-text-v2:0` |
| **LLM** | Sinh câu trả lời từ prompt | Claude / Titan qua Converse API |

## Bước 1 — Bật model access

1. Mở **Amazon Bedrock** → **Model access**
2. Request access cho embedding và chat model ở **ap-southeast-1**
3. Chờ phê duyệt (thường vài phút)

## Bước 2 — Cấu hình `.env`

```
LLM_PROVIDER=bedrock
BEDROCK_LLM_MODEL_ID=anthropic.claude-3-haiku-20240307-v1:0
USE_BEDROCK_EMBEDDING=true
BEDROCK_EMBEDDING_MODEL=amazon.titan-embed-text-v2:0
AWS_DEFAULT_REGION=ap-southeast-1
```

## Bước 3 — Quyền IAM

EC2 instance role hoặc Lambda role cần:

```json
{
  "Effect": "Allow",
  "Action": ["bedrock:InvokeModel", "bedrock:InvokeModelWithResponseStream"],
  "Resource": "*"
}
```

Production nên giới hạn `Resource` theo ARN model cụ thể.

## Tích hợp code

| Module | Dùng Bedrock |
| --- | --- |
| `src/rag_core/embeddings.py` | Titan embedding cho chunk và câu hỏi |
| `src/rag_core/generator.py` | Converse API cho câu trả lời pháp luật tiếng Việt |
| `src/rag_core/qa_service.py` | Điều phối embed → retrieve → generate |

{{% notice info %}}
Dev local có thể dùng `LLM_PROVIDER=gemini` và `GEMINI_API_KEY`. Bedrock là lựa chọn khuyến nghị trên EC2 với IAM role.
{{% /notice %}}

## Kiểm tra

- [ ] Model access đã bật trên console
- [ ] Gọi embedding test trả vector đúng dimension
- [ ] Gọi LLM test trả câu trả lời từ prompt mẫu
- [ ] `POST /ask` trên EC2 trả answer + sources

Tiếp theo: cấu hình **DynamoDB** cho lịch sử hội thoại.
