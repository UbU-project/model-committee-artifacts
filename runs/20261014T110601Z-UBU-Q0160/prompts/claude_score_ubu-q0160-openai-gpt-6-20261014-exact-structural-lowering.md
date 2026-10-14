# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0160`  
Question title: `Which mobile compute API should the mobile GPU backend target?`  
Base commit: `d91f3aeef2b51a1cfb1d279736fcbcbf69bf9a05`

```markdown
## UBU-Q0160: Which mobile compute API should the mobile GPU backend target?

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0156 Blocks: mobile GPU backend selection Resolved by: None Last scored: 2026-10-14 Scored from commit: None

Defining context: DESIGN.md §16.5, §16.10.6; PLANNING_KERNEL_CONTRACT.md §4; `UBU-D0126`, `UBU-D0256`, `UBU-D0283`, `UBU-D0290`, `UBU-D0339`, `UBU-D0358`, `UBU-D0364`.

### Question

Which compute API should implement the in-process mobile GPU backend while preserving CPU certification, provenance and the existing parity contract?

### Subquestions

1. How do Vulkan compute, OpenCL on Adreno, ExecuTorch and NNAPI compare on the actual supported device and driver stack?
2. Which candidate passes exact structural/hard-constraint parity and the named numeric tolerances against the CPU reference path?
3. What latency, memory, energy and sustained thermal behavior does each exhibit for deterministic skeletonization, hard checks, local repair recipes and a short-horizon branch cache?
4. Which APIs and operations are actually available on the selected LineageOS build, and what CPU fallback is required?

5. **Scope answered by `UBU-D0358`.** Retain `UBU-D0290`'s stewardship planning workload for the primary phone capture/review client; full mobile search is not required. Measure contention with the intended bounded camera/voice route before selecting an accelerator.
6. **Existing release gates.** `UBU-D0339` resolved `UBU-Q0183`'s capture/device allocation and sync sequence. Local perception, voice, battery and privacy qualification follow DESIGN.md §4.2, §12.2, §21.1 and §1; `UBU-Q0168`/`UBU-Q0177` retain measured profiles. Planning parity and on-device CPU certification remain independent requirements.

### Current direction

`UBU-D0358` retains mobile stewardship and separates planning API selection from the phone interaction slice already allocated by `UBU-D0339`. DESIGN.md §16.10.6 requires a frozen same-device CPU comparison and candidate execution/evidence matrix, including actual accelerator use, parity, certified-result latency, aggregate resources, sustained thermals, concurrent capture/voice load and fallback. `UBU-D0364` adds matched workload/cache conditions and complete trial accounting, separating accelerated results from full CPU fallback and missing outcomes. No broader planning workload or compute API is selected; perception-model suitability needs its own evidence.

No compute API is selected. `UBU-Q0156` settled the desktop invocation boundary; it does not select a mobile backend. This question is blocked on the on-device parity harness producing measurements. The gate is satisfied only once the stewardship workload has been measured against the CPU reference path on physically connected real hardware under the `UBU-D0283` parity rule. Emulator timings and desktop full-search benchmarks do not answer it.

### Resolution

Partially answered by `UBU-D0358`: retain stewardship planning, apply the existing capture/release allocation and specify a reproducible comparison. `UBU-D0364` adds matched trials and complete outcome accounting. API selection remains Open pending physically connected device measurements and operator review; no hardware result or passing API is inferred from this protocol, API availability or CPU fallback.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "ubu-q0160-openai-gpt-6-20261014-exact-structural-lowering",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0160",
    "base_commit": "d91f3aeef2b51a1cfb1d279736fcbcbf69bf9a05",
    "summary": "Require lossless structural lowering and representation-boundary fixtures in the mobile API comparison; retain CPU routing and keep API selection open.",
    "rationale": "The existing comparison identifies supported operations and dtypes but does not explicitly test whether coordinate conversion and intermediate arithmetic preserve exact constraint decisions. Pinning representation bounds and testing precision, equality and overflow boundaries makes candidate mappings reviewable without inferring an API winner from availability or CPU fallback.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\nindex 816db97..0000000 100644\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5748,3 +5748,15 @@ Account for every attempt, payment, transferred byte, helper/operator effort, en\n Publish evidence and pass/fail/unknown upkeep-limit results by ownership/horizon scenario in the existing cost/support matrix. Period estimates disclose update/recovery frequency and uncertainty. Failed or unknown required limits prevent selection for that scenario; shorter horizons or budgeted replacement require separate evaluation. These trials establish no future update availability, maintenance automation, passing configuration or selected path. Keep the no-worker gap explicit and `UBU-Q0178` Open pending operator-reviewed comparative evidence.\n \n ---\n+\n+## UBU-D0370: Mobile API qualification requires exact structural lowering\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a716.10.6. Narrows `UBU-Q0160`; extends `UBU-D0358`/`UBU-D0364` under the unchanged `UBU-D0283` parity rule.\n+\n+Freeze each candidate's representation of time coordinates, durations, Task/branch indices and masks alongside its operation mapping. Record units, origin, dtypes, supported ranges, intermediate arithmetic bounds and checked CPU dispatch conditions. Lowering must preserve the CPU reference semantics and exact structural comparisons; accelerator-specific rounding, quantization or a coarser planning delta cannot change the request. Structural operations that cannot meet this requirement remain explicitly CPU-resident, or the unchanged request uses CPU stewardship. Backend range limits do not make an otherwise valid request semantically invalid.\n+\n+Add shared synthetic representation-boundary fixtures to the existing real-device parity suite: lossless encode/readback, large absolute timestamps with small separations, translated time origins, supported-range boundaries and duration accumulation. Compare exact outputs before CPU certification, and test unsupported-range routing without silent overflow or truncation. Named score tolerances cannot excuse a changed hard-constraint decision. Publish these outcomes with the existing matched trials and fallback accounting.\n+\n+This specifies candidate implementation evidence without selecting a dtype, compute API or passing configuration. It changes no planning schema, tolerance rule, workload or release/switch criterion. `UBU-Q0160` remains Open pending physically connected device measurements and operator review.\n+\n+---\ndiff --git a/DESIGN.md b/DESIGN.md\nindex 18f32d0..0000000 100644\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -2895,6 +2895,10 @@ Validation uses a physically connected device over `adb`, not an AVD: emulator g\n \n Publish one evidence row for the CPU baseline and each candidate configuration of Vulkan compute, OpenCL on Adreno, ExecuTorch and NNAPI. Identify any runtime/delegate, actual compute API/driver and accelerator used, supported operations/dtypes, CPU-resident portions and unavailable/unsuitable operations with reasons. Treat a runtime configuration as an implementation to measure, rather than assuming its name denotes a GPU API. Demonstrate execution of the claimed accelerated operations on the selected device; API enumeration, library loading, emulation and silent CPU fallback cannot establish it.\n \n+**Exact structural lowering (`UBU-D0370`).** Extend each frozen candidate operation mapping with its CPU-to-accelerator representation of time coordinates, durations, Task/branch indices and masks: units, time origin, storage and arithmetic dtypes, supported ranges, intermediate bounds and checked CPU dispatch conditions. Verify lossless encode/readback at the CPU reference path's declared resolution and preserve its exact interval, dependency and mask semantics. Internal lowering cannot silently round timestamps, quantize durations or change `planning_delta_seconds`. Keep a structural operation CPU-resident when the accelerator cannot preserve those exact semantics; declare and measure that hybrid mapping. For an otherwise valid request outside the candidate's proven bounds, select the CPU path with the unchanged request/seed under the existing budget and fallback rules, rather than reporting a semantic rejection or silently overflowing.\n+\n+Include shared synthetic fixtures with large absolute timestamps separated by one reference time unit; paired fixtures translating all relevant absolute times by a common offset while preserving relative constraints; endpoint comparisons immediately before, at and after equality; Task/branch index and padding boundaries; and duration sums near the declared arithmetic limits. Compare decoded/readback values and existing exact-parity outputs against the CPU reference, normalizing only the declared time origin for translated pairs. Exercise both supported-boundary inputs and otherwise valid inputs outside the accelerator's bounds, with the latter demonstrating explicit CPU routing. Freeze expected dispositions with `UBU-D0364`'s manifest and account for fallback separately. Run the claimed accelerated operations on the physical device and retain policy-permitted input, readback and comparison evidence. A numeric score tolerance cannot excuse a different exact structural or hard-constraint result, and CPU certification cannot repair a failed parity comparison. These fixtures select no representation width, API or passing backend.\n+\n | Evidence | Required comparison |\n | --- | --- |\n | Semantic parity | Replay the same bounded skeleton, hard-check, repair and branch-cache fixtures against the CPU reference. Include \u00a75 of PLANNING_KERNEL_CONTRACT.md's missing-prerequisite and endpoint-equality cases, protected Tasks, stale envelopes, all five \u00a716.5 repair triggers, and cache hits/misses. Enumerate applicable stage outputs and justify any non-applicable stage; record exact structural/feasibility/rejection comparisons and named numeric/statistical checks before CPU certification. CPU rejection cannot turn an accelerator disagreement into a parity pass. |\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\nindex 2de6ca4..0000000 100644\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -2990,43 +2990,43 @@ Enabled Preferences between two routine evergreen Objectives lower at request ti\n \n Resolved by `UBU-D0288`: routine occurrences are mandatory and carry no priority. They are not layered, are sent with `TaskSpec.value = 0.0`, must be placed, and are never omitted by partial placement; when a day cannot hold them all, the user triages.\n \n ---\n \n ## UBU-Q0160: Which mobile compute API should the mobile GPU backend target?\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0156 Blocks: mobile GPU backend selection Resolved by: None Last scored: 2026-10-14 Scored from commit: None\n \n-Defining context: DESIGN.md \u00a716.5, \u00a716.10.6; PLANNING_KERNEL_CONTRACT.md \u00a74; `UBU-D0126`, `UBU-D0256`, `UBU-D0283`, `UBU-D0290`, `UBU-D0339`, `UBU-D0358`, `UBU-D0364`.\n+Defining context: DESIGN.md \u00a716.5, \u00a716.10.6; PLANNING_KERNEL_CONTRACT.md \u00a74; `UBU-D0126`, `UBU-D0256`, `UBU-D0283`, `UBU-D0290`, `UBU-D0339`, `UBU-D0358`, `UBU-D0364`, `UBU-D0370`.\n \n ### Question\n \n Which compute API should implement the in-process mobile GPU backend while preserving CPU certification, provenance and the existing parity contract?\n \n ### Subquestions\n \n 1. How do Vulkan compute, OpenCL on Adreno, ExecuTorch and NNAPI compare on the actual supported device and driver stack?\n 2. Which candidate passes exact structural/hard-constraint parity and the named numeric tolerances against the CPU reference path?\n 3. What latency, memory, energy and sustained thermal behavior does each exhibit for deterministic skeletonization, hard checks, local repair recipes and a short-horizon branch cache?\n 4. Which APIs and operations are actually available on the selected LineageOS build, and what CPU fallback is required?\n \n 5. **Scope answered by `UBU-D0358`.** Retain `UBU-D0290`'s stewardship planning workload for the primary phone capture/review client; full mobile search is not required. Measure contention with the intended bounded camera/voice route before selecting an accelerator.\n 6. **Existing release gates.** `UBU-D0339` resolved `UBU-Q0183`'s capture/device allocation and sync sequence. Local perception, voice, battery and privacy qualification follow DESIGN.md \u00a74.2, \u00a712.2, \u00a721.1 and \u00a71; `UBU-Q0168`/`UBU-Q0177` retain measured profiles. Planning parity and on-device CPU certification remain independent requirements.\n \n ### Current direction\n \n-`UBU-D0358` retains mobile stewardship and separates planning API selection from the phone interaction slice already allocated by `UBU-D0339`. DESIGN.md \u00a716.10.6 requires a frozen same-device CPU comparison and candidate execution/evidence matrix, including actual accelerator use, parity, certified-result latency, aggregate resources, sustained thermals, concurrent capture/voice load and fallback. `UBU-D0364` adds matched workload/cache conditions and complete trial accounting, separating accelerated results from full CPU fallback and missing outcomes. No broader planning workload or compute API is selected; perception-model suitability needs its own evidence.\n+`UBU-D0358` retains mobile stewardship and separates planning API selection from the phone interaction slice already allocated by `UBU-D0339`. DESIGN.md \u00a716.10.6 requires a frozen same-device CPU comparison and candidate execution/evidence matrix, including actual accelerator use, parity, certified-result latency, aggregate resources, sustained thermals, concurrent capture/voice load and fallback. `UBU-D0364` adds matched workload/cache conditions and complete trial accounting, separating accelerated results from full CPU fallback and missing outcomes. `UBU-D0370` adds exact structural lowering and representation-boundary fixtures to the candidate mappings, with explicit CPU routing for unsupported ranges. No broader planning workload or compute API is selected; perception-model suitability needs its own evidence.\n \n No compute API is selected. `UBU-Q0156` settled the desktop invocation boundary; it does not select a mobile backend. This question is blocked on the on-device parity harness producing measurements. The gate is satisfied only once the stewardship workload has been measured against the CPU reference path on physically connected real hardware under the `UBU-D0283` parity rule. Emulator timings and desktop full-search benchmarks do not answer it.\n \n ### Resolution\n \n-Partially answered by `UBU-D0358`: retain stewardship planning, apply the existing capture/release allocation and specify a reproducible comparison. `UBU-D0364` adds matched trials and complete outcome accounting. API selection remains Open pending physically connected device measurements and operator review; no hardware result or passing API is inferred from this protocol, API availability or CPU fallback.\n+Partially answered by `UBU-D0358`: retain stewardship planning, apply the existing capture/release allocation and specify a reproducible comparison. `UBU-D0364` adds matched trials and complete outcome accounting; `UBU-D0370` specifies exact representation and arithmetic evidence. API selection remains Open pending physically connected device measurements and operator review; no hardware result or passing API is inferred from this protocol, API availability or CPU fallback.\n \n ---\n \n ## UBU-Q0161: Task descriptions as lossless prose after Clarify\n \n Status: Solved Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: UBU-D0317 Last scored: 2026-10-11 Scored from commit: None\n \n ### Question\n \n",
    "commit_message": "docs: require exact structural lowering in mobile API trials",
    "validation_notes": [
      "Generated the diff from current working-tree files and proposed contents using git diff --no-index; repository files were not edited.",
      "git apply --check --whitespace=error - passed against the working tree; all hunk counts were independently verified.",
      "Preserved every OPEN_QUESTIONS.md metadata line and confined question edits to UBU-Q0160; UBU-D0370 is unused in the baseline.",
      "No device, API capability or performance tests were run. UBU-Q0160 remains Open pending physically connected device measurements and operator review."
    ],
    "new_questions_added": [],
    "questions_resolved": [],
    "decisions_added": [
      "UBU-D0370"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "ubu-q0160-openai-gpt-6-20261014-exact-structural-lowering",
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
