# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `codex`

## Selected question

Question ID: `UBU-Q0051`  
Question title: `Minimum preference-calibration examples for Phase 1 onboarding and review`  
Base commit: `d8f4ea943adb6bb99f9ecdbc8dcb0fb198c5290c`

```markdown
## UBU-Q0051: Minimum preference-calibration examples for Phase 1 onboarding and review

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0050 Blocks: Phase 1 onboarding quality, Preference calibration, Calendar preview, Log review Resolved by: None Last scored: 2026-05-27 Scored from commit: None

### Question

What is the minimum useful set of preference-calibration examples for Phase 1 onboarding, Calendar preview, and Log review?

### Subquestions

1. Which common situations should UbU use to emotionally ground early Preference judgments?
2. How many examples are enough for Phase 1 without making onboarding burdensome?
3. Which answers become Preferences, Snapshots, Logs, Objective annotations, or noncanonical review notes?
4. How should examples avoid leading the user toward an assumed value model?
5. How should calibration examples distinguish urgent value, emotional cost, social pressure, recovery value, and long-term importance?
6. How should UbU revise or retire examples that repeatedly fail to help the user make accurate judgments?

### Current direction

Preference calibration is MVP important but not an MVP blocker. UbU should use examples of common emotional costs, rewards, and tradeoffs to help users make better Preference statements. Examples are grounding aids, not canonical Preferences, unless the user accepts or edits them into canonical entries.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "codex-ubu-q0051-d0200",
    "provider_id": "codex",
    "model_name": "gpt-5-codex",
    "question_id": "UBU-Q0051",
    "base_commit": "d8f4ea943adb6bb99f9ecdbc8dcb0fb198c5290c",
    "summary": "Resolve UBU-Q0051 by accepting a small Phase 1 preference-calibration library: six neutral example frames, strict presentation budgets, canonical admission rules, neutrality requirements, and versioned revision/retirement semantics.",
    "rationale": "The selected question is answerable because UBU-Q0050 and UBU-D0181 already define the bootstrap, Calendar preview, and Log review surfaces. The proposal keeps calibration lightweight and noncanonical by default, while giving implementers enough fixed examples and admission rules to build Phase 1 onboarding and review prompts without adding a broader psychology ontology or hidden value model.",
    "changed_files": [
      "DESIGN.md",
      "DECISIONS.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -1254,20 +1254,56 @@ Util derivation is typically local to the Objective subset being compared in a s\n ### 8.6 Preference calibration examples\n \n A **PreferenceCalibrationExample** is a user-facing prompt or scenario used to help the user make better Preference judgments. It may describe a common emotional cost, common emotional reward, motivation ambiguity, social-pressure pattern, or tradeoff.\n \n Preference calibration examples are MVP-important onboarding and review aids. They are not canonical value objects unless the user accepts or edits the resulting judgment into a Preference or other canonical entry.\n \n+The minimum Phase 1 calibration library has six example frames:\n+\n+- `urgent_vs_important`: urgent external commitment versus important but nonurgent Objective;\n+- `emotional_cost_vs_visible_progress`: emotionally costly repair, clarification, or maintenance work versus easier visible progress;\n+- `social_pressure_vs_planned_objective`: socially pressured request versus planned user-chosen Objective;\n+- `recovery_vs_more_work`: recovery or rest versus another useful Task;\n+- `short_term_relief_vs_long_term_importance`: short-term relief or cleanup versus long-term capability, relationship, or project importance;\n+- `ambiguity_vs_decomposition`: decomposition of ambiguous work versus starting a larger unclear Task.\n+\n+Each example identifies one or more calibration dimensions: `urgent_value`, `emotional_cost`, `social_pressure`, `recovery_value`, and `long_term_importance`.\n+\n+Default presentation budget:\n+\n+- Bootstrap interview: at most two examples after the user has named real Objectives or candidate Tasks.\n+- Calendar preview: at most one relevant example when the default Plan appears value-misaligned, socially pressured, recovery-fragile, or unusually urgent.\n+- Log review: at most one relevant example for an unreconciled rejection, override, failure, or repeated deviation unless the user asks for more.\n+\n+Answers become canonical only through normal admission:\n+\n+- explicit accepted pairwise Objective comparisons become Preferences;\n+- current affect, availability, capacity, or recovery reports become Snapshots or UniverseState facts when they describe observed current state;\n+- preview/review answers that explain outcomes, rejections, overrides, or calibration interactions become Logs, Log annotations, or Log corrections;\n+- comments about Objective importance or fit that do not declare pairwise order become Objective annotations or review notes;\n+- raw feelings, social-pressure comments, skipped examples, and unaccepted hypotheses remain noncanonical review notes.\n+\n+Calibration wording must avoid leading the user:\n+\n+- use the user's actual Objectives or Tasks when possible;\n+- present plausible costs and rewards for both sides;\n+- avoid preselected answers, hidden scoring, therapy, diagnosis, moral judgment, and \"should\" language;\n+- offer `neither`, `indifferent`, `not applicable`, edit, and free-text paths;\n+- disclose that examples are prompts for self-reporting, not UbU's values.\n+\n Candidate fields for a calibration example include:\n \n - `example_id`\n+- `version`\n - `situation_summary`\n+- `calibration_dimensions`\n - `common_emotional_costs`\n - `common_emotional_rewards`\n - `possible_tradeoffs`\n - `calibration_question`\n - `resulting_object_refs`\n - `source`\n+- `revision_status`\n \n-Open details are tracked in `UBU-Q0051`.\n+Example definitions are versioned derived content. UbU may revise or retire examples when user corrections, skips, edits, repeated non-use, or review feedback show that an example is confusing or leading. Historical Logs are not rewritten.\n \n ---\ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -4190,3 +4190,54 @@ The `UBU-D0175` stop rule applies to replacement questions. A replacement questi\n - Future parser work may add first-class decomposed-status and lineage fields without changing the scoring rule.\n \n ---\n+\n+## UBU-D0200: Phase 1 preference calibration uses six neutral example frames\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a7\u00a72.2.1, 4.1.2, 8.6\n+\n+Resolved question: `UBU-Q0051`.\n+\n+Phase 1 preference calibration is a small prompt library, not a new canonical value object or hidden utility model. The minimum library has six neutral example frames:\n+\n+- urgent external commitment versus important but nonurgent Objective;\n+- emotionally costly repair, clarification, or maintenance work versus easier visible progress;\n+- socially pressured request versus planned user-chosen Objective;\n+- recovery or rest versus another useful Task;\n+- short-term relief or cleanup versus long-term capability, relationship, or project importance;\n+- decomposition of ambiguous work versus starting a larger unclear Task.\n+\n+Presentation budget:\n+\n+- Bootstrap shows at most two examples by default, after the user has named real Objectives or candidate Tasks.\n+- Calendar preview shows at most one relevant example for the Plan being previewed.\n+- Log review shows at most one relevant example for an unreconciled rejection, override, failure, or repeated deviation unless the user asks for more.\n+- Skipping calibration is allowed; UbU records only the ordinary note or decision evidence needed for review.\n+\n+Each example is tagged with one or more Phase 1 calibration dimensions: `urgent_value`, `emotional_cost`, `social_pressure`, `recovery_value`, and `long_term_importance`. These tags explain the tradeoff being surfaced; they are not utility components.\n+\n+Admission rules:\n+\n+- accepted explicit pairwise Objective comparisons become Preferences;\n+- current affect, availability, capacity, or recovery reports become Snapshots or UniverseState facts when they describe observed current state;\n+- preview/review explanations for outcomes, rejections, overrides, or calibration interactions become Logs, Log annotations, or Log corrections;\n+- Objective-importance comments without pairwise order become Objective annotations or noncanonical review notes;\n+- raw feelings, social-pressure comments, skipped examples, and unaccepted hypotheses remain noncanonical review notes.\n+\n+Neutrality rules:\n+\n+- use actual user Objectives or Tasks when possible;\n+- present plausible costs and rewards for both sides;\n+- provide `neither`, `indifferent`, `not applicable`, edit, and free-text paths;\n+- avoid preselected answers, hidden scoring, therapy, diagnosis, moral judgment, and \"should\" language;\n+- disclose that examples are prompts for self-reporting, not UbU's values.\n+\n+Examples are versioned derived content. UbU may revise or retire an example when corrections, skips, edits, repeated non-use, or review feedback show that it is confusing, leading, or unhelpful. Historical Logs stay append-only; only future example presentation changes.\n+\n+**Consequences:**\n+\n+- `UBU-Q0051` is resolved for Phase 1.\n+- Preference calibration can improve onboarding, Calendar preview, and Log review without adding a psychology ontology or canonical value objects.\n+- Phase 1 onboarding remains lightweight because calibration has a strict presentation budget and can be skipped.\n+- Future richer examples may be added as versioned prompt content without schema migration unless they change admission rules or canonical fields.\n+\n+---\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -630,26 +630,4 @@ Resolved. See UBU-D0129.\n ## UBU-Q0051: Minimum preference-calibration examples for Phase 1 onboarding and review\n-\n-Status: Open Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0050 Blocks: Phase 1 onboarding quality, Preference calibration, Calendar preview, Log review Resolved by: None Last scored: 2026-05-27 Scored from commit: None\n-\n-### Question\n-\n-What is the minimum useful set of preference-calibration examples for Phase 1 onboarding, Calendar preview, and Log review?\n-\n-### Subquestions\n-\n-1. Which common situations should UbU use to emotionally ground early Preference judgments?\n-2. How many examples are enough for Phase 1 without making onboarding burdensome?\n-3. Which answers become Preferences, Snapshots, Logs, Objective annotations, or noncanonical review notes?\n-4. How should examples avoid leading the user toward an assumed value model?\n-5. How should calibration examples distinguish urgent value, emotional cost, social pressure, recovery value, and long-term importance?\n-6. How should UbU revise or retire examples that repeatedly fail to help the user make accurate judgments?\n-\n-### Current direction\n-\n-Preference calibration is MVP important but not an MVP blocker. UbU should use examples of common emotional costs, rewards, and tradeoffs to help users make better Preference statements. Examples are grounding aids, not canonical Preferences, unless the user accepts or edits them into canonical entries.\n-\n-### Resolution\n-\n-Open.\n-\n+Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0050 Blocks: Phase 1 onboarding quality, Preference calibration, Calendar preview, Log review Resolved by: UBU-D0200 Last scored: 2026-05-27 Scored from commit: None\n+Resolved. See UBU-D0200.\n ---\n",
    "commit_message": "Resolve Phase 1 preference calibration examples",
    "validation_notes": [
      "Patch validated with git apply --check against base commit d8f4ea943adb6bb99f9ecdbc8dcb0fb198c5290c.",
      "OPEN_QUESTIONS.md converts UBU-Q0051 to a compact tombstone with Resolved by: UBU-D0200.",
      "No files outside DESIGN.md, DECISIONS.md, and OPEN_QUESTIONS.md are modified.",
      "requires_human_review is true because UBU-Q0051 is classified as Human approval required."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0051"
    ],
    "decisions_added": [
      "UBU-D0200"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "codex-ubu-q0051-d0200",
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
