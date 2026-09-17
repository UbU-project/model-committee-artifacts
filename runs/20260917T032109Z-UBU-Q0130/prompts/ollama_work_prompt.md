# Model-Committee Work Proposal Request

You are participating in the UbU `model-committee` process.

Your task is to produce one concrete work proposal as strict JSON.

Do not return prose outside the JSON object.

## Selected question

Question ID: `UBU-Q0130`  
Question title: `Phase 1b mutation envelope causality and idempotency metadata`  
Base commit: `52539e0d38ef969bc76b00c2b7f11f4cb4c95e32`

```markdown
## UBU-Q0130: Phase 1b mutation envelope causality and idempotency metadata

Status: Open Priority: MVP blocker Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0140, UBU-Q0131 Blocks: Phase 1b mutation paths, Phase 2 sync statement compatibility Resolved by: None Last scored: 2026-09-16 Scored from commit: None

Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §4, §8, §9, §15.

### Question

What causality and idempotency metadata must every Phase 1b canonical mutation carry so that a single-Device Phase 1b mutation can later be wrapped as a Phase 2 `SyncStatement` without reshaping the emitted artifact?

### Subquestions

1. Which of the mandated fields are required on every Phase 1b mutation: idempotency key, `observed_versions`, origin Device, actor Identity, `authority_source`, and created, effective, and recorded timestamps?
2. How is `observed_versions` represented against the existing single integer version counter on canonical objects?
3. What abstract seam defers the Phase 2 ordering mechanism (hybrid logical clocks, Lamport clocks, vector clocks, per-object counters, signed append-only log, or content-addressed bundles) without leaking that choice into Phase 1b call sites?
4. How are `effective_time` and `recorded_time` distinguished for an unattended batch advisory run that proposes overnight and is admitted the following morning?
5. What is the deterministic duplicate-handling rule for a replayed idempotency key within a single Device?
6. Do derived artifacts (Plan, Calendar, reports) carry envelope metadata, or are they excluded as `derived_state`?

### Current direction

Phase 1b carries the frozen field set on every mutation but does not choose the causality mechanism, which remains open under UBU-Q0140 and must not be prematurely settled here. The ordering mechanism sits behind a trait so Phase 2 can supply its chosen clock without reshaping anything Phase 1b emits. Bitemporality is load-bearing in Phase 1b rather than theoretical, because unattended advisory batch runs propose at a materially different time than the operator admits, and clarify sessions span days. This is the first Phase 1b ticket's governing constraint: no later mutation path may emit an unshaped mutation.

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
- next decision id: `UBU-D0258`

`DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md` is read-only context and is not in the patch
allowlist: do not propose changes to it.

### OPEN_QUESTIONS.md — related questions

#### UBU-Q0131

```markdown
## UBU-Q0131: Phase 1b Device and execution-context identity

Status: Solved Priority: MVP blocker Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: UBU-Q0148 Blocks: Phase 1b origin-Device attribution, Phase 2 Device registry Resolved by: UBU-D0257 Last scored: 2026-09-16 Scored from commit: None

### Question

What is a Device in Phase 1b, and how are physical devices, UbU Devices, and execution contexts distinguished so that origin-Device attribution on every mutation is stable and meaningful with a single operator on a single machine?

### Subquestions

1. Is a Phase 1b Device a persisted canonical object, a configuration record, or a derived identity?
2. Are the desktop instance, the CLI process, the unattended batch runner, and the advisory worker one Device or several execution contexts under one Device?
3. What Device identifier remains stable across reinstallation, database reset, and re-authorization of external integrations?
4. Does the Google Calendar surface count as a Device, an external projection surface, or neither?
5. What minimum Device registry must exist in Phase 1b, given that a registry of one must not encode assumptions that break at two or many?

### Current direction

Phase 1b has one Device but must attribute every mutation to it explicitly, because canonical state is derived from valid statements rather than from machine hierarchy and no Device is automatically the canonical source of all state. The Quick UbU assumption that the desktop is authoritative and the phone is a command-only client was a correct single-machine simplification and must not be re-encoded as an invariant. The Google Calendar surface is a projection, not a Device. Any Phase 1b answer must avoid a design that only works for one or two Devices.

### Resolution

