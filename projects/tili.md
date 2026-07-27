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
Website first draft **done, uncommitted** — `website/` Astro site, built by herdr agent `tili-web` (split pane in my workspace, Sonnet) 2026-07-27. `bun run build` passes clean. Layout: `src/pages/index.astro` (no-scroll landing, 100dvh flex + clamp()), Starlight docs nested one level deeper (`src/content/docs/docs/*.mdx`) so routes land at `/docs/*` while `/` stays free for the landing page. Theme tokens ported verbatim from dezen.me (`src/styles/tokens.css`), Starlight reskinned via `--sl-color-*` overrides (`starlight-overrides.css`), `ThemeToggle.astro` + `theme.ts` for the dark/light toggle, `CopyInstall.astro` for the brew one-liner. `wrangler.toml` present for the eventual Cloudflare Pages deploy to `tili.dezen.me` (still my manual step, not done).

Known judgment calls to review: footer version/license badges are hardcoded (v0.6.1/MIT, will drift on releases), used the 256×256 `icon-readme.png` for both landing + Starlight sidebar logos. **Nothing committed — diff needs review before merge.**

Previously: shipped v0.6.1 (synced Formula/tili.rb sha256/version) and fixed a notch-inset bug (double-counting the existing menu-bar baseline). Daily-drivable already — tiling, workspaces, hot-reload config, hotkeys, floating rules, multi-monitor, menu bar badge.

## Key decisions / context
- Roadmap (`ROADMAP.md`, unscheduled backlog): third-party status bar integration (SketchyBar, via a `tili subscribe` push protocol or exec-hook), tabbed/stacked containers, sticky windows, native-tab support.
- Design invariants: public API only, event-driven not polling, `WindowFrameSetter` stays the single seam for window-frame mutation (animation plugs in there later without a rewrite).
- Website repo layout: code lives in-tree under `tili/website/` (not a separate repo), isolated from the Cargo workspace/CI. Stack chosen to mirror [[dezen-me]]'s monochrome design tokens for brand consistency.


## Links
- Repo: https://github.com/itsdezen/tili
