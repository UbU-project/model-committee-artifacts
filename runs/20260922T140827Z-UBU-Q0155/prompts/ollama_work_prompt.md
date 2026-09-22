# Model-Committee Work Proposal Request

You are participating in the UbU `model-committee` process.

Your task is to produce one concrete work proposal as strict JSON.

Do not return prose outside the JSON object.

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

- next question id: `UBU-Q0160`
- next decision id: `UBU-D0289`

`DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md` is read-only context and is not in the patch
allowlist: do not propose changes to it.

### OPEN_QUESTIONS.md — related questions

#### UBU-Q0153

```markdown
## UBU-Q0153: Task value and priority for Plan scoring

Status: Solved Priority: MVP blocker Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0155, Phase 1b planner quality Resolved by: UBU-D0277 Last scored: 2026-09-21 Scored from commit: None

Defining context: PLANNING_KERNEL_CONTRACT.md §5, DESIGN.md §16.10, `UBU-D0275`.

### Question

Where do Task value and priority come from for Plan scoring? Today the orchestrator builds every `TaskSpec` with `value: 1.0` and `priority: 1.0`, so the value-scoring stage cannot distinguish one Task from another.

### Subquestions

1. **Source.** Which input sets a Task's value: Objective value, explicit per-Task priority, deadlines, or Quick UbU's review-and-prioritize ranking?
2. **Scale.** How does an ordinal ranking become a bounded numeric value, and is it normalized per request? Utility is transient and never persisted. Which value inputs are canonical, and which are computed per request?
3. **Routine value.** How does a routine instance's value relate to a one-off Task's? Does it rise as a deadline or a missed occurrence approaches?
4. **Scoring policy.** How does Task value interact with the affect and robustness weights in `scoring_policy`?
5. **Revealed preference.** Which part, if any, of learning trade-off weights from review decisions belongs in Phase 1b? The rest stays with `UBU-Q0125`.

### Current direction

Phase 1b takes Task value from explicit operator prioritization: Quick UbU's prioritize flow is ported as admitted records, and the orchestrator maps them onto a bounded numeric scale when it builds each request. The computed values are never persisted as utility. Learning weights from revealed preference stays with `UBU-Q0125`.

### Resolution

Resolved by `UBU-D0277`: Phase 1b uses admitted explicit operator prioritization as the canonical source of Task priority, including the ported Quick UbU review-and-prioritize flow. The CPU kernel converts ordinal priority buckets among the request's eligible schedulable Tasks into transient `TaskSpec.value` and `TaskSpec.priority` values in `[0.1, 1.0]`; ties share values, a single ranked bucket maps to `1.0`, and unranked eligible Tasks use `0.1`. Objective labels, deadlines, routine cadence, and missed occurrences may affect eligibility, risk, urgency diagnostics, review prompts, or later policy work, but they do not automatically become persisted utility. Routine instances inherit explicit occurrence priority when present, then routine-template or evergreen-Objective priority. Stage 3 combines value-weighted utility with robustness, affect-margin, and schedule-diversity only through `scoring_policy`; affect filtering and CPU hard-constraint certification remain gates. Phase 1b records review decisions as evidence but leaves revealed-preference learning and adaptive weights to `UBU-Q0125`.

---
```

### DECISIONS.md — cited decisions

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

#### DESIGN.md §15.2.2 — Skeleton Plans and legitimization

