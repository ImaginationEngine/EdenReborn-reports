# Launch Risk Register (sanitized public summary)

Sanitized summaries of the private risk-audit modules. Full evidence tables,
strategy specifics, and draft wording live in the private repo.

## Row: Feel pass = demo critical path (2026-07-17)

The store hook promises combat *feel* ("built around one promise — how good it feels to
fight"). Risk: shipping the demo before the feel/combat-juice pass lands would make the
hook untruthful and miss the locked wow-factor. **Mitigation:** feel pass is formally on
the demo critical path (build-order #1), not deferrable. Tracked in the production roadmap.

## Row: AI disclosure (Module 2 — upgraded 2026-07-16; **RATIFIED 2026-07-16**)

**Question:** is Steam AI disclosure survivable for a production-quality indie title,
and what posture does the evidence support?

**Evidence summary (graded):**
- Disclosure is now mainstream: ~10.9% of Steam releases (2024) → ~20% (2025) → ~30%
  (2026) carry the AI-content disclosure. The disclosed population is dominated by
  low-effort releases, so raw disclosed-vs-not averages are spurious. **STRONG**
- Best available quality-controlled analysis (causal model over ~10k 2025 releases,
  controlling for developer track record/publisher/genre): a disclosure costs ~53% of
  first-month reviews on average, BUT the penalty concentrates on established studios
  (−40–60%); small/unknown developers saw hardly any measurable penalty. **MODERATE-STRONG**
- Production-quality disclosed successes exist (The Finals, inZOI, Suck Up!) — in all of
  them the AI is either a player-facing feature or scoped, human-reviewed pipeline
  assistance. **STRONG**
- The catastrophic outcomes attach to HIDING, not use: The Alters (undisclosed AI found
  in-game within days → public apology), Clair Obscur (initial denial → award stripped).
  Meanwhile fully human-made games get falsely accused routinely (Shrine's Legacy,
  Crime Scene Cleaner) — non-disclosure provides no safe harbor. **STRONG**
- No verified precedent of AI-as-marketing-hook succeeding for a premium game; player
  sentiment toward genAI remains strongly negative in surveys. **STRONG on absence of
  precedent / MODERATE on sentiment magnitude**
- Gatekeeping exists but is mostly filter-shaped, not ban-shaped (itch.io disclosure+
  filtering; YouTube anti-slop rules). Hard gates verified: one indie publisher with a
  contractual zero-AI policy; one awards body with enforced anti-AI rules. Named
  streamer/festival policies: open follow-up — to be sized, not assumed. **MIXED / GAP**

**Recommendation (RATIFIED 2026-07-16):** transparent, scoped, matter-of-fact
disclosure (assets/tooling, human review stated, no runtime generation), placed in the
Steam disclosure field plus plain page copy; AI is not used as the marketing hook.
Clean-provenance asset regeneration (separate module) makes every disclosure clause
verifiable by construction. Campaign posture marked "defied-expectation as an earned/
discovered story, not paid positioning." One follow-up carried: named streamer/festival
AI-policy sizing before go-to-market lock.

**Sources:** PC Gamer (Game Oracle study; Steam Labs figures), Windows Central,
Tom's Hardware, Sulka Haro's 3-year Steam AI dataset, Engadget (Next Fest share;
The Alters), Game Developer / Game Informer (The Alters), Techdirt (publisher policy),
Kotaku (Next Fest GenAI coverage), Plagiarism Today + PC Gamer (false-accusation cases),
GameSpot (itch.io policy).
