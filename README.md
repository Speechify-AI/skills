# Speechify Agent Skills

Reusable [Agent Skills](https://agentskills.io/specification) for building with
the **Speechify voice API** — text-to-speech, streaming, speech marks, voice
cloning, SSML, multilingual synthesis, and using Speechify as the TTS leg inside
voice agents.

They work with any Agent Skills–compatible coding assistant (Claude Code,
Cursor, Codex, and ~75 others) and follow one rule above all: **never invent
Speechify API facts — verify them against the live source.**

## Install

### Claude Code plugin (recommended)

```
/plugin marketplace add Speechify-AI/skills
/plugin install speechify@speechify-skills
```

This installs all the skills **and** auto-registers the hosted `ask-speechify`
MCP (`https://mcp.speechify.ai/mcp`, public, no key) so skills can verify facts
against the live source — no separate setup step. Skills are then invokable as
`/speechify:<skill-name>`.

### Any agent, via the `skills` CLI

Vercel's open [`skills`](https://www.npmjs.com/package/skills) CLI installs the
raw skill folders into Claude Code, Cursor, Codex, and ~75 others:

```bash
npx skills add Speechify-AI/skills                              # all skills
npx skills add Speechify-AI/skills --skill speechify-text-to-speech   # just one
npx skills add Speechify-AI/skills -g                           # install globally
npx skills add Speechify-AI/skills -a claude-code -a cursor     # pick agents
```

Installing this way doesn't bundle the MCP — run the `speechify-setup-mcp` skill
(or `speechify mcp install`) to connect it. Every skill offers to do this if the
MCP isn't present.

## What's here

| Skill | Use it to… |
| --- | --- |
| `speechify` | Entry point — routes to the right skill and carries the core doctrine. |
| `speechify-setup-api-key` | Get, validate, and configure `SPEECHIFY_API_KEY`. |
| `speechify-text-to-speech` | Synthesise speech from text or SSML (`/v1/audio/speech`). |
| `speechify-streaming` | Low-latency streaming (`/v1/audio/stream and /stream/with-timestamps`). |
| `speechify-speech-marks` | Word/char timestamps → captions, subtitles, highlighting. |
| `speechify-voices` | List, filter, and choose voices. |
| `speechify-voice-cloning` | Clone a voice from a sample and synthesise with it. |
| `speechify-ssml` | SSML structure and emotion/prosody control. |
| `speechify-multilingual` | Multilingual synthesis; language/locale/accent. |
| `speechify-audio-formats` | Pick the right output format (web, telephony, PCM). |
| `speechify-models` | Model selection + deprecation/version discipline. |
| `speechify-voice-agents` | Speechify as the TTS leg in LiveKit / Pipecat / Vapi / Twilio / AI SDK. |
| `speechify-sdks` | Install/init the Python & TypeScript SDKs; native vs SDK. |
| `speechify-cli` | Install and use the `speechify` CLI (`@speechify/cli`) from a terminal/CI. |
| `speechify-setup-mcp` | Connect the hosted Speechify knowledge MCP (`mcp.speechify.ai`) to your editor. |
| `speechify-cookbook` | Match a task to one of the cookbook recipes. |
| `speechify-demos` | Match a use case to a clonable demo. |

## The one rule

Model IDs, voice IDs, endpoints, request fields, output formats, and version
dates **change**. Every skill defers those to a live source instead of baking
them in:

- **`ask-speechify` MCP** — grounded answers over Speechify's repos, SDKs, API
  reference, and docs. Hosted at `https://mcp.speechify.ai/mcp` (public, no key);
  connect it with the `speechify-setup-mcp` skill. Every skill offers to set it
  up if it isn't connected.
- **Docs** — `https://docs.speechify.ai` (append `.md` to any page for
  agent-readable Markdown).
- **SDKs** — `models.list()` / `voices.list()` return the current catalogue.

If a fact isn't confirmed by one of those, the skill tells the agent to go check
rather than guess.

## Design

The set is a superset distilled from the leading voice/AI-infra skill repos
(Cartesia, Deepgram, ElevenLabs, LiveKit, Pipecat, OpenAI/Codex, Vercel), mapped
onto Speechify's product surface. The full rationale — the competitor matrix,
the conventions adopted, and what was deliberately deferred — is in
[`docs/alignment.md`](docs/alignment.md).

## Contributing

Authoring conventions and a skill template are in
[`CONTRIBUTING.md`](CONTRIBUTING.md) and [`templates/SKILL.md`](templates/SKILL.md).

## License

MIT — see [`LICENSE`](LICENSE).
