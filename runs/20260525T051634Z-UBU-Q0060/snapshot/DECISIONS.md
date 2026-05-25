# UbU Decisions

Status: Draft  
Purpose: Lightweight architectural and data-model decision log for the UbU project.

This file records accepted design decisions so they do not need to be rediscovered from chat history. It is not a full specification. Details belong in `DESIGN.md`; unresolved matters belong in `OPEN_QUESTIONS.md`.

---


## UBU-D0001: GitHub repository is canonical for public design

**Status:** Accepted → DESIGN.md §30

The public GitHub repository is the canonical public design process for UbU.

Private chats, notes, and external documents may generate proposals, but accepted decisions become official only when reflected in the repository.

---

## UBU-D0002: Start public design documentation with four Markdown files

**Status:** Accepted → DESIGN.md §30

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

**Status:** Accepted → DESIGN.md §24.1

An **Automation Worker** is the technical execution entity.

**Super Automation** is a product/UX pattern in which UbU abstracts away difficult external interaction by using local services, Automation Workers, APIs, screenshots, photos, or LLM processing.

---

## UBU-D0018: GitHub is a projection of UbU, not the source of truth

**Status:** Accepted → DESIGN.md §26

For dogfooding, GitHub is treated as a low-dimensional projection of canonical UbU state.

UbU is the source of truth.

---

## UBU-D0019: GitHub projection requires reconciliation

**Status:** Accepted → DESIGN.md §26.4

Missed GitHub updates are expected in MVP.

UbU should support a reconciliation report comparing GitHub state to UbU state.

---

## UBU-D0020: Objective status and pipeline state are separate

**Status:** Accepted → DESIGN.md §26.2

`Objective.status` is the canonical UbU lifecycle status.

`pipeline_state` is a workflow/project-management status, such as a GitHub issue pipeline state.

---

## UBU-D0021: One GitHub object may map to many UbU objects and vice versa

**Status:** Accepted → DESIGN.md §26.1

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

**Status:** Accepted → DESIGN.md §27

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

**Status:** Accepted as MVP direction → DESIGN.md §26.3

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

**Status:** Accepted → DESIGN.md §30

The data-model discussion has reached the point of diminishing private returns.

Further unresolved questions should become public GitHub Issues when possible.

---

## UBU-D0056: File authority model for model-committee runs

**Status:** Accepted → DESIGN.md §30

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

The model-committee process may read the canonical design repo, identify unresolved questions, generate proposed answers, run consistency checks, estimate MVP readiness, score open questions, and propose patches.

The model committee does not maintain its own durable question registry. `OPEN_QUESTIONS.md` remains the canonical unresolved-question queue.

---

## UBU-D0058: Model-committee v0.1 is intentionally constrained

**Status:** Accepted → DESIGN.md §3

The first model-committee implementation should be a narrow local Python CLI, not a general model-governance platform.

v0.1 must implement:

- parse `OPEN_QUESTIONS.md`
- run consistency checks
- rank answerable questions
- generate Codex work prompt
- launch Codex CLI work provider
- run configured Ollama work models sequentially by priority
- import and validate candidate work proposals
- mechanically validate patches

---

## UBU-D0059: Model committee outputs are weighted by capability and observed reliability

**Status:** Accepted → DESIGN.md §3

Model-committee synthesis should not use equal one-model-one-vote aggregation. Models may differ in reasoning quality, context length, instruction-following, access tier, cost, and observed reliability.

Premium or deeper cloud models may receive higher default trust weights than free-tier or smaller local models.

---

## UBU-D0060: Open questions are selected by answerability, automation-likelihood, importance, and risk

**Status:** Accepted → DESIGN.md §3

After consistency checks, the model-committee process should score every open question for:

- answerability,
- automation-likelihood,
- importance,
- risk.

Question selection uses answerability as the first gate.

A question should not be selected for ordinary answer/work execution if it has unresolved dependencies, unless those dependencies are answered in the same work item.

Blocked questions may be selected for decomposition work when decomposition can produce replacement questions with fewer, simpler, or no dependencies.

After answerability is established, questions are ranked by automation-likelihood, then importance, then risk ascending.

---

