# AI Start

A full-stack starter template for AI-powered web applications — chatbots, agents, classification APIs, extraction pipelines, dashboards, and more.

Use this repo as a scaffold. `backend/` and `frontend/` are empty shells with conventions, agent instructions, and setup guides. Define your product in the brief, then build only what you need.

## What you get

| Layer | Choice |
| ----- | ------ |
| Backend | Python + FastAPI — all AI logic lives here |
| Frontend | Vite + React SPA + TypeScript — when you need a UI |
| Database | Supabase Postgres — when you need durable state |
| Migrations | SQLAlchemy + Alembic |
| Vector search | Supabase `pgvector` — when you need semantic retrieval |
| Auth | Supabase Auth (email) |
| Hosting | Railway (backend + frontend services) |
| LLM + embeddings | OpenAI |

## Supported project types

| Type | Backend modules | Frontend |
| ---- | --------------- | -------- |
| Grounded RAG chat | `chat/`, `retrieval/`, `assistant/` | Chat UI |
| AI agent with tools | `agents/`, `tools/` | Dashboard or API-only |
| Classification / extraction | `extraction/`, `classification/` | Upload + results |
| Batch processing | `processing/`, `scripts/` | Optional job status UI |
| API-only MVP | `api/`, `llm/` | Skip frontend |

See [docs/patterns/](docs/patterns/) for detailed reference architectures.

## Quick start

1. **Use this template** — "Use this template" on GitHub, or clone to your own repo.
2. **Define your product** — fill in [docs/project-brief.md](docs/project-brief.md).
3. **Read agent instructions** — [AGENTS.md](AGENTS.md) tells coding agents exactly what to do and where code goes.
4. **Set up services** — follow the guides:
   - [Supabase](docs/guides/supabase-setup.md)
   - [Backend](docs/guides/backend-setup.md)
   - [Frontend](docs/guides/frontend-setup.md) (skip if API-only)
5. **Add local data** — [data/](data/README.md) for corpus, fixtures, or sample download scripts.

Full walkthrough: [docs/getting-started.md](docs/getting-started.md)

## Repo layout

```text
your-project/
├── AGENTS.md              # agent playbook — read first
├── README.md
├── backend/               # FastAPI — ALL server + AI code
│   ├── app/               # request-path application code
│   └── scripts/           # CLI / batch jobs
├── frontend/              # React SPA — UI only (optional)
│   └── src/
├── data/                  # local dev inputs (payloads gitignored)
└── docs/
    ├── project-brief.md   # your product definition
    ├── architecture.md    # system boundaries
    ├── patterns/          # optional reference designs
    └── guides/            # setup instructions
```

## Prerequisites

| Tool | Version | Used for |
| ---- | ------- | -------- |
| [Python](https://www.python.org/downloads/) | 3.12+ | Backend |
| [uv](https://docs.astral.sh/uv/getting-started/installation/) | latest | Backend deps + scripts |
| [Node.js](https://nodejs.org/) | 20+ | Frontend (if needed) |
| [pnpm](https://pnpm.io/installation) | latest | Frontend packages |

External services: [Supabase](docs/guides/supabase-setup.md) account, [OpenAI API key](https://platform.openai.com/api-keys) when wiring up the LLM layer.

## For AI coding agents

[AGENTS.md](AGENTS.md) is the source of truth:

- Where backend and frontend code must go
- What agents must and must not do
- How to pick modules for your project type
- Stack, dependency policy, and code style

Service-specific rules: [backend/AGENTS.md](backend/AGENTS.md), [frontend/AGENTS.md](frontend/AGENTS.md).
