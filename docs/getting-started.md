# Getting started

This guide walks you from a fresh template clone to a running local dev environment.

## 1. Create your repo

**GitHub:** click **Use this template** and create a new repository.

**Local clone:**

```bash
git clone https://github.com/your-org/your-project.git
cd your-project
```

Rename references to match your project if you want (project name in docs, Jupyter kernel name, etc.). The template uses generic placeholders — nothing hard-codes a specific product name in code yet.

## 2. Define the product

Open [project-brief.md](project-brief.md) and fill in every section:

- Who are the users?
- What problem does the chatbot solve?
- What documents go in the corpus?
- What does "trust" mean for your domain?
- What is explicitly out of scope?

This brief is the source of truth for product decisions. Point AI coding agents at it when building features.

## 3. Read the architecture

Skim [architecture.md](architecture.md) before writing code. It describes:

- The chat path (browser → FastAPI → Supabase → OpenAI)
- The ingestion path (corpus → chunk → embed → store)
- Hybrid retrieval with RRF fusion
- Grounding and citation policy
- The recommended module layout for backend and frontend

You do not need to implement everything at once. Follow the implementation sequence at the bottom of the architecture doc.

## 4. Set up Supabase

Follow [guides/supabase-setup.md](guides/supabase-setup.md):

1. Create a Supabase project.
2. Collect Project URL, anon key, service role key, and direct database URL.
3. Configure email auth.

Do not create app tables manually in the dashboard — Alembic migrations own the schema.

## 5. Scaffold the backend

From `backend/`:

```bash
uv sync
uv add fastapi uvicorn pydantic pydantic-settings httpx structlog openai supabase pydantic-ai sqlalchemy alembic "psycopg[binary]" pgvector
uv add --dev pytest ruff
```

Copy `.env.example` to `.env` and fill in your Supabase and OpenAI credentials.

Initialize Alembic, add SQLAlchemy models, and run migrations. See [guides/backend-setup.md](guides/backend-setup.md) for details.

Start the API:

```bash
uv run uvicorn app.main:app --reload
```

## 6. Scaffold the frontend

From `frontend/`:

```bash
pnpm create vite . --template react-ts
pnpm install
pnpm add react-router-dom @supabase/supabase-js
pnpm add -D tailwindcss @tailwindcss/vite
pnpm dlx shadcn@latest init
```

Copy `.env.example` to `.env` and fill in `VITE_API_BASE_URL`, `VITE_SUPABASE_URL`, and `VITE_SUPABASE_ANON_KEY`.

Start the dev server:

```bash
pnpm dev
```

See [guides/frontend-setup.md](guides/frontend-setup.md) for the full init commands and checks.

## 7. Prepare your corpus

Put source documents in `data/` for local development:

```text
data/
├── README.md
├── corpus/           # your documents (gitignored if large)
└── examples/         # optional reference downloaders
```

If you need sample data quickly, the template includes an optional SEC EDGAR downloader in `data/examples/sec-edgar/`. Edit the params at the top of the script, then run:

```bash
uv run data/examples/sec-edgar/download.py
```

For your own sources, write a similar script or drop files directly into `data/corpus/`.

## 8. Build in order

Follow the implementation sequence in [architecture.md](architecture.md):

1. Auth (Supabase in frontend, JWT verification in backend)
2. Chat streaming endpoint (stub first, then real LLM)
3. Ingestion pipeline (parse → chunk → embed → store)
4. Hybrid retrieval
5. Grounded answer generation with citations
6. Citation UI in the frontend

Commit early, keep the fast test suite green, and expand scope only when each layer works.

## 9. Deploy

Deploy two Railway services (frontend static build + backend Uvicorn) and point env vars at your hosted Supabase project. Details depend on your hosting choices — the architecture doc describes the target shape.
