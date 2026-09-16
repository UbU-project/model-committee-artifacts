# UbU Decisions

Status: Draft  
Purpose: Lightweight architectural and data-model decision log for the UbU project.

This file records accepted design decisions so they do not need to be rediscovered from chat history. It is not a full specification. Details belong in `DESIGN.md`; unresolved matters belong in `OPEN_QUESTIONS.md`.

---

## UBU-D0001: GitHub repository is canonical for public design

**Status:** Accepted → DESIGN.md §31

The public GitHub repository is the canonical public design process for UbU.

Private chats, notes, and external documents may generate proposals, but accepted decisions become official only when reflected in the repository.

---

## UBU-D0002: Start public design documentation with four Markdown files

**Status:** Accepted → DESIGN.md §31

The initial public design repo should remain simple and LLM-friendly.

Initial files:

- `README.md`
- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

---

## UBU-D0003: UbU is a privacy-first planning and coordination system

**Status:** Accepted → DESIGN.md §1

UbU is not merely a task list, calendar, or project-management dashboard.

UbU is a system for converting messy real-world inputs into explicit, inspectable, recalculable plans.

Inputs may include:

- Tasks
- calendar events
- messages
- external events
- user preferences
- physical state
- affective state

---

## UBU-D0004: User sovereignty is foundational

**Status:** Accepted → DESIGN.md §2.1

The user is the final authority over what the user wants and what occurred.

UbU may recommend, infer, estimate, warn, and automate, but it must not override user sovereignty.

---

## UBU-D0005: Distinguish logistical consistency from philosophical consistency

**Status:** Accepted → DESIGN.md §2.1

UbU distinguishes:

- **Logistical consistency:** hard consistency required by the data model and planner.
- **Philosophical consistency:** broader consistency between stated user goals and actual behavior.

Logistical contradictions must be rejected, repaired, or resolved. Philosophical inconsistencies may be logged and reported later.

**Example:**

A cyclic active preference relation is a logistical contradiction. Ignoring a message despite an Objective to communicate more with someone is a philosophical inconsistency.

---

## UBU-D0006: LLMs are bounded assistants, not the canonical decision engine

**Status:** Accepted → DESIGN.md §2.7

LLMs may assist UbU, but they are not the canonical real-time planner or decision engine.

LLMs may serve as:

- planning oracles,
- advisory systems,
- UI/screenshot interpreters,
- external workflow helpers,
- document interpreters,
- value-reflection assistants.

---

## UBU-D0007: Value is attached to Objectives

**Status:** Accepted → DESIGN.md §7

From a data-model perspective, value is anchored to Objectives.

Tasks may derive value from the Objectives they serve, but Tasks are not the canonical source of value.

---

## UBU-D0008: Preference is a first-class relation object

**Status:** Accepted → DESIGN.md §8.1

Objective value is derived from Preference objects.

MVP Preference fields:

- `objective_a`
- `objective_b`
- `order`
- `acquired_method`
- `acquired_date`
- `enabled`

`order` may represent:

- Objective A preferred to Objective B

---

## UBU-D0009: Ordinal rankings compile into pairwise Preferences

**Status:** Accepted → DESIGN.md §8.2

If the user provides an ordinal ranking, UbU compiles it immediately into pairwise Preference objects.

The original ordinal input may be retained in the log, but the canonical value model uses pairwise Preferences.

---

## UBU-D0010: Preference cycles are logistical consistency errors

**Status:** Accepted → DESIGN.md §8.4

Active cyclic Preferences are not allowed.

Example:

- A preferred to B
- B preferred to C
- C preferred to A

This is a logistical contradiction.

---

## UBU-D0011: Derived utils are transient computational artifacts

**Status:** Accepted → DESIGN.md §8.5

Numeric utility values may be derived from Preferences, but they are transient and volatile.

They may be cached on Objectives, but must be recalculated when needed.

---

## UBU-D0012: Default util spacing uses √2 per preference level

**Status:** Accepted as MVP default, subject to future revision → DESIGN.md §8.5

The default MVP util derivation assigns:

- `1.0` to the lowest preference level,
- each higher level multiplied by `√2`.

Indifferent Objectives are placed at the same level and receive equal util values.

---

## UBU-D0013: UbU has three mutually exclusive operating modes

**Status:** Accepted → DESIGN.md §5

Each UbU instance runs in exactly one mode:

- `user_mode`
- `organization_mode`
- `worker_mode`

The mode is chosen at initialization and cannot change afterward.

---

## UBU-D0014: User mode models intrinsic human affect

**Status:** Accepted → DESIGN.md §5.1

`user_mode` is the mode for an autonomous human user.

Only user mode models intrinsic human affect.

---

## UBU-D0015: Organization mode does not model intrinsic affect

**Status:** Accepted → DESIGN.md §5.2

`organization_mode` represents an organization/project planning instance, but an organization is not a human.

Organization mode does not model intrinsic affect.

---

## UBU-D0016: Worker mode is a special UbU mode for delegated work

**Status:** Accepted → DESIGN.md §5.3

`worker_mode` is a one-device or one-enclave UbU mode used by Automation Workers.

Worker mode may run as:

- a daemon/service,
- a local workstation process,
- a GPU-capable device,
- a thin cloud-control server,
- or another specialized execution environment.

---

## UBU-D0017: Automation Worker is the technical term; Super Automation is a UX/product pattern

**Status:** Accepted → DESIGN.md §25.1

An **Automation Worker** is the technical execution entity.

**Super Automation** is a product/UX pattern in which UbU abstracts away difficult external interaction by using local services, Automation Workers, APIs, screenshots, photos, or LLM processing.

---

## UBU-D0018: GitHub is a projection of UbU, not the source of truth

**Status:** Accepted → DESIGN.md §27

For dogfooding, GitHub is treated as a low-dimensional projection of canonical UbU state.

UbU is the source of truth.

---

## UBU-D0019: GitHub projection requires reconciliation

**Status:** Accepted → DESIGN.md §27.4

Missed GitHub updates are expected in MVP.

UbU should support a reconciliation report comparing GitHub state to UbU state.

---

## UBU-D0020: Objective status and pipeline state are separate

**Status:** Accepted → DESIGN.md §27.2

`Objective.status` is the canonical UbU lifecycle status.

`pipeline_state` is a workflow/project-management status, such as a GitHub issue pipeline state.

---

## UBU-D0021: One GitHub object may map to many UbU objects and vice versa

**Status:** Accepted → DESIGN.md §27.1

GitHub ↔ UbU association is many-to-many.

Examples:

- One GitHub Issue may map to many Objectives.
- One Objective may map to many GitHub Issues.
- PRs, comments, reviews, and CI runs may associate with Objectives or Tasks.

---

## UBU-D0022: Objective has minimal MVP fields

**Status:** Accepted → DESIGN.md §7.2

MVP Objective required fields:

- `objective_id`
- `title_or_description`
- `mode`
- `status`

MVP Objective optional fields:

- `notes`
- `tags`
- `linked_container_refs`

Derived/transient Objective data:

---

## UBU-D0023: Objective modes are one-time or evergreen

**Status:** Accepted → DESIGN.md §7.1

Objectives have mode:

- `one_time`
- `evergreen`

One-time Objectives complete once and do not reactivate.

Evergreen Objectives can become satisfied and later active again.

---

## UBU-D0024: Objective satisfaction is derived in MVP

**Status:** Accepted → DESIGN.md §7.3

Objective satisfaction is not stored directly on Objective in MVP.

It is inferred from:

- Task effects on UniverseState,
- observed Snapshots,
- user declarations,
- other modeled state.

---

## UBU-D0025: WorkItems include Tasks and Containers

**Status:** Accepted → DESIGN.md §9.1

A WorkItem is the abstraction over work-like entities.

A WorkItem may be:

- Task
- Container
- future subtype

---

## UBU-D0026: Plans contain Tasks, not Containers

**Status:** Accepted → DESIGN.md §15.1

Plans contain an ordered array of Tasks.

Plans do not directly contain:

- Containers
- Objectives
- Techniques
- Recipes

---

## UBU-D0027: Static Tasks appear in Plans

**Status:** Accepted → DESIGN.md §9.2

Static Tasks are included directly in Plans.

---

## UBU-D0028: MVP Task schedulability invariant

**Status:** Accepted → DESIGN.md §9.3

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

---

## UBU-D0029: Task preconditions are deterministic UniverseState constraints in MVP

**Status:** Accepted → DESIGN.md §10.1

Task preconditions are deterministic constraints over UniverseState.

MVP preconditions support:

- equality checks
- membership checks
- absence checks
- simple AND/OR logic

Numeric comparisons are not in MVP.

---

## UBU-D0030: Task effects mutate UniverseState

**Status:** Accepted → DESIGN.md §10.2

A Task effect describes predicted mutation of UniverseState if the Task succeeds.

MVP effect object contains:

- scalar success probability
- mutation list

If success probability is `1` or `null`, the effect is assumed to occur when the Task completes.

If a Task fails, UniverseState is unchanged in MVP.

---

## UBU-D0031: Duration uncertainty and effect success probability are distinct

**Status:** Accepted → DESIGN.md §10.3

A Task may have:

- fixed duration or duration PDF,
- scalar success probability on the effect object.

These are separate.

---

## UBU-D0032: UniverseState is first-class

**Status:** Accepted → DESIGN.md §11

UniverseState is a first-class data object.

MVP UniverseState is a lightweight shell with loosely typed facts and events.

Core fields:

- `universe_state_id`
- `timestamp` or valid-at instant
- `facts`
- `numeric_values`
- `set_memberships`
- `event_markers`
- `source_summary`

---

## UBU-D0033: UniverseState uses lightweight free-form keys in MVP

**Status:** Accepted → DESIGN.md §11.2

MVP UniverseState keys are free-form strings.

Default keys should use a lightweight namespace convention, but enforcement is post-MVP.

Values may be text / JSON-like payloads.

---

## UBU-D0034: UniverseState mutation vocabulary

**Status:** Accepted → DESIGN.md §11.3

MVP mutations support:

- `set_fact`
- `clear_fact`
- `increment_numeric`
- `decrement_numeric`
- `add_membership`
- `remove_membership`
- `append_event_marker`

Exact mutation-item schema remains open.

---

## UBU-D0035: Snapshots are observed state updates

**Status:** Accepted → DESIGN.md §12

A Snapshot is an observed update to UniverseState.

User-declared and sensor-derived observations use the same object type.

MVP snapshot fields include:

- `snapshot_id`
- `timestamp`
- `source`
- values
- confidence

---

## UBU-D0036: Latest observed snapshot overrides simulation on conflict

**Status:** Accepted → DESIGN.md §12.1

MVP precedence rule:

- latest observed snapshot overrides simulation on conflicting fields;
- user-declared snapshots are treated as top-priority observations;
- confidence is stored but does not defeat explicit user declaration.

---

## UBU-D0037: Affect belongs to UniverseState in user mode

**Status:** Accepted → DESIGN.md §13

Affect is part of UniverseState in `user_mode`.

Affect is not intrinsic to organizations or machines.

---

## UBU-D0038: MVP affect dimensions

**Status:** Accepted → DESIGN.md §13.1

MVP affect dimensions:

- energy / tiredness
- stress level
- mood

Values are user-queryable.

Energy/tiredness and stress may be scored from `0.0` to `1.0`.

Mood is represented as:

- categorical trinary: `happy`, `sad`, `angry`
- intensity scalar from `0.0` to `1.0`

Interested/bored is independent and derived.

---

## UBU-D0039: Affect confidence decays by age in MVP

**Status:** Accepted → DESIGN.md §13.4

In MVP, affect confidence decreases over time due to staleness.

The threshold/frequency is an algorithm configuration setting.

---

## UBU-D0040: Affect collection is modeled through an evergreen Objective

**Status:** Accepted → DESIGN.md §13.5

UbU may include an evergreen Objective:

> Collect affect information relevant to important usage.

When affect data is missing or stale, the planning algorithm may create UI survey Tasks.

---

## UBU-D0041: External Events are instantaneous

**Status:** Accepted → DESIGN.md §14

An External Event is an instantaneous change in the universe.

External Events have no duration and may overlap Tasks.

They are not part of feasibility evaluation for an individual Plan in MVP.

---

## UBU-D0042: Calendar is a set of possible Plans

**Status:** Accepted → DESIGN.md §15.2

A Calendar is a set of possible Plans.

A Calendar has a default Plan.

The default Plan is a current best recommendation, not a user commitment.

---

## UBU-D0043: Compact Calendar coverage belongs to compact serialization

**Status:** Accepted → DESIGN.md §16.2

Coverage is a property of a compact Calendar representation, not of the abstract Calendar itself.

Coverage represents the probability mass of possible futures covered by the compact representation.

---

## UBU-D0044: Compact Calendar should prefer deterministic planner grammar over opaque PRNG seeds

**Status:** Provisional, refined by `UBU-D0108`, `UBU-D0123`, `UBU-D0124`, and `UBU-D0125` → DESIGN.md §16.3

A compact Calendar may not require PRNG seeds. Earlier design notes used deterministic DFS expansion as the candidate example. The current direction is broader: compact Calendar reconstruction should be based on deterministic, inspectable planner grammar and stored planning metadata, including skeleton Plan structure, legitimization state, candidate lineage, coverage, and repair metadata where appropriate.

DFS-like expansion may still be one implementation technique, but it is not the full architecture.

---

## UBU-D0045: Identity is the external-facing interaction surface

**Status:** Accepted → DESIGN.md §18

UbU interacts with outside agents through Identities.

A human may have multiple Identities.

Organizations and worker-mode instances are externally represented as Identities.

---

## UBU-D0046: Relationship is structured UniverseState data

**Status:** Accepted → DESIGN.md §22

A Relationship is structured UniverseState payload data representing the relationship between two Identities.

MVP Relationship data includes:

- two identity refs;
- one identity controlled by the user;
- user-stated affect state toward the other identity;
- inferred/speculated affect state of the other identity toward the user.

---

## UBU-D0047: Device means execution enclave

**Status:** Accepted → DESIGN.md §23.1

A Device is an execution enclave, not physical hardware.

Examples:

- OS profile
- container
- VM
- secure enclave

---

## UBU-D0048: Zone is a workspace-like instance context

**Status:** Accepted → DESIGN.md §23.2

A Zone is a workspace-like UbU context.

A Device belongs to exactly one Zone.

A Zone may have multiple Devices.

Zones maintain explicit allowlists/denylists of Compartments, defaulting to deny.

---

## UBU-D0049: Compartment is first-class

**Status:** Accepted → DESIGN.md §23.3

A Compartment is a first-class data containment object.

Compartments enforce hard invariants, such as:

- storage backend constraints
- device eligibility constraints
- identity disclosure constraints
- export/integration constraints
- retention constraints
- audit constraints

---

## UBU-D0050: Sensitive content is referenced, not embedded

**Status:** Accepted → DESIGN.md §23.4

If content is specific to a Compartment, a WorkItem should refer to it through a Compartment-scoped reference.

WorkItems must remain structurally usable without dereferencing sensitive content.

---

## UBU-D0051: Risk is reportable, not first-class in MVP

**Status:** Accepted → DESIGN.md §28

Risk is not a first-class object in MVP.

Risk is handled through reports derived from Calendar/Plan analysis.

Examples:

- P90 completion time
- critical path
- deadline miss probability
- affect constraint violation probability
- low coverage warning
- dependency fragility
- worker failure / bottleneck

---

## UBU-D0052: Moot is first-class terminal Task status

**Status:** Accepted → DESIGN.md §9.5

`moot` is a terminal Task status.

It is functionally equivalent to completion for planning, but distinct for logging/reporting.

Moot requires a reason code.

Candidate reason codes remain open.

---

## UBU-D0053: GitHub managed state should be clearly marked

**Status:** Accepted as MVP direction → DESIGN.md §27.3

UbU should avoid fighting manual GitHub edits.

MVP direction:

> UbU writes only clearly marked UbU-managed labels, comments, or blocks, and treats other GitHub edits as external events.

---

## UBU-D0054: The Phase 1 MVP target is dogfooding

**Status:** Accepted → DESIGN.md §4.1

The first MVP should help coordinate the UbU project itself.

Phase 1 focuses on single-user GitHub dogfooding before multi-device sync or multi-user coordination.

---

## UBU-D0055: Scope freeze is now a priority

**Status:** Accepted → DESIGN.md §31

The data-model discussion has reached the point of diminishing private returns.

Further unresolved questions should become public GitHub Issues when possible.

---

## UBU-D0056: File authority model for model-committee runs

**Status:** Accepted → DESIGN.md §31

The canonical source files for model-committee question-answering are:

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

Derived public-facing projections of the canonical design state currently include:

- `README.md`
- `OUTREACH.md`
- `PM_BRIEF.md`
- `FUNDER_BRIEF.md`
- `SOVEREIGN_COORDINATION.md`

---

## UBU-D0057: Model-committee automation is advisory and repo-driven

**Status:** Accepted → DESIGN.md §3

Superseded by `UBU-D0150`. See `UBU-D0150` for the accepted model-committee architecture and constraints.

---

## UBU-D0058: Model-committee v0.1 is intentionally constrained

**Status:** Accepted → DESIGN.md §3

Superseded by `UBU-D0150`. See `UBU-D0150` for the accepted model-committee architecture and constraints.

---

## UBU-D0059: Model committee outputs are weighted by capability and observed reliability

**Status:** Accepted → DESIGN.md §3

Superseded by `UBU-D0150`. See `UBU-D0150` for the accepted model-committee architecture and constraints.

---

## UBU-D0060: Open questions are selected by answerability, automation-likelihood, importance, and risk

**Status:** Accepted → DESIGN.md §3

Superseded by `UBU-D0150`. See `UBU-D0150` for the accepted model-committee architecture and constraints.

---

## UBU-D0061: DECISIONS.md is a bounded decision-memory index

**Status:** Accepted

Superseded by `UBU-D0150`. See `UBU-D0150` for the accepted model-committee architecture and constraints.

---

## UBU-D0062: Model-committee is a bootstrap UbU dogfooding workload

**Status:** Accepted → DESIGN.md §3

Superseded by `UBU-D0150`. See `UBU-D0150` for the accepted model-committee architecture and constraints.

---

## UBU-D0063: Model-committee work is changeset-based

**Status:** Accepted → DESIGN.md §3.2

Superseded by `UBU-D0150`. See `UBU-D0150` for the accepted model-committee architecture and constraints.

---

## UBU-D0064: Model-committee v0.1 uses a provisional filesystem log format

**Status:** Accepted → DESIGN.md §3

Superseded by `UBU-D0150`. See `UBU-D0150` for the accepted model-committee architecture and constraints.

---

## UBU-D0065: Model-committee v0.1 uses provisional quorum and provider-failure rules

**Status:** Accepted → DESIGN.md §3

Superseded by `UBU-D0150`. See `UBU-D0150` for the accepted model-committee architecture and constraints.

---

## UBU-D0066: Model-committee follows a prioritized recursive loop

**Status:** Accepted → DESIGN.md §3.3

The model-committee project should be developed as a bootstrap version of UbU’s future self-automation and dogfooding loop.

The loop has three prioritized modes:

1. system-wide consistency check,
2. question/problem prioritization selection,
3. work.

System-wide consistency checks have highest priority and should run after every merge, UbU directive change, LLM model update, or other state-changing event that could invalidate the project model.

Question/problem prioritization runs after consistency checks and selects the next work item by scoring answerability, automation-likelihood, importance, and risk.

Work is lowest priority and should normally run only after the current project state is coherent and the next work item has been selected.

---

## UBU-D0067: Directive decisions may be appended directly

**Status:** Accepted → DESIGN.md §31

The UbU project may receive direct project-owner directives that are appended to `DECISIONS.md` as accepted decisions without first passing through the ordinary model-committee question-answering loop.

These directive decisions are treated as canonical once committed to `DECISIONS.md`.

---

## UBU-D0068: Model-committee may decompose hard questions into easier questions

**Status:** Accepted → DESIGN.md §3.6

The model-committee process should not treat every increase in open-question count as a failure.

A work proposal may validly split, narrow, or restate a hard question into multiple simpler questions.

---

## UBU-D0069: Model-committee v0.1 uses Codex CLI as the primary model provider

**Status:** Accepted → DESIGN.md §3.1

`model-committee` v0.1 uses Codex CLI as the primary model provider for work proposal generation and work scoring.

The runtime calls `codex exec` with schema-constrained output. Prompts are passed through stdin. Final responses are written to JSON files. JSONL event streams and stderr are preserved in run logs.

Every runtime Codex call must pass `--skip-git-repo-check`.

`model-committee` does not pass deprecated `--disable web_search` flags. If web search must be disabled, that is handled through Codex configuration or profile state outside `model-committee`.

Codex must not directly modify repository files in v0.1. It produces JSON work proposals and score results. Patches are validated and selected by `model-committee`, then written as review artifacts.

---

## UBU-D0070: Model-committee v0.1 authority boundary is explicit

**Status:** Accepted → DESIGN.md §3.10

`model-committee` v0.1 has authority to produce derived analysis, candidate answers, candidate questions, readiness estimates, consistency reports, provider scores, candidate changesets, and review artifacts. It has no authority to create accepted design state without an ordinary committed change to the canonical repo.

Automated actions allowed in v0.1:

- reading canonical design files;
- parsing and scoring open questions;
- running consistency checks;
- generating Codex and Ollama work proposals;
- mechanically validating structured outputs and patches;
- invoking Codex CLI for required scoring;
- selecting a mechanically valid patch from valid proposals when Codex scoring succeeds;
- writing filesystem run logs and review artifacts.

Human review is required for:

- accepting answers, patches, or new questions into canonical design state;
- applying or committing patches to `DESIGN.md`, `DECISIONS.md`, or `OPEN_QUESTIONS.md`;
- closing, decomposing, superseding, archiving, or reprioritizing questions in the canonical repo;
- treating readiness scores as scope-freeze, release, or implementation go/no-go decisions;
- changing provider weights, quorum rules, or provider/network boundaries.

Forbidden actions in v0.1:

- direct OpenAI, Anthropic, Gemini, GitHub, or arbitrary HTTP API calls;
- auto-merge, auto-push, automatic PR creation, or remote GitHub mutation;
- automatic patch application to canonical repo files;
- direct canonical-file edits by Codex CLI, Ollama, or any model provider;
- internal manual override that bypasses failed Codex scoring or patch validation;
- updating derived readiness signals in `README.md` or `OUTREACH.md`;
- treating model output as project-owner directives, canonical user value, or release commitment.

Provider outputs are weighted by configured trust weights and observed reliability metadata rather than raw vote count. Static configured weights are sufficient in v0.1. Adaptive reliability weighting is deferred.

Provider failures are logged as run events. Each failure should preserve the provider ID, model name when known, run phase, failure class, timeout or exit status when available, stderr or raw response artifact path when available, and whether the run still met quorum.

A valid v0.1 committee result requires at least one mechanically valid work proposal and a valid Codex score result. A Codex work proposal should be attempted before patch selection. Two or more valid work proposals are preferred but not required. Failed secondary providers do not invalidate the run when quorum is met.

Codex CLI has subprocess-provider authority only. It may produce schema-constrained proposal and scoring artifacts through `codex exec`, but those artifacts are non-canonical until validated, selected, reviewed by a human, and committed. Direct OpenAI API authority is zero in v0.1 because direct OpenAI API calls are forbidden.

---

## UBU-D0071: MVP Logs are append-only per-instance event records

**Status:** Accepted

Resolved question: `UBU-Q0031`.

MVP Logs use a shared append-only entry envelope with event-specific payloads. Required fields are `log_entry_id`, `schema_version`, `instance_id`, `recorded_at`, `effective_at`, `event_type`, `actor_identity_ref`, `recorded_by_device_ref`, `target_ref`, `result`, `event_payload`, and `provenance`. Optional fields include old/new values, reason, notes, confidence, related Plan references, external references, annotation/correction links, and idempotency keys.

MVP event types are `task_completed`, `task_failed`, `task_moot`, `external_event_observed`, `snapshot_observed`, `objective_transitioned`, `plan_realized`, `decision_recorded`, `recalculation_triggered`, worker mutation submission/application/rejection events, and Log annotation/correction events.

Log entries are immutable once written. An annotation or correction creates a new Log entry that points to the original entry; it does not modify or delete the original. Corrections supersede interpretation of the original entry for query views while preserving the historical record.

Canonical Logs are stored per UbU instance, with device references recorded on entries. Device-local queues may exist for transport and audit, especially in later multi-device sync. MVP retention is indefinite for canonical Logs; archival may move old entries to colder local storage while preserving queryability and integrity. Deletion or redaction is deferred except where required by Compartment retention invariants.

