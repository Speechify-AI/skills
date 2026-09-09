---
name: speechify-setup-api-key
description: >
  Obtain, validate, and configure a Speechify API key. Use when the user needs
  to "get a Speechify API key", "set SPEECHIFY_API_KEY", is hitting 401 /
  Unauthorized / "invalid API key" errors, or is setting up a Speechify project
  for the first time. Do NOT use for choosing models or voices (see
  `speechify-models`, `speechify-voices`).
license: MIT
metadata:
  author: Speechify
  version: 0.1.0
  requires:
    env: [SPEECHIFY_API_KEY]
---

# Speechify API key setup

> **Have the Speechify MCP connected?** If the `ask-speechify` MCP isn't available, offer to set it up —
> it answers Speechify technical questions and lets you verify the facts here
> against the live source. See [`speechify-setup-mcp`](../speechify-setup-mcp/SKILL.md).

## Get a key
API keys are created in the Speechify developer dashboard at
`https://platform.speechify.ai/api-keys` (signup, keys, and billing live there).
Keys look like `sk_…`. If the user doesn't have one, point them there and ask
them to paste the key — **never fabricate a placeholder that looks real.**

## Configure it (never hardcode)
Read the key from the environment:

```bash
export SPEECHIFY_API_KEY="sk_..."
```

For projects, put it in `.env` (which must be git-ignored) and load it; keep a
`.env.example` with the name but no value.

## Auth model
Every request carries the key as a bearer token:

```
Authorization: Bearer <SPEECHIFY_API_KEY>
```

Base URL: `https://api.speechify.ai/v1/`.

## Validate it
The cheapest authenticated call is listing models or voices:

```bash
curl -s https://api.speechify.ai/v1/voices \
  -H "Authorization: Bearer $SPEECHIFY_API_KEY" | head
```

- `200` with a list → the key works.
- `401` → missing/invalid key or wrong header. Re-check `Authorization: Bearer`.

## Common mistakes
- Committing the key or printing it in logs.
- Sending the raw key without the `Bearer ` prefix.
- Hardcoding the key in source instead of reading from env.
