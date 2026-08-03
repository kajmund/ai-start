# AI Start

A starter template for building grounded AI chat apps: React frontend, FastAPI backend, Supabase Postgres with hybrid retrieval (`pgvector` + full-text search), and OpenAI for generation and embeddings.

Use this repo as a scaffold — not a finished product. `backend/` and `frontend/` are empty shells with conventions and setup guides. Fill in your product brief, add your corpus, then build.

## What you get

| Layer | Choice |
| ----- | ------ |
| Backend | Python + FastAPI |
| Frontend | Vite + React SPA + TypeScript |
| Database | Supabase Postgres (users, chats, documents, chunks) |
| Migrations | SQLAlchemy models + Alembic |
| Retrieval | Supabase `pgvector` + Postgres full-text search |
| Auth | Supabase Auth (email) |
| Hosting | Railway (backend + frontend services) |
| LLM + embeddings | OpenAI |

## Quick start

1. **Use this template** — click "Use this template" on GitHub, or clone and push to your own repo.
2. **Define your product** — copy and fill in [docs/project-brief.md](docs/project-brief.md).
3. **Read the architecture** — [docs/architecture.md](docs/architecture.md) describes the target RAG chat design.
4. **Set up services** — follow the guides in order:
   - [Supabase](docs/guides/supabase-setup.md)
   - [Backend](docs/guides/backend-setup.md)
   - [Frontend](docs/guides/frontend-setup.md)
5. **Add your corpus** — put source files in `data/` (see [data/README.md](data/README.md)). An optional SEC EDGAR example lives in [data/examples/sec-edgar/](data/examples/sec-edgar/).

Full walkthrough: [docs/getting-started.md](docs/getting-started.md)

## Repo layout

```text
your-project/
├── AGENTS.md              # agent instructions (read first)
├── README.md              # this file
├── data/                  # local corpus + optional download examples
├── docs/
│   ├── architecture.md    # target system design
│   ├── project-brief.md   # fill-in product brief template
│   └── guides/            # setup instructions
├── backend/               # FastAPI service (scaffold only)
└── frontend/              # React SPA (scaffold only)
```

## Prerequisites

| Tool | Version | Used for | Install |
| ---- | ------- | -------- | ------- |
| [Python](https://www.python.org/downloads/) | 3.12+ | Backend runtime | OS package manager or python.org |
| [uv](https://docs.astral.sh/uv/getting-started/installation/) | latest | Backend deps + data scripts | `curl -LsSf https://astral.sh/uv/install.sh \| sh` |
| [Node.js](https://nodejs.org/) | 20+ (LTS) | Frontend toolchain | nodejs.org or `nvm install --lts` |
| [pnpm](https://pnpm.io/installation) | latest | Frontend package manager | `corepack enable && corepack prepare pnpm@latest --activate` |

You also need accounts/keys for external services once the app is wired up. Start with [docs/guides/supabase-setup.md](docs/guides/supabase-setup.md), then create an [OpenAI API key](https://platform.openai.com/api-keys) when the LLM layer is wired up.

## For AI coding agents

Read [AGENTS.md](AGENTS.md) before touching code. Stack, dependency policy, and code style are locked there. Service-specific notes live in [backend/AGENTS.md](backend/AGENTS.md) and [frontend/AGENTS.md](frontend/AGENTS.md).
