# Model-Committee Work Proposal Request

You are participating in the UbU `model-committee` process.

Your task is to produce one concrete work proposal as strict JSON.

Do not return prose outside the JSON object.

## Selected question

Question ID: `UBU-Q0141`  
Question title: `Minimum acceptable encrypted indirect sync transport`  
Repository working tree: `/home/sean/ubu-design`

HEAD commit (provenance only): `d8ce0c79bb8ebfe3c9c77adf87cbdc1dd317fcd1`

```markdown
## UBU-Q0141: Minimum acceptable encrypted indirect sync transport

Status: Open Priority: Post-MVP Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: Phase 2 sync transport Resolved by: None Last scored: 2026-10-11 Scored from commit: None

Formerly UBU-QSYNC-003 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §8, §27.

### Question

What is the minimum acceptable encrypted indirect sync transport? Should encrypted rclone/rsync-
compatible storage be an MVP-supported path or only design-compatible?

### Resolution

Open.

---

```

## Patch baseline

The patch must apply to the **current working-tree files** in the repository above.
The selected question and context excerpts come from that working tree, which may
include uncommitted ranking and metadata updates.

- Set the JSON `base_commit` field to `d8ce0c79bb8ebfe3c9c77adf87cbdc1dd317fcd1` for provenance. It does not
  define the patch's file contents: do not use `git show HEAD:<path>` or the Git index
  as the original version when it differs from the working tree.
- Use exact working-tree lines for all removed and unchanged lines. Preserve existing
  metadata updates and exclude unrelated edits from the proposal.
- To generate the diff, compare unmodified working-tree copies with the proposed
  contents. Validate against the working tree with `git apply --check`; a successful
  `git apply --check --cached` checks the index and is insufficient.

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

- next question id: `UBU-Q0191`
- next decision id: `UBU-D0313`

`DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md` is read-only context and is not in the patch
allowlist: do not propose changes to it.

### DESIGN.md — referenced sections

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

For Phase 2, a single-live-session sync implementation is sufficient (`UBU-D0312`), with arbitrary N Devices, concurrent offline mutations, partial replicas, and transport-independent integrity-protected statements. Direct exchanges, imported bundles, and user-selected encrypted stores share the same causality, policy, and conflict rules; no Device or transport becomes a permanent canonical authority. Full automatic peer mesh is optional. DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §26's topology research remains a freeze gate, with at least three Devices demonstrating serialized direct exchange and encrypted store-and-forward, recoverable delivery, deterministic admission/conflict handling, and Compartment-safe partial replication as specified by `UBU-D0312`.

---
```

### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md — referenced sections

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

#### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §27 — Test fixtures and certification

```markdown
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
- Allowed values for `Status:` are `Archived`, `Decomposed`, `Deferred`, `Open`, `Solved`, `Superseded`.
- When resolving a question, set its metadata to `Status: Solved` and set `Resolved by:` to the resolving decision id(s).
- Never write `Status: Resolved`; `Resolved by:` is a separate metadata field.
- The `patch` string must be a raw unified diff as produced by `git diff`; do not wrap it in markdown fences or prose.
- Every file diff must start with `diff --git a/<path> b/<path>`, followed by `--- a/<path>` and `+++ b/<path>`.
- Every hunk must include accurate `@@ -old_start,old_count +new_start,new_count @@` ranges and enough unchanged context for `git apply --check` to apply without `--recount`.
- When editing `OPEN_QUESTIONS.md`, anchor hunks with the selected question heading `## UBU-Q0141: Minimum acceptable encrypted indirect sync transport` and its own `### Resolution` section. Do not use a repeated `### Resolution` heading from an earlier or later question as the edit location.
- If resolving the selected question, replace the `Unresolved.` text under that selected question's `### Resolution` section and update that same question's metadata line. Do not insert selected-question resolution text into any other question block.
- Use the existing `UBU-Qxxxx` and `UBU-Dxxxx` numbering conventions.
- Prefer minimal, auditable changesets.
- If the selected question is blocked, propose decomposition only if it produces replacement questions with fewer, simpler, or no dependencies.
- If the selected question is already partially resolved, narrow or clarify it rather than pretending it is fully unresolved.

Return only JSON.
