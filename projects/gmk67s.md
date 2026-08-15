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
Base build complete and pushed, LCD upload path now hardware-verified working: resolution confirmed 128×128 (see below), single-slot upload no longer silently blanks the other slot (fixed + confirmation gate added), image resize uses aspect-preserving center-crop. Unified `gmk67s <subcommand>` CLI, Ink TUI (`gmk67s tui`) with in-TUI file browser, device-verification safety gate before any protocol command. Not yet hardware-verified: the 48-byte config-buffer byte layout (lighting/time-sync fields) and the 36-frame GIF cap — see SPEC.md's verification checklist. No CI/tests yet.

## Key decisions / context
- Built by porting logic from two reference projects (`gmk87-node`, `gmk67sts` in `~/Developer/clones/`) rather than cloning either — spec/facts documented standalone in `SPEC.md`, no mention of the reference projects left in-repo per user's request.
- **Screen resolution confirmed 128×128** (1:1) — via real hardware test: uploaded a 4-quadrant+diagonal test pattern with a `GMK67S_DISPLAY_SIZE` env override and tried candidate sizes until the image rendered without diagonal shear. The earlier "180×180" guess (derived from pixel-count in the vendor's official Windows tool's default image data) was wrong — that data belonged to a generic white-label SDK demo profile, not this specific device. 128×128 also cleanly matches the firmware's 32KB frame-padding boundary (128×128×2 = 32768 bytes exactly, no padding needed), unlike 180×180 which needed padding — a good structural confirmation.
- Found and fixed a real data-loss bug via this same hardware test: uploading to only one LCD slot silently overwrote the other slot with a blank image, because the firmware writes both slots in one contiguous flash write and the protocol can't read back an existing image to preserve it. Now always confirmed (CLI prompt / TUI Yes-No screen) before it happens.
- Explicitly out of scope: custom key layout/VIA support, daemon/background mode, Electron GUI — none of these existed in either reference project.
- Safety principle: never auto-run tests that write/read the real device (see memory `feedback_hardware_test_isolation`) — protocol/logic tests must use mocked HID.
- Package versions kept current at build time rather than matching the older reference project's pins (`node-hid@^3.4.0`, `jimp@^1.6.1` — Jimp v1 API differs from the v0.x used upstream, `ink@^5.2.1`/`react@^18.3.1` — capped below ink@7 since that needs Node 22 and this project targets Node ≥18).

## Links
- Repo: https://github.com/itsdezen/gmk67s
- Reference projects (read-only, not dependencies): `~/Developer/clones/gmk87-node`, `~/Developer/clones/gmk67sts`
