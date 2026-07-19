# Pause report — 2026-07-19 — D-3R closed: frames derived, provenance CLEAN, and a finding that outranks the tiles

## Git state

- `release-engineering` @ **`8380b49`**, tree clean, pushed and in sync.
- Suites: **door regression 16/16**, **GdUnit4 7/7, 0 failures**, 3 seeds.
- **PixelLab spend UNCHANGED at 160/175. Nothing generated. Everything below is 0 gens.**

## What landed

| Commit | What |
|---|---|
| `f10fa7a` | Hard-rule fix: overdriven modulate cleared from the flash class + sweep |
| `06d7249` | FEEL_SPEC F9 knockback + F10 audio; FEEL PASS inbox FP1–FP6 |
| `5c8d978` | **Door frames derived from the v3.2 wall body** (0 gens) |
| `ec22242` | **Provenance audit CLOSED** for the biome-1 environment set |
| `8380b49` | **On-screen measurement finding** written into the style bible |

---

## 1. The finding that outranks the tiles

I measured the shipped walls across the full 24-capture in-engine matrix rather than trusting the
atlas numbers. **Most of the value structure this campaign tuned does not reach the player.**

| | asset space | on screen | survives |
|---|---|---|---|
| wall/floor ΔV (**field** contrast) | 0.290 | **0.049** median | **17%** |
| lit-lip ÷ face (**edge** contrast, ratio) | 2.78 | **1.29** median | **47%** |
| lit-lip − face (edge contrast, absolute) | 0.401 | 0.045 | 11% |

**Two consequences, both now binding in the style bible:**

**(a) ΔV ≥ 0.25 is an ASSET-SPACE rule and does not by itself guarantee readability.** The shipped
wall clears it at 0.290 and still reaches the player at 0.049 — a fifth of the target band. Future
readability claims must be measured from an in-engine capture, not the atlas. A reusable tool for
that is now in the repo.

**(b) EDGE contrast survives lighting roughly 4× better than FIELD contrast** (47% vs 11%). Lighting
is broadly multiplicative, so a *ratio* is preserved where an absolute *difference* collapses — and
low-luminance perception is closer to ratio-based. This is the **third independent confirmation** of
"elevation is sold by edge shape", and it upgrades the mined lip from a nice detail to **the primary
readability mechanism**.

**Said plainly: this partly undercuts the premise of the value-normalisation work.** Normalising the
body to hit ΔV 0.25 was correct and cheap, but the lip was doing most of the work all along. That is
the strongest possible argument for sequencing **M8.2 (the lighting model) ahead of any further tile
work** — the lighting transform, not the tile, is the binding constraint on environment readability.

**Caveat, stated so the number isn't over-read:** the matrix samples walls at varying distance from
the player's light, so the median mixes lit and unlit wall. Even the best-lit wall/floor boundary in
the entire matrix only reaches 0.098. Two method traps were nearly fallen into and are documented:
comparing whole-frame darkest-third to brightest-third conflates light falloff with material
contrast, and the first edge detector locked onto the wrong edge entirely (the negative ΔV it
produced is what exposed it).

---

## 2. Door frames — derived, 0 gens

Frames are now **the same brick as the wall by construction** rather than by prompt: derived from the
normalised v3.2 body with the mined lit-lip and navy-outline colours, so they cannot drift the way a
separately-generated frame would. That was the failure that killed the v3.0 set.

Authored at native **128×64 = exactly the 2-cell baked gap**, drawn 1:1, replacing a 1120×928 sheet
squeezed to 126×74. One texture serves all four directions since the scenes already rotate it.
**Reads as an opening, not a hole:** the top course and lit lip run the *full* width, continuous with
the flanking cap tiles, so the wall's top plane passes unbroken across the doorway and only the
passage below goes dark. Per-type accent on sill and jambs (gold treasure / cyan shop / violet boss).

**Two defects caught by measuring rather than eyeballing:**
1. **Operation order.** The wall applies the lit lip *before* the course flatten-blur, so the blur
   averages its peak down. Applying it after left the frame's top course at V 0.945 against the
   wall's 0.725 — a bright seam exactly where frame meets wall. Reordered; frame and wall now
   measure **identical** at the join.
2. The sill accent was the brightest element on screen (0.945), inverting the value hierarchy.
   Capped under the wall's own lip.

---

