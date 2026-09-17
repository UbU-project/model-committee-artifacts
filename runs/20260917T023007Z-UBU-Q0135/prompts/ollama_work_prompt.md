# Model-Committee Work Proposal Request

You are participating in the UbU `model-committee` process.

Your task is to produce one concrete work proposal as strict JSON.

Do not return prose outside the JSON object.

## Selected question

Question ID: `UBU-Q0135`  
Question title: `Phase 1b local advisory worker protocol`  
Base commit: `50bebb3caae7d6a156b9210087ec3a7814eef9ac`

```markdown
## UBU-Q0135: Phase 1b local advisory worker protocol

Status: Open Priority: MVP important Phase: Phase 1b Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0145, UBU-Q0059 Blocks: Phase 1b advisory operations, Phase 3 worker substitution Resolved by: None Last scored: 2026-09-16 Scored from commit: None

### Question

What is the minimum worker protocol for a Phase 1b local advisory worker invoked through scoped authority and admission, such that a later remote worker is a transport substitution rather than a re-architecture?

### Subquestions

1. What `WorkerAuthority` scope does an advisory worker require, and what is it explicitly denied?
2. What submission and result envelope does an advisory call use, and by what path does its output reach admission?
3. How are worker failures, timeouts, and partial or malformed results represented without corrupting candidate state?
4. What model and provider identity, version, and configuration metadata must a submission carry for provenance?
5. What prevents an advisory worker from mutating canonical state directly, and how is that enforced rather than merely conventional?
6. Which parts of the protocol must remain unchanged when the transport becomes remote, rented, or multi-provider?

### Current direction

Phase 1b builds the local advisory worker behind a scoped authority and the admission boundary so that the marketplace phase becomes a transport substitution. The invariant that advisory proposals never directly mutate state is non-negotiable and already holds in the Quick UbU implementation, where the advisor only enqueues candidates. The general Phase 2 worker-Device protocol for user-owned remote Devices remains open under UBU-Q0145.

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
- next decision id: `UBU-D0255`

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

#### UBU-Q0059

```markdown
## UBU-Q0059: Execution-provider trust, worker backends, and privacy-preserving compute roadmap

Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: UBU-Q0016 Blocks: Phase 2 personal worker devices, hosted compute policy, third-party provider compatibility, Phase 1b advisory worker boundary Resolved by: UBU-D0254 Last scored: 2026-09-16 Scored from commit: None

### Question

How should UbU define the trust, privacy, capability, API, and disclosure boundary for execution backends beyond the current local instance?

### Subquestions

1. What is the minimum Phase 2 personal worker protocol for a user-owned laptop, desktop, or server?
2. How should the planner route work among mobile, local desktop/laptop, personal worker, corporate cloud, and third-party provider backends?
3. What Compartment rules, PII stripping, encryption, redaction, and provenance are required before any external backend receives work?
4. What must be open-core so FOSS contributors can inspect, run, modify, and self-host the planning loop without a hidden service?
5. How should third-party UbU-compatible providers interoperate without being privileged over local execution?
6. Should dedicated personal worker appliances be treated as commercial packaging, reference hardware, or future product work?
7. How should future practical FHE-backed or comparable encrypted compute be prioritized if it becomes viable?
8. Should idle compute monetization be an external API-enabled user choice rather than an UbU corporate marketplace?

### Current direction

Cloud or external compute may improve performance, granularity, and analysis depth, but must not be a hidden mandatory dependency for the open-core planning loop. User-owned worker devices are Phase 2. Dedicated appliances, corporate cloud, third-party providers, compute monetization, and FHE/private encrypted compute are future commercial or strategic-research directions.

