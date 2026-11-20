# magpie-clixad

[Clixad](https://clixad.onrender.com) as a magpie provider: all 15 Clixad
models through Magpie's gateway, with the Clixad account still doing the
billing (free tier first, credits after).

```sh
magpie plugin add D:\BianCheng\clixad\magpie-clixad
magpie plugin login clixad     # pick「从本机 Clixad CLI 导入」
magpie provider test clixad
```

Agents then reach the models as `clixad/<model>`:

```sh
magpie codex clixad/deepseek-v4.1-flash
```

## Sign-in

- **从本机 Clixad CLI 导入** — reads `~/.clixad/config.json` as it is and
  validates the token with `/v1/wallet`. The CLI file is never modified.
- **粘贴 Clixad token** — paste the `adg_…` token from `~/.clixad/config.json`.
  Useful on another machine or for a second account.

## What shows where

- `/v1/models` refreshes the model list per account (the static table above is
  the fallback before sign-in).
- `/v1/wallet` becomes the Usage card: the daily free-credit window and the
  credit balance.

## Notes

- No `auth.refresh`: Clixad tokens are long-lived and the CLI has no published
  refresh endpoint; sign in again with `magpie plugin login clixad` if one is
  revoked.
- All models are listed. `free: true` marks the ones covered by the daily
  free allowance; the rest spend the account's credits.
