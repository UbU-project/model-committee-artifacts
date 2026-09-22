# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0157`  
Question title: `Splittable Tasks and resume overhead`  
Base commit: `d2bc730b2920210cc9e774ff7e509541ea451b7a`

```markdown
## UBU-Q0157: Splittable Tasks and resume overhead

Status: Open Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: save-and-continue planning, interruption-aware rollouts Resolved by: None Last scored: 2026-09-21 Scored from commit: None

Defining context: DESIGN.md §9.4, DESIGN.md §16.3, PLANNING_KERNEL_CONTRACT.md §3, `UBU-D0276`, `UBU-D0277`, `UBU-D0278`, `UBU-D0279`.

### Question

How does planning represent and place Tasks whose work may be split into pieces across chunks, with a modest resume overhead for each resumption, while Tasks that cannot be split stay atomic?

### Subquestions

1. **Policy shape.** Is the Task input a split policy that is either `atomic`, the default, or `splittable { min_piece, resume_overhead, max_pieces }`? Which fields are required? How are defaults chosen, for example by category, with the advisor proposing them for review?
2. **Overhead model.** Is the resume overhead constant for every resumed piece, or does it depend on the time since the previous piece? Does resuming also carry an affect cost?
3. **Piece placement.** Are pieces restricted to at most one per chunk (`UBU-D0279`), excluding splits inside a chunk because they only add overhead? Must every piece lie inside the Task's allowed range (`UBU-D0276`)?
4. **Relation to segments.** Decomposition segments (`UBU-D0278`) are pause points fixed at decomposition time and stay atomic placement units. Does a split policy apply only to individual Tasks, including decomposition children?
5. **Contract.** The split policy would change the kernel contract:
   - `TaskSpec` gains the split policy;
   - Plan steps gain a piece index and a piece count;
   - validation accepts several non-overlapping steps for one Task;
   - dependencies attach to a Task's first and last pieces;
   - rollouts draw one total duration per Task and spend it across that Task's pieces.

   Does this change share one minor contract version with partial placement (`UBU-Q0155`)?
6. **Uncertainty.** A splittable Task that overruns can continue in a later chunk at the cost of one resumption, instead of colliding with the next fixed placement. How do rollouts account for that, and how does it change Plan probability?
7. **Interruptions.** Stochastic external-event modeling is deferred to Phase 2. Once it exists, an interruption during splittable work costs one resumption, while an interruption during atomic work forces a restart. Does Phase 1b need any interruption input, or only the split policy that the later modeling depends on?
8. **Progress.** Planned pieces are Plan steps of one Task. When execution stops partway, how is progress recorded, through Log evidence and a remaining-work estimate on the Task, without the §9.4 structural replacement that a Container implies?
9. **Partial value.** Does progress on a splittable Task within the horizon earn proportional value (`UBU-D0277`), or does value accrue only when the Task is completed?
10. **Projection.** How are pieces shown on projection surfaces, for example as separate Google Calendar events titled "(2/3)"?

### Current direction

A Task's split policy is `atomic` by default. A splittable Task declares a minimum piece, a constant resume overhead added to each resumed piece, and a maximum piece count. Pieces are placed at most one per chunk and inside the Task's allowed range. The chunk-assignment level of `UBU-D0279` therefore decides how much of each splittable Task goes into each chunk, and ordering within chunks is unchanged. Decomposition segments stay atomic. Planned pieces are Plan steps of one Task, and an actual stop partway records progress instead of restructuring the Task. The contract change shares one minor version with partial placement. Splitting is not required for the switch and is implemented after chunked search lands.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "openai-gpt5-codex-UBU-Q0157-001",
    "provider_id": "openai",
    "model_name": "gpt-5-codex",
    "question_id": "UBU-Q0157",
    "base_commit": "d2bc730b2920210cc9e774ff7e509541ea451b7a",
    "summary": "Resolve splittable Tasks as explicit per-Task split policies with atomic default, required splittable fields, constant resume overhead, at most one piece per chunk, same-Task progress tracking, proportional scheduled-work value, and split-aware kernel piece metadata/rollout semantics.",
    "rationale": "The proposal keeps ordinary Tasks and decomposition segments atomic by default while giving long resumable work a bounded, auditable way to span chunks. Required fields avoid hidden category defaults, constant overhead is simple enough for Phase 1b, and first/last-piece dependency semantics preserve existing Task-level effects. Recording progress on the same Task supports save-and-continue without invoking Container restructuring.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md",
      "PLANNING_KERNEL_CONTRACT.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -4490,5 +4490,33 @@ Consequences:\n - `DESIGN.md` \u00a716.10 replaces the in-process typed-call requirement with a persistent local Python worker boundary and records fallback, provenance, reproducibility, chunking, and streaming consequences.\n - `PLANNING_KERNEL_CONTRACT.md` records delivery mode, engine provenance, worker frame semantics, and parity expectations.\n - `OPEN_QUESTIONS.md` marks `UBU-Q0156` solved.\n \n ---\n+\n+## UBU-D0284: Phase 1b splittable Tasks are per-Task chunk pieces with constant resume overhead\n+\n+**Status:** Accepted -> DESIGN.md \u00a79.2, \u00a79.4, \u00a716.3; PLANNING_KERNEL_CONTRACT.md \u00a73, \u00a74, \u00a75. Resolves `UBU-Q0157`.\n+\n+Every schedulable Task has a split policy. The default is `atomic`, which preserves the existing rule that the Task must be planned as one placement unit inside one chunk. A Task may instead declare `splittable` with all three fields required: `min_piece_seconds`, `resume_overhead_seconds`, and `max_pieces`. The minimum piece and resume overhead are non-negative integer seconds, with `min_piece_seconds` positive; `max_pieces` is an integer of at least 2. The store does not silently infer these fields. Advisors may propose category-based defaults as reviewable advisory candidates, but admission records the explicit policy on the Task.\n+\n+For Phase 1b, resume overhead is constant. The first piece consumes only work duration; every later planned or rollout continuation consumes one `resume_overhead_seconds` interval before doing additional work. There is no separate Phase 1b affect cost for resumption. Later interruption modeling may add affect consequences, but the split policy is the only interruption-facing Task input needed now.\n+\n+Pieces are a placement shape for one Task, not new WorkItems and not a Container replacement. A splittable Task may contribute at most one piece to any chunk, and every piece must lie inside the Task's hard `allowed_time_range`/`TaskSpec.window`. Splitting inside a chunk is invalid because it only adds overhead without expanding feasibility. The chunk-assignment level decides how many seconds of a splittable Task are assigned to each eligible chunk, then the ordinary within-chunk ordering places that chunk's piece. Atomic Tasks and compiled decomposition segments remain single placement units.\n+\n+The split policy applies to individual Tasks, including decomposition child Tasks. A compiled decomposition segment remains atomic: if a child inside an admitted multi-child segment needs to be split independently, admission must put segment boundaries around that child or reject/ask for edits. The kernel never splits the interior of a compiled segment.\n+\n+Plan steps may contain several non-overlapping placements for the same Task. Each piece records `piece_index` and `piece_count`; atomic placements use `1/1`. Dependencies, preconditions, and effects attach to the Task as a whole: incoming dependencies and preconditions gate the first piece, and outgoing dependencies and effects become satisfied only after the last piece completes. Projection surfaces may show pieces as separate external objects with stable piece ids and titles such as `(2/3)`, but all pieces point back to the same canonical Task and projection edits reconcile by piece.\n+\n+Rollouts draw one total work duration per Task and spend that sampled work across the Task's pieces. Resume overhead is deterministic occupied time added before each resumed piece. If a piece overruns its planned work budget, a splittable Task may continue in a later eligible chunk when remaining `max_pieces`, window, dependencies, and carried state permit; otherwise the rollout is infeasible in the same way an atomic overrun that collides with a fixed placement is infeasible. This improves Plan probability only through the measured frequency of feasible continuations under the same rollout draws.\n+\n+Progress on a stopped splittable Task is recorded as execution evidence and a remaining-work estimate on the same Task. It is not a \u00a79.4 structural replacement and does not create a Container merely because work paused. For scoring, Phase 1b gives splittable work proportional scheduled-work value for the fraction of sampled total work completed inside the horizon, while dependency satisfaction, effects, completion state, and completion-dependent value still require the last piece. Atomic Tasks earn value only when their one placement is completed or scheduled according to the ordinary candidate semantics.\n+\n+The kernel contract changes share the same Phase 1b minor contract version reserved for partial placement (`UBU-Q0155`): `TaskSpec` gains the split policy, PlanCandidate schedules gain piece metadata, validation accepts multiple ordered non-overlapping pieces for one Task, and rollout consumes one sampled Task duration across those pieces. Splitting is not required for the switch itself and may land after chunked search, provided unsupported splittable Tasks are rejected or treated as unplaceable with an explicit diagnostic rather than silently made atomic.\n+\n+Consequences:\n+\n+- `DESIGN.md` \u00a79.2 records the Task split policy; \u00a79.4 records the relation to decomposition segments; \u00a716.3 records chunk placement and rollout semantics.\n+- `PLANNING_KERNEL_CONTRACT.md` \u00a73 records `TaskSpec.split_policy`; \u00a74 records PlanCandidate piece metadata; \u00a75 records split-aware tensor and rollout semantics.\n+- `OPEN_QUESTIONS.md` marks `UBU-Q0157` solved.\n+\n+---\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -1493,6 +1493,8 @@ Dynamic Tasks may have:\n \n **Phase 1b allowed range (`UBU-D0276`).** A one-off Dynamic Task may carry `allowed_time_range: { earliest_start, latest_finish }`, expressed as absolute UTC instants. The planner must place the Task's entire occupied interval inside that hard range by mapping it to the kernel `TaskSpec.window`. The range is a declared Task input, not a derived decision envelope, due date, deadline, or recurring local time-of-day rule. Routine instances receive their concrete range when recurrence instantiates them. Static Tasks continue to use fixed start and end times rather than representing a fixed commitment as a zero-slack Dynamic range.\n \n+**Phase 1b split policy (`UBU-D0284`).** A schedulable Task has `split_policy`, defaulting to `atomic`. A splittable Task declares `splittable { min_piece_seconds, resume_overhead_seconds, max_pieces }`; all three fields are admitted Task inputs rather than silent category defaults. Advisors may propose defaults for review, but admission records the explicit policy. Pieces are Plan placements of the same Task, not child WorkItems, and actual partial execution is recorded as Log evidence plus a remaining-work estimate on that Task rather than a Container replacement.\n+\n ### 9.3 MVP Task schedulability invariant\n \n A Task is schedulable in MVP if it has:\n@@ -1578,6 +1580,8 @@ Fields that make a schedulable action executable belong on child Tasks: duration\n \n Within a segment, children are planned and repaired as strictly back-to-back work. Phase 1b does not model a maximum-gap edge between children; any admitted gap, wait, recovery, handoff, or pause is represented by a separate Task or by a segment boundary.\n \n+Task split policy applies to individual Tasks, including decomposition child Tasks, but an admitted compiled segment remains an atomic placement unit. If a child in a multi-child segment needs independent splitting, review must create segment boundaries around that child or reject the decomposition until edited; the planner does not split the interior of a compiled segment.\n+\n Phase 1b keeps the planning kernel contract unchanged. Before kernel dispatch, the orchestrator compiles each admitted segment into one temporary placement unit whose duration is the exact sum of fixed child durations, or for stochastic durations the conservative component-wise sum of minimum, mode, and p95 with one rollout sample for the segment. After the kernel places the unit, the orchestrator expands it back into child Plan entries in Container order with contiguous offsets. A native no-gap kernel edge is deferred until measurement shows the conservative segment approximation distorts rollout results.\n \n Children with hard static timing, allowed time ranges, preconditions, dependencies, or effects that cannot safely apply to the whole segment force a segment boundary before that child, or after it when the constraint belongs only to prior work. Shared hard constraints may constrain the compiled segment as a whole. If no sequence of boundaries can preserve the child constraint without creating an internal gap or making the proposal unschedulable, admission rejects the decomposition or requires review edits before admission.\n@@ -2282,6 +2286,8 @@ Execution profiles are additive rather than mutually exclusive. The greedy mean-\n \n **Chunked search (`UBU-D0279`).** Phase 1b candidate construction partitions the horizon into chunks at capacity-occupying Static Tasks and other fixed placements. The search first assigns placement units to chunks. That is the coupled step, constrained by allowed ranges, dependencies, and state carried between chunks. It then orders each chunk's units independently and in parallel, sweeping chunks in time order and pruning at chunk boundaries. Rollouts evaluate chunks separately, sharing latent draws across chunks. The CPU reference path and the GPU engine share these semantics, and the current chunk is the unit of local repair.\n \n+**Splittable Tasks (`UBU-D0284`).** Atomic Tasks and compiled decomposition segments occupy one chunk. A splittable Task may assign at most one piece to each eligible chunk, and every piece must remain inside the Task's allowed range. The chunk-assignment level chooses how much work from the Task enters each chunk; ordinary within-chunk ordering then places that piece. The first piece has no resume overhead, while each later piece reserves the Task's constant `resume_overhead_seconds` before doing more work. Rollouts draw one total work duration for the Task, spend it across pieces, and count a continuation into a later eligible chunk as feasible only when `max_pieces`, window, dependencies, and carried state still permit it. Splittable work earns proportional scheduled-work value for completed sampled work inside the horizon, but completion, effects, and outgoing dependencies still wait for the last piece. Phase 1b needs no additional interruption input beyond the split policy; later stochastic interruption modeling can use the same policy to distinguish continuation from restart.\n+\n **Sweep mechanics and streaming (`UBU-D0280`, `UBU-D0281`).** The sweep expands each branch into up to K alternatives per chunk and keeps the best B partial plans after each chunk depth. It merges branches that reach the same state, and assigns the most constrained units first. Interactive requests stream certified chunk results, so the user can act on the first chunk while later chunks are still being searched; a streamed chunk stays provisional until the user starts it. Batch requests return only the final response.\n \n Plan probability is represented internally as probability metadata rather than only as a display scalar. The minimum record contains a display scalar, a log probability for stable computation, an optional probability interval, provenance over the modeled probabilistic inputs, and correlation-group or scenario references when inputs are not independent. Implementations may multiply probabilities only for inputs declared independent. Correlated or unknown relationships must use joint scenarios, shared random variables, correlation groups, or conservative intervals rather than pretending independence.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -2871,7 +2871,7 @@ The `PlanningRequest`/`PlanningResponse` boundary, CPU certification of every se\n \n ## UBU-Q0157: Splittable Tasks and resume overhead\n \n-Status: Open Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: save-and-continue planning, interruption-aware rollouts Resolved by: None Last scored: 2026-09-21 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: save-and-continue planning, interruption-aware rollouts Resolved by: UBU-D0284 Last scored: 2026-09-21 Scored from commit: None\n \n Defining context: DESIGN.md \u00a79.4, DESIGN.md \u00a716.3, PLANNING_KERNEL_CONTRACT.md \u00a73, `UBU-D0276`, `UBU-D0277`, `UBU-D0278`, `UBU-D0279`.\n \n@@ -2905,6 +2905,6 @@ A Task's split policy is `atomic` by default. A splittable Task declares a minim\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0284`: every schedulable Task has a split policy, defaulting to `atomic`; a splittable Task explicitly declares required `min_piece_seconds`, `resume_overhead_seconds`, and `max_pieces` fields. Resume overhead is constant and applies before each piece after the first, with no separate Phase 1b affect cost or interruption input. Pieces are same-Task Plan placements, at most one per chunk, all inside the Task's allowed range, and never splits inside a chunk. Decomposition segments remain atomic; a decomposition child may be splittable only as an independently segmented placement unit. The kernel contract gains `TaskSpec.split_policy`, PlanCandidate piece metadata, validation for multiple ordered non-overlapping pieces, and rollout semantics that draw one total Task duration and spend it across pieces. Dependencies and effects attach to the Task's first and last pieces. Progress stops are recorded as Log evidence and a remaining-work estimate on the same Task, not a Container replacement. Splittable work earns proportional scheduled-work value for completed sampled work inside the horizon, while completion, effects, and outgoing dependencies require the final piece. Projection surfaces may show separate piece events such as `(2/3)` while preserving one canonical Task. The contract change shares the Phase 1b minor version reserved with partial placement.\n \n ---\ndiff --git a/PLANNING_KERNEL_CONTRACT.md b/PLANNING_KERNEL_CONTRACT.md\n--- a/PLANNING_KERNEL_CONTRACT.md\n+++ b/PLANNING_KERNEL_CONTRACT.md\n@@ -116,6 +116,27 @@ Each `TaskSpec` used by the planning kernel must include either a fixed duration\n \n Each Dynamic `TaskSpec` also carries a single hard `window: { earliest_start, latest_finish }` with RFC 3339 / ISO 8601 UTC timestamps. The CPU side builds it by intersecting the admitted Task's one-off `allowed_time_range`, or the concrete range on an instantiated routine Task, with `PlanningRequest.time_window`. If the intersection is empty or shorter than the Task's minimum possible duration, the Task is unplaceable for that request rather than soft-scored outside the window. Multiple allowed windows are not represented in Phase 1b `TaskSpec`; any future support requires a contract change or CPU-side occurrence/window selection before dispatch.\n \n+Each `TaskSpec` also carries a split policy:\n+\n+```text\n+{ type: \"atomic\" }\n+```\n+\n+or:\n+\n+```text\n+{\n+  type: \"splittable\",\n+  min_piece_seconds: positive_integer,\n+  resume_overhead_seconds: non_negative_integer,\n+  max_pieces: integer >= 2\n+}\n+```\n+\n+`atomic` is the default admitted Task policy and requires one placement. A splittable Task may produce multiple Plan placements for the same Task, but no more than one piece per chunk, no more than `max_pieces` total pieces, and no piece outside the Task's `window`. Splitting within one chunk is invalid.\n+\n+For a splittable Task, the kernel samples one total work duration from the Task's duration model and spends that work across pieces. The first piece has no resume overhead. Each later piece consumes `resume_overhead_seconds` of occupied time before additional sampled work is performed. Incoming dependencies and preconditions gate the first piece; outgoing dependencies and effects are satisfied only by the final piece. Unsupported execution profiles must reject splittable TaskSpecs or mark them unplaceable with an explicit diagnostic rather than silently treating them as atomic.\n+\n ### Fixed duration\n \n ```text\n@@ -249,7 +270,11 @@ Each `PlanCandidate` must carry:\n - `candidate_id`\n - `rank`\n - `candidate_role`: enum, one of `highest_utility`, `most_robust`, `most_schedule_diverse`, `other`.\n-- `schedule`: ordered list of planned Task placements.\n+- `schedule`: ordered list of planned Task placements. Each placement carries:\n+  - `task_id`\n+  - `piece_index`: 1-based integer; atomic placements use `1`.\n+  - `piece_count`: positive integer; atomic placements use `1`.\n+  - start/end or equivalent occupied interval fields required by the implementation profile.\n - `score_summary`:\n   - `utility_score`\n   - `robustness_score`\n@@ -318,6 +343,8 @@ Common names:\n - `task_index`: padded task-slot index within `MAX_PLANNING_TASKS`.\n - `validity_mask`: boolean mask for occupied task slots or valid candidates.\n - `start_time_offsets`: planned start offsets from `time_window.start_time`, in planning ticks or seconds as declared by the implementation profile.\n+- `piece_index`: 1-based planned piece index per placement, with `1` for atomic placements.\n+- `piece_count`: total planned piece count for that Task in the candidate, with `1` for atomic placements.\n - `duration_samples`: sampled duration tensor.\n - `feasible_mask`: boolean candidate feasibility mask after stage-local checks.\n - `surviving_indices`: compacted indices of candidates that survive a filter stage.\n@@ -333,6 +360,7 @@ Consumes:\n - CPU-provided `topological_order` and validity masks;\n - dependency edge data or adjacency tensors;\n - duration-model parameters;\n+- split-policy parameters;\n - time-window and planning-delta parameters;\n - candidate seeds derived from `rng_seed`.\n \n@@ -341,6 +369,7 @@ Produces:\n - candidate schedule tensor or equivalent structured schedule batch;\n - `start_time_offsets`;\n - `duration_samples`;\n - candidate `validity_mask`;\n+- per-placement `piece_index` and `piece_count` metadata;\n - dependency-slack summary;\n - rejection/failure code per invalid candidate.\n@@ -407,5 +436,7 @@ Produces:\n - rollout diagnostics and degradation warnings.\n \n Stage 4 uses deterministic rollout seed derivation from the request seed. Phase 1 may use `rng_seed + 3` as the stage-4 rollout seed stream convention until named substreams are introduced. GPU floating-point rollout summaries are reproducible within the recorded tolerance profile, not bitwise across hardware, drivers, PyTorch versions, or reduction orders.\n+\n+For splittable Tasks, Stage 4 reuses the single sampled total work duration for every piece of that Task in a rollout and accounts for deterministic resume overhead before each resumed piece. A rollout remains feasible when unfinished sampled work can continue in a later planned or repair-eligible piece inside the Task window and `max_pieces`; it is infeasible when remaining work, overhead, fixed placements, dependency state, or the allowed range prevent completion under the candidate's policy. Partial scheduled-work value is proportional to completed sampled work for splittable Tasks, while effects and outgoing dependencies require completion of the last piece.\n \n ### CPU/GPU parity expectations\n",
    "commit_message": "Resolve UBU-Q0157 splittable task policy",
    "validation_notes": [
      "Ran git apply --check on the full proposed patch successfully against base commit d2bc730b2920210cc9e774ff7e509541ea451b7a.",
      "Patch modifies only the allowlisted files: DECISIONS.md, DESIGN.md, OPEN_QUESTIONS.md, and PLANNING_KERNEL_CONTRACT.md.",
      "OPEN_QUESTIONS.md preserves the selected question's single-line metadata format and resolves only UBU-Q0157."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0157"
    ],
    "decisions_added": [
      "UBU-D0284"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "openai-gpt5-codex-UBU-Q0157-001",
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
