# Home Media Planner

A planning workspace for gathering ideas, notes, and specs for my home media setup.

This is **not a code repository** — it's a place to jot, collect, and flesh out thinking before anything gets built or bought. Notes may start as quick scribbles, voice memos, or stray links, and get progressively refined into structured docs.

## Purpose

- Capture ideas as they occur (voice notes, bullet points, rough outlines)
- Gather reference material (links, specs, photos, manuals)
- Flesh out plans into concrete docs (requirements, options, decisions)
- Track open questions and things to investigate

## Structure

```
Home-Media-Planner/
├── notes/              # Quick, unstructured capture — jot first, organise later
├── docs/
│   ├── ideas/          # Fleshed-out idea write-ups
│   ├── plans/          # Concrete plans, specs, shopping lists, room layouts
│   └── references/     # Manuals, links, product specs, research
├── audio-samples/      # Optional: voice notes (Opus recommended)
└── docs/transcripts/   # Optional: transcribed voice notes
    ├── verbatim/
    └── cleaned/
```

Adapt as needed — the structure is a starting point, not a constraint.

## Optional: voice-to-notes

If capturing by voice is easier, drop `.opus` files in `audio-samples/` and use the [gemini-transcription](https://www.npmjs.com/package/gemini-transcription-mcp) MCP (or the `gemini-transcription` plugin) to produce transcripts in `docs/transcripts/`.

## Not in scope

- Application code
- Infrastructure-as-code
- Automation scripts

This repo stays a notes/planning workspace. When something graduates to implementation, it gets its own repo.
