# Model-Committee Work Proposal Request

You are participating in the UbU `model-committee` process.

Your task is to produce one concrete work proposal as strict JSON.

Do not return prose outside the JSON object.

## Selected question

Question ID: `UBU-Q0154`  
Question title: `Phase 1b routines as evergreen-Objective recurrence`  
Base commit: `eff78941c94850ea1f8ec7e3db6dfba1b168477e`

```markdown
## UBU-Q0154: Phase 1b routines as evergreen-Objective recurrence

Status: Open Priority: MVP blocker Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0152 Blocks: Phase 1b routine port, Quick UbU routine import Resolved by: None Last scored: 2026-09-21 Scored from commit: None

Defining context: DESIGN.md §7.4.1, DESIGN.md §15.2.1.1, `UBU-D0213`, `UBU-D0214`, `UBU-D0216`, `UBU-D0275`. This question takes the Phase 1b slice of `UBU-Q0125` subquestions 1 and 10.

### Question

How are Quick UbU routines represented and instantiated in Phase 1b, given that recurrence lives on evergreen Objectives and Tasks are pure instances?

### Subquestions

1. **Schedule subset.** Quick UbU's 71 routine templates use daily, weekly-by-weekday, day-of-month, first-workday-of-month, and first-workday-of-quarter rules. Which subset of the §7.4.1 calendar-style schedule does Phase 1b implement? Does first-workday need a workday calendar, and does that make exceptions (holidays) a Phase 1b requirement?
2. **Routine to Objective.** Is each routine template one evergreen Objective? Where do the template's title, duration, `category_tag`, `occupies_capacity`, reminders (projection-only), nominal start, and allowed time-of-day range live?
3. **Minimal Task synthesis.** Is each occurrence instantiated by a default Technique, or by a direct instantiation rule on the Objective with Technique expansion left in Phase 3?
4. **Instantiation window and identity.** When are occurrence Tasks created (per planning horizon, or over a rolling window)? How are duplicates prevented, for example with deterministic occurrence ids? How do rule edits reach future occurrences that already exist?
5. **Relative routines.** Some templates start a fixed offset after another routine (Quick UbU's `after`). Is that a dependency with a minimum lag, or an allowed range derived from the predecessor?
6. **Static or planned.** Does each instance carry both its nominal start and its allowed range, with a per-routine choice between Static placement and planner placement, so that routines can move to the planner one at a time? Capacity-occupying Static routine instances bound chunks (`UBU-D0279`), so moving a routine to planner placement also merges the chunks it separated.
7. **Completion and misses.** How do done, skipped, and missed occurrences update the evergreen Objective, including streaks (`UBU-Q0111`)?
8. **Import.** How do the existing Quick UbU templates map onto this representation? Importing them is a soft goal.

### Current direction

Each routine template becomes an evergreen Objective carrying a Phase 1b calendar-style schedule in the local time zone, without general exception sets unless first-workday rules require a workday calendar. A direct instantiation rule creates one Task per occurrence inside the planning horizon, with deterministic ids. Instances carry a nominal start and an allowed range (`UBU-Q0152`), and each routine is either placed as a Static Task or planned within its range, so it can move to planner placement once the planner proves itself on it. Technique-based expansion stays in Phase 3.

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
- next decision id: `UBU-D0286`

`DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md` is read-only context and is not in the patch
allowlist: do not propose changes to it.

### OPEN_QUESTIONS.md — related questions

#### UBU-Q0152

```markdown
## UBU-Q0152: Allowed time range on Tasks

Status: Solved Priority: MVP blocker Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0154, routine planning, retirement of occupies_capacity Resolved by: UBU-D0276 Last scored: 2026-09-21 Scored from commit: None

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

Resolved by `UBU-D0276`: One-off Dynamic Tasks use a declared `allowed_time_range: { earliest_start, latest_finish }` absolute UTC interval. The entire planned occupied interval must fit inside it; violations are hard infeasibility, not scored spillover. Static Tasks continue to use fixed start/end times, and decision envelopes remain derived Plan/mobile-repair outputs rather than Task inputs. Routine instances get their concrete range from recurrence instantiation under `UBU-Q0154`; recurring local time-of-day windows and multiple movable windows are outside the one-off Task shape and must be resolved before kernel dispatch. The orchestrator maps the single range to `TaskSpec.window` by intersecting it with the request `time_window`. Local time-of-day rules are converted during routine instantiation with IANA time zone data; ambiguous or nonexistent local endpoints require recorded disambiguation or review before becoming UTC Task ranges. `occupies_capacity` is retired only after implementation evidence shows Dynamic Tasks and routine instances with ranges are placed, partially omitted, and hard-validated without the old gate.

---
```

### DECISIONS.md — cited decisions

#### UBU-D0213

```markdown
## UBU-D0213: Evergreen recurrence gains a calendar-style schedule with exceptions

**Status:** Accepted → DESIGN.md §7.4.1

Resolved question: `UBU-Q0125` (partial; recurrence representation).

The MVP `maintenance_time_decay` recurrence field cannot express scheduled recurrence with named exceptions, such as "Mondays at 09:00, except holidays, when it moves to the next day at 08:00." Phase 3 extends evergreen Objective recurrence with a calendar-style schedule modeled on the RFC 5545 (iCalendar) base-rule-plus-exception pattern.

The schedule carries an RRULE-shaped base rule, EXDATE-shaped exclusions, RDATE-shaped additions, override entries for occurrences that differ in time or parameters, and an optional enablement window (start and end date/time).

The schedule is deterministic: evaluating it against a timezone and exception set yields a fixed occurrence series. This preserves the existing rule that evergreen recurrence is evaluated deterministically before Calendar generation. Stochastic recurrence remains a separate future extension and is out of scope.

The schedule lives on the evergreen Objective, not on a new object. An evergreen Objective whose recurrence yields no resolvable occurrences, or that lacks a default Technique for expansion, is a philosophical-consistency finding surfaced for user correction during ordinary review, not a hard logistical block, unless it prevents a required baseline.

**Consequences:**

- Cadence references that previously pointed at `Task.recurrence` move to this schedule (see `UBU-D0214`).

---
```

#### UBU-D0214

```markdown
## UBU-D0214: `Task.recurrence` is eliminated; recurrence lives on the evergreen Objective

**Status:** Accepted → DESIGN.md §7.4.1, §9

Resolved question: `UBU-Q0125` (partial).

`Task.recurrence` is removed from the Task field set as an over-design. Scheduled recurrence is a property of the evergreen Objective (`UBU-D0213`); a recurrence schedule reactivates the Objective, and planning synthesizes fresh Task instances per reactivation (`UBU-D0216`). This gives recurrence a single source of truth.

Cascading edits:

- Calendar-preview and Log-review cadence now adjust through the recurrence rule on their evergreen system Objectives rather than ordinary Task recurrence.
- Relationship-maintenance cadence already lives on an evergreen Objective's recurrence rule and is unaffected in substance.
- Streak tracking (`UBU-Q0111`) is simplified: recurring-completion chains are counted per evergreen Objective rather than per recurring Task.

**Consequences:**

- Tasks are pure instances; they do not carry their own recurrence.
- `UBU-Q0111` subquestion on per-Task versus per-Objective streak attribution resolves toward per-Objective.

---
```

#### UBU-D0216

```markdown
## UBU-D0216: Objective-to-Task expansion runs pre-kernel; technicalizing is candidate generation, not skeletonization

**Status:** Accepted → DESIGN.md §15.2.1.1, §16.3.1

Resolved question: `UBU-Q0125` (primary).

