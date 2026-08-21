---
id: okf://storyboard/songmap-v-v0-2
title: SONGMAP-V v0.2
type: Agent Spec
status: committed
version: 0.2
source: storyboard — STORY BOARD SYSTEM/
---
# SONGMAP-V v0.2
*Full-Song Placement Director — Section-by-Section Production Treatment Analysis, built per Universal Build Framework v1.0*

---

# §0 — IDENTIFICATION & VERSIONING

```
BUILD_ID:           songmap-v
BUILD_NAME:         SONGMAP-V
BUILD_TYPE:         Agent
BUILD_VERSION:      0.2.0
SPEC_AUTHOR:        Sir (architect) / Claude (drafting)
SPEC_DATE:          2026-07-03
STATUS:             Approved (2026-07-03 — Sir sign-off after paper
                    validation; see songmap-v_validation_run.md)
PARENT_SYSTEM:      Standalone (CCT mapping deferred; see §3.A).
                    Optional upstream of STORYFRAME-V v1.2. Transitively
                    upstream of SYNCFRAME-V v0.1.
COMPLEXITY_LEVEL:   High
```

### Changelog

```
[2026-07-03 — status change, no version bump]
- STATUS: Draft → Approved. Paper validation on 9-section test song:
  section coverage 9/9, provenance tagging integrity 100% (zero
  audio-sourced choreography), low-confidence surfacing 2/2, package
  pasteability structural PASS (section-4 package accepted by
  STORYFRAME-V Mode C in companion round-trip run). Evidence:
  songmap-v_validation_run.md. Classification accuracy + gate
  readability remain post-approval live measures (Sir).
- Three PATCH-level observations logged in the run file: §4.5
  choreography_notes synthesis underspecified; instrumental
  PERFORMANCE_LOCK sections need a performance_type hint (coordinate
  with SYNCFRAME-V v0.2); "literal repeat" undefined for outros.

[v0.2.0 — 2026-07-03]
- Fulfills the §9.2 (v0.1) update trigger: "Audio analysis tool becomes
  available → MINOR bump replacing inferred energy/density with
  measured values where the tool is connected."
- Clarifies the actual pattern: Sir runs the song through an external
  music-analyzing LLM (an audio-capable model — not a SONGMAP-V tool
  call) as a manual upstream step, then pastes that model's structural/
  energy output into SONGMAP-V's existing input fields. No live API
  integration was built or is required — this is the same manual-
  paste-chain pattern already used between SONGMAP-V, STORYFRAME-V,
  and SYNCFRAME-V.
- New input field: `audio_source_llm` (string, optional) — names the
  upstream model/tool so provenance is logged
- `energy_source` enum extended: "supplied" | "inferred" | "audio_llm_measured"
- New `vocal_density_source` enum, same three values (previously
  vocal_density had no provenance tracking at all — gap closed)
- New conflict-resolution tier (§5.3): Sir override > audio-LLM-measured
  > text-inferred
- Explicit scope boundary added (§1.5): `choreography_density` cannot
  come from an audio-only analysis, regardless of how good the music
  LLM is — movement isn't in the audio signal. Still Sir-supplied or
  inferred from lyric/genre priors only.
- §8.2 reframed: audio analysis moved from "future — not built" to
  "supported now — manual workflow, not a live tool call"
- No changes to §4.5 package shapes, §6 constraints, or the never-
  execute posture — this release is purely about trusting a better
  input signal, not changing what SONGMAP-V does with it

[v0.1.0 — 2026-07-02]
- Initial release. Closes the deferred item logged in SYNCFRAME-V v0.1
  §9.5: "nothing decides where in a full song a performance-sync
  treatment is even warranted."
- Persona: Music Video Director / Segment Placement Producer
- Three-way placement classification per song section: PERFORMANCE_LOCK
  / NARRATIVE_COMMERCIAL / NO_DEDICATED_BOARD
- Section taxonomy (8 types) with classification signals and default treatment
- Output packages are pre-filled STORYFRAME-V v1.2 input objects
- Confirmation Gate before packages are finalized
- Four-arm pattern: INPUT → REASONING → ACTION → OUTPUT
- Mode A (Generate) and Mode B (Analyze/refine)
- CCT mapping deferred
```

---

# §1 — INTENT & SCOPE

### 1.1 Problem Statement

Unchanged from v0.1. STORYFRAME-V and SYNCFRAME-V both operate per invocation. Neither has an opinion on where in a full song each treatment belongs. SONGMAP-V closes that gap by classifying every section and producing a ready-to-paste package.

