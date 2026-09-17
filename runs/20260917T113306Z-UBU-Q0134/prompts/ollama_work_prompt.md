# Model-Committee Work Proposal Request

You are participating in the UbU `model-committee` process.

Your task is to produce one concrete work proposal as strict JSON.

Do not return prose outside the JSON object.

## Selected question

Question ID: `UBU-Q0134`  
Question title: `Phase 1b projection conflict classes: auto-resolve versus required review`  
Base commit: `904b4c563317ce591c718c80ed14c6aa417d9d4a`

```markdown
## UBU-Q0134: Phase 1b projection conflict classes: auto-resolve versus required review

Status: Open Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0143, UBU-Q0073 Blocks: Phase 1b projection reconciliation, conflict severity vocabulary Resolved by: None Last scored: 2026-09-17 Scored from commit: None

### Question

Which conflict classes arising between canonical state and an external projection may be auto-resolved in Phase 1b, and which must always require operator review?

### Subquestions

1. How is each external-edit quadrant (known object edited, known object deleted, unknown object appeared, expected object absent) classified as auto-resolvable or review-required?
2. Is an external duration edit auto-admissible, and is an external time edit that pins a dynamic object different in kind?
3. Is a confirmed external deletion auto-resolvable as an object removal, or must it always be reviewed?
4. What conflict severity levels does Phase 1b need, and do they share one vocabulary with the mobile-stewardship severities of UBU-Q0073?
5. How is an auto-resolved projection conflict recorded so that it remains auditable as a first-class event rather than a silent mutation?
6. What happens when an external edit conflicts with a still-pending candidate for the same object?

### Current direction

Phase 1b resolves projection conflicts only, and the existing four-quadrant calendar import classification is the behavior to formalize rather than invent. Device-to-Device sync conflict classes — concurrent edits on two replicas, policy change while offline — remain open under UBU-Q0143 and must not be treated as answered by this entry, which is scoped to the canonical-versus-projection family.

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

- next question id: `UBU-Q0151`
- next decision id: `UBU-D0261`

`DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md` is read-only context and is not in the patch
allowlist: do not propose changes to it.

### OPEN_QUESTIONS.md — related questions

#### UBU-Q0014

```markdown
## UBU-Q0014: UniverseState Mutation Schema

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 implementation Resolved by: UBU-D0130 Last scored: Never Scored from commit: None

Resolved. See UBU-D0130.

---
```

#### UBU-Q0015

```markdown
## UBU-Q0015: Task Precondition Schema

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 implementation Resolved by: UBU-D0131 Last scored: Never Scored from commit: None

Resolved. See UBU-D0131.

---
```

#### UBU-Q0016

```markdown
## UBU-Q0016: Compact Calendar planner grammar and execution profile

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0014, UBU-Q0015 Blocks: Phase 1 implementation Resolved by: UBU-D0151 Last scored: Never Scored from commit: None

Resolved. See UBU-D0151.

---
```

#### UBU-Q0058

```markdown
## UBU-Q0058: Adaptive planning granularity and offline precomputation policy

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0016 Blocks: Mobile-only planning, offline mode, low-power mode, Compact Calendar runtime policy Resolved by: UBU-D0203 Last scored: 2026-05-27 Scored from commit: None

Resolved. See UBU-D0203.

---
```

#### UBU-Q0073

```markdown
## UBU-Q0073: Mobile stewardship metadata and MVP repair rules

Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: UBU-Q0016, UBU-Q0058 Blocks: mobile/local planning UX, Phase 1b device-side provisional re-planning Resolved by: UBU-D0256 Last scored: 2026-09-16 Scored from commit: None

### Question

What compact Calendar metadata and local repair rules are required so mobile UbU can preserve legitimacy and next-action clarity without full global optimization?

### Subquestions

1. What is the exact schema for protected, flexible, and disposable Task criticality?
2. What is the MVP schema for decision envelopes?
3. What conflict severity levels are required?
4. What explanation fragments should be cached with each Task or Plan segment?
5. What simple repair recipes are required for late Task, skipped Task, fatigue report, approaching Static Task, and missing prerequisite?
6. How is the last legitimate Plan stored and compared with current reality?
7. When should mobile ask the user, silently repair, or wait for desktop/cloud refinement?

### Current direction

Mobile should be a real-time steward of Plan legitimacy, user agency, and next-action clarity. MVP should include Task criticality, last legitimate Plan storage, simple repair rules, conflict severity, cached explanations, next-best-action mode, and basic decision envelopes.

Phase 1b re-scope: this question is pulled forward from Phase 2 under the Phase 1b rule, which requires every Quick UbU feature merged into mainline to adopt the restrictions, forms, and functional boundaries of the MVP design it corresponds to, even when that design's implementation phase is beyond Phase 1. Phase 1b emits artifacts whose shape depends on this answer, so it is answered now. Phase 1b scope: device-side provisional re-planning over a redacted shareable slice already exists in the Quick UbU implementation and becomes load-bearing again under the Phase 1b rule, so the criticality, decision-envelope, conflict-severity, cached-explanation, and repair-recipe schemas are required now. Conflict severity levels should be reconciled with the projection conflict classes in UBU-Q0134 so that one severity vocabulary serves both.

### Resolution

Resolved by `UBU-D0256`: Mobile stewardship metadata carries Task criticality, decision envelopes, conflict severity, cached explanation fragments, last-legitimate Plan identity, simple repair recipes, next-best-action mode, recalculation triggers, and remote-assist eligibility. Local repair may silently preserve legitimacy only inside recorded envelopes; it asks for protected/major/blocking choices or missing facts, and waits or escalates when global optimization, policy, or external compute is needed.

---
```

#### UBU-Q0143

```markdown
## UBU-Q0143: Auto-resolvable versus review-required conflict classes

Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0134, Phase 2 conflict resolution Resolved by: UBU-D0248 Last scored: 2026-09-16 Scored from commit: None

Formerly UBU-QSYNC-005 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §16, §17.

### Question

Which conflict classes can be auto-resolved safely, and which must always require user review?

### Resolution

Resolved by `UBU-D0248`: automatic handling is permitted only for deterministic, idempotent, non-authority-changing cases. `duplicate_statement` is auto-collapsed; non-overlapping `stale_prior_version` may be field-merged; `derived_state_stale`, `incomplete_sync_session`, and low-risk `projection_conflict` may be contained, retried, discarded, or recalculated without admitting an unsafe mutation.

Human review is required for conflicts that choose between incompatible user intent, visibility authority, policy authority, Device trust, protected Calendar ownership, or third-party truth. That includes incompatible `concurrent_status_change`, overlapping `stale_prior_version`, `compartment_policy_conflict`, `policy_version_conflict`, `payload_visibility_conflict`, review-worthy `calendar_region_conflict`, review-worthy `projection_conflict`, and any `device_revoked_conflict` that is not merely deterministic rejection of still-pending statements.