Phase 3 specifies how an Objective expands into the work Tasks that satisfy it — the long-acknowledged gap of Technique-generated Tasks. The expansion stage ("technicalizing") selects a Technique for each in-scope Objective and instantiates its Steps into Tasks.

Placement and boundaries:

- Expansion runs **pre-kernel, on the CPU side**, before skeletonization. It does not run inside the planning kernel. In-kernel expansion would break the fixed `task_graph` input, the CPU-owned topological order, the request schema, and the kernel's determinism and no-I/O contract (the last violated the moment Technique selection consults an advisory LLM).
- Default expansion is deterministic: the Objective's default Technique instantiates the baseline Task set, feeding the deterministic default-Plan path.
- Technicalizing — selection among alternative Techniques — is a candidate-generation and value-scoring concern, not a skeletonization concern. Skeletonization continues to do only dependency affixing and ordering.
- Synthesized Static Tasks enter skeletonization; synthesized Dynamic Tasks carry decision envelopes and are placed during candidate generation. No new placement machinery is required.
- Instantiating an already-modeled Technique's Steps is not "inventing a Technique" and does not violate the no-invention rule. Technicalizing and any advisory LLM select only among already-modeled Techniques. Novel Techniques remain the user-approved Expert-Guided DIY / Technique Commissioning flow (`UBU-D0208`).
- If synthesized Tasks have prerequisites, the pipeline is a bounded expand/skeletonize fixpoint with an explicit iteration cap, not a single pass. Exact bounds are open (`UBU-Q0125`).

**Consequences:**

- The §15.2.2 no-invention rule is preserved as a guardrail on technicalizing, not weakened.
- The kernel request/response contract and CPU certification boundary are unchanged.

---
```

#### UBU-D0256

```markdown
## UBU-D0256: Mobile stewardship metadata uses compact envelopes and bounded local repair

**Status:** Accepted → DESIGN.md §§16.5, 16.9. Resolves `UBU-Q0073`.

Mobile stewardship metadata is part of the Compact Calendar and exists to preserve current/next Task legitimacy on constrained devices without pretending to run full global optimization. The MVP record contains Task criticality, decision envelopes, shared conflict severity, cached explanation fragments, last-legitimate-Plan identity, simple repair recipe refs, next-best-action mode, recalculation triggers, and remote-assist eligibility. Learned local policy models and rich branch packaging remain post-MVP.

Task criticality has exactly three levels: `protected`, `flexible`, and `disposable`. Protected Tasks include Static Tasks, hard dependencies/preconditions, user commitments, legitimization support, recovery-critical work, and deadline-fragile work; mobile must not silently drop, shorten below minimum duration, or move them outside envelope. Flexible Tasks may be moved, split, or shortened inside envelope when constraints remain satisfied. Disposable Tasks may be omitted or replaced when doing so does not break downstream legitimacy.

Decision envelopes record the local movement boundary: earliest start, latest start, latest finish, movable windows, minimum and target duration, dependency/precondition refs, affected Static Task refs, affect/resource/location assumptions, repair budget, and staleness deadline. Local repairs are legitimate only while accepted Logs, Snapshots, External Events, elapsed time, and user actions keep the current/next Task inside those envelopes and do not invalidate dependencies, preconditions, affect assumptions, hard constraints, or coverage.

Conflict severity uses one Phase 1b vocabulary for mobile stewardship and projection conflicts: `advisory`, `minor`, `major`, and `blocking`. Advisory conflicts can be recorded as stale/degraded metadata; minor conflicts may be silently repaired inside envelopes; major conflicts require user choice or confirmation while preserving next-action explanation; blocking conflicts mean mobile cannot identify a legitimate current or next Task without clarification or higher-fidelity planning.

Cached explanation fragments are short, non-blaming strings with lineage refs for `why_now`, `matters_because`, `depends_on`, `supports`, `legitimacy_reason`, `if_skipped`, `repair_summary`, and `staleness_summary`. They should carry enough provenance to explain the current recommendation, repair, or clarification without copying private detail into more-disclosive replicas.

MVP repair recipes cover `late_task`, `skipped_task`, `fatigue_report`, `approaching_static_task`, and `missing_prerequisite`. They may condition on accepted local evidence and adjust only flexible/disposable work inside recorded envelopes, preserve Static Tasks and protected support, use next-best-action mode when no safe default exists, and produce diagnostics or clarification Tasks when required starting state is missing. They must not invent canonical state, Resources, Skills, Techniques, external facts, or user commitments.

The last legitimate Plan is stored by digest and replay cursors: Plan id/digest, generated/effective time, horizon, execution profile, legitimacy and hard-validation refs, default segment digest, UniverseState digest, Log/Snapshot/External Event cursors, Compact Calendar digest, protected Task refs, decision-envelope digest, and explanation-lineage digest. Mobile asks the user for protected changes, meaningful omissions, missing state, or major/blocking conflicts; silently repairs only explicit minor cases; and waits for desktop/cloud refinement when the issue is advisory, outside the reactive horizon, low-coverage but not current-action-critical, or requires global optimization.

---
```

#### UBU-D0275

```markdown
## UBU-D0275: Phase 1b runs through the switch, the planner precedes sync, and later phases are renumbered

**Status:** Accepted → DESIGN.md §4, §4.2, §4.7

Phase 1b is the phase in which mainline UbU reproduces the practical features of Quick UbU, the dogfooding user's working personal planner, so that Quick UbU can become legacy software. Its exit criterion is the switch: the dogfooding user's primary daily planning runs on mainline UbU. Until the switch, Quick UbU remains the primary tool and mainline is exercised against non-primary test data.

The switch requires planning that places routine and decomposed work better than the greedy baseline, so the planner belongs to Phase 1b. In addition to the feature ports, Phase 1b therefore includes decomposition into Containers (§9.4) whose ordered child Tasks are grouped into segments that stay together; an allowed time range on Tasks; Task value and priority inputs to Plan scoring; routines as evergreen-Objective recurrence that instantiates Tasks; partial placement when not every Dynamic Task fits the planning horizon; and the desktop GPU backend of §16.10, which was already the Phase 1 performance target and is delivered in Phase 1b together with its CPU reference path. Calendar-style recurrence (§7.4.1) moves to Phase 1b with this decision. The Phase 1b slice of recurrence and Task synthesis is carved out of `UBU-Q0125` when the Phase 1b planner questions are recorded.

The planner precedes multi-device sync, which remains Phase 2. Neither constrains the other. The planning engine is a pure function over `PlanningRequest` and `PlanningResponse` (§16.10.1) and never sees replicas, Devices, mutation envelopes, or Compartments; sync carries canonical state and the Plan artifacts already specified for Phase 1. Sync is not needed for the switch, because the dogfooding configuration is one desktop Device with Google Calendar as a projection surface, which is not a Device (`UBU-D0257`). Building the planner first shortens the path to the switch, starts full dogfooding of mainline sooner, and returns time to the user through more efficient daily planning.

Later phases are renumbered to remove lettered subdivisions:

| Label before `UBU-D0275` | Label after |
|---|---|
| Phase 3A | Phase 3 |
| Phase 3 | Phase 3 |
| Phase 3B | Phase 4 |
| Phase 4+ | Phase 5+ |

Phase 0, Phase 1, Phase 1b, Phase 2, Phase 3+, and Post-MVP keep their labels and meanings. Earlier tickets, commits, and external documents that use the old labels are read through this map.

A phase label names the phase that implements an item. Pulling an individual item forward, as the Phase 1b rule does, re-tags that item to the implementing phase and never renumbers phases. Build order follows the phase table in §4.7.

Consequences:

- `DESIGN.md` §4 gains a Phase 1b section (§4.2) and a phase map (§4.7); Phase 2 and Phase 3 move to §4.3 and §4.4, and Phase 4 and Phase 5+ receive §4.5 and §4.6.
- The `model-committee` phase vocabulary adds `Phase 4` and `Phase 5` so that Open Questions can carry the new labels.
- Phase 2 sync scope, the Device Sync and Compartment contract, and all Phase 1 and Phase 1b decisions are unchanged.

---
```

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

#### DESIGN.md §4 — Release Phases

```markdown
## 4. Release Phases

Phase labels name the phase that implements an item. The phases, their build order, and the mapping from labels used before `UBU-D0275` are recorded in §4.7.

### 4.1 Phase 1: Single-user GitHub dogfooding

A single user uses UbU to coordinate development of UbU itself.

Primary goals:

- import or observe GitHub issues, PRs, reviews, CI events, and milestones;
- represent them in UbU’s Objective/Task/Calendar model;
- generate useful Plans;
- provide a minimal bootstrap interview that creates an initial dogfooding context, current or stale affect Snapshot, and prioritized Objective/Task seed set;
- provide a next-action focus mode that shows one recommended next Task with an inspectable explanation of why it matters now;
- update GitHub as a low-dimensional projection of UbU state;
- expose limitations and open questions through dogfooding.

The frozen Phase 1 implementation set is:

- local single-user `user_mode` instance for UbU-runs-UbU;
- bootstrap interview and explicit seed model creation;
- GitHub issue, PR, review, CI, milestone, and comment import from live data or approved fixtures;
- ExternalReference-style source links sufficient to trace imported GitHub objects;
- Objective, Preference, Task, Container, UniverseState, Snapshot, Plan, Calendar, Log, Identity, Relationship, Compartment, Automation Worker, External Event, and External Reference schemas only to the depth required by the dogfooding loop;
- schedulable Static and Dynamic Tasks with Objective links, durations, dependency/precondition/effect fields, active/completed/failed/moot lifecycle handling, and moot reason codes sufficient for the demo;
- lightweight UniverseState facts, affect Snapshot handling, accepted mutation vocabulary, and deterministic precondition evaluation;
- append-only per-instance Logs with correction, annotation, provenance, worker-submission, and recalculation-trigger entries;
- Plan and Calendar generation for a next work window with a default Plan and inspectable explanation;
- next-action focus mode with start, done, snooze, reject, decompose, explain-more, and feedback controls;
- derived risk and human-complete plan-quality reports covering deadline risk, dependency fragility, worker or automation bottlenecks, stale affect, affect-margin, destructive pressure, and post-plan depletion warnings;
- minimal Compartment guardrails for `local_only`, `no_cloud_llm`, `no_external_export`, allowed integration/device references, low-security labeling, and logged boundary decisions;
- Automation Worker identity, scoped capability grants, explicit assignment/status display, and mutation or projection request submission without direct canonical writes;
- Delegation Substrate-compatible Task formalization fields where needed for dogfooding, worker assignment, or solo self-reminder clarity, including purpose, executor type, authority, expected output, evidence, review, privacy scope, and escalation notes;
- clearly marked GitHub projection previews or human-approved writes for UbU-managed labels, comments, or blocks, plus reconciliation reporting;
- manually structured Release Outreach Pipeline work items and artifact records for release notes, screenshots, scripts, and contributor calls-to-action;
- manually structured EthConf outreach notes and follow-up artifacts that dogfood Association formation and organizational introspection without requiring full multi-user Association automation.

Phase 2 explicitly defers:

- multi-device local-first sync;
- partial replication across Devices, Zones, or Compartments;
- secure cross-device Compartment propagation;
- sync conflict handling;
- cross-device worker or enclave coordination beyond the single local instance boundary.

Phase 3 is the bridge from the bootstrap MVP toward the full personal life-logistics product. It prioritizes minimal Resource/Skill-aware task readiness, private skill-tree foundations, and narrow Identity coordination that directly improves single-user life logistics. Phase 4 (§4.5) begins the full version 1.0 release track, and Phase 5+ (§4.6) continues it.

Phase 3 explicitly includes:

- minimal Resource objects and Task resource requirements sufficient to answer whether a Task is realistically ready to start or complete;
- minimal Skill objects and Task skill requirements sufficient to answer whether the user can perform a Task, needs a refresher, should learn a prerequisite, or should consider purchase/delegation;
- minimal cross-user contextual messaging between UbU instances;
- user-to-user Identity-mediated requests, status updates, questions, commitments, and blockers;
- bounded Message Context Envelopes carrying priority, interrupt recommendation, topic, response expectation, assumptions, ambiguities, provenance, and disclosure policy;
- legacy-message ingestion and limited upgrade paths where both parties use UbU;
- user-to-user Identity commitments, capabilities, and limited disclosure sufficient for narrow coordination;
- early Delegation Substrate use between Identities, if Phase 2/3 authority and privacy boundaries are in place.

Phase 3 still defers:

- full inventory management, full financial management, bank syncing, investment tracking, receipt OCR, tax categorization, and Quicken-like accounting workflows;
- public Skill Barter marketplace operation, payment/settlement flows, reputation markets, dispute resolution, and marketplace governance;
- shared global project truth between users;
- rich subjective Relationship-model exchange;
- full Association synchronization;
- multi-party governance, invitation, revocation, or trust protocols;
- broad personal-data ingestion beyond selected communication integrations.

Phase 1 keeps these abstractions documented for compatibility but does not implement them:

- Technique as a first-class planning object;
- Resource and Skill as first-class planning objects beyond lightweight compatibility placeholders;
- full Compact Calendar planner grammar and high-coverage transport format;
- complete Zone and Device system beyond the current local execution enclave;
- organization-mode and worker-mode web admin consoles;
- richer relationship-management, personal CRM, and longitudinal affect/growth models;
- full Release Outreach Pipeline video generation, rendering, and publication workflow;
- broad email, text-message, file, invoice, note, or personal-data ingestion outside narrow approved dogfooding fixtures;
- full realtime multimodal capture, always-on assistant behavior, public Skill Barter marketplace operation, payment/settlement flows, reputation markets, dispute resolution, and General Contractor subdelegation workflows;
- adaptive model-committee weighting, automatic patch application, GitHub mutation, and direct cloud-provider APIs.

Stop rule:

> A new abstraction may be added to Phase 1 only when it is required to complete the single-user GitHub dogfooding loop, enforce an accepted hard invariant, or avoid a known irreversible schema contradiction. Otherwise it must be documented as a Phase 2, Phase 3, or post-MVP concern and must not block Phase 1 implementation.

After `UBU-D0175`, new MVP blockers are allowed only when discovered during implementation of a concrete Phase 1 slice and accompanied by a blocker certificate naming the blocked object or file, failed acceptance criterion, unsafe fallback, minimum answer needed, and persistence impact. Phase 1 readiness is judged slice-by-slice rather than by global philosophical completion of the model.

### 4.1.0 Phase 1 readiness scoring

Phase 1 readiness is a derived, evidence-backed, human-reviewed signal. It reports `scope_freeze_readiness` and `mvp_readiness` from per-slice evidence, gates, and score caps rather than from raw open-question count or model confidence. Unresolved nonblocking questions do not reduce Phase 1 readiness after `UBU-D0175` unless they carry a valid blocker certificate. Model-committee may compute readiness reports and propose README readiness text, but it must not publish or update public readiness signals automatically. The accepted rubric is `UBU-D0189`.

The minimum dogfooding loop that proves UbU’s core model is:

