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
Recent commits: synced Tili gaps config, added support for Tili's new config flag in the nvim integration, refreshed the nvim plugin lockfile.

## Key decisions / context
- Stow conflicts resolve automatically in `sync.sh` — dotfiles win, no backups kept.
- Unified Kanagawa Dragon theme across nvim/Ghostty/btop/lazygit.
- Claude Code runs inside nvim via claudecode.nvim (official IDE protocol); Ollama/qwen3 handles quick local edits and commit messages offline.
- Working tree clean, branch `main`, no open WIP as of 2026-07-26.


## Links
- Repo: https://github.com/itsdezen/dotfiles