Review-required cases surface as blocking diagnostics with `manual_decision` safe-alternatives; the user's decision is admitted only through a `conflict_resolution` sync statement.

---
```

### DECISIONS.md — cited decisions

#### UBU-D0130

```markdown
## UBU-D0130: UniverseState mutations use dotted targets and envelope-level provenance

**Status:** Accepted → DESIGN.md §11.3

MVP UniverseState mutation items use dotted string targets. The first target segment names the UniverseState collection: `facts`, `numeric_values`, `set_memberships`, or `event_markers`. Remaining segments form the lightweight namespaced key within that collection. Examples include `facts.github.issue.14.pipeline_state`, `numeric_values.affect.energy`, `set_memberships.github.issue.14.labels`, and `event_markers.relationship.rel_123.interactions`.

A mutation item has required `operation` and `target` fields. `payload` is required for all operations except `clear_fact`. `note` is optional for human-readable context. Mutation items do not carry item-level `confidence`, `source`, or `provenance` in MVP; those belong on the containing Task effect, Snapshot, Log entry, worker mutation request, External Reference, import artifact, or projection envelope.

Payload rules are operation-specific. `set_fact` accepts any JSON-compatible value. `clear_fact` has no payload. `increment_numeric` and `decrement_numeric` require numeric delta payloads. `add_membership` and `remove_membership` require JSON scalar member payloads, usually strings. `append_event_marker` requires a JSON object payload representing the lightweight marker.

Mutation lists are unconditional once the containing Task effect succeeds. Per-item conditions are not part of the MVP mutation schema; conditional behavior belongs in Task preconditions, effect success probability, planner branching, or worker/request validation. Implementations should validate the full mutation list before application and apply valid items in list order.

Mutation targets may include affect UniverseState keys in `user_mode`, subject to Snapshot precedence and user sovereignty rules. Organization-mode and worker-mode validators must reject intrinsic-affect mutations. Mutation targets may include Relationship-relevant UniverseState keys, including relationship interaction markers and maintenance facts, but they must respect Compartment policy, mode rules, provenance/logging envelopes, and user-acceptance requirements for private or inferred relationship claims. Task effects, workers, imports, and LLM-assisted flows must not silently overwrite user-declared private affect or Relationship truths.

---
```

#### UBU-D0131

```markdown
## UBU-D0131: Task preconditions use recursive all_of/any_of predicates

**Status:** Accepted → DESIGN.md §10.1

MVP Task preconditions use a recursive boolean object with `all_of` and `any_of` arrays for simple AND/OR composition. A precondition node may be a group node or a leaf predicate. A group node contains `all_of` or `any_of`, each holding one or more precondition nodes. A leaf predicate contains `target`, `predicate`, and optional `expected`.

Precondition targets use the same dotted UniverseState target convention accepted for mutations. The first segment names the UniverseState collection: `facts`, `numeric_values`, `set_memberships`, or `event_markers`. Remaining segments form the lightweight namespaced key inside that collection.

MVP predicates are `equals`, `member_of`, and `absent`. `equals` compares the target value to a JSON-compatible `expected` value. `member_of` checks whether the JSON scalar `expected` value is present in the target set membership. `absent` checks that the target is not present or has been cleared, and does not use `expected`. Numeric comparisons such as greater-than, less-than, ranges, thresholds, and arithmetic expressions are not in MVP.

Preconditions may reference `event_markers` for deterministic marker presence or equality checks. Preconditions may reference affect-related UniverseState keys in `user_mode`, subject to Snapshot precedence and user sovereignty rules; organization-mode and worker-mode validators must reject intrinsic-affect preconditions. Preconditions may reference Relationship-relevant UniverseState keys, including relationship interaction markers and maintenance facts, but private or inferred relationship claims remain subject to Compartment policy, mode rules, provenance/logging envelopes, and user acceptance where required.

A failed precondition makes the Task blocked for planning and execution. It does not make the Task invalid. `unschedulable` is a derived planner result when an otherwise valid Task cannot be placed in the current Calendar scope while satisfying preconditions and Calendar Logic. `invalid` is reserved for malformed Tasks, malformed precondition schema, forbidden targets, or canonical-state contradictions. Unknown, unavailable, or partially modeled preconditions are treated as absent in MVP unless the user or importer explicitly records a deterministic predicate.

---
```

#### UBU-D0151

```markdown
## UBU-D0151: Compact Calendar MVP grammar uses skeleton, legitimization, bounded candidates, and repair metadata

**Status:** Accepted → DESIGN.md §§15.2.2, 16

See DESIGN.md §§15.2.2, 16.

---
```

#### UBU-D0203

```markdown
## UBU-D0203: Adaptive planning granularity uses explicit execution profiles

**Status:** Accepted → DESIGN.md §16.7

See DESIGN.md §16.7.

---
```

#### UBU-D0248

```markdown
## UBU-D0248: Sync conflict auto-resolution is limited to deterministic non-authority cases

**Status:** Accepted → DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §16, §17. Resolves `UBU-Q0143`.

Phase 1b and Phase 2 distinguish three conflict-handling classes:

1. **Automatically resolvable.** A Device may resolve the condition without prompting the user when the result is deterministic, idempotent, and does not choose between incompatible user intent, visibility authority, policy authority, Device trust, protected Calendar ownership, or third-party truth.
2. **Automatically containable or recalculable.** A Device may quarantine, retry, resume, discard an incomplete import, or trigger recalculation without treating the conflicting mutation as admitted.
3. **Human-review-required.** A Device must surface a blocking diagnostic with an immediate `manual_decision` safe-alternative; only the resulting `conflict_resolution` sync statement can enter admitted state.

Automatically resolvable classes:

- `duplicate_statement`: collapse duplicate idempotency keys or duplicate content to the already-admitted statement result.
- Non-overlapping `stale_prior_version`: admit only when deterministic field-level merge proves the stale statement does not affect a field, invariant, policy input, schedule region, or causal precondition changed by the newer version.

Automatically containable or recalculable classes:

- `derived_state_stale`: reject or defer the derived result and recompute from current Plan, Calendar, risk, and policy state.
- `incomplete_sync_session`: do not mark the session complete; resume, retry, or discard while preserving idempotency and dependency metadata.
- Low-risk `projection_conflict`: perform deterministic projection repair only when the canonical-vs-projection rule for that integration explicitly says the external delta can be imported, ignored, or logged without changing protected canonical intent.

Human-review-required classes:

- `concurrent_status_change` whenever the competing statuses encode incompatible user intent, including complete-vs-reject.
- Overlapping or invariant-affecting `stale_prior_version`.
- `compartment_policy_conflict`.
- `policy_version_conflict`.
- `device_revoked_conflict`, except for deterministic rejection of still-pending statements from the revoked Device before any user recovery flow.
- `calendar_region_conflict` whenever the mutation would alter or override a protected Calendar region; deterministic rejection is allowed only when the contract for that region leaves no admissible override path.
- `payload_visibility_conflict`.
- `projection_conflict` whenever projection repair would choose between canonical user intent and independently changed third-party state.

