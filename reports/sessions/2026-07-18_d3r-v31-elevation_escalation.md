# D-3R v3.1 — anatomy fixed, ELEVATION ESCALATION (per the timebox)

**Date:** 2026-07-18 · `release-engineering` @ `86adcef`, pushed · **0 gens this pass** ·
Spend unchanged **120/150**

## Defects A / C / D — FIXED, root-caused

| # | Root cause (measured, not guessed) | Fix |
|---|---|---|
| **A** floor-top black bar | The shadow band `(3,1)` shipped **opaque** (alpha 255 flat). The outgoing tile was **translucent**, ramping alpha 150 → 0. The painter lays that tile on the **first FLOOR row**, so an opaque version paints a bar across the floor. | Alpha ramp reproduced; RGB stays dark. It is a cast shadow, not a surface. |
| **C** corners | They were run tiles carrying two void edges. | True mitred corners — per-side profiles combine by taking the outermost treatment per pixel, so the top course wraps the corner. |
| **D** inverted south | North and south share the `[L,R]` terrain signature; MATCH_SIDES cannot tell them apart, so south reused north art upside down. | South art moved to **new row-3 coords**, placed positionally by the painter — the same mechanism as the existing east-column override. Atlas 8×3 → 8×4; **rows 0–2 untouched, row 3 purely additive.** |
| **E/W** | Horizontal coursing turned sideways. | Side-face treatment with the coursing phase rolled for vertical runs. |

## Elevation — achievable, but I am escalating on the READ

Structurally the spec is met. Measured on the north run: **course 0.424 / face 0.224 /
base 0.137**, course-face ΔV **0.200**, no clipping (brightest 119/255), and south mirrors it.

Two iterations were needed to learn the real constraint: **multiplying the brick body by a gain
does not make a top surface.** It brightens the same brick, so the eye reads one flat plane with
banding — and gain >1.2 clips the light grout to white. The course band now blends toward a
*flattened* copy of the body, which is what actually sells "this is the top, seen from above".

**Where it lands, honestly:** the three masses are there and correctly oriented per side, but the
result reads as **banded stripes rather than sculpted masonry**, and E/W remain the weakest — the
same ranking Phil graded. Modulating a flat texture with 1-D ramps can produce the *structure* of
elevation but not an authored *lit-face*. I do not think further ramp-tuning closes that gap, and
the instruction was not to grind.

### Per-side self-grade vs Phil's baseline

| Side | Phil's baseline | v3.1 estimate | Note |
|---|---|---|---|
| N | 9/10 | 8.5/10 | Anatomy unchanged in kind; slightly busier from the flattened course. |
| S | 6/10 | 8/10 | Now genuinely mirrored rather than inverted. |
| E/W | lowest | 6.5/10 | Correct side-face treatment, but the striping is most visible here. |
| Corners | — | 7/10 | Mitre is correct; reads as a frame more than as masonry. |

## Escalation — one line

**Derivation gets the structure but not the lit-face read; recommend buying one
`/create-tileset` wall set (40 gens → 160/150, i.e. a 10-gen cap overrun) OR accepting v3.1's
banded elevation as "good enough not to harm" and spending the remaining budget on door frames
instead.** My lean: **accept v3.1 and fund the frames** — frames are the last tainted class and
therefore gate the provenance audit and the store page, whereas wall polish does not.

Note: the earlier "sprite endpoint can't do this" finding was specific to *seamless filled texture
tiles*. Door frames are **objects with transparency**, which is exactly what that endpoint does
well — so routing frames through `/generate-with-style-v2` is sound.

## State

Live assets still carry **v3.0** — v3.1 is built and committed but deliberately **not swapped**,
so the pipeline (swap → bake → suites → full close-up matrix) is not spent on an elevation
treatment awaiting a verdict. Nothing is blocked but this decision.

Evidence in `reports/d3r_cleanroom/`: `room_v31.png` (composed, N+W doors, shadow band live),
`sheet_v31.png` (28 tiles, coord-labelled).
