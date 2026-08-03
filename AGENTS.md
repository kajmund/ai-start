# Agent Instructions

This file is the source of truth for any coding agent (Claude Code, Cursor, Codex, etc.) working in this repo. **Read it before touching code.**

## Before you write code

1. Read [docs/project-brief.md](docs/project-brief.md) — if it is still a template, ask the user to define the product or infer from their request.
2. Read this file.
3. Read [backend/AGENTS.md](backend/AGENTS.md) when editing `backend/`.
4. Read [frontend/AGENTS.md](frontend/AGENTS.md) when editing `frontend/`.
5. Read [docs/architecture.md](docs/architecture.md) for system boundaries and deployment shape.
6. If the project is a grounded document chatbot, also read [docs/patterns/grounded-rag-chat.md](docs/patterns/grounded-rag-chat.md).

Do not assume the project is a chatbot. The brief decides the product shape.

## What this template is

A **full-stack starter for AI-powered web applications** — not a finished product and not tied to one use case.

It gives you:

- A Python + FastAPI backend for AI logic, orchestration, and data access
- A Vite + React SPA for user-facing UI (when you need one)
- Supabase for auth and Postgres persistence
- OpenAI for generation and embeddings
- Conventions for where code lives, how config works, and what agents must not do

Projects built from this template include chatbots, agent tools, classification APIs, extraction pipelines, internal dashboards, and batch processors with a UI. Pick the pieces your brief requires.

## Stack

| Layer | Choice | Required? |
| ----- | ------ | --------- |
| Backend | Python + FastAPI | Yes — all AI logic lives here |
| Frontend | Vite + React SPA + TypeScript | Only if the product has a browser UI |
| Database | Supabase Postgres | When you need durable state |
| Migrations | SQLAlchemy + Alembic | When you use Postgres tables |
| Vector search | Supabase `pgvector` | Only for semantic retrieval |
| Auth | Supabase Auth (email) | When users sign in |
| Hosting | Railway (backend + frontend) | Default deployment target |
| LLM + embeddings | OpenAI | Default model provider |

Stack is locked unless the project brief or user explicitly changes it. Do not propose alternatives without a stated reason.

## Repo layout — where everything goes

```text
your-project/
├── AGENTS.md                 # this file — read first
├── README.md                 # human-facing overview
│
├── backend/                  # ALL server-side code (see backend/AGENTS.md)
│   ├── app/                  # FastAPI application — request-path code only
│   │   ├── main.py           # app entrypoint, router registration
│   │   ├── config.py         # env settings — single source of truth
│   │   ├── api/              # HTTP route handlers (thin — delegate to domain)
│   │   ├── auth/             # JWT verification (if user-facing)
│   │   ├── database/         # SQLAlchemy models, Supabase client, queries
│   │   ├── llm/              # OpenAI client, shared prompt helpers
│   │   └── <domain>/         # project-specific modules (see below)
│   ├── scripts/              # CLI tools: ingest, evaluate, seed — NOT imported by app/
│   ├── alembic/              # DB migrations (when using Postgres)
│   ├── tests/                # pytest tests
│   └── pyproject.toml
│
├── frontend/                 # ALL browser code (see frontend/AGENTS.md)
│   └── src/
│       ├── components/       # reusable UI (shadcn/ui under components/ui/)
│       ├── lib/              # env, http, api client, supabase — no UI
│       ├── pages/            # route-level screens
│       ├── App.tsx           # router
│       └── main.tsx
│
├── data/                     # local dev inputs — payloads gitignored
│   ├── corpus/               # source files for ingestion / eval
│   └── examples/             # optional sample download scripts
│
└── docs/                     # specs and guides — never generated code
    ├── project-brief.md      # product definition — source of truth for features
    ├── architecture.md       # system boundaries and deployment
    ├── getting-started.md    # setup walkthrough
    ├── patterns/             # optional reference architectures per product type
    └── guides/               # Supabase, backend, frontend setup
```

### Where to put new code

| Kind of code | Location | Example |
| ------------ | -------- | ------- |
| HTTP endpoint | `backend/app/api/<domain>.py` | `POST /classify` |
| Business / AI logic | `backend/app/<domain>/` | `extraction/parser.py` |
| LLM call + prompt | `backend/app/llm/` or `backend/app/<domain>/` | `llm/client.py`, `agents/instructions.md` |
| DB model / query | `backend/app/database/` | `models.py`, `jobs.py` |
| One-off script | `backend/scripts/` | `scripts/ingest_corpus.py` |
| React page | `frontend/src/pages/` | `pages/Dashboard.tsx` |
| Shared UI component | `frontend/src/components/` | `components/ResultCard.tsx` |
| API client call | `frontend/src/lib/api.ts` | `api.post('/classify', body)` |
| Local test data | `data/corpus/` or `data/examples/` | sample PDFs, CSV fixtures |
| Product spec | `docs/` | update `project-brief.md` |

**Never** put backend logic in `frontend/`. **Never** put React components in `backend/`. **Never** put secrets in `data/` or `docs/`.

### Choosing backend domain modules

Create modules under `backend/app/` based on the project brief — not every project needs all of these:

| Project type | Typical `backend/app/` modules | Frontend needed? |
| ------------ | ------------------------------ | ---------------- |
| Grounded RAG chat | `chat/`, `assistant/`, `retrieval/`, `grounding/` | Yes |
| AI agent with tools | `agents/`, `tools/` | Yes or API-only |
| Classification / extraction API | `extraction/` or `classification/` | Optional admin UI |
| Batch document processing | `jobs/`, `processing/` + `scripts/` | Optional status UI |
| Internal AI dashboard | `analytics/`, `llm/` | Yes |
| API-only MVP | `api/`, `llm/`, `database/` | No — skip `frontend/` |

See [docs/patterns/](docs/patterns/) for detailed reference architectures.

## System boundaries

These rules apply to **every** project type:

**Frontend (`frontend/src/`)**

- Renders UI, manages local state, holds the Supabase browser session
- Calls the backend over JSON with the user's bearer token
- Must NOT: hold OpenAI keys, run LLM calls, run retrieval, use the Supabase service-role key, or write privileged DB records

**Backend (`backend/app/`)**

- Owns all AI calls, business rules, auth verification, and privileged DB writes
- Must NOT: serve static frontend assets in production (Railway runs them separately), or expose service-role keys to clients

**Scripts (`backend/scripts/`)**

- One-off or batch jobs: ingest, evaluate, seed, migrate data
- Run via `uv run python scripts/<name>.py` — not mounted as FastAPI routes unless the product explicitly needs a trigger endpoint

**Data (`data/`)**

- Local inputs and fixtures for development
- Large payloads gitignored — never commit production data or secrets

**Docs (`docs/`)**

- Specs, briefs, architecture notes, setup guides
- Update `project-brief.md` when product scope changes

## What you MUST do

- Read `docs/project-brief.md` before implementing features — it overrides assumptions in this file.
- Put all Python application code under `backend/app/`.
- Put all React code under `frontend/src/`.
- Use `backend/app/config.py` and `frontend/src/lib/env.ts` as the only env entry points.
- Fail fast when required config is missing — no silent fallbacks.
- Keep route handlers thin; put logic in domain modules.
- Validate at boundaries: HTTP input, external API responses, DB writes, untrusted file parsing.
- Add backend tests for non-trivial logic (`backend/tests/`).
- Match existing naming, import style, and file layout when editing existing code.
- Flag new runtime dependencies in the commit message (see dependency policy).

## What you MUST NOT do

- **Do not assume a chatbot** unless the brief or user request says so.
- **Do not call OpenAI from the frontend** — all LLM calls go through the backend.
- **Do not expose Supabase `service_role` key** to the browser or commit it.
- **Do not use Next.js, SSR, or server components** — this is a plain Vite SPA.
- **Do not read `os.getenv` / `process.env` outside the settings modules.**
- **Do not call `load_dotenv` anywhere.**
- **Do not create tables manually in Supabase dashboard** — use Alembic migrations.
- **Do not add npm/yarn lockfiles** — this project uses pnpm only.
- **Do not write frontend tests** — verify with `tsc`, `lint`, and manual browser checks.
- **Do not add feature flags, backwards-compat shims, or premature abstractions** unless explicitly asked.
- **Do not put one-off scripts inside `backend/app/`** — use `backend/scripts/`.
- **Do not add helper libraries** that wrap a few lines of stdlib (see dependency policy).
- **Do not edit unrelated files** when making a focused change.

## Dependency policy

**Default: write it yourself.** Reach for a library only when the alternative would be non-trivial, error-prone, or reinvention of a standard. Every dependency is a liability.

OK to depend on:

- Things genuinely hard to get right: HTTP clients, ASGI servers, SQL drivers, parsers, LLM SDKs, ORM, migrations, auth SDKs.
- The declared stack: FastAPI, React, Vite, Supabase clients, OpenAI SDK, PydanticAI, etc.

Not OK:

- Helper libraries wrapping 5–20 lines of stdlib or platform APIs.
- Frameworks where a function would do.
- "Nicer API" layers on top of an already-present dependency.

Before adding a runtime dep, answer in the commit message:

1. What exactly does it do that we can't write in <30 lines of clear code?
2. How often does it get used?
3. What's its maintenance / transitive-dep footprint?

Per-stack specifics: [backend/AGENTS.md](backend/AGENTS.md), [frontend/AGENTS.md](frontend/AGENTS.md).

## Configuration

A single settings module is the source of truth per service:

- Backend: `backend/app/config.py`
- Frontend: `frontend/src/lib/env.ts`

Do not call `os.getenv` / read `process.env` directly in app code. If a third-party SDK reads env vars directly, mirror them in the settings module.

Fail fast on startup if required config is missing.

## Code style

- **Small, obvious functions.** A 15-line function with clear names beats a three-class abstraction.
- **No premature abstraction.** Three similar lines is better than a badly-named base class. Extract when there's a third caller.
- **No error handling for impossible cases.** Trust internal callers and framework guarantees.
- **Comments:** explain *why* when non-obvious, never *what*. Remove stale TODOs.
- **Keep files focused.** Prefer small modules.
