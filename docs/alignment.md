# Skills alignment: how Speechify's set was designed

This repo's skill set is a **superset** distilled from what the leading voice /
AI-infra companies ship as coding-agent skills, mapped onto Speechify's actual
product surface. This document records the research and the reasoning so the set
can be maintained deliberately rather than by accretion.

## What the field ships

Surveyed 2026-09. All figures are the published, first-party skill sets.

| Company | Repo | # skills | Organising model | Distribution |
| --- | --- | --- | --- | --- |
| **Cartesia** | `cartesia-ai/skills` | 2 (`cartesia-api`, `cartesia-line`) | API-integration vs managed voice-agent | skills.sh + Claude plugin + Cursor + agent-plugins + hosted MCP |
| **Deepgram** | `deepgram/skills` | 6 core + per-SDK product skills | Developer journey: `api` → `docs` → `starters` → `recipes` → `examples` → `setup-mcp` | skills.sh + Claude plugin marketplace |
| **ElevenLabs** | `elevenlabs/skills` + `elevenlabs/plugin` | 10 product + 2 MCP-driver + 30 "Architect" agent-lifecycle | Product skills / account-operation / agent lifecycle | skills.sh + Claude + Cursor + Codex + hosted MCP |
| **LiveKit** | `livekit/agent-skills` | 2 (`livekit-agents`, `livekit-simulations`) | Architecture-only; facts via live-docs MCP | skills.sh + git |
| **Pipecat** | `pipecat-ai/skills` | 3 (`init`, `deploy`, `talk`) | Build → deploy → interact lifecycle; live-discovery via CLI/MCP | Claude plugin marketplace |
| **OpenAI / Codex** | `openai/plugins` | ~500 across 62 plugins | Vendor connectors; Twilio's ~50-skill kit is the best audio template | Codex plugin marketplace |
| **Vercel** | `vercel/vercel-plugin` + `vercel-labs/agent-skills` | 35 (plugin) / 8 (open) | Platform capability skills; overlay+upstream sync | `npx skills` + `npx plugins` |

## What everyone agrees on (the conventions we adopt)

1. **Anthropic Agent Skills format.** One folder per skill; a `SKILL.md` with
   YAML frontmatter (`name`, `description`); optional `references/`, `scripts/`,
   `assets/`. Portable across Claude Code, Cursor, Codex, and ~75 agents via the
   open `skills` CLI. The same folder set installs everywhere.
2. **The `description` is a router, not a title.** It enumerates literal trigger
   phrases *and* explicit NOT-cases so the agent auto-activates the right skill.
   This is the single highest-leverage field.
3. **Progressive disclosure.** Keep `SKILL.md` lean (< ~5k tokens); push depth
   into `references/` loaded on demand.
4. **Never trust model memory for API facts** — the dominant design philosophy
   across Cartesia, LiveKit, Deepgram, and Pipecat. Skills encode *how to think*
   (architecture, latency budgets, decision tables) and defer volatile facts
   (model IDs, voice IDs, endpoints, flags, versions) to a **live source**.
   Speechify's live source is the `ask-speechify` MCP plus docs at
   `https://docs.speechify.ai` (append `.md` to any page for agent-readable
   Markdown).
5. **Multi-channel from one repo.** `npx skills add <owner>/<repo>` for any
   agent, plus a Claude Code plugin marketplace manifest, from a single source of
   truth.
6. **Two architectural layers.** Almost everyone splits (a) **product / API
   integration** from (b) **voice-agent / orchestration**. We do too.
7. **A router / "architect" skill.** Twilio (`tier: discover`), ElevenLabs, and
   Pipecat's `init` all ship an entry skill that qualifies the use case and
   routes to the implementation skills. Ours is `speechify`.

## Speechify's product surface (what our skills must cover)

From the API reference, SDKs, cookbook (63 recipes), and demos (26):

