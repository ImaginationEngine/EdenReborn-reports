# Pause report — 2026-07-19 — D-3R v3.2 mined; frames BLOCKED on budget

## Git state

- `release-engineering` @ **`c798950`**, tree clean of tracked changes, **pushed and in sync**.
- CI green on the branch (`secret-scan`, `door-regression`).
- **PixelLab spend unchanged at 160/175 — nothing generated this session.** Everything below is
  0 gens.

## What landed

| Commit | What |
|---|---|
| *(reports repo)* | Retroactive publication of the missing 2026-07-18 wall-purchase pause |
| `991610c` | **VERIFY-ON-ORIGIN** clause added to the pause-report rule |
| `27facc8` | **v3.2 mined** — authored lit lip, value-normalised, 0 gens |
| `5eaf6f9` `c798950` | **Quality ladder** ratified into the style bible; floor constant superseded; doc-drift cleared |

Evidence: `reports/d3r_cleanroom/v32_compose_stack.png`, `v32_compose_closeup.png`,
`v32_purchase_classified.png`.

## 0. Hygiene — both items done

The 2026-07-18 wall-purchase pause is now published here as
`2026-07-18_d3r-wall-purchase_pause.md`. The gap existed because a pause was reported in chat
while its markdown never reached this repo, and nothing checked. The pause-report rule now
carries a **verify-on-origin** clause: after pushing, `ls-remote` the reports repo, confirm the
file resolves, and put the reports-repo HEAD hash in the chat message. Every "published" carries
its own receipt. This report is its first live use.

## 1. v3.2 mining — what the purchase actually bought

**A measurement correction first.** The ledger recorded "6 full-interior tiles (0,1,2,13,14,15)
vs 1 last time". Quadrant classification shows that count conflated two materials: **0,1,2 are
FULL FLOOR** (mean 0.528, std 0.012 — statistically our own ratified floor) and **13,14,15 are
FULL WALL**. Real full-wall yield is **3, not 6**. Still 3× the previous set, so the measure-first
gate verdict stands, but the number was wrong and is corrected in the ledger.

**The purchase's value is not a better flat body — it is an edge.** The authored lit edge is a
**thin 2px specular lip at V 0.74–0.79**, sitting just inside a dark navy outline (measured: one
transition tile's row 8 at 0.788, another's column 30 at 0.743). v3.1's course is a **fifteen px
bright band**. That shape difference is the whole explanation for "reads as banded stripes rather
than sculpted masonry" — a broad band reads as a stripe painted on a flat surface; a crisp lip
reads as light catching a raised edge.

So v3.2 mines the lip's **colour and treatment** rather than its pixel rows (copied rows would
fight the body's own brick layout), and lets the lip carry the highlight while the broad course
gain drops. The lip is gated by the body's own local brightness so **mortar joints stay dark
through it** — an unbroken bright line is the tell of a painted-on edge; one that breaks at every
joint reads as brick.

**Value normalisation.** Body 0.380 → **0.275** by proportional RGB scaling, chosen over an offset
because it preserves hue and saturation exactly and cannot lift any pixel above the floor.
**ΔV 0.253** vs the 0.528 floor — clears the 0.25 target (v3.1 was 0.277).

### Measured, north run, v3.1 → v3.2

| Band | v3.1 | v3.2 | Δ |
|---|---|---|---|
| lit lip | 0.443 | **0.626** | +0.183 |
| course | 0.427 | 0.366 | −0.060 |
| face | 0.221 | 0.225 | +0.004 |
| base | 0.141 | 0.140 | −0.002 |
| **LIP-face ΔV** | 0.222 | **0.401** | **+0.179** |
| course-face ΔV | 0.206 | 0.141 | −0.065 *(intended trade)* |
| body vs floor | +0.278 | **+0.290** | better separation |
| brightest px | 119/255 | 185/255 | no clipping |

Runs measure 0.217–0.220 and corners 0.158–0.160 across all four orientations — uniform.

