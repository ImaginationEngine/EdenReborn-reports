# Pause report — 2026-07-19 — W-SPEC amendment executed: perspective grammar + CI enforcement

## Git state

- `release-engineering` @ **`4fbf0f5`**, tree clean of tracked changes, pushed.
- CI: three gates now — `door-regression`, `secret-scan`, and the **new `grammar-validate`**
  (first run in flight at publish time; result lands in the chat message with this report).
- **PixelLab spend UNCHANGED at 160/175. Nothing generated — the entire amendment is 0 gens.**

## What landed (one commit, `4fbf0f5`)

| Artifact | What it is |
|---|---|
| `EDEN_PERSPECTIVE_GRAMMAR.md` | **The master artifact** — formal, biome-agnostic perspective/structure grammar |
| `EDEN_WALL_SPEC.md` | Its **first application**: the biome-1 parameter block (born demoted — no standalone wall spec ever existed in-repo; the plan lived in the planning channel) |
| `tools/validate_grammar.py` | Checks A–G + a selftest that synthesizes **all ten** ledger failure classes and must catch each |
| `grammar-validate` workflow | Third CI gate: **grammar violations are now a failing build** |
| D12 (roadmap decision log) | The ratified tier split, recorded |
| GTM devlog backlog line | The devlog note, logged only, per the amendment |

## The tier split, as executed

**Grammar = god-tier.** Ten rule groups (view convention, band anatomy, run-axis, corners, lit
lip, openings, geometry coverage, ground shadow, value interface, occlusion-reserved), stated in
per-side-neutral coordinates so Biome 2+ and the parked Cainos eval inherit by supplying a
parameter block, never by re-deriving rules. Every rule carries **statement / why (canon or
measured citation) / machine check / the ledger failure it prevents**. The failure-mode ledger
maps all ten historical error classes — sideways E/W coursing, inverted south anatomy, corner
substitution, opaque shadow band, silhouette-flattening lip, the 126px frame sliver, derived-join
bright seam, broken coursing at boundaries, banded-stripe elevation, atlas-space tuning that
never reaches the player — each to its rule, its landed fix commit, and its validator check.

**Art execution = WOW-tier by construction.** Spec-satisfaction + one polish pass, then stop —
recorded in the grammar's authority block and as D12. No art was touched this session.

**G10 (occlusion/Y-sort) is RESERVED, deliberately.** The validator wires it as an explicit
always-SKIP so the wiring is visible in every report but enforcement cannot freeze a convention
nobody ratified. It arms the moment the rule lands.

## Enforcement immediately improved the grammar — three findings

This is the god-tier loop working as intended: the first enforcement run corrected the
grammar's own check definitions, and the doc records each amendment inline.

1. **Step-magnitude seam metrics cannot police pixel-art coursing.** Authored course lines are
   sparse *hard* value steps — locally indistinguishable from seams. The shipped W run lands the
   body's hardest authored course line exactly on the tile boundary (via the half-tile roll),
   and the lit lip is *designed* to break at mortar. Any "boundary step vs interior median"
   metric fails the material itself. G3.iii is amended to **period-identity**: a single-body run
   must repeat exactly with period T — catches every assembly error (wrong tile, wrong roll,
   phase drift), never flags authored structure.
2. **The door row's cap→frame joins are authored discontinuities** (the terminator groove that
   wraps the jamb end). The correct machine checks are groove *presence* on cap gap-edges plus
   frame lip *level*, with join brightness policed by profile identity (check E) — not raw
   line continuity.
3. **Capture mode needed wall-evidence gates.** On screen, the wall's lit anatomy sits at its
   *outer* edge and decays to near-black at the floor seam — a strip anchored above the floor
   edge samples the darkest zone and reads "unlit". The wall→floor edge must be the *last*
   absolute step (argmax locks onto lip-vs-void, the bigger step). And a detected band only
   counts as wall if a **lit floor exists below its edge** — otherwise it is the neighbour
   room seen past an unlit wall, and judging it would fail art for scene lighting.

## Results

| Mode | Result |
|---|---|
| `--ci` (deterministic + selftest) | **78 PASS / 0 FAIL / 1 SKIP** (the SKIP is reserved G10) — shipped set is grammar-compliant |
| `--selftest` alone | **10/10 synthesized ledger violations caught** — the validator validates itself before judging art |
| `--captures` on the acceptance matrix (24 captures) | **76 PASS / 0 FAIL / 3 SKIP**, every SKIP self-explaining (unlit wall, band too thin, G10) |

The capture run means the eye-check and the machine-check now see the same evidence: the same
24 PNGs Phil reviews are machine-checked for band axis, lip presence, and shadow presence, with
FAIL reserved for unambiguous violations — a SKIP is never counted as a pass.

## Devlog note (logged, not acted)

The grammar + validator story ("teaching an AI pipeline perspective grammar, with CI
enforcement") is logged in the GTM devlog-track backlog for the revised-thesis channel, to be
produced with the campaign batch — not now.

## Still open (unchanged from the previous pause)

- **Phil's lighting judgement** (from the D-3R close report): is ~83% field-contrast loss the
  intended moody look? That answer decides whether **M8.2** tunes or documents the lighting —
  and M8.2 is now doubly load-bearing: the grammar's G9 defers to it, and the capture validator
  inherits its thresholds.
- M8.4 skill refresh · seed-777 wide + re-score (after the lighting question) · store-page
  readiness · G10 ratification when Phil rules on south-wall overlap.

## Need from Phil

Nothing blocking on the amendment itself — it is landed, green, and enforced. Two standing
questions ride along: the lighting verdict above, and (when convenient) the G10 occlusion
convention, which arms validator check C.
