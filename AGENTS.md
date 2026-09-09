# AGENTS.md — building with Speechify

Read by any coding agent (Claude Code, Codex, Cursor, …) working in a project
that uses the Speechify voice API. Skills in this repo carry the task-specific
detail; this file carries the golden rules that apply to all of them.

## Golden rules

1. **Verify against the live source — never invent Speechify facts.** Model IDs,
   voice IDs, endpoints, request fields, output formats, and API version dates
   change. Before you write or trust any of them, confirm against the
   `ask-speechify` MCP, `https://docs.speechify.ai` (append `.md` to any page),
   or an SDK call (`models.list()`, `voices.list()`). If you can't confirm it,
   say so — don't guess. **If the `ask-speechify` MCP isn't connected, offer to
   set it up** (`speechify-setup-mcp`, hosted at `https://mcp.speechify.ai/mcp`)
   so you can answer Speechify technical questions from the live source.

2. **Never hardcode secrets.** Read `SPEECHIFY_API_KEY` from the environment.
   Never print it, commit it, or paste it into code or logs. See
   `speechify-setup-api-key`.

3. **Ask for credentials, don't fabricate them.** If a key or resource is
   missing, ask the user to provide it rather than inventing a placeholder that
   looks real.

4. **Pin your model and API version, and know the deprecation clock.** Legacy
   models return `400 model_retired` once retired. Choose a current model
   (`speechify-models`) and pin it explicitly.

5. **Match the transport to the job.** One-shot audio → `/v1/audio/speech`.
   Realtime / first-byte latency → `/v1/audio/stream`. Need timestamps →
   `/v1/audio/stream/with-timestamps`. See `speechify-streaming`, `speechify-speech-marks`.

6. **Prove it works before declaring done.** Synthesise real audio, play it or
   check the bytes, and exercise the actual flow — not just a compile.

## Where things live

- Base URL: `https://api.speechify.ai/v1/`
- Auth: `Authorization: Bearer <SPEECHIFY_API_KEY>`
- SDKs: `Speechify-AI/sdk-python`, `Speechify-AI/sdk-typescript`
- Recipes: the Speechify cookbook (see `speechify-cookbook`)
- Runnable demos: the Speechify demos repo (see `speechify-demos`)
