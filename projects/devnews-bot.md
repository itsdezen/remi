---
slug: devnews-bot
status: decommissioned
path: /Users/itsdezen/Developer/devnews-bot
updated: 2026-08-10
---

# devnews-bot

## DECOMMISSIONED (2026-08-10)
User decided to kill this project too, same session as fx.dezen.me's teardown ("tiện thể dọn luôn dự án devnewsbot"). Confirmed via explicit question: full destroy, same treatment as fx.dezen.me. Infra torn down via a dedicated herdr sub-agent (`devnews-teardown`), no backup taken.

**Done**: D1 database `devnews` (id `5bdfc845-6d83-4206-937c-28c4bd7f3693`) deleted, cross-checked against `wrangler.toml` first. Worker `devnews-bot` deleted — verified via `wrangler deployments list` erroring "does not exist" and `wrangler d1 list` coming back empty.

**Not automatable, handed to the user**:
- GitHub repo `github.com/itsdezen/devnews-bot` (private) and local clone at `/Users/itsdezen/Developer/devnews-bot` — `gh repo delete` failed (token missing `delete_repo` scope, same blocker hit on fx.dezen.me), and the user said they'd finish GitHub + bot cleanup themselves. **Don't assume the repo/local dir are gone without checking.**
- Telegram bot itself — the real `TELEGRAM_BOT_TOKEN` was only ever stored as a Cloudflare Worker secret (write-only, unreadable even before the Worker was deleted); the repo's `.dev.vars` only had a placeholder. Couldn't call `deleteWebhook`. User needs to go through @BotFather (`/revoke` or delete the bot) to fully deactivate it — left to the user to do themselves.

Everything below is historical context from when the project was active/live.

## What it is
Developer news aggregator Telegram bot, running entirely on Cloudflare Workers. No AI — deterministic fetch/filter/deliver pipeline (GitHub Trending/Releases, RSS, Hacker News → normalise → dedupe → filter by user prefs → notify/digest). No web dashboard; all config happens via Telegram commands + inline keyboards. Full spec at `SPEC.md` in the project root.

## Current focus
**Live in production, fully verified (2026-07-29).** Built by dedicated herdr sub-agent `devnews-builder` (Opus): 12 commits, 39/39 tests pass, typecheck clean, verified locally first. Deployed via Cloudflare dashboard git integration (Workers Builds connected to `main`, no local `wrangler deploy` — user's stated preference) rather than CLI. D1 database `devnews` created via dashboard (id `5bdfc845-6d83-4206-937c-28c4bd7f3693`, wired into `wrangler.toml`), secrets (`TELEGRAM_BOT_TOKEN`, `TELEGRAM_WEBHOOK_SECRET`) set via dashboard, webhook registered via `/setup?secret=...`. End-to-end confirmed: `/start` on Telegram → webhook → D1 write, all 8 tables present and a real user row confirmed in D1 Console.
- **Gotcha hit during setup**: pasting the full `schema.sql` (with `--` comment lines) into the D1 dashboard Console threw `SQLITE_ERROR: incomplete input` — root cause suspected to be the Console mishandling multi-line `--` comments (likely swallowing the rest of the paste as one comment). Fix: strip all comment lines before pasting, execute comment-free SQL only. Worth remembering for any future D1 Console paste in this or other projects.

## Key decisions / context
- Stack: TypeScript, Cloudflare Workers, D1 (SQLite) for all persistence incl. conversation state (expires_at column), raw Telegram Bot API via a thin swappable `TelegramClient`, hand-rolled RSS/Atom parser (Workers has no DOMParser), Vitest.
- Clean layering (dependency arrows point inward): core (Article model, logger, http) → sources (NewsSource plugin interface + registry) → pipeline (fetch/normalise/dedupe/categorise/plan) → filters (composable predicates) → digest → data (repositories facade) → telegram (router/keyboards/conversations) → scheduler (cron) → index.ts (composition root only).
- Plugin architecture confirmed working as designed: adding a source = implement interface + register in `sources/registry.ts`, nothing else touches.
- Known Workers gotcha hit and fixed: global `fetch` must be called unbound (not as `this.fetch(...)`) or it throws "Illegal invocation" — documented in DECISIONS.md so it doesn't get reintroduced.
- **Deploy workflow preference**: user wants Cloudflare deploys driven by the dashboard's git integration (Workers Builds), not local `wrangler deploy`/`wrangler secret put`. Any future infra change (new binding, new secret, new cron) should be guided as a dashboard step, not a CLI command, unless the user asks otherwise.
- Long-term vision: personal developer information hub, single Telegram interface, future sources (Reddit, X, Dev.to, YouTube, etc.) without architectural rework.

## Links
- Repo: https://github.com/itsdezen/devnews-bot
