# Model-Committee Work Proposal Request

You are participating in the UbU `model-committee` process.

Your task is to produce one concrete work proposal as strict JSON.

Do not return prose outside the JSON object.

## Selected question

Question ID: `UBU-Q0158`  
Question title: `Outcome branching at chunk boundaries and compact Calendar coverage`  
Base commit: `52ff8411256450e3c25c3469891cbf46d9880656`

```markdown
## UBU-Q0158: Outcome branching at chunk boundaries and compact Calendar coverage

Status: Open Priority: MVP important Phase: Phase 1b Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: compact Calendar coverage, mobile stewardship packaging Resolved by: None Last scored: 2026-09-21 Scored from commit: None

Defining context: DESIGN.md §16.2, DESIGN.md §16.3, DESIGN.md §16.5, PLANNING_KERNEL_CONTRACT.md §2, `UBU-D0279`, `UBU-D0280`, `UBU-D0281`.

### Question

How does the compact Calendar cover the stochastic range of possible futures under chunked search? Alternatives per chunk (`UBU-D0280`) are decision branches, not outcome branches. Coverage (§16.2) is probability mass over outcomes: durations, success and failure, and interruptions.

### Subquestions

1. **Outcome states.** Fixed placements realign the clock at every chunk boundary, so futures differ there only in which units completed, how far splittable Tasks progressed (`UBU-Q0157`), and the carried state. Which of these define an outcome branch, and how are near-identical outcomes grouped?
2. **Probabilities.** Per-chunk rollouts with shared latent draws (`UBU-D0279`) yield outcome frequencies. How are outcome probabilities estimated, and with what confidence?
3. **Coverage accounting.** How does the probability mass of the outcomes that have precomputed continuations map to `branch_coverage_target` and `reactive_horizon_seconds` (contract §2), and to the coverage fields of the response?
4. **Budget.** Under a fixed compute budget, how is effort split between decision branching (K alternatives per chunk) and outcome branching (continuations for probable outcomes)?
5. **Packaging.** Which continuations enter the compact Calendar and the mobile stewardship metadata (§16.5), and how many chunks deep?
6. **Inside a chunk.** Is a deviation inside a chunk handled only by decision envelopes and repair recipes (§16.5), rather than by precomputed branches?
7. **Streaming.** Are outcome continuations streamed with interactive chunk results (`UBU-D0281`), or computed after the first chunk is delivered?

### Current direction

At each chunk boundary the sweep precomputes next-chunk continuations for the most probable outcome states, in descending probability, until the coverage target is met within the reactive horizon. Decision alternatives and outcome continuations share one compute budget. A deviation inside a chunk remains local repair. The mobile stewardship package carries the current chunk's plan and the continuations for its probable outcomes.

### Resolution

Open.

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
- next decision id: `UBU-D0285`

`DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md` is read-only context and is not in the patch
allowlist: do not propose changes to it.

### DECISIONS.md — cited decisions

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

#### UBU-D0280

```markdown
## UBU-D0280: The chunk sweep is a bounded beam with state merging and constraint-aware look-ahead

**Status:** Accepted → DESIGN.md §16.3

The chunked search of `UBU-D0279` expands each surviving branch into up to K alternative plans for the next chunk. K defaults to the Phase 1 candidate count of 3 and is tunable. Unbounded, this expansion grows as K raised to the number of chunks, so the sweep keeps a beam: after each chunk depth, only the best B partial plans survive, with B tuned to the execution profile's memory budget.

Branches that reach the same state after a chunk are merged, and the best-scoring one is kept. The state is the set of units still unplaced together with the carried state: UniverseState effects, dependency completion, and the affect summary. Many orderings of one chunk leave the same remainder, so merging turns the sweep into dynamic programming over chunks and removes most redundant branches.

Each chunk's assignment looks ahead cheaply, so that early chunks do not consume units that only fit later or leave deadlines unmet. Units with the fewest remaining eligible chunks, or the nearest deadlines, are assigned first. This most-constrained-first rule is a heuristic ordering of the assignment level, not an additional hard constraint.

The K alternatives for the first chunk stay alive while later chunks are searched. The best of them is presented as soon as it is certified. A later-found continuation may make another first-chunk alternative better by more than a configured replacement margin before the user starts that chunk; the presented plan is then replaced, and the margin keeps it from churning. Once the user starts executing, branches inconsistent with the observed execution are pruned.

