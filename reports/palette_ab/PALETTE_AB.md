# Palette A/B — Direction Mockups for Ratification

**Purpose:** the owner ratifies the palette pole by comparing IDENTICAL scenes (same
generation seed, same layout, same entities, same frame timing) rendered in both
directions. Remap-based — no new art was generated; both directions are runtime
re-skins of the same tiles, so the comparison is fair by construction.

## Direction definitions (PROVISIONAL HSV deltas)

**A — dark field, warm-vivid accents** (the dark-with-accents pole; evidence-safest per
the market research):
- Field: walls hue 199° (blue-teal), mean value ~0.31, sat ≤0.55; floor blue-teal at
  ~41% mean luminance (walls one value step below floor — the genre value idiom).
- Accents: warm light temperature — ambient warmed to (0.70, 0.67, 0.62), player warm
  (gold) pool energy 0.30 → 0.45. Field saturation restrained.

**B — bright warm-vivid field** (the Juicy Realm pole, "sunlit ruin"):
- Field: walls remapped hue → 40° (warm sandstone), sat ≤0.40, mean value → 0.52; floor
  remapped hue → 45°, sat ≤0.40, mean value → 0.58. Walls still a value step below floor.
- Light: ambient lifted to (0.85, 0.80, 0.70); cool fill pool reduced 0.50 → 0.30 so
  accents keep the top contrast band.

## The shots (`palette_ab/`, all same seed=777)
| Scene | A | B |
|---|---|---|
| 1. Standard room, idle | `a_1_idle.png` | `b_1_idle.png` |
| 2. Combat with enemy attack telegraph | `a_2_combat.png` | `b_2_combat.png` |
| 3. Treasure room (pedestal beam) | `a_3_treasure.png` | `b_3_treasure.png` |
| Grayscale strip (capsule-convention check) | `a_gray_strip.png` | `b_gray_strip.png` |

## Observed finding the decision should weigh
In scene 2, the enemy's green telegraph line is **clearly readable in B** (dark line on
bright field) but **effectively invisible in A** (value-match against the dark teal
field) at the identical frame. This is the research's central claim showing up live:
the palette pole is survivable either way, but **the reserved attack-telegraph color
must own the top contrast band for whichever pole is chosen** — direction A requires a
brighter/higher-saturation telegraph treatment; direction B requires keeping the field
from competing with accents. Neither direction passes readability "for free."

## Process notes
- Both directions produced with the remap machinery only (zero art generations).
- A determinism bug was found and fixed while building this (the generator's direction
  shuffle used the global RNG, so identical seeds produced different layouts — now a
  seeded Fisher-Yates; regression suite green before/after).
- On ratification (A / B / adjusted hybrid), the wall remap re-aims to the chosen
  constants and the held wall-art batch unlocks with the readability system binding.
