---
name: lacuna-generate-and-collect
description: >-
  Submit a Lacuna music generation over raw HTTP and collect the finished audio, using the two published
  REST operations directly rather than an SDK. Use when the agent is not in a Node runtime, when the MCP
  server is unavailable, or when the work must run server-side behind a webhook.
api: openapi/lacuna-music-openapi-original.json
operations:
  - createGeneration
  - getGeneration
---

# Generate a track and collect the audio (raw REST)

The provider ships its own skill at `skills/lacuna-music.md`, which routes you to the SDK, the CLI or
the MCP server. Use this one when none of those fit and you are calling the HTTP API directly.

## Before you start

- You need a `lyr_live_` API key from <https://www.lacuna.fm/profile/api>, on an account with an active
  **Pro** plan or above. Read it from an environment variable; never inline it.
- Every generation **spends credits from a real account**. Confirm with the account holder before
  generating more than once.

## 1. Submit the task — `createGeneration`

`POST https://www.lacuna.fm/api/v1/music/generations`, `Authorization: Bearer $LACUNA_API_KEY`.

Body (`GenerateRequest`, `additionalProperties: false`):

- `style` — required, up to 1000 chars.
- `title` — required, up to 200 chars.
- `lyrics` — required unless `instrumental: true`; must contain a non-whitespace character. Use
  `[Verse]` / `[Chorus]` section markers.
- `instrumental` — optional boolean, default `false`.
- `model` — optional, `aether` (default) or `echo`. `nocturne` still validates but the spec marks it as
  retained for backward compatibility only — do not choose it for new work.
- `vocal_gender`, `negative_tags`, `style_weight`, `weirdness_constraint`, `audio_weight` — **`aether`
  only**. Sending any of them with `echo` or `nocturne` is a `400 invalid_request_error / invalid_param`,
  enforced by a conditional schema rule, not by a server-side courtesy.

A success is `202` with a complete `GenerationTask` in `status: pending`. Keep `id`.

**There is no idempotency key.** Credits are deducted synchronously on this POST. If the connection
drops after the request left, do **not** blind-retry — poll for a recent task or ask the account holder.
Retrying is how an agent double-charges someone.

## 2. Wait for it — `getGeneration`

`GET https://www.lacuna.fm/api/v1/music/generations/{id}` with the same header.

- Poll every **5 seconds**. Typical completion is 60–120 seconds.
- `status` is `pending`, `ready` or `failed`. Stop on `ready` or `failed`.
- On `ready`, `tracks[]` each carry `audio_url`, `duration`, `title`, `lyrics`, `image_url`, `tags`,
  `index`. `audio_url` is a hosted CDN URL with no stated expiry.
- On `failed`, read `error.code` / `error.message` on the task; `credits_refunded` will be non-zero.

In production prefer the `job.completed` webhook to polling — see `asyncapi/lacuna-webhooks.yml`.

## Error rules — when to retry and when to stop

Errors are an OpenAI-style envelope: `{ "error": { "type", "code", "message", "param?" } }`. Not RFC 9457.

| Status | Meaning | What to do |
| --- | --- | --- |
| 400 `invalid_param` | Model-incompatible field, or missing lyrics on a vocal track | Fix the body. Never retry unchanged. |
| 401 `authentication_error` | Key missing, invalid, expired or revoked | Stop. Ask for a new key. |
| 402 `insufficient_credits` | Balance too low | Stop. Tell the user to top up. |
| 403 `tier_insufficient` | Account below Pro | Stop. Tell the user to upgrade. |
| 429 `rpm_exceeded` / `concurrent_limit_exceeded` | 60 rpm per key; 10 concurrent on Pro, 20 on Ultra | Honour `Retry-After`, then retry. |
| 503 `model_unavailable` | That model is circuit-broken; `error.model` names it | Switch model and retry. Do **not** loop on the same one — credit costs differ, so nothing auto-falls-back. |
| 500 `api_error` | Upstream failure | Back off and retry once. |

## Verifying the webhook instead of polling

`job.completed` and `job.failed` arrive signed as
`X-Lacuna-Signature: t=<unix-seconds>,v1=<hex>`, an HMAC-SHA256 over `` `${timestamp}.${rawBody}` ``.
Recompute, compare, and reject anything whose timestamp is more than five minutes old. Any `2xx` you
return stops the retry.

## Related artifacts

- `conventions/lacuna-conventions.yml` — the cross-cutting semantics in one place.
- `errors/lacuna-problem-types.yml` — the full error catalog.
- `rate-limits/lacuna-rate-limits.yml` — the limits above, structured.