Automation Workers contribute to Logs through worker Identities by submitting events or mutation requests. The canonical instance validates authority and records applied or rejected worker contributions with provenance, confidence when available, evidence references when available, and idempotency keys.

**Consequences:**

- Logs can support audit, reconciliation, worker accountability, plan-vs-reality feedback, and correction without losing historical claims.
- Detailed event-specific payload schemas may be refined alongside Task, Snapshot, Objective transition, worker mutation, and recalculation-trigger schemas.
- Large-history search can rely on rebuildable indexes over the append-only Log rather than treating indexes as canonical state.

---

## UBU-D0072: Phase 1 public demo is an end-to-end GitHub dogfooding loop

**Status:** Accepted

Resolved question: `UBU-Q0030`.

The Phase 1 public demo must prove that UbU can coordinate UbU's own development in a single-user dogfooding loop. It should use real UbU GitHub issues, PR/review/CI events, and milestone context when possible; if live access is unsafe, unavailable, or non-reproducible, a frozen fixture captured from real UbU GitHub data may be used with fixture provenance shown.

The smallest persuasive demo imports a curated issue set, maps it to Objectives and schedulable Tasks, generates a Calendar with a default Plan, explains the chosen Plan, displays at least one risk report, respects a user-mode affect Snapshot or stale-affect collection Task, shows Automation Worker assignment/status for delegated analysis or GitHub projection work, and writes or previews clearly marked UbU-managed GitHub projection labels/comments/blocks.

Live GitHub mutation is not required in a public recording. A dry-run projection is acceptable if it uses the same projection payloads and validation path that would be written after human approval. The demo must not depend on Phase 2 multi-device sync, Phase 3 multi-user coordination, full RBAC, fully settled Compact Calendar planner grammar, or autonomous remote GitHub mutation.

**Consequences:**

- The demo bar is end-to-end behavior, not complete implementation of every unresolved Phase 1 design question.
- Remaining GitHub projection, worker authority, risk-reporting, affect, and Compact Calendar details may still be refined in their dedicated open questions.
- Public messaging should describe any fixture, dry-run, or human-approval boundary explicitly.

---

## UBU-D0073: Core UbU planning and contributor surfaces are open source

**Status:** Accepted

Resolved question: `UBU-Q0029`.

UbU uses an open-core strategy, but the open core must include the planning kernel and the contributor-facing integration surface. A public contributor must be able to inspect, run, modify, and self-host the core system needed for ordinary single-user and project dogfooding without depending on private replacement components.

The definitely open-source surface includes data model schemas and migrations; the explicit planner and Calendar-generation logic required for Phase 1 dogfooding; GitHub import, triage, projection, reconciliation, fixture/demo tooling, and managed-label/comment/block formats; worker-mode runtime surfaces required for delegated work; compact Calendar serialization needed for transport or analysis; local-first storage and sync protocols when implemented; and the Super Automation extension/API boundary needed for third-party workers, connectors, or local services.

Private or commercial code may exist outside that boundary. Acceptable private areas include hosted-service operations, managed cloud infrastructure, paid support and packaging, premium hosted worker capacity, enterprise administration or compliance layers, proprietary connectors to closed third-party systems, and short-lived experimental prototypes that are not required for the public dogfooding loop.

Private experiments must not become hidden mandatory dependencies for public contributors. If an experimental component becomes necessary for the advertised open-source workflow, UbU must either open it before relying on it publicly or explicitly narrow the public promise. Repositories and packages should be labeled as open core, private experiment, premium hosted service, or external connector so contributors can tell where their work fits.

Implementation repositories should use OSI-approved licenses. The default license for core implementation repos is MPL-2.0 so modifications to core files remain shareable while integrations can be built without relicensing unrelated code. Stronger copyleft may be considered for network-hosted service code, and permissive licensing may be used for small examples, SDK stubs, or interoperability fixtures when that better serves adoption. No contributor agreement should grant unilateral proprietary relicensing of core contributions unless a later accepted decision explicitly changes that rule.

**Consequences:**

- The commercial boundary is service, operations, premium capacity, enterprise layers, proprietary closed-system connectors, and private experiments, not a hidden replacement for the planning kernel.
- Public contributor messaging should avoid claims that require private components.
- Future implementation repos need license files and package labels consistent with this boundary.

---

## UBU-D0074: Phase 1 privacy promise is a minimal Compartment guardrail layer

**Status:** Accepted

Resolved question: `UBU-Q0028`.

Phase 1 implements Compartments as metadata-backed classification and routing guardrails, not as the full multi-device containment system intended for later phases. A Phase 1 Compartment can mark content as local-only, disallow cloud LLM routing, disallow external export, declare allowed integrations, and declare allowed Devices within the limits of the single local Device model.

The hard Phase 1 invariants are narrow: `no_cloud_llm` Compartment content must not be sent to cloud LLM routes; `no_external_export` Compartment content must not be exported, projected, or handed to workers except as redacted structural references; Compartment-marked content crosses boundaries only through an allowed route and user-visible action; and boundary-crossing attempts that reach UbU are recorded in Logs as allowed or denied. Sensitive Compartment content is referenced rather than embedded in ordinary WorkItem structure.

Phase 1 does not promise complete privacy isolation, cryptographic isolation, hardware attestation, secure multi-device partial replication, protection from a malicious local administrator, or automated retention deletion/redaction. Retention policies may be recorded, but enforcement beyond append-only Log retention is post-MVP unless a specific implementation later adds and discloses it.

Un-compartmented content is explicitly labeled `security_level: low`. Low-security content is not compartment-protected and may be routed to configured integrations or cloud LLM-backed Automation Workers only through a user-visible integration or worker action.

Phase 1 may claim local-first operation only in the limited sense that canonical planning state, Logs, and source-linked project model data live in the local single-user UbU instance by default. It must not claim local-only operation, Phase 2 sync, conflict handling, partial replication, or secure multi-device Compartment propagation. Phase 1 may claim cloud LLM usage is optional, explicit, integration-scoped, and advisory; cloud LLMs are not the canonical planner and must not receive `no_cloud_llm` Compartment content.

**Consequences:**

- Public messaging must distinguish privacy-first architecture from the narrower implemented Phase 1 privacy baseline.
- Full Compartment enforcement across sync, retention, device eligibility, and cryptographic isolation remains future work unless separately accepted and implemented.
- Implementations must label un-compartmented content as low-security rather than implying default protection.

---

## UBU-D0075: Organization and worker web admin UIs are post-MVP public surfaces

**Status:** Accepted

Resolved question: `UBU-Q0027`.

Organization-mode public UX is a project operations dashboard. The first screen should emphasize pipeline state, Plan/risk summaries, worker assignments and health, pending worker mutation requests, GitHub projection/reconciliation status, and recent decision/projection/worker Log entries requiring attention. It should avoid personal affect UX and should label admin-equivalent operation when full RBAC is absent.

Worker-mode public UX is an operator console for a worker instance. The first screen should emphasize connection and Identity status, service health and last check-in, active assignment and assignment queue, granted capability scopes, recent Log or mutation submissions, rejection/error state, and local resource status such as GPU availability or cloud compute state where applicable.

Both UIs may expose logs, queues, capability grants, resource status, pipeline state, risk reports, worker assignments, and GitHub projection status when the data is relevant to that mode. These views must default to summarized and redacted operational information and must not leak Compartment-protected payloads, broader planning state, or personal affect data through worker/admin convenience views.

No organization-mode or worker-mode web admin UI is required for Phase 1. Phase 1 requires only the single-user GitHub dogfooding surface and enough visible worker assignment/status information to satisfy the public demo criteria; that information may appear in the user-mode dogfooding UI, CLI output, or run artifacts.

**Consequences:**

- `UBU-Q0027` is resolved as a Post-MVP product direction rather than a Phase 1 implementation requirement.
- Phase 1 public UX scope remains focused on the single-user dogfooding loop.
- Future organization and worker admin UIs have a stable default first-screen direction without forcing RBAC or full admin products into MVP.
- Admin views must respect Compartment boundaries and mode boundaries.

---

## UBU-D0076: Relationship maintenance uses Objectives, history, and risk reports

**Status:** Accepted

Resolved question: `UBU-Q0026`.

In MVP, Relationship remains a structured UniverseState payload between two Identities and stores only the minimal relationship state already accepted: identity refs, the user-controlled Identity, user-stated affect toward the other Identity, and inferred/speculated affect of the other Identity where applicable.

Communication cadence belongs to an evergreen Objective recurrence/reactivation rule, not to the Relationship payload. Interaction evidence belongs in Logs, External Events, Task history, or Compartment/external references depending on source and sensitivity. Message bodies, private notes, or integration-specific history should remain behind Compartment or external-storage references when sensitive.

"Maintain relationship with contributor X" is modeled as an evergreen Objective linked indirectly to the contributor Relationship or relationship-relevant UniverseState key. That Objective can generate Dynamic Tasks such as reply, review, check in, or follow up. Task effects and imported External Events update ordinary UniverseState facts or event markers used to evaluate the Objective; they do not require a special Relationship-maintenance object in Phase 1.

Neglect risk is a risk-report finding derived from the Objective recurrence, observed interaction history, and current Plan/Calendar state. It is not stored on Relationship and does not make risk a first-class MVP object.

GitHub contributor interactions may update relationship-relevant modeled state when they are imported as External Events and associated with the user's GitHub Identity and the contributor Identity. They may satisfy or reactivate a relationship-maintenance Objective, but they must not overwrite user-stated private affect fields or infer sensitive relationship facts as canonical without user acceptance.

For Phase 1, relationship maintenance is supported only through existing Objective, Task, Log, External Event, UniverseState, Identity, Relationship, and risk-report mechanisms. A dedicated relationship-management UI, special cadence schema on Relationship, and full personal CRM behavior are deferred.

**Consequences:**

- Relationship payload stays small and privacy-sensitive.
- Communication cadence remains schedulable and recalculable through Objective recurrence.
- GitHub dogfooding can model contributor follow-up without adding a new relationship subsystem to MVP.
- Future richer relationship-management features can refine cadence evidence and private notes without changing the MVP anchor model.

---

## UBU-D0077: Model-committee v0.1 marks active pre-MVP dogfooding

**Status:** Accepted

`model-committee v0.1` marks a transition from pure design preparation into active pre-MVP dogfooding.

The project now has a runnable bootstrap artifact that can help process UbU design questions, generate reviewable changesets, and make the dogfooding loop visible.

This does not make `model-committee` the full UbU planner. It is a constrained bootstrap workload and advisory Automation Worker pattern.

**Consequences:**

- Public messaging should no longer imply that UbU is only an ideal design without runnable project machinery.
- The next strategic bottleneck is no longer the absence of a runnable skeleton.
- The next strategic bottlenecks are contributor conversion, visible dogfooding output, first-use workflow selection, and prototype-funder discovery.
- Further design writing should generally be subordinated to implementation, recruitment, dogfooding, or market discovery.
- `README.md` and `OUTREACH.md` should describe active pre-MVP dogfooding while preserving the canonical authority of `DESIGN.md`, `DECISIONS.md`, and `OPEN_QUESTIONS.md`.

---

## UBU-D0078: Recruitment targets a small core cohort, not a single co-builder

**Status:** Accepted

UbU’s near-term recruitment goal is to attract a small core cohort of serious, self-directed contributors.

The project should avoid public language implying that there is only one meaningful contributor role or that interested developers are competing for a single privileged position.

Popularity is not the goal, but multiple committed builders could materially accelerate UbU toward sustained full-time development.

**Consequences:**

- Public outreach should welcome several serious contributors.
- Contributor language should emphasize self-direction, seriousness, alignment, and concrete work rather than scarcity.
- The project should prefer committed contributors who can own bounded subsystems over broad passive interest.
- Casual interest remains useful when it produces workflow examples, design feedback, contributor leads, or prototype-funder leads.
- Recruitment success should be measured by concrete follow-up, contributions, design review, prototype funding, or subsystem ownership rather than raw attention.

---

## UBU-D0079: Independent knowledge workers are the first commercial beachhead hypothesis

**Status:** Accepted

UbU will treat privacy-sensitive independent knowledge workers as a leading commercial beachhead hypothesis.

This includes independent technical consultants, freelance developers, security researchers, solo founders, independent academics, technical writers, and similar workers who manage complex multi-client work privately.

This market is compatible with UbU’s north-star vision because these users experience personal self-governance, confidentiality, dependency, deadline, attention, and affect constraints directly.

Prototype funding from this market is acceptable only when it funds trunk features needed by the full personal self-governance product.

**Consequences:**

- Commercial discovery should include interviews with independent knowledge workers, not only FOSS maintainers or Ethereum teams.
- Prototype-funder outreach should test whether these users would fund or prepay for concrete core functionality.
- Acceptable funded work includes the planning kernel, local-first data model, privacy/Compartment model, affect-aware planning, GitHub/calendar/task ingestion, Logs, and recalculable Plans.
- Funding that requires surveillance, generic enterprise dashboards, centralized productivity telemetry, or manager-first reporting is incompatible unless a later accepted decision changes the project boundary.
- This decision is a market hypothesis, not a commitment to abandon FOSS or personal self-governance.

---

## UBU-D0080: Ethereum and FOSS outreach are recruitment and validation channels

**Status:** Accepted

Ethereum, FOSS, protocol, and autonomous developer communities are valuable early outreach channels because they expose UbU to high-autonomy contributors, complex coordination failures, anti-surveillance norms, and developer-tooling expectations.

These communities are not necessarily the first commercial buyer market.

Their primary near-term role is recruitment, workflow discovery, dogfooding validation, contributor-sustainability research, and project-management pain discovery.

**Consequences:**

- ETHConf and similar events should be treated as discovery and recruitment opportunities, not as proof that UbU is primarily a crypto project.
- FOSS outreach should seek concrete workflow examples, contributor candidates, design partners, and maintainers willing to discuss real coordination failures.
- Ethereum/protocol outreach should avoid token-first positioning.
- Project-management positioning should remain subordinate to personal self-governance and contributor sovereignty.
- UbU should not chase generic crypto speculation communities.

---

## UBU-D0081: Affect-aware planning is a core humane-planning requirement

**Status:** Accepted

UbU treats affect-aware planning as a core requirement, not a secondary wellness feature.

Planning systems that ignore fatigue, stress, boredom, motivation, emotional load, recovery, and dignity can produce plans that are technically organized but humanly unrealistic or harmful.

UbU should provide fast feedback about plan success or failure, suggest improvement after failure, respect emotional and physical limits, and still help users grow beyond current limitations.

The goal is neither comfort-maximization nor coercive productivity. The goal is humane self-governance: disciplined action that respects the user’s emotional and physical reality.

**Consequences:**

- Affect belongs to the core planning model in `user_mode`.
- Plans should be evaluated not only for logistical feasibility but also for human realism.
- A plan that repeatedly fails due to affect, fatigue, stress, boredom, or overload should cause model revision rather than user-blaming.
- Risk reports should eventually identify affect-related plan fragility.
- Public messaging may describe conventional planning tools as affect-blind, emotionally incomplete, human-incomplete, or mechanistic.

---

## UBU-D0082: High-quality plans need feedback, dignity, limits, and growth pressure

**Status:** Accepted

A UbU plan is not high-quality merely because it is internally consistent or time-feasible.

A high-quality plan should:

- produce fast feedback about success or failure;
- make failure informative rather than humiliating;
- suggest revision after failure;
- respect the user’s dignity;
- respect emotional and physical limits;
- distinguish sustainable stretch from destructive pressure;
- help the user grow beyond current limitations when appropriate;
- remain recalculable when reality contradicts the model.

This invariant is especially important in `user_mode`, where affect belongs to UniverseState.

**Consequences:**

- Plan evaluation should eventually consider whether a plan supports learning, adaptation, and sustainable self-improvement.
- Failure should generally produce improved modeling, revised constraints, or new suggested actions rather than moralized blame.
- UbU should push users toward growth only within a humane and feedback-sensitive planning loop.
- A plan that leaves the user worse off despite achieving nominal task completion should be considered suspect.
- This decision should inform `DESIGN.md` planning-quality language and future UI/UX choices.

---

## UBU-D0083: Technical essay should present a problem-first planning-kernel thesis

**Status:** Accepted

UbU should publish a problem-first technical essay that explains the planning-kernel thesis without reducing UbU to a product brochure.

The essay should argue that ordinary task managers, calendars, project boards, and opaque AI assistants fail to model real work because they do not represent objectives, state transitions, dependencies, constraints, logs, uncertainty, affect, and human limitations explicitly.

The essay should emphasize that most existing planning tools are affect-blind. They model tasks, deadlines, statuses, and calendar blocks, but not the emotional and physical reality of the person who must execute the plan.

The essay should explain why LLMs are useful but advisory: they can interpret, summarize, propose, and critique, but canonical planning must remain explicit, inspectable, and recalculable.

The essay should point to `model-committee` as UbU’s first visible dogfooding loop and end with a concrete contribution, interview, or prototype-funder request.

**Consequences:**

- The essay should lead with the problem, not with a feature list.
- The essay should avoid generic startup marketing language.
- The essay should invite concrete next actions from developers, maintainers, autonomous-team leads, and prototype funders.
- `OUTREACH.md` may summarize the essay project, but the essay itself may live outside the canonical design files.

---

## UBU-D0084: Public narrative may use moderated long-arc framing

**Status:** Accepted

UbU may publicly describe its long intellectual development history and the fact that modern LLMs have made the project newly viable.

This narrative may be used to attract contributors who want to work on software with durable significance.

Public framing should remain grounded, humble, and specific. It should avoid unrealistic claims, inevitability claims, or overstated promises about changing the world.

**Consequences:**

- Outreach may describe UbU as a long-running personal and technical project whose time may now be arriving.
- The narrative may help explain why the project has a mature design before it has a mature product.
- The narrative should be used to communicate commitment and depth, not destiny or superiority.
- Contributor recruitment should connect the long arc to concrete present work.

---

## UBU-D0085: Avoid exclusionary and scarcity-implying public framing

**Status:** Accepted

UbU public materials should avoid language that unintentionally makes interested contributors feel unwelcome, replaceable, or in competition for a single meaningful role.

UbU should also avoid accusatory language when describing planning systems that fail to model affect.

Preferred terms include:

- affect-blind;
- emotionally incomplete;
- human-incomplete;
- mechanistic planning;
- machine-like planning;
- non-humane planning;
- affect-insensitive planning.

**Consequences:**

- Public recruitment language should refer to a small core cohort or several serious contributors rather than a single highest-priority co-builder slot.
- Public language may still distinguish between serious contributors and passive interest.
- The project may strongly criticize mechanistic planning systems without using negative metaphors.
- Derived public-facing files should be updated when they imply scarcity or exclusion.
- Canonical design files should remain precise and humane in how they discuss affect, dignity, limits, and growth.

---

## UBU-D0086: Public outreach should convert interest into concrete next actions

**Status:** Accepted

UbU should not treat popularity as achievement.

Public outreach is successful when it produces concrete next actions that accelerate the project toward a working self-governance product.

Concrete next actions include:

- code contributions;
- test fixtures;
- design review;
- workflow examples;
- maintainer interviews;
- prototype-funder conversations;
- implementation-ready issues;
- review of `model-committee` run artifacts;
- sustained subsystem ownership;
- credible paths toward full-time development of core features.

---

## UBU-D0087: Commercial funding must preserve self-governance

**Status:** Accepted

UbU may accept funding, consulting, sponsorship, prepayment, or commercial prototype work only when the work accelerates trunk capabilities needed by the personal self-governance product and preserves user sovereignty, contributor sovereignty, privacy, and the open-core boundary.

A funding offer is compatible when it pays for reusable core work such as the planning kernel, local-first data model, privacy and Compartment guardrails, affect-aware planning, GitHub/calendar/task ingestion, Logs, recalculable Plans, risk reports, worker assignment/status, projection/reconciliation, or other features that become ordinary trunk capability.

Incompatible requested features include:

- surveillance-style project management, activity scoring, keystroke or screen monitoring, or always-on productivity telemetry;
- manager-first dashboards that rank, pressure, compare, or discipline contributors instead of supporting explicit self-governance and project coordination;
- centralized productivity telemetry across users, teams, clients, or contributors without explicit user-controlled sharing;
- generic enterprise reporting dashboards that pull UbU away from the planning kernel and toward conventional management software;
- features that expose affect, relationship state, Compartment payloads, or low-security personal context to managers, funders, or customers as a condition of use;
- token-first, speculation-first, governance-token, DAO-dashboard, or financialized coordination work that would reposition UbU as a crypto product;
- bespoke custom branches whose value depends on one funder's private workflow and does not produce reusable trunk capability.

Incompatible funding terms include:

- funder veto or control over the roadmap, canonical design process, accepted decisions, licensing boundary, or contributor access;
- exclusive ownership, assignment, or unilateral proprietary relicensing of open-core work;
- private replacement components for functionality that public contributors need for ordinary dogfooding;
- confidentiality terms that prevent honest public explanation of architecture, privacy limits, funded influence, or contributor-facing behavior;
- required pivot away from personal self-governance, privacy-first architecture, or the open-core planning kernel.

Acceptable prototype funding must default to trunk-first implementation. Customer-specific configuration, connectors, hosting, packaging, support, or compliance work may remain commercial when it stays outside the open-core boundary and does not become required for public dogfooding.

Crypto, tokenization, and DAO-specific requests should be rejected when they are token-first or speculation-first. They should be deferred when they are merely integration-specific and not required for the personal self-governance trunk. Ethereum and FOSS communities remain useful for recruitment, validation, and workflow discovery, not as a reason to reposition UbU as a crypto product.

Paid work should be evaluated with a simple rule: if the funded deliverable would make the open personal self-governance trunk better for independent knowledge workers and future public dogfooding, it may be considered; if it creates a private branch, surveillance surface, or funder-controlled roadmap, it should be rejected or renegotiated.

**Consequences:**

- Prototype-funder discovery can proceed with a clear accept/reject screen.
- Commercial discovery should prioritize independent knowledge-worker workflows that fund core planning, privacy, affect, worker, and projection capabilities.
- Enterprise opportunities are acceptable only when they preserve contributor sovereignty and remain subordinate to the planning-kernel trunk.
- Ethereum or DAO-related opportunities must not drive token-first positioning.
- Governance review remains required for ambiguous funding terms, especially exclusivity, IP assignment, confidentiality, roadmap control, and custom-branch obligations.

---

## UBU-D0088: Committed-contributor onboarding is public, bounded, and fixture-first

**Status:** Accepted

The minimum onboarding path for serious, self-directed contributors is a public, bounded path from context to a small verified contribution. It must not depend on private project-owner chats, unstated roadmap knowledge, private calendars, private GitHub data, or credentials.

A contributor should first read:

- the project overview and core principles in `DESIGN.md`;
- the model-committee dogfooding, Phase 1 public demo, GitHub dogfooding, design-process, open-core, and Phase 1 Compartment sections of `DESIGN.md`;
- the accepted decisions for open-core boundary, minimal Compartment promise, small core cohort recruitment, concrete outreach actions, and commercial self-governance red lines;
- the specific public issue, fixture, or model-committee artifact they intend to touch.

The first command should be a no-private-access local smoke check. For `model-committee`, the expected first command is `uv run model-committee doctor`, followed by a fake-provider or fixture-backed test/run when the implementation repo supports it. Equivalent first commands for later modules must avoid private tokens and should make missing optional providers or credentials visible as diagnostics rather than hidden prerequisites.

The first contribution should be one of: a synthetic or redacted workflow example, a fixture, a parser/validation test, review of a `model-committee` run artifact, documentation tied to a specific open question, or a narrow implementation-ready issue with explicit acceptance criteria. New contributors should not begin with hidden-roadmap work, direct GitHub mutation, Compartment-sensitive payloads, private-context reconstruction, broad planner rewrites, or module ownership.

Before implementation work, contributors must understand these boundaries: canonical design state lives only in committed canonical files; `model-committee` is advisory; v0.1 provider/network and no-auto-apply limits are hard; GitHub is a projection, not the source of truth; LLMs are advisory, not canonical planners; Compartment and low-security-content promises constrain data routing; the planning kernel and contributor-facing integration surface are open core; user sovereignty and mode boundaries are non-negotiable.

Contributor progression is staged:

---

## UBU-D0089: First prototype-funder workflow is commitment-risk review plus next-day plan

**Status:** Accepted → DESIGN.md §4

