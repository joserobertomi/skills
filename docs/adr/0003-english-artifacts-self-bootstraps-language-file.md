# `english-artifacts` self-bootstraps its language file instead of only defaulting

ADR-0001 classified `english-artifacts` as a hard dependency on `docs/agents/artifact-language.md`, written by `/setup-matt-pocock-skills`. Without that file, the skill fell back to a documented default (English artifacts + pt-BR conversation) — silently, every session, with no path to actually persist a different choice short of running the full four-section setup wizard.

In practice this read as broken: a user working in a repo without setup got the same default every time, with no memory of ever having stated a preference, even within the same project across sessions.

**Decision:** `english-artifacts` now checks for `docs/agents/artifact-language.md` itself (workflow step 0). If missing, it asks the conversation-language question once and writes a minimal version of the file directly — no need to run the full setup wizard just to answer one question. `/setup-matt-pocock-skills` remains the way to set this *together with* issue tracker, triage labels, and domain docs, or to revisit the choice later.

This keeps the change additive and scoped to `english-artifacts/SKILL.md`, a file we already own per [[0002]] — no edits to upstream-shared files.
