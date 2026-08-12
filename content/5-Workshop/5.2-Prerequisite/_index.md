---
title: "Preparation Steps"
date: 2026-08-11
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

# Prerequisites — Preparation Steps

## 5.2.1. Prepare source code

- Clone the source code to your local machine
- Check the main folders: `src/`, `views/`, `scripts/`, `deploy/`
- Create a virtual environment
- Install dependencies from `requirements.txt`
- Create `.env` from `.env.sample`

**Quick setup commands:**

Use the commands below to clone the source code and prepare the environment:

```bash
git clone https://github.com/KhanhKoy/vietnamese-legal-llmops
cd vietnamese-legal-llmops
cp .env.sample .env
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

Make sure you fill in all variables in `.env` before running the main modules.

```bash
git clone https://github.com/KhanhKoy/vietnamese-legal-llmops
cd vietnamese-legal-llmops
cp .env.sample .env
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

**Files to read before running the repository:**

| File | Role |
| --- | --- |
| `README.md` | Project overview and run instructions |
| `.env.sample` | Environment configuration template |
| `streamlit_app.py` | Main Streamlit entry point |
| `app.py` | Chainlit entry point |
| `src/api/main.py` | Thin API for `POST /ask` |
| `scripts/build_index.py` | Vector data build script |
| `deploy/docker-compose.yml` | Docker Compose run setup |

**Important environment variable groups in `.env.sample`:**

| Group | Main variables |
| --- | --- |
| Dataset | `HF_DATASET_NAME`, `LOCAL_DEMO_PATH` |
| Chunking | `CHUNK_SIZE_CHARS`, `CHUNK_OVERLAP_CHARS` |
| Embedding | `EMBEDDING_MODEL_NAME`, `EMBEDDING_BATCH_SIZE`, `USE_BEDROCK_EMBEDDING` |
| LLM | `LLM_PROVIDER`, `GEMINI_API_KEY`, `BEDROCK_LLM_MODEL_ID` |
| Database | `USE_PGVECTOR`, `PGHOST`, `PGPORT`, `PGDATABASE`, `PGUSER`, `PGPASSWORD` |
| API | `AUTH_DISABLED`, `ENABLE_API_DOCS`, `CORS_ALLOWED_ORIGINS` |

## 5.2.2. Prepare data

- The repository currently includes one demo file in `data_demo/`: `44_VBHN-VPQH_699655.pdf`
- Besides local data, `src/rag_core/dataset_reader.py` also supports reading from a Hugging Face dataset via `HF_DATASET_NAME`
- Before building the index, decide clearly whether you are using local data or Hugging Face data

**Data sources based on the codebase:**

| Source | Evidence in repository | Notes |
| --- | --- | --- |
| Hugging Face dataset | `HF_DATASET_NAME` in `.env.sample`, `dataset_reader.py` | Default data loading path |
| Local demo file | `data_demo/44_VBHN-VPQH_699655.pdf` | Used for quick checks or demo |

`dataset_reader.py` currently reads fields such as:

- `id`
- `title`
- `so_ky_hieu`
- `loai_van_ban`
- `co_quan_ban_hanh`
- `linh_vuc`
- `tinh_trang_hieu_luc`
- `content_markdown`

After data is loaded, the pipeline will:

1. Normalize metadata
2. Split text into chunks
3. Generate embeddings in batches
4. Write into the vector store for query serving

![1786495670556](image/_index.vi/1786495670556.png)