The smallest fundable workflow for privacy-sensitive independent technical consultants is a local client commitment-risk review plus next-day Plan. It addresses the pain of discovering too late that multi-client commitments, deadlines, dependencies, energy, and unavailable time no longer fit.

The first prototype should not require full inbox, notes, invoice, or message-body ingestion. Required inputs are manual client/project declarations, current commitments/deadlines, tasks or work items, current availability, compartment labels, and a current or stale affect Snapshot. Optional inputs are calendar busy/free blocks and GitHub issue/PR references when the consultant explicitly connects them. Email, notes, invoices, and private client documents may be represented only as user-approved structural references or later explicit connectors that obey Compartment rules.

The primary output is a compartment-aware weekly commitment-risk report with a next-day default Plan. The report should show each client/project compartment at a structural level, identify overcommitment, deadline risk, stale or blocked work, cross-client dependency pressure, and affect/energy constraint risk, then propose the next concrete work window. The output must be inspectable and recalculable from explicit data; it must not rank client value secretly or expose one client's sensitive payload in another client view.

A daily plan alone is too generic for the first funded prototype. A client-compartment view alone is too static. A weekly review alone may be too passive. The smallest valuable bundle is the weekly risk review plus the next-day Plan because it converts private commitments into immediate action without requiring broad surveillance-style ingestion.

Paid prototype work remains on the trunk only if the implementation uses reusable open-core structures: Objectives for client/project outcomes, Tasks for commitments, Compartments for client separation, Logs for declarations and actuals, Calendar/Plan generation for schedule recommendations, risk reports for commitment and dependency pressure, and optional GitHub/calendar ingestion through user-visible routes. Customer-specific templates, data imports, hosting, credentials, and support may remain commercial configuration.

Prototype-funder discovery should test prepaid design-partner structures, not open-ended bespoke consulting. Plausible offers to test are: a paid discovery/review session, a prepaid prototype sponsorship that funds an implementation slice, or a monthly design-partner retainer. As a working hypothesis, discovery can test roughly USD 250-750 for a focused review, USD 2,000-10,000 for a prototype sponsorship, and USD 1,000-3,000/month for a limited design-partner retainer. These are discovery hypotheses, not permanent pricing policy.

---

## UBU-D0090: Long-arc public narrative stays modest and testable

**Status:** Accepted

UbU may say that it grew out of a long personal and technical effort to understand planning, self-governance, privacy, and humane coordination. Public materials should include only enough of that history to explain commitment, design maturity, and why the project has more structure than a fresh prototype.

Modern LLMs should be described as changing the feasibility frontier for UbU, not as making success automatic. The grounded claim is that LLMs now make interpretation, summarization, proposal generation, fixture creation, review, and advisory automation cheap enough to support an explicit planning kernel. LLMs remain bounded assistants; they do not replace UbU's canonical planner, user sovereignty, or inspectable data model.

The long-arc narrative should communicate seriousness and durable possibility. Preferred public phrasing includes `long-running personal and technical project`, `the tools may finally be good enough to build the first narrow version`, `could matter for a long time if the dogfooding and contributor work prove it`, and `worth building carefully in public`.

The detailed version belongs in the first technical essay, selected outreach, talks, and interviews. README-level use should stay short and connect immediately to active dogfooding, concrete contribution paths, and prototype-funder discovery.

Avoid language that implies destiny, superiority, inevitability, or guaranteed world-historical impact. Avoid claims such as `will change the world`, `inevitable`, `once-in-a-generation`, `the future of work`, `solves coordination`, `the only real solution`, `decades ahead`, `everyone will need this`, and `LLMs make this inevitable`.

---

## UBU-D0091: First technical essay makes a precise planning-kernel request

**Status:** Accepted

The first public technical essay should make one central claim: ordinary task managers, calendars, project boards, and opaque AI assistants are useful projections, but they are not enough for real planning because they usually do not model objectives, state transitions, dependencies, constraints, logs, uncertainty, affect, and recalculation as explicit objects.

The working title should be:

> The Planning Kernel: What Task Managers Leave Out

The previously proposed title, `The Planning Kernel: Why Your Task Manager Is Lying to You`, should not be used as the working public title. It is memorable, but it implies accusation and undermines the humane, problem-first tone.

The essay should explain affect-blind planning as human-incomplete planning. Fatigue, stress, boredom, motivation, emotional load, recovery, and dignity are planning constraints. They are not excuses, decorative wellness features, or moral failures. A plan that ignores them can be internally organized while still being unrealistic for the human who must execute it.

The essay should distinguish UbU from adjacent tools as follows:

- task managers record intended work;
- calendars allocate time;
- project boards track workflow status;
- opaque AI assistants can generate plausible suggestions;

---

## UBU-D0092: Plan quality includes fast feedback, dignity, limits, and humane stretch

**Status:** Accepted → DESIGN.md §2.5.1

See DESIGN.md §2.5.1.

---

## UBU-D0093: Public recruitment invites several serious contributors into bounded paths

**Status:** Accepted

UbU's public recruitment copy should invite a small core cohort of serious, self-directed contributors, not a single co-builder, founding slot, or privileged insider. Seriousness is shown by concrete public follow-up: a workflow example, design review, fixture/test contribution, model-committee artifact review, narrow implementation-ready issue, or repeated reviewed work on a bounded subsystem.

Baseline public copy:

> UbU is looking for a small core cohort of serious, self-directed contributors. Good first contributions include workflow examples, design review, synthetic or redacted fixtures, parser or validation tests, model-committee artifact review, and narrow implementation-ready issues. If the work keeps the planning kernel inspectable, privacy-first, and useful for dogfooding, there is room for multiple contributors to earn bounded ownership over subsystems.

Public materials may name the contributor stages from onboarding: workflow informant, design reviewer, fixture/test contributor, implementation contributor, and bounded module owner. They may also route prototype funders and design partners into discovery, but prototype funding is not a contributor rank and must remain governed by the self-governance funding red lines.

The distinction between serious contribution and passive interest should be concrete rather than exclusionary. Passive interest is welcome when it produces workflow examples, design feedback, useful introductions, prototype-funder leads, public issue comments, or future contributor candidates. Serious contributor language should emphasize bounded public work, tests or fixtures, reviewed artifacts, and eventual subsystem ownership.

Avoid public phrases that imply scarcity or competition for one role, including `the co-builder`, `highest-priority contact`, `one founding slot`, `only serious builder`, `competing for a role`, `exclusive inner circle`, `prove you belong`, `hand-picked elite`, and similar language. If `co-builder` is ever used informally, it should not be singular or described as the single highest-priority outcome.

ETHConf and similar follow-up should classify people by their most useful next concrete action: send one workflow example, review a model-committee artifact, comment on a public design question, contribute a fixture/test, take a narrow implementation-ready issue, introduce a serious contributor, or discuss a trunk-compatible prototype sponsorship. Enthusiasm alone is not validation until it becomes one of these follow-ups.

---

## UBU-D0094: Release Outreach Pipeline makes releases explain themselves

**Status:** Accepted

UbU accepts the Release Outreach Pipeline feature bundle, with the tagline:

> UbU should make every release explain itself.

Release management in UbU should treat public and project-facing explanation artifacts as first-class release outputs. In addition to code, tests, changelogs, builds, and deployment artifacts, a meaningful release may require user-facing release notes, developer-facing release notes, screenshots, scripted UI-demo captures, video scripts, narration text, captions, YouTube metadata, public posts, known-limitations summaries, and contributor calls-to-action.

For UbU-runs-UbU, each minor release should normally produce a release outreach package when there is enough change to explain. The package should be derived from current repo state, release notes, accepted decisions, closed issues, updated design files, automated UI screenshots, scripted demo recordings, test fixtures, and explicitly labeled future plans.

Generated outreach must be evidence-bound. Claims about implemented behavior should be traceable to implemented features, accepted decisions, closed issues, release notes, test artifacts, screenshots, demo recordings, or other recorded evidence. Mock behavior, future plans, and speculative goals must be labeled as such.

The pipeline should support a fast public explanation path for nontechnical or lightly technical users and a sharper developer call-to-action path for serious contributors. A video can show the simple user-facing loop - bootstrap, one next task, explanation, feedback, and humane relationship or goal prompts - then close by showing UbU dogfooding its own GitHub issues, design docs, release tasks, and contributor needs.

The feature generalizes beyond UbU's own outreach. Project-management configurations should be able to define communication Objectives for users, developers, maintainers, funders, internal stakeholders, customers, community members, or other audiences. Release communication should become ordinary project work, not an afterthought.

Publication is gated by default. UbU may draft, assemble, render, and prepare outreach artifacts automatically, but external publication requires explicit human approval unless a project has configured a narrow trusted auto-publication policy. The pipeline must respect Compartment, Identity, export, and public-projection boundaries, and it must not leak private planning notes, contributor communications, personal data, or sensitive screenshots into public artifacts.

The implementation should be staged:

- Phase 0: manually structured release notes, screenshot lists, scripts, and calls-to-action.

---

## UBU-D0095: Worker authority uses scoped capability grants

**Status:** Accepted → DESIGN.md §25

See DESIGN.md §25.

---

## UBU-D0096: Phase 1 requires bootstrap interview and next-action focus UX

**Status:** Accepted → DESIGN.md §4.1

See DESIGN.md §4.1.

---

## UBU-D0097: Phase 1 MVP scope is frozen around single-user GitHub dogfooding

**Status:** Accepted → DESIGN.md §4.1

See DESIGN.md §4.1.

---

## UBU-D0098: GitHub external links use first-class External References

**Status:** Accepted → DESIGN.md §19

See DESIGN.md §19.

---

## UBU-D0099: Psychological theory inputs are calibration, discovery, preview, and review layers

**Status:** Accepted → DESIGN.md §2.2.1

See DESIGN.md §2.2.1.

---

## UBU-D0100: Snapshots are immutable partial observed assertions

**Status:** Accepted → DESIGN.md §12

Snapshots are partial observed assertions over specific UniverseState fields. They are not full assertions of all UniverseState and do not imply that omitted fields are absent, unchanged, or unknown. Applying a Snapshot updates only the fields included in that Snapshot.

Snapshot records are immutable once accepted into the append-only Log. The original Snapshot observation is never edited or deleted as canonical history. Annotation, correction, and revocation are represented by later Log entries that point to the original Snapshot observation or Log entry.

MVP confidence is stored at both levels: a required snapshot-level confidence summarizes the observation as a whole, and optional per-field confidence overrides may be present when different observed dimensions have different reliability. If a field has no per-field confidence, it inherits the snapshot-level confidence. For affect Snapshots, the existing MVP rule remains: affect confidence may be treated globally across affect dimensions, with per-dimension confidence deferred unless a Snapshot explicitly provides per-field confidence.

Conflict resolution is field-local. Latest observed Snapshot data overrides simulated state for conflicting fields. User-declared Snapshots have top priority over sensor-derived, imported, inferred, or worker-submitted observations in MVP. If two user-declared Snapshots conflict on the same field, the latest `effective_at` or Snapshot timestamp wins unless a later correction supersedes it. For non-user observations, the application algorithm may use source priority, effective timestamp, and confidence, but confidence does not defeat an explicit user declaration in MVP.

A Snapshot can be corrected or revoked, but only through a new Log entry. Correction that replaces the observed state creates a new Snapshot and links it to the corrected Snapshot or Log entry. Revocation without replacement creates a correction/revocation Log entry that excludes the original Snapshot from corrected query views while preserving the historical claim.

---

## UBU-D0101: Organization mode uses shared objects without intrinsic affect

**Status:** Accepted → DESIGN.md §26

See DESIGN.md §26.

---

## UBU-D0102: Phase 1 bootstrap and next-action UX stays narrow and inspectable

**Status:** Accepted → DESIGN.md §4.1

See DESIGN.md §4.1.

---

## UBU-D0107: Default Plan selection uses Plan probability over deterministic optimized Plans

**Status:** Accepted → DESIGN.md §15

Plan optimization and default Plan selection are distinct operations.

Each Plan represented on a Calendar is deterministic. Once materialized, it is a time-independent representation of predicted Actions/Tasks, similar in shape to a future-facing Log projection. A Plan may eventually include predicted sensor states and expected UniverseState transitions, but it does not internally evaluate its own probability after representation.

Each candidate Plan should be individually optimized for value while satisfying the constraints applicable to the modeled branch that produced it. These constraints include dependencies, deadlines, preconditions, Calendar Logic, affect constraints in `user_mode`, and user Preferences.

The planning algorithm models probabilistic parameters such as Task duration distributions, Task success or failure, external events, interruptions, availability changes, affect uncertainty, and sensor predictions. A particular modeled combination of those probabilistic inputs yields a deterministic candidate Plan. UbU may ascribe a **Plan probability** to that candidate Plan: the probability mass of the branch or parameter combination that yields it.

The default Plan is the deterministic candidate Plan, or one of multiple equal candidate Plans, with the highest Plan probability overall.

---

## UBU-D0108: Skeletonization and legitimization precede default Plan candidate search

**Status:** Accepted as provisional planning architecture → DESIGN.md §15

This decision supersedes the narrower framing that a DFS or DFS-like process directly produces the full default Plan as the conceptual foundation of Compact Calendar planning. DFS-like, BFS-like, greedy, local-search, GPU-parallel, or solver-backed methods may still be used as implementation techniques, but the planning architecture now begins with explicit skeletonization and legitimization.

The planner first creates a **skeleton Plan**. The skeleton Plan affixes Static Tasks, walks backward through dependency DAGs from terminal Static Tasks to dependency roots, and schedules prerequisites before dependents. It is a dependency-valid causal foundation, not an optimized or fully human-viable Plan.

The planner then performs **legitimization**: the process of adding the minimum constraints and support Tasks required to make the skeleton Plan plausibly executable by a real user. This includes affect, recovery, transition, rest, sustainability, and similar human-viability constraints.

The **legitimized skeleton Plan** is the minimum feasible baseline against which richer candidate Plans are compared. Candidate Plan generation, Plan probability assignment, optional Task filling, value optimization, and reactive branch construction occur after this baseline exists.

A short-horizon reactive layer, likely BFS-like or policy/repair based, remains necessary for near-term divergence involving duration variation, early completion, late completion, interruptions, external events, affect shifts, user overrides, and recalculation triggers. The provisional reactive branch horizon remains one hour, with a provisional short-horizon coverage target of `0.99` probability mass. These are configurable heuristics, not immutable design law.

---

## UBU-D0109: Early completion pulls the next valid Dynamic Task forward

**Status:** Accepted as provisional runtime behavior → DESIGN.md §15

When a Task completes earlier than expected, UbU should normally pull the next valid Dynamic Task forward to the current time, provided that dependencies, preconditions, affect constraints, location constraints, Calendar Logic, and user-visible policy allow it.

This supports conservative duration estimates. If the scheduler avoids overly optimistic durations, then early completion is easy to absorb: the next Dynamic Task can begin immediately instead of forcing a disruptive full replanning cycle.

Static Tasks keep fixed start times. If the next Static Task is not yet available and all predetermined Dynamic Tasks before it have been completed or blocked, UbU may offer gap-filling suggestions. The exact evergreen Task model remains open.

---

## UBU-D0110: Compact Calendar planning supports adaptive time granularity

**Status:** Accepted as provisional default policy → DESIGN.md §16

Compact Calendar planning should support configurable time deltas. The provisional defaults are:

- full-detail planning delta: `1 minute`;
- mobile moderate delta: `5 minutes`;
- mobile low-power or offline delta: `15 minutes`;
- reactive branch horizon: `1 hour`;
- short-horizon branch coverage target: `0.99` probability mass.

A one-minute delta aligns with common calendar behavior and should be available when resources permit. Mobile-only, low-power, or offline modes may use coarser deltas to preserve battery, reduce computation, and keep local recalculation responsive.

Known offline windows, such as flights or planned disconnection, should trigger preparatory precomputation when connectivity and compute resources are available. Unexpected offline operation should degrade gracefully to cached explicit state, local planning, coarser deltas, and reduced branch depth.

---

## UBU-D0111: External compute is optional and backend-agnostic for the open-core planning loop

**Status:** Accepted

Cloud or external compute may improve performance, granularity, analysis depth, stochastic branch coverage, and expensive legitimization, but it must not be a hidden mandatory dependency for the open-core planning loop.

The FOSS core should be indifferent to the execution backend. Planning work may run on a mobile device, a local desktop or laptop, a Phase 2 user-owned worker, a dedicated user-owned appliance, an UbU corporate hosted service, a third-party compatible provider, or a future privacy-preserving compute backend.

Execution-provider selection should treat GPU suitability as a first-class criterion. Many practical devices have much more parallel arithmetic capacity in GPU-like hardware than in CPU-bound exact solvers. UbU should therefore prefer hybrid algorithms in which CPU or conservative exact logic certifies skeleton validity, hard constraints, and explanations, while GPU-capable search, simulation, scoring, and learned-model inference evaluate large candidate sets, robustness, affect load, and premium cloud planning. GPU search may propose; exact or conservative validation must certify.

External execution must be mediated by explicit APIs, capability grants, Compartment policy, provenance, user-visible routing decisions, and privacy controls. PII stripping, encryption, redaction, compartment-aware payload minimization, and future privacy-preserving compute such as practical FHE-backed or comparable encrypted computation are strategic directions, but not Phase 1 guarantees unless implemented and disclosed. Cloud planning payloads should transmit the structured timing, dependency, value, constraint, criticality, and legitimacy information needed by the algorithm while stripping or compartmentalizing unnecessary personal detail. Compact Calendar results should return small references, IDs, decision envelopes, explanations, and repair metadata rather than raw personal context where possible.

---

## UBU-D0112: Cloud LLM integration is provider-neutral, local-first, and policy-routed

**Status:** Accepted

UbU's premier LLM execution path remains local-first use of a user-controlled local model provider such as `ollama`, but cloud LLMs are accepted as optional execution providers when the user, Compartment policy, cost policy, and task sensitivity allow them.

The core architecture should model LLM execution through a provider-neutral routing layer rather than treating a local `ollama` call as the only conceptual interface. Supported or planned provider classes include local model providers, user-configured BYOK cloud APIs, optional UbUCorp managed gateways, user-owned remote workers, and future compatible third-party providers.

Cloud LLM execution is not semantically equivalent to local execution. It crosses an external trust boundary and must be governed by Compartment policy, context minimization, redaction where appropriate, cost controls, provenance, visible routing decisions, and output validation. Cloud LLM output remains advisory unless transformed into canonical UbU state through explicit accepted objects and Logs.

UbUCorp may offer managed hosted inference, model routing, support, integrations, enterprise controls, and commercial provider relationships. That convenience service must not become a hidden dependency of the FOSS core, the open-core planning loop, or the UbU protocol. A commercial exclusive provider relationship for UbUCorp's hosted service is acceptable only if the core remains local-capable, provider-neutral, BYOK-capable, and self-hostable.

---

## UBU-D0113: Associations are Identity-scoped perceived coordination structures

**Status:** Accepted → DESIGN.md §19

UbU adopts **Association** as the canonical term for the earlier social-formation concept. An Association is an Identity-scoped model of an emergent group-like coordination pattern. It may represent a friend group, party, amateur league, FOSS project, contributor crew, skill network, marketplace, nonprofit, company, DAO-like project, or other formal or informal group.

Associations are not assumed to have globally objective membership, authority, boundaries, or interpersonal identity. By default, an Association exists as a local, perspective-bound model perceived by an Identity and supported by evidence. Other Identities may maintain overlapping but non-identical models of what they call the same Association.

UbU should represent Association facts through local perception, AssociationAttestations, Relationships, shared Objectives, commitments, norms, Logs, and External References. Legal entities, corporate filing numbers, GitHub organizations, websites, event pages, chat rooms, contracts, payment addresses, and other institutional or external records are External References or evidence, not the complete social reality of the Association.

A future SharedAssociationDescriptor may provide a signed or reviewable shared artifact that multiple Identities can reference, but it is not a God's-eye truth object. It is a coordination artifact that can be accepted, disputed, superseded, or interpreted differently by different Identities.

---

## UBU-D0114: Organizational introspection is a first-class UbU feature

**Status:** Accepted

UbU should treat organizational introspection as a first-class feature, not merely as a side effect of project management or future Association modeling.

Organizational introspection is UbU's ability to analyze evidence-bearing records from an Association, such as documentation, meeting notes, issue trackers, pull requests, governance discussions, chat logs, board minutes, public Discord or IRC history, outreach notes, and other permitted records, then generate reviewable, provenance-backed AssociationAttestations and feedback questions about the Association's actual behavior.

The goal is to help an Association inspect whether its real priorities, overrides, undocumented roles, decision paths, commitments, and work patterns are consistent with its declared mission, values, objectives, and public claims. Generated claims are candidate attestations with evidence, confidence, provenance, review status, and disclosure policy. They are not authoritative social truth.

This feature mirrors personal UbU introspection. For a person, UbU asks whether actual behavior matches stated values and Objectives. For an Association, UbU asks whether actual work, decisions, and resource allocation match declared mission and commitments.

---

## UBU-D0115: EthConf outreach uses one core pitch plus lightweight audience lanes

**Status:** Accepted

EthConf outreach should not splinter into many separate campaigns. The project should use one core UbU thesis with lightweight audience variants for FOSS contributors, prototype funders, general EthConf attendees, and a modest cypherpunk/privacy-builder lane.

The shared thesis is that UbU is local-first, user-sovereign AI planning and coordination infrastructure. It can run locally for privacy, use optional cloud LLM execution when policy allows, and eventually help Associations coordinate through explicit Identities, commitments, evidence, and bounded disclosure.

The cypherpunk/privacy lane should frame **Skill Barter marketplace** as a future specialization of Association modeling: lawful, privacy-preserving skill barter and skilled-work coordination through pseudonymous Identities, scoped work agreements, reputation without unnecessary doxxing, commitments, and user-chosen settlement references where lawful. It must not be framed as an illicit marketplace, sanctions-evasion tool, tax-evasion tool, or token-first product.

---

## UBU-D0116: EthConf outreach dogfoods Association formation and organizational introspection

**Status:** Accepted

UbU's EthConf outreach is itself an Association-forming workflow. The project owner is attempting to form an ad hoc Association around the UbU project through conversations with contributors, reviewers, funders, workflow informants, privacy/cypherpunk builders, and possible design partners.

This outreach should be treated as dogfooding, not mere marketing. Notes, follow-ups, contact classifications, public artifacts, private/redacted observations, and subsequent commitments can be modeled as Objectives, Tasks, Logs, Relationships, External Events, External References, and candidate AssociationAttestations.

The outreach process should later be submitted to LLM-assisted or manually reviewed organizational introspection. UbU should ask whether the actual outreach behavior proves that the project pursued its stated goals: recruiting serious contributors, pressure-testing the design, finding workflow examples, identifying compatible funding, and preserving the privacy-first self-governance mission.

---

## UBU-D0117: Context-rich cross-user messaging is a premier Phase 3 feature

**Status:** Accepted → DESIGN.md §20

Phase 3 minimal multi-user coordination should include a limited but meaningful form of cross-user contextual messaging. This should be treated as a premier Phase 3 feature, not merely as an incidental transport detail.

A UbU message is not only flat text. It is a communication event that may carry structured planning context. Even a minimal Phase 3 message envelope can help the receiving UbU instance decide whether a message should interrupt the current Plan, be deferred to a later review window, become a Task, update an Objective, or remain ordinary communication history.

A minimal Phase 3 **Message Context Envelope** should include only fields that are safe, useful, and policy-permitted, such as:

- sender and receiver Identity references or external references;
- source system or transport;
- raw or human-readable message body;
- message kind, such as request, status update, question, commitment, blocker, reminder, or FYI;
- topic or associated Objective/Task/Association references when disclosure policy allows them;
- requested response kind and any explicit deadline;
- priority and interrupt recommendation;

---

## UBU-D0118: Legacy messaging adapters may upgrade flat messages into UbU contextual messages

**Status:** Accepted as product and interoperability direction → DESIGN.md §20

UbU should be able to ingest and, where permitted, send through legacy communication systems such as WhatsApp, SMS, email, Discord, IRC, Slack, Matrix, or similar systems.

When only one side uses UbU, the legacy message remains a flat external input. UbU may scan, classify, summarize, and convert it into local candidate Tasks, Events, Logs, Relationship observations, or Association evidence, subject to user permission and integration policy.

When both sides use UbU, the two instances should be able to translate the legacy exchange into a UbU-native contextual message format. Depending on the transport and policy, this may happen through a side channel, an attached envelope, an agreed encoding, a linkable External Reference, or another adapter-specific mechanism. The raw legacy text remains the user-visible message; the UbU envelope supplies structured context.