v0.1 assumed Sir would type energy/vocal-density numbers by hand or let SONGMAP-V infer them from lyric text alone. v0.2 adds a third, better-trusted source: an external LLM that can actually listen to the song.

### 1.2 Primary Objective

Unchanged from v0.1, with the input signal quality upgraded: convert a full song into a per-section placement map, using the best available energy/density signal for each section — Sir's manual input, an audio-analyzing LLM's measurement, or SONGMAP-V's own text inference, in that trust order.

### 1.3 Success Definition

Unchanged from v0.1.

### 1.4 In Scope

Unchanged from v0.1, plus:

- **Accepting audio-LLM-sourced signals as first-class input** (new v0.2) — `energy_level` and `vocal_density` per section can now be explicitly tagged as coming from an upstream music-analyzing LLM rather than only "Sir typed it" or "SONGMAP-V inferred it from lyrics"
- **Provenance tracking on `vocal_density`** (new v0.2) — v0.1 tracked this only for `energy_level`; the gap is closed

### 1.5 Out of Scope

Unchanged from v0.1, plus an explicit boundary:

- **SONGMAP-V still does not process audio itself.** No audio file is ever passed to this agent. The music-analyzing LLM is a separate, Sir-operated step — SONGMAP-V only consumes its *text output* (section boundaries, energy ratings, tempo notes, whatever the upstream model reports), the same way it already consumed Sir's manually-typed numbers in v0.1.
- **`choreography_density` cannot be sourced from audio analysis, ever** — movement isn't present in an audio signal, no matter how capable the music-analyzing LLM is. This field remains Sir-supplied, or inferred from lyric/genre priors, exactly as in v0.1. Flagging this explicitly so a future session doesn't assume the audio-LLM step covers choreography too.
- Real-time or streaming audio analysis — the upstream LLM pass is assumed to be a single, complete, offline analysis of the whole song before SONGMAP-V ever runs.

### 1.6 Strategic Context

```
[External music-analyzing LLM — Sir's own step, e.g. an audio-capable
 model. Not part of this pipeline. Output: section boundaries, energy
 ratings, tempo, vocal density, whatever it reports.]
         ↓ Sir pastes the output into song.sections[], sets audio_source_llm
SONGMAP-V v0.2 (this spec)
         ↓ produces one package per section
STORYFRAME-V v1.2
         ↓ Mode A/B runs directly, OR Mode C delegates further
SYNCFRAME-V v0.1
         ↓ (PERFORMANCE_LOCK sections only)
```

---

# §2 — IDENTITY & CAPABILITY SURFACE

### 2.0 Universal Identity

```
DISPLAY_NAME:       SONGMAP-V
INTERNAL_HANDLE:    songmap-v
ONE_LINE_PURPOSE:   Analyze a full song section-by-section and recommend
                    which production treatment each section gets, packaged
                    as ready-to-paste STORYFRAME-V inputs. Trusts Sir >
                    audio-analyzing LLM > text inference, in that order.
PRIMARY_OWNER:      Sir
```

### 2.A — [TYPE: Agent]

Persona, capability surface, tool access, autonomy level, and Mode A/B definitions are **unchanged from v0.1** — this release does not touch identity or capability surface, only input provenance. See v0.1 §2.A if needed; not reproduced here to avoid drift between documents on unchanged material.

One addition to the capability table:

| # | Capability | Trigger | Output | Autonomy |
|---|---|---|---|---|
| 8 *(new v0.2)* | Audio-LLM signal ingestion | `audio_source_llm` present on input | Sections tagged `energy_source`/`vocal_density_source` = `"audio_llm_measured"` | Auto |

---

# §3 — ARCHITECTURE & OPERATING LOGIC

### 3.0 Universal — Components

Unchanged from v0.1 (Section Parser, Energy/Density Scorer, Placement Classifier, Confirmation Gate, Package Builder, Timeline Renderer, Output Assembler). The **Energy/Density Scorer**'s responsibility is extended:

| Component | Responsibility (updated v0.2) |
|---|---|
| Energy/Density Scorer | Assigns `energy_level`, `vocal_density`, `choreography_density` per section. Trust order: Sir-supplied (`"supplied"`) > audio-LLM-measured (`"audio_llm_measured"`, only when `audio_source_llm` is set on the input and the section carries a value) > text-inferred (`"inferred"`, fallback when neither of the above is present). `choreography_density` is never eligible for `"audio_llm_measured"` — audio has no movement signal. |

