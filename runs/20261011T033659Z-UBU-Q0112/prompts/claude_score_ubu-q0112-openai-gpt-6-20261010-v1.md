# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0112`  
Question title: `Waiting-for as an explicit task state`  
Base commit: `d1a334ef26674792d33b6cfe1be97f4e844673b5`

```markdown
## UBU-Q0112: Waiting-for as an explicit task state

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: delegation tracking, planning horizon accuracy Resolved by: None Last scored: 2026-10-10 Scored from commit: None

### Question

A Task where the user is personally waiting on a human response — "waiting for Sarah's reply before I can continue" — has no distinct Task state. Currently it is either left open with low priority or tracked informally. A `WAITING_FOR` state with an optional counterparty reference and expected-by date would improve planning horizon accuracy and delegation visibility. What is the complete `WAITING_FOR` state model?

### Subquestions

1. What fields does a `WAITING_FOR` task state carry: counterparty Identity ref, expected-by date, reminder policy, escalation trigger?
2. How does `WAITING_FOR` interact with the Task precondition model — is it a precondition state or a Task status?
3. How does `WAITING_FOR` differ from a Task assigned to an automation worker that is polling for completion?
4. How is a `WAITING_FOR` task surfaced in the planning horizon — as blocked, as a risk, or as a scheduled review trigger?
5. What Log entries are generated when a `WAITING_FOR` task transitions out of the waiting state?
6. Should `WAITING_FOR` tasks appear in the Calendar preview as blocked slots or be suppressed?

### Current direction

`WAITING_FOR` as an explicit task state is a Phase 2 required feature. It should be modeled as a Task status rather than a separate Task type. The counterparty reference should be an optional Identity ref consistent with the existing Relationship and Identity models.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "ubu-q0112-openai-gpt-6-20261010-v1",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0112",
    "base_commit": "d1a334ef26674792d33b6cfe1be97f4e844673b5",
    "summary": "Define Phase 2 WAITING_FOR as an explicit Task status with evidence-based release, reminders, local escalation review, blocked planning visibility, and auditable transitions.",
    "rationale": "Separating personal waiting from deterministic readiness and worker assignment makes unresolved work visible without reserving execution time or assuming a reply will arrive. Episode/version checks, admitted release evidence, and explicit review policies preserve user authority and prevent stale or duplicate multi-device results from releasing a newer wait.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -4875,3 +4875,15 @@\n **Status:** Accepted \u2192 OPEN_QUESTIONS.md. Amends `UBU-D0305`: its sentence that Phase 1b's open questions keep their tag no longer holds.\n \n On 2026-10-10 the 20 open questions tagged `Phase 1b` were re-tagged `Phase 2` so the Phase 2 ranking selects them. Their origin is recorded by this decision.\n+\n+---\n+\n+## UBU-D0307: Waiting-for is a Phase 2 Task lifecycle status with explicit release and review rules\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a79.5\u2013\u00a79.6. Resolves `UBU-Q0112`; extends `UBU-D0227` for Phase 2 and follows `UBU-D0228`'s wire convention.\n+\n+`WAITING_FOR` (`waiting_for` on the wire) retains the user's responsibility for an existing Task while a human response or outside action is awaited. Its single current waiting record carries an episode id, entry time, reason and release criterion, optional counterparty Identity ref and expected-by estimate, explicit reminder policy, optional local escalation time, and evidence refs. Deterministic automatic restoration requires an explicitly accepted predicate rule over admitted state; dates and raw arrivals are insufficient. Existing preconditions remain independent and worker assignment/polling remains a separate lifecycle.\n+\n+Waiting Tasks and their blocked dependents are excluded before kernel dispatch. The horizon shows blocked waits and derived lateness/dependency risk; Calendar preview uses a waiting list and non-occupying review markers. Follow-up work is separately admitted and scheduled. Expected-by estimates neither reserve time nor certify readiness.\n+\n+Entry, edits, and exit are version-checked, idempotent, and logged atomically; terminal exits also retain the ordinary outcome event. Recalculation, Compartment policy, worker admission, and multi-device conflict rules retain their existing authority. Review and escalation never imply permission to send a message. This decision specifies Phase 2 behavior and makes no implementation claim.\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -1665,7 +1665,54 @@\n \n `DEFERRED_INTENT` \u2014 the user has decided not to plan this Task now but wants to retain it for future consideration. Distinct from low-priority (which remains eligible for planning) and from cancelled (which is terminal). Tasks in `DEFERRED_INTENT` are invisible to the skeleton sampler. The Log review introspection cycle periodically surfaces them for review and structured dismissal. See `UBU-Q0117`.\n \n-`WAITING_FOR` \u2014 the user is personally waiting on a human response or external action before this Task can proceed. Carries an optional counterparty Identity reference and expected-by date. Distinct from a worker-delegated Task. Surfaces in the planning horizon as a blocked risk rather than a schedulable slot. See `UBU-Q0112`.\n+`WAITING_FOR` \u2014 a Phase 2 nonterminal Task status for a personally retained Task awaiting a human response or outside action. The complete waiting record, release rules, worker boundary, planning visibility, and Logs are defined in \u00a79.6 (`UBU-D0307`).\n+\n+### 9.6 Waiting-for lifecycle (Phase 2)\n+\n+`WAITING_FOR` is a nonterminal Task status with wire value `waiting_for` (`UBU-D0307`, following `UBU-D0228`). It records that the user retains responsibility for the Task but cannot continue until a human response or outside action occurs. It extends the Phase 1 lifecycle of `UBU-D0227`; ready, blocked, and in-progress remain derived views. A failed precondition alone does not create this status.\n+\n+#### Waiting record\n+\n+A Task has exactly one `waiting_for` record while its status is `waiting_for`, and the field is absent otherwise. Historical records remain in the append-only Log. The record carries:\n+\n+- `wait_id`: a new opaque episode id on every entry, retained across edits within that episode; it is not a separate WorkItem or canonical object type.\n+- `since`: the effective UTC instant of entry, preserved across reminder snoozes and expectation changes.\n+- `reason` and `resume_condition`: required descriptions of what is awaited and what evidence would permit continuation. An optional `resume_precondition` uses the existing \u00a710.1 predicate schema over admitted UniverseState. `auto_resume` defaults to `false`; `true` requires that predicate and an explicitly accepted rule authorizing restoration to `active` when it becomes true.\n+- `counterparty_identity_ref`: optional existing Identity ref. Absence is valid and does not cause Identity or Relationship creation. The reference conveys neither assignment nor communication authority.\n+- `expected_by`: optional tagged expectation, either `{ kind: instant, at: UTC timestamp }` or `{ kind: local_date, date: YYYY-MM-DD, time_zone: IANA zone, review_at: resolved UTC timestamp }`. For a local date, admission resolves `review_at` to the start of the following local day and retains the date and zone for display and replay. This is an estimate, not a Task deadline, allowed range, promised completion, or release condition.\n+- `reminder_policy`: `{ mode: none | once | repeat, first_review_at?, interval_seconds?, snoozed_until? }`. `once` and `repeat` require a UTC `first_review_at`; `repeat` also requires a positive `interval_seconds`; schedule fields inconsistent with the selected mode are rejected. Admission defaults to `once` at the expected-by boundary when one exists, otherwise `none`; repeating reminders require explicit opt-in. `snoozed_until` is an optional UTC instant that postpones ordinary reminders without changing `since`, `expected_by`, or the release condition. Changing the expectation requires explicitly retaining or revising the reminder schedule.\n+- `escalation_trigger_at`: optional UTC instant. If the wait remains unresolved at that instant, surface a local escalation review of follow-up, alternatives, or abandonment; absence means no scheduled escalation. It grants no authority to contact the counterparty or another person.\n+- `evidence_refs`: optional Log or External Reference links supporting the request and release criterion. Ordinary Task provenance and `compartment_ids` apply to the entire record, notifications, Logs, and derived reports; protected content is referenced and redacted under existing policy.\n+\n+#### Transitions and admission\n+\n+Only an admitted `active` Task enters `waiting_for`. Entry retains the Task id, Objective, executor responsibility, dependencies, preconditions, effects, timing constraints, and remaining-work estimate. If work has started, entry closes its current execution interval with a stopping boundary and partial-work evidence, preserving the remaining-work estimate. After restoration, historical start evidence alone does not imply an ongoing execution interval; a new start records resumed work. Elapsed waiting time is excluded from execution duration and duration learning.\n+\n+`waiting_for -> active` requires admitted evidence satisfying the release criterion, or an explicit user withdrawal/override of that criterion with a reason. A matching `resume_precondition` causes restoration only under the accepted `auto_resume` rule; otherwise it proposes review. Automatic release requires accessible, current admitted evidence for the targets establishing the predicate match. An `absent` match requires an admitted clear; omission, redaction, or staleness in a partial Replica cannot establish release even when the baseline evaluator treats unknown targets as absent. Admission rejects an automatic wait whose release is already confirmed; accepted rules are re-evaluated after relevant admitted state changes. Raw messages, worker observations, the expected-by boundary, reminders, and elapsed time cannot restore or complete the Task by themselves. Restoration re-evaluates all ordinary dependencies and preconditions without clearing them: an `active` Task can remain blocked.\n+\n+`waiting_for -> completed | failed | moot` uses the existing outcome admission rules and evidence. Receipt of a response normally restores `active`; external satisfaction uses the applicable \u00a79.5 moot reason rather than inventing completion. Reminder dismissal and snooze leave the Task waiting; abandonment uses the existing narrow moot reason. On exit, clear the current waiting record and cancel its pending reminders and escalation. A later wait receives a new `wait_id` and explicit expectations and policies.\n+\n+Entry supplies a fresh `wait_id`; edits and releases target the current episode. Every change, including automatic release, checks the expected Task version and carries an idempotency key. Duplicate submissions cannot repeat a canonical transition or create another review/follow-up request; a result for an older wait cannot release a newer one. Authorized workers submit candidates through the normal admission path. Multi-device causality and conflicts use the existing sync contract rather than a separate waiting-state merge rule.\n+\n+An Automation Worker polling for completion remains tracked through its assignment lifecycle (\u00a725.1.2), including lease expiry, delivery, and review. Polling alone does not put its assigned Task into `waiting_for`. A human wait may be observed by a separately assigned polling/review Task or worker role; it does not imply delegation of the user's original Task or an execution lease. Assignment and waiting state never overwrite each other.\n+\n+#### Planning, review, and Calendar preview\n+\n+The CPU excludes waiting Tasks from the executable task graph before kernel dispatch and also excludes Tasks with required, unsatisfied dependencies on them, transitively. It retains those dependency blockers in explanations instead of deleting edges to make dependents schedulable. Waiting consumes no placement or duration sample, contributes no completion/effect credit, and needs no new `TaskSpec` field. An expected-by estimate cannot establish future readiness. Unrelated eligible work remains available.\n+\n+The planning horizon includes a blocked-waits view showing age, release criterion, permitted counterparty details, expectation, next review, overdue state, and affected downstream work. Crossing an expected-by boundary while still waiting creates a qualitative lateness/dependency-fragility finding in derived risk reporting (\u00a728), without inventing a success probability, new Risk object, or scoring penalty. Waits with no expectation remain visible as having unknown release time.\n+\n+Reminder and escalation boundaries use `elapsed_time` recalculation triggers (\u00a729). Canonical review requests use `(task_ref, wait_id, boundary_kind, boundary_at)` as their deterministic idempotency key and deduplicate through existing sync admission; offline notification delivery does not promise global exactly-once behavior. After downtime or snooze, collapse missed repeat reminders into one attention item and compute the next future boundary from the admitted schedule; do not replay a backlog of notifications. Reviews request user attention; any time-consuming follow-up is a separate ordinary Task admitted by the user or an explicit creation policy, linked to this wait and deduplicated by the same boundary key.\n+\n+Calendar preview suppresses the waiting Task's execution slot but displays a separate waiting list and non-occupying review/expectation markers. Only an admitted follow-up Task can reserve review or contact time. Entry invalidates future execution placements through ordinary Plan repair while preserving actual history and \u00a729 override rules; incompatible fixed commitments or override placements are surfaced for review. Leaving the wait does not silently complete its follow-up Tasks.\n+\n+#### Phase 2 Log events\n+\n+Phase 2 adds `task_waiting_entered`, `task_waiting_updated`, and `task_waiting_exited` to the \u00a717 envelope. Each accepted change records Task ref, `wait_id`, expected/resulting Task version, old/new status, effective time, actor and authority source, reason, evidence refs, idempotency key, and the changed waiting fields or policy-permitted protected references. Edits, including snooze and expectation changes, log before/after values without resetting `since`.\n+\n+Exit records whether the release was evidence-confirmed, an explicit user override, or a terminal outcome. A terminal exit also writes the existing `task_completed`, `task_failed`, or `task_moot` entry with its normal payload in the same admitted transaction. Waiting-state updates and their Log entries are atomic; corrected interpretations use \u00a717.3 correction links. Rejected or stale worker requests use existing rejection Logs and cannot emit a successful exit.\n+\n+Entry and exit invalidate affected Plans and risk views and produce the existing `recalculation_triggered` entry: `user_override`, `external_event`, or `worker_request` identifies the actual authority/evidence path; terminal outcomes use their existing trigger kinds. Metadata changes and due reviews normally mark derived views stale; changes affecting current/next work or hard feasibility require immediate recalculation under \u00a729. Batching retains all source event refs.\n \n ---\n \ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -1689,26 +1689,26 @@\n ## UBU-Q0112: Waiting-for as an explicit task state\n \n-Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: delegation tracking, planning horizon accuracy Resolved by: None Last scored: 2026-10-10 Scored from commit: None\n+Status: Resolved Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: delegation tracking, planning horizon accuracy Resolved by: UBU-D0307 Last scored: 2026-10-10 Scored from commit: None\n \n ### Question\n \n A Task where the user is personally waiting on a human response \u2014 \"waiting for Sarah's reply before I can continue\" \u2014 has no distinct Task state. Currently it is either left open with low priority or tracked informally. A `WAITING_FOR` state with an optional counterparty reference and expected-by date would improve planning horizon accuracy and delegation visibility. What is the complete `WAITING_FOR` state model?\n \n ### Subquestions\n \n 1. What fields does a `WAITING_FOR` task state carry: counterparty Identity ref, expected-by date, reminder policy, escalation trigger?\n 2. How does `WAITING_FOR` interact with the Task precondition model \u2014 is it a precondition state or a Task status?\n 3. How does `WAITING_FOR` differ from a Task assigned to an automation worker that is polling for completion?\n 4. How is a `WAITING_FOR` task surfaced in the planning horizon \u2014 as blocked, as a risk, or as a scheduled review trigger?\n 5. What Log entries are generated when a `WAITING_FOR` task transitions out of the waiting state?\n 6. Should `WAITING_FOR` tasks appear in the Calendar preview as blocked slots or be suppressed?\n \n ### Current direction\n \n `WAITING_FOR` as an explicit task state is a Phase 2 required feature. It should be modeled as a Task status rather than a separate Task type. The counterparty reference should be an optional Identity ref consistent with the existing Relationship and Identity models.\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0307` (DESIGN.md \u00a79.6): Phase 2 adds canonical `waiting_for` with an episode-bound waiting record, optional counterparty and expectation, explicit reminder/escalation review, and evidence- or override-based release. Existing preconditions and worker assignments remain independent. Waiting work and its blocked dependents are excluded before kernel dispatch; the horizon and Calendar preview show blocked/risk/review information without occupied waiting slots. Atomic, versioned, idempotent waiting Logs retain terminal outcome events and drive ordinary recalculation.\n \n ---\n",
    "commit_message": "Define Phase 2 waiting-for task lifecycle and review rules",
    "validation_notes": [
      "git apply --check --whitespace=error passed against the supplied/current working tree at HEAD d1a334ef26674792d33b6cfe1be97f4e844673b5; repository files were left unchanged.",
      "The selected question is locally rescored relative to the clean base commit. The patch targets the supplied Answerability score: 100 / Last scored: 2026-10-10 metadata and excludes all other existing scoring edits; applying to a clean base requires that selected rescore first.",
      "OPEN_QUESTIONS.md uses one hunk containing the UBU-Q0112 heading and its own Resolution section; only that question block changes and its metadata remains one line.",
      "Verified the new decision id is unused, every hunk has explicit accurate line counts, and only the three declared allowlisted files appear in the patch.",
      "Checked all six subquestions against existing Task, precondition, worker, Log, risk, recalculation, wire-casing, and sync boundaries, including release evidence on partial Replicas. Waiting is filtered before kernel dispatch; no planning-kernel payload extension is proposed.",
      "Documentation proposal only; no implementation tests or runtime claims. The Accepted decision and Resolved question represent the result of adopting this patch and require human approval."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0112"
    ],
    "decisions_added": [
      "UBU-D0307"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "ubu-q0112-openai-gpt-6-20261010-v1",
    "patch_applies": true,
    "allowlist_passed": true,
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "error": null,
    "normalized_patch": null,
    "warnings": [],
    "ordinary_error": null,
    "recount_error": null,
    "normalization_error": null
  }
]
```

## Provider weights

Provider weights are historical diagnostic context only in v0.2. Do not use
self-trust or author identity as score evidence.

```json
{}
```

## JSON Schema

Your output must satisfy this schema:

```json
{
  "type": "object",
  "additionalProperties": false,
  "required": [
    "scores",
    "selected_proposal_id",
    "selection_rationale"
  ],
  "properties": {
    "scores": {
      "type": "array",
      "items": {
        "type": "object",
        "additionalProperties": false,
        "required": [
          "proposal_id",
          "score",
          "patch_applies",
          "implements_selected_work",
          "preserves_question_schema",
          "avoids_unnecessary_scope",
          "decomposition_quality",
          "risks",
          "required_fixes",
          "rationale"
        ],
        "properties": {
          "proposal_id": {
            "type": "string"
          },
          "score": {
            "type": "integer",
            "minimum": 0,
            "maximum": 100
          },
          "patch_applies": {
            "type": "boolean"
          },
          "implements_selected_work": {
            "type": "boolean"
          },
          "preserves_question_schema": {
            "type": "boolean"
          },
          "avoids_unnecessary_scope": {
            "type": "boolean"
          },
          "decomposition_quality": {
            "type": "string",
            "enum": [
              "none",
              "good",
              "bad",
              "not_applicable"
            ]
          },
          "risks": {
            "type": "array",
            "items": {
              "type": "string"
            }
          },
          "required_fixes": {
            "type": "array",
            "items": {
              "type": "string"
            }
          },
          "rationale": {
            "type": "string"
          }
        }
      }
    },
    "selected_proposal_id": {
      "type": "string"
    },
    "selection_rationale": {
      "type": "string"
    }
  }
}
```

## Scoring requirements

Score each proposal from 0 to 100.

Consider:

- whether the patch applies cleanly;
- whether it implements the selected work;
- whether it preserves the question schema;
- whether it avoids unnecessary scope;
- whether it modifies only allowed files;
- whether it creates useful decomposition if decomposition occurs;
- whether it introduces new risks;
- whether required fixes remain.

Rules:

- Score every proposal in this prompt.
- `selected_proposal_id` must refer to one scored proposal from this prompt.
- Manual override is not allowed in v0.2.
- Prefer a patch that is valid, minimal, auditable, and directly responsive.
- Do not select a proposal whose patch failed mechanical validation.
- Do not score your own provider's proposal unless explicitly asked for diagnostic self-score.

Return only JSON.
