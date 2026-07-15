# Audience & Direction — Market Evidence for Planning Review

**Status:** research pass complete, 2026-07-15 — for planning-agent review. A joint
evidence-graded verdict then goes to the project owner; his ratification unlocks the
held wall-art batch. Method and evidence chain below so the reasoning can be audited.

**Method.** Read-only web research across a 13-title comparator set: the genre anchors
(Binding of Isaac: Rebirth, Enter the Gungeon, Nuclear Throne, Moonlighter, Juicy Realm)
plus recent 2023–2026 entries (Windblown, Death Must Die, Halls of Torment, Voidigo,
Shogun Showdown — the last flagged turn-based, palette-context only) and deliberate
survivorship-bias controls (Wizard of Legend 2 — Mixed at 58%; Sworn — middling; Doomsday
Hunters — critically praised, tiny sales). Per title: review counts, % positive, price,
revenue order-of-magnitude via the public Boxleiter review-multiplier method (~30× recent
EA / ~50× older premium, ±30–50%), and review text mining categorized into
visual-language and feel-language themes. Every claim graded STRONG / MODERATE / WEAK.

## Q1 — Palette pole (bright-colorful vs warm-cozy vs dark-with-accents)

**Verdict: no single pole predicts success — the winning cluster is bimodal, and the
variable that actually tracks outcomes is READABILITY / telegraph contrast.**

- Dark-with-accents holds the biggest anchors (Isaac, Gungeon) and two of the strongest
  recent performers (Death Must Die, Halls of Torment). **Safest pole: MODERATE** evidence
  (confounded by budget/first-mover effects).
- Bright-colorful also wins (Windblown, Voidigo) but reviews document its tax: "too
  bright, exhausting", "lose track of your character". Viable WITH active readability
  engineering. Juicy Realm shows the pole alone doesn't carry weak depth.
- Warm-cozy has **no proven pure action-roguelike hit** in the window (Moonlighter is a
  hybrid; Sworn underperformed — on feel/readability, not warmth). Choosing it is a taste
  call: **flagged "owner's taste decides."**
- **Readability beats palette: STRONG.** Every pole's complaint cluster is a readability
  failure; every success preserved telegraph legibility (Halls of Torment's reserved
  pink/purple attack color = done right; "spell effects obscuring projectiles" = the
  failure mode).

## Q2 — Feel targets

**Verdict: STRONG, stable, make-or-break; the dash/dodge is the centerpiece.**
- Rewarded: *tight, responsive, snappy, pixel-perfect, hard-but-fair.* Punished: *floaty,
  clunky, slow, unresponsive, mushy lock-on, too few i-frames.*
- Cleanest natural experiment: Wizard of Legend 2 — a feel regression alone took a 91%
  franchise to 58% Mixed.
- Reviewers derive feel complaints from visual causes ("enemies not visually clear … so
  combat feels awkward" — Sworn): **Q1 and Q2 converge on telegraph legibility.**
- Parameter anchors (60fps): input buffer 4–6 frames (Celeste-documented), coyote-analog
  5–6 frames, hitstop 60–80ms on kills, 3-tier screenshake (Vlambeer lineage), instant
  response with animation following. Exact Meat Boy / Dead Cells counts are not
  dev-published — flagged as convention, not citation.

## Q3 — Audience

- **Genre veterans are the anchor segment** (Isaac hub ~381k followers, ~154k reviews at
  97%, recent 130k concurrent record) — **STRONG**; the "veterans-first" hypothesis is
  SUPPORTED.
- Streamer channel is real and large (documented lets-play lineage boosting Isaac-likes)
  — **STRONG** on the channel, **MODERATE** on segment sizing.
- Discovery: Next Fest + streamers + tag browsing — **STRONG** on identity.
- Demos (Feb 2026 Next Fest, all-genre): median +806 wishlists, 70th pct +1.8k, 95th pct
  +13.5k. **No roguelike-segmented conversion benchmark exists (WEAK)** — the project's
  own instrumented demo must produce this number.
- Capsule conventions (top quartile): value contrast over saturation (grayscale-check),
  one hero focal point legible at 120px, title in the highest-contrast zone.
- Solo/AI-assisted precedent (Slotbound, confirmed): Steam-accepted disclosure pattern =
  scope AI to non-runtime assets + state human review; channels Discord/X/YouTube;
  demo-reception data conflicted across sources (flagged, verify live before citing).

## Recommendations (graded) for the verdict
1. Build telegraph readability as a first-class system (reserved high-saturation attack
   color, grayscale-tested) — **STRONG**, palette-pole-independent, partially banked
   already.
2. Adopt the feel parameter anchors for the movement/combat pass — **STRONG** direction,
   values tuned on-device.
3. Palette pole: dark-with-accents = evidence-safest; bright viable with engineering;
   warm-cozy unproven for pure action — **MODERATE/WEAK → owner's taste decides** between
   dark-with-accents and his warm-vivid prior. The held wall-art batch resumes on that
   ratification.