Consequences:

- `DESIGN.md` §16.3 records the sweep mechanics.

---
```

#### UBU-D0281

```markdown
## UBU-D0281: Interactive planning streams certified chunk results; batch planning does not

**Status:** Accepted → DESIGN.md §16.3, §16.10.2

A planning request declares either interactive or batch delivery. Interactive delivery serves a user who wants to act now, for example after an interruption, without waiting for a long horizon to finish. The engine emits the results for each chunk depth as that depth completes, and the CPU certifies each chunk result on arrival; certifying chunks separately is sound because chunks are independent given their boundary state (`UBU-D0279`). The first chunk's plan therefore reaches the user almost immediately while later chunks are still being searched. A streamed chunk plan stays provisional until the user starts it (`UBU-D0280`) and is fixed afterwards. The final response completes the stream.

Batch delivery, such as an overnight run over a long horizon, returns only the final response and does not stream.

Streaming changes how an interactive response is delivered. It therefore amends `UBU-D0279`'s statement that the `PlanningRequest`/`PlanningResponse` contract is unchanged: the request gains a delivery mode, and the response gains a streamed partial-result form for interactive delivery. How that stream is framed across the engine boundary is settled together with the invocation boundary in `UBU-Q0156`.

Consequences:

- `DESIGN.md` §16.3 and §16.10.2 record interactive and batch delivery.
- `OPEN_QUESTIONS.md` adds a streaming subquestion to `UBU-Q0156` and records chunk-boundary outcome coverage as `UBU-Q0158`.

---
```

### DESIGN.md — referenced sections

#### DESIGN.md §16.2 — Coverage

```markdown
### 16.2 Coverage

Coverage belongs to the compact serialization, not the abstract Calendar. It is an estimated regeneration metric in Phase 1, not an exact proof.

Coverage represents the modeled probability mass of possible futures covered by the compact representation within the recorded coverage scope, usually the reactive horizon. MVP coverage includes modeled duration uncertainty, Task success/failure uncertainty when success probabilities affect branch reconstruction, and modeled external-event or interruption assumptions. Objective recurrence uncertainty is not included in MVP coverage; evergreen recurrence is evaluated deterministically before Calendar generation or repair unless a later schema adds stochastic recurrence inputs.

A compact Calendar stores `coverage_estimate`, `uncovered_mass_estimate`, `coverage_scope`, `coverage_threshold_used`, `coverage_inputs_summary`, `probability_quality`, and warning or diagnostic refs when inputs are unmodeled, stale, or degraded.

The default Phase 1 regeneration threshold is `0.99` short-horizon branch coverage. The effective policy resolves Calendar-specific override first, then execution-profile or Device policy, then the global default. The effective threshold is stored with the compact Calendar for replayable regeneration decisions.

After time advances, coverage is recalculated by conditioning the compact Calendar on elapsed time and accepted Logs, Snapshots, External Events, and user actions, then discarding expired branch mass and re-estimating the remaining covered mass.

If recalculated coverage falls below the effective threshold, UbU records or batches a `low_compact_calendar_coverage` recalculation trigger and includes the finding in risk reporting. Immediate recalculation is required only when low coverage can affect the current or next recommended Task, hard feasibility, affect legitimacy, or the default Plan; otherwise marking the Calendar stale is sufficient. Low coverage does not directly create a canonical Task or worker assignment.
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

**Splittable Tasks (`UBU-D0284`).** Atomic Tasks and compiled decomposition segments occupy one chunk. A splittable Task may assign at most one piece to each eligible chunk, and every piece must remain inside the Task's allowed range. The chunk-assignment level chooses how much work from the Task enters each chunk; ordinary within-chunk ordering then places that piece. The first piece has no resume overhead, while each later piece reserves the Task's constant `resume_overhead_seconds` before doing more work. Rollouts draw one total work duration for the Task, spend it across pieces, and count a continuation into a later eligible chunk as feasible only when `max_pieces`, window, dependencies, and carried state still permit it. Splittable work earns proportional scheduled-work value for completed sampled work inside the horizon, but completion, effects, and outgoing dependencies still wait for the last piece. Phase 1b needs no additional interruption input beyond the split policy; later stochastic interruption modeling can use the same policy to distinguish continuation from restart.

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

