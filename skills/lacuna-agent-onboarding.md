---
name: lacuna-agent-onboarding
description: >-
  Discover and connect to Lacuna as an autonomous agent — find the surfaces from the well-known
  documents, pick between REST, the hosted MCP server and A2A, and obtain a credential. Use when an
  agent has been pointed at lacuna.fm with no URLs handed to it, or must choose a transport.
api: openapi/lacuna-music-openapi-original.json
operations:
  - createGeneration
  - getGeneration
---

# Onboard onto Lacuna as an agent

Lacuna is one of the few providers where an agent can find everything it needs starting from the domain
alone. Do that rather than hard-coding endpoints.

## 1. Discover

`GET https://www.lacuna.fm/.well-known/api-catalog` (RFC 9727, `application/linkset+json`). It anchors
three surfaces:

| Anchor | `service-desc` | `service-doc` |
| --- | --- | --- |
| `/api` | `https://www.lacuna.fm/api/openapi.json` | `/docs/api` |
| `/mcp` | `https://www.lacuna.fm/.well-known/mcp/server-card.json` | `/docs/mcp` |
| `/a2a` | `https://www.lacuna.fm/.well-known/agent-card.json` | `/docs/api` |

The `/api` anchor also carries a `status` link at `https://www.lacuna.fm/api/health`.

Then read `https://www.lacuna.fm/auth.md` — the provider's own credential-acquisition guide, linked from
`agent_auth.skill` in the authorization-server metadata.

## 2. Get a credential

Two paths, both needing a human at some point. Dynamic client registration (RFC 7591) is **not**
supported, and there is no anonymous tier — every call spends credits from a specific account.

1. **Developer API key** — the only browser-free path, and the one to use. The account holder creates it
   at `https://www.lacuna.fm/profile/api`; it is shown once, begins `lyr_live_`, and needs an active Pro
   plan or above. Send `Authorization: Bearer <key>`.
2. **OAuth 2.0 authorization code + PKCE** — for clients registered manually with Lacuna as first-party.
   Authorize at `/oauth/authorize`, token at `/oauth/token`, revoke at `/oauth/revoke`, JWKS at
   `/.well-known/jwks.json`. `S256` required, public client (`token_endpoint_auth_method: none`), access
   tokens live 15 minutes.

Scopes: `profile`, `music:generate`, `livecoding:agent`, `music:agent`, `lyrics:agent`. Generation over
**any** surface requires `music:generate`.

An unauthenticated request answers `401` with a `WWW-Authenticate: Bearer ... resource_metadata="..."`
header pointing back at `/.well-known/oauth-protected-resource`.

## 3. Pick a surface

| Surface | Endpoint | Use it when |
| --- | --- | --- |
| MCP (Streamable HTTP) | `https://www.lacuna.fm/mcp` | You are an MCP client. `initialize` and `tools/list` are open, so you can inspect the tools before authenticating. `tools/call` needs the credential. Tools: `generate_music`, `get_generation`, `list_models`. |
| REST | `https://www.lacuna.fm/api/v1/music/generations` | You are calling HTTP directly or running server-side with webhooks. Operations: `createGeneration`, `getGeneration`. |
| A2A (JSON-RPC) | `https://www.lacuna.fm/a2a` | You are delegating to Lacuna as a peer agent. `message/send` then `tasks/get`. Auth on every method. |
| MCP (stdio) | `npx -y lacuna-mcp` | Local-only client, no network MCP support. Note this build exposes `wait_for_generation` instead of `list_models`. |

The surfaces are not identical — `list_models` exists only on the hosted MCP server, and the `nocturne`
model appears in the OpenAPI enum but not in the MCP tool schema. See `mcp/lacuna-tool-crosswalk.yml`.

## 4. Spend responsibly

Generation is asynchronous and billed in credits per task — `aether` 50, `echo` 80, `nocturne` 180 —
deducted at submit and refunded automatically on failure. There is **no idempotency key on the submit**,
so a retry loop spends real money. Lacuna's own guidance is to confirm with the account holder before
generating repeatedly. Honour `Retry-After` on `429` and `503`.

## Related artifacts

- `well-known/lacuna-well-known.yml` — every discovery document with its probed status.
- `authentication/lacuna-authentication.yml`, `scopes/lacuna-scopes.yml`.
- `mcp/lacuna-mcp.yml`, `a2a/lacuna-a2a.yml`.