```markdown
### 15.2.2 Skeleton Plans and legitimization

The planning pipeline begins with a **skeleton Plan**. A skeleton Plan affixes all Static Tasks and recursively walks backward through dependency DAGs from terminal Static Tasks to dependency roots, then schedules root prerequisites before their dependents. The skeleton Plan is a causal/dependency foundation, not a complete optimized user Plan.

A dependency is a requirement that some state be true before a dependent Task begins. During skeletonization, UbU must check whether that state is already satisfied by the initial UniverseState before inserting prerequisite Tasks. If a viable skeleton Plan cannot be created, the planning model is blocked, contradictory, incomplete, or impossible enough that UbU should stop ordinary planning and ask the user for clarification with a concrete diagnostic explanation.

After skeletonization, UbU performs **legitimization**. Legitimization adds the minimum human-viability constraints and support Tasks needed to make the skeleton Plan plausibly executable by the user. Examples include affect constraints, breaks, recovery Tasks, meals, sleep, transition buffers, setup/teardown time, context-switch limits, and basic sustainability requirements.

The legitimized skeleton Plan is the baseline feasible Plan. Phase 1 treats full legitimization as a finalist oracle: it runs for the baseline and for the small finalist set selected after cheap pruning, not inside every high-fan-out candidate-construction step unless measurements show it is cheap enough. Semi-legitimization is a conservative cheap filter and ranking signal. It may reject obvious legitimacy failures, but uncertain candidates must reach full legitimization before becoming the default Plan.

Minimum skeleton Plan representation records:

- Static Task placements with fixed start and end times;
- the dependency DAG frontier used for the current planning scope;
- prerequisite roots whose required state is not already true in the initial UniverseState;
- ordered prerequisite chains and the dependency or precondition refs that justify them;
- initial UniverseState assumptions, including known true, known false, assumed, and unresolved planner-relevant facts;
- unsatisfied dependency diagnostics with the failing Task, missing state, causal chain, and clarification alternatives when available.

Skeleton failure diagnostics are blocking when no valid skeleton baseline can be produced for the current Calendar scope. A diagnostic records:

- `failure_class`: one of `missing_starting_state`, `impossible_dependency`, `cyclic_dependency`, `static_task_collision`, `insufficient_calendar_window`, `unavailable_resource`, `blocked_external_event`, or `unknown_precondition`;
- severity, affected Task refs, missing or conflicting state, relevant Static Task or time-window refs, initial UniverseState assumption refs, source or External Event refs, and the shortest causal chain needed to explain the failure;
- safe clarification alternatives, each with an action enum, required user input, and the state mutation or Plan repair it would permit.

The user-facing explanation shows the failed Task or state, the immediate cause, and at most three causal-chain steps by default, with full inspector detail available. Wording frames the problem as an incomplete or contradictory model, not user failure.

Safe alternatives are limited to: provide or correct starting state, mark the required state already satisfied, add a prerequisite Task, relax a deadline or Static Task constraint, extend the planning horizon, remove or moot the blocked Task, choose an already-modeled alternate Technique or Task path, wait for or record an External Event, or ask for manual decision. UbU must not invent a new canonical Resource, Skill, Technique, Preference, or external fact merely to repair skeletonization.

A skeleton failure becomes an immediate blocking prompt when it prevents any valid baseline for the current Calendar, current or next recommended Task, a Static Task placement, a hard dependency or precondition, deadline feasibility, or a required Resource or External Event. It may remain a normal planning warning only when a valid skeleton exists and the failed chain is outside the current recommendation path or future horizon, in which case UbU records the diagnostic and marks the relevant Calendar, explanation, or risk report stale.

Full legitimization records the constraints it enforced and any support Tasks or buffers it inserted. Its hard result is `passed`, `failed`, or `needs_clarification`. Graded fields such as `legitimacy_score`, `legitimacy_margin`, and `legitimacy_delta_from_baseline` are advisory comparison signals, not permission to violate hard legitimacy.

In Phase 1, `full_legitimize` implements the **affect-feasibility filter** (`UBU-D0236`). It computes each active dimension's sigmoid satisfaction from the AffectProfile and the affect observation, marks the candidate affect-feasible only when every dimension meets its threshold, and records per-dimension satisfaction, the `violated_dimensions`, and an `affect_margin` that is the legitimacy margin while affect is the only enforced constraint. It runs in `enforce` for user-facing planning and `warn_only` for onboarding, test, and stale or missing affect. Support-task insertion — breaks, recovery, meals, sleep, and transition buffers — and `semi_legitimize` are deferred follow-ons within full legitimization; Phase 1 legitimization is the affect filter only.

The Phase 1 semi-legitimization heuristic set is affect budget, slack preservation, dependency fragility, user-mode compatibility, local repair viability, and legitimacy-delta estimate. Semi-legitimization returns `passes_cheap_checks`, `reject_obvious`, or `needs_full_legitimization`; it is not sufficient to certify a default Plan.

A higher-value brittle Plan may beat a lower-value humane Plan only when it passes full legitimization, preserves minimum slack and repair envelopes, avoids destructive pressure, and does not leave the user in a worse post-plan state. Otherwise the lower-value humane Plan, or the legitimized skeleton baseline, remains the default.

Recuperative support required for legitimacy is not gap-filling. Meals, sleep, rest, breaks, recovery, setup/teardown, transition buffers, affect collection, and checkpoint work inserted or protected by legitimization are ordinary Tasks or buffers with explanation lineage and protected criticality. They are placed before optional Dynamic Tasks and before `gap_fill_policy` suggestions; crowding them out invalidates or degrades the candidate.

After legitimization, the minimum candidate Plan representation includes materialized Task placements, decision envelopes for movable Tasks, Plan probability metadata, value score, semi-legitimization summary when used, full legitimacy result or score, hard-validation status, protected support refs, and explanation lineage back to Objectives, dependencies, preconditions, affect constraints, worker status, risk findings, and probability inputs.
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

#### PLANNING_KERNEL_CONTRACT.md §4 — `PlanningResponse`

```markdown
## 4. `PlanningResponse`

