---
name: localai-run-an-inference-request
description: >-
  Call a LocalAI instance for chat, embeddings, transcription or speech, picking the right compatibility
  surface and the right error parser for each. Use after orienting against the instance.
api: LocalAI API
spec: openapi/localai-api-openapi.yml
operations:
- POST /v1/chat/completions
- POST /v1/messages
- POST /v1/responses
- POST /v1/embeddings
- POST /v1/audio/transcriptions
- POST /v1/audio/speech
- POST /v1/rerank
- GET /v1/models/capabilities
generated: '2026-08-27'
method: generated
source: >-
  openapi/localai-api-openapi.yml, https://localai.io/reference/api-errors,
  https://localai.io/features/mcp
---

# Run an inference request against LocalAI

LocalAI is a compatibility surface. The single most useful fact is that you do not need a LocalAI client:
point an existing OpenAI, Anthropic or ElevenLabs client at the instance base URL and it works unchanged.

## Pick the surface

| You want | Call | Error envelope |
|---|---|---|
| OpenAI-style chat | `POST /v1/chat/completions` | OpenAI |
| Anthropic-style messages | `POST /v1/messages` | Anthropic |
| OpenAI Responses | `POST /v1/responses` | Open Responses |
| Embeddings | `POST /v1/embeddings` | OpenAI |
| Transcription | `POST /v1/audio/transcriptions` | OpenAI |
| Speech | `POST /v1/audio/speech` or `POST /tts` | OpenAI |
| ElevenLabs-style speech | `POST /v1/text-to-speech/{voice-id}` | OpenAI |
| Reranking | `POST /v1/rerank` | OpenAI |
| Moderation | `POST /v1/moderations` | OpenAI |

The error envelope column is load-bearing: parse errors by the endpoint you called, not by content type.
All three shapes are documented at https://localai.io/reference/api-errors and captured in
`errors/localai-problem-types.yml`.

## Steps

1. **Confirm the model can do the job.** `GET /v1/models/capabilities` returns modalities and
   capabilities per model. A chat request against an embeddings-only model fails at the backend with a
   `500`, not a helpful `400`.

2. **Send the request.** `POST /v1/chat/completions` takes a single required body parameter. Set
   `"stream": true` for SSE token streaming on `/v1/chat/completions`, `/v1/completions`, `/v1/messages`
   and `/v1/responses`.

3. **Transcription is multipart, not JSON.** `POST /v1/audio/transcriptions` takes form-data fields:
   `model` (required), `file` (required), plus optional `temperature`, `timestamp_granularities` and
   `stream`. Omitting `file` returns `400 Bad Request`.

4. **To let the model use tools, use the MCP surface.** `POST /v1/mcp/chat/completions` executes MCP
   tools server-side and feeds results back to the model automatically — you do not receive tool calls to
   execute yourself. Select which configured servers are active for the request with
   `metadata.mcp_servers` (comma-separated names matching the model's MCP config). The loop is capped by
   `agent.max_iterations`, default 10. On `/v1/chat/completions` and `/v1/messages`, no MCP tools are
   injected unless `metadata.mcp_servers` is present.

## Error handling

- `400` — malformed body, missing required field, or a model name not in the instance configuration.
  The message is often just `Bad Request`; check the model name first.
- `401` — no valid credential. `403` — authenticated but not an admin (inference endpoints are open to
  the `user` role, so a `403` here means you called a management endpoint by mistake).
- `500` — backend inference failure. Read `GET /api/backend-logs/{modelId}` (admin) and
  https://localai.io/reference/runtime-errors before retrying.
- `503` — model-load cooldown, or the assistant is disabled and you sent
  `metadata.localai_assistant=true`.

## Rules

- Never retry a mutating call blindly. There is no idempotency key on this API.
- There is no rate limit and no `429`. If you are being throttled, it is the operator's hardware, not a
  policy — back off on latency, not on a header, because no `RateLimit-*` or `Retry-After` header exists.