Phase 1b re-scope: this question is pulled forward from Phase 2 under the Phase 1b rule, which requires every Quick UbU feature merged into mainline to adopt the restrictions, forms, and functional boundaries of the MVP design it corresponds to, even when that design's implementation phase is beyond Phase 1. Phase 1b emits artifacts whose shape depends on this answer, so it is answered now. Phase 1b scope: the local advisory worker (ollama) must be invoked as a worker behind a scoped `WorkerAuthority` and pass admission, never as a privileged internal service, so that a later remote or rented backend is a transport substitution rather than an advisory-layer rebuild. Subquestions 1, 2, 3, and 5 are Phase 1b-relevant. Subquestions 6, 7, and 8 (appliances, FHE, compute monetization) remain future commercial or strategic-research directions and are not answered by Phase 1b. The minimum local worker protocol is decomposed into UBU-Q0135.

### Resolution

Resolved by `UBU-D0254`: every non-local execution backend is an explicitly authorized, interchangeable advisory worker behind `WorkerAuthority`, Compartment/export admission, payload minimization, provenance, user-visible routing, and CPU certification on return. The open-core planner must remain self-hostable through the FOSS schemas, Compact Calendar representation, planning loop, and CPU admission/certification path; corporate hosted compute, third-party providers, appliances, FHE-like encrypted compute, and idle-compute monetization are optional future backends or packaging choices, not privileged dependencies.

---
```

#### UBU-Q0145

```markdown
## UBU-Q0145: Minimum useful worker Device protocol for Phase 2

Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0135, Phase 2 worker Device protocol Resolved by: UBU-D0250 Last scored: 2026-09-16 Scored from commit: None

Formerly UBU-QSYNC-007 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §20.

### Question

What is the minimum useful worker Device protocol for Phase 2?

### Resolution

Resolved by `UBU-D0250`: Phase 2 worker Devices use a scoped request/response protocol over normal sync statements. A controller sends a policy-checked `worker_request` with scope, purpose, allowed operations, input references or redacted context bundle, policy versions, retention deadline, and result schema. The worker may compute only within that scope and returns `worker_result` statements containing derived artifacts, candidate mutations, diagnostics, provenance, and deletion confirmation for transient payloads. Candidate mutations are never admitted state until normal sync-statement admission accepts them.

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

#### UBU-D0250

```markdown
## UBU-D0250: Minimum useful worker Device protocol is scoped request/result over sync

**Status:** Accepted → DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §20. Resolves `UBU-Q0145`.

For Phase 2, the minimum useful worker Device protocol is a policy-checked request/result exchange carried as sync statements, not an independent mutation channel. A controlling Device may issue a `worker_request` only after evaluating current Compartment policy and Device authority.

A `worker_request` contains:

- `request_id`, `controller_device_id`, and `target_worker_device_id` or equivalent opaque Device references;
- `zone_id`, allowed `compartment_ids`, redaction level, purpose, and policy-version references used to authorize the work;
- `allowed_operations`, limited in Phase 2 to candidate planning, simulation, local extraction, summarization, projection preview preparation, and candidate-mutation generation;
- input object references and optional scoped context-bundle digests, with payloads redacted or omitted when policy requires;
- retention deadline and required transient-payload deletion confirmation;
- expected `worker_result` schema, result size limits, and whether partial results are allowed;
- causal parents and observed versions needed for deterministic admission and stale-result detection.

A worker Device must reject the request if its current policy view is missing, stale in a way that could expand access, revoked, or incompatible with the requested Compartment, export, retention, or `no_cloud_llm` constraints. The worker may not broaden the scope, fetch extra protected context on its own authority, retain transient payloads beyond the request, or directly mutate admitted state.

A `worker_result` contains `request_id`, worker identity, the policy versions used, input digests or structural references, operation status, diagnostics safe at the request's visibility level, derived artifacts, projection previews, `worker_result` statements, candidate mutations, and transient-payload deletion confirmation. Candidate mutations are proposals only: they become admitted state solely through the normal sync-statement admission path, including conflict detection, policy checks, provenance, and user-review requirements.

This is intentionally narrower than a general distributed worker marketplace protocol. It is sufficient to let user-owned Devices contribute compute while preserving the §20 boundary that workers can prepare and propose but cannot expand authority or bypass admission.

---
```

