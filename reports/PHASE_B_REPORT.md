# Eden Reborn — Phase B Report (Lighting & VFX Tuning)

**Date:** 2026-07-12 · **Branch:** `release-engineering` · **Status:** B0/B3/B1/B4 done & verified live; B2 deferred, B5 held for live review.

Phase B's goal: kill the "artificial" feel of the lit game and move it toward natural/premium
(Moonlighter-atmospheric). All fixes were built and verified by booting the **real** game, not a
look-dev rig, then held for the art director's eye.

> Sanitized report — describes status, decisions, and file references only. No source, no secrets.

---

## B0 — Systematic "artificial offender" sweep (the punch-list)

Rather than fix only the two offenders flagged by eye (white-flash death + harsh halo), the sweep
audited the whole lit scene (live game + code) and inventoried **14 offenders**, triaged by severity
and mapped to the fix that owns each. Highlights:

- 🔴 **Death white-out** — dying enemies blew out to a pure-white blooming "sun".
- 🔴 **Hard/linear light falloff** — the player pool had a defined cone/circle edge, not a natural soft fade.
- 🔴 **Exposure/glow blowout** — the bright wall tiles bloom into synthetic neon.
- 🟡 A whole **pure-white-flash family** (enemy hit, player damage, boss hits), a **murky-green player pool**
  (warm + cool lights mixing), **flat ambient**, and **projectiles tinted by ambient light**.
- ⚪ Minor: dash/mimic white flashes.

Verified **not** offenders (already correct/natural): static-baked scatter détail, and the combat
prototype's "premium" projectile/telegraph/bomb path — which already follows the readable-element
lighting rule and served as the reference pattern.

The full 14-row table (severity + owning fix) lives in the private working spec.

---

## B3 — Enemy death: white-out → in-palette dissolve (the #1 offender)

**File:** `scenes/Enemy.gd` (death path + new helpers; ~90 lines added).

Replaced the pure-white "vaporize" with a death that reads as *this creature dying in this world*:
- samples the creature's **own colour** from its sprite (weighted average, saturation-lifted;
  achromatic sprites fall back to the biome's alien-green);
- emits a **colour-matched particle burst + a soft expanding poof-ring** in that colour;
- the sprite **dissolves** (clears the white hit-flash, tints to its colour, implodes + fades, eased)
  instead of swelling to white.

Also softened the **enemy hit-flash** from a nuclear white overbright to a bright-but-natural flash
(it was blowing out and blooming on every hit). Player-damage / dash / mimic flashes were left for the
art director's call (they touch player feel).

**Verified live:** death now reads as a coloured poof-dissolve, no white sun.

---

## B1 — Player halo: harsh pool → soft natural glow

**File:** `scripts/WorldLighting.gd` (all tunable constants — the art director dials finals).

- **Falloff shape:** the pool's radial gradient was a 2-stop linear ramp (visible hard edge). Replaced
  with a soft multi-stop taper — full core fading to a soft transparent rim, like natural light.
- **Intensity + colour:** the warm gold aura (the harsh part, and the main source of the murky green
  where it mixed with the cool fill) was **cut ~76%**; the cool fill eased down. The room stays visible
  via ambient + the cool fill, so it isn't a black void.

**Verified live:** harsh green halo → soft neutral glow, no hard edge. The pool is intentionally subtle
now; the two energy knobs are exposed for the art director to dial to taste.

---

## B4 — Projectiles read true colour under lighting

**Files:** `scenes/Tear.gd`, `scenes/EnemyProjectile.gd` (small material additions).

Player shots and vanilla enemy projectiles had no lighting-exempt material, so live lighting tinted
them muddy. Applied the **readable-element rule** (lighting-exempt so they render their true colour),
matching the prototype's premium-bolt path. **Verified live:** shots now pop as true cyan / true green.

---

## B2 — Dynamic shadows: RESEARCHED, DEFERRED (a "want", not a demo blocker)

Researched the proven Godot-4 approach (shadow-enabled 2D lights + occluders, tileset occlusion for
walls, outward-cull, fast crisp pixel-art shadow filter) and its perf profile + top-down pitfalls.

**Decision: defer to post-Phase-D.** Two reasons: (1) the player-following light would cast shadows
that *sweep* as the player moves — the "reads side-view, not top-down" risk; (2) the environmental
props those shadows would fall on don't exist yet, so the payoff now would be walls-only. Revisit once
props exist and the walls are retreated. Research is banked so it needn't be redone.

---

## B5 — Overall calibration: HELD for live review

The gestalt tune (ambient level/gradient, glow threshold, exposure, colour temperature) toward
Moonlighter-natural is **held until the art director reviews B1–B4 in motion**, because it interacts
with the pool-energy values he'll dial. One objective sub-win is already queued: raise the glow
threshold / trim exposure so the bright walls stop blooming into neon (also helped by the planned wall
retreatment in the next phase).

---

## Where things stand

| Item | Status |
|------|--------|
| B0 sweep | ✅ 14-offender punch-list |
| B3 death VFX | ✅ done, verified |
| B1 halo | ✅ done, verified (dial to taste) |
| B4 projectiles | ✅ done, verified |
| B2 shadows | ⏭ deferred post-Phase-D (research banked) |
| B5 calibration | ⏸ held for live review of B1–B4 |

**Next:** art-director live review of B1–B4 (dial the pool) → B5 calibration against those values →
then the living-environment / coherence phases per the working spec.

*Everything above was verified by booting the real game (screenshots captured for each fix). Detailed
before/after imagery and the full offender table are kept in the private repo.*
