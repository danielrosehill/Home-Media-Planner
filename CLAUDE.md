# CLAUDE.md

## Repo purpose

Planning workspace for Daniel's home media setup. Notes-only — **no code, no infra-as-code, no automation scripts**. When something graduates to implementation, it gets its own repo.

## How to work here

- Quick, unstructured capture goes in `notes/` — don't over-structure; jot first, organise later.
- Fleshed-out write-ups go in `docs/ideas/`.
- Concrete plans, specs, shopping lists, room/rack layouts go in `docs/plans/`.
- Reference material (manuals, links, product specs, research) goes in `docs/references/`.
- Voice memos, if used, go in `audio-samples/`; transcripts in `docs/transcripts/{verbatim,cleaned}/`.

## When helping Daniel

- Default to producing markdown notes and plans, not code.
- When he drops a rough idea, help flesh it out into a structured `docs/ideas/<slug>.md` — requirements, options considered, open questions, rough cost/complexity.
- When he's deciding between options, produce a comparison doc in `docs/plans/`.
- Keep file names descriptive and `kebab-case.md`.
- Don't invent product specs — if unsure, mark as `[to confirm]` or put it in open questions.

## Out of scope

- Writing shell scripts, Python, Ansible, Home Assistant YAML, etc. — those belong in dedicated repos.
- Any code generation beyond inline config examples needed to document a decision.
