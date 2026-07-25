---
slug: retinafy
status: active
path: /Users/itsdezen/Developer/retinafy
updated: 2026-07-26
---

# retinafy

## What it is
macOS utility that simulates HiDPI ("Retina") scaling on external displays that don't natively report a HiDPI mode, adding a "Looks like" resolution picker in System Settings ▸ Displays. Pure bash (`retinafy.sh`/`.command`), no compiled code. Requires macOS 26 (Tahoe)+.

## Current focus
Recent hardening pass: confirm-before-wipe on the Overrides directory, dry-run mode + spinner UI, bash 3.2 compatibility fix (`read -i`), EDID compatibility patch for monitors that drop resolution after sleep/wake (Intel Macs only).

## Key decisions / context
- Safety model: only ever writes under `/Library/Displays/Contents/Resources/Overrides`, never touches `/System`, no SIP disable, no kernel extension. Built-in display is excluded from selection entirely.
- Fully reversible — in-app "Disable HiDPI", or delete the Overrides folder; a recovery-mode fallback script is generated at `~/.retinafy-disable`.
- Working tree clean, branch `main`, no open WIP as of 2026-07-26.


## Links
- Repo: https://github.com/itsdezen/retinafy
