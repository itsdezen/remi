---
slug: tiktok-dj-club
status: active
path: /Users/itsdezen/Developer/tiktok-dj-club
updated: 2026-08-12
---

# tiktok-dj-club

## What it is
Three.js virtual DJ club, vertical (9:16) camera framing for TikTok Live. Viewers who join the live stream auto-spawn a matching avatar (gender auto-detected) into the club; they leave when the viewer leaves. A DJ character is always stationed at the booth. Viewers control their avatar via chat comments (numbered actions: jump, sway, etc.), shown in a guide panel top-right. Player actions (wings, spotlight, solo dance, ...) designed now; gifting-triggered premium unlocks are a later phase, not this build.

Stack: Vite (latest) + React + TypeScript, Three.js via `@react-three/fiber`/`@react-three/drei`, state via zustand. Club space design references two local clones for layout/lighting ideas (not code reuse — both are old vanilla-JS/global-script codebases, incompatible with an R3F component architecture): `Developer/clones/AMBRA_CLUB_THREEJS` (dance floor, spotlight rig) and `Developer/clones/Frogrey-Club` (UnrealBloomPass neon glow, audio-reactive lights, crowd-as-participants concept).

## Current focus
2026-08-12: Kicked off — delegated full scaffold + MVP build to a herdr sub-agent (pane split off Remi, `--kind claude`). Scope for first pass: Vite scaffold, vertical camera R3F canvas, club scene (floor/DJ booth/lighting inspired by the two clones), avatar spawn/despawn system driven by a mocked viewer-event feed (join/leave/comment), numbered chat-command → action mapping, top-right guide overlay. Real TikTok Live ingestion (viewer join/leave/comment events) has no public API — TikTok Live data is normally pulled via an unofficial library (e.g. `tiktok-live-connector`) on a small backend relaying over WebSocket to the browser. Sub-agent was told to build a pluggable event-source interface with a demo/mock feed now, not to fake a real TikTok integration.

## Key decisions / context
- Gender auto-detection has no real signal available client-side (no public TikTok profile/gender API) — sub-agent was told to build a pluggable `resolveGender(viewer)` interface with a placeholder heuristic (hash-based/round-robin), swappable later for a real classifier.
- Premium/gift-triggered unlocks are explicitly a later phase — this build only stubs the hook point (an action-trigger function actions can be gated behind later), no payment/gift integration.
- Project lives outside the `remi` repo at `/Users/itsdezen/Developer/tiktok-dj-club` (own git history, not managed by Remi's identity.md auto-push rule).

## Links
