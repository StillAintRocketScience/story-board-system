---
id: okf://storyboard/storyframe-v-v1-4-shotgrammar-validation-run
title: STORYFRAME-V v1.4 §5.7 — Shot Grammar Validation Run
type: Validation Run
status: complete
version: 1.0
source: STORY BOARD SYSTEM
---

# STORYFRAME-V v1.4 §5.7 — Shot Grammar Validation Run

## RUN META

```
AGENT UNDER TEST:   STORYFRAME-V v1.4 §5.7 (Shot Grammar Extension)
EXECUTED:           2026-09-23 by Claude Code
GATE CONDITION:     Sir's sign-off was conditioned on validation FIRST
                    ("Approve, but validate first")
METHOD:             Paper validation. Retro-tag REAL produced frames with
                    §5.7; derive-test Axis 2; seeded-failure probes.
COST:               $0. No generation. No model calls.
OUTCOME:            FAIL on first pass -> 4 repairs -> PASS on re-test
```

**Why this run matters:** §5.7 was trimmed hard earlier the same day (Axis 1, 33 → 19 tokens) to remove
five collisions with §5.2. This run tests whether that trim cut *too* far. **It did.**

---

# PART 1 — RUN A: RETRO-TAG A REAL PRODUCED FRAME SET

**Material:** the 15-frame Nike Air Force 1 sequence from `storyframe-v_validation_run.md` — genuine
produced output, not invented for this test. Each frame carries a real `CAMERA:` note.

**Test:** can §5.7 Axis 1 (post-trim, 19 tokens) plus §5.2 express every camera note?

| # | Real CAMERA note | Tagging | Verdict |
|---|---|---|---|
| 1 | Slow overhead push-in | `overhead` + `push_in` | ✅ clean |
| 2 | Close push through the box opening | `close_up` + `push_in` | ✅ clean |
| 3 | **Locked-off product lens** | `locked_off` — **CUT IN TRIM** | ❌ **no token** |
| 4 | Macro lens glide | §5.2 `macro` + movement "glide" | 🔶 approximate (`dolly`) |
| 5 | Slow lateral slide | movement "lateral slide" | 🔶 approximate (`dolly`) |
| 6 | **Tight top-down macro** | `top_down` — **CUT IN TRIM** + §5.2 `macro` | ❌ **no token** |
| 7 | Low-angle macro pan | `low_angle` + `pan` + §5.2 `macro` | ✅ clean |
| 8 | **Low wide-angle shot** | `low_angle` + "wide" → R0 sends to §5.2 | ❌ **R0 ambiguity** |
| 9 | Low tracking shot | `low_angle` + §5.2 `tracking` | ✅ clean |
| 10 | High-speed close-up | `close_up`; "high-speed" = frame rate | ⬜ out of grammar scope |
| 11 | 360 orbit | §5.2 `orbit` | ✅ clean |
| 12 | Static beauty frame | `static` | ✅ clean |
| 13 | **Wide cinematic angle** | "wide" → R0 sends to §5.2 | ❌ **R0 ambiguity** |
| 14 | Slow low-angle push-in | `low_angle` + `push_in` | ✅ clean |
| 15 | Static final frame | `static` | ✅ clean |

**RUN A FIRST PASS: FAIL — 10/15 expressible (67%).**

Three distinct defects, all caused by the trim:

- **D1 — two tokens were wrongly cut.** `locked_off` and `top_down` were removed as "synonyms" of `static`
  and `overhead`. Frames 3 and 6 use both **verbatim**, as deliberate creative choices. *"Locked-off
  product lens"* is not *"static"* — it is a specific product-photography instruction. A synonym in a
  thesaurus is not a synonym in a shot list.
- **D2 — R0 has a `wide` edge case.** R0 sends the token `wide` to §5.2 (intent). Frames 8 and 13 use
  "wide" in the *framing/lens* sense. R0 as written left them untaggable in either taxonomy.
- **D3 — no lateral/glide movement token.** Frames 4 and 5 approximate to `dolly`.

---

# PART 2 — RUN B: AXIS 2 DERIVE-TEST

**Axis 2 could not be retro-tagged.** No character frame set exists anywhere in the family — every
validation run to date is product (Nike AF1) or architectural (The Strategist tower/vault sequence).
**Recorded as a finding:** the family has never produced a human-subject frame set, which is itself part
of why no subject-state vocabulary ever emerged.

