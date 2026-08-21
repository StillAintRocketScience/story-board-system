---
id: okf://storyboard/modec-roundtrip-validation-run
title: STORYFRAME-V v1.2 Mode C ↔ SYNCFRAME-V v0.1 — Round-Trip Validation Run
type: Validation Record
status: draft
source: storyboard — STORY BOARD SYSTEM/
---
# STORYFRAME-V v1.2 Mode C ↔ SYNCFRAME-V v0.1 — Round-Trip Validation Run
*Paper execution of the delegation chain, fed by SONGMAP-V's section-4 package*

---

## RUN META

```
agents_under_test:  STORYFRAME-V v1.2.0 (Mode C) + SYNCFRAME-V v0.1.0 (delegated)
test_type:          Delegation round-trip (paper execution) + seeded-failure halt test
test_artifact:      Section-4 PERFORMANCE_LOCK package from songmap-v_validation_run.md
                    ("Static in the Signal" chorus — original test material)
purpose:            Validate §4.5 call shape, §4.3 return shape, MERGE_RULE,
                    zone-priority resolution, color-channel separation, and
                    halt propagation — gate condition for Draft → Approved
executed_by:        Claude (per Sir's paper-validate-then-approve directive)
date:               2026-07-03
```

---

# PART 1 — RUN A: HAPPY PATH

### 1.1 STORYFRAME-V Mode C intake

Input = section-4 package verbatim from SONGMAP-V. `performance_brief` present → **Mode C triggered** ✓. Commercial Type Classifier, Format Selector, and Confirmation Gate skipped per §2.A Mode C definition ✓. Brief Parser → Beat Sequencer produce a 12-frame plan (panel_count 12 from package; section duration 16s → non-uniform per-frame durations weighted by vocal sub-beat).

### 1.2 Delegation call (STORYFRAME-V → SYNCFRAME-V, §4.5 shape)

```json
{
  "delegated_from_storyframe_v": true,
  "vocal_or_dialogue_text": "You're the static in the signal, the ghost in my stereo / Can't tune you out, can't let you go",
  "choreography_notes": "Peak chorus choreography, density 9/10 (Sir-supplied): full-body isolation hits on 'static' and 'ghost', level change on 'can't let you go', console as staging anchor",
  "performer_ref": "Soraia — mixed-heritage woman, lean build, charcoal shroud",
  "environment_desc": "Abandoned radio station, dust in volumetric light, dead monitors, one live console glowing amber",
  "emotional_register": null,
  "panel_count": 12,
  "image_model": "nano_banana",
  "video_model": "seedance",
  "sars_context": false
}
```

SYNCFRAME-V Arm 1: hierarchy case 4 (Mode C dependency) → accepts STORYFRAME-V's pre-sequenced beats, skips own Steps 1–2 ✓. Validation: vocal text non-empty ✓, performer_ref present ✓, panel_count 12 in 6..20 ✓, models in enum ✓ → **ACCEPTED**.

`emotional_register` null → inferred from choreography note + lyric tone: *defiant longing* (per Arm 2 Step 4 "supplied or inferred from the choreography note"). Flagged: `[ASSUMING: emotional_register inferred as 'defiant longing' — not supplied]`

### 1.3 SYNCFRAME-V execution trace (Steps 3–6)

**Viseme mapping — phoneme trace (every non-REST panel traceable, §7.2 metric 1):**

