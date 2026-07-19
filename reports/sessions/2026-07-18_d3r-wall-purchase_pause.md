# Pause report — 2026-07-18 — D-3R hope-tier wall purchase + provenance close-out

> **Published retroactively (2026-07-19).** This pause was reported in chat at the time but the
> markdown was never pushed here, so the co-review record had a gap between the v3.1 elevation
> escalation and the session checkpoint. Reconstructed from the private repo's commit history,
> provenance audit, and session handoff. The standing pause-report rule now carries a
> verify-on-origin clause so this failure mode is caught at the pause itself.

## Git state (at the pause)

- Branch `release-engineering` @ **`70b08b7`**, tree clean of tracked changes, pushed and in sync.
- Subsequently checkpointed at **`7909630`** (session handoff), also pushed. CI green on both:
  `secret-scan` and `door-regression` succeeded.
- **PixelLab spend: 160/175** (cap raised by Phil this session). 15 gens remain, earmarked for
  door frames.

## What landed

| Commit | What |
|---|---|
| `86adcef` | **v3.1 compositor** — per-side wall anatomy + elevation, defects A/C/D fixed, **0 gens** |
| `e7ecfd8` | **v3.2 wall purchase** — hope-tier `/create-tileset` buy, **40 gens**, passed the measure-first gate |
| `b007bcb` | Provenance audit rows for walls/floor marked CLEARED after the swap |
| `70b08b7` | Last stale taint reference cleared from cleanroom rule 2 |

Evidence captures published alongside: `reports/d3r_cleanroom/` (v3.1 room + sheet, v3.2 sheet).

## v3.1 — per-side anatomy, zero spend

v3.0 applied **one band ordering to every side** — correct for north (Phil graded it 9/10), wrong
everywhere else (S 6/10, E/W lowest). Four defects root-caused and fixed in the compositor
(`tools/_kt_d3r/build_walls_v3.py`, ~310 lines changed):

- **A — shadow band shipped opaque.** The outgoing tile was translucent (alpha ramp 150→0) and the
  painter lays it on the *first floor row*, so an opaque tile painted a black bar across the floor.
  Alpha ramp restored.
- **C — corners were run tiles carrying two void edges.** Now true mitred corners: per-side profiles
  combine by taking the outermost treatment per pixel, so the top course wraps the corner.
- **D — south reused north art upside down.** N and S share a terrain signature that the match-sides
  mechanism cannot separate, so south art now lives at new row-3 coords for positional placement by
  the painter — the same mechanism as the existing east-column override. Atlas grew 8×3 → 8×4;
  **rows 0–2 coords untouched, row 3 purely additive** (the frozen D-2b contract holds).
- **E/W** — side runs get side-face treatment with the coursing phase rolled, so a side wall no
  longer reads as a horizontal wall turned sideways.

**Elevation, measured (N run):** course 0.424 / face 0.224 / base 0.137 → **course-face ΔV 0.200**,
no clipping (brightest 119/255). South mirrors correctly. Door-cap terminators restored — gap-facing
edges 0.285/0.298 vs 0.220 plain.

Two iterations were needed: multiplying the body by a gain only brightens the same brick (reads as
banding, and above ~1.2 it clips grout to white). The course band now blends toward a *flattened*
copy of the body, which is what sells a separate plane.

**Known limitation — and why the purchase happened.** The structure measures correctly but reads as
**banded stripes rather than sculpted masonry**; E/W weakest. Modulating a flat texture with 1-D
ramps yields the *structure* of elevation, not an authored lit face.

v3.1 was committed **deliberately unswapped** — live assets still carried v3.0. Spending the
swap/bake/suite/close-up pipeline on a treatment awaiting a verdict would have been wasted, and it
preserves a clean fallback. **v3.1 is the guaranteed floor: nothing below it ships.**

## v3.2 — the hope-tier purchase, measured before integration

Phil-ratified single `/create-tileset` buy (40 gens). **Measured before any integration**, per the
ruling:

