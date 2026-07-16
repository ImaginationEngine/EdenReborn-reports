# D-2b Wall Autotile Completeness — Acceptance Matrix (2026-07-16)

Co-review evidence for Phil's 2026-07-16 failure inventory (door junctions +
corners). All captures are live in-game close-ups (World.tscn, seed 11, 2×
camera zoom), one per failure class, taken by the same rig before and after.

## Failure inventory → resolution

| # | Failure class (Phil's capture) | Root cause found | Fix | Evidence |
|---|---|---|---|---|
| 1 | End-caps missing/wrong both sides of W and E doors | One generic horizontal-art cap tile used at all 12 junction contexts | 4 direction/side-specific V end-caps (left/right art × above/below door) | `after/BaseRoom_door_W.png`, `after/BaseRoom_door_E.png`, `after/RoomTiny_door_W.png` |
| 2 | Vertical cells above E door receiving horizontal-context tiles | Same generic cap (horizontal black-gap band sideways) | E-wall caps carry V-right art; door gap cells now empty (frame fills them) | `after/BaseRoom_door_E.png` |
| 3 | Inner corners (SW, SE): coursing doesn't wrap | SW tile carried **unflipped NW art**; SE carried a third NW-style art | SW recomposed (NW flip + S-band splice from authored H-run); SE = mirror | `after/*_corner_SW.png`, `after/*_corner_SE.png` |
| 4 | Outer-corner notch (NE region) | NE used a retouched-source corner whose vertical arm misaligned with the V-right column | NE = mirror of the authored NW corner | `after/*_corner_NE.png` |

`before/` holds the same-rig captures of the failing classes for side-by-side.

## What changed (by construction, not spot-fix)

- Wall terrain set (MATCH_SIDES) completed to **full ring coverage**: every
  side-bit signature a rectangular ring with 2-cell door gaps can produce now
  has an exact tile — runs, 4 corners, and the 4 door end-cap signatures
  [L], [R], [T], [B] that were missing.
- Painter now paints the ring **exclusively via `set_cells_terrain_connect`**
  (one call, door gaps left empty), plus a deterministic east-column art
  override (MATCH_SIDES cannot distinguish left/right wall art — both [T,B]).
- All 4 live room templates re-baked; door-gap cells are empty by contract
  (the door frame sprite covers them — measured 126×74 px vs the 128 px gap).
- **New orientation invariant test** (GdUnit4, 3 seeds, CI): derives the
  expected tile for every ring cell from its computed neighbor configuration
  and asserts the placed atlas coord matches exactly — door-junction cells
  must carry their direction/side cap, corner cells their matching corner.
  This closes the blind spot of the old peering-bit test.

## Test results (local, full suite)

- door_regression (A9 matrix, 16 cases): **PASSED**
- wall_adjacency (3 seeds): **PASSED**
- wall_orientation (NEW, 3 seeds): **PASSED**

Zero PixelLab spend — all new tiles composed from the authored sources.

## Capture index

19 after-captures across the 3 template types present on the seed-11 floor
(BaseRoom 4 doors + 4 corners, RoomNarrow N/S doors + 4 corners, RoomTiny
W door + 4 corners). Door captures frame both flanking caps.