#### DESIGN.md §16.10.2 — CPU/GPU handoff contract

```markdown
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
```

#### DESIGN.md §16.10.4 — Pipeline stages

```markdown
#### 16.10.4 Pipeline stages

The GPU engine has four first-class pipeline stages, with semantic boundaries specified in `PLANNING_KERNEL_CONTRACT.md §5`.

1. **Skeleton sampling** — parallel shifted-log-normal or fixed duration sampling across candidates with vectorized propagation over the CPU-provided topological order.

2. **`affect_legitimacy_filter`** — batch sigmoid affect-constraint evaluation across surviving candidates. This stage implements only the sigmoid affect-constraint portion of legitimization. Full legitimization in UbU design is broader: it makes a skeleton Plan human-viable by respecting affect, recovery, transition, rest, sustainability, and support constraints. The `affect_legitimacy_filter` GPU stage does not replace or subsume full legitimization.

3. **Value scoring** — parallel utility, approximate robustness, affect-margin, and schedule-diversity scoring across surviving candidates.

Phase 1b value scoring uses CPU-computed, request-local Task value/priority metadata derived from admitted pairwise Task Preferences (`UBU-D0277`, `UBU-D0282`). Objective labels, deadlines, routine cadence, and revealed-preference evidence may inform review, risk, urgency, or later policy work, but the GPU stage receives only the bounded transient values supplied in the `PlanningRequest`.

4. **Monte Carlo rollout** — joint scenario simulation for finalists using correlation-group Gaussian copula sampling with deterministic rollout seed derivation.

GPU search proposes candidates. Hard constraint certification and final Plan validity are performed by the CPU kernel using exact or conservative validation.

In Phase 1b the stages operate on chunked candidates (`UBU-D0279`, §16.3): skeleton sampling builds candidates chunk by chunk, and the Monte Carlo rollout shares its latent draws across chunks. Chunked search stays inside one planning invocation for the whole request; the worker may batch chunks internally to fit memory, but the CPU does not dispatch each chunk as a separate semantic request.
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

#### PLANNING_KERNEL_CONTRACT.md §2 — `PlanningRequest`

```markdown
## 2. `PlanningRequest`

`PlanningRequest` is the complete CPU-to-planning-engine handoff object for Phase 1.

### Required fields

- `schema_version`
- `planner_version`
- `request_id`
- `effective_time`
- `generated_at`
- `mode`: enum, one of `fresh_generation`, `repair`.
- `delivery_mode`: enum, one of `interactive`, `batch`. Interactive delivery permits chunk-result stream frames; batch delivery returns only the final response.
- `rng_seed`: integer seed. Required for reproducibility and peer debugging.
- `time_window`:
  - `start_time`: RFC 3339 / ISO 8601 UTC timestamp.
  - `end_time`: RFC 3339 / ISO 8601 UTC timestamp.
  - `planning_delta_seconds`: positive integer. Phase 1 default is `60`.
- `horizon_policy`:
  - `reactive_horizon_seconds`: positive integer. Phase 1 default is `3600`.
  - `branch_coverage_target`: float in `(0, 1]`. The CPU supplies the effective value after Calendar, execution-profile, Device, and global policy resolution. Phase 1 default is `0.99`.
- `compute_budget`:
  - `max_planning_tasks`: positive integer. Phase 1 default is `256`.
  - `n_candidates`: positive integer chosen by the CPU kernel.
  - `n_rollouts`: non-negative integer chosen by the CPU kernel. Phase 1 default is `1000` per finalist.
  - `k_candidates`: positive integer. Phase 1 default is `3`.
  - `max_wall_clock_ms`: optional positive integer budget.
- `task_graph`:
  - `tasks`: ordered list of `TaskSpec` objects.
  - `dependency_edges`: list of directed edges `{before_task_id, after_task_id}`.
  - `topological_order`: CPU-provided topological task ID list. The CPU validates the DAG and the GPU engine does not perform its own topological sort.
- `universe_state_snapshot`:
  - `snapshot_id`.
  - `snapshot_effective_time`.
  - `state_refs` or compact state facts needed by the planner.
  - `payload_policy_summary`: compact CPU-generated summary of the Compartment, redaction, and disclosure policies applied during context assembly.