#### UBU-D0254

```markdown
## UBU-D0254: Execution backends are interchangeable advisory workers behind authority, minimization, and certification

**Status:** Accepted → DESIGN.md §16.8. Resolves `UBU-Q0059`.

UbU execution providers beyond the currently controlling local instance are advisory workers, not privileged planning authorities. A backend may propose candidate Plans, simulations, summaries, extraction results, projection previews, candidate mutations, diagnostics, or repair suggestions only within an explicit `WorkerAuthority`. It cannot directly mutate canonical state, broaden its own scope, fetch additional protected context, decide Compartment export policy, or bypass admission. Returned artifacts become usable only after the controlling CPU kernel validates schema, policy provenance, hard constraints, skeleton validity, affect or legitimacy claims, and any candidate mutations through the ordinary admission path.

The open-core planning loop must remain self-hostable without a hidden service. The FOSS boundary includes the canonical planning schemas, Compact Calendar representation, deterministic skeletonization and precondition/effect evaluation, conservative CPU hard-constraint validation, admission and provenance checks, payload-safety validation, and enough local execution profile support to produce a legitimate default Plan or a blocking diagnostic. Hosted, rented, GPU, learned-model, or third-party compute may improve speed, branch coverage, candidate diversity, or analysis depth, but they are substitutions behind the same request/result boundary, not requirements for correctness or access to the user's state.

Every worker invocation carries a scoped authority envelope with the backend kind, provider or Device identity, requested capability, purpose, allowed `zone_id` and `compartment_ids`, redaction level, policy-version references, retention deadline, export/network/tool permissions, compute budget, model or solver identity when applicable, request ID, causal parents, and expected response schema. Worker capabilities are additive and narrow: candidate planning, simulation, local extraction, summarization, projection-preview preparation, and candidate-mutation generation are separate grants. Absence of a grant means denial.

Routing chooses the least-disclosing backend that satisfies the requested capability, policy, freshness, latency, energy, and compute-budget requirements. Mobile local execution is the stewardship fallback for current/next Task clarity and cached repair. Local desktop/laptop execution is the default high-fidelity self-hosted path when available. Phase 2 user-owned workers may receive work through the sync-carried worker protocol. Corporate hosted compute and third-party UbU-compatible providers require explicit policy allowance and user-visible routing disclosure. Third-party providers use the same APIs, safety proof, provenance, and return validation as UbU-hosted providers and receive no privileged access over local execution.

Before any external backend receives work, the controller must evaluate Compartment policy, export limits, Device or provider trust, `no_cloud_llm`-style constraints, retention rules, and purpose compatibility. The transmitted payload is a minimized `PlanningRequest`, `worker_request`, or derived context bundle over a redacted Replica: direct PII, stable object IDs, Compartment labels, secrets, integration credentials, and unrelated UniverseState are omitted, generalized, or replaced with redacted handles unless policy explicitly authorizes disclosure. Transport must be encrypted; retained payloads must obey the request retention deadline; diagnostics and logs must avoid reintroducing stripped identifiers. The request records a payload-safety proof or equivalent provenance summary, and the result echoes the policy versions, input digests or structural references, provider/model identity, telemetry safe for the authority level, and deletion confirmation when transient payload deletion was required.

Dedicated personal worker appliances are future packaging or reference-hardware work, not a special trust class. They must register and operate as ordinary user-owned worker Devices. Future practical FHE-backed or comparable encrypted compute is a strategic research backend class: it is worth tracking and may reduce disclosure when viable, but it must still use the same authority, capability, provenance, routing, and CPU-certification boundary unless a later accepted design proves stronger semantics. Idle-compute monetization, if ever supported, should be an external API-enabled user choice with explicit opt-in, revocation, policy limits, and auditability rather than an UbU corporate marketplace or a default use of personal compute.
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

The open-core boundary includes the canonical planning schemas, Compact Calendar representation, deterministic skeletonization and precondition/effect evaluation, conservative CPU hard-constraint validation, admission and provenance checks, payload-safety validation, and enough local execution-profile support to produce a legitimate default Plan or blocking diagnostic without a hidden service. Hosted, rented, GPU, learned-model, or third-party compute may improve speed and depth, but they are replaceable request/result providers.

Routing selects the least-disclosing capable backend under policy, freshness, latency, energy, and compute-budget constraints. Mobile local execution remains the stewardship fallback for current/next Task clarity and cached repair; local desktop/laptop execution is the default high-fidelity self-hosted path when available; Phase 2 user-owned workers use the sync-carried worker protocol; corporate hosted compute and third-party UbU-compatible providers require explicit policy allowance and user-visible routing disclosure. Third-party providers receive no privileged access over local execution and must use the same authority, payload-safety proof, provenance, response validation, and CPU certification boundary as UbU-hosted providers.

Before external egress, the controller evaluates Compartment policy, export limits, Device or provider trust, `no_cloud_llm`-style constraints, retention rules, and purpose compatibility. Payloads are minimized `PlanningRequest`, `worker_request`, or derived context bundles over redacted Replicas; direct PII, stable object IDs, Compartment labels, secrets, integration credentials, and unrelated UniverseState are omitted, generalized, or replaced with redacted handles unless policy explicitly authorizes disclosure. Transport is encrypted, retention is bounded, diagnostics must not reintroduce stripped identifiers, and results echo policy versions, input digests or structural references, provider/model identity, safe telemetry, and required deletion confirmation.

Dedicated personal worker appliances are future packaging or reference hardware for ordinary user-owned worker Devices, not a separate trust class. Practical FHE-backed or comparable encrypted compute remains strategic research until it can preserve the same authority, provenance, routing, and certification boundary with stronger disclosure properties. Idle-compute monetization, if ever supported, is an external API-enabled user opt-in with revocation, policy limits, and auditability rather than an UbU corporate marketplace or default use of personal compute.

### 16.9 MVP Compact Calendar implementation

The minimum Phase 1 Compact Calendar implementation stores the legitimized skeleton baseline, the default Plan, the active execution profile, coverage estimate, uncovered-mass estimate, effective coverage threshold, decision envelopes, Task criticality, cached explanation fragments, probability provenance, and last-legitimate-Plan reference. It stores user-previewed, risk-report, and debug/reproducibility Plans only when those artifacts are actually generated or needed for audit.

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

Phase 1 has no mobile GPU target. The required mobile fallback is CPU stewardship for current or next Task hard checks, cached last-legitimate Plan repair, decision envelopes, and simple repair recipes; exact mobile metadata remains in `UBU-Q0073`.

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

#### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §20 — User-owned worker Devices

```markdown
## 20. User-owned worker Devices

Worker Devices **may**: receive scoped context bundles; run planner candidates; run
simulations; perform local LLM extraction if policy allows; generate summaries; return
candidate mutations; return `worker_result` statements; prepare projection previews.

Worker Devices **may not**: directly mutate admitted state without sync-statement
admission; bypass Compartment policy; retain payloads beyond retention policy; route
`no_cloud_llm` data through cloud services; export `no_external_export` content; expand
their own authority; silently keep data after revocation.

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
- When editing `OPEN_QUESTIONS.md`, anchor hunks with the selected question heading `## UBU-Q0135: Phase 1b local advisory worker protocol` and its own `### Resolution` section. Do not use a repeated `### Resolution` heading from an earlier or later question as the edit location.
- If resolving the selected question, replace the `Unresolved.` text under that selected question's `### Resolution` section and update that same question's metadata line. Do not insert selected-question resolution text into any other question block.
- Use the existing `UBU-Qxxxx` and `UBU-Dxxxx` numbering conventions.
- Prefer minimal, auditable changesets.
- If the selected question is blocked, propose decomposition only if it produces replacement questions with fewer, simpler, or no dependencies.
- If the selected question is already partially resolved, narrow or clarify it rather than pretending it is fully unresolved.

Return only JSON.