This creates a practical bridge from legacy systems into richer UbU-to-UbU communication without requiring the rest of the world to abandon existing messaging platforms first.

---

## UBU-D0119: Structured message extraction is a bounded LLM-assisted normalization layer

**Status:** Accepted as architectural direction → DESIGN.md §20

UbU will ingest large volumes of unstructured direct-message and group-chat text from legacy systems. A dedicated **Message Context Extractor** should normalize those inputs into strict UbU JSON structures.

The extractor should take raw message text plus available metadata, such as source system, channel type, channel purpose, sender, receiver, Identity mapping, Association mapping, timestamp, thread context, Relationship context, and Compartment policy. It should output bounded structures describing message kind, topic, priority, interrupt level, candidate Tasks, Objective links, assumptions, ambiguities, actionability, response expectation, confidence, and provenance.

The extractor is not the canonical planner and must not silently mutate canonical state. Its outputs are candidate interpretations that pass through schema validation, provenance marking, confidence scoring, repair loops, and user or policy acceptance where required.

Phase 3 and early post-MVP implementations should begin with general LLMs constrained by strict schemas, grammar-constrained output where practical, validation, and repair. Fine-tuned, distilled, or adapter-trained extractor models may become attractive after UbU has stable schemas and enough corrected examples. Training a new foundation model from scratch is not currently justified.

---

## UBU-D0120: Personalized voice/TTS descriptors are optional consent-gated communication metadata

**Status:** Accepted as future product direction, not Phase 1 scope

UbU may eventually support optional voice or pronunciation descriptors so that a receiving UbU instance can render text messages using text-to-speech that approximates the sender's usual voice, pronunciation, cadence, or expressive style.

This can be viewed as a communication-compression feature: instead of sending a noisy audio recording, a user may send text plus a voice descriptor or voice-profile reference, allowing the receiver's device to synthesize a locally rendered spoken version. The same idea may also help accessibility, hands-free interaction, language learning, and more expressive asynchronous updates.

Voice data is sensitive. A voice descriptor or voice profile must be opt-in, revocable where practical, policy-governed, and clearly separated from authentication. UbU should not treat a synthesized voice as proof that a human actually spoke the words. Implementations should consider visible disclosure, watermarking or provenance markers, anti-impersonation controls, and restrictions on cross-context reuse.

---

## UBU-D0121: Messaging interoperability can support value-led viral outreach without coercive growth loops

**Status:** Accepted as outreach/product direction

Legacy messaging integration can create a natural adoption path. A UbU user interacting with a non-UbU contact may receive immediate value from local extraction, prioritization, task creation, and planning integration. If both people install UbU, they can exchange richer structured context and reduce ambiguity, missed requests, and priority confusion.

This is a plausible viral outreach mechanism: the product becomes more useful when counterparties also use it. However, UbU should not use manipulative dark patterns, spammy invitations, forced signatures, or guilt-based prompts. The adoption message should be value-led: richer coordination, clearer priority, less lost context, and better respect for both users' time.

---

## UBU-D0122: Greedy mean-duration planning is a benchmark, not the canonical planner

**Status:** Accepted as baseline algorithm policy → DESIGN.md §15

UbU may define a deliberately unintelligent greedy baseline planner for comparison. The baseline inserts Static Tasks first, places modeled External Events at their expected mean-point start time where applicable, ranks Dynamic Tasks by temporary value-per-minute from the Preference DAG divided by expected mean duration, and fills time from the earliest available slot forward while checking only local viability. It does not backtrack and should be expected to be inefficient, brittle, and strategically myopic.

This baseline is useful because later planners can be evaluated against a simple deterministic reference: total value, missed deadlines, affect burden, dependency failures, fragility under interruption, and explanation quality should improve over the baseline.

---

## UBU-D0123: Calendar planning begins from explicit UniverseState assumptions

**Status:** Accepted as provisional planning-model requirement → DESIGN.md §15

Every Calendar planning run must begin from an initial UniverseState. For MVP, the preferred case is a deterministic initial UniverseState in which planner-relevant starting facts are known, assumed, or explicitly unresolved. A future probabilistic initial UniverseState may assign probabilities to known possible starting states, but that mode is more complex and likely beyond MVP.

Skeleton Plan generation must check whether dependencies are already satisfied in the initial UniverseState before inserting prerequisite Tasks. A dependency does not automatically mean that a new Task should be scheduled; it means that a required state must be true before the dependent Task begins.

If the planner cannot create a viable skeleton Plan, normal planning should halt and UbU should immediately present a diagnostic explanation. The user should see the failed dependency, conflicting Static Task, impossible timing, cyclic dependency, missing state, or unavailable resource, plus selectable alternatives where possible.

---

## UBU-D0124: Legitimization makes skeleton Plans human-viable before optional optimization

**Status:** Accepted as provisional planning architecture → DESIGN.md §15

**Legitimization** is the planning phase that takes a skeleton Plan and adds the minimum additional constraints and support Tasks required to make the Plan plausibly executable by the user. It converts a dependency-valid but potentially unrealistic skeleton Plan into a minimally human-viable Plan.

Legitimization may add or enforce affect constraints, recovery Tasks, breaks, meals, rest, sleep, transition buffers, setup/teardown time, context-switch limits, motivation constraints, and other human sustainability requirements. It is distinct from later optimization: it asks whether the Plan could be realistically performed, not whether all available value has been added.

The legitimized skeleton Plan is the baseline feasible Plan. Optional Dynamic Tasks, gap-filling work, and higher-value candidate Plans should be compared against it.

If full legitimization is cheap, it can be used as a frequent validity oracle while adding and removing optional Tasks. If full legitimization is expensive, UbU needs semi-legitimization heuristics such as affect-budget estimates, slack preservation, dependency-fragility scoring, user-mode compatibility checks, local repair checks, and legitimacy-delta estimates before invoking full legitimization on finalist candidates.

---

## UBU-D0125: Compact Calendar search is GPU-aware hybrid planning

**Status:** Accepted as implementation direction → DESIGN.md §16

UbU should not assume that the central planning engine is a CPU-heavy exact solver. Solver/library selection should treat GPU suitability as a first-class criterion across mobile devices, laptops/desktops, user-owned workers, and cloud providers.

The preferred architecture is hybrid. CPU or conservative exact logic handles dependency graph traversal, skeleton validity, hard constraints, contradiction diagnosis, final validation, and explanation. GPU-capable methods handle large candidate expansion, stochastic scenario simulation, affect scoring, robustness scoring, learned-model inference, and premium cloud planning.

Existing CPU-oriented solvers such as CP-SAT, SMT/MaxSMT, or local-search systems may remain useful for prototypes, exact finalist validation, contradiction explanation, and desktop/server experiments. They should not become the only planning path if GPU-friendly search, simulation, or scoring better matches available compute.

---

## UBU-D0126: Mobile planning is real-time stewardship, not full global optimization

**Status:** Accepted as mobile planning UX direction

A mobile device has the least computational headroom but the highest demand for immediate user-facing adaptation. Mobile UbU therefore should not be designed as the full global planner. Its role is real-time stewardship of Plan legitimacy, user agency, and next-action clarity.

Beyond short-horizon BFS precomputation, compact Calendars should support decision envelopes, protected/flexible/disposable region metadata, last-legitimate-Plan repair, precomputed repair recipes, fast local policy selection, progressive planning feedback, cached explanations, conflict severity levels, next-best-action mode, opportunistic idle/charging computation, optional remote assist without dependency, and uncertainty-aware UI.

The MVP-friendly subset is narrower: Task criticality, last legitimate Plan storage, simple repair rules, conflict severity levels, cached explanations, next-best-action mode, and basic decision envelopes.

---

## UBU-D0127: VoxPopuli is an optional EthConf demo, not a Phase 1 replacement

**Status:** Accepted as optional outreach/demo concept

UbU may demonstrate an optional **VoxPopuli** flow at EthConf: before structured bootstrap questions, a user speaks freely about what they wish would happen, what feels disorganized, or what kind of planning help they want. An LLM-assisted extractor converts that natural-language input into candidate Objectives, Tasks, constraints, preferences, and planning assumptions for the user to inspect, correct, accept, or reject.

The value of this flow is demonstration and trust-building. It shows that LLMs can help turn abstract human concerns into explicit structured planning material, while UbU remains the inspectable planning system that consumes accepted structures.

This must not override the Phase 1 narrow bootstrap requirement. VoxPopuli is optional, experimental, and useful for EthConf/public demonstrations only if it does not displace higher-priority dogfooding, contributor, or funder deliverables.

---

## UBU-D0128: Planning horizons may exceed visible Calendar windows and should front-load fragile prerequisites

**Status:** Accepted as provisional planning architecture → DESIGN.md §15

The user-visible Calendar window and the internal planning horizon do not have to be identical. If the user asks for a one-day Calendar, UbU may need to reason beyond that visible window to avoid cutting dependency chains, Techniques, deadlines, or preparation sequences at the boundary.

Within reasonable detailed planning windows, such as one day to roughly one week, UbU may use a bias toward completing fragile prerequisite work as early as reasonably viable. This is intended to reduce last-minute impossible choices, especially when later interruptions, affect deterioration, external events, or user overrides would otherwise threaten a tight dependency chain.

This early-preparation bias is not unlimited. Beyond a reasonable detailed horizon, preparation may be too premature or speculative. Long-horizon preference and temporal-discounting questions remain separate from short-horizon operational planning. For short operational windows, UbU may treat time discounting as negligible by default, while still respecting explicit user Preferences and current affect.

---

## UBU-D0129: Phase 1 bootstrap and next-action UX uses explicit object-backed minimum loop

**Status:** Accepted → DESIGN.md §4

See DESIGN.md §4.

---

## UBU-D0130: UniverseState mutations use dotted targets and envelope-level provenance

**Status:** Accepted → DESIGN.md §11.3

MVP UniverseState mutation items use dotted string targets. The first target segment names the UniverseState collection: `facts`, `numeric_values`, `set_memberships`, or `event_markers`. Remaining segments form the lightweight namespaced key within that collection. Examples include `facts.github.issue.14.pipeline_state`, `numeric_values.affect.energy`, `set_memberships.github.issue.14.labels`, and `event_markers.relationship.rel_123.interactions`.

A mutation item has required `operation` and `target` fields. `payload` is required for all operations except `clear_fact`. `note` is optional for human-readable context. Mutation items do not carry item-level `confidence`, `source`, or `provenance` in MVP; those belong on the containing Task effect, Snapshot, Log entry, worker mutation request, External Reference, import artifact, or projection envelope.

Payload rules are operation-specific. `set_fact` accepts any JSON-compatible value. `clear_fact` has no payload. `increment_numeric` and `decrement_numeric` require numeric delta payloads. `add_membership` and `remove_membership` require JSON scalar member payloads, usually strings. `append_event_marker` requires a JSON object payload representing the lightweight marker.

Mutation lists are unconditional once the containing Task effect succeeds. Per-item conditions are not part of the MVP mutation schema; conditional behavior belongs in Task preconditions, effect success probability, planner branching, or worker/request validation. Implementations should validate the full mutation list before application and apply valid items in list order.

Mutation targets may include affect UniverseState keys in `user_mode`, subject to Snapshot precedence and user sovereignty rules. Organization-mode and worker-mode validators must reject intrinsic-affect mutations. Mutation targets may include Relationship-relevant UniverseState keys, including relationship interaction markers and maintenance facts, but they must respect Compartment policy, mode rules, provenance/logging envelopes, and user-acceptance requirements for private or inferred relationship claims. Task effects, workers, imports, and LLM-assisted flows must not silently overwrite user-declared private affect or Relationship truths.

---

## UBU-D0131: Task preconditions use recursive all_of/any_of predicates

**Status:** Accepted → DESIGN.md §10.1

MVP Task preconditions use a recursive boolean object with `all_of` and `any_of` arrays for simple AND/OR composition. A precondition node may be a group node or a leaf predicate. A group node contains `all_of` or `any_of`, each holding one or more precondition nodes. A leaf predicate contains `target`, `predicate`, and optional `expected`.

Precondition targets use the same dotted UniverseState target convention accepted for mutations. The first segment names the UniverseState collection: `facts`, `numeric_values`, `set_memberships`, or `event_markers`. Remaining segments form the lightweight namespaced key inside that collection.

MVP predicates are `equals`, `member_of`, and `absent`. `equals` compares the target value to a JSON-compatible `expected` value. `member_of` checks whether the JSON scalar `expected` value is present in the target set membership. `absent` checks that the target is not present or has been cleared, and does not use `expected`. Numeric comparisons such as greater-than, less-than, ranges, thresholds, and arithmetic expressions are not in MVP.

Preconditions may reference `event_markers` for deterministic marker presence or equality checks. Preconditions may reference affect-related UniverseState keys in `user_mode`, subject to Snapshot precedence and user sovereignty rules; organization-mode and worker-mode validators must reject intrinsic-affect preconditions. Preconditions may reference Relationship-relevant UniverseState keys, including relationship interaction markers and maintenance facts, but private or inferred relationship claims remain subject to Compartment policy, mode rules, provenance/logging envelopes, and user acceptance where required.

A failed precondition makes the Task blocked for planning and execution. It does not make the Task invalid. `unschedulable` is a derived planner result when an otherwise valid Task cannot be placed in the current Calendar scope while satisfying preconditions and Calendar Logic. `invalid` is reserved for malformed Tasks, malformed precondition schema, forbidden targets, or canonical-state contradictions. Unknown, unavailable, or partially modeled preconditions are treated as absent in MVP unless the user or importer explicitly records a deterministic predicate.

---

## UBU-D0132: Realtime multimodal LLMs are optional interaction backends, not authoritative planners

**Status:** Accepted → DESIGN.md §21

Realtime multimodal LLMs may power live voice, video, interruption detection, meeting capture, discovery mode, pronunciation or activity feedback, and short-horizon task monitoring. They are interaction backends and perceptual/extraction aids, not the authoritative UbU planner.

Realtime model output must enter UbU as structured, provenance-bearing candidate updates. Examples include `ObservedEvent`, `UserInterruption`, `TaskProgressDelta`, `AffectSignal`, `PlanDeviation`, `ExternalConditionChange`, `ClarificationQuestion`, `WorkItemCandidate`, `LogEntryCandidate`, and `AssociationAttestationCandidate`.

UbU distinguishes **model-time awareness** from **planner-time semantics**. A realtime model may notice elapsed time, silence, overlapping speech, interruption, or a changing audiovisual scene. UbU's planner remains responsible for deciding whether that observation changes Task state, Calendar validity, Plan legitimacy, Logs, Objectives, or recalculation triggers.

Realtime operation requires explicit user-visible modes, such as passive/off, text-only, voice session, active discovery mode, meeting/logging mode, high-privacy local-only mode, and cloud-assisted mode. Continuous capture must not become covert surveillance or an implied authorization to mutate canonical state.

---

## UBU-D0133: LLMs are replaceable cognitive backends and structured outputs are candidate updates

**Status:** Accepted → DESIGN.md §21

UbU should treat LLMs as replaceable cognitive backends. They may interpret, summarize, classify, translate, transcribe, propose structures, critique plans, call tools, generate UI drafts, and assist extraction. They do not own canonical truth, canonical value, canonical memory, or canonical planning state.

Strict schemas and structured outputs are necessary but not sufficient. A JSON object that satisfies a schema may still be semantically wrong, policy-invalid, Compartment-invalid, dependency-invalid, affect-invalid, stale, maliciously influenced, or unsupported by evidence.

The admission pipeline is therefore:

```text
LLM output
  -> schema validation
  -> semantic validation
  -> policy and Compartment validation
  -> provenance attachment
  -> conflict detection
```

---

## UBU-D0134: Context assembly is a privacy-relevant governed act

**Status:** Accepted

As LLM context windows grow, UbU must not assume that sending more context is automatically better. Context assembly is itself a privacy-relevant action that may cross Identity, Compartment, Association, provider, and retention boundaries.

A future `ContextBundle` should describe why context was assembled and what it exposed:

```text
ContextBundle
  - purpose
  - source object references
  - Compartments included
  - Identities exposed
  - Association references exposed
  - provider or model destination
```

---

## UBU-D0135: UbU should be both an MCP-style client and an MCP-style server with capability boundaries

**Status:** Accepted → DESIGN.md §21

UbU should support MCP-style integration boundaries in both directions.

As a client, UbU can connect to external tools and data sources such as GitHub, calendars, email, local scripts, file stores, home automation, and model/agent services.

As a server, UbU may expose narrow, scoped, reviewable affordances to outside agents, such as:

- create candidate Task;
- read a current Plan summary;
- append or submit a Log candidate;
- request user clarification;
- submit a proposed Plan repair;
- query Objective status;
- submit a candidate AssociationAttestation;

---

## UBU-D0136: Delegated agency is first-class in planning

**Status:** Accepted → DESIGN.md §21

UbU Tasks may be performed by the user, a local agent, a cloud agent, a tool, another human Identity, an Association, or a delegated coordinator. The executor is part of the planning problem and must not be hidden inside an opaque automation step.

A delegated Task should distinguish:

- Task intent;
- authorization source;
- executor;
- observer;
- reviewer;
- granted authority;
- expected output;
- completion evidence;

---

## UBU-D0137: Delegation Substrate is the near-term model for preparing Task delegation

**Status:** Accepted → DESIGN.md §21

Use **Delegation Substrate** as the near-term tool/model for preparing Task delegation. It is not the full Skill Barter marketplace. It is the explicit representation that makes a Task ready to be performed, reviewed, or handed off by clarifying purpose, executor, authority, expected output, evidence, constraints, and completion criteria.

The Delegation Substrate is also valuable for Tasks the user intends to perform solo. Formalizing the same fields can act as a self-reminder of how and why the Task needs to be performed, what evidence would show completion, what authority is being used, and what constraints matter.

A minimum Delegation Substrate packet may include:

```text
DelegationPacket
  - Task or Objective reference
  - intended executor type
  - purpose / why this matters
  - expected output
  - authority granted or self-authority note
```

---

## UBU-D0138: General Contractor is a first-class delegated coordination role

**Status:** Accepted → DESIGN.md §21

A **General Contractor** is an Identity, Agent, or Association delegated authority to coordinate multiple subordinate executors in order to satisfy an Objective or complete a Container of Tasks, subject to explicit authority, budget, privacy, review, evidence, and escalation constraints.

This role is distinct from an ordinary executor. A General Contractor may decompose work, assign subtasks, supervise human or agent executors, collect evidence, report status, and submit candidate updates. UbU must still model the General Contractor's authority as bounded and reviewable.

---

## UBU-D0139: Skill Barter marketplace is an outreach and future-market direction, not a Phase 1 marketplace commitment

**Status:** Accepted

The **Skill Barter marketplace** should be presented as a future marketplace direction and EthConf NYC outreach hook, especially for cypherpunk and privacy-oriented audiences. It signals that UbU preserves much of the original cryptocurrency ethos: voluntary coordination, sovereign identity, privacy, open markets, FOSS development, pseudonymous capability, and user-controlled settlement references where lawful.

Skill Barter can attract younger developers with drive, time, and interest in FOSS contribution by showing that UbU is not merely another productivity app. It is a coordination substrate that could eventually support privacy-preserving skilled-work exchange among human and agentic executors.

A mature Skill Barter marketplace would naturally create demand for FHE, ZK, secure compute, private reputation, private escrow-like commitments, selective disclosure, and other high-privacy technologies compatible with the future Ethereum ecosystem. This should remain an architectural and outreach signal, not a token-first or speculation-first positioning.

---

## UBU-D0140: Computer-use and background agents require authority, audit, rollback, and prompt-injection handling

**Status:** Accepted → DESIGN.md §21

Computer-use agents and background agents are high-risk external actors, not ordinary pure functions. They may operate browsers, accounts, files, credentials, APIs, and external systems where prompt injection, irreversible side effects, disclosure, or stale assumptions can cause harm.

Future `AgentAction` or `BackgroundProcess` models should record:

```text
AgentAction / BackgroundProcess
  - executor Identity or agent reference
  - trigger or schedule
  - authority scope
  - credentials or integrations used
  - Compartment and Identity scope
  - external surface touched
```

---

## UBU-D0141: Local/on-device inference is a first-class execution tier

**Status:** Accepted

UbU should treat local and on-device inference as a first-class execution tier, not merely a fallback for unavailable cloud models. Local inference supports privacy, offline use, lower marginal cost, responsiveness, and user sovereignty.

A provisional execution hierarchy is:

```text
Tier 0: deterministic local code
Tier 1: small local/on-device model
Tier 2: user-owned desktop or worker model
Tier 3: user-selected cloud model via BYOK
Tier 4: optional UbUCorp-managed hosted service
```

Local models may handle first-pass extraction, privacy classification, simple message triage, candidate Task/Log creation, affect journaling summaries, and compartment routing. Cloud or larger models may be reserved for high-complexity planning, deep design review, organizational introspection over large archives, difficult ambiguity resolution, and expensive multimodal reasoning when policy allows.

---

## UBU-D0142: UbU UX should evolve into a state-transition cockpit

**Status:** Accepted

UbU should not be framed as chat plus calendar. The long-term UX should become a **state-transition cockpit**: a user interface that presents the current WorkItem, relevant state, candidate transitions, constraints, explanations, evidence, and review controls.

The Phase 1 one-next-Task UX remains the narrow proof. Later interfaces may adapt the visible control surface to the current state transition:

- reply to a message;
- choose or start the next Task;
- review a Log discrepancy;
- repair a Plan conflict;
- inspect a Delegation Substrate packet;
- approve or reject an agent action;
- review an AssociationAttestation;
- run organizational introspection;

---

## UBU-D0143: Community-specific EthConf briefs are derived presentation layers

**Status:** Accepted

UbU should maintain a small set of community-specific derived documents for EthConf and adjacent outreach when the audience has a materially different trust barrier, motivation, or call to action.

The durable derived audience documents are:

- `README.md` for upcoming technical contributors and technically serious readers who need the project entry point;
- `OUTREACH.md` for FOSS maintainers and general software engineers who may become technical contributors;
- `PM_BRIEF.md` for project leads, technical PMs, protocol leads, release coordinators, and people coordinating autonomous contributors;
- `FUNDER_BRIEF.md` for grantmakers, sponsors, hackathon judges, aligned funders, and prototype funders;
- `SOVEREIGN_COORDINATION.md` for cypherpunks, privacy engineers, Ethereum privacy builders, FHE/ZK/secure-compute researchers, and sovereign-coordination audiences;
- `ORG_INTROSPECTION_BRIEF.md` for mission-driven projects, FOSS maintainers, nonprofits, DAOs, foundations, and teams that want evidence-backed mission alignment.

These files are presentation layers. They must not introduce new design authority. Their source of truth remains `DESIGN.md`, `DECISIONS.md`, and `OPEN_QUESTIONS.md`.

---

## UBU-D0144: Inter-instance protocol is a generic envelope family with worker API profiles

**Status:** Accepted

UbU should have one generic inter-instance protocol family for communication between UbU instances, worker-mode instances, and future compatible peers. The protocol is a family of typed envelopes with shared authority, provenance, Compartment, identity, idempotency, versioning, and review semantics, not one undifferentiated endpoint that treats every payload as the same kind of action.

The protocol should eventually support at least these payload families:

- contextual messages and Message Context Envelopes;
- worker assignments and assignment responses;
- status updates and check-ins;
- mutation requests and projection requests;
- capability grant issuance, acknowledgement, rotation, and revocation;
- External Event submission;
- Snapshot or Log candidate submission where authorized;
- recalculation requests;

---

## UBU-D0145: Phase 1 risk reports are derived artifacts, not canonical risk objects

**Status:** Accepted → DESIGN.md §28

See DESIGN.md §28.

---

## UBU-D0146: Phase 1 recalculation triggers use logged trigger records

**Status:** Accepted → DESIGN.md §29

See DESIGN.md §29.

---

## UBU-D0147: Moot reason codes are a closed MVP enum

**Status:** Accepted → DESIGN.md §9.5

See DESIGN.md §9.5.

---

## UBU-D0148: Task-to-Container mutation preserves Task identity as history

**Status:** Accepted → DESIGN.md §9.4

A Task does not literally change type into a Container by reusing its handle. Task-to-Container mutation is modeled as a structural replacement: the original Task remains an immutable historical Task record, a new Container is created with a new `container_id`, and the original Task transitions to `moot` when the restructuring replaces it for planning.

The Container records lineage back to the original Task through fields or metadata such as `origin_task_ref`, `mutation_reason`, `mutation_log_ref`, child WorkItem refs, and provenance. The original Task handle remains valid for Logs, External References, Plan history, projection history, and audit. It must not be reused as the Container handle.

