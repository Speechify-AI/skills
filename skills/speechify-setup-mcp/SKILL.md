---
name: speechify-setup-mcp
description: >
  Install and connect the hosted Speechify MCP server (ask-speechify) so the
  agent can answer Speechify technical questions and verify API/SDK facts live.
  Use when the user says "set up the Speechify MCP", "connect ask-speechify",
  "add the Speechify docs/knowledge MCP", "I want live Speechify answers", or
  when any other Speechify skill needs the MCP and it isn't connected yet. Not
  for installing these skills (that's `npx skills`) or the Speechify CLI (see
  `speechify-cli`).
license: MIT
metadata:
  author: Speechify
  version: 0.1.0
---

# Set up the Speechify MCP (ask-speechify)

Speechify hosts a public **MCP server** that answers questions grounded on
Speechify's repos, SDKs, API reference, and docs — the live source these skills
tell you to verify facts against. Connect it once and the agent can check model
IDs, voice fields, endpoints, and usage on demand instead of guessing.

- **Endpoint:** `https://mcp.speechify.ai/mcp` (Streamable HTTP)
- **Auth:** none — the endpoint is public and pinned to public knowledge only.
- **Tools:** `ask` (grounded natural-language answer + citations) and `search`
  (ranked source passages, no synthesis).

## Option A — direct (recommended)

Claude Code:
```bash
claude mcp add --transport http speechify-ask https://mcp.speechify.ai/mcp
```

Any Streamable HTTP client (Cursor, VS Code, Claude Desktop, Windsurf) — add to
its MCP config:
```jsonc
{
  "mcpServers": {
    "speechify-ask": {
      "type": "http",
      "url": "https://mcp.speechify.ai/mcp"
    }
  }
}
```

## Option B — via the Speechify CLI

If the user has (or wants) the CLI, it writes the client config for you:
```bash
speechify mcp install --client claude-code   # or cursor | claude-desktop | windsurf | vscode
speechify mcp install --all                  # every detected client
speechify mcp install --print                # show the config, write nothing
```
See [`speechify-cli`](../speechify-cli/SKILL.md). (`speechify mcp` also runs a
stdio relay to the same hosted server.)

## Verify it works
Restart/reload the client, then confirm the `speechify-ask` server is connected
and the `ask` / `search` tools are listed. A quick probe:
```bash
curl -s -X POST https://mcp.speechify.ai/mcp \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json, text/event-stream' \
  -d '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2025-06-18","capabilities":{},"clientInfo":{"name":"probe","version":"0"}}}'
# → serverInfo: {"name":"speechify-ask",...}
```

## Common mistakes
- Adding it as a stdio/command server when the hosted endpoint is HTTP — use
  `--transport http` / `"type": "http"`.
- Expecting to supply an API key — the hosted MCP needs none.
