# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `codex`

## Selected question

Question ID: `UBU-Q0052`  
Question title: `Discovery-mode action inference and override semantics`  
Base commit: `8e9c1551daab2b74e9e27908360f834512f71a22`

```markdown
## UBU-Q0052: Discovery-mode action inference and override semantics

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0050 Blocks: Discovery mode, mobile sensor workflow, behavior reconciliation, habit-pattern inference Resolved by: None Last scored: 2026-05-27 Scored from commit: None

### Question

What is the minimum Phase 1 model for discovery mode, actual-action inference, and user override semantics?

### Subquestions

1. How does the user select discovery mode, pause it, exit it, and inspect what it collected?
2. Which mobile sensor, integration, quick-note, or app-state signals are acceptable Phase 1 discovery inputs?
3. How should UbU represent actual user action when it differs from the Calendar?
4. When does an override become a Log entry, Snapshot, Preference update, Task status change, Objective reconsideration, or unresolved review item?
5. What clarification prompt is required before treating repeated behavior as a habit pattern?
6. How should UbU reconcile undetailed or under-specified time periods without overclaiming certainty?
7. How should Discovery mode preserve user sovereignty and avoid covert surveillance semantics?

### Current direction

Discovery mode is a user-selectable workflow state available at any time, especially from the mobile app. It may gather evidence useful for later Log review and UbU-directed reconciliation, but inferred observations are not final truth until accepted, corrected, or otherwise handled according to user-visible rules.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "codex-ubu-q0052-d0201",
    "provider_id": "codex",
    "model_name": "gpt-5",
    "question_id": "UBU-Q0052",
    "base_commit": "8e9c1551daab2b74e9e27908360f834512f71a22",
    "summary": "Resolve UBU-Q0052 by defining Discovery mode as an opt-in, visible workflow state that stores narrow Phase 1 signals as reviewable evidence, admits actual actions and overrides through existing Log/Snapshot/Preference/Task/Objective/recalculation rules, and requires user clarification before habit-pattern inference.",
    "rationale": "The proposal keeps Phase 1 minimal by using existing Log review, plan_realized, decision_recorded, Snapshot, Task lifecycle, Preference, Objective, and recalculation mechanisms instead of adding a broad realtime ontology. It answers the selected question while preserving user sovereignty, Compartment hard boundaries, and the no-covert-surveillance semantics already accepted for realtime and discovery workflows.",
    "changed_files": [
      "DESIGN.md",
      "DECISIONS.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -879,8 +879,10 @@\n \n 7. **Discovery mode when appropriate**\n    - Allow the user to choose discovery mode at any time.\n-   - Use discovery mode to collect or preserve evidence about what the user is doing or did, especially from mobile sensors or configured integrations.\n-   - Defer ambiguous interpretation until later Log review or user-visible reconciliation.\n+   - Show active, paused, ended, and pending-review state, enabled sources, routing or retention limits, and evidence count.\n+   - Provide pause, resume, exit, inspect, reject, delete, and review controls.\n+   - Collect only enabled Phase 1 evidence such as quick notes, user-selected current action, Calendar context, configured integration events, coarse motion or location category, focus state, and app-state category.\n+   - Treat inferred observations as reviewable evidence, not final truth; defer ambiguous interpretation until Log review or user-visible reconciliation.\n \n The Phase 1 version may be simple and fixture-backed. Fixture behavior must be labeled as fixture behavior, mock recommendations must not be described as implemented planning, and public demos should show the exact boundary between explicit UbU objects, approved fixtures, and implemented planner behavior. The public demonstration pattern is bootstrap, one recommended next Task, explanation, user feedback, recalculation, and full-Plan inspection. It does not require broad email, text-message, file, invoice, note, or personal-data ingestion. It must not claim complete life-modeling, therapeutic authority, autonomous life coaching, complete planning automation, full privacy isolation, Phase 2 sync, or Phase 3 multi-user coordination. The purpose is to demonstrate the core UbU experience: one meaningful next action, with an explanation, grounded in explicit state.\n \n@@ -1682,13 +1684,34 @@\n \n ### 12.2 Discovery mode\n \n-**Discovery mode** is a user-selectable workflow state, not a fourth instance operating mode. A user may choose discovery mode at any time to let UbU gather or preserve evidence about actual behavior for later reconciliation.\n-\n-Discovery mode is especially important for a mobile app because embedded sensors, location categories, motion state, app activity, calendar context, and explicit quick notes may help reconstruct what happened during undetailed or under-specified time periods.\n-\n-Discovery mode must preserve user sovereignty. Sensor-derived, integration-derived, or inferred observations are evidence, not final truth. Ambiguous periods should be reconciled through later Log review, user clarification, or correction before UbU treats them as stable habit patterns, Preference changes, or Objective evidence.\n-\n-Open details are tracked in `UBU-Q0052`.\n+**Discovery mode** is a user-selectable workflow state, not a fourth instance operating mode. It is off by default and may be started, paused, resumed, exited, or inspected by the user at any time. The UI must show active capture state, enabled sources, local/cloud routing status, retention limits, and pending-review evidence count.\n+\n+Phase 1 session states are `inactive`, `active`, `paused`, `ended`, and `pending_review`. A minimal `DiscoveryEvidenceItem` is a reviewable candidate artifact with session ref, effective time or interval, source kind, signal kind, payload ref or redacted summary, confidence, Compartment or low-security label, provenance, and review status. Raw evidence is not final truth.\n+\n+Allowed Phase 1 discovery inputs are intentionally narrow:\n+\n+- explicit quick notes, user-selected current action, voice or text notes intentionally captured by the user, and manual start/stop markers;\n+- UbU app state, Task controls, Calendar or default Plan context, timers, focus state, and foreground app category or app identifier when explicitly enabled;\n+- configured integration events already allowed by Compartment, External Event, and External Reference policy;\n+- coarse motion category such as stationary, walking, transit, or driving;\n+- coarse user-defined location category or geofence event, not continuous raw location by default;\n+- device state needed for interpretation, such as screen on/off or network/offline state.\n+\n+Phase 1 discovery excludes covert or broad capture by default: continuous microphone, camera, screen recording, keystroke logging, raw message bodies, raw file contents, raw GPS trails, and cross-Identity sharing are outside the default discovery input set. Any later use of those sources requires a separate explicit mode, Compartment review, routing disclosure, and user approval.\n+\n+Admission rules:\n+\n+- A user-confirmed actual action that reconciles a planned interval is recorded with `plan_realized`; if it completes, fails, or moots a Task, the corresponding Task lifecycle Log is also written.\n+- A user override of the current recommendation records `decision_recorded` with `decision_kind = system_recommendation_overridden`, `authority_source = user_override`, chosen action or Task when known, optional reason, and a `user_override` recalculation trigger when planner-relevant.\n+- A user-declared current state becomes a Snapshot only when it asserts observed state such as affect, availability, location category, or capacity.\n+- Preferences change only through explicit accepted pairwise Preference statements. Repeated behavior and inferred reasons are not Preferences.\n+- Task estimates, dependencies, status, Objective annotations, or Objective status change only after user acceptance or normal validated admission. Otherwise the item remains an unresolved review item.\n+\n+Undetailed periods should be represented with uncertainty rather than filled in by inference. The review UI should allow `unknown`, `private`, `rest`, `interruption`, `planned Task`, `different Task`, `quick note`, `other`, and free-text correction. UbU must not infer Preference changes, Objective failure, habit patterns, or moral meaning from unknown or private time.\n+\n+Before treating repeated behavior as a habit pattern, UbU must ask a clarification prompt such as: `I have seen this pattern more than once: [behavior] during [context]. Should UbU plan around it, help you change it, treat it as not a pattern, or leave it unresolved?` Valid answers are `endorse_and_plan_around`, `tolerate_but_review`, `unwanted_help_change`, `not_a_pattern`, and `leave_unresolved`.\n+\n+Discovery mode preserves sovereignty by remaining visible, opt-in, source-scoped, pausable, inspectable, and correctable. Users may reject, delete where retention policy permits, mark private/unknown, correct, defer, or accept evidence before it becomes canonical state. Compartment policy, `no_cloud_llm`, `no_external_export`, allowed-device, and allowed-integration denials remain hard boundaries.\n \n ---\n \n@@ -2307,7 +2330,7 @@\n \n A Log review Task may:\n \n-- reconcile undetailed or under-specified time periods;\n+- reconcile DiscoveryEvidenceItems and undetailed or under-specified time periods by accepting, correcting, rejecting, deleting, marking private/unknown, or deferring them;\n - ask whether a user override reflected a better local judgment, bad timing, missing preconditions, wrong duration estimate, stale affect data, social pressure, or changed Preference;\n - compare planned Tasks against completed, failed, moot, snoozed, rejected, or unobserved Tasks;\n - convert user-approved observations into corrected Logs, Snapshots, Preferences, Objective annotations, Task estimate updates, or recalculation triggers;\ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -3613,7 +3613,7 @@\n - `UBU-Q0053` is resolved for Phase 1.\n - Self-determination theory and theory of planned behavior remain interface and reporting influences, not Phase 1 ontology.\n - Review Tasks can be implemented with existing Task, Log, Snapshot, Preference, Objective, Report, and recalculation-trigger mechanisms.\n-- Detailed preference-calibration examples remain in `UBU-Q0051`; discovery-mode inference remains in `UBU-Q0052`; deeper affect/personality modeling remains post-MVP in `UBU-Q0074`.\n+- Preference-calibration examples are resolved by `UBU-D0200`; discovery-mode inference is resolved by `UBU-D0201`; deeper affect/personality modeling remains post-MVP in `UBU-Q0074`.\n \n ---\n \n@@ -4241,3 +4241,43 @@\n - Future richer examples may be added as versioned prompt content without schema migration unless they change admission rules or canonical fields.\n \n ---\n+\n+## UBU-D0201: Discovery mode uses reviewable evidence and explicit override admission\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a7\u00a74.1.2, 12.2, 17.8\n+\n+Resolved question: `UBU-Q0052`.\n+\n+Discovery mode is a user-selectable workflow state, not an instance mode and not always-on surveillance. It is off by default. The user may start, pause, resume, exit, inspect, reject, delete where retention policy permits, correct, defer, or accept collected evidence. UI must show active capture state, enabled sources, routing status, retention limits, and pending-review count.\n+\n+Phase 1 session states are `inactive`, `active`, `paused`, `ended`, and `pending_review`. Discovery observations are stored as reviewable candidate artifacts, not final truth. A minimal evidence item records session ref, effective time or interval, source kind, signal kind, payload ref or redacted summary, confidence, Compartment or low-security label, provenance, and review status.\n+\n+Allowed Phase 1 inputs are narrow:\n+\n+- explicit quick notes, user-selected current action, intentional voice/text notes, and manual start/stop markers;\n+- UbU app state, Task controls, Calendar or default Plan context, timers, focus state, and explicitly enabled foreground app category or app identifier;\n+- configured integration events already allowed by existing External Event and External Reference policy;\n+- coarse motion state, coarse user-defined location category or geofence event, and basic device state such as screen or network state.\n+\n+Excluded by default: covert continuous microphone, camera, screen recording, keystroke logging, raw message bodies, raw file contents, raw GPS trails, and cross-Identity sharing. Later use of any excluded source requires a separate explicit mode, Compartment review, routing disclosure, and user approval.\n+\n+Admission rules:\n+\n+- accepted actual actions that reconcile planned intervals use `plan_realized`; accepted Task outcomes also write `task_completed`, `task_failed`, or `task_moot`;\n+- overrides of a recommendation write `decision_recorded` with `decision_kind = system_recommendation_overridden`, `authority_source = user_override`, chosen action or Task when known, optional reason, and a planner-relevant `user_override` recalculation trigger;\n+- user-declared observed state becomes a Snapshot only when it asserts current state such as affect, availability, capacity, or location category;\n+- Preferences change only through explicit accepted pairwise Preference statements; repeated behavior and inferred reasons are not Preferences;\n+- Task estimates, dependencies, status, Objective annotations, or Objective status change only through user acceptance or normal validated admission; otherwise the evidence remains an unresolved review item.\n+\n+Undetailed time periods must preserve uncertainty. Review may mark a period as `unknown`, `private`, `rest`, `interruption`, `planned_task`, `different_task`, `quick_note`, or `other`. UbU must not infer Preference changes, Objective failure, habit patterns, or moral meaning from unknown or private time.\n+\n+Before treating repeated behavior as a habit pattern, UbU asks a clarification prompt: `I have seen this pattern more than once: [behavior] during [context]. Should UbU plan around it, help you change it, treat it as not a pattern, or leave it unresolved?` Valid answers are `endorse_and_plan_around`, `tolerate_but_review`, `unwanted_help_change`, `not_a_pattern`, and `leave_unresolved`.\n+\n+**Consequences:**\n+\n+- `UBU-Q0052` is resolved for Phase 1.\n+- Discovery mode can support mobile evidence gathering without converting sensor inference into canonical truth.\n+- Overrides are authoritative user evidence, but durable model changes still pass through explicit Log, Snapshot, Preference, Task, Objective, and recalculation admission rules.\n+- Habit-pattern inference requires user clarification before it affects planning as a stable pattern.\n+\n+---\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -636,31 +636,8 @@\n ---\n \n ## UBU-Q0052: Discovery-mode action inference and override semantics\n-\n-Status: Open Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0050 Blocks: Discovery mode, mobile sensor workflow, behavior reconciliation, habit-pattern inference Resolved by: None Last scored: 2026-05-27 Scored from commit: None\n-\n-### Question\n-\n-What is the minimum Phase 1 model for discovery mode, actual-action inference, and user override semantics?\n-\n-### Subquestions\n-\n-1. How does the user select discovery mode, pause it, exit it, and inspect what it collected?\n-2. Which mobile sensor, integration, quick-note, or app-state signals are acceptable Phase 1 discovery inputs?\n-3. How should UbU represent actual user action when it differs from the Calendar?\n-4. When does an override become a Log entry, Snapshot, Preference update, Task status change, Objective reconsideration, or unresolved review item?\n-5. What clarification prompt is required before treating repeated behavior as a habit pattern?\n-6. How should UbU reconcile undetailed or under-specified time periods without overclaiming certainty?\n-7. How should Discovery mode preserve user sovereignty and avoid covert surveillance semantics?\n-\n-### Current direction\n-\n-Discovery mode is a user-selectable workflow state available at any time, especially from the mobile app. It may gather evidence useful for later Log review and UbU-directed reconciliation, but inferred observations are not final truth until accepted, corrected, or otherwise handled according to user-visible rules.\n-\n-### Resolution\n-\n-Open.\n-\n+Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0050 Blocks: Discovery mode, mobile sensor workflow, behavior reconciliation, habit-pattern inference Resolved by: UBU-D0201 Last scored: 2026-05-27 Scored from commit: None\n+Resolved. See UBU-D0201.\n ---\n \n ## UBU-Q0053: Calendar preview and Log review psychological annotations\n",
    "commit_message": "Resolve discovery-mode action inference semantics",
    "validation_notes": [
      "Patch modifies only allowed canonical files.",
      "Generated patch passed git apply --check against the current workspace.",
      "UBU-Q0052 is converted to a compact solved tombstone anchored at its own heading.",
      "No new open questions are introduced.",
      "requires_human_review is true because UBU-Q0052 is marked Human approval required."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0052"
    ],
    "decisions_added": [
      "UBU-D0201"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "codex-ubu-q0052-d0201",
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
