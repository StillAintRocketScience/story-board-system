# STORYFRAME-V v1.3 Mode D — Website Validation Run
*Paper execution of the Website mode: one NAICS client round-trip (non-chained), one SARS Lookbook round-trip (chained), plus seeded-failure input tests*

---

## RUN META

```
agent_under_test:   STORYFRAME-V v1.3.0 (Mode D)
test_type:          Paper execution, both consumer types + seeded-failure tests
test_artifacts:     Run A — Appendix A.7 brief (Ember & Oak, naics_client)
                    Run B — SARS Lookbook brief (The Strategist, sars_lookbook,
                    hand-built for this run; canon-neutral, no Gate 4 exposure)
purpose:            Validate §5.6 classification, §4.3 Mode D gate shape,
                    reference-rule presence, §5.4.7 real-asset override,
                    chain plan integrity + §4.7 producer loop emission,
                    business_data precedence (§5.4.9), [GAP] tagging on
                    unverified client figures, and Arm 1 rejection rules —
                    gate condition for Draft → Approved
executed_by:        Claude (per Sir's paper-validate-then-approve directive,
                    2026-07-04)
date:               2026-07-04
```

---

# PART 1 — RUN A: NAICS CLIENT, NON-CHAINED (Ember & Oak)

### 1.1 Intake & routing

Input = A.7 JSON verbatim. `website_brief` present, no `performance_brief` → **Mode D triggered**, hierarchy case 2 ✓. `brief` text present within `website_brief` → URL ingestion skipped ✓. `hero_reference.type == "real_asset"` with 2 paths → passes Arm 1 validation ✓.

### 1.2 Classification & gate

Signals read: "steakhouse," menu `business_data`, hours, reservation-driven → **`local_business`**, confidence 94%. §5.3 commercial taxonomy not consulted ✓.

Gate rendered per §4.3 Mode D shape:

```
─────────────────────────────────────────────
STORYFRAME-V — WEBSITE GATE (MODE D)
─────────────────────────────────────────────
Website Archetype:  local_business
Rationale:          Restaurant with structured menu/hours data and a
                    reservation goal — classic local-business destination site.
Confidence:         94%
Chained:            NO
Consumer:           naics_client

Proposed Clip Plan:
  Clips:            3  [AUTO-SELECTED]
  Per-clip:         1080p · 16:9 · ~8s · std · no audio
  Hero reference:   REAL ASSETS: 2 supplied

Section skeleton:   hero+motto → story → menu (Fire/Field) →
                    reservation form → hours+map

[ASSUMING: motto "Six dishes. One fire." lifted from brief prose]

Confirm to proceed, or provide corrections.
─────────────────────────────────────────────
```

Gate is blocking ✓; paper-Sir confirms.

### 1.3 Manifest assembly

`hero_image_prompt` = **null** — §5.4.7 real-asset override applied ✓. `reference_rule` emitted:

> "Attach the supplied reference photos (ribeye-flame.jpg for food shots, dining-room.jpg for space shots) to every generation so the dish, flame character, and room read as the same real restaurant in all clips. Do not stylize away from the photos."

**Reference-rule-first check (§7.2 / §6.1):** all 3 `frames[].motion_prompt` open with the applicable reference sentence — 3/3 ✓. Sample, clip 1:

```
clip-01-fire-hero.mp4 — frames[1].motion_prompt:
"Use the attached photo ribeye-flame.jpg as the product reference — same cut,
same char pattern, same flame color. Slow-motion macro of the ribeye searing
over open wood flame, embers rising into darkness, cinematic amber light
pulling detail off the crust. std mode, 1080p, 16:9, no audio, ~8s."
```

`chain_plan: null`, `producer_loop: null` — archetype not chained ✓. Filenames follow `clip-<nn>-<slug>.mp4` ✓.

### 1.4 Site Build Brief

- Menu section populated from `business_data.menu` verbatim (Fire: Ribeye 48, Strip 42 / Field: Roasted beets 14) — §5.4.9 structured-data precedence ✓; brief prose supplied tone only ✓
- No invented stats: brief contains no review counts or "years open" figures, and none were fabricated; the one derived line (motto) carries the `[ASSUMING]` tag from the gate ✓ (§6.3 client-figure rule exercised: nothing to [GAP]-tag because nothing was invented — negative case passes)
- `scroll_bindings`: clip 1 `scrub_frame_sequence` (hero), clips 2–3 `pinned_reveal` ✓ — every section with a `bound_clip` has a matching binding, 3/3 ✓
- `assets_contract` lists the exact 3 manifest filenames + drop-folder path ✓
- `hitl_gates`: all 6 gates present, in §4.7 order ✓

**RUN A: PASS.**

---

# PART 2 — RUN B: SARS LOOKBOOK, CHAINED (The Strategist)

### 2.1 Input (hand-built for this run)

```json
{
  "mode": "D",
  "website_brief": {
    "site_name": "The Strategist — World Lookbook",
    "brief": "Scroll-driven descent into The Strategist's world: from the city skyline at night down through the tower, the war-room floor, and into the archive vault. Tone: controlled, chess-cold, expensive. The site IS the descent.",
    "consumer": "sars_lookbook",
    "hero_reference": { "type": "generate" },
    "business_data": { "registry_refs": ["VAR:strategist-tower-v1",
                                          "VAR:war-room-key-v2"] }
  },
  "image_model": "nano_banana",
  "video_model": "seedance"
}
```

