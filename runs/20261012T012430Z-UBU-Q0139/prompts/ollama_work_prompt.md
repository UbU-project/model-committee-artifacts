# Model-Committee Work Proposal Request

You are participating in the UbU `model-committee` process.

Your task is to produce one concrete work proposal as strict JSON.

Do not return prose outside the JSON object.

## Selected question

Question ID: `UBU-Q0139`  
Question title: `Phase 2 sync topology: peer-to-peer mesh or single-live-session`  
Repository working tree: `/home/sean/ubu-design`

HEAD commit (provenance only): `72f8188accfe15d8eb36cf649903db9eb5d3f1e3`

```markdown
## UBU-Q0139: Phase 2 sync topology: peer-to-peer mesh or single-live-session

Status: Open Priority: Post-MVP Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: Phase 2 sync topology Resolved by: None Last scored: 2026-10-11 Scored from commit: None

Formerly UBU-QSYNC-001 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §3, §25, §26.

### Question

Does Phase 2 require full peer-to-peer mesh synchronization, or is a single-live-session model
sufficient if the sync statement format is N-device compatible and transport-independent?

### Resolution

Open.

---

```

## Patch baseline

The patch must apply to the **current working-tree files** in the repository above.
The selected question and context excerpts come from that working tree, which may
include uncommitted ranking and metadata updates.

- Set the JSON `base_commit` field to `72f8188accfe15d8eb36cf649903db9eb5d3f1e3` for provenance. It does not
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
- next decision id: `UBU-D0312`

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

---
```

### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md — referenced sections

#### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §3 — Phase 2 sync model

```markdown
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
```

#### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §25 — Minimum Phase 2 requirements

```markdown
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
```

#### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §26 — Permitted initial limitations

```markdown
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
- When editing `OPEN_QUESTIONS.md`, anchor hunks with the selected question heading `## UBU-Q0139: Phase 2 sync topology: peer-to-peer mesh or single-live-session` and its own `### Resolution` section. Do not use a repeated `### Resolution` heading from an earlier or later question as the edit location.
- If resolving the selected question, replace the `Unresolved.` text under that selected question's `### Resolution` section and update that same question's metadata line. Do not insert selected-question resolution text into any other question block.
- Use the existing `UBU-Qxxxx` and `UBU-Dxxxx` numbering conventions.
- Prefer minimal, auditable changesets.
- If the selected question is blocked, propose decomposition only if it produces replacement questions with fewer, simpler, or no dependencies.
- If the selected question is already partially resolved, narrow or clarify it rather than pretending it is fully unresolved.

Return only JSON.
