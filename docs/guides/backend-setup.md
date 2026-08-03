# Backend setup

Python + FastAPI backend for all AI logic, orchestration, and server-side data access. Read [../../AGENTS.md](../../AGENTS.md) and [../AGENTS.md](../AGENTS.md) for layout rules.

## Init (from empty `backend/`)

Core dependencies:

```bash
cd backend
uv sync
uv add fastapi uvicorn pydantic pydantic-settings httpx structlog openai
uv add --dev pytest ruff
```

Add when your project needs them:

```bash
# Supabase auth + DB
uv add supabase sqlalchemy alembic "psycopg[binary]"

# Typed LLM agents
uv add pydantic-ai

# Semantic search
uv add pgvector
```

Create the app skeleton under `backend/app/`:

```text
app/
├── main.py       # FastAPI entrypoint
├── config.py     # Pydantic settings
└── api/          # route handlers
```

## Database migrations

When using Postgres, Alembic owns schema changes.

Initialize once from `backend/`:

```bash
uv run alembic init alembic
```

Configure `alembic/env.py` to import SQLAlchemy metadata from `app.database.models` and read `DATABASE_URL` from `app.config.settings`. Use the **direct** Supabase connection URL.

Create a migration after model changes:

```bash
uv run alembic revision --autogenerate -m "add tables"
```

Review every generated migration. Add explicit ops for features autogenerate misses:

- `create extension if not exists vector`
- `vector(1536)` columns
- generated `tsvector` columns
- HNSW and GIN indexes
- RLS enablement and policies

Apply:

```bash
uv run alembic upgrade head
```

## Run

```bash
cd backend
uv sync
uv run alembic upgrade head   # if using Postgres
uv run uvicorn app.main:app --reload
```

## Imports (`from app...`)

`backend/app` is installed as an editable package by `uv sync`. The `[build-system]` section in `pyproject.toml` enables this.

```bash
cd backend
uv run uvicorn app.main:app --reload
```

Direct execution also works:

```bash
uv run python app/main.py
```

## Jupyter

```bash
cd backend
uv run python -m ipykernel install --user --name ai-start-backend --display-name "AI Start Backend"
```

```python
from app.config import settings
```

## Scripts

Batch and one-off jobs live in `backend/scripts/` — not in `app/`:

```bash
uv run python scripts/ingest_corpus.py
```

Scripts may import from `app.*`; `app/` must not import from `scripts/`.

## Sample data

Optional SEC EDGAR downloader for RAG experiments: `data/examples/sec-edgar/download.py`

```bash
uv run data/examples/sec-edgar/download.py
```

Project-specific ingest scripts belong in `backend/scripts/`.
