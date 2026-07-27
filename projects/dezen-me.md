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
Assets & SEO optimization pass — **merged to `main` (FF) and pushed** as `9782023` "🚀 Optimize images & harden SEO metadata" (41 files, 2026-07-26); optim/assets-seo branch deleted. CI auto-deploys via Cloudflare Workers Builds git integration. Done by a herdr claudemax agent per a Remi diagnostic:
- og-image.png 1.26 MB → 57 KB (+webp); total public image weight ~2.6 MB → ~0.9 MB.
- 6 external-CDN project images self-hosted to `public/projects/*.{webp,jpg}` (jpg = crawler-friendly OG); equipment PNGs → webp (old PNGs deleted).
- Per-route/per-project OG (absolute URLs + og:image:alt), JSON-LD CreativeWork + BreadcrumbList on `/projects/$slug`, raw.githubusercontent.com preconnect, longer cache headers for content images, explicit img width/height + decoding=async.
- `projectSchema.image` relaxed `.url()` → `.string()` (root-relative paths aren't valid URLs). typecheck/lint/test all pass.
- Not yet done (need user decision): GSAP→CSS swap (contradicts §4/§10 contract), filling Equipment "specs TBD". Recommend a `bun run dev` visual smoke test before merge/deploy.

## Key decisions / context
- Run `bun run dev`, not `bun --bun run dev` — Bun's own WebSocket implementation hangs the Cloudflare dev server inspector; plain `bun run dev` runs Vite under Node via shebang.
- Working tree clean, branch `main`, no open WIP as of 2026-07-26.


## Links
- Repo: https://github.com/itsdezen/dezen.me
