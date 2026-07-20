# Pause report — 2026-07-19 — W-SPEC Phase 1: grammar self-audit, three new rules

## Git state

- `release-engineering` @ **`83f41c8`**, tree clean, pushed. **0 gens; no art changed** — study phase.

## The uncomfortable part first: the grammar codified the defect

G3.ii read: *"masonry coursing remains gravity-horizontal on every side."* That was not a rule —
it was **the defect, written down as law**. I derived it by reading the shipped walls and
codifying what they did. The validator built from that grammar was therefore *structurally blind*
to it: **the machine agreed with the art because the art had taught it.**

That is worse than an uncovered gap, and it is why both defects sailed through a green CI gate.
The retraction is left visible in the doc rather than quietly edited.

**Standing consequence, now clause §13.1:** *a rule that merely describes the current art is not
a rule.* Every rule must derive from the ratified projection plus the reference set, justifiable
**without reference to what we shipped**. Areas with no check are now listed explicitly, and a
defect found by eye that the validator passed is a **validator bug of equal severity to the art
bug** — both get ledger rows.

## The two stacked defects

**#11 PROJECTION MISMATCH (dominant).** The wall art carries **linear-perspective orthogonals** —
converging vanishing-point lines — inside a **parallel-projection** game. The converging lines
contradict the flat parallel floor and equal-height walls. The eye reads that contradiction long
before it can name it, which is exactly the "reads fake / off" report. They are also currently
doing the elevation work that real anatomy should own.

**#12 COURSING AXIS.** Brick long-axis is **horizontal on all four sides**. I confirmed this
independently from the shipped atlas at zoom — W and E runs plainly carry horizontal bricks. It
must be **perpendicular to each wall's inward normal**, i.e. **vertical on E/W**.

This is the same error class as ledger #1 (bands rotated) and #2 (south run upside down), one
level deeper: per-side **anatomy** was fixed while per-side **material** was not — which is why
E/W still read weakest after v3.1/v3.2 fixed everything else about them.

## Three rules added

- **G11 PROJECTION CONSISTENCY** *(top priority)* — one ratified parallel projection; no vanishing
  point; wall height position-invariant; depth from band anatomy + contact shadow, **never** from
  drawn perspective lines. **Convention ratified as `EDEN-TD-PARALLEL`**: top-down parallel with a
  thin oblique reveal on the interior-facing face only. Not isometric, not dimetric, not one- or
  two-point. The Isaac/Moonlighter interior grammar.
  *Check A2:* fit long-line angles per wall band; PASS only on a single parallel family; FAIL on a
  vanishing point, on angle drift along the run, or on position-variant band widths.
- **G12 COURSING AXIS** — masonry long axis lies **along the run axis**. N/S horizontal, **E/W
  vertical**. Coursing rotates with the wall because in top-down parallel you are looking at a
  solid extruded along that run.
  *Check A3:* dominant edge orientation over the brick-face zone, sliced along the side's own band
  axis; FAIL on mismatch.
- **G13 STRUCTURAL ELEVATION** — height must **survive suppression of decorative linework**. The
  orthogonals were carrying the depth, so removing them must be paired with strengthening the
  anatomy, not simply deleting lines and leaving a flat slab.
  *Check B2:* line-suppress the band, re-run the §2 signature; FAIL if separation collapses.

Judging viewpoint is now a **binding clause**: room scale, player's view. Both defects survived
tile-level review and were only caught at room scale.

## Honesty about the gate

**Checks A2 / A3 / B2 do not exist yet.** A green `--ci` therefore **does NOT certify projection
or coursing** — it certifies only the rules that have checks, and the two defects sit precisely in
the two areas that had none. This is now written into the enforcement contract, and
`EDEN_WALL_SPEC`'s compliance table marks the shipped biome-1 walls **KNOWN NON-COMPLIANT** with
G11/G12 pending the art fix. "Validator green" must not be cited as wall correctness until those
checks land.

## A measurement trap worth keeping

My first automated G12 check reported E/W as *"long-axis vertical"* — i.e. **correct** — because
slicing the same rows for every side measures the **band structure** on E/W, not the coursing. The
vertical lit-lip band swamped the brick signal. A zoomed visual showed the bricks are plainly
horizontal. Recorded in the rule: the slice must follow each side's own band axis, and automated
orientation verdicts need visual confirmation before they are trusted.

## Next (Phases 2–3, not started)

Build A2/A3/B2 → then the art fix: re-author the wall material to `EDEN-TD-PARALLEL` with correct
per-side coursing and anatomy-carried elevation. Note the ladder's "no further wall attempts this
phase" constraint governs *purchases*; this is a projection-correctness fix, so the scope call is
Phil's.

## Need from Phil

1. **Confirm `EDEN-TD-PARALLEL` as ratified** (or correct it — pure top-down vs a slight 3/4
   oblique changes what A2 enforces).
2. **Scope call:** does fixing #11/#12 count against the standing "no further wall attempts this
   phase"? It is correctness, not a quality re-roll — but it is wall work.

The lighting verdict → M8.2 queue is unchanged behind this.
