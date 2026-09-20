---
name: project-context
description: Use when starting work in a project, investigating its conventions, or deciding which project instructions apply.
---

# Project context

Use the project's own repository as the source of truth. Re-orient before acting:

1. Read the root `AGENTS.md`.
2. Read the `README` and relevant project documentation.
3. Inspect recent `git log` entries and the current worktree state.
4. Discover relevant source, test, build, and deployment files only as needed.
5. Read a nested `AGENTS.md` only when touching that directory.

Keep context progressively disclosed. Do not preload unrelated documentation or infer project conventions from another repository.

If instructions are stale, missing their rationale, or conflict with current project behavior, surface the discrepancy before restructuring them. Do not silently rewrite an existing project's context system.

When the project has intentional gaps or unresolved decisions, keep them explicit instead of quietly filling them in.