`PlanningResponse` is the planning-engine-to-CPU return object.

### Required fields

- `schema_version`
- `planner_version`
- `request_id`
- `rng_seed_echo`
- `generated_at`
- `status`: enum, one of `ok`, `partial`, `rejected`, `engine_error`.
- `engine_provenance`:
  - `backend_kind`: enum, one of `cpu_reference`, `gpu_worker`.
  - `invocation_kind`: enum, one of `in_process_cpu`, `persistent_python_worker`.
  - `engine_version`: implementation version string.
  - `framework`: optional string, required as `pytorch` for the Phase 1b GPU worker.
  - `framework_version`: optional string.
  - `device_summary`: optional compact local hardware/runtime summary, for example GPU model or `cpu`.
  - `tolerance_profile`: optional name of the numeric tolerance profile used for parity checks.
  - `cpu_certification_status`: enum, one of `not_yet_certified`, `certified`, `rejected_by_cpu`.
- `plan_candidates`: ranked list of `PlanCandidate` objects.
- `diagnostics`:
  - `candidate_counts_by_stage`
  - `n_skeleton_candidates`
  - `n_after_affect_filter`
  - `n_after_value_scoring`
  - `n_finalists_rollout`
  - `rejection_counts_by_reason`
  - `warnings`
  - `skeleton_failure_diagnostics`: list of `SkeletonFailureDiagnostic` objects. Empty when skeletonization and prerequisite validation do not fail; required when `status = rejected` because no skeleton baseline exists.
  - `probability_quality`: enum, one of `full`, `degraded_numeric_jitter`, `degraded_independence`, `not_estimated`.
  - `coverage_scope`: optional enum, one of `reactive_horizon`, `full_window`, `repair_scope`.
  - `coverage_estimate`: optional numeric branch-coverage estimate.
  - `uncovered_mass_estimate`: optional numeric estimate, normally `max(0, 1 - coverage_estimate)` for the same scope.
  - `coverage_threshold_used`: optional numeric threshold used for regeneration decisions.
  - `coverage_below_threshold`: optional boolean.
  - `coverage_confidence`: optional object summarizing probability estimation method, rollout count, confidence level, and lower/upper bounds for the coverage estimate.
  - `outcome_continuation_summary`: optional compact counts and digests for precomputed chunk-boundary outcome states, including covered outcome count, uncovered sampled outcome count, and whether the target was budget-limited.
  - `compute_telemetry`: optional backend timing and resource summary, including `duration_ms` when available.

### `SkeletonFailureDiagnostic`

A skeleton failure diagnostic is the bounded payload for halted or degraded skeleton generation. Minimum fields:

