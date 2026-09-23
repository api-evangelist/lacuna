---
name: lacuna-preflight-and-budget
description: >-
  Check a Lacuna credential, plan, credit balance and rate limits with one free call before spending
  money on a generation. Use before any first generation, after any 401/402/403, when deciding how many
  tracks a budget allows, or on a schedule to watch for key expiry.
api: openapi/_original/lacuna-music-openapi-original.json
operations:
  - getMe
  - createGeneration
---

# Check before you spend on Lacuna

Every Lacuna generation costs real credits, deducted the moment `POST /v1/music/generations` returns
`202`. There is **no idempotency key, no dry run, and no way to cancel or refund a generation you
started** — if you fire twice, you pay twice, and nothing takes it back. The provider's own Agent Skill
says so in as many words: *"confirm with the account holder before generating repeatedly — an agent in
a retry loop spends real money."*

`GET /v1/me` is the one free call on this API. The spec states plainly that no credits are consumed and
names it "the endpoint to point a connection test at". Use it as a precondition gate, not an
afterthought.

## 1. Read the account

```http
GET /v1/me HTTP/1.1
Host: www.lacuna.fm
Authorization: Bearer <lyr_live_... or OAuth access token>
```

Base URL is `https://www.lacuna.fm/api`, so the full URL is
`https://www.lacuna.fm/api/v1/me`. Same `Authorization: Bearer` header as every other Lacuna surface.

A `200` returns the `Account` object:

```json
{
  "id": "...",
  "plan": "pro",
  "credits":     { "subscription": 7200, "onetime": 0, "total": 7200 },
  "rate_limits": { "requests_per_minute": 60, "concurrent_generations": 10 },
  "auth": {
    "kind": "api_key",
    "scopes": ["music:generate"],
    "key": { "id": "...", "name": "...", "expires_at": "2027-01-01T00:00:00Z" }
  }
}
```

## 2. Gate on what it tells you

Check these **in this order** and stop at the first failure. Each one maps to an error you would
otherwise have discovered by spending a call — or by spending credits.

| Check | Field | If it fails |
| --- | --- | --- |
| Credential is live | a `200` at all | A `401` means `invalid_api_key`, `key_revoked`, `key_expired` or `invalid_token`. Do not retry — ask the account holder for a new credential. |
| Plan is eligible | `plan` is `pro` or `ultra` | `free` and `basic` cannot call the music API. You will get `403 permission_error / tier_insufficient`. Tell the account holder to upgrade; retrying never helps. |
| Scope is granted | `music:generate` in `auth.scopes` | Without it, generation returns `403 insufficient_scope` on REST, MCP and A2A alike. Re-authorize; do not retry. |
| Budget covers the work | `credits.total` | Compare against the model cost **before** submitting — see step 3. Too few gives `402 insufficient_credits`, which no retry fixes. |
| Key is not about to expire | `auth.key.expires_at` | Only present when `auth.kind` is `api_key`, and it may be `null` (no expiry). If it is close, ask for a rotation now rather than failing mid-run. |
| Concurrency budget | `rate_limits.concurrent_generations` | Never have more than this many tasks in flight, or you get `429 concurrent_limit_exceeded`. |
| Request budget | `rate_limits.requests_per_minute` | Cap your polling and submitting against this, or you get `429 rpm_exceeded`. |

**Read the limits from here, not from the docs.** The documentation publishes per-plan defaults
(60 rpm; 10 concurrent on Pro, 20 on Ultra) but the spec says account-specific overrides may apply.
`/v1/me` is the only place the *effective* limits for your credential are published.

## 3. Do the budget arithmetic before you submit

Credit cost is per generation and depends on the model:

| Model | Credits |
| --- | ---: |
| `aether` (default) | 50 |
| `echo` | 80 |
| `nocturne` | 180 |

So `credits.total / cost` is the number of generations you can still afford. Compute it, and if the
account holder asked for more tracks than that, **stop and tell them the shortfall before generating
any of them** — a partial run that dies on `402` halfway has already spent the credits it consumed and
cannot get them back.

`credits.subscription` is spent before `credits.onetime`, so a total that looks healthy may be mostly
non-renewing one-time credits. Report the split when it matters to the decision.

## 4. Only then generate

Once every gate passes, go to `createGeneration` — see `lacuna-generate-and-collect.md` for the submit
and poll loop. Carry two rules with you:

- **Never blind-retry a `POST /v1/music/generations` that timed out.** With no idempotency key you
  cannot tell a lost response from a lost request, and a retry may be a second purchase. Poll for a
  task you may already have created, or ask the account holder, before re-submitting.
- **Re-run `/v1/me` after a run** to confirm what was actually spent, and after any `402`/`403` to see
  which precondition changed underneath you.

## What this check cannot do

- It does **not** reserve or hold credits. Another client on the same account can spend them between
  your check and your submit. Treat the balance as advisory, and keep the gap small.
- It does **not** let you rehearse a generation. There is no dry-run or cost-estimate parameter on
  `createGeneration`; this is a precondition check, not a preview.
- It is **not available over MCP or A2A.** A live anonymous `tools/list` on 2026-09-11 returned only
  `generate_music`, `get_generation` and `list_models`, and the agent card advertises one skill. If you
  are working through the hosted MCP server, you must make this one call over plain REST to get any of
  the information above.

## Provenance

Grounded in `openapi/_original/lacuna-music-openapi-original.json` (`operationId: getMe`, schema
`Account`), fetched from `https://www.lacuna.fm/api/openapi.json` on 2026-09-11, plus the error table
in `https://www.lacuna.fm/auth.md`. Every field name and status code above appears in one of those two
documents.
