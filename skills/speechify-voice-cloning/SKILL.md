---
name: speechify-voice-cloning
description: >
  Create a cloned Speechify voice from an audio sample and synthesize speech with
  it. Use when the user wants to "clone a voice", "create a custom voice", "use my
  own voice", "make a voice from a recording", or "narrate in a specific person's
  voice". Requires consent to clone the source voice. For picking an existing
  stock voice see `speechify-voices`.
license: MIT
metadata:
  author: Speechify
  version: 0.1.0
  requires:
    env: [SPEECHIFY_API_KEY]
---

# Voice cloning

Create a custom voice from a short sample, then use its `voice_id` in synthesis
like any other voice.

## Consent first
Only clone a voice you have permission to clone. Confirm the user has rights/
consent for the source recording before proceeding.

## Flow
1. Provide a clean speech sample — **10–30 seconds**, single speaker, minimal
   background noise. (Confirm current duration/format requirements via
   `ask-speechify` or `https://docs.speechify.ai/tts/guides/voice-cloning`.)
2. **Complete the consent challenge (now required).** Create one via
   `POST /v1/voices/consent-challenges`, then pass its `consent_challenge_id`
   plus a `consent_recording` file (a spoken-consent clip, ~5–30s, ≤25 MB) when
   creating the voice. This is the default from `Speechify-Version: 2026-09-13`;
   omitting it returns a `400`.
3. Create the voice — `POST /v1/voices` / `client.voices.create()`. Verify the
   exact request shape live — it changes.
4. Use the returned `voice_id` in `POST /v1/audio/speech` or the streaming
   endpoints, exactly like a stock voice. Cloned voices also appear in
   `voices.list()` (with `type: personal`).

## Verify first
The create-voice endpoint path, accepted sample formats, and duration limits are
volatile — confirm against the live docs / `ask-speechify` before implementing.
List your cloned voices with `voices.list()` (they appear alongside stock voices).

## Common mistakes
- Noisy, multi-speaker, or too-short samples → poor clones.
- Assuming a cloned voice supports every model — check model support
  ([`speechify-models`](../speechify-models/SKILL.md)).
- Cloning without consent.

See the `voice-cloning` recipe via [`speechify-cookbook`](../speechify-cookbook/SKILL.md)
and the `next-voice-cloning-app` / `voice-cloning-narration` demos via
[`speechify-demos`](../speechify-demos/SKILL.md).
