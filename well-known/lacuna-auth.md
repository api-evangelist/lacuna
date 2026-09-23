# auth.md

Lacuna (https://www.lacuna.fm) generates AI music. This document tells an autonomous agent how to obtain and
use a credential against the Lacuna REST API, the remote MCP server, and the A2A endpoint.
Follow the steps in order; do not skip ahead.

## Audience

Agents acting on behalf of a Lacuna account holder. There is no anonymous tier — every call spends
credits from a specific account, so every call must be attributable to one.

Lacuna does **not** support dynamic client registration (RFC 7591), ID-JAG assertions, or an
anonymous claim ceremony. There are exactly two ways to get a credential, and both involve a human
account holder at some point.

## Discovery

An unauthenticated call answers 401 and points back at the metadata:

```http
POST /api/v1/music/generations HTTP/1.1
Host: www.lacuna.fm
Content-Type: application/json
```

Response (401):

```http
HTTP/1.1 401 Unauthorized
Content-Type: application/json
WWW-Authenticate: Bearer realm="lacuna", resource_metadata="https://www.lacuna.fm/.well-known/oauth-protected-resource", error="invalid_token", error_description="Invalid API key"
```

| Document | URL |
| --- | --- |
| Protected resource metadata (RFC 9728) | https://www.lacuna.fm/.well-known/oauth-protected-resource |
| Authorization server metadata (RFC 8414) | https://www.lacuna.fm/.well-known/oauth-authorization-server |
| API catalog (RFC 9727) | https://www.lacuna.fm/.well-known/api-catalog |
| OpenAPI description | https://www.lacuna.fm/api/openapi.json |
| MCP server card | https://www.lacuna.fm/.well-known/mcp/server-card.json |
| A2A agent card | https://www.lacuna.fm/.well-known/agent-card.json |
| JWKS (OAuth access token verification) | https://www.lacuna.fm/.well-known/jwks.json |

## Step 1 — Obtain a credential

### Method A — Developer API key (recommended for autonomous agents)

This is the only path that works without a browser, but it has **no registration endpoint**: the
account holder issues the key from the dashboard and hands it to you. Do not attempt to provision
one programmatically — there is no such API.

Registration URL (human, in a browser): https://www.lacuna.fm/profile/api

- Credential format: a bearer token prefixed `lyr_live_`.
- Requires an active Pro plan or higher on the account.
- Scopes and an optional expiry are fixed at creation time.
- Revoke at the same URL; revocation takes effect immediately.

If you have no key and no human is available, stop here and ask the account holder for one. Report
the registration URL above verbatim.

### Method B — OAuth 2.0 authorization code + PKCE

For clients registered with Lacuna as first-party applications (currently the official browser
extension and the iOS app). Client registration is manual — contact support@lacuna.fm. If you were
not issued a `client_id`, use Method A instead.

**Step B1 — authorization request.** Open this in a user agent; it answers 302 to your registered
redirect URI carrying `code`. `client_instance_id` is a Lacuna-specific required parameter: 43
characters of base64url (32 random bytes), stable for one installation.

```http
GET /oauth/authorize?response_type=code&client_id=<client_id>&client_instance_id=<43-char base64url>&redirect_uri=<registered_uri>&scope=music%3Agenerate&state=<state>&code_challenge=<BASE64URL(SHA256(verifier))>&code_challenge_method=S256 HTTP/1.1
Host: www.lacuna.fm
```

`code_challenge_method=S256` is mandatory. The verifier is 43–128 characters from
`[A-Za-z0-9._~-]`. The authorization code expires in 120 seconds and is single-use.

**Step B2 — token exchange.** Public clients only; there is no client secret.

```http
POST /oauth/token HTTP/1.1
Host: www.lacuna.fm
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&code=<code>&redirect_uri=<registered_uri>&client_id=<client_id>&client_instance_id=<same value as Step B1>&code_verifier=<verifier>
```

Response (200):

```json
{
  "access_token": "...",
  "token_type": "Bearer",
  "expires_in": 900,
  "scope": "music:generate"
}
```

The access token lives 15 minutes. A `refresh_token` is present only for clients registered to
receive one (the iOS app); the browser extension re-runs Step B1 instead. Refresh tokens rotate on
every use:

```http
POST /oauth/token HTTP/1.1
Host: www.lacuna.fm
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&refresh_token=<refresh_token>&client_id=<client_id>&client_instance_id=<same value as Step B1>
```

The official iOS app can start a Lacuna session directly from the native Sign in with Apple sheet.
This private grant is registered only for `lacuna-ios`; third-party clients cannot use it. The app
must send Apple's identity token, the original 43-character nonce, and Apple's single-use
authorization `code`. A missing or blank `code` is rejected with `invalid_request`; Lacuna
tokens are issued only after Apple accepts the code and confirms it belongs to the same Apple user.
The response uses the same rotating Lacuna refresh-token shape described above:

```http
POST /oauth/token HTTP/1.1
Host: www.lacuna.fm
Content-Type: application/x-www-form-urlencoded

grant_type=urn%3Alacuna%3Aparams%3Aoauth%3Agrant-type%3Aapple-assertion&client_id=lacuna-ios&client_instance_id=<43-char base64url>&id_token=<Apple identity token>&nonce=<raw 43-char base64url>&code=<Apple authorization code>&scope=profile%20music%3Aagent%20lyrics%3Aagent
```

## Step 2 — Use the credential

Send the same header on every surface, whether the credential came from Method A or Method B:

```http
POST /api/v1/music/generations HTTP/1.1
Host: www.lacuna.fm
Authorization: Bearer <api-key-or-access-token>
Content-Type: application/json

{
  "title": "Midnight Drive",
  "style": "lofi hip hop, warm tape saturation",
  "instrumental": true
}
```

Response (202) returns a task; generation is asynchronous, so poll it:

```http
GET /api/v1/music/generations/<id> HTTP/1.1
Host: www.lacuna.fm
Authorization: Bearer <api-key-or-access-token>
```

The full parameter list is in the OpenAPI description linked above.

| Surface | Endpoint | Notes |
| --- | --- | --- |
| REST | https://www.lacuna.fm/api/v1/music/generations | POST to submit, GET /{id} to poll |
| MCP (Streamable HTTP) | https://www.lacuna.fm/mcp | Stateless; initialize and tools/list are open, tools/call requires auth |
| A2A (JSON-RPC) | https://www.lacuna.fm/a2a | message/send, tasks/get; auth required on every method |

## Step 3 — Revocation

API keys are revoked by the account holder at https://www.lacuna.fm/profile/api. OAuth tokens are revoked over
the wire (RFC 7009) — this accepts either an access token or a refresh token, and always answers
200:

```http
POST /oauth/revoke HTTP/1.1
Host: www.lacuna.fm
Content-Type: application/x-www-form-urlencoded

token=<access_token-or-refresh_token>&client_id=<client_id>
```

## Scopes

- `profile`
- `music:generate`
- `livecoding:agent`
- `music:agent`
- `lyrics:agent`

Music generation over REST, MCP, and A2A all require `music:generate`. Requesting a scope the
client is not registered for fails the authorization request with `invalid_scope`.

## Errors

| Status | Code | Meaning | What to do |
| --- | --- | --- | --- |
| 401 | `invalid_api_key` | The key does not exist | Do not retry. Ask the account holder for a valid key |
| 401 | `key_revoked` / `key_expired` | The key was revoked or has expired | Drop the credential; ask for a new one |
| 401 | `invalid_token` | OAuth access token invalid, expired, or revoked | Refresh if you hold a refresh token, otherwise restart Step B1 |
| 403 | `insufficient_scope` | Credential lacks the required scope | Do not retry. The required scope is in the WWW-Authenticate header |
| 402 | `insufficient_credits` | The account is out of credits | Stop. Tell the account holder; retrying will not help |
| 429 | `rpm_exceeded` / `concurrent_limit_exceeded` | Rate or concurrency limit | Back off for the number of seconds in Retry-After, then retry |
| 5xx | — | Transient server error | Exponential backoff, retry the same request |

## Cost and limits

Generation is asynchronous and **costs credits per task**, deducted at submission and refunded
automatically if the task fails. Pricing is at https://www.lacuna.fm/pricing. Requests are rate limited per key
and per account concurrency; 429 responses carry Retry-After.

Confirm with the account holder before generating repeatedly — an agent in a retry loop spends real
money.

Report abuse or credential compromise to support@lacuna.fm.
