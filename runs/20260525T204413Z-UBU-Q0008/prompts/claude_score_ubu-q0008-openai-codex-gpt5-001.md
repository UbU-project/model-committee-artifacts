# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai-codex`

## Selected question

Question ID: `UBU-Q0008`  
Question title: `Worker Assignment Model`  
Base commit: `620d9d0fb9994916928e61b2dcd8a58b2918406e`

```markdown
## UBU-Q0008: Worker Assignment Model

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0007 Blocks: Phase 1 implementation Resolved by: Unresolved Last scored: Never Scored from commit: None

Workers need a way to discover or receive work.

### Question

1. Does an organization-mode instance explicitly assign Tasks to workers?
2. Or do workers poll for Tasks matching their capabilities?
3. Can multiple workers observe the same Task?
4. Can multiple workers compete for the same Task?
5. What happens if a worker disappears mid-Task?
6. Is assignment itself logged?
7. Can workers reject assignments?
8. Can workers request clarification Tasks?
9. Can workers spawn child Tasks?

### Current MVP leaning

Explicit assignment is likely simplest.

### Resolution

Unresolved.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "ubu-q0008-openai-codex-gpt5-001",
    "provider_id": "openai-codex",
    "model_name": "gpt-5",
    "question_id": "UBU-Q0008",
    "base_commit": "620d9d0fb9994916928e61b2dcd8a58b2918406e",
    "summary": "Resolve the Worker Assignment Model by making Phase 1 worker discovery parent-directed explicit assignment with lease-based lifecycle state, logged transitions, rejection, clarification, expiration, and mutation-request-only child Task proposals.",
    "rationale": "Explicit assignment matches the current MVP leaning and the accepted worker capability model. It avoids premature marketplace or competitive claiming semantics, keeps worker authority bounded by capability grants and Compartment policy, makes assignment auditable through Logs, and leaves retry construction, mutation request schema, and Delegation Substrate details to their dedicated open questions.",
    "changed_files": [
      "DESIGN.md",
      "DECISIONS.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -1861,6 +1861,7 @@\n - `plan_realized`\n - `decision_recorded`\n - `recalculation_triggered`\n+- `worker_assignment_updated`\n - `worker_mutation_submitted`\n - `worker_mutation_applied`\n - `worker_mutation_rejected`\n@@ -2520,6 +2521,26 @@\n \n A single worker-mode instance may serve multiple parent organization-mode or user-mode instances only through separate parent-specific capability grants, credentials, and audit trails. Cross-parent data sharing is forbidden unless each parent explicitly grants a route and all relevant Compartment policies allow it. Workers may operate for user-mode instances, but affect and other personal data require especially narrow read-subset grants and must respect `no_cloud_llm`, `no_external_export`, and low-security disclosure rules.\n \n+### 24.1.2 Worker assignment lifecycle\n+\n+Phase 1 worker work discovery is explicit assignment by the parent UbU instance. A worker may poll a parent-specific assignment inbox or receive pushed notifications, but the inbox returns only work that the parent has already assigned or offered to that worker Identity. Worker check-ins may advertise health, availability, local resource state, and granted capability metadata so the parent can choose an eligible worker; they are not an open task-claim or marketplace mechanism.\n+\n+A worker assignment is a scoped execution lease that binds one Task or Delegation Substrate packet to one worker Identity for one active executor slot. The minimum assignment record includes `assignment_id`, parent instance ref, `task_ref`, worker Identity ref, capability grant ref, assigned-by Identity or authority source, assignment status, lease or heartbeat deadline, expected output summary, review requirement, idempotency key, and provenance. The assignment may reference Compartment decisions or ContextBundles when the work handoff exposes protected or low-security content.\n+\n+The minimum Phase 1 assignment statuses are `offered`, `accepted`, `in_progress`, `clarification_requested`, `delivered`, `completed`, `rejected_by_worker`, `cancelled`, `expired`, and `failed`. `delivered` means the worker has submitted output or evidence for parent review; `completed` means the canonical instance accepted the assignment outcome or otherwise determined that no further worker action is required. Task status remains canonical on the Task itself and is not replaced by assignment status.\n+\n+Multiple workers may observe the same Task only through explicit read grants, observer roles, or separate review assignments. Observation does not confer execution authority. In Phase 1, only one worker may hold the active execution lease for a given executor slot on a Task. If apparently parallel work is needed, UbU should model separate child Tasks or separate named assignment roles rather than allowing anonymous workers to compete for the same Task. Competitive worker claiming, work stealing, and marketplace-style bidding are deferred.\n+\n+Every assignment lifecycle transition is logged. Phase 1 extends the Log event-type set with `worker_assignment_updated`; its payload records the assignment id, old and new status when applicable, worker Identity, Task ref, capability grant ref, lease deadline, reason, evidence refs, idempotency key, and whether recalculation was requested. Assignment events that affect the current or next recommended Task, worker bottleneck risk, or Plan feasibility create or batch a `recalculation_triggered` Log entry using the existing trigger taxonomy.\n+\n+Workers may reject assignments by returning `rejected_by_worker` with a reason and evidence when relevant. Rejection does not mutate the Task directly. The canonical instance decides whether to reassign, ask the user, mark the Task blocked, revise the Delegation Substrate packet, or leave the Task available for manual work.\n+\n+If a worker disappears mid-Task, the parent marks the assignment `expired` after the heartbeat or lease deadline, logs the transition, and treats the Task as still unresolved unless separate accepted evidence proves completion or mootness. Late worker submissions after expiration are stale by default and must pass expected-prior-version, idempotency, authority, and review checks before they can affect canonical state. The parent may then reassign the Task, create a retry or repair Task, request clarification, or surface the worker bottleneck in risk reporting. Detailed retry construction remains governed by `UBU-Q0020`.\n+\n+Workers may request clarification by submitting `clarification_requested` status or an authorized mutation/request payload. The canonical instance may convert that into a clarification Task, user prompt, revised assignment packet, or rejection of the request. Workers may propose child Tasks or Task-to-Container restructuring only through authorized mutation requests; approved restructuring follows the accepted Task-to-Container and child Task semantics. Workers do not directly create canonical child Tasks in Phase 1.\n+\n+### 24.1.3 Model-committee worker pattern\n+\n A model-committee worker is an Automation Worker pattern that reads canonical project state, runs configured provider workflows against a selected open question or problem, produces candidate changesets, scores candidate changesets, and writes reviewable artifacts.\n \n In v0.1, this means Codex CLI plus local Ollama proposal workflows. In v0.2, this expands to Codex and Claude Code frontier-provider cross-scoring, with local providers remaining useful for diversity and fallback.\ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -2790,3 +2790,36 @@\n - LLM routing can proceed without privileging Ollama, UbUCorp, or any single cloud API as the canonical interface.\n - `UBU-Q0079`, `UBU-Q0080`, `UBU-Q0083`, and `UBU-Q0084` may depend on this routing boundary while still refining MCP tools, Delegation Substrate fields, ContextBundle governance, and background-agent policy.\n - Provider-specific SDK details, exact secret-store implementation, and long-context bundle review UX may evolve without changing the minimum boundary.\n+\n+\n+---\n+\n+## UBU-D0158: Phase 1 worker work discovery uses explicit assignments\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a724.1.2\n+\n+Resolved question: `UBU-Q0008`.\n+\n+Phase 1 worker work discovery uses explicit assignment by the parent UbU instance. Workers may poll a parent-specific assignment inbox or receive pushed notifications, but the inbox contains only work already offered or assigned to that worker Identity. Worker check-ins may advertise health, availability, local resource state, and capability metadata so the parent can select an eligible worker; they are not an open task-claim, work-stealing, bidding, or marketplace mechanism.\n+\n+A worker assignment is a scoped execution lease binding one Task or Delegation Substrate packet to one worker Identity for one active executor slot. The minimum assignment record includes `assignment_id`, parent instance ref, `task_ref`, worker Identity ref, capability grant ref, assigned-by Identity or authority source, assignment status, lease or heartbeat deadline, expected output summary, review requirement, idempotency key, and provenance. Assignment handoffs that expose protected or low-security content must also reference the relevant Compartment decision or ContextBundle.\n+\n+The minimum Phase 1 assignment statuses are `offered`, `accepted`, `in_progress`, `clarification_requested`, `delivered`, `completed`, `rejected_by_worker`, `cancelled`, `expired`, and `failed`. Assignment status does not replace Task status. A delivered assignment is awaiting parent review; a completed assignment means the canonical instance accepted the outcome or determined that no more worker action is required.\n+\n+Multiple workers may observe the same Task only through explicit read grants, observer roles, or separate review assignments. Observation does not confer execution authority. Only one worker may hold the active execution lease for a given executor slot on a Task in Phase 1. If parallel work is needed, UbU should model separate child Tasks or separate named assignment roles rather than allowing multiple workers to compete for the same Task. Competitive worker claiming and marketplace-style bidding are deferred.\n+\n+Every assignment lifecycle transition is logged. This decision extends the Phase 1 Log event-type set with `worker_assignment_updated`. Its payload records the assignment id, old and new status when applicable, worker Identity, Task ref, capability grant ref, lease deadline, reason, evidence refs, idempotency key, and whether recalculation was requested. Assignment events that affect the current or next recommended Task, worker bottleneck risk, or Plan feasibility create or batch a `recalculation_triggered` Log entry using the existing trigger taxonomy.\n+\n+Workers may reject assignments by returning `rejected_by_worker` with a reason and evidence when relevant. Rejection does not mutate the Task directly. The canonical instance decides whether to reassign, ask the user, mark the Task blocked, revise the Delegation Substrate packet, or leave the Task available for manual work.\n+\n+If a worker disappears mid-Task, the parent marks the assignment `expired` after the heartbeat or lease deadline, logs the transition, and treats the Task as unresolved unless separate accepted evidence proves completion or mootness. Late worker submissions after expiration are stale by default and must pass expected-prior-version, idempotency, authority, and review checks before they can affect canonical state. The parent may then reassign the Task, create a retry or repair Task, request clarification, or surface the worker bottleneck in risk reporting. Detailed retry construction remains open in `UBU-Q0020`.\n+\n+Workers may request clarification through assignment status or an authorized mutation/request payload. The canonical instance may convert the request into a clarification Task, user prompt, revised assignment packet, or rejection of the request. Workers may propose child Tasks or Task-to-Container restructuring only through authorized mutation requests; approved restructuring follows the accepted Task-to-Container and child Task semantics. Workers do not directly create canonical child Tasks in Phase 1.\n+\n+**Consequences:**\n+\n+- `UBU-Q0008` is resolved for Phase 1 implementation.\n+- Assignment is parent-directed and audit-oriented; worker polling is only a delivery mechanism for explicit assignments.\n+- Phase 1 avoids competitive worker claiming while preserving future compatibility with richer delegation, General Contractor, and Skill Barter workflows.\n+- Worker disappearance, rejection, clarification, and child-Task proposals are handled through logged assignment transitions and mutation/request review rather than direct canonical writes.\n+- `UBU-Q0020`, `UBU-Q0021`, `UBU-Q0009`, and `UBU-Q0080` remain open for retry construction, automation parent/child shape, mutation request schema, and Delegation Substrate details.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -171,7 +171,7 @@\n \n ## UBU-Q0008: Worker Assignment Model\n \n-Status: Open Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0007 Blocks: Phase 1 implementation Resolved by: Unresolved Last scored: Never Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0007 Blocks: Phase 1 implementation Resolved by: UBU-D0158 Last scored: Never Scored from commit: None\n \n Workers need a way to discover or receive work.\n \n@@ -193,7 +193,7 @@\n \n ### Resolution\n \n-Unresolved.\n+Resolved. See UBU-D0158.\n \n ---\n \n",
    "commit_message": "Resolve worker assignment model for Phase 1",
    "validation_notes": [
      "Patch generated against base commit 620d9d0fb9994916928e61b2dcd8a58b2918406e in /home/sean/ubu-design.",
      "Validated with git apply --check using the generated patch on stdin.",
      "OPEN_QUESTIONS.md preserves the selected question metadata as a single line and anchors the resolution inside UBU-Q0008.",
      "The proposal adds one decision, UBU-D0158, and resolves only UBU-Q0008; dependent retry, parent/child, mutation request, and Delegation Substrate details remain open."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0008"
    ],
    "decisions_added": [
      "UBU-D0158"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "ubu-q0008-openai-codex-gpt5-001",
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
