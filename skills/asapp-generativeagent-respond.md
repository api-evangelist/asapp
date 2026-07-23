---
name: Run GenerativeAgent on a conversation
description: Create or update a conversation, add customer messages, trigger GenerativeAgent to analyze/respond, and stream/poll its state.
api: openapi/asapp-generativeagent-openapi.yml
operations: [createOrUpdateConversation, createMessage, postAnalyze, postStreams, getState]
---

# Run GenerativeAgent on a conversation

Use ASAPP GenerativeAgent to have an AI agent analyze and respond to a live customer conversation.

## Auth & environment
- Send every request with headers `asapp-api-id` and `asapp-api-secret` (per-environment key pair).
- Base URL: `https://api.sandbox.asapp.com` (test) or `https://api.asapp.com` (production).

## Steps
1. **Create/attach the conversation** — `createOrUpdateConversation` (Conversations API). Provide your `externalId`; the call is an idempotent upsert (a `409` on retry means it was already persisted — treat as success).
2. **Add the inbound message** — `createMessage` with the customer's text on the conversation.
3. **Trigger the agent** — `postAnalyze` (operation `postAnalyze`) to have GenerativeAgent analyze the conversation and produce a response.
4. **Stream events** — call `postStreams` to create a GenerativeAgent event-streaming URL (SSE), then read GenerativeAgent events (responses, handoff signals) from that stream.
5. **Poll state if needed** — `getState` returns the current GenerativeAgent state for the conversation.

## Rules
- Idempotency: reference the same `externalId` to avoid duplicate conversations; see `conventions/asapp-conventions.yml`.
- Errors arrive as `{ "error": { "requestId", "message", "code" } }`; quote `requestId` to support. See `errors/asapp-problem-types.yml`.
- Rate limit: 100 req/s spike arrest; back off `1s, 2s, 4s` on `429`.
