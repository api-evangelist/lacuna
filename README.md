# Lacuna (lacuna)

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
