# Reference patterns

Optional architecture guides for specific AI product types. **Do not implement these unless [project-brief.md](../project-brief.md) calls for them.**

| Pattern | When to use |
| ------- | ----------- |
| [Grounded RAG chat](grounded-rag-chat.md) | Document Q&A with citations and hybrid retrieval |

When starting a new project:

1. Fill in `project-brief.md` and pick a project type.
2. Read [architecture.md](../architecture.md) for shared boundaries.
3. Read the matching pattern doc if one exists.
4. Create only the backend/frontend modules the pattern (and brief) require.

If no pattern matches, design modules from the brief using the repo layout in [AGENTS.md](../../AGENTS.md).
