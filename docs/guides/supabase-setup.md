# Supabase setup

Use Supabase when the project needs **Postgres** and/or **Auth**. Skip this guide for stateless API-only prototypes.

Typical uses: user sign-in, durable app state, pgvector semantic search.

## 1. Create an account

1. Go to [supabase.com](https://supabase.com) and sign up (GitHub or email).
2. Confirm your email if prompted.
3. The free tier is enough for local development.

## 2. Create a project

1. Open [New project](https://supabase.com/dashboard/new).
2. Pick your organization.
3. Set a **project name** (e.g. `My AI App`).
4. Choose a **database password** — save it; needed for direct DB access and Alembic.
5. Pick a **region** close to you.
6. Wait until status is healthy (~1–2 minutes).

## 3. Collect credentials

| Value | Where to find it | Used by |
| ----- | ---------------- | ------- |
| **Project URL** | Dashboard → **Project Settings** → **API** → Project URL | Frontend + backend |
| **anon (public) key** | Same page → `anon` `public` key | Frontend (browser-safe) |
| **service_role (secret) key** | Same page → `service_role` `secret` key | Backend only |
| **Project ref** | Dashboard URL or `supabase projects list` | CLI |
| **Direct database URL** | Dashboard → **Database** → Connection string (direct) | Alembic + backend DB |
| **Database password** | Set at project creation | Direct Postgres connection |

```bash
supabase projects api-keys --project-ref <your-project-ref>
```

Keep `service_role` out of git, client bundles, and frontend env files.

## 4. Auth settings (when using auth)

Default: email auth only.

1. Dashboard → **Authentication** → **Providers** → leave **Email** enabled.
2. For local dev, consider disabling "Confirm email" under **Authentication** → **Email** (re-enable for production).

## 5. Database schema

Schema is managed by **Alembic from the backend** — do not create production tables manually in the dashboard.

What migrations may create (depending on project type):

- App tables (users, jobs, chat threads, etc.)
- `vector` extension + embedding columns (RAG projects)
- Full-text search columns and indexes
- Row-level security policies

Use the **direct/session** connection URL for Alembic — not the transaction pooler.

From `backend/`:

```bash
uv run alembic upgrade head
```

See [Backend setup](backend-setup.md).

## Next steps

- [Backend setup](backend-setup.md)
- [Frontend setup](frontend-setup.md) — if the project has a UI