## UBU-D0061: DECISIONS.md is a bounded decision-memory index

**Status:** Accepted

`DECISIONS.md` is not an unbounded historical transcript. It is a bounded decision-memory index used to preserve accepted constraints and prevent settled questions from being rediscovered.

Full decision history is preserved by git history and model-committee logs.

---

## UBU-D0062: Model-committee is a bootstrap UbU dogfooding workload

**Status:** Accepted → DESIGN.md §3

The model-committee project should begin as a constrained external bootstrap tool, but should move toward the mainline UbU implementation as soon as the mainline codebase can host it cleanly.

---

## UBU-D0063: Model-committee work is changeset-based

**Status:** Accepted → DESIGN.md §3.2

The model-committee process should not rely on a hidden external editor, VS Code agent, or other opaque implementation actor to apply accepted answers.

After the conceptual answer phase, the committee should run an explicit work phase in which models produce concrete changesets.

The work phase is followed by work scoring, work selection, local patch application in later versions, and review artifact generation before consistency checks run.

---

## UBU-D0064: Model-committee v0.1 uses a provisional filesystem log format

**Status:** Accepted → DESIGN.md §3

For v0.1, the model-committee implementation should define a provisional filesystem log format in code.

The final log and provenance format may be refined later by the model-committee process itself.

---

## UBU-D0065: Model-committee v0.1 uses provisional quorum and provider-failure rules

**Status:** Accepted → DESIGN.md §3

For v0.1, model-committee runs should use simple provisional quorum and provider-failure rules.

These rules are sufficient to begin implementation and may be refined later.

Provisional quorum rules:

- Minimum valid work proposals: 1.
- Preferred valid work proposals: 2 or more.
- A Codex work proposal should be attempted before selecting a patch.
- Ollama work proposals should be attempted sequentially by configured priority.
- If Ollama responses finish within timeout and validate, they are included in Codex scoring.
- If an Ollama provider fails, the failure is logged and the run may continue if Codex produces at least one valid work proposal.
- If no valid work proposal exists, the run writes logs and exits nonzero.

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

**Status:** Accepted → DESIGN.md §30

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

- `UBU-Q0031` is resolved for Phase 1 MVP.
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

- `UBU-Q0030` is resolved for Phase 1 release criteria.
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

- `UBU-Q0029` is resolved for Phase 1 governance.
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

- `UBU-Q0028` is resolved for Phase 1 MVP messaging and minimum implementation promises.
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

- `UBU-Q0026` is resolved for Phase 1 modeling.
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

- `UBU-Q0048` is resolved for Phase 1 commercial red-line policy.
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

Phase 1 models human-complete plan quality as derived analysis over Plans, Tasks, Logs, Snapshots, affect constraints, and risk reports. It does not add a first-class canonical PlanQuality object in MVP. Any cached assessment is advisory and recalculable.

A candidate Plan should be checked for:

- fast feedback: important work has an observable checkpoint soon enough to revise before large loss;
- checkpoint coverage: success, failure, blocked preconditions, user overrides, affect Snapshots, or External Events can reveal whether the Plan is still accurate;
- affect margin: scheduled work does not consume energy, stress, mood, or recovery capacity beyond configured user-mode limits;
- dignity preservation: failure presentation does not moralize the user, expose avoidable embarrassment, or reuse wording that frames observed limits as character flaws;
- non-blaming revision: failed execution should suggest model repair, smaller Tasks, added checkpoints, updated estimates, recovery, clarification, delegation, or Objective reconsideration;
- humane stretch: the Plan may exceed the current baseline when feedback is close, recovery margin remains, and the expected post-plan state is not worse;
- destructive pressure: the Plan depends on overriding observed limits, lacks recovery, hides failure until too late, or repeatedly requires performance above observed capacity.

Task failure remains an ordinary Log result, but the interpretation of that failure should be plan-centered. UbU should treat failure as evidence about estimates, constraints, dependencies, affect state, interruptions, or Objective fit unless the user explicitly records another interpretation.

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

**Status:** Accepted → DESIGN.md §24

Automation Worker authority is represented through explicit capability grants associated with a worker Identity. The Identity is the external-facing actor and credential subject; the capability grant is the authoritative object that says what the worker may do for a specific parent UbU instance.

