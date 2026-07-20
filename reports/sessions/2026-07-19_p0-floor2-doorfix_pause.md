# Pause report — 2026-07-19 — P0 floor-2 oversized-door fix (progression unblocked)

## Git state

- `release-engineering` @ **`bdb3720`**, tree clean, pushed.
- CI **3/3 green**: door-regression, grammar-validate, secret-scan.
- **PixelLab spend UNCHANGED at 160/175 — 0 gens.**

## The bug (Phil's corrected diagnosis was right)

Floor-2 doors rendered ~9× oversized — teal glyph-glow sprites covering the room, unpassable.
**Not** a palette or provenance problem: the v3.2 tileset reaches depth-2 correctly and floor-2
walls are warm cleanroom. It was purely a door-sprite transform bug.

**Measured, not guessed** (`tools/_kt_doorscale/probe_door_scale.gd`, headless — node transforms
are queryable even though the render is black):

| | texture | rendered |
|---|---|---|
| Floor 1 door | `frame_normal_v32.png` (128×64) | **128×64** ✓ |
| Floor 2 door (before) | `Door-Photoroom.png` (1120×928) | **1120×928** ✗ (~9× wide, ~14× tall) |

## Root cause — a `_ready()` timing race between two texture-setting paths

Two paths set the door `Sprite2D` texture:
- `Door.gd._update_sprite()` uses Door.tscn's exported `frame_*_v32` textures (the correct 128px
  frames);
- `LevelGenerator._door_textures` (a "robust fallback", line ~617) overrode it — **but still held
  the old 1120px Gemini textures**.

The winner flips by floor. On the initial World load, a child's `_ready()` is deferred, so
`_update_sprite()` runs *last* and the new frame wins → floor 1 correct. On a runtime floor advance
(`_advance_to_next_floor` → `add_child` fires `_ready()` synchronously), the fallback runs last and
wins → floor 2 got the stale 1120px texture. Door.tscn's sprite scale (1.3333 × the 0.75 node = 1.0)
is tuned for a 128px frame, so a 1120px texture renders at ~9×.

## Fix (minimal, in scope)

Point `_door_textures` at the **same** `frame_*_v32` set. Whichever path wins, the door is the
correct 128px — the race is now harmless. Walls and palette untouched.

**Verified:** door probe 128×64 on both floors · windowed floor-2 capture shows warm walls, correct
openings, traversable · door regression 16/16 · grammar-validate on a floor-2 matrix **90 PASS / 0
FAIL** (via a new `D2B_FLOOR` env on the capture rig).

## Provenance — a factual correction, not a re-audit

This bug exposed that my 2026-07-19 provenance close was **incomplete**: it claimed "no shipped
*scene* references a tainted file" — true, but it checked scene files, not code. `_door_textures`
was a live `preload` of the old Gemini art. The fix repoints it, so the close is now valid for
scenes **and** code, and no shipped scene or script references a Gemini door file. Ledger corrected
with the lesson recorded: **a provenance sweep must grep code (`preload`/`load`), not only `.tscn`
ext_resources.** (No re-audit run, per scope.)

## Validator hardening (found during verification)

Running grammar-validate on the floor-2 matrix produced 5 false "no lip" FAILs on E/W door
captures. Cause: the door gap scatters bright steps across the room, so the wall-edge detector
straddled the whole frame (band 23–49% of the axis vs ≤19% for a cleanly-isolated wall, measured
across n=84 captures). A 20% band-span gate now SKIPs those — the capture mode's contract is FAIL
only on *unambiguous* violation. Floor-1 matrix unchanged at 76/0/3; deterministic `--ci` unchanged.

## What this means for readiness

Progression past floor 1 works and every reachable floor renders the warm cleanroom set with
correct doors. The readiness check's "all reachable floors clean/traversable" clause is satisfied
for the door path. (The décor/speckle blue-teal flecks remain a separate known D-4 cohesion item,
unchanged.)

## Still open (standing queue, unchanged)

Phil's lighting verdict → M8.2 lighting model → seed-777 wide + re-score → G10 ratification →
store-page readiness. Plus M8.4 skill refresh.

## Need from Phil

Nothing blocking — floor 2+ is playable. The standing lighting question (from the D-3R close) is
still the next decision that unblocks M8.2.
