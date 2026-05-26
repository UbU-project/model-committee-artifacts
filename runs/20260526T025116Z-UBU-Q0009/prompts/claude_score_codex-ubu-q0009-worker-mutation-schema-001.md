# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `codex`

## Selected question

Question ID: `UBU-Q0009`  
Question title: `Worker Mutation Request Schema`  
Base commit: `1c18876262a0e6d041989a128e36947b2b31f807`

```markdown
## UBU-Q0009: Worker Mutation Request Schema

Status: Open Priority: MVP blocker Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0007 Blocks: Phase 1 implementation Resolved by: Unresolved Last scored: Never Scored from commit: None

Automation Workers need a bounded way to submit changes back to canonical UbU state.

### Question

1. Does a worker submit:
   - event records,
   - mutation requests,
   - proposed patches,
   - or all three?
2. What fields are required?
   - worker Identity
   - authority source
   - target object
   - operation
   - expected prior version
   - new value
   - reason
   - evidence reference
   - timestamp
   - idempotency key
3. Are valid mutations applied immediately?
4. Are invalid mutation attempts logged?
5. Can mutation requests be batched atomically?
6. Can workers request creation of new Objectives or Tasks?
7. Are worker mutations reversible?
8. How does UbU prevent stale worker mutations from overwriting newer canonical state?

### Resolution

Unresolved.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "codex-ubu-q0009-worker-mutation-schema-001",
    "provider_id": "codex",
    "model_name": "gpt-5-codex",
    "question_id": "UBU-Q0009",
    "base_commit": "1c18876262a0e6d041989a128e36947b2b31f807",
    "summary": "Resolve the Phase 1 worker mutation request schema by defining bounded worker submission payloads, required mutation-request fields, closed operation kinds, review and auto-apply rules, atomic batching, create-request behavior, append-only reversal, and stale-overwrite prevention.",
    "rationale": "The existing worker capability and assignment decisions already prohibit direct canonical writes, but Phase 1 still needs an auditable admission schema for worker-submitted changes. This proposal makes worker outputs candidate envelopes validated by the parent instance, preserves capability and Compartment boundaries, logs submitted/applied/rejected outcomes, supports optional atomic batches, and uses expected prior versions plus idempotency keys to prevent stale worker results from overwriting newer state.",
    "changed_files": [
      "DESIGN.md",
      "DECISIONS.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -1916,7 +1916,7 @@\n \n ### 17.5 Automation Worker contributions\n \n-Automation Workers contribute to Logs through their worker Identity. A worker may submit events or mutation requests, but the canonical instance validates authority and writes the canonical Log entry. Accepted worker mutations create applied entries; invalid or unauthorized attempts create rejected entries.\n+Automation Workers contribute to Logs through their worker Identity. A worker may submit events or mutation requests, but the canonical instance validates authority and writes the canonical Log entry. A worker submission first creates a `worker_mutation_submitted` or operation-specific submission record when it enters the canonical instance. Accepted worker mutations create applied entries; invalid, unauthorized, stale, or policy-denied attempts create rejected entries. Requests that require human or policy review remain candidates until an authorized actor accepts or rejects them.\n \n Worker-supplied `idempotency_key`, provenance, evidence references, and confidence metadata are retained when available so duplicate submissions and stale worker results can be detected.\n \n@@ -2598,7 +2598,29 @@\n \n Workers may request clarification by submitting `clarification_requested` status or an authorized mutation/request payload. The canonical instance may convert that into a clarification Task, user prompt, revised assignment packet, or rejection of the request. Workers may propose child Tasks or Task-to-Container restructuring only through authorized mutation requests; approved restructuring follows the accepted Task-to-Container and child Task semantics. Workers do not directly create canonical child Tasks in Phase 1.\n \n-### 24.1.3 Model-committee worker pattern\n+### 24.1.3 Worker mutation request schema\n+\n+Phase 1 worker submissions use three bounded payload families:\n+\n+- event or observation submissions, such as authorized External Event, Snapshot, Log-candidate, status, clarification, or recalculation requests;\n+- mutation requests, which are the normal worker path for changing canonical UbU objects;\n+- proposed patch artifacts, which are reviewable evidence or an explicit patch-style mutation request for supported textual or external artifacts, never direct canonical edits.\n+\n+A worker mutation request is a candidate state-transition envelope. It is not a canonical write until the parent instance validates it, applies it when policy allows, and writes the corresponding Log entries. The minimum request fields are `mutation_request_id`, `schema_version`, `parent_instance_ref`, `worker_identity_ref`, `capability_grant_ref`, `authority_source`, `submitted_at`, `target_ref`, `operation`, `operation_payload` or `new_value`, `reason`, `evidence_refs`, `provenance`, `idempotency_key`, and either `expected_prior_version` or an explicit no-prior-version reason for create-only requests. Optional fields include `assignment_ref`, `effective_at`, `old_value_observed`, `originating_context_bundle_refs`, `compartment_refs`, `compartment_policy_result`, `confidence`, `review_requirement`, `batch_id`, `batch_atomicity`, and `supersedes_request_ref`.\n+\n+Phase 1 mutation operations are closed and typed. They include UniverseState mutation-list requests using the accepted mutation vocabulary; Task lifecycle transition requests, including completion, failure, moot, and estimate or delegation-field updates; Objective transition requests; External Reference create, verify, supersede, or duplicate-link requests; `pipeline_state` projection-state requests; Task or Objective candidate creation requests; Task-to-Container restructuring requests; and Log correction or annotation requests. New operation kinds require a schema migration or accepted decision. Projection writes, GitHub API actions, and external side effects remain separate projection or AgentAction requests and do not become direct canonical mutations.\n+\n+Valid requests are not always applied immediately. A request may be applied immediately only when schema validation, semantic validation, capability scope, Compartment/export policy, idempotency, expected-prior-version checks, assignment lease status, and operation review policy all pass and the request's `review_requirement` allows automatic application. Otherwise a valid request remains a submitted candidate awaiting human or policy review. Invalid, unauthorized, stale, duplicate-conflicting, or policy-denied requests are logged as `worker_mutation_rejected` with a sanitized reason and evidence references when safe.\n+\n+Batches are allowed when the envelope declares a `batch_id` and `batch_atomicity`. `all_or_nothing` batches validate the whole batch before application and either apply every item in declared order or reject the batch without partial canonical mutation. `independent_items` batches may apply valid items and reject invalid items separately, but this is not atomic. MVP implementations may restrict atomic batches to one parent instance and one validation transaction.\n+\n+Workers may request creation of new Objectives, Tasks, External References, child Tasks, or Containers only as mutation requests or candidates. They do not receive direct create authority in Phase 1. Objective creation should require review by default unless a later policy grants a very narrow auto-create path. Task creation may be auto-applied only when the grant, assignment, operation kind, target scope, and review policy explicitly allow it.\n+\n+Worker mutations are reversible only through append-only repair. UbU does not rewrite or delete the original request or applied Log entry. Reversal uses a compensating mutation, Log correction, superseding object, Task moot transition, or projection repair. Requests should include enough observed old value, evidence, and expected-prior-version metadata to make compensation possible when the operation is logically reversible. Irreversible external side effects require projection or AgentAction mitigation metadata rather than ordinary mutation reversal.\n+\n+Stale overwrite prevention is mandatory. The canonical instance compares `expected_prior_version` or equivalent target version metadata with current canonical state before applying mutation requests. A mismatch rejects the request or converts it into a conflict candidate for review; it must not silently overwrite newer state. Idempotency keys prevent duplicate application, assignment leases prevent expired worker work from being accepted accidentally, and capability-grant versions prevent revoked or changed authority from authorizing late submissions.\n+\n+### 24.1.4 Model-committee worker pattern\n \n A model-committee worker is an Automation Worker pattern that reads canonical project state, runs configured provider workflows against a selected open question or problem, produces candidate changesets, scores candidate changesets, and writes reviewable artifacts.\n \ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -3044,3 +3044,36 @@\n - `UBU-Q0004` remains open for the exact `pipeline_state` storage model, but label and projection events can already be triaged as evidence or drift.\n - `UBU-Q0010` remains open for token custody; this decision does not require any specific actor to hold GitHub write credentials.\n - GitHub event handling now aligns with append-only Logs, External References, worker assignment, recalculation triggers, and managed projection reconciliation.\n+\n+\n+---\n+\n+## UBU-D0164: Worker mutation requests are candidate envelopes with versioned admission\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a7\u00a717.5, 24.1.3\n+\n+Resolved question: `UBU-Q0009`.\n+\n+Phase 1 Automation Workers submit bounded candidate payloads, not direct canonical writes. The three supported payload families are event or observation submissions, mutation requests, and proposed patch artifacts. Event or observation submissions cover authorized External Event, Snapshot, Log-candidate, status, clarification, and recalculation requests. Mutation requests are the normal worker path for changing canonical UbU objects. Proposed patches are reviewable evidence or explicit patch-style mutation requests for supported textual or external artifacts; they are never direct edits to canonical state.\n+\n+A worker mutation request is a state-transition envelope. The minimum fields are `mutation_request_id`, `schema_version`, `parent_instance_ref`, `worker_identity_ref`, `capability_grant_ref`, `authority_source`, `submitted_at`, `target_ref`, `operation`, `operation_payload` or `new_value`, `reason`, `evidence_refs`, `provenance`, `idempotency_key`, and either `expected_prior_version` or an explicit no-prior-version reason for create-only requests. Optional fields include `assignment_ref`, `effective_at`, `old_value_observed`, `originating_context_bundle_refs`, `compartment_refs`, `compartment_policy_result`, `confidence`, `review_requirement`, `batch_id`, `batch_atomicity`, and `supersedes_request_ref`.\n+\n+Phase 1 mutation operations are a closed typed set: UniverseState mutation-list requests; Task lifecycle transition requests, including completion, failure, moot, and estimate or delegation-field updates; Objective transition requests; External Reference create, verify, supersede, or duplicate-link requests; `pipeline_state` projection-state requests; Task or Objective candidate creation requests; Task-to-Container restructuring requests; and Log correction or annotation requests. New operation kinds require a schema migration or accepted decision. Projection writes, GitHub API actions, and external side effects remain separate projection or AgentAction requests.\n+\n+A valid request is applied immediately only when schema validation, semantic validation, capability scope, Compartment/export policy, idempotency, expected-prior-version checks, assignment lease status, and operation review policy all pass and the request's `review_requirement` allows automatic application. Otherwise a valid request remains a submitted candidate awaiting human or policy review. Invalid, unauthorized, stale, duplicate-conflicting, or policy-denied requests are logged as `worker_mutation_rejected` with a sanitized reason and evidence references when safe. Every received mutation request creates a `worker_mutation_submitted` Log entry or operation-specific submission record; applied requests create `worker_mutation_applied` entries.\n+\n+Batches are allowed when the request envelope declares a `batch_id` and `batch_atomicity`. `all_or_nothing` batches validate the whole batch before application and either apply every item in declared order or reject the batch without partial canonical mutation. `independent_items` batches may apply valid items and reject invalid items separately, but this is not atomic. MVP implementations may restrict atomic batches to one parent instance and one validation transaction.\n+\n+Workers may request creation of new Objectives, Tasks, External References, child Tasks, or Containers only as mutation requests or candidates. They do not receive direct create authority in Phase 1. Objective creation should require review by default unless a later accepted policy grants a very narrow auto-create path. Task creation may be auto-applied only when the grant, assignment, operation kind, target scope, and review policy explicitly allow it.\n+\n+Worker mutations are reversible only through append-only repair. UbU does not rewrite or delete the original request or applied Log entry. Reversal uses a compensating mutation, Log correction, superseding object, Task moot transition, or projection repair. Requests should include enough observed old value, evidence, and expected-prior-version metadata to make compensation possible when the operation is logically reversible. Irreversible external side effects require projection or AgentAction mitigation metadata rather than ordinary mutation reversal.\n+\n+Stale overwrite prevention is mandatory. The canonical instance compares `expected_prior_version` or equivalent target version metadata with current canonical state before applying mutation requests. A mismatch rejects the request or converts it into a conflict candidate for review; it must not silently overwrite newer state. Idempotency keys prevent duplicate application, assignment leases prevent expired worker work from being accepted accidentally, and capability-grant versions prevent revoked or changed authority from authorizing late submissions.\n+\n+**Consequences:**\n+\n+- `UBU-Q0009` is resolved for Phase 1 implementation.\n+- Worker authority remains request-based and bounded by capability grants, Compartment policy, expected versions, idempotency, assignment leases, and review policy.\n+- Valid worker outputs can support automation without granting direct canonical write authority.\n+- Invalid and stale worker submissions remain auditable without polluting canonical state.\n+- `UBU-Q0013`, `UBU-Q0020`, `UBU-Q0021`, `UBU-Q0049`, and `UBU-Q0084` remain open for authority-source vocabulary, retry construction, automation child structure, release-outreach artifacts, and external AgentAction side-effect modeling.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -199,7 +199,7 @@\n \n ## UBU-Q0009: Worker Mutation Request Schema\n \n-Status: Open Priority: MVP blocker Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0007 Blocks: Phase 1 implementation Resolved by: Unresolved Last scored: Never Scored from commit: None\n+Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0007 Blocks: Phase 1 implementation Resolved by: UBU-D0164 Last scored: Never Scored from commit: None\n \n Automation Workers need a bounded way to submit changes back to canonical UbU state.\n \n@@ -230,7 +230,7 @@\n \n ### Resolution\n \n-Unresolved.\n+Resolved. See UBU-D0164.\n \n ---\n \n",
    "commit_message": "Resolve worker mutation request schema",
    "validation_notes": [
      "Patch is limited to DESIGN.md, DECISIONS.md, and OPEN_QUESTIONS.md.",
      "OPEN_QUESTIONS.md keeps the selected question metadata on one line and updates only the UBU-Q0009 block.",
      "Ran git apply --check against the current workspace at base commit 1c18876262a0e6d041989a128e36947b2b31f807; it passed."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0009"
    ],
    "decisions_added": [
      "UBU-D0164"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "codex-ubu-q0009-worker-mutation-schema-001",
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
