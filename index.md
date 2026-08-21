---
id: okf://storyboard/index
title: storyboard — OKF Bundle Index
type: Index
status: draft
source: OKF migration 2026-08-21 (storyboard bundle, migration order step 5 — final bundle)
tenancy: first-party
owner: i65-digital-labs
base_url: null
feeds_predictive_model: false
---

# storyboard — OKF bundle

**Tenancy: first-party** (i65-owned). The standalone-but-SARS-portable **Storyboard System family** —
three agent specs (STORYFRAME-V, SYNCFRAME-V, SONGMAP-V) that produce storyboard / performance-sync
prompt packages, plus their paper-validation run records. Lives in the separate mounted repo
`STORY BOARD SYSTEM/` (`story-board-system.git`). **Private — never publishes** (`base_url: null`).
Migrated 2026-08-21 (Gate #1, the FINAL bundle → OKF 5 of 5); 11 concepts tagged, validates **0 HARD**.

## Concepts (11)
- [STORYFRAME-V v1.2 Mode C ↔ SYNCFRAME-V v0.1 — Round-Trip Validation Run](modec_roundtrip_validation_run.md) — `okf://storyboard/modec-roundtrip-validation-run` — Validation Record · draft
- [STORYFRAME-V v1.3 Mode D — Website Validation Run](moded_website_validation_run.md) — `okf://storyboard/moded-website-validation-run` — Validation Record · draft
- [SONGMAP-V v0.1](songmap-v_v0.1.md) — `okf://storyboard/songmap-v-v0-1` — Agent Spec · superseded · v0.1
- [SONGMAP-V v0.2](songmap-v_v0.2.md) — `okf://storyboard/songmap-v-v0-2` — Agent Spec · committed · v0.2
- [SONGMAP-V v0.2.0 — Validation Run](songmap-v_validation_run.md) — `okf://storyboard/songmap-v-validation-run` — Validation Record · committed
- [STORYFRAME-V v1.0](storyframe-v_v1.0.md) — `okf://storyboard/storyframe-v-v1-0` — Agent Spec · superseded · v1.0
- [STORYFRAME-V v1.1](storyframe-v_v1.1.md) — `okf://storyboard/storyframe-v-v1-1` — Agent Spec · superseded · v1.1
- [STORYFRAME-V v1.2](storyframe-v_v1.2.md) — `okf://storyboard/storyframe-v-v1-2` — Agent Spec · superseded · v1.2
- [STORYFRAME-V v1.3](storyframe-v_v1.3.md) — `okf://storyboard/storyframe-v-v1-3` — Agent Spec · committed · v1.3
- [STORYFRAME-V v1.0.0 — Validation Run](storyframe-v_validation_run.md) — `okf://storyboard/storyframe-v-validation-run` — Validation Record · draft
- [SYNCFRAME-V v0.1](syncframe-v_v0.1.md) — `okf://storyboard/syncframe-v-v0-1` — Agent Spec · committed · v0.1

## Exempt (1 — okf.yaml → storyboard.exceptions)
- `storyboard-prompt-builder.md` — an external SKILL/loader artifact (name/description trigger
  frontmatter), explicitly flagged in-file as **"NOT storyboard-family canon"**; carries `okf: exempt`.

## Version families
STORYFRAME-V (v1.0→v1.3, current **v1.3** committed; v1.0–v1.2 superseded) and SONGMAP-V
(v0.1→v0.2, current **v0.2** committed; v0.1 superseded) keep prior versions on disk for history —
supersession tracked by `status:` (no `supersedes:` keys, per bundle convention).
