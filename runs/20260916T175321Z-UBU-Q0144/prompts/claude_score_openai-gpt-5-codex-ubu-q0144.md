# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0144`  
Question title: `Best-effort deletion and redaction on offline Devices`  
Base commit: `8515062888a6d0cadb7c7005e9b86f012b9d22d5`

```markdown
## UBU-Q0144: Best-effort deletion and redaction on offline Devices

Status: Open Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0132, Phase 2 deletion propagation Resolved by: None Last scored: 2026-09-16 Scored from commit: None

Formerly UBU-QSYNC-006 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §12, §18.

### Question

How should UbU represent best-effort deletion/redaction on Devices that were offline when
Compartment policy changed?

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "openai-gpt-5-codex-UBU-Q0144",
    "provider_id": "openai",
    "model_name": "gpt-5-codex",
    "question_id": "UBU-Q0144",
    "base_commit": "8515062888a6d0cadb7c7005e9b86f012b9d22d5",
    "summary": "Resolve UBU-Q0144 by adding a deletion/redaction enforcement record that tracks offline Device status as pending, attempted, confirmed, failed, or accepted unknown exposure without claiming success prematurely.",
    "rationale": "The referenced contract already requires policy updates, best-effort purge/redaction attempts, stale/offline treatment, and non-leaking diagnostics. A small data-model decision can make that behavior auditable by recording the target Device, policy update, affected object references, requested action or redacted representation, attempt state, confirmation evidence, and exposure state. This preserves causality and idempotency while respecting the rule that UbU must not claim enforcement on an offline or unreachable Device until confirmation or explicit user acceptance of unknown exposure.",
    "changed_files": [
      "OPEN_QUESTIONS.md",
      "DECISIONS.md"
    ],
    "patch": "diff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -2566,18 +2566,22 @@ Review-required cases surface as blocking diagnostics with `manual_decision` safe\n ## UBU-Q0144: Best-effort deletion and redaction on offline Devices\n \n-Status: Open Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0132, Phase 2 deletion propagation Resolved by: None Last scored: 2026-09-16 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0132, Phase 2 deletion propagation Resolved by: UBU-D0249 Last scored: 2026-09-16 Scored from commit: None\n \n Formerly UBU-QSYNC-006 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a712, \u00a718.\n \n ### Question\n \n How should UbU represent best-effort deletion/redaction on Devices that were offline when\n Compartment policy changed?\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0249`: represent enforcement against offline Devices as a per-Device deletion/redaction enforcement record linked to the policy update and affected objects.\n+\n+Devices that have not confirmed remain `potentially_exposed`; UbU may record attempts and retries, but may not claim purge or redaction success until confirmation or explicit user acceptance of an unknown exposure state.\n+\n+Diagnostics and user-facing review may disclose only structural status, not restricted payload or Compartment identity.\n \n ---\n \n ## UBU-Q0145: Minimum useful worker Device protocol for Phase 2\ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -3777,3 +3777,32 @@ Human-review-required classes:\n - `projection_conflict` whenever projection repair would choose between canonical user intent and independently changed third-party state.\n \n For auditability, automatic handling must still emit enough local diagnostic and log metadata to explain what was collapsed, recomputed, quarantined, rejected, or admitted. Review-required conflicts are surfaced through the diagnostic/prompt path, not as synchronized Tasks.\n+\n+---\n+\n+## UBU-D0249: Offline deletion and redaction use explicit enforcement records\n+\n+**Status:** Accepted \u2192 DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a712, \u00a718. Resolves `UBU-Q0144`.\n+\n+When a Compartment policy change requires deletion, purge, or redaction on a Device that may be offline, stale, revoked, or unreachable, UbU represents the obligation as a per-target enforcement record rather than as completed deletion.\n+\n+The record contains:\n+\n+- `target_device_id`, or an equivalent opaque Device reference allowed at the current visibility level;\n+- `policy_update_statement_id` and causal references to the tombstone, redaction, or purge request;\n+- `affected_object_ids` or redacted structural object references sufficient for idempotent retry and audit;\n+- `requested_action`: one of `redact_replica`, `purge_replica`, `apply_tombstone`, or `recalculate_derived_state`;\n+- `requested_representation`, when redaction rather than purge is allowed;\n+- `attempt_state`: one of `pending_delivery`, `delivered_unconfirmed`, `attempted_unconfirmed`, `confirmed`, `failed_retryable`, `failed_terminal`, or `user_accepted_unknown`;\n+- `confirmed_at` and confirmation statement reference when the target Device proves enforcement occurred;\n+- `exposure_state`: one of `not_exposed`, `potentially_exposed`, `confirmed_removed`, or `unknown_accepted`.\n+\n+`pending_delivery`, `delivered_unconfirmed`, and `attempted_unconfirmed` all mean UbU must treat the target replica as potentially stale and potentially exposed. They may satisfy audit that UbU attempted enforcement, but they do not satisfy enforcement success.\n+\n+A Device that later reconnects applies the latest admissible policy before exposing affected content, executes the requested redaction or purge idempotently, emits confirmation or failure, and recalculates derived state whose visibility changed.\n+\n+Until confirmation, diagnostics and user-facing review may state only structural status such as \"A protected object may still exist on an offline Device.\" They must not include the protected payload, the Compartment id or label, or reason strings that reveal the Compartment subject.\n+\n+User-facing review may offer an explicit `user_accepted_unknown` outcome only after presenting the exposure as unknown rather than successful. This closes the immediate obligation for planning and audit purposes, but does not rewrite history as confirmed deletion.\n+\n+This resolves Phase 1b representation. Transport-specific retries, Device recovery flows, and full Phase 2 deletion propagation policy remain later implementation work.\n",
    "commit_message": "Resolve offline deletion enforcement representation",
    "validation_notes": [
      "Patch modifies only OPEN_QUESTIONS.md and DECISIONS.md, both allowed files.",
      "OPEN_QUESTIONS.md keeps the selected question metadata on one line and anchors the resolution under UBU-Q0144.",
      "No changes are proposed to DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md, which is read-only context.",
      "Hunk ranges were constructed from the repository line numbers at base context, but git apply was not run because the workspace is read-only."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0144"
    ],
    "decisions_added": [
      "UBU-D0249"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "openai-gpt-5-codex-UBU-Q0144",
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