All new child Tasks created during decomposition, preemption, retry, or worker expansion receive new Task handles. Existing Tasks may be grouped under the Container by reference when appropriate, but grouping does not rewrite their handles. A continuation child may inherit user-facing title context from the original Task, but it is still a new Task with its own lifecycle.

Intent-level fields stay on the Container or lineage metadata: title or summary, served Objective refs, parent or dependency context, external-reference lineage, Compartment/security labels, authority/provenance, and explanatory notes about why the work was split. Action-level fields belong on child Tasks: duration or duration PDF, preconditions, effects, executor/delegation fields, worker assignment/status, expected output, evidence requirements, child-specific dependencies, and child-specific deadlines.

Child Tasks may inherit or narrow Objective refs, Compartment refs, authority source, relevant External Reference context, and non-sensitive notes when those remain valid for the child. They do not silently inherit stale status, completion evidence, modeled effects, worker status, or projection state from the original Task.

The default original-Task lifecycle transition for decomposition or regrouping is `moot` with reason code `replaced_by_new_plan_structure`. Use narrower accepted moot codes when more accurate: `delegated` when responsibility moves out of the executor scope without decomposition, `superseded` when a newer Task, Objective, decision, or source artifact replaces the Task, and `duplicate` when another canonical work item already represents the same work.

External IDs are preserved through External References, not by reusing WorkItem handles. When an external object such as a GitHub Issue represents the whole unit of work, it should link to the Container. When the same external object supports or evidences a specific child Task, child-level External References may also be created with the appropriate relation type. The original Task's historical External References remain intact; new references express the replacement or child relationship rather than rewriting history.

For GitHub-linked Tasks, decomposition should keep the GitHub Issue or PR traceable to the Container and project only clearly marked child-level status when needed. GitHub projection and reconciliation use External References to decide whether the external object represents the Container, a child Task, or both.

Automation Worker child Tasks are ordinary child Tasks grouped under a Container, with worker/delegation metadata and capability boundaries. Workers may propose Task-to-Container restructuring only through authorized mutation requests. The canonical instance validates authority, expected prior version, Compartment/export policy, External Reference changes, and idempotency, then writes applied or rejected Log entries.

---

## UBU-D0149: Objective status transitions are mode-specific and logged

**Status:** Accepted → DESIGN.md §7.3

Objective status transitions are constrained by Objective mode. One-time Objectives and evergreen Objectives share the same status enum, but not every status is valid for every mode.

For one-time Objectives, valid statuses are `active`, `completed`, `abandoned`, `invalid`, and `superseded`. `satisfied` is not valid for one-time Objectives. Legal one-time transitions are:

```text
active -> completed
active -> abandoned
active -> invalid
active -> superseded
completed -> invalid
completed -> superseded
abandoned -> invalid
abandoned -> superseded
superseded -> invalid
```

`completed`, `abandoned`, and `superseded` are terminal for normal one-time planning. A completed one-time Objective does not reactivate. If the user later wants the same kind of outcome again, UbU should model that as a new Objective or as an explicit supersession, not as `completed -> active`.

For evergreen Objectives, valid statuses are `active`, `satisfied`, `abandoned`, `invalid`, and `superseded`. `completed` is not valid for evergreen Objectives. Legal evergreen transitions are:

```text
active -> satisfied
active -> abandoned
active -> invalid
active -> superseded
satisfied -> active
satisfied -> abandoned
satisfied -> invalid
satisfied -> superseded
abandoned -> invalid
abandoned -> superseded
superseded -> invalid
```

`abandoned` and `superseded` are terminal for normal evergreen planning. If an abandoned or superseded evergreen concern later becomes relevant again, UbU should usually create or select a new Objective rather than silently reactivating the old one.

`invalid` may occur from any Objective status and is terminal. It means the Objective record should not participate in normal planning because it is malformed, impossible, contradictory, forbidden by mode rules, or admitted by mistake. `invalid` is not a user preference to stop pursuing an otherwise valid Objective; that is `abandoned`.

`superseded` may occur from any non-`invalid` Objective status. It means a newer Objective, decision, import result, or source artifact has replaced the Objective for planning and traceability. Supersession should preserve lineage to the replacement Objective or source when known.

`abandoned` does not occur from any state. It is legal only from `active` one-time Objectives and from `active` or `satisfied` evergreen Objectives. It represents a user-authoritative or authority-source-authorized decision to stop pursuing an otherwise valid Objective.

Evergreen `satisfied -> active` reactivation is a canonical transition only when the accepted current state changes through user declaration, authorized observation/import, or elapsed-time recurrence evaluation. Hypothetical Plan simulation may predict that an evergreen Objective will become active in a future branch, but simulation alone does not mutate canonical Objective status.

Every accepted canonical Objective status transition creates an append-only Log entry with event type `objective_transitioned`, including old status, new status, actor or authority source, reason, effective time, and provenance when available. Simulated status changes inside candidate Plans or risk reports are predictions, not canonical transitions, and do not create `objective_transitioned` Logs unless accepted as actual state.

---

## UBU-D0150: Model-committee v0.2 adopts schema-native Claude Code cross-scoring

**Status:** Accepted → DESIGN.md §3

`model-committee v0.2` extends the v0.1 bootstrap loop by adding Claude Code CLI as a second frontier provider for both proposal generation and scoring.

This is a contract-level version change, not a small provider addition. v0.1 remains the narrow baseline, but v0.2 updates the dogfooding architecture to make disagreement between independent frontier providers visible, reviewable, and useful.

v0.2 scope includes:

- Claude Code CLI as a second frontier work and score provider;
- schema-native Claude Code structured output using `--json-schema`;
- cross-scoring between frontier providers;
- a first-class score matrix in run manifests and review artifacts;
- disagreement flags in `review.md`;
- a Claude Code config block;
- updated quorum rules;
- `doctor` checks for Claude availability and structured-output support;
- final operator-run artifact-publication instructions targeting `../model-committee-artifacts`.

v0.2 remains out of scope for:

- multi-turn Claude Code sessions;
- GitHub API integration;
- adaptive model weights;
- full Association automation;
- UbU planning-kernel work;
- automatic artifact push, merge, PR creation, or canonical design-state mutation.

Claude Code should be invoked as a subprocess provider, not through direct Anthropic API calls made by `model-committee` itself. The provider/network policy distinction is explicit:

- `model-committee` must not directly call Anthropic APIs;
- `model-committee` may invoke approved external CLI subprocesses when explicitly enabled by configuration;
- those provider CLIs may perform their own network/API calls according to their upstream authentication, billing, and policy configuration;
- every provider invocation must be logged with provider ID, model name or alias, argv shape, timeout, exit code, stdout/stderr artifact paths, schema-validation result, and relevant usage/cost metadata when available.

The installed Claude Code CLI version for the v0.2 target environment is confirmed as `2.1.146`, and that version supports `--json-schema`. Therefore Claude Code should use schema-native structured output as the primary path rather than Ollama-style JSON extraction.

The Claude Code provider should parse validated structured output from the CLI JSON envelope. When `--output-format json` and `--json-schema` are used, the schema-conforming payload should be read from `structured_output` rather than from free-form text. Prompt-embedded JSON extraction is only a compatibility fallback if a future environment lacks working schema-native output.

Claude tool authority must be restricted explicitly. For scripted schema-output runs, the default should be no tools or the narrowest necessary tool set. If file inspection is needed, use `--tools Read`. Do not rely on `--allowedTools` alone as a sandbox boundary, because allowing a tool without prompting is not the same as restricting the available tool set.

Cross-scoring rules:

- Codex scores Claude-authored proposals.
- Claude Code scores Codex-authored proposals.
- A provider's self-score may be retained as diagnostic metadata, but it does not count as quorum evidence.
- Local/Ollama providers remain useful for diversity, dissent, fallback, and offline review, but they do not replace the required frontier cross-score unless a later decision expands quorum policy.
- Score results should be stored as matrix entries with `proposal_id`, `author_provider`, `scorer_provider`, score, validity, rationale, risks, and required fixes.

v0.2 automated selection requires:

- at least one valid work proposal;
- at least one valid cross-score from a different frontier provider;
- no hard validation failures on the selected patch;
- no critical disagreement flag unless an explicit manual override mechanism is used outside automatic selection.

Default disagreement and review thresholds:

- frontier score gap of 25 or more points: human review required;
- selected score below 70: human review required;
- any selected patch validation failure: no selection;
- no valid cross-score from a different frontier provider: no automated selection.

v0.2 should add a distinct human-review-required exit code, provisionally `9`, for quorum or disagreement outcomes that are not the same failure mode as v0.1's invalid selected patch exit code `7`.

The generated `review.md` should include a final operator step to publish the run artifact to the sibling artifact repository:

```bash
RUN_ID="<run-id>"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```

These commands are instructions for a human operator or separately authorized release process. `model-committee v0.2` should generate them, not execute them automatically.

---

## UBU-D0151: Compact Calendar MVP grammar uses skeleton, legitimization, bounded candidates, and repair metadata

**Status:** Accepted → DESIGN.md §§15.2.2, 16

See DESIGN.md §§15.2.2, 16.

---

## UBU-D0152: Extrospection is first-class Relationship review

**Status:** Accepted → DESIGN.md §§2.10.1, 22

Extrospection is a first-class Relationship review feature. It applies the evidence-backed attestation pattern used by introspection and AssociationAttestation to the user's Relationship model.

Relationships have **epistemic asymmetry**: the user has first-person access to the user's own declarations and experiences, while UbU can only maintain evidence-backed hypotheses about another party's perspective. Extrospection must therefore target the user's counterparty-perspective hypotheses, Relationship scopes, trust assumptions, reciprocity assumptions, affective impact, and functionality assumptions. It must not claim authoritative access to another person's inner state.

Extrospection should present evidence in tension rather than deliver an oracle verdict. It may surface confirming evidence, disconfirming evidence, ambiguity, insufficient evidence, clarification prompts, possible model updates, and introspection handoffs. The goal is evidence-backed confrontation and self-governance, not counterparty prosecution.

---

## UBU-D0153: Relationship perspectives, extrospection findings, and evidence policy are structured and reviewable

**Status:** Accepted → DESIGN.md §§22.1, 22.2

The richer Relationship model should lazily decompose the user's side into:

- `ownDeclaredPerspectiveHistory`;
- `ownObservedBehaviorRefs`;
- `ownAffectHistoryRefs`;
- `ownReflections`.

User declarations about a Relationship are versioned over time, not overwritten as a single timeless field.

Counterparty perspective should be represented as domain- and scope-tagged `counterpartyPerspectiveHypotheses`, not as a monolithic `speculatedPerspective`. UbU may detect internal tension among those hypotheses as a finding about the user's model consistency, not as a claim about the counterparty.

Extrospection assessments should be separated into `ScopeAssessment`, `BoundaryAssessment`, `ReciprocityAssessment`, `TrustCalibrationAssessment`, `AffectiveAssessment`, and `FunctionalityAssessment`. `ScopeAssessment` logically precedes the others. Negative affect is not equivalent to dysfunction, and reviewed value-aligned discomfort may be recorded as `userAcceptedDiscomfort`.

Extrospection findings use the lifecycle `candidate`, `deferred`, `resurfaced`, `reviewed_accepted`, `reviewed_rejected`, `superseded`, and `archived`. Unreviewed findings must not silently update durable Relationship state. Rejected findings should be retained enough to suppress repeated bad framings.

Durable evidence should prefer Compartment-aware `EvidenceRef`s, selectors, summaries, typed retention policy, typed redaction policy, and EvidenceUsePolicy over copied private excerpts. Cross-Relationship inference is prohibited by default. Evidence should decay in salience unless reactivated by new evidence, user review, or a current Objective.

---

## UBU-D0154: RelationshipScopeTransition is an Objective target, not a separate planning system

**Status:** Accepted → DESIGN.md §§7.5, 22.3

Users may explicitly create Objectives to change their own participation in a Relationship, invite or test a mutual scope change, or clarify whether another party is willing to enter a different scope.

`RelationshipScopeTransition` is a semantic target/reference for such Objectives. It is implemented through ordinary Objectives, Techniques, Steps, Tasks, Logs, extrospection reviews, introspection reviews, and UniverseState mutations. UbU should not create a parallel relationship-planning system.

The Relationship object remains primarily descriptive, but it keeps reverse references to active, completed, abandoned, declined, or superseded RelationshipScopeTransitions that affected it. This prevents declaration history from appearing to change spontaneously when a scope change resulted from a deliberate Objective.

Valid RelationshipScopeTransition work targets user-controlled behavior: disclosures, invitations, boundary-setting, availability changes, communication changes, and clarification attempts. Counterparty feelings, decisions, and Identity remain autonomous and uncertain.

---

## UBU-D0155: RelationshipScopeTransition separates process success from outcome and blocks optimized persuasion semantics

**Status:** Accepted → DESIGN.md §§7.5, 22.3

Consent-dependent and mutual RelationshipScopeTransitions must separate `process_success_criteria` from `outcome_observations`. A transition attempt can be process-successful even when the counterparty declines. Counterparty response is evidence, not the user's success or failure.

The `transition_type` enum is provisionally:

- `unilateral`;
- `exploratory`;
- `mutual`;
- `retroactive_scope_clarification`.

All actionable Steps and Tasks must be anchored to user-controlled behavior. A Step is malformed if its completion criterion requires a counterparty mental state, feeling, or decision.

RelationshipScopeTransition Techniques should satisfy epistemic transparency: the counterparty should receive accurate information sufficient for autonomous decision-making. Exploratory transitions follow a single-iteration rule: one disclosure or proposal, one clarification if ambiguous, and termination of the current Objective on authentic refusal. Future reopening requires materially new evidence, a new Objective, and stronger review.

The prohibited-strategy taxonomy is typed and includes artificial urgency, false scarcity, vulnerability exploitation, incremental boundary erosion, strategic information withholding, emotional state manipulation, leveraging privileged affect knowledge, manufactured dependency, social pressure through third parties, retaliation or threat, coercive leverage, deceptive self-presentation, and repeated pursuit after refusal.

The affect-knowledge firewall is canonical: extrospection-derived affect knowledge may be used for harm avoidance, not persuasion optimization. Romantic, professional, dependency-heavy, and power-asymmetric transitions may carry stronger default advisory safeguards, including power/vulnerability checks, pacing constraints, and graceful nonachievement paths.

---

## UBU-D0156: Relationship safeguards are default-on advisory unless structural boundaries apply

**Status:** Accepted → DESIGN.md §§2.10.2, 22.4, 23.3

Relationship-transition ethical reasoning, extrospection checks, introspection checks, manipulation-risk review, TrustCalibrationAssessment, power/vulnerability checks, pacing checks, and graceful nonachievement planning are wise default-on safeguards. Users should generally use them. However, UbU's prime directive is to respect the user's autonomy and decisions, so these behavioral-risk safeguards must not be unconditional hard gates by default.

Hard boundaries should be limited to structurally enforceable product invariants and explicit policies, including:

- Compartment boundaries;
- privacy policy;
- data-access authorization;
- EvidenceUsePolicy restrictions;
- export prohibitions;
- identity/Compartment isolation;
- provenance integrity;
- audit-record integrity;
- integration authorization;
- unavoidable provider/platform/legal constraints.

Behavioral-risk checks such as manipulation, coercion, harassment, stalking, deception, romantic/professional power asymmetry, rumination, and relationship-transition ethics depend on fallible classification. Treating them as hard gates risks false positives that block legitimate actions, false negatives that create misplaced trust, and a false impression that UbU can reliably prevent misuse or illegal behavior.

Behavioral safeguards should generally be default-on, user-aware, uncertainty-transparent, user-overrideable advisory checks with introspection consequences when bypassed. A user may configure some advisory checks as self-imposed required gates. Bypassing or disabling recommended safeguards is itself introspection-relevant evidence about revealed priorities and possible conflicts with declared values.

---

## UBU-D0157: LLM provider routing uses provider descriptors and policy-gated route decisions

**Status:** Accepted → DESIGN.md §2.11

See DESIGN.md §2.11.

---

## UBU-D0158: Phase 1 worker work discovery uses explicit assignments

**Status:** Accepted → DESIGN.md §25.1.2

See DESIGN.md §25.1.2.

---

## UBU-D0159: GitHub projection writes only managed surfaces and reconciles drift

**Status:** Accepted → DESIGN.md §27

See DESIGN.md §27.

---

## UBU-D0160: Model-committee run logs preserve reproducible provenance

**Status:** Accepted → DESIGN.md §3

See DESIGN.md §3.

---

## UBU-D0161: Model-committee work proposals are validated patch artifacts

**Status:** Accepted → DESIGN.md §3.2

Resolved question: `UBU-Q0038`.

The `model-committee` work phase represents implementation as explicit patch artifacts, not hidden editor state or direct provider edits.

A work proposal is a schema-constrained envelope with at least: proposal ID, provider ID, model name, selected question or problem ID, base commit, summary, rationale, changed-file list, raw unified diff, suggested commit message, validation notes, declared new questions, declared resolved questions, declared decisions, and a human-review-required flag. The raw unified diff is the proposed state transition. Summaries, rationales, and commit messages help review, but they do not replace the patch.

Before work scoring, `model-committee` validates proposal JSON against the active schema, confirms the proposal targets the selected question or work item and expected base commit, checks changed files against the work item's allowlist, parses the unified diff, verifies that the patch applies to the recorded base snapshot, rejects forbidden paths or generated/private artifacts, verifies referenced question and decision IDs, and runs work-item-specific semantic checks when available. For v0.1 design work, the writable set is only `DESIGN.md`, `DECISIONS.md`, and `OPEN_QUESTIONS.md`. Later code, schema, fixture, and bug-fix work uses the same proposal envelope but supplies a different allowlist, validators, tests, and expected review artifacts.

Mechanically invalid proposals are retained in run logs with diagnostics, but they are not eligible for automatic selection and do not count as valid work proposals for quorum. The scorer may be shown invalid-proposal diagnostics for critique, but selection must choose only from mechanically valid proposals.

Work scoring evaluates at least: correctness against the selected question or problem; consistency with accepted design decisions and authority boundaries; mechanical validity; minimality and focus; reviewability of the patch and commit message; risk, reversibility, and blast radius; validation or test adequacy; maintainability and generality beyond the immediate prompt; and whether new questions or decisions are warranted rather than silently expanding scope. Score records include score, validity judgment, rationale, risks, required fixes, and validation assumptions.

Models may score their own proposals only for diagnostic metadata. Self-scores do not count as quorum evidence and must be labeled non-quorum. In v0.1, Codex is the required scoring provider for valid Codex and Ollama work proposals, and Ollama proposals are included only after they pass the same schema and patch validation. In v0.2, Codex and Claude Code cross-score each other's frontier proposals; local/Ollama providers remain useful for diversity, dissent, fallback, and offline review but do not replace the required frontier cross-score unless a later decision changes quorum policy.

If a scorer selects a proposal that is later discovered to be mechanically invalid, automatic selection fails. The run must not silently apply or commit that patch. It writes review artifacts, validation diagnostics, and the invalid-selected-patch failure result. Choosing another proposal requires a valid score/selection record over the remaining mechanically valid proposals or a new run. v0.2 quorum, score-threshold, and disagreement failures use the human-review-required path rather than being treated as successful automatic selection.

`model-committee` v0.1 and v0.2 do not automatically apply patches to canonical files, create local commits, push artifacts, open pull requests, or mutate GitHub. A human operator may apply `selected.patch`, inspect `review.md`, run the relevant validation commands, and commit normally using `commit_message.txt` as a suggestion. Automatic local commit remains out of scope until a later accepted decision defines explicit approval, clean-worktree, validation, rollback, and authority rules.

**Consequences:**

- `UBU-Q0038` is resolved for the v0.1/v0.2 work-phase contract.
- `UBU-D0063`, `UBU-D0069`, `UBU-D0070`, `UBU-D0150`, and `UBU-D0160` remain compatible; this decision fills in the proposal, validation, scoring, invalid-selection, and commit-boundary details.
- The work phase generalizes from design-document patches to code changes and bug fixes through work-item-specific file allowlists, validators, tests, and review artifacts rather than through a different provider authority model.
- Any future automatic patch application or local commit feature requires a separate accepted decision.

---

## UBU-D0162: ContextBundles govern long-context exposure

**Status:** Accepted → DESIGN.md §21.3

Resolved question: `UBU-Q0083`.

A `ContextBundle` is the governed package of context assembled for a model, tool, worker, agent, or long-context review. It is not a transient prompt string or implementation detail. It is the auditable boundary between UbU's explicit state and an execution backend.

The minimum `ContextBundle` records `context_bundle_id`, `schema_version`, purpose, requested operation, actor Identity, related Task/Objective/workflow refs, route request and decision refs when used for LLM routing, destination provider/tool/worker/model refs, provider class and boundary classification, source refs, source selectors or ranges, source snapshot refs or hashes, Compartment refs, Compartment policy results, Identity refs exposed, Association refs exposed, Relationship refs exposed when applicable, data categories exposed, sensitivity summary, token or size estimate, redaction policy, minimization rules applied, excluded-source summary, prompt-injection exposure summary when untrusted external text is included, retention policy, training or secondary-use policy, user-visible summary, approval state, approval Log ref when applicable, creation and expiry times, invocation refs, downstream candidate refs, exposure summary ref, and Log refs.

Raw payload is allowed only when required by the task and allowed by policy. The preferred representation is references, selectors, summaries, hashes, redacted excerpts, and structural fields. Repository-scale, chat-archive-scale, or organizational-introspection bundles must record the selected paths, objects, time ranges, messages, or evidence selectors and also summarize excluded material. A broad archive name without selectors is not enough provenance for review.

Compartment policy is checked before context assembly and again before invocation. If any source is denied for the destination, UbU must either exclude that source and record the exclusion or deny the bundle. Mixed-Compartment bundles inherit the most restrictive applicable route. User approval, provider settings, workflow rules, and capability grants cannot override `no_cloud_llm`, `no_external_export`, `local_only`, or other hard Compartment denials.

User approval is required before routing a ContextBundle when protected payload, low-security un-compartmented content beyond structural references, Relationship evidence, Association evidence, private messages, broad repository or archive content, or other long-context bulk material leaves the local boundary, crosses to a cloud provider, external provider, remote worker, managed gateway, third-party tool, or crosses a materially different Identity or Association disclosure boundary. Approval is also required when provider, model, operator, region, retention/training profile, data category, minimization policy, source scope, or destination changes materially from an already approved workflow template.

Policy-based approval is allowed only for prior user-approved rules that name the workflow, destination class, allowed Compartments or data categories, maximum source scope, minimization rule, retention profile, cost limits when relevant, expiry, and review requirement. Such rules must fail closed on hard Compartment denials or materially broader context than the rule allowed. Local-only deterministic processing or local model use may proceed without per-run approval when local policy permits it and no new disclosure boundary is crossed.

After invocation, UbU records a context exposure summary as a review artifact. The summary includes actual provider/model/tool, invocation refs, source counts and categories, Compartments, Identities, Associations, and Relationships exposed, raw-versus-summary/reference volume, redactions and exclusions, retention/training profile, prompt-injection exposure notes, approval and routing Log refs, downstream candidate refs, and denied or omitted context that may affect output quality. Exposure summaries must not contain secrets or unnecessary raw payload.

ContextBundles are immutable after use. Corrections, narrower reruns, broader reruns, or provider changes create a new bundle or superseding revision linked by Logs. Candidate updates, AssociationAttestations, Log candidates, Plan repairs, Delegation Substrate packets, worker mutation requests, and other downstream outputs must carry originating ContextBundle refs or equivalent provenance. Approval, denial, invocation, output admission, and output rejection Logs link back to the relevant ContextBundle.

**Consequences:**

- Long-context model use can proceed only through explicit minimization, approval, routing, exposure-summary, and provenance records.
- Organizational introspection and repository/chat archive review can use large models without treating large context windows as permission to send everything.
- `UBU-Q0092` can refine typed Relationship evidence policies while relying on the ContextBundle boundary for model routing.
- `UBU-Q0100` can distinguish ContextBundle Compartment/export denials as hard structural boundaries rather than advisory safeguards.

---

## UBU-D0163: GitHub event triage is normalized, idempotent, and task-oriented

**Status:** Accepted → DESIGN.md §27.5

See DESIGN.md §27.5.

---

## UBU-D0164: Worker mutation requests are candidate envelopes with versioned admission

**Status:** Accepted → DESIGN.md §§17.5, 24.1.3

See DESIGN.md §§17.5, 24.1.3.

---

