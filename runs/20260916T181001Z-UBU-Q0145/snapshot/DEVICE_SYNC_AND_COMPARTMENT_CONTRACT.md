# DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md

> **Contract identifier:** `device-sync-contract/0.1`
> **Sibling of:** `planning-kernel-contract/0.1` (frozen; implemented in Phase 0 under
> profile `planning-kernel-contract/phase0-profile/0.1`).
> **Status:** Phase 2 contract boundary. Binding for Phase 2 implementation; advisory
> (guardrail-only) for Phase 1/Phase 0.
> **Reconciliation note:** This contract reuses existing UbU schema vocabulary wherever
> it exists. Terms that are net-new to Phase 2 — `Device`, `Zone`, `Replica`,
> `SyncStatement`, `tombstone`, `recorded_time` — are marked **(net-new)** on first use.
> See [Appendix A](#appendix-a--schema-reconciliation) for the full invented→real mapping.

---

## 1. Purpose

This contract defines the minimum semantics required for UbU Phase 2 multi-device
operation. It specifies how Devices exchange sync statements, how partial replicas are
represented, how Compartment policy constrains replication, how offline mutations are
admitted or rejected, and how sync events affect Plan and Calendar legitimacy.

This contract is implementation-facing. Phase 2 code, schema definitions, test fixtures,
and mobile/desktop sync behavior should conform to it.

Where `planning-kernel-contract/0.1` answers:

> Given a valid UbU state, Tasks, constraints, affect assumptions, and planning policy,
> what must the planner produce?

this contract answers:

> Given multiple user-authorized Devices, partial replicas, offline operation, Zone
> boundaries, Compartment policies, and sync statements from different origins, what
> state may be exchanged, admitted, redacted, rejected, or reconciled?

Its central purpose is to prevent Phase 2 from becoming an ad hoc sync layer bolted onto
Phase 1. Sync is defined here as a first-class UbU semantic layer, not file copying,
database replication, or mobile app state sharing.

### Core Phase 2 claim

> **Phase 2 proves that one user can run UbU coherently across multiple trusted devices
> and personal workers without relying on a central canonical server, without assuming
> one permanent primary device, and without violating Compartment, privacy, or export
> boundaries.**

This is the bridge between Phase 1 (one local single-user dogfooding instance), Phase 2
(one user, arbitrary devices, local-first sync), and Phase 3+ (multi-user,
Association-aware, Resource/Skill-aware coordination).

---

## Central invariants

These two invariants govern the entire contract and are stated before anything else.

### Sync-sovereignty invariant

```text
A Phase 2 UbU system may synchronize through direct peers, local network exchange,
removable/imported bundles, or user-selected encrypted storage transports. It need not
maintain more than one live synchronization session at a time. However, its state model,
sync statements, causality metadata, Device registry, conflict handling, and Compartment
propagation rules must support an arbitrary number of Devices and must not depend on a
centralized canonical server or permanent primary Device.
```

### Compartment-safety invariant

```text
No synchronization path may silently expose object payloads, derived sensitive content,
projection records, diagnostics, conflict explanations, OR Compartment identity/labels
across Device, Zone, Compartment, Identity, retention, or export boundaries that current
policy does not allow.
```

The Compartment-safety invariant is strengthened from earlier drafts: it now explicitly
covers **Compartment identity and labels**, not only payloads. See §10 and §12.

---

## 2. Scope and non-scope

### In scope

```text
Phase 2 includes:

- arbitrary-number-of-Devices architecture;
- one-user multi-device synchronization;
- partial replication;
- Device registration and revocation;
- Zone-aware replication boundaries;
- Compartment-aware replication eligibility;
- offline mutation queues;
- sync statements as transport-independent state-change envelopes;
- conflict detection and conflict review;
- sync-triggered Plan/Calendar invalidation;
- user-owned worker Device coordination;
- support for encrypted indirect sync transports;
- redacted Calendar and Task views on restricted Devices.
```

### Out of scope

```text
Phase 2 does not include:

- multi-user shared UbU state;
- Association workspace synchronization;
- Skill Barter marketplace operation;
- full organization-mode administration;
- arbitrary third-party app bidirectional sync;
- full enterprise policy management;
- cloud-hosted canonical UbU as a required architecture;
- proof of physical deletion from offline Devices;
- hardware attestation;
- full active peer-to-peer mesh unless later adopted by Phase 2 research.
```

Phase 2 is **multi-device single-user sync** — not "all integrations" and not
"multi-user UbU."

---

## 3. Phase 2 sync model

Phase 2 implements single-user, multi-device, local-first synchronization **without**
assuming a centralized canonical server, centralized authoritative instance, or single
always-online planning authority.

A UbU user may operate multiple Devices. Each Device may hold a partial or full replica
of some UbU state, subject to Zone and Compartment policy. Devices exchange
integrity-protected sync statements. Replication may occur directly between Devices,
through local network discovery, through removable/imported sync packages, or through
user-selected storage transports such as encrypted rclone- or rsync-compatible stores.

No Device is inherently the permanent canonical source merely because it is a laptop,
desktop, phone, worker, or home server. Authority is operation-specific, object-specific,
policy-specific, and causality-dependent.

> A Device is authoritative only for the statements it is allowed to make, not for the
> user's entire UbU state.

### Live session cardinality vs Device cardinality

These are two different limits and must not be conflated:

| Limit                       | Phase 2 value                          | May change later? |
| --------------------------- | -------------------------------------- | ----------------- |
| **Device cardinality**      | Arbitrary (N). Hard data-model rule.   | No — never relax. |
| **Live session cardinality**| May be 1. Implementation convenience.  | Yes — raise freely.|

Phase 2 may require **no more than one live synchronization session at a time**. It must
not require any fixed number of Devices beyond one registered Device for bootstrap,
but the architecture must support arbitrary Devices. The earlier "one connected peer at a time" phrasing
is retired because indirect transports (encrypted rclone/rsync stores, importable
bundles) are store-and-forward fan-in/fan-out across all N Devices — there is no live
peer in that path at all. The constraint that actually matters is *live session
cardinality*, not peer count. Nothing in the schema, identifiers, causality metadata,
Compartment policy, or sync-statement format may assume a maximum of two Devices.

---

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

## 6. Zone model **(net-new)**

A Zone is a replication context, not a server.

```json
{
  "zone_id": "zone_ubu_project",
  "zone_label": "UbU Project",
  "zone_kind": "project",
  "allowed_device_ids": ["dev_laptop_001", "dev_desktop_worker_001", "dev_phone_001"],
  "default_compartment_policy": "deny_unless_allowed",
  "replication_policy": "partial_by_compartment",
  "external_projection_policy": {
    "github_allowed": true,
    "google_calendar_allowed": false
  }
}
```

Zone semantics:

* Devices may belong to multiple Zones.
* Objects may be scoped to one or more Zones.
* Zone policy may further restrict Compartment routing.
* Zone membership does not override Compartment denial.
* Compartment allowance does not override Zone denial.
* Zone policy cannot silently weaken a Compartment policy.

---

## 7. Compartment model

A Compartment policy object must define replication, routing, retention, and export
policy. It references compartments by `compartment_id` (singular of the existing
`compartment_ids`).

```json
{
  "compartment_id": "comp_relationship_private",
  "label": "Relationship private",
  "sensitivity": "high",
  "replication_policy": {
    "default_replication": "none",
    "allowed_device_ids": ["dev_laptop_001"],
    "allowed_zone_ids": ["zone_personal"],
    "allow_offline_cache": false,
    "allow_worker_access": false,
    "allowed_replication_levels": ["none", "existence_only", "metadata_only"]
  },
  "routing_policy": {
    "no_cloud_llm": true,
    "no_external_export": true,
    "redaction_required_for_projection": true
  },
  "retention_policy": {
    "default_retention": "indefinite",
    "deletion_mode": "tombstone_then_best_effort_purge"
  }
}
```

Compartment rules are **hard product boundaries**, not advisory behavioral safeguards.
The `label` field above lives only inside the policy store on Devices authorized for the
Compartment; it is never transmitted to a Device that the Compartment denies (see §10,
§12, and the Compartment-safety invariant). Authorized policy replicas may receive the
Compartment IDs and labels needed to enforce policy. Restricted object replicas must
not receive real Compartment IDs or labels for Compartments they are not authorized to
know.

`existence_only` is not automatically safe. A Compartment may set
`default_replication: none`, in which case even the existence of an object must not
replicate to denied Devices.

---

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

## 10. Replica state and partial replication

Allowed replication levels:

| Level              | Meaning                                                               |
| ------------------ | --------------------------------------------------------------------- |
| `none`             | Object does not replicate.                                            |
| `existence_only`   | Device knows a hidden object exists.                                  |
| `redacted_summary` | Device receives a safe summary.                                       |
| `metadata_only`    | Device receives status/timing metadata only.                          |
| `structural`       | Device receives IDs, dependencies, timing, status, but not payload.   |
| `full`             | Device receives full object content.                                  |
| `derived_only`     | Device receives derived warnings or risk states, not source evidence. |

This matters because a Device may need to schedule around a private block without seeing
its contents.

### Corrected redacted example (no Compartment identity leak)

The earlier draft's example leaked the very thing §12 and §23 forbid: it sent the real
`compartment_id` (`comp_relationship_private`) and a specific `redaction_reason` down to
the restricted Device. A "relationship private" 45-minute block at 2pm is itself
informative. The corrected representation carries **no Compartment identity, no
Compartment label, and a generic reason**. On a restricted Device, `redaction_level` is
the only safe signal, and any handle is an opaque local placeholder with no semantic
content:

```json
{
  "redacted_object_ref": "opaque_local_object_91ab",
  "source_object_ref": null,
  "replication_level": "metadata_only",
  "title": null,
  "visible_label": "Protected block",
  "duration_minutes": 45,
  "scheduled_start": "2026-06-01T14:00:00-04:00",
  "redaction_level": "restricted",
  "compartment_ref": "opaque_local_handle_7f3a",
  "redaction_reason": "policy_restricted"
}
```

`redacted_object_ref` and `compartment_ref` here are opaque, per-Device, non-reversible
handles — they are **not** the real object ID or `compartment_id`, and they must not be
derivable back to those IDs. Their only purpose is to let the restricted Device render
or group restricted blocks consistently without learning what they are. If grouping is
sensitive, omit `compartment_ref` entirely; if object-level correlation is sensitive,
rotate `redacted_object_ref` according to the future redacted-handle policy.

### Hard invariant (redaction identity)

```text
No Compartment identity (compartment_id) and no human-readable Compartment label may
cross a boundary that denies the payload. A restricted Device learns at most: that a
protected object exists, its timing/duration, and that policy restricts it — never which
Compartment, nor anything from which the Compartment could be inferred.
```

---

## 11. Replication eligibility rules

```text
replication_allowed(object, target_device, requested_level) iff:

1.  target_device is registered and not revoked;
2.  target_device belongs to an allowed Zone;
3.  object's Compartment allows target_device;
4.  requested_level is allowed by the Compartment;
5.  object retention policy allows storage on target_device;
6.  object export/routing policy does not forbid the route;
7.  target_device capability_profile can enforce required restrictions;
8.  object does not embed forbidden cross-Compartment payloads;
9.  the statement's observed policy versions are still admissible or have been reconciled;
10. no user override or policy update has denied this route.
```

If the predicate fails, the sync system must deny replication or provide only an allowed
redacted representation (§10) — and that representation must satisfy the §10 redaction
identity invariant.

---

## 12. Redaction and structural visibility

Restricted Devices **may** receive: existence; time block; duration; generic conflict
status; generic review requirement; urgency level; a "sync required before projection"
warning; a protected Calendar block.

Restricted Devices **must not** receive: forbidden titles; notes; private evidence;
sensitive diagnostic details; source snippets; external references that reveal private
context; conflict messages that leak sensitive payload; **the Compartment id or label**;
**any reason string that names the Compartment or its subject**.

Bad:

```text
Cannot sync relationship note about Alice's divorce to this phone.
```

Also bad (leaks Compartment identity, which the strengthened invariant now forbids):

```text
Cannot sync object in compartment comp_relationship_private to this Device.
```

Good:

```text
A protected object cannot sync to this Device under current policy.
```

### Source-taint rules for derived state

Derived state inherits the most restrictive Compartment, export, retention, and routing
policy of its source set unless an explicit redaction transform produces a lower-
sensitivity artifact with recorded provenance. This applies to risk reports, Calendar
warnings, diagnostics, summaries, worker results, projection previews, conflict
explanations, and `diagnostic_summary` statements.

A `diagnostic_summary` or `worker_result` must not be treated as harmless metadata. It
may replicate only at a level allowed by the strictest source policy, unless the system
can prove that an approved redaction transform removed the restricted content and
recorded that transformation.

---

## 13. Offline operation

Allowed offline operations may include: view already-replicated state; mark visible Tasks
complete; snooze visible Tasks; reject a next action; create local candidate Tasks;
append local Log observations; create friction notes; queue projection attempts; run
local Plan repair if enough state is present.

Offline operations produce sync statements carrying: origin Device; actor Identity;
`authority_source`; observed prior versions; `effective_time`; `recorded_time`; causal
parents; idempotency key; `compartment_ids`; admission policy.

An offline completion reuses the existing `LogOutcome` vocabulary (`completed`,
`skipped`, `blocked`) and `task_ref` convention:

```json
{
  "sync_statement_id": "syncstmt_complete_001",
  "statement_kind": "mutation",
  "mutation_kind": "task_completed",
  "origin_device_id": "dev_phone_001",
  "actor_identity_id": "identity_user_main",
  "authority_source": "user",
  "object_refs": ["task_123"],
  "task_ref": "task_123",
  "outcome": "completed",
  "observed_versions": { "task_123": "v17" },
  "observed_policy_versions": { "comp_personal": "v12" },
  "effective_time": "2026-06-01T14:20:00-04:00",
  "recorded_time": "2026-06-01T14:22:00-04:00",
  "offline": true,
  "idempotency_key": "phone-task123-complete-20260601T1420"
}
```

---

## 14. Mutation admission

Mutation **creation** is distinct from mutation **admission**. Any authorized Device may
create a sync statement for an operation it is allowed to attempt. The operation is
admitted only if: the origin Device was authorized; the actor Identity had permission;
the object was visible at the required level; the Compartment allowed the operation;
causality checks pass; conflict rules allow admission; policy did not change in a way that
invalidates the operation; the mutation is not a duplicate; the mutation does not require
human review.

Admission outcomes:

```text
admitted
admitted_with_repair_required
admitted_as_log_only
rejected
requires_review
superseded
duplicate_ignored
policy_blocked
```

Worked rule (offline completion):

```text
If a Device marks a Task complete offline and the Task remains active when the statement
is reconciled, admit the completion. If the Task was superseded before reconciliation,
preserve the completion as a Log observation (LogEntry, outcome=completed) but do not
resurrect the superseded Task.
```

---

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

## 18. Compartment policy propagation

When a Compartment policy changes after replication (e.g. a Task previously allowed on the
phone is moved to a Compartment the phone may no longer store):

1. stop future full replication;
2. emit a `policy_update` sync statement;
3. mark the existing replica as requiring redaction or purge;
4. log a best-effort purge/redaction attempt;
5. do not claim success until the target Device confirms;
6. treat offline Devices as potentially stale;
7. prevent diagnostics from leaking the affected payload **or its Compartment identity**;
8. recalculate derived state if visibility changed.

> UbU must not claim that a Compartment policy was enforced on an offline, stale, or
> unreachable Device unless it has evidence that the enforcement action occurred.

### Tombstone, redaction, and purge semantics

A `tombstone` statement means the object is no longer active/admissible but preserves
enough metadata for causality, idempotency, conflict resolution, and audit.

A `redaction` statement means payload or derived content is removed, downgraded, or
replaced by an allowed representation for one or more replicas while preserving required
structural/audit metadata.

A physical `purge` is a best-effort deletion operation. UbU must not claim purge success
on an offline, stale, revoked, or unreachable Device until that Device confirms the
operation or the user accepts an explicit unknown/exposure state.

---

## 19. Device revocation and stale Devices

Revoked Devices: receive no new sync packages; cannot submit automatically admitted
statements; may have pending statements quarantined; may require user recovery review;
may trigger Compartment-exposure warnings; may trigger token rotation for external
integrations; may invalidate worker assignments.

Stale Devices: are not necessarily malicious; may hold outdated state; may produce valid
but stale statements; should trigger UX warnings; should not be treated as fully
policy-updated until confirmed.

Known anti-pattern:

> A user may intentionally power on only one Device at a time and power it off before
> synchronization occurs. This is a UX/state-awareness problem, not a reason to
> centralize authority.

Mitigations: stale-state warnings; encrypted sync-folder recommendation; "sync before
projection" prompts; pending sync-statement counters; export/import encrypted sync
bundle; known-Device last-seen display.

---

## 20. User-owned worker Devices

Worker Devices **may**: receive scoped context bundles; run planner candidates; run
simulations; perform local LLM extraction if policy allows; generate summaries; return
candidate mutations; return `worker_result` statements; prepare projection previews.

Worker Devices **may not**: directly mutate admitted state without sync-statement
admission; bypass Compartment policy; retain payloads beyond retention policy; route
`no_cloud_llm` data through cloud services; export `no_external_export` content; expand
their own authority; silently keep data after revocation.

---

## 21. External projection records

If a Device writes to an external system (Google Calendar, GitHub, Todoist) in a later
phase or integration path, it must emit a `projection_record` sync statement.

```json
{
  "sync_statement_id": "syncstmt_projection_001",
  "statement_kind": "projection_record",
  "origin_device_id": "dev_laptop_001",
  "integration": "google_calendar",
  "external_object_ref": "gcal_event_abc",
  "source_object_refs": ["calendar_block_123"],
  "projection_kind": "create_event",
  "idempotency_key": "gcal-calendar-block-123-create-20260601T150000",
  "observed_versions": { "calendar_block_123": "v8" },
  "projection_policy_version": "v3",
  "external_write_status": "succeeded",
  "external_previous_ref": null,
  "projected_at": "2026-06-01T15:00:00-04:00",
  "observed_sync_state": {
    "known_pending_statements": 0,
    "known_stale_device_ids": ["dev_desktop_worker_001"]
  }
}
```

Phase 2 does **not** need to implement Google Calendar or Todoist sync. But if a Device
projects externally, projection must be auditable and reconcilable.

> External projection is not proof of single-device authority. It is an action by an
> authorized Device that must emit auditable sync/projection statements.

### External token custody

Full external token custody, secret replication, rotation, and recovery are Phase 3
requirements. In Phase 2, external tokens and related credentials are treated as normal
Compartment-assigned data and must obey the same replication, routing, retention, and
export policy as any other sensitive object. Phase 2 sync statements may reference token
capability IDs or projection authority, but they should not introduce a special-purpose
secret-replication system.

Phase 3 should define how external integration tokens, local encryption keys, and worker
credentials are stored, rotated, revoked, and optionally replicated across Devices
without violating Compartment policy.

---

## 22. Sync-triggered planning recalculation

Recalculation in Phase 2 reuses the existing `PlanningMode.repair` value (present in the
frozen schema "for fidelity," not supported in Phase 0). Phase 2 is where `repair` is
first implemented.

Triggers:

```text
sync_state_changed
device_reconnected
device_revoked
offline_mutation_admitted
offline_mutation_rejected
compartment_policy_changed
replication_level_changed
worker_status_changed
projection_record_received
policy_version_conflict_detected
conflict_detected
conflict_resolved
offline_window_changed
stale_device_detected
```

| Trigger                        | Possible response                                  |
| ------------------------------ | -------------------------------------------------- |
| Offline completion admitted    | Short-horizon repair or full recalculation.        |
| Task mutation rejected         | Restore or recalculate Plan.                       |
| Device reconnected             | Review pending statements.                         |
| Compartment visibility changed | Recompute redacted Calendar views.                 |
| Worker unavailable             | Reassign or degrade compute policy.                |
| Conflict unresolved            | Freeze affected Calendar region or request review. |
| External projection detected   | Reconcile projected state.                         |

---

## 23. Diagnostics, explanations, and UX warnings

Phase 2 sync must be user-legible. Required warning examples:

* "This Device has not synced with your laptop since yesterday."
* "Three pending sync statements are waiting for exchange."
* "This Plan may be stale because another known Device has unsynced state."
* "A protected Calendar block is hidden on this Device."
* "An item cannot be shown here due to current policy."
* "Before projecting to an external calendar, synchronize or accept stale-state risk."
* "A previous sync session did not complete; resume or review."

Diagnostics must obey Compartment restrictions and the §10 redaction-identity invariant.
Restricted diagnostics say:

```text
A protected object cannot sync to this Device.
```

never:

```text
Your relationship note about X cannot sync to this Device.
```

and never name the Compartment.

---

## 24. Security and privacy invariants

1. A Device cannot receive full payloads from a Compartment it is not authorized to store.
2. A worker cannot receive broader context than its capability grant allows.
3. `no_cloud_llm` content cannot route through cloud processing because of sync.
4. `no_external_export` content cannot appear in external projection, sync diagnostics,
   screenshots, or worker artifacts.
5. Revoked Devices cannot receive new sync payloads.
6. Stale offline Devices cannot be represented as policy-compliant until confirmed.
7. Sensitive deletion/redaction must be logged as a state transition (`tombstone` /
   `redaction` statement), not silently edited.
8. Sync diagnostics must not leak sensitive payloads.
9. Derived fields must not reconstruct forbidden content.
10. Conflict reports shown on lower-trust Devices must be redacted.
11. Policy changes must fail closed when visibility is ambiguous.
12. Incomplete sync must not be represented as complete.
13. **No Compartment id or label crosses a boundary that denies the payload** (the
    strengthened §10 invariant, stated here as a hard security rule).
14. Derived state, diagnostics, worker results, and projection previews inherit the
    strictest source policy unless an approved redaction transform with provenance
    lowers the sensitivity of the artifact.

---

## 25. Minimum Phase 2 requirements

Phase 2 must include:

1. arbitrary-number-of-Devices architecture;
2. stable Device registry;
3. Device registration;
4. Device revocation;
5. Zone-aware sync metadata;
6. Compartment-aware replication policy;
7. at least three replication levels: `full`, `metadata_only`, `none`;
8. sync statement envelope format;
9. transport-independent sync semantics;
10. an implementation that requires **no more than one live synchronization session at a
    time** (live session cardinality 1), with an N-Device data model;
11. indirect encrypted sync transport compatibility;
12. offline mutation queue;
13. per-object version or causality metadata;
14. observed policy version metadata where policy affects admission or replication;
15. idempotency keys;
16. conflict detection;
17. manual conflict review path (via the §17 diagnostic/prompt surface);
18. redacted Calendar/Task blocks satisfying the §10 redaction-identity invariant;
19. sync-triggered Plan invalidation;
20. worker Device registration;
21. worker result/candidate sync statements;
22. stale Device warnings;
23. test fixtures for denied Compartment replication.

> Phase 2 must be architected for an arbitrary number of Devices. It may require no more
> than one live synchronization session at a time, but schemas, identifiers, causality
> metadata, conflict handling, Device registration, Compartment policy, and sync
> statement formats must not assume a maximum of two Devices.

---

## 26. Permitted initial limitations

| Limitation                                    | Acceptable in Phase 2? | Condition                                              |
| --------------------------------------------- | ---------------------: | ------------------------------------------------------ |
| One live sync session at a time               |                    Yes | Data model must support N Devices.                     |
| No full automatic peer mesh                   |               Possibly | Must be researched before final Phase 2 freeze.        |
| Limited automatic conflict resolution         |                    Yes | Manual review path required.                           |
| Disconnect-during-sync partly undefined       |            Temporarily | Must be detectable and safe.                           |
| No hardware attestation                       |                    Yes | Do not claim hardware-backed guarantees.               |
| No multi-user sync                            |                    Yes | Phase 2 is single-user.                                |
| No Google Calendar/Todoist bidirectional sync |                    Yes | Projection records may be modeled but not implemented. |
| No cryptographic deletion proof               |                    Yes | Must state best-effort limitation.                     |

---

## 27. Test fixtures and certification

**Device tests:** register one Device; register N Devices; revoke Device; stale-Device
warning; Device denied Compartment payload; Device receives metadata-only redacted block.

**Sync statement tests:** valid statement admitted; duplicate idempotency key ignored;
stale prior version detected; stale policy version detected; incomplete sync session
detected through missing checkpoint/manifest commit; indirect sync bundle imported;
statement from revoked Device quarantined.

**Compartment tests:** allowed Compartment replicates full payload; denied Compartment
does not replicate; denied Compartment produces redacted block; **redacted block carries
no compartment_id or label** (redaction-identity invariant); policy update stops future
replication; offline Device not falsely marked purged; diagnostic message leaks neither
payload nor Compartment identity.

**Conflict tests:** offline completion admitted; offline completion after supersession
becomes Log-only; concurrent status changes require review; protected Calendar conflict
requires review; Compartment policy conflict fails closed; **manual resolution emits a
`conflict_resolution` statement and the contested region is `candidate`/`pending` until
that statement is admitted** (determinism-closure test); **the manual-review surface is a
blocking diagnostic, not a synchronized Task** (no-loop test).

**Planning tests:** sync mutation invalidates Plan; admitted completion triggers Calendar
repair (`PlanningMode.repair`); rejected mutation does not silently change Plan; worker
availability change affects compute planning; redacted Calendar view preserves timing
without payload or Compartment identity.

**Projection tests:** projection record emitted with idempotency key, observed source
versions, projection policy version, and external write status; projection from stale
Device warns; projection diagnostics redacted; external projection does not imply central
authority.

---

## 28. Open questions

The `UBU-QSYNC-*` series previously held here has been merged into the canonical
`OPEN_QUESTIONS.md` register as `UBU-Q0139`-`UBU-Q0150`. It was kept separate on the
assumption that these questions were not applicable before Phase 2; seven of them turned
out to block Phase 1b, so they now live with every other open question and are ranked and
worked through the same path.

This section is retained as a redirect only. Do not add new questions here.

| Retired ID | Canonical ID | Phase |
|---|---|---|
| `UBU-QSYNC-001` | `UBU-Q0139` | Phase 2 |
| `UBU-QSYNC-002` | `UBU-Q0140` | Phase 1b |
| `UBU-QSYNC-003` | `UBU-Q0141` | Phase 2 |
| `UBU-QSYNC-004` | `UBU-Q0142` | Phase 2 |
| `UBU-QSYNC-005` | `UBU-Q0143` | Phase 1b |
| `UBU-QSYNC-006` | `UBU-Q0144` | Phase 1b |
| `UBU-QSYNC-007` | `UBU-Q0145` | Phase 1b |
| `UBU-QSYNC-008` | `UBU-Q0146` | Phase 2 |
| `UBU-QSYNC-009` | `UBU-Q0147` | Phase 2 |
| `UBU-QSYNC-010` | `UBU-Q0148` | Phase 1b |
| `UBU-QSYNC-011` | `UBU-Q0149` | Phase 1b |
| `UBU-QSYNC-012` | `UBU-Q0150` | Phase 1b |

---

## Relationship to existing UbU docs

| File                                      | Protects                        |
| ----------------------------------------- | ------------------------------- |
| `planning-kernel-contract/0.1`            | Plan legitimacy                 |
| `device-sync-contract/0.1`                | State legitimacy across Devices |

The planning contract says what counts as a valid Plan. The device sync contract says
what counts as valid replicated state. Phase 2 needs both, because a multi-device planner
cannot be trusted if the state beneath it is incoherent, stale, overexposed, or silently
policy-violating.

---

## Applicability to Phase 1 / Phase 0

This file does not expand Phase 1/Phase 0 implementation scope. Phase 1 may reference the
future sync contract but is **not** required to implement N-device sync, Device
revocation, partial replication, indirect sync bundles, offline mutation reconciliation,
or cross-device Compartment enforcement.

However, Phase 1/Phase 0 schemas must avoid choices that make this contract impossible.
**These are the only parts of this contract with a near-term deadline, because they are
the only parts Phase 0 code can violate:**

Phase 1/Phase 0 should avoid:

* assuming one permanent local canonical instance in object IDs;
* embedding local filesystem paths as durable object identity;
* treating Device as irrelevant metadata;
* treating Compartment as a display label only (it is `compartment_ids` +
  `redaction_level` today, and a hard policy boundary tomorrow);
* building Logs that cannot be merged;
* building Task state transitions without origin/actor/provenance — note that
  `authority_source` (`UBU-D0185` as amended by `UBU-D0226`), already carried on every `TaskSpec` and `LogEntry`, is
  precisely this provenance slot and must be populated honestly;
* building projection records without enough audit metadata.

> Do not implement Phase 2 sync yet, but do not make Phase 2 sync architecturally painful.

The Phase 0 status is good on these points: `authority_source` is on every Task and Log
entry, `compartment_ids`/`redaction_level` exist as real (if inert) fields, IDs are stable
`*_id` strings rather than filesystem paths, and the `LogEntry` shape is mergeable. The
main thing to protect through the ETHConf build is that none of these regress under
codegen pressure.

---

## Applicability to Phase 2

Binding. Before Phase 2 is considered complete, UbU should demonstrate: multiple
registered Devices; an arbitrary-number-of-Devices data model; at least one real
two-Device sync path; sync semantics not limited to two Devices; at least one indirect
encrypted transport path or design-compatible sync bundle; Compartment-restricted
replication; redacted Calendar/Task representation satisfying the redaction-identity
invariant; an offline Task mutation queue; conflict detection; a manual conflict review
path; sync-triggered Plan invalidation; worker Device participation; stale Device
warnings; safe behavior on revoked Devices; and tests proving forbidden Compartment
payloads (and Compartment identities) do not replicate.

---

## Most important design commitments

1. No centralized canonical server assumption.
2. No permanent primary Device assumption.
3. Arbitrary-number-of-Devices architecture is required.
4. **Live session cardinality may be 1; Device cardinality may not.** (One *live session*
   is an implementation limit; two *Devices* is never a data-model limit.)
5. Sync statements are transport-independent.
6. Encrypted indirect transport must be design-compatible.
7. Compartment policy is a hard boundary — including its identity and label.
8. Canonical state is logical admitted event state, not "whatever one machine says."
9. **Determinism holds over the resolution-inclusive closure**; the human merge decision
   is a `conflict_resolution` event, and contested regions are `candidate`/`pending`
   until it lands.
10. Offline changes become reconcilable statements, not silent overwrites.
11. Sync must preserve Plan and Calendar legitimacy.
12. Restricted replicas use opaque local handles rather than global object or Compartment
    IDs when those identifiers would leak restricted information.

---

## Appendix A — Schema reconciliation

Names from earlier drafts mapped to the real frozen Phase 0 vocabulary. Reuse the
right-hand column.

| Earlier draft term            | Use instead (real / aligned)                          | Notes |
| ----------------------------- | ----------------------------------------------------- | ----- |
| `compartment_allowlist` / `compartment_denylist` | `effective_compartment_access` summaries plus Compartment policy | Device-level lists are too coarse as authority sources. |
| `compartment_refs` (on stmt)  | `compartment_ids`                                     | Matches `PrivacyAndProvenance.compartment_ids`. |
| `allowed_devices` (Zone)      | `allowed_device_ids`                                  | `*_id` convention. |
| `known_stale_devices`         | `known_stale_device_ids`                              | Same. |
| `redaction_reason` (specific) | `redaction_level` + generic `redaction_reason`        | Restricted Devices get `restricted` / `policy_restricted` only. |
| `recorded_time`               | `recorded_time` **(net-new)**, paired with real `effective_time` | `effective_time` already exists; `recorded_time` is the Phase 2 companion. |
| actor / provenance fields     | `actor_identity_id` + `authority_source` (`AuthoritySource`) | Provenance backbone already exists. Use the closed enum's real members only (`UBU-D0226`): ordinary user-driven actions use `user`; `user_override` is reserved for explicit override cases. `AuthoritySource` remains a **closed** enum; membership changes require a recorded decision and a schema-change ticket. |
| task completion refs          | `task_ref` + `outcome` (`LogOutcome`)                 | Existing `LogEntry` vocabulary. |
| "recalculate plan"            | `PlanningMode.repair`                                 | Present in schema, unsupported in Phase 0, first implemented in Phase 2. |
| manual-merge "review Task"    | `SkeletonFailureDiagnostic` + `prompt_policy: immediate_blocking_prompt` + `SafeAlternative.action: manual_decision` | Existing blocking-prompt surface; avoids the planner loop (§17). |

**Net-new Phase 2 nouns (no Phase 0 foothold):** `Device`, `Zone`, `Replica`,
`SyncStatement`, `tombstone`, `redaction`, `recorded_time`. These are introduced by this contract and
should be added to the Phase 2 schema deliberately, not retrofitted into the frozen
Phase 0 `schema.py`.

**Established UbU model nouns (already named, deferred past Phase 0):** `Compartment`,
`Identity` — both appear in the Phase 0 cutlist as deferred multi-user/Phase 1+ models.
This contract treats them as established vocabulary, not inventions.
