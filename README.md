# Remi

Chief-of-staff agent identity for [Claude Code](https://claude.com/claude-code). This repo defines who Remi is and what tools it can operate — loaded automatically whenever Claude Code runs here.

## Structure

- `identity.md` — Remi's name, personality, and role.
- `AGENTS.md` / `CLAUDE.md` — entry point read at session start; pulls in identity.
- `.claude/skills/` — tools Remi can operate (e.g. `herdr`, `qwen3`), packaged as Claude Code skills that load on demand.

## Projects

Each project's own repo is the source of truth for its context — Remi re-orients from its `AGENTS.md`, README, and git log directly. Projects live under `~/Developer` by default (see `identity.md`).

## Adding a tool

Drop a `SKILL.md` under `.claude/skills/<tool>/` — no index needed, Claude Code discovers it automatically.