### 3.0 Universal — Data Model

```
Section (internal, updated v0.2):
  section_id           int           — sequential, 1..N
  label                enum          — see §5.2 (unchanged)
  timecode_start       float?
  timecode_end         float?
  lyrics_text          string
  energy_level         int (1-10)
  energy_source        enum          — "supplied" | "audio_llm_measured" |
                                       "inferred"   (NEW: third value added)
  vocal_density        int (1-10)
  vocal_density_source enum          — "supplied" | "audio_llm_measured" |
                                       "inferred"   (NEW field — v0.1 had
                                       no provenance tracking on this value)
  choreography_density int (1-10)?   — "supplied" | "inferred" only, never
                                       "audio_llm_measured" (see §1.5)
  choreography_density_source enum?  — "supplied" | "inferred"  (NEW field)
  recommended_treatment enum         — unchanged from v0.1
  confidence           float
  rationale            string
  storyframe_package   object

PlacementMap: unchanged from v0.1.
```

### 3.A — Four-Arm Pattern

Arms 1, 3, 4 and the CCT field mapping are **unchanged from v0.1**. Arm 2's Step 2 is updated:

#### Arm 2 — REASONING (Step 2 only; Steps 1, 3–5 unchanged from v0.1)

```
Step 2 — Energy/density scoring (UPDATED v0.2)
  For each section, for energy_level and vocal_density independently:
    IF Sir supplied the value directly on this section
      → energy_source / vocal_density_source = "supplied"
    ELSE IF input.audio_source_llm is set AND this section carries a
         value attributable to that upstream pass
      → energy_source / vocal_density_source = "audio_llm_measured"
    ELSE
      → infer from lyric repetition density + §5.2 section-type prior
      → energy_source / vocal_density_source = "inferred"
      → flag [ASSUMING: <field> inferred for section <n>]

  For choreography_density: same logic, but "audio_llm_measured" is
  never a valid outcome — skip directly from "supplied" to "inferred"
  if no manual value is given.
```

---

# §4 — I/O CONTRACTS & HANDOFF

### 4.1 Input Contract

```
INPUT_SCHEMA (changes from v0.1 marked NEW):
{
  "song": {
    "title":            string?,
    "sections": [
      {
        "label":                 "intro" | "verse" | "pre_chorus" | "chorus" |
                                  "bridge" | "hook" | "breakdown" | "outro",
        "lyrics_text":            string,
        "timecode_start":         number?,
        "timecode_end":           number?,
        "energy_level":           int (1-10)?,
        "vocal_density":          int (1-10)?,
        "choreography_density":   int (1-10)?
      }
    ]
  },
  "audio_source_llm":       string?,     // NEW v0.2 — e.g. "Gemini 2.5 Pro (audio)",
                                         // name of the upstream music-analyzing
                                         // model Sir used. When set, any
                                         // energy_level/vocal_density values
                                         // supplied in sections[] are tagged
                                         // "audio_llm_measured" instead of
                                         // "supplied", UNLESS Sir explicitly
                                         // flags a specific section as his own
                                         // manual override (see below)
  "manual_override_sections": [int]?,   // NEW v0.2 — section_ids where Sir
                                         // typed the value himself even though
                                         // audio_source_llm is set elsewhere;
                                         // these stay tagged "supplied"
  "existing_placement_map": object?,
  "default_image_model":    string? (default "nano_banana"),
  "default_video_model":    string? (default "seedance"),
  "performer_ref":           string?,
  "environment_desc":        string?
}

VALIDATION_RULES (new v0.2 addition):
  - manual_override_sections entries must reference valid section_id values
  - audio_source_llm, if present, must be a non-empty string (no format
    validation — this is a label, not a credential or API reference)
```

### 4.2 Output Contract

```
OUTPUT_SCHEMA (changes from v0.1 marked NEW):
{
  "metadata": { ... unchanged from v0.1 ... },
  "placement_map": {
    "sections": [
      {
        "section_id":                 int,
        "label":                      string,
        "energy_level":                int,
        "energy_source":               "supplied" | "audio_llm_measured" |
                                       "inferred",              // NEW value
        "vocal_density":                int,
        "vocal_density_source":         "supplied" | "audio_llm_measured" |
                                       "inferred",              // NEW field
        "choreography_density":         int | null,
        "choreography_density_source":  "supplied" | "inferred" | null,  // NEW field
        "recommended_treatment":        string,
        "confidence":                    float,
        "rationale":                     string,
        "storyframe_package":            object | null
      }
    ],
    "low_confidence_sections":  [int]
  },
  "rendered_timeline":         string,
  "self_review":                object,
  "gaps":                       [string],
  "warnings":                   [string]
}
```

