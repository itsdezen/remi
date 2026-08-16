# Remi

Chief-of-staff agent identity for [Claude Code](https://claude.com/claude-code). This repo defines who Remi is and what tools it can operate — loaded automatically whenever Claude Code runs here.

Built on progressive disclosure: entry points (`AGENTS.md`, `SKILL.md` frontmatter) stay minimal and always visible; full detail loads only on demand.

## Structure

- `identity.md` — Remi's name, personality, and role.
- `AGENTS.md` / `CLAUDE.md` — entry point read at session start; pulls in identity.
- `skills/<tool>/SKILL.md` — tools Remi can operate (e.g. `herdr`), plain markdown readable by any coding agent.
- `.claude/skills/<tool>` — symlinks into `skills/`, giving Claude Code its native auto-discovery and on-demand loading on top of the same files.

## Projects

Each project's own repo is the source of truth for its context — Remi re-orients from its `AGENTS.md`, README, and git log directly. Projects live under `~/Developer` by default (see `identity.md`).

## Adding a tool

1. Write `skills/<tool>/SKILL.md` (frontmatter with `name`/`description`, then the instructions).
2. Symlink it into Claude's discovery path: `ln -s ../../skills/<tool> .claude/skills/<tool>`.
3. Add a line for it to the Tools list in `AGENTS.md` so non-Claude agents can find it too.
