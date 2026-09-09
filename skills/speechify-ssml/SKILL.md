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

> **Have the Speechify MCP connected?** If the `ask-speechify` MCP isn't available, offer to set it up —
> it answers Speechify technical questions and lets you verify the facts here
> against the live source. See [`speechify-setup-mcp`](../speechify-setup-mcp/SKILL.md).

SSML is markup you pass in the `input` field to control how text is spoken —
pauses, emphasis, pronunciation, prosody, and emotional delivery.

## Basics
Wrap the text in `<speak>…</speak>` and use SSML tags inside. Common tags:
- `<break time="500ms"/>` — insert a pause (also accepts `strength`).
- `<emphasis level="strong">…</emphasis>` — stress a phrase (`reduced` / `moderate` / `strong`).
- `<prosody rate="..." pitch="..." volume="...">…</prosody>` — shape delivery (named steps or percentages).
- `<sub alias="...">…</sub>` — control pronunciation / substitute text (names, acronyms, account numbers). (Standard `say-as` is **not** documented — use `<sub>`.)

Pass the SSML as `input` to `POST /v1/audio/speech` (or the streaming endpoints)
exactly as you would plain text.

## Emotion is a Speechify tag, not standard SSML
Emotion is **not** a generic SSML feature — it's a custom Speechify element:

```xml
<speak><speechify:style emotion="cheerful">Great to see you!</speechify:style></speak>
```

The `emotion` values are a fixed set (verify the current list): `angry`,
`cheerful`, `sad`, `terrified`, `relaxed`, `fearful`, `surprised`, `calm`,
`assertive`, `energetic`, `warm`, `direct`, `bright`. There is **no** `excited`
or `serious` — reach for `energetic` / `direct` instead.

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
