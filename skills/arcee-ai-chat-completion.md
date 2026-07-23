---
name: Run a chat completion with an Arcee model
description: Authenticate, discover an available model, and call the OpenAI-compatible chat completions endpoint on the Arcee Platform API.
api: openapi/arcee-ai-afm-openapi.json
operations:
  - get_models_v1_models_get
  - create_chat_completion_v1_chat_completions_post
  - get_user_credits_v1_usage_credits_get
---

# Run a chat completion with an Arcee model

The Arcee Platform inference API is **OpenAI-compatible**. Base URL: `https://api.arcee.ai/api/v1`.

## Auth
Create an API key in the Arcee Platform dashboard and send it on every request:
`Authorization: Bearer <API_KEY>`. See `authentication/arcee-ai-authentication.yml`.

## Steps

1. **Discover models** — `get_models_v1_models_get` (`GET /v1/models`). Pick a model id such as
   `trinity-mini`, `trinity-large-preview`, or `trinity-large-thinking`.
2. **(Optional) Check credits** — `get_user_credits_v1_usage_credits_get` (`GET /v1/usage/credits`)
   to confirm the key has balance; a call with no balance returns **402 Insufficient Balance**.
3. **Create a completion** — `create_chat_completion_v1_chat_completions_post`
   (`POST /v1/chat/completions`) with a body of `model` and `messages`. Optional params match OpenAI:
   `temperature`, `top_p`, `max_tokens`, `stream`, `tools`, `tool_choice`, `response_format`, `seed`.
   - Set `stream: true` to receive Server-Sent Events (`text/event-stream`).
   - Read the answer from `choices[0].message.content`; reasoning models also return
     `choices[0].message.reasoning`.

## Error handling
Errors return `{"detail": "<message>"}`. Handle `401` (bad key), `402` (no balance), `422`
(invalid parameters — inspect `detail[].loc`), and `429` (rate limited — back off). See
`errors/arcee-ai-problem-types.yml`. There is no idempotency key; do not assume retries are safe
for billing.
