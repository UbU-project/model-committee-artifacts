# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0168`  
Question title: `Snapshot, conversational, streamed and retrospective latency classes`  
Base commit: `f12f2407c8e4c4a5b77a84a1c6a3b6f0e8727a74`

```markdown
## UBU-Q0168: Snapshot, conversational, streamed and retrospective latency classes

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-14 Scored from commit: None

### Question

Which latency classes and degradation behavior are required for snapshots, conversational interaction, streamed supervision and retrospective analysis?

### Current direction

Delta §17.6. `UBU-D0325` defines the four latency classes and deadline, freshness, degradation and interruption semantics in DESIGN.md §21.1.4. Select actual user-visible deadlines, source/context-age limits, stream gap/backlog caps and battery/thermal conditions through revision-pinned local-device evidence. Capture latency remains distinct from planner streaming, admission and proof of execution. No measured performance numbers or supported-hardware claims are established.

### Resolution

Open. `UBU-D0325` answers the class/degradation architecture; numerical profiles and measured acceptance remain unresolved. Close only after evidence for each enabled Device/backend/mode pins implementation and policy revisions, selects finite deadlines and freshness/resource ceilings, and demonstrates the specified ordinary, sustained-load, offline, interruption and stale-result behavior. `UBU-Q0177` retains secure hardware selection, `UBU-Q0128` retains quantitative restricted-attention profiles and `UBU-Q0183` retains delivery sequence. A parameterized contract alone is insufficient to close this question.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0168-deadline-boundary-fixtures",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0168",
    "base_commit": "f12f2407c8e4c4a5b77a84a1c6a3b6f0e8727a74",
    "summary": "Add explicit deadline-boundary, stream-invalidation and interruption-race qualification fixtures while keeping numerical latency acceptance open.",
    "rationale": "UBU-D0325 already defines the four classes. Reproducible boundary fixtures clarify deadline equality, delayed timer callbacks, stream restart eligibility and separate fallback/cancellation bounds. They strengthen measured qualification without inventing performance numbers or treating a parameterized contract as resolution.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\nindex 8f9578c..8abd3b0 100644\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5280,3 +5280,15 @@ Record the synthetic specimen, pinned builds and actual normalization, SQLite cl\n These results disqualify the tested inline mapping through that normalization branch. They establish a current implementation loss, not a governing host-contract gap across all compliant existing payload/ref mappings. A future adapter using this path must demonstrate preservation or leave the interpretation unresolved before executable review; native `ok`, durable candidate storage and API retention cannot substitute for claim/version/profile validation or rendered qualifier review.\n \n `UBU-Q0166` remains Open for complete Discovery claim/evidence mappings, adapter validation, faithful normalization and presentation, admission and authorized-exchange fixtures. Synthetic refs remain unresolved; pool close/reopen is not a process/crash-recovery test, and direct API calls test neither HTTP authorization nor disclosure middleware. No schema, enum, FactProvenance field, pin, migration or producer authority changes here; `UniverseTarget` remains name-only.\n+\n+---\n+\n+## UBU-D0335: Latency qualification includes deadline-boundary and invalidation fixtures\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a721.1.4. Narrows `UBU-Q0168`; numeric profiles and real-device acceptance remain Open.\n+\n+Require reproducible controller fixtures around elapsed deadlines, stream freshness/gap/backlog limits and interruption races. An outstanding elapsed deadline expires at equality; release checks cannot rely on a timely timer callback. Separate per-window stream deadlines from the finite consented session, and require explicit restart/resume and fresh usable output before resumed live guidance.\n+\n+Pin separate fallback, output-quiescence and resource-release bounds. Record policy-minimized expected/observed traces and run the actual controller with deterministic clock/adapter fixtures alongside revision-pinned real-device trials. Failed enforcement cannot be averaged away by a fast median or counted as useful completion.\n+\n+These are qualification requirements, not measured results. `UBU-D0325` retains the four-class contract and `UBU-D0331` the hardware trial protocol. `UBU-Q0168` remains Open until finite numerical profiles and passing local-device evidence are reviewed; no hardware, kernel API or wire-schema change is selected.\ndiff --git a/DESIGN.md b/DESIGN.md\nindex 9fc4836..3f43a40 100644\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -3777,6 +3777,21 @@ Offline operation uses available permitted local backends and manual routes; no\n \n **Interruption.** Pause, stop, silence, cancellation, a superseding turn, source/Compartment or Device change, and consent revocation invalidate the affected pending output/action bindings locally without waiting for a backend acknowledgment. Stop the affected acquisition/output and request cancellation; reject late callbacks for live guidance or dispatch even while the backend finishes. Require bounded cancellation/resource release under its permitted lifecycle; an adapter without that bound cannot enable the class. Preserve already-admitted state, permitted durable sources/drafts and ordinary idempotency outcomes. Interrupted work does not establish an effect or authorize external-action replay; existing outcome/idempotency rules still apply. Reviewing retained candidates later neither resumes capture nor restores approval; restart/handoff requires new local bindings and ordinary review.\n \n+**Deadline-boundary qualification (`UBU-D0335`).** An outstanding elapsed deadline expires when the controller's monotonic time is at or beyond its bound. Recheck time and current generation when releasing live output or dispatching; a delayed timer callback cannot make an expired result eligible. Serialize interruption/invalidation with release: an already-observed cancellation or superseding turn wins over a queued result. Pin separate finite expiry-to-visible-fallback, local-stop-to-output-quiescence and cancellation-to-resource-release ceilings; backend acknowledgment cannot delay local invalidation.\n+\n+For streams, start each permitted observation window's processing clock when that window closes, before queueing, using its mapped controller-monotonic timestamp; an unknown mapping cannot establish live freshness. Bound the acquisition window and consented session separately. Before the first usable update, the first-useful-result deadline bounds waiting. Afterward, only presentation of a fresh, valid usable update resets the update-gap clock; progress, stale results and dropped windows do not. Crossing an age, gap or backlog limit invalidates affected pending live output and withdraws displayed guidance. Resumption needs explicit restart/resume, a fresh generation, current consent and a fresh usable update before showing live guidance.\n+\n+Run the following fixtures against the actual controller with a deterministic clock and delayed/duplicate adapter callbacks, then demonstrate the corresponding behavior in the pinned real-device trials. Place events immediately before, exactly at and immediately after each applicable elapsed deadline; for age/backlog ceilings, exercise both equality and exceedance under the declared profile comparator.\n+\n+| Class | Required boundary fixture | Required disposition |\n+|---|---|---|\n+| `snapshot_capture` | Queueing/acquisition consumes the budget; interpretation or retry returns at/after expiry. | Acquisition ends, late live output rejects, permitted saved input survives and unsaved input is identified; retry cannot renew the original deadline. |\n+| `conversational` | Tokens precede a missed useful-response deadline; silence or a superseding turn races with queued speech/results. | Tokens never satisfy the useful-result gate; old speech/results cannot resume or overwrite the new turn, and the bounded fallback remains available. |\n+| `streamed_supervision` | No first update; then stale/duplicate updates, a gap and queued/in-flight overload, followed by apparent backend recovery. | Bound initial waiting and aggregate backlog; withdraw live guidance, expose permitted coverage gaps and reject old updates without automatic resumption. |\n+| `retrospective` | Progress/checkpoint activity and competing interactive work straddle total-job expiry. | Progress/yielding cannot renew the deadline; pause at a permitted checkpoint or stop, retaining sources and truthful partial coverage. |\n+\n+Each fixture report binds profile/implementation/policy revisions, minimized job/generation and source-epoch aliases, clock basis/resolution, relevant event offsets, queued/in-flight high-water marks, and expected versus observed disposition. Include acknowledgment, useful presentation, invalidation, fallback, output-quiescence and resource-release offsets where applicable. Diagnostics inherit source restrictions and do not require retained media or a new Log/wire schema. Release of expired/cancelled live output, stale admission/dispatch, renewal of the same job's deadline, unauthorized resumption or loss of permitted saved input fails that profile's enforcement gate; aggregate timing percentiles cannot excuse it. Later ordinary review of retained candidates with fresh bindings remains permitted. Deterministic fixtures establish controller behavior only, not achieved latency or hardware support.\n+\n **Measured enablement gate (`UBU-Q0168`).** Select numeric ceilings only from revision-pinned evidence on each proposed Device/backend/mode; record OS/runtime/model/template, implementation and policy revisions, source/context sizes, load and instrumentation. Measure controller acceptance, acknowledgment, first useful result, completion, observation age at presentation, stream gaps and stop/output-quiescence separately from admission, first CPU-certified planner chunk and final planner response. Report sample counts, median and tail distributions, misses, errors/degraded outcomes, memory/storage, battery/energy where observable, and sustained thermal behavior; unavailable metrics stay explicit. Report successful interpretations separately from unresolved/degraded outcomes; latency alone establishes neither correctness nor usefulness.\n \n Exercise cold and warm operation, competing work and sustained streams; offline/backend loss, low battery and thermal/resource exhaustion; interruption during acquisition/inference/output; queue overload and deadline expiry; source/context changes, corrections, revocation, restart/handoff and delayed/duplicate results. Demonstrate bounded waiting/backlog, working manual fallback, truthful saved/coverage/freshness status, no stale admission/dispatch and no hidden rerouting. Publish the selected finite ceilings and demonstrated fallback for each enabled class; an unmeasured class remains unavailable. Hardware selection remains `UBU-Q0177`, restricted-attention numeric profiles remain `UBU-Q0128`, and delivery sequence remains `UBU-Q0183`. `UBU-Q0168` stays Open for these numerical profiles and measured acceptance; this contract asserts no implementation, supported hardware or achieved latency.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\nindex bb4e98b..7eefb18 100644\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3217,25 +3217,25 @@ Resolved by `UBU-D0324`: DESIGN.md \u00a721.1.1 defines clarification, bounded ordin\n ## UBU-Q0168: Snapshot, conversational, streamed and retrospective latency classes\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-14 Scored from commit: None\n \n ### Question\n \n Which latency classes and degradation behavior are required for snapshots, conversational interaction, streamed supervision and retrospective analysis?\n \n ### Current direction\n \n-Delta \u00a717.6. `UBU-D0325` defines the four latency classes and deadline, freshness, degradation and interruption semantics in DESIGN.md \u00a721.1.4. Select actual user-visible deadlines, source/context-age limits, stream gap/backlog caps and battery/thermal conditions through revision-pinned local-device evidence. Capture latency remains distinct from planner streaming, admission and proof of execution. No measured performance numbers or supported-hardware claims are established.\n+Delta \u00a717.6. `UBU-D0325` defines the four latency classes and deadline, freshness, degradation and interruption semantics in DESIGN.md \u00a721.1.4. Select actual user-visible deadlines, source/context-age limits, stream gap/backlog caps and battery/thermal conditions through revision-pinned local-device evidence. Capture latency remains distinct from planner streaming, admission and proof of execution. No measured performance numbers or supported-hardware claims are established. `UBU-D0335` adds deadline-boundary, stream-invalidation and interruption-race fixtures with separately bounded fallback, output quiescence and resource release; simulated timings do not select numerical profiles.\n \n ### Resolution\n \n-Open. `UBU-D0325` answers the class/degradation architecture; numerical profiles and measured acceptance remain unresolved. Close only after evidence for each enabled Device/backend/mode pins implementation and policy revisions, selects finite deadlines and freshness/resource ceilings, and demonstrates the specified ordinary, sustained-load, offline, interruption and stale-result behavior. `UBU-Q0177` retains secure hardware selection, `UBU-Q0128` retains quantitative restricted-attention profiles and `UBU-Q0183` retains delivery sequence. A parameterized contract alone is insufficient to close this question.\n+Open. `UBU-D0325` answers the class/degradation architecture; numerical profiles and measured acceptance remain unresolved. Close only after evidence for each enabled Device/backend/mode pins implementation and policy revisions, selects finite deadlines and freshness/resource ceilings, and demonstrates the specified ordinary, sustained-load, offline, interruption and stale-result behavior. `UBU-Q0177` retains secure hardware selection, `UBU-Q0128` retains quantitative restricted-attention profiles and `UBU-Q0183` retains delivery sequence. `UBU-D0335` makes controller boundary enforcement an explicit qualification gate; its fixtures still need passing implementation and real-device evidence. A parameterized contract alone is insufficient to close this question.\n \n ---\n \n ## UBU-Q0169: Local AV segmentation and bounded agent/tool interfaces\n \n Status: Solved Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: UBU-D0326 Last scored: 2026-10-12 Scored from commit: None\n \n ### Question\n \n How are local audio/video streams segmented, and what scoped interfaces expose frame extraction, event detection and relevant-section isolation?\n",
    "commit_message": "docs: specify latency boundary qualification fixtures for UBU-Q0168",
    "validation_notes": [
      "Generated with git diff --no-index between unchanged working-tree copies and proposed contents; normalized only temporary path prefixes.",
      "git apply --check --whitespace=error passed against the current working tree with exit code 0.",
      "All question metadata lines, including existing ranking updates, remain unchanged; OPEN_QUESTIONS.md edits are confined to UBU-Q0168.",
      "Repository files were not modified. Controller fixtures and real-device performance trials were not executed; UBU-Q0168 remains Open."
    ],
    "new_questions_added": [],
    "questions_resolved": [],
    "decisions_added": [
      "UBU-D0335"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0168-deadline-boundary-fixtures",
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
