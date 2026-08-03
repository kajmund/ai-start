# Project brief — [Your Project Name]

Fill in this template before building. Delete the placeholder text and guidance notes as you go.

## The product

**[Your Project Name]** is [one sentence: what it is and who it's for].

Example: *An internal chatbot that lets support agents query product documentation and get sourced, citable answers.*

## The users

- **Primary users:** [role, count, context]
- **How they work today:** [current workflow without the chatbot]
- **What success looks like:** [measurable outcome]

## The problem

[Describe the pain point in plain language. What takes too long? What breaks trust? What scales poorly?]

The work is:

- [pain 1]
- [pain 2]
- [pain 3]

## What we're building

A [internal / customer-facing] chatbot where users can:

- Ask questions in plain English about [your corpus]
- Get sourced answers that cite [specific source + location — page, section, timestamp, etc.]
- Trust the answer enough to [downstream action]
- Use it from [browser / Slack / etc.], logged in with [auth method]
- See their own past conversations

## Example questions

List 5–10 real questions your users would ask. These become acceptance criteria for retrieval and grounding.

1. [Example question 1]
2. [Example question 2]
3. [Example question 3]
4. [Example question 4]
5. [Example question 5]

## What "trust" means here

Define the non-negotiables for your domain:

- **Never invent facts.** If the answer isn't in the corpus, say so.
- **Always cite.** Every factual claim links to a source.
- **Show the underlying passage** so the user can verify in one click.
- **[Domain-specific rule]** e.g. no medical advice, no legal conclusions, no investment recommendations.

A wrong but confident answer is worse than no answer.

## Constraints

- **Corpus:** [document types, sources, date range, size]
- **Users:** [count, auth method]
- **Hosting:** [cloud footprint, team infra capacity]
- **Latency:** [acceptable response time]
- **Budget:** [OpenAI / hosting limits if relevant]

## Out of scope (explicitly)

- [Feature or integration you're not building]
- [Feature or integration you're not building]
- [Feature or integration you're not building]

## Definition of done

[How you'll know the MVP works — pilot criteria, metrics, or user feedback threshold.]

Example: *Five users try it for a week and report it saves at least 2 hours each. If yes, roll out to the full team.*
