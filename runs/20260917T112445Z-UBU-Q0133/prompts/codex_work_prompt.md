# Model-Committee Work Proposal Request

You are participating in the UbU `model-committee` process.

Your task is to produce one concrete work proposal as strict JSON.

Do not return prose outside the JSON object.

## Selected question

Question ID: `UBU-Q0133`  
Question title: `Phase 1b redacted Handle stability and disclosure limits`  
Base commit: `7e750c63e5fb3b38bc9cf2f80632642e95481de5`

```markdown
## UBU-Q0133: Phase 1b redacted Handle stability and disclosure limits

Status: Open Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0150 Blocks: Phase 1b Compartment egress, redaction-identity enforcement Resolved by: None Last scored: 2026-09-17 Scored from commit: None

### Question

Should Phase 1b redacted object and Compartment Handles be stable per Device, per session, or per egress operation, and what may a Handle reveal?

### Subquestions

1. Is a Handle stable for the lifetime of the object, the lifetime of the Device, or a single egress operation?
2. May a Handle reveal existence, time block, and duration only, and what is explicitly withheld?
3. How is the redaction-identity invariant enforced so that no Compartment id or label crosses a denied boundary, not merely no payload?
4. Does Handle stability leak correlation across repeated egress, and is that acceptable for a single operator with a single Compartment set?
5. How do the existing `Handle` projection and clearance egress filter map onto real `CompartmentLabel` values, replacing the three-tier simplification?

### Current direction

The existing `Handle` projection and clearance egress filter went largely unused after the Google Calendar pivot and become load-bearing again under the Phase 1b rule: they are the compartment egress mechanism the redaction-identity invariant requires. The settled part is that a Handle hides content, not existence. The open part is stability granularity and whether per-operation instability is worth the correlation resistance for a single operator.

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
- next decision id: `UBU-D0260`

`DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md` is read-only context and is not in the patch
allowlist: do not propose changes to it.

### OPEN_QUESTIONS.md — related questions

#### UBU-Q0150

```markdown
## UBU-Q0150: Redacted object and Compartment handle stability scope

Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0133, Phase 2 redaction identity Resolved by: UBU-D0253 Last scored: 2026-09-16 Scored from commit: None

Formerly UBU-QSYNC-012 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §10, §12.

### Question

Should redacted object/Compartment handles be stable per Device, per sync session, per Calendar
window, or per object version, and how much correlation risk is acceptable?

### Resolution

Resolved by `UBU-D0253`: Redacted object handles are local rotating aliases stable only for the receiving Device, the redacted source-object version, and the declared projection window, with rotation on version, redaction, policy, authority, or window changes. Compartment grouping handles are omitted by default; when policy permits them, they are stable only for the receiving Device, projection window, and policy epoch. Acceptable correlation is limited to local UI continuity and de-duplication inside one authorized restricted projection window; durable cross-Device, cross-window, cross-version, or Compartment-pattern correlation is not acceptable.
```

### DECISIONS.md — cited decisions

#### UBU-D0253

```markdown
## UBU-D0253: Redacted handles are local rotating aliases

**Status:** Accepted → DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §10, §12. Resolves `UBU-Q0150`.

Redacted object and Compartment handles are privacy-preserving aliases for a restricted Replica. They are never real object IDs, `compartment_id` values, Compartment labels, or encodings from which a restricted Device can recover or recognize those identities.

For `redacted_object_ref`, the default stability scope is the receiving Device, the redacted source-object version, and the declared projection window. The handle may remain stable across retries or repeated sync of that same version within that same window so the restricted Device can de-duplicate and render consistently, but it rotates when the source object version, redaction level, policy epoch, target Device authority, or projection window changes.

For `compartment_ref`, the default is omission. If policy explicitly permits restricted grouping, the grouping handle is stable only for the receiving Device, declared projection window, and policy epoch. It rotates across Devices, across projection windows, and across policy epochs, and it must be omitted whenever the fact of grouping would itself leak sensitive context.

All redacted handles must be generated as high-entropy random aliases or keyed aliases whose key is unavailable to the restricted Device. Handles must carry no semantic prefix, reason code, sequence, compartment vocabulary, or stable global identifier.

The acceptable correlation risk is limited to local correlation needed for one authorized restricted Device to maintain UI continuity and de-duplication inside one projection window. Durable cross-Device, cross-window, cross-version, or Compartment-pattern correlation is not acceptable for redacted replicas.

---
```

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

#### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §10 — Replica state and partial replication

```markdown
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
```

#### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §12 — Redaction and structural visibility

```markdown
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
- When editing `OPEN_QUESTIONS.md`, anchor hunks with the selected question heading `## UBU-Q0133: Phase 1b redacted Handle stability and disclosure limits` and its own `### Resolution` section. Do not use a repeated `### Resolution` heading from an earlier or later question as the edit location.
- If resolving the selected question, replace the `Unresolved.` text under that selected question's `### Resolution` section and update that same question's metadata line. Do not insert selected-question resolution text into any other question block.
- Use the existing `UBU-Qxxxx` and `UBU-Dxxxx` numbering conventions.
- Prefer minimal, auditable changesets.
- If the selected question is blocked, propose decomposition only if it produces replacement questions with fewer, simpler, or no dependencies.
- If the selected question is already partially resolved, narrow or clarify it rather than pretending it is fully unresolved.

Return only JSON.
