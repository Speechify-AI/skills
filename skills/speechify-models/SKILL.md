---
name: speechify-models
description: >
  Choose a Speechify TTS model, understand the trade-offs (latency vs
  multilingual coverage), and follow the deprecation / version-pinning
  discipline. Use when the user asks "which model should I use", "what models are
  available", hits "400 model_retired", or needs to pin a model/API version for
  reproducibility. Pairs with `speechify-voices` (voice must support the model).
license: MIT
metadata:
  author: Speechify
  version: 0.1.0
  requires:
    env: [SPEECHIFY_API_KEY]
---

# Models & versioning

## The rule: never state the model list from memory
Model IDs and their capabilities change, and old models get retired. **Always**
confirm the current catalogue live:

```bash
curl -s https://api.speechify.ai/v1/models \
  -H "Authorization: Bearer $SPEECHIFY_API_KEY"
```

Or `client.models.list()` (SDK), or the `ask-speechify` MCP.

## Current shape (verify before relying on it)
As of writing:
- `simba-3.2` — English, lowest latency.
- `simba-3.0` — multilingual (English + de, es, fr, it, pt).

Legacy `simba-english` / `simba-multilingual` are **retiring**: they return
`400 model_retired` after 2026-09-21 and are switched off entirely on
2026-11-21. Treat these dates as a live fact to re-confirm, not gospel — check
the docs/changelog.

## Choosing
- English-only, latency-sensitive (agents, realtime) → the fastest English model.
- Non-English or mixed language → a multilingual model
  ([`speechify-multilingual`](../speechify-multilingual/SKILL.md)).
- The chosen voice must support the chosen model
  ([`speechify-voices`](../speechify-voices/SKILL.md)).

## Pin for reproducibility
- Set `model` explicitly on every request — don't rely on a default.
- Where the API supports a dated version header, pin it so behaviour is stable.
- Use idempotency practices for retried requests.

## Common mistakes
- Hardcoding a retired model → `400 model_retired`. Migrate before the cutoff.
- Assuming a default model — always specify.

See the `list-models` and `version-pinning-and-idempotency` recipes via
[`speechify-cookbook`](../speechify-cookbook/SKILL.md).