## UBU-D0165: Safeguards distinguish hard boundaries from advisory behavioral risk

**Status:** Accepted → DESIGN.md §§2.10.2, 22.4

See DESIGN.md §§2.10.2, 22.4.

---

## UBU-D0166: GPU planning kernel stochastic model

**Status:** Accepted → DESIGN.md §16.10; PLANNING_KERNEL_CONTRACT.md §§3, 7

See DESIGN.md §16.10.

---

## UBU-D0167: GPU planning kernel affect constraint architecture

**Status:** Accepted → DESIGN.md §§13.7, 16.10; PLANNING_KERNEL_CONTRACT.md §6

See DESIGN.md §§13.7, 16.10.

---

## UBU-D0168: GPU engine internal architecture

**Status:** Accepted → DESIGN.md §16.10; PLANNING_KERNEL_CONTRACT.md §5

See DESIGN.md §16.10.

---

## UBU-D0169: CPU/GPU interface contract

**Status:** Accepted → DESIGN.md §16.10; PLANNING_KERNEL_CONTRACT.md §§1-4

See DESIGN.md §16.10.

---

## UBU-D0170: PlanningRequest and PlanningResponse Phase 1 schema

**Status:** Accepted → PLANNING_KERNEL_CONTRACT.md §§1-4

`PlanningRequest` and `PlanningResponse` are now specified as the Phase 1 planning-kernel boundary. The contract includes explicit schema versioning, planner versioning, request IDs, effective time, generation time, reproducible RNG seed, time-window policy, one-minute full-detail planning delta default, one-hour reactive horizon default, `0.99` short-horizon branch coverage target default, compute budget, task graph with CPU-provided `topological_order`, UniverseState snapshot, AffectProfile, scoring policy, constraint policy, payload policy summary, payload-safety proof, optional external event assumptions, optional repair context, optional explanation request, and optional debug flags.

`PlanningResponse` must return ranked PlanCandidates plus diagnostics rather than only a schedule. Diagnostics include candidate counts by stage, rejection counts by reason, warnings, probability-quality classification, optional coverage estimate, optional compute telemetry such as `duration_ms`, and stage funnel counts such as `n_skeleton_candidates`, `n_after_affect_filter`, `n_after_value_scoring`, and `n_finalists_rollout`.

`effective_time` is the logical planning time and may differ from `generated_at` for replay, fixture testing, or repair from a recorded Log point. `affect_constraint_mode: warn_only` is an explicit operational mode for test fixtures, onboarding, debugging, or missing AffectProfile situations; it is not silent threshold relaxation. User-facing contexts default to `enforce`.

**Consequences:**

- Implementation work can begin against this schema without waiting for mobile, cloud, realtime, or multi-user payload fields.
- Future schema changes must preserve replayability through explicit versioning.
- Privacy compliance remains represented by both a compact policy summary and a CPU-generated payload-safety proof, not by a single human-readable flag.

---

## UBU-D0171: GPU pipeline stage-boundary contract

**Status:** Accepted → PLANNING_KERNEL_CONTRACT.md §5

The four GPU pipeline stages have Phase 1 semantic input/output boundaries: `skeleton_sampling`, `affect_legitimacy_filter`, `value_scoring`, and `monte_carlo_rollout`.

The design contract specifies the semantic data each stage consumes and produces. Exact tensor dtypes, device placement, batching mechanics, PyTorch module layout, and implementation classes belong in `model-committee`. This prevents `ubu-design` from becoming an implementation-code repository while still giving implementation generators enough structure to proceed.

Stage 1 consumes a CPU-provided `topological_order`; the GPU engine does not discover graph order. Stage 4 uses deterministic rollout seed derivation from the request seed. The Phase 1 default rollout budget is `n_rollouts = 1000` per finalist unless overridden by the CPU kernel's compute budget.

`PLANNING_KERNEL_CONTRACT.md` includes a Phase 1 recommended tensor profile using implementation-facing names such as `start_time_offsets`, `validity_mask`, `feasible_mask`, `surviving_indices`, `feasibility_scores`, `composite_scores`, `top_k_indices`, and probability intervals. These names guide implementation and tests without turning `ubu-design` into the PyTorch source tree.

**Consequences:**

- The stage names, semantic boundaries, CPU-provided topological order requirement, and deterministic seed-stream convention are canonical.
- Implementation details may vary as long as they preserve the contract and replayable output semantics.

---

## UBU-D0172: Correlation-group matrix construction and PSD handling

**Status:** Accepted → PLANNING_KERNEL_CONTRACT.md §7

Phase 1 correlation groups use positive latent-factor loading construction. `strength` is constrained to `[0, 1]`. Negative correlations, signed loadings, learned correlations, and richer latent-factor models are deferred beyond Phase 1.

This is not a philosophical rejection of negative correlations. Anti-correlations are mathematically valid and planning-relevant, but arbitrary signed pairwise-correlation declarations are likely to produce non-PSD matrices and confusing user-facing validation failures. Phase 1 therefore uses a deterministic PSD-by-construction model and records signed/negative correlation support as Phase 2 work, likely through signed latent factors, explicit Cholesky-style parameterization, or a signed partial-correlation model.

Each Task's group-strength vector is normalized when its squared norm exceeds `0.95^2`, preserving relative declared strengths while reserving residual idiosyncratic variance. The matrix is constructed as `C = L * L^T + diag(1 - row_norm(L)^2)`. This yields a symmetric unit-diagonal positive semi-definite matrix by construction up to floating-point error. Multiple shared groups combine through the dot product of normalized loading vectors.

CPU validation checks finite values, duplicate group declarations, range constraints, symmetry, unit diagonal, and factorization. Numeric jitter may be applied for floating-point error with explicit degraded diagnostics. Silent nearest-PSD projection is not a Phase 1 semantic repair. If factorization still fails after jitter, the CPU kernel either rejects the request or runs an explicitly degraded independent rollout with `probability_quality = degraded_independence`, depending on caller strictness and visible diagnostics.

**Consequences:**

- The MVP planner supports positive shared-delay correlation while avoiding arbitrary matrix repair.
- Degraded independent rollout is allowed only as an explicit diagnostic fallback, not as silent certainty.
- Negative-correlation support remains a recorded roadmap item rather than an indefinite deferral.

---

## UBU-D0173: Sigmoid affect constraint semantics and bootstrap UX

**Status:** Accepted → PLANNING_KERNEL_CONTRACT.md §6

Sigmoid affect outputs are constraint-satisfaction scores in `[0, 1]`. They are not literal success probabilities, not durable user preferences, and not canonical utility. Each active affect dimension has `dimension`, `direction`, `location`, `scale`, `threshold`, and optional freshness metadata.

Phase 1 directions are: energy is `higher_is_better`; stress is `lower_is_better`; mood intensity is `lower_is_better` and means affective arousal/volatility intensity, not mood valence. Positive excitement and negative agitation can both consume planning capacity, so Phase 1 treats high intensity as constraining.

This `mood_intensity` choice is a deliberate Phase 1 simplification. The longer-term design treats arousal/activation as task-dependent: routine tasks may favor low arousal, while creative, social, or physical tasks may require a higher optimal range. Phase 2 should add a `bounded_optimum` direction with `optimal_low` and `optimal_high` fields plus task/category affect overrides.

Candidate affect feasibility requires satisfaction greater than or equal to the active threshold for each active dimension at the CPU-selected evaluation point or points. Missing or stale affect observations must be surfaced through warning, quick check-in, bootstrap defaults, or `warn_only` mode; stale assumptions must not be silently presented as current measured state.

Phase 1 UX must not require ordinary users to edit raw sigmoid parameters. The UI should ask qualitative calibration questions and map answers into sigmoid parameters. Conservative bootstrap defaults are allowed and marked for review: energy `location = 4.0`, `scale = 1.5`, `threshold = 0.5`; stress `location = 7.0`, `scale = 1.5`, `threshold = 0.5`; mood intensity `location = 8.0`, `scale = 1.5`, `threshold = 0.5`. Advanced users may inspect and edit raw sigmoid parameters.

**Consequences:**

- Raw sigmoid configuration is no longer a normal-user prerequisite for Phase 1 onboarding.
- The planning kernel can implement `affect_legitimacy_filter` without resolving richer affect inference or cross-dimension models.
- The bounded-optimum arousal model with task/category overrides is explicitly roadmapped for Phase 2, not abandoned.

---

## UBU-D0174: Shifted log-normal duration semantics and invalid triple handling

**Status:** Accepted → PLANNING_KERNEL_CONTRACT.md §3

Phase 1 stochastic Task durations use `shifted_lognormal_p95` with fields `min_seconds`, `mode_seconds`, and `p95_seconds`. All duration fields use seconds for consistency with `planning_delta_seconds` and all other planning-kernel time fields.

- `min_seconds` is the optimistic lower support shift. It is used as the mathematical lower support for sampling. It is not a claim that real-world durations physically cannot be shorter; it is a modeling prior representing the user's subjective best-case estimate.
- `mode_seconds` is the most likely duration.
- `p95_seconds` is the 95th percentile and not a hard upper cap. The planner may sample durations greater than `p95_seconds`.

The shifted distribution is `D = min_seconds + LogNormal(mu, sigma)`. The canonical conversion is:

```text
a   = mode_seconds - min_seconds
b   = p95_seconds  - min_seconds
z95 = 1.6448536269514722

sigma = (-z95 + sqrt(z95^2 + 4 * ln(b / a))) / 2
mu    = ln(a) + sigma^2
```

Invalid stochastic triples (`min_seconds >= mode_seconds` or `mode_seconds >= p95_seconds`) are rejected at schema validation time. The schema must not silently repair invalid triples. A UI may offer to convert `min = mode = p95` to a fixed-duration model. Fixed known durations use `{type: "fixed", seconds: N}` as a delta distribution; they must not be represented as a very tight log-normal.

Validation occurs twice: structurally at `TaskSpec` construction and again when the CPU builds `PlanningRequest` tensors. Errors should identify the Task ID and violated ordering or unit constraint.

The explicit `p95_seconds` field name replaces the ambiguous `max` field name used in prior design drafts.

**Consequences:**

- Duration semantics are deterministic and replayable.
- `p95_seconds` naming removes prior ambiguity about whether `max` was P90, P95, P99, or a hard cap.
- Seconds-unit consistency eliminates unit-conversion bugs at stage boundaries.

---

## UBU-D0175: Phase 1 design automation stop rule

**Status:** Accepted

Broad pre-MVP design automation stops after resolution of the Phase 1 planning-kernel blockers `UBU-Q0102` through `UBU-Q0106`.

Future design work may block Phase 1 only when it is required to implement the single-user GitHub dogfooding loop, enforce an accepted hard invariant, specify a contract needed by currently planned code, or avoid a known irreversible schema contradiction.

All other open questions must be deferred, converted into implementation tickets, answered as post-MVP design work, or represented by conservative defaults, TODOs, feature flags, or versioned placeholder fields.

`model-committee` may still analyze design questions and propose patches, but it must no longer create new MVP blockers unless the blocker is discovered during implementation of a concrete Phase 1 slice and includes a blocker certificate:

- the exact implementation object, file, or acceptance test blocked;
- the failed acceptance criterion;
- why a conservative default, TODO, feature flag, or placeholder is unsafe;
- the minimum answer needed to unblock implementation;
- whether the answer is expected to mutate persistent schema or only local code behavior.

A new MVP blocker should normally replace or narrow an existing blocker, not expand Phase 1 scope. If a proposed blocker is philosophical, outreach-oriented, Phase 2/3-oriented, or merely improves completeness, it must not block Phase 1.

Implementation should now proceed slice-by-slice. A slice may begin when no unresolved question blocks that slice. Phase 1 readiness is therefore evaluated by implementation-slice readiness, not by global philosophical completion of the design model.

**Consequences:**

- Broad design expansion no longer blocks Phase 1 implementation.
- Implementation-local design clarification remains allowed when backed by a blocker certificate.
- `model-committee` transitions from pre-MVP design-freeze assistance toward implementation-support dogfooding.
- Future open questions may remain in `OPEN_QUESTIONS.md` without preventing Phase 1 implementation unless they meet the D0175 blocker standard.

---

## UBU-D0176: PLANNING_KERNEL_CONTRACT.md is a canonical source file for model-committee runs

**Status:** Accepted

Supersedes the file list in UBU-D0056.

