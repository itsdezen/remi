---
slug: itsdezen
status: active
path: /Users/itsdezen/Developer/itsdezen
updated: 2026-07-26
---

# itsdezen

## What it is
GitHub profile README repo ("Hi, It's Dezen"). `build_readme.py` (Python) + a GitHub Actions workflow auto-fill follower/star/fork counts and latest release lines into README.md on a schedule.

## Current focus
Just fixed the auto-build workflow: markdown links were rendering as raw text because an HTML block was swallowing the markdown, and the workflow needed `contents:write` permission to push its own README updates.

## Key decisions / context
- README content between `<!-- github_stats -->` / `<!-- recent_releases -->` markers is machine-generated — don't hand-edit those blocks.
- Working tree clean, branch `main`, no open WIP as of 2026-07-26.


## Links
- Repo: https://github.com/itsdezen/itsdezen
