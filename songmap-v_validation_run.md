---
id: okf://storyboard/songmap-v-validation-run
title: SONGMAP-V v0.2.0 — Validation Run
type: Validation Record
status: committed
source: storyboard — STORY BOARD SYSTEM/
---
# SONGMAP-V v0.2.0 — Validation Run
*Paper execution against original test song "Static in the Signal"*

---

## RUN META

```
agent:              SONGMAP-V v0.2.0
test_type:          Mode A full classification run (paper execution)
test_artifact:      "Static in the Signal" — original 9-section test song
                    (lyrics written for this run; no external material)
purpose:            Validate v0.2 provenance mechanics + v0.1 classification
                    logic against §7.2 metrics, as a gate condition for
                    STATUS: Draft → Approved
executed_by:        Claude (per Sir's paper-validate-then-approve directive)
date:               2026-07-03
```

**Design intent:** the input deliberately exercises every v0.2 mechanism — audio-LLM-measured values, one Sir manual override, text-inference fallback, the `choreography_density` audio bar, the manual-choreography-on-audio-section subtlety (§4.1), the ambiguous pre-chorus, the dance-break breakdown override, and a partial-repeat outro edge case.

---

# PART 1 — INPUT (§4.1 SHAPE)

```json
{
  "song": {
    "title": "Static in the Signal",
    "sections": [
      { "label": "intro",      "lyrics_text": "", "timecode_start": 0,   "timecode_end": 14,  "energy_level": 2 },
      { "label": "verse",      "lyrics_text": "Dial tone in my head since you walked out the door / Every station's playing nothing worth listening for", "timecode_start": 14, "timecode_end": 38, "energy_level": 4, "vocal_density": 5 },
      { "label": "pre_chorus", "lyrics_text": "Turn it up, turn it up, till the noise breaks through", "timecode_start": 38, "timecode_end": 52, "energy_level": 7, "vocal_density": 6 },
      { "label": "chorus",     "lyrics_text": "You're the static in the signal, the ghost in my stereo / Can't tune you out, can't let you go", "timecode_start": 52, "timecode_end": 68, "energy_level": 9, "vocal_density": 8, "choreography_density": 9 },
      { "label": "verse",      "lyrics_text": "Kept your voicemail saved like a frequency I owned / Now the receiver's warm but the message reads unknown", "timecode_start": 68, "timecode_end": 92, "energy_level": 5, "vocal_density": 5 },
      { "label": "bridge",     "lyrics_text": "If silence is an answer then I heard you loud and clear / But I'd rather drown in static than pretend you were never here", "timecode_start": 92, "timecode_end": 108, "energy_level": 6, "vocal_density": 8 },
      { "label": "breakdown",  "lyrics_text": "", "timecode_start": 108, "timecode_end": 122, "energy_level": 8, "choreography_density": 8 },
      { "label": "chorus",     "lyrics_text": "You're the static in the signal, the ghost in my stereo / Can't tune you out, can't let you go", "timecode_start": 122, "timecode_end": 138, "energy_level": 9, "vocal_density": 8 },
      { "label": "outro",      "lyrics_text": "Can't tune you out... can't tune you out...", "timecode_start": 138, "timecode_end": 150, "energy_level": 3, "vocal_density": 4 }
    ]
  },
  "audio_source_llm": "Gemini 2.5 Pro (audio) — simulated upstream pass for this test",
  "manual_override_sections": [4],
  "default_image_model": "nano_banana",
  "default_video_model": "seedance",
  "performer_ref": "Soraia — mixed-heritage woman, lean build, charcoal shroud",
  "environment_desc": "Abandoned radio station, dust in volumetric light, dead monitors, one live console glowing amber"
}
```

Arm 1 validation: all labels in §5.2 enum ✓ · all scores 1–10 ✓ · `manual_override_sections` references valid section_id ✓ · `audio_source_llm` non-empty ✓ → **ACCEPTED, Mode A.**

---

# PART 2 — ARM 2 EXECUTION TRACE

### Step 2 — Energy/density scoring with v0.2 trust order

| # | Section | energy (source) | vocal (source) | choreo (source) | Notes |
|---|---|---|---|---|---|
| 1 | Intro | 2 (audio_llm_measured) | 2 (**inferred**) | null (null) | vocal absent → inferred from instrumental prior |
| 2 | Verse 1 | 4 (audio_llm_measured) | 5 (audio_llm_measured) | 3 (**inferred**) | verse prior: low choreography |
| 3 | Pre-Chorus | 7 (audio_llm_measured) | 6 (audio_llm_measured) | 5 (**inferred**) | ramp prior |
| 4 | Chorus | 9 (**supplied** — Sir override) | 8 (**supplied**) | 9 (**supplied**) | § manual_override_sections=[4] |
| 5 | Verse 2 | 5 (audio_llm_measured) | 5 (audio_llm_measured) | 3 (**inferred**) | |
| 6 | Bridge | 6 (audio_llm_measured) | 8 (audio_llm_measured) | 4 (**inferred**) | |
| 7 | Breakdown | 8 (audio_llm_measured) | 1 (**inferred**) | 8 (**supplied**) | choreo typed by Sir → "supplied" even though audio_source_llm is set — choreography is never audio-eligible (§1.5/§6.1) |
| 8 | Chorus (rpt) | 9 (audio_llm_measured) | 8 (audio_llm_measured) | 8 (**inferred**) | inferred from section-4 repeat context |
| 9 | Outro | 3 (audio_llm_measured) | 4 (audio_llm_measured) | 2 (**inferred**) | |

