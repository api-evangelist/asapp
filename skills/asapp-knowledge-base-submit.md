---
name: Add knowledge base content via API
description: Submit a new/updated knowledge base article and retrieve the resulting submission and article.
api: openapi/asapp-knowledge-base-openapi.yml
operations: [createSubmission, getSubmission, getArticle]
---

# Add knowledge base content via API

Programmatically feed articles into the GenerativeAgent Knowledge Base.

## Auth & environment
- Headers `asapp-api-id` + `asapp-api-secret`. Knowledge Base base host: `https://api.test.asapp.com` (test) / `https://api.asapp.com` (production).

## Steps
1. **Create a submission** — `createSubmission` with the article `title` and `content`. ASAPP processes it into a final version.
2. **Check submission status** — `getSubmission` by its id to see whether it was approved.
3. **Fetch the article** — `getArticle` by id. If the submission was not approved the article is not created and a `404` is returned.

## Rules
- Poll `getSubmission` before assuming the article exists; handle the `404` on `getArticle` as "not yet approved."
- Error envelope + retry rules: `conventions/asapp-conventions.yml`, `errors/asapp-problem-types.yml`.
