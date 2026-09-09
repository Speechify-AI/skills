---
name: speechify-ssml
description: >
  Control Speechify synthesis with SSML — pauses, emphasis, pronunciation,
  prosody (rate/pitch/volume), and emotion. Use when the user wants to "add
  pauses", "control pronunciation", "make it sound excited/sad/calm", "adjust
  speaking rate or pitch", "use SSML", or shape delivery for IVR/audiobooks. For
  plain text synthesis see `speechify-text-to-speech`.
license: MIT
metadata:
  author: Speechify
  version: 0.1.0
  requires:
    env: [SPEECHIFY_API_KEY]
---

# SSML & emotion

SSML is markup you pass in the `input` field to control how text is spoken —
pauses, emphasis, pronunciation, prosody, and emotional delivery.

## Basics
Wrap the text in `<speak>…</speak>` and use SSML tags inside. Common tags:
- `<break time="500ms"/>` — insert a pause.
- `<emphasis>…</emphasis>` — stress a phrase.
- `<prosody rate="..." pitch="..." volume="...">…</prosody>` — shape delivery.
- pronunciation/say-as tags for numbers, dates, acronyms.
- emotion control for excited/calm/serious/etc. delivery.

Pass the SSML as `input` to `POST /v1/audio/speech` (or the streaming endpoints)
exactly as you would plain text.

## Verify first
The **exact supported SSML tags, attributes, and emotion values are
model-dependent and change.** Confirm the current set against
`https://docs.speechify.ai` (append `.md`) or the `ask-speechify` MCP before
relying on a specific tag — an unsupported tag may be ignored or error.

## Common mistakes
- Unescaped `&`, `<`, `>` inside SSML text → parse errors. Escape them.
- Missing the `<speak>` root.
- Assuming a tag works on every model — emotion/prosody support varies by model
  ([`speechify-models`](../speechify-models/SKILL.md)).

See the `ssml-emotion` recipe via [`speechify-cookbook`](../speechify-cookbook/SKILL.md)
and the `ssml-emotion-tts` / `ivr-ssml` demos via
[`speechify-demos`](../speechify-demos/SKILL.md).
