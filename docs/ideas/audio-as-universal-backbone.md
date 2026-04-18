# Audio as a Universal Backbone — Aspiration

Captured: 2026-04-18

**Status:** documentation only. Recording the aspiration and the use cases it has to cover — not yet picking a technology stack.

## The underlying intuition

Once you've invested in a multi-room audio layer (SBCs + speakers + Snapcast synchronisation), it feels wasteful for each media source to bypass it and ship audio out its own DAC / HDMI path. The point of multi-room audio, in Daniel's view, is that **everything that plays sound should be able to route through it** — not just music apps that happen to know about Snapcast.

Put differently: Snapcast (or whatever sync layer wins) should be treated as a **universal audio sink**, and every audio-producing surface in the house should be able to target it.

## Use cases this backbone should cover

### 1. TV / movie audio routed through the multi-room layer

- Scenario: watching something in the living room via Kodi / Plex / streaming app.
- Several speakers in that room are already wired up as Snapcast clients.
- Desire: the TV/movie audio itself plays via Snapcast to those speakers (and optionally to a grouped zone), rather than exiting through the TV's own speakers or a single DAC.
- Today: most media servers/clients don't have a first-class Snapcast output. They ship audio out their local audio device, which sidesteps the whole sync layer.
- Note: yes, you could sync externally via the DAC, but if the Snapcast infrastructure already exists that's a wasted architecture.

### 2. Music and podcasts (already covered in `audio-layer-options.md`)

- Spotify app as controller, Snapcast as the transport, named groups as targets. Cross-reference only — full treatment is in that doc.

### 3. Home Assistant TTS / notification audio

- Daniel has HA automations that emit TTS messages.
- These notifications are expected to work **on the same audio layer** — i.e. TTS should play on a specific speaker or a named group, not on some separate notification-only path.
- Implication: whatever stack is chosen, HA must be able to push TTS audio into it cleanly, and targeting (single speaker / group) has to work the same way it does for music.

### 4. Intercom / PA system

- Aspiration, not yet built: use the same speaker mesh + some kind of microphone capture (browser mic, phone mic) to enable **room-to-room intercom** or **whole-house PA announcements**.
- "Capture a mic feed from the browser or phone, push it to a speaker or a group" is the mental model.
- This would make the speaker network a household backbone — not just for media consumption, but for everyday communication.

## The real meta-goal

> "Find one way to layer all these in without getting lost in Icecast and a bunch of different technologies."

i.e. resist the outcome where each use case bolts on its own streaming technology (Icecast for X, librespot for Y, some other bridge for Z, yet another path for TTS, a fifth thing for the intercom). The win condition is a **single coherent audio backbone** with well-defined inputs (media clients, Spotify, TTS, mic capture) and well-defined outputs (individual speakers, named groups), with as few intermediate technologies as possible.

## Requirements this implies (for later evaluation)

To be applied when options are eventually compared — not now:

- Every audio-producing surface in the house must have a supported path into the sync layer: media clients (Kodi / Plex / browser video), Spotify, HA TTS, ad-hoc mic capture.
- Targeting semantics must be **uniform** across all sources. "Play on Main group" should mean the same thing whether the source is Spotify, a TTS notification, or an intercom announcement.
- Collision / priority behaviour needs a clear answer: what happens if the TV is playing movie audio on the Main group and an HA TTS notification fires? (Duck? Interrupt? Queue? Ignore?) Same question for intercom announcements.
- Minimise the number of distinct streaming technologies in the path.

## Open questions

- Which media clients currently in use (Kodi on the Pi, Plex clients, Android TV apps, Netflix/YouTube on Android TV) actually have a clean route into a Snapcast-style sink, and which are effectively closed boxes?
- Is browser/phone mic capture → speaker a viable intercom primitive given acceptable latency, or does it need dedicated hardware? [to confirm]
- How does HA TTS behave today when the target speaker is mid-playback of something else?
- Is there a single backbone technology that genuinely covers all four use-case families, or is the honest answer "two, with a clean boundary between them"?

## Explicitly out of scope right now

Still in documentation mode. Do not evaluate Icecast vs Snapcast vs PulseAudio-over-network vs PipeWire-over-network vs anything else. The job here is to have written down, clearly, what the backbone needs to do — so that when options *are* compared, the criteria are already on paper.
