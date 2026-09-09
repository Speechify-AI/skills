---
name: speechify-speech-marks
description: >
  Get and use word/character-level speech marks (timestamps) from Speechify to
  build captions, subtitles, karaoke-style word highlighting, or to align audio
  with UI. Use when the user asks for "word timestamps", "captions", "subtitles",
  "highlight words as they're spoken", "SRT/VTT", or "sync text to audio". Depends
  on `speechify-text-to-speech` (one-shot) or `speechify-streaming` (realtime) to
  produce the audio.
license: MIT
metadata:
  author: Speechify
  version: 0.1.0
  requires:
    env: [SPEECHIFY_API_KEY]
---

# Speech marks (timestamps & captions)

Speech marks give you the timing of each word/character in the synthesised
audio, so you can highlight text, generate subtitles, or align a UI.

## Where they come from
- **One-shot:** `POST /v1/audio/speech` returns speech marks alongside the audio
  in the same JSON response. See [`speechify-text-to-speech`](../speechify-text-to-speech/SKILL.md).
- **Streaming:** `POST /v1/audio/stream/with-timestamps` emits them inside
  `speech.chunk` SSE events. See [`speechify-streaming`](../speechify-streaming/SKILL.md).

## The timing rule
Speech-mark times are **absolute milliseconds from the start of the synthesis.**
When streaming, concatenate all audio chunks into one continuous stream and apply
the marks against that single timeline — do not reset the clock per chunk.

## Verify first
The exact speech-mark object shape (nested chunks, `start`/`end`, `type`) can
change. Confirm the current schema against the API reference (`https://docs.speechify.ai`,
append `.md`) or the `ask-speechify` MCP before writing a parser.

## The mark schema (watch the field names)
Each mark carries **two different pairs** — don't mix them up:
- `start_time` / `end_time` (doubles) — **millisecond** timings. Use these for
  captions and highlighting.
- `start` / `end` (integers) — **character offsets** into the input text, not ms.
- plus `type`, `value`, and nested `chunks`.

Confirm the current shape against the live API reference — but building captions
off `start`/`end` instead of `start_time`/`end_time` is the classic broken parser.

## Building captions
1. Synthesise with marks (one-shot or streaming).
2. Walk the marks, taking each word's `start_time` / `end_time` (milliseconds).
3. Emit your caption format (SRT/VTT: convert ms → `HH:MM:SS,mmm`).
4. For live highlighting, schedule UI updates against playback currentTime using
   the same absolute-ms values.

## Common mistakes
- Estimating word timings instead of using the returned marks.
- Per-chunk time bases when streaming (times are absolute).
- Off-by-one between character-level and word-level marks — pick the granularity
  your UI needs.

See the `speech-marks` recipe via [`speechify-cookbook`](../speechify-cookbook/SKILL.md)
and the `captions-speech-marks` demo via [`speechify-demos`](../speechify-demos/SKILL.md).
