# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0133`  
Question title: `Phase 1b redacted Handle stability and disclosure limits`  
Base commit: `7e750c63e5fb3b38bc9cf2f80632642e95481de5`

```markdown
## UBU-Q0133: Phase 1b redacted Handle stability and disclosure limits

Status: Open Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0150 Blocks: Phase 1b Compartment egress, redaction-identity enforcement Resolved by: None Last scored: 2026-09-17 Scored from commit: None

### Question

Should Phase 1b redacted object and Compartment Handles be stable per Device, per session, or per egress operation, and what may a Handle reveal?

### Subquestions

1. Is a Handle stable for the lifetime of the object, the lifetime of the Device, or a single egress operation?
2. May a Handle reveal existence, time block, and duration only, and what is explicitly withheld?
3. How is the redaction-identity invariant enforced so that no Compartment id or label crosses a denied boundary, not merely no payload?
4. Does Handle stability leak correlation across repeated egress, and is that acceptable for a single operator with a single Compartment set?
5. How do the existing `Handle` projection and clearance egress filter map onto real `CompartmentLabel` values, replacing the three-tier simplification?

### Current direction

The existing `Handle` projection and clearance egress filter went largely unused after the Google Calendar pivot and become load-bearing again under the Phase 1b rule: they are the compartment egress mechanism the redaction-identity invariant requires. The settled part is that a Handle hides content, not existence. The open part is stability granularity and whether per-operation instability is worth the correlation resistance for a single operator.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0133-P-gpt5-codex-20260917",
    "provider_id": "openai",
    "model_name": "gpt-5-codex",
    "question_id": "UBU-Q0133",
    "base_commit": "7e750c63e5fb3b38bc9cf2f80632642e95481de5",
    "summary": "Resolve UBU-Q0133 by adopting the UBU-D0253 rotating-alias policy as the Phase 1b Handle egress rule, with redacted object Handles stable only within receiving Device, source-object version, and projection window; Compartment Handles omitted by default; and all real Compartment identity stripped at the clearance egress filter.",
    "rationale": "UBU-Q0150 already settled the acceptable stability scope and correlation limit. This proposal makes that prior decision load-bearing for Phase 1b implementation by spelling out what the Handle itself may disclose, where real CompartmentLabel values are used, and where they must stop: inside the trusted clearance filter before restricted serialization. It avoids per-operation churn as the default because retry de-duplication and UI continuity need bounded stability, while still rejecting durable cross-window, cross-version, cross-Device, and Compartment-pattern correlation.",
    "changed_files": [
      "OPEN_QUESTIONS.md",
      "DECISIONS.md"
    ],
    "patch": "diff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -2314,25 +2314,25 @@ Resolved by `UBU-D0259`: Phase 1b canonical deletion is represented as a sync-re\n ## UBU-Q0133: Phase 1b redacted Handle stability and disclosure limits\n \n-Status: Open Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0150 Blocks: Phase 1b Compartment egress, redaction-identity enforcement Resolved by: None Last scored: 2026-09-17 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: UBU-Q0150 Blocks: Phase 1b Compartment egress, redaction-identity enforcement Resolved by: UBU-D0260 Last scored: 2026-09-17 Scored from commit: None\n \n ### Question\n \n Should Phase 1b redacted object and Compartment Handles be stable per Device, per session, or per egress operation, and what may a Handle reveal?\n \n ### Subquestions\n \n 1. Is a Handle stable for the lifetime of the object, the lifetime of the Device, or a single egress operation?\n 2. May a Handle reveal existence, time block, and duration only, and what is explicitly withheld?\n 3. How is the redaction-identity invariant enforced so that no Compartment id or label crosses a denied boundary, not merely no payload?\n 4. Does Handle stability leak correlation across repeated egress, and is that acceptable for a single operator with a single Compartment set?\n 5. How do the existing `Handle` projection and clearance egress filter map onto real `CompartmentLabel` values, replacing the three-tier simplification?\n \n ### Current direction\n \n The existing `Handle` projection and clearance egress filter went largely unused after the Google Calendar pivot and become load-bearing again under the Phase 1b rule: they are the compartment egress mechanism the redaction-identity invariant requires. The settled part is that a Handle hides content, not existence. The open part is stability granularity and whether per-operation instability is worth the correlation resistance for a single operator.\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0260`: Phase 1b uses the local rotating-alias policy from `UBU-D0253` as the binding egress rule. Redacted object Handles are stable only for the receiving Device, redacted source-object version, and declared projection window, and rotate on version, redaction, policy, authority, or window changes. Compartment Handles are omitted by default; when policy permits grouping, they are stable only for the receiving Device, projection window, and policy epoch. A Handle itself reveals no semantic fact; the surrounding redacted projection may reveal only policy-allowed existence, timing, duration, and generic status, while object ids, Compartment ids, Compartment labels, reason vocabulary, sequence, stable global identifiers, and inferable Compartment identity are withheld at the clearance egress filter.\n \n ---\ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -3982,15 +3982,31 @@ Derived artifacts such as Plans, Calendars, reports, risk summaries, and project\n ## UBU-D0259: Phase 1b tombstones retain structural identity and admit best-effort purge\n \n **Status:** Accepted \u2192 DESIGN.md \u00a723.1. Resolves `UBU-Q0132`.\n \n Phase 1b canonical deletion, retirement, removal, and admitted candidate discard are represented by a tombstone mutation and retained tombstone record, not by hard deletion of the canonical object row. A tombstone is canonical lifecycle state: it says the object id is no longer active or admissible while preserving enough structure for causality, idempotency, undo eligibility, conflict detection, audit, and projection cleanup.\n \n The Phase 1b tombstone record retains only policy-safe structure: `object_id`, `object_kind`, the latest object version reference, tombstone lifecycle state and reason code, created/effective/recorded lifecycle timestamps, origin/provenance or mutation-envelope reference, observed-version and observed-policy-version refs needed to validate the tombstone, policy-safe Compartment refs required for local enforcement, optional superseding/decomposition refs, and projection-deletion obligation refs. It does not retain active payload fields such as titles, notes, private evidence, source snippets, human-readable Compartment labels, sensitive reason strings, cached explanations, or derived summaries when those fields are subject to purge or redaction.\n \n Every tombstone is admitted through the `UBU-D0258` mutation-envelope path. The envelope belongs to the tombstone mutation; the durable tombstone stores or references the envelope fields needed for replay and audit rather than existing as an unenveloped side table update. Replaying the same tombstone idempotency key with the same canonical tombstone payload returns the existing result and must not emit duplicate projection deletion attempts.\n \n Decomposition parent retirement becomes a tombstone of the parent Task plus the existing decomposition record's parent snapshot. Undo is an admitted mutation that observes the tombstone and the decomposition record, clears the tombstone only if policy still permits restoration and an authorized restorable snapshot payload is still present, and increments the parent object's version. If the parent payload or snapshot has been physically purged or redacted beyond restoration, undo must not synthesize the old payload from the tombstone; it may instead create a repair or clarification path requiring new user input.\n \n A best-effort physical purge may remove payload blobs, snapshots, derived caches, local projection payload, and external-handle material after or alongside tombstoning, but it does not erase the minimal tombstone while that record is needed for causality, idempotency, audit, or retry. If policy later requires the tombstone itself to be redacted for a Device, the Device receives only an allowed structural representation or an opaque redacted handle; it must not receive forbidden Compartment ids or labels.\n \n External projections are separate `projection_state`. Tombstoning a canonical object may enqueue idempotent projection-deletion obligations keyed by projection surface, external object reference, canonical object id, and tombstone mutation, but success or failure of those queued deletions does not determine whether the canonical object is tombstoned. Projection deletion retries may keep only the minimum external handle allowed by policy, and projection diagnostics must remain structural if payload or Compartment identity is restricted.\n+\n+---\n+\n+## UBU-D0260: Phase 1b redacted Handles disclose only opaque local projection identity\n+\n+**Status:** Accepted \u2192 DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a710, \u00a712. Resolves `UBU-Q0133`.\n+\n+Phase 1b redacted Handles use the `UBU-D0253` rotating-alias rule directly. A `redacted_object_ref` is neither lifetime-stable nor session-global nor regenerated for every retry by default; it is stable only for the receiving Device, the redacted source-object version, and the declared projection window, and rotates when the source version, redaction level, policy epoch, target authority, or projection window changes.\n+\n+`compartment_ref` remains omitted by default. When policy explicitly permits restricted grouping, it is an opaque grouping alias stable only for the receiving Device, declared projection window, and policy epoch. It must be omitted whenever grouping would reveal sensitive context or a Compartment pattern, even if the payload itself is already redacted.\n+\n+A Handle itself may reveal no semantic fact beyond being an opaque local placeholder. The redacted projection around it may expose only the metadata allowed by the effective replication level, such as existence, scheduled time, duration, and generic non-Compartment status. It must withhold canonical object IDs, source object refs, `compartment_id` values, human-readable Compartment labels, reason strings naming the Compartment or subject, semantic prefixes, sequence numbers, stable global identifiers, and any value from which the restricted side can recover or infer Compartment identity.\n+\n+The clearance egress filter enforces the redaction-identity invariant before serialization. It evaluates the real object `compartment_ids` and effective `CompartmentLabel`/policy set inside the trusted boundary, chooses the permitted replication level, then emits only allowed metadata plus opaque aliases whose generation key or lookup table is unavailable to the restricted Device. The old three-tier simplification is not an egress vocabulary: real Compartment labels are enforcement inputs, while restricted outputs carry no Compartment label unless the target authority permits the payload boundary itself.\n+\n+This accepts only the correlation needed for local UI continuity, retry de-duplication, and consistent rendering inside one authorized projection window. Per-operation instability is reserved for policies that forbid even that local continuity; durable cross-Device, cross-window, cross-version, or Compartment-pattern correlation remains unacceptable.\n",
    "commit_message": "Resolve Phase 1b redacted Handle stability",
    "validation_notes": [
      "Confirmed repository HEAD is 7e750c63e5fb3b38bc9cf2f80632642e95481de5.",
      "Ran git apply --check on the proposed patch successfully.",
      "Patch modifies only allowlisted files: OPEN_QUESTIONS.md and DECISIONS.md.",
      "OPEN_QUESTIONS.md preserves the selected question's single-line metadata format and edits the UBU-Q0133 resolution block directly."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0133"
    ],
    "decisions_added": [
      "UBU-D0260"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0133-P-gpt5-codex-20260917",
    "patch_applies": true,
    "allowlist_passed": true,
    "changed_files": [
      "DECISIONS.md",
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
