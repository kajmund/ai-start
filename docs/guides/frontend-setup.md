# Frontend setup

Vite + React SPA for browser UI. Skip this guide if the project is API-only.

Read [../../AGENTS.md](../../AGENTS.md) and [../AGENTS.md](../AGENTS.md) for layout rules.

## Init (from empty `frontend/`)

```bash
cd frontend
pnpm create vite . --template react-ts
pnpm install
pnpm add react-router-dom @supabase/supabase-js
pnpm add -D tailwindcss @tailwindcss/vite
pnpm dlx shadcn@latest init
```

Add when the project needs them:

```bash
# Chat streaming UI (RAG chat projects)
pnpm add ai @ai-sdk/react
```

Create the skeleton under `frontend/src/`:

```text
src/
├── lib/
│   ├── env.ts        # env validation
│   ├── http.ts       # fetch wrapper
│   ├── api.ts        # backend API client
│   └── supabase.ts   # browser auth client (if needed)
├── pages/
├── components/
├── App.tsx
└── main.tsx
```

## Run

```bash
cd frontend
pnpm install
pnpm dev
```

## Check

```bash
pnpm tsc --noEmit
pnpm lint
```

## Env

Copy `.env.example` to `.env`:

```text
VITE_API_BASE_URL=http://localhost:8000
VITE_SUPABASE_URL=https://your-project-ref.supabase.co
VITE_SUPABASE_ANON_KEY=your-anon-public-key
```

Only `VITE_`-prefixed vars are exposed to the browser. Never put secrets here.
