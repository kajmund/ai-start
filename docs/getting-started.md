# Getting started

From template clone to running local dev environment.

## 1. Create your repo

**GitHub:** click **Use this template** → create a new repository.

**Local clone:**

```bash
git clone https://github.com/your-org/your-project.git
cd your-project
```

## 2. Define the product

Open [project-brief.md](project-brief.md) and fill in every section:

- Pick a **project type** (RAG chat, agent, extraction API, etc.)
- Define users, problem, and acceptance criteria
- Check which technical pieces you need (frontend, auth, pgvector, etc.)

This brief is the source of truth. Point AI coding agents at it before asking them to build features.

## 3. Read the rules

Before writing code, agents (and humans) should read:

1. [AGENTS.md](../AGENTS.md) — where code goes, must/must-not rules
2. [architecture.md](architecture.md) — system boundaries
3. [backend/AGENTS.md](../backend/AGENTS.md) or [frontend/AGENTS.md](../frontend/AGENTS.md) — depending on what you're building
4. Matching [pattern doc](patterns/) if your project type has one

## 4. Set up Supabase

Follow [guides/supabase-setup.md](guides/supabase-setup.md) if the project needs auth or Postgres:

1. Create a Supabase project.
2. Collect URL, anon key, service role key, and direct database URL.
3. Configure email auth.

Skip Supabase entirely for a stateless API-only prototype (use env vars only).

## 5. Scaffold the backend

From `backend/`:

```bash
uv sync
uv add fastapi uvicorn pydantic pydantic-settings httpx structlog openai
uv add --dev pytest ruff
```

Add more deps as needed:

```bash
# Auth + database
uv add supabase sqlalchemy alembic "psycopg[binary]"

# Agents
uv add pydantic-ai

# Semantic search
uv add pgvector
```

Copy `.env.example` to `.env` and fill in credentials.

Create the app skeleton:

```text
backend/app/
├── main.py       # FastAPI app + health route
├── config.py     # settings
└── api/          # route handlers
```

Start the API:

```bash
uv run uvicorn app.main:app --reload
```

See [guides/backend-setup.md](guides/backend-setup.md) for migrations, imports, and Jupyter setup.

## 6. Scaffold the frontend (if needed)

Skip this step for API-only MVPs.

From `frontend/`:

```bash
pnpm create vite . --template react-ts
pnpm install
pnpm add react-router-dom @supabase/supabase-js
pnpm add -D tailwindcss @tailwindcss/vite
pnpm dlx shadcn@latest init
```

Copy `.env.example` to `.env` and fill in `VITE_API_BASE_URL`, `VITE_SUPABASE_URL`, and `VITE_SUPABASE_ANON_KEY`.

```bash
pnpm dev
```

See [guides/frontend-setup.md](guides/frontend-setup.md) for checks.

## 7. Prepare local data (if needed)

```text
data/
├── corpus/       # your source files
└── examples/     # optional sample downloaders
```

Drop files into `data/corpus/` or use the optional SEC EDGAR example:

```bash
uv run data/examples/sec-edgar/download.py
```

Write project-specific download/seed scripts in `data/examples/` or `backend/scripts/`.

## 8. Build the core feature

Order depends on project type. General sequence:

1. Config + health check
2. Auth (if user-facing)
3. Core domain logic in `backend/app/<domain>/`
4. API routes in `backend/app/api/`
5. DB models + migrations (if persisting)
6. Frontend pages (if UI)
7. Batch scripts (if offline processing)

For RAG chat specifically, follow [patterns/grounded-rag-chat.md](patterns/grounded-rag-chat.md).

## 9. Deploy

Default: two Railway services (frontend static build + backend Uvicorn) + hosted Supabase. API-only projects deploy backend only.

Point production env vars at hosted Supabase and OpenAI. See [architecture.md](architecture.md#deployment-railway).