**Found only on the COMPOSED probe, not in isolation.** The 4-body roll pool broke the brick
coursing at every tile boundary. That was free on v3.1's near-featureless body; on this one it read
as broken masonry — a worse artifact than repetition. The purchased full-wall tile is seamless
(wrap delta 0.008 vs 0.095 inner), so one body now gives continuous coursing. That also cured a
per-side inconsistency (one corner's peak went 0.557 → 0.757, into line with the rest). This is the
ASSET PROBE rule paying for itself a second time.

**The v3.1 path regenerates byte-identical — verified.** The guaranteed floor is untouched, and
both profiles now live in one compositor selected by argument, so there is no forked builder to
drift.

**Self-assessment (Phil's eye still decides):** v3.2 beats v3.1 on every measured axis and, in the
composed room probe, decisively on read — v3.1 barely resolves as masonry at all. Recommendation is
that **v3.2 proceeds**, with v3.1 banked and one command away.

## 2. Frames — BLOCKED, needs a one-line decision

**The cheapest documented `generate-with-style-v2` request costs 20 gens. 15 remain.** It cannot be
run without an overrun, so nothing was spent.

A second finding argues for not spending them anyway. The door frame sprite renders at
**126px into a 128px baked gap** (sprite scale 0.15 × node scale 0.75 on a 1120px texture). The
handoff flagged this as suspect; it is real. Against v3.1's low-contrast wall the resulting sliver
hid, but against v3.2's high-contrast masonry it will read. **That is a geometry bug, and no amount
of generated art fixes it.**

### Options

| | Approach | Cost | Note |
|---|---|---|---|
| **A** *(recommended)* | Derive frames + casing from the v3.2 wall family, and fix 126→128 | **0 gens** | Guarantees the frame is *literally the same brick* as the wall. Material mismatch between separately-sourced pieces is exactly what killed the v3.0 set. Ships inside budget. |
| B | Authorize **+5 gens** (cap 175→180) for one 64px request | 20 gens | 16 candidates returned. Keeps a generated-art option if derivation disappoints. |
| C | Try a 48px request | unknown | Cost undocumented; may land under 15 or may overrun — the risk the no-silent-overrun rule exists to prevent. |

**A is recommended**, with B held in reserve. Derivation also closes the provenance audit trivially:
frames derived from cleanroom walls are clean by construction, making biome 1's visible set
cleanroom end to end.

## 3. Style bible — both debts cleared

The **quality ladder** Phil ordered is now written into the bible as a binding section: Isaac-parity
= shippable minimum (the v3.1 floor, nothing below ships) · Juicy-Moonlighter = hope tier (the
purchase was its one authorized attempt) · Moonlighter-Cainos = grail, parked at the Biome-2 gate.
Includes the standing constraint that **no further wall attempts happen this phase** regardless of
how the best-of lands.

The **floor value constant** (still reading V~0.44) is superseded to 0.52, marked rather than
silently overwritten, with the reason it binds: wall readability is defined as a *separation* from
the floor, so the two constants move together.

Two generalisable rules were banked alongside: readability is a separation and never an absolute;
and elevation is sold by **edge shape**, not band brightness — when an asset measures right but
reads flat, check the shape of the highlight before touching its brightness.

## Re-scores

**None this session** — the walls are mined and committed but **not swapped**, so the scored artefact
is unchanged. Re-score follows the swap, alongside the full close-up matrix, exactly as the previous
pause set out.

## Still open (the batch close, gated on the decision above)

Swap best-of + frames → bake → all three suites → full close-up matrix (N/S/E/W runs, 4 corners,
every door both sides) + seed-777 wide, published for co-review → per-side self-grade against Phil's
baseline → provenance audit close → store-page readiness check → checkpoint.

## Need from Phil

**One decision: frames via A (derive, 0 gens) or B (+5 gens over cap)?** A is recommended and needs
no budget. Everything downstream is unblocked the moment this lands.
