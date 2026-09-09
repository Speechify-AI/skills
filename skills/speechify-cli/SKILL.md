---
name: speechify-cli
description: >
  Install and use the Speechify CLI (`@speechify/cli`, the `speechify` binary) to
  drive the API from the terminal — synthesize speech, list voices, and hit any
  endpoint. Use when the user wants to "use Speechify from the command line",
  "install the Speechify CLI", "run speechify say", "log in to the CLI", or script
  TTS in a shell/CI. For app code use the SDKs (`speechify-sdks`); for connecting
  the knowledge MCP see `speechify-setup-mcp`.
license: MIT
metadata:
  author: Speechify
  version: 0.1.0
  requires:
    env: [SPEECHIFY_API_KEY]
---

# Speechify CLI

> **Have the Speechify MCP connected?** If the `ask-speechify` MCP isn't available, offer to set it up —
> it answers Speechify technical questions and lets you verify the facts here
> against the live source. See [`speechify-setup-mcp`](../speechify-setup-mcp/SKILL.md).

`@speechify/cli` (binary `speechify`) is the terminal companion to the API:
authenticate once, then synthesize and call endpoints from the shell or CI.

## Verify first
The CLI is early (v0.0.2) — confirm the current install command, flags, and
command surface from the `Speechify-AI/cli` README or `speechify --help` before
relying on specifics.

## Install & authenticate
```bash
npm install -g @speechify/cli      # confirm the published install command (repo README)
speechify login --api-key sk_…     # validates, then stores the key in your OS keychain
speechify whoami --check           # show + live-verify how you're authenticated
```
Credential precedence per run: `--api-key` → `$SPEECHIFY_API_KEY` → stored key.
Get a key at `https://platform.speechify.ai/api-keys` (see
[`speechify-setup-api-key`](../speechify-setup-api-key/SKILL.md)).

## Core commands
```bash
# Synthesize
speechify say "Text to speak" --voice george --format mp3 --out speech.mp3 --play
echo "from a pipe" | speechify say -           # read text from stdin ("-" blocks until EOF)

# Voices
speechify voices list --locale en --gender female --search warm

# Raw authenticated passthrough (gh-api style) for anything the typed commands don't cover
speechify api /v1/voices
speechify api /v1/audio/speech -f input="hello" -f voice_id=george   # -f implies POST
```
`say --format` accepts `wav | mp3 | ogg | aac | pcm` (default `mp3`). Add `--json`
to any command for machine-readable stdout.

## Connect the knowledge MCP
```bash
speechify mcp install --client claude-code   # wire the hosted MCP into your editor
```
See [`speechify-setup-mcp`](../speechify-setup-mcp/SKILL.md).

## Agent & scripting behaviour
- Inside an AI agent (Claude Code, Cursor, Codex) the CLI auto-switches to
  **agent mode**: stdout is JSON with `context` + next-step `hints`. `--json`
  forces a bare payload; `SPEECHIFY_OUTPUT=human|json|agent` overrides detection.
- Missing required input in a non-interactive run → a structured **needs-input**
  spec on stdout, exit code `2` (read `inputs`, supply as flags, re-invoke).
- Exit codes follow sysexits: `78` config/auth-missing, `77` auth, `75`
  rate-limited, `65` bad input, `69` upstream/timeout.

## Common mistakes
- Piping text without `-` in an agent/CI context — pass `-` so it waits for stdin.
- Assuming a command exists — the surface is young; check `speechify --help`.
