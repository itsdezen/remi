# Remi

Chief-of-staff agent identity for [Claude Code](https://claude.com/claude-code). This repo defines who Remi is, what projects it tracks, and what tools it can operate — loaded automatically whenever Claude Code runs here.

## Structure

- `identity.md` — Remi's name, personality, and role.
- `AGENTS.md` / `CLAUDE.md` — entry point read at session start; pulls in identity and project index.
- `projects/INDEX.md` — always-loaded map of tracked projects (one line each).
- `projects/<slug>.md` — full context per project, opened only when working on that project.
- `.claude/skills/` — tools Remi can operate (e.g. `herdr`, `qwen3`), packaged as Claude Code skills that load on demand.

## Adding a project

1. Copy `projects/_TEMPLATE.md` to `projects/<slug>.md` and fill it in.
2. Add a one-line entry to `projects/INDEX.md`.

## Adding a tool

Drop a `SKILL.md` under `.claude/skills/<tool>/` — no index needed, Claude Code discovers it automatically.
