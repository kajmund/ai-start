# Project brief — [Your Project Name]

Fill in this template before building. Delete placeholder text as you go. Coding agents read this file to decide **what** to build — [AGENTS.md](../AGENTS.md) tells them **how** and **where**.

## Project type

Pick one (delete the others):

- [ ] **Grounded RAG chat** — document Q&A with citations ([pattern](../patterns/grounded-rag-chat.md))
- [ ] **AI agent** — multi-step agent with tools
- [ ] **Classification / extraction API** — structured output from inputs
- [ ] **Batch processor** — ingest and process documents offline
- [ ] **Internal dashboard** — AI-powered analytics or workflow UI
- [ ] **API-only MVP** — no frontend initially
- [ ] **Other:** [describe]

## The product

**[Your Project Name]** is [one sentence: what it is and who it's for].

Examples:

- *An internal chatbot that lets support agents query product docs with cited answers.*
- *An API that extracts invoice fields from uploaded PDFs.*
- *A dashboard where analysts run AI classification on customer feedback.*

## The users

- **Primary users:** [role, count, context]
- **How they work today:** [current workflow without this product]
- **What success looks like:** [measurable outcome]

## The problem

[Describe the pain point. What takes too long? What breaks trust? What scales poorly?]

## What we're building

[Describe the core capability — adapt to your project type, don't assume chat.]

Examples by type:

**RAG chat:** Users ask questions in plain English, get sourced answers with citations.

**Extraction API:** Users upload documents, receive structured JSON fields.

**Agent:** Users describe a goal, the agent uses tools to complete multi-step tasks.

**Batch:** Operators trigger jobs that process a corpus and store results in Postgres.

## Example inputs / acceptance criteria

List 5–10 real inputs or scenarios. These become test cases and acceptance criteria.

1. [Example 1]
2. [Example 2]
3. [Example 3]
4. [Example 4]
5. [Example 5]

## Quality bar

Define non-negotiables for your domain:

- **Accuracy:** [e.g. never invent facts / maintain >90% field accuracy]
- **Transparency:** [e.g. always cite sources / show confidence scores]
- **Safety:** [e.g. no medical advice / no PII in logs]
- **Failure mode:** [e.g. say "insufficient data" rather than guess]

A wrong but confident answer is worse than no answer (for most AI products).

## Technical scope

Check what this project needs:

- [ ] Frontend (React SPA)
- [ ] Supabase Auth (email)
- [ ] Postgres tables + Alembic migrations
- [ ] pgvector / semantic search
- [ ] Streaming responses (chat)
- [ ] Batch scripts (`backend/scripts/`)
- [ ] File upload handling

## Constraints

- **Data sources:** [document types, APIs, volume]
- **Users:** [count, auth method]
- **Hosting:** [Railway / other, budget]
- **Latency:** [acceptable response time]
- **Model budget:** [OpenAI spend limits if relevant]

## Out of scope (explicitly)

- [Feature you're not building]
- [Feature you're not building]
- [Feature you're not building]

## Definition of done

[How you'll know the MVP works — pilot criteria, metrics, or user feedback threshold.]

Example: *Five users test for a week; 4/5 say it saves meaningful time. Roll out if yes.*