1. bootstrap the operator, project context, available work window, constraints, and current or stale affect Snapshot;
2. import or load a curated UbU GitHub fixture with issues, PR/review/CI signals, milestone context, and source links;
3. map those inputs into Objectives, Tasks, External Events, Logs, UniverseState facts, and External References;
4. generate a Calendar with a default Plan for the next work window;
5. present one recommended next Task with an explanation of Objective value, dependencies, deadlines, affect constraints, worker status, and risk findings;
6. record completion, failure, snooze, rejection, decomposition, or override as Log evidence and trigger recalculation when appropriate;
7. preview or perform a human-approved UbU-managed GitHub projection update;
8. show a reconciliation or risk summary that makes the changed model inspectable.

### 4.1.1 Phase 1 public demo criteria

The Phase 1 public demo should use real UbU GitHub issues or a frozen fixture captured from real UbU GitHub data when live access is unsafe, unavailable, or non-reproducible.

The smallest persuasive demo is an end-to-end single-user dogfooding loop:

1. import a curated set of UbU issues, PR/review/CI signals, and milestone context;
2. map those inputs into Objectives, schedulable Tasks, External Events, and traceable source links;
3. generate a Calendar with a default Plan for the next work window;
4. show why the Plan was chosen, including dependencies and worker assignment or worker status;
5. show user-mode affect constraints by applying a current affect Snapshot or by creating an affect-collection Task when affect data is stale or missing;
6. write or preview clearly marked UbU-managed GitHub projection labels, comments, or managed blocks;
7. show a risk summary covering at least deadline risk, dependency fragility, and worker or automation bottlenecks.

The demo must not require Phase 2 sync, Phase 3 multi-user coordination, full RBAC, a complete Compact Calendar UI, or autonomous remote GitHub mutation. If live GitHub writes are unsafe for the public recording, a dry-run projection is acceptable only when it shows the exact payload that would be written after human approval.

### 4.1.2 Phase 1 user-facing loop

Phase 1 must include a minimal first-person UX loop for dogfooding. The loop is intentionally narrow: it collects explicit answers, creates explicit UbU objects, recommends one next Task, records what happened, and recalculates from Logs and current state.

1. **Bootstrap interview**
   - Ask exactly the minimum questions needed to seed the first dogfooding Plan:
     1. `What are you trying to move forward right now?`
     2. `Which project context should UbU use for this first session?`
     3. `How much usable time do you have for the next work window?`
     4. `Is there a deadline, meeting, release target, or external event that changes what matters today?`
     5. `What work should UbU consider first?`
     6. `What is already blocked, unavailable, or not worth recommending right now?`
     7. `How are your energy, stress, and mood right now?`
     8. `When choosing between useful work, what should UbU favor today?`
   - Record the project/context answer as UniverseState facts and source-linked Logs.
   - Record available time, static commitments, deadlines, and immediate constraints as UniverseState facts, Tasks, External Events, or Logs.
   - Record affect answers as a user-declared Snapshot, or create an affect-collection Task when the user skips or the data is stale.
   - Record favored tradeoffs as Preferences only when the user accepts an explicit Preference statement; otherwise retain them as Log notes or Objective annotations.
   - Seed a small Objective/Task set from user answers plus approved GitHub fixtures or configured imports.

2. **Minimum data for one recommendation**
   - A user or operator Identity.
   - At least one active Objective.
   - At least one active schedulable Task linked to that Objective with title and duration or work-window estimate.
   - A current work window or next available time.
   - Current or explicitly stale affect state.
   - Known hard blockers, deadlines, dependencies, and source references for the recommended Task when available.
   - If this minimum is missing, the next recommended Task should be a clarification, import, or affect-collection Task rather than pretending to optimize.

3. **Plan generation**
   - Generate a candidate default Plan from explicit modeled state.
   - Preserve inspectability of the full Plan.
   - Explain relevant Objective value, constraints, dependencies, deadlines, risk findings, worker status, and affect constraints.
   - Mark any fixture-backed, hardcoded, or mock recommendation path directly in the UI and run artifact.

4. **Calendar preview**
   - Treat Calendar preview as a notable recurring Task.
   - Default cadence is before the first recommended Task in a user-declared work window and after a material Calendar regeneration when UbU is about to rely on the new default Plan.
   - Let the user inspect the candidate Calendar or default Plan before relying on it.
   - Ask whether the Plan feels plausible, motivating, humane, and consistent with the user's current context.
   - Ask only lightweight model-repair questions about expected execution, perceived control, social pressure, and missing context; store raw psychological comments as review notes unless the user accepts a concrete canonical update.
   - Allow the user to correct Preferences, Objectives, Tasks, affect Snapshots, availability, or estimates before execution.
   - Let the user snooze, skip, or adjust Calendar-preview cadence through the cadence rule on its evergreen system Objective (§7.4.1).

5. **Next-action focus mode**
   - Present one recommended next Task as the default UI surface.
   - Show title, Objective link, estimated duration or work window, current status, and source/provenance label.
   - Show `why this matters now` with at least the served Objective, timing reason, main dependency or blocker status, affect/staleness status, and top risk or opportunity considered.
   - Show what UbU considered, including visible counts or links for candidate Tasks, blocked Tasks, hard constraints, affect constraints, worker/projection state, and imported or fixture sources.
   - Provide controls for start, done, snooze, reject, decompose, override, and explain more.
   - Keep full Plan inspection one action away through a Plan drawer, timeline, or inspector that shows ordered Tasks, assumptions, source links, and explanation fragments.
   - One-task focus reduces cognitive load; it must not hide the Plan, turn the recommendation into opaque automation, or imply that the user has committed to the default Plan.

6. **Feedback, Log review, and recalculation**
   - After `done`, ask whether completion matched the estimate, whether the modeled effect happened, and whether energy or stress changed enough to update the Snapshot.
   - After failure or blocked execution, ask what assumption failed: missing dependency, wrong duration, stale affect, interruption, unclear Task, changed priority, or other.
   - After snooze, ask for the next review time or condition and whether the reason is timing, affect, dependency, or user preference.
   - After rejection, ask whether the Task is wrong, not valuable now, blocked, too large, already done, duplicate, or should become moot.
   - After override, record the chosen Task or action, the reason if supplied, and whether the override should update Preferences, availability, Task estimates, Objective status, or only this Plan.
   - Treat Log review as a notable recurring Task. Default cadence is at the end of a work window or at the next startup when there are unreconciled deviations, with a weekly catch-up if routine review is skipped.
   - Store reviewed outcomes as canonical updates only when they change Logs, Snapshots, Preferences, Objectives, Tasks, or recalculation triggers; otherwise keep autonomy, competence, relatedness, attitude, subjective-norm, perceived-control, and expected-execution comments as noncanonical review notes or derived report inputs.
   - Record outcomes as Logs and trigger recalculation when the outcome changes Task status, availability, affect state, dependencies, Objective status, or user Preferences.
   - Treat failure, rejection, override, and deviation as model evidence, not user blame.

7. **Discovery mode when appropriate**
   - Allow the user to choose discovery mode at any time.
   - Show active, paused, ended, and pending-review state, enabled sources, routing or retention limits, and evidence count.
   - Provide pause, resume, exit, inspect, reject, delete, and review controls.
   - Collect only enabled Phase 1 evidence such as quick notes, user-selected current action, Calendar context, configured integration events, coarse motion or location category, focus state, and app-state category.
   - Treat inferred observations as reviewable evidence, not final truth; defer ambiguous interpretation until Log review or user-visible reconciliation.

The Phase 1 version may be simple and fixture-backed. Fixture behavior must be labeled as fixture behavior, mock recommendations must not be described as implemented planning, and public demos should show the exact boundary between explicit UbU objects, approved fixtures, and implemented planner behavior. The public demonstration pattern is bootstrap, one recommended next Task, explanation, user feedback, recalculation, and full-Plan inspection. It does not require broad email, text-message, file, invoice, note, or personal-data ingestion. It must not claim complete life-modeling, therapeutic authority, autonomous life coaching, complete planning automation, full privacy isolation, Phase 2 sync, or Phase 3 multi-user coordination. The purpose is to demonstrate the core UbU experience: one meaningful next action, with an explanation, grounded in explicit state.