## 3. Provenance — CLOSED for the biome-1 environment set

Walls CLEARED · floor CLEARED · **door frames CLEARED** · décor/speckle forensically cleared. No
shipped scene references a Gemini or Isaac-lineage file.

**The claim is deliberately scoped.** "Cleanroom end to end" covers the biome-1
**environment/tileset visible set**. It does **not** cover three WEAK-grade rows still open —
health UI, a spikes placeholder, and one enemy placeholder. Those sit outside the environment batch
and are slated for replacement anyway, but they are **not** cleared, and the ledger now says
explicitly that a store-page asset must not feature them. Claiming blanket clearance would have been
exactly the kind of stale status line the checkpoint rule warns about.

Housekeeping recorded, not done: retired source tilesets and the now-unreferenced Gemini door files
are still on disk. Nothing references them so they cannot reach a build, but they should be deleted
before any source-tree distribution.

---

## 4. Per-side self-grade vs Phil's baseline

Baseline was **N 9/10 · S 6/10 · E/W lowest**. Graded in two registers, because the finding above
makes a single number misleading:

| Side | Baseline | Asset space now | On screen now | Why |
|---|---|---|---|---|
| N | 9 | **9** | ~6 | Anatomy unchanged; materially better (real brick + authored lip) |
| S | 6 | **8.5** | ~6 | **Fix D finally landed live** — south had its own art since v3.1 but nothing placed it. Now structurally equal to N |
| E | low | **8** | ~6 | Side-face treatment + rolled coursing + lip on the outward edge |
| W | low | **8** | ~6 | As E |

Asset-space runs now measure within **0.003** of each other across all four sides (0.217–0.220) and
corners within 0.002 — the per-side inconsistency Phil graded is gone. **The on-screen column is
flat at ~6 not because the art is flat but because the lighting compresses all four equally.** That
is the whole argument of §1 in one row.

Corners verified from the matrix: the top course **mitres** around every corner with no crossing
bands — fix C holds live.

---

## 5. Hard-rule violation — fixed, and it had siblings

The dash flash used an overdriven brightness above the HDR bloom threshold, which the project bans
outright, and never restored the player's tint. Fixed onto the sanctioned clamped shader path, with
restoration made deterministic at the single exit from the dashing state.

**A pattern grep found one site. A proper scan of every colour literal in shipping code found
eleven** — including one hiding inside a `lerp` target that no reasonable regex would catch. Three
were unambiguously the banned class (white hit/reveal flashes, two of them in enemy code the earlier
rework had missed) and are fixed. The other eight are **coloured telegraphs** — boss state tints, a
charge telegraph, a bomb blink — where brightness is doing design work, so clamping changes the
intended look. Those are logged for the feel pass rather than changed blind. The scan is now a
committed tool so the check is repeatable.

---

## 6. Feel spec

**F9 knockback** and **F10 audio** added — the two largest structural gaps the syllabus pass found
(no knockback row at all despite it being the strongest impact channel in this lineage; no audio row
despite audio being about half of the canonical polish block). F10 is flagged **blocked on
licensing, not tuning** — the current audio is all placeholder with no commercial grant.

Six conflicts logged to a FEEL PASS inbox, deliberately not actioned. **FP1 carries the fairness
note:** dash i-frames are 0.1 s against a 0.15 s dash, so the last ~50 ms of a *committed,
un-cancellable* action is vulnerable — worse than an ordinary tuning miss, because the player has
already surrendered control and can only watch the hit land.

---

## Still open

- **Seed-777 wide capture** and **re-score both axes** — note that a re-score now scores
  lighting-compressed art, so §1 should be settled first or the score will misattribute.
- **Store-page readiness check.**
- **M8.2 EDEN_LIGHTING_MODEL** — the empirical case for it is made above; the section itself is not
  yet authored. This is now the highest-value remaining item.
- **M8.4 skill refresh** — 8 concrete corrections already itemised from the API audit.
- Queued, log-only: an empirical probe of the lower-terrain chaining field at the Biome-2 gate,
  before any purchase relies on it.

## Need from Phil

**One judgement, not a blocker:** §1 says the lighting is eating ~83% of the wall's field contrast.
Is that the intended moody look — in which case the tile bar should be *lowered* and the budget
moved to lighting and edges — or is it further than intended? That answer sets whether M8.2 tunes
the lighting or just documents it. Everything else here is landed and green.
