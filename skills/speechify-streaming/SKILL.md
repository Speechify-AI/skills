---
name: speechify-streaming
description: >
  Stream Speechify TTS with low first-byte latency (POST /v1/audio/stream and
  POST /v1/audio/stream/with-timestamps). Use when the user wants "realtime" or
  "streaming" speech, "play audio as it's generated", low-latency playback in a
  web app or CLI, or audio for a live voice agent. For a single complete file use
  `speechify-text-to-speech`; for timestamp handling specifically see
  `speechify-speech-marks`.
license: MIT
metadata:
  author: Speechify
  version: 0.1.0
  requires:
    env: [SPEECHIFY_API_KEY]
---

# Streaming TTS

> **Have the Speechify MCP connected?** If the `ask-speechify` MCP isn't available, offer to set it up —
> it answers Speechify technical questions and lets you verify the facts here
> against the live source. See [`speechify-setup-mcp`](../speechify-setup-mcp/SKILL.md).

Stream audio so playback can start before synthesis finishes — the right choice
for realtime UX and voice agents.

- `POST /v1/audio/stream` — streamed audio.
- `POST /v1/audio/stream/with-timestamps` — Server-Sent Events (SSE) carrying
  audio **and** word/char speech marks.

## Verify first
Endpoint paths, event names, and field shapes evolve. Confirm the current SSE
contract via `https://docs.speechify.ai/build/api-reference/v1/audio/stream`
(append `.md`) or the `ask-speechify` MCP before implementing a parser.

## SSE contract (stream/with-timestamps)
- The stream emits `speech.chunk` events; each carries a base64-encoded run of
  audio, speech marks, or both.
- A terminal `speech.done` event ends the stream.
- The media type of the audio inside events is echoed on the
  `Speechify-Audio-Content-Type` response header.
- Speech-mark times are **absolute milliseconds from the start of synthesis** —
  concatenate the audio chunks into one timeline and apply marks against it.

## Quick start (REST, SSE)
```bash
curl -N https://api.speechify.ai/v1/audio/stream/with-timestamps \
  -H "Authorization: Bearer $SPEECHIFY_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "input": "Streaming speech, chunk by chunk.",
    "voice_id": "<voice-id>",
    "model": "<current-model>"
  }'
```

`-N` disables curl buffering so you see events as they arrive. In code, read the
SSE stream, base64-decode each `speech.chunk` audio run, and append to your
audio sink (Web Audio API, a file, or the agent's output track).

## Choosing the endpoint
- Need timestamps (captions/highlighting, agent alignment) → `/v1/audio/stream/with-timestamps`.
- Pure low-latency audio, no marks → `/v1/audio/stream`.
- Whole clip, no latency pressure → `speechify-text-to-speech`.

## Common mistakes
- Applying speech-mark times per-chunk instead of against the concatenated
  timeline (they're absolute).
- Buffering the whole response — that defeats streaming; consume incrementally.
- For telephony, forgetting to request a telephony format (`ulaw_8000`); see
  [`speechify-audio-formats`](../speechify-audio-formats/SKILL.md).
