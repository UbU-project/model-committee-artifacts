# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `codex`

## Selected question

Question ID: `UBU-Q0004`  
Question title: `Pipeline State`  
Base commit: `0d363ad36a99f2cbff8c1ef183742a8ea378d8ed`

```markdown
## UBU-Q0004: Pipeline State

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0003 Blocks: Phase 1 implementation Resolved by: Unresolved Last scored: 2026-05-26 Scored from commit: None

`Objective.status` is the canonical UbU lifecycle status. `pipeline_state` is workflow/project-management state.

### Question

1. Is `pipeline_state` generic or GitHub-specific?
2. Can one Objective have multiple pipeline states for multiple projections?
3. Is `pipeline_state` stored on Objective directly or in projection metadata?
4. What is the MVP pipeline enum?
5. Can Automation Workers mutate `pipeline_state`?
6. Does every pipeline state transition create an event/log entry?

### Candidate MVP enum

```text
unlabeled
invalid
under_specified
valid_unprioritized
unassigned
in_process_awaiting_pr
awaiting_review
awaiting_ci
complete
```

### Resolution

Unresolved.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "codex-ubu-q0004-pipeline-state-001",
    "provider_id": "codex",
    "model_name": "gpt-5",
    "question_id": "UBU-Q0004",
    "base_commit": "0d363ad36a99f2cbff8c1ef183742a8ea378d8ed",
    "summary": "Resolve UBU-Q0004 by defining pipeline_state as generic projection-scoped workflow metadata, stored outside Objective, with a closed Phase 1 enum, request-only worker mutation, and mandatory transition logging.",
    "rationale": "This keeps Objective.status as the canonical lifecycle field while allowing GitHub and future projections to maintain independent workflow views. The proposal uses existing projection, worker mutation, External Reference, and append-only Log boundaries instead of adding direct worker write authority or embedding projection state on Objective.",
    "changed_files": [
      "DESIGN.md",
      "DECISIONS.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -2033,10 +2033,11 @@ MVP event types are:\n - `task_completed`\n - `task_failed`\n - `task_moot`\n - `external_event_observed`\n - `snapshot_observed`\n - `objective_transitioned`\n+- `pipeline_state_transitioned`\n - `plan_realized`\n - `decision_recorded`\n - `recalculation_triggered`\n - `worker_assignment_updated`\n@@ -2906,17 +2907,50 @@ Contributor interactions may also be associated with a GitHub Identity and relat\n ### 26.2 Pipeline state\n \n-`pipeline_state` is distinct from `Objective.status`.\n-\n-Candidate pipeline states:\n+`pipeline_state` is generic projection-scoped workflow/project-management state. It is not GitHub-specific, although Phase 1 uses it first for GitHub dogfooding and managed-label projection.\n+\n+`pipeline_state` is not stored directly on `Objective` in Phase 1. It is stored as projection metadata keyed by one projection plus one target Objective. One Objective may therefore have multiple current pipeline states for multiple projections, such as GitHub issue labels, a future organization board, or a local release workflow.\n+\n+MVP projection-state record:\n+\n+- `pipeline_state_id`\n+- `pipeline_projection_id`\n+- `target_ref` (`Objective` only in Phase 1)\n+- `pipeline_state`\n+- `authority_source`\n+- `source_refs`\n+- `external_reference_refs`\n+- `updated_at`\n+- `version`\n+- `provenance`\n+\n+A projection has at most one current `pipeline_state` for a given Objective. History is reconstructed from Logs, not by storing multiple historical states on the Objective.\n+\n+Phase 1 `pipeline_state` enum:\n \n - `unlabeled`\n - `invalid`\n - `under_specified`\n - `valid_unprioritized`\n - `unassigned`\n - `in_process_awaiting_pr`\n - `awaiting_review`\n - `awaiting_ci`\n - `complete`\n+\n+Enum semantics:\n+\n+- `unlabeled`: no accepted projection label/state has been assigned.\n+- `invalid`: the projected work item is malformed, duplicate-invalid, impossible, or not admissible as planned work.\n+- `under_specified`: more triage or clarification is required before prioritization.\n+- `valid_unprioritized`: valid work exists but has not been ranked or selected for execution.\n+- `unassigned`: valid prioritized work has no executor or active assignment.\n+- `in_process_awaiting_pr`: work is assigned or in progress and the next expected external projection is a PR or comparable artifact.\n+- `awaiting_review`: implementation or artifact exists and needs human, maintainer, or worker review.\n+- `awaiting_ci`: review or merge path is waiting on CI or comparable automated verification.\n+- `complete`: the projection workflow is complete; this does not by itself prove `Objective.status = completed` or `satisfied`.\n+\n+Automation Workers do not directly mutate `pipeline_state`. A worker may submit a `pipeline_state` projection-state mutation request when its capability grant permits `mutation_request.submit` for the target projection and Objective. The canonical instance validates authority, expected prior version, Compartment/export policy, External References, idempotency, and review policy before applying or rejecting the request.\n+\n+Every accepted `pipeline_state` transition creates a `pipeline_state_transitioned` Log entry with old state, new state, projection id, target Objective ref, actor or authority source, reason, effective time, provenance, source refs, external refs, and any requested projection update. Rejected, stale, or unauthorized worker requests are logged through worker mutation rejection events.\n \n ### 26.3 GitHub projection\ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -3617,3 +3617,54 @@ Question wording must use model-repair framing:\n - Detailed preference-calibration examples remain in `UBU-Q0051`; discovery-mode inference remains in `UBU-Q0052`; deeper affect/personality modeling remains post-MVP in `UBU-Q0074`.\n \n ---\n+\n+## UBU-D0182: Pipeline state is projection-scoped workflow metadata\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a7\u00a717.2, 26.2\n+\n+Resolved question: `UBU-Q0004`.\n+\n+`pipeline_state` is generic projection-scoped workflow/project-management metadata, not GitHub-specific. Phase 1 uses it first for GitHub issue/PR dogfooding and managed labels.\n+\n+`pipeline_state` is not stored directly on `Objective` in Phase 1. It is stored in projection metadata keyed by `pipeline_projection_id` and target Objective. One Objective may have multiple current pipeline states across different projections. A projection has at most one current state per Objective; historical changes are represented by Logs.\n+\n+Minimum record fields:\n+\n+- `pipeline_state_id`\n+- `pipeline_projection_id`\n+- `target_ref` (`Objective` only in Phase 1)\n+- `pipeline_state`\n+- `authority_source`\n+- `source_refs`\n+- `external_reference_refs`\n+- `updated_at`\n+- `version`\n+- `provenance`\n+\n+The Phase 1 enum is:\n+\n+- `unlabeled`\n+- `invalid`\n+- `under_specified`\n+- `valid_unprioritized`\n+- `unassigned`\n+- `in_process_awaiting_pr`\n+- `awaiting_review`\n+- `awaiting_ci`\n+- `complete`\n+\n+`complete` is workflow completion for the projection and does not by itself prove `Objective.status = completed` or `satisfied`.\n+\n+Automation Workers do not directly mutate `pipeline_state`. A worker may submit a `pipeline_state` projection-state mutation request through `mutation_request.submit`; the canonical instance validates authority, expected prior version, Compartment/export policy, External References, idempotency, and review policy before applying or rejecting it.\n+\n+Every accepted `pipeline_state` transition creates a `pipeline_state_transitioned` Log entry with old state, new state, projection id, target Objective ref, actor or authority source, reason, effective time, provenance, source refs, external refs, and any requested projection update. Rejected, stale, or unauthorized worker requests are logged through worker mutation rejection events.\n+\n+**Consequences:**\n+\n+- `UBU-Q0004` is resolved for Phase 1.\n+- Objective lifecycle remains on `Objective.status`; workflow state stays projection-scoped.\n+- GitHub managed labels project accepted `pipeline_state` values but do not become canonical state.\n+- Multiple projections can coexist without changing the Objective schema.\n+- Worker authority remains request-only for pipeline changes.\n+\n+---\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -66,36 +66,6 @@ Resolved. See UBU-D0098.\n ## UBU-Q0004: Pipeline State\n-\n-Status: Open Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0003 Blocks: Phase 1 implementation Resolved by: Unresolved Last scored: 2026-05-26 Scored from commit: None\n-\n-`Objective.status` is the canonical UbU lifecycle status. `pipeline_state` is workflow/project-management state.\n-\n-### Question\n-\n-1. Is `pipeline_state` generic or GitHub-specific?\n-2. Can one Objective have multiple pipeline states for multiple projections?\n-3. Is `pipeline_state` stored on Objective directly or in projection metadata?\n-4. What is the MVP pipeline enum?\n-5. Can Automation Workers mutate `pipeline_state`?\n-6. Does every pipeline state transition create an event/log entry?\n-\n-### Candidate MVP enum\n-\n-```text\n-unlabeled\n-invalid\n-under_specified\n-valid_unprioritized\n-unassigned\n-in_process_awaiting_pr\n-awaiting_review\n-awaiting_ci\n-complete\n-```\n-\n-### Resolution\n-\n-Unresolved.\n-\n----\n-\n+Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0003 Blocks: Phase 1 implementation Resolved by: UBU-D0182 Last scored: 2026-05-26 Scored from commit: None\n+Resolved. See UBU-D0182.\n+---\n+\n ## UBU-Q0005: GitHub Event Triage Rules\n",
    "commit_message": "Resolve pipeline state model",
    "validation_notes": [
      "Patch is scoped to DESIGN.md, DECISIONS.md, and OPEN_QUESTIONS.md.",
      "Patch was checked with git apply --check in the workspace.",
      "UBU-Q0004 dependency UBU-Q0003 is already solved, so no decomposition is needed.",
      "OPEN_QUESTIONS.md converts UBU-Q0004 to a compact solved tombstone anchored at the selected question heading."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0004"
    ],
    "decisions_added": [
      "UBU-D0182"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "codex-ubu-q0004-pipeline-state-001",
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
