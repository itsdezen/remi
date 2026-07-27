---
slug: tili
status: active
path: /Users/itsdezen/Developer/tili
updated: 2026-07-26
---

# tili

## What it is
i3-style tiling window manager for macOS. Rust, public Accessibility API only (no SIP disable). Cargo workspace: `tili-tree` (layout algorithms), `tili-ax` (Accessibility integration), `tili-config` (KDL, hot-reload), `tili-ipc`, `tili-daemon`, `tili-cli`, `tili-menubar`. Installed via Homebrew tap (`itsdezen/homebrew-tap`), signed releases.

## Current focus
Website **shipped and pushed to `main`** (2026-07-27) — `website/` Astro + Starlight site, built via herdr agent `tili-web`. Landing `/` (no-scroll, 100dvh + clamp(), monochrome theme matching dezen.me, dark/light toggle, Lucide-icon CTA row) + docs `/docs` (Configuration/Commands/Menu Bar rewritten from actual source code not README, Changelog page sourced from CHANGELOG.md, logo-only nav). Favicon/logo went through several iterations — final state: theme-adaptive favicon.svg (media-query fill swap, matches dezen.me exactly), transparent favicon.ico/96x96 PNG with no padding (glyph fills canvas edge-to-edge), apple-touch-icon/manifest PNGs intentionally left opaque per Apple/PWA convention. Deploy target: Cloudflare Workers (static assets, `wrangler.jsonc`) — **CI still needs the user to connect the repo in the Cloudflare dashboard** (root directory must be set to `website/` since this is a monorepo) and add `tili.dezen.me` as a custom domain; not something I can do without dashboard access.

Previously: shipped v0.6.1 (synced Formula/tili.rb sha256/version) and fixed a notch-inset bug (double-counting the existing menu-bar baseline). Daily-drivable already — tiling, workspaces, hot-reload config, hotkeys, floating rules, multi-monitor, menu bar badge.

## Key decisions / context
- Roadmap (`ROADMAP.md`, unscheduled backlog): third-party status bar integration (SketchyBar, via a `tili subscribe` push protocol or exec-hook), tabbed/stacked containers, sticky windows, native-tab support.
- Design invariants: public API only, event-driven not polling, `WindowFrameSetter` stays the single seam for window-frame mutation (animation plugs in there later without a rewrite).
- Website repo layout: code lives in-tree under `tili/website/` (not a separate repo), isolated from the Cargo workspace/CI. Stack mirrors [[dezen-me]]'s monochrome design tokens for brand consistency; deploys via Cloudflare Workers static assets rather than Pages, matching how dezen.me deploys (git-connected, no local `wrangler deploy`).
- `main` branch has PR + status-check protection rules, but push access bypasses them (repo owner) — website commits went straight to `main` as a one-off exception, not a new standing practice.
- Working tree clean, branch `main`, no open WIP as of 2026-07-27.

## Links
- Repo: https://github.com/itsdezen/tili
- Website: https://tili.dezen.me (pending Cloudflare dashboard connection)