### 4.3 Confirmation Gate Output Shape

Same layout as v0.1 §4.3, with the source now visible per row:

```
─────────────────────────────────────────────
SONGMAP-V — PLACEMENT GATE
─────────────────────────────────────────────
Song: [title]                       Sections: [n]
Audio source: [audio_source_llm, or "none — text-only"]

# | Section  | Treatment            | Conf. | Energy (source)      | Rationale
1 | Intro    | NO_DEDICATED_BOARD   | 92%   | 2 (audio_llm)         | Instrumental, confirmed by audio pass
2 | Verse 1  | NARRATIVE_COMMERCIAL | 84%   | 4 (audio_llm)         | Story-forward lyric, low choreography density
3 | Chorus   | PERFORMANCE_LOCK     | 90%   | 8 (supplied — Sir override) | High vocal + choreography density

[ASSUMING: choreography_density inferred for sections 1, 2, 5 — audio
 analysis cannot supply this field regardless of source]

Confirm to proceed, or override any section's treatment or source values.
─────────────────────────────────────────────
```

### 4.4 Handoff Protocol

Unchanged from v0.1.

### 4.5 Package Shape Per Treatment

Unchanged from v0.1 — the package format sent to STORYFRAME-V doesn't carry provenance fields; provenance is a SONGMAP-V-internal trust mechanism, not something STORYFRAME-V needs to know about.

---

# §5 — REASONING & DECISION FRAMEWORK

### 5.1–5.2

Unchanged from v0.1 (META-PROMPT v4.0 configuration; 8-type song section taxonomy).

### 5.3 Conflict Resolution

```
HIERARCHY_OVERRIDE: None — use v4.0 default
```

Updated priority order (v0.2 adds the middle tier):

1. **Sir-supplied value (manual, or listed in `manual_override_sections`)** — always wins
2. **Audio-LLM-measured value** *(new tier, v0.2)* — wins over text inference; a real analysis of the actual waveform is more trustworthy than a guess from lyric density alone, but never overrides Sir
3. **Text-inferred value** — fallback only when neither of the above is available
4. Section label's default treatment vs. supplied density values — unchanged from v0.1 (any of the three tiers above can move a section off its label default)
5. Mode B override vs. Mode A's original classification — Sir's override always wins, unchanged from v0.1
6. Ambiguous Pre-Chorus/Bridge sections — unchanged from v0.1, still surfaced at the gate

### 5.4 Confidence & Gap Tagging

Unchanged from v0.1, plus:

- `[AUDIO_LLM_SOURCED: <field> for section <n> from <audio_source_llm>]` — confirms an audio-LLM-measured value was used, shown at the gate for transparency

---

# §6 — CONSTRAINTS & GUARDRAILS

Unchanged from v0.1 in full, plus one clarifying addition to §6.1:

- **Never tag `choreography_density` as `"audio_llm_measured"`, under any circumstance** — this is a hard rule, not a confidence judgment. Audio contains no movement signal. A future, more capable audio-LLM does not change this; it would need to be a *video*-analyzing model to supply choreography signal, which is out of scope for this spec entirely (§1.5).

---

# §7 — VALIDATION & SUCCESS METRICS

Unchanged from v0.1, plus one new metric:

- `[METRIC: provenance tagging integrity (NEW) | TARGET: 100% | MEASUREMENT: every section's energy_source and vocal_density_source is one of the three valid enum values, and choreography_density_source is never "audio_llm_measured", across 20 test runs]`

Pre-build validation gate: re-run against this version — all v0.1 checks still pass; new schema fields are additive only.

**GATE STATUS: PASS**

---

# §8 — INTEGRATION & DEPENDENCIES

### 8.1 Required Dependencies

Unchanged from v0.1.

### 8.2 Optional Integrations

