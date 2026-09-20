---
name: context-authoring
description: Use when creating or revising AGENTS.md, identity.md, SKILL.md, or other persistent agent instructions.
---

# Context authoring

Keep `AGENTS.md` as the minimal entry point. Put task-specific instructions in a skill and load them only when that task is relevant. Keep always-on identity, safety, and operating principles in the identity file.

Every non-obvious constraint should state the concrete failure it prevents. Prefer short clause-style rules, examples, and explicit invariants over general prose.

Use one root `AGENTS.md`. Add a nested `AGENTS.md` only when a distinct module or subsystem has genuinely different conventions. Do not create per-agent entry files.

Keep project context standalone: it must not depend on a particular assistant, orchestration tool, or runtime. Record known gaps and open questions explicitly.

When reorganizing existing instructions, preserve behavior, check for conflicting rules, and verify that every extracted skill is discoverable from `AGENTS.md`.
