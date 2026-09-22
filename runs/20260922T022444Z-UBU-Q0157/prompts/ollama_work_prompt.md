# Model-Committee Work Proposal Request

You are participating in the UbU `model-committee` process.

Your task is to produce one concrete work proposal as strict JSON.

Do not return prose outside the JSON object.

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

## Canonical design context

The excerpts below are **not** the whole canonical files. They are the subset selected by
following references from the selected question: its dependencies, the decisions those
questions cite, and the sections those cite in turn.

Read them under these rules:

- **Absence is not non-existence.** Ids and sections you cannot see here still exist.
  Never conclude that a question is unasked, a decision unmade, or a section absent
  because it is not shown.
- **Do not renumber or reuse ids.** Allocate new entries from the next free ids below.
- **Patch context must match the real file.** Hunks are applied against the full files,
  not against these excerpts, and `git apply` locates them by surrounding context. Quote
  surrounding lines exactly as they appear here.
- **Scope your edits to what you can see.** Do not delete or rewrite text on the
  assumption that it duplicates something outside these excerpts.

Next free ids — use these for anything you add:

- next question id: `UBU-Q0159`
- next decision id: `UBU-D0284`

`DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md` is read-only context and is not in the patch
allowlist: do not propose changes to it.

### DECISIONS.md — cited decisions

#### UBU-D0276

```markdown
## UBU-D0276: Dynamic Task allowed ranges are single hard absolute occupancy intervals

**Status:** Accepted → DESIGN.md §9.2, §16.5; PLANNING_KERNEL_CONTRACT.md §3. Resolves `UBU-Q0152`.

One-off Dynamic Tasks express when they may occupy Calendar time with `allowed_time_range: { earliest_start, latest_finish }`, where both endpoints are absolute UTC instants and `earliest_start < latest_finish`. The entire planned occupied interval for the Task must fit inside this range. Placement outside the range is hard infeasibility, not a soft penalty.

The allowed range is a declared Task input. It is distinct from due dates, deadlines, or target dates, which may affect scoring, risk, explanation, or completion legitimacy but do not by themselves define occupancy eligibility. It is also distinct from the `UBU-D0256` decision envelope, which is derived from a concrete Plan and compact Calendar policy for mobile stewardship and local repair.

Static Tasks and Dynamic allowed ranges are mutually exclusive at the Task scheduling layer. A Static Task uses fixed start and end times and enters the skeleton directly. A Dynamic Task uses `allowed_time_range` plus duration and is placed by the planner. A zero-slack range is not a substitute for a Static Task commitment; if the wall-clock slot itself is committed, the Task is Static.

For kernel dispatch, the CPU converts the single allowed range to the per-Task `TaskSpec.window` by intersecting it with `PlanningRequest.time_window`. If the intersection is empty or shorter than the Task's minimum possible duration, that Task is unplaceable for the request. Phase 1b does not add multiple movable windows to `TaskSpec`; future multi-window support needs a later contract change or CPU-side selection before dispatch.

Local time-of-day constraints are not stored on one-off Tasks. Recurrence templates may describe local windows, but instantiation resolves each occurrence into a concrete UTC `allowed_time_range` using the schedule's IANA time zone and recorded DST disambiguation. Ambiguous or nonexistent local endpoints must be resolved or surfaced for review before the occurrence becomes a kernel input.

`occupies_capacity` is retired as a scheduling semantic once Phase 1b implementation evidence shows that Dynamic Tasks and routine instances with admitted ranges are placed, omitted through partial-placement rules, and hard-validated without the old gate. Until that evidence exists, existing compatibility fields may remain as migration scaffolding, but the canonical rule is that a schedulable Dynamic Task with duration and an allowed range occupies planning capacity.

Consequences:

- `DESIGN.md` §9.2 records `allowed_time_range` as the Phase 1b Dynamic Task input and distinguishes it from Static Task timing.
- `PLANNING_KERNEL_CONTRACT.md` §3 records the one-window `TaskSpec.window` mapping and hard infeasibility rule.
- `OPEN_QUESTIONS.md` marks `UBU-Q0152` solved.

---
```

#### UBU-D0277

```markdown
## UBU-D0277: Phase 1b Task value is explicit priority normalized per planning request

**Status:** Accepted → DESIGN.md §16.10; PLANNING_KERNEL_CONTRACT.md §5. Resolves `UBU-Q0153`.

Phase 1b Task value for Plan scoring comes from admitted explicit operator prioritization, including the Quick UbU review-and-prioritize flow once it is ported as admitted records. The canonical persisted input is an ordinal Task priority declaration, not a persisted utility number. Objective labels, deadlines, due dates, routine cadence, and imported source urgency may affect eligibility, risk reporting, explanation, or future policy work, but they do not become canonical Task value in Phase 1b unless the operator admits them as explicit prioritization.

When the CPU kernel builds a `PlanningRequest`, it maps admitted ordinal priority among the request's eligible schedulable Tasks onto bounded transient metadata. `TaskSpec.value` is a float in `[0.1, 1.0]`: the highest priority bucket maps to `1.0`, the lowest ranked bucket maps to `0.1`, ties share a bucket, a single ranked bucket maps to `1.0`, and unranked eligible Tasks use `0.1`. `TaskSpec.priority` carries the corresponding normalized priority rank for tie-breaks, diagnostics, and explanation. These computed numbers are request-local and are never persisted as utility.

Routine instances are scored as ordinary Task instances after recurrence instantiation. They inherit explicit occurrence priority when present; otherwise they inherit the priority declared on the routine template or evergreen Objective that produced them. Missed occurrences, approaching local windows, or due dates may create review prompts, risk findings, or deadline/urgency diagnostics, but they do not automatically rewrite base value in Phase 1b.

In Stage 3, utility is the value-weighted scheduled work signal for a candidate. It combines with approximate robustness, affect-margin, and schedule-diversity only through the request's `scoring_policy` weights. The Stage 2 affect filter and CPU hard-constraint certification remain gates: high value cannot legitimize affect-infeasible or hard-invalid Plans, and low value does not make a Task ineligible unless partial-placement selection later omits it.

Phase 1b records review decisions, overrides, snoozes, rejections, and operator reprioritization as evidence for later learning, but it does not learn trade-off weights or automatically mutate priority from revealed preference. Adaptive weighting and preference learning remain with `UBU-Q0125`.

Consequences:

- `DESIGN.md` §16.10 records that Stage 3 consumes CPU-computed transient value/priority metadata from admitted prioritization records.
- `PLANNING_KERNEL_CONTRACT.md` §5 fixes the Phase 1b normalization and non-persistence rule for `TaskSpec.value` and `TaskSpec.priority`.
- `OPEN_QUESTIONS.md` marks `UBU-Q0153` solved.

---
```

#### UBU-D0278

