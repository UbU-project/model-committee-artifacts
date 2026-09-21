# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0153`  
Question title: `Task value and priority for Plan scoring`  
Base commit: `6db0bed1e0d46f1b148c10c1e2d17592bff39212`

```markdown
## UBU-Q0153: Task value and priority for Plan scoring

Status: Open Priority: MVP blocker Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0155, Phase 1b planner quality Resolved by: None Last scored: 2026-09-21 Scored from commit: None

Defining context: PLANNING_KERNEL_CONTRACT.md §5, DESIGN.md §16.10, `UBU-D0275`.

### Question

Where do Task value and priority come from for Plan scoring? Today the orchestrator builds every `TaskSpec` with `value: 1.0` and `priority: 1.0`, so the value-scoring stage cannot distinguish one Task from another.

### Subquestions

1. **Source.** Which input sets a Task's value: Objective value, explicit per-Task priority, deadlines, or Quick UbU's review-and-prioritize ranking?
2. **Scale.** How does an ordinal ranking become a bounded numeric value, and is it normalized per request? Utility is transient and never persisted. Which value inputs are canonical, and which are computed per request?
3. **Routine value.** How does a routine instance's value relate to a one-off Task's? Does it rise as a deadline or a missed occurrence approaches?
4. **Scoring policy.** How does Task value interact with the affect and robustness weights in `scoring_policy`?
5. **Revealed preference.** Which part, if any, of learning trade-off weights from review decisions belongs in Phase 1b? The rest stays with `UBU-Q0125`.

### Current direction

Phase 1b takes Task value from explicit operator prioritization: Quick UbU's prioritize flow is ported as admitted records, and the orchestrator maps them onto a bounded numeric scale when it builds each request. The computed values are never persisted as utility. Learning weights from revealed preference stays with `UBU-Q0125`.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "openai-gpt5-codex-UBU-Q0153-20260921",
    "provider_id": "openai",
    "model_name": "GPT-5 Codex",
    "question_id": "UBU-Q0153",
    "base_commit": "6db0bed1e0d46f1b148c10c1e2d17592bff39212",
    "summary": "Resolve UBU-Q0153 by making Phase 1b Task value derive from admitted explicit operator prioritization, normalized per PlanningRequest into transient bounded TaskSpec value/priority metadata, with revealed-preference learning deferred to UBU-Q0125.",
    "rationale": "The selected question is answerable from the current direction and Phase 1b planner boundary. This proposal keeps canonical state simple: persist ordinal priority declarations, compute bounded numeric values only at request-build time, treat routine instances as ordinary instantiated Tasks with inherited priority, and combine utility with robustness, affect margin, and diversity through scoring_policy without weakening affect or hard-constraint gates.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md",
      "PLANNING_KERNEL_CONTRACT.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -4321,7 +4321,29 @@\n Consequences:\n \n - `DESIGN.md` \u00a79.2 records `allowed_time_range` as the Phase 1b Dynamic Task input and distinguishes it from Static Task timing.\n - `PLANNING_KERNEL_CONTRACT.md` \u00a73 records the one-window `TaskSpec.window` mapping and hard infeasibility rule.\n - `OPEN_QUESTIONS.md` marks `UBU-Q0152` solved.\n \n ---\n+\n+## UBU-D0277: Phase 1b Task value is explicit priority normalized per planning request\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a716.10; PLANNING_KERNEL_CONTRACT.md \u00a75. Resolves `UBU-Q0153`.\n+\n+Phase 1b Task value for Plan scoring comes from admitted explicit operator prioritization, including the Quick UbU review-and-prioritize flow once it is ported as admitted records. The canonical persisted input is an ordinal Task priority declaration, not a persisted utility number. Objective labels, deadlines, due dates, routine cadence, and imported source urgency may affect eligibility, risk reporting, explanation, or future policy work, but they do not become canonical Task value in Phase 1b unless the operator admits them as explicit prioritization.\n+\n+When the CPU kernel builds a `PlanningRequest`, it maps admitted ordinal priority among the request's eligible schedulable Tasks onto bounded transient metadata. `TaskSpec.value` is a float in `[0.1, 1.0]`: the highest priority bucket maps to `1.0`, the lowest ranked bucket maps to `0.1`, ties share a bucket, a single ranked bucket maps to `1.0`, and unranked eligible Tasks use `0.1`. `TaskSpec.priority` carries the corresponding normalized priority rank for tie-breaks, diagnostics, and explanation. These computed numbers are request-local and are never persisted as utility.\n+\n+Routine instances are scored as ordinary Task instances after recurrence instantiation. They inherit explicit occurrence priority when present; otherwise they inherit the priority declared on the routine template or evergreen Objective that produced them. Missed occurrences, approaching local windows, or due dates may create review prompts, risk findings, or deadline/urgency diagnostics, but they do not automatically rewrite base value in Phase 1b.\n+\n+In Stage 3, utility is the value-weighted scheduled work signal for a candidate. It combines with approximate robustness, affect-margin, and schedule-diversity only through the request's `scoring_policy` weights. The Stage 2 affect filter and CPU hard-constraint certification remain gates: high value cannot legitimize affect-infeasible or hard-invalid Plans, and low value does not make a Task ineligible unless partial-placement selection later omits it.\n+\n+Phase 1b records review decisions, overrides, snoozes, rejections, and operator reprioritization as evidence for later learning, but it does not learn trade-off weights or automatically mutate priority from revealed preference. Adaptive weighting and preference learning remain with `UBU-Q0125`.\n+\n+Consequences:\n+\n+- `DESIGN.md` \u00a716.10 records that Stage 3 consumes CPU-computed transient value/priority metadata from admitted prioritization records.\n+- `PLANNING_KERNEL_CONTRACT.md` \u00a75 fixes the Phase 1b normalization and non-persistence rule for `TaskSpec.value` and `TaskSpec.priority`.\n+- `OPEN_QUESTIONS.md` marks `UBU-Q0153` solved.\n+\n+---\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -2480,9 +2480,11 @@\n 1. **Skeleton sampling** \u2014 parallel shifted-log-normal or fixed duration sampling across candidates with vectorized propagation over the CPU-provided topological order.\n \n 2. **`affect_legitimacy_filter`** \u2014 batch sigmoid affect-constraint evaluation across surviving candidates. This stage implements only the sigmoid affect-constraint portion of legitimization. Full legitimization in UbU design is broader: it makes a skeleton Plan human-viable by respecting affect, recovery, transition, rest, sustainability, and support constraints. The `affect_legitimacy_filter` GPU stage does not replace or subsume full legitimization.\n \n 3. **Value scoring** \u2014 parallel utility, approximate robustness, affect-margin, and schedule-diversity scoring across surviving candidates.\n+\n+Phase 1b value scoring uses CPU-computed, request-local Task value/priority metadata derived from admitted explicit prioritization records (`UBU-D0277`). Objective labels, deadlines, routine cadence, and revealed-preference evidence may inform review, risk, urgency, or later policy work, but the GPU stage receives only the bounded transient values supplied in the `PlanningRequest`.\n \n 4. **Monte Carlo rollout** \u2014 joint scenario simulation for finalists using correlation-group Gaussian copula sampling with deterministic rollout seed derivation.\n \n GPU search proposes candidates. Hard constraint certification and final Plan validity are performed by the CPU kernel using exact or conservative validation.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -2752,6 +2752,6 @@\n ## UBU-Q0153: Task value and priority for Plan scoring\n \n-Status: Open Priority: MVP blocker Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0155, Phase 1b planner quality Resolved by: None Last scored: 2026-09-21 Scored from commit: None\n+Status: Solved Priority: MVP blocker Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0155, Phase 1b planner quality Resolved by: UBU-D0277 Last scored: 2026-09-21 Scored from commit: None\n \n Defining context: PLANNING_KERNEL_CONTRACT.md \u00a75, DESIGN.md \u00a716.10, `UBU-D0275`.\n \n@@ -2770,9 +2770,9 @@\n ### Current direction\n \n Phase 1b takes Task value from explicit operator prioritization: Quick UbU's prioritize flow is ported as admitted records, and the orchestrator maps them onto a bounded numeric scale when it builds each request. The computed values are never persisted as utility. Learning weights from revealed preference stays with `UBU-Q0125`.\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0277`: Phase 1b uses admitted explicit operator prioritization as the canonical source of Task priority, including the ported Quick UbU review-and-prioritize flow. The CPU kernel converts ordinal priority buckets among the request's eligible schedulable Tasks into transient `TaskSpec.value` and `TaskSpec.priority` values in `[0.1, 1.0]`; ties share values, a single ranked bucket maps to `1.0`, and unranked eligible Tasks use `0.1`. Objective labels, deadlines, routine cadence, and missed occurrences may affect eligibility, risk, urgency diagnostics, review prompts, or later policy work, but they do not automatically become persisted utility. Routine instances inherit explicit occurrence priority when present, then routine-template or evergreen-Objective priority. Stage 3 combines value-weighted utility with robustness, affect-margin, and schedule-diversity only through `scoring_policy`; affect filtering and CPU hard-constraint certification remain gates. Phase 1b records review decisions as evidence but leaves revealed-preference learning and adaptive weights to `UBU-Q0125`.\n \n ---\ndiff --git a/PLANNING_KERNEL_CONTRACT.md b/PLANNING_KERNEL_CONTRACT.md\n--- a/PLANNING_KERNEL_CONTRACT.md\n+++ b/PLANNING_KERNEL_CONTRACT.md\n@@ -337,15 +337,19 @@\n Consumes:\n \n - surviving candidate schedules;\n - Task value/priority metadata;\n - scoring policy weights;\n - affect-margin outputs;\n - schedule-diversity policy.\n+\n+Phase 1b `TaskSpec.value` is a CPU-computed float in `[0.1, 1.0]` derived from admitted explicit Task prioritization for the request. Given dense priority buckets ordered best to worst, bucket position `p`, and bucket count `m`, value is `1.0` when `m = 1` and otherwise `1.0 - 0.9 * p / (m - 1)`. Tied Tasks share `p`; unranked eligible Tasks use `0.1`. `TaskSpec.priority` carries the corresponding normalized priority rank for tie-breaking, diagnostics, and explanation.\n+\n+The canonical persisted input is the admitted ordinal priority declaration. The bounded value and normalized priority are transient request metadata and must not be persisted as utility. Routine instances enter Stage 3 as ordinary TaskSpecs after recurrence instantiation: explicit occurrence priority wins, then routine-template or evergreen-Objective priority. Objective labels, deadlines, missed occurrences, and cadence do not rewrite value/priority unless admitted prioritization changes the underlying rank.\n \n Produces:\n \n - utility score;\n - approximate robustness score before finalist rollout;\n - affect-margin score;\n - schedule-diversity score;\n - `composite_scores`;\n",
    "commit_message": "Resolve UBU-Q0153 task value priority scoring",
    "validation_notes": [
      "Ran `git apply --check` successfully for the full proposed patch against the current workspace at base commit 6db0bed1e0d46f1b148c10c1e2d17592bff39212.",
      "Patch only touches the allowlisted design files and preserves the single-line OPEN_QUESTIONS.md metadata format.",
      "No repository files were modified while preparing this proposal."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0153"
    ],
    "decisions_added": [
      "UBU-D0277"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "openai-gpt5-codex-UBU-Q0153-20260921",
    "patch_applies": true,
    "allowlist_passed": true,
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md",
      "PLANNING_KERNEL_CONTRACT.md"
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
