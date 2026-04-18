# Video Consumption & Display Usage — Current State

Captured: 2026-04-18

**Status:** documentation only. No options, no recommendations yet — just recording how things are used today.

## What Daniel actually watches

- **No traditional TV viewing.** Scheduled/broadcast TV is not part of the routine.
- **Documentaries.**
- **Netflix.**
- **YouTube.**
- **News — occasional.** Free-to-air Israeli news, watched legally over the local broadcast signal / official streams.

## Geographic context

- Household is based in **Israel**. Geo-availability materially affects what streaming services/content are accessible and how. Any future architecture decisions need to account for this rather than assume a US/EU catalogue.

## Video clients in use

- **Android TV** — bedroom, driving the Nebula projector. Used for Netflix, YouTube, Plex.
- **Raspberry Pi — living room TV.** Runs **Plex client** and **Kodi**. (Kodi previously omitted from the architecture vision doc — noted here.)
- **Plex server** — central, serving both clients from the NAS library.

## Displays are not single-purpose

The TVs / main display surfaces are **not just media centres**. They're toggled between several distinct uses:

- **Media playback** (Plex, Kodi, Netflix, YouTube).
- **Dashboards.** Examples Daniel has used:
  - A **geopolitical / news dashboard** — heavily used during the recent Iran conflict.
  - An **IP camera viewer** for monitoring the child (nursery camera).
- Switching between these modes is a normal part of daily use, not an edge case.

Implication for any future design: a solution that assumes the TV is "just a media centre running one app" doesn't fit. The display has to comfortably host media apps **and** arbitrary dashboards/web views, and the switch between them needs to be easy.

## Controller pain point

- Too many controllers/remotes in circulation. Each client/device tends to come with its own.
- Daniel is comfortable using **his phone** as the universal controller across all of them, if the stack supports it.
- An alternative that would also work: a **single keyboard** that can be redirected to different TVs (i.e. one physical input device, switchable target).
- Concepts floated in passing (just captured, not evaluated): **unified remote** style clients.

## Things to inventory later

- Exact list of apps/surfaces launched on the bedroom Android TV.
- Exact list of apps/surfaces launched on the living-room Pi (Plex vs Kodi split — when is each used?).
- Which dashboards are "real" recurring uses vs one-offs.
- Whether the IP camera viewer is a dedicated app, a browser page, or a HA dashboard.

## Explicitly out of scope right now

Per Daniel's instruction: stay in documentation mode. Do **not** jump to proposing architectures, remote-control products, launcher apps, or KVM-style solutions yet. Those come after the current state is fully captured.
