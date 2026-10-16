# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0168`  
Question title: `Snapshot, conversational, streamed and retrospective latency classes`  
Base commit: `1d922f99e6a0b46c86141d8eabbfdda8eb5dcf51`

```markdown
## UBU-Q0168: Snapshot, conversational, streamed and retrospective latency classes

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-15 Scored from commit: None

### Question

Which latency classes and degradation behavior are required for snapshots, conversational interaction, streamed supervision and retrospective analysis?

### Current direction

Delta §17.6. `UBU-D0325` defines the four latency classes and deadline, freshness, degradation and interruption semantics in DESIGN.md §21.1.4. Select actual user-visible deadlines, source/context-age limits, stream gap/backlog caps and battery/thermal conditions through revision-pinned local-device evidence. Capture latency remains distinct from planner streaming, admission and proof of execution. No measured performance numbers or supported-hardware claims are established. `UBU-D0335` adds deadline-boundary, stream-invalidation and interruption-race fixtures with separately bounded fallback, output quiescence and resource release; simulated timings do not select numerical profiles. `UBU-D0361` separates calibration from frozen acceptance trials and requires complete request/session outcome accounting with declared timely-useful and stream-coverage criteria.

`UBU-D0367` adds clock qualification for Device sleep/controller suspension and conservative source/worker timing mappings, with controller fixtures and deployment evidence or enforced mode exclusion. Numerical selection still requires the frozen real-device acceptance evidence above.

`UBU-D0374` adds per-scope observation-frontier checks for out-of-order stream results and coverage measured from eligible presentation intervals. Still-fresh older or duplicate updates cannot replace newer guidance or renew gap/coverage accounting; controller fixtures and deployment evidence remain required.

### Resolution

Open. `UBU-D0325` answers the class/degradation architecture; numerical profiles and measured acceptance remain unresolved. Close only after evidence for each enabled Device/backend/mode pins implementation and policy revisions, selects finite deadlines and freshness/resource ceilings, and demonstrates the specified ordinary, sustained-load, offline, interruption and stale-result behavior. `UBU-Q0177` retains secure hardware selection, `UBU-Q0128` retains quantitative restricted-attention profiles and `UBU-Q0183` retains delivery sequence. `UBU-D0335` makes controller boundary enforcement an explicit qualification gate; its fixtures still need passing implementation and real-device evidence. `UBU-D0361` additionally requires frozen per-condition acceptance limits and complete outcome/coverage reports; correct fallback alone cannot qualify ordinary usefulness. A parameterized contract alone is insufficient to close this question.

`UBU-D0367` further requires suspension-aware enforcement and bounded clock-mapping checks; its fixtures and deployment trials remain outstanding. This selects no numerical values or passing profile and does not close the question.

`UBU-D0374` further specifies stream presentation ordering and coverage accounting; its required enforcement fixtures and deployment trials remain outstanding. Numerical selection and measured acceptance remain Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0168-openai-ingress-latency",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0168",
    "base_commit": "1d922f99e6a0b46c86141d8eabbfdda8eb5dcf51",
    "summary": "Bound input queues and measure user-visible latency from the original request offer, alongside controller-relative class deadlines.",
    "rationale": "Controller-acceptance clocks can exclude earlier input queues, allowing a delayed request to appear timely after handoff. Original-offer budgets, ingress boundary fixtures and fixed offered-load trials close that accounting gap without inventing numerical profiles or closing UBU-Q0168.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5870,3 +5870,15 @@ Include client decoding and numeric domains/encodings in the existing host-mappi\n These are numeric carrier controls with incomplete qualifications and unresolved evidence, not a complete companion mapping, deployed WebView result or admission/exchange test. No repair or governing schema gap is established; `UBU-Q0166` remains Open. `UBU-D0334`'s normalization loss and `UBU-D0373`'s presentation limitation remain. No schema, enum, FactProvenance field, pin, migration or producer authority changes here.\n \n ---\n+\n+## UBU-D0380: User-visible latency includes request ingress before controller acceptance\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a721.1.4. Narrows `UBU-Q0168`; numerical profiles and measured acceptance remain Open.\n+\n+Bind end-to-end acknowledgment, first-useful-result and total-job ceilings to each route's original offer event before internal queueing. Bound ingress queues and record offer, acceptance/refusal and presentation separately with qualified clocks. Existing class deadlines remain separately observable and intersect the remaining offer budgets; handoff and retries cannot restart user-visible waiting.\n+\n+Expire or invalidate pending ingress locally and prevent late dispatch/release under the existing fallback, interruption and retention rules. A fast acceptance-relative result cannot qualify as timely after its original offer ceiling. Keep pre-acceptance refusals, expiry and missing outcomes in `UBU-D0361`'s offered-request accounting.\n+\n+Require \u00a721.1.4's delayed-handoff, ingress-saturation, boundary and interruption fixtures, plus pinned deployment trials driven by frozen arrivals for claimed concurrent/sustained workloads. Report ingress and end-to-end timings alongside class timings; an incompletely offered workload cannot qualify a smaller denominator. These are enforcement and evidence requirements, not numerical selection or passing implementation/hardware evidence. `UBU-Q0168` stays Open pending the existing frozen acceptance trials and operator review; no kernel API or wire-schema change is selected.\n+\n+---\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -4165,6 +4165,12 @@ Report the timely-useful numerator over all offered requests in each ordinary co\n \n Publish a per-class, per-condition acceptance table linking the frozen profile and trial plan to counts, distributions, coverage, each limit and pass/fail/unknown. An unknown required metric or missed mandatory limit withholds the affected profile; pooled results cannot hide a failed condition. Passing cancellation/fallback fixtures cannot compensate for missed ordinary usefulness, and rapid ordinary results cannot compensate for failed enforcement, privacy or retention gates. Publish calibration results separately and obtain operator review before enablement. This adds an evidence-selection gate, not measured values or a new wire schema; `UBU-Q0168` remains Open.\n \n+**Request ingress and offered-load timing (`UBU-D0380`).** Pin the original offer event at the application input boundary before internal queueing: one-shot request submission, the completed-turn event before any remaining transcription/interpretation, broker observation-window close or retrospective enqueue. Record offer, controller acceptance/refusal and useful presentation separately with `UBU-D0367`'s clock qualification. Freeze finite ingress queue count/byte limits and positive offer-to-acknowledgment, offer-to-first-useful-result and offer-to-total-job ceilings with the existing ordering. Keep the class clocks above separately observable; at acceptance and release, enforce their intersection with the remaining original offer budgets. Moving work through an input queue, controller handoff or retry cannot renew user-visible waiting.\n+\n+Expiry while awaiting controller acceptance invalidates the pending request, produces the existing bounded fallback and prevents later dispatch. Interruption also invalidates queued ingress locally without waiting for acceptance; it cannot trigger old output on a superseding turn. Accepted work retains the ordinary expiry, retention and interruption rules. A useful result within its controller-relative ceiling but outside its original offer ceiling cannot count as timely useful under `UBU-D0361`. Requests refused, expired or lost before acceptance remain in the offered-request denominator; missing timing evidence cannot establish a timely outcome.\n+\n+Extend the actual-controller fixtures to delay input handoff and saturate ingress queues, with acceptance/results immediately before, at and after the original offer ceilings, plus interruption before acceptance and a late retry. Assert bounded fallback, no expired/cancelled dispatch or live release, unchanged original budgets and preserved permitted saved input. For claimed concurrent/sustained workloads, freeze arrival times and bursts independently of earlier completions, using independent sessions where turn semantics require. Report planned/actual offers, ingress wait/high-water marks and offer-to-presentation timings alongside the existing acceptance-relative distributions. Harness delays or unsubmitted scheduled requests invalidate the affected workload run rather than shrinking its denominator. Demonstrate these paths on the pinned deployment with qualification controls active; failed required bounds or unknown required timing withhold the affected profile. This adds no numerical values, passing evidence or wire-schema fields; `UBU-Q0168` remains Open.\n+\n **Stream presentation order (`UBU-D0374`).** Within each scope of streamed guidance and live generation, the controller binds observation windows before dispatch to the broker's original source epochs, sequence/sample positions and intervals under \u00a721.1.5. For each required source, the presented observation frontier is the latest presented window end in that source's original ordered sample timeline; no frontier is presented initially. Retries preserve window identity; callback arrival, worker completion and model timestamps cannot define observation order. At release, an update must advance at least one required source's frontier and regress none, while satisfying every required source's freshness and ordinary release checks. An identical window cannot advance the frontier through regenerated wording. Different sources/epochs have no shared sequence order; unknown ordering cannot establish live eligibility, and source-epoch changes use the existing invalidation and fresh-generation rules.\n \n Reject duplicate or regressing updates for current guidance even when their elapsed deadlines and age limits still pass. They cannot overwrite newer guidance or reset the update-gap clock. A newer eligible update may bypass unfinished older windows without waiting for an ordered backlog; queueing or completion alone never advances the presented frontier. Before any useful presentation, an older window may still qualify if a newer window failed or remains pending and all original limits and bindings hold. Existing withdrawal, explicit-resume and historical-review rules continue to apply.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3303,34 +3303,38 @@ Resolved by `UBU-D0324`: DESIGN.md \u00a721.1.1 defines clarification, bounded ordin\n ## UBU-Q0168: Snapshot, conversational, streamed and retrospective latency classes\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-15 Scored from commit: None\n \n ### Question\n \n Which latency classes and degradation behavior are required for snapshots, conversational interaction, streamed supervision and retrospective analysis?\n \n ### Current direction\n \n+`UBU-D0380` additionally bounds request ingress from the original offer event, intersects end-to-end and controller-relative budgets, and requires delayed-handoff/queue fixtures plus frozen offered-load timing. Acceptance-relative speed cannot hide an earlier input wait; numerical selection still requires real-device evidence.\n+\n Delta \u00a717.6. `UBU-D0325` defines the four latency classes and deadline, freshness, degradation and interruption semantics in DESIGN.md \u00a721.1.4. Select actual user-visible deadlines, source/context-age limits, stream gap/backlog caps and battery/thermal conditions through revision-pinned local-device evidence. Capture latency remains distinct from planner streaming, admission and proof of execution. No measured performance numbers or supported-hardware claims are established. `UBU-D0335` adds deadline-boundary, stream-invalidation and interruption-race fixtures with separately bounded fallback, output quiescence and resource release; simulated timings do not select numerical profiles. `UBU-D0361` separates calibration from frozen acceptance trials and requires complete request/session outcome accounting with declared timely-useful and stream-coverage criteria.\n \n `UBU-D0367` adds clock qualification for Device sleep/controller suspension and conservative source/worker timing mappings, with controller fixtures and deployment evidence or enforced mode exclusion. Numerical selection still requires the frozen real-device acceptance evidence above.\n \n `UBU-D0374` adds per-scope observation-frontier checks for out-of-order stream results and coverage measured from eligible presentation intervals. Still-fresh older or duplicate updates cannot replace newer guidance or renew gap/coverage accounting; controller fixtures and deployment evidence remain required.\n \n ### Resolution\n \n Open. `UBU-D0325` answers the class/degradation architecture; numerical profiles and measured acceptance remain unresolved. Close only after evidence for each enabled Device/backend/mode pins implementation and policy revisions, selects finite deadlines and freshness/resource ceilings, and demonstrates the specified ordinary, sustained-load, offline, interruption and stale-result behavior. `UBU-Q0177` retains secure hardware selection, `UBU-Q0128` retains quantitative restricted-attention profiles and `UBU-Q0183` retains delivery sequence. `UBU-D0335` makes controller boundary enforcement an explicit qualification gate; its fixtures still need passing implementation and real-device evidence. `UBU-D0361` additionally requires frozen per-condition acceptance limits and complete outcome/coverage reports; correct fallback alone cannot qualify ordinary usefulness. A parameterized contract alone is insufficient to close this question.\n \n `UBU-D0367` further requires suspension-aware enforcement and bounded clock-mapping checks; its fixtures and deployment trials remain outstanding. This selects no numerical values or passing profile and does not close the question.\n \n `UBU-D0374` further specifies stream presentation ordering and coverage accounting; its required enforcement fixtures and deployment trials remain outstanding. Numerical selection and measured acceptance remain Open.\n \n+`UBU-D0380` further requires bounded ingress and original-offer timing in the enforcement fixtures and deployment acceptance reports. Those checks remain outstanding; no numerical profile is selected and this question remains Open.\n+\n ---\n \n ## UBU-Q0169: Local AV segmentation and bounded agent/tool interfaces\n \n Status: Solved Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: UBU-D0326 Last scored: 2026-10-12 Scored from commit: None\n \n ### Question\n \n How are local audio/video streams segmented, and what scoped interfaces expose frame extraction, event detection and relevant-section isolation?\n \n",
    "commit_message": "docs: bound request ingress in latency qualification",
    "validation_notes": [
      "Generated diffs from current working-tree contents and in-memory proposed contents using git diff --no-index; normalized stdin headers to canonical paths.",
      "git apply --check and git apply --check --whitespace=error passed against the working tree.",
      "Verified UBU-D0380 is unused, UBU-Q0168 metadata is unchanged, and neighboring question blocks are unchanged.",
      "Repository files remain unchanged. No controller fixtures or real-device trials ran; numerical qualification remains outstanding."
    ],
    "new_questions_added": [],
    "questions_resolved": [],
    "decisions_added": [
      "UBU-D0380"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0168-openai-ingress-latency",
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
