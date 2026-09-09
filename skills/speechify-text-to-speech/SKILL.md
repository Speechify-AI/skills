---
name: speechify-text-to-speech
description: >
  Synthesize speech audio from text or SSML with the Speechify API
  (POST /v1/audio/speech). Use when the user wants to "convert text to speech",
  "generate a voiceover / narration / audio file", "read this text aloud", or
  "make an MP3/WAV from text" as a single one-shot request. For realtime or
  first-byte-latency-sensitive playback use `speechify-streaming`; for word
  timestamps use `speechify-speech-marks`.
license: MIT
metadata:
  author: Speechify
  version: 0.1.0
  requires:
    env: [SPEECHIFY_API_KEY]
---

# Text to speech

`POST /v1/audio/speech` synthesises text or SSML and returns the **complete**
audio plus speech-mark metadata and billing in one JSON response. Use it when
you want the whole clip at once (files, batch jobs, short prompts). For streaming
playback, use [`speechify-streaming`](../speechify-streaming/SKILL.md).

## Verify first
`voice_id`, `model`, and the accepted `output_format` values change. Confirm the
current ones via `voices.list()` / `models.list()`, the `ask-speechify` MCP, or
`https://docs.speechify.ai` before hardcoding them. See
[`speechify-models`](../speechify-models/SKILL.md) and
[`speechify-voices`](../speechify-voices/SKILL.md).

## Request fields
- `input` (required) — text or SSML, up to **2,000 characters** per request for
  `/v1/audio/speech`. (The streaming endpoint allows longer input — see
  [`speechify-streaming`](../speechify-streaming/SKILL.md).)
- `voice_id` (required) — a voice identifier from `voices.list()`.
- `model` (optional) — defaults to `simba-3.0`. **Always set it explicitly** so
  behaviour is stable and you control the deprecation clock (see
  [`speechify-models`](../speechify-models/SKILL.md)).
- `audio_format` (optional) — `mp3`, `wav`, `ogg`, `aac`, `pcm` (default `wav`).
- `output_format` (optional) — granular sample-rate/bitrate control (e.g.
  `pcm_16000`, `ulaw_8000`, `mp3_24000_128`); **takes precedence** over
  `audio_format`. See [`speechify-audio-formats`](../speechify-audio-formats/SKILL.md).
- `language` (optional) — a BCP-47 tag (e.g. `en-US`, `es-ES`) selecting the
  language; if omitted, the voice's locale is used. See
  [`speechify-multilingual`](../speechify-multilingual/SKILL.md).

## Quick start (REST)
```bash
curl -s https://api.speechify.ai/v1/audio/speech \
  -H "Authorization: Bearer $SPEECHIFY_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "input": "Hello from Speechify.",
    "voice_id": "<voice-id-from-voices.list>",
    "model": "<current-model>",
    "audio_format": "mp3"
  }'
```

The response is JSON: base64-encoded `audio_data`, plus `audio_format`,
`output_format`, `speech_marks`, and `billable_characters_count`. Decode the
base64 to write the audio file. (Confirm the current field set against the live
API reference.)

## Quick start (SDK)
Prefer the Python or TypeScript SDK for typed access and base64 handling — see
[`speechify-sdks`](../speechify-sdks/SKILL.md) and
[`speechify-cookbook`](../speechify-cookbook/SKILL.md) for the `quickstart` recipe
in your language.

## Common mistakes
- Treating the response as raw audio bytes — it's JSON with **base64** audio;
  decode before writing.
- Passing SSML without telling the API (see [`speechify-ssml`](../speechify-ssml/SKILL.md)).
- Hardcoding a retired model → `400 model_retired`. Pin a current one.
- Sending > 2,000 characters to `/v1/audio/speech` — chunk the text, or switch
  to the streaming endpoint (higher input limit).
