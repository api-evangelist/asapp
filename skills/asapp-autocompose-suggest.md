---
name: Get agent reply suggestions with AutoCompose
description: Generate next-message suggestions for a human agent, plus spelling correction and profanity checks before send.
api: openapi/asapp-autocompose-openapi.yml
operations: [getSuggestions, getSpellingCorrection, getEvaluation]
---

# Get agent reply suggestions with AutoCompose

Use ASAPP AutoCompose to augment a human agent composing a reply.

## Auth & environment
- Headers `asapp-api-id` + `asapp-api-secret`; base `https://api.sandbox.asapp.com` or `https://api.asapp.com`.

## Steps
1. **Suggest the next message** — `getSuggestions` returns suggested next agent messages for the conversation.
2. **Spell-check as they type** — `getSpellingCorrection` corrects the current word once fully typed (call after space characters).
3. **Guard the send** — `getEvaluation` checks text for profanity/obscenity before the message is sent.
4. (Optional) record usage with `createMessageSentEvent` / `createMessageAnalyticEvent` so ASAPP improves future suggestions.

## Rules
- Call `getSettings` first if you need to disable features in high-latency scenarios.
- Error envelope + retry rules: `conventions/asapp-conventions.yml`, `errors/asapp-problem-types.yml`.
