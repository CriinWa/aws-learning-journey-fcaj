---
title: "Frontend"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1.1 </b> "
---

# Frontend

Law-Chatbot has **two UIs**. On the EC2 demo, the primary UI is **Streamlit**. **Chainlit** is an alternate RAG chat UI that calls QAService in-process.

| UI | Role in this workshop | Backend connection |
| --- | --- | --- |
| **Streamlit** | Main product UI, EC2 demo on port **8501** | HTTP **POST /ask** to api.main |
| **Chainlit** | Compat / experimental RAG UI | In-process QAService |

## Streamlit — primary UI

### Entry and configuration

- Entry file: **streamlit_app.py**
- Page title: Vietnamese Legal Assistant; wide layout; CSS **assets/style.css**
- On boot: initialize_database() and session state
- Router: not logged in → register or login; logged in as admin → views.admin; otherwise → views.chatbot
- Default port **8501** (.streamlit/config.toml and Docker Compose)
- Local: streamlit run streamlit_app.py
- Docker: APP_MODE=streamlit in deploy/entrypoint.sh

### Views

| View | File | Responsibility |
| --- | --- | --- |
| Login | views/login.py | Login form → authenticate_user; block Inactive users; set role |
| Register | views/register.py | Validate username/email/password → create_user with role user |
| Chatbot | views/chatbot.py | Sessions, suggestions, API call, sources, feedback |
| Admin | views/admin.py | KPI dashboard, user management, CSV logs, top_k / temperature / model settings |

### How it calls the API

In **views/chatbot.py**:

- Env var **API_URL** — defaults toward **http://127.0.0.1:8000/ask**; appends **/ask** if missing
- On Compose: API_URL=http://api:8000/ask
- process_user_query sends a POST JSON body with question and top_k
- Expected JSON: answer, sources with title / snippet / score
- **Chat history does not go through the API** — it is stored via src/storage on SQLite or Postgres

### User journeys

**End user:** Register / Login → Chatbot → create or select a session → ask → answer + source expander → like/dislike → logout.

**Admin:** Login as admin → Admin Dashboard → KPIs, users, logs, settings → logout. Admins do **not** enter the default chatbot view.

## Chainlit — alternate UI

- Entry: **app.py**
- Run: chainlit run app.py or scripts/run_chainlit.py; Docker APP_MODE=chainlit
- Shares **QAService** with the RAG backend
- Can log history to DynamoDB when ENABLE_CHAT_HISTORY=true
- Local auth only when both CHAINLIT_DEV_USERNAME and CHAINLIT_DEV_PASSWORD are set; needs CHAINLIT_AUTH_SECRET
- In the current code, the UI **updates after ask completes** — it does not token-stream with stream_token yet

## Frontend-related environment variables

| Variable | Purpose |
| --- | --- |
| API_URL | **POST /ask** endpoint for Streamlit |
| APP_DB_BACKEND | postgres or sqlite for users/chats |
| USE_PGVECTOR | Influences app DB backend when APP_DB_BACKEND is unset |
| PGHOST, PGPORT, PGDATABASE, PGUSER, PGPASSWORD | Postgres app DB connection |
| CHAINLIT_DEV_USERNAME, CHAINLIT_DEV_PASSWORD, CHAINLIT_AUTH_SECRET | Local Chainlit auth |
| QA_TOP_K, ENABLE_CHAT_HISTORY, DYNAMODB_* | Chainlit / history settings |

API_URL may be missing from .env.sample but is **required** when Streamlit talks to a separate FastAPI process.