### 4.1.3 Release Outreach Pipeline dogfooding

The Release Outreach Pipeline should become part of ordinary UbU-runs-UbU release management. A minor release should produce a release outreach package when the current project state contains enough user-visible, developer-visible, or contributor-visible change to justify public explanation.

The Phase 1 package is manual but structured. Minimum package artifacts are:

- `manifest.json` with release, source, Identity, Compartment, status, and hash metadata;
- `claim_register.json` with audience, support status, evidence refs, and review state for each public claim;
- `evidence_index.json` for source artifacts, hashes, selectors, and provenance;
- release-note, script, narration/caption, announcement, and publication-metadata drafts when relevant;
- `media_refs.json` for approved screenshots, UI-test exports, fixture captures, or recordings;
- `export_review.json`, `approvals.json`, and `publication_plan.json`;
- known-limitations, future-work, and contributor call-to-action notes tied to real artifacts.

Media provenance records must distinguish approved live captures, approved fixture/synthetic captures, UI-test exports, and mock/demo-only captures. Every claim in scripts, release notes, and public posts links to claim-register evidence and uses the support labels accepted in `UBU-D0180`.

Phase 1 dogfooding may generate text drafts, collect approved screenshots or recordings, assemble a publication plan, and record human approvals. It may use local renderers or LLM/script workers only as Automation Workers that produce candidate artifacts under capability grants.

Post-MVP work includes automatic UI capture generation, demo-flow export, script drafting from repository state, narration and caption generation, thumbnail generation, video rendering automation, platform upload, social posting, mailing-list publication, analytics feedback, and trusted auto-publication policies.

The full video-generation and publication pipeline remains future work. Phase 1 should generate reviewable communication artifacts before trying to automate external publication.

### 4.1.4 EthConf outreach as Association-introspection dogfooding

EthConf outreach is a Phase 1 dogfooding workflow for Association formation and organizational introspection.

Phase 1 represents the workflow with ordinary Objectives, Tasks, Logs, External Events, External References, Release Outreach package artifacts, public dogfooding review packages, and manually reviewed candidate AssociationAttestations. It does not require full Association automation, multi-user Association sync, automatic contact analysis, or broad message/archive ingestion.

Minimum evidence set:

- canonical design files and accepted decisions;
- public dogfooding artifacts such as model-committee review packages, selected patches, issue fixtures, release outreach packages, and public retrospectives;
- structured EthConf outreach notes with private details separated from redacted public summaries;
- follow-up Tasks, missed follow-ups, public replies, contributor/funder/design-partner next actions, and outcome Logs.

Minimum outputs:

- candidate AssociationAttestations about declared project mission, outreach commitments, observed follow-through, and evidence gaps;
- mission-alignment questions, especially whether outreach behavior supports the privacy-first self-governance, contributor-recruitment, workflow-discovery, and compatible-funding goals;
- priority-drift observations and commitment-mismatch notes;
- follow-up Tasks with owners, review dates, evidence refs, and public/private disclosure labels;
- redacted retrospective notes suitable for public dogfooding artifacts.

The core review prompt is: `Does UbU's actual outreach behavior prove, from reviewable evidence, that the project pursued its stated goals?` The answer may be yes, no, mixed, or insufficient evidence, and must cite evidence or gaps rather than model confidence alone.

Public artifacts expose only redacted summaries, evidence selectors, hashes, public links, reviewed claims, and approved follow-up Tasks. They must not expose private notes, raw contact details, unapproved conversation content, private funding terms, private relationship hypotheses, or Compartment-protected payloads.

Acceptance criteria: the workflow produces one evidence-backed retrospective, at least one candidate AssociationAttestation or explicit no-evidence finding, at least one mission-alignment question, any needed follow-up Tasks, an export review, and a public-safe redacted artifact that labels fixture/manual/LLM-assisted content. A human reviewer must be able to trace each public claim to evidence or to a recorded evidence gap.

### 4.1.5 Optional VoxPopuli EthConf demo

UbU may optionally demonstrate a **VoxPopuli** flow at EthConf if it does not displace higher-priority dogfooding, contributor, or funder deliverables. In this flow, a user speaks freely about what they wish would happen or what feels disorganized. An LLM-assisted extractor converts that natural-language input into candidate Objectives, Tasks, constraints, preferences, and planning assumptions for the user to inspect, correct, accept, or reject.

VoxPopuli is a trust-building outreach hook, not a replacement for the narrow Phase 1 bootstrap interview. It should demonstrate how LLMs can help structure abstract human concerns while UbU remains the explicit, inspectable planning system.

### 4.1.6 Phase 1 implementation priority order

Phase 1 implementation proceeds in this priority order:

1. Remove all in-memory orchestrator state (`MemoryState`) and fully implement `ubu_store` as the canonical local admission and persistence path for the running loop.
2. Log event vocabulary: correction, annotation, provenance, worker-submission, and recalculation-trigger entries.
3. UniverseState facts.
4. Affect Snapshot content, accepted mutation vocabulary, and deterministic precondition evaluation.
5. Structured bootstrap interview with seed Objective, Task, and Snapshot creation.
6. Plan and Calendar generation for a next work window with a default Plan and inspectable explanation.
7. Activate `ubu_github_adapter` as a functioning orchestrator dependency for import, projection preview, approved writes, and reconciliation.

The remaining §4.1 frozen-set items follow in bootstrap-dependency-driven ordering: persistence and vocabulary precede the features that must reference them.

**Phase 1 realization (`UBU-D0241`).** Step 3 (UniverseState facts) is foundationally realized, together with the accepted mutation vocabulary and deterministic precondition evaluation named in step 4, implemented as the §11 container and pure `ubu-core` semantics and persisted by `ubu-store`. The precondition, effect, and bootstrap-fact wiring of these facts into the loop is realized across Wiring-A/B/C (`UBU-D0242`): preconditions gate planning, a completed Task's effects mutate the facts, and the bootstrap records the initial facts under the `UBU-D0243` namespace convention. Step 4's affect-Snapshot content and step 5's interactive bootstrap interview remain.

### 4.2 Phase 1b: Quick UbU merge through the switch

Phase 1b extends the Phase 1 system until it can replace Quick UbU, the separately developed personal planner that serves as the dogfooding user's working daily tool, so that Quick UbU becomes legacy software (`UBU-D0275`).

Exit criterion:

> The switch: the dogfooding user's primary daily planning runs on mainline UbU.

Until the switch, Quick UbU remains the primary tool and mainline is exercised against non-primary test data.

**The Phase 1b rule.** Every Quick UbU feature merged into mainline adopts the restrictions, forms, and functional boundaries of the MVP design it corresponds to, even when that design's implementation phase is later. Open questions that block Phase 1b are answered during Phase 1b whatever phase they were previously tagged, and the items they pull forward are re-tagged Phase 1b.

Phase 1b includes:

- the Phase 1b foundations already decided, including sync-ready mutation envelopes, the Device registry, tombstones, redacted Handles, projection-conflict repair, Device-local credentials, the local advisory worker, advisory batches, and first-class advisory candidate review;
- the Quick UbU practical features, ported under the Phase 1b rule;
- the planner the switch requires: decomposition into Containers whose ordered child Tasks are grouped into segments that stay together (§9.4); an allowed time range on Tasks; Task value and priority inputs to Plan scoring; routines as evergreen-Objective recurrence that instantiates Tasks (§7.4.1); partial placement when not every Dynamic Task fits the planning horizon; and the desktop GPU backend with its CPU reference path (§16.10).

