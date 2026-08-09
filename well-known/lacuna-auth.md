# auth.md

Lacuna (https://www.lacuna.fm) generates AI music. This file tells an autonomous agent how to obtain and
use credentials against the Lacuna API, the remote MCP server, and the A2A endpoint.

## Audience

Agents acting on behalf of a Lacuna account holder. There is no anonymous tier — every call
spends credits from a specific account, so every call must be attributable to one.

## Discovery

| Document | URL |
| --- | --- |
| Protected resource metadata (RFC 9728) | `https://www.lacuna.fm/.well-known/oauth-protected-resource` |
| Authorization server metadata (RFC 8414) | `https://www.lacuna.fm/.well-known/oauth-authorization-server` |
| API catalog (RFC 9727) | `https://www.lacuna.fm/.well-known/api-catalog` |
| OpenAPI description | `https://www.lacuna.fm/api/openapi.json` |
| MCP server card | `https://www.lacuna.fm/.well-known/mcp/server-card.json` |
| A2A agent card | `https://www.lacuna.fm/.well-known/agent-card.json` |

Any request without a usable credential answers `401` with a
`WWW-Authenticate: Bearer ... resource_metadata="..."` header pointing back at the metadata above.

## Registration

Lacuna does **not** support dynamic client registration (RFC 7591), ID-JAG assertions, or an
anonymous claim ceremony. There are exactly two ways an agent gets a credential, and both
require a human account holder at some point.

### 1. Developer API key (recommended for autonomous agents)

The account holder creates a key at `https://www.lacuna.fm/profile/api` and hands it to the agent.

- Format: `Authorization: Bearer lyr_live_...`
- Requires an active Pro plan or higher on the account.
- Keys carry scopes and an optional expiry, both set at creation time.
- Revoke at the same URL; revocation takes effect immediately.

This is the only path that works without a browser.

### 2. OAuth 2.0 authorization code + PKCE

For clients registered with Lacuna as first-party applications (currently the official browser
extension and the iOS app). Client registration is manual — contact support@lacuna.fm.

- Authorization: `https://www.lacuna.fm/oauth/authorize`
- Token: `https://www.lacuna.fm/oauth/token`
- Revocation: `https://www.lacuna.fm/oauth/revoke`
- JWKS: `https://www.lacuna.fm/.well-known/jwks.json`
- `code_challenge_method=S256` is required; `token_endpoint_auth_method` is `none` (public clients).
- Access tokens live 15 minutes. Refresh tokens are issued to the iOS client only.

## Scopes

- `profile`
- `music:generate`
- `livecoding:agent`
- `music:agent`
- `lyrics:agent`

Music generation over REST, MCP, and A2A all require `music:generate`.

## Using the credential

Send the same header everywhere:

```http
Authorization: Bearer <api-key-or-access-token>
```

| Surface | Endpoint | Notes |
| --- | --- | --- |
| REST | `https://www.lacuna.fm/api/v1/music/generations` | `POST` to submit, `GET /{id}` to poll |
| MCP (Streamable HTTP) | `https://www.lacuna.fm/mcp` | Stateless; `initialize` and `tools/list` are open, `tools/call` requires auth |
| A2A (JSON-RPC) | `https://www.lacuna.fm/a2a` | `message/send`, `tasks/get`; auth required on every method |

## Cost and limits

Generation is asynchronous and **costs credits per task**, deducted at submission and refunded
automatically if the task fails. Pricing is at `https://www.lacuna.fm/pricing`. Requests are rate limited
per key and per account concurrency; `429` responses carry `Retry-After`.

Confirm with the account holder before generating repeatedly — an agent in a retry loop spends
real money.

## Revocation

- API keys: revoke at `https://www.lacuna.fm/profile/api`.
- OAuth tokens: `POST https://www.lacuna.fm/oauth/revoke`.

Report abuse or credential compromise to support@lacuna.fm.
