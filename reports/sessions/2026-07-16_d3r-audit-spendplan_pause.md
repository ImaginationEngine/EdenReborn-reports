# Pause report — 2026-07-16 — D-3R provenance audit done · SPEND PLAN for approval (gate)

**This turn stops at the spend-approval gate — no generation performed** (per directive:
spend plan for approval before any generation).

## Git state
- Private repo `release-engineering` @ `3725485`, pushed, tree clean.
- PixelLab: **Tier 2 "Pixel Artisan" ACTIVE, 5000/5000 generations** (the sub that had
  lapsed 2026-05-28 is renewed — this also resolves the standing §4 commercial-grant
  concern *while active*; balance is no longer 3,781).

## D-3R Step 1–2 complete (committed): provenance audit + cleanroom rules
Full A/B/C classification in the private `ASSET_LICENSES.md` (append-only, dated). Headline:
- **TAINTED, in regen scope:** walls (`walls_v2.png` ← `Tileset.png`+`(RETOUCHED)`), floor
  (`floor_wang.png` ← the `Tileset(RETOUCHED)` floor cells, via Wang-seamless + remap).
- **TAINTED, NEWLY found (beyond the walls+floor expectation):** the door art is
  **Gemini/Nano-Banana output** (`assets/Doors/Gemini_Generated_Image_*`, `door_*`,
  `Door-Photoroom`). Flagged for a decision (below).
- **UNKNOWN → treat as tainted (verify):** `decor.png`, `speckle.png`, some UI hearts,
  old placeholder art (spikes/flying-enemy).
- **CLEAN:** robot + walk/shoot, enemies, bosses, projectiles, item icons, pickups, floor
  portal, and `floor_plabcobble.png` (a clean PixelLab cobble already in-repo, banked for
  biome 2). D-2b terrain/painter/test CODE is clean and carries over.

## SPEND PLAN (for approval) — flagship cleanroom batch

**Approach (cleanroom-safe):** all generation via the sanctioned REST tool
(`tools/pixellab_generate.py`, not the MCP base64 path). Inputs = TEXT PROMPTS + a
**procedurally-built clean palette swatch** (from the ratified constants — pure PIL, zero
tainted pixels) as `--color-image`. **No tainted image is ever passed as reference.** New
tiles are composed into the frozen D-2b atlas (source id 4, frozen coords) by the same
D-2b compositing/paint/inject machinery, but from CLEAN inputs — this is the biome-swap
capability's first live exercise.

| Family | What | Est. generations (incl. iteration) |
|---|---|---|
| Palette swatch | Built procedurally from ratified constants — **0 gens** | 0 |
| Floor | Seamless Wang tileset (`--tileset`, 64px, high top-down), tuned to floor bar ≥6.5, V~0.44/sat≤0.30 | ~3–5 |
| Walls | Base wall strip (top+front face) + corner + end-cap pieces, one shared clean style seed → composed into runs/corners/caps; V0.31/sat≤0.42 | ~8–12 |
| Door casing/trim | Tiles that dress the opening so the door reads authored, not punched-hole (Phil's flag); active-door edges only | ~3–5 |
| Iteration/readability reserve | PixelLab drifts bright/serious (STYLE_BIBLE) — reserve for misses + readability-gate re-rolls | ~10 |

- **First-pass estimate: ~25–35 generation-units; hard cap 60 before I check back in**
  (tilesets may count as multiple units; 5000 available so quota is not the constraint —
  the constraint is landing style cohesion + the ≥6.5 floor bar).
- Readability gate (`readability_gate.py`) per family; all suites (door/adjacency/
  orientation) must pass on swap; acceptance captures → `reports/d3r_cleanroom/`.

**DECISIONS I need from Phil before generating:**
1. **Approve the spend plan** (approach + ~25–35 gen first pass, cap 60)?
2. **Door frames (newly-found Gemini taint):** fold door-frame regen into THIS batch
   (+~6–10 gens for 4 door types), or defer to a follow-on cleanroom batch? (They're
   store-page-visible and tainted — I recommend folding in soon, but it's beyond your
   original walls+floor+casing scope, so your call.)
3. **décor/speckle (UNKNOWN):** want me to spend ~10 min verifying whether `decor.png`
   was cut from `Tileset.png` (→ tainted, +~2–4 gens) before or after the main batch?

## M6 (running in this latency window)
- Hook + identity + **naming** research spawned as a background pass; result will land in
  its own sessions-channel note. (D-3R art is name-independent, so it does not block.)

## Context / next
- D-3R generation begins on approval of the plan above. Nothing else blocking.