For auditability, automatic handling must still emit enough local diagnostic and log metadata to explain what was collapsed, recomputed, quarantined, rejected, or admitted. Review-required conflicts are surfaced through the diagnostic/prompt path, not as synchronized Tasks.

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

### DESIGN.md — referenced sections

#### DESIGN.md §10.1 — Preconditions

```markdown
### 10.1 Preconditions

Preconditions are deterministic constraints over UniverseState.

If preconditions are unknown or partially modeled, they are treated as absent in MVP.

MVP preconditions use a recursive object with `all_of` and `any_of` arrays for simple AND/OR composition. A leaf predicate has `target`, `predicate`, and optional `expected` fields. `target` uses the same dotted UniverseState target convention as mutations, beginning with `facts`, `numeric_values`, `set_memberships`, or `event_markers`.

MVP predicates are:

- `equals`, for exact JSON-compatible equality;
- `member_of`, for checking that `expected` is present in a `set_memberships` target;
- `absent`, for checking that a target is not present or has been cleared.

Numeric comparisons are not in MVP, even for `numeric_values` targets. Numeric values may be checked only by equality or absence.

Preconditions may reference event markers, affect-related UniverseState keys in `user_mode`, and Relationship-relevant UniverseState keys when the containing Task is allowed to see those targets. Organization-mode and worker-mode validation must reject intrinsic-affect preconditions. Relationship-relevant and Compartment-protected targets remain subject to Compartment policy, mode rules, and user-acceptance requirements for private or inferred claims.

A failed precondition means the Task is blocked for planning and execution, not invalid. `unschedulable` is a derived planner result when no placement satisfies otherwise valid preconditions and Calendar Logic; `invalid` is reserved for malformed Tasks, malformed precondition schema, forbidden targets, or contradictions in canonical state.

**Phase 1 realization (`UBU-D0241`).** The deterministic evaluator — recursive `all_of`/`any_of`, leaf predicates `equals`/`member_of`/`absent`, unknown or partially modeled targets treated as absent, malformed preconditions surfaced as evaluation errors — is implemented as a pure `ubu-core` function over a UniverseState. Attaching `preconditions` to the Task schema and consuming the evaluator are realized in Wiring-A (`UBU-D0242`); effect application and bootstrap fact-recording remain.

**Phase 1 realization (`UBU-D0242`, Wiring-A).** A Task carries optional `preconditions`, and the orchestrator evaluates them against the current UniverseState via the pure `ubu-core` evaluator during request building, partitioning Tasks: satisfied or absent → eligible, a failed precondition → **blocked** (excluded from planning and surfaced), a malformed precondition → **invalid** (surfaced distinctly). Only eligible Tasks reach the kernel, which is unchanged. This realizes the §10.1 blocked result in the orchestrator; the planner's placement-based `unschedulable` is a separate concern, untouched.
```

#### DESIGN.md §11.3 — Mutation vocabulary

```markdown
### 11.3 Mutation vocabulary

MVP mutations support:

- `set_fact`
- `clear_fact`
- `increment_numeric`
- `decrement_numeric`
- `add_membership`
- `remove_membership`
- `append_event_marker`

MVP mutation items use a small deterministic schema. Required fields are `operation` and `target`; `payload` is required except for `clear_fact`; `note` is optional. Mutation items do not carry item-level confidence, source, or provenance in MVP. Those belong on the containing Task effect, Snapshot, Log entry, worker mutation request, External Reference, or other envelope.

Targets are dotted strings. The first segment names the UniverseState collection being changed: `facts`, `numeric_values`, `set_memberships`, or `event_markers`. Remaining segments form the lightweight namespaced key inside that collection. Example targets include `facts.github.issue.14.pipeline_state`, `numeric_values.affect.energy`, `set_memberships.github.issue.14.labels`, and `event_markers.relationship.rel_123.interactions`. The first post-collection segment is a controlled-vocabulary `<subject>` governed by `UBU-D0243`, the final segment is a snake_case `<predicate>`, and any segments between form an entity/instance path.

Payload rules by operation:

- `set_fact`: any JSON-compatible value;
- `clear_fact`: no payload;
- `increment_numeric` / `decrement_numeric`: numeric delta payload;
- `add_membership` / `remove_membership`: JSON scalar member payload, usually a string;
- `append_event_marker`: JSON object payload describing the lightweight marker.

A Task effect's mutation list is unconditional once that effect succeeds. Conditional behavior belongs in Task preconditions, effect success probability, or planner branching, not in individual mutation items. Implementations should validate the full mutation list before applying it and apply valid items in list order.

Mutation targets may include affect-related UniverseState keys in `user_mode`; organization and worker modes must reject intrinsic-affect mutations. Mutation targets may include Relationship-relevant UniverseState keys, but inferred or worker-generated relationship facts remain subject to Compartment policy, mode rules, provenance/logging envelopes, and user acceptance where required. Task effects should not silently overwrite user-declared private affect or Relationship truths.

**Phase 1 realization (`UBU-D0242`, Wiring-B).** A minimal `InstanceMode` (the §5 modes) and a pure `ubu-core` validation enforce the organization/worker-mode rejection of intrinsic-affect targets for **both** preconditions (§1585) and mutations (this section). A target is intrinsic-affect by namespace — the segment after its collection is `affect` (e.g. `numeric_values.affect.energy`). In `user_mode` such targets are permitted; in organization or worker mode they make the Task `invalid`. The orchestrator passes the instance mode (`user_mode` for the MVP single-user instance), so the rejection is dormant but encoded and tested. The user-declared-private-truths guard above remains deferred (it needs a provenance/acceptance model).

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

#### DESIGN.md §16 — Compact Calendar

```markdown
## 16. Compact Calendar

A compact Calendar is a transport/storage representation of a Calendar.

It is intended to efficiently represent a large set of possible Plans.

Compact Calendar support is considered important for MVP because it enables recursive self-analysis and transport between devices/workers.

### 16.1 Compact Calendar concept

A compact Calendar may include:

- Task set;
- Static Tasks;
- Dynamic Tasks;
- skeleton Plan metadata;
- legitimized skeleton baseline;
- ordering constraints;
- duration distributions;
- Task success probabilities;
- external-event trigger distributions;
- modeled interruption distributions;
- affect-state uncertainty and constraints;
- deterministic expansion grammar;
- probability provenance expressions and correlation-group references;
- expansion threshold(s);
- coverage value;
- default Plan reference or materialized default Plan;
- stored user-previewed, risk-report, or debug Plans when policy requires them;
- short-horizon branch cache;
- decision envelopes;
- protected, flexible, and disposable Task metadata;
- cached explanations and repair recipes;
- execution-mode metadata such as time delta, branch horizon, CPU/GPU resource limits, and privacy-routing limits.

