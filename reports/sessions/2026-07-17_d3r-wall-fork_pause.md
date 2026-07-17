# Pause report — 2026-07-17 — D-3R wall integration fork (decision needed) + GLIMMO insurance

Evidence: `reports/d3r_cleanroom/floor_darkened_seed777.png`,
`reports/d3r_cleanroom/wall_tileset_candidates_seed777.png`.

## Progress
- **Floor: DONE** — generated (cleanroom, seed 777) + value-darkened to V~0.44, banked.
- **Walls: generated + in hand, but paradigm-mismatched.**
  - Plain-sprite path (your approved first choice) **FAILED the visual read** — it produced
    decorative objects (slab/spiral/frame/mask), not connected wall structure. Per your pre-
    authorization, reverted to the wall-tileset path (cap 130), no round-trip.
  - The wall Wang tileset was **charged (40 gens) but lost to a client timeout** (no local
    save). **Recovered all 16 tiles for FREE** via a server-side re-fetch — **no re-spend.**
  - **Spend: 100/130** (floor 40 + failed plain-sprite 20 + wall tileset 40). Budget intact.

## The wall decision (blocks continuation)
The recovered walls are a floor↔wall **BLOB Wang set** (rounded, Moonlighter-style THICK walls).
Two issues:
1. **Color:** they generated **blue-grey, off the warm ratified palette** → needs a warm remap
   (cheap pixel pass, reuse the floor-remap math; no regen).
2. **Paradigm mismatch (the real fork):** the frozen **D-2b atlas is a thin 1-tile ring**
   (H-run top+front face, V-runs, sharp corners, door end-caps). The blob-Wang set does not map
   cleanly onto it. Choose one:
   - **(A) Adopt a Wang-wall painter** — arguably a nicer, more premium wall look, but it
     changes the D-2b machinery, the terrain contract, and needs new tests. Bigger change.
   - **(B) Extract thin-ring pieces** from the Wang set into the frozen D-2b atlas — keeps all
     D-2b machinery/tests, but the rounded blob art may not crop into clean thin rings.
   - **(C) Re-approach** wall generation for thin-ring pieces specifically.

My lean: **(A)** if you're open to a wall-system change (the blob/thick-wall look is more
premium and the tileset already produces correct seamless edges/corners), else **(B)**. Either
way the warm remap applies first.

## GLIMMO insurance screen (your item 2) — recorded (private doc)
Runner-up screened: **CAUTION → serviceable backup, not clearly superior to TINKO.** Steam CLEAR,
Class 41 GO, Class 9 GO-with-caution (a live "Glimmo AI Journal" class-9 app), domains
constrained (`.com`/`.io`/`.app` taken by the journal app → SEO tax + differentiated handles
needed). Interpreted "v2 runner-up" as GLIMMO (shortlist #2); if you meant BLEEPO (#1), say so.

## Need from Phil
1. **Wall fork: A / B / C?** (This is the gate to finish D-3R.)
2. TINKO remains the primary (formal counsel clearance still pending your side).
- Context here is deep — the chosen wall path + warm remap + compose + swap + acceptance
  (seed-777 wide) + suites + re-score run in a fresh window from the updated continuation handoff.
