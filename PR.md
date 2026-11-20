# Add Clixad to the plugin market

Adds `@magpie-community/opencode-clixad-auth` (provider id `clixad`) and its
registry entry.

## What it is

Clixad (clixad.onrender.com) is an OpenAI-compatible gateway with 15 models
whose billing stays on the user's own Clixad account: a daily free allowance
first, credits after. There is no other Clixad entry in the registry, so this
is a new channel rather than a duplicate.

## How it signs in

Two methods, both against the user's existing Clixad account:

- **Reuse the local Clixad CLI** — reads `~/.clixad/config.json` as it is
  (`token`, `login`/`email`, `tokenGateway`), reads nothing else and writes
  nothing; the token is checked against `/v1/wallet` before it is kept.
- **Paste the token** — accepts the `adg_…` token from that file; also
  checked against `/v1/wallet`.

Clixad tokens are long-lived and Clixad has no refresh endpoint, so the plugin
declares no `auth.refresh`.

## What it shows

- `/v1/models` per account, refreshed at each listing; a static 15-model
  catalog before sign-in and whenever a read fails.
- `/v1/wallet` as the Usage card: the day's free-credit window
  (`daily_credits` + `extra_credits` − `credits_left`, resetting at 00:00 UTC)
  and the credit balance. No request counts or spend figures are invented.

## Requests

`https://clixad.onrender.com/v1` (or the gateway URL named by the CLI config),
Chat Completions via `@ai-sdk/openai-compatible`. The loader sets
`Authorization: Bearer` on every request; streaming, tool calls and reasoning
levels pass through unchanged.

## Checks

- `node --test` — 7 tests, all pass.
- `bun scripts/check.mjs clixad` in this repository — `✓ registry.json`,
  `✓ clixad`.

The package carries `index.mjs`, `opencode.mjs` (OpenCode 1.x / magpie),
`opencode2.mjs` (OpenCode 2.x, byte-identical to `scripts/opencode2.mjs`),
`README.md`, `LICENSE` (MIT) and `index.test.mjs`.

Source repository (mirror): https://github.com/YeWen0130/magpie-clixad

Unofficial; not affiliated with Clixad.
