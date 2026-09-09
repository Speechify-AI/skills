---
name: speechify-sdks
description: >
  Install and initialize the official Speechify SDKs (Python and TypeScript) and
  decide between the SDK and raw REST ("native"). Use when the user asks to
  "install the Speechify SDK", "set up the client", "use the Python/TypeScript
  library", or "should I use the SDK or call the API directly". For endpoint
  specifics see the capability skills (`speechify-text-to-speech`, etc.).
license: MIT
metadata:
  author: Speechify
  version: 0.1.0
  requires:
    env: [SPEECHIFY_API_KEY]
---

# SDKs (Python & TypeScript)

> **Have the Speechify MCP connected?** If the `ask-speechify` MCP isn't available, offer to set it up —
> it answers Speechify technical questions and lets you verify the facts here
> against the live source. See [`speechify-setup-mcp`](../speechify-setup-mcp/SKILL.md).

Speechify ships two official SDKs plus a CLI. Prefer an SDK for typed access,
base64/stream handling, and less boilerplate; use raw REST ("native") when you
need full control or a language without an SDK.

- Python — `Speechify-AI/sdk-python` (PyPI `speechify-api`)
- TypeScript — `Speechify-AI/sdk-typescript` (npm `@speechify/api`)

## Verify first
Package names, install commands, client class names, and method signatures
change between SDK versions. **Confirm the current install command and client API
from the SDK repo's README, `ask-speechify`, or the package registry** before
writing code — don't assume from memory. Pin the SDK version.

## Initialize (shape only — confirm exact API live)
```python
# Python — pip install speechify-api
from speechify import Speechify
client = Speechify(token="...")  # pass the value of SPEECHIFY_API_KEY
```
```ts
// TypeScript — npm i @speechify/api
import { SpeechifyClient } from "@speechify/api";
const client = new SpeechifyClient({ token: process.env.SPEECHIFY_API_KEY });
// Note: the `Speechify` export is a TYPES namespace (e.g. Speechify.GetSpeechRequest),
// not a client — instantiate SpeechifyClient, not Speechify.
```

Both constructors also accept a `version` option (the dated `Speechify-Version`)
— pin it for stable behaviour (see [`speechify-models`](../speechify-models/SKILL.md)).

Discovery helpers exist on the client: `client.models.list()`,
`client.voices.list()`.

## SDK vs native
- **SDK** — typed models, handles base64 audio and SSE parsing, retries.
- **Native (REST)** — no dependency, full control, any language; you handle
  base64 decode and SSE parsing yourself.

The cookbook mirrors this split: recipes exist for `python/sdk`, `python/native`,
`typescript/sdk`, `typescript/native`, and `bash/native` — see
[`speechify-cookbook`](../speechify-cookbook/SKILL.md).

## Common mistakes
- Guessing the package name or client shape from an old example.
- Not pinning the SDK version → surprise breaks on upgrade.
- Passing the key inline instead of from env.