**Phase 1 surfacing (`UBU-D0238`).** The Compact Calendar surfaces the selected finalist's `display_probability` with its Wilson interval, the p10 rollout robustness, and the `probability_quality` (`full`/`degraded_numeric_jitter`/`degraded_independence`/`not_estimated`); non-finalist alternatives are surfaced with `not_estimated`.

**Phase 1 surfacing (`UBU-D0240`).** The Compact Calendar and next-action also surface the risk report — the aggregated `level` and the categorized findings, with blocking findings prominent — and the six `human_complete_plan_quality` signals with their non-blaming `revision_suggestions`.

### 16.2 Coverage

Coverage belongs to the compact serialization, not the abstract Calendar. It is an estimated regeneration metric in Phase 1, not an exact proof.

Coverage represents the modeled probability mass of possible futures covered by the compact representation within the recorded coverage scope, usually the reactive horizon. MVP coverage includes modeled duration uncertainty, Task success/failure uncertainty when success probabilities affect branch reconstruction, and modeled external-event or interruption assumptions. Objective recurrence uncertainty is not included in MVP coverage; evergreen recurrence is evaluated deterministically before Calendar generation or repair unless a later schema adds stochastic recurrence inputs.

A compact Calendar stores `coverage_estimate`, `uncovered_mass_estimate`, `coverage_scope`, `coverage_threshold_used`, `coverage_inputs_summary`, `probability_quality`, and warning or diagnostic refs when inputs are unmodeled, stale, or degraded.

The default Phase 1 regeneration threshold is `0.99` short-horizon branch coverage. The effective policy resolves Calendar-specific override first, then execution-profile or Device policy, then the global default. The effective threshold is stored with the compact Calendar for replayable regeneration decisions.

After time advances, coverage is recalculated by conditioning the compact Calendar on elapsed time and accepted Logs, Snapshots, External Events, and user actions, then discarding expired branch mass and re-estimating the remaining covered mass.

If recalculated coverage falls below the effective threshold, UbU records or batches a `low_compact_calendar_coverage` recalculation trigger and includes the finding in risk reporting. Immediate recalculation is required only when low coverage can affect the current or next recommended Task, hard feasibility, affect legitimacy, or the default Plan; otherwise marking the Calendar stale is sufficient. Low coverage does not directly create a canonical Task or worker assignment.

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

Plan probability is represented internally as probability metadata rather than only as a display scalar. The minimum record contains a display scalar, a log probability for stable computation, an optional probability interval, provenance over the modeled probabilistic inputs, and correlation-group or scenario references when inputs are not independent. Implementations may multiply probabilities only for inputs declared independent. Correlated or unknown relationships must use joint scenarios, shared random variables, correlation groups, or conservative intervals rather than pretending independence.

### 16.3.1 Multi-Technique candidate generation and outcome comparison (Phase 3)

When an Objective can be satisfied by more than one already-modeled Technique (technicalizing, §15.2.1.1), candidate generation may produce competing candidate Plans that differ by which Technique satisfies which Objective. Comparing those candidates by their predicted real-world outcomes, and letting the user choose, is a premier Phase 3 capability and the demand-side driver for the Technique database (`UBU-Q0119`): the engine consumes Technique variety, so every additional Technique with a distinct cost/time/affect/Resource signature widens the achievable outcome frontier and improves personalization.

**Bounded search, not enumeration.** UbU must not enumerate the full cross-product of Techniques across Objectives; that is `k^N` candidates and contradicts the bounded-search commitment (§16.1, MVP planning does not require exhaustive optimal search). The required shape is **branch-and-bound with semi-legitimization as the cheap pruner**: dominated and obviously-infeasible Technique combinations are pruned during construction using `reject_obvious` / `passes_cheap_checks` (§15.2.2), and only a small Pareto-frontier finalist set reaches full legitimization and full scoring. Dominance pruning runs as a bound during search, not as a filter after enumeration.

**Dominance over the full outcome vector.** A candidate dominates another only if it is at least as good on **every** outcome axis, including robustness, affect-margin, dependency fragility, and Plan probability — not only money and time. A candidate that saves money but is more fragile or has lower completion probability is **not** dominated and must not be pruned. Pruning on a partial vector would discard exactly the robust Plans legitimization exists to protect.

**Surface outcomes, not utils.** Utils are transient internal computational artifacts, not canonical user values (§2.2, §8.5). The user-facing comparison must show concrete predicted terminal UniverseState — money expended, time spent, Resources consumed, affect cost, relaxation, and Plan probability — not a util scalar. Money is shown here as a **generic cost outcome only** (e.g. "this Technique consumes $3.95"); analysis of which account is affected, balances, or overdraft thresholds requires a financial model that UbU does not yet specify and is deferred to Phase 3B/4+. UbU must not imply affordability or account impact without that model.

**User choice and revealed preference.** The user's declared or learned trade-off weights parameterize the existing `scoring_policy` weights (`utility_weight`, `robustness_weight`, `affect_margin_weight`, `schedule_diversity_weight`; `PLANNING_KERNEL_CONTRACT.md` §2). Two modes follow: auto-select applies the weights; ask-the-user presents a small, diverse finalist set and lets the user pick. A revealed choice is a **proposal, not a fact** (`UBU-D0217`): it may propose a weight or Preference update surfaced for explicit user acceptance, and must never silently rewrite the trade-off vector. A single choice is a weak signal (one inequality in weight-space) and must be accumulated conservatively. Because trade-offs are affect- and state-conditioned, learned weights are a function of current state, not a fixed global vector.

**Confidence-gated automation.** Eventual automatic selection requires two gates in series: a confidence threshold **and** a user-granted, revocable auto-choice authority, modeled on `Auto-choice eligibility` as a governance gate separate from automation-likelihood (§3.7.1) and on the trusted-auto-publication pattern (§2.6). Confidence alone never authorizes automatic selection. Auto-selected choices remain logged, inspectable, and overridable.

**Cognitive load and cadence.** The finalist comparison uses the same UX surface and roughly the same choice counts as Calendar preview (§4.1.2): present a small curated set along the most salient trade-off axis for the current decision, elicit only when the frontier is genuinely ambiguous or the trade-off estimate is stale, and auto-select via learned/declared weights otherwise. Diverging the comparison choice count or weights from Calendar-preview defaults is a user configuration. Within-noise finalists are presented as a tie with a `sensitivity_summary`, not a manufactured ranking.

