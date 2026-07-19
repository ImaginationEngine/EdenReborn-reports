# Pause report — 2026-07-19 — v3.2 walls LIVE + fix D completed + M8 syllabi

Continues `2026-07-19_d3r-v32-mining_pause.md` (same day, after the frames decision landed).

## Git state

- `release-engineering` @ **`d617e52`**, tree **CLEAN**, **pushed and in sync** (no ahead-count).
- Suites green: **door regression 16/16**, **GdUnit4 7/7 cases, 0 failures**, 3 seeds.
- **PixelLab spend UNCHANGED at 160/175 — nothing generated. Every deliverable below is 0 gens.**

## What landed

| Commit | What |
|---|---|
| `664820f` | **v3.2 walls SWAPPED LIVE**; fix D completed across painter, runtime and test |
| `56ca22a` | M8.1/M8.3/M8.6 craft syllabi; door frame gap coverage fixed |
| `84c414f` | M8.7 syllabus-first standing rule; M8.5 Decision Log D10 |
| `d617e52` | Session checkpoint + handoff |

## The swap — fix D was only half-landed, and hid in two more places

The v3.1 work added row-3 SOUTH art but was never swapped in, so nothing ever *placed* it.
Completing that turned up two further carriers of the same bug:

1. **Painter** — needed a `SOUTH_MAP` override, the south-row analogue of the existing east-column
   one. North and south share the same terrain signature, so the matcher resolves the south edge to
   the north pool and runs the wall anatomy upside down.
2. **Runtime** — inactive south edges were being sealed back to solid with **north** run tiles,
   reintroducing the same upside-down anatomy on **every dead end**. Invisible until the art had
   enough contrast to show it.
3. **Terrain** — row-3 coords registered as art-only (positional placement, out of the terrain
   pool), for the same reason the east-side art is.

The atlas grew 8×3 → 8×4, so **coords changed** and a re-bake *was* required — the standing
"don't re-bake on a pixels-only swap" note did not apply here.

**The orientation test failed 100 assertions against correct output**, because it asserted the
*pre-fix* expectation: south cells carrying north art. It was updated to a **tighter** invariant —
north and south each require their own oriented pool and their own door caps — not a loosened one.
The test's own header licenses exactly this: if it passes while a capture shows a seam, fix the test.

## Door geometry — the suspicion was real

The frame sprite rendered **126.0px into a 2-cell 128px baked gap** (sprite scale × node scale on a
1120px texture). Fixed to exact coverage. v3.2's contrast would have exposed the sliver that v3.1's
low-contrast wall hid. **This is a geometry bug — no amount of generated art would have fixed it.**

## M8 — three syllabi distilled in parallel, 0 gens

Delegated to three parallel research agents writing their docs directly, so a large volume of source
material never entered the main working context.

**PIXEL_CRAFT_RULES** — all **four** of our independently-measured findings are **confirmed by
canon, none contradicted**. Two are refined:
- the lit lip must also be **inset from the silhouette** (a highlight sitting on the outline
  flattens form) — our version didn't specify that;
- composed-context judgment is **insufficient without correct zoom**: isolation in *scale* is the
  same error as isolation in *space*.

Two findings worth carrying: **no canonical source supplies a number** for wall/floor separation, so
our ΔV ≥ 0.25 is our own contribution rather than received craft. Conversely the greyscale + squint
test was prescribed in 2007 and we re-derived it independently — hence non-negotiable.

**FEEL_CRAFT_RULES** — companion to the feel spec (spec holds parameters, this holds the why).
8 rules already carried, **12 new**, **6 conflicts including three live build bugs**. The largest
spec gaps are structural: no knockback row (the strongest impact channel in this lineage) and no
audio row (about half of Swink's polish block).

**PIXELLAB_MASTERY** — endpoint cost table with UNCONFIRMED flags rather than guesses. It **corrects
our own repair-loop hypothesis**: 20 generations buys ONE inpaint but SIXTEEN candidates from the
style endpoint, so inpaint wins against a full tileset re-buy but *loses* against regeneration for
exploration — "inpaint to protect a banked asset, regenerate to explore". It also flags that two
official documentation surfaces **disagree** on whether the lower-terrain chaining field actually
chains; that is unresolved and must not be bet on at 40 generations.

**M8.7** generalises build-vs-search from tooling to **knowledge**: before any craft-heavy phase,
one bounded syllabus pass *before* practicing. Trailer-craft and press-kit are queued and explicitly
**not** to be run yet.

## Found in passing — flagged, not fixed

- **A hard-rule violation in live code:** the dash flash sets an overdriven brightness above the HDR
  bloom threshold, which the project bans outright, and never restores the player's tint on dash
  end. A surviving instance of a bug removed elsewhere in an earlier rework. Raised as its own task.
- Two related feel findings: projectile hits fire hitstop+shake on **every** hit against a spec that
  says destructive confirms only; and dash i-frames are shorter than the dash itself, leaving the
  tail of a committed dash vulnerable.
- The wall terrain builder's collision calls all error (they run before the source is attached).
  Confirmed pre-existing and not gameplay-affecting — code walls carry runtime collision.

## Ledger correction

The purchase record claimed **6 full-interior tiles**. Quadrant classification shows three of those
six are **floor** tiles, statistically identical to our own ratified floor. Real full-wall yield is
**3**. Still 3× the previous set so the gate verdict stands — but any plan assuming six wall bodies
is wrong. Corrected in the ledger.

## Re-scores

**None yet** — the swap is live and green, but the close-up matrix has not been captured, so there is
no new scored artefact to compare. Re-score rides with the matrix.

## Still open

Derive door frames from the v3.2 wall family (0 gens, must cover the 128px gap) → **full close-up
matrix** (N/S/E/W runs, 4 corners, every door both sides, + seed-777 wide), judged at **gameplay zoom
as well as close-up** → per-side self-grade → provenance close (frames are the last tainted class) →
readiness check. Then M8.2 (lighting model) and M8.4 (skill refresh).

## Need from Phil

**Nothing blocking.** The frames decision (derive, 0 gens) is being executed; the best-of verdict
needs his eye on the matrix, which is the next window's first deliverable and arrives with the
evidence attached. v3.1 remains banked and one command away.
