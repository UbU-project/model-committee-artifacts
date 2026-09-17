# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0132`  
Question title: `Phase 1b tombstone and best-effort purge semantics`  
Base commit: `87c6cc6c70c9794193dc4d016e5fe197fd5fc7b3`

```markdown
## UBU-Q0132: Phase 1b tombstone and best-effort purge semantics

Status: Open Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0144, UBU-Q0130 Blocks: Phase 1b deletion paths, decomposition undo, projection deletion Resolved by: None Last scored: 2026-09-17 Scored from commit: None

### Question

How should Phase 1b represent deletion and redaction of canonical objects so that tombstone-then-best-effort-purge semantics hold on a single Device?

### Subquestions

1. What tombstone record shape replaces hard deletion for retired Tasks, removed Tasks, and discarded candidates?
2. How does decomposition's parent retirement become a tombstone plus the existing parent snapshot rather than a row deletion?
3. How does undo un-tombstone a retired parent without resurrecting a payload that was purged rather than merely tombstoned?
4. What does a tombstone retain versus purge: id, Compartment label, timestamps, or nothing beyond existence?
5. How do tombstoned canonical objects interact with queued external projection deletions, which are a separate and non-canonical surface?
6. Does a tombstone itself carry envelope metadata per UBU-Q0130?

### Current direction

Phase 1b replaces hard deletes with tombstones. This makes decomposition undo cleaner rather than harder: parent retirement becomes a tombstone, and undo un-tombstones and restores the snapshot the decomposition record already captures. Purge propagation to Devices that were offline when Compartment policy changed remains open under UBU-Q0144 and is not answered by Phase 1b.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "openai-gpt-5-codex-UBU-Q0132-001",
    "provider_id": "openai",
    "model_name": "gpt-5-codex",
    "question_id": "UBU-Q0132",
    "base_commit": "87c6cc6c70c9794193dc4d016e5fe197fd5fc7b3",
    "summary": "Resolve Phase 1b canonical deletion as tombstone-first lifecycle state with best-effort payload purge, preserving only structural identity, causality, audit, undo eligibility, and projection-cleanup refs.",
    "rationale": "This answers the selected subquestions by making tombstones canonical, envelope-bearing mutations rather than hard deletes or unenveloped side effects. It preserves enough structure for idempotency, observed-version checks, decomposition undo, audit, and queued projection deletion while explicitly excluding payload, human-readable Compartment labels, sensitive reasons, and derived explanation content from the retained tombstone after purge/redaction. Undo remains possible only from an authorized retained snapshot, so best-effort purge cannot be silently reversed.",
    "changed_files": [
      "OPEN_QUESTIONS.md",
      "DECISIONS.md",
      "DESIGN.md"
    ],
    "patch": "diff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -2286,7 +2286,7 @@\n \n ## UBU-Q0132: Phase 1b tombstone and best-effort purge semantics\n \n-Status: Open Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0144, UBU-Q0130 Blocks: Phase 1b deletion paths, decomposition undo, projection deletion Resolved by: None Last scored: 2026-09-17 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: UBU-Q0144, UBU-Q0130 Blocks: Phase 1b deletion paths, decomposition undo, projection deletion Resolved by: UBU-D0259 Last scored: 2026-09-17 Scored from commit: None\n \n ### Question\n \n@@ -2307,7 +2307,7 @@\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0259`: Phase 1b canonical deletion is represented as a sync-ready tombstone mutation plus a retained minimal tombstone record, not a hard row deletion. The tombstone keeps stable structural identity, version/provenance, policy-safe Compartment refs, lifecycle timestamps, and projection-deletion/audit refs while purging payload, human labels, sensitive reasons, private evidence, derived explanation content, and Compartment labels when purge or redaction applies. Decomposition retires the parent by tombstoning it and linking to the existing decomposition parent snapshot; undo may un-tombstone only while an authorized restorable snapshot still exists, and otherwise must not recreate purged payload. External projection deletion remains a separate idempotent projection obligation queued from the tombstone and never becomes the canonical delete.\n \n ---\n \ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -3976,3 +3976,21 @@\n Within one origin Device, `(origin_device_id, idempotency_key)` is the duplicate-detection key. Replaying it with the same canonical payload returns the previously recorded result and performs no new write, version increment, side-effect emission, projection push, or derived-state invalidation. Replaying it with a different canonical payload is an `idempotency_key_conflict`; the new payload is not admitted except through an explicit conflict-resolution mutation.\n \n Derived artifacts such as Plans, Calendars, reports, risk summaries, and projection previews are `derived_state`. They carry lineage, provenance, input digests, freshness, and invalidation metadata, but not the canonical mutation envelope unless UbU admits the artifact itself as canonical state. The mutation that records, invalidates, or publishes such an artifact carries the envelope.\n+\n+---\n+\n+## UBU-D0259: Phase 1b tombstones retain structural identity and admit best-effort purge\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a723.1. Resolves `UBU-Q0132`.\n+\n+Phase 1b canonical deletion, retirement, removal, and admitted candidate discard are represented by a tombstone mutation and retained tombstone record, not by hard deletion of the canonical object row. A tombstone is canonical lifecycle state: it says the object id is no longer active or admissible while preserving enough structure for causality, idempotency, undo eligibility, conflict detection, audit, and projection cleanup.\n+\n+The Phase 1b tombstone record retains only policy-safe structure: `object_id`, `object_kind`, the latest object version reference, tombstone lifecycle state and reason code, created/effective/recorded lifecycle timestamps, origin/provenance or mutation-envelope reference, observed-version and observed-policy-version refs needed to validate the tombstone, policy-safe Compartment refs required for local enforcement, optional superseding/decomposition refs, and projection-deletion obligation refs. It does not retain active payload fields such as titles, notes, private evidence, source snippets, human-readable Compartment labels, sensitive reason strings, cached explanations, or derived summaries when those fields are subject to purge or redaction.\n+\n+Every tombstone is admitted through the `UBU-D0258` mutation-envelope path. The envelope belongs to the tombstone mutation; the durable tombstone stores or references the envelope fields needed for replay and audit rather than existing as an unenveloped side table update. Replaying the same tombstone idempotency key with the same canonical tombstone payload returns the existing result and must not emit duplicate projection deletion attempts.\n+\n+Decomposition parent retirement becomes a tombstone of the parent Task plus the existing decomposition record's parent snapshot. Undo is an admitted mutation that observes the tombstone and the decomposition record, clears the tombstone only if policy still permits restoration and an authorized restorable snapshot payload is still present, and increments the parent object's version. If the parent payload or snapshot has been physically purged or redacted beyond restoration, undo must not synthesize the old payload from the tombstone; it may instead create a repair or clarification path requiring new user input.\n+\n+A best-effort physical purge may remove payload blobs, snapshots, derived caches, local projection payload, and external-handle material after or alongside tombstoning, but it does not erase the minimal tombstone while that record is needed for causality, idempotency, audit, or retry. If policy later requires the tombstone itself to be redacted for a Device, the Device receives only an allowed structural representation or an opaque redacted handle; it must not receive forbidden Compartment ids or labels.\n+\n+External projections are separate `projection_state`. Tombstoning a canonical object may enqueue idempotent projection-deletion obligations keyed by projection surface, external object reference, canonical object id, and tombstone mutation, but success or failure of those queued deletions does not determine whether the canonical object is tombstoned. Projection deletion retries may keep only the minimum external handle allowed by policy, and projection diagnostics must remain structural if payload or Compartment identity is restricted.\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -3197,6 +3197,10 @@\n \n Derived artifacts such as Plans, Calendars, reports, risk summaries, and projection previews are `derived_state`: they carry lineage, provenance, input digests, and freshness data, but not the canonical mutation envelope unless a later operation admits a derived artifact itself as canonical state. The mutation that records or invalidates derived state carries the envelope.\n \n+Phase 1b deletion of canonical objects is lifecycle mutation, not row removal. Retired Tasks, removed Tasks, decomposition-retired parents, and admitted discarded candidates become tombstones with stable object id, object kind, current version reference, lifecycle state/reason code, envelope/provenance reference, created/effective/recorded lifecycle timestamps, policy-safe Compartment refs needed for local enforcement, and any decomposition, supersession, audit, or projection-deletion refs needed for replay. Tombstones do not retain active payload fields, private evidence, cached explanations, human-readable Compartment labels, or sensitive reason text after those fields are redacted or purged.\n+\n+A decomposition that retires a parent Task records the parent as tombstoned and relies on the decomposition record's parent snapshot as the restore source. Undo is a new admitted mutation that observes the tombstone and snapshot, un-tombstones the parent only when policy permits and the snapshot payload still exists, and refuses to resurrect content that was physically purged or redacted beyond restoration. Projection deletions caused by tombstones are queued idempotent obligations in `projection_state`; their retry or confirmation status never replaces the canonical tombstone.\n+\n ### 23.2 Zone\n \n A **Zone** is a workspace-like UbU instance context.\n",
    "commit_message": "Resolve Phase 1b tombstone purge semantics",
    "validation_notes": [
      "Generated as a minimal documentation-only proposal touching only OPEN_QUESTIONS.md, DECISIONS.md, and DESIGN.md.",
      "git apply --check succeeded against the live workspace state that includes the provided Q0132 scoring metadata.",
      "The workspace has pre-existing uncommitted OPEN_QUESTIONS.md scoring-date changes relative to base commit 87c6cc6c70c9794193dc4d016e5fe197fd5fc7b3; this proposal is anchored to the selected question text supplied in the request."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0132"
    ],
    "decisions_added": [
      "UBU-D0259"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "openai-gpt-5-codex-UBU-Q0132-001",
    "patch_applies": true,
    "allowlist_passed": true,
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "error": null,
    "normalized_patch": null,
    "warnings": [],
    "ordinary_error": null,
    "recount_error": null,
    "normalization_error": null
  }
]
```

## Provider weights

Provider weights are historical diagnostic context only in v0.2. Do not use
self-trust or author identity as score evidence.

```json
{}
```

## JSON Schema

Your output must satisfy this schema:

```json
{
  "type": "object",
  "additionalProperties": false,
  "required": [
    "scores",
    "selected_proposal_id",
    "selection_rationale"
  ],
  "properties": {
    "scores": {
      "type": "array",
      "items": {
        "type": "object",
        "additionalProperties": false,
        "required": [
          "proposal_id",
          "score",
          "patch_applies",
          "implements_selected_work",
          "preserves_question_schema",
          "avoids_unnecessary_scope",
          "decomposition_quality",
          "risks",
          "required_fixes",
          "rationale"
        ],
        "properties": {
          "proposal_id": {
            "type": "string"
          },
          "score": {
            "type": "integer",
            "minimum": 0,
            "maximum": 100
          },
          "patch_applies": {
            "type": "boolean"
          },
          "implements_selected_work": {
            "type": "boolean"
          },
          "preserves_question_schema": {
            "type": "boolean"
          },
          "avoids_unnecessary_scope": {
            "type": "boolean"
          },
          "decomposition_quality": {
            "type": "string",
            "enum": [
              "none",
              "good",
              "bad",
              "not_applicable"
            ]
          },
          "risks": {
            "type": "array",
            "items": {
              "type": "string"
            }
          },
          "required_fixes": {
            "type": "array",
            "items": {
              "type": "string"
            }
          },
          "rationale": {
            "type": "string"
          }
        }
      }
    },
    "selected_proposal_id": {
      "type": "string"
    },
    "selection_rationale": {
      "type": "string"
    }
  }
}
```

## Scoring requirements

Score each proposal from 0 to 100.

Consider:

- whether the patch applies cleanly;
- whether it implements the selected work;
- whether it preserves the question schema;
- whether it avoids unnecessary scope;
- whether it modifies only allowed files;
- whether it creates useful decomposition if decomposition occurs;
- whether it introduces new risks;
- whether required fixes remain.

Rules:

- Score every proposal in this prompt.
- `selected_proposal_id` must refer to one scored proposal from this prompt.
- Manual override is not allowed in v0.2.
- Prefer a patch that is valid, minimal, auditable, and directly responsive.
- Do not select a proposal whose patch failed mechanical validation.
- Do not score your own provider's proposal unless explicitly asked for diagnostic self-score.

Return only JSON.
