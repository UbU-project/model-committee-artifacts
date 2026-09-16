# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0145`  
Question title: `Minimum useful worker Device protocol for Phase 2`  
Base commit: `ab795e369ff46df40fd3077a4b0dce210e818ce0`

```markdown
## UBU-Q0145: Minimum useful worker Device protocol for Phase 2

Status: Open Priority: MVP important Phase: Phase 1b Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0135, Phase 2 worker Device protocol Resolved by: None Last scored: 2026-09-16 Scored from commit: None

Formerly UBU-QSYNC-007 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §20.

### Question

What is the minimum useful worker Device protocol for Phase 2?

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "ubu-q0145-gpt5-codex-20260916",
    "provider_id": "openai",
    "model_name": "gpt-5-codex",
    "question_id": "UBU-Q0145",
    "base_commit": "ab795e369ff46df40fd3077a4b0dce210e818ce0",
    "summary": "Resolve UBU-Q0145 by defining the minimum Phase 2 worker Device protocol as scoped worker_request and worker_result sync statements, with workers limited to computation and candidate output while normal admission remains the only path to admitted state.",
    "rationale": "The proposal is intentionally narrow and matches the referenced \u00a720 boundary: user-owned workers may receive scoped context and produce candidate plans, simulations, summaries, projection previews, worker_result statements, and candidate mutations, but may not mutate state, bypass Compartment policy, retain restricted payloads, or expand authority. Encoding the protocol as ordinary sync statements keeps it compatible with the accepted Phase 2 causality, policy, conflict, deletion, and admission decisions without creating a second distributed authority path.",
    "changed_files": [
      "OPEN_QUESTIONS.md",
      "DECISIONS.md"
    ],
    "patch": "diff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -2586,7 +2586,7 @@\n \n ## UBU-Q0145: Minimum useful worker Device protocol for Phase 2\n \n-Status: Open Priority: MVP important Phase: Phase 1b Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0135, Phase 2 worker Device protocol Resolved by: None Last scored: 2026-09-16 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0135, Phase 2 worker Device protocol Resolved by: UBU-D0250 Last scored: 2026-09-16 Scored from commit: None\n \n Formerly UBU-QSYNC-007 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a720.\n \n@@ -2596,7 +2596,7 @@\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0250`: Phase 2 worker Devices use a scoped request/response protocol over normal sync statements. A controller sends a policy-checked `worker_request` with scope, purpose, allowed operations, input references or redacted context bundle, policy versions, retention deadline, and result schema. The worker may compute only within that scope and returns `worker_result` statements containing derived artifacts, candidate mutations, diagnostics, provenance, and deletion confirmation for transient payloads. Candidate mutations are never admitted state until normal sync-statement admission accepts them.\n \n ---\n \ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -3806,3 +3806,27 @@\n User-facing review may offer an explicit `user_accepted_unknown` outcome only after presenting the exposure as unknown rather than successful. This closes the immediate obligation for planning and audit purposes, but does not rewrite history as confirmed deletion.\n \n This resolves Phase 1b representation. Transport-specific retries, Device recovery flows, and full Phase 2 deletion propagation policy remain later implementation work.\n+\n+---\n+\n+## UBU-D0250: Minimum useful worker Device protocol is scoped request/result over sync\n+\n+**Status:** Accepted \u2192 DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a720. Resolves `UBU-Q0145`.\n+\n+For Phase 2, the minimum useful worker Device protocol is a policy-checked request/result exchange carried as sync statements, not an independent mutation channel. A controlling Device may issue a `worker_request` only after evaluating current Compartment policy and Device authority.\n+\n+A `worker_request` contains:\n+\n+- `request_id`, `controller_device_id`, and `target_worker_device_id` or equivalent opaque Device references;\n+- `zone_id`, allowed `compartment_ids`, redaction level, purpose, and policy-version references used to authorize the work;\n+- `allowed_operations`, limited in Phase 2 to candidate planning, simulation, local extraction, summarization, projection preview preparation, and candidate-mutation generation;\n+- input object references and optional scoped context-bundle digests, with payloads redacted or omitted when policy requires;\n+- retention deadline and required transient-payload deletion confirmation;\n+- expected `worker_result` schema, result size limits, and whether partial results are allowed;\n+- causal parents and observed versions needed for deterministic admission and stale-result detection.\n+\n+A worker Device must reject the request if its current policy view is missing, stale in a way that could expand access, revoked, or incompatible with the requested Compartment, export, retention, or `no_cloud_llm` constraints. The worker may not broaden the scope, fetch extra protected context on its own authority, retain transient payloads beyond the request, or directly mutate admitted state.\n+\n+A `worker_result` contains `request_id`, worker identity, the policy versions used, input digests or structural references, operation status, diagnostics safe at the request's visibility level, derived artifacts, projection previews, `worker_result` statements, candidate mutations, and transient-payload deletion confirmation. Candidate mutations are proposals only: they become admitted state solely through the normal sync-statement admission path, including conflict detection, policy checks, provenance, and user-review requirements.\n+\n+This is intentionally narrower than a general distributed worker marketplace protocol. It is sufficient to let user-owned Devices contribute compute while preserving the \u00a720 boundary that workers can prepare and propose but cannot expand authority or bypass admission.\n",
    "commit_message": "Resolve minimum Phase 2 worker Device protocol",
    "validation_notes": [
      "Checked base commit matches ab795e369ff46df40fd3077a4b0dce210e818ce0.",
      "Validated the proposed patch with git apply --check against the current workspace.",
      "Patch only modifies OPEN_QUESTIONS.md and DECISIONS.md, and preserves the single-line OPEN_QUESTIONS.md metadata format."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0145"
    ],
    "decisions_added": [
      "UBU-D0250"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "ubu-q0145-gpt5-codex-20260916",
    "patch_applies": true,
    "allowlist_passed": true,
    "changed_files": [
      "DECISIONS.md",
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