**Affect-conditioned introspection.** Repeated choices logged under a recognizable affect or time-pressure context may produce an introspection finding routed through habit-pattern reconciliation (§12.2), e.g. surfacing that the user has repeatedly chosen money over work-time when rushed before leaving, and offering to move the decision earlier (the night before, when less rushed). This finding must surface the observed **correlation** and offer a **context** remedy that changes when and how the decision is made; it must not assert a psychological mechanism, blame the user, or nudge toward the option UbU scores higher. Nudging toward a scored-better option would decide values for the user and violate `UBU-D0217`.

### 16.4 Reactive short-horizon branch layer

The default Plan is not enough for real-time use because reality can diverge from the modeled branch. UbU therefore also needs a lightweight reactive layer, likely BFS-like, policy-based, or repair-recipe-based, that starts at the current instant and maintains a short-horizon cache or reconstruction path for high-probability near-term alternatives.

The reactive layer should cover near-term alternatives involving planned Tasks, duration variation, early completion, late completion, interruptions, relevant external events, affect shifts, user overrides, eligible evergreen gap-fill suggestions, and immediate recalculation triggers. The provisional default horizon is one hour. Mobile-only, low-power, or offline operation may shorten this horizon.

When early completion creates a gap before the next Static Task and no predetermined Dynamic Task can be pulled forward, the reactive layer may compute a small ranked suggestion set from eligible gap-fillers. It should preserve the protected baseline and ask before starting or materializing the suggested Task in Phase 1.

This layer exists to let the user's current Calendar and Log update quickly when something actually happens. It should not attempt to enumerate every possible future to the end of the Calendar scope.

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

The MVP decision envelope is `decision_envelope: { earliest_start, latest_start, latest_finish, movable_windows, min_duration_seconds, target_duration_seconds, dependency_refs, precondition_refs, affected_static_refs, affect_assumption_refs, resource_or_location_refs, repair_budget_seconds, stale_after }`. `movable_windows` is an ordered list of `{ start, end }` intervals in which the Task may begin or occupy time, depending on the Task's duration fields. Empty or absent movable windows mean the Task is fixed or must be handled by hard Calendar Logic rather than local motion.

The shared conflict severity vocabulary is:

- `advisory`: stale or degraded metadata exists, but the current and next recommendation remain legitimate.
- `minor`: a local repair inside recorded envelopes preserves all protected commitments and can be undone or inspected later.
- `major`: the current or next recommendation remains explainable, but the user must choose, confirm, or supply missing information before mobile changes a protected commitment or meaningful tradeoff.
- `blocking`: mobile cannot identify a legitimate current or next Task because a hard constraint, Static Task, precondition, dependency, affect legitimacy requirement, or required starting state is violated or unknown.

This severity vocabulary is also the Phase 1b projection-conflict vocabulary: projection-specific classes may map into these severities, but must not invent a parallel severity ladder without a later decision.

Cached explanation fragments are short, non-blaming strings with lineage refs. MVP fragments are `why_now`, `matters_because`, `depends_on`, `supports`, `legitimacy_reason`, `if_skipped`, `repair_summary`, and `staleness_summary`. Each fragment may reference Task ids, Objective refs, dependency/precondition refs, Static Task refs, risk finding refs, and compact Calendar provenance, but should avoid copying private details into more-disclosive replicas.

A repair recipe record is `repair_recipe: { trigger, applies_to_refs, eligible_when, steps, ask_policy, fallback, explanation_fragment_ref }`. MVP triggers are:

- `late_task`: condition on elapsed time and accepted Logs; keep protected Tasks and Static Tasks fixed, compress or drop only flexible/disposable work inside envelopes, then offer the next valid Dynamic Task or next-best action. Escalate to `major` or `blocking` if a protected latest finish, hard precondition, or Static Task would be violated.
- `skipped_task`: for disposable Tasks, record omission and continue; for flexible Tasks, reschedule within the envelope; for protected Tasks, ask before omission or create a blocking diagnostic if no legitimate alternate exists.
- `fatigue_report`: record the user report as a Log/Snapshot input, protect recovery-critical Tasks, reduce to protected and low-affect eligible work, and surface rest or clarification when affect assumptions no longer support the Plan.
- `approaching_static_task`: preserve the Static Task, insert required transition/setup/teardown if already modeled, stop or park the current Task when necessary, and ask when the remaining time cannot satisfy a protected current Task.
- `missing_prerequisite`: re-evaluate the deterministic precondition/dependency chain; if a modeled prerequisite Task is available, schedule or pull it forward inside its envelope; otherwise show a clarification Task or blocking prompt. Mobile must not invent a satisfied state, Resource, Skill, Technique, or external fact.

The last legitimate Plan record is `last_legitimate_plan: { plan_id, plan_digest, generated_at, effective_time, horizon, execution_profile, legitimacy_result_ref, hard_validation_ref, default_segment_digest, universe_state_digest, log_cursor, snapshot_refs, external_event_cursor, compact_calendar_digest, protected_task_refs, decision_envelope_digest, explanation_lineage_digest }`. Mobile compares current reality to this record by replaying accepted Logs, Snapshots, External Events, elapsed time, and user actions since the recorded cursors, then checking whether current and next Task placements remain inside decision envelopes with dependencies, preconditions, affect assumptions, Static Tasks, and coverage still valid.

Mobile may silently repair only for `minor` conflicts where the recipe is explicit, all changes stay inside decision envelopes, protected Tasks and support Tasks are preserved, failed preconditions are not hidden, and the explanation cache remains accurate enough for next-action display. Mobile asks the user for `major` conflicts, protected/discretionary tradeoffs, missing starting state, meaningful omission, or any repair that would change a user commitment. Mobile waits for desktop/cloud refinement or marks the Calendar stale when the issue is advisory, outside the reactive horizon, low coverage but not current-action-critical, or requires global optimization. It enters next-best-action mode when there is no safe default but there is at least one legitimate local Task or clarification Task to show.

### 16.6 Early completion and forward-pull behavior

When a Task completes earlier than expected, the default behavior should be to pull the next valid Dynamic Task forward to the present time if dependencies, preconditions, affect constraints, location constraints, and other Calendar Logic allow it.

This supports a conservative duration bias: when uncertain, the scheduler may prefer duration estimates that are not overly optimistic. Early completion then becomes easy to absorb by starting the next valid Dynamic Task immediately rather than leaving useless gaps or requiring disruptive global replanning.

Static Tasks retain fixed start times. If no predetermined Dynamic Task can validly fill time before the next Static Task, the planner considers evergreen gap-fill suggestions under section 15.4 and `gap_fill_policy` rather than treating the gap as unexplained failure.

This forward-pull semantic is the natural counterpart to the log-normal duration model. Log-normal duration distributions are right-skewed: delays stack asymmetrically and early completions are relatively rare. The forward-pull repair handles early completion cheaply — no global replan, just pull the next eligible Dynamic Task forward — while late completion requires no special action since the distribution already biases toward longer-than-mean durations. For lay audiences, this is well described as a stop-light model: a sequence of independent delay sources where slowdowns accumulate and early green lights simply mean you move a little sooner. See also: `UBU-D0166`.

