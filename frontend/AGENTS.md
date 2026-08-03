# Frontend — agent notes

Vite + React SPA for browser UI. Read [../AGENTS.md](../AGENTS.md) first.

## Role

The frontend handles **user interaction only**:

- Render pages and components
- Manage local UI state
- Hold the Supabase browser session (when auth is needed)
- Call the backend API with the user's JWT

It does **not** run AI, touch OpenAI, perform retrieval, or use privileged credentials.

Skip building frontend pages entirely if the project is API-only (see `docs/project-brief.md`).

## Stack

- **Vite + React + TypeScript** (strict). **Not Next.js** — no SSR, server components, or file-based routing.
- **Tailwind CSS** for styling — classes inline, no CSS modules or styled-components.
- **shadcn/ui** for UI primitives — add via `pnpm dlx shadcn@latest add <name>`.
- **React Router** for client-side routing.
- **`@supabase/supabase-js`** for browser auth (email only unless brief says otherwise).

Add when the project needs them:

- **Vercel AI SDK** — chat streaming UI (RAG chat projects)
- Other UI libs only with justification in the commit message

## Layout

```text
frontend/
├── src/
│   ├── components/           # reusable UI
│   │   └── ui/               # shadcn primitives (generated)
│   ├── lib/                  # no UI — framework-agnostic helpers
│   │   ├── env.ts            # ONLY place that reads import.meta.env
│   │   ├── supabase.ts       # browser Supabase client
│   │   ├── http.ts           # fetch wrapper, timeouts, ApiError
│   │   └── api.ts            # typed backend API calls
│   ├── pages/                # one file per route / screen
│   │   ├── Home.tsx
│   │   └── <Feature>/        # group related pages if needed
│   ├── App.tsx               # router definition
│   ├── main.tsx              # entrypoint
│   └── index.css             # Tailwind directives + theme tokens
├── index.html
├── vite.config.ts
├── tsconfig.json
└── package.json
```

### Where to put UI code

| UI element | Location |
| ---------- | -------- |
| Full screen / route | `src/pages/<Name>.tsx` |
| Reusable widget | `src/components/<Name>.tsx` |
| shadcn button, dialog, etc. | `src/components/ui/` (via CLI) |
| Backend API call | `src/lib/api.ts` |
| Auth/session helper | `src/lib/supabase.ts` |

Do not create `src/services/`, `src/hooks/` folders preemptively — add when a pattern repeats three times.

## Package manager

**pnpm only.** Lockfile: `pnpm-lock.yaml`. Delete any `package-lock.json` or `yarn.lock` that appear.

**Minimum release age: 7 days** (`.npmrc`). Override per-install only with commit justification.

## Backend integration

- Base URL from `VITE_API_BASE_URL` via `lib/env.ts`.
- All backend calls through `api` from `@/lib/api` — handles JSON, bearer token, timeouts, typed errors.
- Never pass auth tokens through component props — the api client reads the session.
- Never call OpenAI or third-party AI APIs directly from the browser.

```typescript
// lib/api.ts — conceptual shape
export const api = {
  get: <T>(path: string) => request<T>("GET", path),
  post: <T>(path: string, body: unknown) => request<T>("POST", path, body),
  // ...
};
```

## Configuration

- All env reads in `src/lib/env.ts` — validate required vars at boot.
- Vars prefixed `VITE_` only. Never expose secrets to the client.

Typical vars:

```text
VITE_API_BASE_URL
VITE_SUPABASE_URL          # if using auth
VITE_SUPABASE_ANON_KEY     # if using auth
```

## Page patterns by project type

| Project type | Typical pages |
| ------------ | ------------- |
| RAG chat | `Chat`, `ThreadList`, login |
| Agent dashboard | `Dashboard`, `RunDetail`, settings |
| Classification UI | `Upload`, `Results`, admin |
| Batch processor | `JobList`, `JobDetail`, status view |
| Minimal API wrapper | `Home` + one feature page |

See [../docs/patterns/](../docs/patterns/) for reference UI flows.

## Code style

- TypeScript strict — no `any`; prefer `unknown` and narrow.
- One component per file, small enough to read on one screen.
- Tailwind classes inline — global tokens in `index.css` only.
- `useState` / `useReducer` / `useContext` first for state.

## Testing

**No frontend tests.** Verify with:

```bash
pnpm tsc --noEmit
pnpm lint
```

Plus manual browser checks. Do not add vitest, Playwright, or Cypress unless the user explicitly changes this policy.

## Anti-patterns

- OpenAI / LLM calls from React components
- Reading `import.meta.env` outside `lib/env.ts`
- HTTP libraries (axios, ky) when `fetch` suffices
- Next.js, SSR, or a Node server in front of the SPA
- Custom CSS files or styled-components alongside Tailwind
- Hand-rolling shadcn primitives
- Mixing multiple state libraries (Zustand + Jotai + Redux)
- Backend logic or DB access in the frontend
