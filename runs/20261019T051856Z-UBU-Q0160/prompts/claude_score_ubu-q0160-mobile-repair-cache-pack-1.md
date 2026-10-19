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
Base commit: `34ebafbf3a85d4f691205331a823ab0eefe838c9`

```markdown
## UBU-Q0160: Which mobile compute API should the mobile GPU backend target?

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0156 Blocks: mobile GPU backend selection Resolved by: None Last scored: 2026-10-19 Scored from commit: None

Defining context: DESIGN.md §16.5, §16.10.6; PLANNING_KERNEL_CONTRACT.md §4; `UBU-D0126`, `UBU-D0256`, `UBU-D0283`, `UBU-D0290`, `UBU-D0339`, `UBU-D0358`, `UBU-D0364`, `UBU-D0370`, `UBU-D0377`.

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

`UBU-D0358` retains mobile stewardship and separates planning API selection from the phone interaction slice already allocated by `UBU-D0339`. DESIGN.md §16.10.6 requires a frozen same-device CPU comparison and candidate execution/evidence matrix, including actual accelerator use, parity, certified-result latency, aggregate resources, sustained thermals, concurrent capture/voice load and fallback. `UBU-D0364` adds matched workload/cache conditions and complete trial accounting, separating accelerated results from full CPU fallback and missing outcomes. `UBU-D0370` adds exact structural lowering and representation-boundary fixtures to the candidate mappings, with explicit CPU routing for unsupported ranges. `UBU-D0377` adds in-process attempt invalidation, buffer ownership and residual-work accounting to timeout/fallback trials. No broader planning workload or compute API is selected; perception-model suitability needs its own evidence.

No compute API is selected. `UBU-Q0156` settled the desktop invocation boundary; it does not select a mobile backend. This question is blocked on the on-device parity harness producing measurements. The gate is satisfied only once the stewardship workload has been measured against the CPU reference path on physically connected real hardware under the `UBU-D0283` parity rule. Emulator timings and desktop full-search benchmarks do not answer it.

### Resolution

Partially answered by `UBU-D0358`: retain stewardship planning, apply the existing capture/release allocation and specify a reproducible comparison. `UBU-D0364` adds matched trials and complete outcome accounting; `UBU-D0370` specifies exact representation and arithmetic evidence. `UBU-D0377` specifies in-process failure ownership and its evidence. `UBU-D0383` supplies the first concrete structural fixture pack; the remaining workload and measurement gates still apply. API selection remains Open pending physically connected device measurements and operator review; no hardware result or passing API is inferred from this protocol, API availability or CPU fallback.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0160-mobile-repair-cache-pack-1",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0160",
    "base_commit": "34ebafbf3a85d4f691205331a823ab0eefe838c9",
    "summary": "Specify concrete repair and branch-cache fixtures for the mobile API comparison; retain measured API selection as Open.",
    "rationale": "UBU-D0383 supplies structural fixtures, while repair and cache workloads remain generic. Explicit recipes, forced placements and invalidation cases give the remaining stewardship comparison concrete inputs without inventing hardware results or choosing an API. Existing CPU certification, parity and measurement gates remain authoritative.",
    "changed_files": [
      "DESIGN.md",
      "DECISIONS.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -2952,6 +2952,22 @@ M1 exercises construction; M2\u2013M6 feed explicit candidate schedules through the\n \n Freeze repetitions, order and cold/warm conditions under `UBU-D0364`, retaining every offered case and actual accelerated/CPU disposition. This initial pack supplies no timing result or complete qualification: all five repair triggers, branch-cache behavior, `UBU-D0370`'s representation/range cases, named numerical checks, failure ownership and the existing sustained/concurrent resource comparisons remain required. No fixture file, passing harness or API is established by this specification.\n \n+**First repair/cache fixture pack (`UBU-D0387`).** Materialize `mobile-repair-cache-pack/1` alongside `mobile-structural-pack/1`. Reuse T, the 3600-second window, 60-second planning delta and seed 160 above; durations are fixed and Tasks atomic. Each row starts from independent synthetic state. For R1\u2013R5, retain protected Static S [1200,1800), with its anchor unchanged; unmentioned dependencies and constraints are absent or satisfied. Repair triggers are already-admitted synthetic inputs supplied by test setup. The recipes below are explicit fixture policies, not general defaults or authority to admit new state.\n+\n+Before trials, pin the existing controller/kernel paths and serialized inputs for prior Plans, accepted evidence, envelopes, recipes and continuation refs. Supply fresh envelopes and sufficient finite repair budgets unless a row specifies otherwise. Freeze exact ids, versions, masks, affect inputs and expected controller/stage outputs, including severity, confirmation and fallback dispositions. A missing implementation or mapping remains an explicit gap; do not invent `PlanningRequest` fields to carry stewardship metadata. Check expected values independently against the untouched CPU reference before freezing goldens, and diagnose disagreements as for `UBU-D0383`.\n+\n+| Case | Concrete input and fixture policy | Required repair/cache result |\n+| --- | --- | --- |\n+| R1: `late_task` | Flexible F was [0,600), is unstarted at effective time T+300, and requires 600 seconds. Its envelope permits occupancy only within [0,900). The recipe shifts F to the current time if it fits. | F becomes [300,900); S is unchanged. No shortening or omission is needed. |\n+| R2: `skipped_task` | Disposable D was [0,600), has no dependents, and has an accepted skip. The recipe permits its omission. Independently repeat with a protected P at that placement and a recipe requiring confirmation before omission. | D is omitted and S retained. The protected variant requests confirmation; it cannot silently adopt an omission, invent completion or treat the skip as approval. |\n+| R3: `fatigue_report` | Recovery-critical protected R is [0,600), disposable D is [600,1200), and the accepted fatigue Snapshot has pinned affect inputs whose independently checked eligibility is R=true and D=false. The recipe permits omitting D, which has no dependents. | Omit only D; preserve R and S and the supplied affect/eligibility results. No inferred Preference or invented state update follows. |\n+| R4: `approaching_static_task` | Disposable D was [900,1200), is unstarted at T+1140, requires 300 seconds and may occupy only [900,1200). The recipe permits omission when it cannot finish before S. No transition/setup Task is modeled. | Omit D; preserve S [1200,1800). Do not truncate D into the remaining 60 seconds, overlap/move S or invent transition/setup work. |\n+| R5: `missing_prerequisite` | Protected B was [0,600), conditioned on admitted fact `facts.project.mobile_fixture.ready = true`. An accepted correction sets it to false; no modeled producer is available. The recipe requires clarification rather than assuming satisfaction. | Block B and surface the missing-prerequisite clarification/limitation; preserve S. No B placement, invented producer or satisfied fact may escape validation. |\n+| C1: matching continuation, hit versus miss | At the chunk boundary T+1800, the observed boundary outcome matches a certified continuation containing only C, fixed at 600 seconds with Task window [1800,2400) and precondition `facts.project.mobile_fixture.ready = true`; the carried fact is true and the envelope is fresh. Compare the exact entry present with an empty cache. The miss path reconstructs this forced placement. | Both paths return C [1800,2400) after current validity checks and on-device CPU certification. Record hit versus reconstruction and their complete costs separately; matching schedules alone cannot prove a cache lookup or accelerated execution. |\n+| C2: invalid continuation | Repeat C1 independently after (a) an accepted correction makes `facts.project.mobile_fixture.ready` false, with no producer, and (b) the cached envelope has `stale_after = T+1740`, with current time T+1800 and otherwise unchanged admitted inputs. Retain the old cache entry in each starting state. | Neither old entry is usable as a hit. In (a), block C and surface clarification/limitation. In (b), use the empty-cache reconstruction path and current inputs to certify C [1800,2400); the expired entry supplies no authority. Distinguish invalidation, reconstruction and certification. |\n+\n+Exercise the actual repair/cache acceptance path and compare applicable accelerator outputs before CPU certification as well as the resulting controller disposition. Preserve accepted inputs and Static anchors, and enforce each row's protected-work disposition; these tests authorize no canonical mutation. Pin and verify the starting entry digests or empty state for each paired run under `UBU-D0364`, retaining all offered requests and actual accelerated/CPU dispositions. A CPU-resident repair or cache hit is useful evidence of that mapping, not proof of GPU work. This pack supplies neither fixture files nor passing results; representation-boundary, numerical, failure-ownership, sustained/resource and concurrent camera/voice evidence remain required. API selection and `UBU-Q0160` remain Open.\n+\n | Evidence | Required comparison |\n | --- | --- |\n | Semantic parity | Replay the same bounded skeleton, hard-check, repair and branch-cache fixtures against the CPU reference. Include \u00a75 of PLANNING_KERNEL_CONTRACT.md's missing-prerequisite and endpoint-equality cases, protected Tasks, stale envelopes, all five \u00a716.5 repair triggers, and cache hits/misses. Enumerate applicable stage outputs and justify any non-applicable stage; record exact structural/feasibility/rejection comparisons and named numeric/statistical checks before CPU certification. CPU rejection cannot turn an accelerator disagreement into a parity pass. |\ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5964,3 +5964,15 @@ The next fixture needs an identified evidence write/read path, exact revision re\n This is source inspection, with no new validator, round-trip, service or exchange execution. `UBU-Q0166` stays Open under `UBU-D0321`'s existing gate. No mapping, repair, governing gap, schema, enum, FactProvenance field, pin, migration or adapter enablement is established.\n \n ---\n+\n+## UBU-D0387: Mobile repair and branch-cache trials use concrete stewardship fixtures\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a716.10.6. Narrows `UBU-Q0160`'s remaining workload fixtures; extends `UBU-D0383` without selecting an API.\n+\n+Adopt `mobile-repair-cache-pack/1` alongside the structural pack. Its independent synthetic cases cover all five \u00a716.5 repair triggers, protected-skip confirmation, matched continuation hits and reconstruction, changed admitted state and expired envelopes. Explicit fixture recipes and forced placements make expected changes inspectable without selecting general repair defaults. Pin actual host/controller mappings and exact serialized inputs before trials; an absent path remains an implementation gap rather than an invented planning field.\n+\n+Freeze independently checked expected controller/stage outputs and pinned CPU goldens. Exercise the actual repair/cache acceptance paths, compare applicable accelerator outputs before CPU certification and retain on-device certification before presentation. Match and verify starting cache entries, account for every request and distinguish CPU-resident/cache work from demonstrated accelerated operations under `UBU-D0364`.\n+\n+This supplies concrete experiment cases, not materialized fixtures, a passing harness or device measurements. All existing parity, representation, numerical, failure, resource and contention gates remain. No schema, workload expansion or switch/release criterion changes; `UBU-Q0160` stays Open pending physically connected device measurements and operator review.\n+\n+---\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3001,31 +3001,31 @@\n ## UBU-Q0160: Which mobile compute API should the mobile GPU backend target?\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0156 Blocks: mobile GPU backend selection Resolved by: None Last scored: 2026-10-19 Scored from commit: None\n \n Defining context: DESIGN.md \u00a716.5, \u00a716.10.6; PLANNING_KERNEL_CONTRACT.md \u00a74; `UBU-D0126`, `UBU-D0256`, `UBU-D0283`, `UBU-D0290`, `UBU-D0339`, `UBU-D0358`, `UBU-D0364`, `UBU-D0370`, `UBU-D0377`.\n \n ### Question\n \n Which compute API should implement the in-process mobile GPU backend while preserving CPU certification, provenance and the existing parity contract?\n \n ### Subquestions\n \n 1. How do Vulkan compute, OpenCL on Adreno, ExecuTorch and NNAPI compare on the actual supported device and driver stack?\n 2. Which candidate passes exact structural/hard-constraint parity and the named numeric tolerances against the CPU reference path?\n 3. What latency, memory, energy and sustained thermal behavior does each exhibit for deterministic skeletonization, hard checks, local repair recipes and a short-horizon branch cache?\n 4. Which APIs and operations are actually available on the selected LineageOS build, and what CPU fallback is required?\n \n 5. **Scope answered by `UBU-D0358`.** Retain `UBU-D0290`'s stewardship planning workload for the primary phone capture/review client; full mobile search is not required. Measure contention with the intended bounded camera/voice route before selecting an accelerator.\n 6. **Existing release gates.** `UBU-D0339` resolved `UBU-Q0183`'s capture/device allocation and sync sequence. Local perception, voice, battery and privacy qualification follow DESIGN.md \u00a74.2, \u00a712.2, \u00a721.1 and \u00a71; `UBU-Q0168`/`UBU-Q0177` retain measured profiles. Planning parity and on-device CPU certification remain independent requirements.\n \n ### Current direction\n \n `UBU-D0358` retains mobile stewardship and separates planning API selection from the phone interaction slice already allocated by `UBU-D0339`. DESIGN.md \u00a716.10.6 requires a frozen same-device CPU comparison and candidate execution/evidence matrix, including actual accelerator use, parity, certified-result latency, aggregate resources, sustained thermals, concurrent capture/voice load and fallback. `UBU-D0364` adds matched workload/cache conditions and complete trial accounting, separating accelerated results from full CPU fallback and missing outcomes. `UBU-D0370` adds exact structural lowering and representation-boundary fixtures to the candidate mappings, with explicit CPU routing for unsupported ranges. `UBU-D0377` adds in-process attempt invalidation, buffer ownership and residual-work accounting to timeout/fallback trials. No broader planning workload or compute API is selected; perception-model suitability needs its own evidence.\n \n No compute API is selected. `UBU-Q0156` settled the desktop invocation boundary; it does not select a mobile backend. This question is blocked on the on-device parity harness producing measurements. The gate is satisfied only once the stewardship workload has been measured against the CPU reference path on physically connected real hardware under the `UBU-D0283` parity rule. Emulator timings and desktop full-search benchmarks do not answer it.\n \n ### Resolution\n \n-Partially answered by `UBU-D0358`: retain stewardship planning, apply the existing capture/release allocation and specify a reproducible comparison. `UBU-D0364` adds matched trials and complete outcome accounting; `UBU-D0370` specifies exact representation and arithmetic evidence. `UBU-D0377` specifies in-process failure ownership and its evidence. `UBU-D0383` supplies the first concrete structural fixture pack; the remaining workload and measurement gates still apply. API selection remains Open pending physically connected device measurements and operator review; no hardware result or passing API is inferred from this protocol, API availability or CPU fallback.\n+Partially answered by `UBU-D0358`: retain stewardship planning, apply the existing capture/release allocation and specify a reproducible comparison. `UBU-D0364` adds matched trials and complete outcome accounting; `UBU-D0370` specifies exact representation and arithmetic evidence. `UBU-D0377` specifies in-process failure ownership and its evidence. `UBU-D0383` supplies the first concrete structural fixture pack; `UBU-D0387` adds concrete cases for all five repair triggers and matched, missing or invalid branch-cache entries. Fixture materialization and the remaining measurement gates still apply. API selection remains Open pending physically connected device measurements and operator review; no hardware result or passing API is inferred from this protocol, API availability or CPU fallback.\n \n ---\n",
    "commit_message": "docs: specify mobile repair and branch-cache parity fixtures",
    "validation_notes": [
      "Generated the unified diff from unmodified working-tree text and proposed in-memory contents; repository files were not changed.",
      "git apply --check passed against the current working tree; git apply --check --whitespace=error-all also passed.",
      "Only DESIGN.md, DECISIONS.md and UBU-Q0160 in OPEN_QUESTIONS.md change; the question metadata and surrounding question blocks are preserved.",
      "No fixture implementation, CPU-reference execution or physical-device trial was performed; no API or configuration is qualified."
    ],
    "new_questions_added": [],
    "questions_resolved": [],
    "decisions_added": [
      "UBU-D0387"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0160-mobile-repair-cache-pack-1",
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
