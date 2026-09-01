---
name: localai-orient-against-an-instance
description: >-
  Work out what a LocalAI instance is, what it can do, and what you are allowed to call — before
  attempting any inference. Use this first against any LocalAI base URL you have not called before.
api: LocalAI API
spec: openapi/localai-api-openapi.yml
operations:
- GET /.well-known/localai.json
- GET /api/instructions
- GET /api/instructions/{name}
- GET /v1/models
- GET /v1/models/capabilities
- GET /system
generated: '2026-08-27'
method: generated
source: >-
  openapi/localai-api-openapi.yml, https://localai.io/docs/features/api-discovery/,
  https://localai.io/features/authentication
---

# Orient against a LocalAI instance

LocalAI is self-hosted, so every instance is different: a different model set, a different backend set,
different features compiled in, and a different authentication posture. Never assume. The contract
declares no `operationId` on any operation, so operations are referenced here as `METHOD path`.

## Before you start

- You need a base URL for an instance somebody runs. There is no vendor endpoint. The documented default
  address is `http://localhost:8080`.
- Since v4.9.0 authentication is deny-by-default. If the instance has either auth mode configured, every
  route below except the first three requires credentials.
- Credentials go in `Authorization: Bearer <key>`, or `x-api-key`, or `xi-api-key`, or a `token` cookie.

## Steps

1. **Read the instance manifest, anonymously.**
   `GET /.well-known/localai.json` — returns the instance version, the endpoint URLs (flat and
   categorized) and runtime capability flags for `config_metadata`, `config_patch`, `vram_estimate`,
   `mcp`, `agents` and `p2p`. This is the only call that tells you which optional subsystems are live.
   It stays anonymous even when auth is configured.

2. **Read the instruction index.**
   `GET /api/instructions` — a compact list of instruction areas (chat-inference, audio, images,
   model-management, config-management, monitoring, mcp, agents, video) with a description and a URL
   each. Also anonymous.

3. **Pull the area you care about.**
   `GET /api/instructions/{name}` returns a markdown guide by default, or a raw OpenAPI fragment with
   `?format=json`. Prefer `?format=json` — it gives you the operation shapes for that area directly.

4. **Authenticate before going further.** Everything below needs credentials. If you get `401` with
   `{"error":{"code":401,"message":"An authentication key is required","type":"invalid_request_error"}}`
   and a `WWW-Authenticate: Bearer` header, you have no valid credential. If you get `403`, you are
   authenticated but not an admin.

5. **List what is installed.**
   `GET /v1/models` — the OpenAI-compatible model list. Unbounded; there is no pagination on this
   operation.

6. **Learn what each model can actually do.**
   `GET /v1/models/capabilities` — the model list enriched with capabilities (chat, vision, tools) and
   input/output modalities (text, image, audio, video). Use this, not the model name, to decide whether a
   model can serve the request you are about to make.

7. **Check the instance itself (admin only).**
   `GET /system` returns instance information. `GET /metrics` exposes Prometheus metrics.

## Rules

- Do not guess a model name. A name that is not in `GET /v1/models` returns `400 Bad Request`, not `404`.
- If the instance runs with `LOCALAI_OPAQUE_ERRORS=true`, every error comes back as a bare status code
  with an empty body. Treat any empty-bodied 4xx/5xx as opaque-errors mode and fall back to the status.
- Retrying is not free: no operation on this API is idempotent and there is no idempotency key. See
  `conventions/localai-conventions.yml`.
