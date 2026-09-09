---
name: speechify-voice-agents
description: >
  Use Speechify as the text-to-speech leg inside a realtime voice agent or LLM
  app — LiveKit, Pipecat, Vapi, Twilio, the Vercel AI SDK, or Mastra. Use when
  the user wants to "build a voice agent", "give my agent a voice", "add
  Speechify TTS to LiveKit/Pipecat/Vapi/Twilio", "stream agent responses as
  speech", or "use Speechify with the AI SDK". For plain file synthesis see
  `speechify-text-to-speech`.
license: MIT
metadata:
  author: Speechify
  version: 0.1.0
  requires:
    env: [SPEECHIFY_API_KEY]
---

# Speechify in voice agents

> **Have the Speechify MCP connected?** If the `ask-speechify` MCP isn't available, offer to set it up —
> it answers Speechify technical questions and lets you verify the facts here
> against the live source. See [`speechify-setup-mcp`](../speechify-setup-mcp/SKILL.md).

In a voice agent, Speechify is the **TTS leg**: the LLM produces text, Speechify
turns it into audio, and a transport plays it to the caller. STT and turn-taking
come from the framework, not Speechify.

## Architecture that works
1. **Stream, don't batch.** Use [`speechify-streaming`](../speechify-streaming/SKILL.md)
   so speech starts as tokens arrive — first-byte latency is what callers feel.
2. **Match the audio format to the transport.** Telephony wants `ulaw_8000`; most
   agent frameworks want PCM at a specific sample rate. See
   [`speechify-audio-formats`](../speechify-audio-formats/SKILL.md).
3. **Pick a low-latency model** ([`speechify-models`](../speechify-models/SKILL.md)).
4. **Handle barge-in.** When the user interrupts, stop the current synthesis/
   playback promptly.

## Framework starting points (clone the demo)
Speechify ships runnable integration demos — start from the one that matches and
adapt. Find them via [`speechify-demos`](../speechify-demos/SKILL.md):

- **LiveKit** — `livekit-voice-agent-node`, `livekit-voice-agent-python`, `livekit-agent-speechify-python`
- **Pipecat** — `pipecat-agent-speechify`
- **Vapi** — `vapi-custom-voice`
- **Twilio** — `twilio-call-streaming`, `ivr-ssml` (telephony; use `ulaw_8000`)
- **Vercel AI SDK** — `ai-sdk-speechify-speech`, `vercel-ai-sdk`
- **Mastra** — `mastra-agent-speechify`
- Broader showcase — `voice-agent-showcase`

## Verify first
Integration adapters and their config (voice IDs, model names, format flags)
change with SDK/framework versions. Confirm the current wiring from the demo's
README and `ask-speechify` rather than from memory.

## Common mistakes
- Batching full TTS before playback → high perceived latency. Stream.
- Sample-rate mismatch with the transport → distorted audio.
- No barge-in handling → the agent talks over the user.