Resolved by `UBU-D0257`: Phase 1b uses one persisted registered Device record for the operator-controlled execution enclave, with a stable `device_id` restored from local registration material rather than derived from hardware, app install, database contents, or external integration authorization. The desktop UI, CLI, unattended batch runner, and local advisory worker are execution contexts under that Device unless separately admitted with their own Device authority; Google Calendar is an external projection surface, not a Device. The Phase 1b registry may contain one entry but must use the future multi-Device shape, explicit trust/capability/Zone/Compartment fields, and default-deny semantics.

---
```

#### UBU-Q0140

```markdown
## UBU-Q0140: Phase 2 causality mechanism for sync ordering

Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0130, Phase 2 sync statement compatibility Resolved by: UBU-D0247 Last scored: 2026-09-16 Scored from commit: None

Formerly UBU-QSYNC-002 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §8, §15.

### Question

What exact causality mechanism should Phase 2 use: hybrid logical clocks, Lamport clocks, vector
clocks, per-object version counters, content-addressed logs, or a hybrid?

### Resolution

Resolved by `UBU-D0247`: Phase 2 uses a hybrid causality stack: content-addressed signed sync statements form the durable append-only log; every statement carries a per-origin hybrid logical clock tick for compact total-order tie breaking and queue progress; `causal_parents` preserve the statement DAG; `observed_versions` and `observed_policy_versions` are per-object/version preconditions used for deterministic conflict detection, not the sole clock.

This rejects a pure Lamport clock because wall-time adjacency is useful for operator review, a pure vector clock because N-device and partial replicas make vectors too wide and leaky, and pure per-object counters because cross-object mutations and policy updates must retain causal edges.

---
```

#### UBU-Q0148

```markdown
## UBU-Q0148: Distinguishing physical devices, UbU Devices, installs, and workers

Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0131, Phase 2 Device registry Resolved by: UBU-D0251 Last scored: 2026-09-16 Scored from commit: None

Formerly UBU-QSYNC-010 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §4, §5.

### Question

How should UbU distinguish physical devices, UbU Devices/execution enclaves, app installs, browser
sessions, and worker processes? Can one physical machine host multiple UbU Devices/enclaves, and
under what Identity/Compartment conditions?

### Resolution

Resolved by `UBU-D0251`: UbU Device identity is per registered execution enclave, not per physical machine, install, browser session, or process. Those substrates become separate UbU Devices only when intentionally admitted with their own `device_id`, trust state, capability profile, Zone membership, authorized Identity association, and effective Compartment access. One physical machine may host multiple Devices when isolation, Identity authority, and Compartment policy support treating each enclave independently. Projection-only app installs or browser sessions are not Devices, and workers are Devices only when they hold independent policy-scoped execution authority.

---
```

### DECISIONS.md — cited decisions

#### UBU-D0247

```markdown
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
```

#### UBU-D0251

```markdown
## UBU-D0251: Device identity is per execution enclave, not per physical machine

**Status:** Accepted → DESIGN.md §23.1. Resolves `UBU-Q0148`.

A UbU Device is a registered execution enclave. A physical machine, app install, browser session, or worker process is not automatically a Device; it is a substrate or runtime context until UbU admits it as a separately identifiable enclave with its own `device_id`, trust state, capability profile, Zone membership, and effective Compartment access.

One physical machine may host multiple UbU Devices when each enclave is intentionally registered, has isolation appropriate to its claimed trust and capabilities, is associated with an authorized Identity, belongs to exactly one Zone, and receives only policy-authorized Compartment knowledge and payload access. Separate OS profiles, containers, VMs, secure enclaves, browser profiles, removable-import environments, and worker runtimes may therefore be separate Devices.

An app install or browser session that only presents another Device's state is a projection surface or execution context, not a Device. A worker process is a Device only when it holds independent execution authority and participates under Device policy; otherwise it is a child process of the controlling Device. Worker candidate mutations still require normal admission and cannot expand Identity, Zone, or Compartment authority.

---
```

#### UBU-D0257

```markdown
## UBU-D0257: Phase 1b has one persisted Device and several execution contexts

**Status:** Accepted → DESIGN.md §23.1. Resolves `UBU-Q0131`.