### 16.7 Adaptive granularity and offline operation

Compact Calendar runtime uses configurable execution profiles. The default Phase 1 presets are:

- `full_detail`: 60-second delta, 3600-second reactive horizon, `0.99` branch coverage target;
- `mobile_moderate`: 300-second delta, 3600-second reactive horizon, `0.99` branch coverage target;
- `mobile_low_power`: 900-second delta, 1800-second reactive horizon, `0.95` branch coverage target;
- `offline_steward`: 900-second delta, 1800-second reactive horizon, `0.95` branch coverage target, using precomputed branches and local repair metadata.

The one-, five-, and fifteen-minute deltas are presets, not schema constants. Calendar policy, Device policy, user settings, and runtime conditions may choose different positive deltas, horizons, coverage targets, and compute budgets. The effective values are stored with the compact Calendar or PlanningRequest so replay and explanation use the same policy inputs.

Switching to a coarser profile is allowed for `low_battery`, `low_power_mode`, `thermal_pressure`, `offline`, `expected_offline_window`, `heavy_workload`, `user_setting`, `external_worker_unavailable`, and `compute_budget_exceeded`. Returning to a finer profile is allowed when power, temperature, connectivity, worker availability, idle time, or user settings permit it. A switch records its reason in runtime or compact Calendar metadata; if it can affect the current or next recommendation, hard feasibility, affect legitimacy, or coverage threshold result, UbU records or batches a recalculation trigger using the relevant existing trigger kind.

When UbU switches to a coarser delta, the user-facing explanation must disclose the active profile, reason, effective delta, reactive horizon, coverage target or estimate, expected loss of precision, guarantees that still hold, and how to request more detailed planning or wait for a finer-capability backend.

Known offline windows trigger preparatory precomputation when connectivity and compute are available. The precompute package should cover the declared offline window plus the current reactive horizon when feasible, and should store the default Plan, last legitimate Plan, decision envelopes, Task criticality, cached explanations, simple repair recipes, and high-probability branch materialization or reconstruction instructions up to the effective coverage target. Precomputation is bounded by battery, thermal, user, Compartment, and compute-budget policy.

Unexpected offline operation degrades to cached compact Calendar state, local explicit algorithms, coarser deltas, reduced branch horizon and coverage target, simple repair recipes, and next-best-action mode. It must not pretend cloud, worker, or LLM-assisted analysis is still available.

Cached branches remain usable only while accepted Logs, Snapshots, External Events, elapsed time, and user actions stay inside their decision envelopes and while dependencies, preconditions, affect assumptions, and coverage remain valid. A branch expires when divergence leaves its envelope, a required dependency or precondition changes, a Static Task conflict appears, affect data becomes invalid for the Plan, coverage falls below the effective threshold, or the branch passes its recorded expiry.

The minimum mobile-only guarantee is the core UbU loop on one local device: preserve explicit state, show one recommended next Task or a clarification Task, explain why it matters now, honor Static Tasks and hard constraints conservatively, expose affect/staleness and coverage degradation, record user feedback in Logs, and run local repair for the current or next Task. Mobile may use coarser granularity, shallower branch coverage, reduced analysis depth, and fewer LLM-assisted features; it may not hide a mandatory external compute dependency.

### 16.8 GPU-aware execution backends and privacy boundary

The open-core planning loop must not depend on a hidden cloud service. Mobile-only UbU must preserve the core UbU experience, but it should do so as a real-time steward of Plan legitimacy, user agency, and next-action clarity rather than as the full global optimizer.

Cloud or external compute may improve performance, granularity, analysis depth, branch coverage, and expensive legitimization, but must not be a hidden mandatory dependency for the open-core planning loop. Solver selection should treat GPU suitability as a first-class criterion. CPU or conservative exact logic should certify skeleton validity, hard constraints, contradiction diagnostics, and explanations; GPU-capable search, simulation, scoring, and learned-model inference should handle large candidate sets, stochastic robustness, affect scoring, and premium cloud planning where available.

Possible execution backends include:

- local mobile execution;
- local desktop or laptop execution;
- Phase 2 user-owned worker devices;
- dedicated user-owned worker appliances;
- UbU corporate hosted compute;
- third-party UbU-compatible compute providers;
- future privacy-preserving compute services such as practical FHE-backed or comparable encrypted computation.

The FOSS planner and Compact Calendar representation should be backend-agnostic. Execution providers are interchangeable through explicit APIs, capability grants, Compartment policy, provenance, and user-visible routing decisions. GPU search may propose candidate Plans, but hard constraints and final Plan validity must be certified by exact or conservative validation. Cloud planning payloads should minimize PII and transmit only the structured timing, dependency, value, constraint, criticality, and legitimacy information needed for the selected provider.

**Backend trust boundary (`UBU-D0254`).** Backends beyond the controlling local instance are advisory workers behind an explicit `WorkerAuthority`; they may propose candidates, simulations, extraction or summary artifacts, projection previews, diagnostics, repair suggestions, and candidate mutations, but they do not mutate canonical state or decide their own scope. Returned artifacts are usable only after schema validation, Compartment/export admission, provenance validation, hard-constraint certification, and ordinary CPU commit or review paths.

**Phase 1b local advisory worker protocol (`UBU-D0255`).** A local advisory worker is invoked through a `LocalAdvisorySubmission` with a `WorkerAuthority`, minimized by-value payload, expected result schema, timeout, compute budget, result size limit, partial-result allowance, causal parents, policy-version refs, input digests, and provider/model configuration metadata. Its `WorkerAuthority` grants only named advisory capabilities and denies canonical writes, admission decisions, direct StateStore handles, extra context fetches, authority expansion, export-policy decisions, ungranted network/tool use, credential access, and retention past the request deadline.

A `LocalAdvisoryResult` echoes the authority, request, policy, input, provider, model, and configuration provenance and returns status `ok`, `partial`, `rejected`, `timeout`, `worker_error`, `malformed_result`, or `cancelled` with schema-valid artifacts, proposed candidates, proposed candidate mutations, diagnostics, safe telemetry, and deletion confirmation when required. Errors, timeouts, cancelled work, and malformed raw output are diagnostic records or quarantined digests, not candidate-state mutations; partial output is usable only artifact by artifact after schema validation.

The worker receives no mutable canonical-state capability. Only the controller/admission path can translate valid advisory artifacts into candidate-queue entries, projection previews, diagnostics, or candidate mutation envelopes, and those proposals remain subject to ordinary schema, policy, provenance, hard-constraint, affect/legitimacy, mutation, and user-review checks. Later remote, rented, or multi-provider workers must preserve the same authority envelope, status semantics, provenance, payload-minimization proof, input digests, retention/deletion semantics, result-size and partial-result rules, and CPU/admission certification; transport may add encryption, signatures, attestations, or sync-carried `worker_request`/`worker_result` statements but not a second mutation channel.