Phase 1b defers multi-device sync and everything else assigned to Phase 2 and later.

### 4.3 Phase 2: Single-user multi-device synchronization

A single user runs UbU across multiple Devices / execution enclaves.

Phase 2 validates:

- local-first sync;
- partial replication;
- Zone and Compartment boundaries;
- device roles;
- conflict handling;
- user-owned worker execution on the user's laptop, desktop, or other trusted personal compute device.

Design rubric:

> Phase 2 should feel like multi-agent coordination, but all agents happen to agree.

A Phase 2 personal worker is not a mandatory cloud dependency. It is an optional execution backend for a user's own UbU instance. It may provide more CPU, GPU, memory, storage, network availability, or battery-independent runtime than the mobile device while preserving the user's practical control over the compute environment.

### 4.4 Phase 3: Minimal multi-user / Identity coordination

Multiple humans coordinate through explicit Identities, capabilities, limited disclosure, and commitments.

Phase 3 should enable coordination without surveillance or shared global truth.

### 4.5 Phase 4: Version 1.0 release track

Phase 4 begins the full version 1.0 release track. It expands Resource and Skill usability, Technique-database integration, DIY-versus-purchase/hire planning, and the user-facing capability graph without requiring the public Skill Barter marketplace.

### 4.6 Phase 5+: Version 1.0+ release track

Phase 5+ continues the full version 1.0+ track with mature inventory, financial-management extensions, public or federated Skill Barter, reputation/evidence, dispute workflows, and other multi-party marketplace features.

### 4.7 Phase map and build order

| Phase | Scope | Status |
|---|---|---|
| Phase 0 | ETHConf NYC demo | Complete |
| Phase 1 | Single-user GitHub dogfooding (§4.1) | Feature-complete |
| Phase 1b | Quick UbU merge through the switch, including the planner (§4.2) | In progress |
| Phase 2 | Single-user multi-device synchronization (§4.3) | Next |
| Phase 3 | Minimal multi-user / Identity coordination and Resource/Skill task readiness (§4.4) | Planned |
| Phase 4 | Version 1.0 release track (§4.5) | Planned |
| Phase 5+ | Version 1.0+ release track (§4.6) | Planned |

Phases are built in table order. A phase label names the phase that implements an item; pulling an item forward re-tags that item and never renumbers phases (`UBU-D0275`). Labels used before `UBU-D0275` map as follows: Phase 3A becomes Phase 3, Phase 3B becomes Phase 4, and Phase 4+ becomes Phase 5+.

---
```

#### DESIGN.md §7.4.1 — Calendar-style recurrence with exceptions (Phase 1b)

```markdown
#### 7.4.1 Calendar-style recurrence with exceptions (Phase 1b)

The MVP `maintenance_time_decay` field cannot express scheduled recurrence with named exceptions, such as "Mondays at 09:00, except company holidays, when it moves to the next day at 08:00." Phase 1b extends evergreen recurrence with a calendar-style schedule modeled on the RFC 5545 (iCalendar) base-rule-plus-exception pattern that generic calendar event schedules use.

A Phase 1b recurrence schedule carries (`UBU-D0275`):

- a base rule (RRULE-shaped): frequency, interval, by-day/by-month-day/by-hour constraints, time-of-day, duration, and a timezone reference;
- exclusion dates (EXDATE-shaped): occurrences removed from the base rule, e.g. holidays;
- additional dates (RDATE-shaped): occurrences added outside the base rule;
- override entries: occurrences whose time, duration, or other parameters differ from the base rule, e.g. "the holiday occurrence moves to the next day at 08:00";
- an optional enablement window: a start date/time and end date/time outside which the schedule produces no occurrences.

The schedule must be **deterministic**: evaluating it against a given timezone and exception set yields a fixed occurrence series. This preserves the existing rule (§16.2) that evergreen recurrence is evaluated deterministically before Calendar generation. Stochastic recurrence remains a separate future extension and is out of scope for this schema.

This schema lives on the **evergreen Objective**, not on a separate object. It replaces the eliminated `Task.recurrence` field (see `UBU-D0214`): a recurrence schedule reactivates the Objective, and planning then synthesizes the Tasks that satisfy it (see §15.2.1.1). It is the canonical home for cadence that previously lived on Tasks, including Calendar-preview and Log-review cadence (§4.1.2, §17.8) and relationship-maintenance cadence (the evergreen "keep this relationship warm" Objective).

A recurrence schedule with no resolvable occurrences inside its enablement window, or that references an undefined exception set, is a philosophical-consistency finding surfaced during ordinary review, not a hard logistical block, unless it prevents a required baseline (§15.2.2). See `UBU-D0213`.
```

#### DESIGN.md §9 — WorkItems, Tasks, and Containers

```markdown
## 9. WorkItems, Tasks, and Containers

### 9.1 WorkItem

A **WorkItem** is the abstraction over concrete work-like entities.

A WorkItem may be:

- Task
- Container
- possible future subtype

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

**Phase 1b split policy (`UBU-D0284`).** A schedulable Task has `split_policy`, defaulting to `atomic`. A splittable Task declares `splittable { min_piece_seconds, resume_overhead_seconds, max_pieces }`; all three fields are admitted Task inputs rather than silent category defaults. Advisors may propose defaults for review, but admission records the explicit policy. Pieces are Plan placements of the same Task, not child WorkItems, and actual partial execution is recorded as Log evidence plus a remaining-work estimate on that Task rather than a Container replacement.

### 9.3 MVP Task schedulability invariant

A Task is schedulable in MVP if it has:

- stable ID
- Objective link
- duration or duration PDF
- active status
- title

Permitted:

- dependencies may be empty
- earliest-start may be absent
- due may be absent
- cost may be absent
- effect may be absent in exceptional cases

Tasks with no known modeled effect may be reportable as low-utility or “meaningless” for plan scoring.

### 9.3.1 Evergreen gap-fillers

Phase 1 does not add an evergreen Task subtype. An evergreen gap-filler is an ordinary active Dynamic Task, usually serving an evergreen Objective, with a `gap_fill_policy` that lets the planner consider it when an otherwise discretionary gap exists.

Required `gap_fill_policy` fields:

- `eligible`: `true`;
- `minimum_useful_duration_seconds`;
- `maximum_useful_duration_seconds`;
- `cooldown_policy`, including an explicit `none_declared` value when no cooldown applies;
- `autonomy_mode`, which is `suggest_only` in Phase 1.

Optional `gap_fill_policy` fields:

- `preferred_duration_seconds` when different from the Task duration model;
- `location_scope`, such as `anywhere`, `current_location`, or a broad user-defined category;
- `required_context_refs`, such as required material, device, app, or integration state;
- `affect_suitability`, when the Task has narrower affect fit than normal Task affect constraints;
- `rank_hint`, for local tie-breaking among otherwise similar candidates.

Existing Task fields carry Objective link, title, status, duration, dependencies, preconditions, effects, and provenance. Readiness should use ordinary preconditions when possible. Recurrence is not a Task field: scheduled recurrence lives on the evergreen Objective (§7.4.1) and planning synthesizes Task instances per reactivation (`UBU-D0214`).

Recovery, meals, sleep, transition buffers, setup/teardown, and other legitimization support work are not gap-fillers by default. They protect human viability before optional gap work is considered.

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

Task split policy applies to individual Tasks, including decomposition child Tasks, but an admitted compiled segment remains an atomic placement unit. If a child in a multi-child segment needs independent splitting, review must create segment boundaries around that child or reject the decomposition until edited; the planner does not split the interior of a compiled segment.

