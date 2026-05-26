# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai-codex`

## Selected question

Question ID: `UBU-Q0006`  
Question title: `Objective and Task Explosion Control`  
Base commit: `89428f48d1bea78c61d872f955e05a7111282a7b`

```markdown
## UBU-Q0006: Objective and Task Explosion Control

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0005 Blocks: Phase 1 implementation Resolved by: Unresolved Last scored: 2026-05-26 Scored from commit: None

GitHub events may create analysis Objectives or Tasks. This could generate excessive noise.

### Question

1. When does a GitHub event deserve a new Objective?
2. When is it appended to an existing Objective?
3. When is it merely logged?
4. When does it create a Task?
5. How are duplicate analysis Objectives detected?
6. Do analysis Objectives inherit value from parent Objectives?
7. Do analysis Objectives automatically become moot or completed after resolution?
8. How does UbU prevent its own management process from becoming too noisy?

### Resolution

Unresolved.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "codex-ubu-q0006-d0183",
    "provider_id": "openai-codex",
    "model_name": "gpt-5-codex",
    "question_id": "UBU-Q0006",
    "base_commit": "89428f48d1bea78c61d872f955e05a7111282a7b",
    "summary": "Resolve Objective and Task explosion control by adding bounded admission rules for GitHub-derived Objectives, analysis Objectives, Tasks, duplicate detection, value inheritance, automatic closure, and noise budgets.",
    "rationale": "The proposal narrows GitHub event handling so unique events remain logged, but only durable work, bounded analysis, or concrete next actions create additional planning objects. Analysis Objectives become parent-scoped, deduplicated, instrumentally valued, and automatically closed through normal lifecycle transitions, preventing management work from expanding without reviewable value.",
    "changed_files": [
      "DESIGN.md",
      "DECISIONS.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -3032,11 +3032,28 @@ Phase 1 event-class defaults are:\n - `ci_failed`: Creates an External Event and a failure-analysis or fix Task, and may request Automation Worker assignment when an eligible worker has an explicit assignment path.\n - `ci_passed`: Creates an External Event; usually logs or unblocks existing Tasks and may trigger projection update, but creates a new Task only when a next action such as merge review is required.\n - `milestone_changed`: Creates an External Event; may create or update a release Objective, deadline fact, Calendar constraint, or release-planning Task.\n \n-GitHub events create or update Objectives only when they introduce durable desired state, release scope, or accepted project work that is not already represented by an Objective. Routine comments, labels, CI transitions, and PR updates should attach as evidence to existing Objectives or Tasks rather than creating analysis Objectives by default.\n-\n-GitHub events create Tasks when they imply a concrete next action: triage, clarification, review, fix, projection repair, reconciliation, release-planning, or contributor follow-up. Imported Tasks use normal Task admission, External References, provenance, Compartment/export checks, and duplicate detection.\n+GitHub Objective and Task creation is capped by admission rules:\n+\n+- A GitHub event creates a new Objective only when it introduces durable desired state, release scope, accepted project work, or an investigation that cannot be represented as a Task under an existing Objective.\n+- An analysis Objective is a one-time Objective tagged `analysis`, linked to at least one parent Objective or source External Reference, and must include a crisp question, termination condition, owner or review path, source refs, and duplicate key.\n+- An event is appended to an existing Objective when the source object is already represented, shares the same durable desired state, or supplies evidence, blocker, status, acceptance, scope, deadline, or contributor-follow-up information for that Objective.\n+- A unique in-scope event is merely logged only in the sense that it creates the External Event and Log required by this section but no Objective or Task. This is the default for evidence-only labels, routine comments, status churn, CI transitions that confirm known state, and low-priority observations that only mark caches stale.\n+- A Task is created only for a concrete next action with an executor path or review owner and an actionable completion criterion: triage, clarification, review, fix, projection repair, reconciliation, release planning, merge readiness, contributor follow-up, or bounded analysis. If an active equivalent Task already exists, the event is appended as evidence instead.\n+\n+Analysis Objective duplicate detection uses a normalized key over parent Objective refs, analysis kind or failure class, normalized source refs, affected artifact or GitHub object, unresolved question text, and active or terminal status. If a matching active Objective exists, the new event is appended. If a matching terminal Objective exists and the new evidence materially reopens the question, UbU creates a superseding analysis Objective linked by `supersedes`; otherwise it logs duplicate or idempotent evidence.\n+\n+Analysis Objectives do not receive independent user value in Phase 1. They derive instrumental scheduling value from parent Objective refs and accepted Preferences, capped by the parent scope. An analysis Objective without a valid parent or durable source link is under-specified and should create a clarification or review Task rather than compete as high-value work.\n+\n+Analysis Objectives close automatically when their termination condition is satisfied by accepted evidence. They transition to `completed` when the analysis question is answered or the required artifact is accepted; they transition to `abandoned`, `invalid`, or `superseded`, or cause related Tasks to become `moot` with accepted reason codes, when the source work disappears, duplicates another work item, becomes obsolete, or is replaced by a newer plan structure. All closures are logged.\n+\n+Noise-control defaults:\n+\n+- one GitHub delivery may create at most one new Objective and one immediate Task unless a human-approved decomposition or worker mutation request creates a Container and child Tasks;\n+- low-priority evidence is batched into reconciliation, Calendar preview, Log review, or risk-report refresh instead of creating prompt-facing Tasks;\n+- generated analysis work needs a review window, owner or worker assignment path, and stale-after policy;\n+- repeated equivalent events update External References, Logs, verification metadata, or cache staleness rather than creating more Objectives or Tasks.\n \n A GitHub event triggers recalculation with trigger kind `github_update` when it can affect the current or next recommended Task, a dependency or precondition, Task lifecycle state, Objective status, deadline or milestone constraints, worker assignment need, projection state, or risk-report validity. Low-priority observations that do not affect the current Plan may mark Calendar, projection, explanation, or report caches stale.\n \n Automation Worker assignment is never caused by raw GitHub data alone. The GitHub event must first create or update an admitted Task or Delegation Substrate packet, and the parent UbU instance must explicitly assign eligible worker work under the accepted worker-assignment model.\ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -3018,8 +3018,8 @@ Phase 1 triage rules for the candidate event classes are:\n - `ci_failed`: create an External Event plus a failure-analysis or fix Task; request Automation Worker assignment only when an admitted Task or Delegation Substrate packet has an eligible worker and explicit parent assignment path.\n - `ci_passed`: create an External Event; usually unblock or complete waiting Tasks and may request projection update; create a new Task only when a next action such as merge review, release step, or projection repair is required.\n - `milestone_changed`: create an External Event; create or update a release Objective, deadline fact, Calendar constraint, or release-planning Task when the change affects planned work.\n \n-GitHub events create Objectives only when they introduce durable desired state, release scope, or accepted project work that is not already represented by an Objective. Routine comments, labels, CI transitions, PR updates, and reviews attach as evidence to existing Objectives or Tasks rather than creating analysis Objectives by default. The remaining noise-control details stay in `UBU-Q0006`.\n+GitHub events create Objectives only when they introduce durable desired state, release scope, or accepted project work that is not already represented by an Objective. Routine comments, labels, CI transitions, PR updates, and reviews attach as evidence to existing Objectives or Tasks rather than creating analysis Objectives by default.\n \n GitHub events create Tasks when they imply concrete next action: triage, clarification, review, fix, projection repair, reconciliation, release planning, merge readiness, or contributor follow-up. Imported Tasks use normal Task admission, External References, provenance, Compartment/export checks, and duplicate detection.\n \n@@ -3039,8 +3039,7 @@ Missed events are reconstructed during reconciliation by comparing live GitHub is\n \n - `UBU-Q0005` is resolved for Phase 1 implementation.\n - The candidate MVP event classes have deterministic default triage without requiring GitHub to become canonical state.\n-- `UBU-Q0006` remains open for stricter Objective and Task explosion control, especially around analysis Objectives.\n-- `UBU-Q0004` remains open for the exact `pipeline_state` storage model, but label and projection events can already be triaged as evidence or drift.\n+- `UBU-Q0004` is resolved by `UBU-D0182`; label and projection events can already be triaged as evidence or drift.\n - `UBU-Q0010` remains open for token custody; this decision does not require any specific actor to hold GitHub write credentials.\n - GitHub event handling now aligns with append-only Logs, External References, worker assignment, recalculation triggers, and managed projection reconciliation.\n \n@@ -3668,3 +3667,44 @@ Every accepted `pipeline_state` transition creates a `pipeline_state_transitione\n - Worker authority remains request-only for pipeline changes.\n \n ---\n+\n+## UBU-D0183: GitHub analysis work uses parent scope and noise budgets\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a726.5\n+\n+Resolved question: `UBU-Q0006`.\n+\n+GitHub-derived Objectives and Tasks are admitted only when they reduce uncertainty or create executable work. Every unique in-scope GitHub observation still creates the External Event and Log required by `UBU-D0163`; most observations do not create new Objectives or Tasks.\n+\n+New Objective admission:\n+\n+- create a durable Objective only for durable desired state, release scope, or accepted project work not already represented;\n+- create an analysis Objective only when the investigation cannot be represented as a Task under an existing Objective and has a parent Objective or durable source link, a crisp question, termination condition, owner or review path, source refs, and duplicate key;\n+- otherwise append the event as evidence to an existing Objective or Task.\n+\n+Task admission:\n+\n+- create a Task only for a concrete next action with an actionable completion criterion, such as triage, clarification, review, fix, projection repair, reconciliation, release planning, merge readiness, contributor follow-up, or bounded analysis;\n+- append evidence to an active equivalent Task instead of creating a duplicate;\n+- batch low-priority evidence into reconciliation, Calendar preview, Log review, or risk-report refresh when no immediate action is needed.\n+\n+Analysis Objective duplicate detection uses a normalized key over parent Objective refs, analysis kind or failure class, source refs, affected artifact or GitHub object, unresolved question text, and active or terminal status. A matching active Objective receives appended evidence. A matching terminal Objective is reopened only by a superseding analysis Objective when new evidence materially reopens the question; otherwise the event is logged as duplicate or idempotent evidence.\n+\n+Analysis Objectives are instrumental. They do not receive independent Preferences in Phase 1. Scheduling value derives from parent Objective refs and is capped by the parent scope. A candidate analysis Objective without a parent or durable source link is under-specified.\n+\n+Analysis Objectives close through normal Objective transitions. They complete when accepted evidence answers the question or accepts the artifact. They become terminal or make related Tasks moot with accepted reason codes when duplicated, superseded, externally satisfied, obsolete, invalid, or no longer relevant. Closures are logged.\n+\n+Noise budget defaults:\n+\n+- one GitHub delivery creates at most one new Objective and one immediate Task unless a human-approved decomposition or worker mutation request creates a Container and child Tasks;\n+- generated analysis work has a review window, owner or worker assignment path, and stale-after policy;\n+- repeated equivalent events update External References, Logs, verification metadata, or cache staleness rather than creating more work.\n+\n+**Consequences:**\n+\n+- `UBU-Q0006` is resolved for Phase 1.\n+- GitHub event handling remains task-oriented and does not turn every comment, label, CI transition, or PR update into management work.\n+- Analysis work can still be modeled when it is bounded, parent-scoped, deduplicated, and automatically closed.\n+- Implementations can enforce Objective and Task admission before worker assignment, recalculation, or GitHub projection requests.\n+\n+---\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -114,22 +114,4 @@ Resolved. See UBU-D0163.\n ## UBU-Q0006: Objective and Task Explosion Control\n-\n-Status: Open Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0005 Blocks: Phase 1 implementation Resolved by: Unresolved Last scored: 2026-05-26 Scored from commit: None\n-\n-GitHub events may create analysis Objectives or Tasks. This could generate excessive noise.\n-\n-### Question\n-\n-1. When does a GitHub event deserve a new Objective?\n-2. When is it appended to an existing Objective?\n-3. When is it merely logged?\n-4. When does it create a Task?\n-5. How are duplicate analysis Objectives detected?\n-6. Do analysis Objectives inherit value from parent Objectives?\n-7. Do analysis Objectives automatically become moot or completed after resolution?\n-8. How does UbU prevent its own management process from becoming too noisy?\n-\n-### Resolution\n-\n-Unresolved.\n-\n+Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0005 Blocks: Phase 1 implementation Resolved by: UBU-D0183 Last scored: 2026-05-26 Scored from commit: None\n+Resolved. See UBU-D0183.\n ---\n",
    "commit_message": "Resolve GitHub Objective and Task explosion controls",
    "validation_notes": [
      "Patch validated with git apply --check against the current workspace.",
      "Only allowed canonical files are modified.",
      "UBU-Q0006 is converted to a compact solved tombstone anchored at its own heading.",
      "Superseded references in UBU-D0163 are removed or updated."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0006"
    ],
    "decisions_added": [
      "UBU-D0183"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "codex-ubu-q0006-d0183",
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
