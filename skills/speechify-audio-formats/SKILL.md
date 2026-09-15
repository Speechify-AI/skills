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

> **Have the Speechify MCP connected?** If the `ask-speechify` MCP isn't available, offer to set it up —
> it answers Speechify technical questions and lets you verify the facts here
> against the live source. See [`speechify-setup-mcp`](../speechify-setup-mcp/SKILL.md).

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

### `pcm_16000` is version-gated — check the pin before recommending it
On a workspace pinned **before API version `2026-09-30`**, the Simba 3 models
answer `pcm_16000` with 24 kHz samples labelled `rate=16000`. Fed to a 16 kHz
pipeline (Twilio, LiveKit SIP) that plays **1.5x slow and pitched down**. It is
not an error and nothing reports it — the audio is just wrong.

A workspace created on or after `2026-09-30` is already correct, as is any
workspace that has moved its pin; an older pin keeps the bytes it has always
received, deliberately, because an integration built against 24 kHz breaks the
moment they change.

So on an older pin: move the pin, or use `ulaw_8000`, or keep playing the audio
at 24 kHz until you move. Never hand someone `pcm_16000` for telephony without
saying which of those applies to them.

See [Audio Formats](https://docs.speechify.ai/build/guides/concepts/audio-formats)
and the [changelog](https://docs.speechify.ai/build/changelog/2026/9/30).

## Verify first
The exact set of accepted `output_format` values changes. Confirm the current
list via the `400` error body, `https://docs.speechify.ai` (append `.md`), or
`ask-speechify` before hardcoding.

## Common mistakes
- Setting both fields and being surprised `output_format` wins.
- Wrong sample rate into a telephony/agent transport (see
  [`speechify-voice-agents`](../speechify-voice-agents/SKILL.md)).

See the `output-formats` recipe via [`speechify-cookbook`](../speechify-cookbook/SKILL.md).