Phase 1b keeps the planning kernel contract unchanged. Before kernel dispatch, the orchestrator compiles each admitted segment into one temporary placement unit whose duration is the exact sum of fixed child durations, or for stochastic durations the conservative component-wise sum of minimum, mode, and p95 with one rollout sample for the segment. After the kernel places the unit, the orchestrator expands it back into child Plan entries in Container order with contiguous offsets. A native no-gap kernel edge is deferred until measurement shows the conservative segment approximation distorts rollout results.

Children with hard static timing, allowed time ranges, preconditions, dependencies, or effects that cannot safely apply to the whole segment force a segment boundary before that child, or after it when the constraint belongs only to prior work. Shared hard constraints may constrain the compiled segment as a whole. If no sequence of boundaries can preserve the child constraint without creating an internal gap or making the proposal unschedulable, admission rejects the decomposition or requires review edits before admission.

When execution finishes a child early or late, completed children stay fixed as history. The unstarted suffix of its segment remains a contiguous placement unit for local repair, starting no earlier than the actual completion time of the prior child. If the suffix no longer satisfies admitted hard constraints, repair replans from the next segment boundary or surfaces a reviewable repair candidate; it does not scatter remaining siblings as independent filler.

Undoing a decomposition is another admitted structural replacement, not deletion. It creates a restored Task with a new Task handle carrying the original intent and prior schedulable fields when still valid, marks the Container and child Tasks as superseded or moot by that restoration, and retains the original moot event, mutation log, child Task history, Plan history, External Reference lineage, and review/admission events for audit.

External References are preserved by linking the external object to the new Container when it represents the larger work and to child Tasks only when the external object supports, evidences, or projects that specific child. The original Task's historical External References are not rewritten; new `supersedes`, `projection_of`, `supports`, or `evidence_for` references may be added according to the accepted External Reference model.

GitHub-linked Task decomposition should keep the GitHub Issue or PR traceable to the Container and add child-level External References only for actionable subwork that needs projection or reconciliation.

Automation/Super Automation expansion uses the same structural-replacement rule; worker-specific child Task details are in §25.1.2.

### 9.5 Moot

`moot` is a first-class terminal Task status.

It is functionally equivalent to completion for planning, but distinct for logs and reporting.

MVP moot reason codes are schema-controlled enum values:

- `externally_satisfied`
- `superseded`
- `delegated`
- `no_longer_relevant`
- `invalidated_by_universe_change`
- `replaced_by_new_plan_structure`
- `user_declared_moot`
- `automation_obsolete`
- `duplicate`

The MVP list is sufficient for Phase 1 and intentionally closed to free-form reason codes. New canonical reason codes require an explicit schema migration or accepted decision. Implementations may store human-readable notes on the `task_moot` Log entry, but the canonical `moot_reason_code` remains one of the enum values.

`duplicate` is included because duplicate work is common in imported GitHub/project queues and should be queryable separately from generic plan supersession.

`delegated` remains separate from `externally_satisfied`. `delegated` means the Task is no longer work for this executor because responsibility moved to another executor, worker, Identity, or delegation path. It does not by itself assert that the underlying Objective or required world state has been satisfied. `externally_satisfied` means the desired state became true through another action or event, regardless of delegation.

Use the narrowest accurate code:

- `externally_satisfied`: the required state is already true because of outside action or observation.
- `superseded`: this Task is replaced by a newer Task, Objective, decision, or source artifact.
- `delegated`: responsibility moved out of this Task's executor scope and will be tracked elsewhere.
- `no_longer_relevant`: the Task is no longer useful, but not because the world invalidated it.
- `invalidated_by_universe_change`: an external state change made the Task impossible or wrong.
- `replaced_by_new_plan_structure`: decomposition, regrouping, or planning restructure replaced this Task without changing the underlying intent.
- `user_declared_moot`: the user explicitly says the Task should be treated as moot and no more specific code is appropriate.
- `automation_obsolete`: a worker, automation path, or generated work item is obsolete because automation state changed.
- `duplicate`: the Task duplicates another active, completed, or canonical work item.

Readiness and execution states such as proposed, ready, blocked, and in-progress are derived views, not canonical Task status (`UBU-D0227`). Candidacy is the store's candidate/admitted distinction; ready and blocked are computed from dependencies and deterministic precondition evaluation; in-progress is `active` plus recorded start evidence in the Log. Derived readiness may appear in API responses, UI, and reports, but it is never persisted as canonical Task status, and `canceled` is not a canonical status — the specific moot reason code is used instead.

**Phase 2 status additions (not in Phase 1):**

`DEFERRED_INTENT` — the user has decided not to plan this Task now but wants to retain it for future consideration. Distinct from low-priority (which remains eligible for planning) and from cancelled (which is terminal). Tasks in `DEFERRED_INTENT` are invisible to the skeleton sampler. The Log review introspection cycle periodically surfaces them for review and structured dismissal. See `UBU-Q0117`.

`WAITING_FOR` — the user is personally waiting on a human response or external action before this Task can proceed. Carries an optional counterparty Identity reference and expected-by date. Distinct from a worker-delegated Task. Surfaces in the planning horizon as a blocked risk rather than a schedulable slot. See `UBU-Q0112`.

---
```

#### DESIGN.md §15.2.1.1 — Objective expansion and technicalizing (Phase 3)

```markdown
### 15.2.1.1 Objective expansion and technicalizing (Phase 3)

Phase 1 receives a pre-materialized `task_graph` (see `PLANNING_KERNEL_CONTRACT.md` §2): a list of `TaskSpec`s, dependency edges, and a CPU-provided topological order. The expansion of an Objective into the work Tasks that satisfy it is, in Phase 1, an unspecified pre-kernel responsibility done by the user or by manual Task entry. Phase 3 specifies it. This is the stage that fills the long-acknowledged gap of Technique-generated Tasks (§15.4), and it supersedes the eliminated `TaskFactory` object (`UBU-D0215`), whose template-expansion role is subsumed by Technique instantiation into a Container (§9, §10.6).

**Where it runs.** Objective expansion runs **pre-kernel, on the CPU side**, before skeletonization. It does not run inside the planning kernel. Running it inside the kernel would break four things the kernel guarantees: the fixed `task_graph` input, the CPU-owned topological order, the absence of Objectives and Techniques from the request schema, and — critically — the kernel's determinism and no-I/O contract, which would be violated the moment Technique selection consulted an advisory LLM. Keeping expansion pre-kernel preserves all four (`UBU-D0216`).

**What it does.** For each reactivated evergreen Objective (and each one-time Objective in scope), expansion selects a Technique and instantiates that Technique's Steps into a Container of `TaskSpec`s with their intra-Technique dependency edges, typed as Static or Dynamic. Static synthesized Tasks then enter skeletonization for affixing and ordering; Dynamic synthesized Tasks carry decision envelopes and are placed during candidate generation (§16.4). A synthesized "take out the garbage tonight or tomorrow morning" Task is a Dynamic Task with a placement window, handled by the same machinery as user-created Dynamic Tasks; it does not need new placement logic.

**Default Technique vs. technicalizing.** Expansion has two modes:

- **Default expansion (deterministic).** The Objective's default Technique instantiates into the baseline Task set. This feeds the deterministic default-Plan path (§15.2.1) and preserves determinism. An evergreen Objective is well-formed for automatic expansion only if it has a default Technique; an Objective with no Technique, or a default Technique that cannot instantiate, is a philosophical-consistency finding surfaced for user correction (`UBU-D0213`), not a hard block, unless it prevents a required baseline.
- **Technicalizing (selection among alternatives).** When alternative Techniques exist for an Objective, expansion may enumerate a **bounded** set of instantiations (default plus alternatives) and hand them to candidate generation as competing candidates. Selection among them is a candidate-generation and value-scoring concern (§16), not a skeletonization concern. Skeletonization continues to do only dependency affixing and ordering; technicalizing does not add a selection layer inside it.