The open-core boundary includes the canonical planning schemas, Compact Calendar representation, deterministic skeletonization and precondition/effect evaluation, conservative CPU hard-constraint validation, admission and provenance checks, payload-safety validation, and enough local execution-profile support to produce a legitimate default Plan or blocking diagnostic without a hidden service. Hosted, rented, GPU, learned-model, or third-party compute may improve speed and depth, but they are replaceable request/result providers.

Routing selects the least-disclosing capable backend under policy, freshness, latency, energy, and compute-budget constraints. Mobile local execution remains the stewardship fallback for current/next Task clarity and cached repair; local desktop/laptop execution is the default high-fidelity self-hosted path when available; Phase 2 user-owned workers use the sync-carried worker protocol; corporate hosted compute and third-party UbU-compatible providers require explicit policy allowance and user-visible routing disclosure. Third-party providers receive no privileged access over local execution and must use the same authority, payload-safety proof, provenance, response validation, and CPU certification boundary as UbU-hosted providers.

Before external egress, the controller evaluates Compartment policy, export limits, Device or provider trust, `no_cloud_llm`-style constraints, retention rules, and purpose compatibility. Payloads are minimized `PlanningRequest`, `worker_request`, or derived context bundles over redacted Replicas; direct PII, stable object IDs, Compartment labels, secrets, integration credentials, and unrelated UniverseState are omitted, generalized, or replaced with redacted handles unless policy explicitly authorizes disclosure. Transport is encrypted, retention is bounded, diagnostics must not reintroduce stripped identifiers, and results echo policy versions, input digests or structural references, provider/model identity, safe telemetry, and required deletion confirmation.

Dedicated personal worker appliances are future packaging or reference hardware for ordinary user-owned worker Devices, not a separate trust class. Practical FHE-backed or comparable encrypted compute remains strategic research until it can preserve the same authority, provenance, routing, and certification boundary with stronger disclosure properties. Idle-compute monetization, if ever supported, is an external API-enabled user opt-in with revocation, policy limits, and auditability rather than an UbU corporate marketplace or default use of personal compute.

### 16.9 MVP Compact Calendar implementation

The minimum Phase 1 Compact Calendar implementation stores the legitimized skeleton baseline, the default Plan, the active execution profile, coverage estimate, uncovered-mass estimate, effective coverage threshold, decision envelopes, Task criticality, conflict severity, cached explanation fragments, simple repair recipes, probability provenance, and last-legitimate-Plan record. It stores user-previewed, risk-report, and debug/reproducibility Plans only when those artifacts are actually generated or needed for audit.

High-probability near-term alternatives within the reactive horizon may be stored as materialized branches or as deterministic reconstruction instructions. Non-selected candidate Plans, expensive global repairs, and low-probability branches are reconstructed on demand from the compact grammar, current UniverseState, Logs, Snapshots, External Events, repair recipes, and cached provenance.

MVP planning does not require exhaustive optimal search, cloud compute, GPU execution, exact coverage proof, learned policy models, or a complete transport grammar for every post-MVP uncertainty source. The required implementation is a deterministic skeletonizer, minimum legitimizer, greedy baseline, bounded candidate expansion, hard-constraint validator, Plan probability/provenance record, compact default-Plan package, short-horizon repair cache or reconstruction path, and low-coverage recalculation trigger.

### 16.10 GPU desktop execution backend

The Phase 1 performance target is a local desktop/laptop GPU backend. A small CPU reference path or fixture-backed deterministic path remains required for tests, CI, and contributors without GPU access. Mobile and cloud planner backends are deferred beyond Phase 1.

The implementation-facing contract for this section is `PLANNING_KERNEL_CONTRACT.md`. `DESIGN.md` defines the architectural intent; the contract file defines the Phase 1 schema and boundary details.

#### 16.10.1 Pure function contract

The GPU planning engine is a pure function. Its input is a `PlanningRequest`. Its output is a `PlanningResponse` containing ranked PlanCandidates, diagnostics, warnings, and probability-quality metadata. The engine has no side effects, performs no I/O, and holds no mutable state. The CPU kernel owns all canonical state mutation. The GPU engine is advisory only: it proposes; the CPU kernel validates and commits.

The engine is invoked as a typed Python function call, not a subprocess or service. The framework is PyTorch.

#### 16.10.2 CPU/GPU handoff contract

The CPU/GPU boundary is defined by two typed objects: `PlanningRequest` and `PlanningResponse`, specified in `PLANNING_KERNEL_CONTRACT.md`.

`PlanningRequest` includes schema version, planner version, request ID, effective time, generated-at time, mode, RNG seed, time-window policy, horizon policy, compute budget, task graph with CPU-provided `topological_order`, UniverseState snapshot, AffectProfile, scoring policy, constraint policy, payload policy summary, and privacy/provenance payload-safety proof. Optional fields include external event assumptions, repair context, explanation request, and debug flags.

`PlanningResponse` returns ranked PlanCandidates plus diagnostics, rejection counts, warnings, probability-quality metadata, optional coverage estimate, optional compute telemetry, and stage funnel counts. `K=3` PlanCandidates is the Phase 1 default:

- highest-utility candidate;
- most-robust candidate;
- most-schedule-diverse candidate.

K is user-configurable to allow power users to tune computational resource use. The Phase 1 default rollout budget is `n_rollouts = 1000` per finalist unless overridden by the CPU kernel's compute budget.

#### 16.10.3 Tensor layout

All tensor batches use padded fixed-size shape `(N_CANDIDATES, MAX_PLANNING_TASKS, ...)` with an explicit boolean validity mask tensor marking valid task slots. Ragged tensors are not used in Phase 1.

`MAX_PLANNING_TASKS = 256` is the Phase 1 planning window ceiling, defined at a single site. Future premium or wide-horizon tiers may increase this ceiling by scalar configuration, subject to memory, scenario-count, correlation-matrix, validation-cost, and backend performance limits. Linear scaling is not assumed as a mathematical guarantee.

#### 16.10.4 Pipeline stages

The GPU engine has four first-class pipeline stages, with semantic boundaries specified in `PLANNING_KERNEL_CONTRACT.md §5`.

1. **Skeleton sampling** — parallel shifted-log-normal or fixed duration sampling across candidates with vectorized propagation over the CPU-provided topological order.

2. **`affect_legitimacy_filter`** — batch sigmoid affect-constraint evaluation across surviving candidates. This stage implements only the sigmoid affect-constraint portion of legitimization. Full legitimization in UbU design is broader: it makes a skeleton Plan human-viable by respecting affect, recovery, transition, rest, sustainability, and support constraints. The `affect_legitimacy_filter` GPU stage does not replace or subsume full legitimization.

