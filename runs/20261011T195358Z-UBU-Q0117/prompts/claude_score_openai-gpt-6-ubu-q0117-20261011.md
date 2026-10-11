# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0117`  
Question title: `deferred_intent task status and dismissal review`  
Base commit: `f5ed2d2d0ca010e458a87dd2a395f05cc6278c50`

```markdown
## UBU-Q0117: deferred_intent task status and dismissal review

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: planning horizon clarity, introspection completeness Resolved by: None Last scored: 2026-10-11 Scored from commit: None

### Question

UbU needs a named Task status for ideas that are not yet commitments and not cancelled — things the user has explicitly decided not to plan but wants to remember. A `DEFERRED_INTENT` status distinct from open and cancelled would prevent such items from cluttering the planning horizon. The Log review introspection cycle should periodically surface deferred-intent Tasks for review, including prompting the user to dismiss Tasks that are no longer attainable or desirable. What is the complete deferred_intent model?

### Subquestions

1. What fields distinguish a `DEFERRED_INTENT` task from an open low-priority task: explicit user deferral action, optional defer-until date, optional reason?
2. How does `DEFERRED_INTENT` interact with the planner: are these tasks invisible to the skeleton sampler, or available as optional fill tasks with very low priority?
3. What Log entry is created when a Task is moved to `DEFERRED_INTENT`?
4. What is the review cadence for surfacing deferred-intent Tasks: weekly, monthly, on Log review, or triggered by related Objective changes?
5. What dismissal options does the review prompt offer: promote to active, keep deferred, cancel permanently, or escalate to an Objective review?

### Current direction

`DEFERRED_INTENT` is a Phase 2 required status addition to the Task model. It is distinct from low-priority and distinct from cancelled. The periodic Log review report should surface deferred-intent Tasks with a structured dismissal prompt. The preference declaration requirement — that a Task must eventually have an explicit Preference commitment or be dismissed — should be enforced through this review cycle rather than at creation time.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "openai-gpt-6-UBU-Q0117-20261011",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0117",
    "base_commit": "f5ed2d2d0ca010e458a87dd2a395f05cc6278c50",
    "summary": "Complete the Phase 2 deferred-intent lifecycle, planner exclusion, review cadence, dismissal actions, and audit events; resolve UBU-Q0117 with UBU-D0308.",
    "rationale": "The working tree already establishes deferred-intent status and sampler exclusion. This proposal supplies the missing operational rules while preserving pairwise Preferences, terminal moot reasons, mandatory routine controls, user authority, and the existing kernel and sync boundaries.",
    "changed_files": [
      "DESIGN.md",
      "DECISIONS.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -1663,7 +1663,7 @@ Readiness and execution states such as proposed, ready, blocked, and in-progress\n \n **Phase 2 status additions (not in Phase 1):**\n \n-`DEFERRED_INTENT` \u2014 the user has decided not to plan this Task now but wants to retain it for future consideration. Distinct from low-priority (which remains eligible for planning) and from cancelled (which is terminal). Tasks in `DEFERRED_INTENT` are invisible to the skeleton sampler. The Log review introspection cycle periodically surfaces them for review and structured dismissal. See `UBU-Q0117`.\n+`DEFERRED_INTENT` \u2014 a Phase 2 nonterminal Task status, with wire value `deferred_intent`, for an explicit user decision to retain an idea without planning it. The complete deferral record, planning exclusion, review cadence, promotion, and dismissal rules are defined in \u00a79.7 (`UBU-D0308`).\n \n `WAITING_FOR` \u2014 a Phase 2 nonterminal Task status for a personally retained Task awaiting a human response or outside action. The complete waiting record, release rules, worker boundary, planning visibility, and Logs are defined in \u00a79.6 (`UBU-D0307`).\n \n@@ -1714,6 +1714,53 @@ Exit records whether the release was evidence-confirmed, an explicit user overri\n \n Entry and exit invalidate affected Plans and risk views and produce the existing `recalculation_triggered` entry: `user_override`, `external_event`, or `worker_request` identifies the actual authority/evidence path; terminal outcomes use their existing trigger kinds. Metadata changes and due reviews normally mark derived views stale; changes affecting current/next work or hard feasibility require immediate recalculation under \u00a729. Batching retains all source event refs.\n \n+### 9.7 Deferred-intent lifecycle (Phase 2)\n+\n+`DEFERRED_INTENT` (`deferred_intent` on the wire) is a nonterminal Task status for an explicit user choice to remember work without committing to plan it (`UBU-D0308`). It extends `UBU-D0227` for Phase 2 and follows `UBU-D0228`. Low-priority `active` Tasks remain planning candidates; deferred Tasks do not. Dismissal uses terminal `moot` with an existing \u00a79.5 reason code, never a new `cancelled` status.\n+\n+#### Deferral record and admission\n+\n+A Task has exactly one `deferred_intent` record while its status is `deferred_intent`, and the field is absent otherwise. The record carries:\n+\n+- `deferral_id`: a new opaque episode id for each entry, retained across edits and reviews in that episode; it is not a separate WorkItem.\n+- `since`: effective UTC entry time, preserved across keep-deferred choices and schedule edits.\n+- `reason`: optional user explanation; omission is valid and does not invite an inferred canonical reason.\n+- `defer_until`: optional tagged review boundary, either `{ kind: instant, at: UTC timestamp }` or `{ kind: local_date, date: YYYY-MM-DD, time_zone: IANA zone, review_at: resolved UTC timestamp }`. For a local date, admission resolves and retains the UTC start of that date in the stated zone. It controls ordinary review timing, not an allowed range, deadline, promised start, or automatic reactivation.\n+- `review_interval_seconds`: positive interval, default `2592000` (30 days). Cadence configuration is a Setting, not a Preference; the admitted record retains the effective interval for replay.\n+- `last_review_at`: optional UTC time of the latest explicit review action. Merely displaying, closing, or ignoring a prompt does not set it.\n+- `next_review_at`: required finite UTC instant, initially the future `defer_until` boundary when supplied, otherwise `since + review_interval_seconds`. An explicit keep-deferred or Objective-review action records `last_review_at` and uses a newly chosen or retained future `defer_until` boundary; if none remains, it clears an expired date and sets `last_review_at + review_interval_seconds`. An explicit date/interval edit recomputes the boundary from the retained future date or the edit time plus the interval. Reason-only edits do not postpone review; no edit resets `since`.\n+\n+Entry requires an explicit user deferral action. It may admit a newly captured deferred Task with stable id, title, and ordinary provenance before an Objective link, duration, or Preference is supplied, or move an admitted `active` Task to `deferred_intent`. Existing Task identity, intent, dependencies, effects, constraints, Preferences, and remaining-work evidence are retained. Advisors and workers may propose deferral but cannot infer or admit it from low priority, missed execution, or missing Preference. A waiting Task must first leave `waiting_for` under \u00a79.6; the two current records never coexist. Terminal Tasks remain terminal; reconsideration creates a new linked Task through ordinary admission.\n+\n+Started work records a stopping boundary and partial-work evidence before entry. Conflicting running assignments, fixed commitments, or override placements must be resolved through their existing admission and repair rules before deferral is admitted. Deferral grants no authority to cancel an external event or stop a worker. Routine occurrence Tasks retain \u00a77.4.1's mandatory semantics and use explicit skip, move, or recurrence-edit controls; deferral cannot replace those controls. A someday idea about a routine may be captured as a separate non-occurrence Task.\n+\n+#### Planning boundary\n+\n+The CPU excludes deferred Tasks from the executable task graph before kernel dispatch, including skeleton sampling, optional gap-fill selection, duration sampling, capacity budgets, and completion/effect credit. It also excludes dependents with required, unsatisfied dependencies on them, transitively, preserving those blockers in explanations rather than deleting edges. Unrelated eligible work remains available. No `TaskSpec` field or priority floor is added, and elapsed deferral time is not execution duration or duration-learning evidence.\n+\n+Entry invalidates future placements through ordinary Plan repair while preserving actual execution history. Deferred Tasks remain accessible in a separate retained ideas view and review report, with age, reason, next review, linked Objective, and affected downstream work. They reserve no Calendar slot. Promotion re-evaluates ordinary schedulability fields, dependencies, and preconditions; an `active` Task may still be blocked. Neither a review date nor an Objective change promotes the Task.\n+\n+#### Review cadence and actions\n+\n+Regular Log review (\u00a717.8) includes due deferred Tasks; default deferred-item cadence is 30 days, independently of the more frequent Log-review Task. A chosen `defer_until` postpones ordinary surfacing until that boundary. If review was skipped or the Device was offline, the next Log review or startup surfaces one overdue attention item per current episode, without replaying missed reminders. A material admitted change to a linked Objective's status or desired outcome queues an earlier review even before `defer_until`; it cannot cancel, complete, or promote the Task by itself. Manual review is always available.\n+\n+Scheduled review requests use `(task_ref, deferral_id, next_review_at)` as an idempotency key; Objective-triggered requests use `(task_ref, deferral_id, objective_change_log_ref)`. Coalesce outstanding causes into one attention item per current episode under existing sync admission. Due boundaries use the existing `elapsed_time`/`log_review_due` paths. A stale request for an earlier episode or superseded review boundary cannot mutate the Task. A review action that retains deferral acknowledges the presented cause refs and leaves only the next future boundary and any unresolved Objective-review request. Attention items occupy no time; any additional review/follow-up Task is separately admitted and scheduled.\n+\n+The prompt shows current intent, attainability, desirability, and whether explicit Preference commitment remains unresolved, and offers:\n+\n+- **Promote to active:** supply ordinary \u00a79.3 schedulability fields and explicitly confirm/cite an enabled, user-accepted pairwise Preference involving this Task or its linked Objective, or admit that comparison during review. Record the Preference refs with the promotion decision. An inferred ranking, scalar priority, or unary commitment flag is insufficient; if no relevant comparison can yet be made, keep deferred or open Objective review. Dependencies and preconditions remain independently evaluated.\n+- **Keep deferred:** optionally edit the reason, date, or interval and record a finite future `next_review_at` under the record's scheduling rule. Without a retained/chosen future date, use the admitted interval after review (30 days by default). Keeping an idea does not create a Preference commitment.\n+- **Dismiss permanently:** explicitly transition to `moot` using the narrowest existing reason, such as `no_longer_relevant`, `invalidated_by_universe_change`, or `user_declared_moot`. Retain the Task and its history; do not delete it or assert completion.\n+- **Review linked Objective:** keep the Task deferred and record the next review boundary while coalescing an unresolved Objective-review request for this episode and Objective. If no Objective is linked, offer normal Objective creation/linking review. This action does not declare a Preference or create a time reservation by itself.\n+\n+The Phase 2 Preference declaration requirement is enforced at review and promotion, not at idea creation. Keep-deferred, Objective-review, skipped, or ignored prompts do not resolve a missing commitment; it remains flagged for later review. There is no automatic dismissal after an age or number of missed reviews. Closing or ignoring the prompt leaves the attention request pending and retains `next_review_at`; a due boundary remains overdue. It cannot stand in for an explicit keep-deferred action or acceptance. Compartment policy applies to records and reports; missing, redacted, or stale Replica data cannot establish infeasibility, user acceptance, or a lifecycle transition.\n+\n+#### Phase 2 Log events\n+\n+Phase 2 adds `task_deferred_intent_entered`, `task_deferred_intent_updated`, and `task_deferred_intent_exited` using the \u00a717 envelope. Each accepted change atomically records Task ref, `deferral_id`, expected/resulting Task version, old/new status, effective time, actor and authority source, idempotency key, optional reason/evidence refs, and before/after record fields or policy-permitted protected references. Entry records the explicit deferral action; updates include keep-deferred and review-schedule edits; exit records promotion or the admitted terminal outcome and clears the current record and pending review requests. Existing completion/failure/moot admission remains applicable; a terminal exit also appends its ordinary outcome event in the same transaction.\n+\n+Review choices use `decision_recorded` with Phase 2 `decision_kind: deferred_intent_review` and the \u00a717.0 payload, including review ref, available actions, chosen action, optional reason capture, and any accepted Preference or Objective refs. An accepted review action records its decision atomically with any Task change, newly accepted Preference, and review-request update. Skipping or ignoring records no acceptance and no invented Preference. Every mutation checks the current Task version and episode id and uses existing idempotency, authority, worker-candidate, and sync-conflict rules. Corrections append \u00a717.3 correction links. Entry and exit invalidate affected Plans and reports and use existing \u00a729 recalculation triggers; metadata-only review edits normally mark derived views stale.\n+\n ---\n \n ## 10. Task Preconditions and Effects\n@@ -2836,7 +2883,7 @@ Log review should not become a blame ritual. It is a model-maintenance Task for\n \n - Friction annotations: when actual Task duration significantly exceeds planned duration, the review should prompt the user for an optional friction note explaining the cause. Friction notes feed the Bayesian duration learning system. See `UBU-Q0113`, `UBU-Q0116`.\n - Streak surfacing: recurring Task completion chains are surfaced as motivational signals. See `UBU-Q0111`.\n-- Deferred-intent review: Tasks in `DEFERRED_INTENT` status are periodically surfaced with a structured dismissal prompt. Tasks that are no longer attainable or desirable should be dismissed rather than accumulating indefinitely. See `UBU-Q0117`.\n+- Deferred-intent review: due `deferred_intent` Tasks surface on Log review or startup, with a default 30-day item interval and earlier review for material linked-Objective changes. The prompt offers promotion with explicit Preference commitment, keep deferred with a next review, permanent dismissal through `moot`, or Objective review; missed prompts never imply acceptance or dismissal. See \u00a79.7 and `UBU-D0308`.\n - Circadian affect pattern report: after sufficient Log history, time-of-day patterns in affect state may be surfaced as a standard report to inform planning priors.\n \n ### 17.9 Authority source metadata\ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -4887,3 +4887,17 @@ On 2026-10-10 the 20 open questions tagged `Phase 1b` were re-tagged `Phase 2` s\n Waiting Tasks and their blocked dependents are excluded before kernel dispatch. The horizon shows blocked waits and derived lateness/dependency risk; Calendar preview uses a waiting list and non-occupying review markers. Follow-up work is separately admitted and scheduled. Expected-by estimates neither reserve time nor certify readiness.\n \n Entry, edits, and exit are version-checked, idempotent, and logged atomically; terminal exits also retain the ordinary outcome event. Recalculation, Compartment policy, worker admission, and multi-device conflict rules retain their existing authority. Review and escalation never imply permission to send a message. This decision specifies Phase 2 behavior and makes no implementation claim.\n+\n+---\n+\n+## UBU-D0308: Deferred intent is retained work excluded from planning and resolved through explicit review\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a79.5, \u00a79.7, \u00a717.8. Resolves `UBU-Q0117`; completes its existing planning-exclusion direction, extends `UBU-D0227` for Phase 2, and follows `UBU-D0228`'s wire convention.\n+\n+`DEFERRED_INTENT` (`deferred_intent` on the wire) requires explicit user deferral, preserves Task identity and history, and carries one current record with episode id, entry time, optional reason and tagged defer-until boundary, review interval, last explicit review, and finite next review time. Capture needs no creation-time Preference; promotion supplies ordinary schedulability fields and cites an enabled, explicitly user-accepted pairwise Task or linked-Objective Preference. Low priority, missed execution, and inferred value cannot create deferral or commitment. Mandatory routine occurrences retain their existing skip/move controls.\n+\n+The CPU excludes deferred work and dependents with unsatisfied required dependencies before kernel dispatch. Deferred work cannot become filler, occupy Calendar time, earn effect credit, or contribute deferral time to duration learning. Ordinary Plan repair preserves execution history and external obligations; no `TaskSpec` extension is needed.\n+\n+Default review interval is 30 days, surfaced at Log review or startup when due; a chosen date delays ordinary review and a material admitted Objective change can surface it earlier. Dates never reactivate work. The four actions are promote, keep deferred with a future review, dismiss permanently through the existing narrow `moot_reason_code`, and review the linked Objective while retaining deferral. Keep, skip, and Objective review do not discharge an unresolved Preference requirement; the review cycle continues without forced expiry or inferred acceptance.\n+\n+Entry, edits, exit, and review decisions are version-checked, idempotent, and append-only logged under \u00a79.7; terminal exits also retain their ordinary outcome Log. Attention requests are coalesced per episode and respect existing Compartment, authority, worker-admission, recalculation, and device-sync rules. This decision specifies Phase 2 behavior and makes no implementation claim.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -1811,27 +1811,31 @@ Open.\n \n ## UBU-Q0117: deferred_intent task status and dismissal review\n \n-Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: planning horizon clarity, introspection completeness Resolved by: None Last scored: 2026-10-11 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: planning horizon clarity, introspection completeness Resolved by: UBU-D0308 Last scored: 2026-10-11 Scored from commit: None\n \n ### Question\n \n UbU needs a named Task status for ideas that are not yet commitments and not cancelled \u2014 things the user has explicitly decided not to plan but wants to remember. A `DEFERRED_INTENT` status distinct from open and cancelled would prevent such items from cluttering the planning horizon. The Log review introspection cycle should periodically surface deferred-intent Tasks for review, including prompting the user to dismiss Tasks that are no longer attainable or desirable. What is the complete deferred_intent model?\n \n ### Subquestions\n \n 1. What fields distinguish a `DEFERRED_INTENT` task from an open low-priority task: explicit user deferral action, optional defer-until date, optional reason?\n 2. How does `DEFERRED_INTENT` interact with the planner: are these tasks invisible to the skeleton sampler, or available as optional fill tasks with very low priority?\n 3. What Log entry is created when a Task is moved to `DEFERRED_INTENT`?\n 4. What is the review cadence for surfacing deferred-intent Tasks: weekly, monthly, on Log review, or triggered by related Objective changes?\n 5. What dismissal options does the review prompt offer: promote to active, keep deferred, cancel permanently, or escalate to an Objective review?\n \n ### Current direction\n \n `DEFERRED_INTENT` is a Phase 2 required status addition to the Task model. It is distinct from low-priority and distinct from cancelled. The periodic Log review report should surface deferred-intent Tasks with a structured dismissal prompt. The preference declaration requirement \u2014 that a Task must eventually have an explicit Preference commitment or be dismissed \u2014 should be enforced through this review cycle rather than at creation time.\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0308`; see DESIGN.md \u00a79.7 and \u00a717.8.\n+\n+Phase 2 `deferred_intent` records an explicit user deferral with an episode id, entry time, optional reason and defer-until boundary, and finite next review time. It is excluded before kernel dispatch, including filler work and dependents with unsatisfied required dependencies. Deferral, edits, exits, and review choices are logged atomically under the existing authority and sync rules.\n+\n+The default item review interval is 30 days, surfaced on due Log review/startup or earlier after a material linked-Objective change. Dates request review and never reactivate work. Review offers promotion with explicit accepted pairwise Preference commitment, keep deferred with a future review, terminal dismissal through an existing moot reason, or Objective review while retaining deferral. Preference is required through review/promotion rather than idea creation; ignored or postponed prompts cannot imply commitment or automatic dismissal.\n \n ---\n \n",
    "commit_message": "docs: resolve deferred-intent lifecycle and review (UBU-Q0117)",
    "validation_notes": [
      "Generated with git diff --no-index against current working-tree files and proposed in-memory contents; normalized only diff path headers and omitted index lines.",
      "git apply --check - and git apply --check --whitespace=error - passed against the working tree.",
      "Verified the selected question heading and its own Resolution share one hunk; replaced its actual Open. text and preserved metadata except Status and Resolved by.",
      "Verified only three allowlisted files change, UBU-D0308 is unused and added once, and no new question ids are allocated.",
      "Validated JSON keys, types, allowed filenames, identifier patterns, and serialization round trip.",
      "No repository files were written. Changes specify Phase 2 design without an implementation or runtime-test claim."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0117"
    ],
    "decisions_added": [
      "UBU-D0308"
    ],
    "requires_human_review": false
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "openai-gpt-6-UBU-Q0117-20261011",
    "patch_applies": true,
    "allowlist_passed": true,
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "question_schema_passed": true,
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
- Allowed question `Status:` values are `Archived`, `Decomposed`, `Deferred`, `Open`, `Solved`, `Superseded`; a resolved question must use `Status: Solved`.
- Reject `Status: Resolved` as a question-schema violation. Set `preserves_question_schema` to false and require `Status: Solved` as the fix; `Resolved by:` is a separate metadata field.
- Do not score your own provider's proposal unless explicitly asked for diagnostic self-score.

Return only JSON.
