# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0161`  
Question title: `Task descriptions as lossless prose after Clarify`  
Base commit: `4b0ceaa788cff06361786b685ace45b3f43b6091`

```markdown
## UBU-Q0161: Task descriptions as lossless prose after Clarify

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-11 Scored from commit: None

### Question

How should an additional local model round turn Clarify answers into readable Task-description prose while retaining all acquired knowledge for subsequent Clarify rounds?

### Current direction

The operator wants a lore field, not an accumulating Q&A transcript. Prose must preserve every answer, qualification and unresolved issue; it must not invent agreement. Decide how original answers, synthesis, corrections and later interview context are retained, reviewed and bounded. The retention mechanism and acceptance criterion remain open; this is not permission to replace existing descriptions lossy.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0161-openai-gpt-6-20261011",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0161",
    "base_commit": "4b0ceaa788cff06361786b685ace45b3f43b6091",
    "summary": "Adopt reviewed Task-description prose backed by exact Clarify source history, with complete coverage, linked corrections and bounded later-context retrieval.",
    "rationale": "Exact sources preserve acquired knowledge independently of model paraphrase. Bidirectional coverage and explicit source-by-source review make omissions and invented agreement reviewable, while version checks, finite budgets and inherited privacy rules prevent stale or silently lossy replacement.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5033,3 +5033,21 @@ Before enabling this optional extension, version and align the Task, sync-admiss\n - Competing/stale resolutions, resolution before Task creation, a failed completion update, Device revocation, and policy/visibility changes on partial replicas: independent resolution, blocking review, current authority, and protection of denied payload, source identity, and Compartment identity remain intact.\n \n These are future implementation gates, not completed evidence and not additional gates for the diagnostic-only Phase 2 release.\n+\n+---\n+\n+## UBU-D0317: Clarify produces reviewed Task lore backed by exact source history\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a71. Resolves `UBU-Q0161`; uses `UBU-D0274`'s candidate/admission boundary.\n+\n+Adopt readable Task-description prose backed by separate durable per-Task records of original lore, operator edits, exact Clarify questions/answers, necessary context and linked corrections. Save submitted answers through ordinary admission independently of the additional local synthesis job. Originals and admitted revision provenance remain queryable while the Task is retained, subject to current privacy and retention policy.\n+\n+Synthesis is an advisory candidate with a complete source manifest and bidirectional coverage. Every answer, qualification, correction and unresolved issue must survive in prose; superseded claims retain their temporal meaning. Structural validation supports explicit source-by-source operator review and cannot prove semantic losslessness. Version-bound approval admits only the description revision and provenance; failures preserve the prior description and answers. Later Clarify retrieves originals rather than treating the narrative as its sole memory. Finite budgets fail without truncation or silent eviction; policy-directed deletion also governs derivatives.\n+\n+Before enabling this workflow, demonstrate:\n+\n+- Existing lore and answers containing negation, quantities, conditions, uncertainty, contradictions and corrections survive synthesis and a later Clarify round; omitted qualifications and invented agreement fail acceptance.\n+- Model failure/rejection, operator prose edits and concurrent source/description changes preserve saved answers and require fresh coverage review before replacement.\n+- Long or oversized histories, exhausted storage and queryable archival respect finite limits without silent loss; restricted replicas and policy-directed purge cannot disclose or reconstruct denied source content through prose, coverage or cached context.\n+\n+These are required implementation checks, not evidence of completed implementation or a claim that arbitrary model paraphrase is provably lossless.\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -58,6 +58,18 @@ Generate visible text, counts, urgency, and reappearance only from evidence or d\n \n **Future schedulable manual review (`UBU-D0316`).** A later versioned extension may admit a shared Dynamic review Task with independent, coherent scheduling inputs and a protected conflict-episode identity. Its target is evidence to review, not admitted contested effects. Place it only in validated unaffected Calendar capacity; otherwise keep it unplaced. The diagnostic/prompt must still admit a permitted source-conflict `conflict_resolution` without review Task creation, placement, or completion. Conflicts in the Task, its scheduling/projection artifacts, or resolution workflow terminate at that diagnostic and never spawn review-of-review Tasks. Task lifecycle reconciliation follows resolution; snooze or deletion cannot clear the underlying blocker. Current authority and source privacy govern every step. The first Phase 2 sync release retains \u00a717's diagnostic-only model until this extension's versioned contracts and behavioral gates pass.\n \n+**Task lore after Clarify (`UBU-D0317`).** After a Clarify round, persist the operator's submitted answers through ordinary admission independently of synthesis, then offer one additional local synthesis job. The Task description is readable narrative. Keep the pre-existing description, subsequent operator edits, exact questions/answers and their necessary context as separate durable per-Task source records with stable ids, versions, actor/Device/session/round provenance, effective/recorded times, hashes and privacy/retention policy. Versioned context refs are sufficient only while the referenced context remains retrievable. Corrections append records with explicit supersession links; they never edit originals. Accepted model prose is a derived revision, not independent evidence. Raw audio/video retention is not required.\n+\n+The local model returns an `AdvisoryCandidate` containing proposed prose, model/template provenance, the complete source manifest/digest and a source-to-prose coverage map. Every answer, qualification, original lore statement, correction and unresolved issue must map to prose spans; every factual prose assertion must map back to sources. Equivalent repetitions may share wording. Superseded assertions retain a temporal account of the old and corrected meanings; disagreements, unknowns, declined answers and open questions remain explicit. Preserve attribution, negation, conditions, quantities and uncertainty. Unsupported inferences remain separate labelled suggestions, not synthesized lore.\n+\n+Review shows the description diff and a source-by-source checklist with exact source text beside its mapped prose. Deterministic checks require complete source/span coverage, valid references, finite bounds and current bindings; structural coverage and another model's opinion cannot establish semantic fidelity. Explicit operator approval confirms readable prose preserves all source meanings and invents no facts or agreement. Offer edit, regenerate or keep the prior description; edited prose needs renewed coverage review. Recheck authority, policy, Task-description/source/candidate versions and source digest before atomically admitting the description revision and its provenance. Concurrent changes invalidate approval and use ordinary conflict review. Failure, rejection or model unavailability preserves the prior description and saved answers. Lore approval does not admit typed planner constraints or authorize external action.\n+\n+Later Clarify uses admitted prose plus the source manifest, unresolved issues and correction links. Supply exact question/context/answer records for topics being revisited and make remaining permitted originals retrievable. Check the originals before treating a topic as settled; absence from a prompt is not absence of knowledge. Missing records make context explicitly incomplete. A partial Replica unable to establish the full source frontier may offer a scoped preview but cannot replace the Task description.\n+\n+Declare finite storage, prompt, result and job time/call limits. Retain source history and admitted revision/provenance while the Task is retained; archival must remain queryable. Chunk long inputs along complete source records and track coverage across chunks through the final prose. If complete reviewable prose cannot fit, stop and keep saved sources rather than truncate or drop knowledge; no cloud fallback. At storage exhaustion, pause Clarify and show any unsaved input; never claim it saved or silently evict history. Rejected candidate payloads follow the ordinary minimized retention lifecycle.\n+\n+Source records, synthesized prose, coverage maps and subsequent context inherit all source disclosure, routing and retention restrictions; local processing does not waive them. Reject synthesis into a Task whose destination policy cannot protect its sources. Recheck policy on every read, sync and export. Authorized redaction/purge also removes disallowed content from dependent prose, revisions and cached context; prevent reconstruction from retained derivatives. Keep only permitted minimized gap metadata, disclose incompleteness without revealing denied existence, and never claim lossless retention beyond that scope.\n+\n ---\n \n ## 2. Core Principles\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3018,19 +3018,19 @@ Open pending that measurement gate; no hardware result is inferred from API avai\n \n ## UBU-Q0161: Task descriptions as lossless prose after Clarify\n \n-Status: Open Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-11 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: UBU-D0317 Last scored: 2026-10-11 Scored from commit: None\n \n ### Question\n \n How should an additional local model round turn Clarify answers into readable Task-description prose while retaining all acquired knowledge for subsequent Clarify rounds?\n \n ### Current direction\n \n The operator wants a lore field, not an accumulating Q&A transcript. Prose must preserve every answer, qualification and unresolved issue; it must not invent agreement. Decide how original answers, synthesis, corrections and later interview context are retained, reviewed and bounded. The retention mechanism and acceptance criterion remain open; this is not permission to replace existing descriptions lossy.\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0317`: Task descriptions become readable, explicitly reviewed prose backed by exact per-Task source history. Original lore, answers, context and linked corrections remain queryable; a bidirectional coverage map and source-by-source operator review preserve every answer, qualification and unresolved issue without inventing agreement. Approval is bound to current source/description versions, and failures or exceeded limits keep prior prose and saved answers. Later Clarify retrieves originals; privacy/retention policy also governs derived prose and any completeness claims. This resolves the product contract; implementation and behavioral evidence remain outstanding.\n \n ---\n \n",
    "commit_message": "docs: define lossless Clarify Task lore and resolve UBU-Q0161",
    "validation_notes": [
      "Generated hunks with git diff --no-index against current working-tree text and in-memory proposed contents, normalizing headers to repository paths; no HEAD or index contents were used.",
      "git apply --check passed against /home/sean/ubu-design (exit 0). Repository files were left unmodified.",
      "Only three allowlisted files change. The selected question's single-line metadata and own Resolution are updated together; other metadata and neighboring questions are preserved.",
      "UBU-D0317 is unused in the working-tree decision ledger. The proposal requires human review.",
      "Documentation only: implementation checks are specified, not implemented or reported as passed."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0161"
    ],
    "decisions_added": [
      "UBU-D0317"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0161-openai-gpt-6-20261011",
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
