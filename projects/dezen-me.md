---
slug: dezen-me
status: active
path: /Users/itsdezen/Developer/dezen.me
updated: 2026-07-26
---

# dezen.me

## What it is
Personal website/blog. TanStack + Vite, deployed to Cloudflare Workers (wrangler). Bun for package management/scripts. Engineering contract lives in `AGENTS.md`.

## Current focus
Visual polish on the `/uses` page — flattened Equipment/Stack/Environment grids (dropped category headers), real equipment photos, adaptive-monogram favicon. Just came off fixing a production bug (`chunkImportMap` 404ing every chunk).

## Key decisions / context
- Run `bun run dev`, not `bun --bun run dev` — Bun's own WebSocket implementation hangs the Cloudflare dev server inspector; plain `bun run dev` runs Vite under Node via shebang.
- Working tree clean, branch `main`, no open WIP as of 2026-07-26.


## Links
- Repo: https://github.com/itsdezen/dezen.me