Phase 1 capability verbs are `task.read`, `objective.read`, `universe_state.read_subset`, `external_event.append`, `snapshot.submit`, `mutation_request.submit`, `recalculation.request`, and `projection.github.request_update`. Direct creation or mutation of Tasks, Objectives, UniverseState, `pipeline_state`, or GitHub projection state is not granted to workers in Phase 1. Workers submit mutation or projection requests, and the canonical instance validates, applies, rejects, and logs the result.

Capability grants may be scoped by object ID, Objective subtree, Task set, Compartment, external integration, operation kind, and time window. Compartment policy is a hard upper bound on any worker grant: a grant cannot authorize cloud LLM routing, external export, worker handoff, or payload disclosure that the relevant Compartment forbids.

Workers can be revoked by disabling or deleting grants and invalidating or rotating credentials. Revocation affects future access and submissions only; prior Log entries remain append-only history. Credential rotation creates a new credential version for the worker Identity while preserving audit continuity unless the underlying grants are changed.

A worker may serve multiple organization-mode or user-mode parent instances only through separate parent-specific grants, credentials, and audit trails. Cross-parent data sharing is forbidden unless each parent explicitly grants the route and all relevant Compartment policies allow it. User-mode workers are allowed, but access to affect or other personal data must use narrow read-subset grants and obey Compartment and low-security disclosure rules.

---

## UBU-D0096: Phase 1 requires bootstrap interview and next-action focus UX

**Status:** Accepted → DESIGN.md §4.1

UbU Phase 1 must include a minimal first-person user-facing loop, not merely an internal planner, GitHub importer, model-committee loop, or project-state analyzer.

Phase 1 requires two user-facing UX primitives:

1. **Bootstrap interview**: UbU begins by asking a small number of questions that help form an initial model of the user, current context, important Objectives, relevant constraints, current or stale affect Snapshot, and immediate dogfooding/project context.

2. **Next-action focus mode**: UbU can present one recommended next Task at a time, with an explanation of why that Task matters now. The full Plan remains inspectable, but the default user experience may reduce immediate cognitive load by showing a single next action.

The Phase 1 next-action screen should include, at minimum:

- the recommended Task;
- estimated duration or work window when available;
- why this Task matters now;
- what inputs or constraints UbU considered;
- current affect or stale-affect status when relevant;

---

## UBU-D0097: Phase 1 MVP scope is frozen around single-user GitHub dogfooding

**Status:** Accepted → DESIGN.md §4.1

Phase 1 is frozen as the minimum single-user `user_mode` implementation that proves UbU can coordinate UbU's own development through explicit state, an inspectable Plan, one recommended next action, feedback, recalculation, and bounded GitHub projection.

The exact Phase 1 feature set is:

- local single-user UbU instance for UbU-runs-UbU;
- bootstrap interview, current or stale affect Snapshot handling, and initial Objective/Task seed creation;
- live or fixture-backed GitHub import for issues, PRs, reviews, CI events, milestones, comments, and source links;
- ExternalReference-style mapping between GitHub objects and UbU Objectives, Tasks, External Events, and Logs;
- MVP Objective, Preference, Task, Container, UniverseState, Snapshot, Plan, Calendar, Log, Identity, Relationship, Compartment, Automation Worker, External Event, External Reference, and deferred Association-related schemas only to the depth required for the dogfooding loop;
- schedulable Static and Dynamic Tasks with Objective links, duration, dependency/precondition/effect fields, lifecycle status, and moot handling;
- lightweight UniverseState mutation and precondition evaluation sufficient for Task effects, affect, relationship-relevant facts, and GitHub/project facts;
- append-only per-instance Logs with provenance, correction, annotation, worker-submission, and recalculation-trigger entries;

---

## UBU-D0098: GitHub external links use first-class External References

**Status:** Accepted → DESIGN.md §19

GitHub-to-UbU links are represented by first-class `ExternalReference` objects rather than by embedding all many-to-many source links directly on core objects. Lightweight `external_refs` fields may still appear on Log entries, provenance payloads, or import artifacts as convenience references, but they are not the authoritative external-link model.