The canonical source files for model-committee question-answering and work-proposal generation are:

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`
- `PLANNING_KERNEL_CONTRACT.md`

`PLANNING_KERNEL_CONTRACT.md` is a Phase 1 design artifact referenced by UBU-D0169 through UBU-D0174. It defines the CPU/GPU planning-kernel boundary contract including `PlanningRequest`, `PlanningResponse`, `TaskSpec` duration semantics, GPU pipeline stage boundaries, sigmoid affect constraint schema, and correlation matrix construction. Model-committee runs must read it as context and may propose patches to it subject to the same patch-validation rules as the other canonical source files.

---

## UBU-D0177: Phase 1 MCP tools are capability-scoped candidate surfaces

**Status:** Accepted → DESIGN.md §21.4

See DESIGN.md §21.4.

---

## UBU-D0178: Model-committee consistency gates prioritization and ordinary work

**Status:** Accepted → DESIGN.md §3.3

Resolved question: `UBU-Q0039`.

`model-committee` enforces the loop accepted in `UBU-D0066` with three priority levels:

1. system-wide consistency;
2. question/problem prioritization;
3. ordinary work.

Each run records loop state: base commit, canonical input hashes, consistency status, blocking failure IDs, warning IDs, selected loop mode, selected work item, provider/version baseline, and whether derived scores were reused or invalidated.

System-wide consistency checks run before prioritization and ordinary work. They are triggered by:

- new base commit, merge, rebase, checkout, or human-applied patch affecting canonical files;
- directive decision or other direct edit to `DECISIONS.md`;
- edits to `DESIGN.md`, `DECISIONS.md`, `OPEN_QUESTIONS.md`, or `PLANNING_KERNEL_CONTRACT.md`;
- prompt, schema, validator, quorum, provider-weight, provider-config, or tool-version changes;
- enabled LLM model, model-alias, or provider capability changes;
- Codex CLI, Claude Code CLI, Ollama, or other approved provider CLI version or behavior changes;
- explicit operator request, scheduled audit, doctor run, or prior run ending with consistency, quorum, or validation failure;
- derived-document changes when derived-document checks are enabled.

Hard consistency failures block ordinary prioritization and ordinary work. Hard failures include:

- missing or unparseable canonical files;
- duplicate question or decision IDs;
- malformed question metadata;
- dependency references to missing questions;
- dependency cycles;
- invalid `Resolved by` references;
- solved, decomposed, deferred, or superseded status contradictions;
- selected-work dependency violations;
- patch-base mismatch or canonical input changes after the consistency snapshot;
- forbidden-path proposals.

Warnings do not block prioritization when they cannot invalidate the selected work item. Warnings include stale derived documents, unscored or `TBD` ranking fields, optional provider failures when quorum remains satisfiable, nonblocking provider version drift, stale historical scores, and derived artifact publication gaps. A run that continues with warnings must record them and exclude stale scores from automatic selection.

Hard failures are converted into consistency problem records with stable failure key, severity, affected files or object IDs, evidence, suggested repair, and whether a human decision is required. If the repair is mechanical and within the allowed file set, model-committee may solicit repair patches. If the repair exposes an unresolved design choice, it may create or update an open question. New MVP blockers must satisfy the `UBU-D0175` blocker-certificate rule.

Ordinary work may proceed against a known inconsistency only when the selected work item repairs or narrows that inconsistency, declares the relevant failure IDs, avoids relying on invalid derived ranking, and reruns consistency after the candidate patch. It must not mix unrelated design answering with consistency repair unless both are necessary for the same failure.

LLM model updates and provider CLI updates do not retroactively change committed canonical design state or historical run logs. They do invalidate reusable derived rankings, readiness estimates, and score evidence that depend on the old provider baseline. Before automatic selection reuses such artifacts, model-committee must rerun consistency and, when the selected proposal depends on old scores, rescore or require human review.

Future integrated UbU Automation Worker behavior should map this loop to ordinary worker semantics: consistency checks are high-priority assigned work; repair outputs are mutation or patch candidates; canonical state changes only after parent validation and human repository review where required.

**Consequences:**

- Consistency repair is the only work mode allowed while hard failures are present.
- Prioritization may run with warnings only when warning provenance is recorded and stale score evidence is excluded.
- Provider and model updates invalidate reusable derived scores without rewriting accepted canonical state or historical run logs.

---

## UBU-D0179: Public dogfooding artifacts publish sanitized review packages

**Status:** Accepted → DESIGN.md §3.1.3

Resolved question: `UBU-Q0046`.

`model-committee` public dogfooding should publish sanitized review packages, not complete private run logs by default. The public package must show enough evidence for a contributor to verify the loop: selected question or problem, base commit, canonical input hashes, provider proposals, mechanical validation, score matrix or scoring summary, quorum or disagreement outcome, selected patch, suggested commit message, human-review status, and eventual canonical commit, Issue, or PR link when available.

Default public artifacts:

- `review.md`;
- `selected.patch` when a selected or reviewable candidate exists;
- `commit_message.txt` when available;
- `manifest.public.json` or an equivalent redacted manifest;
- score-matrix and validation summaries;
- links to the source question, related Issue or PR, accepted decision, and final commit when available.

Not public by default:

- raw provider logs, prompts, stderr, JSONL traces, and full manifests;
- private chain-of-thought or hidden reasoning;
- unsafe or policy-violating provider outputs;
- API keys, bearer tokens, credential paths, private environment dumps, or secrets;
- unredacted sensitive context or Compartment-protected payloads.

Raw artifacts may be published only after explicit redaction and artifact-safety review. Hashes and summarized diagnostics should be preferred when the raw material is not needed for public review.

Failed and partially successful runs are publishable when labeled by failure class and next review action. Acceptable public failure classes include parse failure, schema failure, invalid patch, no valid proposal, no quorum, selected score below threshold, critical disagreement, provider timeout, and artifact-safety block. A failed run should demonstrate bounded authority and preserved evidence; it must not imply accepted design state.

`model-committee` may generate publication commands and GitHub/PR link placeholders, but it must not auto-publish artifacts, open PRs, push branches, mutate GitHub, apply patches, or mark canonical questions solved. Human review remains required to publish artifacts, accept patches, and connect a run to a final repository commit.

**Consequences:**

- Public credibility comes from reviewable run packages and accepted commits, not from dumping every raw provider artifact.
- `UBU-Q0063` may depend on this policy for organizational-introspection dogfooding artifacts.
- Future implementation should add an artifact-safety check and a redacted public manifest schema before publishing run packages.

---

## UBU-D0180: Release Outreach Pipeline uses structured packages and gated publication

**Status:** Accepted → DESIGN.md §§2.6, 4.1.3

See DESIGN.md §§2.6, 4.1.3.

---

## UBU-D0181: Calendar preview and Log review use lightweight review annotations

**Status:** Accepted → DESIGN.md §§4.1.2, 17.8

See DESIGN.md §§4.1.2, 17.8.

---

## UBU-D0182: Pipeline state is projection-scoped workflow metadata

**Status:** Accepted → DESIGN.md §§17.2, 26.2

See DESIGN.md §§17.2, 26.2.

---

## UBU-D0183: GitHub analysis work uses parent scope and noise budgets

**Status:** Accepted → DESIGN.md §27.5

See DESIGN.md §27.5.

---

## UBU-D0184: GitHub tokens are actor-held scoped credentials

**Status:** Accepted → DESIGN.md §27.3.1

See DESIGN.md §27.3.1.

---

## UBU-D0185: Authority source is a closed MVP source-path enum

**Status:** Accepted → DESIGN.md §§17.9, 25.1 — value set and carrier exemption superseded by `UBU-D0226`

See DESIGN.md §§17.9, 25.1.

---

## UBU-D0186: Compact Calendar coverage is estimated regeneration metadata

**Status:** Accepted → DESIGN.md §§16.2, 16.9; PLANNING_KERNEL_CONTRACT.md §§2, 4

See DESIGN.md §§16.2, 16.9.

---

## UBU-D0187: Worker retries use failed attempts and retry siblings

**Status:** Accepted → DESIGN.md §§25.1.2, 27

See DESIGN.md §§25.1.2, 27.

---

## UBU-D0188: Automation expansion uses Containers with ordinary child Tasks

**Status:** Accepted → DESIGN.md §§9.4, 24.1.2

See DESIGN.md §§9.4, 24.1.2.

---

## UBU-D0189: Phase 1 readiness scoring uses gated derived evidence

**Status:** Accepted → DESIGN.md §4.1

See DESIGN.md §4.1.

---

## UBU-D0190: Automation eligibility uses three governance classes

**Status:** Accepted → DESIGN.md §3.7.1

Resolved question: `UBU-Q0035`.

`Auto-choice eligibility` classifies how much authority model-committee automation has over an open-question answer. It is separate from answerability, automation-likelihood, importance, and risk. Automatic selection remains advisory; canonical acceptance still requires ordinary human repository review and commit.

Allowed values:

- `Auto eligible`: automation may rank, propose, validate, score, and select a review candidate when dependencies, consistency, validators, quorum, and patch checks pass. Use only for bounded, mechanically reviewable work under accepted constraints.
- `Human approval required`: automation may draft, decompose, score, and prepare review artifacts, but the run must mark the result human-review-required. Use for durable schema/API/data-model decisions; security, privacy, Compartment, Identity, authority, worker, projection, relationship, affect, public-claim, release-outreach, or MVP-blocker decisions.
- `Human only`: automation may summarize, detect inconsistencies, ask clarifying questions, or prepare option memos, but it must not auto-select an answer. Use for project-owner directives, scope freeze, release/go-no-go, licensing or IP changes, funding acceptance, roadmap or mission pivots, legal commitments, public commitments, and overrides of failed consistency, quorum, disagreement, or validation gates.

Classification rules:

- Answerability remains the first gate. A blocked question is ineligible for ordinary answering regardless of automation class unless its dependencies are solved in the same work item.
- Automation-likelihood ranks questions only after answerability and automation eligibility are known.
- Unknown or mixed cases use the stricter class.
- A model-generated patch may not lower an existing question's human-involvement class without human review.
- A new `UBU-D0175` blocker certificate is `Human approval required` unless it also creates a `Human only` commitment.

**Consequences:**

- Future question ranking can distinguish answerability, automation likelihood, and governance eligibility instead of treating them as one score.
- New or updated open questions should carry the strictest applicable `Auto-choice eligibility` value.
- Human-only and human-approval-required work may still benefit from automation, but only as bounded review support.

---

## UBU-D0191: UbU's root product is individual life logistics

**Status:** Accepted → DESIGN.md §§1, 2.3.1, 4

See DESIGN.md §§1, 2.3.1, 4.

---

## UBU-D0192: Resource is a core life-logistics abstraction with a thin Phase 3 boundary

**Status:** Accepted → DESIGN.md §§4, 10.4

See DESIGN.md §§4, 10.4.

---

## UBU-D0193: Skill is an Identity-owned rust-prone capability predicate

**Status:** Accepted → DESIGN.md §§10.5, 21.7

Skill should be a first-class object. A Skill is an Identity-owned capability that can satisfy Task dependencies, unlock Techniques, reduce risk, reduce cost, improve quality, or make a DIY path available.

Skills can be learned, tested, evidenced, improved through practice, and allowed to rust over time. A Skill claim should carry provenance or evidence appropriate to its use. Self-declared evidence may be sufficient for private planning, while public, delegated, or marketplace-visible claims need stronger evidence and confidence semantics.

Skills should support fields such as owner Identity, proficiency level, confidence, verification status, last used time, last tested time, rust model, prerequisite Skills, unlocked Techniques, evidence references, and Compartment.

**Consequences:**

- Task readiness must eventually consider both external Resources and embodied Skills.
- `Skill` should not be collapsed into a free-text tag or marketplace profile.
- The private Skill model should precede public Skill Barter.

---

## UBU-D0194: Techniques can form a real-life capability graph and skill tree

**Status:** Accepted → DESIGN.md §§10.6, 21.7

A Technique is a reusable procedure that transforms Resources, Skills, time, attention, and other preconditions into an Objective-serving result. A Technique can require Skills, consume or transform Resources, test or reinforce Skills, produce new Resources, and unlock later Techniques.

A large UbU-run or UbU-compatible Technique database should be treated as a premier future feature. It can allow the user to select or automatically schedule Skill-learning Tasks that unlock DIY work, maintenance, professional labor skills, and future Skill Barter opportunities.

The skill-tree analogy is useful when grounded in real evidence and real capability rather than fake points. UbU can gamify real life by making capability acquisition visible, schedulable, inspectable, and economically meaningful.

**Consequences:**

- Technique database design should connect Resource requirements, Skill requirements, learning paths, verification criteria, risk, cost, and failure consequences.
- TaskFactories should remain compatible with Resource and Skill requirements.
- Outreach may use “gamifying real life” when it explains that the game mechanics correspond to real-world capability and evidence.

---

## UBU-D0195: DIY-versus-purchase/hire/barter tradeoff is a core planning comparison

**Status:** Accepted → DESIGN.md §10.6

Resource and Skill modeling create a natural comparison between buying a replacement, hiring a professional, doing the work oneself, learning first and then doing the work, bartering Skill, or deferring.

UbU should eventually compare these alternatives using time cost, money cost, affect cost, risk, consequence of failure, Resource availability, Skill level, Skill rust, learning value, future unlocks, and marketplace or barter value.

This tradeoff supports economic self-sufficiency without forcing the user toward DIY. Hiring, buying, or deferring may be the correct decision when risk, affect, time, legal constraints, or quality requirements justify it.

**Consequences:**

- Financial management should connect to Objectives, Tasks, Techniques, Resources, and Skills rather than start as a standalone ledger clone.
- User-facing recommendations should not moralize DIY; they should explain tradeoffs.
- Regulated or dangerous domains need careful evidence, risk, and external-provider boundaries.

---

## UBU-D0196: Skill Barter should be an open user-sovereign skill economy direction

**Status:** Accepted → DESIGN.md §21.7

See DESIGN.md §21.7.

---

## UBU-D0197: Phase 3B and Phase 4+ are the full version 1.0 release track

**Status:** Accepted → DESIGN.md §4

See DESIGN.md §4.

---

## UBU-D0198: Counterfactual decisions use decision_recorded payloads

**Status:** Accepted → DESIGN.md §17.0

Resolved question: `UBU-Q0107`.

MVP counterfactual logging uses the existing `decision_recorded` Log event type. It does not add separate event types for every rejected candidate. A `decision_recorded` entry is required whenever UbU presents a meaningful user-facing option, warning, proposal, alert, or recommendation and the user rejects, dismisses, overrides, bypasses, or ignores it.

Required `decision_kind` values:

- `plan_candidate_rejected`;
- `task_suggestion_dismissed`;
- `suggestion_declined`;
- `system_recommendation_overridden`;
- `worker_proposal_declined`;
- `safeguard_advisory_bypassed`;
- `alert_dismissed_or_ignored`.

Minimum `event_payload` records:

- `presented_ref` or compact redacted `presented_candidate_summary`;
- available actions and the observed `user_action`;
- chosen and rejected refs when applicable;
- optional `reason_capture`;
- system-state refs in `decision_context`;
- whether the entry is eligible for future preference inference.

User-stated reasons are never mandatory. `reason_capture.reason_source` is one of `user_stated`, `user_selected_code`, `system_inferred`, or `none_given`; inferred reasons are review notes, not canonical Preferences.

Counterfactual decision entries are append-only. Later Preferences, Task changes, Objective changes, safeguard policy changes, worker reassignments, annotations, or corrections may cite the decision Log ref, but they do not rewrite the original decision entry.

Preference inference may consume only uncorrected counterfactual entries and must distinguish stated, selected-code, inferred, and missing reasons. Absence of a counterfactual entry is not evidence of user acceptance.

**Consequences:**

- Introspection and preference inference have reviewable evidence for rejected options, not only accepted outcomes.
- Optional reason prompts can stay lightweight without losing the fact of the decision.
- No new Log event type is required beyond `decision_recorded`.

## UBU-D0199: Question decomposition reduces design burden, not question count

**Status:** Accepted → DESIGN.md §§3.6, 3.7

See DESIGN.md §§3.6, 3.7.

---

## UBU-D0200: Phase 1 preference calibration uses six neutral example frames

**Status:** Accepted → DESIGN.md §§2.2.1, 4.1.2, 8.6

See DESIGN.md §§2.2.1, 4.1.2, 8.6.

---

## UBU-D0201: Discovery mode uses reviewable evidence and explicit override admission

**Status:** Accepted → DESIGN.md §§4.1.2, 12.2, 17.8

See DESIGN.md §§4.1.2, 12.2, 17.8.

---

## UBU-D0202: Evergreen gap-fillers are ordinary Dynamic Task suggestions

**Status:** Accepted → DESIGN.md §§9.3.1, 15.4, 16.4, 16.6

See DESIGN.md §§9.3.1, 15.4, 16.4, 16.6.

---

## UBU-D0203: Adaptive planning granularity uses explicit execution profiles

**Status:** Accepted → DESIGN.md §16.7

See DESIGN.md §16.7.

---

## UBU-D0204: Phase 1 organizational introspection uses manual outreach retrospectives

**Status:** Accepted → DESIGN.md §4.1.4

See DESIGN.md §4.1.4.

---

## UBU-D0205: Canonical use-case statement and capability framing

**Status:** Accepted → DESIGN.md §1, README.md

The canonical UbU use-case statement is:

> UbU helps an individual user solve everyday life problems by transforming a desired outcome into a legitimate, executable plan. It does this by modeling the current state of the user's life, the desired state, required Tasks, available and missing Resources, available and missing Skills, reusable Techniques, affect and energy constraints, financial tradeoffs, public or marketplace options, expert-guided alternatives, and evidence from execution.

The canonical use-case formula is:

> **Objective + Current State + Constraints + Resources + Skills + Techniques + Preferences + External Options → Legitimate Plan**

The deeper philosophical framing is:

> UbU helps people build the capabilities, resources, routines, and relationships needed to actually live the life they choose.

This makes UbU a **capability engine for real life**, not merely a scheduler or task manager.

**Consequences:**

- Public-facing files should use the canonical statement and formula when introducing UbU to new audiences.
- The capability framing strengthens the product identity and distinguishes UbU from adjacent tools.

---

## UBU-D0206: Government and public-resource symmetry is a design invariant

**Status:** Accepted → DESIGN.md §2.3.1

Any institution that can constrain a user's plan may also provide Resources, permissions, remedies, or procedures that improve the user's plan. UbU must model both sides.

UbU should not only surface regulations, permits, and legal constraints. It should also identify and plan around grants, subsidies, benefit programs, weatherization assistance, workforce training, legal aid, public infrastructure, hardship waivers, appeals, enrollment windows, and other government- or community-supported Resources.

Many public Resources are conditionally unlockable: they become available only after the user completes prerequisite actions such as applying for a benefit, collecting documents, filing a form, or requesting an accommodation. UbU should represent these as conditional Resource availability states and generate the prerequisite Tasks automatically.

**Consequences:**

- UbU avoids being one-sided: it is not only a constraint-modeling system but also a resource-discovery and entitlement-navigation system.
- The benefits navigation and public-program access use cases become natural extensions of the Resource model.
- This is particularly important for users in economic precarity, life transition, or unfamiliar institutional environments.

---

## UBU-D0207: Library and Community Resource Mode is a named feature direction

**Status:** Accepted → DESIGN.md §2.3.1, OUTREACH.md

Libraries, tool libraries, makerspaces, public workshops, repair cafés, seed libraries, and community resource centers should be treated as Resource providers in UbU's planning model.

Strong marketing lines for this feature direction:

> **UbU turns your library card into a real-life skill tree.**

> **Borrow the tool. Learn the skill. Do the project. Keep the capability.**

> **Own less. Do more. Learn more. Waste less.**

This feature supports the public-good, frugality, sustainability, self-sufficiency, and life-upgrade narratives and democratizes access to Resources previously available only to those with money or social capital.

**Consequences:**

- Community Resources become first-class planning inputs alongside owned, rented, and purchased Resources.
- The Library and Community Resource Mode is a Phase 3B product hook that requires no new ontology — it is an application of the existing Resource model to a new category of providers.

---

## UBU-D0208: Expert-Guided DIY and Technique Commissioning are first-class product concepts

**Status:** Accepted → DESIGN.md §2.3.1, OPEN_QUESTIONS.md

UbU should support a middle tier between generic guides (Tier 1) and hiring a professional to do the whole job (Tier 3):

> **Tier 2: Expert diagnosis + custom Technique Package — paid, specific to the user's actual situation.**

A **Technique Request** is submitted by the user with evidence about their specific problem: photos, measurements, model numbers, symptoms, skill level, available tools and Resources, budget, time window, and risk tolerance.

A **Technique Package** is returned by a skilled expert: diagnosis, parts list, tool list, step-by-step instructions calibrated to the user's skill level, safety warnings, verification criteria, and the conditions under which to stop and hire a professional.

This is not generic AI advice. It is AI-orchestrated expert delegation that produces a custom executable Technique for the user's actual situation. A skilled person can sell diagnosis and instructions as a distinct economic product, separate from labor.

**Consequences:**

- A new marketplace primitive emerges that does not exist in current platforms: situated expert knowledge packaged as an executable artifact.
- Retired tradespeople, experienced professionals, and domain experts can monetize knowledge without physical labor.
- Users gain access to expert guidance at a fraction of a full service call.
- New open questions should be added for the Technique Request and Technique Package schema.

---

## UBU-D0209: Task-driven Resource Exchange is a named strategic direction

**Status:** Accepted → DESIGN.md §2.3.1, SOVEREIGN_COORDINATION.md, FUNDER_BRIEF.md

The Resource abstraction should extend to all access modes: owned, borrowed, rented, bought, sold, leased, bartered, or reserved Resources. This creates a planning primitive distinct from existing search-driven marketplaces:

> **"I need access to Resource X, near place Y, during time window Z, below price P, because it unlocks Task T."**

The key distinction is:

> **Task-driven markets, not search-driven markets.**

Existing marketplaces start from search. UbU's Resource Exchange starts from the user's plan. The access method is generated from the plan's needs, not from a keyword query.

UbU can compare: borrow free from a neighbor or library; use a tool library; rent locally; buy used; buy new; hire someone who already owns it; barter; learn the Skill and use a shared Resource; delay the Task; or cancel the Task.

**Staging:**
- Early: UbU recommends external options (library, Craigslist, rental shop, Home Depot).
- Middle: UbU helps create listings and bids ("Need tile saw Saturday 10am–4pm").
- Later: UbU-native task-aware marketplace with bids, reservations, escrow, condition records, and reputation.

**Consequences:**

- The Resource Exchange becomes a named Phase 3B/4+ direction distinct from the Skill Barter marketplace.
- Ethereum fits naturally as the settlement and trust layer beneath a task-driven Resource and Skill exchange.

---

## UBU-D0210: Skeleton Plan failures use bounded diagnostics and blocking clarification

**Status:** Accepted → DESIGN.md §15.2.2; PLANNING_KERNEL_CONTRACT.md §4

Resolved question: `UBU-Q0070`.

Skeleton generation must not continue into ordinary optimization when no valid skeleton baseline exists for the current Calendar scope. It returns a bounded `SkeletonFailureDiagnostic` and asks for clarification or a user choice.

MVP `failure_class` values:

- `missing_starting_state`;
- `impossible_dependency`;
- `cyclic_dependency`;
- `static_task_collision`;
- `insufficient_calendar_window`;
- `unavailable_resource`;
- `blocked_external_event`;
- `unknown_precondition`.

The diagnostic payload records diagnostic ID, severity, failure class, affected Task refs, missing or conflicting state, relevant time-window or Static Task refs, initial UniverseState ref, source or External Event refs, a compact causal chain, safe alternatives, prompt policy, and a short non-blaming user-facing summary.

Default explanation budget is the failed Task or state, the immediate cause, and at most three causal-chain steps. Full dependency/precondition detail stays one inspector action away.

Safe alternatives are limited to providing or correcting starting state, marking the state already satisfied, adding a prerequisite Task, relaxing a deadline or Static constraint, extending the planning horizon, removing or mooting the blocked Task, choosing an already-modeled alternate Technique or Task path, waiting for or recording an External Event, or manual decision. UbU must not invent canonical Resources, Skills, Techniques, Preferences, or external facts just to repair skeletonization.

Use `immediate_blocking_prompt` when the failure prevents a valid baseline for the current Calendar, current or next recommendation, Static Task placement, hard dependency/precondition, deadline feasibility, required Resource, or required External Event. Use `planning_warning` only when a valid skeleton still exists and the failed chain is outside the current recommendation path or future horizon; then record the diagnostic and mark the relevant Calendar, explanation, or risk report stale.

**Consequences:**

- Skeleton failure is model repair, not low-quality optimization.
- User clarification can produce explicit state updates, Task changes, Plan repair, or manual decisions through normal admission and Log paths.
- Implementations can validate failure handling through the `SkeletonFailureDiagnostic` payload in `PLANNING_KERNEL_CONTRACT.md`.

---

## UBU-D0211: Semi-legitimization prunes candidates before full legitimacy validation

**Status:** Accepted → DESIGN.md §15.2.2; PLANNING_KERNEL_CONTRACT.md §4

See DESIGN.md §15.2.2.

---

## UBU-D0212: Phase 1 planner solver selection uses CPU certification

**Status:** Accepted → DESIGN.md §16.10; PLANNING_KERNEL_CONTRACT.md §2

See DESIGN.md §16.10.

---

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

## UBU-D0215: `TaskFactory` is eliminated; Technique instantiation into a Container subsumes it

**Status:** Accepted → DESIGN.md §15.2.1.1; OPEN_QUESTIONS.md `UBU-Q0115` tombstone

Resolved question: `UBU-Q0115`.

`TaskFactory` is removed as an over-design. Its role — expanding a template into a set of Tasks, dependency edges, and an Objective structure — is already provided by Technique instantiation, which expands a Technique's Steps into a Container of child Tasks with intra-Technique edges. A second object doing the same work is redundant.

Recurring project scaffolding, the use case `UBU-Q0115` reserved for TaskFactory, is served by an evergreen Objective with a calendar-style recurrence schedule (`UBU-D0213`) whose reactivations drive Technique-based expansion (`UBU-D0216`).

**Consequences:**

- `UBU-Q0115` is closed as resolved-by-elimination.
- No new template object is introduced; expansion reuses Technique, Step, Container, and Objective.

---

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

## UBU-D0217: Revealed preference is a proposal, not a fact; automatic selection is confidence- and authority-gated

**Status:** Accepted → DESIGN.md §16.3.1

Resolved question: `UBU-Q0125` (partial); related to `UBU-Q0074`.

When UbU presents competing candidate Plans and the user selects one, that choice is a proposal about the user's trade-offs, not a canonical fact. It may propose a weight or Preference update surfaced for explicit user acceptance; it must never silently rewrite the user's trade-off vector. This is an axiom: UbU does not decide values for the user.

Supporting constraints:

- A single choice is a weak signal (one inequality in weight-space) and must be accumulated conservatively.
- Trade-offs are affect- and state-conditioned; learned weights are a function of current state, not a fixed global vector.
- Eventual automatic selection requires two gates in series: a confidence threshold and a user-granted, revocable auto-choice authority. Confidence alone never authorizes automatic selection. The model is `Auto-choice eligibility` as a governance gate separate from automation-likelihood, plus the trusted-auto-publication pattern.
- Auto-selected choices remain logged, inspectable, and overridable.
- Affect-conditioned introspection findings may surface an observed correlation and offer a context remedy (change when and how a decision is made), but must not assert a psychological mechanism, blame the user, or nudge toward the option UbU scores higher.

**Consequences:**

- The trade-off-comparison engine cannot become an autonomous value-setting system.
- Confidence and authority are kept as separate gates.

---

## UBU-D0218: Multi-Technique candidate comparison uses bounded branch-and-bound and surfaces outcomes, not utils

**Status:** Accepted → DESIGN.md §16.3.1

Resolved question: `UBU-Q0125` (partial); demand-side driver for `UBU-Q0119`.

When more than one already-modeled Technique can satisfy an Objective, candidate generation may produce competing Plans that differ by Technique and compare their predicted real-world outcomes for user choice.

Required properties:

- **No cross-product enumeration.** The full `k^N` Technique cross-product contradicts the bounded-search commitment. The required shape is branch-and-bound with semi-legitimization (`reject_obvious` / `passes_cheap_checks`) as the cheap pruner; only a small Pareto-frontier finalist set reaches full legitimization and full scoring. Dominance is a bound during search, not a post-enumeration filter.
- **Full-vector dominance.** A candidate dominates only if at least as good on every outcome axis, including robustness, affect-margin, dependency fragility, and Plan probability — not only money and time. Cheaper-but-more-fragile candidates are not dominated.
- **Surface outcomes, not utils.** The user sees concrete predicted terminal UniverseState (money, time, Resources, affect, relaxation, Plan probability), not a util scalar. Money is a generic cost outcome only; account identity, balances, and overdraft analysis require a financial model deferred to Phase 3B/4+.
- **Cognitive load and cadence.** The comparison reuses the Calendar-preview UX surface and roughly its choice counts; diverging is a user configuration. Within-noise finalists are presented as a tie with a `sensitivity_summary`, not a manufactured ranking.
- This engine is the demand-side driver for the Technique database (`UBU-Q0119`): more Technique variety widens the achievable outcome frontier.

**Consequences:**

- Adding outcome axes increases scoring sensitivity (many-objective dominance resistance and ranking instability under noise); robust multi-objective ranking under uncertainty is flagged as open research in `UBU-Q0125`.
- The killer-feature framing is "see and choose across every axis," not "maximize all axes," which have no joint maximum.

---

## UBU-D0219: External identity-and-access standards posture

**Status:** Accepted

UbU adopts external identity-and-access standards — IAM — by layer, with deliberately asymmetric treatment: a standard used for external interoperability is surfaced verbatim at the boundary, and a standard used only as an internal engine stays below the design vocabulary.

**SPIFFE/SPIRE is a conformance target (outward-facing).** It is adopted at the Delegation Substrate, federation, marketplace, worker, hosted-planning, boundary-agent, and commercial-wire boundary so that workload identity, execution provenance, and authority evidence are cryptographically attestable and externally auditable. SPIFFE terms — trust domain, SPIFFE ID, SVID, node/workload attestation, federation — appear unaliased at that boundary. SPIFFE represents UbU-controlled workloads, worker instances, Devices when exposed as execution enclaves, Compartment-scoped boundary agents, hosted services, and delegation endpoints. It does **not** replace human Identity or user-sovereign Identity.

**OPA is an implementation substrate (inward-facing).** OPA may realize the policy checker, but its terms — Rego, PDP, PEP, bundle, and especially "decision" — are realization notes only and must not enter core vocabulary. OPA can return structured policy outputs, but conventional allow/deny authorization patterns are the wrong product vocabulary for UbU's graduated model. OPA must not flatten semi-legitimization, full legitimization, needs-clarification states, or bypass-as-introspection-evidence into a boolean gate.

**SAML/OIDC is boundary-only.** External enterprise authentication may be supported at the commercial wire when required, with OIDC/OAuth2 preferred for greenfield federation. External IdPs never become the source of truth for user-sovereign Identity.

- `authority_source` and the **claim register** keep their names. At a boundary, `authority_source` may be backed by one or more verifiable claims, including an SVID identifying the executing workload or boundary agent. The SVID authenticates the workload; UbU capability grants, Log provenance, Compartment policy, task-specific `authority_scope`, and review/admission rules establish whether that authenticated actor is authorized for the action.
- **IdentityAttestation** is workload/boundary-agent evidence unless explicitly bound to a human-approved authority path by UbU-native provenance. It must not merge with **AssociationAttestation**.
- SPIFFE IDs, SVIDs, trust-domain names, federation bundles, issuance logs, and boundary telemetry are potentially correlating identifiers. Any SPIFFE/SPIRE profile must treat namespace shape, SVID lifetime, bundle exposure, logging, and federation scope as privacy-critical design parameters.
- The trust-domain-to-Compartment granularity choice is left open as `UBU-Q0126` and must consider at least three candidates: one sovereign trust domain, per-Compartment trust domains, and a hybrid control-plane root with purpose-scoped / pairwise / Compartment-scoped boundary federation.
- No SPIFFE, SPIRE, OPA, OIDC, SAML, SVID issuance, enterprise federation, or commercial-wire identity implementation is required for Phase 1 dogfooding.

---

## UBU-D0220: Directional authority

**Status:** Accepted → DESIGN.md §2.19

Authority and intent originate at the individual and flow outward into coordination, never inward. This is the invariant that distinguishes a legitimate social outgrowth of self-governance from self-governance absorbed into a coordination platform; from the schema alone the two are nearly indistinguishable, and only the direction of authority separates them. The invariant constrains the multi-scale coordination directions (`UBU-D0222`, DESIGN.md §32): Associations, Delegations, super-connectors, and marketplaces are legitimate only while authority continues to originate at the individual.

---

## UBU-D0221: Eject-not-override for shared-authority compartments

**Status:** Accepted → DESIGN.md §23.6

For any Compartment whose policy is not solely user-set, sovereignty is preserved at the device boundary rather than at the field level. The user can always destroy or eject such a Compartment wholesale, but cannot selectively override its external policy while retaining that Compartment's data. A voluntarily-accepted, always-destroyable container keeps the sovereignty claim intact; a Compartment the user cannot destroy is the line at which UbU would become the thing it opposes. This invariant gates the protective-compartment direction (`UBU-D0222`) and any shared-authority coordination (DESIGN.md §32.5).

---

## UBU-D0222: Named multi-scale and sovereign-coordination strategic directions (Phase 3+)

**Status:** Accepted → DESIGN.md §32

See DESIGN.md §32.

---

## UBU-D0223: Cloud and premium compute is leak-minimization, not leak-elimination

**Status:** Accepted → DESIGN.md §32.6

Any cloud computation leaks proportional to its duration through access patterns, timing, and resource profile; FHE protects the plaintext, not the side channels. A long-horizon premium planning run, including a prospective UbU Corp premium tier, is the largest instance of a leak the architecture already accepts when it is bounded and consented. Documentation must name the residual leak as residual and require it to be ephemeral and Compartment-scoped, rather than implying a zero-leak guarantee that "local-first, inspectable" could be misread as making.

---

## UBU-D0224: Feature-to-data map as a planned full-product legibility artifact

**Status:** Accepted → DESIGN.md §3.11

Alongside bootstrap-dependency build ordering, the project maintains a feature-to-data map: for each feature, the data components it actually invokes at runtime versus those merely present in the model. It is a legibility instrument that makes "this feature needs nearly everything" falsifiable, prevents quiet scope creep, and gives reviewers a precise dependency picture per slice. It is a planned full-product artifact, not a Phase 1 deliverable.

---

## UBU-D0225: Competitive positioning against AI calendar and scheduling assistants

**Status:** Accepted → DESIGN.md §31

UbU is differentiated first positively: AI auto-schedulers take the user's to-do list and events as given and optimize their placement (the *when* of an already-decided *what*), whereas UbU is the generative goal-and-values layer that originates *what belongs there at all* from goals, values, affect, and resources, working from introspected rather than inferred sensor state. The defensibility claim is an incentive argument, not a capability one: an incumbent could build local-first but will not, because matching UbU's sovereignty means abandoning the pooled-corpus data asset its model depends on, and a partial "private mode" delivers a promise where UbU delivers a verifiable architecture. Public messaging must avoid the absolute "they cannot," frame it as "structurally disincentivized," tie it to the running demo and open repository, and calibrate by audience: `WHAT_IS_UBU.md` carries only the positive category line with no competitor names or accusations; `SOVEREIGN_COORDINATION.md` and `FUNDER_BRIEF.md` carry the full incentive/verifiability argument.

---

## UBU-D0226: AuthoritySource is a pure authority-path enum; information source moves to provenance

**Status:** Accepted → DESIGN.md §17.9; DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §8, §13, Appendix A; docs/PHASE1_CONTRACT_BOUNDARIES.md. Supersedes the `UBU-D0185` value set and carrier exemption; the `UBU-D0185` principles — a coarse closed enum that is never sufficient authorization by itself — stand.

See DESIGN.md §17.9.

---

## UBU-D0227: Task readiness states are derived views, not canonical status

**Status:** Accepted → DESIGN.md §9.5; docs/PHASE1_CONTRACT_BOUNDARIES.md

The canonical Task lifecycle is `active`, `completed`, `failed`, `moot` per DESIGN.md §9.5, with `moot_reason_code` as the existing closed enum. Readiness and execution states such as proposed, ready, blocked, and in-progress are derived: candidacy is the store's candidate/admitted distinction; ready and blocked are computed from dependencies and deterministic precondition evaluation; in-progress is `active` plus recorded start evidence in the Log. Derived readiness may appear in API responses, UI, and reports but must not be persisted as canonical Task status. `canceled` is not a canonical status; the specific moot reason code is used instead.

**Consequences:** the `ubu-schemas` `task-status` schema is corrected to the canonical lifecycle and gains the `moot_reason_code` closed enum; `ubu-core`, `ubu-store`, `ubu-orchestrator`, and `ubu-ui` references to derived states are reworked as views.

---

## UBU-D0228: Phase 1 wire convention: snake_case JSON field names

**Status:** Accepted → ubu-schemas `CONTRACT.md`; docs/PHASE1_CONTRACT_BOUNDARIES.md

All Phase 1 JSON wire and schema field names use snake_case, matching the literal field vocabulary of `PLANNING_KERNEL_CONTRACT.md` and `DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md` (`authority_source`, `schema_version`, `request_id`, `moot_reason_code`, and the rest). Enum values are snake_case. Rust types mirror the wire through serde rename conventions, and the serde-to-schema lockstep enforces the convention in fixtures and CI. This applies to `ubu-schemas`, `ubu-core`, and every consumer; generated TypeScript types follow the wire names.

**Consequences:** the current camelCase fields in `ubu-schemas` and `ubu-core` are corrected before any consumer code grows; Phase 2 sync statements require no casing translation layer.

---

## UBU-D0229: Phase 1 ID-registry expansion to all fifteen object types

**Status:** Accepted → ubu-schemas id registry (single machine-readable source of truth per docs/PHASE1_CONTRACT_BOUNDARIES.md)

Six prefixes are added so that every Phase 1 canonical object type is admissible: `pref_` → Preference; `container_` → Container; `ustate_` → UniverseState; `identity_` → Identity; `rel_` → Relationship; `xevent_` → External Event. Each uses the existing prefixed lowercase unhyphenated UUIDv7 suffix pattern. The registry in `ubu-schemas` remains the single machine-readable source of truth; this record authorizes the closed-set change. The current `universe-state` schema models a snapshot view rather than a facts container; the UniverseState facts schema is first-slice implementation work under DESIGN.md §4.1.6.

**Consequences:** the `ubu-core` `ObjectType` enum and `ubu-store` admission mappings are extended in lockstep.

---

## UBU-D0230: Compartment guardrails as policy-summary members with a logged boundary decision

**Status:** Accepted → DESIGN.md §4.1 guardrail list; ubu-schemas `policy-summary`

See DESIGN.md §4.1.

---

## UBU-D0231: Orchestrator state is the ubu-store admission boundary; MemoryState is eliminated

**Status:** Accepted → docs/PHASE1_CONTRACT_BOUNDARIES.md; ubu-store `PHASE1_STORE_CONTRACT.md`; ubu-orchestrator. Implements the Phase 1 local state contract and admission vocabulary established by `UBU-D0226`, `UBU-D0227`, `UBU-D0229`, and `UBU-D0230`.

The `ubu-orchestrator` holds no canonical state of its own. The ephemeral in-memory `MemoryState` carried by the scaffold is removed: the orchestrator opens `ubu-store` on startup from a configurable database path, applies or verifies migrations, and serves every canonical read and write through the store. Candidate-versus-admitted status is the store's distinction, not an orchestrator status, and admitted Objectives, Tasks, Log events, and the other canonical object types are persisted only through store admission, which carries `Provenance.authority_source`. Derived readiness states (`UBU-D0227`) may appear in API responses but are never persisted as Task status. `schema_version` is validated and echoed end to end, and an unknown or missing version returns a structured diagnostic at the API boundary rather than panicking. The loopback HTTP surface remains a temporary transport pending the Phase 2 Tauri command bridge and is not a security boundary; this record adds no session, CSRF, or per-request token defenses.

**Consequences:** `MemoryState` and the in-memory maps it backed are deleted from `ubu-orchestrator`, which now depends on `ubu-store` at a pinned rev. The `ubu-devshell` fixture smoke test (`scripts/run-fixture-demo.sh`) is repointed at the store-backed orchestrator against a throwaway, isolated store, so the cross-repo smoke test exercises the admission boundary rather than an in-memory path. This record retires the scaffold's ephemeral-state placeholder; it does not expand Phase 1 scope, add Phase 2 replication, or implement the user-facing loop.

---

## UBU-D0232: Phase 1 next-action selection is a deterministic readiness-ordered skeleton rule with explicit action recording

**Status:** Accepted → docs/PHASE1_CONTRACT_BOUNDARIES.md; ubu-orchestrator; ubu-ui; ubu-devshell. Builds on `UBU-D0227` (derived readiness), `UBU-D0210` (bounded skeleton diagnostics), and `UBU-D0226` (authority paths).

The Phase 1 next-action recommendation is a deterministic, readiness-ordered selection over admitted Tasks, not planner output. Readiness is derived per `UBU-D0227`: candidacy is the store's admitted-versus-candidate distinction, and ready and blocked are computed from dependencies and deterministic precondition evaluation. The single recommended Task is chosen by an explicit priority order with a stable tiebreak, and readiness is never persisted as Task status. The recommendation carries a deterministic, templated explanation referencing the parent Objective, the readiness state, and provenance `source_refs`; the explanation is never model-generated and never asserts affect-legitimization, optimization, or planner provenance. When no Task is ready, the result is a bounded diagnostic per `UBU-D0210`, not an opaque empty response.

Recording a user action against the recommendation is an explicit canonical write through store admission. `complete` transitions the Task to the canonical `completed` status with `authority_source = user`; `override` records rejection of the recommendation with `authority_source = user_override` per `UBU-D0226`. Each action admits an append-only Log event with provenance. An optional `snooze` records a defer Log event only and does not implement snooze-aware readiness. This selection rule is a Phase 1 skeleton that the planning kernel subsumes once it exists; it must not be mistaken for planner output, and it adds no affect weighting, Calendar, or recalculation.

**Consequences:** `ubu-orchestrator` gains `next_action` selection, the templated explanation, the bounded empty/blocked diagnostic, and the action-recording endpoint; `ubu-ui` renders the next-Task view with act and override over loopback; the `ubu-devshell` fixture smoke test exercises the full onboard-to-act loop store-backed and offline. Where the Log event vocabulary lacks a dedicated task-transition member, action recording uses `decision_recorded` pending a separate decision to add a closed-enum member; no closed enum is extended without its own ticket. This record retires the *no runnable end-to-end dogfooding loop* readiness cap; it adds no planner, Calendar, projection write, or recalculation.

---

## UBU-D0233: GitHub projection is preview, per-batch approval, gated worker write, and reconciliation with conflict surfacing

**Status:** Accepted → ubu-orchestrator; ubu-github-adapter; ubu-ui; ubu-devshell. Implements `UBU-D0159` for Phase 1 and conforms to the frozen `ubu-schemas` projection family (`projection-preview`, `projection-operation`, `projection-approval`, `projection-result`, `projection-reconciliation`, `github-label-write`).

GitHub projection in Phase 1 is a four-stage flow: a deterministic, side-effect-free preview of the managed-label operations UbU would write; an explicit per-batch approval of the whole preview; a worker write that emits only after approval; and reconciliation that reads observed state back and compares it against the last applied projection. There is no auto-write: nothing leaves the machine without an explicit approval. The write is restricted to managed labels only — it never touches issue bodies, comments, open/close state, or non-managed labels — and is a constrained worker action recorded with `authority_source = automation_worker` per `UBU-D0226`, never user-equivalent authority. GitHub is not canonical: reconciliation produces a `matched`/`drifted`/`missing` result and, on divergence, surfaces a conflict for the user rather than silently overwriting in either direction; an external change the user accepts is admitted through store admission with GitHub provenance. Projection-layer records (preview, approval, result, reconciliation) are persisted durably for auditability, distinct from canonical-object admission.

**Consequences:** `ubu-github-adapter` provides a mockable managed-label write and reconciliation read; `ubu-orchestrator` exposes preview, approval, gated write, and reconciliation; `ubu-ui` renders the preview diff, the per-batch approve control, the projection result, and conflict surfacing over loopback; `ubu-devshell` exercises the preview → approve → write → reconcile loop offline against a mock GitHub backend. Phase 1 verifies projection against a mock, not live GitHub; live writes and non-label surfaces are deferred. This record refines `UBU-D0159` into the Phase 1 implementation; it adds no planner, Calendar, or recalculation.

---

## UBU-D0234: External export is gated by a single authoritative deny-by-default boundary with worker-authority and redaction-identity invariants

**Status:** Accepted → ubu-core; ubu-orchestrator; ubu-devshell. Extends `UBU-D0230` (Compartment guardrails as policy-summary members with a logged boundary decision) from a recorded decision into an enforced runtime chokepoint.

Every export-class operation — in Phase 1, a GitHub managed-label write — passes a single authoritative enforcement gate before it can emit. The `ubu-core` Legitimizer is the only adjudication path and the only issuer of an export permit; the permit is constructible solely by the gate on an `Accepted` legitimization, so permission to emit cannot be fabricated by a caller and no orchestrator code path can reach an external write without it. Adjudication is deny-by-default: an export-class operation is rejected when the effective Compartment policy cannot be resolved, when `local_only` or `no_external_export` forbids export, or when the legitimization is not `Accepted`. Two invariants are enforced at the gate: the worker-authority invariant rejects user-equivalent authority (`user`, `user_override`) on the export path, permitting only `automation_worker` per `UBU-D0226`; and the redaction-identity export-boundary invariant forbids Compartment names and labels from crossing a denied boundary in denial surfaces or emitted payloads. Every adjudication, allow or deny, writes a `compartment_boundary_decided` Log entry built from the Legitimizer's payload as the single source of truth.

**Consequences:** `ubu-core` owns the gate, the export permit, the worker-authority and redaction-identity checks, and a deny-by-default property-test suite; `ubu-orchestrator` routes all projection export through the core gate, holds no duplicate adjudication, and makes the adapter write reachable only by presenting a permit, with bypass-resistance tests; `ubu-devshell` runs the deny path and the export-boundary, worker-authority, and redaction checks as standing contributor diagnostics. Redaction-identity is enforced on the export path here, with full cross-cutting serializer coverage left as a named follow-up. This record makes the `UBU-D0230` boundary real at runtime; it adds no new export classes and does not implement `no_cloud_llm` enforcement, which has no cloud-LLM call to gate in Phase 1.

---

## UBU-D0235: The Phase 1 Plan is a canonical timed artifact regenerated by override-safe recalculation

**Status:** Accepted → DESIGN.md §15, §16, §29; PLANNING_KERNEL_CONTRACT.md; ubu-schemas (`planning/plan-step`, `planning/plan`); ubu-planning-kernel; ubu-orchestrator; ubu-ui; ubu-devshell. Refines `UBU-D0124` (legitimization makes skeleton Plans human-viable), `UBU-D0151` (Compact Calendar grammar), and `UBU-D0227` (canonical Task lifecycle and derived readiness).

See DESIGN.md §15.

---

## UBU-D0236: Affect legitimization is the Phase 1 human-viability filter via sigmoid affect constraints

**Status:** Accepted → DESIGN.md §13, §15.2.2; PLANNING_KERNEL_CONTRACT.md §6; ubu-schemas (`planning/affect-profile`, `core/snapshot` affect observation, planning-response legitimization fields); ubu-planning-kernel; ubu-orchestrator; ubu-ui; ubu-devshell. Refines `UBU-D0124` (legitimization makes skeleton Plans human-viable) and builds on `UBU-D0235` (canonical timed Plan).

See DESIGN.md §13.

---

## UBU-D0237: The kernel contract types are the planning surface; Phase C-1 adds value scoring, bounded candidates, and semi-legitimization

**Status:** Accepted → DESIGN.md §15.2, §16.3.1; PLANNING_KERNEL_CONTRACT.md §3, §4, §5; ubu-planning-kernel; ubu-orchestrator; ubu-ui; ubu-devshell; ubu-schemas (removal of the thin planning stubs). Refines `UBU-D0124`, `UBU-D0151`, and `UBU-D0211`; builds on `UBU-D0235` and `UBU-D0236`.

See DESIGN.md §15.2.

---

## UBU-D0238: Phase C-2 adds the Monte Carlo rollout, and rollout re-ranks the default Plan

**Status:** Accepted → DESIGN.md §15.2.1, §16; PLANNING_KERNEL_CONTRACT.md §3, §5, §7; ubu-planning-kernel; ubu-orchestrator; ubu-ui; ubu-devshell. Refines `UBU-D0151` and the §15.2.1 default-by-Plan-probability selection; builds on `UBU-D0237` (value scoring and bounded candidates).

See DESIGN.md §15.2.1.

---

## UBU-D0239: The Task carries an optional duration estimate and correlation-group membership, feeding the rollout

**Status:** Accepted → DESIGN.md §9 (Tasks), §15.2.1; PLANNING_KERNEL_CONTRACT.md §3; ubu-schemas (`core/task`), ubu-store, ubu-orchestrator, ubu-devshell. Builds on `UBU-D0237` and `UBU-D0238`, which consume these inputs.

See DESIGN.md §9.

---

## UBU-D0240: Derived risk and human-complete plan-quality reports

**Status:** Accepted → DESIGN.md §2.5.1, §16; ubu-schemas (`api/risk-report` enrichment, new `api/human-complete-plan-quality`), ubu-orchestrator, ubu-ui, ubu-devshell. Builds on `UBU-D0238` (the planning kernel emits the signals these reports aggregate).

See DESIGN.md §2.5.1.

---

## UBU-D0241: UniverseState facts container and deterministic precondition/mutation semantics

**Status:** Accepted → DESIGN.md §10.1, §11, §4.1.6; ubu-schemas (`core/universe-state` reshape, new mutation-item and precondition schemas), ubu-core, ubu-store, ubu-devshell. Builds on `UBU-D0229` (which added the `ustate_` prefix and `ObjectType::UniverseState`).

See DESIGN.md §10.1.

---

## UBU-D0242: Wiring the UniverseState facts container into the loop

**Status:** Accepted → DESIGN.md §10.1, §10.2, §11.3, §4.1.6; ubu-schemas, ubu-core, ubu-orchestrator, ubu-devshell. Builds on `UBU-D0241` (the facts container and its pure mutation/precondition semantics), which this program consumes rather than re-implements.

See DESIGN.md §10.1.

---

## UBU-D0243: UniverseState namespace convention: subject–predicate with a controlled subject vocabulary

**Status:** Accepted → DESIGN.md §11.2, §11 (the §1744 dotted-target grammar). Standalone; governs all UniverseState targets (facts, preconditions, mutations) across `ubu-schemas`, `ubu-core`, `ubu-orchestrator`. First applied by Wiring-C (`UBU-D0242`).

The §1744 grammar fixes that a target is a dotted string rooted in one of the four collections (`facts`, `numeric_values`, `set_memberships`, `event_markers`) with a namespaced key, but it does not fix the key vocabulary. The first segment after the collection is effectively a permanent top-level taxonomy referenced by every fact, precondition target, and mutation target, so it is governed deliberately.

**Convention.** A target is `<collection>.<subject>(.<entity-path>)?.<predicate>`, an entity–attribute (EAV / RDF subject–predicate) model:

- `<collection>` is one of the four §11.1 collections.
- `<subject>` is the first segment after the collection and **must be a member of the controlled subject vocabulary** below — an entity or domain root.
- `<entity-path>` is an optional sequence of nested-entity or instance segments (e.g. `issue.14`, `rel_123`).
- `<predicate>` is the final segment: a snake_case attribute name. Where a clean standard attribute name exists (schema.org / Dublin Core), it is preferred as a light overlay (e.g. `due_at`); external vocabularies are not adopted wholesale.

Examples: `facts.operator.work_style`, `facts.project.repository`, `numeric_values.affect.energy`, `facts.github.issue.14.pipeline_state`, `set_memberships.github.issue.14.labels`, `event_markers.relationship.rel_123.interactions`.

**Initial controlled subject vocabulary:** `operator` (the operating individual — not "user" or "self"), `project`, `github`, `affect`, `relationship`. The latter three ratify the subjects the design already used by example; `operator` and `project` are introduced by the Wiring-C bootstrap.

**Rule for adding a subject root.** Adding a subject to the controlled vocabulary requires a recorded `UBU-D` decision (governed like the closed enums). A subject root is a snake_case singular noun naming an entity or domain — never an instance, an attribute, a provenance/source, or a reverse-DNS authority prefix. Predicates and entity-path segments do not require a decision; only the top-level subject set is governed.

**Consequences:** the organization/worker-mode intrinsic-affect rejection (`UBU-D0242`, Wiring-B) keys off `<subject> == affect`, which this record formalizes. The Wiring-C bootstrap records facts only under `operator` and `project`. Source/provenance stays on the containing envelope (§11.3), never in the subject (this is why an origin-rooted convention was rejected). Future facts use the governed vocabulary; new subject roots are added only by recorded decision.

---

## UBU-D0244: Live GitHub managed-label projection policy

**Status:** Accepted → DESIGN.md §2.5 (the export boundary), §5 (instance modes). Governs the live projection path in `ubu-orchestrator` and `ubu-github-adapter`. First applied by the live-GitHub wave (O19/GA1/D18).

See DESIGN.md §2.5.

---

## UBU-D0245: Live GitHub ingestion policy

**Status:** Accepted → DESIGN.md §27 (GitHub import). Governs the live import path in `ubu-orchestrator` and `ubu-github-adapter`. First applied by the live-ingestion wave (O20/GA2/D19). Companion to `UBU-D0244` (live projection).

See DESIGN.md §27.

---

## UBU-D0246: The model-committee sends dependency-closure context, not whole canonical files

**Status:** Accepted → model-committee `IMPLEMENTATION_CONTRACT.md` v0.4; `PROMPT_CONTEXT_PLAN.md`. Extends `UBU-D0150` (model-committee architecture) and `UBU-D0176` (file authority model).

`model-committee` v0.3 injected every canonical file into every work prompt in full. Against this repository that was about 769,000 characters, roughly 7.7x the tool's own prompt budget, and it grew with the corpus rather than with the question being answered.

Prose compression was tried first and reached its floor. Sixty Phase 1 decisions were compressed against their `→ DESIGN.md §N` pointers and twelve solved questions were tombstoned, removing about 178,000 characters. Measured duplication across the whole corpus after that is under 4,000 characters. The remaining bulk is not redundant, so the budget could not be closed by editing text.

v0.4 sends only the context a selected question needs, resolved by following the reference graph this repository already maintains:

- the selected question, plus the transitive closure of its `Depends on:`;
- the decisions those questions name in `Resolved by:`, plus decisions cited in their bodies;
- the sections those cite by file-qualified `§` reference;
- a small always-include core for cross-cutting material nothing links to.

Mean prompt context falls to about 0.40x the budget from 7.7x. `prompt_size_warning` becomes a per-question signal instead of firing on every run.

Consequences for this repository, which are the load-bearing part of this decision:

1. **Section pointers are now functional, not decorative.** A question that links to nothing is answered with almost no context. The `→ DESIGN.md §N` convention already carried by 189 decisions is what makes closure work, and it must be maintained on new questions and decisions. `check` reports `QUESTION_CONTEXT_THIN` and `QUESTION_SECTION_REF_UNRESOLVED` so a missing or broken edge is visible rather than silent.

2. **Only file-qualified references resolve.** A bare `§15` is ambiguous across five source files and is ignored rather than guessed at. Write `DESIGN.md §15`, not `§15`.

3. **`DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md` is a read-only fifth source.** Phase 1b questions depend on vocabulary defined only there — `SyncStatement`, `observed_versions`, `effective_time`, `recorded_time`, `derived_state` appear zero times in `DESIGN.md`. It is read and injected but deliberately not in the patch allowlist, so the committee may reason about it and not rewrite it. Extending write authority to it would be a change to `UBU-D0176` and is not made here.

4. **The file-hygiene prompt rules are withdrawn.** v0.3 instructed models to tombstone solved questions, remove duplicate information across source files, and compress to minimum. All three require seeing the whole corpus. Under excerpts, "remove duplicate information" would instruct a model to delete text whose other copy it cannot see. Hygiene that still matters belongs in `check`, where whole files are visible. Note the consequence: nothing now instructs tombstoning of a question this process resolves, and that is a known gap.

5. **Runs stay auditable.** `manifest.prompt_context` records the exact questions, decisions, and sections a prompt was built from, and `runs/<run-id>/snapshot/` still holds the full files. A reviewer can see both what the model was shown and what the repository contained.

This does not change what is canonical. Accepted design state still exists only when a human operator commits to this repository.

---

## UBU-D0247: Phase 2 sync uses hybrid HLC, DAG, and content-addressed statements

**Status:** Accepted → DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §8, §15. Resolves `UBU-Q0140`.

Phase 2 sync ordering uses a hybrid causality mechanism rather than selecting a single scalar clock.

Canonical components:

- The durable unit is the signed or integrity-protected `SyncStatement`; accepted statements are retained as an append-only, content-addressed log or bundle record.
- Each statement has a deterministic content address computed over its canonical signed payload, excluding replica-local metadata such as `received_time` and `admitted_time`.
- Each origin Device maintains a hybrid logical clock. The HLC tick is included in the statement payload and advances on local statement creation and on admitting remote causal parents.
- `causal_parents` define the statement DAG and are the authoritative happened-before evidence across Devices.
- `observed_versions` and `observed_policy_versions` are per-object preconditions used to detect stale writes, policy races, and deterministic conflicts; they are not a replacement for the statement DAG.
- Admitted-state application order is a deterministic topological traversal of available causal parents, with ties broken by HLC tick, origin Device ID, and sync statement ID.

The HLC tick is an ordering aid and operator-review affordance, not an authority source. It never overrides missing causal parents, observed-version failures, policy failures, or invalid statement integrity. `effective_time`, `recorded_time`, `received_time`, and `admitted_time` retain their separate meanings and must not be collapsed into the causality clock.

Rejected alternatives:

- Pure Lamport clocks lose useful physical-time adjacency for review and diagnostics while still requiring deterministic tie breakers.
- Pure vector clocks are too large and privacy-leaky for N-device sync with partial, redacted, stale, or restricted replicas.
- Pure per-object counters cannot represent cross-object mutations, policy races, worker results, or conflict-resolution statements without an additional statement-level causal graph.
- Pure content-addressed bundles provide integrity and deduplication but do not, by themselves, encode happened-before ordering.

This decision is design-compatible with direct peer, local LAN, removable-file, and encrypted indirect transports because the causality evidence lives in the signed statement payload and its content address rather than in any canonical server.

---

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

## UBU-D0249: Offline deletion and redaction use explicit enforcement records

**Status:** Accepted → DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §12, §18. Resolves `UBU-Q0144`.

When a Compartment policy change requires deletion, purge, or redaction on a Device that may be offline, stale, revoked, or unreachable, UbU represents the obligation as a per-target enforcement record rather than as completed deletion.

The record contains:

- `target_device_id`, or an equivalent opaque Device reference allowed at the current visibility level;
- `policy_update_statement_id` and causal references to the tombstone, redaction, or purge request;
- `affected_object_ids` or redacted structural object references sufficient for idempotent retry and audit;
- `requested_action`: one of `redact_replica`, `purge_replica`, `apply_tombstone`, or `recalculate_derived_state`;
- `requested_representation`, when redaction rather than purge is allowed;
- `attempt_state`: one of `pending_delivery`, `delivered_unconfirmed`, `attempted_unconfirmed`, `confirmed`, `failed_retryable`, `failed_terminal`, or `user_accepted_unknown`;
- `confirmed_at` and confirmation statement reference when the target Device proves enforcement occurred;
- `exposure_state`: one of `not_exposed`, `potentially_exposed`, `confirmed_removed`, or `unknown_accepted`.

`pending_delivery`, `delivered_unconfirmed`, and `attempted_unconfirmed` all mean UbU must treat the target replica as potentially stale and potentially exposed. They may satisfy audit that UbU attempted enforcement, but they do not satisfy enforcement success.

A Device that later reconnects applies the latest admissible policy before exposing affected content, executes the requested redaction or purge idempotently, emits confirmation or failure, and recalculates derived state whose visibility changed.

Until confirmation, diagnostics and user-facing review may state only structural status such as "A protected object may still exist on an offline Device." They must not include the protected payload, the Compartment id or label, or reason strings that reveal the Compartment subject.

User-facing review may offer an explicit `user_accepted_unknown` outcome only after presenting the exposure as unknown rather than successful. This closes the immediate obligation for planning and audit purposes, but does not rewrite history as confirmed deletion.

This resolves Phase 1b representation. Transport-specific retries, Device recovery flows, and full Phase 2 deletion propagation policy remain later implementation work.

---

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