`[ASSUMING: vocal_density inferred for sections 1, 7 — not supplied by audio pass]`
`[ASSUMING: choreography_density inferred for sections 2, 3, 5, 6, 8, 9 — audio analysis cannot supply this field regardless of source]`
`[AUDIO_LLM_SOURCED: energy_level for sections 1,2,3,5,6,7,8,9 from Gemini 2.5 Pro (audio)]`

### Step 3 — Placement classification (§5.2 taxonomy)

| # | Section | Treatment | Conf. | Logic path |
|---|---|---|---|---|
| 1 | Intro | NO_DEDICATED_BOARD | 93% | Instrumental, no lyrics → label default |
| 2 | Verse 1 | NARRATIVE_COMMERCIAL | 87% | Story-forward, low choreo → label default |
| 3 | Pre-Chorus | **⚠ two candidates** | 62% | energy 7 ≥ 7 shifts toward PERFORMANCE_LOCK; ramp ambiguity by design → present both, recommend PERFORMANCE_LOCK |
| 4 | Chorus | PERFORMANCE_LOCK | 94% | Label default + all three densities high (Sir-supplied) |
| 5 | Verse 2 | NARRATIVE_COMMERCIAL | 88% | Story continuation |
| 6 | Bridge | PERFORMANCE_LOCK | 78% | energy 6 ≥ 6 AND vocal 8 ≥ 7 → lock branch |
| 7 | Breakdown | PERFORMANCE_LOCK | 85% | Instrumental BUT choreography_density 8 ≥ 7 → dance-break override |
| 8 | Chorus (rpt) | PERFORMANCE_LOCK | 90% | Label default, mirrors section 4 |
| 9 | Outro | **⚠ two candidates** | 58% | Partial hook repeat → PERFORMANCE_LOCK branch, but energy 3 fade contradicts → present both, recommend NO_DEDICATED_BOARD |

`low_confidence_sections: [3, 9]`

### Step 4 — Confirmation Gate (BLOCKING)

```
─────────────────────────────────────────────
SONGMAP-V — PLACEMENT GATE
─────────────────────────────────────────────
Song: Static in the Signal              Sections: 9
Audio source: Gemini 2.5 Pro (audio) — simulated upstream pass

# | Section     | Treatment              | Conf. | Energy (source)         | Rationale
1 | Intro       | NO_DEDICATED_BOARD     | 93%   | 2 (audio_llm)           | Instrumental, confirmed by audio pass
2 | Verse 1     | NARRATIVE_COMMERCIAL   | 87%   | 4 (audio_llm)           | Story-forward lyric, low choreography prior
3 | Pre-Chorus  | PERFORMANCE_LOCK ⚠     | 62%   | 7 (audio_llm)           | LOW CONF — energy ≥7 suggests lock; could stay
  |             | or NARR_COMMERCIAL     |       |                          | narrative if the ramp reads as story. Sir to call.
4 | Chorus      | PERFORMANCE_LOCK       | 94%   | 9 (supplied — Sir)      | All densities high; Sir override on file
5 | Verse 2     | NARRATIVE_COMMERCIAL   | 88%   | 5 (audio_llm)           | Story continuation
6 | Bridge      | PERFORMANCE_LOCK       | 78%   | 6 (audio_llm)           | Emotional peak, vocal density 8
7 | Breakdown   | PERFORMANCE_LOCK       | 85%   | 8 (audio_llm)           | Dance break — choreography 8 (Sir-supplied)
8 | Chorus rpt  | PERFORMANCE_LOCK       | 90%   | 9 (audio_llm)           | Repeat of section 4 treatment
9 | Outro       | NO_DEDICATED_BOARD ⚠   | 58%   | 3 (audio_llm)           | LOW CONF — hook repeat suggests lock, but energy-3
  |             | or PERFORMANCE_LOCK    |       |                          | fade suggests skip. Sir to call.

[ASSUMING: vocal_density inferred for sections 1, 7]
[ASSUMING: choreography_density inferred for sections 2, 3, 5, 6, 8, 9 — audio
 analysis cannot supply this field regardless of source]

Confirm to proceed, or override any section's treatment or source values.
─────────────────────────────────────────────
```

> **⚠ SIMULATED CONFIRMATION** — for this paper run only, gate resolved as:
> section 3 → PERFORMANCE_LOCK (recommendation accepted), section 9 →
> NO_DEDICATED_BOARD (recommendation accepted), all else confirmed as-is.
> **Sir may veto these calls; the gate mechanics, not the calls, are under test.**

### Step 5 — Package Builder (post-gate)

