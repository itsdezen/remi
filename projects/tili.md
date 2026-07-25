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
Just shipped v0.6.1 (synced Formula/tili.rb sha256/version) and fixed a notch-inset bug (double-counting the existing menu-bar baseline). Daily-drivable already — tiling, workspaces, hot-reload config, hotkeys, floating rules, multi-monitor, menu bar badge.

## Key decisions / context
- Roadmap (`ROADMAP.md`, unscheduled backlog): third-party status bar integration (SketchyBar, via a `tili subscribe` push protocol or exec-hook), tabbed/stacked containers, sticky windows, native-tab support.
- Design invariants: public API only, event-driven not polling, `WindowFrameSetter` stays the single seam for window-frame mutation (animation plugs in there later without a rewrite).
- Working tree clean, branch `main`, no open WIP as of 2026-07-26.


## Links
- Repo: https://github.com/itsdezen/tili
