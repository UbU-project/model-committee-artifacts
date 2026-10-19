# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `anthropic`

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
    "proposal_id": "ubu-q0160-mobile-representation-pack-d0387",
    "provider_id": "anthropic",
    "model_name": "claude-fable-5-1",
    "question_id": "UBU-Q0160",
    "base_commit": "34ebafbf3a85d4f691205331a823ab0eefe838c9",
    "summary": "Adds UBU-D0387, which freezes `mobile-representation-pack/1`: the second concrete fixture pack for the mobile parity harness, materializing UBU-D0370's representation and range cases beside UBU-D0383's structural pack. It replays the structural pack's endpoint-equality and anchor/overlap cases at four frozen window starts (the 2026 origin, a 1970 origin-near control, and 2038-01-19T03:00:00Z and 2106-02-07T06:10:00Z whose compared placements contain Unix seconds 2^31 and 2^32), adds slot/batch-position boundary cases under padding, and defines declared-bound chains one planning delta below, at and above each candidate mapping's own bound with mandatory CPU routing above it. DESIGN.md \u00a716.10.6 gains the concrete table and freeze rules; UBU-Q0160 stays Open with its context, direction and resolution updated. No API, representation width, timing result or schema change is selected.",
    "rationale": "UBU-Q0160 is blocked on physically connected device measurements, so the useful work is narrowing the harness so trials can be frozen and run. UBU-D0383 made the skeleton/hard-check slice concrete but left UBU-D0370's representation/range cases abstract: candidates must freeze dtypes, origins and ranges, yet no shared fixture states which absolute magnitudes and boundaries to probe or what results are expected. A lossy time lowering (single-precision absolute seconds, 32-bit wrap, mask off-by-one at slot 255 or the last batch position) is the most likely way an accelerator silently changes a hard-constraint decision while CPU certification hides the disagreement. This pack fixes concrete, independently checkable values: window starts chosen so the structural pack's 60-second separations fall where single-precision seconds are spaced 128 apart and where signed/unsigned 32-bit seconds wrap, a near-origin control that separates magnitude failures from wrong relative rules, index boundaries at Task slot 255 and the declared maximum candidate batch, and parametric declared-bound chains whose above-bound case must route to CPU stewardship with the unchanged request and seed under UBU-D0364 accounting. Expected results are reused from the already-frozen M2/M5/M6 semantics, so no new planning semantics are introduced and no representation width is selected; any mapping preserving exact second-resolution semantics across its declared range passes. This is a minimal, auditable addition consistent with UBU-D0358/D0364/D0370/D0383 and leaves the repair/branch-cache, numerical, failure-ownership and sustained-trial gates and the API selection itself untouched.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -4130,4 +4130,16 @@\n Retain the complete repair/cache, representation-boundary, numerical, failure, resource, contention and sustained-trial requirements. This selects initial experiment inputs, not a complete harness, passing parity result, numerical acceptance limit or compute API. No planning schema or release/switch criterion changes; `UBU-Q0160` remains Open pending physically connected device measurements and operator review.\n \n ---\n+\n+## UBU-D0387: Mobile parity representation cases use frozen window starts and declared bounds\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a716.10.6. Narrows `UBU-Q0160`'s harness evidence by materializing `UBU-D0370` beside `UBU-D0383`; API selection remains Open.\n+\n+Adopt `mobile-representation-pack/1` as the second synthetic fixture pack for the mobile parity harness. It replays `mobile-structural-pack/1`'s endpoint-equality and anchor/overlap cases with every absolute time translated to four frozen window starts: the structural pack's 2026 origin, 1970-01-02 as the origin-near control, and 2038-01-19T03:00:00Z and 2106-02-07T06:10:00Z, chosen so that the compared placements contain Unix seconds 2^31 and 2^32. Feasibility results and 60-second separations must be identical at every window start, and readback must match to the second. Slot and batch cases place the known valid and invalid candidates at the first and last Task slots and candidate positions under padding. Declared-bound cases build dependency chains whose final offsets lie one planning delta below, at and above each mapping's own declared range or arithmetic bound; the case above the bound must route to CPU stewardship with the unchanged request and seed.\n+\n+DESIGN.md \u00a716.10.6 fixes the concrete values, expected structural results and freeze rules. Expected values are integers and contract timestamps independent of any encoder, checked against the untouched CPU reference before goldens are frozen; accelerator output is compared before CPU certification, and every offered case retains its actual accelerated or CPU disposition. The chosen magnitudes make common lossy lowerings observable without selecting a representation width: any mapping that preserves exact second-resolution semantics across its declared range passes, and a mapping that fails at its own declared boundary is withheld.\n+\n+This selects experiment inputs only. It establishes no fixture file, timing result, passing harness, numerical tolerance, representation width, compute API or schema change. The repair/branch-cache workload, named numerical checks, failure-ownership and sustained/concurrent requirements remain required; `UBU-Q0160` remains Open pending physically connected device measurements and operator review.\n+\n+---\n \ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -2660,5 +2660,15 @@\n \n Freeze repetitions, order and cold/warm conditions under `UBU-D0364`, retaining every offered case and actual accelerated/CPU disposition. This initial pack supplies no timing result or complete qualification: all five repair triggers, branch-cache behavior, `UBU-D0370`'s representation/range cases, named numerical checks, failure ownership and the existing sustained/concurrent resource comparisons remain required. No fixture file, passing harness or API is established by this specification.\n \n+**Representation-boundary fixture pack (`UBU-D0387`).** Use `mobile-representation-pack/1` to materialize the representation and range cases above. Reuse `mobile-structural-pack/1`'s conventions: contract UTC timestamps, `planning_delta_seconds = 60`, `rng_seed = 160`, atomic Tasks with fixed durations, pinned valid synthetic inputs for every other required field, and explicit candidate schedules fed through the existing hard-check/stage boundary. Offsets below are half-open second offsets from the named window start; one planning delta is the separation unit, and readback is compared at the contract's one-second timestamp resolution. Each row is an independent fixture family whose exact Task ids, topological order, masks, stage inputs and expected values are frozen before trials. Unix seconds are given only to make the chosen magnitudes checkable; fixtures carry contract timestamps.\n+\n+| Case | Concrete input | Required structural result |\n+| --- | --- | --- |\n+| P1: translated endpoint pairs | Replay M2's three B placements and M5's three anchor/overlap checks with every absolute time translated so that the [0,3600) window starts at W0 = 2026-10-16T09:00:00Z (Unix second 1792141200, the structural pack's T), W1 = 2038-01-19T03:00:00Z (2147482800), W2 = 2106-02-07T06:10:00Z (4294966200) and W3 = 1970-01-02T00:00:00Z (86400). Every B placement at W1 contains second 2147483648 and at W2 contains second 4294967296; every absolute second at W0 exceeds 2^30, where single-precision seconds are spaced 128 apart. | Results are identical at all four window starts: M2 gives dependency/hard-constraint feasibility false/false, true/true and true/true, and M5 gives true/false, true/false and true/true. Decoded/readback timestamps and durations equal the inputs to the second. A result that varies with the window start fails, whether by wrap, saturation or collapse of the 60-second separation; W3 distinguishes a magnitude-dependent failure from a wrong relative rule. |\n+| P2: slot and batch boundaries | Place M2's equality candidate with A in Task slot 0 and B in slot 255 of a 256-slot mask, then exchange the slots; slots 1\u2013254 are inactive under false masks and carry overlapping [0,3600) intervals. Separately place M6's valid and invalid candidates at the first and last positions of the frozen mapping's declared maximum candidate batch, padding the remaining positions the same way, then exchange them. | Masks, both feasibility fields, rejection classes and readback slot indices equal the unpadded `mobile-structural-pack/1` results. No padded slot or position contributes an interval, result or rejection, and the invalid candidate cannot abort or contaminate the valid one at either position. |\n+| P3: declared bounds | For each frozen mapping's declared supported range or intermediate arithmetic bound on offsets, durations and their sums, in its declared unit, build a dependency chain of at most 256 atomic Tasks with fixed durations that are multiples of the planning delta, placed back to back from offset 0 in a window just long enough, so that the chain's final end offset is one planning delta below the bound, exactly at the bound and one planning delta above it. Translate each request to W3 as its control. Record a bound unreachable by any contract-valid request with that justification instead of a fixture. | The CPU reference reports the same feasible chain for all three requests and their controls. The candidate executes the first two with exact parity and routes the third to CPU stewardship with the unchanged request and seed under the existing budget and fallback rules, recorded as CPU fallback in `UBU-D0364`'s accounting rather than as a semantic rejection, silent overflow or truncation. A mapping that fails at its own declared boundary is withheld. |\n+\n+P1 exercises translation and magnitude, P2 indexing and P3 each mapping's own limits, so P3's concrete windows are frozen with the mapping under `UBU-D0364`'s manifest before acceptance runs. W0, W1 and W2 place the compared seconds beyond 2^30 and across 2^31 and 2^32, making common lossy lowerings observable without selecting a representation width: any mapping that preserves exact second-resolution interval semantics across its declared range passes. Expected values are integers and contract timestamps independent of any candidate's encoder or decoder; check them against the untouched CPU reference before freezing goldens and diagnose a discrepancy before acceptance. Compare accelerator output before CPU certification and retain every offered case with its actual accelerated or CPU disposition. This pack supplies no timing result, passing configuration, numerical tolerance or API; the repair/branch-cache workload, named numerical checks, failure ownership and the sustained/concurrent resource comparisons remain required.\n+\n | Evidence | Required comparison |\n | --- | --- |\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -5588,7 +5588,7 @@\n ## UBU-Q0160: Which mobile compute API should the mobile GPU backend target?\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0156 Blocks: mobile GPU backend selection Resolved by: None Last scored: 2026-10-19 Scored from commit: None\n \n-Defining context: DESIGN.md \u00a716.5, \u00a716.10.6; PLANNING_KERNEL_CONTRACT.md \u00a74; `UBU-D0126`, `UBU-D0256`, `UBU-D0283`, `UBU-D0290`, `UBU-D0339`, `UBU-D0358`, `UBU-D0364`, `UBU-D0370`, `UBU-D0377`.\n+Defining context: DESIGN.md \u00a716.5, \u00a716.10.6; PLANNING_KERNEL_CONTRACT.md \u00a74; `UBU-D0126`, `UBU-D0256`, `UBU-D0283`, `UBU-D0290`, `UBU-D0339`, `UBU-D0358`, `UBU-D0364`, `UBU-D0370`, `UBU-D0377`, `UBU-D0383`, `UBU-D0387`.\n \n ### Question\n@@ -5608,11 +5608,11 @@\n ### Current direction\n \n-`UBU-D0358` retains mobile stewardship and separates planning API selection from the phone interaction slice already allocated by `UBU-D0339`. DESIGN.md \u00a716.10.6 requires a frozen same-device CPU comparison and candidate execution/evidence matrix, including actual accelerator use, parity, certified-result latency, aggregate resources, sustained thermals, concurrent capture/voice load and fallback. `UBU-D0364` adds matched workload/cache conditions and complete trial accounting, separating accelerated results from full CPU fallback and missing outcomes. `UBU-D0370` adds exact structural lowering and representation-boundary fixtures to the candidate mappings, with explicit CPU routing for unsupported ranges. `UBU-D0377` adds in-process attempt invalidation, buffer ownership and residual-work accounting to timeout/fallback trials. No broader planning workload or compute API is selected; perception-model suitability needs its own evidence.\n+`UBU-D0358` retains mobile stewardship and separates planning API selection from the phone interaction slice already allocated by `UBU-D0339`. DESIGN.md \u00a716.10.6 requires a frozen same-device CPU comparison and candidate execution/evidence matrix, including actual accelerator use, parity, certified-result latency, aggregate resources, sustained thermals, concurrent capture/voice load and fallback. `UBU-D0364` adds matched workload/cache conditions and complete trial accounting, separating accelerated results from full CPU fallback and missing outcomes. `UBU-D0370` adds exact structural lowering and representation-boundary fixtures to the candidate mappings, with explicit CPU routing for unsupported ranges. `UBU-D0377` adds in-process attempt invalidation, buffer ownership and residual-work accounting to timeout/fallback trials. `UBU-D0383` and `UBU-D0387` freeze the first concrete fixture packs: structural skeleton/hard-check cases, and representation cases at frozen window starts, slot/batch boundaries and each mapping's declared bounds. No broader planning workload or compute API is selected; perception-model suitability needs its own evidence.\n \n No compute API is selected. `UBU-Q0156` settled the desktop invocation boundary; it does not select a mobile backend. This question is blocked on the on-device parity harness producing measurements. The gate is satisfied only once the stewardship workload has been measured against the CPU reference path on physically connected real hardware under the `UBU-D0283` parity rule. Emulator timings and desktop full-search benchmarks do not answer it.\n \n ### Resolution\n \n-Partially answered by `UBU-D0358`: retain stewardship planning, apply the existing capture/release allocation and specify a reproducible comparison. `UBU-D0364` adds matched trials and complete outcome accounting; `UBU-D0370` specifies exact representation and arithmetic evidence. `UBU-D0377` specifies in-process failure ownership and its evidence. `UBU-D0383` supplies the first concrete structural fixture pack; the remaining workload and measurement gates still apply. API selection remains Open pending physically connected device measurements and operator review; no hardware result or passing API is inferred from this protocol, API availability or CPU fallback.\n+Partially answered by `UBU-D0358`: retain stewardship planning, apply the existing capture/release allocation and specify a reproducible comparison. `UBU-D0364` adds matched trials and complete outcome accounting; `UBU-D0370` specifies exact representation and arithmetic evidence. `UBU-D0377` specifies in-process failure ownership and its evidence. `UBU-D0383` supplies the first concrete structural fixture pack and `UBU-D0387` the representation-boundary pack that materializes `UBU-D0370`'s cases; the repair/branch-cache workload, numerical, failure-ownership and measurement gates still apply. API selection remains Open pending physically connected device measurements and operator review; no hardware result or passing API is inferred from this protocol, API availability or CPU fallback.\n \n ---\n",
    "commit_message": "UBU-D0387 for mobile representation-boundary fixtures",
    "validation_notes": [
      "Bash, Read and Grep were disabled in this session, so `git apply --check` could not be run; all removed and unchanged lines are quoted verbatim from the working-tree excerpts supplied with the request (DESIGN.md \u00a716.10.6 UBU-D0383 block and Evidence table header, DECISIONS.md UBU-D0383 block, OPEN_QUESTIONS.md UBU-Q0160 block).",
      "Hunk line-number starts are estimates because the files could not be read; hunk line counts are exact (DECISIONS.md -4/+16, DESIGN.md -5/+15, OPEN_QUESTIONS.md -7/+7 and -11/+11). `git apply` locates hunks by context and reports an offset when the start differs; please confirm with `git apply --check` before merge.",
      "The DECISIONS.md entry is inserted immediately after UBU-D0383, the last decision visible in the supplied context, with one blank trailing context line assumed after its closing `---`. UBU-D0384 to UBU-D0386 exist but were not visible; if DECISIONS.md must stay in numeric order, move the UBU-D0387 block to the end of the file after UBU-D0386 at merge.",
      "Arithmetic checked: 2026-10-16T09:00:00Z = Unix 1792141200 (> 2^30 = 1073741824, single-precision ulp 128 s); 2038-01-19T03:00:00Z = 2147482800 = 2^31 - 848, so offsets 540-1260 contain 2^31; 2106-02-07T06:10:00Z = 4294966200 = 2^32 - 1096, so offsets 540-1260 contain 2^32; 1970-01-02T00:00:00Z = 86400.",
      "Expected structural results reuse the already-frozen M2 (false/false, true/true, true/true), M5 (true/false, true/false, true/true) and M6 semantics from UBU-D0383; no new planning semantics, schema field, provenance member, tolerance value, representation width or compute API is introduced.",
      "UBU-Q0160 remains `Status: Open`; only its Defining context, Current direction and Resolution text are updated. Metadata line, scores and phase are unchanged. PLANNING_KERNEL_CONTRACT.md is untouched.",
      "P3 is parametric on each candidate mapping's declared bound, consistent with UBU-D0370 and UBU-D0364's manifest; it includes an explicit disposition for bounds unreachable by any contract-valid request so an unbounded 64-bit mapping is recorded rather than failed."
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
    "proposal_id": "ubu-q0160-mobile-representation-pack-d0387",
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
