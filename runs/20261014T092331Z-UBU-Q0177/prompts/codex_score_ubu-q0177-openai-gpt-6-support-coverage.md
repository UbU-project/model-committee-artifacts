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
Base commit: `2022f14781aaa057fd2965c7bec1ea2c71e09cfc`

```markdown
## UBU-Q0177: Minimum viable secure local hardware for multimodal interaction

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-14 Scored from commit: None

### Question

What measured local hardware profile can deliver the MVP multimodal experience securely and usefully?

### Current direction

Delta §17.15. Evaluate model quality, memory, latency, energy, sustained thermal behavior, endpoint security and local storage on supported devices. Distinguish primary mobile interaction from mobile planning workload (`UBU-Q0160`). No modern-phone universality or cloud-versus-local quality ranking is assumed.

`UBU-D0331` specifies revision-pinned Device-role profiles, predeclared usefulness/resource criteria, real-device cold/warm and sustained trials, endpoint/storage gates and a reviewed support matrix in DESIGN.md §1. This narrows the qualification process without selecting hardware, a mobile planner or a compute API. `UBU-D0336` adds isolation fixtures across inference jobs and measures the enforced isolation/reset costs within that profile.

### Resolution

Open. `UBU-D0331` answers the qualification protocol only. Close after actual real-device evidence demonstrates the declared MVP slices, numeric latency/resource profiles, endpoint and retention controls, sustained behavior and useful manual degradation, and the operator approves a scoped hardware/software support matrix. No measured minimum or supported Device is established by the protocol. `UBU-D0336` adds runtime-isolation gates; those fixtures also require passing deployment evidence before selection.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0177-openai-gpt-6-support-coverage",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0177",
    "base_commit": "2022f14781aaa057fd2965c7bec1ea2c71e09cfc",
    "summary": "Require measured, operator-approved hardware profiles covering every mandatory MVP capture slice before closing UBU-Q0177.",
    "rationale": "UBU-D0331 and UBU-D0336 already define qualification. Clarify how that evidence completes hardware selection under UBU-D0339's bounded desktop, bounded phone and desktop realtime requirements. Record partial coverage without treating one bounded camera-and-voice result as full MVP support; qualify shared-hardware workloads or demonstrate enforced mode exclusions. No hardware measurements or supported minimum are asserted.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\nindex edf17cc..877b6d3 100644\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5654,3 +5654,15 @@ Account for every offered workload request, including refusals, timeouts, failur\n DESIGN.md \u00a721.1.4 supplies the accounting and pass/fail requirements alongside `UBU-D0331`/`UBU-D0336`'s endpoint/runtime trials and `UBU-D0335`'s enforcement fixtures. Correct bounded fallback alone cannot qualify an ordinary workload that misses its declared usefulness limit. This selects no numerical ceiling, passing implementation or supported hardware; `UBU-Q0168` stays Open pending operator-reviewed evidence.\n \n ---\n+\n+## UBU-D0362: MVP hardware selection requires coverage of every mandatory capture slice\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a71. Narrows `UBU-Q0177`; measured hardware selection remains Open.\n+\n+Apply `UBU-D0331`/`UBU-D0336` to a support set covering every mandatory \u00a74.2 slice allocated by `UBU-D0339`: bounded laptop/desktop camera-and-voice, bounded phone capture/review with its selected inference route, and finite laptop/desktop realtime audio/video. Map each slice to exact capture, review and inference Device profiles and any inter-Device route, with revision-pinned passing whole-route evidence and operator approval bound to those revisions. Qualify proposed workloads on shared physical hardware or demonstrate enforced mode exclusions.\n+\n+Operator-approved passing configurations may establish scoped partial support. Keep failed or missing slices and endpoint/route gates visible; close `UBU-Q0177` only after every mandatory slice has an approved evidence-backed configuration. A bounded camera-and-voice path cannot substitute for the realtime slice.\n+\n+This clarifies completeness within the existing release scope. It supplies no measured result, supported minimum or mobile planner/API selection and changes no switch or release commitment.\n+\n+---\ndiff --git a/DESIGN.md b/DESIGN.md\nindex 2ac2027..c5ae5d7 100644\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -102,6 +102,10 @@ Run paired synthetic camera-and-voice jobs A/B under distinct grants, with B den\n \n Record pinned fixture/grant versions, expected and observed access/release outcomes, runtime permission observations, reset/termination behavior and policy-safe evidence links in the qualification report. Every enabled reuse/concurrency mode must demonstrate the allowed control and zero unauthorized reads/releases in these fixtures; unobserved mandatory paths remain unqualified. Repeat the cold/warm and sustained workload measurements with the enforced isolation, reset and cache restrictions active, accounting for their time, energy and aggregate memory/storage costs. Publish limitations beside the support matrix. These fixtures supply no passing runtime, hardware selection or forensic-erasure result.\n \n+**MVP hardware support coverage (`UBU-D0362`).** Select a set of qualified hardware/software profiles that collectively covers every mandatory capture/review slice in \u00a74.2 (`UBU-D0339`): bounded laptop/desktop camera-and-voice, bounded phone capture/review with its selected inference route, and finite laptop/desktop realtime audio/video. For each required slice, the support matrix identifies the exact capture, review and inference Device profiles, any inter-Device route, enabled sources/classes, declared concurrency and session limits, passing whole-route evidence and operator approval bound to those revisions. Shared physical hardware may cover several slices; qualify the proposed combined workload or enforce and demonstrate the declared mode exclusions under `UBU-D0331`/`UBU-D0336`.\n+\n+Record pass, fail or missing evidence for each mandatory slice and each required endpoint/route gate. Operator-approved passing configurations may establish support for their qualified slices while incomplete coverage remains visible. Close `UBU-Q0177` only after the operator approves a complete support set with passing evidence for every mandatory slice; a bounded camera-and-voice result alone cannot establish the realtime slice. This clarifies hardware-selection completeness under the existing release scope and supplies no measured result or mobile planner/API selection.\n+\n **Economically accessible local paths (`UBU-D0332`).** Compare two candidate routes for a bounded camera-and-voice slice under `UBU-D0331`:\n \n - **Existing single Device:** bounded still-image capture and push-to-talk audio, with smaller/quantized or task-specific local models capable of CPU inference, finite context and serialized inference jobs. Evaluate supported older hardware without assuming that age, nominal RAM or model size establishes eligibility.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\nindex 90aab43..08b157c 100644\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3400,31 +3400,33 @@ Open.\n ## UBU-Q0177: Minimum viable secure local hardware for multimodal interaction\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-14 Scored from commit: None\n \n ### Question\n \n What measured local hardware profile can deliver the MVP multimodal experience securely and usefully?\n \n ### Current direction\n \n Delta \u00a717.15. Evaluate model quality, memory, latency, energy, sustained thermal behavior, endpoint security and local storage on supported devices. Distinguish primary mobile interaction from mobile planning workload (`UBU-Q0160`). No modern-phone universality or cloud-versus-local quality ranking is assumed.\n \n `UBU-D0331` specifies revision-pinned Device-role profiles, predeclared usefulness/resource criteria, real-device cold/warm and sustained trials, endpoint/storage gates and a reviewed support matrix in DESIGN.md \u00a71. This narrows the qualification process without selecting hardware, a mobile planner or a compute API. `UBU-D0336` adds isolation fixtures across inference jobs and measures the enforced isolation/reset costs within that profile.\n \n+`UBU-D0362` makes hardware-selection completeness explicit: the approved profile set must cover all mandatory capture/review slices in DESIGN.md \u00a74.2 (`UBU-D0339`), with qualified endpoints, any worker routes and demonstrated shared-hardware workload limits.\n+\n ### Resolution\n \n-Open. `UBU-D0331` answers the qualification protocol only. Close after actual real-device evidence demonstrates the declared MVP slices, numeric latency/resource profiles, endpoint and retention controls, sustained behavior and useful manual degradation, and the operator approves a scoped hardware/software support matrix. No measured minimum or supported Device is established by the protocol. `UBU-D0336` adds runtime-isolation gates; those fixtures also require passing deployment evidence before selection.\n+Open. `UBU-D0331` answers the qualification protocol only. Close after actual real-device evidence demonstrates the declared MVP slices, numeric latency/resource profiles, endpoint and retention controls, sustained behavior and useful manual degradation, and the operator approves a scoped hardware/software support matrix. No measured minimum or supported Device is established by the protocol. `UBU-D0336` adds runtime-isolation gates; those fixtures also require passing deployment evidence before selection. `UBU-D0362` requires that selection to cover every mandatory \u00a74.2 slice, with each endpoint and any inter-Device route qualified; partial coverage leaves this question Open.\n \n ---\n \n ## UBU-Q0178: Economically accessible local compute paths\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-14 Scored from commit: None\n \n ### Question\n \n Which lower-cost local compute paths prevent economically precarious users from being excluded from the multimodal experience?\n \n ### Current direction\n \n Delta \u00a717.16. Consider smaller local models, bounded snapshots, CPU fallback, reduced latency classes and operator-controlled trusted hardware without a hidden hosted-model dependency. Measure the useful tradeoffs before claiming reach or costs. Compute accessibility is part of the mission, not a promised device minimum.\n",
    "commit_message": "docs: require complete MVP hardware support coverage",
    "validation_notes": [
      "Generated the diff with git diff --no-index from current working-tree contents and proposed contents supplied through stdin; normalized destination labels and hashes.",
      "git apply --check against the current working tree passed.",
      "git apply --check --whitespace=error-all passed.",
      "Verified UBU-D0362 is unused, only UBU-Q0177's question block changes, and its single-line metadata remains unchanged: Status Open; Resolved by None.",
      "Repository files remain unchanged. No real-device measurements were performed; hardware selection remains Open and requires human review."
    ],
    "new_questions_added": [],
    "questions_resolved": [],
    "decisions_added": [
      "UBU-D0362"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0177-openai-gpt-6-support-coverage",
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
