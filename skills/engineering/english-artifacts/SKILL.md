---
name: english-artifacts
description: Write durable artifacts (CLAUDE.md, specs, PRDs, ADRs, READMEs, design docs, code, identifiers, comments, docstrings, commit messages, issue/PR titles and bodies) in English while keeping the day-to-day conversation in Portuguese (pt-BR). Use when working in a Portuguese-speaking project, when creating or editing persistent documentation/specs/code, or when the user mentions "idioma dos artefatos", "docs em inglês", bilingual workflow, or English-first artifacts.
---

# English Artifacts, Native-Language Conversation

Persistent artifacts stay in **English**. Live conversation stays in the project's **conversation language**.

The two languages come from `docs/agents/artifact-language.md`. If that file exists, read it and substitute its configured conversation language wherever this skill says "Portuguese (pt-BR)" below.

If the file is absent, don't just silently default every session — bootstrap it once (see step 0 of the Workflow) so the choice persists for next time. `/setup-matt-pocock-skills` covers this file as part of a larger wizard (issue tracker, triage labels, domain docs); run it instead if you want to configure those too, or if you want to revisit the choice later.

## The rule

Decide by **lifespan in the context window**, not by who is reading right now:

| Will an agent re-read this in a future turn/session? | Language |
|---|---|
| **Yes — artifact**: CLAUDE.md, specs, PRDs, ADRs, README, design docs, code, identifiers, comments, docstrings, commit messages, issue/PR titles and bodies | **English** |
| **No — conversation**: chat replies, explanations, reasoning, questions to the user, status updates | **Portuguese (pt-BR)** |

When in doubt ask: *will this re-enter the context window later?* If yes → English.

## Why (one line)

The high-token material that is re-read every turn is where English pays off: English instructions empirically beat in-language prompts on multilingual benchmarks, and English tokenizes more cheaply — so each artifact earns the gain every time it re-enters context. Full backing in [REFERENCE.md](REFERENCE.md).

## Workflow

0. **Bootstrap the language file if missing.** Check for `docs/agents/artifact-language.md`. If it's not there, ask the user once: *"Which language should I use for our conversation? (Artifacts will stay in English unless you say otherwise.)"* Then write `docs/agents/artifact-language.md`:

   ```markdown
   # Artifact Language

   Which language persistent artifacts are written in vs. the live conversation. The
   `english-artifacts` skill reads this file.

   | Surface | Language |
   | ------- | -------- |
   | **Artifacts** — CLAUDE.md, specs, PRDs, ADRs, READMEs, design docs, code, identifiers, comments, docstrings, commit messages, issue/PR titles and bodies | **English** |
   | **Conversation** — chat replies, explanations, reasoning, questions to the user, status updates | **<answer>** |
   ```

   Use this to answer the question once per repo, not once per turn — once the file exists, skip this step and just read it. (`/setup-matt-pocock-skills` writes the same file with more ceremony, as part of a larger wizard; either path is fine.)
1. **Detect the surface.** Editing/creating a file, commit, issue, PR, or spec → artifact. Talking to the user → conversation.
2. **Write artifacts in English** even when the user asked in Portuguese. Translate the *deliverable*, never the chat.
3. **Reply in Portuguese** about what you did.
4. **Don't mix.** No Portuguese comments inside English code; no English small-talk. Keep each surface monolingual.
5. **Preserve domain terms.** If an English artifact must reference a Portuguese business/legal term, keep it verbatim and gloss it in English in parentheses, e.g. `nota fiscal (Brazilian tax invoice)`.

## Examples

- User (pt): "cria um spec pro fluxo de faturamento" → file `docs/billing-flow.spec.md` written in English; you reply in pt-BR: "Pronto, criei o spec em inglês em `docs/billing-flow.spec.md`."
- User (pt): "explica como o tokenizer funciona" → explanation in Portuguese (ephemeral conversation).
- Commit → English: `Add retry to billing webhook`; your chat summary → Portuguese.

## Edge cases

- **Product/UI copy & i18n** shown to pt-BR end users is product content, not an agent artifact → keep it in Portuguese (or the project's i18n convention). This rule governs *engineering* artifacts, not localized product strings.
- **Direct quotes** from Portuguese sources stay verbatim.
- **Explicit override wins.** If the user says "esse doc tem que ser em português", obey the user.
