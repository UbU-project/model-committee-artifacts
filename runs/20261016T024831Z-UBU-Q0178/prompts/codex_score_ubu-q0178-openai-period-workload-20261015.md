# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0178`  
Question title: `Economically accessible local compute paths`  
Base commit: `9ba60b8be290eb8abc477fe362ef14a6ffaa7b06`

```markdown
## UBU-Q0178: Economically accessible local compute paths

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-15 Scored from commit: None

### Question

Which lower-cost local compute paths prevent economically precarious users from being excluded from the multimodal experience?

### Current direction

Delta §17.16. Consider smaller local models, bounded snapshots, CPU fallback, reduced latency classes and operator-controlled trusted hardware without a hidden hosted-model dependency. Measure the useful tradeoffs before claiming reach or costs. Compute accessibility is part of the mission, not a promised device minimum.

`UBU-D0332` specifies a comparison of bounded CPU-capable single-Device inference and an existing personal worker, with whole-route cost/effort budgets and the qualification gates in DESIGN.md §1. Actual useful lower-cost profiles and cost envelopes remain unselected. `UBU-D0337` adds explicit ownership scenarios, entry/period cash limits and all-attempt accounting per useful multimodal outcome; the no-worker scenario and any remaining gap must stay visible. `UBU-D0363` adds first-use setup and interrupted package/model acquisition trials, accounting for transferred data, helper resources and peak installation/recovery storage by starting-state/ownership scenario. `UBU-D0369` adds measured secure-upkeep and interrupted-update recovery, including route downtime and affected requalification costs within each ownership/horizon scenario.

### Resolution

Open. `UBU-D0332` answers the candidate-path and cost-evidence contract; `UBU-D0337` adds entry/period cash and useful-outcome accounting only. `UBU-D0363` adds setup and acquisition-recovery evidence; `UBU-D0369` adds secure-upkeep and interrupted-update recovery evidence. No measured path is selected. Close after operator-reviewed comparative real-device results select at least one qualified lower-cost camera-and-voice path for explicit ownership scenarios within declared entry, period and total cash, effort, energy and storage budgets, with useful end-to-end outcomes and mandatory security/retention gates met. Keep unqualified ownership scenarios and any no-worker gap explicit. A protocol, smaller model or text-only fallback cannot establish economically accessible multimodal reach.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0178-openai-period-workload-20261015",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0178",
    "base_commit": "9ba60b8be290eb8abc477fe362ef14a6ffaa7b06",
    "summary": "Require lower-cost routes to meet a frozen period workload within declared hardware availability and budgets.",
    "rationale": "Period cost estimates need evidence that the route can deliver the required useful multimodal outcomes when its endpoints are available. Coupling workload delivery and budget acceptance exposes burst demand, competing hardware use and worker absence while reusing existing qualification trials and request accounting.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\nindex 6b392fe..4c19fd1 100644\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5822,3 +5822,15 @@ Changed bindings require affected rechecks and renewed operator approval. Preser\n This makes qualification enforceable during use without selecting hardware, an update mechanism, a mobile planner or a compute API. It supplies no passing implementation or deployment evidence; `UBU-Q0177` remains Open pending complete mandatory-slice evidence and operator approval.\n \n ---\n+\n+## UBU-D0376: Lower-cost selection must meet the declared period workload\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a71. Narrows `UBU-Q0178`; measured lower-cost selection remains Open.\n+\n+Extend `UBU-D0332`/`UBU-D0337` with a frozen demand schedule for each ownership/budget scenario: a positive required count of useful camera-and-voice outcomes per budget period, offered-request timing including bursts, finite completion/active-effort limits, and endpoint availability and competing-use windows. Apply the same demand and frozen useful-outcome rubric to candidate routes and the named cost comparator. Owning a worker cannot establish that it is available when needed.\n+\n+Exercise a predeclared finite demand window covering the claimed bursts and availability boundaries on the exact route with deployed qualification controls active. Reuse existing qualification trials where schedule/load match, preserving their minimum counts and sustained durations. Use `UBU-D0361`'s offered-request/retry accounting and `UBU-D0337`'s stricter multimodal outcome denominator. Report attained useful outcomes, unmet requests, queue wait/backlog, total workflow time and all-attempt costs; refusals, unavailable endpoints and unfinished work cannot disappear from the demand record. Existing freshness, retention and job deadlines still apply.\n+\n+Publish observed-window results separately from period projections, with availability, competing-use and upkeep-downtime assumptions and uncertainty. Add pass/fail/unknown workload-target results beside the existing budget/gate results. A route qualifies only when both the required useful workload and whole-route budgets pass for that scenario; missing capacity evidence prevents selection. Reduced demand, longer waits or additional hardware require a separately evaluated scenario. Keep the no-worker gap visible and `UBU-Q0178` Open pending operator-reviewed comparative evidence; this selects no measured path or cost envelope.\n+\n+---\ndiff --git a/DESIGN.md b/DESIGN.md\nindex d8f3e0b..a798a54 100644\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -153,6 +153,12 @@ Measure all attempts through the first useful camera-and-voice outcome after ins\n \n Publish evidence and pass/fail/unknown for each required upkeep limit by ownership/horizon scenario in the existing cost/support matrix. Period estimates disclose update/recovery frequency and uncertainty rather than treating the trial as a guarantee of future update availability. Failed or unknown required limits prevent lower-cost selection for the affected scenario; evaluate shorter horizons or budgeted replacement separately instead of pricing unsupported operation as a saving. Preserve the explicit no-worker gap. This adds evidence for secure upkeep and selects no maintenance-automation architecture or measured path; `UBU-Q0178` remains Open pending operator-reviewed comparative results.\n \n+**Period workload feasibility (`UBU-D0376`).** Extend `UBU-D0332`/`UBU-D0337`'s comparison by freezing a demand schedule for each ownership/budget scenario before acceptance: a positive required number of rubric-qualified camera-and-voice outcomes per budget period, offered-request count/timing including bursts, finite completion and active-effort limits, and capture/review/inference endpoint availability and competing-use windows. Apply the same demand and frozen rubric to candidate routes and the named lower-cost comparator. Ownership alone cannot establish worker availability.\n+\n+Exercise a predeclared finite demand window covering the claimed bursts and availability boundaries on the exact route with its deployed qualification controls active. Reuse `UBU-D0331`'s trials where schedule/load match without reducing minimum counts or sustained durations. Apply `UBU-D0361`'s offered-request/retry accounting and `UBU-D0337`'s multimodal denominator: each requested outcome counts at most once, and manual-only fallback contributes none. Record attained useful outcomes, unmet requests, queue wait/backlog, total workflow time and all-attempt costs. Refusals, unavailable endpoints and unfinished work remain in offered demand; waiting cannot extend freshness, raw-media retention or job deadlines.\n+\n+Publish observed-window outcomes separately from period projections in the existing cost/support matrix. State projection assumptions and uncertainty for demand, endpoint availability, competing use and secure-upkeep downtime; projected completions are not observed outcomes. Record pass/fail/unknown for the required period useful-outcome target alongside every existing budget and mandatory gate. Selection requires both workload and whole-route budget passes for the same scenario; missing capacity evidence prevents selection. Lower demand, longer waits or additional hardware define separately evaluated scenarios. Preserve the explicit no-worker gap. This adds workload feasibility to cost acceptance and establishes no measured configuration, price or population reach; `UBU-Q0178` remains Open.\n+\n ---\n \n ## 2. Core Principles\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\nindex b91abac..1816259 100644\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3485,30 +3485,32 @@ Open. `UBU-D0331` answers the qualification protocol only. Close after actual re\n ## UBU-Q0178: Economically accessible local compute paths\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-15 Scored from commit: None\n \n ### Question\n \n Which lower-cost local compute paths prevent economically precarious users from being excluded from the multimodal experience?\n \n ### Current direction\n \n Delta \u00a717.16. Consider smaller local models, bounded snapshots, CPU fallback, reduced latency classes and operator-controlled trusted hardware without a hidden hosted-model dependency. Measure the useful tradeoffs before claiming reach or costs. Compute accessibility is part of the mission, not a promised device minimum.\n \n-`UBU-D0332` specifies a comparison of bounded CPU-capable single-Device inference and an existing personal worker, with whole-route cost/effort budgets and the qualification gates in DESIGN.md \u00a71. Actual useful lower-cost profiles and cost envelopes remain unselected. `UBU-D0337` adds explicit ownership scenarios, entry/period cash limits and all-attempt accounting per useful multimodal outcome; the no-worker scenario and any remaining gap must stay visible. `UBU-D0363` adds first-use setup and interrupted package/model acquisition trials, accounting for transferred data, helper resources and peak installation/recovery storage by starting-state/ownership scenario. `UBU-D0369` adds measured secure-upkeep and interrupted-update recovery, including route downtime and affected requalification costs within each ownership/horizon scenario.\n+`UBU-D0332` specifies a comparison of bounded CPU-capable single-Device inference and an existing personal worker, with whole-route cost/effort budgets and the qualification gates in DESIGN.md \u00a71. Actual useful lower-cost profiles and cost envelopes remain unselected. `UBU-D0337` adds explicit ownership scenarios, entry/period cash limits and all-attempt accounting per useful multimodal outcome; the no-worker scenario and any remaining gap must stay visible. `UBU-D0363` adds first-use setup and interrupted package/model acquisition trials, accounting for transferred data, helper resources and peak installation/recovery storage by starting-state/ownership scenario. `UBU-D0369` adds measured secure-upkeep and interrupted-update recovery, including route downtime and affected requalification costs within each ownership/horizon scenario. `UBU-D0376` adds a frozen period demand schedule and endpoint-availability assumptions, coupling budget acceptance to useful multimodal workload delivery.\n \n ### Resolution\n \n Open. `UBU-D0332` answers the candidate-path and cost-evidence contract; `UBU-D0337` adds entry/period cash and useful-outcome accounting only. `UBU-D0363` adds setup and acquisition-recovery evidence; `UBU-D0369` adds secure-upkeep and interrupted-update recovery evidence. No measured path is selected. Close after operator-reviewed comparative real-device results select at least one qualified lower-cost camera-and-voice path for explicit ownership scenarios within declared entry, period and total cash, effort, energy and storage budgets, with useful end-to-end outcomes and mandatory security/retention gates met. Keep unqualified ownership scenarios and any no-worker gap explicit. A protocol, smaller model or text-only fallback cannot establish economically accessible multimodal reach.\n \n+`UBU-D0376` additionally requires the declared period useful-outcome target to pass under the evaluated availability and competing-use assumptions; a budget pass alone cannot qualify a route.\n+\n ---\n \n ## UBU-Q0179: Audience routing for the social-impact thesis\n \n Status: Open Priority: Post-MVP Phase: Post-MVP Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: None Resolved by: None Last scored: Never Scored from commit: None\n \n ### Question\n \n Which additional parts of the social-impact thesis belong in public overview, funder, nonprofit/government outreach and canonical design material?\n \n ### Current direction\n \n",
    "commit_message": "docs: require period workload feasibility for lower-cost multimodal paths",
    "validation_notes": [
      "git apply --check passed against the current working tree.",
      "Generated from unchanged working-tree contents and in-memory proposed text; no repository files were modified.",
      "Confirmed the three-file allowlist, unused UBU-D0376, selected-question hunk anchors and unchanged single-line question metadata.",
      "No real-device measurements were performed; UBU-Q0178 remains Open pending evidence and operator approval."
    ],
    "new_questions_added": [],
    "questions_resolved": [],
    "decisions_added": [
      "UBU-D0376"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0178-openai-period-workload-20261015",
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
