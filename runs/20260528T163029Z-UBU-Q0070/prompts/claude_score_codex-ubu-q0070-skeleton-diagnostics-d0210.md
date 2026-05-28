# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `codex`

## Selected question

Question ID: `UBU-Q0070`  
Question title: `Skeleton Plan failure diagnostics and user clarification flow`  
Base commit: `91db4ee5ff116480e5d3d0a7bead6314ddc38793`

```markdown
## UBU-Q0070: Skeleton Plan failure diagnostics and user clarification flow

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0014, UBU-Q0015, UBU-Q0016 Blocks: robust planning failure handling Resolved by: None Last scored: 2026-05-28 Scored from commit: None

### Question

When skeleton Plan generation fails, what exact diagnostic payload and user clarification flow should UbU produce?

### Subquestions

1. Which failure classes are required for MVP: missing starting state, impossible dependency, cyclic dependency, Static Task collision, insufficient Calendar window, unavailable resource, blocked External Event, or unknown precondition?
2. How should UbU explain the failed causal chain without overwhelming the user?
3. Which alternatives can UbU safely suggest: relax deadline, remove Task, add prerequisite, mark state already satisfied, choose alternate Technique, extend planning horizon, or ask for manual decision?
4. When should this become an immediate blocking prompt instead of a normal planning warning?

### Current direction

Skeleton Plan failure is a critical model-consistency failure. UbU should not proceed as if the Plan merely needs optimization. It should explain the cause and request clarification or a user choice.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "codex-ubu-q0070-skeleton-diagnostics-d0210",
    "provider_id": "codex",
    "model_name": "gpt-5",
    "question_id": "UBU-Q0070",
    "base_commit": "91db4ee5ff116480e5d3d0a7bead6314ddc38793",
    "summary": "Resolve UBU-Q0070 by defining MVP skeleton Plan failure classes, the SkeletonFailureDiagnostic payload, safe clarification alternatives, explanation budget, and blocking-vs-warning prompt policy.",
    "rationale": "The existing design already says skeletonization must halt on missing or contradictory causal foundations, but it lacks an implementation-facing diagnostic contract. This proposal adds a bounded payload to the planning contract, mirrors the user-facing flow in DESIGN.md, records the accepted decision as UBU-D0210, and tombstones the open question.",
    "changed_files": [
      "DESIGN.md",
      "PLANNING_KERNEL_CONTRACT.md",
      "DECISIONS.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -1983,6 +1983,18 @@\n - ordered prerequisite chains and the dependency or precondition refs that justify them;\n - initial UniverseState assumptions, including known true, known false, assumed, and unresolved planner-relevant facts;\n - unsatisfied dependency diagnostics with the failing Task, missing state, causal chain, and clarification alternatives when available.\n+\n+Skeleton failure diagnostics are blocking when no valid skeleton baseline can be produced for the current Calendar scope. A diagnostic records:\n+\n+- `failure_class`: one of `missing_starting_state`, `impossible_dependency`, `cyclic_dependency`, `static_task_collision`, `insufficient_calendar_window`, `unavailable_resource`, `blocked_external_event`, or `unknown_precondition`;\n+- severity, affected Task refs, missing or conflicting state, relevant Static Task or time-window refs, initial UniverseState assumption refs, source or External Event refs, and the shortest causal chain needed to explain the failure;\n+- safe clarification alternatives, each with an action enum, required user input, and the state mutation or Plan repair it would permit.\n+\n+The user-facing explanation shows the failed Task or state, the immediate cause, and at most three causal-chain steps by default, with full inspector detail available. Wording frames the problem as an incomplete or contradictory model, not user failure.\n+\n+Safe alternatives are limited to: provide or correct starting state, mark the required state already satisfied, add a prerequisite Task, relax a deadline or Static Task constraint, extend the planning horizon, remove or moot the blocked Task, choose an already-modeled alternate Technique or Task path, wait for or record an External Event, or ask for manual decision. UbU must not invent a new canonical Resource, Skill, Technique, Preference, or external fact merely to repair skeletonization.\n+\n+A skeleton failure becomes an immediate blocking prompt when it prevents any valid baseline for the current Calendar, current or next recommended Task, a Static Task placement, a hard dependency or precondition, deadline feasibility, or a required Resource or External Event. It may remain a normal planning warning only when a valid skeleton exists and the failed chain is outside the current recommendation path or future horizon, in which case UbU records the diagnostic and marks the relevant Calendar, explanation, or risk report stale.\n \n Legitimization records the constraints it enforced and any support Tasks or buffers it inserted. The minimum legitimization output includes affect limits in `user_mode`, recovery, breaks, meals, sleep, rest, transition buffers, setup and teardown time, context-switch limits, slack thresholds, dependency-fragility thresholds, and a threshold result of `passed`, `failed`, or `needs_clarification`.\n \ndiff --git a/PLANNING_KERNEL_CONTRACT.md b/PLANNING_KERNEL_CONTRACT.md\n--- a/PLANNING_KERNEL_CONTRACT.md\n+++ b/PLANNING_KERNEL_CONTRACT.md\n@@ -192,6 +192,7 @@\n   - `n_finalists_rollout`\n   - `rejection_counts_by_reason`\n   - `warnings`\n+  - `skeleton_failure_diagnostics`: list of `SkeletonFailureDiagnostic` objects. Empty when skeletonization and prerequisite validation do not fail; required when `status = rejected` because no skeleton baseline exists.\n   - `probability_quality`: enum, one of `full`, `degraded_numeric_jitter`, `degraded_independence`, `not_estimated`.\n   - `coverage_scope`: optional enum, one of `reactive_horizon`, `full_window`, `repair_scope`.\n   - `coverage_estimate`: optional numeric branch-coverage estimate.\n@@ -199,6 +200,28 @@\n   - `coverage_threshold_used`: optional numeric threshold used for regeneration decisions.\n   - `coverage_below_threshold`: optional boolean.\n   - `compute_telemetry`: optional backend timing and resource summary, including `duration_ms` when available.\n+\n+### `SkeletonFailureDiagnostic`\n+\n+A skeleton failure diagnostic is the bounded payload for halted or degraded skeleton generation. Minimum fields:\n+\n+- `diagnostic_id`\n+- `severity`: enum, one of `blocking`, `warning`.\n+- `failure_class`: enum, one of `missing_starting_state`, `impossible_dependency`, `cyclic_dependency`, `static_task_collision`, `insufficient_calendar_window`, `unavailable_resource`, `blocked_external_event`, `unknown_precondition`.\n+- `primary_task_ref`: optional when the failure is Calendar-wide.\n+- `affected_task_refs`: list of Task refs.\n+- `missing_or_conflicting_state`: object with `target`, `predicate`, optional `expected`, optional `observed`, and `state_status` enum, one of `missing`, `known_false`, `conflicting`, `unresolved`, `denied_by_policy`.\n+- `causal_chain`: ordered list of compact fragments with Task ref, dependency or precondition ref, required state, optional producer Task ref, and status.\n+- `time_window_refs`: list of relevant Calendar or deadline refs.\n+- `static_task_conflict_refs`: list of Static Task refs for fixed-time conflicts.\n+- `initial_universe_state_ref`: optional UniverseState or Snapshot ref.\n+- `source_refs`: list of provenance or External Reference refs.\n+- `external_event_refs`: list of relevant External Event refs.\n+- `safe_alternatives`: list of alternatives, each with `action`, `label`, `requires_user_input`, `resulting_change_summary`, and optional candidate mutation or repair ref.\n+- `prompt_policy`: enum, one of `immediate_blocking_prompt`, `planning_warning`.\n+- `user_facing_summary`: short non-blaming explanation text.\n+\n+Allowed `safe_alternatives.action` values are `provide_starting_state`, `mark_state_already_satisfied`, `add_prerequisite_task`, `relax_deadline`, `extend_planning_horizon`, `remove_or_moot_task`, `choose_alternate_technique_or_task_path`, `wait_for_external_event`, and `manual_decision`.\n \n ### `PlanCandidate`\n \ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -4409,3 +4409,38 @@\n - Ethereum fits naturally as the settlement and trust layer beneath a task-driven Resource and Skill exchange.\n \n ---\n+\n+## UBU-D0210: Skeleton Plan failures use bounded diagnostics and blocking clarification\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a715.2.2; PLANNING_KERNEL_CONTRACT.md \u00a74\n+\n+Resolved question: `UBU-Q0070`.\n+\n+Skeleton generation must not continue into ordinary optimization when no valid skeleton baseline exists for the current Calendar scope. It returns a bounded `SkeletonFailureDiagnostic` and asks for clarification or a user choice.\n+\n+MVP `failure_class` values:\n+\n+- `missing_starting_state`;\n+- `impossible_dependency`;\n+- `cyclic_dependency`;\n+- `static_task_collision`;\n+- `insufficient_calendar_window`;\n+- `unavailable_resource`;\n+- `blocked_external_event`;\n+- `unknown_precondition`.\n+\n+The diagnostic payload records diagnostic ID, severity, failure class, affected Task refs, missing or conflicting state, relevant time-window or Static Task refs, initial UniverseState ref, source or External Event refs, a compact causal chain, safe alternatives, prompt policy, and a short non-blaming user-facing summary.\n+\n+Default explanation budget is the failed Task or state, the immediate cause, and at most three causal-chain steps. Full dependency/precondition detail stays one inspector action away.\n+\n+Safe alternatives are limited to providing or correcting starting state, marking the state already satisfied, adding a prerequisite Task, relaxing a deadline or Static constraint, extending the planning horizon, removing or mooting the blocked Task, choosing an already-modeled alternate Technique or Task path, waiting for or recording an External Event, or manual decision. UbU must not invent canonical Resources, Skills, Techniques, Preferences, or external facts just to repair skeletonization.\n+\n+Use `immediate_blocking_prompt` when the failure prevents a valid baseline for the current Calendar, current or next recommendation, Static Task placement, hard dependency/precondition, deadline feasibility, required Resource, or required External Event. Use `planning_warning` only when a valid skeleton still exists and the failed chain is outside the current recommendation path or future horizon; then record the diagnostic and mark the relevant Calendar, explanation, or risk report stale.\n+\n+**Consequences:**\n+\n+- Skeleton failure is model repair, not low-quality optimization.\n+- User clarification can produce explicit state updates, Task changes, Plan repair, or manual decisions through normal admission and Log paths.\n+- Implementations can validate failure handling through the `SkeletonFailureDiagnostic` payload in `PLANNING_KERNEL_CONTRACT.md`.\n+\n+---\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -1035,27 +1035,8 @@\n \n ## UBU-Q0070: Skeleton Plan failure diagnostics and user clarification flow\n \n-Status: Open Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0014, UBU-Q0015, UBU-Q0016 Blocks: robust planning failure handling Resolved by: None Last scored: 2026-05-28 Scored from commit: None\n-\n-### Question\n-\n-When skeleton Plan generation fails, what exact diagnostic payload and user clarification flow should UbU produce?\n-\n-### Subquestions\n-\n-1. Which failure classes are required for MVP: missing starting state, impossible dependency, cyclic dependency, Static Task collision, insufficient Calendar window, unavailable resource, blocked External Event, or unknown precondition?\n-2. How should UbU explain the failed causal chain without overwhelming the user?\n-3. Which alternatives can UbU safely suggest: relax deadline, remove Task, add prerequisite, mark state already satisfied, choose alternate Technique, extend planning horizon, or ask for manual decision?\n-4. When should this become an immediate blocking prompt instead of a normal planning warning?\n-\n-### Current direction\n-\n-Skeleton Plan failure is a critical model-consistency failure. UbU should not proceed as if the Plan merely needs optimization. It should explain the cause and request clarification or a user choice.\n-\n-### Resolution\n-\n-Open.\n-\n+Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0014, UBU-Q0015, UBU-Q0016 Blocks: robust planning failure handling Resolved by: UBU-D0210 Last scored: 2026-05-28 Scored from commit: None\n+Resolved. See UBU-D0210.\n ---\n \n ## UBU-Q0071: Legitimization and semi-legitimization cost model\n",
    "commit_message": "Resolve skeleton Plan failure diagnostics",
    "validation_notes": [
      "Generated against the current workspace content corresponding to the requested canonical files.",
      "Validated the generated patch with git apply --check via stdin.",
      "Changes are documentation/contract only and limited to DESIGN.md, DECISIONS.md, OPEN_QUESTIONS.md, and PLANNING_KERNEL_CONTRACT.md.",
      "Human review remains required because UBU-Q0070 is marked Human approval required."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0070"
    ],
    "decisions_added": [
      "UBU-D0210"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "codex-ubu-q0070-skeleton-diagnostics-d0210",
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
