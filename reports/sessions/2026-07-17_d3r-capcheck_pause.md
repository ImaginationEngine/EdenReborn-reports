# Pause report — 2026-07-17 — D-3R CAP CHECK-IN (pipeline validated; budget decision needed)

Per directive: pause at cap check-in. **Generation has started; live game is untouched**
(all candidates in scratch — nothing swapped, nothing can break). Checking in because the
floor alone consumed most of the approved cap.

## Progress this turn
- **décor/speckle VERIFIED clean-of-taint** (forensic: not cut from the tainted tileset;
  sparse transparent overlays). Reclassified A. Off-warm-palette is a D-4 cohesion item, not
  cleanroom. (`ASSET_LICENSES.md`.)
- **Feel pass marked DEMO CRITICAL PATH** in the roadmap + this register (the hook promises it).
- **Cleanroom pipeline VALIDATED end-to-end.** Built a procedural ratified-palette swatch
  (zero tainted pixels) → generated the floor from **text prompt + swatch only, seed 777,
  no tainted reference**. Result (`reports/d3r_cleanroom/floor_candidates_seed777.png`):
  warm sandstone, flat chunky shading, subtle cracks — **clearly premium vs the old floor,
  on-palette**. Reads slightly bright vs V~0.44 → a cheap value-darken post-pass fixes it (no regen).

## The budget reality (the reason for this check-in)
- **A Wang tileset costs 40 generations in one job.** The floor used **40 of the 60 cap**;
  balance 4960/5000. Only **20 left under the cap**, with walls + doors + door-casing still to go.
- The cap of 60 was set before we knew a tileset = 40 gens. It no longer fits the batch.

## Decision needed from Phil (one of):
1. **Raise the first-pass cap** to cover the rest. Rough estimate if walls also need a
   tileset-class job: floor 40 + walls ~40 + doors ~15 + casing ~8 + iteration ~20 ≈ **~120**.
2. **Cheaper wall path (my recommendation):** generate walls as a few **plain 64px sprite
   pieces** (top/front face, corner, end-cap) — likely ~1–4 gens each — and compose them into
   the frozen D-2b atlas with our own clean code (the D-2b compositor, clean inputs). Keeps the
   remainder near ~30–40 gens → **total ≈ 75–80**, a smaller cap bump. Trade-off: plain-sprite
   wall quality is unvalidated (the floor tileset path is the proven one).

Recommend **option 2 with a cap of ~90** (headroom for iteration), reverting to a wall tileset
only if the plain-sprite walls don't read well.

## Continuation
- Given context depth here, I recommend the wall/door/casing generation + compose + live swap +
  acceptance (incl. the seed-777 wide) + re-score run in a **fresh window**, reusing the
  **already-generated floor (40 gens banked in `tools/_kt_d3r/`, do not regenerate)**.
- **NAMING v2** (mascot-name shortlist) running in background; lands in its own note.

## Need from Phil
- Approve a cap (option 1 ~120, or option 2 ~90 recommended), and confirm fresh-window continuation.
