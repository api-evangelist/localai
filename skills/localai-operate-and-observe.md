---
name: localai-operate-and-observe
description: >-
  Diagnose a misbehaving LocalAI instance and account for its usage — traces, backend logs, system info,
  metrics, middleware decision logs and per-user token accounting. Use when a request failed, was slow,
  or when someone asks who consumed what.
api: LocalAI API
spec: openapi/localai-api-openapi.yml
operations:
- GET /system
- GET /metrics
- GET /api/traces
- GET /api/traces/summary
- GET /api/traces/{id}
- GET /api/backend-traces
- GET /api/backend-logs/{modelId}
- GET /backend/monitor
- GET /api/p2p
generated: '2026-08-27'
method: generated
source: >-
  openapi/localai-api-openapi.yml, https://localai.io/features/tracing,
  https://localai.io/features/middleware/, https://localai.io/features/authentication
---

# Operate and observe a LocalAI instance

Admin-only. Every operation here returns `403` for a non-admin user under the user auth system, with the
exception of `GET /metrics`, which the documentation lists as user-accessible.

## Diagnosing a failed request

1. **Start at the API trace, not the log.** `GET /api/traces` accepts `limit`, `offset` and `full` query
   parameters — the only paginated listing family on this API, alongside `GET /api/backend-traces` and
   `GET /api/agent/jobs`. Use `limit` to bound it. `GET /api/traces/summary` gives the aggregate;
   `GET /api/traces/{id}` gives one request in full.

2. **Then go down a layer.** `GET /api/backend-traces` and `GET /api/backend-traces/{id}` cover the
   backend side. `GET /api/backend-logs/{modelId}` returns the raw backend logs for one model, and
   `WebSocket /ws/backend-logs/{modelId}` streams them live — use the stream when reproducing, the
   snapshot when reading after the fact.

3. **Distinguish an API error from a runtime error.** An envelope with `error.code` and `error.message`
   is an API error (`errors/localai-problem-types.yml`). Messages like `could not load model`,
   `grpc service not ready`, `SIGILL` or a VRAM out-of-memory are runtime errors from the backend —
   different reference, https://localai.io/reference/runtime-errors.

4. **Check capacity.** `GET /system` for instance information, `GET /backend/monitor` for backend state,
   `GET /metrics` for Prometheus exposition. `POST /backend/shutdown` and `POST /backend/load` control a
   backend process directly — both are destructive to in-flight requests; confirm with the human first.

## Middleware forensics

If a request was rewritten or blocked rather than failing:

- `GET /api/router/decisions` — why the intelligent router picked the downstream model it picked.
  Filterable by `correlation_id`, `user_id`, `router_model`, `limit`. In-process 5,000-entry ring buffer,
  not persisted; pair it with usage records for anything long-horizon.
- `GET /api/pii/events` — PII redactions, blocks and admission denials. Filterable by `correlation_id`,
  `user_id`, `pattern_id` (e.g. `ner:EMAIL`), `kind`, `origin`. A `400 pii_blocked` came from here.
- `GET /api/middleware/status` — one round-trip aggregate of per-model PII state, detectors, router
  status, MITM status and admission status.

None of these three paths appears in the published Swagger document. They are documented at
https://localai.io/features/middleware/ — treat their exact shapes as unverified against the contract.

## Usage accounting

Only when authentication is enabled. `GET /api/auth/usage` returns the caller's own usage;
`GET /api/auth/admin/usage` returns every user's. Both take `period` = `day` | `week` | `month` | `all`;
the admin variant also takes `user_id`. The `/sources` siblings break the same data down by API key and
by source class (apikey / web / legacy). The `by_key` list is server-capped at 200 entries and sets
`"truncated": true` when more would qualify — check that flag before reporting a total as complete.

## Rules

- Trace and log clears (`POST /api/traces/clear`, `POST /api/backend-traces/clear`,
  `POST /api/backend-logs/{modelId}/clear`) are permanent and have no undo. Never clear to "tidy up".
- Report `truncated: true` when you see it. A capped list reported as a total is a wrong answer.
- If every error body is empty, the instance runs `LOCALAI_OPAQUE_ERRORS=true` and you will get no
  diagnostic detail over the API at all — say so rather than guessing at causes.
