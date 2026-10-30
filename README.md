# Lacuna

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

Lacuna (lacuna.fm), operated by JOYLINK LTD, is an AI music creation platform that turns lyrics or a plain-text style description into complete songs with vocals, alongside lyrics writing, word-level timed lyrics (LRC/SRT/VTT), lyric video export, AI mastering, mashups, stem separation, album-cover art, MIDI tooling and sheet-music conversion, plus long-form AI radio for focus, sleep and ambience. Developers get a documented REST music-generation API (OpenAPI 3.1 published at /api/openapi.json), an official TypeScript SDK, a `lacuna` CLI, and an unusually complete agent surface: a hosted Streamable-HTTP MCP server at /mcp with an anonymous tools/list, an A2A JSON-RPC agent endpoint at /a2a with a published agent card, an RFC 9727 api-catalog, RFC 8414 / RFC 9728 / RFC 7009 OAuth discovery and revocation, an agent-oriented auth.md, an llms.txt, and packaged Agent Skills. The REST contract carries three operations: submit a generation, poll it, and — added since August — a free GET /v1/me that returns the calling credential's plan, credit balance, scopes, key expiry and effective rate limits, which is the only precondition check available on an API that offers no idempotency key, no dry run and no way to cancel or refund a generation once submitted. API access requires a Pro plan or above and is billed in credits per generation.

**Areas:** AI Music · Music Generation · AI Song Generator · AI Lyrics Generator · Audio · MIDI · Songwriting · Generative AI · MCP Server · Agent Skills · A2A · Developer Tools

## Lacuna on APIs.io

Everything profiled for Lacuna is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/lacuna/](https://apis.io/providers/lacuna/)** — or go straight to one of the APIs or properties below.

### APIs (5)

- [Lacuna MCP Server](https://apis.io/apis/lacuna/lacuna-mcp-server/) — Hosted Model Context Protocol server (Streamable HTTP) at https://www.lacuna.fm/mcp, protocol 2025-06-18. initialize and tools/list answer anonymously and…
- [Lacuna A2A Agent](https://apis.io/apis/lacuna/lacuna-a2a-agent/) — Agent-to-Agent JSON-RPC endpoint at https://www.lacuna.fm/a2a advertising the "Lacuna Music" agent card at /.well-known/agent-card.json (protocolVersion…
- [Lacuna Music API Events](https://apis.io/apis/lacuna/lacuna-lacuna-music-api-api/) — The outbound event surface of the Lacuna Music API. Lacuna publishes no AsyncAPI document; it describes its events natively in the OpenAPI 3.1 `webhooks`…
- [Lacuna Music API](https://apis.io/apis/lacuna/lacuna-music-api/) — Music generation endpoints.
- [Lacuna Account API](https://apis.io/apis/lacuna/lacuna-account-api/) — Account, plan and credential introspection.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### Arazzo workflows (1)

Multi-step workflows that chain operations into a business task.

- [Generate a Lacuna track and collect the audio](https://apis.io/arazzos/lacuna/lacuna-generate-and-collect/)

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Lacuna Webhooks](https://apis.io/asyncapis/lacuna/lacuna-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [Lacuna Music](https://apis.io/servers/lacuna/lacuna/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Lacuna Agentic Access](https://apis.io/agentic-access/lacuna/lacuna-agentic-access/)

#### Collections (2)

Postman and Open Collection request collections.

- [Lacuna Music Lacuna Music API API](https://apis.io/collections/lacuna/open-lacuna-lacuna-music-api-api/)
- [Lacuna Music API](https://apis.io/collections/lacuna/open-lacuna-music-api/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Lacuna Music Overlay](https://apis.io/overlays/lacuna/lacuna-music-overlay/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Lacuna Authentication](https://apis.io/security/lacuna/lacuna-authentication/)
- [Lacuna Domain Security](https://apis.io/security/lacuna/lacuna-domain-security/)
- [Lacuna Vulnerability Disclosure](https://apis.io/security/lacuna/lacuna-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Lacuna Scopes](https://apis.io/scopes/lacuna/lacuna-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Lacuna Plans](https://apis.io/plans/lacuna/lacuna-plans/)

#### Rate limits (1)

Published request limits.

- [Lacuna Rate Limits](https://apis.io/rate-limits/lacuna/lacuna-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.lacuna.fm/)
- [Documentation](https://www.lacuna.fm/docs)
- [Getting started](https://www.lacuna.fm/docs)
- [API reference](https://www.lacuna.fm/docs/api)
- [Pricing](https://www.lacuna.fm/pricing)
- [Sign up](https://www.lacuna.fm/signin)
- [Support](https://www.lacuna.fm/contact)
- [Changelog](https://www.lacuna.fm/changelog)
- [Blog](https://www.lacuna.fm/blog)
- [Security](https://www.lacuna.fm/.well-known/security.txt)
- [Terms of service](https://www.lacuna.fm/terms)
- [Privacy policy](https://www.lacuna.fm/privacy)
- [GitHub](https://github.com/JOYLINK-LTD)

## API Evangelist rating

**Kin Score 65.8** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 75.0 |
| Contract quality | 59.0 |
| Contract governance | 18.2 |
| Operational transparency | 63.2 |
| Developer ergonomics | 78.6 |
| Access clarity | 76.3 |

**Agent readiness 58.1** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | verified |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | partial |
| Rate Limit Signal | verified |
| Event Surface Described | derived (partial credit) |
| Agent Skills | yes |
| Well Known Catalog | yes |
| Consent Identity | — |
| Agent Card | flavored (partial credit) |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | verified |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | distinct |

**Regulatory alignment 37.3** — AI & Biometrics

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/lacuna/](https://apis.io/providers/lacuna/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Lacuna: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/lacuna/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
