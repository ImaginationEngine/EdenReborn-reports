# D-3R pause — B integration: ruling premise disproved, alternative built

**Date:** 2026-07-18 · **Branch:** `release-engineering` · **Spend:** 100/130 (unchanged — this
session spent **0 generations**) · **Balance:** 4900/5000

**Headline:** the B ruling ("proceed with B — Wang edge tiles into the frozen atlas") rests on a
premise that measurement disproves. B as specified fails. A zero-spend alternative that satisfies
the same intent is built and rendered. **Two rulings needed before the swap.**

---

## 1. What was checked, and how

Before generating anything, the 16-tile warm Wang set and the 16 banked floor tiles were measured
numerically (per-quadrant mean HSV value) and rendered in situ into the ratified 13×7 playfield
with a 1-tile wall ring. Pure compositing — no generation spend.

## 2. Finding 1 — the Wang set is not internally consistent (B fails)

| element | mean V |
|---|---|
| `wang_15` (solid brick body) | **0.25** |
| "wall" material inside every *transition* tile | **0.41 – 0.52** |
| "floor" material inside every *transition* tile | **0.70** |
| our new dark floor | **0.44** |

`wang_15` is dark brick. The wall side of every edge/corner tile is a *different, much lighter*
material, and their floor side is lighter than our actual floor. Dropped into the ring, the edge
tiles read as a **bright frame around the room**, not a wall — see panel B of `room_compare.png`.

Why the B-probe passed anyway: it tiled `wang_15` against itself (one tile → necessarily seamless)
and judged the edge tiles separately. It never composited the two together against the new floor.
The probe result is real; the inference from it was not.

**Only `wang_15` is usable from the wall set.** The other 15 tiles are inventory for a future biome,
not this atlas.

## 3. Finding 2 — the floor is not done

Classified by quadrant spread, **2 of 16** banked floor tiles are true full-interior variants:

- `dark_0` — V 0.53, **featureless** (no detail at all)
- `dark_15` — V 0.34, good slab detail

The other 14 are Wang transition tiles. Random-picking them as "variants" produces the visible
patchwork in panels B/C of `room_compare.png`. The frozen painter contract wants **4 interchangeable
variants**; the banked set supplies effectively **one**, because the two usable tiles sit 0.19 apart
in value and cannot be mixed without blotching.

This directly couples to wall readability:

| floor choice | wall/floor ΔV | in-situ read |
|---|---|---|
| `dark_0` (V 0.53) | **0.28** | wall reads clearly — panel D of `room_v3.png` |
| `dark_15` (V 0.34) | **0.09** | wall and floor merge; slab pattern fights brick — panel E |

So the only floor that makes the wall read is the one with no detail in it.

## 4. What was built instead (zero spend)

A new cleanroom compositor, `tools/_kt_d3r/build_walls_v3.py` (141 lines), which takes its **only
pixel source** from `wang_15` and synthesises the entire 21-tile frozen atlas procedurally — void
bands, gradient ramps, base contact shadow, corner cases, door-cap terminator courses, shadow band.
Clean by construction; the frozen coord contract and the D-2b terrain machinery are untouched.

This is the compositor path already pre-authorised in the continuation handoff §3 ("compose into the
frozen atlas via our own clean code… flips/crops of our own output stay clean") — sourced from
`wang_15` rather than new plain sprites, which is why it costs nothing.

Band anatomy was recovered by measuring the outgoing atlas (void solid ~10px → ramp ~12px → face
~35px → base shadow ~8px) and the void-edge assignment per coord was mapped from it, so the new
atlas reproduces the outgoing structural read rather than guessing at it.

**Supersedes** `tools/_kt_d2wall/build_walls_v2.py`, which sources two tainted images and must be
archived when the swap lands.

### Known weaknesses of the v3 atlas (not blockers, but real)

1. **Pattern repetition** — every tile derives from one brick body, so mortar coursing repeats on a
   visible rhythm around the ring. Horizontal/vertical flips only partly break it.
2. **Edge-grout stripe** — `wang_15` carries bright mortar on its own tile edge. On V-run tiles
   (no void band on that edge) it exposes as a repeating bright horizontal tick down wall columns.
3. **Flatter than the outgoing art** — this is the flat-brick trade already accepted in the ruling,
   but stated plainly: the outgoing 2.5D lit-face-plus-ledge wall has more depth than v3 does.

A first cut of the cap terminator blew out to near-white (the artificial-light failure mode); that
was caught and tightened before this report — brightest cap pixel now 156, was ~255.

## 5. Evidence

All in `reports/d3r_cleanroom/`:

- `room_compare.png` — A current build · B B-as-specified (**fails**) · C `wang_15`-only ring
- `room_v3.png` — A current build · D v3 + `dark_0` · E v3 + `dark_15` (floor coupling)
- `sheet_walls_v3.png` — the composed 21-tile atlas, coord-labelled
- `sheet_wang_warm.png` — the 16 warm Wang tiles, index-labelled
- `sheet_floor_dark.png` — the 16 banked floor tiles, index-labelled

## 6. Need from Phil — two rulings

**Ruling 1 — walls.** Accept `walls_v3` (cleanroom, on-contract, 0 gens) as the wall paradigm for
this biome? Options: accept as-is · accept + spend a few gens on 2–3 extra brick bodies to break the
repetition · reject and re-open the wall approach.

**Ruling 2 — floor.** The standing instruction is **DO NOT REGENERATE THE FLOOR** (40 gens sunk).
That instruction was written on the belief the floor was complete; it supplies one usable variant,
not four. Recommendation: a **targeted top-up of ~4–6 gens** for flat-interior variants at V ≈ 0.52
(matching `dark_0`'s value, so wall separation holds) carrying light slab detail, anchored to the
existing palette swatch plus `dark_0`. That is a different, much smaller ask than redoing the
40-gen Wang set, and fits inside the remaining budget.

Nothing has been swapped into the live build. `assets/tilesets/biome1/walls_v2.png` and
`floor_wang.png` are still the outgoing tainted assets; the swap, bake, door-regression matrix,
capture and re-score all remain queued behind these two rulings.
