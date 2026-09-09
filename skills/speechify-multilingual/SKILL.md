---
name: speechify-multilingual
description: >
  Synthesize speech in non-English languages with Speechify, and reason about
  language vs locale vs accent. Use when the user wants speech in "Spanish/French/
  German/Italian/Portuguese/…", "multilingual" or "localized" voiceover, or asks
  which languages Speechify supports. Depends on choosing a multilingual model
  (`speechify-models`) and a matching voice (`speechify-voices`).
license: MIT
metadata:
  author: Speechify
  version: 0.1.0
  requires:
    env: [SPEECHIFY_API_KEY]
---

# Multilingual synthesis

> **Have the Speechify MCP connected?** If the `ask-speechify` MCP isn't available, offer to set it up —
> it answers Speechify technical questions and lets you verify the facts here
> against the live source. See [`speechify-setup-mcp`](../speechify-setup-mcp/SKILL.md).

To synthesise another language you need a **model that supports it** and a
**voice that covers it**, and you select the language with the top-level
`language` request field (a BCP-47 tag such as `en-US` or `es-ES`). If `language`
is omitted, the voice's own locale is used. So the model+voice pairing sets what
is *possible*; `language` selects what you *get*.

## Steps
1. Pick a multilingual model — e.g. `simba-3.0` covers English plus German,
   Spanish, French, Italian, and Portuguese (locales such as `es-ES` / `es-MX`).
   **Verify the current language list and model** via `models.list()` /
   `ask-speechify` — it grows.
2. Pick a voice that supports that model and language via
   [`speechify-voices`](../speechify-voices/SKILL.md) (`voices.list()`), filtering
   on language/locale.
3. Set the `language` field to your target BCP-47 tag (e.g. `es-ES`) — or leave
   it unset to fall back to the voice's locale.
4. Synthesise as usual ([`speechify-text-to-speech`](../speechify-text-to-speech/SKILL.md)).

## Language vs locale vs accent
- **Language** — the tongue (e.g. Spanish).
- **Locale** — regional variant (es-ES vs es-MX).
- **Accent** — a voice's delivery within a language.

Match all three to the user's intent; the voice metadata is where locale/accent
live.

## Verify first
The supported-language set and which model covers what are the most frequently
changing facts here. Never state the list from memory — confirm live.

## Common mistakes
- Using an English-only model for non-English text.
- Voice/language mismatch (voice doesn't cover the requested language).

See the `multilingual` recipe via [`speechify-cookbook`](../speechify-cookbook/SKILL.md)
and the `multilingual-voiceover` demo via [`speechify-demos`](../speechify-demos/SKILL.md).