- `diagnostic_id`
- `severity`: enum, one of `blocking`, `warning`.
- `failure_class`: enum, one of `missing_starting_state`, `impossible_dependency`, `cyclic_dependency`, `static_task_collision`, `insufficient_calendar_window`, `unavailable_resource`, `blocked_external_event`, `unknown_precondition`.
- `primary_task_ref`: optional when the failure is Calendar-wide.
- `affected_task_refs`: list of Task refs.
- `missing_or_conflicting_state`: object with `target`, `predicate`, optional `expected`, optional `observed`, and `state_status` enum, one of `missing`, `known_false`, `conflicting`, `unresolved`, `denied_by_policy`.
- `causal_chain`: ordered list of compact fragments with Task ref, dependency or precondition ref, required state, optional producer Task ref, and status.
- `time_window_refs`: list of relevant Calendar or deadline refs.
- `static_task_conflict_refs`: list of Static Task refs for fixed-time conflicts.
- `initial_universe_state_ref`: optional UniverseState or Snapshot ref.
- `source_refs`: list of provenance or External Reference refs.
- `external_event_refs`: list of relevant External Event refs.
- `safe_alternatives`: list of alternatives, each with `action`, `label`, `requires_user_input`, `resulting_change_summary`, and optional candidate mutation or repair ref.
- `prompt_policy`: enum, one of `immediate_blocking_prompt`, `planning_warning`.
- `user_facing_summary`: short non-blaming explanation text.

Allowed `safe_alternatives.action` values are `provide_starting_state`, `mark_state_already_satisfied`, `add_prerequisite_task`, `relax_deadline`, `extend_planning_horizon`, `remove_or_moot_task`, `choose_alternate_technique_or_task_path`, `wait_for_external_event`, and `manual_decision`.

### `PlanCandidate`

Each `PlanCandidate` must carry:

- `candidate_id`
- `rank`
- `candidate_role`: enum, one of `highest_utility`, `most_robust`, `most_schedule_diverse`, `other`.
- `schedule`: ordered list of planned Task placements. Each placement carries:
  - `task_id`
  - `piece_index`: 1-based integer; atomic placements use `1`.
  - `piece_count`: positive integer; atomic placements use `1`.
  - start/end or equivalent occupied interval fields required by the implementation profile.
- `score_summary`:
  - `utility_score`
  - `robustness_score`
  - `affect_margin_score`
  - `schedule_diversity_score`
  - `total_score`
- `feasibility_summary`:
  - `hard_constraints_assumed_satisfied_by_engine`: boolean. This is advisory only.
  - `affect_feasible`: boolean.
  - `minimum_affect_score`.
  - `violated_affect_dimensions`.
- `semi_legitimization_summary`: optional cheap prevalidation summary:
  - `result`: enum, one of `passes_cheap_checks`, `reject_obvious`, `needs_full_legitimization`.
  - `affect_budget_ok`: optional boolean.
  - `slack_preserved`: optional boolean.
  - `dependency_fragility_ok`: optional boolean.
  - `user_mode_compatible`: optional boolean.
  - `local_repair_viable`: optional boolean.
  - `legitimacy_delta_estimate`: optional numeric estimate relative to the legitimized skeleton baseline.
- `probability_summary`:
  - `display_probability`: optional scalar.
  - `log_probability`: optional numeric value for stable computation.
  - `probability_interval`: optional interval.
  - `provenance_refs`: list of stochastic-input references.
- `explanation_fragments`: optional list of user-facing explanation fragments.
- `validation_hints`: optional CPU-consumable hints. These must not be treated as certification.

The CPU kernel must validate any returned candidate before canonical Plan commit. The committed Plan records the response's `engine_provenance` together with the CPU certification result.

### `PlanningStreamFrame`

Interactive delivery is a sequence of worker frames for a single `PlanningRequest`: zero or more chunk results followed by exactly one final response. Batch delivery emits only the final response.

Common frame fields:

- `schema_version`
- `request_id`
- `frame_index`: zero-based monotonically increasing integer within the request.
- `frame_type`: enum, one of `chunk_result`, `final_response`, `engine_error`, `cancelled`.

A `chunk_result` frame carries:

- `chunk_depth`: positive integer sweep depth completed.
- `chunk_id` or deterministic chunk range identifier.
- `partial_response`: a CPU-certifiable partial `PlanningResponse` form containing only candidates and diagnostics whose placements end at or before the completed chunk depth.

Chunk-result partial responses may include outcome-continuation summaries and references for already certified boundary outcomes at or before the completed chunk depth. Later frames may increase coverage; they must not remove a continuation that the user has started executing except through the ordinary post-start pruning and repair rules.

A `final_response` frame carries exactly one complete `PlanningResponse`. `engine_error` and `cancelled` frames are transport outcomes and do not certify a Plan. The CPU may surface a streamed chunk only after CPU certification of that frame's partial response.

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
- `piece_index`: 1-based planned piece index per placement, with `1` for atomic placements.
- `piece_count`: total planned piece count for that Task in the candidate, with `1` for atomic placements.
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
- split-policy parameters;
- time-window and planning-delta parameters;
- candidate seeds derived from `rng_seed`.

Produces:

- candidate schedule tensor or equivalent structured schedule batch;
- `start_time_offsets`;
- `duration_samples`;
- candidate `validity_mask`;
- per-placement `piece_index` and `piece_count` metadata;
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

Phase 1b `TaskSpec.value` is a CPU-computed float in `[0.1, 1.0]` derived from admitted explicit Task prioritization for the request. Given dense priority buckets ordered best to worst, bucket position `p`, and bucket count `m`, value is `1.0` when `m = 1` and otherwise `1.0 - 0.9 * p / (m - 1)`. Tied Tasks share `p`; unranked eligible Tasks use `0.1`. In Phase 1b the CPU sends `TaskSpec.priority` as `1.0`, because Stage 3 utility multiplies value by priority; the normalized priority rank stays in the orchestrator's explanation and diagnostic records (`UBU-D0282`). Mandatory routine occurrences are outside this range: they are not layered and are sent with `TaskSpec.value = 0.0` (`UBU-D0288`).

The canonical persisted input is the set of admitted pairwise Task Preferences, layered into dense buckets at request time (`UBU-D0009`, `UBU-D0282`). The bounded value and normalized priority are transient request metadata and must not be persisted as utility. Routine instances enter Stage 3 as mandatory TaskSpecs after recurrence instantiation: they carry no priority, must be placed, and are never selected for omission (`UBU-D0288`). Objective labels, deadlines, missed occurrences, and cadence do not rewrite value/priority unless admitted prioritization changes the underlying rank.

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

For splittable Tasks, Stage 4 reuses the single sampled total work duration for every piece of that Task in a rollout and accounts for deterministic resume overhead before each resumed piece. A rollout remains feasible when unfinished sampled work can continue in a later planned or repair-eligible piece inside the Task window and `max_pieces`; it is infeasible when remaining work, overhead, fixed placements, dependency state, or the allowed range prevent completion under the candidate's policy. Partial scheduled-work value is proportional to completed sampled work for splittable Tasks, while effects and outgoing dependencies require completion of the last piece.

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
- When editing `OPEN_QUESTIONS.md`, anchor hunks with the selected question heading `## UBU-Q0155: Partial placement when the horizon cannot hold every Dynamic Task` and its own `### Resolution` section. Do not use a repeated `### Resolution` heading from an earlier or later question as the edit location.
- If resolving the selected question, replace the `Unresolved.` text under that selected question's `### Resolution` section and update that same question's metadata line. Do not insert selected-question resolution text into any other question block.
- Use the existing `UBU-Qxxxx` and `UBU-Dxxxx` numbering conventions.
- Prefer minimal, auditable changesets.
- If the selected question is blocked, propose decomposition only if it produces replacement questions with fewer, simpler, or no dependencies.
- If the selected question is already partially resolved, narrow or clarify it rather than pretending it is fully unresolved.

Return only JSON.
