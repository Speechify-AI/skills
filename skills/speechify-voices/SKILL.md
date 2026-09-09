---
name: speechify-voices
description: >
  List, filter, and choose Speechify voices, and read voice metadata (language,
  gender, model support, preview). Use when the user asks "what voices are
  available", "list voices", "find a voice for <language/style>", "which voice
  should I use", or needs a `voice_id` for synthesis. For cloning a custom voice
  see `speechify-voice-cloning`; for model choice see `speechify-models`.
license: MIT
metadata:
  author: Speechify
  version: 0.1.0
  requires:
    env: [SPEECHIFY_API_KEY]
---

# Voices

Every synthesis needs a `voice_id`. This skill is how you find the right one.

## The rule: never invent a voice_id
Voice IDs and the catalogue change constantly. **Always** fetch the live list —
never hardcode an ID from memory or an old example.

```bash
curl -s https://api.speechify.ai/v1/voices \
  -H "Authorization: Bearer $SPEECHIFY_API_KEY"
```

Or via SDK: `client.voices.list()` (Python / TypeScript). See
[`speechify-sdks`](../speechify-sdks/SKILL.md).

## Choosing
Each voice carries metadata — confirmed fields include `id`, `display_name`,
`locale`, `gender`, `type` (e.g. shared vs a `personal`/cloned voice), and
`models` (which models it supports). Filter on:
- **Language/locale** — the voice's `locale` must match your target language and
  model (see [`speechify-multilingual`](../speechify-multilingual/SKILL.md)).
- **Model support** — check `models` covers the model you plan to use
  ([`speechify-models`](../speechify-models/SKILL.md)).
- **Persona** — pick by `display_name` / `gender`, and audition candidates by
  synthesising a short sample.

## Verify first
Confirm the exact response fields (`id` vs `voice_id`, nested language objects)
against the API reference or `ask-speechify` — shapes evolve.

## Common mistakes
- Reusing a `voice_id` from a tutorial that no longer exists → `400`/`404`.
- Choosing a voice that doesn't support your model or language.

See the `list-voices` and `voice-language-model-support` recipes via
[`speechify-cookbook`](../speechify-cookbook/SKILL.md).
