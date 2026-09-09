# Contributing skills

## Layout

One folder per skill; the folder name **must equal** the `name` in frontmatter.
Every skill name is prefixed `speechify-` (except the `speechify` router) so it
stays globally unique when installed alongside other vendors' skills.

```
skills/
  speechify-text-to-speech/
    SKILL.md            # required
    references/         # optional — deep detail, loaded on demand
    scripts/            # optional — runnable helpers
    assets/             # optional — templates, configs
```

## Frontmatter

```yaml
---
name: speechify-text-to-speech          # must match the folder name
description: >
  One or two sentences on what the skill does, then explicit trigger phrases
  and NOT-cases. This is a router, not a title — pack it with the words a user
  would actually say, and say when NOT to fire.
license: MIT
metadata:
  author: Speechify
  version: 0.1.0
  requires:
    env: [SPEECHIFY_API_KEY]
---
```

- **`name`, `description`** are the only functional fields; keep them portable.
- **`description`** is the highest-leverage line. State when the skill should and
  should **not** trigger, and name the sibling skill to use instead.
- Keep `SKILL.md` under ~5k tokens. Push depth into `references/`.

## Body shape

A reliable structure (borrowed from the field's best sets):

1. **When to use / when not to** — one line each; link the sibling skill.
2. **Verify first** — the live-source reminder for this skill's facts.
3. **Core steps / decision table** — how to think, not a spec dump.
4. **Copy-paste quick start** — minimal, env-based, runnable.
5. **Common mistakes** — the traps specific to this capability.

## The one rule

Do **not** bake in volatile facts (model IDs, voice IDs, endpoints, output
formats, version dates). Reference the live source instead:

- `ask-speechify` MCP
- `https://docs.speechify.ai` (append `.md` to any page)
- SDK discovery calls (`models.list()`, `voices.list()`)

A skill that hardcodes a model ID will rot. A skill that tells the agent how to
find the current one will not. See `docs/alignment.md` for the full rationale.

## After editing

- Bump `metadata.version` for the skill you changed.
- If you add a skill, list it in `README.md` and register it in
  `.claude-plugin/marketplace.json`.
