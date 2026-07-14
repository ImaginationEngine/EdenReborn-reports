# Phase D Plan — Coherence & Environment Art (for planning review)

**Status:** PROPOSED — awaiting planning-agent review + Phil approval. No batch has started.
**Date:** 2026-07-14 · **Prepared by:** Claude Code (execution agent)
**Working spec:** the demo-readiness QA backlog (private repo). Phases A–B closed; C (living
environment) intentionally sequenced after D per the review decision trail — see Batch notes.

---

## 1. Current-state score (same rubric as the campaign-start QA)

| Axis | Campaign start | Now (est.) | Frozen bar | Gap |
|---|---|---|---|---|
| Visual coherence | 1.5 / 10 | **~3 / 10** | **6 / 10** | ~3 |
| Environment quality | 4 / 10 | **~4.5 / 10** | **7 / 10** | ~2.5 |

Why the movement: Phases A+B removed the *artificial* breaks (broken geometry, door bugs,
white-out flashes, blown-out hit lights, harsh halo, off-palette projectiles — all closed,
including the B6 hit-feedback rework and B7 particle-fade fix). Everything now sits under one
lighting model with a consistent non-blooming feedback standard. But the environment ART is
untouched: neon-cyan walls fight the muted floor, one wall tile stamped ~180×, rooms are
oversized flat voids with zero props, and asset scales drift. Those are exactly Phase D's items.
**These scores are the execution agent's estimate — Phil's eye is the judge; re-score together
at batch boundaries.**

## 2. Scope extracted (everything Phase D owns)

From the backlog D-section: D0 floor (timeboxed, ≥6.5), D1 wall retreat, D2 wall variety,
D3 wall→floor transition, D4 asset-ratio rule, D5 test-prop replacement, D6 ruin props +
tactical grid entities + fairness rule, D8 one-screen rooms, D9 natural boundaries,
D10 room-shape variety, D7 overall coherence (the sum).
Routed in: **A8** (N/S wall tiles vertically oriented — an orientation bug, lands in the wall
batch). Final batch: **O10/O11** (flat ambient + exposure/glow calibration — re-scoped from B5
to be judged against the finished environment).
Folded minor items: minimap visual polish, enemy-vs-floor contrast check, HUD readability
check, asset-cohesion audit (AC1) — all land in the final calibration batch.