```markdown
## UBU-D0278: Phase 1b decomposition segments are Container split points compiled as placement units

**Status:** Accepted → DESIGN.md §9.4, §21.2.1. Resolves `UBU-Q0151`.

Phase 1b represents an admitted decomposition as a structural Task-to-Container replacement. The Container carries `container_id`, `origin_task_ref`, `mutation_reason`, `mutation_log_ref`, ordered child Task refs, segment split points, lineage, and provenance. Children are ordinary Task records with their own executable fields; the original Task becomes historical and moot with `replaced_by_new_plan_structure` when the decomposition preserves the underlying intent.

Segments are stored as split points over the ordered child list, not as a separate list of child groups. A split point ends the current contiguous segment after a child; no split points means the entire ordered child list is one segment. Materialized segment ids may be derived from the Container version and child-index range but are not separate WorkItems.

Within a segment, children stay strictly back-to-back. Phase 1b does not add a maximum-gap edge: if there should be a pause, handoff, recovery interval, wait, or independent scheduling opportunity, the decomposition uses a segment boundary or an explicit child Task.

The planner handoff is orchestrator-side compilation. Each admitted segment becomes one temporary placement unit sent through the existing planning kernel contract. Fixed child durations sum exactly; stochastic child durations use a conservative component-wise sum of minimum, mode, and p95 with one rollout sample for the whole segment. After placement, the orchestrator expands the unit into child Plan entries with contiguous offsets. Native no-gap kernel edges are deferred until measurement shows that the conservative segment approximation materially distorts rollout results.

Child-specific hard constraints are preserved by segmentation. Static timing, allowed time ranges, preconditions, dependencies, or effects that safely apply to the whole segment constrain the compiled placement unit. Constraints that apply only at an interior child force a boundary before or after that child as appropriate. If review cannot express the decomposition as valid contiguous segments without violating a child constraint, admission rejects the candidate or requires edits.

`Decomposition` advisory candidates normalize to the target Task ref, proposed Container summary or inherited original-intent fields, ordered child specs with stable proposed-child keys, titles, duration models, child-specific constraints and provenance, and split markers after child positions. The advisor proposes split markers at natural pause points and before constraint boundaries; review can edit child specs, order, and split markers before admission. Unadmitted proposed segments stay candidate-state preview material.

Progress and repair operate on the unstarted suffix of the current segment. Completed children remain historical facts. If a child finishes early or late, local repair keeps the remaining siblings contiguous from the actual completion boundary; if the suffix can no longer satisfy admitted hard constraints, repair replans from the next segment boundary or surfaces a reviewable repair candidate rather than scattering siblings independently.

Undo is another admitted structural replacement, not erasure. Undo creates a restored Task with a new Task handle carrying the original intent and prior schedulable fields when still valid, marks the decomposition Container and children as superseded or moot by the restoration, and retains the original Task's moot event, mutation logs, child Task history, Plan history, External Reference lineage, and review/admission records.

Consequences:

- `DESIGN.md` §9.4 records Phase 1b Container fields, split-point segment representation, strict adjacency, orchestrator-side segment compilation, repair, and undo behavior.
- `DESIGN.md` §21.2.1 records the normalized `Decomposition` advisory candidate shape.
- `OPEN_QUESTIONS.md` marks `UBU-Q0151` solved.

---
```

#### UBU-D0279

```markdown
## UBU-D0279: Phase 1b planning search is chunked at capacity-occupying Static Tasks

**Status:** Accepted → DESIGN.md §16.3, §16.10.4

Capacity-occupying Static Tasks, together with preserved or frozen placements, partition the planning horizon into chunks: the free intervals before, between, and after those fixed placements. Non-capacity Static Tasks do not bound chunks. Placement units are not preempted, so every atomic unit, whether a Dynamic Task or a compiled decomposition segment (`UBU-D0278`), lies entirely inside one chunk.

Candidate search has two levels. The first level assigns placement units to chunks. This is the coupled part of the problem:

- each unit occupies exactly one chunk;
- allowed time ranges (`UBU-D0276`) restrict which chunks a unit may use;
- dependencies and effects flow forward in time;
- affect and UniverseState evolve from chunk to chunk.

The second level orders the units assigned to each chunk. Given an assignment, chunks are independent, so their orderings are evaluated in parallel. Equivalently, the search sweeps the chunks breadth-first in time order. Each branch carries its unplaced units and its carried state, and branches are pruned at chunk boundaries, instead of being generated for the whole horizon and rejected late.

Fixed placements bound delay propagation, so Monte Carlo rollouts evaluate chunks separately. Each rollout draws the latent correlation-group factors once and shares them across all chunks, and it is feasible only if every chunk is feasible under those shared draws.

The CPU reference path and the GPU engine share these search semantics; the GPU engine adds breadth, not different meaning. The `PlanningRequest`/`PlanningResponse` contract, the stage boundaries of `PLANNING_KERNEL_CONTRACT.md` §5, and CPU certification of the selected Plan are unchanged. A chunk axis in tensor layouts is an implementation choice within the recommended tensor profile.

Chunks also scope runtime repair. The current chunk, up to the next fixed placement, is the unit of local repair, consistent with the §16.5 repair recipes, and compact Calendar packaging may carry alternatives per chunk.

A horizon with few fixed placements yields large chunks, and for those the within-chunk problem is the whole problem. Long availability blocks, such as working hours, are expressed as allowed ranges on the Tasks they admit, not as capacity-occupying Static Tasks, which would leave those Tasks nowhere to go. A unit longer than every chunk it may occupy cannot be placed. The remedies are decomposition split points, splittable Tasks (`UBU-Q0157`), and partial placement (`UBU-Q0155`).

Consequences:

- `DESIGN.md` §16.3 records chunked search, and §16.10.4 applies it to the GPU pipeline stages.
- `OPEN_QUESTIONS.md` updates `UBU-Q0154`, `UBU-Q0155`, and `UBU-Q0156` for chunked search and records splittable Tasks as `UBU-Q0157`.

---
```

### DESIGN.md — referenced sections

#### DESIGN.md §9.2 — Task

```markdown
### 9.2 Task

A **Task** is a schedulable WorkItem.

Tasks may be:

- Static Task
- Dynamic Task

#### Static Task

A Static Task has fixed start and end times.

Examples:

- meeting
- class
- appointment

Static Tasks are included directly in Plans.

#### Dynamic Task

A Dynamic Task has flexible scheduling.

Dynamic Tasks may have:

- duration or duration PDF
- allowed time range
- dependencies
- preconditions
- effects
- Objective link
- success probability
- affect delta
- expected cost

**Phase 1 realization (`UBU-D0239`).** A Phase 1 Task carries an optional `duration_estimate` — either a fixed scalar or the §3 three-point shifted-log-normal (`min_seconds`/`mode_seconds`/`p95_seconds`) — and optional `correlation_groups` membership, defaulting to a fixed estimate and independence when absent. These are the per-Task duration uncertainty and correlation inputs the planning rollout consumes; the store admits and persists them, and the orchestrator carries them into the kernel `TaskSpec`.

**Phase 1b allowed range (`UBU-D0276`).** A one-off Dynamic Task may carry `allowed_time_range: { earliest_start, latest_finish }`, expressed as absolute UTC instants. The planner must place the Task's entire occupied interval inside that hard range by mapping it to the kernel `TaskSpec.window`. The range is a declared Task input, not a derived decision envelope, due date, deadline, or recurring local time-of-day rule. Routine instances receive their concrete range when recurrence instantiates them. Static Tasks continue to use fixed start and end times rather than representing a fixed commitment as a zero-slack Dynamic range.
```

#### DESIGN.md §9.4 — Container

