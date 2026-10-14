# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0113`  
Question title: `Friction annotation on Log entries`  
Base commit: `33f0d132cde965e0237f7224edb0bc89c9c95605`

```markdown
## UBU-Q0113: Friction annotation on Log entries

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0107 Blocks: duration model improvement, introspection quality Resolved by: None Last scored: 2026-10-14 Scored from commit: None

### Question

A lightweight optional friction annotation on Log entries — "took longer because the API was down," "interrupted three times," "underestimated complexity" — would feed the duration model review and the introspection cycle without requiring a separate system. What is the minimum friction annotation schema on Log entries, and how does it interact with the Bayesian duration learning system?

### Subquestions

1. Is friction annotation a free-text field, a structured enum, or both?
2. Which Log entry types support friction annotation: Task completion, Task failure, plan deviation, others?
3. How do friction annotations feed the Bayesian duration model — are they a correction signal, a distribution adjustment trigger, or a manual override?
4. How are friction annotations surfaced during the Log review introspection cycle?
5. Should friction annotation be prompted by the system when actual duration significantly exceeds planned duration?
6. How does friction annotation interact with the counterfactual log (Q0107) when the friction was a user decision rather than an external factor?

### Current direction

Friction annotation on Log entries is a Phase 2 required feature. It should be an optional structured field on Task completion Log entries, prompted by the system when actual duration deviates significantly from planned duration. Free-text is acceptable for Phase 2 MVP; structured taxonomy is post-MVP.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0113-friction-annotation-v1",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0113",
    "base_commit": "33f0d132cde965e0237f7224edb0bc89c9c95605",
    "summary": "Define optional attributed friction notes, non-blocking overrun prompts and an evidence-preserving duration-learning boundary.",
    "rationale": "A bounded free-text envelope and existing Log annotation/correction links keep Phase 2 lightweight and auditable. Original duration evidence and counterfactual decisions remain intact; friction informs review without silently altering estimates. The Bayesian update rule remains with existing post-MVP question UBU-Q0116.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5590,3 +5590,19 @@ Record choices through existing decision Logs with optional reasons and no Prefe\n Repeated bypasses support requested review of warning quality/configuration. Unsolicited review reminders require an explicitly enabled, scoped finite count/window and attention budget; they prove no inconsistency. Persist suppression, deduplicate operations and honor rejected claims across surfaces. DESIGN.md \u00a722.4 specifies required enablement fixtures, not passing implementation evidence; broader proactive extrospection thresholds remain `UBU-Q0089`.\n \n ---\n+\n+## UBU-D0357: Optional friction notes explain execution without overriding duration evidence\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a717.8.1. Resolves `UBU-Q0113`; applies `UBU-D0198` while leaving the Bayesian update rule to `UBU-Q0116`.\n+\n+Phase 2 adds optional `event_payload.friction` with bounded free-text `note_text`, a Task ref for Task-specific `plan_realized` deviations, and optional related `decision_recorded` refs. Support completed and failed Task outcomes and those deviations through existing event types; later additions/corrections use the existing append-only Log links and preserve attribution, model provenance and outcome evidence. A cause taxonomy remains post-MVP.\n+\n+Offer a non-blocking prompt once per completion when actual execution duration is at least 1.5 times its original planned baseline and exceeds it by at least 300 seconds. Comparable admitted evidence is required; waiting/deferred time is excluded. Manual notes remain available; Skip and disabling prompts preserve execution and suppress repeated requests. Log alert dismissal under the counterfactual contract without Preference inference.\n+\n+Log review displays the original prediction, actual evidence/coverage, difference, current notes/corrections and permitted linked decisions. Friction is qualitative review context: it cannot by itself subtract duration, exclude/down-weight observations or alter distributions. Count a completion once across notes; failure/partial/deviation evidence is not a completed-duration sample. Statistical grouping, censoring, weighting and covariates remain `UBU-Q0116`'s post-MVP work, and learned estimates require explicit user review and ordinary validated admission.\n+\n+Link friction arising from user choices to the required counterfactual record without replacing it or making reasons mandatory. Inferred reasons remain inferred; omission establishes no acceptance, absence of friction or causal conclusion. All notes and derivatives inherit current privacy/retention restrictions.\n+\n+DESIGN.md \u00a717.8.1 supplies schema, prompt, review and enablement requirements. This resolves the annotation/interface contract, not Bayesian mathematics or implementation conformance; no planning-kernel change is required.\n+\n+---\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -3063,11 +3063,35 @@ Log review should not become a blame ritual. It is a model-maintenance Task for\n \n **Phase 2 and post-MVP additions to the Log review report:**\n \n-- Friction annotations: when actual Task duration significantly exceeds planned duration, the review should prompt the user for an optional friction note explaining the cause. Friction notes feed the Bayesian duration learning system. See `UBU-Q0113`, `UBU-Q0116`.\n+- Friction annotations: optional attributed notes, overrun prompts and duration-learning review evidence follow \u00a717.8.1 (`UBU-D0357`); Bayesian updating remains post-MVP under `UBU-Q0116`.\n - Streak surfacing: recurring Task completion chains are surfaced as motivational signals. See `UBU-Q0111`.\n - Deferred-intent review: due `deferred_intent` Tasks surface on Log review or startup, with a default 30-day item interval and earlier review for material linked-Objective changes. The prompt offers promotion with explicit Preference commitment, keep deferred with a next review, permanent dismissal through `moot`, or Objective review; missed prompts never imply acceptance or dismissal. See \u00a79.7 and `UBU-D0308`.\n - Circadian affect pattern report: after sufficient Log history, time-of-day patterns in affect state may be surfaced as a standard report to inform planning priors.\n \n+### 17.8.1 Phase 2 friction annotations\n+\n+**Optional evidence (`UBU-D0357`).** Friction is a small object in `event_payload.friction`, with free-text content and no Phase 2 cause taxonomy:\n+\n+- `note_text`: required nonblank text, at most 2048 Unicode characters;\n+- `task_ref`: required for a Task-specific `plan_realized` deviation; otherwise inherited from the Task outcome's `target_ref`. If supplied on a Task outcome, it must match that target;\n+- `decision_refs`: optional list of at most eight distinct `decision_recorded` Log refs relevant to the same execution context.\n+\n+Support friction on `task_completed`, `task_failed`, and Task-specific deviations recorded with `plan_realized`; no new deviation event type is introduced. Other event types retain ordinary notes. The friction object may be supplied when the event is admitted, or later through `log_annotation_added` with `annotation_of` pointing to that outcome/deviation entry. Reject invalid or oversized notes without silent truncation. Each note inherits the shared envelope's actor, Device, effective/recorded times and provenance. Model explanations remain labelled advisory candidates until accepted; acceptance retains their inferred/model provenance and establishes no proven cause. Omission means no explanation supplied, not friction-free execution.\n+\n+Replacing or withdrawing a note appends `log_correction_added`, with `correction_of` pointing to the entry containing the superseded note and the correction explicitly scoped to its friction assertion. Originals remain unchanged; correcting friction alone cannot change completion or duration claims. On a friction-only correction, `event_payload.friction` is the replacement object or `null` for withdrawal. Review and future learning follow admitted correction links, expose unresolved competing corrections, and retain distinct attributed notes rather than selecting by latest timestamp.\n+\n+**Overrun prompt.** Offer one non-blocking Add note / Skip prompt for a completion without a current friction note, at completion or during regular Log review if not already offered, when comparable durations satisfy `actual_seconds >= 1.5 * planned_seconds` and `actual_seconds - planned_seconds >= 300`. This is a UI heuristic, not an outlier or likelihood rule. `planned_seconds` is the positive planned execution duration from the Plan/version used at the start of that attempt, summed across pieces for a splittable Task; retain that baseline ref rather than comparing against a subsequently revised estimate. `actual_seconds` uses the corrected admitted execution intervals for the same attempt and duration basis. Exclude `waiting_for` and deferred time; missing or uncertain coverage or baseline suppresses the automatic prompt, while manual notes remain available.\n+\n+Deduplicate by the completion Log ref, persist offered/skipped state across review and restart, and coalesce it on sync; concurrent offline Devices may display before learning another Device's suppression. Skipping or disabling automatic friction prompts never delays completion or Plan repair. A skipped presented overrun alert uses \u00a717.0's `decision_recorded` with `decision_kind: alert_dismissed_or_ignored`, optional reasons and `preference_inference_allowed: false`; it creates no friction note. Failure and deviation notes are available manually without an automatic overrun prompt.\n+\n+**Review and learning boundary.** The Log review report shows the Task/outcome ref, original planned baseline and actual duration with their evidence/coverage, absolute and relative difference when comparable, current attributed notes and correction status, and permitted linked decisions. Offer add/correct note, inspect evidence/decision, or propose a separate Task-estimate/model correction; skipping requires no explanation. Notes, refs, reports and derived model inputs obey current source/Compartment disclosure and retention policy; denied context is never copied into a note or summary.\n+\n+Phase 2 captures explanatory review evidence; Bayesian updating remains post-MVP under `UBU-Q0116`. A note is neither a measurement correction nor a duration override: never infer minutes to subtract from text, automatically discard/down-weight an observation, or change distribution parameters merely because friction is present. Numeric errors require separately admitted corrections to duration evidence. Future learning consumes the corrected outcome/duration evidence with its linked current notes and provenance, counting each completion once regardless of annotation count. Failed/partial attempts and deviations are not completed-duration draws; any censored-data, grouping, covariate or outlier policy remains for `UBU-Q0116`. Missing notes establish no cause or normality. Learned `(min_seconds, mode_seconds, p95_seconds)` suggestions require explicit user review and ordinary validated admission before affecting Plans; the kernel continues to receive only admitted duration models.\n+\n+**User choices.** When an override or rejected recommendation contributed to friction, link its existing `decision_recorded` entry through `decision_refs`. Record any qualifying unlogged choice under \u00a717.0, preserving optional reasons and their source; a friction note cannot replace the counterfactual record or convert inferred reasons into stated ones or canonical Preferences. A user decision alone establishes neither blame nor estimation error.\n+\n+Before enabling this feature, require fixtures for optional/schema-bounded notes and supported targets; immutable additions/corrections and replay without duplicate duration observations; exact threshold boundaries, missing baselines/coverage, excluded waiting/deferred time and durable Skip suppression; failure/deviation handling, model-origin provenance, linked counterfactual reasons and denied context; and review-required estimate changes. These are acceptance requirements, not passing implementation evidence.\n+\n ### 17.9 Authority source metadata\n \n `authority_source` is a coarse closed enum recording only the authority path for an accepted object, projection state, or candidate mutation (`UBU-D0185` as amended by `UBU-D0226`). It does not replace actor Identity, capability grants, External References, expected-prior-version checks, review policy, or Log provenance.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -1715,28 +1715,28 @@ Resolved by `UBU-D0307` (DESIGN.md \u00a79.6): Phase 2 adds canonical `waiting_for`\n \n ## UBU-Q0113: Friction annotation on Log entries\n \n-Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0107 Blocks: duration model improvement, introspection quality Resolved by: None Last scored: 2026-10-14 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0107 Blocks: duration model improvement, introspection quality Resolved by: UBU-D0357 Last scored: 2026-10-14 Scored from commit: None\n \n ### Question\n \n A lightweight optional friction annotation on Log entries \u2014 \"took longer because the API was down,\" \"interrupted three times,\" \"underestimated complexity\" \u2014 would feed the duration model review and the introspection cycle without requiring a separate system. What is the minimum friction annotation schema on Log entries, and how does it interact with the Bayesian duration learning system?\n \n ### Subquestions\n \n 1. Is friction annotation a free-text field, a structured enum, or both?\n 2. Which Log entry types support friction annotation: Task completion, Task failure, plan deviation, others?\n 3. How do friction annotations feed the Bayesian duration model \u2014 are they a correction signal, a distribution adjustment trigger, or a manual override?\n 4. How are friction annotations surfaced during the Log review introspection cycle?\n 5. Should friction annotation be prompted by the system when actual duration significantly exceeds planned duration?\n 6. How does friction annotation interact with the counterfactual log (Q0107) when the friction was a user decision rather than an external factor?\n \n ### Current direction\n \n Friction annotation on Log entries is a Phase 2 required feature. It should be an optional structured field on Task completion Log entries, prompted by the system when actual duration deviates significantly from planned duration. Free-text is acceptable for Phase 2 MVP; structured taxonomy is post-MVP.\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0357` (DESIGN.md \u00a717.8.1): Phase 2 uses optional free-text friction objects on completed/failed Task outcomes and Task-specific `plan_realized` deviations, with attributed append-only additions/corrections and optional counterfactual decision links. Comparable overruns prompt once without blocking execution. Review preserves duration evidence; notes never directly override measurements or distributions, and learned estimates require user approval. Bayesian mathematics remains `UBU-Q0116`; implementation conformance is still required.\n \n ---\n \n",
    "commit_message": "docs: resolve UBU-Q0113 friction annotations and duration review boundary",
    "validation_notes": [
      "Read unmodified current working-tree files and generated the patch using git diff --no-index with proposed contents supplied on stdin; normalized only diff file labels and omitted index headers.",
      "git apply --check - passed against /home/sean/ubu-design working-tree files; the index was not used as the baseline.",
      "Verified only the three allowlisted documentation files differ, UBU-D0357 is unique, and only UBU-Q0113 metadata/resolution changes in OPEN_QUESTIONS.md; its scoring metadata and all other question blocks, including open UBU-Q0116, are preserved.",
      "Repository files were not modified. Schema, prompt and learning acceptance fixtures are specified requirements; no implementation tests were run.",
      "Validated required JSON fields, types, identifier patterns and file allowlist; the question hunk includes UBU-Q0113's heading and its own Resolution section."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0113"
    ],
    "decisions_added": [
      "UBU-D0357"
    ],
    "requires_human_review": false
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0113-friction-annotation-v1",
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
