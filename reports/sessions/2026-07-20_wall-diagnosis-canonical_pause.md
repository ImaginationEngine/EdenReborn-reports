# Pause — 2026-07-20 — Wall diagnosis verified canonical (+ a co-review gap closed)

**Session type:** verification checkpoint. **No art generated, no gameplay code changed.**

## 1. Git state

| | |
|---|---|
| Branch | `release-engineering` |
| HEAD | `a71be42` — **pushed, in sync with origin** |
| Prior checkpoint | `45aaa62` (the 2026-07-19c handoff) |
| Tree | clean of tracked changes (~212 untracked scratch files, by convention) |
| CI | `secret-scan` ✅ · `grammar-validate` ✅ · `door-regression` ✅ |

## 2. Why this pause exists

The planning agent asked to confirm the **finalized wall diagnosis** is canonical in the
repo, not just agreed in chat. It is. Verified in three independent places:

1. **Master grammar** (`_cowork/docs/EDEN_PERSPECTIVE_GRAMMAR.md`) — §10f defines **G14
   VOLUMETRIC ANATOMY** as "the dominant rule; this is the WOW gap"; the §12 error ledger
   carries all four rows at their corrected status.
2. **The 2026-07-19c handoff** (`_cowork/handoffs/`) — §3 diagnosis, §4 validator state.
3. **`DOCS_INDEX.md`** current-state row — already reads "WALL DIAGNOSIS FINALIZED".

No drift between them. Nothing needed re-deciding.

## 3. The diagnosis, as it now stands in the repo

**The biome-1 walls are a flat 2-value brick texture plus a lighting vignette.** No lit
top-cap, no distinct front face, no contact shadow. It reads flat **because it is flat.**

Confirmed from Phil's annotated live capture, where each mark is a specific tell:

- **N/S:** uniform vertical mortar joints on **one flat grid** — no cap-vs-face difference
  in joint spacing. A real top-cap over a receding face would show two spacings. One
  spacing = one plane.
- **E/W:** **continuous full-height** vertical lines — a flat vertical strip carrying
  wrong-axis coursing *and* no top-cap/face break. An uninterrupted top-to-bottom line
  means nothing structural crosses it.

### Error ledger

| # | Defect | Status |
|---|---|---|
| ~~11~~ | Linear-perspective orthogonals | **DISSOLVED** — mortar joints, not perspective. No convergence exists. |
| **12** | Brick long-axis horizontal on all sides (must be vertical on E/W) | **REAL, OPEN** — machine-detected on 4/4 live E/W runs, and confirmed by eye. Folded into the #14 fix. |
| ~~13~~ | Elevation carried by decorative linework | **DISSOLVED** — no elevation to fake; the check found nothing because nothing was there. |
| **14** | **MISSING VOLUMETRIC ANATOMY** | **DOMINANT — the WOW gap, the core fix.** |

**The reconciling synthesis (do not lose this):** the atlas *does* carry a band value step
(~0.14), but only ~17% of asset-space *field* contrast survives to the player, so that step
lands at **~0.02 on screen** — which is precisely "flat texture + vignette". The earlier
lighting finding and this diagnosis are the same fact. Consequence, now encoded in G14:
value steps are specified as **on-screen** targets and back-solved to asset space, never the
reverse — and each zone needs a **break/edge**, because edge contrast survives ~4× better
than field contrast.

### Validator state

Checks A/B/C/D/E/F/G (rules G1–G9) built and green. **A3** (coursing axis) built and working
— it is what machine-detects #12. **B2** built and passing — its *surprising pass* is what
dissolved #13. **A2** (projection) is **not ready** and has been demoted to **insurance-only**
(no convergence exists in biome 1 to catch; kept as a guard for future biome art).
**A4 (G14) is NOT BUILT — it is the priority check.**

> **Green CI does not certify wall correctness.** The biome-1 walls are **known
> non-compliant** with G12/G14; known-open defects report loudly but do not gate. Please
> don't read "grammar-validate green" as walls-are-fine.

## 4. Found and fixed this session

1. **A co-review gap — the reason this report matters.** The finalized diagnosis had **never
   been published here.** No report in `reports/sessions/` mentioned the flat-wallpaper
   verdict or G14; every published report predates the finalization. The prior handoff's
   claim of "pause reports published + verified on origin" was true about the *hash* but
   **misleading about the coverage** — the hash was real, the reports under it were older.
   This report closes it. **Lesson: verify that the published set covers the finding, not
   just that the push landed.**
2. **A stale status line in the handoff** — its git-state section named the commit *before*
   its own checkpoint commit, so a fresh agent would read a hash that isn't HEAD. Corrected
   in place; git is the truth, docs are the cache.
3. **Phil's capture was recorded as observations, not as tells** — the handoff said *what*
   the marks were but not *why they prove flatness*. Now records the reasoning, so the
   diagnosis is re-derivable rather than taken on faith.

No new dated handoff was created: this session banked no new work, and spawning a duplicate
handoff would violate edit-don't-duplicate. Corrections went into the existing one.

## 5. Next — Phase 1 close-out (unchanged, still the plan)

1. **Reference decomposition — the PRIMARY deliverable.** Measure the three-zone anatomy of
   the reference set for **E/W and N/S**: top-cap height, face height, contact-shadow width,
   and the **value steps between zones**. **These numbers are the fix spec**, and they pin
   the grammar's one open parameter (face reveal depth).
   ⚠️ Reference screenshots are **measurement-only**: never generation input (cleanroom
   rule), and **never published here**. The *numbers* are clean and publishable; the images
   are not.
2. **Build check A4 (G14)** to the validator's contract — it must catch a synthesized flat
   wallpaper *before* it is allowed to judge real art. The current shipped wall is the
   canonical selftest fail case.
3. **The feathered grid** — determine whether it is per-tile lighting falloff (a tiling-seam
   artifact) distinct from the mortar joints; if so, log as its own minor defect.
4. **Annotated reference mosaic (the WOW bar) + gap table + costed plan.**

## 6. ⏸ Need from Phil — the ratification gate

**No generation happens before this.** The gate is: the **annotated mosaic** (including the
**face-depth number**) plus the **costed plan**, ratified. The core fix is **re-authored
walls** with top-cap / face / contact-shadow anatomy and correct per-side coursing (vertical
on E/W) — authored generation, so the request must state deliverable class, count, and
target values up front, per the asset probe + purchase rule.

The freeze governs everything past **A3/A4-green + one polish pass**. Correctness fixes
against a ratified, checkable spec are permitted; quality re-rolls against a rising taste
standard are not. The discriminator is mechanical: *does the work target a named validator
check?*

## 7. Context

Verification-only session — context well under the threshold for opening a new batch. The
next session can open a full craft batch at full context.