- **TTS** — `POST /v1/audio/speech` (text or SSML → audio + speech marks + billing in one JSON response).
- **Streaming** — `POST /v1/audio/stream` and `POST /v1/audio/stream-with-timestamps` (SSE; `speech.chunk` / `speech.done` events; base64 audio; `Speechify-Audio-Content-Type` header).
- **Models** — `simba-3.2` (English, lowest latency), `simba-3.0` (multilingual: en, de, es, fr, it, pt). Legacy `simba-english` / `simba-multilingual` are retiring (`400 model_retired` after 2026-09-21; off 2026-11-21). Verify the live list via `models.list()`.
- **Voices** — list / filter / metadata; **voice cloning** from a 10–30s sample.
- **Speech marks** — word/char timestamps; times are absolute ms from synthesis start.
- **SSML + emotion**, **multilingual**, **output formats** (`mp3`/`wav`/`ogg`/`aac`; granular `output_format`: `pcm_16000`, `pcm_24000`, `ulaw_8000`, `mp3_24000_128`, …), **watermark**, **version pinning / idempotency**, **error handling**.
- **Auth** — `Authorization: Bearer sk_…`; base URL `https://api.speechify.ai/v1/`.
- **SDKs** — Python (`Speechify-AI/sdk-python`) and TypeScript (`Speechify-AI/sdk-typescript`); plus a Speechify CLI.
- **Integrations (demos)** — LiveKit (Node + Python), Pipecat, Vapi, Twilio, Vercel AI SDK, Mastra, Deepgram shim, Discord, Slack.

Speechify is **TTS-first** (not an STT vendor). The set therefore centres on
synthesis, streaming, voices, captions, and Speechify-as-the-TTS-leg inside
other companies' voice-agent pipelines — the exact white space no competitor's
skill set covers.

## The Speechify superset

Tiered like ElevenLabs/Deepgram, split API-vs-agent like Cartesia, thin-and-
architecture-first like LiveKit, journey-organised like Deepgram.

### Tier 0 — Entry & setup
- **`speechify`** — router + core doctrine (base URL, auth, model currency, the "verify against live source" rule, and when to use each other skill). Always-on entry point.
- **`speechify-setup-api-key`** — obtain, validate, and configure `SPEECHIFY_API_KEY`.

### Tier 1 — Core TTS product skills
- **`speechify-text-to-speech`** — synthesise speech from text/SSML via `/v1/audio/speech`.
- **`speechify-streaming`** — low-latency streaming via `/v1/audio/stream(-with-timestamps)`; web/CLI playback.
- **`speechify-speech-marks`** — word/char timestamps → captions, subtitles, karaoke highlighting.
- **`speechify-voices`** — list, filter, and choose voices; language/model support.
- **`speechify-voice-cloning`** — clone a voice from a sample and synthesise with it.
- **`speechify-ssml`** — SSML structure and emotion/prosody control.
- **`speechify-multilingual`** — multilingual synthesis, language/locale/accent taxonomy.
- **`speechify-audio-formats`** — pick the right output format (web, telephony `ulaw_8000`, PCM, WAV).
- **`speechify-models`** — model selection + the deprecation/version-pinning discipline.

### Tier 2 — Integration / voice agents
- **`speechify-voice-agents`** — use Speechify as the TTS leg inside realtime voice agents (LiveKit, Pipecat, Vapi, Twilio) and the Vercel AI SDK / Mastra; latency, barge-in, streaming into the pipeline.

### Tier 3 — SDK & discovery
- **`speechify-sdks`** — install/init the Python & TypeScript SDKs; native REST vs SDK; when to use which.
- **`speechify-cookbook`** — match a task to one of the 63 cookbook recipes.
- **`speechify-demos`** — match a use case to one of the 26 clonable demos.

## Deliberately deferred (roadmap, not v1)

Patterns worth adopting as the repo matures, tracked here so the omission is a
decision, not a gap:

- **Per-SDK × product skills** (Deepgram model) — only if the SDK surface grows
  enough that folding language examples into each product skill stops scaling.
- **MCP-driver skills** (ElevenLabs `agents-platform`) — a skill that *operates a
  Speechify account* via MCP rather than writing app code.
- **An agent-lifecycle "Architect" set** (ElevenLabs) — build → tools → test →
  tune → deploy → monitor. Overkill until Speechify ships an agent platform.
- **`setup-mcp`** (Deepgram) — wire the `ask-speechify` MCP into the user's
  editor. Fold into `speechify` for now.
- **An `evals/` harness** (ElevenLabs) — per-skill trigger + functional evals.
- **A generated `api` skill** (Deepgram) — build the API-reference skill from the
  OpenAPI spec so it never drifts.
- **Cursor `.mdc` rule + `.cursor-plugin` / `.codex-plugin` manifests** — the
  `skills` CLI already installs into those agents; add native plugin manifests
  when there's demand.