```markdown
### 9.4 Container

A **Container** groups WorkItems.

Containers are not directly scheduled in Plans. Plans contain Tasks.

A Container may represent:

- decomposed work
- preempted work
- Technique instantiation
- grouped child Tasks
- Super Automation workflow structure

Container completion is derived from child states.

A Container is complete when all child Tasks are either:

- completed
- moot

Task-to-Container mutation is a structural replacement, not an in-place type change. The original Task handle remains a historical Task handle and is not reused as the Container handle.

Mutation creates a new Container with a new `container_id`. The Container records `origin_task_ref`, `mutation_reason`, `mutation_log_ref`, child WorkItem refs, and lineage/provenance sufficient to trace the restructuring.

For Phase 1b decompositions, the Container record is the admitted structure that keeps an ordered checklist together. It must include `container_id`, `origin_task_ref`, `mutation_reason`, `mutation_log_ref`, ordered child Task refs, segment split points, lineage, and provenance. The ordered child refs are Task refs, not embedded WorkItems, for every schedulable child created or linked by the decomposition.

Segments are represented as split points over the ordered child Task list: each split point means the current segment ends after that child and the next child starts a new segment. No split points means the whole ordered list is one segment. Segment ids, when materialized, are derived from the Container version and contiguous child-index ranges; they are not independent canonical WorkItems.

All child Tasks created by decomposition, preemption, retry, or worker expansion receive new Task handles. Existing child or continuation Tasks may be linked into the Container, but the mutation operation does not reassign the original Task handle to a child.

The original Task transitions to `moot` with reason code `replaced_by_new_plan_structure` when decomposition or regrouping preserves the underlying intent. If responsibility moves to another executor without decomposition, `delegated` may be more accurate; if a newer source artifact replaces the Task, `superseded` may be more accurate.

Fields that describe the original intent stay on the Container or its lineage metadata: title or summary, served Objective refs, parent/dependency context, external-reference lineage, Compartment/security labels, authority/provenance, and notes needed to explain why the work was split.

Fields that make a schedulable action executable belong on child Tasks: duration or duration PDF, preconditions, effects, executor/delegation fields, worker assignment/status, expected output, evidence requirements, and any child-specific dependencies or deadlines. Child Tasks may inherit or narrow Objective refs, Compartment refs, authority source, and External Reference context when valid, but they do not silently inherit stale status, completion evidence, or modeled effects that no longer apply.

Within a segment, children are planned and repaired as strictly back-to-back work. Phase 1b does not model a maximum-gap edge between children; any admitted gap, wait, recovery, handoff, or pause is represented by a separate Task or by a segment boundary.

Phase 1b keeps the planning kernel contract unchanged. Before kernel dispatch, the orchestrator compiles each admitted segment into one temporary placement unit whose duration is the exact sum of fixed child durations, or for stochastic durations the conservative component-wise sum of minimum, mode, and p95 with one rollout sample for the segment. After the kernel places the unit, the orchestrator expands it back into child Plan entries in Container order with contiguous offsets. A native no-gap kernel edge is deferred until measurement shows the conservative segment approximation distorts rollout results.

Children with hard static timing, allowed time ranges, preconditions, dependencies, or effects that cannot safely apply to the whole segment force a segment boundary before that child, or after it when the constraint belongs only to prior work. Shared hard constraints may constrain the compiled segment as a whole. If no sequence of boundaries can preserve the child constraint without creating an internal gap or making the proposal unschedulable, admission rejects the decomposition or requires review edits before admission.

When execution finishes a child early or late, completed children stay fixed as history. The unstarted suffix of its segment remains a contiguous placement unit for local repair, starting no earlier than the actual completion time of the prior child. If the suffix no longer satisfies admitted hard constraints, repair replans from the next segment boundary or surfaces a reviewable repair candidate; it does not scatter remaining siblings as independent filler.

Undoing a decomposition is another admitted structural replacement, not deletion. It creates a restored Task with a new Task handle carrying the original intent and prior schedulable fields when still valid, marks the Container and child Tasks as superseded or moot by that restoration, and retains the original moot event, mutation log, child Task history, Plan history, External Reference lineage, and review/admission events for audit.

External References are preserved by linking the external object to the new Container when it represents the larger work and to child Tasks only when the external object supports, evidences, or projects that specific child. The original Task's historical External References are not rewritten; new `supersedes`, `projection_of`, `supports`, or `evidence_for` references may be added according to the accepted External Reference model.

GitHub-linked Task decomposition should keep the GitHub Issue or PR traceable to the Container and add child-level External References only for actionable subwork that needs projection or reconciliation.

Automation/Super Automation expansion uses the same structural-replacement rule; worker-specific child Task details are in §25.1.2.
```

#### DESIGN.md §16.3 — Planner grammar direction

```markdown
### 16.3 Planner grammar direction

Compact Calendar planning is no longer framed as a bare DFS process that directly produces the default Plan. The current architecture is:

1. Build the skeleton Plan from Static Tasks and dependency DAGs.
2. Legitimize the skeleton Plan into a minimally human-viable baseline.
3. Generate richer candidate Plans by adding optional Dynamic Tasks, alternative placements, and alternative Techniques for the same Objectives (§16.3.1).
4. Use semi-legitimization or full legitimization to reject unrealistic candidates.
5. Validate finalists against hard constraints.
6. Select the user-facing default Plan by Plan probability among deterministic candidate Plans.
7. Package compact repair metadata for runtime use.

DFS-like search may still be useful for candidate construction. BFS-like search may still be useful for near-term divergence. Greedy selection may still be useful as a deliberately unintelligent baseline. None of those algorithms is the complete planning architecture by itself.

Execution profiles are additive rather than mutually exclusive. The greedy mean-duration planner is the required MVP benchmark. Local and mobile profiles must support deterministic skeletonization, conservative legitimization, exact hard-constraint checks, local repair recipes, and a short-horizon branch cache. Desktop, worker, and hosted profiles may add DFS-like candidate construction, local search, solver-backed finalist validation, and GPU-friendly scoring or simulation. GPU or learned search may propose and score candidates, but exact or conservative validation certifies the selected Plan.

**Chunked search (`UBU-D0279`).** Phase 1b candidate construction partitions the horizon into chunks at capacity-occupying Static Tasks and other fixed placements. The search first assigns placement units to chunks. That is the coupled step, constrained by allowed ranges, dependencies, and state carried between chunks. It then orders each chunk's units independently and in parallel, sweeping chunks in time order and pruning at chunk boundaries. Rollouts evaluate chunks separately, sharing latent draws across chunks. The CPU reference path and the GPU engine share these semantics, and the current chunk is the unit of local repair.

**Sweep mechanics and streaming (`UBU-D0280`, `UBU-D0281`).** The sweep expands each branch into up to K alternatives per chunk and keeps the best B partial plans after each chunk depth. It merges branches that reach the same state, and assigns the most constrained units first. Interactive requests stream certified chunk results, so the user can act on the first chunk while later chunks are still being searched; a streamed chunk stays provisional until the user starts it. Batch requests return only the final response.

Plan probability is represented internally as probability metadata rather than only as a display scalar. The minimum record contains a display scalar, a log probability for stable computation, an optional probability interval, provenance over the modeled probabilistic inputs, and correlation-group or scenario references when inputs are not independent. Implementations may multiply probabilities only for inputs declared independent. Correlated or unknown relationships must use joint scenarios, shared random variables, correlation groups, or conservative intervals rather than pretending independence.
```

#### DESIGN.md §16.5 — Mobile stewardship metadata

