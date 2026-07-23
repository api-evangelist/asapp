---
name: Summarize a conversation with AI Summary
description: Retrieve a free-text summary, structured data, and intent for a conversation, and submit feedback.
api: openapi/asapp-autosummary-openapi.yml
operations: [getFreeTextSummary, retrieveFreeTextSummary, retrieveStructuredData, createIntent, createFeedbackEvent]
---

# Summarize a conversation with AI Summary

Use ASAPP AutoSummary (AI Summary) to extract a human-readable summary, structured data, and intent from a conversation ASAPP already knows.

## Auth & environment
- Headers `asapp-api-id` + `asapp-api-secret`; base `https://api.sandbox.asapp.com` or `https://api.asapp.com`.

## Steps
1. **Generate/fetch the free-text summary** — `getFreeTextSummary` (or `retrieveFreeTextSummary`) for a concise conversation summary.
2. **Pull structured data** — `retrieveStructuredData` for the entities/fields ASAPP extracted (configured via Structured Data fields).
3. **Get the intent** — `createIntent` returns the primary intent (code + human-readable name), or `NO_INTENT`. Requires intent support enabled on the account.
4. **Send feedback** — `createFeedbackEvent` to submit a corrected/updated summary tied to the summary `id`, improving future results.

## Rules
- Reference conversations by ASAPP `conversationId` (or `externalId`).
- Some endpoints require the feature enabled for the account — a `403` means request the entitlement from ASAPP.
- Error envelope + retry rules: `conventions/asapp-conventions.yml`, `errors/asapp-problem-types.yml`.
