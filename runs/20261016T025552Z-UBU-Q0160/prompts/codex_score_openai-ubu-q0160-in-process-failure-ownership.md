# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0160`  
Question title: `Which mobile compute API should the mobile GPU backend target?`  
Base commit: `358fd62877223eda7b5e0bd04607ae227fed98f8`

```markdown
## UBU-Q0160: Which mobile compute API should the mobile GPU backend target?

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0156 Blocks: mobile GPU backend selection Resolved by: None Last scored: 2026-10-15 Scored from commit: None

Defining context: DESIGN.md §16.5, §16.10.6; PLANNING_KERNEL_CONTRACT.md §4; `UBU-D0126`, `UBU-D0256`, `UBU-D0283`, `UBU-D0290`, `UBU-D0339`, `UBU-D0358`, `UBU-D0364`, `UBU-D0370`.

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

`UBU-D0358` retains mobile stewardship and separates planning API selection from the phone interaction slice already allocated by `UBU-D0339`. DESIGN.md §16.10.6 requires a frozen same-device CPU comparison and candidate execution/evidence matrix, including actual accelerator use, parity, certified-result latency, aggregate resources, sustained thermals, concurrent capture/voice load and fallback. `UBU-D0364` adds matched workload/cache conditions and complete trial accounting, separating accelerated results from full CPU fallback and missing outcomes. `UBU-D0370` adds exact structural lowering and representation-boundary fixtures to the candidate mappings, with explicit CPU routing for unsupported ranges. No broader planning workload or compute API is selected; perception-model suitability needs its own evidence.

No compute API is selected. `UBU-Q0156` settled the desktop invocation boundary; it does not select a mobile backend. This question is blocked on the on-device parity harness producing measurements. The gate is satisfied only once the stewardship workload has been measured against the CPU reference path on physically connected real hardware under the `UBU-D0283` parity rule. Emulator timings and desktop full-search benchmarks do not answer it.

### Resolution

Partially answered by `UBU-D0358`: retain stewardship planning, apply the existing capture/release allocation and specify a reproducible comparison. `UBU-D0364` adds matched trials and complete outcome accounting; `UBU-D0370` specifies exact representation and arithmetic evidence. API selection remains Open pending physically connected device measurements and operator review; no hardware result or passing API is inferred from this protocol, API availability or CPU fallback.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "openai-ubu-q0160-in-process-failure-ownership",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0160",
    "base_commit": "358fd62877223eda7b5e0bd04607ae227fed98f8",
    "summary": "Specify mobile GPU timeout handling, buffer ownership and CPU fallback accounting while retaining the measured API-selection gate.",
    "rationale": "Existing requirements cover fallback and late-result rejection but leave ownership of still-active submissions and buffers unspecified. This clarification prevents stale cache updates and premature buffer reuse, and includes residual accelerator costs in fallback measurements. API selection remains Open pending physical-device evidence and operator review.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5834,3 +5834,15 @@ Exercise a predeclared finite demand window covering the claimed bursts and avai\n Publish observed-window results separately from period projections, with availability, competing-use and upkeep-downtime assumptions and uncertainty. Add pass/fail/unknown workload-target results beside the existing budget/gate results. A route qualifies only when both the required useful workload and whole-route budgets pass for that scenario; missing capacity evidence prevents selection. Reduced demand, longer waits or additional hardware require a separately evaluated scenario. Keep the no-worker gap visible and `UBU-Q0178` Open pending operator-reviewed comparative evidence; this selects no measured path or cost envelope.\n \n ---\n+\n+## UBU-D0377: Mobile GPU timeouts preserve attempt and buffer ownership\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a716.10.6. Narrows `UBU-Q0160`'s fallback evidence; extends `UBU-D0358`/`UBU-D0364` without selecting an API.\n+\n+A timeout or cancellation invalidates a mobile accelerator attempt, but establishes neither device completion nor resource release. Keep CPU fallback and limitation display independent of blocked accelerator calls. Bind callbacks and buffers to a CPU-owned attempt generation; serialize invalidation with acceptance so late results cannot publish, populate branch caches or replace a fallback result.\n+\n+Retain ownership of in-flight resources until completion or demonstrated safe teardown, use separate CPU fallback storage, and stop further dispatch through the affected planning runtime instance while timed-out work remains unresolved. Freeze finite response, outstanding-submission and retained-memory limits. Account for residual accelerator work together with fallback costs under the original request budget. Process loss is not successful CPU fallback; restart retains the existing fresh-validity checks.\n+\n+Require \u00a716.10.6's delayed-completion, readback/cancellation-race, repeated-timeout and stale-callback fixtures plus policy-safe physical-device observations. Injected callbacks establish controller behavior only, not driver recovery. Publish pending versus released resources and actual outcomes under the existing accounting rules. Failed required bounds withhold the candidate configuration. The in-process boundary, planning schemas, parity rule and release/switch criteria remain unchanged; `UBU-Q0160` stays Open pending real-device measurements and operator review.\n+\n+---\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -2918,6 +2918,12 @@ Include shared synthetic fixtures with large absolute timestamps separated by on\n | Shared phone budget | Repeat the proposed concurrent planning, UI and sync load with the bounded camera/voice route intended under \u00a74.2, accounting for every participating endpoint. Apply the existing perception/retention/deadline qualification gates to that route; do not substitute planning timings for capture-to-review measurements. Report contention, stop responsiveness and resource degradation without expanding the planning workload. |\n | Fallback and recovery | Exercise unavailable operations/runtime, initialization/allocation failure, accelerator timeout/failure, interruption, low battery and thermal limits. CPU selection/fallback must preserve request/seed and semantic checks, reject late accelerator results and fit the remaining request budget; otherwise expose the planning limitation or an eligible cached/clarification path under \u00a716.5. Record the implementation that actually produced the returned result in existing provenance. A process restart requires fresh validity checks before Plan reuse. |\n \n+**In-process failure ownership (`UBU-D0377`).** Keep CPU fallback and limitation display schedulable without waiting for accelerator completion or acquiring locks held by blocked accelerator calls. Bind each submission, callback and buffer to the original request and a CPU-owned local attempt generation. Serialize expiry/cancellation with result acceptance: invalidate the accelerator attempt before starting fallback or exposing a limitation. Late results cannot publish, populate branch caches or replace the fallback result. Attempt generations are implementation bookkeeping, not new planning-contract fields.\n+\n+Stopping a wait establishes neither device completion nor resource release. Retain in-flight buffers and runtime handles until completion or demonstrated safe teardown; CPU fallback uses separate storage. Stop further dispatch through the affected planning runtime instance while timed-out work remains unresolved. Freeze finite invalidation-to-fallback/limitation response ceilings, outstanding-submission limits and retained-memory limits with the candidate mapping. Include unresolved accelerator work and CPU fallback together in aggregate memory, energy and sustained-load accounting; fallback still has only the original request's remaining budget. Process loss cannot count as successful CPU fallback, and restarting requires the existing fresh Plan-validity checks.\n+\n+Extend the frozen fault cohorts with completion after timeout, cancellation racing readback, repeated requests while timed-out work remains outstanding, and old callbacks after fallback or a new request. Exercise actual controller/adapter result and cache acceptance, buffer ownership and resource high-water marks, then demonstrate supported failure behavior on the physical device under the existing pre-flight/recovery rules. Callback injection proves controller behavior only; it supplies no driver-cancellation or resource-reclamation evidence. Report measured response times, completion/release versus still-pending resources, process loss and missed deadlines through `UBU-D0364`'s outcome accounting. A failed declared bound withholds that configuration; thread separation alone establishes no crash isolation.\n+\n **Matched trials and outcome accounting (`UBU-D0364`).** Keep calibration used to propose limits separate from acceptance runs. Before acceptance, freeze a workload manifest with request/fixture digests and seed order, task and branch limits, repair inputs, cache starting state and hit/miss mix, offered-request cadence and concurrency, and expected result for each case. Apply the same manifest to the same-device CPU baseline and each candidate configuration. Include a positive minimum proportion of timely, parity-correct expected results for each ordinary cold, warm, sustained and concurrent-load condition. Declare fault/interruption cohorts and their expected degraded dispositions separately; never move failed ordinary trials into those cohorts. Changes to mappings, limits or the workload after observing acceptance results require a new comparison revision and affected reruns.\n \n For each original offered request, record acceptance or refusal, actual accelerated operations and declared CPU-resident portions, result/certification disposition, fallback and total elapsed work. Distinguish a result exercising its declared accelerated operations from full CPU fallback, expected rejection/clarification, expiry, cancellation, error and no result. An expected rejection or clarification counts as correct only under that fixture's frozen rubric; it does not count as an accelerated Plan. Count the original request once; retries and fallback share its original deadline and contribute all their time and resource costs. Exact parity remains mandatory, including on expected rejection cases.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -2999,31 +2999,31 @@\n ## UBU-Q0160: Which mobile compute API should the mobile GPU backend target?\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0156 Blocks: mobile GPU backend selection Resolved by: None Last scored: 2026-10-15 Scored from commit: None\n \n-Defining context: DESIGN.md \u00a716.5, \u00a716.10.6; PLANNING_KERNEL_CONTRACT.md \u00a74; `UBU-D0126`, `UBU-D0256`, `UBU-D0283`, `UBU-D0290`, `UBU-D0339`, `UBU-D0358`, `UBU-D0364`, `UBU-D0370`.\n+Defining context: DESIGN.md \u00a716.5, \u00a716.10.6; PLANNING_KERNEL_CONTRACT.md \u00a74; `UBU-D0126`, `UBU-D0256`, `UBU-D0283`, `UBU-D0290`, `UBU-D0339`, `UBU-D0358`, `UBU-D0364`, `UBU-D0370`, `UBU-D0377`.\n \n ### Question\n \n Which compute API should implement the in-process mobile GPU backend while preserving CPU certification, provenance and the existing parity contract?\n \n ### Subquestions\n \n 1. How do Vulkan compute, OpenCL on Adreno, ExecuTorch and NNAPI compare on the actual supported device and driver stack?\n 2. Which candidate passes exact structural/hard-constraint parity and the named numeric tolerances against the CPU reference path?\n 3. What latency, memory, energy and sustained thermal behavior does each exhibit for deterministic skeletonization, hard checks, local repair recipes and a short-horizon branch cache?\n 4. Which APIs and operations are actually available on the selected LineageOS build, and what CPU fallback is required?\n \n 5. **Scope answered by `UBU-D0358`.** Retain `UBU-D0290`'s stewardship planning workload for the primary phone capture/review client; full mobile search is not required. Measure contention with the intended bounded camera/voice route before selecting an accelerator.\n 6. **Existing release gates.** `UBU-D0339` resolved `UBU-Q0183`'s capture/device allocation and sync sequence. Local perception, voice, battery and privacy qualification follow DESIGN.md \u00a74.2, \u00a712.2, \u00a721.1 and \u00a71; `UBU-Q0168`/`UBU-Q0177` retain measured profiles. Planning parity and on-device CPU certification remain independent requirements.\n \n ### Current direction\n \n-`UBU-D0358` retains mobile stewardship and separates planning API selection from the phone interaction slice already allocated by `UBU-D0339`. DESIGN.md \u00a716.10.6 requires a frozen same-device CPU comparison and candidate execution/evidence matrix, including actual accelerator use, parity, certified-result latency, aggregate resources, sustained thermals, concurrent capture/voice load and fallback. `UBU-D0364` adds matched workload/cache conditions and complete trial accounting, separating accelerated results from full CPU fallback and missing outcomes. `UBU-D0370` adds exact structural lowering and representation-boundary fixtures to the candidate mappings, with explicit CPU routing for unsupported ranges. No broader planning workload or compute API is selected; perception-model suitability needs its own evidence.\n+`UBU-D0358` retains mobile stewardship and separates planning API selection from the phone interaction slice already allocated by `UBU-D0339`. DESIGN.md \u00a716.10.6 requires a frozen same-device CPU comparison and candidate execution/evidence matrix, including actual accelerator use, parity, certified-result latency, aggregate resources, sustained thermals, concurrent capture/voice load and fallback. `UBU-D0364` adds matched workload/cache conditions and complete trial accounting, separating accelerated results from full CPU fallback and missing outcomes. `UBU-D0370` adds exact structural lowering and representation-boundary fixtures to the candidate mappings, with explicit CPU routing for unsupported ranges. `UBU-D0377` adds in-process attempt invalidation, buffer ownership and residual-work accounting to timeout/fallback trials. No broader planning workload or compute API is selected; perception-model suitability needs its own evidence.\n \n No compute API is selected. `UBU-Q0156` settled the desktop invocation boundary; it does not select a mobile backend. This question is blocked on the on-device parity harness producing measurements. The gate is satisfied only once the stewardship workload has been measured against the CPU reference path on physically connected real hardware under the `UBU-D0283` parity rule. Emulator timings and desktop full-search benchmarks do not answer it.\n \n ### Resolution\n \n-Partially answered by `UBU-D0358`: retain stewardship planning, apply the existing capture/release allocation and specify a reproducible comparison. `UBU-D0364` adds matched trials and complete outcome accounting; `UBU-D0370` specifies exact representation and arithmetic evidence. API selection remains Open pending physically connected device measurements and operator review; no hardware result or passing API is inferred from this protocol, API availability or CPU fallback.\n+Partially answered by `UBU-D0358`: retain stewardship planning, apply the existing capture/release allocation and specify a reproducible comparison. `UBU-D0364` adds matched trials and complete outcome accounting; `UBU-D0370` specifies exact representation and arithmetic evidence. `UBU-D0377` specifies in-process failure ownership and its evidence. API selection remains Open pending physically connected device measurements and operator review; no hardware result or passing API is inferred from this protocol, API availability or CPU fallback.\n \n ---\n",
    "commit_message": "docs: define mobile GPU timeout and buffer ownership evidence",
    "validation_notes": [
      "Generated from current working-tree files and proposed in-memory contents using git diff --no-index; normalized destination headers to repository paths.",
      "git apply --check --whitespace=error succeeded against the current working tree.",
      "Verified hunk counts, unchanged context and a question hunk scoped exclusively to UBU-Q0160. Its single-line Open metadata is preserved.",
      "Verified UBU-D0377 is unused in the baseline and only declared allowlisted files change.",
      "Repository files remain unchanged. No device commands or hardware measurements were run."
    ],
    "new_questions_added": [],
    "questions_resolved": [],
    "decisions_added": [
      "UBU-D0377"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "openai-ubu-q0160-in-process-failure-ownership",
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
