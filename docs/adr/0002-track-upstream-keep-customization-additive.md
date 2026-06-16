# Track upstream; keep fork customization additive

This repo is a fork of [`mattpocock/skills`](https://github.com/mattpocock/skills) and pulls updates from it via an `upstream` remote. Every change we make is also a future merge-conflict surface: when upstream edits a file we have also edited, `git merge upstream/main` conflicts. The cost of a customization is therefore not its one-time effort but its recurring merge cost across every future sync.

We classify our divergences by that cost and prefer the cheap ones:

- **Additive (preferred)** — new files upstream never touches: new skill folders (e.g. `english-artifacts/`), new seed templates, new ADRs. Near-zero merge cost. This is why `english-artifacts` ships as its own skill plus a new `## Agent skills` section in the setup, rather than by rewriting existing skills.
- **Clearly-ours docs** — files we intentionally own and reframe (`README.md`, `CONTEXT.md`). Conflicts are expected but localized and ours to resolve.
- **Edits scattered across upstream-shared files (avoid)** — a string changed in many otherwise-pristine upstream files. This is the expensive case.

**Decision: do not make cosmetic renames of upstream-shared skills, slugs, or pointers.** We tried renaming `setup-matt-pocock-skills` → `setup-joserobertomi-skills`; it touched ~13 files including the `run /setup-matt-pocock-skills` pointers in `to-issues`, `to-prd`, `triage`, and `review`, each becoming a permanent conflict point. The gain was purely cosmetic, so we reverted it. Branding belongs in the README (which already credits Matt Pocock), not in shared skill names.

If a low-friction rename is ever genuinely needed, set up `git rerere` (to replay the identical resolution each sync) plus a documented fork-delta manifest before hand-editing shared files — don't absorb the recurring conflict manually.
