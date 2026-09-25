---
name: speechify-setup-mcp
description: >
  Install and connect the hosted Speechify MCP server (speechify-docs) so the
  agent can answer Speechify technical questions and verify API/SDK facts live.
  Use when the user says "set up the Speechify MCP", "connect speechify-docs",
  "add the Speechify docs MCP", "I want live Speechify answers", or
  when any other Speechify skill needs the MCP and it isn't connected yet. Not
  for installing these skills (that's `npx skills`) or the Speechify CLI (see
  `speechify-cli`).
license: MIT
metadata:
  author: Speechify
  version: 0.1.0
---

# Set up the Speechify MCP (speechify-docs)

Speechify's docs ship a public **MCP server** that searches the Speechify docs
and API reference, the live source these skills tell you to verify facts
against. Connect it once and the agent can check model
IDs, voice fields, endpoints, and usage on demand instead of guessing.

- **Endpoint:** `https://docs.speechify.ai/_mcp/server` (Streamable HTTP)
- **Auth:** none. The endpoint is public and serves the public docs only.
- **Tools:** `searchDocs` (relevant doc passages, each with its source URL).

> **Already installed the Claude Code plugin?** If you added
> `speechify@speechify-skills` via `/plugin install`, this MCP is registered
> automatically — you can skip the rest of this skill.

## Option A — direct (recommended)

Claude Code:
```bash
claude mcp add --transport http speechify-docs https://docs.speechify.ai/_mcp/server
```

Any Streamable HTTP client (Cursor, VS Code, Claude Desktop, Windsurf) — add to
its MCP config:
```jsonc
{
  "mcpServers": {
    "speechify-docs": {
      "type": "http",
      "url": "https://docs.speechify.ai/_mcp/server"
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
stdio relay to the same hosted server.) The CLI writes the older
`https://mcp.speechify.ai/mcp` address, which permanently redirects (308) to
`https://docs.speechify.ai/_mcp/server`, so both work.

## Verify it works
Restart/reload the client, then confirm the `speechify-docs` server is connected
and the `searchDocs` tool is listed. A quick probe:
```bash
curl -s -X POST https://docs.speechify.ai/_mcp/server \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json, text/event-stream' \
  -d '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2025-06-18","capabilities":{},"clientInfo":{"name":"probe","version":"0"}}}'
# → serverInfo: {"name":"fern-docs-mcp-server",...}
```

## Common mistakes
- Adding it as a stdio/command server when the hosted endpoint is HTTP — use
  `--transport http` / `"type": "http"`.
- Expecting to supply an API key — the hosted MCP needs none.
