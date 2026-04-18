# Home Media Architecture — Vision & Current State

Captured: 2026-04-18

## Objectives

- Streamlined enjoyment of home media (video + audio) across rooms.
- Reduce complication — fewer moving parts, clearer mental model of what lives where.
- Reduce brittleness — current stack has fragile layers (see pain points) that need hardening or replacing.
- Keep ownership of content (legally owned digital videos, TV series, music library) under a single, trusted storage root.

## Current setup

### Storage
- **NAS** — primary store for legally owned digital video (films, TV series). Acts as the content library root.

### Video / streaming
- **Plex server** — runs on a central server; indexes the NAS library and streams to clients.
- **Clients:**
  - **Bedroom** — Android TV device driving a Nebula projector. Plex client.
  - **Living room** — Raspberry Pi connected to a TV. Runs Plex client **and Kodi**.

See `video-consumption-and-displays.md` for what's actually watched (documentaries, Netflix, YouTube, occasional Israeli free-to-air news), the Israel geo-availability constraint, and the fact that TVs are also used for non-media surfaces (geopolitical dashboard, IP nursery-camera viewer). Also captures the controller-proliferation pain point.

### Audio / multi-room
- Several **SBCs** distributed around the house, each wired to a speaker.
- Each SBC runs a **Snapcast client**.
- Typical use: listening to podcasts; occasionally per-room music; sometimes grouped into room zones for whole-home or themed playback.
- **Home Assistant + Music Assistant** orchestrates the audio layer and feeds Snapcast.

### Control plane
- **Home Assistant** — the glue: scenes, room grouping, voice/automation triggers into the media stack.

## Pain points

- **Home Assistant → Music Assistant → Snapcast chain is brittle.** This is the single biggest source of friction. Failure modes to characterise (to be documented in a dedicated troubleshooting/decision doc):
  - [to confirm] Snapcast clients dropping / desyncing.
  - [to confirm] Music Assistant losing track of players or stalling on stream start.
  - [to confirm] Group playback behaving inconsistently vs single-room.
- Too many layers between "I want to hear X in room Y" and sound actually coming out.

## Direction / what to explore in this repo

This repo is the place to think through — in markdown, not code — how to reshape the stack around the objectives above. Candidate workstreams to flesh out as their own `docs/ideas/*.md` or `docs/plans/*.md`:

1. **Audio layer simplification.** See `audio-layer-options.md` — fleshed out separately. Core tension: Daniel wants Spotify's native app as the controller *and* wants "whole house" / "main speakers (ex-nursery)" as first-class targets. Current MA + Snapcast + Spotify-integration glue can't do both without brittle scripting.
1a. **Audio as a universal backbone.** See `audio-as-universal-backbone.md`. Broader aspiration: the multi-room sync layer shouldn't just carry music — TV/movie audio from Kodi/Plex, HA TTS notifications, and a future browser/phone-mic intercom/PA should all route through the same backbone, with uniform targeting. Meta-goal: one coherent layer, not a pile of Icecast/librespot/other bridges.
2. **Client standardisation.** Android TV vs Raspberry Pi vs SBC — is there a case to converge on fewer client OS/hardware profiles?
3. **Plex alternatives / complements.** Whether Plex stays the canonical video server or whether Jellyfin / a lighter option earns a look.
4. **Role of Home Assistant.** Keep HA as control plane only, or let it own more of the media logic? Where does the boundary sit?
5. **Room inventory.** Per-room: what device, what speakers, what it's actually used for, what breaks.

## Open questions

- Exact failure modes of the HA/MA/Snapcast layer — needs logging/observation before redesigning.
- Which rooms have SBCs today and what's on each? (Inventory doc needed.)
- Is the NAS model / capacity / redundancy adequate for the planned library growth? [to confirm]
- Any content sources outside the NAS (streaming subscriptions, local rips in progress) that the architecture needs to accommodate?

## Out of scope for this repo

Per repo CLAUDE.md: no code, no Home Assistant YAML, no Ansible, no shell scripts. When any workstream above graduates to implementation, it forks off into its own repo.
