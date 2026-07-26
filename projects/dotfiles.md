---
slug: dotfiles
status: active
path: /Users/itsdezen/Developer/dotfiles
updated: 2026-07-26
---

# dotfiles

## What it is
Personal macOS dev environment, managed with GNU Stow. Covers zsh+zinit+Starship, mise (node/bun/pnpm/python/uv/rust/go), Neovim (LazyVim), Ghostty, herdr, Tili, Ollama (local qwen3:8b), gh CLI, lazygit, superfile, btop. One idempotent `sync.sh` drives install/bootstrap/uninstall.

## Current focus
Migrated the entire color scheme from Kanagawa Dragon to Nord (2026-07-26), old theme fully removed. Committed and pushed (0499370).

## Key decisions / context
- Stow conflicts resolve automatically in `sync.sh` — dotfiles win, no backups kept.
- Unified Nord theme across nvim (gbprod/nord.nvim), Ghostty (built-in `theme = Nord`), btop (built-in `nord.theme`), lazygit (hand-authored hex, no by-name support), starship (hand-authored `[palettes.nord]`), superfile (hand-authored `theme/nord.toml`, chroma has a native `nord` syntax style), herdr (built-in `name = "nord"`).
- Claude Code runs inside nvim via claudecode.nvim (official IDE protocol); Ollama/qwen3 handles quick local edits and commit messages offline.
- Working tree clean, branch `main`, no open WIP as of 2026-07-26.


## Links
- Repo: https://github.com/itsdezen/dotfiles
