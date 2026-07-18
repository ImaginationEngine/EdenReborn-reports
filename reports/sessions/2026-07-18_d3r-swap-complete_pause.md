# D-3R pause — cleanroom swap COMPLETE, suites green, re-scored

**Date:** 2026-07-18 · **Branch:** `release-engineering` @ `3272432`, pushed, in sync
**Spend:** **120/130** (ruling expected ~112 — see §2) · Balance 4880/5000

Both rulings executed. Biome-1 walls and floor are now **cleanroom by construction**; the
tainted lineage is out of the live build.

---

## 1. What landed

| Item | Result |
|---|---|
| Edge-grout tick | Fixed **in the compositor**, 0 gens. `wang_15` row 0 measured V 0.45 vs a 0.247 interior with no partner at row 63 — an edge artifact, not a mortar course. `condition_source()` flattens any 1px border line deviating from its inward neighbour, so future bodies inherit the fix. Also improves seam continuity. |
| Multi-body weave | Compositor takes N bodies, spread **within each terrain pool** (MATCH_SIDES picks any signature-matching tile, so pool-mates sharing a body would still repeat). Degrades cleanly to single-body. |
| Process rule | Recorded in CLAUDE.md hard rules **and** `ASSET_LICENSES.md` cleanroom rule 6. |
| Walls + floor | Swapped live. Frozen contract untouched — same 21 atlas coords, source id 4, `FLOOR_S 3` / `FLOOR_V (0,0)..(3,0)`. |
| Tainted v2 builder | Archived to `_archive/d2_era_tools/` with a README explaining why it can only ever reproduce tainted output. |

**No re-bake was needed.** Only texture *pixels* changed; the `.tres` references by path, so the
rebuilt terrain source came out byte-identical and the baked room templates stayed valid. Verified
by an empty `git diff` on the tileset resource.

## 2. The generation spend failed — and why that matters

The authorised wall-body generation **failed and is logged as spent**: 7 of 8 candidates returned
96–100% transparent (V≈0.00); the one with content was 73% transparent. This is the **second**
failure of exactly this kind (the first, 2026-07-17, cost 20 gens and produced decorative objects).

**Structural finding, now recorded:** `/generate-with-style-v2` returns *objects on transparent
backgrounds*. It cannot produce seamless filled texture tiles at all. Only `/create-tileset`
(40 gens) yields filled tiles. That endpoint no longer fits the cap.

This is precisely the failure the new process rule names — the deliverable class
("N interchangeable full-interior variant bodies") was not purchasable from the endpoint used.
The rule is now written down in two places, from two independent instances.

**Both top-ups were therefore delivered by derivation at 0 gens** (`derive_variants.py`):

- **Wall bodies** — torus *rolls* of `wang_15`, which re-phase the mortar coursing so adjacent
  tiles no longer share a brick alignment. Rotations deliberately unused: a 90° turn stands the
  coursing on end and reads as a wrong wall.
- **Floor variants** — `dark_0` (flat, V 0.53) carrying `dark_15`'s **high-frequency structure
  only** (its deviation from its own local mean), gain 0.34, at four rotations. The detail layer
  is *rotated* per variant because `dark_15`'s slab period is ~32px, so a 32px roll maps onto
  itself and the floor reads as a regular grid. Floors are orientation-agnostic; brick coursing
  is not.

Derivation from our own clean output is explicitly clean under the CLEANROOM ASSET RULE.

## 3. Verification — all green

- **Door regression 16/16 PASS** (4 directions × 300/700 px/s), exit 0
- **GdUnit4 3/3 suites**: door matrix · wall-adjacency (seeds 11/22/33) · wall-orientation
  (seeds 11/22/33), exit 0
- **In-engine before/after**, real `World.tscn`, same rig and seed, 4 room types →
  `reports/d3r_cleanroom/accept_ab.png`

## 4. Measured outcome

| Metric | Old | New |
|---|---|---|
| wall/floor separation ΔV | 0.086 | **0.277** (3.2×) |
| floor hue / saturation | 34° / 0.304 | **35° / 0.300** (on-palette) |
| floor value | 0.438 | **0.528** (per the ruling's V≈0.52) |

The floor did **not** lose warmth — hue and saturation are within noise of the ratified values.
It is the same palette family lifted purely in value. That lift is what buys the 3.2× separation,
and it visibly brightens rooms against the ratified V~0.44 look. `ASSET_LICENSES.md` records that
the ruling's V≈0.52 supersedes the older V~0.44 constant, with STYLE_BIBLE to follow.

## 5. Re-score (execution agent's ESTIMATE — Phil's eye is the judge)

| Axis | Prior | New | Bar | Reason |
|---|---|---|---|---|
| Coherence | 6 | **6** | ≥6 | Held, not regressed. Wall and floor are now *more* unified with each other — one generation lineage, one palette family, and a wall that finally separates from the floor. Offset by the flat-top wall losing the old 2.5D depth cue, and by props/speckle still being tuned against a darker floor. Net: unified with minor seams. |
| Environment | 5 | **5.5** | ≥7 | Real gains in room readability (3.2× separation makes walkable-vs-solid instantly parseable) and wall variety (4 bodies break the single-body rhythm). But **prop/entity density and living elements are untouched**, and those are what hold this axis down. Rooms remain lightly furnished and static, so it stays in the "barren but functional" band rather than reaching "furnished". |

**Environment remains below the ≥7 bar.** That is expected: this batch's purpose was IP
provenance, not furnishing. Closing the gap to 7 is a density/liveliness problem (props, sway,
critters), not a tile problem.

### Honest caveats

- The capture rig's `normal_combat` room came out with a different layout between the two runs,
  so that one panel is **not** strictly like-for-like. The other three are.
- The wall is flatter than the outgoing art. That is the accepted trade, but it is visible.
- The shading-band option remains in pocket for Phil's live acceptance eye.

## 6. Found in passing — pre-existing, flagged not fixed

`tools/build_wall_terrain.gd` adds collision polygons **before** attaching the source to the
tileset, so all 40 such calls fail silently and source 4's tiles carry no collision. Verified
pre-existing (source 4's physics state is byte-identical before and after a run) and not
gameplay-affecting — the script's own header notes runtime collision comes from code walls and
tilemap physics stays disabled. It is a latent trap for whoever enables tilemap physics later.
Left untouched deliberately: the frozen D-2b machinery is load-bearing and this batch had no
mandate to refactor it.

## 7. Need from Phil

1. **Live acceptance eye** on `accept_ab.png` — specifically whether the brighter floor (V 0.528)
   and the flatter wall are the right trade for the 3.2× readability gain, or whether the
   in-pocket shading band should come out.
2. **Budget** — 10 gens remain of 130. Door frames and casing (queued steps 3–4 of the original
   plan) are **not** funded, and the sprite endpoint cannot deliver them regardless. Those need
   either a cap raise for a `/create-tileset` purchase, or a derivation approach.
3. **Store-page readiness check** is now unblocked on provenance grounds — walls and floor are
   clean by construction.
