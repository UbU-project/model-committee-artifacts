# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0152`  
Question title: `Allowed time range on Tasks`  
Base commit: `6cccfc13603d3dd7ec5ae0218cbdfa5a74623082`

```markdown
## UBU-Q0152: Allowed time range on Tasks

Status: Open Priority: MVP blocker Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0154, routine planning, retirement of occupies_capacity Resolved by: None Last scored: 2026-09-21 Scored from commit: None

Defining context: DESIGN.md §9.2, DESIGN.md §16.5, PLANNING_KERNEL_CONTRACT.md §2, `UBU-D0256`, `UBU-D0275`.

### Question

What Task input expresses the range of times a Dynamic Task may occupy, and how does planning consume it?

### Subquestions

1. **Shape.** Is the range a single `{ earliest_start, latest_finish }` interval, an ordered list of movable windows like the decision envelope's `movable_windows`, or a recurring local time-of-day window such as 09:00–11:45?
2. **Input versus output.** The decision envelope (`UBU-D0256`) is derived from a Plan for mobile repair. How is the Task's declared range kept distinct from it, and from due dates and deadlines?
3. **Relation to `static_window`.** Are the two mutually exclusive, or is a Static Task a range with no slack?
4. **Hard or soft.** Must placement fall inside the range, or may it spill outside at a scored cost?
5. **Kernel mapping.** `TaskSpec` carries one per-task `window`, which the kernel intersects with the plan window. Several windows would need either a contract change or the orchestrator choosing one window per horizon.
6. **Retiring `occupies_capacity`.** The orchestrator currently refuses non-capacity Dynamic Tasks, because `occupies_capacity` is a bridge until the planner can place hard-to-schedule routine Tasks. Do allowed ranges plus the Phase 1b planner retire the field, and on what evidence?
7. **Time zones.** The planning timeline is UTC seconds. How are local time-of-day ranges converted, including across DST changes?

### Current direction

One-off Tasks carry an absolute `{ earliest_start, latest_finish }` range, which the orchestrator maps onto `TaskSpec.window`. Routine instances receive their range from the recurrence rule when they are instantiated (`UBU-Q0154`). The range is a hard constraint. Multiple movable windows are deferred. The range is a declared input, and the decision envelope stays a derived output.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0152-P-gpt5-codex-001",
    "provider_id": "openai",
    "model_name": "gpt-5-codex",
    "question_id": "UBU-Q0152",
    "base_commit": "6cccfc13603d3dd7ec5ae0218cbdfa5a74623082",
    "summary": "Resolve UBU-Q0152 by adding UBU-D0276: one-off Dynamic Tasks use a single hard absolute UTC allowed_time_range that maps to TaskSpec.window, while recurrence resolves local windows into concrete UTC instance ranges before kernel dispatch.",
    "rationale": "This follows the current direction and avoids widening the kernel contract for Phase 1b. It keeps Task input distinct from derived mobile decision envelopes, preserves Static Tasks as fixed commitments rather than zero-slack Dynamic ranges, treats the allowed range as a hard constraint, and makes retirement of occupies_capacity evidence-gated rather than instantaneous.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md",
      "PLANNING_KERNEL_CONTRACT.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -4297,7 +4297,31 @@ Consequences:\n Consequences:\n \n - `DESIGN.md` \u00a74 gains a Phase 1b section (\u00a74.2) and a phase map (\u00a74.7); Phase 2 and Phase 3 move to \u00a74.3 and \u00a74.4, and Phase 4 and Phase 5+ receive \u00a74.5 and \u00a74.6.\n - The `model-committee` phase vocabulary adds `Phase 4` and `Phase 5` so that Open Questions can carry the new labels.\n - Phase 2 sync scope, the Device Sync and Compartment contract, and all Phase 1 and Phase 1b decisions are unchanged.\n \n ---\n+\n+## UBU-D0276: Dynamic Task allowed ranges are single hard absolute occupancy intervals\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a79.2, \u00a716.5; PLANNING_KERNEL_CONTRACT.md \u00a73. Resolves `UBU-Q0152`.\n+\n+One-off Dynamic Tasks express when they may occupy Calendar time with `allowed_time_range: { earliest_start, latest_finish }`, where both endpoints are absolute UTC instants and `earliest_start < latest_finish`. The entire planned occupied interval for the Task must fit inside this range. Placement outside the range is hard infeasibility, not a soft penalty.\n+\n+The allowed range is a declared Task input. It is distinct from due dates, deadlines, or target dates, which may affect scoring, risk, explanation, or completion legitimacy but do not by themselves define occupancy eligibility. It is also distinct from the `UBU-D0256` decision envelope, which is derived from a concrete Plan and compact Calendar policy for mobile stewardship and local repair.\n+\n+Static Tasks and Dynamic allowed ranges are mutually exclusive at the Task scheduling layer. A Static Task uses fixed start and end times and enters the skeleton directly. A Dynamic Task uses `allowed_time_range` plus duration and is placed by the planner. A zero-slack range is not a substitute for a Static Task commitment; if the wall-clock slot itself is committed, the Task is Static.\n+\n+For kernel dispatch, the CPU converts the single allowed range to the per-Task `TaskSpec.window` by intersecting it with `PlanningRequest.time_window`. If the intersection is empty or shorter than the Task's minimum possible duration, that Task is unplaceable for the request. Phase 1b does not add multiple movable windows to `TaskSpec`; future multi-window support needs a later contract change or CPU-side selection before dispatch.\n+\n+Local time-of-day constraints are not stored on one-off Tasks. Recurrence templates may describe local windows, but instantiation resolves each occurrence into a concrete UTC `allowed_time_range` using the schedule's IANA time zone and recorded DST disambiguation. Ambiguous or nonexistent local endpoints must be resolved or surfaced for review before the occurrence becomes a kernel input.\n+\n+`occupies_capacity` is retired as a scheduling semantic once Phase 1b implementation evidence shows that Dynamic Tasks and routine instances with admitted ranges are placed, omitted through partial-placement rules, and hard-validated without the old gate. Until that evidence exists, existing compatibility fields may remain as migration scaffolding, but the canonical rule is that a schedulable Dynamic Task with duration and an allowed range occupies planning capacity.\n+\n+Consequences:\n+\n+- `DESIGN.md` \u00a79.2 records `allowed_time_range` as the Phase 1b Dynamic Task input and distinguishes it from Static Task timing.\n+- `PLANNING_KERNEL_CONTRACT.md` \u00a73 records the one-window `TaskSpec.window` mapping and hard infeasibility rule.\n+- `OPEN_QUESTIONS.md` marks `UBU-Q0152` solved.\n+\n+---\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -1475,20 +1475,23 @@ Static Tasks are included directly in Plans.\n #### Dynamic Task\n \n A Dynamic Task has flexible scheduling.\n \n Dynamic Tasks may have:\n \n - duration or duration PDF\n+- allowed time range\n - dependencies\n - preconditions\n - effects\n - Objective link\n - success probability\n - affect delta\n - expected cost\n \n **Phase 1 realization (`UBU-D0239`).** A Phase 1 Task carries an optional `duration_estimate` \u2014 either a fixed scalar or the \u00a73 three-point shifted-log-normal (`min_seconds`/`mode_seconds`/`p95_seconds`) \u2014 and optional `correlation_groups` membership, defaulting to a fixed estimate and independence when absent. These are the per-Task duration uncertainty and correlation inputs the planning rollout consumes; the store admits and persists them, and the orchestrator carries them into the kernel `TaskSpec`.\n+\n+**Phase 1b allowed range (`UBU-D0276`).** A one-off Dynamic Task may carry `allowed_time_range: { earliest_start, latest_finish }`, expressed as absolute UTC instants. The planner must place the Task's entire occupied interval inside that hard range by mapping it to the kernel `TaskSpec.window`. The range is a declared Task input, not a derived decision envelope, due date, deadline, or recurring local time-of-day rule. Routine instances receive their concrete range when recurrence instantiates them. Static Tasks continue to use fixed start and end times rather than representing a fixed commitment as a zero-slack Dynamic range.\n \n ### 9.3 MVP Task schedulability invariant\n \n A Task is schedulable in MVP if it has:\n@@ -2316,5 +2319,7 @@ The MVP criticality object is `criticality: { level, reason_refs, recovery_criti\n The MVP criticality object is `criticality: { level, reason_refs, recovery_critical, deadline_fragile, min_duration_seconds, user_confirmation_required, disposal_effect }`. `level` is one of `protected`, `flexible`, or `disposable`; `reason_refs` point to Objectives, Static Tasks, dependencies, preconditions, legitimization support, user commitments, or risk findings; `disposal_effect` is a short enum-or-string summary such as `none`, `lost_optional_value`, `missed_deadline_risk`, or `breaks_dependency`.\n \n+Task-level `allowed_time_range` is an admitted scheduling input for Dynamic Tasks. A decision envelope is later derived from a specific Plan placement, criticality, dependencies, preconditions, and repair policy so mobile can preserve or repair that Plan; it must not be treated as the source of the Task's allowed range.\n+\n The MVP decision envelope is `decision_envelope: { earliest_start, latest_start, latest_finish, movable_windows, min_duration_seconds, target_duration_seconds, dependency_refs, precondition_refs, affected_static_refs, affect_assumption_refs, resource_or_location_refs, repair_budget_seconds, stale_after }`. `movable_windows` is an ordered list of `{ start, end }` intervals in which the Task may begin or occupy time, depending on the Task's duration fields. Empty or absent movable windows mean the Task is fixed or must be handled by hard Calendar Logic rather than local motion.\n \n The shared conflict severity vocabulary is:\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -2722,5 +2722,5 @@ Open.\n ## UBU-Q0152: Allowed time range on Tasks\n \n-Status: Open Priority: MVP blocker Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0154, routine planning, retirement of occupies_capacity Resolved by: None Last scored: 2026-09-21 Scored from commit: None\n+Status: Solved Priority: MVP blocker Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0154, routine planning, retirement of occupies_capacity Resolved by: UBU-D0276 Last scored: 2026-09-21 Scored from commit: None\n \n Defining context: DESIGN.md \u00a79.2, DESIGN.md \u00a716.5, PLANNING_KERNEL_CONTRACT.md \u00a72, `UBU-D0256`, `UBU-D0275`.\n@@ -2745,6 +2745,6 @@ One-off Tasks carry an absolute `{ earliest_start, latest_finish }` range, which\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0276`: One-off Dynamic Tasks use a declared `allowed_time_range: { earliest_start, latest_finish }` absolute UTC interval. The entire planned occupied interval must fit inside it; violations are hard infeasibility, not scored spillover. Static Tasks continue to use fixed start/end times, and decision envelopes remain derived Plan/mobile-repair outputs rather than Task inputs. Routine instances get their concrete range from recurrence instantiation under `UBU-Q0154`; recurring local time-of-day windows and multiple movable windows are outside the one-off Task shape and must be resolved before kernel dispatch. The orchestrator maps the single range to `TaskSpec.window` by intersecting it with the request `time_window`. Local time-of-day rules are converted during routine instantiation with IANA time zone data; ambiguous or nonexistent local endpoints require recorded disambiguation or review before becoming UTC Task ranges. `occupies_capacity` is retired only after implementation evidence shows Dynamic Tasks and routine instances with ranges are placed, partially omitted, and hard-validated without the old gate.\n \n ---\ndiff --git a/PLANNING_KERNEL_CONTRACT.md b/PLANNING_KERNEL_CONTRACT.md\n--- a/PLANNING_KERNEL_CONTRACT.md\n+++ b/PLANNING_KERNEL_CONTRACT.md\n@@ -108,5 +108,7 @@ The existing `scoring_policy` weights (`utility_weight`, `robustness_weight`, `a\n-## 3. `TaskSpec` duration and correlation fields\n+## 3. `TaskSpec` duration, allowed-range, and correlation fields\n \n Each `TaskSpec` used by the planning kernel must include either a fixed duration model or a three-point shifted-log-normal duration model.\n+\n+Each Dynamic `TaskSpec` also carries a single hard `window: { earliest_start, latest_finish }` with RFC 3339 / ISO 8601 UTC timestamps. The CPU side builds it by intersecting the admitted Task's one-off `allowed_time_range`, or the concrete range on an instantiated routine Task, with `PlanningRequest.time_window`. If the intersection is empty or shorter than the Task's minimum possible duration, the Task is unplaceable for that request rather than soft-scored outside the window. Multiple allowed windows are not represented in Phase 1b `TaskSpec`; any future support requires a contract change or CPU-side occurrence/window selection before dispatch.\n \n ### Fixed duration\n",
    "commit_message": "Resolve UBU-Q0152 allowed Task ranges",
    "validation_notes": [
      "Patch validated with `git apply --check` against the current workspace.",
      "No files were modified while preparing this proposal; the diff is proposal text only."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0152"
    ],
    "decisions_added": [
      "UBU-D0276"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0152-P-gpt5-codex-001",
    "patch_applies": true,
    "allowlist_passed": true,
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md",
      "PLANNING_KERNEL_CONTRACT.md"
    ],
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
- Do not score your own provider's proposal unless explicitly asked for diagnostic self-score.

Return only JSON.
