---
__cgxx: |
  # vim:set expandtab shiftwidth=2 filetype=markdown foldlevel=3:
  # SPDX-License-Identifier: GPL-3.0-only

  #
  #
  # ~chewygumxx/cfw-filebin.git
  # ::: :/README.md
  #
  #

ctime: 2026-10-02
title: "cfw-filebin"
description: "[Cloudflare Worker] File upload bin"
tags:
  - cloudflare-worker
  - file-upload
---

# cfw-filebin

\[Cloudflare Worker\] File upload bin

## CI

`.github/workflows/ci.yaml` calls the shared
[standard workflow](https://github.com/chewygumxx/.github#standard-workflow):
commitlint, the header sync, generic lint and format checks for workflows,
shell and zsh scripts, TOML, YAML and `.editorconfig`, and the metadata sync.
This repository's own `bun run check` follows, against the commit the header
sync pushed.

## Development

- `bun run commit` composes a commit interactively.
- `bun run dev` serves the Worker locally, and `bun run deploy` deploys it.
- `bun run test` runs `test/` in the Workers runtime with Vitest and
  `@cloudflare/vitest-pool-workers`.
- `bun run types` regenerates `worker-configuration.d.ts` after a change to the
  bindings or vars in `wrangler.jsonc`; `bun run check` fails when it is stale.
- `bun run check` runs the checks CI runs: the typecheck, the types check, the
  tests, Biome's format and lint checks, Markdown lint, the YAML checks
  (prettier, then yamllint with `@chewygumxx/yamllint-config`), tombi's TOML
  format and lint checks and a check that rejects em dashes. A Dependabot pull
  request that bumps only `wrangler` can fail that check until `bun run types`
  is run on its branch.
- `bun run format` applies Biome formatting, prettier's to YAML and tombi's to
  TOML, which Biome does not read.

The pre-commit hook runs the same checks on staged files. The commit-msg hook
runs commitlint.

## Deploying

`.github/workflows/deploy.yaml` deploys the Worker with `wrangler deploy` after
CI passes on `main`, and when run by hand. It stays skipped, which GitHub counts
as passing, until you set these in the repository's settings:

- the variable `CLOUDFLARE_ACCOUNT_ID`, your Cloudflare account ID
- the secret `CLOUDFLARE_API_TOKEN`, an API token that can edit Workers

`create-repo` sets neither.
