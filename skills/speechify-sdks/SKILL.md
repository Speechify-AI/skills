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

Speechify ships two official SDKs plus a CLI. Prefer an SDK for typed access,
base64/stream handling, and less boilerplate; use raw REST ("native") when you
need full control or a language without an SDK.

- Python — `Speechify-AI/sdk-python`
- TypeScript — `Speechify-AI/sdk-typescript`

## Verify first
Package names, install commands, client class names, and method signatures
change between SDK versions. **Confirm the current install command and client API
from the SDK repo's README, `ask-speechify`, or the package registry** before
writing code — don't assume from memory. Pin the SDK version.

## Initialize (shape only — confirm exact API live)
```python
# Python — confirm package name & client from sdk-python README
from speechify import Speechify
client = Speechify(token="...")  # read from SPEECHIFY_API_KEY env
```
```ts
// TypeScript — confirm package name & client from sdk-typescript README
import { Speechify } from "@speechify/...";
const client = new Speechify({ token: process.env.SPEECHIFY_API_KEY });
```

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