- `affect_profile`: required Phase 1 profile using the schema in §6.
- `scoring_policy`:
  - `utility_weight`.
  - `robustness_weight`.
  - `affect_margin_weight`.
  - `schedule_diversity_weight`.
  - omitted weights default to the CPU kernel's Phase 1 defaults. All weights must be non-negative; zero is valid to disable a scoring dimension.
- `constraint_policy`:
  - `hard_constraints_always_required`: must be `true`.
  - `affect_constraint_mode`: enum, one of `enforce`, `warn_only`. Phase 1 default is `enforce` in user-facing contexts and `warn_only` in non-user-facing test fixtures or explicit onboarding/debug contexts.
- `privacy_and_provenance`:
  - `compartment_ids`: list of compartment IDs represented in the request.
  - `redaction_level`: enum or policy label.
  - `payload_safety_proof`: CPU-generated assertion that no denied Compartment data, forbidden identity disclosure, export-banned data, or integration-prohibited data is included.
  - `payload_verified_safe`: derived boolean that must be true before dispatch; it is not a substitute for `payload_safety_proof`.

### Optional fields

- `external_event_assumptions`: list of structured stochastic or fixed external event assumptions. Defaults to empty.
- `repair_context`: required when `mode = repair`, absent otherwise.
  - `prior_plan_id`
  - `last_legitimate_plan_ref`
  - `observed_divergence_refs`
  - `repair_scope`: enum, one of `local`, `remaining_window`, `full_window`.
- `explanation_request`:
  - `include_rejection_summary`: boolean.
  - `include_score_breakdown`: boolean.
  - `include_user_facing_fragments`: boolean.
- `debug_flags`: implementation-specific diagnostic toggles. Must not change canonical semantics.

### Deferred fields

The Phase 1 contract deliberately defers mobile GPU targets, cloud GPU provider metadata, encrypted-compute metadata, cross-user coordination payloads, realtime stream state, and premium wide-horizon planning-provider negotiation. These may be added later without changing the Phase 1 CPU/GPU authority boundary.

Solver/library identity is not part of `PlanningRequest`. Optional OR-Tools, SMT/MaxSMT, local-search, mobile GPU, cloud GPU, or learned-model backends must preserve this request/response contract and remain advisory until CPU certification. Backend choice is recorded on `PlanningResponse.engine_provenance`, not requested as a semantic planner input.

### Phase 3 note: Objective expansion is pre-kernel; `scoring_policy` carries trade-off weights

The Phase 3 Objective-to-Task expansion and technicalizing stage (DESIGN.md §15.2.1.1) runs on the CPU side **before** this contract is invoked. The kernel continues to receive a fully materialized `task_graph`; it does not expand Objectives, select Techniques, or consult any advisory model. This preserves the fixed `task_graph` input, the CPU-provided `topological_order`, the absence of Objectives/Techniques from `PlanningRequest`, and the kernel's determinism and no-I/O guarantee. Alternative Technique instantiations enter as separate candidates (either enumerated CPU-side and scored independently, or, if a later schema adds them, as mutually-exclusive choice-group nodes the kernel selects among deterministically — see `UBU-Q0125` subquestion 3).

The existing `scoring_policy` weights (`utility_weight`, `robustness_weight`, `affect_margin_weight`, `schedule_diversity_weight`) are the parameterization point for the user's trade-off preferences in multi-Technique outcome comparison (DESIGN.md §16.3.1). A user's revealed choice may **propose** an update to these weights for explicit user acceptance; the kernel itself neither learns nor mutates them, consistent with `UBU-D0217`.

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
- When editing `OPEN_QUESTIONS.md`, anchor hunks with the selected question heading `## UBU-Q0158: Outcome branching at chunk boundaries and compact Calendar coverage` and its own `### Resolution` section. Do not use a repeated `### Resolution` heading from an earlier or later question as the edit location.
- If resolving the selected question, replace the `Unresolved.` text under that selected question's `### Resolution` section and update that same question's metadata line. Do not insert selected-question resolution text into any other question block.
- Use the existing `UBU-Qxxxx` and `UBU-Dxxxx` numbering conventions.
- Prefer minimal, auditable changesets.
- If the selected question is blocked, propose decomposition only if it produces replacement questions with fewer, simpler, or no dependencies.
- If the selected question is already partially resolved, narrow or clarify it rather than pretending it is fully unresolved.

Return only JSON.
