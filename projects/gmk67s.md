---
slug: gmk67s
status: active
path: /Users/itsdezen/Developer/gmk67s
updated: 2026-08-15
---

# gmk67s

## What it is
Node.js CLI (+ Ink TUI) for the ZUOYA GMK67-S keyboard: LCD image/GIF upload, RGB underglow + LED config, lighting presets, time sync, config dump, factory-default restore. Talks to the keyboard over its vendor HID protocol (VID `0x320f`/PID `0x5055`, verified directly on real hardware via `ioreg`). Repo: https://github.com/itsdezen/gmk67s (public, pushed).

## Current focus
Base build complete and pushed: unified `gmk67s <subcommand>` CLI, Ink TUI (`gmk67s tui`), device-verification safety gate before any protocol command, confirmation prompts for factory-restore/image-overwrite, config-write rollback on failure. Not yet done: end-to-end hardware verification of the LCD upload path (protocol + 180×180 resolution assumption) and of the config-buffer byte layout — see SPEC.md's verification checklist. No CI/tests yet.

## Key decisions / context
- Built by porting logic from two reference projects (`gmk87-node`, `gmk67sts` in `~/Developer/clones/`) rather than cloning either — spec/facts documented standalone in `SPEC.md`, no mention of the reference projects left in-repo per user's request.
- Screen resolution locked at 180×180 (1:1) — derived from pixel-count in the vendor's official Windows tool's default image data (`~/Downloads/Image Custom Tool/DefaultData/e_ScreenCustom*.json`), user explicitly confirmed this as settled fact, not a placeholder.
- Explicitly out of scope: custom key layout/VIA support, daemon/background mode, Electron GUI — none of these existed in either reference project.
- Safety principle: never auto-run tests that write/read the real device (see memory `feedback_hardware_test_isolation`) — protocol/logic tests must use mocked HID.
- Package versions kept current at build time rather than matching the older reference project's pins (`node-hid@^3.4.0`, `jimp@^1.6.1` — Jimp v1 API differs from the v0.x used upstream, `ink@^5.2.1`/`react@^18.3.1` — capped below ink@7 since that needs Node 22 and this project targets Node ≥18).

## Links
- Repo: https://github.com/itsdezen/gmk67s
- Reference projects (read-only, not dependencies): `~/Developer/clones/gmk87-node`, `~/Developer/clones/gmk67sts`
