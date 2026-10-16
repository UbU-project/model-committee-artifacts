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
Base commit: `206a876869bf01054ced1f325e38d0ff014af5cb`

```markdown
## UBU-Q0168: Snapshot, conversational, streamed and retrospective latency classes

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-15 Scored from commit: None

### Question

Which latency classes and degradation behavior are required for snapshots, conversational interaction, streamed supervision and retrospective analysis?

### Current direction

Delta §17.6. `UBU-D0325` defines the four latency classes and deadline, freshness, degradation and interruption semantics in DESIGN.md §21.1.4. Select actual user-visible deadlines, source/context-age limits, stream gap/backlog caps and battery/thermal conditions through revision-pinned local-device evidence. Capture latency remains distinct from planner streaming, admission and proof of execution. No measured performance numbers or supported-hardware claims are established. `UBU-D0335` adds deadline-boundary, stream-invalidation and interruption-race fixtures with separately bounded fallback, output quiescence and resource release; simulated timings do not select numerical profiles. `UBU-D0361` separates calibration from frozen acceptance trials and requires complete request/session outcome accounting with declared timely-useful and stream-coverage criteria.

`UBU-D0367` adds clock qualification for Device sleep/controller suspension and conservative source/worker timing mappings, with controller fixtures and deployment evidence or enforced mode exclusion. Numerical selection still requires the frozen real-device acceptance evidence above.

### Resolution

Open. `UBU-D0325` answers the class/degradation architecture; numerical profiles and measured acceptance remain unresolved. Close only after evidence for each enabled Device/backend/mode pins implementation and policy revisions, selects finite deadlines and freshness/resource ceilings, and demonstrates the specified ordinary, sustained-load, offline, interruption and stale-result behavior. `UBU-Q0177` retains secure hardware selection, `UBU-Q0128` retains quantitative restricted-attention profiles and `UBU-Q0183` retains delivery sequence. `UBU-D0335` makes controller boundary enforcement an explicit qualification gate; its fixtures still need passing implementation and real-device evidence. `UBU-D0361` additionally requires frozen per-condition acceptance limits and complete outcome/coverage reports; correct fallback alone cannot qualify ordinary usefulness. A parameterized contract alone is insufficient to close this question.

`UBU-D0367` further requires suspension-aware enforcement and bounded clock-mapping checks; its fixtures and deployment trials remain outstanding. This selects no numerical values or passing profile and does not close the question.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "ubu-q0168-stream-presentation-frontier",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0168",
    "base_commit": "206a876869bf01054ced1f325e38d0ff014af5cb",
    "summary": "Prevent still-fresh out-of-order stream updates from replacing newer guidance or inflating live-guidance coverage.",
    "rationale": "An older and a newer observation window can both pass deadline and freshness checks within the same generation. Presentation-frontier checks prevent the older result from replacing newer guidance or resetting its gap clock. Coverage measured from eligible presentation intervals prevents duplicate, overlapping or retrospective results from overstating availability. Numerical profiles still require frozen real-device acceptance evidence.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5798,3 +5798,15 @@ This demonstrates a presentation limitation even when normalization is bypassed.\n Require the existing resolved-content, faithful-review and admission gates before enabling a perceptual adapter. `UBU-Q0166` remains Open; `UBU-D0334`'s inline normalization loss remains unrepaired. No schema, enum, FactProvenance field, pin, migration or producer authority changes here.\n \n ---\n+\n+## UBU-D0374: Stream presentation cannot regress the observation frontier\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a721.1.4. Narrows `UBU-Q0168`; numerical profiles and measured acceptance remain Open.\n+\n+Order live stream updates within each guidance scope by their original broker-bound observation windows, preserving source epochs and retry identity. Advance the presented frontier only on actual eligible presentation; a multi-source update must advance at least one required source and regress none. Duplicate or older updates cannot overwrite newer guidance or reset the gap clock even when still fresh. Newer eligible updates may bypass unfinished older work; queueing or completion cannot establish presentation order. All existing freshness, invalidation and explicit-resume checks remain required.\n+\n+Measure live-guidance coverage as a union of eligible presentation intervals, clipped by age/gap expiry, invalidation and withdrawal. Rejected or duplicate callbacks and repeated wording cannot extend coverage; overlapping eligible intervals count once, and later analysis cannot fill earlier presentation gaps. Keep analyzed observation coverage distinct and retain the declared active-session denominator.\n+\n+Require DESIGN.md \u00a721.1.4's actual-controller reordered-window, duplicate/retry, failed-newer-window, overlap, multi-source and release-race fixtures plus pinned deployment evidence. These are enforcement and accounting requirements, not passing results, numerical values or a new wire schema. `UBU-Q0168` stays Open pending the existing frozen real-device acceptance trials and operator review.\n+\n+---\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -4099,6 +4099,14 @@ Report the timely-useful numerator over all offered requests in each ordinary co\n \n Publish a per-class, per-condition acceptance table linking the frozen profile and trial plan to counts, distributions, coverage, each limit and pass/fail/unknown. An unknown required metric or missed mandatory limit withholds the affected profile; pooled results cannot hide a failed condition. Passing cancellation/fallback fixtures cannot compensate for missed ordinary usefulness, and rapid ordinary results cannot compensate for failed enforcement, privacy or retention gates. Publish calibration results separately and obtain operator review before enablement. This adds an evidence-selection gate, not measured values or a new wire schema; `UBU-Q0168` remains Open.\n \n+**Stream presentation order (`UBU-D0374`).** Within each scope of streamed guidance and live generation, the controller binds observation windows before dispatch to the broker's original source epochs, sequence/sample positions and intervals under \u00a721.1.5. For each required source, the presented observation frontier is the latest presented window end in that source's original ordered sample timeline; no frontier is presented initially. Retries preserve window identity; callback arrival, worker completion and model timestamps cannot define observation order. At release, an update must advance at least one required source's frontier and regress none, while satisfying every required source's freshness and ordinary release checks. An identical window cannot advance the frontier through regenerated wording. Different sources/epochs have no shared sequence order; unknown ordering cannot establish live eligibility, and source-epoch changes use the existing invalidation and fresh-generation rules.\n+\n+Reject duplicate or regressing updates for current guidance even when their elapsed deadlines and age limits still pass. They cannot overwrite newer guidance or reset the update-gap clock. A newer eligible update may bypass unfinished older windows without waiting for an ordered backlog; queueing or completion alone never advances the presented frontier. Before any useful presentation, an older window may still qualify if a newer window failed or remains pending and all original limits and bindings hold. Existing withdrawal, explicit-resume and historical-review rules continue to apply.\n+\n+Count live-guidance coverage as the union of intervals during which the scoped guidance is available as current and remains fresh and usable, beginning at actual eligible presentation and ending at the earliest applicable age/gap expiry, invalidation or withdrawal. Overlapping updates cannot multiply covered time; rejected or duplicate callbacks cannot extend it, and later analysis cannot backfill time before presentation. Keep `UBU-D0361`'s declared active-session denominator and report analyzed observation coverage separately.\n+\n+Extend `UBU-D0335`'s actual-controller fixtures with two still-fresh windows A then B whose results return B then A; duplicate/retried B callbacks; a failed or pending B followed by eligible A before the first result; overlapping windows; and multi-source updates that advance one required source while regressing another, including equal timestamps across sources. Assert eligible presentations and frontier changes, rejected live releases, unchanged gap deadlines on rejection and exact covered-time intervals. Include release races with expiry and interruption, then demonstrate the behavior in the pinned deployment trials. Incorrect ordering or inflated coverage fails the affected stream profile; simulated timings select no numerical profile. Record minimized window/frontier aliases and expected/observed outcomes in the existing fixture reports without a new canonical object or wire-schema field.\n+\n #### 21.1.5 Local AV segmentation and scoped analysis\n \n **Architecture contract (`UBU-D0326`).** Adopt a controller-owned local media broker with the logical adapter profile `local-media/1`. Session consent enables only named sources on eligible execution-enclave Devices on the operator's own hardware; sharing hardware grants no access between enclaves. Capture controls remain visible under \u00a712.2 and \u00a721.1.3. Models and agents can request analysis of supplied media spans, but cannot start/resume capture, enumerate sources, renew consent or enlarge a grant. This profile specifies an interface boundary, not an implemented MCP service, frozen wire-schema change or new planner entity.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3287,34 +3287,38 @@ Resolved by `UBU-D0324`: DESIGN.md \u00a721.1.1 defines clarification, bounded ordin\n ## UBU-Q0168: Snapshot, conversational, streamed and retrospective latency classes\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-15 Scored from commit: None\n \n ### Question\n \n Which latency classes and degradation behavior are required for snapshots, conversational interaction, streamed supervision and retrospective analysis?\n \n ### Current direction\n \n Delta \u00a717.6. `UBU-D0325` defines the four latency classes and deadline, freshness, degradation and interruption semantics in DESIGN.md \u00a721.1.4. Select actual user-visible deadlines, source/context-age limits, stream gap/backlog caps and battery/thermal conditions through revision-pinned local-device evidence. Capture latency remains distinct from planner streaming, admission and proof of execution. No measured performance numbers or supported-hardware claims are established. `UBU-D0335` adds deadline-boundary, stream-invalidation and interruption-race fixtures with separately bounded fallback, output quiescence and resource release; simulated timings do not select numerical profiles. `UBU-D0361` separates calibration from frozen acceptance trials and requires complete request/session outcome accounting with declared timely-useful and stream-coverage criteria.\n \n `UBU-D0367` adds clock qualification for Device sleep/controller suspension and conservative source/worker timing mappings, with controller fixtures and deployment evidence or enforced mode exclusion. Numerical selection still requires the frozen real-device acceptance evidence above.\n \n+`UBU-D0374` adds per-scope observation-frontier checks for out-of-order stream results and coverage measured from eligible presentation intervals. Still-fresh older or duplicate updates cannot replace newer guidance or renew gap/coverage accounting; controller fixtures and deployment evidence remain required.\n+\n ### Resolution\n \n Open. `UBU-D0325` answers the class/degradation architecture; numerical profiles and measured acceptance remain unresolved. Close only after evidence for each enabled Device/backend/mode pins implementation and policy revisions, selects finite deadlines and freshness/resource ceilings, and demonstrates the specified ordinary, sustained-load, offline, interruption and stale-result behavior. `UBU-Q0177` retains secure hardware selection, `UBU-Q0128` retains quantitative restricted-attention profiles and `UBU-Q0183` retains delivery sequence. `UBU-D0335` makes controller boundary enforcement an explicit qualification gate; its fixtures still need passing implementation and real-device evidence. `UBU-D0361` additionally requires frozen per-condition acceptance limits and complete outcome/coverage reports; correct fallback alone cannot qualify ordinary usefulness. A parameterized contract alone is insufficient to close this question.\n \n `UBU-D0367` further requires suspension-aware enforcement and bounded clock-mapping checks; its fixtures and deployment trials remain outstanding. This selects no numerical values or passing profile and does not close the question.\n \n+`UBU-D0374` further specifies stream presentation ordering and coverage accounting; its required enforcement fixtures and deployment trials remain outstanding. Numerical selection and measured acceptance remain Open.\n+\n ---\n \n ## UBU-Q0169: Local AV segmentation and bounded agent/tool interfaces\n \n Status: Solved Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: UBU-D0326 Last scored: 2026-10-12 Scored from commit: None\n \n ### Question\n \n How are local audio/video streams segmented, and what scoped interfaces expose frame extraction, event detection and relevant-section isolation?\n \n ### Current direction\n \n Delta \u00a717.7. `UBU-D0326` specifies source-epoch chunking, bounded local buffering, original timestamps/coverage, purpose-scoped frame/event/section operations, revocation/cancellation and intermediate-artifact leases in DESIGN.md \u00a721.1.5. Raw-media retention defaults and operator controls remain `UBU-Q0170`\u2013`UBU-Q0171`; numeric latency/resource acceptance remains `UBU-Q0168`. No MCP or agent interface is implemented by this record.\n \n",
    "commit_message": "docs: bind streamed guidance to observation presentation order",
    "validation_notes": [
      "Generated with git diff --no-index against current working-tree files and proposed contents on stdin; normalized destination headers and omitted optional index lines.",
      "git apply --check --whitespace=error - passed against the current working tree.",
      "Verified the UBU-Q0168 hunk includes its heading and own Resolution section; single-line metadata and adjacent question blocks are preserved.",
      "Verified UBU-D0374 was unused, only three allowlisted files appear in the patch, and the working tree remains unchanged.",
      "Documentation proposal only; controller fixtures, deployment measurements and numerical profile selection remain outstanding."
    ],
    "new_questions_added": [],
    "questions_resolved": [],
    "decisions_added": [
      "UBU-D0374"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "ubu-q0168-stream-presentation-frontier",
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
