# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai-codex`

## Selected question

Question ID: `UBU-Q0046`  
Question title: `Public dogfooding artifacts for contributor credibility`  
Base commit: `979db8d46c9f62291795f17fe4a2e4bb83c2df04`

```markdown
## UBU-Q0046: Public dogfooding artifacts for contributor credibility

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0036, UBU-Q0038 Blocks: Contributor recruitment Resolved by: UBU-D0077, UBU-D0086 Last scored: 2026-05-26 Scored from commit: None

### Question

What public dogfooding artifacts should `model-committee` expose to make UbU credible and actionable to potential contributors?

### Subquestions

1. Which run artifacts should be committed, linked, summarized, or ignored?
2. How should selected patches, review notes, and commit messages be presented?
3. How should public artifacts avoid leaking private reasoning or unsafe provider outputs?
4. What would convince a developer that the loop is real and reviewable?
5. How should artifacts connect to GitHub Issues or PRs?
6. How should failed or partially successful runs be shown without damaging credibility?

### Current direction

The project should show the dogfooding loop, not merely describe it. Artifacts should demonstrate that open questions become model-assisted proposals, reviewable patches, decisions, and updated canonical state. v0.2 review artifacts should also show cross-model score matrices, quorum outcomes, disagreement flags, and the operator-run publication command for copying the run to `../model-committee-artifacts`.

### Resolution

Partially resolved by `UBU-D0077`, `UBU-D0086`, and `UBU-D0150`; exact long-term artifact publication policy remains open, but v0.2 has a concrete operator-run publication path for model-committee run artifacts.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "codex-ubu-q0046-d0179-public-artifacts",
    "provider_id": "openai-codex",
    "model_name": "gpt-5-codex",
    "question_id": "UBU-Q0046",
    "base_commit": "979db8d46c9f62291795f17fe4a2e4bb83c2df04",
    "summary": "Adds UBU-D0179 to define the public model-committee dogfooding artifact policy, mirrors the policy in DESIGN.md, and tombstones UBU-Q0046 as solved.",
    "rationale": "UBU-Q0046 is answerable because its dependencies on run-log provenance and changeset-based work are solved. The proposal narrows publication to sanitized review packages that prove the loop is real while excluding secrets, private reasoning, unsafe raw outputs, and unnecessary raw provider logs. It also covers GitHub/PR linkage and public treatment of failed or human-review-required runs without expanding model-committee authority.",
    "changed_files": [
      "DESIGN.md",
      "DECISIONS.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -418,6 +418,23 @@\n Provider invocation records must include `invocation_id`, provider ID, provider class, model name or alias, phase, command/argv shape with secrets redacted, timeout, start and end timestamps, exit status, stdout/stderr artifact paths, raw output path, parsed output path, schema path or hash, validation result, and failure class when applicable. They must not store API keys, bearer tokens, credential files, private environment dumps, or unredacted secrets.\n \n v0.2 review artifacts should expose the score matrix, disagreement flags, quorum result, selected patch validation status, selected score, cross-score provenance, and the operator-run artifact-publication commands. The commands are part of the review artifact and must not be executed automatically.\n+\n+### 3.1.3 Public dogfooding artifact policy\n+\n+Public dogfooding artifacts should publish a sanitized review package, not the complete private run log by default. The public package is evidence that the loop is real and reviewable: canonical inputs led to provider proposals, validated patches, cross-scores, quorum or disagreement outcomes, human review, and a later canonical commit when accepted.\n+\n+Default public artifacts are:\n+\n+- `review.md`, including run ID, base commit, selected question, selected proposal summary, validation result, score matrix summary, quorum or disagreement result, failed-provider summary, and publication commands.\n+- `selected.patch` and `commit_message.txt` when a candidate exists.\n+- `manifest.public.json` or an equivalent redacted manifest containing hashes, provider/model IDs, artifact paths, scores, validation status, selected proposal ID, exit code, and links to the accepted commit, Issue, or PR when available.\n+- Score-matrix and validation summaries sufficient to reproduce the selection judgment without exposing full prompts or private reasoning.\n+\n+The complete local run directory may be archived outside the design repo. Public publication must exclude secrets, private environment dumps, credentials, private chain-of-thought, unsafe provider text, unredacted raw prompts when they contain sensitive context, and raw provider logs that are not needed for public review. Redacted raw artifacts may be published only when they help diagnose a run and pass the same artifact-safety check.\n+\n+Successful, failed, and human-review-required runs should all be publishable. Failed runs should be labeled by failure class, such as parse failure, invalid patch, no quorum, provider timeout, or critical disagreement, and should explain the next review action. This improves credibility when failures show bounded authority, preserved evidence, and no silent state mutation.\n+\n+GitHub connection is by reference, not automatic mutation. A public artifact should name the related open question, Issue, PR, accepted decision, selected patch, and final commit when available. `model-committee` may generate links and operator instructions, but opening PRs, pushing artifacts, and accepting canonical design changes remain human actions.\n \n ### 3.2 Changeset-based work phase\n \ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -3470,3 +3470,43 @@\n - Provider and model updates invalidate reusable derived scores without rewriting accepted canonical state or historical run logs.\n \n ---\n+\n+## UBU-D0179: Public dogfooding artifacts publish sanitized review packages\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a73.1.3\n+\n+Resolved question: `UBU-Q0046`.\n+\n+`model-committee` public dogfooding should publish sanitized review packages, not complete private run logs by default. The public package must show enough evidence for a contributor to verify the loop: selected question or problem, base commit, canonical input hashes, provider proposals, mechanical validation, score matrix or scoring summary, quorum or disagreement outcome, selected patch, suggested commit message, human-review status, and eventual canonical commit, Issue, or PR link when available.\n+\n+Default public artifacts:\n+\n+- `review.md`;\n+- `selected.patch` when a selected or reviewable candidate exists;\n+- `commit_message.txt` when available;\n+- `manifest.public.json` or an equivalent redacted manifest;\n+- score-matrix and validation summaries;\n+- links to the source question, related Issue or PR, accepted decision, and final commit when available.\n+\n+Not public by default:\n+\n+- raw provider logs, prompts, stderr, JSONL traces, and full manifests;\n+- private chain-of-thought or hidden reasoning;\n+- unsafe or policy-violating provider outputs;\n+- API keys, bearer tokens, credential paths, private environment dumps, or secrets;\n+- unredacted sensitive context or Compartment-protected payloads.\n+\n+Raw artifacts may be published only after explicit redaction and artifact-safety review. Hashes and summarized diagnostics should be preferred when the raw material is not needed for public review.\n+\n+Failed and partially successful runs are publishable when labeled by failure class and next review action. Acceptable public failure classes include parse failure, schema failure, invalid patch, no valid proposal, no quorum, selected score below threshold, critical disagreement, provider timeout, and artifact-safety block. A failed run should demonstrate bounded authority and preserved evidence; it must not imply accepted design state.\n+\n+`model-committee` may generate publication commands and GitHub/PR link placeholders, but it must not auto-publish artifacts, open PRs, push branches, mutate GitHub, apply patches, or mark canonical questions solved. Human review remains required to publish artifacts, accept patches, and connect a run to a final repository commit.\n+\n+**Consequences:**\n+\n+- `UBU-Q0046` is resolved.\n+- Public credibility comes from reviewable run packages and accepted commits, not from dumping every raw provider artifact.\n+- `UBU-Q0063` may depend on this policy for organizational-introspection dogfooding artifacts.\n+- Future implementation should add an artifact-safety check and a redacted public manifest schema before publishing run packages.\n+\n+---\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -788,30 +788,8 @@\n ---\n \n ## UBU-Q0046: Public dogfooding artifacts for contributor credibility\n-\n-Status: Open Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0036, UBU-Q0038 Blocks: Contributor recruitment Resolved by: UBU-D0077, UBU-D0086 Last scored: 2026-05-26 Scored from commit: None\n-\n-### Question\n-\n-What public dogfooding artifacts should `model-committee` expose to make UbU credible and actionable to potential contributors?\n-\n-### Subquestions\n-\n-1. Which run artifacts should be committed, linked, summarized, or ignored?\n-2. How should selected patches, review notes, and commit messages be presented?\n-3. How should public artifacts avoid leaking private reasoning or unsafe provider outputs?\n-4. What would convince a developer that the loop is real and reviewable?\n-5. How should artifacts connect to GitHub Issues or PRs?\n-6. How should failed or partially successful runs be shown without damaging credibility?\n-\n-### Current direction\n-\n-The project should show the dogfooding loop, not merely describe it. Artifacts should demonstrate that open questions become model-assisted proposals, reviewable patches, decisions, and updated canonical state. v0.2 review artifacts should also show cross-model score matrices, quorum outcomes, disagreement flags, and the operator-run publication command for copying the run to `../model-committee-artifacts`.\n-\n-### Resolution\n-\n-Partially resolved by `UBU-D0077`, `UBU-D0086`, and `UBU-D0150`; exact long-term artifact publication policy remains open, but v0.2 has a concrete operator-run publication path for model-committee run artifacts.\n-\n+Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0036, UBU-Q0038 Blocks: Contributor recruitment Resolved by: UBU-D0179 Last scored: 2026-05-26 Scored from commit: None\n+Resolved. See UBU-D0179.\n ---\n \n ## UBU-Q0047: Minimum committed-contributor onboarding path\n",
    "commit_message": "Resolve UBU-Q0046 public dogfooding artifact policy",
    "validation_notes": [
      "Patch generated against base commit 979db8d46c9f62291795f17fe4a2e4bb83c2df04.",
      "Changes are limited to DESIGN.md, DECISIONS.md, and OPEN_QUESTIONS.md.",
      "OPEN_QUESTIONS.md converts UBU-Q0046 to the required compact tombstone format.",
      "No new questions are introduced; UBU-D0179 fully resolves the selected question."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0046"
    ],
    "decisions_added": [
      "UBU-D0179"
    ],
    "requires_human_review": false
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "codex-ubu-q0046-d0179-public-artifacts",
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