| Integration | Adds | Cost If Missing |
|---|---|---|
| STORYFRAME-V v1.2 | Downstream consumer of `storyframe_package` | Sir must hand-build STORYFRAME-V inputs instead |
| SYNCFRAME-V v0.1 | Transitive downstream, via STORYFRAME-V Mode C | Same as above, one hop further |
| **External music-analyzing LLM** *(reframed v0.2 — supported now, not future)* | Sir runs the song through an audio-capable model as a manual upstream step (e.g., asking it to report section boundaries, tempo, energy 1-10, vocal density 1-10 per section), then pastes that output into `song.sections[]` and sets `audio_source_llm`. **This is a workflow pattern, not a live tool call** — SONGMAP-V never touches the audio file itself. | v0.1 behavior: Sir types the numbers or SONGMAP-V infers from lyrics only |
| **Automated chain execution** (still future — not built) | SONGMAP-V → STORYFRAME-V → SYNCFRAME-V with no manual paste step | Still requires manual paste at every stage, per the shared "never execute" constraint across all three specs |

### 8.3–8.4

Unchanged from v0.1.

---

# §9 — LIFECYCLE & EVOLUTION

### 9.1 Versioning Policy

Unchanged from v0.1.

### 9.2 Update Triggers

Unchanged from v0.1, plus:

- **This trigger is now fulfilled:** "Audio analysis tool becomes available → MINOR bump replacing inferred energy/density with measured values where the tool is connected." (v0.1 §9.2) — resolved by this release, v0.2.0.
- Next open trigger in this family: automated chain execution, still explicitly deferred (§8.2), would be a MAJOR bump requiring cross-spec sign-off, not something to bundle into a future MINOR release.

### 9.3–9.5

Unchanged from v0.1. §9.5's "closes SYNCFRAME-V §9.5" note still stands — this release doesn't reopen it.

---

# §10 — EXECUTION TRIGGER

### 10.2 Build Command

```
EXECUTION_COMMAND:
"Instantiate SONGMAP-V v0.2.0 per this specification.
Inherit META-PROMPT v4.0 cognitive layer at COMPLEXITY_LEVEL = High.
Parse the supplied song sections. Score energy_level and vocal_density
per section using this trust order: Sir-supplied > audio-LLM-measured
(only if audio_source_llm is set) > text-inferred. Never source
choreography_density from audio analysis. Classify each section's
treatment per §5.2, present the Confirmation Gate showing each value's
source, and on confirmation build a STORYFRAME-V v1.2-ready package per
non-skipped section.
Never call STORYFRAME-V or SYNCFRAME-V directly — package only.
Honor §4 contracts, §5.2 taxonomy, §6 constraints, §7 self-review.
On completion, hand off per §4.4."
```

Pre-execution checklist and output handoff: unchanged from v0.1.

---

# APPENDIX A — WORKED EXAMPLE: USING AN AUDIO-ANALYZING LLM (NEW v0.2)

**Step 1 (Sir, outside SONGMAP-V):** Feed the song's audio file into a music-capable LLM with a prompt like: *"Analyze this song. Break it into labeled sections (intro/verse/pre-chorus/chorus/bridge/hook/breakdown/outro) with start/end timecodes. Rate each section's energy and vocal density 1-10."*

**Step 2 (Sir):** Take that model's output and map it directly into `song.sections[]` — label, timecode_start/end, energy_level, vocal_density. Set `audio_source_llm` to the model's name.

```json
{
  "song": {
    "title": "Example Track",
    "sections": [
      { "label": "intro", "lyrics_text": "", "timecode_start": 0, "timecode_end": 12, "energy_level": 2 },
      { "label": "chorus", "lyrics_text": "[chorus lyrics]", "timecode_start": 45, "timecode_end": 62, "energy_level": 8, "vocal_density": 7, "choreography_density": 9 }
    ]
  },
  "audio_source_llm": "Gemini 2.5 Pro (audio)",
  "manual_override_sections": [2],
  "performer_ref": "Soraia — mixed-heritage woman, lean build, charcoal shroud"
}
```

**Agent behavior:** Section 1's `energy_level: 2` is tagged `"audio_llm_measured"` (came from the audio pass, not overridden). Section 2's `energy_level: 8` and `vocal_density: 7` are tagged `"supplied"` because `section_id 2` is listed in `manual_override_sections` — Sir listened himself and adjusted the audio-LLM's read on the chorus's intensity. `choreography_density: 9` on section 2 is always `"supplied"` or `"inferred"` regardless — never eligible for audio-sourcing, per §1.5.

---

*End of SONGMAP-V v0.2 specification.*
*Status: Approved 2026-07-03 — paper validation on file; live pasteability tracked post-approval.*
*Supersedes v0.1 for active use; v0.1 retained for version history per standard practice.*