MVP `ExternalReference` required fields are `external_reference_id`, `external_system`, `external_object_type`, `external_object_id`, `ubu_object_type`, `ubu_object_id`, `relation_type`, `confidence`, `created_by_identity_ref`, `created_at`, `last_verified_at`, `sync_policy`, `projection_policy`, and `provenance`.

Phase 1 external references may target Objectives, Tasks, External Events, and Log entries. This supports GitHub Issues mapping to multiple Objectives, Objectives mapping to multiple Issues, PRs and CI runs being retained as External Events, and comments or reviews acting as evidence for Tasks, Objective transitions, reconciliation, or projection decisions.

MVP relation types are `represents`, `supports`, `evidence_for`, `source_event_for`, `projection_of`, `duplicate_of`, and `supersedes`. Relation types are schema-controlled enums in MVP, not free-form labels.

Duplicate detection uses a normalized uniqueness key over `external_system`, `external_object_type`, `external_object_id`, `ubu_object_type`, `ubu_object_id`, and `relation_type`. Importers must normalize equivalent GitHub identifiers such as repository name, issue or PR number, node ID, URL, comment ID, run ID, and webhook delivery ID before comparing. Reobserving the same external reference updates verification metadata or creates an idempotent no-op Log entry rather than duplicating the external reference. Distinct relation types between the same objects are allowed.

Automation Workers cannot directly create or mutate canonical External References in Phase 1. They may submit external-reference mutation requests only through authorized `mutation_request.submit` capability grants. The canonical instance validates authority, Compartment/export policy, duplicate keys, expected prior version, and provenance, then logs applied or rejected outcomes.

---

## UBU-D0099: Psychological theory inputs are calibration, discovery, preview, and review layers

**Status:** Accepted → DESIGN.md §2.2.1

UbU should not expose internally computed utility values as user-facing truth. Derived utils remain transient computational artifacts used for scheduling, ranking, and comparison. User-facing value remains grounded in explicit Preferences, user declarations, Logs, Snapshots, review, and later correction.

Psychological and philosophical decision-theory inputs should usually enter Phase 1 through calibration, discovery, Calendar preview, Log review, reports, and reusable Tasks rather than through a large new psychology ontology.

Prospect-theory implications are accepted as preference-calibration requirements, not as an exposed utility model. UbU may show default preference examples and common-situation emotional-value examples during onboarding, Calendar preview, or Log review so the user can make more thoughtful Preference statements. These examples are grounding aids. They do not become canonical Preferences unless accepted by the user.

Temporal discounting is usually part of Preference for short-horizon Phase 1 planning. Long-horizon Objectives may later require explicit future-self or commitment-device modeling, but this is not a Phase 1 blocker.

Habit-related behavior should be handled through discovery mode, Logs, Snapshots, configured integrations, sensor-derived observations, and later clarification prompts. Discovery mode is a user-selectable workflow state that the user may choose at any time, especially from a mobile app with useful embedded sensors. Discovery mode supports later Log review and UbU-directed reconciliation for undetailed or under-specified time periods.

The Calendar is advisory. User overrides remain authoritative and should be treated as model evidence, not disobedience. Repeated overrides may trigger review, preference recalibration, Task decomposition, Objective reconsideration, or habit-pattern hypotheses only after appropriate user-visible reconciliation.

Calendar preview and Log review are notable Tasks that should run on a regular basis. They help verify whether UbU is correctly modeling the user's intended behavior, actual behavior, affective constraints, and preference judgments. These review Tasks should remain inspectable, interruptible, and adjustable by the user.

Self-determination theory and theory of planned behavior should primarily inform Calendar preview, Log review, reporting annotations, and optional user comments about motivation, autonomy, competence, relatedness, attitude, subjective norms, perceived control, and expected execution. Detailed modeling of those constructs remains open unless required by a concrete MVP workflow.

Narrative identity is partly addressed through Objectives and Reports. Social identity theory, social choice theory, and game theory are important post-MVP open-question areas and should be tracked explicitly without blocking Phase 1 implementation.

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

**Status:** Accepted → DESIGN.md §25