| Frame | Vocal sub-beat | Dominant phoneme | Viseme | AU sequence (upper unless REST) | Zone-priority |
|---|---|---|---|---|---|
| 1 | "You're the" | /jʊr/ rounded | `U_WQ` | AU1+AU2 (anticipatory lift) | — |
| 2 | "STA-tic" | /æ/ stressed open | `AI` | AU4+AU7 (percussive intensity, isolation hit) | — |
| 3 | "in the" | /ð/ | `L` | AU1 (carry-through) | — |
| 4 | "SIG-nal" | /s/→/ɪ/ blend | `SIBILANT` ⚠ 68% | AU4 (held furrow) | — |
| 5 | "the GHOST" | /oʊ/ | `O` | AU5+AU2 (eyes widen, isolation hit) | — |
| 6 | "in my STE-re-o" | /ɛ/ dominant, /oʊ/ tail | `E` | AU1+AU6 (ache under the phrase) | — |
| 7 | *(instrumental fill)* | — | `REST` | AU4+AU15 (full-face permitted on REST: jaw set, corners down) | — |
| 8 | "Can't TUNE you out" | /u/ | `U_WQ` | AU4+AU7 (defiance builds) | — |
| 9 | "can't LET you" | /ɛ/ | `E` | AU1+AU5 (plea flash) | — |
| 10 | "GO" (climax) | /oʊ/ held | `O` | AU6+AU2 — **AU12 requested by register, DROPPED** | ✓ `zone_priority_applied: true` |
| 11 | "go..." (melisma decay) | /oʊ/ sustained | `O` | AU1 (release) | — |
| 12 | *(button pose)* | — | `REST` | AU12+AU6 (full-face permitted on REST: rueful half-smile lands AFTER the vocal) | — |

Tags emitted:
`[GAP: viseme uncertain at panel 4 — /s/ onset vs /ɪ/ nucleus blend, 68% confidence; SIBILANT chosen on consonant dominance]`
`[ZONE_PRIORITY_APPLIED: panel 10 — AU12 dropped in favor of viseme O]`

**Color channel assembly (Step 6) — sample, frame 10 (climax):**

| Channel | Color | Annotation (written separately, never blended — §6.1) |
|---|---|---|
| body_movement | Red | "level drop to knees on 'go', arms release from console" |
| camera_movement | Blue | "slow push-in tightens through the drop, ends chest-up" |
| framing_composition | Green | "performer center-weighted, console edge frames right third" |
| lighting_direction | Orange | "amber console glow keys from below-right, dust volumetrics behind" |
| vocal_emotional | Purple | "peak emphasis beat — mouth rounded open O held, eyes wide (AU2+AU6); the beat to protect" |
| lens_label | Black | "35mm, frame 10/12" |

All 12 frames carry all applicable channels populated separately; REST frames carry `vocal_emotional` describing the held silence beat. Zero blended annotations.

### 1.4 Delegation return + MERGE

Return conforms to SYNCFRAME-V §4.3 `DELEGATION_RETURN_SHAPE`: `frames_addendum[12]` (frame_id, ipa_notes, facs_notes, color_annotation_layer) + `color_legend` ✓.

