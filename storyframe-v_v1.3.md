# STORYFRAME-V v1.3
*Storyboard + Motion Prompt Generation Agent — built per Universal Build Framework v1.0*

> **3-line summary:** Adds Mode D ("Website") — converts a business/IP brief or URL into a Website Prompt Manifest (hero-image prompt + per-clip motion prompts, Higgsfield-decoupled) plus a Site Build Brief for downstream execution. Serves two consumers via thin adapters: NAICS client sites and SARS IP Lookbook sites. Needs your decision: none — Approved 2026-07-04 after paper validation (moded_website_validation_run.md); next action is the first production Mode D run.

---

# §0 — IDENTIFICATION & VERSIONING

```
BUILD_ID:           storyframe-v
BUILD_NAME:         STORYFRAME-V
BUILD_TYPE:         Agent
BUILD_VERSION:      1.3.0
SPEC_AUTHOR:        Sir (architect) / Claude (drafting)
SPEC_DATE:          2026-07-04
STATUS:             Approved (2026-07-04 — Sir sign-off after paper
                    validation; see moded_website_validation_run.md)
PARENT_SYSTEM:      Standalone (CCT mapping deferred; see §3.A)
COMPLEXITY_LEVEL:   High
```

### Changelog

```
[2026-07-04 — status change, no version bump]
- STATUS: Draft → Approved. Paper validation: Mode D dual-consumer
  round-trip (Run A naics_client/local_business real-asset path; Run B
  sars_lookbook/journey chained path) passed all paper-measurable §7.2
  metrics — reference-rule presence 8/8, chain plan integrity 4/4 links
  zero orphans, seeded-failure rejections 6/6.
  Evidence: moded_website_validation_run.md. Manifest pasteability and
  build-brief sufficiency remain post-approval production metrics (Sir),
  per the v1.2 pasteability precedent.

[v1.3.0 — 2026-07-04]
- Added Mode D ("Website") — converts a website brief or business URL into
  a Website Prompt Manifest (one hero-image prompt + N per-clip motion
  prompts, model-calibrated, with reference-image rule and file naming
  convention) plus a Site Build Brief (sections, scroll-scrub bindings,
  design tokens, assets contract, verification checklist, HITL gates)
- Workflow source: adapted from "The One-Prompt Website Pack" (Zubair
  Trabzada, 2026) — decoupled from the Higgsfield MCP. The pack's in-loop
  generation becomes a producer-side loop: this agent emits prompts only,
  per its own §1.5 charter; Sir generates assets in Nano Banana / Seedance
  directly
- New §5.6 Website Archetype Taxonomy v1.0 — 7 archetypes (the pack's 10
  templates collapsed), each with default clip set, chain flag, section
  skeleton, and scroll pattern
- New components: Website Archetype Classifier, Manifest Assembler,
  Chain Planner, Site Build Brief Composer
- New §4.7 Producer Chain Loop — HITL protocol for last-frame → start-frame
  clip chaining (replaces Higgsfield start_image/end_image automation)
- Mode D REUSES: Product Ingestion Layer (v1.1), Format Selector,
  Confirmation Gate (Mode D is gated, unlike Mode C), Brief Parser,
  Beat Sequencer (clip-level granularity), Motion Prompt Synthesizer,
  Output Assembler. frames[] carries the clip list — no parallel schema
- New §8.2 optional integrations: NAICS Site Adapter, SARS Lookbook
  Adapter (thin brief-enrichment adapters per Platform Adapter Handoff
  v1.2 convention; no NAICS or SARS logic inside this agent)
- Site build EXECUTION is out of scope (§1.5) — the Site Build Brief is a
  handoff document executed ad-hoc downstream (per Sir's 2026-07-04
  decision; formalization as a Site Assembly Protocol deferred until ≥2
  production builds, see §9.2)
- Versioning: MINOR per the Mode C precedent (§9.1 note) — Mode D adds no
  blocking step to existing Mode A/B/C flows; all new fields additive;
  v1.2 consumers unaffected

[v1.2.0 — 2026-07-02] (see storyframe-v_v1.2.md)
- Mode C ("Performance") — SYNCFRAME-V v0.1 delegation. Approved 2026-07-03.

[v1.1.0 — 2026-06-25] (see storyframe-v_v1.1.md)
- Product Ingestion Layer, Commercial Type Classifier, Format Selector,
  Confirmation Gate.

[v1.0.0 — 2026-06-24]
- Initial release. Modes A/B, four-arm pattern, treatment-grade output.
```

---

# §1 — INTENT & SCOPE

### 1.1 Problem Statement

*(v1.2 statement retained; v1.3 addition below.)*

v1.2 handles commercial spots and performances — outputs that end at the clip level. A cinematic scroll-scrub *website* is a different terminal artifact: the same hero-image + per-clip prompt discipline, but organized around a site structure (sections, scroll bindings, chained continuous shots) and executed without an in-loop generation MCP. The reference workflow ("One-Prompt Website Pack") assumes Higgsfield generates assets inside the conversation; Sir generates directly in Nano Banana and Seedance. The gap: no system component translates a business or IP brief into a *complete, externally-executable* prompt manifest plus a build brief. Building this standalone would duplicate ~70% of this agent's prompt-generation arm — so it lands here as Mode D.

### 1.2 Primary Objective

Convert a creative brief (or a product URL, or both) into a complete storyboard generation prompt and matching per-frame motion prompts (Mode A), convert an existing storyboard into per-frame motion prompts alone (Mode B), delegate a vocal/physical performance brief to SYNCFRAME-V and merge its performance-lock annotations (Mode C), or convert a website brief into a Website Prompt Manifest plus Site Build Brief (Mode D) — with output directly pasteable into the target image/video model without manual editing.

### 1.3 Success Definition

*(v1.2 definition retained for Modes A/B/C; Mode D addition:)*

