# Lacuna

Lacuna (lacuna.fm), operated by JOYLINK LTD, is an AI music creation platform that turns lyrics or a plain-text style description into complete songs with vocals, alongside lyrics writing, word-level timed lyrics, lyric video export, AI mastering, mashups, stem separation, album-cover art, MIDI tooling and sheet-music conversion, plus long-form AI radio.

- **Provider:** https://www.lacuna.fm
- **Documentation:** https://www.lacuna.fm/docs
- **API base URL:** https://www.lacuna.fm/api
- **OpenAPI:** https://www.lacuna.fm/api/openapi.json (3.1.0)
- **Hosted MCP:** https://www.lacuna.fm/mcp (Streamable HTTP, protocol 2025-06-18)
- **A2A:** https://www.lacuna.fm/a2a (agent card at /.well-known/agent-card.json)
- **Tags:** AI Music, Music Generation, AI Song Generator, AI Lyrics Generator, Audio, MIDI, Songwriting, Generative AI, MCP Server, Agent Skill, A2A, Developer Tools
- **Public API:** yes
- **Agent-native (MCP / A2A / llms.txt / skills):** yes

## APIs

- **Lacuna Music API** — REST music generation. `POST /v1/music/generations` submits a task, `GET /v1/music/generations/{id}` polls it, and four HMAC-signed webhooks (`job.completed`, `job.failed`, `credits.low`, `key.expiring`) push state. Bearer auth with a `lyr_live_` key, OpenAI-style error envelope, 60 rpm per key, credits deducted on submit and refunded on failure. Requires a Pro plan or above. (`https://www.lacuna.fm/api`)
- **Lacuna MCP Server** — hosted Streamable-HTTP MCP server. `initialize` and `tools/list` answer anonymously with real JSON Schema input contracts; `tools/call` requires `music:generate` or an API key. Tools: `generate_music`, `get_generation`, `list_models`. (`https://www.lacuna.fm/mcp`)
- **Lacuna A2A Agent** — JSON-RPC agent endpoint with a published agent card (protocolVersion 0.3.0), one skill `generate_music`. Auth required on every method. (`https://www.lacuna.fm/a2a`)

## Correction to the original submission profile

The community-submission profile recorded "no public machine-readable contract (no OpenAPI/AsyncAPI/GraphQL/Postman)" and described the MCP server as local-stdio-only. **Both are wrong as of 2026-08-09.** A real OpenAPI 3.1.0 is served at `https://www.lacuna.fm/api/openapi.json` — reachable in one hop from the RFC 9727 api-catalog at `/.well-known/api-catalog` — and a hosted MCP endpoint plus an A2A endpoint shipped in the 2026-08-09 release. The stdio `lacuna-mcp` npm package still exists, but it is now the local sibling of a network server, not the whole MCP story.

## What stands out

Lacuna publishes a fuller machine discovery surface than most companies many times its size: RFC 9727 api-catalog, RFC 8414 authorization-server metadata, RFC 9728 protected-resource metadata, RFC 9116 security.txt, JWKS, an MCP server card, an A2A agent card, an `llms.txt`, a `Content-Signal` line in `robots.txt`, and an `/auth.md` written specifically to tell an autonomous agent how to get a credential. An agent handed only the domain can find every surface unaided.

## Where it is thin

- **No idempotency key on a credit-spending POST.** `createGeneration` deducts credits synchronously and offers no replay contract, so a retried submit charges twice. The provider's own Agent Skill warns agents not to loop — guidance standing in for a mechanism.
- **No status page, no SLA.** `/status` 404s and `status.lacuna.fm` does not resolve; only a liveness endpoint at `/api/health` exists.
- **No deprecation policy**, despite already carrying a legacy model codename (`nocturne`) marked "not recommended for new integrations" with no removal date.
- **No published compliance program** — no SOC 2, ISO 27001 or trust center.
- **No AsyncAPI** for a real four-event webhook surface that is already fully schema'd in the OpenAPI `webhooks` block.

_Enriched by the API Evangelist enrichment pipeline, 2026-08-09._