Phase 1b has one UbU Device: the registered operator-controlled execution enclave that owns the local StateStore and admission path. It is a persisted registry record, not a physical-machine identity, not an app-install identity, not a browser/session identity, and not a value derived from the database contents or external integrations.

The Phase 1b `device_id` is assigned at Device registration and restored from operator-controlled local registration material, such as a config or secret-store record outside the ordinary mutable database. Reinstallation, database reset, and external integration reauthorization preserve Device continuity only when that registration material is preserved or restored. If it is lost, the next installation is a new Device and must not claim the old `device_id` merely because it runs on the same physical machine.

Desktop UI sessions, CLI processes, unattended batch runs, and the local advisory worker are execution contexts under the Phase 1b Device unless one is explicitly admitted later as its own Device with independent trust, capability, Zone, Identity, and Compartment policy. Their mutations and candidate mutations use the controlling Device's `origin_device_id`; context ids, process ids, request ids, backend ids, and model provenance may refine auditability but do not confer Device authority.

External projection surfaces, including Google Calendar, are not Devices in Phase 1b. They may receive projected state, provide external events or integration evidence, and appear in provenance as integration or projection surfaces, but they do not receive a UbU `device_id` and cannot be the origin Device for admitted mutations.

The minimum Phase 1b Device registry may have one row, but it must be modeled as the future registry rather than as a singleton constant. It records `device_id`, label/kind, registration metadata, registered Identity association, trust state, sync state, exactly one Zone membership, capability profile, effective Compartment access summary, and last-seen or local-observed timestamp. The single entry is not proof of global authority, canonical-source status, or permission to know every Compartment; unlisted Compartment access remains default-denied.
```

### DESIGN.md — referenced sections

#### DESIGN.md §23.1 — Device

```markdown
### 23.1 Device

A **Device** is an execution enclave, not necessarily physical hardware. A physical machine, app install, browser session, and worker process are substrates or runtime contexts; they become UbU Devices only when admitted as separately identifiable execution enclaves with their own `device_id`, trust state, capability profile, Zone membership, and effective Compartment access.

Examples:

- OS user profile
- container
- VM
- secure enclave
- browser profile or session
- worker process or worker runtime

One physical machine may host multiple Devices when each enclave is intentionally registered, isolated enough for its trust and capability claims, associated with an authorized Identity, assigned to exactly one Zone, and granted only the Compartments it is allowed to know about. App installs and browser sessions that merely render or project another Device's state are projection surfaces or execution contexts, not Devices. Workers are Devices only when they hold independent execution authority under policy; otherwise they are child processes of the controlling Device.

In Phase 1b, the dogfooding installation has exactly one registered Device: the operator-controlled execution enclave that owns the local StateStore and admission path. The desktop UI, CLI invocation, unattended batch runner, and local advisory worker are execution contexts under that Device unless a later registration explicitly grants one of them independent Device authority. Every mutation they emit or propose carries the same `origin_device_id`; execution-context identifiers may appear as provenance but do not create authority.

The Phase 1b `device_id` is a stable registered identifier restored from operator-controlled local registration material, such as a config or secret-store record outside the ordinary mutable database. It is not derived from physical hardware, app installation identity, database contents, Google authorization, or other external integration credentials. Reinstalling the app, resetting the database, or reauthorizing integrations therefore preserves the Device only when the registration material is preserved or restored; otherwise the installation must register as a new Device rather than claiming continuity.

The minimum Phase 1b Device registry may contain a single entry, but it uses the same shape required for later registries: `device_id`, label/kind, registration metadata, registered Identity association, trust state, sync state, exactly one Zone membership for the Device, capability profile, effective Compartment access summary, and last-seen or local-observed timestamp. Missing Compartment access is implicit denial, and no code path may special-case the registry cardinality as proof that the Device is canonical, omniscient, or authoritative over projected surfaces such as Google Calendar.
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

#### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §5 — Device model **(net-new)**

