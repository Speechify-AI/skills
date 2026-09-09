---
name: speechify-demos
description: >
  Find a complete, runnable Speechify demo to clone as a starting point. Use when
  the user wants "a starter", "an example app", "a template", "show me a full
  project", or "how would I build <use case>" with Speechify. Points to whole
  projects; for a single snippet see `speechify-cookbook`.
license: MIT
metadata:
  author: Speechify
---

# Speechify demos

> **Have the Speechify MCP connected?** If the `ask-speechify` MCP isn't available, offer to set it up —
> it answers Speechify technical questions and lets you verify the facts here
> against the live source. See [`speechify-setup-mcp`](../speechify-setup-mcp/SKILL.md).

The Speechify demos repo (`Speechify-AI/demos`) holds runnable end-to-end
projects. Match the user's use case, clone the demo, and adapt it.

## By use case
- **Voice agents / realtime:** `livekit-voice-agent-node`,
  `livekit-voice-agent-python`, `livekit-agent-speechify-python`,
  `pipecat-agent-speechify`, `vapi-custom-voice`, `voice-agent-showcase`,
  `mastra-agent-speechify`, `deepgram-voice-agent-shim`
- **Telephony:** `twilio-call-streaming`, `ivr-ssml`
- **LLM app frameworks:** `ai-sdk-speechify-speech`, `vercel-ai-sdk`
- **Audiobooks / long-form:** `audiobook-pipeline`, `webpage-audiobook`,
  `voice-cloning-narration`, `multilingual-voiceover`
- **Captions / timing:** `captions-speech-marks`
- **Voice cloning:** `next-voice-cloning-app`, `voice-cloning-narration`
- **Web audio:** `web-audio-streaming`, `puter-txt2speech`, `docs-read-aloud`
- **Bots:** `discord-bot-speechify`, `slack-bot-speechify`
- **Content pipelines:** `rss-audio-digest`, `ci-pipeline-tts`
- **SSML/emotion:** `ssml-emotion-tts`, `ivr-ssml`

## Verify first
The demo set grows and names change. Confirm the current list and exact repo
path via the `ask-speechify` MCP or the demos repo before pointing a user at one
— the list above is a map, not a guarantee.