**Honoring the no-invention rule.** Instantiating an already-modeled Technique's Steps into Tasks is not invention and does not violate the §15.2.2 rule against inventing a new canonical Technique to repair skeletonization. Technicalizing selects only among **already-modeled** Techniques, and any advisory LLM may rank only among them. Generating a genuinely novel Technique remains the separate, user-approved Expert-Guided DIY / Technique Commissioning flow (§10.6, `UBU-D0208`), never in-planner synthesis.

**Ordering and termination.** If synthesized Tasks have their own prerequisites, the pipeline is expand → skeletonize → possibly re-expand prerequisites → legitimize, rather than a single pass. For shallow maintenance Objectives (garbage, standup) a single pre-skeleton pass suffices. The schema must not assume shallowness; a bounded expand/skeletonize fixpoint with an explicit iteration cap is the conservative default. Exact fixpoint bounds are an open item (`UBU-Q0125`).
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

**Outcome continuations (`UBU-D0285`).** At a chunk boundary, an outcome state is the equivalence class over completed units, failed or omitted units when they affect reconstruction, splittable progress and remaining work, carried UniverseState/dependency/affect state, modeled external-event or interruption assumptions, and the next chunk cursor. Rollout frequencies under shared latent draws estimate each state's probability with recorded confidence. The sweep ranks boundary outcome states by probability and precomputes CPU-certified next-chunk continuations until `branch_coverage_target` is met inside `reactive_horizon_seconds` or the compute budget is exhausted. Decision alternatives and outcome continuations share that budget: current-chunk certification and the requested K alternatives take priority, then continuation work follows descending outcome probability with recovery-critical and deadline-fragile ties first. Deviations inside a chunk use decision envelopes and repair recipes, not intra-chunk precomputed branches. Interactive streaming may expose outcome continuations incrementally after the first certified chunk, with final coverage reported in the final response.

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

The MVP-friendly subset is Task criticality, last legitimate Plan storage, simple repair rules, conflict severity levels, cached explanations, next-best-action mode, basic decision envelopes, and compact outcome-continuation refs for probable chunk-boundary outcomes. Unbounded rich branch packaging and learned local policy models are post-MVP by default.

**Phase 1b mobile stewardship schema (`UBU-D0256`).** A compact Calendar stores `mobile_stewardship_metadata` for each Task or Plan segment that mobile may recommend, move, omit, or explain. The record is compact and by reference where possible; it contains `criticality`, `decision_envelope`, `conflict_severity`, `explanation_fragments`, `repair_recipe_refs`, optional `outcome_continuation_refs`, and optional `recalculation_triggers` and `remote_assist_eligible`. Unbounded rich branch packaging, learned local policy, and global re-optimization remain outside MVP.

`outcome_continuation_refs` point to certified next-chunk continuations by boundary outcome digest, probability estimate, confidence summary, coverage scope, and stale-after value. Mobile uses them only after observed execution reaches the corresponding chunk boundary; inside the current chunk it still relies on decision envelopes, repair recipes, and escalation when local repair is no longer legitimate.

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

#### DESIGN.md §16.9 — MVP Compact Calendar implementation

```markdown
### 16.9 MVP Compact Calendar implementation

The minimum Phase 1 Compact Calendar implementation stores the legitimized skeleton baseline, the default Plan, the active execution profile, coverage estimate, uncovered-mass estimate, effective coverage threshold, decision envelopes, Task criticality, conflict severity, cached explanation fragments, simple repair recipes, probability provenance, and last-legitimate-Plan record. It stores user-previewed, risk-report, and debug/reproducibility Plans only when those artifacts are actually generated or needed for audit.

High-probability near-term alternatives within the reactive horizon may be stored as materialized branches or as deterministic reconstruction instructions. Non-selected candidate Plans, expensive global repairs, and low-probability branches are reconstructed on demand from the compact grammar, current UniverseState, Logs, Snapshots, External Events, repair recipes, and cached provenance.

MVP planning does not require exhaustive optimal search, cloud compute, GPU execution, exact coverage proof, learned policy models, or a complete transport grammar for every post-MVP uncertainty source. The required implementation is a deterministic skeletonizer, minimum legitimizer, greedy baseline, bounded candidate expansion, hard-constraint validator, Plan probability/provenance record, compact default-Plan package, short-horizon repair cache or reconstruction path, and low-coverage recalculation trigger.
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
  - `branch_coverage_target`: float in `(0, 1]`. The CPU supplies the effective value after Calendar, execution-profile, Device, and global policy resolution. It applies to the modeled probability mass of certified chunk-boundary outcome continuations within `reactive_horizon_seconds`, not to the count of K decision alternatives. Phase 1 default is `0.99`.
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

#### PLANNING_KERNEL_CONTRACT.md §3 — `TaskSpec` duration, allowed-range, and correlation fields

```markdown
## 3. `TaskSpec` duration, allowed-range, and correlation fields

Each `TaskSpec` used by the planning kernel must include either a fixed duration model or a three-point shifted-log-normal duration model.

Each Dynamic `TaskSpec` also carries a single hard `window: { earliest_start, latest_finish }` with RFC 3339 / ISO 8601 UTC timestamps. The CPU side builds it by intersecting the admitted Task's one-off `allowed_time_range`, or the concrete range on an instantiated routine Task, with `PlanningRequest.time_window`. If the intersection is empty or shorter than the Task's minimum possible duration, the Task is unplaceable for that request rather than soft-scored outside the window. Multiple allowed windows are not represented in Phase 1b `TaskSpec`; any future support requires a contract change or CPU-side occurrence/window selection before dispatch.

Each `TaskSpec` also carries a split policy:

```text
{ type: "atomic" }
```

or:

```text
{
  type: "splittable",
  min_piece_seconds: positive_integer,
  resume_overhead_seconds: non_negative_integer,
  max_pieces: integer >= 2
}
```

`atomic` is the default admitted Task policy and requires one placement. A splittable Task may produce multiple Plan placements for the same Task, but no more than one piece per chunk, no more than `max_pieces` total pieces, and no piece outside the Task's `window`. Splitting within one chunk is invalid.

For a splittable Task, the kernel samples one total work duration from the Task's duration model and spends that work across pieces. The first piece has no resume overhead. Each later piece consumes `resume_overhead_seconds` of occupied time before additional sampled work is performed. Incoming dependencies and preconditions gate the first piece; outgoing dependencies and effects are satisfied only by the final piece. Unsupported execution profiles must reject splittable TaskSpecs or mark them unplaceable with an explicit diagnostic rather than silently treating them as atomic.

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
- When editing `OPEN_QUESTIONS.md`, anchor hunks with the selected question heading `## UBU-Q0154: Phase 1b routines as evergreen-Objective recurrence` and its own `### Resolution` section. Do not use a repeated `### Resolution` heading from an earlier or later question as the edit location.
- If resolving the selected question, replace the `Unresolved.` text under that selected question's `### Resolution` section and update that same question's metadata line. Do not insert selected-question resolution text into any other question block.
- Use the existing `UBU-Qxxxx` and `UBU-Dxxxx` numbering conventions.
- Prefer minimal, auditable changesets.
- If the selected question is blocked, propose decomposition only if it produces replacement questions with fewer, simpler, or no dependencies.
- If the selected question is already partially resolved, narrow or clarify it rather than pretending it is fully unresolved.

Return only JSON.
