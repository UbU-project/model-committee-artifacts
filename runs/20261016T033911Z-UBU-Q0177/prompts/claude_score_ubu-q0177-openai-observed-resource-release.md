# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0177`  
Question title: `Minimum viable secure local hardware for multimodal interaction`  
Base commit: `a3b9e57b71e4d3a06aafab3c8cd2e71549507d5d`

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

`UBU-D0375` makes qualification an enforced use-time binding: missing or mismatched deployment pins withhold dependent modes and invalidate affected active work. Changed bindings require affected rechecks and renewed operator approval.

### Resolution

Open. `UBU-D0331` answers the qualification protocol only. Close after actual real-device evidence demonstrates the declared MVP slices, numeric latency/resource profiles, endpoint and retention controls, sustained behavior and useful manual degradation, and the operator approves a scoped hardware/software support matrix. No measured minimum or supported Device is established by the protocol. `UBU-D0336` adds runtime-isolation gates; those fixtures also require passing deployment evidence before selection. `UBU-D0362` requires that selection to cover every mandatory §4.2 slice, with each endpoint and any inter-Device route qualified; partial coverage leaves this question Open.

`UBU-D0368` adds complete-route locality evidence; its required real-device trials remain outstanding and establish no supported configuration. `UBU-D0375` adds deployment-binding enforcement and change fixtures; those checks also require passing evidence and select no hardware.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0177-openai-observed-resource-release",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0177",
    "base_commit": "a3b9e57b71e4d3a06aafab3c8cd2e71549507d5d",
    "summary": "Require observed backend cleanup and resource release when qualifying multimodal hardware.",
    "rationale": "The existing qualification contract requires bounded cancellation but does not specify what establishes backend resource release. Distinguishing immediate invalidation from actual cleanup prevents pending work from disappearing from resource totals or reusing buffers across grants. This narrowly clarifies the existing gate while preserving all mandatory-slice evidence and operator approval requirements.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\nindex 9f4683e..cf069e0 100644\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5882,3 +5882,15 @@ Expire or invalidate pending ingress locally and prevent late dispatch/release u\n Require \u00a721.1.4's delayed-handoff, ingress-saturation, boundary and interruption fixtures, plus pinned deployment trials driven by frozen arrivals for claimed concurrent/sustained workloads. Report ingress and end-to-end timings alongside class timings; an incompletely offered workload cannot qualify a smaller denominator. These are enforcement and evidence requirements, not numerical selection or passing implementation/hardware evidence. `UBU-Q0168` stays Open pending the existing frozen acceptance trials and operator review; no kernel API or wire-schema change is selected.\n \n ---\n+\n+## UBU-D0381: Multimodal qualification verifies backend release after cancellation\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a71. Narrows `UBU-Q0177`; measured hardware selection remains Open.\n+\n+Extend `UBU-D0331`/`UBU-D0336`'s cancellation and reuse evidence to actual backend quiescence, source-dependent buffer cleanup and runtime/worker resource release. Cancellation status, process exit or worker disconnection alone cannot establish complete-route release. Keep outstanding work generation-bound, prevent premature buffer reuse and withhold further dispatch through an affected runtime instance while release is unconfirmed. Immediate invalidation, controls and useful manual completion remain independent of backend progress.\n+\n+Charge unresolved resources and any qualified fallback to the same aggregate limits; neither containment nor pending cleanup extends existing release or retention deadlines. Require delayed-completion, repeated-cancellation, teardown-failure and late-callback/write fixtures with actual lifecycle observations, plus real-device evidence and costs in the existing support matrix. Disconnected workers require endpoint-local enforcement and truthful unconfirmed remote-release status.\n+\n+This specifies evidence for the existing bounded-release gate without supplying passing results, selecting hardware or changing the mobile planning/API allocation. `UBU-Q0177` remains Open pending the complete mandatory-slice evidence and operator approval.\n+\n+---\ndiff --git a/DESIGN.md b/DESIGN.md\nindex d77a81b..5c221d7 100644\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -118,6 +118,12 @@ During each job, pin verified artifacts and enforcement settings or detect chang\n \n Require actual-controller fixtures for an approved matching route, model replacement at the same path, changed delegated-provider/permission/egress settings, worker-profile mismatch, restart with stale approval and change between dispatch and result release. Verify observed source access, output suppression and dependent-mode unavailability, including useful manual completion. Repeat the existing cold/warm and sustained measurements with these controls active and publish their costs, failures and evidence in the support matrix. These requirements establish no supported deployment; `UBU-Q0177` remains Open.\n \n+**Observed multimodal resource release (`UBU-D0381`).** Extend `UBU-D0331`/`UBU-D0336` qualification to the actual lifecycle of source-dependent buffers, pending backend submissions and runtime/worker handles after timeout, stop, expiry or revocation. Local invalidation still blocks further reads and result release immediately. A cancelled status, cancellation acknowledgment, process exit or lost worker link alone cannot establish complete-route cleanup or resource release. Bind outstanding work and buffers to the original job generation; prevent their reuse until backend quiescence and required cleanup are demonstrated. Verified read-only weights retain `UBU-D0336`'s separate treatment.\n+\n+Keep unresolved resources charged to whole-route memory/storage/concurrency limits, including any separately qualified fallback. Withhold further dispatch through the affected runtime instance while release is unconfirmed; immediate controls and useful manual completion must remain schedulable. This containment cannot extend source/intermediate retention or the existing finite cancellation-to-resource-release ceiling. A failed or unobserved required release/retention bound prevents qualification. For a disconnected worker, exercise endpoint-local enforcement and report remote release as unconfirmed until permitted endpoint evidence is available.\n+\n+Extend the existing distinct-grant fixtures with delayed backend completion, repeated cancellation, teardown failure and late callbacks/writes while a subsequent job is requested. Freeze expected dispositions and observe actual dispatch, buffer ownership and denied reuse, not just generated output. Record invalidation, backend quiescence, cleanup and resource-release offsets separately, with pending-resource high-water marks. Controller injections alone supply no deployed backend-release evidence; demonstrate the supported lifecycle on the exact physical endpoints. Include unresolved work, teardown and any fallback in the existing cold/warm and sustained resource measurements and support matrix. These are runtime-lifecycle observations, not forensic-erasure guarantees or passing hardware evidence; `UBU-Q0177` remains Open.\n+\n **Economically accessible local paths (`UBU-D0332`).** Compare two candidate routes for a bounded camera-and-voice slice under `UBU-D0331`:\n \n - **Existing single Device:** bounded still-image capture and push-to-talk audio, with smaller/quantized or task-specific local models capable of CPU inference, finite context and serialized inference jobs. Evaluate supported older hardware without assuming that age, nominal RAM or model size establishes eligibility.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\nindex b165827..025888d 100644\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3477,29 +3477,33 @@\n ## UBU-Q0177: Minimum viable secure local hardware for multimodal interaction\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-15 Scored from commit: None\n \n ### Question\n \n What measured local hardware profile can deliver the MVP multimodal experience securely and usefully?\n \n ### Current direction\n \n Delta \u00a717.15. Evaluate model quality, memory, latency, energy, sustained thermal behavior, endpoint security and local storage on supported devices. Distinguish primary mobile interaction from mobile planning workload (`UBU-Q0160`). No modern-phone universality or cloud-versus-local quality ranking is assumed.\n \n `UBU-D0331` specifies revision-pinned Device-role profiles, predeclared usefulness/resource criteria, real-device cold/warm and sustained trials, endpoint/storage gates and a reviewed support matrix in DESIGN.md \u00a71. This narrows the qualification process without selecting hardware, a mobile planner or a compute API. `UBU-D0336` adds isolation fixtures across inference jobs and measures the enforced isolation/reset costs within that profile.\n \n `UBU-D0362` makes hardware-selection completeness explicit: the approved profile set must cover all mandatory capture/review slices in DESIGN.md \u00a74.2 (`UBU-D0339`), with qualified endpoints, any worker routes and demonstrated shared-hardware workload limits.\n \n `UBU-D0368` extends qualification to every enabled processing stage, including delegated OS services, with connected/offline, backend-loss and reconnection evidence for actual provider calls, egress restrictions and queued retries. The support matrix binds the complete pipeline and its resource/retention costs.\n \n `UBU-D0375` makes qualification an enforced use-time binding: missing or mismatched deployment pins withhold dependent modes and invalidate affected active work. Changed bindings require affected rechecks and renewed operator approval.\n \n+`UBU-D0381` requires observed backend quiescence, buffer cleanup and resource release after cancellation. Unresolved work remains generation-bound and charged to aggregate limits, with further dispatch through the affected runtime withheld; cancellation status or disconnection alone cannot establish release.\n+\n ### Resolution\n \n Open. `UBU-D0331` answers the qualification protocol only. Close after actual real-device evidence demonstrates the declared MVP slices, numeric latency/resource profiles, endpoint and retention controls, sustained behavior and useful manual degradation, and the operator approves a scoped hardware/software support matrix. No measured minimum or supported Device is established by the protocol. `UBU-D0336` adds runtime-isolation gates; those fixtures also require passing deployment evidence before selection. `UBU-D0362` requires that selection to cover every mandatory \u00a74.2 slice, with each endpoint and any inter-Device route qualified; partial coverage leaves this question Open.\n \n `UBU-D0368` adds complete-route locality evidence; its required real-device trials remain outstanding and establish no supported configuration. `UBU-D0375` adds deployment-binding enforcement and change fixtures; those checks also require passing evidence and select no hardware.\n \n+`UBU-D0381` adds actual backend-release and pending-resource fixtures, including disconnected-worker enforcement. These still require real-device evidence within the existing release/retention bounds and establish no supported profile.\n+\n ---\n \n ## UBU-Q0178: Economically accessible local compute paths\n",
    "commit_message": "docs: require observed multimodal backend resource release",
    "validation_notes": [
      "Generated the diff against current working-tree contents; git apply --check --whitespace=error passed.",
      "Three allowlisted documents, 22 insertions; UBU-Q0177 metadata preserved and its own heading and Resolution anchor the question hunk.",
      "No repository files changed. No real-device trials performed; UBU-Q0177 remains Open and no hardware is qualified."
    ],
    "new_questions_added": [],
    "questions_resolved": [],
    "decisions_added": [
      "UBU-D0381"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0177-openai-observed-resource-release",
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
