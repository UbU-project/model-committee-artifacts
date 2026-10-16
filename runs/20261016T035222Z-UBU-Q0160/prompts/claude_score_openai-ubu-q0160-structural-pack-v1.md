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
Base commit: `8e1d0a7905a856ceddc9ac463ba1fe949275a68c`

```markdown
## UBU-Q0160: Which mobile compute API should the mobile GPU backend target?

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0156 Blocks: mobile GPU backend selection Resolved by: None Last scored: 2026-10-15 Scored from commit: None

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

Partially answered by `UBU-D0358`: retain stewardship planning, apply the existing capture/release allocation and specify a reproducible comparison. `UBU-D0364` adds matched trials and complete outcome accounting; `UBU-D0370` specifies exact representation and arithmetic evidence. `UBU-D0377` specifies in-process failure ownership and its evidence. API selection remains Open pending physically connected device measurements and operator review; no hardware result or passing API is inferred from this protocol, API availability or CPU fallback.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "openai-ubu-q0160-structural-pack-v1",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0160",
    "base_commit": "8e1d0a7905a856ceddc9ac463ba1fe949275a68c",
    "summary": "Specify concrete synthetic fixtures for the first mobile skeletonization and hard-check parity slice.",
    "rationale": "API selection requires physical-device evidence that is not supplied here. Concrete inputs and independently checked expected structural outputs make the first harness slice implementable while preserving the existing selection gates and the question's Open status.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5906,3 +5906,15 @@ Allocate the existing 30 cold/100 warm ordinary trials across the four cases and\n Retain the existing scenario-specific budgets, period demand schedule, sustained sessions, setup/upkeep evidence and mandatory security/retention gates. Publish case-level evidence in the existing cost/support matrix. This selects reproducible experiment inputs, not hardware, numerical acceptance limits, prices, passing results or an affordability claim; `UBU-Q0178` remains Open pending operator-reviewed real-device comparison and selection.\n \n ---\n+\n+## UBU-D0383: First mobile parity slice uses concrete structural fixtures\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a716.10.6. Narrows `UBU-Q0160`'s first harness slice; API selection remains Open.\n+\n+Adopt `mobile-structural-pack/1` as the first synthetic skeletonization and hard-check fixture pack. DESIGN.md \u00a716.10.6 specifies forced placements, dependency endpoint comparisons, missing-prerequisite cases, containment, Static anchors, overlap and mixed masked batches with explicit expected structural results. Materialize these meanings through the existing planning and internal stage boundaries; invalid candidate probes are validator inputs, not malformed requests or user-visible Plans.\n+\n+Freeze exact serialized inputs, independently checked expected values and pinned CPU outputs before candidate trials. Compare each candidate's outputs before CPU certification, using the untouched CPU request and reference path rather than a shared accelerator lowering as the oracle. A reference disagreement requires diagnosis before acceptance; it cannot redefine the stated semantics.\n+\n+Retain the complete repair/cache, representation-boundary, numerical, failure, resource, contention and sustained-trial requirements. This selects initial experiment inputs, not a complete harness, passing parity result, numerical acceptance limit or compute API. No planning schema or release/switch criterion changes; `UBU-Q0160` remains Open pending physically connected device measurements and operator review.\n+\n+---\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -2933,6 +2933,21 @@ Publish one evidence row for the CPU baseline and each candidate configuration o\n \n Include shared synthetic fixtures with large absolute timestamps separated by one reference time unit; paired fixtures translating all relevant absolute times by a common offset while preserving relative constraints; endpoint comparisons immediately before, at and after equality; Task/branch index and padding boundaries; and duration sums near the declared arithmetic limits. Compare decoded/readback values and existing exact-parity outputs against the CPU reference, normalizing only the declared time origin for translated pairs. Exercise both supported-boundary inputs and otherwise valid inputs outside the accelerator's bounds, with the latter demonstrating explicit CPU routing. Freeze expected dispositions with `UBU-D0364`'s manifest and account for fallback separately. Run the claimed accelerated operations on the physical device and retain policy-permitted input, readback and comparison evidence. A numeric score tolerance cannot excuse a different exact structural or hard-constraint result, and CPU certification cannot repair a failed parity comparison. These fixtures select no representation width, API or passing backend.\n \n+**First structural fixture pack (`UBU-D0383`).** Use `mobile-structural-pack/1` for the first skeletonization/hard-check harness slice. Let `T = 2026-10-16T09:00:00Z`; intervals below are half-open second offsets from T, materialized as contract UTC timestamps. Use time_window [T,T+3600), `planning_delta_seconds = 60`, `rng_seed = 160` and atomic Tasks with fixed 600-second durations. Supply all other required fields with pinned valid synthetic inputs. Each row is an independent fixture family; freeze its exact Task ids, CPU-provided topological order, masks, stage inputs and expected outputs before trials.\n+\n+| Case | Concrete input | Required structural result |\n+| --- | --- | --- |\n+| M1: forced skeleton | A and B each last 600 seconds, with Task windows [0,600) and [600,1200), and dependency A \u2192 B. Protected capacity-occupying Static S is anchored at 1200 for 600 seconds. | Placements A [0,600), B [600,1200), S [1200,1800); free chunks [0,1200) and [1800,3600). Dependency and hard-constraint feasibility are both true. |\n+| M2: dependency endpoints | No Static Task; A and B each last 600 seconds with windows [0,1800), A \u2192 B and A placed [0,600). Validate B at [540,1140), [600,1200) and [660,1260). | Dependency/hard-constraint feasibility are false/false, true/true and true/true respectively. Equality permits touching endpoints. |\n+| M3: missing prerequisites | From M2's equality case, omit A while retaining B. Separately add C \u2192 B and retain A/B while omitting C. In a third request with only A \u2192 B, omit that entire optional chain and retain independent C [0,600). | First two candidates: both feasibility fields false and the existing missing-prerequisite rejection class. Third: both true. Missing prerequisites produce no invented completion/slack or lookup failure. |\n+| M4: containment | M2's windows and dependency, A [0,600), but B [1200,1800), outside B's narrowed Task window [600,1200). | Dependency feasibility true; hard-constraint feasibility false. |\n+| M5: Static anchor and overlap | Independently validate S anchored at 1200 for 600 seconds but placed [1260,1860), within a broad [0,3600) window. Separately validate correctly anchored S with independent C [1140,1740), then C [600,1200); C's window is [0,3600). | With no dependency edges, dependency feasibility is true throughout. Hard-constraint feasibility is false for the shifted anchor, false for overlap and true for touching endpoints. |\n+| M6: mixed batch and padding | Batch M2's equality candidate with M3's missing-A candidate. Repeat with inactive padded slots containing overlapping [0,3600) intervals under false masks. | Preserve each candidate's exact masks, feasibility and rejection result; padding changes none. The invalid candidate cannot abort or contaminate the valid one. |\n+\n+M1 exercises construction; M2\u2013M6 feed explicit candidate schedules through the existing hard-check/stage boundary. Do not ask candidate search to generate a malformed request or count CPU rejection of an incorrect accelerator output as a pass. Record exact schedules, chunks, masks, both feasibility fields and rejection classes where applicable. Check the stated outcomes against the untouched CPU reference before freezing goldens; diagnose a fixture/reference discrepancy before acceptance instead of blessing the observed result. Keep expected timestamp/integer values independent of the candidate's encoder/decoder and compare accelerator output before CPU certification.\n+\n+Freeze repetitions, order and cold/warm conditions under `UBU-D0364`, retaining every offered case and actual accelerated/CPU disposition. This initial pack supplies no timing result or complete qualification: all five repair triggers, branch-cache behavior, `UBU-D0370`'s representation/range cases, named numerical checks, failure ownership and the existing sustained/concurrent resource comparisons remain required. No fixture file, passing harness or API is established by this specification.\n+\n | Evidence | Required comparison |\n | --- | --- |\n | Semantic parity | Replay the same bounded skeleton, hard-check, repair and branch-cache fixtures against the CPU reference. Include \u00a75 of PLANNING_KERNEL_CONTRACT.md's missing-prerequisite and endpoint-equality cases, protected Tasks, stale envelopes, all five \u00a716.5 repair triggers, and cache hits/misses. Enumerate applicable stage outputs and justify any non-applicable stage; record exact structural/feasibility/rejection comparisons and named numeric/statistical checks before CPU certification. CPU rejection cannot turn an accelerator disagreement into a parity pass. |\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3001,31 +3001,31 @@\n ## UBU-Q0160: Which mobile compute API should the mobile GPU backend target?\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0156 Blocks: mobile GPU backend selection Resolved by: None Last scored: 2026-10-15 Scored from commit: None\n \n Defining context: DESIGN.md \u00a716.5, \u00a716.10.6; PLANNING_KERNEL_CONTRACT.md \u00a74; `UBU-D0126`, `UBU-D0256`, `UBU-D0283`, `UBU-D0290`, `UBU-D0339`, `UBU-D0358`, `UBU-D0364`, `UBU-D0370`, `UBU-D0377`.\n \n ### Question\n \n Which compute API should implement the in-process mobile GPU backend while preserving CPU certification, provenance and the existing parity contract?\n \n ### Subquestions\n \n 1. How do Vulkan compute, OpenCL on Adreno, ExecuTorch and NNAPI compare on the actual supported device and driver stack?\n 2. Which candidate passes exact structural/hard-constraint parity and the named numeric tolerances against the CPU reference path?\n 3. What latency, memory, energy and sustained thermal behavior does each exhibit for deterministic skeletonization, hard checks, local repair recipes and a short-horizon branch cache?\n 4. Which APIs and operations are actually available on the selected LineageOS build, and what CPU fallback is required?\n \n 5. **Scope answered by `UBU-D0358`.** Retain `UBU-D0290`'s stewardship planning workload for the primary phone capture/review client; full mobile search is not required. Measure contention with the intended bounded camera/voice route before selecting an accelerator.\n 6. **Existing release gates.** `UBU-D0339` resolved `UBU-Q0183`'s capture/device allocation and sync sequence. Local perception, voice, battery and privacy qualification follow DESIGN.md \u00a74.2, \u00a712.2, \u00a721.1 and \u00a71; `UBU-Q0168`/`UBU-Q0177` retain measured profiles. Planning parity and on-device CPU certification remain independent requirements.\n \n ### Current direction\n \n `UBU-D0358` retains mobile stewardship and separates planning API selection from the phone interaction slice already allocated by `UBU-D0339`. DESIGN.md \u00a716.10.6 requires a frozen same-device CPU comparison and candidate execution/evidence matrix, including actual accelerator use, parity, certified-result latency, aggregate resources, sustained thermals, concurrent capture/voice load and fallback. `UBU-D0364` adds matched workload/cache conditions and complete trial accounting, separating accelerated results from full CPU fallback and missing outcomes. `UBU-D0370` adds exact structural lowering and representation-boundary fixtures to the candidate mappings, with explicit CPU routing for unsupported ranges. `UBU-D0377` adds in-process attempt invalidation, buffer ownership and residual-work accounting to timeout/fallback trials. No broader planning workload or compute API is selected; perception-model suitability needs its own evidence.\n \n No compute API is selected. `UBU-Q0156` settled the desktop invocation boundary; it does not select a mobile backend. This question is blocked on the on-device parity harness producing measurements. The gate is satisfied only once the stewardship workload has been measured against the CPU reference path on physically connected real hardware under the `UBU-D0283` parity rule. Emulator timings and desktop full-search benchmarks do not answer it.\n \n ### Resolution\n \n-Partially answered by `UBU-D0358`: retain stewardship planning, apply the existing capture/release allocation and specify a reproducible comparison. `UBU-D0364` adds matched trials and complete outcome accounting; `UBU-D0370` specifies exact representation and arithmetic evidence. `UBU-D0377` specifies in-process failure ownership and its evidence. API selection remains Open pending physically connected device measurements and operator review; no hardware result or passing API is inferred from this protocol, API availability or CPU fallback.\n+Partially answered by `UBU-D0358`: retain stewardship planning, apply the existing capture/release allocation and specify a reproducible comparison. `UBU-D0364` adds matched trials and complete outcome accounting; `UBU-D0370` specifies exact representation and arithmetic evidence. `UBU-D0377` specifies in-process failure ownership and its evidence. `UBU-D0383` supplies the first concrete structural fixture pack; the remaining workload and measurement gates still apply. API selection remains Open pending physically connected device measurements and operator review; no hardware result or passing API is inferred from this protocol, API availability or CPU fallback.\n \n ---\n",
    "commit_message": "docs(planning): specify first mobile structural parity fixtures",
    "validation_notes": [
      "Generated hunks with git diff --no-index against unchanged working-tree files and proposed contents on stdin; normalized paths and limited question context to UBU-Q0160.",
      "git apply --check passed against the current working tree; repository files were not modified.",
      "Preserved UBU-Q0160's single-line metadata and Open status; its resolution hunk includes the selected question heading and its own Resolution section.",
      "Checked fixture expectations against PLANNING_KERNEL_CONTRACT.md \u00a75; no device commands, harness execution, API availability checks or performance measurements were performed."
    ],
    "new_questions_added": [],
    "questions_resolved": [],
    "decisions_added": [
      "UBU-D0383"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "openai-ubu-q0160-structural-pack-v1",
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
