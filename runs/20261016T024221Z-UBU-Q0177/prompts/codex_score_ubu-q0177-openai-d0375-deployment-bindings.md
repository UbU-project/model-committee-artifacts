# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0177`  
Question title: `Minimum viable secure local hardware for multimodal interaction`  
Base commit: `70b2ebab4139bfdf3b4c030e6436e6e4530b5f8f`

```markdown
## UBU-Q0177: Minimum viable secure local hardware for multimodal interaction

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-15 Scored from commit: None

### Question

What measured local hardware profile can deliver the MVP multimodal experience securely and usefully?

### Current direction

Delta §17.15. Evaluate model quality, memory, latency, energy, sustained thermal behavior, endpoint security and local storage on supported devices. Distinguish primary mobile interaction from mobile planning workload (`UBU-Q0160`). No modern-phone universality or cloud-versus-local quality ranking is assumed.

`UBU-D0331` specifies revision-pinned Device-role profiles, predeclared usefulness/resource criteria, real-device cold/warm and sustained trials, endpoint/storage gates and a reviewed support matrix in DESIGN.md §1. This narrows the qualification process without selecting hardware, a mobile planner or a compute API. `UBU-D0336` adds isolation fixtures across inference jobs and measures the enforced isolation/reset costs within that profile.

`UBU-D0362` makes hardware-selection completeness explicit: the approved profile set must cover all mandatory capture/review slices in DESIGN.md §4.2 (`UBU-D0339`), with qualified endpoints, any worker routes and demonstrated shared-hardware workload limits.

`UBU-D0368` extends qualification to every enabled processing stage, including delegated OS services, with connected/offline, backend-loss and reconnection evidence for actual provider calls, egress restrictions and queued retries. The support matrix binds the complete pipeline and its resource/retention costs.

### Resolution

Open. `UBU-D0331` answers the qualification protocol only. Close after actual real-device evidence demonstrates the declared MVP slices, numeric latency/resource profiles, endpoint and retention controls, sustained behavior and useful manual degradation, and the operator approves a scoped hardware/software support matrix. No measured minimum or supported Device is established by the protocol. `UBU-D0336` adds runtime-isolation gates; those fixtures also require passing deployment evidence before selection. `UBU-D0362` requires that selection to cover every mandatory §4.2 slice, with each endpoint and any inter-Device route qualified; partial coverage leaves this question Open.

`UBU-D0368` adds complete-route locality evidence; its required real-device trials remain outstanding and establish no supported configuration.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0177-openai-D0375-deployment-bindings",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0177",
    "base_commit": "70b2ebab4139bfdf3b4c030e6436e6e4530b5f8f",
    "summary": "Enforce approved multimodal deployment bindings before capture, dispatch and result release.",
    "rationale": "Existing qualification rules require rechecks after configuration changes but leave enforcement during use implicit. Add concrete checks and change fixtures so altered models, delegated services, permissions or worker profiles cannot inherit prior qualification. Keep UBU-Q0177 Open for real-device measurements and operator-approved coverage of every mandatory slice.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5810,3 +5810,15 @@ Measure live-guidance coverage as a union of eligible presentation intervals, cl\n Require DESIGN.md \u00a721.1.4's actual-controller reordered-window, duplicate/retry, failed-newer-window, overlap, multi-source and release-race fixtures plus pinned deployment evidence. These are enforcement and accounting requirements, not passing results, numerical values or a new wire schema. `UBU-Q0168` stays Open pending the existing frozen real-device acceptance trials and operator review.\n \n ---\n+\n+## UBU-D0375: Qualified multimodal modes remain bound to the tested deployment\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a71. Narrows `UBU-Q0177`; measured hardware selection remains Open.\n+\n+Enforce `UBU-D0331`/`UBU-D0336`/`UBU-D0368`'s required deployment pins before source acquisition and inference dispatch across the complete route. Missing or mismatched bindings withhold dependent modes. Pin artifacts and enforcement settings through each job or detect changes before further source access/release; recheck qualification bindings at result release and invalidate affected active work under the existing interruption and retention rules. A changed shared component affects every dependent route.\n+\n+Changed bindings require affected rechecks and renewed operator approval. Preserve permitted saved state, immediate controls and useful manual completion; restored support cannot resume capture or replay work automatically. Require DESIGN.md \u00a71's matching, changed-component, worker, restart and dispatch/release-race fixtures against actual controller access and output behavior. Include enforcement costs in the existing cold/warm and sustained measurements and support matrix.\n+\n+This makes qualification enforceable during use without selecting hardware, an update mechanism, a mobile planner or a compute API. It supplies no passing implementation or deployment evidence; `UBU-Q0177` remains Open pending complete mandatory-slice evidence and operator approval.\n+\n+---\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -112,6 +112,12 @@ On the exact deployment with its egress restrictions active, exercise the frozen\n \n Attach policy-safe manifest and observation evidence with pass/fail/missing stage dispositions to the support matrix. Measure the complete pipeline with its deployed restrictions active under the existing cold/warm and sustained trials, including every enabled stage's aggregate resource and retention costs. Apply affected rechecks when a stage, delegated provider, destination or enforcement configuration changes. These requirements supply no passing deployment or supported hardware; `UBU-Q0177` stays Open pending the existing complete-support-set evidence and operator approval.\n \n+**Qualification bindings at use (`UBU-D0375`).** Bind enabled multimodal modes to the operator-approved profile and complete pipeline manifest. Before source acquisition and inference dispatch, compare the effective deployment with its required pins under `UBU-D0331`/`UBU-D0336`/`UBU-D0368` across every execution endpoint, including actual model identity, delegated providers, effective permissions and retention/egress controls. Record how each required binding is verified. Missing or mismatched required bindings withhold every dependent mode; a stable pathname, backend label or past passing result cannot establish a match.\n+\n+During each job, pin verified artifacts and enforcement settings or detect changes and invalidate affected work before further source access or result release. Recheck the job's qualification bindings at release. On missing or mismatched bindings, apply existing interruption, cancellation and retention rules: stop affected acquisition/output, reject late live results, preserve permitted saved state and keep immediate controls and the manual loop available. A shared component change affects all dependent routes. Changed bindings require affected rechecks and renewed operator approval before use; restored support never resumes capture or replays work automatically.\n+\n+Require actual-controller fixtures for an approved matching route, model replacement at the same path, changed delegated-provider/permission/egress settings, worker-profile mismatch, restart with stale approval and change between dispatch and result release. Verify observed source access, output suppression and dependent-mode unavailability, including useful manual completion. Repeat the existing cold/warm and sustained measurements with these controls active and publish their costs, failures and evidence in the support matrix. These requirements establish no supported deployment; `UBU-Q0177` remains Open.\n+\n **Economically accessible local paths (`UBU-D0332`).** Compare two candidate routes for a bounded camera-and-voice slice under `UBU-D0331`:\n \n - **Existing single Device:** bounded still-image capture and push-to-talk audio, with smaller/quantized or task-specific local models capable of CPU inference, finite context and serialized inference jobs. Evaluate supported older hardware without assuming that age, nominal RAM or model size establishes eligibility.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3457,25 +3457,27 @@\n ## UBU-Q0177: Minimum viable secure local hardware for multimodal interaction\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-15 Scored from commit: None\n \n ### Question\n \n What measured local hardware profile can deliver the MVP multimodal experience securely and usefully?\n \n ### Current direction\n \n Delta \u00a717.15. Evaluate model quality, memory, latency, energy, sustained thermal behavior, endpoint security and local storage on supported devices. Distinguish primary mobile interaction from mobile planning workload (`UBU-Q0160`). No modern-phone universality or cloud-versus-local quality ranking is assumed.\n \n `UBU-D0331` specifies revision-pinned Device-role profiles, predeclared usefulness/resource criteria, real-device cold/warm and sustained trials, endpoint/storage gates and a reviewed support matrix in DESIGN.md \u00a71. This narrows the qualification process without selecting hardware, a mobile planner or a compute API. `UBU-D0336` adds isolation fixtures across inference jobs and measures the enforced isolation/reset costs within that profile.\n \n `UBU-D0362` makes hardware-selection completeness explicit: the approved profile set must cover all mandatory capture/review slices in DESIGN.md \u00a74.2 (`UBU-D0339`), with qualified endpoints, any worker routes and demonstrated shared-hardware workload limits.\n \n `UBU-D0368` extends qualification to every enabled processing stage, including delegated OS services, with connected/offline, backend-loss and reconnection evidence for actual provider calls, egress restrictions and queued retries. The support matrix binds the complete pipeline and its resource/retention costs.\n \n+`UBU-D0375` makes qualification an enforced use-time binding: missing or mismatched deployment pins withhold dependent modes and invalidate affected active work. Changed bindings require affected rechecks and renewed operator approval.\n+\n ### Resolution\n \n Open. `UBU-D0331` answers the qualification protocol only. Close after actual real-device evidence demonstrates the declared MVP slices, numeric latency/resource profiles, endpoint and retention controls, sustained behavior and useful manual degradation, and the operator approves a scoped hardware/software support matrix. No measured minimum or supported Device is established by the protocol. `UBU-D0336` adds runtime-isolation gates; those fixtures also require passing deployment evidence before selection. `UBU-D0362` requires that selection to cover every mandatory \u00a74.2 slice, with each endpoint and any inter-Device route qualified; partial coverage leaves this question Open.\n \n-`UBU-D0368` adds complete-route locality evidence; its required real-device trials remain outstanding and establish no supported configuration.\n+`UBU-D0368` adds complete-route locality evidence; its required real-device trials remain outstanding and establish no supported configuration. `UBU-D0375` adds deployment-binding enforcement and change fixtures; those checks also require passing evidence and select no hardware.\n \n ---\n",
    "commit_message": "docs: enforce multimodal qualification bindings during use",
    "validation_notes": [
      "git apply --check --whitespace=error - passed against the current working-tree files.",
      "Only DECISIONS.md, DESIGN.md \u00a71 and the selected UBU-Q0177 block change; its single-line metadata remains byte-for-byte unchanged.",
      "UBU-D0375 is unused in the original files and appears exactly once as a decision heading in the proposal.",
      "All source working-tree files remain unchanged; no device trials or implementation fixtures were run."
    ],
    "new_questions_added": [],
    "questions_resolved": [],
    "decisions_added": [
      "UBU-D0375"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0177-openai-D0375-deployment-bindings",
    "patch_applies": true,
    "allowlist_passed": true,
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "question_schema_passed": true,
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
- Allowed question `Status:` values are `Archived`, `Decomposed`, `Deferred`, `Open`, `Solved`, `Superseded`; a resolved question must use `Status: Solved`.
- Reject `Status: Resolved` as a question-schema violation. Set `preserves_question_schema` to false and require `Status: Solved` as the fix; `Resolved by:` is a separate metadata field.
- Do not score your own provider's proposal unless explicitly asked for diagnostic self-score.

Return only JSON.