```markdown
### 16.5 Mobile stewardship metadata

A compact Calendar should carry metadata that lets constrained devices preserve legitimacy without performing full global replanning. Useful metadata includes:

- protected, flexible, and disposable Task criticality;
- decision envelopes such as earliest start, latest finish, and movable windows;
- cached explanations for why a Task matters and what depends on it;
- conflict severity levels;
- last-legitimate-Plan references;
- simple repair recipes;
- recovery-critical and deadline-fragile markers;
- recalculation triggers and remote-assist eligibility.

The MVP-friendly subset is Task criticality, last legitimate Plan storage, simple repair rules, conflict severity levels, cached explanations, next-best-action mode, and basic decision envelopes. Rich branch packaging and learned local policy models are post-MVP by default.

**Phase 1b mobile stewardship schema (`UBU-D0256`).** A compact Calendar stores `mobile_stewardship_metadata` for each Task or Plan segment that mobile may recommend, move, omit, or explain. The record is compact and by reference where possible; it contains `criticality`, `decision_envelope`, `conflict_severity`, `explanation_fragments`, `repair_recipe_refs`, and optional `recalculation_triggers` and `remote_assist_eligible`. Rich branch packaging, learned local policy, and global re-optimization remain outside MVP.

Task criticality is:

- `protected`: required for hard Calendar Logic, Static Tasks, prerequisites, dependencies, full legitimization support, recovery or safety, or a user-declared must-do commitment. Mobile must not silently drop it, shorten it below its minimum duration, or move it outside its envelope.
- `flexible`: legitimate to move, split, or shorten within its envelope when dependencies, preconditions, affect assumptions, and hard constraints remain satisfied. Mobile may silently repair flexible Tasks only when the resulting conflict severity is `minor`.
- `disposable`: optional or opportunistic work that may be skipped, replaced by an eligible next-best action, or left for desktop/cloud refinement when doing so does not break protected or flexible downstream commitments.

The MVP criticality object is `criticality: { level, reason_refs, recovery_critical, deadline_fragile, min_duration_seconds, user_confirmation_required, disposal_effect }`. `level` is one of `protected`, `flexible`, or `disposable`; `reason_refs` point to Objectives, Static Tasks, dependencies, preconditions, legitimization support, user commitments, or risk findings; `disposal_effect` is a short enum-or-string summary such as `none`, `lost_optional_value`, `missed_deadline_risk`, or `breaks_dependency`.

Task-level `allowed_time_range` is an admitted scheduling input for Dynamic Tasks. A decision envelope is later derived from a specific Plan placement, criticality, dependencies, preconditions, and repair policy so mobile can preserve or repair that Plan; it must not be treated as the source of the Task's allowed range.

The MVP decision envelope is `decision_envelope: { earliest_start, latest_start, latest_finish, movable_windows, min_duration_seconds, target_duration_seconds, dependency_refs, precondition_refs, affected_static_refs, affect_assumption_refs, resource_or_location_refs, repair_budget_seconds, stale_after }`. `movable_windows` is an ordered list of `{ start, end }` intervals in which the Task may begin or occupy time, depending on the Task's duration fields. Empty or absent movable windows mean the Task is fixed or must be handled by hard Calendar Logic rather than local motion.

The shared conflict severity vocabulary is:

- `advisory`: stale or degraded metadata exists, but the current and next recommendation remain legitimate.
- `minor`: a local repair inside recorded envelopes preserves all protected commitments and can be undone or inspected later.
- `major`: the current or next recommendation remains explainable, but the user must choose, confirm, or supply missing information before mobile changes a protected commitment or meaningful tradeoff.
- `blocking`: mobile cannot identify a legitimate current or next Task because a hard constraint, Static Task, precondition, dependency, affect legitimacy requirement, or required starting state is violated or unknown.

This severity vocabulary is also the Phase 1b projection-conflict vocabulary: projection-specific classes may map into these severities, but must not invent a parallel severity ladder without a later decision.

**Phase 1b projection conflict policy (`UBU-D0261`).** Projection reconciliation uses the sync conflict class `projection_conflict` with projection-specific quadrants: `known_object_edited`, `known_object_deleted`, `unknown_object_appeared`, and `expected_object_absent`. Automatic handling is allowed only when the result is deterministic, idempotent, within projection policy and decision envelopes, and does not choose between canonical user intent, protected Calendar ownership, or independently changed third-party truth.

The quadrant defaults are:

- `known_object_edited`: auto-resolvable only for non-authority metadata repair or for a permitted edit inside the object's explicit decision envelope. A duration edit to a flexible or disposable Dynamic Task may be `minor` when minimum duration, dependencies, affect assumptions, and Static Tasks remain valid; an edit that pins or moves a Dynamic Task to an external wall-clock time is `major` unless the projection policy already granted that editable window. Protected, Static, dependency-bearing, or precondition-affecting edits are `major` or `blocking`.
- `known_object_deleted`: never silently tombstones or removes the canonical object. If the object is already tombstoned, withdrawn from the projection window, or awaiting idempotent projection deletion, the absence may be `advisory`; otherwise deletion of an active canonical object is `major`, or `blocking` when current/next legitimacy depends on it.
- `unknown_object_appeared`: is contained as external evidence, import candidate, or projection diagnostic rather than admitted canonical state. It remains `advisory` or `minor` when it does not claim a user commitment or protected slot; it becomes `major` when accepting it would create or override a commitment, dependency, Static Task, or third-party-truth assertion.
- `expected_object_absent`: is `advisory` when explained by tombstone, policy/window rotation, or completed projection deletion; it may be `minor` when UbU can retry or recreate the projection idempotently without overriding external truth. It is `major` or `blocking` when the missing external object affects a protected commitment, a current/next recommendation, a required precondition, or Calendar validity.

Every automatic projection repair emits an auditable `projection_conflict_event` record with the conflict id, projection surface, quadrant, severity, canonical object ref when any, external object ref or redacted handle when allowed, observed object and policy versions, external delta digest, action taken, candidate refs when relevant, resulting mutation or projection-operation refs, and recorded time. If the action admits canonical state, it uses the ordinary mutation-envelope or tombstone path; otherwise it remains projection state, diagnostic state, or candidate metadata and is not a silent canonical mutation.

When an external delta touches the same object as a still-pending candidate, admission compares both against the candidate's observed versions before either is accepted. Exact duplicates collapse; deterministic non-overlapping deltas may update the pending candidate's provenance or candidate mutation proposal. Overlapping deltas quarantine or supersede the candidate and surface a `major` review diagnostic, escalating to `blocking` when current/next legitimacy depends on resolving it. External projection evidence cannot force a pending candidate into admitted state.

Cached explanation fragments are short, non-blaming strings with lineage refs. MVP fragments are `why_now`, `matters_because`, `depends_on`, `supports`, `legitimacy_reason`, `if_skipped`, `repair_summary`, and `staleness_summary`. Each fragment may reference Task ids, Objective refs, dependency/precondition refs, Static Task refs, risk finding refs, and compact Calendar provenance, but should avoid copying private details into more-disclosive replicas.

A repair recipe record is `repair_recipe: { trigger, applies_to_refs, eligible_when, steps, ask_policy, fallback, explanation_fragment_ref }`. MVP triggers are:

- `late_task`: condition on elapsed time and accepted Logs; keep protected Tasks and Static Tasks fixed, compress or drop only flexible/disposable work inside envelopes, then offer the next valid Dynamic Task or next-best action. Escalate to `major` or `blocking` if a protected latest finish, hard precondition, or Static Task would be violated.
- `skipped_task`: for disposable Tasks, record omission and continue; for flexible Tasks, reschedule within the envelope; for protected Tasks, ask before omission or create a blocking diagnostic if no legitimate alternate exists.
- `fatigue_report`: record the user report as a Log/Snapshot input, protect recovery-critical Tasks, reduce to protected and low-affect eligible work, and surface rest or clarification when affect assumptions no longer support the Plan.
- `approaching_static_task`: preserve the Static Task, insert required transition/setup/teardown if already modeled, stop or park the current Task when necessary, and ask when the remaining time cannot satisfy a protected current Task.
- `missing_prerequisite`: re-evaluate the deterministic precondition/dependency chain; if a modeled prerequisite Task is available, schedule or pull it forward inside its envelope; otherwise show a clarification Task or blocking prompt. Mobile must not invent a satisfied state, Resource, Skill, Technique, or external fact.

The last legitimate Plan record is `last_legitimate_plan: { plan_id, plan_digest, generated_at, effective_time, horizon, execution_profile, legitimacy_result_ref, hard_validation_ref, default_segment_digest, universe_state_digest, log_cursor, snapshot_refs, external_event_cursor, compact_calendar_digest, protected_task_refs, decision_envelope_digest, explanation_lineage_digest }`. Mobile compares current reality to this record by replaying accepted Logs, Snapshots, External Events, elapsed time, and user actions since the recorded cursors, then checking whether current and next Task placements remain inside decision envelopes with dependencies, preconditions, affect assumptions, Static Tasks, and coverage still valid.

Mobile may silently repair only for `minor` conflicts where the recipe is explicit, all changes stay inside decision envelopes, protected Tasks and support Tasks are preserved, failed preconditions are not hidden, and the explanation cache remains accurate enough for next-action display. Mobile asks the user for `major` conflicts, protected/discretionary tradeoffs, missing starting state, meaningful omission, or any repair that would change a user commitment. Mobile waits for desktop/cloud refinement or marks the Calendar stale when the issue is advisory, outside the reactive horizon, low coverage but not current-action-critical, or requires global optimization. It enters next-best-action mode when there is no safe default but there is at least one legitimate local Task or clarification Task to show.
```

