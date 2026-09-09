---
name: speechify
description: >
  Entry point for building anything with the Speechify voice API. Use whenever
  the user wants to "add text-to-speech", "generate audio / narration / a
  voiceover", "read text aloud", "stream TTS", "clone a voice", "add captions or
  word timestamps", "use SSML", "synthesize speech in another language", or "use
  Speechify" — and you're not yet sure which specific Speechify skill applies.
  Routes to the right skill and carries the rules that apply to all of them.
  Do NOT use for unrelated audio libraries or non-Speechify TTS providers.
license: MIT
metadata:
  author: Speechify
  version: 0.1.0
  requires:
    env: [SPEECHIFY_API_KEY]
---

# Building with Speechify

Speechify is a **text-to-speech** platform: turn text or SSML into natural
speech, stream it with low latency, get word-level timestamps, clone voices, and
plug it into voice agents as the TTS leg. This skill orients you and routes to
the specialised skill for the task.

## The one rule (applies to every Speechify skill)

**Never invent Speechify API facts.** Model IDs, voice IDs, endpoints, request
fields, output formats, and API version dates change. Confirm them against a
live source before you write or trust them:

- **`ask-speechify` MCP** — grounded answers over Speechify's repos, SDKs, API
  reference, and docs.
- **Docs** — `https://docs.speechify.ai` (append `.md` to any page for
  agent-readable Markdown).
- **SDK discovery** — `models.list()`, `voices.list()`.

If you can't confirm a fact, say so and go check — don't guess.

## Essentials

- Base URL: `https://api.speechify.ai/v1/`
- Auth: `Authorization: Bearer <SPEECHIFY_API_KEY>` (read from env; never hardcode).
- One-shot synthesis: `POST /v1/audio/speech` → audio + speech marks + billing in one JSON response.
- Streaming: `POST /v1/audio/stream`, `POST /v1/audio/stream/with-timestamps` (SSE).
- SDKs: Python (`Speechify-AI/sdk-python`), TypeScript (`Speechify-AI/sdk-typescript`).

## Route to the right skill

| The task is about… | Use |
| --- | --- |
| Getting/validating the API key | [`speechify-setup-api-key`](../speechify-setup-api-key/SKILL.md) |
| Synthesising audio from text or SSML | [`speechify-text-to-speech`](../speechify-text-to-speech/SKILL.md) |
| Low-latency / realtime streaming | [`speechify-streaming`](../speechify-streaming/SKILL.md) |
| Word timestamps, captions, highlighting | [`speechify-speech-marks`](../speechify-speech-marks/SKILL.md) |
| Choosing / listing voices | [`speechify-voices`](../speechify-voices/SKILL.md) |
| Cloning a voice from a sample | [`speechify-voice-cloning`](../speechify-voice-cloning/SKILL.md) |
| SSML structure, emotion, prosody | [`speechify-ssml`](../speechify-ssml/SKILL.md) |
| Non-English / multilingual synthesis | [`speechify-multilingual`](../speechify-multilingual/SKILL.md) |
| Output format (web, telephony, PCM) | [`speechify-audio-formats`](../speechify-audio-formats/SKILL.md) |
| Which model, deprecation, version pinning | [`speechify-models`](../speechify-models/SKILL.md) |
| Speechify inside LiveKit/Pipecat/Vapi/Twilio/AI SDK | [`speechify-voice-agents`](../speechify-voice-agents/SKILL.md) |
| Installing/using the Python or TS SDK | [`speechify-sdks`](../speechify-sdks/SKILL.md) |
| Finding a code recipe | [`speechify-cookbook`](../speechify-cookbook/SKILL.md) |
| Finding a runnable demo to clone | [`speechify-demos`](../speechify-demos/SKILL.md) |

## Before you finish
Prove it: synthesise real audio and check the bytes or play it. A compile is not
a working integration.