| Metric | v3.0 set (previous) | v3.2 set (this buy) | Verdict |
|---|---|---|---|
| Transition-tile deviation from full-tile mean | 0.16–0.27 | **0.027 mean / 0.047 max** | **PASS** |
| Full-interior tiles yielded | 1 | **6** (indices 0,1,2,13,14,15) | **PASS** |
| Authored lit-top-edge / lit-face shading | absent | present | **PASS** |
| Body value vs floor (0.528) | — | V 0.380 → **ΔV 0.148** | **below band** (see below) |

**What fixed it:** stating the **deliverable class and the material-consistency requirement
explicitly** in the prompt — that the brick material, colour and value must be identical across
every tile *including all edge/corner/transition tiles*. The v3.0 set failed precisely there, which
is why it yielded a single usable tile. This is the process rule from this batch paying for itself
on its first use.

**Anchors were clean:** the ratified palette swatch as colour-image and our own derived floor
variant as reference-image. No tainted input. Logged in the provenance ledger with prompt, anchor
and date, per the cleanroom rule.

**The one open defect:** the mined body sits at **V 0.380** against our 0.528 floor — **ΔV 0.148**,
below v3.1's 0.277 and below the proven readability band. Value normalisation plus a retune of the
per-side profile constants (they were tuned for a 0.251 body) is required before it can be judged;
a straight swap would clip or wash out. That is the first task of the next window, with a target of
**ΔV ≥ 0.25**.

## Provenance corrections

Two doc-drift findings, both true positives caught by the pre-commit hook:

1. The audit still listed the live wall and floor textures as **TAINTED** after the cleanroom swap
   (`3272432`) had already replaced both. Rows now marked **CLEARED/historical** with the swap
   commit cited.
2. Cleanroom rule 2 still **barred those same two files as reference inputs**. After the swap they
   *are* the clean builds — that wording would have blocked a future agent from using valid anchors.
   Corrected, and the archived path for the retired v2 builder repointed.

**Doors are now the last tainted class.** The door-art folder contains raw Gemini-generated files
and cannot be confirmed clean, so it is binned as tainted per the rule. That row closes when the new
frames land — at which point **biome 1's visible set is cleanroom end to end.**

## Two structural findings worth keeping

1. **`/generate-with-style-v2` cannot produce seamless filled texture tiles.** It returns *objects on
   transparent backgrounds*. Proven twice across 40 gens; the second attempt returned 7/8 candidates
   at 96–100% transparent. Only `/create-tileset` yields filled tiles. It **is** the right endpoint
   for door frames, which are an objects-class deliverable.
2. **Cost model:** a 64px `generate-with-style-v2` request costs **20 gens** and returns 16
   candidates (the count flag controls only how many are *saved*, not spend). `/create-tileset` costs
   **40**. Always check balance before and after.

## Re-scores

No re-score at this pause — live assets still carried v3.0, so the scored artefact had not changed
since the swap-complete report. The next re-score follows the best-of swap, alongside the full
close-up matrix.

## Next steps

1. Mine v3.2 into the compositor: value-normalise toward **ΔV ≥ 0.25** while preserving the authored
   lit edges, and retune the per-side profile constants for the new body. Frozen contract and painter
   unchanged — material mining only.
2. Door frames — objects-class, ≤15 gens, style-anchored to the new wall family. Casing
   derivation-first. The 126px-in-128px casing geometry is suspect against the new art and needs
   verifying against the gap cells.
3. Best-of {v3.2-mined, v3.1} → swap → bake → all three suites → **full close-up matrix** (N/S/E/W
   runs, 4 corners, every door both sides) + seed-777 wide, published for co-review → per-side
   self-grade against Phil's baseline.
4. Provenance audit close (frames → CLEAN) → quality ladder into the style bible → store-page
   readiness check.

**Standing constraint:** no further wall attempts this phase under any outcome.

## Need from Phil

Nothing blocking at this pause. The best-of verdict needs his eye on the close-up matrix, which is
produced in the next window — the ask arrives with the evidence attached.