#### DESIGN.md §16.10 — GPU desktop execution backend

```markdown
### 16.10 GPU desktop execution backend

The Phase 1 performance target is a local desktop/laptop GPU backend, delivered in Phase 1b as part of the planner the switch requires (`UBU-D0275`). A small CPU reference path or fixture-backed deterministic path remains required for tests, CI, and contributors without GPU access. Mobile and cloud planner backends are deferred beyond Phase 1.

The implementation-facing contract for this section is `PLANNING_KERNEL_CONTRACT.md`. `DESIGN.md` defines the architectural intent; the contract file defines the Phase 1 schema and boundary details.

#### 16.10.1 Pure function contract

The GPU planning engine is a pure function. Its input is a `PlanningRequest`. Its output is a `PlanningResponse` containing ranked PlanCandidates, diagnostics, warnings, and probability-quality metadata. The engine has no canonical side effects, performs no UbU I/O, and holds no request-semantic mutable state. The CPU kernel owns all canonical state mutation. The GPU engine is advisory only: it proposes; the CPU kernel validates and commits.

In Phase 1b the engine is invoked through a persistent local Python worker process owned by the CPU kernel (`UBU-D0283`). The boundary is local length-prefixed JSON frames over pipes, not a network service. This replaces the earlier in-process typed-call direction so the Rust desktop kernel keeps crash isolation from Python, CUDA, and PyTorch failures, avoids embedding the Python interpreter and its lock in the Rust process, and amortizes Python startup by reusing the worker. The framework is PyTorch.

#### 16.10.2 CPU/GPU handoff contract

The CPU/GPU semantic boundary is defined by two typed objects: `PlanningRequest` and `PlanningResponse`, specified in `PLANNING_KERNEL_CONTRACT.md`. Worker-spawn envelopes such as `GpuAdvisoryRequest` and `GpuAdvisoryResponse` may wrap those objects for process management, framing, cancellation, and telemetry, but they do not define separate planning semantics. Time values crossing the semantic boundary use the contract's RFC 3339 UTC timestamps; integer Unix seconds and tensor offsets are implementation-local lowerings.

`PlanningRequest` includes schema version, planner version, request ID, effective time, generated-at time, mode, RNG seed, time-window policy, horizon policy, compute budget, task graph with CPU-provided `topological_order`, UniverseState snapshot, AffectProfile, scoring policy, constraint policy, payload policy summary, and privacy/provenance payload-safety proof. Optional fields include external event assumptions, repair context, explanation request, and debug flags.

`PlanningResponse` returns ranked PlanCandidates plus diagnostics, rejection counts, warnings, probability-quality metadata, optional coverage estimate, optional compute telemetry, and stage funnel counts. `K=3` PlanCandidates is the Phase 1 default:

- highest-utility candidate;
- most-robust candidate;
- most-schedule-diverse candidate.

K is user-configurable to allow power users to tune computational resource use. The Phase 1 default rollout budget is `n_rollouts = 1000` per finalist unless overridden by the CPU kernel's compute budget.

Interactive requests may be answered as a stream of certified chunk results followed by the final `PlanningResponse`. Batch requests return only the final response (`UBU-D0281`). In Phase 1b the worker emits framed `PlanningStreamFrame` messages: zero or more `chunk_result` frames for interactive delivery, then exactly one `final_response` frame. Each chunk result is certified by the CPU kernel on arrival before it can become user-visible.

Backend selection is CPU-owned. If policy disables GPU use, no compatible local GPU/PyTorch runtime is available, the worker fails, or the compute budget does not justify worker dispatch, the CPU reference path is used. Responses and committed Plans record backend provenance, including backend kind, invocation kind, engine version, framework, device summary when available, request id, RNG seed, tolerance profile, and CPU certification status. GPU scores and rollout estimates are reproducible within the recorded tolerance profile, not bit for bit across hardware; CPU certification and CPU reference goldens are authoritative.

#### 16.10.3 Tensor layout

All tensor batches use padded fixed-size shape `(N_CANDIDATES, MAX_PLANNING_TASKS, ...)` with an explicit boolean validity mask tensor marking valid task slots. Ragged tensors are not used in Phase 1.

`MAX_PLANNING_TASKS = 256` is the Phase 1 planning window ceiling, defined at a single site. Future premium or wide-horizon tiers may increase this ceiling by scalar configuration, subject to memory, scenario-count, correlation-matrix, validation-cost, and backend performance limits. Linear scaling is not assumed as a mathematical guarantee.

#### 16.10.4 Pipeline stages

The GPU engine has four first-class pipeline stages, with semantic boundaries specified in `PLANNING_KERNEL_CONTRACT.md §5`.

1. **Skeleton sampling** — parallel shifted-log-normal or fixed duration sampling across candidates with vectorized propagation over the CPU-provided topological order.

2. **`affect_legitimacy_filter`** — batch sigmoid affect-constraint evaluation across surviving candidates. This stage implements only the sigmoid affect-constraint portion of legitimization. Full legitimization in UbU design is broader: it makes a skeleton Plan human-viable by respecting affect, recovery, transition, rest, sustainability, and support constraints. The `affect_legitimacy_filter` GPU stage does not replace or subsume full legitimization.

3. **Value scoring** — parallel utility, approximate robustness, affect-margin, and schedule-diversity scoring across surviving candidates.

Phase 1b value scoring uses CPU-computed, request-local Task value/priority metadata derived from admitted pairwise Task Preferences (`UBU-D0277`, `UBU-D0282`). Objective labels, deadlines, routine cadence, and revealed-preference evidence may inform review, risk, urgency, or later policy work, but the GPU stage receives only the bounded transient values supplied in the `PlanningRequest`.

4. **Monte Carlo rollout** — joint scenario simulation for finalists using correlation-group Gaussian copula sampling with deterministic rollout seed derivation.

GPU search proposes candidates. Hard constraint certification and final Plan validity are performed by the CPU kernel using exact or conservative validation.

In Phase 1b the stages operate on chunked candidates (`UBU-D0279`, §16.3): skeleton sampling builds candidates chunk by chunk, and the Monte Carlo rollout shares its latent draws across chunks. Chunked search stays inside one planning invocation for the whole request; the worker may batch chunks internally to fit memory, but the CPU does not dispatch each chunk as a separate semantic request.

#### 16.10.5 Stochastic duration model

Task durations use either a fixed duration model or a shifted log-normal distribution. The stochastic model is parameterized by `(min_seconds, mode_seconds, p95_seconds)`.

- `min_seconds` is the optimistic lower support shift used by the planner distribution; it is a subjective best-case modeling prior, not a claim about physical impossibility.
- `mode_seconds` is the most likely duration.
- `p95_seconds` is the high-but-plausible 95th percentile, not a hard upper cap.

The shifted distribution is `D = min_seconds + LogNormal(mu, sigma)`, with the canonical conversion formula specified in `PLANNING_KERNEL_CONTRACT.md §3`. The planner may sample durations greater than `p95_seconds`; that is intentional.

Fixed known durations use a separate fixed-duration model and are represented as a delta distribution. Invalid stochastic triples are rejected by schema validation rather than silently repaired.

For lay audiences this is well described as a stop-light model: a sequence of independent delay sources where slowdowns stack asymmetrically and early arrivals are absorbed cheaply by forward-pulling the next eligible Dynamic Task.

#### 16.10.6 Correlation groups and rollout matrix

Each stochastic-duration Task may carry `correlation_groups: [{group: str, strength: float}]`. In Phase 1, `strength` is a positive latent-factor loading in `[0, 1]`; negative correlations are deferred.

The CPU kernel constructs a deterministic positive semi-definite correlation matrix by normalizing per-Task positive factor loadings and computing `C = L * L^T + diag(1 - row_norm(L)^2)`. Multiple shared groups combine through the dot product of normalized loading vectors. Numeric jitter may be applied for floating-point error with explicit degraded diagnostics. Silent nearest-PSD projection is not a Phase 1 semantic repair.

Negative correlations are not rejected philosophically. They are deferred because signed pairwise-correlation declarations can create non-PSD matrices and confusing validation failures. Phase 2 should support anti-correlations through signed latent factors, explicit Cholesky-style parameterization, or a signed partial-correlation model.

The RNG seed is an explicit required input, ensuring plan generation is reproducible for peer debugging.

#### 16.10.7 Solver selection and deferred backend targets

Phase 1 does not require OR-Tools, Z3, CP-SAT, SMT, MaxSMT, or another external exact solver as a runtime dependency. The mandatory certification path is the built-in CPU kernel: dependency DAG validation, deterministic precondition and effect evaluation, skeleton validity, bounded contradiction diagnostics, full legitimization, hard Calendar Logic checks, provenance validation, payload-safety validation, and final Plan commit.

Solver and library candidates are evaluation targets:

- built-in CPU graph and precondition validator: required reference path and certification source;
- OR-Tools CP-SAT: optional finalist schedule-feasibility and contradiction-minimization experiment;
- Z3 or comparable SMT/MaxSMT: optional logical contradiction-diagnosis experiment;
- PyTorch or CPU local-search libraries: optional advisory candidate-optimization experiments;
- learned models: post-MVP advisory ranking or parameter estimation unless CPU admission certifies the output.

Phase 1 has no mobile GPU target. The required mobile fallback is CPU stewardship for current or next Task hard checks, cached last-legitimate Plan repair, decision envelopes, and simple repair recipes as defined by `UBU-D0256`.

Premium or cloud GPU planning is governed by the execution-provider boundary in `UBU-D0254`, with concrete products deferred to later provider work. Any future cloud backend must preserve the same typed `PlanningRequest`/`PlanningResponse` boundary, Compartment and export gating, payload minimization, provenance, and CPU certification on return.

See also: `UBU-D0166`, `UBU-D0167`, `UBU-D0168`, `UBU-D0169`, `UBU-D0170`, `UBU-D0171`, `UBU-D0172`, `UBU-D0173`, `UBU-D0174`, `UBU-D0212`, and `PLANNING_KERNEL_CONTRACT.md`.

---
```

