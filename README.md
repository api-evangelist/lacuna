# Lacuna (lacuna)

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

Lacuna (lacuna.fm), operated by JOYLINK LTD, is an AI music creation platform that turns lyrics or a plain-text style description into complete songs with vocals, alongside lyrics writing, word-level timed lyrics (LRC/SRT/VTT), lyric video export, AI mastering, mashups, stem separation, album-cover art, MIDI tooling and sheet-music conversion, plus long-form AI radio for focus, sleep and ambience. Developers get a documented REST music-generation API (OpenAPI 3.1 published at /api/openapi.json), an official TypeScript SDK, a `lacuna` CLI, and an unusually complete agent surface: a hosted Streamable-HTTP MCP server at /mcp with an anonymous tools/list, an A2A JSON-RPC agent endpoint at /a2a with a published agent card, an RFC 9727 api-catalog, RFC 8414 / RFC 9728 OAuth discovery, an agent-oriented auth.md, an llms.txt, and a packaged Agent Skill. API access requires a Pro plan or above and is billed in credits per generation.

**APIs.json:** [https://lacuna.apievangelist.com/apis.yml](https://lacuna.apievangelist.com/apis.yml)

## Tags

- AI Music
- Music Generation
- AI Song Generator
- AI Lyrics Generator
- Audio
- MIDI
- Songwriting
- Generative AI
- MCP Server
- Agent Skill
- A2A
- Developer Tools

## Timestamps

- **Created:** 2026-08-02
- **Modified:** 2026-08-09

## APIs

### Lacuna MCP Server

Hosted Model Context Protocol server (Streamable HTTP) at https://www.lacuna.fm/mcp, protocol 2025-06-18. initialize and tools/list answer anonymously and return real input schemas; tools/call requires an OAuth 2.0 access token with music:generate or a lyr_live_ API key. Three tools — generate_music, get_generation, list_models. A stdio sibling ships on npm as lacuna-mcp for local clients.

- **Human URL:** [https://www.lacuna.fm/docs/mcp](https://www.lacuna.fm/docs/mcp)
- **Base URL:** `https://www.lacuna.fm/mcp`

#### Tags

- MCP Server
- Generative AI
- AI Music
- Developer Tools

#### Properties

- [M C P Server](mcp/lacuna-mcp.yml)
- [Tool Crosswalk](mcp/lacuna-tool-crosswalk.yml)
- [Documentation](https://www.lacuna.fm/docs/mcp)
- [Postman Collection](collections/lacuna-lacuna-music-api-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/lacuna-lacuna-music-api-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Postman Collection](collections/lacuna-music-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/lacuna-music-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Lacuna A2A Agent

Agent-to-Agent JSON-RPC endpoint at https://www.lacuna.fm/a2a advertising the "Lacuna Music" agent card at /.well-known/agent-card.json (protocolVersion 0.3.0). One skill, generate_music, which accepts a style description or a structured data part, runs asynchronously, and returns audio artifacts via tasks/get. Auth is required on every method; anonymous calls return 401 authentication_error.

- **Human URL:** [https://www.lacuna.fm/docs/api](https://www.lacuna.fm/docs/api)
- **Base URL:** `https://www.lacuna.fm/a2a`

#### Tags

- A2A
- Agent Skill
- Generative AI
- AI Music

#### Properties

- [Agent Card](a2a/lacuna-a2a.yml)
- [Documentation](https://www.lacuna.fm/docs/api)
- [Postman Collection](collections/lacuna-lacuna-music-api-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/lacuna-lacuna-music-api-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Postman Collection](collections/lacuna-music-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/lacuna-music-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Lacuna Lacuna Music API API

The Lacuna Music API API from Lacuna — 0 operation(s) for lacuna music api.

- **Human URL:** [https://www.lacuna.fm/docs/api](https://www.lacuna.fm/docs/api)
- **Base URL:** `https://www.lacuna.fm/api`

#### Tags

- Lacuna Music API

#### Properties

- [OpenAPI](openapi/lacuna-lacuna-music-api-api-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/lacuna-lacuna-music-api-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/lacuna-lacuna-music-api-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Documentation](https://www.lacuna.fm/docs/api)
- [API Reference](https://www.lacuna.fm/api/openapi.json)
- [Webhooks](asyncapi/lacuna-webhooks.yml)
- [Error Catalog](errors/lacuna-problem-types.yml)
- [Rate Limits](rate-limits/lacuna-rate-limits.yml)

### Lacuna Music API

Music generation endpoints.

- **Human URL:** [https://www.lacuna.fm/docs/api](https://www.lacuna.fm/docs/api)
- **Base URL:** `https://www.lacuna.fm/api`

#### Tags

- Music

#### Properties

- [OpenAPI](openapi/lacuna-music-api-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/lacuna-music-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/lacuna-music-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Documentation](https://www.lacuna.fm/docs/api)
- [API Reference](https://www.lacuna.fm/api/openapi.json)
- [Webhooks](asyncapi/lacuna-webhooks.yml)
- [Error Catalog](errors/lacuna-problem-types.yml)
- [Rate Limits](rate-limits/lacuna-rate-limits.yml)

## Common Properties

- [Agentic Access](agentic-access/lacuna-agentic-access.yml)
- [Developer Portal](https://www.lacuna.fm/docs)
- [Documentation](https://www.lacuna.fm/docs)
- [API Reference](https://www.lacuna.fm/docs/api)
- [Getting Started](https://www.lacuna.fm/docs)
- [Support](https://www.lacuna.fm/contact)
- [Blog](https://www.lacuna.fm/blog)
- [GitHub Organization](https://github.com/JOYLINK-LTD)
- [Pricing](https://www.lacuna.fm/pricing)
- [Sign Up](https://www.lacuna.fm/signin)
- [Terms of Service](https://www.lacuna.fm/terms)
- [Privacy Policy](https://www.lacuna.fm/privacy)
- [Changelog](https://www.lacuna.fm/changelog)
- [Changelog](changelog/lacuna-changelog.yml)
- [OpenAPI](openapi/_original/lacuna-music-openapi-original.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [L L Ms Txt](llms/lacuna-llms.txt)
- [Well Known](well-known/lacuna-well-known.yml)
- [Security Txt](well-known/lacuna-security.txt)
- [A P I Catalog](well-known/lacuna-api-catalog.json)
- [M C P Server](mcp/lacuna-mcp.yml)
- [Tool Crosswalk](mcp/lacuna-tool-crosswalk.yml)
- [Agent Card](a2a/lacuna-a2a.yml)
- [Agent Skill](skills/_index.yml)
- [Packages](packages/lacuna-packages.yml)
- [S D Ks](packages/lacuna-packages.yml)
- [C L I](cli/lacuna-cli.yml)
- [Authentication](authentication/lacuna-authentication.yml)
- [O Auth Scopes](scopes/lacuna-scopes.yml)
- [Conventions](conventions/lacuna-conventions.yml)
- [Error Catalog](errors/lacuna-problem-types.yml)
- [Webhooks](asyncapi/lacuna-webhooks.yml)
- [Lifecycle](lifecycle/lacuna-lifecycle.yml)
- [Conformance](conformance/lacuna-conformance.yml)
- [Domain Security](security/lacuna-domain-security.yml)
- [Vulnerability Disclosure](security/lacuna-vulnerability-disclosure.yml)
- [Security](https://www.lacuna.fm/.well-known/security.txt)
- [Rate Limits](rate-limits/lacuna-rate-limits.yml)
- [Plans](plans/lacuna-plans.yml)
- [Data Model](data-model/lacuna-data-model.yml)
- [Overlay](overlays/lacuna-music-overlay.yaml)
- [Arazzo](arazzo/lacuna-generate-and-collect.yml) — [Arazzo Specification](https://spec.openapis.org/arazzo/latest.html)

## Maintainers

**FN:** JOYLINK LTD
**Email:** support@lacuna.fm
**URL:** https://github.com/JOYLINK-LTD/lacuna-toolkit
