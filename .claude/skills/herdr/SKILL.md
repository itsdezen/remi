---
name: herdr
description: Use when checking on, launching, prompting, or shutting down AI coding agents running in terminal panes/workspaces via the herdr multiplexer, or when the user mentions "herdr", other running agents, or wants a dedicated pane/workspace for a project. Not for editing code — only for orchestrating agent panes.
---

# herdr

Terminal workspace multiplexer for running and orchestrating multiple AI coding agent CLIs (Claude Code, Codex, Gemini, Cursor, OpenCode, etc.) side by side in one terminal — locally or over SSH. Home: https://herdr.dev. CLI: `herdr`.

**herdr agents, not Claude Code subagents.** "Spawn/manage an agent" always means a real `herdr agent start` process in its own pane — never the Agent/Task tool. A Task subagent is ephemeral, lives only inside this session's context window, and doesn't exist as far as `herdr` is concerned; it can't be listed, prompted later, or checked on after this session ends. If the user wants an agent running work in parallel that they can check on, hand off to, or that should outlive this conversation, it must go through `herdr agent start`, not Agent/Task.

## Preflight — always do this first
`herdr status` — if `server: not running`, no session is up yet. Starting one (`herdr` or `herdr --session <name>`) launches a persistent background daemon that outlives this shell. Confirm with the user before starting a new session, unless they've already asked you to work inside herdr.

## Concepts
- Hierarchy: **workspace** → **tab** → **pane** → **agent**. A pane is a terminal; an agent is a supported CLI detected running inside a pane.
- Status rolls up: every pane/agent has `agent_status` (`idle | working | blocked | done | unknown`); tabs and workspaces report the same field as a rollup of everything inside them. Checking a workspace tells you if *anything* inside needs attention without opening a single pane.
- Targets: `agent`/`pane` commands take a `<target>` — a pane_id or agent name from `list`/`get`. Never hardcode one; look it up first, IDs are per-session.

## Orchestration workflow — survey → narrow → inspect → act
Go narrower only as needed, and stop as soon as you have the answer:

1. **Survey** (cheapest): `herdr workspace list` — one line per workspace with rolled-up `agent_status`. If everything reads idle/working and nothing needs you, stop here.
2. **Narrow**: `herdr tab list --workspace <id>` or `herdr agent list` — find exactly which tab/pane/agent is `blocked` or `done`.
3. **Inspect**: `herdr agent get <target>` for metadata, or `herdr agent read <target> --lines 40` for recent output — read only the one agent that needs attention, not everyone's.
4. **Act**: `herdr agent prompt <target> "<instruction>" --wait --until done` to send input and block until it settles, or `herdr agent wait <target> --until blocked done` to poll without sending anything.

Never run `herdr api snapshot` as a first move — it dumps the entire session state (every workspace/tab/pane/agent) in one shot and defeats the point of going step by step. Only reach for it if you genuinely need the full picture at once.

## Choosing a model when spawning an agent
Both major providers organize models into the same three tiers — match the tier to the task, not the provider brand. Sources: Anthropic's model catalog (`shared/models.md` in the `claude-api` skill) and OpenAI's Codex docs (developers.openai.com/codex/models, developers.openai.com/api/docs/guides/model-selection).

| Tier | `--kind claude` | `--kind codex` | Use for |
|---|---|---|---|
| Flagship / deep reasoning | Opus 4.8 (`claude-opus-4-8`) — highly autonomous, state-of-the-art long-horizon agentic work; Fable 5 (`claude-fable-5`) for the hardest long-horizon work if the user explicitly wants max capability | GPT-5.6 Sol — "strongest capability for complex coding, computer use, research" | Long autonomous coding runs, architecture/planning, ambiguous or high-stakes work, deep debugging |
| Balanced everyday | Sonnet 5 (`claude-sonnet-5`) — near-Opus quality on coding/agentic at Sonnet cost | GPT-5.6 Terra — "the everyday workhorse" | Interactive quick edits, day-to-day coding, most tasks — the default choice |
| Fast / cheap | Haiku 4.5 (`claude-haiku-4-5`) | GPT-5.6 Luna — "strong capability at the lowest cost" | High-volume simple tasks: classification, extraction, quick lookups, structured repeatable work |

- **Model catalogs move fast** (new generations every few months) — don't hardcode a model name from memory. For Claude, check `shared/models.md` in the `claude-api` skill. For Codex, run `codex debug models` or check developers.openai.com/codex/models.
- **Passing the model through `herdr agent start`**: the trailing `-- [AGENT_ARG]...` forwards args to the underlying CLI, e.g. `herdr agent start work1 --kind claude --pane p1 -- --model claude-opus-4-8` or `... --kind codex --pane p1 -- --model gpt-5.6-sol`. Both CLIs also support switching mid-session (`/model`) once attached.
- Default to the balanced tier unless the task clearly calls for more depth or is simple/high-volume — same "start simple, escalate only when needed" principle as everywhere else in this guide.

## Command cheat sheet

Discover:
- `herdr workspace list` / `herdr tab list [--workspace ID]` / `herdr pane list [--workspace ID]` / `herdr agent list`
- `herdr agent get <target>` · `herdr pane get <target>` · `herdr agent explain [target]` (why an agent was/wasn't detected)

Create:
- `herdr workspace create [--cwd PATH] [--label TEXT]`
- `herdr tab create [--workspace ID] [--cwd PATH] [--label TEXT]`
- `herdr pane split [--pane ID] [--direction right|down] [--cwd PATH]`
- `herdr agent start <NAME> --kind <KIND> --pane <ID>` — pane must already be sitting at an interactive shell prompt
- `herdr worktree create --branch NAME [--base REF] [--workspace ID]` — new git worktree in its own workspace

Control an agent:
- `herdr agent prompt <target> "<text>" [--wait] [--until STATUS...] [--timeout MS]`
- `herdr agent wait <target> [--until STATUS...] [--timeout MS]`
- `herdr agent send-keys <target> <keys...>` — raw keypresses, for things `prompt` can't express (e.g. approving a y/n)
- `herdr agent focus <target>` / `herdr agent rename <target> <name>`

Teardown:
- `herdr pane close <pane_id>` · `herdr tab close <tab_id>` · `herdr workspace close <workspace_id>`
- Closing kills the process inside. Confirm before closing a pane/tab/workspace whose agent is mid-turn (`agent_status: working`).

## Notes
- `list`/`get` commands print human-readable text by default — there's no blanket `--json` flag on `list`. `herdr api snapshot --output PATH` is the one place to get the full JSON tree if truly needed.
- Supported `--kind` values for `agent start` (v0.7.5): pi, claude, codex, gemini, cursor, devin, agy, cline, omp, mastracode, opencode, copilot, kimi, kiro, droid, amp, grok, hermes, kilo, qodercli, maki.
