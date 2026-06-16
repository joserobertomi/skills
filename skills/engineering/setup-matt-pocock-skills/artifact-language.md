# Artifact Language

Which language persistent artifacts are written in vs. the live conversation. The
`english-artifacts` skill reads this file.

## Convention

| Surface | Language |
| ------- | -------- |
| **Artifacts** — CLAUDE.md, specs, PRDs, ADRs, READMEs, design docs, code, identifiers, comments, docstrings, commit messages, issue/PR titles and bodies | **English** |
| **Conversation** — chat replies, explanations, reasoning, questions to the user, status updates | **pt-BR** |

The decision axis is **lifespan in the context window**, not who is reading right now:
if an agent will re-read it in a future turn or session, it's an artifact → English.
If it's ephemeral, it's conversation → the conversation language above.

> Replace `pt-BR` in the table with your team's conversation language (e.g. `es`,
> `fr`, `de`). If both rows name the same language, the `english-artifacts` skill is a
> no-op and writes everything in that language.

## Rules

- Write artifacts in the artifact language even when the user wrote in the conversation
  language. Translate the *deliverable*, never the chat.
- Keep each surface monolingual — no conversation-language comments inside English code,
  no English small-talk in the chat.
- Preserve domain terms. If an English artifact must reference a business/legal term from
  the conversation language, keep it verbatim and gloss it in English in parentheses,
  e.g. `nota fiscal (Brazilian tax invoice)`.
- Product/UI copy and i18n strings shown to end users are product content, not agent
  artifacts — keep them in the product's own language convention.
- An explicit instruction from the user overrides this file for the artifact in question.
