# Model-Committee Work Proposal Request

You are participating in the UbU `model-committee` process.

Your task is to produce one concrete work proposal as strict JSON.

Do not return prose outside the JSON object.

## Selected question

Question ID: `UBU-Q0143`  
Question title: `Auto-resolvable versus review-required conflict classes`  
Base commit: `78a14eb5860c3595821c6f6f90f73844166592fb`

```markdown
## UBU-Q0143: Auto-resolvable versus review-required conflict classes

Status: Open Priority: MVP important Phase: Phase 1b Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0134, Phase 2 conflict resolution Resolved by: None Last scored: 2026-09-16 Scored from commit: None

Formerly UBU-QSYNC-005 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §16, §17.

### Question

Which conflict classes can be auto-resolved safely, and which must always require user review?

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
- next decision id: `UBU-D0248`

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
- When editing `OPEN_QUESTIONS.md`, anchor hunks with the selected question heading `## UBU-Q0143: Auto-resolvable versus review-required conflict classes` and its own `### Resolution` section. Do not use a repeated `### Resolution` heading from an earlier or later question as the edit location.
- If resolving the selected question, replace the `Unresolved.` text under that selected question's `### Resolution` section and update that same question's metadata line. Do not insert selected-question resolution text into any other question block.
- Use the existing `UBU-Qxxxx` and `UBU-Dxxxx` numbering conventions.
- Prefer minimal, auditable changesets.
- If the selected question is blocked, propose decomposition only if it produces replacement questions with fewer, simpler, or no dependencies.
- If the selected question is already partially resolved, narrow or clarify it rather than pretending it is fully unresolved.

Return only JSON.