Organization mode uses the shared UbU core object model except where fields or behavior are intrinsically personal-affect-specific. Objective, Preference, Task, Container, UniverseState, Snapshot, Plan, Calendar, Log, Identity, Relationship, Compartment, Automation Worker, External Event, External Reference, and deferred Association-related objects are available in organization mode to the depth required by the organization or project planning workflow.

Organization mode does not model intrinsic affect. In mode-specific schemas, intrinsic affect fields are absent. In shared implementation schemas that contain affect-capable fields for storage or migration convenience, those fields are disabled in organization mode and validators must reject organization-created intrinsic-affect values rather than silently treating them as unused.

Relationship objects may exist in organization mode without affect dimensions. Organization-mode Relationships represent structured UniverseState between Identities such as contributors, maintainers, workers, projects, vendors, integrations, or external organizations. Non-affect relationship-relevant project information belongs in ordinary UniverseState facts, External Events, Logs, External References, candidate AssociationAttestations, Objectives, Tasks, or risk reports. Organization mode must not claim that the organization has a private emotional state toward another Identity.

Organization-created Objectives, Preferences, and Tasks require `authority_source` metadata. In MVP this metadata may be coarse, but it must identify the authority path for the object, such as an admin-equivalent operator Identity, imported project policy, imported external event, Automation Worker submission, or human-approved import/projection action. The detailed vocabulary remains open in `UBU-Q0013`.

Before RBAC exists, organization-mode human users are represented as operator Identities with admin-equivalent authority over the instance. This is an MVP simplification only. Their actions must still be logged with actor Identity and provenance so future RBAC can be introduced without rewriting history.

Organization-mode instances may later receive limited signals from personal user-mode instances only through explicit user-controlled sharing, Identity-mediated authorization, Compartment/export checks, and clear provenance. Phase 1 does not implement personal-to-organization cross-instance sharing. Future designs should prefer structural signals such as availability, commitment status, task completion, or user-approved projection summaries rather than raw affect, private relationship state, or broad personal context.

---

## UBU-D0102: Phase 1 bootstrap and next-action UX stays narrow and inspectable

**Status:** Accepted → DESIGN.md §4.1

The Phase 1 bootstrap interview and next-action focus UX are the first user-facing proof that UbU is more than an internal planner, GitHub importer, or automation loop. They must stay narrow, inspectable, and honest about implemented capability.

The bootstrap interview should ask only the minimum useful set of questions needed to seed the current user, context, available work window, important Objectives, relevant constraints, and current or stale affect Snapshot. It should not imply broad personal-data ingestion, therapeutic authority, complete life modeling, or autonomous coaching unless those capabilities have been separately implemented and disclosed.

The next-action focus mode should present one recommended Task at a time with a clear explanation of why that Task matters now. The full Plan must remain inspectable. One-task focus is a cognitive-load reduction pattern, not permission to hide the planner, omit Calendar preview, or turn UbU into opaque automation.

This decision is compatible with the Compact Calendar planning architecture. The default Plan supplies the inspectable ordered context behind the next-action recommendation, while reactive recalculation keeps the recommendation responsive when reality diverges. The planning layer should support the narrow UX rather than expanding Phase 1 scope beyond the single-user dogfooding loop.

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

The minimum Phase 1 bootstrap interview asks only enough to recommend one useful dogfooding Task from explicit state. The required questions are:

1. `What are you trying to move forward right now?`
2. `Which project context should UbU use for this first session?`
3. `How much usable time do you have for the next work window?`
4. `Is there a deadline, meeting, release target, or external event that changes what matters today?`
5. `What work should UbU consider first?`
6. `What is already blocked, unavailable, or not worth recommending right now?`
7. `How are your energy, stress, and mood right now?`
8. `When choosing between useful work, what should UbU favor today?`

The answers map into existing Phase 1 objects. Project context, availability, deadlines, constraints, fixture/import source, and hard blockers become UniverseState facts, External Events, Tasks, External References, and Log entries as appropriate. Affect answers become a user-declared Snapshot; skipped or stale affect data creates or prioritizes an affect-collection Task. Initial work answers seed Objectives and Tasks. Tradeoff answers become Preferences only when UbU presents an explicit Preference statement and the user accepts it; otherwise they remain Log notes, Objective annotations, or noncanonical preview/review evidence.

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

