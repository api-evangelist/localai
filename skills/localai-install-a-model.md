---
name: localai-install-a-model
description: >-
  Find, size, install and verify a model on a LocalAI instance without stranding the host — rehearse the
  VRAM fit first, start the install, poll the job, and know exactly what is reversible afterwards.
api: LocalAI API
spec: openapi/localai-api-openapi.yml
operations:
- GET /models/available
- GET /models/galleries
- POST /api/models/vram-estimate
- POST /models/apply
- GET /models/jobs/{uuid}
- GET /v1/models
- POST /models/delete/{name}
- PUT /api/models/{name}/{action}
generated: '2026-08-27'
method: generated
source: >-
  openapi/localai-api-openapi.yml, https://localai.io/features/localai-assistant,
  https://localai.io/features/authentication
---

# Install a model on LocalAI

This is an ADMIN flow. Every operation below returns `403` for an authenticated non-admin user when the
user auth system is enabled. Installing a model downloads gigabytes onto the operator's disk and can
exhaust their VRAM — rehearse before you write.

## Steps

1. **List what the galleries offer.** `GET /models/available` returns the installable model catalogue;
   `GET /models/galleries` returns the configured gallery sources. Neither takes a limit or offset —
   expect a large unbounded response.

2. **Rehearse the fit. Do this before every install.** `POST /api/models/vram-estimate` estimates VRAM
   usage for a model. This is the one genuine dry-run surface in the model flow; there is no preview mode
   on the install itself. If the estimate does not fit the host, stop here and report it — do not install
   and then delete.

3. **Confirm with the human.** Installation is a mutating action with no preview/apply split. The
   provider's own MCP assistant guards `install_model` with a prompt rule requiring user confirmation
   before the call; hold yourself to the same rule.

4. **Start the install.** `POST /models/apply` with the required request body. It returns a job, not a
   finished model.

5. **Poll the job.** `GET /models/jobs/{uuid}` returns the job status. Poll until it reports completion;
   do not re-POST `/models/apply` because a poll looked slow — the operation is not idempotent and a
   second call is a second install.

6. **Verify.** `GET /v1/models` should now list the model. `GET /v1/models/capabilities` confirms which
   modalities it actually serves.

7. **If a job is still running and should not be**, `POST /api/operations/{id}/cancel` cancels it. This
   endpoint is documented in the authentication reference but is absent from the published Swagger
   document — treat its shape as unverified.

## Reversibility — read before you write

| Action | Reversal | Window |
|---|---|---|
| `POST /models/apply` | `POST /models/delete/{name}` | None stated; available while installed |
| `PUT /api/models/{name}/{action}` (enable/disable) | Call again with the opposite action | None stated |
| `PUT /api/models/toggle-pinned/{name}/{action}` | Call again with the opposite action | None stated |
| `POST /backends/upgrade/{name}` | **NONE** | No rollback endpoint exists |
| `PATCH /api/models/config-json/{name}` | **NONE** | No version history; read the config first |

Two of these have no documented inverse. Before a backend upgrade or a config patch, capture the current
state yourself — that read is the only rollback available. Full detail in
`conventions/localai-conventions.yml`.

## Rules

- Never install a model the human did not name or approve.
- Never call `POST /backends/upgrade/{name}` speculatively. It is irreversible by any documented API.
- A `400` here usually means the model name is not in a configured gallery, not that the body was
  malformed.
