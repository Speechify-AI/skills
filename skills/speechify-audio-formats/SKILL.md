---
name: speechify-audio-formats
description: >
  Choose the right Speechify output audio format for the target — web playback,
  telephony, or raw PCM. Use when the user asks about "MP3/WAV/OGG/AAC", "sample
  rate", "bitrate", "PCM", "mu-law / ulaw_8000", "telephony audio", or hits a 400
  about an unsupported format. Governs the `audio_format` / `output_format`
  request fields used by `speechify-text-to-speech` and `speechify-streaming`.
license: MIT
metadata:
  author: Speechify
  version: 0.1.0
  requires:
    env: [SPEECHIFY_API_KEY]
---

# Audio output formats

Two request fields control output:

- **`audio_format`** — container/codec: `mp3`, `wav`, `ogg`, `aac`, `pcm` (default `wav`).
- **`output_format`** — granular sample-rate/bitrate control (e.g. `pcm_16000`,
  `pcm_24000`, `ulaw_8000`, `mp3_24000_128`). When present it **takes precedence**
  over `audio_format`.

An invalid `output_format` returns `400` with the list of supported values —
read that list rather than guessing.

## Pick by target
| Target | Format |
| --- | --- |
| Web / general playback | `mp3` (or an `mp3_*` output_format) |
| Lossless / editing | `wav` |
| Telephony (Twilio, IVR) | `ulaw_8000` (8 kHz mu-law) |
| Feeding a raw audio pipeline / voice agent | `pcm_16000` / `pcm_24000` |

For telephony and voice-agent transports the sample rate must match what the
transport expects — mismatched rates cause chipmunk/slow-motion audio.

## Verify first
The exact set of accepted `output_format` values changes. Confirm the current
list via the `400` error body, `https://docs.speechify.ai` (append `.md`), or
`ask-speechify` before hardcoding.

## Common mistakes
- Setting both fields and being surprised `output_format` wins.
- Wrong sample rate into a telephony/agent transport (see
  [`speechify-voice-agents`](../speechify-voice-agents/SKILL.md)).

See the `output-formats` recipe via [`speechify-cookbook`](../speechify-cookbook/SKILL.md).