MERGE_RULE check (STORYFRAME-V §4.5): all 12 `frame_id`s in addendum match existing `frames[]` ids 1–12, **zero orphans** ✓. `ipa_notes`/`facs_notes`/`color_annotation_layer` written onto matching frames ✓. `color_legend` → `metadata.performance_sync` ✓. Sound + Motion Arc and Camera Language panel produced by STORYFRAME-V as usual (Mode C doesn't suppress them) ✓.

**Sample merged frame (10) as it lands in STORYFRAME-V output:**

```json
{
  "frame_id": 10,
  "visual_description": "Soraia — mixed-heritage woman, lean build, charcoal shroud — drops to knees at the amber-lit console, head thrown back. Red arrow: level drop, arms release. Blue arrow: slow push-in ending chest-up. Green marks: center-weighted, console right third. Orange marks: amber key below-right, dust volumetrics. Purple mark on mouth/eyes: peak beat. Black text: 35mm, 10/12.",
  "ipa_notes": "mouth rounded and open, lips forward (O — held /oʊ/ on 'GO')",
  "facs_notes": "cheeks lift with crow's-feet crease (AU6), outer brows lift (AU2); lip-corner pull (AU12) dropped — zone priority, viseme protected",
  "color_annotation_layer": { "body_movement": "...", "camera_movement": "...", "framing_composition": "...", "lighting_direction": "...", "vocal_emotional": "..." },
  "motion_prompt": "1.6s: she drops to her knees as the held note lands, camera pushes in slow, amber glow from the console under-lights her face, mouth held in a rounded open shape, eyes wide, dust drifting through the light."
}
```

---

# PART 2 — RUN B: SEEDED-FAILURE HALT TEST

Same package, `panel_count` deliberately corrupted to **24** (out of SYNCFRAME-V's 6..20 range).

| Step | Expected | Observed |
|---|---|---|
| SYNCFRAME-V Arm 1 validation | `[INPUT_REJECTED: panel_count out of range]` | Rejected exactly so ✓ |
| STORYFRAME-V MERGE_RULE on rejection | "propagate the same halt — do not partially merge" | STORYFRAME-V output halts with the propagated rejection; `frames[]` left without any addendum fields; no partial merge ✓ |

---

# PART 3 — VALIDATION VERDICT

**SYNCFRAME-V v0.1 §7.2:**

| Metric | Target | Observed | Result |
|---|---|---|---|
| Viseme-lyric alignment | 100% traceable | 10/10 non-REST panels traced to a specific phoneme (table above) | ✓ PASS |
| Zone-priority conflict rate | 0 unresolved | 1 conflict raised (frame 10), resolved by documented drop + flag; 0 unresolved | ✓ PASS |
| Color-tag completeness | 100% applicable channels | 12/12 frames, all applicable channels populated separately | ✓ PASS |
| Pasteability | ≥95% over 20 runs | Live render measure | ⏸ DEFERRED (Sir, post-approval) |
| Gap surfacing | ≥90% on ambiguous briefs | Seeded ambiguity (panel 4 blend) surfaced at 68% conf; full 10-brief blind review | ✓ mechanism verified / ⏸ full measure deferred |

**STORYFRAME-V v1.2 §7.2 (new Mode C metrics):**

| Metric | Target | Observed | Result |
|---|---|---|---|
| Mode C delegation merge integrity | 100%, zero orphans | 12/12 frame_ids matched, zero orphaned addenda | ✓ PASS |
| Mode C halt propagation | 100%, zero partial merges | Seeded failure propagated, zero partial merge (Run B) | ✓ PASS |
| Frame count adherence | 100% ±0 | 12 requested, 12 produced | ✓ PASS |
| Hero-subject consistency | 100% verbatim anchor | performer_ref anchor verbatim in all frames containing the hero | ✓ PASS |

**Cross-spec integration findings:**

1. **[INTERFACE GAP — non-blocking]** SONGMAP-V's §4.5 `performance_brief` shape has no `emotional_register` field, but STORYFRAME-V's §4.5 delegation call forwards `input.performance_brief.emotional_register`. Result: every SONGMAP-routed Mode C run arrives with `emotional_register: null` and SYNCFRAME-V infers it. Legal and handled (inference path exists), but if Sir wants register control from the song level, SONGMAP-V v0.2.1 should add the field. PATCH-level, logged for both specs.
2. **Instrumental PERFORMANCE_LOCK** (SONGMAP section 7, dance break) would arrive at SYNCFRAME-V with empty `vocal_or_dialogue_text` — which SYNCFRAME-V **rejects** (`[INPUT_REJECTED: no vocal or dialogue content supplied]`). The chain handles this correctly via halt propagation, but it means dance-break sections currently cannot flow through Mode C at all — they need STORYFRAME-V Mode A instead, or SYNCFRAME-V v0.2 needs an all-REST/dance mode. **This is the most substantive finding of the run** — still non-blocking for approval (the specs behave exactly as written; the gap is a scope seam, already flagged as observation 2 in the SONGMAP run).

**VERDICT: PASS on all paper-measurable metrics, both specs.** Deferred items are live-render measures. Two PATCH-level seams logged above.

---

*End of Mode C round-trip validation run.*