**Status:** Accepted → DESIGN.md §27

Risk reports in MVP are derived, recalculable analyses over Calendars, Plans, Tasks, Logs, Snapshots, External Events, worker status, External References, and compact Calendar metadata. Risk remains reportable state, not a first-class canonical object.

The Phase 1 MVP risk-report set is:

- `p90_completion_time`;
- `critical_path`;
- `deadline_miss_probability`;
- `affect_constraint_violation_probability`;
- `low_compact_calendar_coverage_warning`;
- `dependency_fragility`;
- `worker_or_automation_bottleneck`;
- `stale_affect_warning`;
- `destructive_pressure_warning`;
- `post_plan_depletion_warning`.

Additional findings may be computed when source data is already available, but they are not required Phase 1 reports: relationship-maintenance neglect, preference uncertainty, repeated override or deviation patterns, motivation mismatch, stale or missing discovery-mode reconciliation, and overdue Calendar preview or Log review.

Risk reports are computed on demand from the current Calendar or Plan. Implementations may cache a risk-report artifact with a Calendar, Plan, run artifact, release package, or UI view for auditability and performance. Cached reports are derived state and must be invalidated or marked stale when relevant Tasks, Logs, Snapshots, External Events, worker status, compact Calendar coverage, or recalculation triggers change.

Automation Workers may compute or refresh risk-report artifacts only through explicit capability grants. Worker-produced risk reports are advisory until admitted by the canonical instance. Workers may submit report artifacts, refresh requests, mutation requests, or projection requests according to authority; they do not create canonical Risk objects or directly mutate Tasks.

Risk reports may recommend follow-up Tasks or submit Task candidates through the normal request/review path, but they must not silently create canonical Tasks in MVP. Typical follow-ups include clarification, affect collection, dependency repair, worker retry or escalation, Calendar regeneration, Log review, Calendar preview, and GitHub projection or reconciliation.

Risk reporting is part of UbU-runs-UbU release ceremonies. Release readiness and Release Outreach Pipeline packages should include deadline risk, critical path, dependency fragility, worker or automation bottleneck, and low coverage warnings when inputs exist, while respecting Compartment, Identity, export, and public-projection boundaries.

UbU's PERT-superiority demonstration should be precise: Phase 1 should not claim to replace every PERT use. It should show that UbU handles the planning dimensions PERT leaves out: explicit preconditions and UniverseState, affect constraints, worker status, compact Calendar coverage, recalculation from Logs and Snapshots, and actionable Plan repair or next-Task recommendations.

---

## UBU-D0146: Phase 1 recalculation triggers use logged trigger records

**Status:** Accepted → DESIGN.md §28

Recalculation triggers are explicit event-like records that tell UbU when a Calendar, Plan, risk-report cache, explanation cache, or next-action recommendation may no longer reflect the current modeled state. A trigger is not a separate canonical domain mutation by itself; it references the Log entry, Snapshot, External Event, worker request, or clock condition that changed the planner's inputs.

The Phase 1 trigger kinds are:

- `task_completed`;
- `task_failed`;
- `task_moot`;
- `observed_snapshot`;
- `affect_confidence_decay`;
- `external_event`;
- `github_update`;
- `user_override`;
- `calendar_preview_due`;
- `log_review_due`;
- `discovery_mode_reconciliation_due`;
- `elapsed_time`;
- `low_compact_calendar_coverage`;
- `worker_request`.

Phase 1 user feedback controls such as snooze, reject, decompose, and override should be represented through ordinary Task, Log, or user-override payloads rather than separate trigger kinds unless implementation evidence shows that a distinct trigger kind is required.

Phase 2 trigger kinds are limited to sync and personal-worker conditions that can invalidate a local Calendar: `sync_state_changed`, `device_reconnected`, `remote_worker_status_changed`, and `offline_window_changed`. These belong with multi-device local-first sync and user-owned worker coordination.

Post-MVP trigger families include native cross-user message arrival, AssociationAttestation review, broad legacy-message ingestion, background AgentAction policy events, external compute budget or provider changes, and other high-level agentic or multi-user coordination events.