#### DESIGN.md §21.2.1 — Phase 1b advisory candidate lifecycle

```markdown
#### 21.2.1 Phase 1b advisory candidate lifecycle

An `AdvisoryCandidate` is the first-class object form for Phase 1b review-queue proposals, including tag, dependency, preference, decomposition, clarification-question, and other advisory outputs before admission. It is `candidate_state`, not admitted state, until an admission decision creates the ordinary canonical mutation or review event.

The minimum record contains `advisory_candidate_id`, `schema_version`, `candidate_kind`, lifecycle state, object version, target or scope refs, normalized proposal shape, payload ref or redacted payload summary, source evidence refs or hashes, confidence, per-field provenance, proposed/effective time or interval, proposing actor or model/tool metadata, origin Device and execution-context provenance, idempotency key, optional suppression key, `compartment_ids`, a review-visible Compartment label or redacted label, disclosure and retention policy, review order metadata, and correction, rejection, deferral, resurfacing, supersession, admission, or archive links. Candidates are per-object records rather than aggregate queue blobs so each candidate can be versioned, replicated, redacted, and reviewed under its own Compartment policy.

The normalized proposal shape for a `Decomposition` candidate contains the target Task ref, proposed Container summary or inherited original-intent fields, ordered proposed child Tasks with stable proposed-child keys, titles, duration models, child-specific constraints and provenance, and split markers after child positions. The advisor proposes split markers only at natural pause points or where child-specific hard constraints require a boundary; review may edit child text, durations, constraints, order, and split markers before admission. Until admitted, these proposed segments remain candidate-state preview material and never become hidden planner constraints.

Lifecycle states are `proposed`, `deferred`, `resurfaced`, `admitted`, `rejected`, `superseded`, and `archived`. New candidates start as `proposed`; `proposed` candidates may be admitted, rejected, deferred, superseded, or archived. Deferred candidates may become `resurfaced`, rejected, superseded, or archived. Resurfaced candidates may be admitted, rejected, deferred again, superseded, or archived. Admitted, rejected, and superseded candidates remain historical records and may later be archived according to retention policy; archiving is not erasure unless a separate redaction or deletion policy permits it.

Unadmitted candidates may influence only review surfaces: ordering, grouping, explanations, deduplication diagnostics, clarification prompts, local suggestions, and explicitly labeled preview diffs. They must not mutate Tasks, tags, dependencies, decompositions, clarification answers, Preferences, Logs, Snapshots, Plans, Calendars, Relationships, Associations, projection state, capability grants, disclosure policy, external messages, or external actions. They must not become hidden planner constraints. Enforcement is by requiring admitted-state reducers, planners, projection writers, and mutation call sites to consume only admitted canonical objects or explicit preview inputs, while candidate reads remain scoped to review APIs that preserve candidate-state labels and Compartment policy.

Deferral preserves an unresolved proposal without treating it as false. A deferred candidate may resurface only on materially new evidence, user request, a policy-triggered review interval, a relevant accepted change to the candidate target or dependencies, or arrival of a clarification or external reference that directly changes reviewability. Resurfacing links the prior deferral, trigger evidence, and reason the old deferral no longer controls; it creates a review item and never admits the candidate automatically.

Rejection is durable correction metadata. A rejected candidate retains a privacy-minimized suppression record containing candidate kind, normalized proposal shape, target and scope shape, Compartment and redaction class, evidence hashes or source fingerprints, extractor/model/schema version, prompt or template digest when relevant, rejection reason or user correction, deciding actor Identity, `authority_source`, decided-at time, retention or purge policy, and the suppression key. The rejected payload may be redacted or purged under retention policy once the suppression record is sufficient; retaining the key must not make the rejected proposal accepted, exportable, more visible, or usable as evidence that the rejected content was true.

Admission, rejection, deferral, resurfacing, supersession, and archival decisions are first-class review events carrying actor Identity, `authority_source`, origin Device, observed candidate version, observed policy versions when policy is relied on, effective time, recorded time, idempotency key, and links to resulting canonical mutations or replacement candidates. Admission emits the ordinary sync-ready mutation envelope for the admitted object change; rejection emits the decision event and suppression record without creating the proposed canonical state.

Review order never changes state category. Proposed and resurfaced candidates may be prioritized ahead of deferred or archived records, but a contested region remains `candidate_state` or `pending_state` until an admission or conflict-resolution event is itself admitted. UI, APIs, derived previews, and reports must label candidate diffs as candidate or preview material and must not present them as `admitted_state` merely because they are high priority, old, repeated, or uncontested in the queue.
```

#### DESIGN.md §1 — Overview

