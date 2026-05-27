# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `codex`

## Selected question

Question ID: `UBU-Q0021`  
Question title: `Automation Worker Parent / Child Structure`  
Base commit: `d72e4d14e05182aad6f96cec6baa1f15cc6802c6`

```markdown
## UBU-Q0021: Automation Worker Parent / Child Structure

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0008 Blocks: Phase 1 implementation Resolved by: Unresolved Last scored: 2026-05-26 Scored from commit: None

Automation Workers may create canonical child Tasks.

### Question

1. Is the outer Automation/Super Automation Task also the Container?
2. Or is a separate Container created under it?
3. Are child Tasks ordinary Dynamic Tasks?
4. Or are child Tasks a dedicated automation-step subtype?
5. If child Tasks are ordinary Dynamic Tasks, how is automation-specific metadata stored?
6. How are retries connected to the original child Task?

### Resolution

Unresolved.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "codex-ubu-q0021-d0188",
    "provider_id": "codex",
    "model_name": "gpt-5-codex",
    "question_id": "UBU-Q0021",
    "base_commit": "d72e4d14e05182aad6f96cec6baa1f15cc6802c6",
    "summary": "Resolve Automation Worker parent/child structure by using Task-to-Container restructuring, ordinary child Tasks, existing worker/delegation metadata envelopes, and D0187 retry siblings.",
    "rationale": "This aligns the answer with existing Task-to-Container, worker assignment, mutation request, and retry decisions. It avoids a new automation-step subtype, preserves the rule that Plans contain Tasks not Containers, and keeps worker-created children parent-admitted rather than direct worker canonical writes.",
    "changed_files": [
      "DESIGN.md",
      "DECISIONS.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -1329,8 +1329,10 @@ Fields that make a schedulable action executable belong on child Tasks: duration\n Fields that make a schedulable action executable belong on child Tasks: duration or duration PDF, preconditions, effects, executor/delegation fields, worker assignment/status, expected output, evidence requirements, and any child-specific dependencies or deadlines. Child Tasks may inherit or narrow Objective refs, Compartment refs, authority source, and External Reference context when valid, but they do not silently inherit stale status, completion evidence, or modeled effects that no longer apply.\n \n External References are preserved by linking the external object to the new Container when it represents the larger work and to child Tasks only when the external object supports, evidences, or projects that specific child. The original Task's historical External References are not rewritten; new `supersedes`, `projection_of`, `supports`, or `evidence_for` references may be added according to the accepted External Reference model.\n \n-GitHub-linked Task decomposition should keep the GitHub Issue or PR traceable to the Container and add child-level External References only for actionable subwork that needs projection or reconciliation. Automation Worker child Tasks are ordinary child Tasks with worker/delegation metadata; workers may propose this restructuring only through authorized mutation requests, and the canonical instance validates and logs the applied or rejected mutation.\n+GitHub-linked Task decomposition should keep the GitHub Issue or PR traceable to the Container and add child-level External References only for actionable subwork that needs projection or reconciliation.\n+\n+Automation/Super Automation expansion uses the same structural-replacement rule; worker-specific child Task details are in \u00a724.1.2.\n \n ### 9.5 Moot\n \n@@ -2786,8 +2786,20 @@ The minimum Phase 1 assignment statuses are `offered`, `accepted`, `in_progress`\n Workers may reject assignments by returning `rejected_by_worker` with a reason and evidence when relevant. Rejection does not mutate the Task directly. The canonical instance decides whether to reassign, ask the user, mark the Task blocked, revise the Delegation Substrate packet, or leave the Task available for manual work.\n \n If a worker disappears mid-Task, the parent marks the assignment `expired` after the heartbeat or lease deadline, logs the transition, and treats the Task as still unresolved unless separate accepted evidence proves completion or mootness. Late worker submissions after expiration are stale by default and must pass expected-prior-version, idempotency, authority, and review checks before they can affect canonical state. The parent may then reassign the Task, create a retry or repair Task, request clarification, or surface the worker bottleneck in risk reporting. Retry construction follows the worker retry semantics below.\n \n-Workers may request clarification by submitting `clarification_requested` status or an authorized mutation/request payload. The canonical instance may convert that into a clarification Task, user prompt, revised assignment packet, or rejection of the request. Workers may propose child Tasks or Task-to-Container restructuring only through authorized mutation requests; approved restructuring follows the accepted Task-to-Container and child Task semantics. Workers do not directly create canonical child Tasks in Phase 1.\n+Workers may request clarification by submitting `clarification_requested` status or an authorized mutation/request payload. The canonical instance may convert that into a clarification Task, user prompt, revised assignment packet, or rejection of the request.\n+\n+#### Automation child Task structure\n+\n+An Automation/Super Automation Task that expands into multiple canonical children is structurally replaced by a separate Container. The Task itself is not reused as the Container handle. The original Task usually becomes `moot` with reason code `replaced_by_new_plan_structure`.\n+\n+Automation child Tasks are ordinary Tasks, normally Dynamic Tasks. Phase 1 does not define an automation-step subtype. A child may be Static only when it independently has fixed start and end times under ordinary Static Task rules.\n+\n+Workflow-level automation metadata belongs on Container lineage and provenance. Child-specific automation metadata belongs on ordinary Task delegation fields, Delegation Substrate packets when present, worker assignments, capability-grant refs, worker mutation request refs, originating ContextBundle refs, External References, and Logs.\n+\n+Workers may propose child Tasks or the restructuring Container only through authorized mutation requests. The canonical instance validates scope, expected prior version, Compartment/export policy, idempotency, review policy, and External Reference changes before admitting canonical children.\n+\n+Retries use the worker retry semantics below: a failed child stays failed, and retry work is a new sibling Task under the same Container or lineage with retry metadata.\n \n #### Worker retry semantics\n \ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -2820,7 +2820,7 @@ Workers may request clarification through assignment status or an authorized muta\n - Assignment is parent-directed and audit-oriented; worker polling is only a delivery mechanism for explicit assignments.\n - Phase 1 avoids competitive worker claiming while preserving future compatibility with richer delegation, General Contractor, and Skill Barter workflows.\n - Worker disappearance, rejection, clarification, and child-Task proposals are handled through logged assignment transitions and mutation/request review rather than direct canonical writes.\n-- `UBU-Q0021` and `UBU-Q0080` remain open for automation parent/child shape and Delegation Substrate details.\n+- Delegation Substrate details remain open in `UBU-Q0080`.\n \n ---\n \n@@ -3073,7 +3073,7 @@ Stale overwrite prevention is mandatory. The canonical instance compares `expect\n - Worker authority remains request-based and bounded by capability grants, Compartment policy, expected versions, idempotency, assignment leases, and review policy.\n - Valid worker outputs can support automation without granting direct canonical write authority.\n - Invalid and stale worker submissions remain auditable without polluting canonical state.\n-- `UBU-Q0021` and `UBU-Q0084` remain open for automation child structure and external AgentAction side-effect modeling.\n+- `UBU-Q0084` remains open for external AgentAction side-effect modeling.\n \n ---\n \n@@ -3853,8 +3853,38 @@ Worker failure affects derived risk reports. Failed, expired, rejected, repeated\n **Consequences:**\n \n - `UBU-Q0020` is resolved for Phase 1.\n - Attempt history remains auditable because failed attempts are preserved instead of overwritten.\n-- `UBU-Q0021` remains open for automation parent/child structure, but retry lineage no longer depends on that answer.\n+- Retry lineage is independent of whether the retry root began as a direct assignment or as an automation child Task.\n - Implementations can use a conservative retry default without schema-level infinite loops.\n \n ---\n+\n+## UBU-D0188: Automation expansion uses Containers with ordinary child Tasks\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a7\u00a79.4, 24.1.2\n+\n+Resolved question: `UBU-Q0021`.\n+\n+Phase 1 Automation/Super Automation expansion uses Task-to-Container restructuring. The outer automation Task is not also the Container. When an accepted worker or parent workflow decomposes automation into canonical children, the parent creates a separate Container with a new `container_id`, records `origin_task_ref` and mutation provenance, and usually moves the original Task to `moot` with `replaced_by_new_plan_structure`.\n+\n+Child Tasks are ordinary Tasks, normally Dynamic Tasks. Phase 1 does not add an `automation_step` subtype. A child may be Static only when it independently has fixed start and end times under ordinary Static Task rules.\n+\n+Automation-specific metadata stays in existing envelopes:\n+\n+- Container lineage/provenance for workflow-level intent, source Task, mutation request, and grouping;\n+- Task delegation/executor fields or Delegation Substrate packets for child-specific execution intent, expected output, evidence, privacy scope, review, and escalation;\n+- worker assignment records for lease, status, heartbeat, delivery, and review;\n+- capability grants, mutation request refs, ContextBundle refs, External References, and Logs for authority, provenance, evidence, and audit.\n+\n+Workers may propose child Tasks or the restructuring Container only through authorized mutation requests. The canonical parent validates scope, expected prior version, Compartment/export policy, idempotency, review policy, and External Reference changes before admitting canonical children.\n+\n+Retries use `UBU-D0187`: a failed child remains failed, and retry work is a new sibling Task under the same Container or lineage with retry metadata.\n+\n+**Consequences:**\n+\n+- `UBU-Q0021` is resolved for Phase 1 implementation.\n+- Automation workflows do not require a new WorkItem subtype.\n+- Plans continue to contain Tasks only; Containers group automation structure and derive completion from child state.\n+- Worker-created means worker-proposed and parent-admitted, not direct worker canonical write authority.\n+\n+---\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -354,20 +354,4 @@ Resolved. See UBU-D0187.\n ## UBU-Q0021: Automation Worker Parent / Child Structure\n-\n-Status: Open Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0008 Blocks: Phase 1 implementation Resolved by: Unresolved Last scored: 2026-05-26 Scored from commit: None\n-\n-Automation Workers may create canonical child Tasks.\n-\n-### Question\n-\n-1. Is the outer Automation/Super Automation Task also the Container?\n-2. Or is a separate Container created under it?\n-3. Are child Tasks ordinary Dynamic Tasks?\n-4. Or are child Tasks a dedicated automation-step subtype?\n-5. If child Tasks are ordinary Dynamic Tasks, how is automation-specific metadata stored?\n-6. How are retries connected to the original child Task?\n-\n-### Resolution\n-\n-Unresolved.\n-\n+Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0008 Blocks: Phase 1 implementation Resolved by: UBU-D0188 Last scored: 2026-05-26 Scored from commit: None\n+Resolved. See UBU-D0188.\n ---\n",
    "commit_message": "Resolve automation worker child structure",
    "validation_notes": [
      "Validated patch syntax and applicability with git apply --check against the current workspace at base commit d72e4d14e05182aad6f96cec6baa1f15cc6802c6.",
      "OPEN_QUESTIONS.md tombstones UBU-Q0021 with Resolved by: UBU-D0188 using the selected question heading as the edit anchor.",
      "Stale references describing UBU-Q0021 as open were removed from earlier decision consequences."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0021"
    ],
    "decisions_added": [
      "UBU-D0188"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "codex-ubu-q0021-d0188",
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
