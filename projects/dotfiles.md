---
slug: dotfiles
status: active
path: /Users/itsdezen/Developer/dotfiles
updated: 2026-07-26
---

# dotfiles

## What it is
Personal macOS dev environment, managed with GNU Stow. Covers zsh+zinit+Starship, mise (node/bun/pnpm/python/uv/rust/go), Neovim (LazyVim), Ghostty, herdr, Tili, OpenCode, gh CLI, lazygit, superfile, btop. One idempotent `sync.sh` drives install/bootstrap/uninstall.

## Current focus
2026-08-11: Stowed OpenCode's config (`opencode.jsonc`, symlinked from `~/.config/opencode/`) as a proper GNU Stow package, wired into `sync.sh` (PACKAGES + NO_FOLD_PACKAGES) and documented in CLAUDE.md/README.md. Codex got only an empty skeleton package (`codex/.codex/.gitkeep`) — its real `~/.codex/config.toml` held nothing but auto-generated project-trust/hook-hash state, no portable config worth version-controlling yet. Claude Code was already stowed from earlier work (`claude` package → `~/.claude/settings.json`), so nothing new needed there. Secrets/session data (auth.json, `.claude.json`, `~/.claude/projects`) deliberately excluded. Committed locally (c23cc6a), **not pushed** — 1 commit ahead of origin, awaiting user go-ahead. Done via delegated herdr sub-agent, babysat in default permission mode.

2026-08-11: Ollama fully removed — stow package, Brewfile entry, sync.sh provisioning, and the codecompanion.nvim + claudecode.nvim plugin specs that depended on it (whichkey.lua icon overrides also dropped since they only existed for those two plugins). Machine cleaned: `brew uninstall ollama`, `~/.ollama` deleted (4.9GB, qwen3:8b model), ~5GB disk freed. Commit 5ed5b42. Added OpenCode (SST's terminal AI agent) to Brewfile, Brewfile-only like Claude Code/Codex (dff6246). Also added jq/yq/direnv/shellcheck to Brewfile as agent-workflow utilities — deliberately skipped git/htop/tmux (redundant with Xcode CLT git, existing btop, and herdr) and bat/eza/fzf/tree/watch (human-interactive QoL, not agent-relevant) from a longer list the user proposed; wired direnv's zsh hook so it actually activates (7298869). All done via a delegated herdr sub-agent, babysat in default permission mode.

2026-08-09: rebound tili's `manage` mode entry from `alt-shift-s` to `alt-shift-m` in personal config (`tili/.config/tili/tili.kdl`), matching tili's own upcoming default rebind for its next minor release. Committed and pushed (5b0f32a).

Migrated the entire color scheme from Kanagawa Dragon to Nord (2026-07-26), old theme fully removed. Committed and pushed (0499370).

## Key decisions / context
- Stow conflicts resolve automatically in `sync.sh` — dotfiles win, no backups kept.
- Unified Nord theme across nvim (gbprod/nord.nvim), Ghostty (built-in `theme = Nord`), btop (built-in `nord.theme`), lazygit (hand-authored hex, no by-name support), starship (hand-authored `[palettes.nord]`), superfile (hand-authored `theme/nord.toml`, chroma has a native `nord` syntax style), herdr (built-in `name = "nord"`).
- Working tree clean, branch `main`, no open WIP as of 2026-08-11.


## Links
- Repo: https://github.com/itsdezen/dotfiles