### 2.2 Classification & gate

Signals: "descent into," "the site IS the descent," staged locations → **`journey`**, confidence 91%, **Chained: YES**. Gate proposes 5 clips (journey default), 1080p·16:9·~8s·std·no audio, hero reference GENERATE. `[CHAINED: 4 links — §4.7 producer loop required]` marker present in rendered_markdown header ✓. Paper-Sir confirms.

### 2.3 Manifest & chain plan

`hero_image_prompt` (non-null, generate path): single Nano Banana prompt establishing the tower exterior at night per VAR:strategist-tower-v1 description — verbatim-anchor discipline held across all 5 clip prompts ✓.

**Chain plan integrity (§7.2 metric):**

| Link | from_clip | to_clip | handoff | clips[].chain.start_from |
|---|---|---|---|---|
| 1 | 1 (skyline approach) | 2 (tower glass descent) | final_frame_as_start | clip 1: `hero_ref` ✓ |
| 2 | 2 | 3 (executive floor glide) | final_frame_as_start | clip 3: `final_frame_of:2` ✓ |
| 3 | 3 | 4 (war-room reveal) | final_frame_as_start | clip 4: `final_frame_of:3` ✓ |
| 4 | 4 | 5 (archive vault hold) | final_frame_as_start | clip 5: `final_frame_of:4` ✓ |

Single unbroken sequence, clip 1 starts from `hero_ref`, zero orphan frame_ids, every ChainLink references an existing frame — **100% integrity** ✓.

`producer_loop` emitted with the full §4.7 five-step instruction block, HUMAN/AUTOMATED labels intact ✓. Reference-rule-first: 5/5 clip prompts ✓.

### 2.4 Site Build Brief

Journey skeleton: hero question ("How far down does control go?") → one fact per zone × 4 → registry-ref spec callouts → CTA. `scroll_bindings`: all five clips `scrub_frame_sequence` as one concatenated descent + fixed progress indicator naming each zone ✓. Canon exposure check: registry refs consumed read-only from `business_data`; no canon *changes* proposed — Gate 4 not triggered, correctly upstream of this agent ✓. `hitl_gates`: 6 gates including gate 4 (chain seam approval per link — 4 seams) ✓.

**RUN B: PASS.**

---

# PART 3 — RUN C: SEEDED-FAILURE INPUT TESTS

| # | Seeded input | Expected (§3.A Arm 1) | Observed |
|---|---|---|---|
| 1 | `website_brief` with neither `brief` nor `source_url` | `[INPUT_REJECTED: website_brief empty — supply brief text or source_url]` | Rejected exactly so; no partial output ✓ |
| 2 | `clip_count: 12` (user-stated, out of 3–8) | `[INPUT_REJECTED: clip_count out of range for Mode D]` | Rejected ✓ |
| 3 | `performance_brief` AND `website_brief` both present | `[INPUT_REJECTED: conflicting dedicated briefs — supply one]` | Rejected ✓ |
| 4 | `hero_reference.type: "real_asset"`, `real_asset_paths: []` | `[INPUT_REJECTED: real_asset declared but no paths supplied]` | Rejected ✓ |
| 5 | `website_archetype: "microsite"` (not in §5.6) | `[INPUT_REJECTED: unknown website_archetype "microsite"]` | Rejected ✓ |
| 6 | Top-level `frame_count: 15` alongside valid website_brief | `[WARNING: frame_count not used in Mode D — use website_brief.clip_count]`, run proceeds | Warned + proceeded ✓ |

**RUN C: PASS — 6/6.**

---

# PART 4 — VALIDATION VERDICT

**STORYFRAME-V v1.3 §7.2 — paper-measurable metrics:**

| Metric | Target | Result |
|---|---|---|
| Reference-rule presence | 100% | 8/8 clip prompts across Runs A+B ✓ |
| Chain plan integrity | 100% | 4/4 links, zero orphans, hero_ref start (Run B) ✓ |
| Archetype classification accuracy | ≥80% | 2/2 on seeded briefs (small paper sample — full 15-brief blind test remains a production metric) |
| Manifest pasteability | ≥95% | **Deferred to production** (requires live model renders — same posture as v1.2's pasteability metric) |
| Build-brief sufficiency | ≥80% | **Deferred to production** (requires live builder sessions) |

**Structural checks:** §4.3 Mode D gate blocking and correctly shaped (both runs); §5.4.7 real-asset override (Run A) and generate path (Run B) both exercised; §5.4.9 business_data precedence held; §6.3 no-fabricated-client-figures held (negative case); Arm 1 rejection surface 6/6.

## VERDICT: PASS — all paper-measurable §7.2 metrics and structural checks satisfied.

Gate condition for Draft → Approved is met. Live pasteability and build-brief sufficiency tracked post-approval, per the v1.2 precedent.

---

*Executed 2026-07-04 — evidence file for storyframe-v_v1.3.md §0 status change.*
