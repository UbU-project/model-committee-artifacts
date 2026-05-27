# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai-codex`

## Selected question

Question ID: `UBU-Q0020`  
Question title: `Automation Worker Retry Semantics`  
Base commit: `b0d71c9e540db17ee7e78c22ff07ba71b0e39ecd`

```markdown
## UBU-Q0020: Automation Worker Retry Semantics

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0008 Blocks: Phase 1 implementation Resolved by: Unresolved Last scored: 2026-05-26 Scored from commit: None

Automation Workers may fail or produce failed child Tasks.

### Question

1. If a worker-created child Task fails, does UbU:
   - mutate the same Task,
   - create retry sibling,
   - mark failed then create replacement,
   - mark moot?
2. Is retry policy defined by:
   - Task
   - Objective
   - Worker
   - integration type
3. How are retry attempts logged?
4. How are repeated failures prevented from creating infinite loops?
5. Does a worker failure affect risk reports?

### Current leaning

Failed attempt + retry sibling is likely best for auditability.

### Resolution

Unresolved.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "codex-ubu-q0020-retry-semantics",
    "provider_id": "openai-codex",
    "model_name": "gpt-5-codex",
    "question_id": "UBU-Q0020",
    "base_commit": "b0d71c9e540db17ee7e78c22ff07ba71b0e39ecd",
    "summary": "Resolve Automation Worker retry semantics by preserving failed attempts, creating retry sibling Tasks, defining retry-policy precedence, logging retry chains through existing event surfaces, capping automatic retries, and feeding worker retry state into derived risk reports.",
    "rationale": "Failed attempt plus retry sibling preserves auditability and avoids rewriting Task history. A clear policy hierarchy keeps retry behavior local when specified while allowing conservative defaults. Attempt caps, idempotency, expected-prior-version checks, nonretryable failure classes, and review after exhaustion prevent infinite retry loops. Risk reporting remains derived and recalculable, consistent with existing MVP boundaries.",
    "changed_files": [
      "DESIGN.md",
      "DECISIONS.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -2784,11 +2784,27 @@\n Every assignment lifecycle transition is logged. Phase 1 extends the Log event-type set with `worker_assignment_updated`; its payload records the assignment id, old and new status when applicable, worker Identity, Task ref, capability grant ref, lease deadline, reason, evidence refs, idempotency key, and whether recalculation was requested. Assignment events that affect the current or next recommended Task, worker bottleneck risk, or Plan feasibility create or batch a `recalculation_triggered` Log entry using the existing trigger taxonomy.\n \n Workers may reject assignments by returning `rejected_by_worker` with a reason and evidence when relevant. Rejection does not mutate the Task directly. The canonical instance decides whether to reassign, ask the user, mark the Task blocked, revise the Delegation Substrate packet, or leave the Task available for manual work.\n \n-If a worker disappears mid-Task, the parent marks the assignment `expired` after the heartbeat or lease deadline, logs the transition, and treats the Task as still unresolved unless separate accepted evidence proves completion or mootness. Late worker submissions after expiration are stale by default and must pass expected-prior-version, idempotency, authority, and review checks before they can affect canonical state. The parent may then reassign the Task, create a retry or repair Task, request clarification, or surface the worker bottleneck in risk reporting. Detailed retry construction remains governed by `UBU-Q0020`.\n+If a worker disappears mid-Task, the parent marks the assignment `expired` after the heartbeat or lease deadline, logs the transition, and treats the Task as still unresolved unless separate accepted evidence proves completion or mootness. Late worker submissions after expiration are stale by default and must pass expected-prior-version, idempotency, authority, and review checks before they can affect canonical state. The parent may then reassign the Task, create a retry or repair Task, request clarification, or surface the worker bottleneck in risk reporting. Retry construction follows the worker retry semantics below.\n \n Workers may request clarification by submitting `clarification_requested` status or an authorized mutation/request payload. The canonical instance may convert that into a clarification Task, user prompt, revised assignment packet, or rejection of the request. Workers may propose child Tasks or Task-to-Container restructuring only through authorized mutation requests; approved restructuring follows the accepted Task-to-Container and child Task semantics. Workers do not directly create canonical child Tasks in Phase 1.\n \n+#### Worker retry semantics\n+\n+A failed worker-created child Task is not mutated back to `active`. The failed attempt remains a failed Task with its own assignment, evidence, and Logs. When retry is warranted, UbU creates a new retry sibling Task with a new `task_id` under the same parent Container or lineage when one exists, linked by `retry_of_task_ref`, `retry_root_task_ref`, `attempt_number`, `retry_policy_summary`, and failure or evidence refs.\n+\n+Worker assignment failure on an existing Task may leave the underlying Task unresolved when no attempt Task was created. In that case retry or repair work is represented by a new assignment or retry/repair Task; accepted completion evidence or mootness is required before the underlying work stops being planned.\n+\n+The effective retry policy resolves from Task or Delegation Substrate packet, then parent Container or Objective, then worker or integration default, then global Phase 1 default. Worker and integration defaults cannot exceed capability grants, Compartment/export policy, assignment leases, idempotency, expected-prior-version, or review policy.\n+\n+The Phase 1 global default is at most three total attempts per retry root, including the initial attempt. No automatic retry is created for policy denials, authorization failures, Compartment/export denials, invalid mutation requests, missing required human review, or stale expected-prior-version conflicts.\n+\n+Each retry attempt has its own assignment, idempotency key, status transitions, and Task outcome Logs. Failure is logged with `task_failed`, assignment changes with `worker_assignment_updated`, worker request failures with `worker_mutation_rejected` when applicable, and retry-sibling admission through the normal Task creation or mutation-request admission path. Retry metadata must keep the attempt chain queryable without rewriting prior attempts.\n+\n+When attempts are exhausted, UbU stops automatic retry creation, leaves the latest attempt failed unless another accepted lifecycle transition applies, and routes the parent work to human review, clarification, reassignment, repair, or a moot/supersession decision through ordinary admission rules.\n+\n+Worker failure affects derived reports and recalculation. Failed, expired, rejected, repeated, or exhausted worker attempts feed `worker_or_automation_bottleneck`, may increase dependency fragility or deadline risk, and create or batch recalculation triggers when they can affect the current or next recommended Task, Plan feasibility, or risk-report validity.\n+\n ### 24.1.3 Worker mutation request schema\n \n Phase 1 worker submissions use three bounded payload families:\n@@ -3133,7 +3149,9 @@\n Risk reports are computed on demand from the current Calendar or Plan. Implementations may cache a risk-report artifact with the Calendar, Plan, run artifact, or release package for auditability and UI performance, but the cache is derived state and must be invalidated or marked stale when relevant Logs, Snapshots, Tasks, worker status, External Events, compact Calendar coverage, or recalculation triggers change.\n \n Automation Workers may compute or refresh risk-report artifacts when granted appropriate read capability, but worker output is advisory until admitted by the canonical instance. Workers may submit report artifacts, report-refresh requests, mutation requests, or projection requests according to their grants; they do not create canonical Risk objects or directly mutate Tasks.\n \n A risk report may recommend or request Tasks. It must not silently create canonical Tasks in MVP. Human-approved or policy-approved follow-up Tasks may include clarification, affect collection, dependency repair, worker retry/escalation, Calendar regeneration, Log review, Calendar preview, or GitHub projection/reconciliation work.\n+\n+Worker retry state is an input to `worker_or_automation_bottleneck`: active retry chains, repeated failure classes, retry exhaustion, expired leases, and stale late submissions should be surfaced when they threaten current work or release readiness.\n \n Risk reports are part of release ceremonies for UbU-runs-UbU. A release readiness or release outreach package should include, at minimum, deadline risk, critical path, dependency fragility, worker/automation bottleneck, and low coverage warnings when the inputs exist. Public release artifacts should cite only reviewable report summaries and must not expose sensitive Compartment payloads.\ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -2812,5 +2812,5 @@\n Workers may reject assignments by returning `rejected_by_worker` with a reason and evidence when relevant. Rejection does not mutate the Task directly. The canonical instance decides whether to reassign, ask the user, mark the Task blocked, revise the Delegation Substrate packet, or leave the Task available for manual work.\n \n-If a worker disappears mid-Task, the parent marks the assignment `expired` after the heartbeat or lease deadline, logs the transition, and treats the Task as unresolved unless separate accepted evidence proves completion or mootness. Late worker submissions after expiration are stale by default and must pass expected-prior-version, idempotency, authority, and review checks before they can affect canonical state. The parent may then reassign the Task, create a retry or repair Task, request clarification, or surface the worker bottleneck in risk reporting. Detailed retry construction remains open in `UBU-Q0020`.\n+If a worker disappears mid-Task, the parent marks the assignment `expired` after the heartbeat or lease deadline, logs the transition, and treats the Task as unresolved unless separate accepted evidence proves completion or mootness. Late worker submissions after expiration are stale by default and must pass expected-prior-version, idempotency, authority, and review checks before they can affect canonical state. The parent may then reassign the Task, create a retry or repair Task, request clarification, or surface the worker bottleneck in risk reporting. Retry construction is defined by `UBU-D0187`.\n \n Workers may request clarification through assignment status or an authorized mutation/request payload. The canonical instance may convert the request into a clarification Task, user prompt, revised assignment packet, or rejection of the request. Workers may propose child Tasks or Task-to-Container restructuring only through authorized mutation requests; approved restructuring follows the accepted Task-to-Container and child Task semantics. Workers do not directly create canonical child Tasks in Phase 1.\n@@ -2823,3 +2823,3 @@\n - Worker disappearance, rejection, clarification, and child-Task proposals are handled through logged assignment transitions and mutation/request review rather than direct canonical writes.\n-- `UBU-Q0020`, `UBU-Q0021`, `UBU-Q0009`, and `UBU-Q0080` remain open for retry construction, automation parent/child shape, mutation request schema, and Delegation Substrate details.\n+- `UBU-Q0021` and `UBU-Q0080` remain open for automation parent/child shape and Delegation Substrate details.\n \n@@ -3076,3 +3076,3 @@\n - Invalid and stale worker submissions remain auditable without polluting canonical state.\n-- `UBU-Q0020`, `UBU-Q0021`, and `UBU-Q0084` remain open for retry construction, automation child structure, and external AgentAction side-effect modeling.\n+- `UBU-Q0021` and `UBU-Q0084` remain open for automation child structure and external AgentAction side-effect modeling.\n \n@@ -3811,8 +3811,50 @@\n **Consequences:**\n \n - `UBU-Q0017` is resolved for Phase 1.\n - Coverage is auditable enough for regeneration without pretending mathematical completeness.\n - The same default `0.99` value remains the short-horizon branch target and regeneration threshold unless overridden by recorded policy.\n - Objective recurrence uncertainty can be added later as a typed stochastic input without changing the MVP coverage boundary.\n \n ---\n+\n+## UBU-D0187: Worker retries use failed attempts and retry siblings\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a7\u00a724.1.2, 27\n+\n+Resolved question: `UBU-Q0020`.\n+\n+Phase 1 does not mutate failed worker-created child Tasks back to `active`. A failed attempt remains a failed Task with its own assignment, evidence, and Logs. When retry is warranted, the parent creates a new retry sibling Task with a new `task_id` under the same parent Container or lineage when one exists. The retry sibling carries `retry_of_task_ref`, `retry_root_task_ref`, `attempt_number`, `retry_policy_summary`, `source_failure_log_refs`, and evidence refs.\n+\n+Worker assignment failure on an existing Task may leave the underlying Task unresolved when no attempt Task was created. In that case retry or repair work is represented by a new assignment or retry/repair Task; accepted completion evidence or mootness is required before the underlying work stops being planned.\n+\n+The effective retry policy resolves in this order:\n+\n+- Task or Delegation Substrate packet;\n+- parent Container or Objective;\n+- worker or integration default;\n+- global Phase 1 default.\n+\n+Worker and integration defaults cannot exceed capability grants, Compartment/export policy, assignment leases, idempotency, expected-prior-version, or review policy. Policy denials, authorization failures, Compartment/export denials, invalid mutation requests, required human review, and stale expected-prior-version conflicts are not automatically retryable.\n+\n+The global Phase 1 default is at most three total attempts per retry root, including the initial attempt. A policy may set a lower cap or require human review after any failure. Each retry attempt receives its own assignment and idempotency key. Retrying repeats precondition, capability, Compartment, expected-version, and review checks against current state.\n+\n+Logging uses existing event surfaces:\n+\n+- failed attempt Task: `task_failed`;\n+- assignment status change: `worker_assignment_updated`;\n+- worker request admission or denial: `worker_mutation_submitted`, `worker_mutation_applied`, or `worker_mutation_rejected`;\n+- retry decision or retry sibling admission: normal Task creation or mutation-request admission path with retry metadata;\n+- planner impact: `recalculation_triggered` when the failure affects current or next Task, feasibility, worker bottleneck risk, or report validity.\n+\n+Infinite retry loops are blocked by retry-root attempt caps, idempotency keys, expected-prior-version checks, assignment lease checks, failure-class nonretryability, and human review after cap exhaustion. Exhaustion stops automatic retry creation and routes parent work to human review, clarification, reassignment, repair, or an ordinary Task lifecycle decision such as leaving the latest attempt failed or marking work moot with an accepted reason code.\n+\n+Worker failure affects derived risk reports. Failed, expired, rejected, repeated, or exhausted worker attempts feed `worker_or_automation_bottleneck` and may affect dependency fragility, deadline risk, release readiness, and recalculation urgency. They do not create canonical Risk objects or silently create follow-up Tasks.\n+\n+**Consequences:**\n+\n+- `UBU-Q0020` is resolved for Phase 1.\n+- Attempt history remains auditable because failed attempts are preserved instead of overwritten.\n+- `UBU-Q0021` remains open for automation parent/child structure, but retry lineage no longer depends on that answer.\n+- Implementations can use a conservative retry default without schema-level infinite loops.\n+\n+---\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -346,31 +346,4 @@\n ## UBU-Q0020: Automation Worker Retry Semantics\n-\n-Status: Open Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0008 Blocks: Phase 1 implementation Resolved by: Unresolved Last scored: 2026-05-26 Scored from commit: None\n-\n-Automation Workers may fail or produce failed child Tasks.\n-\n-### Question\n-\n-1. If a worker-created child Task fails, does UbU:\n-   - mutate the same Task,\n-   - create retry sibling,\n-   - mark failed then create replacement,\n-   - mark moot?\n-2. Is retry policy defined by:\n-   - Task\n-   - Objective\n-   - Worker\n-   - integration type\n-3. How are retry attempts logged?\n-4. How are repeated failures prevented from creating infinite loops?\n-5. Does a worker failure affect risk reports?\n-\n-### Current leaning\n-\n-Failed attempt + retry sibling is likely best for auditability.\n-\n-### Resolution\n-\n-Unresolved.\n-\n+Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0008 Blocks: Phase 1 implementation Resolved by: UBU-D0187 Last scored: 2026-05-26 Scored from commit: None\n+Resolved. See UBU-D0187.\n ---\n",
    "commit_message": "Resolve automation worker retry semantics",
    "validation_notes": [
      "Patch validated with git apply --check against the current workspace at base commit b0d71c9e540db17ee7e78c22ff07ba71b0e39ecd.",
      "UBU-Q0020 is tombstoned with Status: Solved and Resolved by: UBU-D0187.",
      "No new questions are added; UBU-Q0021 remains open for automation parent/child structure."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0020"
    ],
    "decisions_added": [
      "UBU-D0187"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "codex-ubu-q0020-retry-semantics",
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