```markdown
## 1. Overview

**UbU** is a privacy-first planning, coordination, and self-governance system for implementing life logistics.

UbU takes messy real-world inputs—tasks, calendar events, messages, external events, user preferences, physical/emotional state, Resource availability, Skill capability, integration data, realtime interaction streams, and agent or worker outputs—and turns them into explicit, inspectable, recalculable Plans.

UbU is not merely a task list or calendar application. Its core motivation is to help an individual human being plan and implement the logistics of an actual life: what matters, what must happen, what the world must contain, what the user is capable of doing, what can be learned, what should be bought or delegated, and what should happen next. Organizational coordination is an important emergent property of the same model, not the root purpose.

The canonical UbU use-case formula is:

> **Objective + Current State + Constraints + Resources + Skills + Techniques + Preferences + External Options → Legitimate Plan**

The canonical use-case statement: UbU helps an individual user solve everyday life problems by transforming a desired outcome into a legitimate, executable plan. It does this by modeling the current state of the user's life, the desired state, required Tasks, available and missing Resources, available and missing Skills, reusable Techniques, affect and energy constraints, financial tradeoffs, public or marketplace options, expert-guided alternatives, and evidence from execution. UbU's purpose is not merely to schedule time, but to help the user determine what path can realistically move life from the current state toward the desired state.

The deeper purpose is to help people build the capabilities, resources, routines, and relationships needed to actually live the life they choose — not just to schedule it.

The first MVP is designed around **dogfooding**: using UbU to coordinate the design, development, release, and maintenance of UbU itself.

Recent LLM and agentic-AI changes reinforce UbU's core boundary: realtime, multimodal, tool-using, memory-bearing models are valuable interaction and extraction backends, but UbU remains the user-sovereign state-transition, planning, logging, privacy, and review layer.

For Phase 1, UbU must also be understandable as a first-person user experience. The minimal user-facing loop is: answer a small number of bootstrapping questions, allow UbU to construct an initial context model, receive one recommended next Task, inspect why that Task matters now, act or override, then let UbU learn from the result through Logs, Snapshots, and recalculation.

---
```

### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md — referenced sections

#### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §4 — Core definitions

```markdown
## 4. Core definitions

### Device **(net-new)**

A `Device` is a UbU execution enclave, not merely physical hardware. Examples: phone,
laptop, desktop, home server, browser session, removable sync-bundle importer, personal
GPU worker, temporary recovery environment.

### Zone **(net-new)**

A `Zone` is a user-controlled replication and work-context boundary — not a server.
Examples: personal Zone, household Zone, UbU project Zone, work/client Zone, experimental
Zone.

### Compartment

A `Compartment` is an existing UbU model noun (already referenced in the Phase 0 cutlist
as a deferred Phase 1+ model). In Phase 0 it surfaces only as `compartment_ids:
list[str]` and `redaction_level: str` on `PrivacyAndProvenance`. Phase 2 introduces a
**Compartment policy object** (replication/routing/retention/export) that references
compartments by the same `compartment_ids` convention. The policy object itself is
net-new; the compartment id vocabulary is not.

### SyncStatement **(net-new)**

A `SyncStatement` is a signed or integrity-protected state-change envelope exchangeable
through many transports.

### Replica **(net-new)**

A `Replica` is the local state view held by a Device. It may be full, partial, redacted,
stale, or restricted.

### Admitted state

Admitted state is the deterministic result of applying accepted sync statements, policy
updates, corrections, and conflict-resolution statements according to this contract's
causality and conflict rules. See §9 for the precise determinism claim.

---
```

### PLANNING_KERNEL_CONTRACT.md — referenced sections

#### PLANNING_KERNEL_CONTRACT.md §3 — `TaskSpec` duration, allowed-range, and correlation fields

```markdown
## 3. `TaskSpec` duration, allowed-range, and correlation fields

Each `TaskSpec` used by the planning kernel must include either a fixed duration model or a three-point shifted-log-normal duration model.

Each Dynamic `TaskSpec` also carries a single hard `window: { earliest_start, latest_finish }` with RFC 3339 / ISO 8601 UTC timestamps. The CPU side builds it by intersecting the admitted Task's one-off `allowed_time_range`, or the concrete range on an instantiated routine Task, with `PlanningRequest.time_window`. If the intersection is empty or shorter than the Task's minimum possible duration, the Task is unplaceable for that request rather than soft-scored outside the window. Multiple allowed windows are not represented in Phase 1b `TaskSpec`; any future support requires a contract change or CPU-side occurrence/window selection before dispatch.

### Fixed duration

```text
{ type: "fixed", seconds: positive_integer }
```

A fixed duration is represented as a delta distribution. It must not be represented as a very tight log-normal distribution.

### Shifted log-normal three-point duration

```text
{
  type: "shifted_lognormal_p95",
  min_seconds: non_negative_number,
  mode_seconds: positive_number,
  p95_seconds: positive_number
}
```

Semantics:

- `min_seconds` is the optimistic lower support shift. It is used as the mathematical lower support for sampling. It is not a claim that real-world durations physically cannot be shorter; it is a modeling prior representing the user's subjective best-case estimate.
- `mode_seconds` is the most likely duration.
- `p95_seconds` is the 95th percentile and not a hard upper cap. The planner may and will sample durations greater than `p95_seconds`.
- All units are seconds.
- Required ordering for this model: `0 <= min_seconds < mode_seconds < p95_seconds`.

The shifted distribution is:

```text
D = min_seconds + LogNormal(mu, sigma)
```

Let:

```text
a   = mode_seconds - min_seconds
b   = p95_seconds  - min_seconds
z95 = 1.6448536269514722
```

Then:

```text
sigma = (-z95 + sqrt(z95^2 + 4 * ln(b / a))) / 2
mu    = ln(a) + sigma^2
```

Invalid triples are rejected at `TaskSpec` construction and again when the CPU builds `PlanningRequest` tensors. Errors should identify the Task ID and the violated ordering, unit, or positivity constraint. A UI may offer to convert `min = mode = p95` to `type: fixed`, but the schema must not silently repair an invalid log-normal triple.

The explicit `p95_seconds` field name replaces the ambiguous `max` field name used in prior design drafts.

### Correlation groups

Each stochastic-duration Task may carry:

```text
correlation_groups: [{ group: string, strength: float }]
```

Phase 1 rules:

- `strength` is a positive latent-factor loading in `[0, 1]`.
- Negative correlations are deferred beyond Phase 1.
- Empty `correlation_groups` means independent idiosyncratic duration noise.
- Duplicate group names within one Task are invalid.
- Group names are opaque stable identifiers; display labels belong elsewhere.

---
```

#### PLANNING_KERNEL_CONTRACT.md §5 — GPU pipeline stage boundaries