**Binding constraints (locked by Phil's review):**
- **D8:** standard combat rooms → Isaac-style ~13×7-tile one-screen; current huge size
  reserved for outdoor/large designs only.
- **D10:** long/narrow/tiny single-cell variety IN scope; multi-cell 2×2/L rooms DEFERRED —
  will not be built.
- **D6:** flatness fixed by grid-placed entities (rocks/pits/blocks/hazards) governed by an
  explicit WRITTEN fairness rule (no unavoidable damage, no blocked critical paths, no
  hazards on door approaches).
- **D4:** a systematic asset-ratio rule documented in the style bible, applied forward AND
  retroactively — no per-asset eyeballing.

## 3. Ordered batches

### Batch D-1 — Rules + room geometry (pure code/doc; no art spend)
- **D8**: standard rooms to ~13×7 one-screen; whole room visible; camera bounds verified.
- **D10**: room-template pool with long/narrow/tiny single-cell shapes; difficulty-graded
  pools (Isaac's room-pool model) so variety scales later without generator changes.
- **Proposed inclusion (needs approval):** A10 room-count formula (`3.33 × depth + 5–6`,
  cap 20) — same generator code area, natural to land here; currently an unscheduled
  Phase-A remainder.
- **D4**: write the asset-ratio rule into the style bible (entity sizes as tile-multiples,
  anchored to the 64px tile and player footprint — reference-game ratios researched first),
  then retroactively rescale the known offenders (grass, scrap, keys, bombs).
- Research-first: Isaac's actual room dimensions/pool structure; entity-to-tile size ratios
  in Isaac/Gungeon.
- **Gate:** door-regression test before/after (this batch touches rooms/transitions).
- Expected impact: coherence +1 (orientation/legibility, scale sanity), environment +0.5.

### Batch D-2 — The wall pass (art + code)
- **A8 bug**: fix N/S wall tile orientation (striped-pattern read).
- **D1**: retreat walls to the moody palette (value down, desaturate toward the floor
  family) — kills the dominant neon-vs-muted mismatch, the single loudest coherence break.
- **D2**: 2–3 wall variants to break the ×180 stamp.
- **D3**: wall→floor transition band (edge tiles or light-driven shadow drop).
- **Industry-standard technique (proposed):** Godot TileMapLayer **terrain sets
  (autotiling)** for wall runs + transitions instead of hand-stamped tiles — makes D2/D3
  systematic and reusable for biome 2.
- Art: PixelLab tileset work (balance check before any spend); code: terrain-set wiring.
- Expected impact: coherence +1.5, environment +1. **Largest single coherence lever.**

### Batch D-3 — Floor to ≥6.5 (TIMEBOXED, research-first)
- **D0**: study how Moonlighter / Sea of Stars / Gungeon build premium top-down floors
  (value structure, macro-variation, detail density) and apply the proven approach to the
  existing Wang floor — rework, not restart. **Timebox: one research pass + one refactor
  pass; if 6.5 isn't cleared, escalate the decision rather than iterate.**
- Expected impact: environment +1.
- Note: judged only after D-2 (walls set the floor's value context).

### Batch D-4 — Props & tactical grid entities (the big art batch)
- **D5**: replace the seed/tree/crystal test placements with real ruin art.
- **D6**: author the alien-ruin grid-entity set — rubble/rocks (destructible cover),
  chasm/pit cells, ruin blocks (chokepoints), hazard(s) — plus ambient ruin props.
  **The WRITTEN fairness rule ships in the same batch** (doc + placement-code enforcement):
  no unavoidable damage, no blocked critical path, no hazards adjacent to doors, cover
  always flankable.
- **Industry-standard techniques (proposed):** grid-snapped obstacle placement on the room
  grid (Isaac model) via the existing ObstacleSpawners markers; blue-noise scatter +
  door-exclusion zones for ambient props; optional **residue-decal layer** (faint in-palette
  scorch/stain where enemies die — the "permanence" trick from the B7 research, cheap
  battle-aftermath readability).
- Art: PixelLab (largest spend of the phase — itemized balance check first); code: spawner
  + fairness validation.
- Expected impact: environment +1.5, coherence +1. **Largest single environment lever.**

### Batch D-5 — Natural boundaries for large/outdoor rooms
- **D9**: objects + natural boundaries (water/elevation/ruin edges) so the reserved large
  rooms read as intentional outdoor spaces (Juicy Realm reference). Depends on D-4 props.
- **Gate:** door-regression test (touches room layouts).
- Expected impact: environment +0.5.

### Batch D-FINAL — Global calibration against the finished environment
- **O10**: replace the flat uniform ambient with gradient/variation.
- **O11**: re-tune exposure + glow threshold/intensity against the retreated walls (the
  old values were calibrated for neon walls that no longer exist by this point).
- **D7 + AC1**: full-room coherence re-score; asset-cohesion audit (one palette/value/
  scale family); enemy-vs-floor contrast and HUD readability checks; minimap visual polish.
- Expected impact: coherence +0.5–1 (the "one deliberate world" unification).
- **This is the stop line: when re-score hits 6 coherence / 7 environment, Phase D is DONE.
  No further polish inside this phase.**

## 4. Cross-batch rules
- Every batch: verify in the LIVE game, screenshot evidence, Phil review, commit+push at
  the batch boundary, re-score both axes before opening the next batch.
- Door-regression test mandatory around D-1 and D-5 (and any other room/door/transition
  touch).
- All PixelLab generations follow the style-bible cohesion cues + readability gate;
  credit-balance check before each art batch.
- Score estimates above are directional; cumulative path is ~3→6+ coherence and
  ~4.5→7+ environment if each batch lands its estimate.

## 5. Out of scope (unchanged)
Secret rooms · shop item audit · input-scheme options · jump feel · dynamic shadows (B2,
post-D) · multi-cell room shapes · monetization-related work · damage vignette (parked).

---
*Review requested: batch order, the A10 inclusion proposal in D-1, the terrain-set and
residue-decal technique proposals, and the timebox terms on D-3.*
