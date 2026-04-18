# Planning Preferences & Ground Rules

Captured: 2026-04-18

Durable preferences that apply to all ideas, plans, and diagrams in this repo. Any future proposal that violates one of these should call it out explicitly as a tradeoff.

## 1. Home Assistant is not the integration backbone

HA has useful automations but tends to introduce brittleness at the integration surface — the current HA + Music Assistant + Snapcast chain is the concrete pain point that drove this whole planning effort.

**Rule:** the media/audio backbone must be a **standalone system**. HA is only ever an external caller that talks *into* it through a well-defined interface (e.g. posting a TTS clip to an HTTP endpoint). HA is never the glue that holds the backbone together.

**How to apply:** when sketching any architecture, show HA as an external box with a single narrow arrow into the backbone. If a proposed option only works by baking logic into HA automations/integrations, flag that as a con.

## 2. Snapcast (or whatever sync layer wins) is a universal audio sink

Every audio-producing surface in the house should be able to target the multi-room sync layer — music, podcasts, TV/movie audio, HA TTS, intercom/PA. No use case gets to sidestep it and ship audio out its own DAC.

## 3. The native Spotify app is a first-class controller

Solutions that require giving up the native Spotify app in favour of a secondary UI (e.g. Music Assistant / Iris) are dispreferred. Named groups should appear as selectable speakers inside the Spotify app itself.

## 4. One coherent layer, not a pile of bridges

Meta-goal, in Daniel's words: *"find one way to layer all these in without getting lost in Icecast and a bunch of different technologies."* Minimise the number of distinct streaming technologies in any proposed path.

## 5. Stay in documentation mode until told otherwise

This repo is planning-only. No code, no Home Assistant YAML, no Ansible, no shell scripts (see root `CLAUDE.md`). When an idea graduates to implementation, it forks into its own repo.

## 6. Don't jump to options until the current state is captured

When Daniel describes a new pain point or use case, document it first. Only produce option analyses / recommendations when he signals readiness (or the documentation for that area is already in place).

## 7. Public-safe diagrams

This repo is public. Diagrams and docs must not contain real IP addresses, hostnames, MAC addresses, or other network identifiers. Topology and role labels only.
