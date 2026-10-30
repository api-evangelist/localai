# LocalAI

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

LocalAI is the open-source, self-hosted AI inference engine that acts as a drop-in replacement for the OpenAI and Anthropic REST APIs. A single Go binary (or container) exposes 123 HTTP operations across chat completions, completions, embeddings, image and video generation, 3D generation, text-to-speech, speech-to-text, audio classification and diarization, object detection, depth estimation, reranking, moderation, tokenization, face and voice recognition, and the OpenAI Realtime API over WebSocket and WebRTC — all running locally on CPU or GPU with no external API calls and no data leaving the host. Beyond OpenAI parity, LocalAI ships its own management surface: a model and backend gallery, P2P and federated distributed inference, an agent/job runtime with a Skills system and Agent Hub, request middleware for NER-based PII redaction and policy-based intelligent model routing, tracing, per-user usage accounting, and a first-party stdio MCP server (`local-ai mcp-server`) that exposes the admin surface as agent tools. Authentication spans legacy shared API keys through a full user system with roles, sessions, GitHub OAuth and OIDC single sign-on. Distributed under the MIT licence by Ettore Di Giacinto (mudler) and community contributors.

**Areas:** Artificial Intelligence · Machine Learning · LLM · Inference · Self-Hosted · Open Source · Agents · MCP · Speech · Computer Vision · Embeddings · Edge Computing

## LocalAI on APIs.io

Everything profiled for LocalAI is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/localai/](https://apis.io/providers/localai/)** — or go straight to one of the APIs or properties below.

### APIs (26)

- [LocalAI Agent Jobs API](https://apis.io/apis/localai/localai-agent-jobs-api/) — The agent-jobs API from LocalAI — 7 operation(s) for agent-jobs.
- [LocalAI Audio API](https://apis.io/apis/localai/localai-audio-api/) — The audio API from LocalAI — 14 operation(s) for audio.
- [LocalAI Backends API](https://apis.io/apis/localai/localai-backends-api/) — The backends API from LocalAI — 11 operation(s) for backends.
- [LocalAI Branding API](https://apis.io/apis/localai/localai-branding-api/) — The branding API from LocalAI — 3 operation(s) for branding.
- [LocalAI Config API](https://apis.io/apis/localai/localai-config-api/) — The config API from LocalAI — 6 operation(s) for config.
- [LocalAI Depth API](https://apis.io/apis/localai/localai-depth-api/) — The depth API from LocalAI — 1 operation(s) for depth.
- [LocalAI Detection API](https://apis.io/apis/localai/localai-detection-api/) — The detection API from LocalAI — 1 operation(s) for detection.
- [LocalAI Embeddings API](https://apis.io/apis/localai/localai-embeddings-api/) — The embeddings API from LocalAI — 1 operation(s) for embeddings.
- [LocalAI Face Recognition API](https://apis.io/apis/localai/localai-face-recognition-api/) — The face-recognition API from LocalAI — 6 operation(s) for face-recognition.
- [LocalAI Images API](https://apis.io/apis/localai/localai-images-api/) — The images API from LocalAI — 3 operation(s) for images.
- [LocalAI Inference API](https://apis.io/apis/localai/localai-inference-api/) — The inference API from LocalAI — 7 operation(s) for inference.
- [LocalAI Instructions API](https://apis.io/apis/localai/localai-instructions-api/) — The instructions API from LocalAI — 2 operation(s) for instructions.
- [LocalAI MCP API](https://apis.io/apis/localai/localai-mcp-api/) — The mcp API from LocalAI — 1 operation(s) for mcp.
- [LocalAI Models API](https://apis.io/apis/localai/localai-models-api/) — The models API from LocalAI — 10 operation(s) for models.
- [LocalAI Moderation API](https://apis.io/apis/localai/localai-moderation-api/) — The moderation API from LocalAI — 1 operation(s) for moderation.
- [LocalAI Monitoring API](https://apis.io/apis/localai/localai-monitoring-api/) — The monitoring API from LocalAI — 16 operation(s) for monitoring.
- [LocalAI Nodes API](https://apis.io/apis/localai/localai-nodes-api/) — The Nodes API from LocalAI — 3 operation(s) for nodes.
- [LocalAI Pii API](https://apis.io/apis/localai/localai-pii-api/) — The pii API from LocalAI — 2 operation(s) for pii.
- [LocalAI Rerank API](https://apis.io/apis/localai/localai-rerank-api/) — The rerank API from LocalAI — 1 operation(s) for rerank.
- [LocalAI Router API](https://apis.io/apis/localai/localai-router-api/) — The router API from LocalAI — 3 operation(s) for router.
- [LocalAI Tokenize API](https://apis.io/apis/localai/localai-tokenize-api/) — The tokenize API from LocalAI — 4 operation(s) for tokenize.
- [LocalAI Video API](https://apis.io/apis/localai/localai-video-api/) — The video API from LocalAI — 1 operation(s) for video.
- [LocalAI Voice Profiles API](https://apis.io/apis/localai/localai-voice-profiles-api/) — The voice-profiles API from LocalAI — 3 operation(s) for voice-profiles.
- [LocalAI Voice Recognition API](https://apis.io/apis/localai/localai-voice-recognition-api/) — The voice-recognition API from LocalAI — 6 operation(s) for voice-recognition.
- [LocalAI P2p API](https://apis.io/apis/localai/localai-p2p-api/) — The p2p API from LocalAI — 2 operation(s) for p2p.
- [LocalAI 3 D API](https://apis.io/apis/localai/localai-3-d-api/) — The 3 D API from LocalAI — 2 operation(s) for 3 d.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [LocalAI Realtime API](https://apis.io/asyncapis/localai/localai-realtime-asyncapi/)

#### Event channels (2)

Individual events and webhooks the provider emits.

- [/v1/realtime](https://apis.io/channels/localai/v1-realtime/)
- [/ws/backend-logs/{modelId}](https://apis.io/channels/localai/ws-backend-logs-modelid/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [LocalAI Assistant MCP server](https://apis.io/servers/localai/localai-assistant-mcp-server/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Localai Api Overlay](https://apis.io/overlays/localai/localai-api-overlay/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Localai Authentication](https://apis.io/security/localai/localai-authentication/)
- [Localai Domain Security](https://apis.io/security/localai/localai-domain-security/)
- [Localai Vulnerability Disclosure](https://apis.io/security/localai/localai-vulnerability-disclosure/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Localai Plans Pricing](https://apis.io/plans/localai/localai-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Localai Rate Limits](https://apis.io/rate-limits/localai/localai-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://localai.io/)
- [Documentation](https://localai.io/docs/)
- [Getting started](https://localai.io/basics/getting_started/)
- [API reference](https://localai.io/docs/reference/)
- [Support](https://github.com/mudler/LocalAI/discussions)
- [Blog](https://localai.io/blog/)
- [Security](https://github.com/mudler/LocalAI/blob/master/SECURITY.md)
- [GitHub](https://github.com/mudler/LocalAI)

## API Evangelist rating

**Kin Score 38.5** — thin · rubric 0.24.0 · scored 2026-10-17

| Facet | Score |
|---|---|
| Discoverability | 71.7 |
| Contract quality | 45.0 |
| Contract governance | 4.5 |
| Operational transparency | 36.8 |
| Developer ergonomics | 73.2 |
| Access clarity | 0.0 |

**Agent readiness 30.9** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | documented (partial credit) |
| MCP Server | documented (partial credit) |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | derived (partial credit) |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |

**Regulatory alignment 20.9** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/localai/](https://apis.io/providers/localai/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for LocalAI: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/localai/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
