---
name: git-workflow
description: Use when inspecting repository changes, creating commits or branches, preparing a PR, or handling Git operations.
---

# Git workflow

Before committing, inspect `git status`, the complete diff, and `git log --oneline -10`. Stage only files belonging to the task and never commit secrets.

Use the repository's existing commit style. This repository uses Conventional Commits with these types: `feat`, `fix`, `docs`, `chore`, `refactor`, and `test`. Commit messages are concise and in English.

Keep branch names to at most three hyphen-separated words. PR titles use the same style as commits.

Confirm before push, force-push, branch deletion, reset, or other hard-to-reverse operations unless the repository's own standing instructions explicitly authorize the operation. Never use `git reset --hard` or `git checkout --` to discard work without explicit approval.

After changes, verify the final status and diff. Do not amend a commit unless explicitly requested.
