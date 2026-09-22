# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0155`  
Question title: `Partial placement when the horizon cannot hold every Dynamic Task`  
Base commit: `e4aceaf28ce6f8f59e8ff1ae6faa5a8418b32d0b`

```markdown
## UBU-Q0155: Partial placement when the horizon cannot hold every Dynamic Task

Status: Open Priority: MVP blocker Phase: Phase 1b Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0153 Blocks: bounded-horizon planning with a real backlog Resolved by: None Last scored: 2026-09-22 Scored from commit: None

Defining context: DESIGN.md §15.2.2, DESIGN.md §16.3, PLANNING_KERNEL_CONTRACT.md §4, `UBU-D0275`, `UBU-D0277`, `UBU-D0279`.

### Question

How should planning behave when not every Dynamic Task fits the planning horizon? Today the kernel fails the whole skeleton when any single Task has insufficient window, so one oversized backlog item removes the entire Plan.

### Subquestions

1. **Local or blocking.** Which failures stay local to one Task, such as a Dynamic Task with insufficient window, and which block the Plan, such as Static collisions, dependency cycles, and precondition contradictions? §15.2.2 allows a skeleton failure outside the recommendation path to remain a warning.
2. **Deferral.** How is an unplaced Task reported, and what reason does it carry? Are its dependents deferred with it?
3. **Selection.** When capacity is short, which Tasks are left out: lowest value (`UBU-D0277`), latest deadline, or a fixed order? What is the deterministic tie-break? Mandatory routine occurrences are never left out; when they alone do not fit, the user triages.
4. **Contract.** Does `PlanningResponse` gain an additive list of unplaced Tasks, and does that require a contract version change? Should that change share one minor version with the split-policy change of `UBU-Q0157`?
5. **Horizon extension.** Should the orchestrator first retry with an extended horizon, the §15.2.2 safe alternative, before deferring Tasks?
6. **Surfacing.** How do unplaced Tasks appear in risk reports and next-action?
7. **Chunks and splitting.** Under chunked search (`UBU-D0279`), a unit longer than every chunk it may occupy cannot be placed even when the horizon holds enough free time in total. Is that reported as its own unplaced reason? If splittable Tasks are adopted (`UBU-Q0157`), may partial placement place part of a splittable Task and carry the remainder forward?

### Current direction

A Dynamic Task that cannot fit becomes a per-Task unplaced diagnostic, and the rest of the Plan proceeds. Static collisions and structural failures remain plan-blocking. Tasks are left out in order of lowest value, then latest deadline, then id; mandatory routine occurrences are never left out. `PlanningResponse` gains an additive unplaced-Task list under a minor contract version. Under chunked search, a unit is unplaced when no chunk can accept it, and the unplaced reason distinguishes a lack of total time from the lack of a large enough chunk.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "openai-gpt5-codex-UBU-Q0155-D0289",
    "provider_id": "openai",
    "model_name": "gpt-5-codex",
    "question_id": "UBU-Q0155",
    "base_commit": "e4aceaf28ce6f8f59e8ff1ae6faa5a8418b32d0b",
    "summary": "Resolve partial placement by making optional Dynamic Task capacity failures local unplaced diagnostics while preserving blocking behavior for Static, structural, mandatory routine, support, and required-path failures.",
    "rationale": "This keeps bounded-horizon planning useful with a real backlog: one oversized optional Task no longer removes the whole Plan, while mandatory work and hard model contradictions still require user triage. The proposal aligns selection with accepted request-local Task value, distinguishes chunk-size failures from aggregate-capacity failures, and reuses the Phase 1b minor contract version already reserved for split-policy changes.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md",
      "PLANNING_KERNEL_CONTRACT.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -4613,3 +4613,33 @@\n - `OPEN_QUESTIONS.md` marks `UBU-Q0159` solved and records the mandatory rule in `UBU-Q0155`.\n \n ---\n+\n+## UBU-D0289: Phase 1b partial placement keeps valid Plans when optional Dynamic work does not fit\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a715.2.2, \u00a716.3; PLANNING_KERNEL_CONTRACT.md \u00a71, \u00a74. Resolves `UBU-Q0155`. Refines `UBU-D0275`, `UBU-D0277`, `UBU-D0279`, `UBU-D0284`, and `UBU-D0288`.\n+\n+Phase 1b planning treats optional Dynamic backlog placement as best-effort once the Static, mandatory, dependency, and legitimacy baseline can be built. A single optional Dynamic Task with too little window or too little capacity no longer rejects the whole Plan. It becomes an unplaced Task diagnostic, and the response may still return Plan candidates with `status = partial`.\n+\n+The blocking/local boundary is:\n+\n+- Blocking: Static collisions, dependency cycles, impossible dependencies, hard precondition contradictions, missing required starting state, required Resource or External Event failures, deadline infeasibility for required work, and any inability to place mandatory routine occurrences or required support Tasks. These stop ordinary planning when they prevent a valid baseline or required recommendation path.\n+- Local: optional Dynamic Tasks, optional splittable remainders, and dependents whose only problem is that their prerequisite optional Task was unplaced. These are reported in `PlanningResponse.diagnostics.unplaced_tasks`.\n+\n+Before reporting an optional Task as unplaced solely because of horizon or window pressure, the CPU kernel may attempt one policy-bounded horizon extension when `extend_planning_horizon` is a safe alternative and the extension stays inside the request's privacy, payload, compute, and user-visible bounds. If extension is skipped or exhausted, the diagnostic records that fact. The planner must not silently expand beyond policy, hide the changed horizon, or use horizon extension to avoid reporting risk.\n+\n+Selection is deterministic. Static placements, mandatory routine occurrences, required support Tasks, and prerequisites of placed work are protected first. Optional Dynamic placement units are omitted in order of lowest `TaskSpec.value`, then latest deadline or `latest_finish`, then Task id. Missing deadlines sort after concrete deadlines. Dependents of an unplaced Task are not scheduled independently; they are deferred with reason `deferred_dependency` and name the unplaced prerequisite.\n+\n+Chunked search distinguishes why a Task is unplaced. `insufficient_total_capacity` means the eligible horizon lacks enough free time in aggregate. `no_eligible_chunk_large_enough` means aggregate time may exist, but no allowed chunk can hold the atomic unit. `outside_allowed_window`, `unsupported_split_policy`, `omitted_lower_value`, and `horizon_extension_limit` preserve the other common explanations. For splittable Tasks, partial placement may place valid pieces inside the horizon and carry the remainder forward as an unplaced diagnostic; scheduled-work value is proportional to placed work, while completion, effects, and outgoing dependencies still require the final piece.\n+\n+Unplaced diagnostics are user-visible planning facts. They feed risk reports, Plan explanation, and next-action context. Focus mode must not recommend an unplaced Task as executable; it may recommend an explicit triage action such as decompose the Task, reprioritize, extend the horizon, relax a window, skip or move a commitment, or remove/moot stale work.\n+\n+The contract change is additive and shares the Phase 1b minor contract version with the split-policy change from `UBU-D0284`: `PlanningResponse.diagnostics` gains `unplaced_tasks`, and `status = partial` is the normal successful response when at least one candidate exists but optional Dynamic work remains unplaced.\n+\n+Consequences:\n+\n+- `DESIGN.md` \u00a715.2.2 records the blocking/local boundary for insufficient optional Dynamic capacity.\n+- `DESIGN.md` \u00a716.3 records deterministic omission order, horizon-extension behavior, chunk-specific reasons, splittable carry-forward semantics, and next-action surfacing.\n+- `PLANNING_KERNEL_CONTRACT.md` \u00a71 records the shared minor version, and \u00a74 defines `diagnostics.unplaced_tasks` and `UnplacedTaskDiagnostic`.\n+- `OPEN_QUESTIONS.md` marks `UBU-Q0155` solved.\n+\n+---\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -2173,6 +2173,8 @@\n \n A skeleton failure becomes an immediate blocking prompt when it prevents any valid baseline for the current Calendar, current or next recommended Task, a Static Task placement, a hard dependency or precondition, deadline feasibility, or a required Resource or External Event. It may remain a normal planning warning only when a valid skeleton exists and the failed chain is outside the current recommendation path or future horizon, in which case UbU records the diagnostic and marks the relevant Calendar, explanation, or risk report stale.\n \n+Partial placement (`UBU-D0289`) refines the insufficient-window case for optional Dynamic Tasks. Once a valid baseline exists, failure to fit an optional Dynamic Task in the current horizon is a local unplaced diagnostic rather than a whole-Plan rejection. Structural failures, Static collisions, hard dependency or precondition contradictions, missing required state or resources, and any inability to place mandatory routine occurrences or required support Tasks remain blocking because they prevent a legitimate baseline or required path.\n+\n Full legitimization records the constraints it enforced and any support Tasks or buffers it inserted. Its hard result is `passed`, `failed`, or `needs_clarification`. Graded fields such as `legitimacy_score`, `legitimacy_margin`, and `legitimacy_delta_from_baseline` are advisory comparison signals, not permission to violate hard legitimacy.\n \n In Phase 1, `full_legitimize` implements the **affect-feasibility filter** (`UBU-D0236`). It computes each active dimension's sigmoid satisfaction from the AffectProfile and the affect observation, marks the candidate affect-feasible only when every dimension meets its threshold, and records per-dimension satisfaction, the `violated_dimensions`, and an `affect_margin` that is the legitimacy margin while affect is the only enforced constraint. It runs in `enforce` for user-facing planning and `warn_only` for onboarding, test, and stale or missing affect. Support-task insertion \u2014 breaks, recovery, meals, sleep, and transition buffers \u2014 and `semi_legitimize` are deferred follow-ons within full legitimization; Phase 1 legitimization is the affect filter only.\n@@ -2303,6 +2305,8 @@\n **Chunked search (`UBU-D0279`).** Phase 1b candidate construction partitions the horizon into chunks at capacity-occupying Static Tasks and other fixed placements. The search first assigns placement units to chunks. That is the coupled step, constrained by allowed ranges, dependencies, and state carried between chunks. It then orders each chunk's units independently and in parallel, sweeping chunks in time order and pruning at chunk boundaries. Rollouts evaluate chunks separately, sharing latent draws across chunks. The CPU reference path and the GPU engine share these semantics, and the current chunk is the unit of local repair.\n \n **Splittable Tasks (`UBU-D0284`).** Atomic Tasks and compiled decomposition segments occupy one chunk. A splittable Task may assign at most one piece to each eligible chunk, and every piece must remain inside the Task's allowed range. The chunk-assignment level chooses how much work from the Task enters each chunk; ordinary within-chunk ordering then places that piece. The first piece has no resume overhead, while each later piece reserves the Task's constant `resume_overhead_seconds` before doing more work. Rollouts draw one total work duration for the Task, spend it across pieces, and count a continuation into a later eligible chunk as feasible only when `max_pieces`, window, dependencies, and carried state still permit it. Splittable work earns proportional scheduled-work value for completed sampled work inside the horizon, but completion, effects, and outgoing dependencies still wait for the last piece. Phase 1b needs no additional interruption input beyond the split policy; later stochastic interruption modeling can use the same policy to distinguish continuation from restart.\n+\n+**Partial placement (`UBU-D0289`).** Candidate construction is best-effort for optional Dynamic work after Static placements, mandatory routine occurrences, baseline/support work, and prerequisites of placed work are protected. If capacity is short, optional units are omitted deterministically by lowest value, then latest deadline or `latest_finish`, then Task id. The response stays usable when at least one certifiable candidate remains, with omitted units reported as unplaced diagnostics; their dependents are deferred rather than independently scheduled. The CPU may make one policy-bounded horizon-extension attempt before deferral when extension is an allowed safe alternative, but it records whether extension was skipped, attempted, or exhausted. Under chunked search, unplaced reasons distinguish not enough total free capacity from no eligible chunk large enough for an atomic unit. Splittable Tasks may be partially placed across eligible chunks and carry remaining work forward, but completion, effects, and outgoing dependencies wait for the final piece. Unplaced diagnostics feed Plan explanation, risk reports, and next-action context; focus mode may recommend a triage action for unplaced work, but not the unplaced work itself.\n \n **Sweep mechanics and streaming (`UBU-D0280`, `UBU-D0281`).** The sweep expands each branch into up to K alternatives per chunk and keeps the best B partial plans after each chunk depth. It merges branches that reach the same state, and assigns the most constrained units first. Interactive requests stream certified chunk results, so the user can act on the first chunk while later chunks are still being searched; a streamed chunk stays provisional until the user starts it. Batch requests return only the final response.\n \ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -2810,7 +2810,7 @@\n \n ## UBU-Q0155: Partial placement when the horizon cannot hold every Dynamic Task\n \n-Status: Open Priority: MVP blocker Phase: Phase 1b Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0153 Blocks: bounded-horizon planning with a real backlog Resolved by: None Last scored: 2026-09-22 Scored from commit: None\n+Status: Solved Priority: MVP blocker Phase: Phase 1b Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: UBU-Q0153 Blocks: bounded-horizon planning with a real backlog Resolved by: UBU-D0289 Last scored: 2026-09-22 Scored from commit: None\n \n Defining context: DESIGN.md \u00a715.2.2, DESIGN.md \u00a716.3, PLANNING_KERNEL_CONTRACT.md \u00a74, `UBU-D0275`, `UBU-D0277`, `UBU-D0279`.\n \n@@ -2834,7 +2834,7 @@\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0289`: Phase 1b uses partial placement for optional Dynamic Tasks. A valid baseline Plan is allowed to proceed when optional Dynamic backlog items cannot fit; each omitted or partially carried-forward Task is returned in `PlanningResponse.diagnostics.unplaced_tasks`, and the response status is `partial` when candidates exist but that list is non-empty. Blocking failures remain blocking when they prevent the baseline or a required recommendation path: Static collisions, dependency cycles, hard precondition contradictions, missing required starting state/resource/external event, deadline infeasibility for required work, and mandatory routine or support Tasks that cannot be placed. The CPU may attempt one policy-bounded horizon extension before reporting optional Tasks as unplaced; skipped or exhausted extension is recorded on the diagnostic. Capacity omission is deterministic: protect Static placements, mandatory routine occurrences, baseline/support work, and prerequisites of placed work, then omit optional units by lowest `TaskSpec.value`, latest deadline or `latest_finish`, and task id. Dependents of an unplaced Task are deferred with reason `deferred_dependency`. Chunked search distinguishes insufficient total free capacity from no eligible chunk large enough for an atomic unit. Splittable Tasks may receive some pieces inside the horizon and carry the remainder forward as an unplaced diagnostic; completion, effects, and outgoing dependencies still wait for the final piece. Unplaced Tasks appear in risk reports, Plan explanation, and next-action context, but the next-action recommendation never selects an unplaced Task except as an explicit triage action such as decompose, reprioritize, extend the horizon, or change commitments. The additive response field shares the Phase 1b minor contract version with the split-policy change.\n \n ---\n \ndiff --git a/PLANNING_KERNEL_CONTRACT.md b/PLANNING_KERNEL_CONTRACT.md\n--- a/PLANNING_KERNEL_CONTRACT.md\n+++ b/PLANNING_KERNEL_CONTRACT.md\n@@ -16,7 +16,7 @@\n \n ### Required envelope fields\n \n-- `schema_version`: semantic contract version string. Phase 1 baseline: `planning-kernel-contract/0.1`.\n+- `schema_version`: semantic contract version string. Phase 1 baseline: `planning-kernel-contract/0.1`; Phase 1b split-policy and partial-placement additions share `planning-kernel-contract/0.2`.\n - `planner_version`: implementation version string for the planner backend that produced or consumed the object.\n - `request_id`: stable identifier generated by the CPU kernel before dispatch. Enables audit, deduplication, and replay.\n - `effective_time`: logical planning start time in RFC 3339 / ISO 8601 UTC form.\n@@ -220,7 +220,7 @@\n - `request_id`\n - `rng_seed_echo`\n - `generated_at`\n-- `status`: enum, one of `ok`, `partial`, `rejected`, `engine_error`.\n+- `status`: enum, one of `ok`, `partial`, `rejected`, `engine_error`. `partial` means at least one PlanCandidate is returned but optional Dynamic work is reported in `diagnostics.unplaced_tasks`.\n - `engine_provenance`:\n   - `backend_kind`: enum, one of `cpu_reference`, `gpu_worker`.\n   - `invocation_kind`: enum, one of `in_process_cpu`, `persistent_python_worker`.\n@@ -240,6 +240,7 @@\n   - `rejection_counts_by_reason`\n   - `warnings`\n   - `skeleton_failure_diagnostics`: list of `SkeletonFailureDiagnostic` objects. Empty when skeletonization and prerequisite validation do not fail; required when `status = rejected` because no skeleton baseline exists.\n+  - `unplaced_tasks`: list of `UnplacedTaskDiagnostic` objects. Empty when every eligible optional Dynamic Task is fully placed; required when `status = partial`. Mandatory routine occurrences and other required baseline work are not selected for omission; inability to place them remains a blocking skeleton or legitimacy diagnostic.\n   - `probability_quality`: enum, one of `full`, `degraded_numeric_jitter`, `degraded_independence`, `not_estimated`.\n   - `coverage_scope`: optional enum, one of `reactive_horizon`, `full_window`, `repair_scope`.\n   - `coverage_estimate`: optional numeric branch-coverage estimate.\n@@ -271,6 +272,23 @@\n - `user_facing_summary`: short non-blaming explanation text.\n \n Allowed `safe_alternatives.action` values are `provide_starting_state`, `mark_state_already_satisfied`, `add_prerequisite_task`, `relax_deadline`, `extend_planning_horizon`, `remove_or_moot_task`, `choose_alternate_technique_or_task_path`, `wait_for_external_event`, and `manual_decision`.\n+\n+### `UnplacedTaskDiagnostic`\n+\n+An unplaced Task diagnostic reports optional Dynamic work that was omitted, deferred, or only partially placed while the rest of the Plan proceeds. Minimum fields:\n+\n+- `diagnostic_id`\n+- `task_ref`\n+- `reason`: enum, one of `insufficient_total_capacity`, `no_eligible_chunk_large_enough`, `outside_allowed_window`, `omitted_lower_value`, `deferred_dependency`, `unsupported_split_policy`, `horizon_extension_limit`.\n+- `deferred_by_task_refs`: list of Task refs that caused `deferred_dependency`, empty otherwise.\n+- `affected_dependent_task_refs`: list of dependent Task refs also deferred by this diagnostic.\n+- `eligible_chunk_refs`: list of chunks or deterministic chunk-range identifiers considered for the Task.\n+- `placed_piece_count`: optional integer for splittable Tasks; absent for atomic Tasks.\n+- `remaining_work_seconds_estimate`: optional numeric estimate for splittable carry-forward work.\n+- `selection_rank`: optional object recording the omission ordering inputs: `value`, optional `deadline_or_latest_finish`, and `task_id`.\n+- `horizon_extension`: enum, one of `not_applicable`, `skipped_by_policy`, `attempted_exhausted`.\n+- `safe_alternatives`: list of alternatives, each with `action`, `label`, `requires_user_input`, `resulting_change_summary`, and optional candidate mutation or repair ref. Unplaced-task action values are `decompose_task`, `reprioritize_task`, `extend_planning_horizon`, `relax_task_window`, `remove_or_moot_task`, and `manual_decision`.\n+- `user_facing_summary`: short non-blaming explanation text for risk reports, Plan explanation, and next-action triage.\n \n ### `PlanCandidate`\n \n",
    "commit_message": "Resolve UBU-Q0155 partial placement semantics",
    "validation_notes": [
      "Generated a minimal documentation-only unified diff against the current workspace at base commit e4aceaf28ce6f8f59e8ff1ae6faa5a8418b32d0b.",
      "Validated the generated patch with git apply --check against the current workspace context.",
      "No code or schema-generation tests were run because this proposal changes only canonical design and contract documents."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0155"
    ],
    "decisions_added": [
      "UBU-D0289"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "openai-gpt5-codex-UBU-Q0155-D0289",
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