```markdown
## 5. GPU pipeline stage boundaries

The following stage-boundary contracts are design artifacts. Exact tensor dtypes, device placement, batching mechanics, and PyTorch implementation classes belong in `model-committee`, but their semantic inputs and outputs are fixed here. Chunked search is executed inside one planning invocation for the whole request; the worker may batch chunks internally to fit memory, but separate chunks are not separate semantic requests.

### Phase 1 recommended tensor profile

The Phase 1 PyTorch implementation should use padded fixed-size tensors and masks using stable implementation-facing names. This profile is recommended for tests and code generation, but exact dtype and class layout remain implementation details.

Common names:

- `task_index`: padded task-slot index within `MAX_PLANNING_TASKS`.
- `validity_mask`: boolean mask for occupied task slots or valid candidates.
- `start_time_offsets`: planned start offsets from `time_window.start_time`, in planning ticks or seconds as declared by the implementation profile.
- `duration_samples`: sampled duration tensor.
- `feasible_mask`: boolean candidate feasibility mask after stage-local checks.
- `surviving_indices`: compacted indices of candidates that survive a filter stage.
- `feasibility_scores`: per-candidate or per-candidate/per-dimension satisfaction scores.
- `composite_scores`: stage-3 aggregate scores before rollout.
- `top_k_indices`: finalist indices selected for rollout.
- `probability_interval_low` / `probability_interval_high`: lower and upper probability interval outputs when estimable.

### Stage 1: `skeleton_sampling`

Consumes:

- CPU-provided `topological_order` and validity masks;
- dependency edge data or adjacency tensors;
- duration-model parameters;
- time-window and planning-delta parameters;
- candidate seeds derived from `rng_seed`.

Produces:

- candidate schedule tensor or equivalent structured schedule batch;
- `start_time_offsets`;
- `duration_samples`;
- candidate `validity_mask`;
- dependency-slack summary;
- rejection/failure code per invalid candidate.

### Stage 2: `affect_legitimacy_filter`

Consumes:

- stage-1 candidate schedules;
- candidate validity masks;
- AffectProfile tensors;
- predicted or declared affect state needed by the Phase 1 filter.

Produces:

- per-candidate per-dimension affect satisfaction scores in `[0, 1]`;
- per-candidate aggregate minimum affect score;
- affect-margin score;
- `feasible_mask`;
- `surviving_indices`;
- violated-dimension codes.

This stage implements only the sigmoid affect-constraint portion of legitimization. Full legitimization remains broader than this GPU stage.

### Stage 3: `value_scoring`

Consumes:

- surviving candidate schedules;
- Task value/priority metadata;
- scoring policy weights;
- affect-margin outputs;
- schedule-diversity policy.

Phase 1b `TaskSpec.value` is a CPU-computed float in `[0.1, 1.0]` derived from admitted explicit Task prioritization for the request. Given dense priority buckets ordered best to worst, bucket position `p`, and bucket count `m`, value is `1.0` when `m = 1` and otherwise `1.0 - 0.9 * p / (m - 1)`. Tied Tasks share `p`; unranked eligible Tasks use `0.1`. In Phase 1b the CPU sends `TaskSpec.priority` as `1.0`, because Stage 3 utility multiplies value by priority; the normalized priority rank stays in the orchestrator's explanation and diagnostic records (`UBU-D0282`).

The canonical persisted input is the set of admitted pairwise Task Preferences, layered into dense buckets at request time (`UBU-D0009`, `UBU-D0282`). The bounded value and normalized priority are transient request metadata and must not be persisted as utility. Routine instances enter Stage 3 as ordinary TaskSpecs after recurrence instantiation: explicit occurrence priority wins, then routine-template or evergreen-Objective priority. Objective labels, deadlines, missed occurrences, and cadence do not rewrite value/priority unless admitted prioritization changes the underlying rank.

Produces:

- utility score;
- approximate robustness score before finalist rollout;
- affect-margin score;
- schedule-diversity score;
- `composite_scores`;
- `top_k_indices` or finalist selection set for Monte Carlo rollout.

### Stage 4: `monte_carlo_rollout`

Consumes:

- top-K or top-M finalist candidates;
- shifted-log-normal duration parameters;
- correlation matrix from §7;
- external event assumptions;
- rollout count and rollout seed stream.

Produces:

- feasibility frequency by candidate;
- outcome distribution summaries;
- worst-case or lower-percentile robustness estimate;
- probability summaries and intervals when estimable, including `probability_interval_low` and `probability_interval_high` where supported;
- rollout diagnostics and degradation warnings.

Stage 4 uses deterministic rollout seed derivation from the request seed. Phase 1 may use `rng_seed + 3` as the stage-4 rollout seed stream convention until named substreams are introduced. GPU floating-point rollout summaries are reproducible within the recorded tolerance profile, not bitwise across hardware, drivers, PyTorch versions, or reduction orders.

### CPU/GPU parity expectations

Parity tests compare the GPU worker with the CPU reference path and CPU-only goldens. Schema decoding, chunk partitioning, task-slot `validity_mask`, dependency feasibility, hard-constraint feasibility, rejection classes, and CPU-certified selected Plan validity must match exactly. Floating-point scores, rollout frequencies, probability intervals, and schedule-diversity scores match by documented absolute/relative tolerances or statistical acceptance tests tied to rollout count and seed. CPU certification is the final authority for any selected Plan.

---
```

## JSON Schema

Your output must satisfy this schema:

```json
{
  "type": "object",
  "additionalProperties": false,
  "required": [
    "proposal_id",
    "provider_id",
    "model_name",
    "question_id",
    "base_commit",
    "summary",
    "rationale",
    "changed_files",
    "patch",
    "commit_message",
    "validation_notes",
    "new_questions_added",
    "questions_resolved",
    "decisions_added",
    "requires_human_review"
  ],
  "properties": {
    "proposal_id": {
      "type": "string"
    },
    "provider_id": {
      "type": "string"
    },
    "model_name": {
      "type": "string"
    },
    "question_id": {
      "type": "string",
      "pattern": "^UBU-Q[0-9]{4}$"
    },
    "base_commit": {
      "type": "string"
    },
    "summary": {
      "type": "string"
    },
    "rationale": {
      "type": "string"
    },
    "changed_files": {
      "type": "array",
      "items": {
        "type": "string",
        "enum": [
          "DECISIONS.md",
          "DESIGN.md",
          "OPEN_QUESTIONS.md",
          "PLANNING_KERNEL_CONTRACT.md"
        ]
      }
    },
    "patch": {
      "type": "string"
    },
    "commit_message": {
      "type": "string"
    },
    "validation_notes": {
      "type": "array",
      "items": {
        "type": "string"
      }
    },
    "new_questions_added": {
      "type": "array",
      "items": {
        "type": "string",
        "pattern": "^UBU-Q[0-9]{4}$"
      }
    },
    "questions_resolved": {
      "type": "array",
      "items": {
        "type": "string",
        "pattern": "^UBU-Q[0-9]{4}$"
      }
    },
    "decisions_added": {
      "type": "array",
      "items": {
        "type": "string",
        "pattern": "^UBU-D[0-9]{4}$"
      }
    },
    "requires_human_review": {
      "type": "boolean"
    }
  }
}
```

## Requirements

- Return exactly one JSON object.
- Do not include hidden reasoning, `<think>` tags, markdown fences, or explanatory text outside the JSON object.
- All fields are required.
- Arrays may be empty.
- The patch must be a full git-diff-style patch.
- The patch may modify only:
  - `DESIGN.md`
  - `DECISIONS.md`
  - `OPEN_QUESTIONS.md`
  - `PLANNING_KERNEL_CONTRACT.md`
- Do not modify `README.md`, `OUTREACH.md`, hidden files, scripts, code files, or generated logs.
- Preserve the single-line metadata format in `OPEN_QUESTIONS.md`.
- The `patch` string must be a raw unified diff as produced by `git diff`; do not wrap it in markdown fences or prose.
- Every file diff must start with `diff --git a/<path> b/<path>`, followed by `--- a/<path>` and `+++ b/<path>`.
- Every hunk must include accurate `@@ -old_start,old_count +new_start,new_count @@` ranges and enough unchanged context for `git apply --check` to apply without `--recount`.
- When editing `OPEN_QUESTIONS.md`, anchor hunks with the selected question heading `## UBU-Q0157: Splittable Tasks and resume overhead` and its own `### Resolution` section. Do not use a repeated `### Resolution` heading from an earlier or later question as the edit location.
- If resolving the selected question, replace the `Unresolved.` text under that selected question's `### Resolution` section and update that same question's metadata line. Do not insert selected-question resolution text into any other question block.
- Use the existing `UBU-Qxxxx` and `UBU-Dxxxx` numbering conventions.
- Prefer minimal, auditable changesets.
- If the selected question is blocked, propose decomposition only if it produces replacement questions with fewer, simpler, or no dependencies.
- If the selected question is already partially resolved, narrow or clarify it rather than pretending it is fully unresolved.

Return only JSON.
