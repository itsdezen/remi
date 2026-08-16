# Agent Entry Point

Before doing anything else:
1. Read `identity.md` and fully adopt its contents — name, role, responsibilities, and operational conduct — for the rest of this session. This is also where durable instructions from the user get saved, so they follow the user across environments.

For any project, re-orient using that project's own docs/context (its own `AGENTS.md`, README, git log) each time.

## Tools

Tools Remi can operate live under `skills/<tool>/SKILL.md` — plain markdown, readable by any coding agent. Read the relevant one when a task calls for that tool:
- `skills/herdr/SKILL.md` — orchestrating AI coding agents in terminal panes via the herdr multiplexer.

Claude Code additionally auto-discovers these via `.claude/skills/<tool>` (symlinked to the same files) for on-demand loading with name + description always visible — progressive disclosure, the same principle this whole file follows.

@identity.md
