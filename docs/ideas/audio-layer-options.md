# Audio Layer — Problem Statement & Options

Captured: 2026-04-18

## What Daniel actually wants to do

Two very ordinary use cases that are currently hard:

1. **Target an individual speaker** — e.g. send a podcast to one SBC/speaker.
2. **Target a predefined group** — with at minimum two named groups:
   - **Whole house** — every speaker.
   - **Main speakers** — everywhere *except* the nursery (young child may be sleeping).

Constraint: switching between "individual speaker" and "a named group" must be **fast and low-friction** — opening the Snapcast server web UI and re-assigning clients every time is not acceptable.

## Primary content source

- **Spotify Premium** — used heavily for podcasts and music.
- Daniel wants to be able to use the **native Spotify app** as the controller (pick episode, scrub, queue) and have it land on a chosen speaker or group.
- Individual speakers can be made visible to Spotify by running a Spotify Connect listener on each client. Unification across clients today only happens via Snapcast.

## Current stack's failure modes

- **Music Assistant**
  - Playback is often laggy to start.
  - Feed population is unreliable.
  - The **Iris** frontend doesn't work well.
  - **Squeezelite** path is more reliable but feels dated.
  - **Spotify integration is a mess** — works for basic cases inside MA, but combining it with use of the native Spotify app creates stream collisions. The only workaround is custom scripting to arbitrate streams, which is exactly the kind of complexity this repo is trying to eliminate.
- **Snapcast**
  - Good at synchronising multiple clients.
  - Group management UX is poor — web UI only, not suited to "change target zone in 2 seconds from my phone".
- **Net effect:** the flexible path (use Spotify app directly + still be able to hit a group) requires brittle glue. The simple path (everything through Music Assistant) degrades the source experience.

## The core tension

Daniel wants **both** of:
- (a) Use Spotify's own app as the primary controller, because it's the best UX for the content source.
- (b) Treat "whole house" and "main speakers" as first-class targets the way individual Spotify Connect speakers are.

Spotify Connect has no native concept of a synchronised group across arbitrary speakers — only its own multi-Connect grouping, which doesn't include SBC/Snapcast endpoints. Bridging (a) and (b) is what forces the current complexity.

## Option space to evaluate

These are directions to flesh out — not decisions.

### A. Keep Spotify app as controller; expose groups as Spotify Connect endpoints

Run a Spotify Connect receiver (e.g. `librespot`-based) that feeds into Snapcast, and expose **each predefined group as its own "virtual speaker"** in Spotify:

- "House" endpoint → librespot → Snapcast stream → every client.
- "Main" endpoint → librespot → Snapcast stream → all clients except nursery.
- Per-room endpoints → librespot → single Snapcast client each.

Pros: native Spotify app works end-to-end; groups become just another speaker name; no script arbitration.
Cons: needs one librespot instance per exposed endpoint; collision behaviour (what if two endpoints play at once?) needs defining; nursery exclusion is enforced by stream membership, which needs to be stable.
Open questions: does this stay stable across SBC reboots? How does pause/play latency feel vs native Connect?

### B. Drop Music Assistant; thin controller in front of Snapcast only

Remove Music Assistant from the path entirely. Use Spotify app (via Connect + bridge) for Spotify; keep Snapcast as the sync layer; replace MA with something minimal (or just Home Assistant buttons / a small dashboard) that does one job: **re-target the active stream to a named group**.

Pros: fewer layers; Music Assistant's brittleness is removed.
Cons: lose MA's aggregation of non-Spotify sources; need to define how non-Spotify content (local library, other streamers) plays.

### C. Harden the current stack

Leave the topology as-is (HA + MA + Snapcast + Spotify integration), but invest in diagnosing the specific MA lag / Iris / Spotify-integration failures and fix or work around them. Accept Music Assistant as the Spotify controller; don't try to use the Spotify app natively.

Pros: no migration.
Cons: doesn't address the root complaint — Daniel *wants* to use the Spotify app. This option essentially asks him to give that up.

### D. Commit to Squeezelite / LMS path

Squeezelite is noted as more reliable than MA but old-fashioned. Logitech Media Server has mature group/zone semantics and a Spotify plugin. Evaluate whether the UX floor (dated interface) is acceptable given the reliability ceiling.

Pros: known reliable; zones are a first-class concept.
Cons: UX; Spotify plugin quality [to confirm]; doesn't solve "I want to drive from the Spotify app".

## Decision criteria (to apply once options are fleshed out)

- Can Daniel change target (single speaker ↔ "Main" ↔ "House") in **≤ 2 taps** on his phone?
- Does the **native Spotify app** work as a first-class controller, or is he forced into a secondary UI?
- **Nursery exclusion** must be reliable — the "Main" group should never accidentally include the nursery speaker.
- How many moving parts does the solution add vs remove?
- What breaks if any single component (HA, MA, Snapserver, an SBC) is down?

## Open questions

- How many SBC/speaker endpoints exist today, and which room is each in? (Needs inventory — see architecture vision doc.)
- Is the nursery speaker always the *same* SBC, or does it move? (Affects how "Main" is defined — by device ID or by room tag.)
- Non-Spotify audio sources in active use today? (Local library, other podcast apps, radio, etc.) — determines whether option B's "drop MA" is viable.
- Acceptable latency for pause/skip from the Spotify app landing on a grouped Snapcast stream?

## Next step

Before choosing a direction: produce the **room/speaker inventory** and decide the **nursery-exclusion rule** (device-based vs room-based). Both feed directly into option A's endpoint design.