Accepted Phase 1 triggers are logged using the existing Log event type `recalculation_triggered`. The event payload should include `trigger_kind`, `scope`, `target_refs`, `source_event_refs`, `requested_action`, `batch_key`, `reason`, and optional `idempotency_key`. `requested_action` is either `recalculate_now` or `mark_calendar_stale`.

Automation Workers cannot directly create canonical triggers. A worker with the `recalculation.request` capability may submit a recalculation request. The canonical instance validates capability, target scope, Compartment/export policy, source evidence, and idempotency before recording an accepted `recalculation_triggered` Log entry or a rejected worker request/mutation Log entry.

Triggers may be batched when they share the same Calendar or Plan scope and no trigger in the batch requires user-visible immediate repair before the batch window closes. Batching must preserve each source reference and reason. A short event-loop batch is acceptable for multiple imported GitHub events, worker updates, or stale markers.

Immediate recalculation is required when the trigger can change the current or next recommended Task, hard feasibility, Task lifecycle state, dependency or precondition truth, Objective status, affect legitimacy, worker assignment needed for current work, or compact Calendar coverage below the configured minimum.

Default Phase 1 immediate triggers are:

- `task_completed`, `task_failed`, and `task_moot` for current, planned, dependency-relevant, or Objective-relevant Tasks;
- `user_override`;
- planner-relevant `observed_snapshot`;
- planner-relevant `external_event`;
- planner-relevant `github_update`;
- `low_compact_calendar_coverage`;
- urgent accepted `worker_request`.

Stale marking is sufficient when the trigger only means cached Plans, Calendars, explanations, or reports must be refreshed before later reliance. Default stale-only triggers are:

- `calendar_preview_due`;
- `log_review_due`;
- `discovery_mode_reconciliation_due`;
- `elapsed_time` outside the reactive repair envelope;
- `affect_confidence_decay` that has not crossed a configured planning threshold or current affect constraint;
- low-priority `external_event` or `github_update` observations that do not affect the current Plan.

`elapsed_time` is materialized only when crossing a modeled boundary such as Task start or end, Static Task proximity, review due time, stale-affect threshold, reactive horizon expiry, offline precompute boundary, or compact Calendar expiration. It does not create continuous clock-tick Log entries.

---

## UBU-D0147: Moot reason codes are a closed MVP enum

**Status:** Accepted → DESIGN.md §9.5

`moot` remains a first-class terminal Task status that is functionally equivalent to completion for planning but distinct for logs, reporting, audit, and user review. Every Task transition to `moot` requires a reason code.

The Phase 1 MVP moot reason-code enum is:

- `externally_satisfied`;
- `superseded`;
- `delegated`;
- `no_longer_relevant`;
- `invalidated_by_universe_change`;
- `replaced_by_new_plan_structure`;
- `user_declared_moot`;
- `automation_obsolete`;
- `duplicate`.

This list is sufficient for MVP. It covers externally completed work, supersession, delegation, user-directed closure, world-state invalidation, plan-structure replacement, obsolete automation output, and duplicate imported or generated work. Additional nuance belongs in the `task_moot` Log entry's notes, reason text, provenance, External References, or correction/annotation entries rather than in ad hoc canonical codes.

`duplicate` is included because duplicate Tasks are common when importing GitHub Issues, PRs, comments, fixtures, worker outputs, or decomposed work. Duplicate closure must remain queryable and auditable rather than being collapsed into generic supersession.

`delegated` is separate from `externally_satisfied`. `delegated` means this Task is no longer assigned to this executor because responsibility moved to another executor, worker, Identity, Association, or delegation path. It does not assert that the underlying Objective or required world state is already satisfied. `externally_satisfied` means the required state became true through another action, observation, import, or external event.

Reason codes are enum-only in MVP. Implementations may accept human-readable notes and implementation-local diagnostic labels, but canonical `moot_reason_code` values must be one of the accepted enum values. New canonical reason codes require an explicit schema migration or accepted decision.

Selection guidance:

- use `externally_satisfied` when the required state is already true;
- use `superseded` when a newer Task, Objective, decision, or source artifact replaces this Task;
- use `delegated` when responsibility moved to another executor and will be tracked elsewhere;
- use `no_longer_relevant` when the Task is no longer useful but not invalidated by the world;
- use `invalidated_by_universe_change` when an external state change made the Task wrong or impossible;
- use `replaced_by_new_plan_structure` when decomposition, regrouping, or restructuring replaced this Task while preserving the underlying intent;
- use `user_declared_moot` when the user explicitly says the Task is moot and no more specific code applies;
- use `automation_obsolete` when worker or automation state makes generated work obsolete;
- use `duplicate` when another active, completed, or canonical work item already represents the same work.

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

Resolved question: `UBU-Q0016`.

Compact Calendar planning for Phase 1 uses a staged explicit grammar rather than a bare DFS grammar.

The minimum skeleton Plan representation includes:

- Static Task placements with fixed start and end times;
- dependency DAG frontier for the current planning scope;
- prerequisite roots whose required state is not already true in the initial UniverseState;
- ordered prerequisite chains with dependency and precondition references;
- initial UniverseState assumptions, including known true, known false, assumed, and unresolved planner-relevant facts;
- unsatisfied dependency diagnostics with failing Task, missing state, causal chain, and clarification alternatives where available.

Skeleton generation must check the initial UniverseState before inserting a prerequisite Task. A dependency requires a state, not necessarily a new Task. If skeletonization cannot produce a viable baseline, ordinary planning stops and UbU reports a concrete diagnostic instead of continuing into optimization.

Legitimization adds the minimum human-viability constraints and support work required to make the skeleton plausible for execution:

- affect constraints in `user_mode`;
- recovery, break, meal, sleep, and rest requirements when applicable;
- transition buffers;
- setup and teardown time;
- context-switch limits;
- slack and dependency-fragility thresholds.

The legitimized skeleton baseline is the minimum feasible Plan. Support work inserted by legitimization is represented as ordinary Tasks or buffers with explanation lineage, not as hidden planner magic.

The minimum candidate Plan representation after legitimization includes:

- materialized Task placements;
- decision envelopes for movable Tasks;
- Plan probability metadata;
- value score;
- legitimacy threshold result or score;
- hard-validation status;
- explanation lineage back to Objectives, dependencies, preconditions, affect constraints, worker status, risk findings, and probability inputs.

Execution profiles are allowed as follows:

- `baseline`: greedy mean-duration planning is required as a benchmark and fallback reference.
- `mobile_local`: deterministic skeletonization, conservative legitimization, exact hard-constraint checks, local repair recipes, basic decision envelopes, cached explanations, and short-horizon BFS-like branch or repair reconstruction are required.
- `desktop_or_worker`: DFS-like candidate construction, local search, larger branch horizons, cached subplans, and solver-backed finalist validation are allowed.
- `solver_validation`: CP-SAT, SMT/MaxSMT, local-search, or comparable exact/conservative solvers may validate finalists, diagnose contradictions, or certify hard constraints.
- `gpu_or_hosted`: GPU-friendly scoring, stochastic simulation, affect scoring, robustness scoring, and learned-model inference may propose or rank candidates, but exact or conservative validation must certify selected Plans.

Search methods are implementation techniques, not semantic authority. DFS-like, BFS-like, greedy, solver-backed, GPU-friendly, and local repair methods may be combined as long as hard constraints, explanations, and selected-Plan validity remain inspectable.

Plan probability is represented as probability metadata with:

- a scalar display probability;
- a log probability for stable computation;
- an optional probability interval;
- a provenance expression over modeled probabilistic inputs;
- correlation-group, scenario, joint-distribution, or shared-random-variable references when inputs are not independent.

Implementations must not multiply probabilistic inputs unless independence is explicitly declared by the model. Correlated or unknown relationships must use joint scenarios, shared random variables, correlation groups, or conservative intervals rather than pretending independence. MVP may be conservative and mark correlation unknown instead of producing false precision.

Compact Calendar encoding should include, when present in the planning scope:

- skeleton Plan metadata;
- legitimized skeleton baseline;
- ordering constraints;
- duration PDFs;
- Task success probabilities;
- external-event and interruption distributions;
- affect constraints;
- decision envelopes;

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