3. **Value scoring** — parallel utility, approximate robustness, affect-margin, and schedule-diversity scoring across surviving candidates.

4. **Monte Carlo rollout** — joint scenario simulation for finalists using correlation-group Gaussian copula sampling with deterministic rollout seed derivation.

GPU search proposes candidates. Hard constraint certification and final Plan validity are performed by the CPU kernel using exact or conservative validation.

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

#### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §16 — Conflict detection

```markdown
## 16. Conflict detection

| Conflict                      | Meaning                                             |
| ----------------------------- | --------------------------------------------------- |
| `stale_prior_version`         | Statement was based on an older object version.     |
| `concurrent_status_change`    | Multiple Devices changed status incompatibly.       |
| `compartment_policy_conflict` | Object changed while policy changed.                |
| `policy_version_conflict`      | Statement relies on stale or mismatched policy versions. |
| `device_revoked_conflict`     | Statement came from a revoked Device.               |
| `calendar_region_conflict`    | Mutation affects protected Calendar region.         |
| `payload_visibility_conflict` | Device submitted evidence it should not possess.    |
| `projection_conflict`         | Third-party state changed independently.            |
| `derived_state_stale`         | Statement relied on stale Plan/Calendar/risk state. |
| `duplicate_statement`         | Same idempotency key or content already applied.    |
| `incomplete_sync_session`     | Sync disconnected before a safe checkpoint.         |

> **Naming note (conflict class vs trigger).** `policy_version_conflict` is a *conflict
> class* (a detected state, listed above). `policy_version_conflict_detected` in §22 is
> the corresponding *recalculation trigger* (an event that fires planning response). They
> are deliberately distinct identifiers — one names the condition, the other names the
> event that the condition raises — and must not be collapsed into a single symbol during
> codegen. The same class→trigger relationship holds for the other conflict/trigger pairs
> (e.g. `conflict_detected` / `conflict_resolved` in §22 correspond to the lifecycle of
> any class in this table).

---
```

#### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §17 — Conflict resolution

```markdown
## 17. Conflict resolution

| Situation                             | Default                                                  |
| ------------------------------------- | -------------------------------------------------------- |
| Duplicate same idempotency key        | Collapse as duplicate.                                   |
| Offline completion, Task still active | Admit completion.                                        |
| Offline completion, Task superseded   | Preserve as Log observation only.                        |
| Offline snooze, Plan changed          | Treat as preference evidence; trigger recalculation.     |
| Concurrent complete vs reject         | Manual review unless deterministic rule adopted.         |
| Compartment policy conflict           | Fail closed; require review.                             |
| Revoked Device                        | Reject pending statements unless recovered by user.      |
| Protected Calendar region conflict    | Reject or require review.                                |
| Log entry conflict                    | Append both; correction if needed.                       |
| Incomplete sync session               | Do not treat sync as complete; resume or discard safely. |

Minimum incomplete-sync semantics:

* A sync session is complete only after a `checkpoint` statement or equivalent manifest
  commit is admitted.
* Partial receipt must not admit dependent payloads without their required statements.
* Repeated import must be idempotent.
* A failed session must leave enough metadata to resume, retry, or discard without
  treating the session as admitted.

> Conflict resolution is not merely database reconciliation. It may alter Plan
> legitimacy, Calendar validity, Device trust, Compartment visibility, or projection
> safety.

### The human merge decision is an event — and how it is surfaced

Per §9, a manual resolution is recorded as a `conflict_resolution` sync statement, which
enters the causal closure like any mutation. This subsection specifies **how the need for
that decision is surfaced to the user**, and resolves the open question of whether the
review should be modeled as a schedulable Task.

**Decision: surface manual merge through the existing diagnostic/prompt path, not as a
re-planned Task.** The Phase 0 schema already provides exactly the right surface:

* `SkeletonFailureDiagnostic` with `severity: "blocking"` and
  `prompt_policy: "immediate_blocking_prompt"` — a "stop and decide now" affordance that
  already exists and is already rendered by the loop.
* `SafeAlternative` with `action: "manual_decision"` — the existing action shape for "a
  human must choose here."
* `failure_class` extended (Phase 2) with a sync-conflict member; `state_status:
  "denied_by_policy"` already exists for the Compartment-conflict case.

A sync conflict that needs human review therefore emits a blocking diagnostic with an
immediate prompt and a `manual_decision` safe-alternative. The user's choice emits a
`conflict_resolution` statement. That statement — not the prompt — is what enters
admitted state.

**Why not a first-class Task.** Modeling the review as a high/critical Task that
re-enters the planner's `task_graph` risks a causal/engine loop, which is the exact
hazard flagged in design:

1. *Causal regress.* The review Task is itself synced state. A second conflict can arise
   *about* that Task (two Devices create it concurrently; its Calendar placement hits a
   protected region), requiring a review-of-the-review, and so on without a fixed point.
2. *Engine loop.* The planner needs a coherent admitted state to schedule a Task. But the
   region is incoherent *because* of the unresolved conflict. Gating the resolution
   behind a scheduled Task makes planning legitimacy depend on the very conflict the Task
   exists to resolve. Plan ⟂ resolution becomes circular.

The diagnostic/prompt path breaks both loops: the prompt is `derived_state` (recomputable,
device-local, never itself an admitted mutation that must converge across Devices), and
the resolution is a single `conflict_resolution` statement that does not require a
planned Task to exist first.

**Optional UX sugar (constrained).** A Device *may* render a convenience "review item"
that looks Task-like. If it does, that item must be:

* `derived_only` / device-local — it is never a `mutation` that enters shared
  `admitted_state`;
* not a precondition for admitting the `conflict_resolution` statement;
* not itself subject to multi-device conflict convergence.

In other words: the review may *look* like a Task to the user, but it must not *be* a
synchronized Task object. If a future phase wants a genuinely schedulable, syncable review
Task, that requires solving the regress above first — tracked as `UBU-Q0147`.

---
```

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
- When editing `OPEN_QUESTIONS.md`, anchor hunks with the selected question heading `## UBU-Q0134: Phase 1b projection conflict classes: auto-resolve versus required review` and its own `### Resolution` section. Do not use a repeated `### Resolution` heading from an earlier or later question as the edit location.
- If resolving the selected question, replace the `Unresolved.` text under that selected question's `### Resolution` section and update that same question's metadata line. Do not insert selected-question resolution text into any other question block.
- Use the existing `UBU-Qxxxx` and `UBU-Dxxxx` numbering conventions.
- Prefer minimal, auditable changesets.
- If the selected question is blocked, propose decomposition only if it produces replacement questions with fewer, simpler, or no dependencies.
- If the selected question is already partially resolved, narrow or clarify it rather than pretending it is fully unresolved.

Return only JSON.