```markdown
## 5. Device model **(net-new)**

ID and ref naming follows the existing schema convention: identities are `*_id`,
references to objects are `*_ref` / `*_refs`. Compartment permissions reuse the
`compartment_ids` noun rather than an invented `allowlist`/`denylist` term.

```json
{
  "device_id": "dev_laptop_001",
  "device_label": "Sean's Laptop",
  "device_kind": "laptop",
  "registered_at": "2026-06-01T10:00:00-04:00",
  "registered_by_identity_id": "identity_user_main",
  "trust_state": "trusted",
  "sync_state": "active",
  "zone_memberships": ["zone_personal", "zone_ubu_project"],
  "capability_profile": {
    "can_store_full_state": true,
    "can_store_partial_state": true,
    "can_run_planner": true,
    "can_run_worker_jobs": true,
    "can_hold_external_tokens": true,
    "can_receive_notifications": true,
    "can_use_cloud_llm": false
  },
  "effective_compartment_access": [
    {
      "compartment_id": "comp_public",
      "max_replication_level": "full",
      "offline_cache_allowed": true,
      "worker_access_allowed": true,
      "diagnostic_visibility": "full"
    },
    {
      "compartment_id": "comp_personal",
      "max_replication_level": "metadata_only",
      "offline_cache_allowed": false,
      "worker_access_allowed": false,
      "diagnostic_visibility": "generic_only"
    }
  ],
  "last_seen_at": "2026-06-01T12:00:00-04:00"
}
```

`effective_compartment_access` is an illustrative cached/effective policy summary, not
the complete source of authority. Final replication eligibility is still evaluated from
the object, Zone, Compartment policy, Device capability profile, policy versions, and
user policy updates.

**A Device's effective-access summary enumerates only Compartments the Device is
authorized to know about.** Compartments the Device is denied are NOT listed — not even
with `max_replication_level: none` — because the summary itself replicates to (or lives
on) the Device it describes, and naming a denied Compartment by its real `compartment_id`
would hand that Device the identity and existence of a Compartment it is not authorized
to know. Denial of any unlisted Compartment is implicit under default-deny. This is a
direct consequence of the redaction-identity invariant (§10) and security invariant #13
(§24): no real Compartment id or label may reach a Device that the Compartment denies,
and a Device's own registry record is not exempt.

Required `trust_state` values: `unregistered`, `trusted`, `limited`, `stale`, `revoked`,
`lost`, `retired`.

Required `sync_state` values: `active`, `offline`, `pending_sync`, `syncing`,
`sync_failed`, `stale`, `revoked`.

---
```

#### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §8 — Sync statement model **(net-new)**

```markdown
## 8. Sync statement model **(net-new)**

The sync statement is the primary transport-independent unit. `actor_identity_id` and
`authority_source` reuse the existing provenance vocabulary (`AuthoritySource` enum):
every statement carries the same provenance backbone already present on `TaskSpec` and
`LogEntry`.

```json
{
  "sync_statement_id": "syncstmt_01JXYZ",
  "origin_device_id": "dev_phone_001",
  "actor_identity_id": "identity_user_main",
  "authority_source": "user",
  "zone_id": "zone_personal",
  "statement_kind": "mutation",
  "object_refs": ["task_123"],
  "compartment_ids": ["comp_personal"],
  "observed_versions": { "task_123": "v17" },
  "observed_policy_versions": { "comp_personal": "v12" },
  "causal_parents": ["syncstmt_01JXYA"],
  "idempotency_key": "dev-phone-task123-update-20260601T120000",
  "payload_ref": "encrypted_blob_ref_456",
  "effective_time": "2026-06-01T12:00:00-04:00",
  "recorded_time": "2026-06-01T12:00:02-04:00",
  "transport": "encrypted_rclone",
  "integrity": { "signature": "..." }
}
```

Note: `effective_time` is the existing schema field (on `PlanningRequest`, `LogEntry`,
`UniverseStateSnapshot`). `recorded_time` **(net-new)** is the Phase 2 companion that
records when the statement entered the local queue; the pair distinguishes when something
happened from when it was logged. `received_time` and `admitted_time` are replica-local
metadata, not necessarily signed statement payload: `received_time` records when a
Device first received the statement, and `admitted_time` records when that Device
admitted it into its local admitted-state view.

Required `statement_kind` values:

```text
mutation
policy_update
tombstone
redaction
projection_record
conflict_resolution
device_registration
device_revocation
checkpoint
worker_result
diagnostic_summary
```

Required `transport` values (sync semantics must be identical across all of them):

```text
direct_peer
local_lan
removable_file
encrypted_rclone
encrypted_rsync
cloud_relay_user_selected
manual_import
```

Phase 2 need not support every listed transport. It must support at least one direct
or local sync path and must be design-compatible with at least one encrypted indirect
transport. Whether encrypted rclone/rsync compatibility is implemented or only
fixture-certified remains `UBU-Q0141`.

`cloud_relay_user_selected` is permitted only as a user-selected transport for
encrypted sync statements or opaque bundles. It must not become a required canonical
server, must not hold plaintext unless explicitly authorized by policy, and must not
become the source of admitted state.

---
```

#### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §9 — Canonical state as admitted event state

```markdown
## 9. Canonical state as admitted event state

Admitted UbU state is the deterministic result of applying all admitted sync statements
**and conflict-resolution statements** according to this contract. No Device is
automatically the canonical source of all state. Devices contribute statements; the
admitted state is derived from valid statements, not from machine hierarchy.

### Determinism is over the resolution-inclusive closure

This is the explicit reconciliation of the determinism claim with the manual-review
reality of §17.

```text
Determinism holds over the closure of statements that INCLUDES conflict_resolution
statements. A conflict_resolution statement is itself a first-class sync statement
(statement_kind: conflict_resolution) carrying origin Device, actor Identity, and
authority_source, exactly like any other statement.

Until the conflict_resolution statement(s) for a contested region have been admitted,
that region is NOT admitted_state. It is pending_state or candidate_state. The word
"deterministic" applies only once the resolution events are part of the closure.
```

Consequences:

* A human merge decision is **not** outside the event model. It is recorded **as an
  event** (`conflict_resolution`) and participates in causality like any mutation.
* Two honest Devices may each hold a *replica* they believe is current, but neither may
  represent a contested region as `admitted_state` before the resolution event lands.
  Pre-resolution, the correct state category is `pending`/`candidate`, never `admitted`.
* This keeps "deterministic" honest: the reducer is a pure function of a statement set
  that includes resolution statements, not a function that secretly depends on
  out-of-band human choices.

State categories:

| State category     | Meaning                                                     |
| ------------------ | ----------------------------------------------------------- |
| `admitted_state`   | Deterministically accepted state (resolution-inclusive).    |
| `replica_state`    | A Device's local view.                                      |
| `pending_state`    | Locally queued changes not yet reconciled.                  |
| `candidate_state`  | Proposed changes not yet accepted; includes contested regions awaiting resolution. |
| `derived_state`    | Computed Plan, Calendar, risk, or summary.                  |
| `projection_state` | External-system representation.                             |
| `redacted_state`   | Restricted representation for unauthorized Devices.         |

---
```

#### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §15 — Versioning, causality, and idempotency

```markdown
## 15. Versioning, causality, and idempotency

The contract requires: stable object IDs; stable Device IDs; stable Zone IDs; stable
Compartment IDs; per-object version references (`observed_versions`); origin Device on
every statement; actor Identity and `authority_source` on every statement; observed prior
versions; causal parents; idempotency keys; created/effective/recorded timestamps;
deterministic duplicate handling; and observed policy versions for any Compartment,
Zone, projection, or routing policy relied on by the statement.

Candidate mechanisms (not yet mandated — see `UBU-Q0140`): hybrid logical clocks,
Lamport clocks, vector clocks, per-object version counters, signed append-only event log,
content-addressed sync bundles. Phase 2 must not adopt a design that only works for two
Devices.

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
- When editing `OPEN_QUESTIONS.md`, anchor hunks with the selected question heading `## UBU-Q0130: Phase 1b mutation envelope causality and idempotency metadata` and its own `### Resolution` section. Do not use a repeated `### Resolution` heading from an earlier or later question as the edit location.
- If resolving the selected question, replace the `Unresolved.` text under that selected question's `### Resolution` section and update that same question's metadata line. Do not insert selected-question resolution text into any other question block.
- Use the existing `UBU-Qxxxx` and `UBU-Dxxxx` numbering conventions.
- Prefer minimal, auditable changesets.
- If the selected question is blocked, propose decomposition only if it produces replacement questions with fewer, simpler, or no dependencies.
- If the selected question is already partially resolved, narrow or clarify it rather than pretending it is fully unresolved.

Return only JSON.
