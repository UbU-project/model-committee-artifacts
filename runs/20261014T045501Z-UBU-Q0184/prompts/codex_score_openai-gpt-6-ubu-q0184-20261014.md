# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0184`  
Question title: `Missing-prerequisite candidate feasibility`  
Base commit: `001ab5c8ee3aa246a64595d3a735ef6d9c712642`

```markdown
## UBU-Q0184: Missing-prerequisite candidate feasibility

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-14 Scored from commit: None

### Question

Does a candidate that omits a prerequisite report infeasible, or incomplete?

### Current direction

P1B-79 records, but does not resolve, this separate boundary. Rust reports `dependency_feasibility = false` when a prerequisite is absent. Python directly indexes prerequisite placements when computing dependency margins; an absent prerequisite raises `KeyError`, rather than being skipped or reporting true. Its placement path also directly indexes prerequisites. Neither computation changes in P1B-79. No live run has exercised a candidate that retains a dependent while omitting its prerequisite. Stage 1's hard-constraint plan-validity conjunct already requires dependency presence; the distinct dependency field's missing-prerequisite meaning still needs a semantic decision (`UBU-D0171`, `UBU-D0301`).

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "openai-gpt-6-UBU-Q0184-20261014",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0184",
    "base_commit": "001ab5c8ee3aa246a64595d3a735ef6d9c712642",
    "summary": "Define missing prerequisites as candidate infeasibility: both Stage 1 feasibility fields are false, with a normal rejection reason.",
    "rationale": "A request dependency is an explicit requirement for a placed dependent. Omitting its prerequisite violates candidate closure even when timing cannot be computed. Boolean false follows the existing hard-constraint validity rule without treating Rust or Python behavior as authority. Safe rejection preserves other candidates and legitimate optional-work deferral; the semantic decision requires human approval.",
    "changed_files": [
      "DECISIONS.md",
      "OPEN_QUESTIONS.md",
      "PLANNING_KERNEL_CONTRACT.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\nindex e5f483c..e7fc5c3 100644\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -2610,7 +2610,7 @@ The design contract specifies the semantic data each stage consumes and produces\n \n Stage 1 consumes a CPU-provided `topological_order`; the GPU engine does not discover graph order. Stage 4 uses deterministic rollout seed derivation from the request seed. The Phase 1 default rollout budget is `n_rollouts = 1000` per finalist unless overridden by the CPU kernel's compute budget.\n \n-Stage 1 produces both `dependency_feasibility` and `hard_constraint_feasibility`. The former reports prerequisite completion at or before dependent starts where prerequisites are present; missing-prerequisite semantics remain unanswered in `UBU-Q0184`. The latter is the conjunction of plan validity (non-empty, no duplicate Task, `start < end`, every dependency present and earlier in declared placement order), dependency feasibility by time, plan/Task-window containment and exact Static-anchor starts, and pairwise placement disjointness. Overlap uses `start < other.end && end > other.start`; touching endpoints are allowed. Stage 2's `feasible_mask` is a different field. This enumeration records the P1B-79 definition before either implementation changes, without widening exact comparison.\n+Stage 1 produces both `dependency_feasibility` and `hard_constraint_feasibility`. The former requires every prerequisite of each placed dependent to be present and complete at or before the dependent starts; a missing prerequisite makes it false (`UBU-D0340`, resolving `UBU-Q0184`). The latter is the conjunction of plan validity (non-empty, no duplicate Task, `start < end`, every dependency present and earlier in declared placement order), dependency feasibility by time, plan/Task-window containment and exact Static-anchor starts, and pairwise placement disjointness. Overlap uses `start < other.end && end > other.start`; touching endpoints are allowed. Stage 2's `feasible_mask` is a different field. The hard-constraint enumeration records the P1B-79 definition before either implementation changes, without widening exact comparison.\n \n `PLANNING_KERNEL_CONTRACT.md` includes a Phase 1 recommended tensor profile using implementation-facing names such as `start_time_offsets`, `validity_mask`, `feasible_mask`, `surviving_indices`, `feasibility_scores`, `composite_scores`, `top_k_indices`, and probability intervals. These names guide implementation and tests without turning `ubu-design` into the PyTorch source tree.\n \n@@ -5342,3 +5342,15 @@ Require existing revision-pinned host fidelity/admission, access/usefulness, num\n Develop alongside Phase 2 on non-primary test data. Cross-Device routes and candidate exchange use existing trust, Compartment and sync gates, preserving fresh local consent and review; the existing three-Device sync freeze requirement does not require AV on every Device. Release integration/acceptance follows Phase 2 completion and the deferred switch. Missing capture evidence holds the MVP release, without adding a switch condition or reopening Phase 1b. Phone capture allocates no broader mobile planning workload or compute API; `UBU-Q0160` retains any stewardship amendment.\n \n Phone streaming, meeting/multi-party logging, retrospective or cloud-assisted interaction, other sensitive sources and restricted-attention interaction beyond immediate local controls may follow as separately qualified slices. Always-on consent is never inferred, and Phase 3 supplies no capture release gate. Publish exact supported Device/backend/mode combinations, limitations and fallbacks. This resolves scope allocation and sequence only; implementation and operator-reviewed release acceptance remain outstanding.\n+\n+---\n+\n+## UBU-D0340: Missing prerequisites make evaluated candidates infeasible\n+\n+**Status:** Accepted \u2192 PLANNING_KERNEL_CONTRACT.md \u00a75. Resolves `UBU-Q0184`; refines `UBU-D0171` and applies `UBU-D0301`.\n+\n+An evaluated Stage 1 candidate retaining a dependent Task while omitting any prerequisite required by the request reports `dependency_feasibility = false` and `hard_constraint_feasibility = false`, with a missing-prerequisite rejection reason. This is an infeasible candidate, not an incomplete feasibility result. Dependency feasibility remains boolean.\n+\n+Dependency feasibility requires presence and timely completion for every request dependency whose dependent is placed. With no such dependencies it is true, while the other hard constraints still apply. This follows the existing plan-validity requirement for dependency presence rather than treating either implementation's behavior as authority. Optional-work omission still defers affected dependents under `UBU-D0289`; an absent placement cannot establish prerequisite completion.\n+\n+Detect absence before placement or dependency-slack lookups, identify the reason in Stage 1's existing per-candidate rejection/failure output, and preserve evaluation of other candidates. Require shared CPU/GPU goldens covering absence, multiple prerequisites, timing equality and lateness, dependency-free work, permitted omission of an entire optional chain and mixed valid/invalid batches. This resolves the field semantics; implementation conformance and live parity evidence remain outstanding.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\nindex bdcd168..f85d824 100644\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3507,21 +3507,21 @@ Resolved by `UBU-D0339`: DESIGN.md \u00a74.2 specifies the ordered mandatory mode/de\n ---\n \n ## UBU-Q0184: Missing-prerequisite candidate feasibility\n \n-Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-14 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: UBU-D0340 Last scored: 2026-10-14 Scored from commit: None\n \n ### Question\n \n Does a candidate that omits a prerequisite report infeasible, or incomplete?\n \n ### Current direction\n \n-P1B-79 records, but does not resolve, this separate boundary. Rust reports `dependency_feasibility = false` when a prerequisite is absent. Python directly indexes prerequisite placements when computing dependency margins; an absent prerequisite raises `KeyError`, rather than being skipped or reporting true. Its placement path also directly indexes prerequisites. Neither computation changes in P1B-79. No live run has exercised a candidate that retains a dependent while omitting its prerequisite. Stage 1's hard-constraint plan-validity conjunct already requires dependency presence; the distinct dependency field's missing-prerequisite meaning still needs a semantic decision (`UBU-D0171`, `UBU-D0301`).\n+P1B-79 records, but does not resolve, this separate boundary. Rust reports `dependency_feasibility = false` when a prerequisite is absent. Python directly indexes prerequisite placements when computing dependency margins; an absent prerequisite raises `KeyError`, rather than being skipped or reporting true. Its placement path also directly indexes prerequisites. Neither computation changes in P1B-79. No live run has exercised a candidate that retains a dependent while omitting its prerequisite. Stage 1's hard-constraint plan-validity conjunct already requires dependency presence; `UBU-D0340` settles the distinct dependency field's missing-prerequisite meaning as `false` (`UBU-D0171`, `UBU-D0301`).\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0340`: an evaluated candidate retaining a dependent while omitting any required prerequisite is infeasible, with both `dependency_feasibility` and `hard_constraint_feasibility` set to `false` and a missing-prerequisite rejection reason. PLANNING_KERNEL_CONTRACT.md \u00a75 requires safe missing-placement handling and exact-parity fixtures. This settles candidate semantics; implementation conformance and live evidence remain outstanding.\n \n ---\n \n ## UBU-Q0185: Advisory decode controls or operator-owned runtime policy\ndiff --git a/PLANNING_KERNEL_CONTRACT.md b/PLANNING_KERNEL_CONTRACT.md\nindex 000f879..d0d439b 100644\n--- a/PLANNING_KERNEL_CONTRACT.md\n+++ b/PLANNING_KERNEL_CONTRACT.md\n@@ -406,7 +406,9 @@ Produces:\n - `hard_constraint_feasibility`;\n - rejection/failure code per invalid candidate.\n \n-`dependency_feasibility` reports whether each prerequisite finishes at or before its dependent placement starts for candidates whose prerequisites are present. The meaning of a candidate missing a prerequisite remains open in `UBU-Q0184`; this definition does not settle that case.\n+`dependency_feasibility` is a boolean over request dependency edges whose dependent Task is placed in the candidate. It is `true` exactly when every such prerequisite is present in the candidate and finishes at or before its dependent placement starts. If no such edges exist, it is `true`; the other hard-constraint checks still apply. A candidate retaining a dependent while omitting any required prerequisite reports `dependency_feasibility = false` and `hard_constraint_feasibility = false` and is rejected as infeasible, not assigned an incomplete feasibility result (`UBU-D0340`).\n+\n+Detect missing prerequisites before placement or dependency-slack lookups. Record the missing-prerequisite reason in Stage 1's existing per-candidate rejection/failure output; do not fabricate prerequisite completion or numeric slack, or abort otherwise evaluable candidates with a lookup error. Unfinished internal construction cannot be certified as a feasible Stage 1 output. Permitted optional-work omission defers affected dependents rather than retaining them without prerequisites (`UBU-D0289`).\n \n `hard_constraint_feasibility` is the conjunction of:\n \n@@ -415,6 +417,8 @@ Produces:\n - containment: every placement inside the plan window, inside its Task window when declared, and starting exactly at its Static anchor when declared;\n - pairwise disjointness: no two placements overlap in time. Overlap means `start < other.end && end > other.start`; touching endpoints are allowed.\n \n+Shared CPU/GPU goldens must cover a retained dependent with an absent prerequisite (both feasibility fields `false`, with a missing-prerequisite rejection reason), one of multiple prerequisites absent, prerequisite completion exactly at dependent start (dependency feasibility `true`), prerequisite completion after dependent start (dependency feasibility `false`), dependency-free work, and valid independent work with an entire optional prerequisite/dependent chain omitted (dependency feasibility `true`). Compare both boolean fields and rejection classes exactly, and include a mixed valid/invalid batch to verify that a missing prerequisite does not abort other candidates.\n+\n `feasible_mask` is the Stage 2 name, not Stage 1's `hard_constraint_feasibility`. A field named in an exact-parity clause must be enumerated at the stage that produces it (`UBU-D0301`).\n \n ### Stage 2: `affect_legitimacy_filter`\n",
    "commit_message": "docs(planning): define missing-prerequisite candidate infeasibility",
    "validation_notes": [
      "Generated the diff from current working-tree contents and proposed contents using git diff; repository paths and blob identifiers were normalized.",
      "git apply --check --whitespace=error passed against the current working tree without --cached or --recount.",
      "Verified that only UBU-Q0184 changes in OPEN_QUESTIONS.md, with its heading and Resolution in one hunk; all metadata except Status and Resolved by is preserved.",
      "Verified the three changed paths are allowlisted and repository files remain unmodified.",
      "Parity fixtures are required by the proposal, not executed here; implementation conformance and live evidence remain outstanding."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0184"
    ],
    "decisions_added": [
      "UBU-D0340"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "openai-gpt-6-UBU-Q0184-20261014",
    "patch_applies": true,
    "allowlist_passed": true,
    "changed_files": [
      "DECISIONS.md",
      "OPEN_QUESTIONS.md",
      "PLANNING_KERNEL_CONTRACT.md"
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
