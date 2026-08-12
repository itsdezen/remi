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
2026-08-12: **MVP built and committed** (`73573d9`) by a herdr sub-agent, verified (`tsc -b`, `npm run build`, `oxlint` all clean) and confirmed independently before closing the pane. Stack: Vite + React 19 + TypeScript, `@react-three/fiber`/`drei`/`postprocessing`, zustand. Vertical 9:16 canvas (`App.css`).

What exists: `src/scene/` (floor, elevated DJ booth, moving lights + sweeping spotlights, laser beams, disco accent, UnrealBloom-style post-processing, fake BPM pulse clock — drawing on `AMBRA_CLUB_THREEJS`'s booth/floor layout and `Frogrey-Club`'s bloom/light-rig approach) · `src/avatars/` (primitive low-poly humanoid, male/female palette variants, procedural animation state machine — idle-dance always on plus jump/sway/wave/spin/fistpump/watching/solo — permanent never-despawning DJ avatar, slot allocator so crowd avatars don't overlap) · `src/liveEvents/` (`LiveEventSource` interface + `MockLiveEventSource` simulating join/leave/numbered-comment traffic) · `src/gender/resolveGender.ts` (hash-based placeholder) · `src/actions/triggerAction.ts` (single chokepoint every action flows through, deliberately centralized for a future gift-gate wrapper) · `src/ui/GuidePanel.tsx` (top-right command legend). Commands 1–8: jump/sway/wave/spin/fistpump/wings/spotlight/solo. `README.md` has run instructions + a Roadmap section for what's explicitly out of scope this pass.

Not done this pass (by design, see README Roadmap): real TikTok Live ingestion (no public API — normal approach is an unofficial lib like `tiktok-live-connector` on a small Node backend relaying over WebSocket), real rigged GLTF avatar models (currently primitive geometry), gifting/premium gate on `triggerAction`, and a smarter gender signal. Visual in-browser smoke test wasn't completed (claude-in-chrome wasn't connected in the sub-agent's pane; declined installing Playwright+Chromium as a heavier substitute — build/lint/typecheck passing was judged sufficient for this pass). Worth an actual `npm run dev` + eyeball pass next session.

## Key decisions / context
- Gender auto-detection has no real signal available client-side (no public TikTok profile/gender API) — sub-agent was told to build a pluggable `resolveGender(viewer)` interface with a placeholder heuristic (hash-based/round-robin), swappable later for a real classifier.
- Premium/gift-triggered unlocks are explicitly a later phase — this build only stubs the hook point (an action-trigger function actions can be gated behind later), no payment/gift integration.
- Project lives outside the `remi` repo at `/Users/itsdezen/Developer/tiktok-dj-club` (own git history, not managed by Remi's identity.md auto-push rule).

## Links