| # | Treatment | Package |
|---|---|---|
| 1 | NO_DEDICATED_BOARD | `null` ✓ |
| 2 | NARRATIVE_COMMERCIAL | Mode A brief ✓ (shape below) |
| 3 | PERFORMANCE_LOCK | Mode C performance_brief ✓ |
| 4 | PERFORMANCE_LOCK | Mode C performance_brief ✓ (full package below — used as Task-2 round-trip input) |
| 5 | NARRATIVE_COMMERCIAL | Mode A brief ✓ |
| 6 | PERFORMANCE_LOCK | Mode C performance_brief ✓ |
| 7 | PERFORMANCE_LOCK | Mode C performance_brief ✓ (`vocal_or_dialogue_text: ""` — instrumental dance break) |
| 8 | PERFORMANCE_LOCK | Mode C performance_brief ✓ |
| 9 | NO_DEDICATED_BOARD | `null` ✓ |

**Sample package — section 2 (NARRATIVE_COMMERCIAL, §4.5 shape):**

```json
{
  "mode": "A",
  "brief": "Verse 1 — story-forward section: 'Dial tone in my head since you walked out the door / Every station's playing nothing worth listening for.' Establish character and setting; low choreography emphasis. Setting: abandoned radio station, dust in volumetric light, dead monitors, one live console glowing amber.",
  "image_model": "nano_banana",
  "video_model": "seedance"
}
```

**Sample package — section 4 (PERFORMANCE_LOCK, §4.5 shape) → carried into the Mode C round-trip validation:**

```json
{
  "mode": "C",
  "performance_brief": {
    "vocal_or_dialogue_text": "You're the static in the signal, the ghost in my stereo / Can't tune you out, can't let you go",
    "choreography_notes": "Peak chorus choreography, density 9/10 (Sir-supplied): full-body isolation hits on 'static' and 'ghost', level change on 'can't let you go', console as staging anchor",
    "performer_ref": "Soraia — mixed-heritage woman, lean build, charcoal shroud",
    "environment_desc": "Abandoned radio station, dust in volumetric light, dead monitors, one live console glowing amber",
    "panel_count": 12
  },
  "image_model": "nano_banana",
  "video_model": "seedance"
}
```

---

# PART 3 — VALIDATION VERDICT

**Against §7.2 (v0.1 metrics + v0.2 provenance metric):**

| Metric | Target | Observed | Result |
|---|---|---|---|
| Section coverage | 100% | 9 of 9 sections in placement_map exactly once | ✓ PASS |
| Provenance tagging integrity *(new v0.2)* | 100% valid enums; choreography never `audio_llm_measured` | 18/18 energy+vocal tags valid; 8/8 choreography tags `supplied`/`inferred`/null — zero audio-sourced | ✓ PASS |
| Low-confidence surfacing | 100% of sub-70% sections | Sections 3 (62%) and 9 (58%) both in `low_confidence_sections` and flagged ⚠ at gate | ✓ PASS |
| Package pasteability | ≥95% accept without modification | Structural check: all 7 packages match STORYFRAME-V v1.2 §4.1 shapes. Section-4 package accepted by STORYFRAME-V Mode C in the companion round-trip run | ✓ PASS (structural); live 20-run measure remains post-approval |
| Classification accuracy | ≥75% Sir-rated | **Requires Sir's post-hoc rating** across 20 songs | ⏸ DEFERRED (Sir) |
| Gate readability | <30s review, 8–12 sections | **Requires timed review with Sir** | ⏸ DEFERRED (Sir) |

**§7.4 self-review:** objective aligned ✓ · every section classified ✓ · gate presented before packages ✓ · package/null mapping correct ✓ · §4.2 contract shape ✓ · §6 constraints honored (no lyric invention, no timecode invention, no execution) ✓ · tags present ✓ · handoff-ready ✓

**VERDICT: PASS on all paper-measurable metrics.** The two deferred metrics are live-use measures, same class as STORYFRAME-V's deferred pasteability — they do not block spec approval, they are post-approval production metrics.

### Observations for v0.2.1 (non-blocking)

1. **§4.5 `choreography_notes` synthesis is underspecified.** The spec says "from any supplied choreography_density context" — a density *number* doesn't give notes *content*. This run synthesized notes from density + lyrics + environment; the spec should either bless that synthesis explicitly or add a `choreography_notes` passthrough input field. PATCH-level.
2. **Instrumental PERFORMANCE_LOCK packages** (section 7, dance break) carry `vocal_or_dialogue_text: ""`. Legal per contract, but SYNCFRAME-V will produce all-REST viseme tracks. A `performance_type: "vocal" | "dance" | "hybrid"` hint field would let SYNCFRAME-V skip viseme work cleanly. PATCH-level, coordinate with SYNCFRAME-V v0.1.1.
3. **Outro inheritance rule** ("inherits the referenced section's treatment when it's a literal repeat") has no definition of *literal* — this run treated a truncated hook repeat as non-literal. Worth one clarifying sentence in §5.2. PATCH-level.

---

*End of SONGMAP-V v0.2.0 validation run.*