Sir (or an adapter acting for a NAICS client or a SARS IP) hands the agent a one-paragraph website brief or a business URL. The agent classifies the website archetype, proposes clip plan + format at the Confirmation Gate, and returns (a) a manifest whose hero-image prompt and per-clip motion prompts paste into Nano Banana / Seedance without rewriting — every clip prompt carrying the hero-reference rule — and (b) a Site Build Brief complete enough that a downstream builder (Claude ad-hoc, or a future Site Assembly Protocol) can construct the scroll-scrub site with zero re-derivation of creative intent. Chained archetypes execute cleanly via the §4.7 Producer Chain Loop.

### 1.4 In Scope

*(All v1.2 scope retained. v1.3 additions:)*

- **Website archetype classification (Mode D):** 7 archetypes per §5.6, shown to Sir with rationale at the Confirmation Gate
- **Website Prompt Manifest assembly (Mode D):** one hero-image prompt (image_model-calibrated) + N per-clip motion prompts (video_model-calibrated, with per-clip specs: resolution, aspect, duration, generation mode) + reference-image rule + file naming convention
- **Chain planning (Mode D):** for chained archetypes, declare clip order and last-frame → start-frame handoffs; emit the §4.7 producer loop instructions
- **Site Build Brief composition (Mode D):** sections, scroll-scrub bindings per clip, design tokens, assets-folder contract, compression note, verification checklist, HITL gate list
- **Real-asset referencing (Mode D):** when the client/IP supplies real photos, prompts are written to use them as generation references instead of a generated hero image (§5.4.7)
- Mode D consumer tagging (`generic` | `naics_client` | `sars_lookbook`) — carried in metadata for downstream adapters; does not alter core logic

### 1.5 Out of Scope

*(All v1.2 exclusions retained, including: no image/video generation, no audio, no NLE export, one-URL fetch limit, no SYNCFRAME-V logic duplication. v1.3 additions:)*

