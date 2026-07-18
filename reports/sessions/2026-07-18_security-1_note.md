# Session note — 2026-07-18 — SECURITY-1 (secret hygiene)

Findings are sanitized — no credential value is ever reproduced here.

## Full-history secret scan — BOTH repos CLEAN
- Scanned the entire git history of the private source repo AND this public reports repo
  for high-signal credential values (provider tokens, private-key blocks, quoted long-value
  assignments to credential-named vars), with values masked in the scan output.
- **Result: no live credential values in either repo's history.** Every pattern hit was a
  false positive of the safe kind: policy/documentation text ("never commit the PixelLab
  key / Quiver token"), environment-variable reads (`os.environ.get(...)`), keystore *setup
  instructions* (no actual passwords), and a GdUnit string-fuzzer named `test_password`.
- The analytics (Quiver) token is documented as **editor-local and pending** — never committed.
- **No rotation required.**

## Hardening added (private repo)
- **Targeted secret scanner** (`dev/hooks/secret_scan.py`) — high-signal patterns, tuned to
  not false-positive on the project's policy docs; masks any value it prints.
- **Pre-push gate** (`dev/hooks/pre-push`) — blocks a push whose commits contain a secret
  (prefers gitleaks if installed, else the scanner). Enable with `git config core.hooksPath dev/hooks`.
- **CI workflow** (`secret-scan.yml`) — gitleaks full-history on every push/PR + the scanner as
  a redundant floor.
- **Channel-split policy written into CLAUDE.md** (already practiced): secrets + source →
  private repo only; sanitized reports → public; enforced by the pre-push gate + CI.

## Gitignore coverage
- Private repo: `.env` + `*.keystore` already ignored; `.env` is not tracked. ✓
- Public reports repo: **added a `.gitignore`** (defense-in-depth) covering `.env`, key/pem/
  keystore/credential/token globs, and `*.import` sidecars — so an accidental drop can't commit.

## Need from Phil
- Nothing. No exposed secrets, no rotation. The gates are version-controlled; enabling the
  local pre-push hook is a one-time `git config core.hooksPath dev/hooks` per clone.