Axis 2 was instead derive-tested against the Persona Generator's **22 working shot directives**
(`10-ACTIVE/PERSONA_SHEET_METHOD_v0.2` §3) — a real, in-use shot list.

| Persona group | Directives | Expressible by Axis 2 (pre-repair) |
|---|---|---|
| Face Angles | 8 | **5/8** — `front`, 3/4 L/R, profile L/R ✅; ***looking up*, *looking down*, *head tilted* ❌** |
| Expressions | 6 | 6/6 ✅ |
| Body Shots | 8 | 8/8 ✅ |

**RUN B FIRST PASS: FAIL — 19/22 (86%).**

- **D4 — Axis 2 had no head-orientation dimension.** Facing covers yaw only. Pitch (up/down) and roll
  (tilt) had nowhere to go.

---

# PART 3 — REPAIRS APPLIED

| # | Repair | Justification |
|---|---|---|
| **R-1** | Restored `locked_off` (Movement) and `top_down` (Angle) | Both used verbatim in produced work. R6's ≥2× evidence bar is met across the frame set. Counts marked `†` in §5.7 to show they come from the frame set, not the five spec files. |
| **R-2** | Added rule **R0.1** — the `wide` disambiguation | R0 governs the token *string*. Intent sense → §5.2 `wide`. Framing sense → Axis 1 size rungs + angle, never re-using the string `wide` inside Axis 1. |
| **R-3** | Added Axis 2 **Head** dimension: `head_up`, `head_down`, `head_tilt` | Closes D4. |
| **R-4** | Documented `glide` / `lateral slide` as an accepted approximation to `dolly` | A dedicated `truck` token appeared **once** — below R6's ≥2× bar. **Deliberately not invented.** R6 is its route in if it recurs. |

**Note on R-4:** the cheap fix was to add `truck` and score 15/15 clean. That would have been inventing
vocabulary to pass my own test. It is recorded as an accepted approximation instead.

---

# PART 4 — RE-TEST AFTER REPAIRS

| Run | Metric | Target | Result |
|---|---|---|---|
| A | Real frames expressible | 100% accounted | **15/15** — 12 clean ✅, 2 accepted approximations (4, 5), 1 out of grammar scope (10, frame rate) |
| A | Frames blocked by a missing token | 0 | **0** ✅ (was 2) |
| A | Frames blocked by R0 ambiguity | 0 | **0** ✅ (was 2) |
| B | Persona directives expressible | 100% | **22/22** ✅ (was 19/22) |
| — | Axis 1 ↔ §5.2 token collisions | 0 | **0** ✅ |
| — | §5.2 modified? | No | **Byte-identical to v1.3** ✅ |
| — | v1.3 modified? | No | **Untouched — Aug-21 mtime, 49,178 B** ✅ |

## PART 5 — SEEDED-FAILURE PROBES

Each seeded input must be rejected by the named rule.

| # | Seeded violation | Rule | Rejected |
|---|---|---|---|
| 1 | `macro` used as an Axis 1 **size** token | R0 | ✅ |
| 2 | `wide` written into `camera_note` in the framing sense | R0.1 | ✅ |
| 3 | Two size tokens on one frame (`medium` + `close_up`) | R2 | ✅ |
| 4 | `profile_left` placed in `camera_note` | R3 | ✅ |
| 5 | `eye_level` + `static` stated explicitly as defaults | R4 | ✅ |
| 6 | Two-subject frame given one bare `subject_note`, no relational token | R5 | ✅ |
| 7 | Invented token `snorricam` used inline | R6 | ✅ |

**PART 5: PASS — 7/7.**

---

# VERDICT

## **PASS — after repairs.** Gate condition for Draft → Approved is met.

**The first pass failed and that is the point of the run.** The same-day scope trim removed two tokens
that real produced frames use verbatim, left an R0 edge case two frames hit, and shipped an Axis 2 that
could not express 3 of 22 directives from the very tool it was harvested from. Four repairs closed all
four defects; one was deliberately closed by *documenting an approximation* rather than inventing a token
to make the score look better.

**Carried forward, not fixed here:**
- Axis 2 has **never been exercised against real family output**, because none exists. Its first
  production use is its real test. Tracked as a post-approval metric, matching the v1.2/v1.3 precedent of
  deferring live metrics.
- `truck` (lateral movement) sits at 1 occurrence. R6 admits it at 2.
- `storyboard-prompt-builder.md` consolidation remains parked and unresolved.

---

*Executed 2026-09-23 — evidence file for `storyframe-v_v1.4.md` §0 status change.*
