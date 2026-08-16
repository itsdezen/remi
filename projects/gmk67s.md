---
slug: gmk67s
status: active
path: /Users/itsdezen/Developer/gmk67s
updated: 2026-08-16
---

# gmk67s

## What it is
Node.js CLI (+ Ink TUI) for the ZUOYA GMK67-S keyboard: LCD image/GIF upload, RGB underglow + LED config, lighting presets, time sync, config dump, factory-default restore. Talks to the keyboard over its vendor HID protocol (VID `0x320f`/PID `0x5055`, verified directly on real hardware via `ioreg`). Repo: https://github.com/itsdezen/gmk67s (public, pushed).

## Current focus
LCD upload path hardware-verified working end-to-end, including a full UX redesign confirmed on real hardware. Resolution confirmed 128×128 (see below). Upload API/CLI/TUI reworked to drop the "slot" concept entirely — `gmk67s upload <file1> [file2]`: 1 file uses the full 72-frame budget and there is no second image at all (matches vendor app behavior); 2 files split the budget (up to 36 each, remainder shifts to whichever is smaller). No more "this will erase the other slot" confirmation — a full image-memory refresh is now the expected default, not a surprising side effect. Unified `gmk67s <subcommand>` CLI, Ink TUI (`gmk67s tui`) with in-TUI file browser. Not yet hardware-verified: the 48-byte config-buffer byte layout (lighting/time-sync fields), and the new 72-frame budget itself — see SPEC.md's verification checklist. No CI/tests yet.

Refactored to TypeScript + Bun (2026-08-15, staff-executed, committed & pushed `6c07a87`). All `src/**/*.js` → `.ts`/`.tsx` (tui converted to real JSX now that Bun runs it natively, replacing the old `React.createElement`-only style). Package manager switched npm → Bun (no pnpm — user redirected mid-task); no build/dist step, `bin`/`exports`/scripts point straight at `.ts` sources with `#!/usr/bin/env bun` shebangs. `bun run typecheck` (`tsc --noEmit`) passes clean. Non-hardware CLI paths smoke-tested (`--help`, `loadPreset --list`); upload/diagnostic/timesync/restore-factory/tui not exercised since they touch real HID.

## Key decisions / context
- Built by porting logic from two reference projects (`gmk87-node`, `gmk67sts` in `~/Developer/clones/`) rather than cloning either — spec/facts documented standalone in `SPEC.md`, no mention of the reference projects left in-repo per user's request.
- **Screen resolution confirmed 128×128** (1:1) — via real hardware test: uploaded a 4-quadrant+diagonal test pattern with a `GMK67S_DISPLAY_SIZE` env override and tried candidate sizes until the image rendered without diagonal shear. The earlier "180×180" guess (derived from pixel-count in the vendor's official Windows tool's default image data) was wrong — that data belonged to a generic white-label SDK demo profile, not this specific device. 128×128 also cleanly matches the firmware's 32KB frame-padding boundary (128×128×2 = 32768 bytes exactly, no padding needed), unlike 180×180 which needed padding — a good structural confirmation.
- Found a real data-loss bug via this same hardware test: uploading to only one LCD slot silently overwrote the other slot with a blank image (firmware writes both in one contiguous write, protocol can't read back an existing image). Initially patched with a confirmation prompt, then the user redesigned the whole upload model around this constraint instead: every upload is a full refresh, 1 or 2 images chosen up front, no persistent "slot" the user manages — matches how the vendor's own app behaves.
- Offline dry-run tooling (image resize/crop/RGB565-encode pipeline tested without any device access, using the project's own production functions) proved valuable twice: caught nothing wrong with the image pipeline itself when diagnosing the "blurry image" report (correctly pointed the investigation at resolution/protocol instead), and verified the frame-budget math for the upload redesign before ever touching hardware.
- Explicitly out of scope: custom key layout/VIA support, daemon/background mode, Electron GUI — none of these existed in either reference project.
- Safety principle: never auto-run tests that write/read the real device (see memory `feedback_hardware_test_isolation`) — protocol/logic tests must use mocked HID.
- Package versions kept current at build time rather than matching the older reference project's pins (`node-hid@^3.4.0`, `jimp@^1.6.1` — Jimp v1 API differs from the v0.x used upstream, `ink@^5.2.1`/`react@^18.3.1` — capped below ink@7 since that needs Node 22 and this project targets Node ≥18).

## Frame budget raised 36 → 72 (2026-08-16, staff-executed, committed & pushed `111552e`)
User wanted to push GIF upload past the assumed 36-frame limit. Investigated the vendor's official Windows app (`Image Custom Tool`, moved from `~/Downloads` to `~/Developer/Image Custom Tool` for inspection) first — it's a generic/white-label multi-device tool, not gmk67s-specific (bundled `DefaultData/Keyboard.json` has VID `3141`/PID `4103`, not gmk67s's real VID `0x320f`/PID `0x5055`), and contained no extractable frame-count constant in any data/string-table file (checked ASCII + UTF-16LE) — a cap, if any, is compiled into the `.exe` logic, not exposed statically.
- Real answer came from the user directly: this tool descends from a GMK87 tool (240×135 screen, confirmed-working 36-frame budget), and GMK87/GMK67-S share the same flash/image-memory region. Math: GMK87's frame = 240×135×2 (RGB565) = 64,800 bytes, padded to 65,536 bytes/frame (2×32KB blocks) → 36×65,536 = 2,359,296 bytes total budget. GMK67-S's frame = 128×128×2 = 32,768 bytes exactly (1×32KB block, no padding — already hardware-confirmed). Same total budget ÷ 32,768 = **72 frames** — exactly double, since each GMK67-S frame costs half what a GMK87 frame does.
- This also explains an inconsistency flagged earlier: SPEC.md's "65536 bytes padded frame size" was GMK87's figure, carried over unchanged during the original porting rather than recalculated for this device's actual 32×32KB-block frame size.
- Implemented via a herdr staff session (`gmk67s-frame-limit` pane): `device.ts`'s enforced cap and `sendImageMagick.ts`'s `MAX_TOTAL_FRAMES` constant both 36→72 (the 1-image/2-image split logic already derived from the constant, so the 18/18 split became 36/36 automatically, no separate hardcoded value to fix); SPEC.md's frame-size and limit tables corrected and re-derived; README/api.ts doc comments updated. `bun run typecheck` clean.
- **Still not hardware-verified** — 72 is a calculated value from shared-flash reasoning, not confirmed by an actual 72-frame upload to physical GMK67-S hardware. Per [[feedback_hardware_test_isolation]], that test needs to be run deliberately (ask first) rather than automatically.

## Links
- Repo: https://github.com/itsdezen/gmk67s
- Reference projects (read-only, not dependencies): `~/Developer/clones/gmk87-node`, `~/Developer/clones/gmk67sts`
