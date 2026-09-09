---
name: speechify-cookbook
description: >
  Find the right Speechify cookbook recipe — minimal, runnable code for one
  feature in one language. Use when the user wants "an example", "sample code",
  "how do I do X with Speechify", or "a snippet" for a specific capability. Points
  to recipes rather than a full app; for a complete clonable project see
  `speechify-demos`.
license: MIT
metadata:
  author: Speechify
  version: 0.1.0
---

# Speechify cookbook

> **Have the Speechify MCP connected?** If the `ask-speechify` MCP isn't available, offer to set it up —
> it answers Speechify technical questions and lets you verify the facts here
> against the live source. See [`speechify-setup-mcp`](../speechify-setup-mcp/SKILL.md).

The Speechify cookbook (`Speechify-AI/cookbook`) holds small, runnable recipes.
Match the user's task to a recipe, then adapt it.

## Two axes
- **Language / mode:** `bash/native`, `python/native`, `python/sdk`,
  `typescript/native`, `typescript/sdk`. "native" = raw REST; "sdk" = official
  SDK (see [`speechify-sdks`](../speechify-sdks/SKILL.md)).
- **Topic:** the capability you want.

## Topics (recipe → skill)
| Recipe topic | Related skill |
| --- | --- |
| `quickstart` | [`speechify-text-to-speech`](../speechify-text-to-speech/SKILL.md) |
| `streaming` | [`speechify-streaming`](../speechify-streaming/SKILL.md) |
| `speech-marks` | [`speechify-speech-marks`](../speechify-speech-marks/SKILL.md) |
| `list-voices`, `voice-language-model-support` | [`speechify-voices`](../speechify-voices/SKILL.md) |
| `voice-cloning` | [`speechify-voice-cloning`](../speechify-voice-cloning/SKILL.md) |
| `ssml-emotion` | [`speechify-ssml`](../speechify-ssml/SKILL.md) |
| `multilingual` | [`speechify-multilingual`](../speechify-multilingual/SKILL.md) |
| `output-formats` | [`speechify-audio-formats`](../speechify-audio-formats/SKILL.md) |
| `list-models`, `version-pinning-and-idempotency` | [`speechify-models`](../speechify-models/SKILL.md) |
| `watermark`, `error-handling` | — |

## Verify first
The recipe set grows and paths change. Confirm the current list and exact path
via the `ask-speechify` MCP or the cookbook repo before linking a user to one —
the table above is a map, not a guarantee.

Path shape: `recipes/audio/<language>/<native|sdk>/<topic>/`.
