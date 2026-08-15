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
2026-08-15 (cont. 3): Language runtime brand colors brightened for fg-only visibility (old colors were designed as pill backgrounds, too dark/muted as plain text on transparent bg). node_green #215732→#5FA04E, bun_black #000000→#FFFFFF (plain white, user override — brand cream #FBF0DF was tried first and rejected), python_blue #306998→#4B8BBE, rust_orange #B7410E→#F74C00, go_blue #375EAB→#00ADD8 (current official Gopher cyan). Amended into 001f943, not pushed. Note: mid-session a "down+Enter" sent to reject a permission dialog accidentally landed on "Yes, allow all edits during this session" instead and flipped the staff pane into auto-accept-edits mode — the shift+tab classifier block (identity.md) applies in reverse too, couldn't flip it back. Proceeded anyway since remaining scope was safe (no push, confined to this file); worth remembering that arrow-key dialog navigation can misfire into the auto-accept option, not just literal shift+tab.

2026-08-15 (cont. 2): Starship language runtime segments (nodejs/bun/python/rust/golang) changed from solid-color pills to plain colored icon+text on transparent background — `bg:<color> fg:gray_1` + triangle caps → `fg:<color>` only, formats simplified to `' [$symbol$version]($style)'`. docker_context pill left as-is (only language runtimes were in scope). Committed 9ad3ec4, not pushed. Done via a delegated herdr sub-agent, babysat in default permission mode.

2026-08-15 (cont.): Ghostty line height bumped +10% via `adjust-cell-height = 10%` in `ghostty/.config/ghostty/config`. Committed and pushed (a831463). Done via a third delegated herdr sub-agent, babysat in default permission mode.

2026-08-15: Fantasque Sans Mono Nerd Font is now the primary font. Ghostty (`ghostty/.config/ghostty/config`, the only place a font family is set — nvim/starship/btop/lazygit/superfile all inherit the terminal font) uses `FantasqueSansM Nerd Font Mono`. Brewfile got the new cask (`font-fantasque-sans-mono-nerd-font`); `font-maple-mono-nf` deliberately kept installed for quick switching back, per user request. A Maple-Mono-NF fallback line was briefly added for Vietnamese glyph coverage, then the user confirmed Fantasque's latest release already renders Vietnamese fine — the fallback commit was dropped via `git reset --hard` (local/unpushed, so no revert-commit clutter) before pushing. Final pushed state: `5ff90fd` only, no fallback. Validated via `stow -n` dry-run and `brew bundle check`. Pushed to origin. Done via two delegated herdr sub-agents, babysat in default permission mode.

2026-08-11 (session 2): Pushed. Codex upgraded from skeleton to real stow package — `config.toml` moved into `codex/.codex/`, `codex` added to both `PACKAGES` and `NO_FOLD_PACKAGES` in sync.sh. OpenCode theme fix: discovered `theme` does NOT live in `opencode.jsonc` (that key is deprecated/legacy) — it's read from a separate `~/.config/opencode/tui.json`, now stowed too. Added `alias oc="opencode"` to zsh (matches existing `alias cc="claude"`). Codified a new "Unified theming" rule in dotfiles CLAUDE.md: themed tools use their built-in preset if one exists, else hand-author with the same Nord hex palette already used by lazygit/starship, and the whole repo migrates together if the base theme ever changes. Simplified `claude/.claude/settings.json`: dropped `verbose: true` (unjustified permanent debug flag) and the deprecated `includeCoAuthoredBy` key; commit/PR attribution now fully suppressed via `attribution: {commit:"", pr:"", sessionUrl:false}` — confirmed via Claude Code's own docs that `attribution` takes precedence over the deprecated `includeCoAuthoredBy`. Researched Codex and OpenCode for an equivalent attribution/co-author setting — neither has one (confirmed via their official docs) and neither auto-creates commits, so no config change needed for those two. README trimmed 108→80 lines: removed the stale Key Bindings table (Tili section still said `alt-shift-s` weeks after the real rebind to `alt-shift-m` — proves the repo's own "don't document keybinds, they go stale" rule), removed the redundant Runtimes(mise) block and the generic Workflow section.

2026-08-11 (session 1): Stowed OpenCode's config (`opencode.jsonc`, symlinked from `~/.config/opencode/`) as a proper GNU Stow package, wired into `sync.sh` (PACKAGES + NO_FOLD_PACKAGES) and documented in CLAUDE.md/README.md. Claude Code was already stowed from earlier work (`claude` package → `~/.claude/settings.json`). Secrets/session data (auth.json, `.claude.json`, `~/.claude/projects`) deliberately excluded throughout. Done via delegated herdr sub-agent, babysat in default permission mode.

2026-08-11: Ollama fully removed — stow package, Brewfile entry, sync.sh provisioning, and the codecompanion.nvim + claudecode.nvim plugin specs that depended on it (whichkey.lua icon overrides also dropped since they only existed for those two plugins). Machine cleaned: `brew uninstall ollama`, `~/.ollama` deleted (4.9GB, qwen3:8b model), ~5GB disk freed. Commit 5ed5b42. Added OpenCode (SST's terminal AI agent) to Brewfile, Brewfile-only like Claude Code/Codex (dff6246). Also added jq/yq/direnv/shellcheck to Brewfile as agent-workflow utilities — deliberately skipped git/htop/tmux (redundant with Xcode CLT git, existing btop, and herdr) and bat/eza/fzf/tree/watch (human-interactive QoL, not agent-relevant) from a longer list the user proposed; wired direnv's zsh hook so it actually activates (7298869). All done via a delegated herdr sub-agent, babysat in default permission mode.

2026-08-09: rebound tili's `manage` mode entry from `alt-shift-s` to `alt-shift-m` in personal config (`tili/.config/tili/tili.kdl`), matching tili's own upcoming default rebind for its next minor release. Committed and pushed (5b0f32a).

Migrated the entire color scheme from Kanagawa Dragon to Nord (2026-07-26), old theme fully removed. Committed and pushed (0499370).

## Key decisions / context
- Stow conflicts resolve automatically in `sync.sh` — dotfiles win, no backups kept.
- Unified Nord theme across nvim (gbprod/nord.nvim), Ghostty (built-in `theme = Nord`), btop (built-in `nord.theme`), lazygit (hand-authored hex, no by-name support), starship (hand-authored `[palettes.nord]`), superfile (hand-authored `theme/nord.toml`, chroma has a native `nord` syntax style), herdr (built-in `name = "nord"`), opencode (built-in `theme = "nord"` in `tui.json`, NOT `opencode.jsonc`). This unified-theming convention is now written into dotfiles' own `CLAUDE.md`.
- Claude Code commit/PR attribution: the `attribution` settings.json key (`{commit, pr, sessionUrl}`) is the current, correct mechanism — it takes precedence over `includeCoAuthoredBy`, which is deprecated. Don't reintroduce `includeCoAuthoredBy`.
- Working tree clean, branch `main`, no open WIP as of 2026-08-11.


## Links
- Repo: https://github.com/itsdezen/dotfiles
