# Eden Reborn — Public Reports

This is a **public** repository that holds **sanitized, shareable status reports and handoffs**
for the Eden Reborn project. It exists so the reports can be fetched by URL (no copy-paste).

> **The game's source code lives in a separate PRIVATE repository.** This repo contains
> **only** reports — status, architecture summaries, decisions, and file references.

## What this repo is for

Claude Code (working in the private source repo) writes shareable session reports here, commits,
and pushes. The reports can then be fetched directly by raw URL, e.g.:

```
https://raw.githubusercontent.com/ImaginationEngine/EdenReborn-reports/main/reports/<REPORT>.md
```

Reports live under [`reports/`](reports/).

## 🔒 SECURITY DISCIPLINE (this repo is PUBLIC — read before adding anything)

Because anyone on the internet can read this repo, a report published here must **NEVER** contain:

- **Secrets / credentials of any kind** — the PixelLab API key, the Quiver analytics token, or any
  other key/token/password. Reference them **by name only** (e.g. "the PixelLab key in `.env`"),
  never paste the value.
- **Source code excerpts of any real size** — describe what code does and **reference file paths +
  line counts** (e.g. "`scripts/WorldLighting.gd` — falloff reshaped, ~20 lines"). Do **not**
  reproduce actual source.
- **Sensitive go-to-market / strategy specifics** — anything you would not want a competitor reading
  (unreleased pricing tactics, exact wishlist/revenue internals, unannounced launch timing, etc.).

**Reports = status, architecture, decisions, file references. NOT source. NOT secrets.**

Anything sensitive stays in the **private** repo's own `_cowork/handoffs/` and `_cowork/docs/`.
Only the sanitized, shareable version is published here.

## Sharing workflow

1. Code writes/updates a report under `reports/` in this repo, commits, and pushes.
2. Share the **raw** URL (`raw.githubusercontent.com/.../reports/<file>.md`) with whoever needs it.
3. They fetch it directly — no copy-paste.