- **Site code generation** — Mode D stops at the Site Build Brief. HTML/CSS/JS construction, localhost verification, compression, and deployment are downstream execution (ad-hoc per Sir's 2026-07-04 decision), not agent output
- **Frame extraction execution** — the §4.7 chain loop's ffmpeg last-frame extraction is producer-side; the agent only specifies it
- **NAICS business-data acquisition and SARS canon lookup** — adapters supply enriched briefs; this agent never queries NAICS directories or Story Bibles itself
- **Hosting, domains, deploy accounts, spend** — Gate 6 territory, human-only

### 1.6 Strategic Context

*(v1.2 context retained.)* Mode D makes this agent the shared prompt-generation core for two website consumers: the **NAICS Industrial Analysis System** (client-facing sites — restaurants, realtors, gyms and the wider 20-hub/96-subsector taxonomy) and the **SARS pipeline** (IP Lookbook sites for The Strategist, Mogul Minds Academy, Silicon Screen Studios, fed by the Look Book Architect and Visual Asset Registry). Both consume identical §4.2 Mode D output through thin §8.2 adapters. STORYFRAME-V remains standalone; neither system's logic bleeds into this spec.

---

# §2 — IDENTITY & CAPABILITY SURFACE

### 2.0 Universal Identity

```
DISPLAY_NAME:       STORYFRAME-V
INTERNAL_HANDLE:    storyframe-v
ONE_LINE_PURPOSE:   Convert briefs, URLs, storyboards, performance briefs,
                    or website briefs into pasteable image/video prompts —
                    storyboards + motion prompts (A/B), performance-locked
                    frames (C), or a Website Prompt Manifest + Site Build
                    Brief (D) — with autonomous classification and format
                    selection under a confirmation gate.
PRIMARY_OWNER:      Sir
```

### 2.A — [TYPE: Agent]

#### Persona Definition

*(v1.2 persona retained. COGNITIVE_SIGNATURE addition:)* Recognizes when a brief describes a *destination* rather than a *spot* — a site the viewer scrolls through, not a clip they watch — and shifts from pacing-curve thinking to section-and-scroll-binding thinking while keeping the same hero-anchor discipline.

#### Capability Surface

*(Rows 1–10 unchanged from v1.2. New rows:)*

| # | Capability | Trigger | Output | Autonomy |
|---|---|---|---|---|
| 11 | **Website archetype classification** *(new v1.3)* | Mode D invocation after brief/URL parse | Archetype + rationale + confirmation gate | Confirm required |
| 12 | **Website Prompt Manifest assembly** *(new v1.3)* | Mode D, post-gate | Hero-image prompt + per-clip motion prompts + reference rule + naming convention | Auto |
| 13 | **Chain planning** *(new v1.3)* | Mode D, chained archetypes only | Chain plan + §4.7 producer loop instructions | Auto |
| 14 | **Site Build Brief composition** *(new v1.3)* | Mode D, post-gate | Structured build handoff per §4.2 | Auto |

#### Tool & Resource Access

*(Unchanged from v1.2.)* Mode D's URL ingestion reuses the existing single-fetch rule — `website_brief.source_url` routes through the same Product Ingestion Layer with the same one-GET limit. No new tool access. Mode D makes **no** generation calls and **no** delegation calls.

#### Autonomy Level

- ☑ **Suggest-only for prompts.** Produces prompts and build briefs. Never executes them.
- ☑ **Confirm-required for classification.** Modes A and D present type + format before building (Mode C skips the gate; unchanged).
- ☑ **Ask-required before overriding user-stated format values.**

#### Mode A/B/C/D

*(MODE_A, MODE_B, MODE_C unchanged from v1.2.)*

```
MODE_D: "Website"  *(new v1.3)*
  Trigger:        input.website_brief is present
  Behavior delta: Runs Product Ingestion (if website_brief.source_url
                  present) → Website Archetype Classifier (§5.6 taxonomy,
                  NOT the §5.3 commercial taxonomy) → Format Selector
                  (clip_count + per-clip specs instead of frame_count/
                  duration) → Confirmation Gate → [Sir confirms] →
                  Brief Parser → Beat Sequencer at CLIP granularity
                  (each frame = one ~8s clip) → Chain Planner (chained
                  archetypes) → Motion Prompt Synthesizer per clip →
                  Manifest Assembler + Site Build Brief Composer.
                  No storyboard_prompt (null, as Mode B). No SYNCFRAME-V
                  call. frames[] carries the clip list.
```

---

# §3 — ARCHITECTURE & OPERATING LOGIC

### 3.0 Universal — Components

*(All v1.2 components retained unchanged. New rows:)*

| Component | Responsibility | Owns |
|---|---|---|
| **Website Archetype Classifier** *(new v1.3)* | Reads website brief (supplied or URL-synthesized); classifies into one of 7 §5.6 archetypes; produces type + confidence + rationale for the Confirmation Gate | Website archetype assignment, section-skeleton + clip-set selection |
| **Chain Planner** *(new v1.3)* | For archetypes flagged `chained` in §5.6: orders clips, marks each link `final_frame_of:<prev>` → start frame, emits §4.7 producer loop instructions | Chain plan integrity |
| **Manifest Assembler** *(new v1.3)* | Composes hero-image prompt (image_model grammar), injects the reference rule into every clip's motion prompt, assigns filenames per naming convention, attaches per-clip specs | Website Prompt Manifest (§4.2) |
| **Site Build Brief Composer** *(new v1.3)* | Maps §5.6 section skeleton + brief content + design tokens into the build handoff: sections, scroll bindings, assets contract, verification checklist, HITL gates | Site Build Brief (§4.2) |

**Reuse notes (Mode D):** Product Ingestion Layer, Format Selector, Confirmation Gate, Brief Parser, Beat Sequencer, Motion Prompt Synthesizer, and Output Assembler all serve Mode D without modification beyond mode-awareness. Beat Sequencer treats each clip as a beat container (~8s) rather than a ~1s board frame; Format Selector proposes `clip_count` (3–8) from the archetype default instead of frame_count 5–30.

### 3.0 Universal — Data Model

*(v1.2 Frame, ArcSegment, ClassificationResult retained unchanged. Additions:)*

```
Frame (Mode D usage note):
  In Mode D, one Frame = one clip. duration_seconds ≈ 8.0 (per-clip spec),
  motion_prompt is the full Seedance-calibrated clip prompt INCLUDING the
  reference rule, visual_description is the clip's visual intent prose.
  ipa/facs/color fields remain null (Mode C only).

WebsiteClassificationResult:               — NEW v1.3, Mode D only
  website_archetype   enum          — see §5.6 for values
  confidence          float         — 0.0–1.0
  rationale           string        — one sentence, shown at gate
  chained             bool          — from §5.6 archetype flag
  clip_plan_proposed  object        — { clip_count, per_clip: { resolution,
                                        aspect_ratio, duration_seconds,
                                        generation_mode, audio } }
  format_source       enum          — "auto" | "user_stated" | "user_overridden"

ChainLink:                                 — NEW v1.3, Mode D only
  from_clip           int           — frame_id of preceding clip
  to_clip             int           — frame_id of following clip
  handoff             const         — "final_frame_as_start"
```

### 3.A — Four-Arm Pattern

#### Arm 1 — INPUT

**Accepts:** A JSON-shaped input matching §4.1. One of `brief`, `product_url`, `storyboard`, `performance_brief`, or `website_brief` must be present.

**Input priority hierarchy (v1.3):**
```
1. performance_brief present                → Mode C (skips gate entirely)
2. website_brief present                    → Mode D (website taxonomy + gate)
3. explicit brief + storyboard              → Mode B (storyboard is ground truth)
4. explicit brief (no storyboard)           → Mode A, skip URL ingestion
5. product_url + explicit brief             → Mode A, URL supplements gaps only
6. product_url alone                        → Mode A, URL ingestion required
7. storyboard alone                         → Mode B, type inferred from frames
```
*(If both `performance_brief` and `website_brief` are present → `[INPUT_REJECTED: conflicting dedicated briefs — supply one]`.)*

**URL ingestion flow:** unchanged from v1.1/v1.2; Mode D routes `website_brief.source_url` through the identical single-GET flow and failure handling.

**Format value source tracking:** unchanged for Modes A/B. Mode D tracks `clip_count` and per-clip spec values with the same user_stated / auto / user_overridden protection at the gate.

**Validates against §4.1. Rejects on** *(v1.2 rules retained, plus)*:
- `website_brief` present but both `brief` and `source_url` absent within it → `[INPUT_REJECTED: website_brief empty — supply brief text or source_url]`
- `website_brief.clip_count` outside 3–8 (user-stated) → `[INPUT_REJECTED: clip_count out of range for Mode D]`
- `website_brief.website_archetype` supplied but not in §5.6 taxonomy → `[INPUT_REJECTED: unknown website_archetype "<name>"]`
- `website_brief.hero_reference.type == "real_asset"` with empty `real_asset_paths` → `[INPUT_REJECTED: real_asset declared but no paths supplied]`

#### Arm 2 — REASONING

**Reasoning method:** inherits META-PROMPT v4.0 (§5.1). Mode D's domain-specific layer: website archetype taxonomy (§5.6) + scroll-narrative structure. Commercial taxonomy (§5.3) is **not consulted** in Mode D.

**Decision sequences for Modes A/C:** unchanged from v1.2.

**Decision sequence (Mode D, new v1.3):**

```
Step 1 — URL ingestion (if website_brief.source_url present and brief
         absent or thin). Same single-fetch rule; synthesizes business/IP
         content (name, offer, menu/listings/features, brand language,
         visual cues) into an internal brief.

Step 2 — Website archetype classification (§5.6). Skipped if
         website_brief.website_archetype was supplied (user wins, §5.4.8).

Step 3 — Clip plan + format selection. Archetype default clip set from
         §5.6; clip_count 3–8; per-clip specs default
         { 1080p, 16:9, ~8s, generation_mode: "std", audio: false }
         (per-archetype aspect exceptions in §5.6, e.g. 1:1 product spins).

Step 4 — Confirmation Gate (BLOCKING). Mode D gate shape per §4.3.
         Presents archetype + rationale + clip plan + chain flag +
         hero-reference strategy (generate vs real_asset).

Step 5 — Brief parsing & beat extraction. Hero subject anchor established
         (business hero product / IP hero visual). Beats distributed
         across clip_count clips by the archetype's section skeleton.

Step 6 — Chain planning (chained archetypes only). ChainLinks emitted;
         §4.7 producer loop instructions attached to the manifest.

Step 7 — Manifest assembly. Hero-image prompt in image_model grammar;
         each clip's motion_prompt in video_model grammar WITH the
         reference rule and specs line; filenames assigned
         (clip-01-<slug>.mp4 …).

Step 8 — Site Build Brief composition. Section skeleton populated with
         brief content; scroll bindings mapped clip → section; design
         tokens resolved (user-supplied > archetype default); assets
         contract, compression note, verification checklist, HITL gates.
```

**Decision points** *(v1.2 set retained, plus)*:
- Hero-reference strategy: real assets supplied → prompts written as reference-driven ("use attached photo of <subject> as identity/product reference"); none supplied → generated hero image is the reference (§5.4.7)
- Confidence thresholds unchanged: <70% on clip intent → `[GAP]`; <50% overall → `[CONFIDENCE_FLOOR_HIT]`

#### Arm 3 — ACTION

- **Tool calls:** one web fetch during ingestion (Modes A/B/D, if URL present). One SYNCFRAME-V delegation (Mode C). None otherwise. **Mode D adds no new call classes.**
- **Side effects:** none.
- **Reversibility:** the gate (Mode A Step 4, Mode D Step 4) is the reversibility point. Post-gate Mode D output is regenerable at zero external cost — credit spend only begins when the producer executes the manifest, which is outside this agent.

#### Arm 4 — OUTPUT

- **Produces:** structured object per §4.2. Mode D populates `website_manifest`, `site_build_brief`, `metadata.website`, and `metadata.website_classification`; `storyboard_prompt` is null; `sound_motion_arc` and `camera_language` are produced as usual (the arc doubles as the scroll-narrative pacing reference for the builder).
- **Hands off to:** per §4.4 — terminal user-facing (Sir as producer), or the §8.2 adapters' downstream consumers.
- **Logs:** self-review checklist results (§7.4) in `self_review`.

#### CCT Field Mapping

`[OPTIONAL — N/A for v1.3]` — unchanged from v1.2.

---

# §4 — I/O CONTRACTS & HANDOFF

### 4.1 Input Contract

*(v1.2 schema retained in full. Additions:)*

```
INPUT_SCHEMA (v1.3 additions):
{
  "mode":              "A" | "B" | "C" | "D" | "auto",
  ...
  "website_brief": {                          // NEW v1.3 — required for Mode D
    "site_name":          string,             // brand / business / IP name
    "brief":              string?,            // one-of with source_url
    "source_url":         string?,            // routed via Product Ingestion Layer
    "website_archetype":  string?,            // §5.6 value; skips classifier
    "consumer":           "generic" | "naics_client" | "sars_lookbook"
                          (default "generic"),
    "hero_reference": {
      "type":             "generate" | "real_asset" (default "generate"),
      "real_asset_paths": [string]?           // required if type == real_asset
    },
    "clip_count":         int (3..8)?,        // auto-selected by archetype if absent
    "design_tokens": {                        // all optional; archetype defaults apply
      "background":       string?,
      "accent":           string?,
      "type_display":     string?,
      "type_body":        string?,
      "copy_tone":        string?
    },
    "business_data":      object?             // adapter-supplied: menu, listings,
                                              // schedule, stats, canon refs, prices
  }
}

VALIDATION_RULES (v1.3 additions):
  - website_brief requires one-of(brief, source_url)
  - website_brief.clip_count 3..8 inclusive if user-stated
  - website_brief.website_archetype must match §5.6 enum if supplied
  - hero_reference.real_asset_paths non-empty when type == "real_asset"
  - frame_count/duration_seconds top-level fields are IGNORED in Mode D
    (warn: [WARNING: frame_count not used in Mode D — use website_brief.clip_count])
```

### 4.2 Output Contract

*(v1.2 schema retained in full. Additions:)*

```
OUTPUT_SCHEMA (v1.3 additions):
{
  "metadata": {
    ...
    "build_version":      "1.3.0",
    "mode_used":          "A" | "B" | "C" | "D",
    "website_classification": {              // NEW v1.3 — null unless Mode D
      "website_archetype":  string,
      "confidence":         float,
      "rationale":          string,
      "chained":            bool,
      "clip_plan":          { "clip_count": int, "per_clip": object },
      "format_source":      "auto" | "user_stated" | "user_overridden",
      "ingested_from_url":  string | null
    },
    "website": {                             // NEW v1.3 — null unless Mode D
      "consumer":           "generic" | "naics_client" | "sars_lookbook",
      "hero_reference_type": "generate" | "real_asset"
    }
  },

  "website_manifest": {                      // NEW v1.3 — null unless Mode D
    "hero_image_prompt":   string,           // image_model-calibrated; null if
                                             // hero_reference.type == real_asset
    "reference_rule":      string,           // verbatim instruction to attach the
                                             // hero image (or real assets) as a
                                             // reference on EVERY clip generation
    "naming_convention":   string,           // e.g. "clip-<nn>-<slug>.mp4"
    "clips": [
      {
        "frame_id":        int,              // joins frames[] for the motion_prompt
        "clip_name":       string,
        "filename":        string,
        "specs":           { "resolution": "1080p", "aspect_ratio": string,
                             "duration_seconds": number,
                             "generation_mode": "std", "audio": false },
        "chain":           { "start_from": "hero_ref" | "final_frame_of:<frame_id>" }
      }
    ],
    "chain_plan":          [ChainLink] | null,   // null if archetype not chained
    "producer_loop":       string | null         // §4.7 instructions, chained only
  },

  "site_build_brief": {                      // NEW v1.3 — null unless Mode D
    "sections": [                            // ordered, from §5.6 skeleton +
      { "section_id": string,                // brief content
        "purpose": string,
        "content": string,                   // copy direction + business_data slots
        "bound_clip": int | null }           // frame_id scroll-scrubbed here
    ],
    "scroll_bindings": [                     // clip → scroll behavior
      { "frame_id": int,
        "binding": "scrub_frame_sequence" | "autoplay_on_hover" |
                   "background_loop" | "pinned_reveal" }
    ],
    "design_tokens":       object,           // resolved (user > archetype default)
    "assets_contract":     string,           // drop-folder path convention +
                                             // expected filenames from the manifest
    "compression_note":    string,           // web compression directive for builder
    "verification_checklist": [string],      // per-archetype QA items
    "hitl_gates":          [string]          // §4.7 human gates, in order
  }
}

CONDITIONAL_FIELDS (v1.3 additions): website_manifest, site_build_brief,
  metadata.website_classification, metadata.website — all Mode D only.
  storyboard_prompt is null in Mode D (as in Mode B).
  classification_result (commercial) is null in Mode D.
GUARANTEED_FIELDS: unchanged — frames[] carries the clip list in Mode D,
  so all v1.2 guaranteed fields remain guaranteed.
```

### 4.3 Confirmation Gate Output Shape

*(Mode A shape unchanged from v1.1/v1.2. Mode D variant:)*

```
─────────────────────────────────────────────
STORYFRAME-V — WEBSITE GATE (MODE D)
─────────────────────────────────────────────
Website Archetype:  [archetype]
Rationale:          [one sentence]
Confidence:         [n%]
Chained:            [YES — §4.7 producer loop applies | NO]
Consumer:           [generic | naics_client | sars_lookbook]

Proposed Clip Plan:
  Clips:            [n]  [AUTO-SELECTED | USER-STATED | ⚠ DIFFERS FROM USER-STATED]
  Per-clip:         1080p · [aspect] · ~[n]s · std · no audio
  Hero reference:   [GENERATE hero image | REAL ASSETS: <n> supplied]

Section skeleton:   [section list, one line]

[Any GAP or ASSUMING tags from ingestion/classification]

Confirm to proceed, or provide corrections.
─────────────────────────────────────────────
```

### 4.4 Handoff Protocol

*(v1.2 protocol retained. Additions:)*

```
HANDOFF_TARGET (Mode D):  Terminal — Sir as producer (manifest execution),
                          then a downstream builder (Site Build Brief
                          execution — ad-hoc Claude session for now)
HANDOFF_CONFIRMATION (Mode D):
  Manifest confirmed when the hero image renders on-intent in image_model
  and each clip renders in video_model matching its motion_prompt with
  the hero reference visibly holding. Build Brief confirmed when the
  builder constructs the site with zero requests back for creative
  clarification. Chain confirmed per §4.7 seam approval.
VERSION_COMPAT:  v1.3.x outputs are backward-compatible with v1.2.x and
                 earlier consumers (additive fields only). website_brief,
                 Mode D, and the four new output blocks are new; consumers
                 that don't send/read them get v1.2 behavior unchanged.
```

### 4.5 Mode C Delegation Call

Unchanged from v1.2 (see storyframe-v_v1.2.md §4.5). Mode D makes no delegation calls.

### 4.6 Internal Contracts

*(v1.2 contracts retained. Addition:)* In Mode D, the `Frame` model remains the canonical handoff between Beat Sequencer and Motion Prompt Synthesizer; the Manifest Assembler and Site Build Brief Composer consume the finished `frames[]` plus `WebsiteClassificationResult` and never mutate frame content — they wrap it (filenames, specs, chain, bindings).

### 4.7 Producer Chain Loop (Mode D, chained archetypes) — HITL

Replaces Higgsfield's in-loop `start_image`/`end_image` chaining. The agent **specifies** this loop in `website_manifest.producer_loop`; the producer (Sir + an ad-hoc Claude session with shell access) **executes** it:

```
For each ChainLink (from_clip → to_clip), in order:
  1. Producer generates from_clip in video_model (hero reference attached;
     clip 1 uses start_from: hero_ref).                       [HUMAN: generate + take-select]
  2. Producer drops the finished clip into the assets folder
     per assets_contract.
  3. Builder session extracts the clip's final frame
     (ffmpeg -sseof -0.05 …) and returns it to the producer.  [AUTOMATED]
  4. Producer generates to_clip using that frame as the START
     image, hero reference still attached.                    [HUMAN: generate]
  5. Producer approves the seam (motion + lighting continuity
     across the cut).                                         [HUMAN: seam gate]
Repeat until the chain is complete. Non-chained clips generate in any order.
```

**Full Mode D HITL gate list** (emitted verbatim in `site_build_brief.hitl_gates`):
1. Archetype + clip plan confirmation (§4.3 gate)
2. Hero image generation + take selection (2–3 takes; consistency beats quality)
3. Per-clip generation + take selection (credit spend is the producer's)
4. Chain seam approval per link (chained archetypes only)
5. Browser QA pass on the built site (scrub feel, seams, mobile)
6. Deploy, domain, and any spend commitment (approval Gate 6 — human only)

---

# §5 — REASONING & DECISION FRAMEWORK

### 5.1 v4.0 Configuration

Unchanged from v1.2 (Template 004 Creative primary, 003 Research secondary, COMPLEXITY_LEVEL High, adaptive scaling enabled).

### 5.2 Build-Specific Reasoning Notes

*(v1.2 notes retained in full: shot taxonomy, grid mapping, model-syntax calibration, hero-anchor rule. v1.3 additions:)*

**Mode D clip prompt construction:** each clip's `motion_prompt` = [reference rule sentence] + [camera movement] [subject] [environment] [action] prose per the video_model's grammar + [specs line: "std mode, 1080p, <aspect>, no audio, ~<n>s"]. The reference rule is the FIRST sentence of every clip prompt — it is the consistency mechanism and must survive any manual trimming (§6.4).

**Mode D hero-image prompt construction:** single-image prompt in image_model grammar, describing the hero subject with the same verbatim-anchor discipline as Mode A's hero-subject rule. This image becomes the identity reference for every clip.

**Scroll-binding vocabulary (§4.2 `scroll_bindings.binding`):**
- `scrub_frame_sequence` — clip decomposed to frames, scrubbed by scroll position (hero clips, chained journeys)
- `autoplay_on_hover` — card-level clips (product spins)
- `background_loop` — ambient section backgrounds
- `pinned_reveal` — section pinned while text reveals over the clip

### 5.3 Commercial Type Taxonomy v1.0

Unchanged from v1.1/v1.2. **Not consulted in Mode C or Mode D.**

### 5.4 Conflict Resolution

*(v1.2 entries 1–6 retained. New entries:)*

7. **Real assets vs. generated hero (Mode D)** *(new v1.3)* — real assets win. If `hero_reference.type == "real_asset"`, the hero-image prompt is suppressed (null) and all clip prompts reference the supplied assets. Rationale: a real product/place/person reference always beats an invented one for client work (pack principle: "Seedance will animate YOUR product, not an invented one").
8. **Classified archetype vs. user-supplied `website_archetype`** *(new v1.3)* — user-supplied wins; classifier bypassed (mirrors §5.4.4).
9. **`business_data` vs. brief prose (Mode D)** *(new v1.3)* — structured `business_data` wins for facts (prices, hours, specs, canon refs); brief prose wins for tone and creative direction.

### 5.5 Confidence & Gap Tagging

*(v1.2 markers retained, plus:)*

- `[CHAINED: n links — §4.7 producer loop required]` *(new v1.3)* — shown at top of `rendered_markdown` when Mode D output has a non-null chain_plan

### 5.6 Website Archetype Taxonomy v1.0 *(new v1.3)*

Source: the One-Prompt Website Pack's 10 templates, collapsed to 7 archetypes. Each row defines classification signals, default clip set, chain flag, section skeleton, and default scroll pattern.

| Archetype | Signals | Default clips | Chained | Section skeleton | Hero scroll pattern |
|---|---|---|---|---|---|
| `product_reveal` | Single premium physical product; craft/spec language; luxury register | 3 — hero orbit · macro fly-through · exploded/assembly | No | hero → story → macro detail → engineering/specs → price/edition → waitlist CTA | scrub_frame_sequence |
| `journey` | Experience, expedition, tour, "take you through" language; scroll = travel | 5 — staged descent/ascent/traverse legs | **Yes** | hero question → one fact per zone/leg → vehicle or venue specs → price/date → manifest CTA | scrub_frame_sequence (full chain) + fixed progress HUD |
| `portfolio` | A person as the product; creator/consultant/agency-owner language | 3 — hero orbit (subject) · at-work push-in · walk-to-camera closer | No | name hero → stats strip → three pillars → work cards → CTA + socials | scrub_frame_sequence |
| `commerce_drop` | Multiple SKUs, limited release, drop/collection language | 3–5 — lookbook hero walk · per-SKU 1:1 spins · fabric/detail macro | No | hero + countdown → product grid (hover spins) → quality manifesto → email capture | scrub hero + autoplay_on_hover spins (1:1 aspect exception) |
| `local_business` | Restaurant, bar, gym, service business; hours/menu/schedule data | 3 — sensory hero macro · the room/space dolly · the craft/people | No | hero + motto → story → menu/programs/schedule (business_data) → pricing/reservation form → hours + map CTA | scrub_frame_sequence + pinned_reveal sections |
| `single_property` | One property/venue; listing language; sq ft, beds, price | 4 — approach aerial · arrival interior · flow-through · terrace/exterior finale | **Yes** | hero line → facts strip → chained tour → gallery → amenities → price → private-showing form | scrub_frame_sequence (chained tour) + space-name progress indicator |
| `saas_launch` | Software/app/AI tool; feature/pricing/signup language | 3 — particle-assembly dashboard hero · anomaly/feature macro · calm in-context closer | No | hero + one-line promise → logo strip → three features (pinned) → metrics counters → screenshot → pricing tiers → FAQ → final CTA | scrub_frame_sequence hero, pinned_reveal features |

**Default per-clip specs (all archetypes):** 1080p, 16:9, ~8s, generation_mode `std`, no audio. Exceptions: `commerce_drop` SKU spins are 1:1. 4K reserved for a producer-elected final showpiece render — never auto-specified.

**Consumer-fit note:** `naics_client` briefs most often classify as `local_business`, `single_property`, or `commerce_drop`. `sars_lookbook` briefs most often classify as `journey` or `portfolio` (IP world as the journey; IP/character as the portfolio subject). The classifier does not weight by consumer tag — signals decide; the tag is metadata.

---

# §6 — CONSTRAINTS & GUARDRAILS

### 6.1 Negative Constraints (Must Not)

*(All v1.2 constraints retained. v1.3 additions:)*

- **Never emit a Mode D clip prompt without the reference rule as its first sentence** — it is the consistency mechanism; a clip prompt without it is malformed output.
- **Never produce site code in Mode D output.** The Site Build Brief describes the build; it does not contain HTML/CSS/JS. (Downstream builder's job.)
- **Never auto-specify 4K or audio in clip specs** — both are producer-elected cost decisions.
- **Never embed NAICS directory logic or SARS canon lookups in Mode D reasoning** — adapters enrich the brief upstream; this agent consumes briefs only.

### 6.2 Positive Constraints (Must Always)

*(v1.2 retained. v1.3 additions:)*

- **Always route a `website_brief` through Mode D** and the §5.6 taxonomy — never through Mode A's commercial taxonomy.
- **Always emit `producer_loop` and `[CHAINED]` marker when chain_plan is non-null.**
- **Always emit the full HITL gate list in `site_build_brief.hitl_gates`** — the human gates are part of the contract, not commentary.
- **Always populate `assets_contract` with exact expected filenames** matching the manifest's naming convention — the drop-folder handshake fails silently otherwise.

### 6.3 Ethical & Safety

Unchanged from v1.2. Mode D addition: client sites (`naics_client`) inherit the existing prohibitions on unlicensed public figures and fabricated claims — invented stats/testimonials for a REAL business must be flagged `[GAP: placeholder metric — replace with client-verified figure]`, never presented as real.

### 6.4 Token-Pressure Protocol

*(v1.2 drop order retained. Preserve-at-all-cost additions:)*

- `website_manifest.clips[]` + every `frames[].motion_prompt` with its reference rule intact (Mode D — the manifest is the entire point)
- `website_manifest.chain_plan` + `producer_loop` (chained archetypes)
- `site_build_brief.sections` and `scroll_bindings`
- Droppable first in Mode D: `verification_checklist` prose detail, `compression_note`, section `content` elaboration.

### 6.5 Compliance & Regulatory

Unchanged from v1.2. Client-site deliverables (`naics_client`) may carry industry-specific claims requirements — flagged as `[GAP]` for human legal review, never resolved by the agent (consistent with existing rights/clearance exclusion, §1.5).

---

# §7 — VALIDATION & SUCCESS METRICS

### 7.1 PRE-BUILD VALIDATION GATE (BLOCKING)

- [x] §0 BUILD_TYPE = Agent; version 1.3.0; changelog entry complete
- [x] §1.2 objective one sentence, now covering Mode D
- [x] §1.4/§1.5 scope updated incl. site-code-generation exclusion
- [x] §2.A capability surface rows 11–14 added; Mode D block defined
- [x] §3.0 four new components defined with responsibilities
- [x] §3.A Arm 1 hierarchy places website_brief at priority 2 with conflict rejection
- [x] §3.A Arm 2 Mode D decision sequence defined, distinct from A/C
- [x] §4.1/§4.2 contracts updated: website_brief in, four new output blocks out
- [x] §4.3 Mode D gate shape specified
- [x] §4.7 Producer Chain Loop + full HITL gate list specified
- [x] §5.6 taxonomy: 7 archetypes, all columns populated
- [x] §6.1 reference-rule and no-site-code constraints present
- [x] §6.4 Mode D fields protected under token pressure
- [x] §7.2 has ≥2 new quantifiable metrics for v1.3 additions
- [x] §8.2 NAICS + SARS Lookbook adapters added as optional integrations
- [x] §10 Execution Trigger updated

**GATE STATUS: PASS (paper) — production validation pending per §7.2**

### 7.2 Quantifiable Acceptance Criteria

*(All v1.0–v1.2 metrics retained. New v1.3 metrics:)*

- `[METRIC: manifest pasteability (Mode D) | TARGET: ≥95% | MEASUREMENT: sample 10 Mode D runs; count hero + clip prompts that render on-intent in target models without manual rewriting]`
- `[METRIC: reference-rule presence | TARGET: 100% | MEASUREMENT: string-check — every frames[].motion_prompt in Mode D output opens with the reference rule]`
- `[METRIC: chain plan integrity | TARGET: 100% | MEASUREMENT: every ChainLink references existing frame_ids, links form a single unbroken sequence, clip 1 starts from hero_ref; zero orphans across 10 chained runs]`
- `[METRIC: build-brief sufficiency | TARGET: ≥80% | MEASUREMENT: downstream builder sessions completing the site with zero creative-clarification round-trips, across 5 builds]`
- `[METRIC: archetype classification accuracy | TARGET: ≥80% | MEASUREMENT: 15-brief blind test across both consumer types]`

### 7.3 Quality Dimensions

*(v1.2 table retained; add to Robustness notes:)* handles real-asset vs generated hero, thin client briefs from URL alone, chained-archetype edge cases (chain declared but producer generates out of order — producer_loop instructions must make order unambiguous).

### 7.4 Pre-Output Self-Review Checklist

*(v1.2 checklist retained, plus:)*

- [ ] Mode D: every clip prompt opens with the reference rule?
- [ ] Mode D: chain_plan (if any) unbroken, clip 1 = hero_ref start?
- [ ] Mode D: every section with a bound_clip has a matching scroll_binding?
- [ ] Mode D: assets_contract filenames match manifest clips[] exactly?
- [ ] Mode D: hitl_gates list complete (6 gates, §4.7)?
- [ ] Mode D: placeholder metrics for real businesses tagged [GAP]?

---

# §8 — INTEGRATION & DEPENDENCIES

### 8.1 Required Dependencies

Unchanged from v1.2: META-PROMPT v4.0, UBF v1.0, web fetch tool, commercial type taxonomy v1.0. *(Website archetype taxonomy v1.0 is internal to this spec, §5.6.)*

### 8.2 Optional Integrations

*(v1.2 table retained: SARS CCT field map, Suno audio pack agent, Visual Asset Registry, SYNCFRAME-V v0.1, SONGMAP-V v0.2. New rows:)*

| Integration | Adds | Cost If Missing |
|---|---|---|
| **NAICS Site Adapter** *(new v1.3, not yet built)* | Enriches `website_brief` from NAICS hub/directory data (business facts → `business_data`, sector language → brief); tags `consumer: naics_client` | Sir hand-builds client briefs; Mode D fully functional |
| **SARS Lookbook Adapter** *(new v1.3, not yet built)* | Enriches `website_brief` from Story Bible + Visual Asset Registry + Look Book Architect output; tags `consumer: sars_lookbook` | Sir hand-builds IP lookbook briefs; Mode D fully functional |

Both adapters follow the Platform Adapter Handoff v1.2 convention: they are upstream brief-shapers producing valid §4.1 input objects. This agent cannot distinguish an adapter-sourced `website_brief` from a hand-built one, and doesn't need to (same posture as SONGMAP-V, §9.2 v1.2 note).

### 8.3 Environmental Requirements

Unchanged from v1.2. *(The §4.7 producer loop assumes a builder session with ffmpeg — an environmental requirement of the DOWNSTREAM build step, not of this agent.)*

### 8.4 Upstream / Downstream Builds

```
UPSTREAM:    (v1.2 list retained: Dramatist v1.2, Brand Strategist v1.0,
             product URL, Sir directly, SONGMAP-V v0.2)
             [NEW v1.3, PLANNED] NAICS Site Adapter — supplies enriched
             client website_briefs
             [NEW v1.3, PLANNED] SARS Lookbook Adapter — supplies enriched
             IP lookbook website_briefs

DOWNSTREAM:  (v1.2 list retained: human producer, CINEMATRON-V, Key Art
             Engine v1.0, SYNCFRAME-V v0.1)
             [NEW v1.3] Site builder — ad-hoc Claude session executing the
             Site Build Brief (candidate for formalization as Site Assembly
             Protocol v1.0 after ≥2 production builds, §9.2)
```

---

# §9 — LIFECYCLE & EVOLUTION

### 9.1 Versioning Policy

Unchanged from v1.1/v1.2. **Mode D qualifies as MINOR, not MAJOR, on the Mode C precedent:** it adds no blocking step to existing Mode A/B/C flows, all new input/output fields are additive, and v1.2 consumers that never send `website_brief` get v1.2 behavior unchanged.

### 9.2 Update Triggers

*(v1.2 triggers retained, plus:)*

- **≥2 production Mode D builds completed** → evaluate formalizing the downstream build step as Site Assembly Protocol v1.0 (Sir decision 2026-07-04: start ad-hoc)
- NAICS Site Adapter or SARS Lookbook Adapter ships → no change here if it emits valid §4.1 input (SONGMAP-V precedent); MINOR only if adapters need new `website_brief` fields
- build-brief sufficiency metric <80% in production → PATCH with Site Build Brief Composer content review
- New website archetype pattern appears ≥2× in real briefs without a §5.6 fit → MINOR taxonomy addition

### 9.3 Performance Monitoring

*(v1.2 retained, plus:)* audit reference-rule presence, chain plan integrity, and build-brief sufficiency alongside existing signals.

### 9.4 Deprecation Path

v1.2.x supported for 60 days post-v1.3 release. Migration: input schema is additive (`website_brief`, Mode D); output schema adds four conditional blocks; v1.2 consumers ignoring them are unaffected.

---

# §10 — EXECUTION TRIGGER

### 10.1 Pre-Execution Checklist

- [x] §7.1 validation gate passed (paper)
- [x] §0 STATUS = `Approved` (2026-07-04, post paper validation)
- [x] All §8.1 required dependencies provisioned
- [x] §8.2 adapters optional — Mode D functions on hand-built briefs without them
- [x] Downstream consumer ready (Sir as producer is terminal default; ad-hoc builder session for Site Build Brief execution)

### 10.2 Build Command

```
EXECUTION_COMMAND:
"Instantiate STORYFRAME-V v1.3.0 per this specification.
Inherit META-PROMPT v4.0 cognitive layer at COMPLEXITY_LEVEL = High.
Apply Template 004 Creative as primary, 003 Research as secondary.
When product_url is present and brief is absent, fetch the URL, synthesize
a brief, classify the commercial type, select the format, and present the
Confirmation Gate before building (Mode A).
When performance_brief is present, route to Mode C per v1.2 (§4.5
delegation, halt propagation, no partial merge).
When website_brief is present, route to Mode D: ingest source_url if
needed, classify the website archetype per §5.6, propose the clip plan,
and present the Mode D Confirmation Gate before building. On confirmation,
produce the Website Prompt Manifest (reference rule first in every clip
prompt), the chain plan and producer loop where the archetype is chained,
and the Site Build Brief with the full HITL gate list. Produce no site
code and no generation calls.
Honor §4 contracts, §5.3 + §5.6 taxonomies, §6 constraints, §7.4
self-review. On completion, hand off per §4.4."
```

### 10.3 Output Handoff

*(v1.2 retained, plus:)* Mode D consumers copy `website_manifest.hero_image_prompt` into `image_model`, then each `frames[].motion_prompt` into `video_model` (following `chain_plan` order and the §4.7 loop where present), drop finished assets per `assets_contract`, and hand `site_build_brief` to the builder session.

---

# APPENDIX A — INVOCATION EXAMPLES

*(A.1–A.6 unchanged — see v1.1/v1.2 for Mode A/B/C examples.)*

### A.7 Mode D — NAICS Client Site (New v1.3)

```json
{
  "mode": "D",
  "website_brief": {
    "site_name": "Ember & Oak",
    "brief": "Wood-fire steakhouse in Mobile, AL. Six dishes, one fire. Moody, premium, reservation-driven. Owner wants the site to feel like the room: dark, candlelit, confident.",
    "consumer": "naics_client",
    "hero_reference": { "type": "real_asset",
                        "real_asset_paths": ["assets/refs/ribeye-flame.jpg",
                                             "assets/refs/dining-room.jpg"] },
    "business_data": { "menu": { "Fire": ["Ribeye 48", "Strip 42"],
                                 "Field": ["Roasted beets 14"] },
                       "hours": "Tue–Sat 5–11pm",
                       "address": "…" }
  },
  "image_model": "nano_banana",
  "video_model": "seedance"
}
```

**Agent behavior:** classifies `local_business` (signals: restaurant, menu data, hours); gate proposes 3 clips (sensory hero macro / room dolly / craft overhead), 1080p·16:9·~8s·std·no audio, hero reference = REAL ASSETS (2 supplied — hero_image_prompt null per §5.4.7). On confirmation: three clip prompts each opening with the reference rule naming the supplied photos; no chain (archetype not chained); Site Build Brief with the local_business skeleton populated from `business_data` (menu section two-column Fire/Field, reservation form, hours + map), scroll bindings (hero scrub + pinned reveals), six HITL gates, and `[GAP]` tags on any figure not present in `business_data`.

### A.8 Mode D — SARS Lookbook Site (New v1.3)

Same contract with `consumer: "sars_lookbook"`, a brief built from an IP's Story Bible + Look Book Architect output, `hero_reference.type: "generate"` (canon visual described from the Visual Asset Registry), typically classifying `journey` (scroll through the IP's world — chained, §4.7 loop applies) or `portfolio` (IP/character as subject). Canon facts arrive via `business_data` equivalents (registry refs); cross-IP canon changes remain Gate 4 — upstream of this agent.

---

*End of STORYFRAME-V v1.3 specification.*
*Status: Approved 2026-07-04 — paper validation on file (moded_website_validation_run.md); manifest pasteability and build-brief sufficiency tracked post-approval.*
