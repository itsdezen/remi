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
2026-08-16: CLAUDE.md content moved into a new universal AGENTS.md (genericized, no Claude-specific phrasing); CLAUDE.md reduced to a single `@AGENTS.md` pointer line. Committed and pushed (205c960). Done via a delegated herdr staff session, babysat in default permission mode.

2026-08-09: **v0.7.0 shipped** — tagged and pushed, release CI running (build/publish/sync-homebrew-tap on GitHub Actions). User tested locally first (build + live badge check), then had Remi run the full release process via herdr sub-agents. 2 commits on `main`: `998e289` (feature work) + `066d2b3` (🚀 release v0.7.0 — Cargo.toml/Cargo.lock/CHANGELOG.md). Tag `v0.7.0` pushed, CI (`Release`/`CI` workflows) picked it up immediately.

Feature content (3 changes), CI gate (fmt/clippy -D warnings/test --workspace/build --workspace) all green:
1. Docs: new "Built-in default modes" subsection in `configuration.mdx` explaining `resize` (sticky, alt-shift-semicolon in/escape-enter out) and `manage` (auto-exit, one-shot) modes, plus a `:::note` clarifying `alt-shift-g` ("quick join left" in `main`) vs. `manage` mode's full 4-direction `alt-shift-h/j/k/l` joins — both dispatch the same `join <dir>` command. `commands.mdx` cross-references it.
2. Rebound `manage` mode's default entry from `alt-shift-s` to `alt-shift-m` (`example/tili.kdl`, matching test fixture in `state.rs`, `CHANGELOG.md` entry) — verified zero repo-wide conflicts before changing. Personal dotfiles config (`dotfiles/tili/.config/tili/tili.kdl`) already updated to match, committed+pushed (5b0f32a) — see [[dotfiles]].
3. Menu bar badge now shows a distinct glyph per active mode instead of always the same dot: ● (main/custom, unchanged size), ↔ (resize, `MODE_GLYPH_FONT_SIZE`=10.0), ⚙ (manage, sized to match the workspace name's own text height via `name_font.pointSize()` — both sizing tweaks came from user feedback after the first pass). New `Command::CurrentMode` in `tili-ipc` + `WmState::current_mode()` getter; daemon's `main.rs` read-only exclude-list (the thing that prevents a poll from self-triggering a wake loop) extracted into a tested `command_is_read_only()` helper. Rides the existing event-driven `WaitForChange` path — no polling added, updates in lockstep with mode changes.

## Release process (for next time)
Documented in `CONTRIBUTING.md`'s "Release engineering" section. Steps: (1) bump `[workspace.package] version` in `Cargo.toml`, (2) `cargo check --workspace` to refresh `Cargo.lock`, (3) move `CHANGELOG.md`'s `## [Unreleased]` content down into a new `## [X.Y.Z] - <date>` section (leaving `Unreleased` empty above), (4) commit as `🚀 release vX.Y.Z` (version/changelog files only — feature work gets its own earlier commit), (5) `git push origin main`, (6) `git tag -a vX.Y.Z -m vX.Y.Z && git push origin vX.Y.Z`. Pushing the tag triggers `.github/workflows/release.yml`: gate (fmt/clippy/test) → build+package per-target → publish GitHub release → `sync-homebrew-tap` job auto-updates both this repo's `Formula/tili.rb` template and the separate `itsdezen/homebrew-tap` repo (no manual formula edits needed). Repo owner push bypasses the branch-protection PR requirement (confirmed again this release, same as v0.6.1).

Previously: website **shipped and pushed to `main`** (2026-07-27) — `website/` Astro + Starlight site, built via herdr agent `tili-web`. Landing `/` (no-scroll, 100dvh + clamp(), monochrome theme matching dezen.me, dark/light toggle, Lucide-icon CTA row) + docs `/docs` (Configuration/Commands/Menu Bar rewritten from actual source code not README, Changelog page sourced from CHANGELOG.md, logo-only nav). Favicon/logo went through several iterations — final state: theme-adaptive favicon.svg (media-query fill swap, matches dezen.me exactly), transparent favicon.ico/96x96 PNG with no padding (glyph fills canvas edge-to-edge), apple-touch-icon/manifest PNGs intentionally left opaque per Apple/PWA convention. Deploy target: Cloudflare Workers (static assets, `wrangler.jsonc`) — **CI still needs the user to connect the repo in the Cloudflare dashboard** (root directory must be set to `website/` since this is a monorepo) and add `tili.dezen.me` as a custom domain; not something I can do without dashboard access.

2026-07-27: added thumbnail image — README header thumbnail (`assets/thumbnail.png`) and website og:image/twitter:image (`website/public/og-image.png`, was previously the 512x512 app logo). Also generated a GitHub social-preview-ready copy (1280x672, ~790KB, under GitHub's 1MB/1280x640 guidance) at `~/Desktop/tili-social-preview.png` for manual upload in repo Settings.

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
