# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0140`  
Question title: `Phase 2 causality mechanism for sync ordering`  
Base commit: `3bfc8e22fc26bccbb616a65622e6853dfe39099e`

```markdown
## UBU-Q0140: Phase 2 causality mechanism for sync ordering

Status: Open Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0130, Phase 2 sync statement compatibility Resolved by: None Last scored: 2026-09-16 Scored from commit: None

Formerly UBU-QSYNC-002 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §8, §15.

### Question

What exact causality mechanism should Phase 2 use: hybrid logical clocks, Lamport clocks, vector
clocks, per-object version counters, content-addressed logs, or a hybrid?

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "ubu-q0140-hybrid-causality-v1",
    "provider_id": "openai",
    "model_name": "gpt-5-codex",
    "question_id": "UBU-Q0140",
    "base_commit": "3bfc8e22fc26bccbb616a65622e6853dfe39099e",
    "summary": "Resolve UBU-Q0140 by selecting a hybrid Phase 2 causality mechanism: content-addressed signed SyncStatements as the durable append-only record, per-origin hybrid logical clocks for compact tie breaking and progress, causal_parents as the authoritative statement DAG, and observed object/policy versions as deterministic conflict preconditions.",
    "rationale": "The hybrid choice fits the existing sync statement model without requiring a canonical server or two-device-only assumptions. HLCs provide compact ordering and review-friendly approximate time; the DAG preserves happened-before relationships across transports; content addresses provide integrity and deduplication for direct and indirect sync; observed versions keep object and policy conflicts explicit. Pure Lamport clocks lose useful time adjacency, vector clocks are too wide and privacy-leaky for partial/redacted replicas, per-object counters cannot cover cross-object and policy causality alone, and content-addressed bundles do not by themselves define ordering.",
    "changed_files": [
      "OPEN_QUESTIONS.md",
      "DECISIONS.md"
    ],
    "patch": "diff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -2494,16 +2494,18 @@ Open.\n ## UBU-Q0140: Phase 2 causality mechanism for sync ordering\n \n-Status: Open Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0130, Phase 2 sync statement compatibility Resolved by: None Last scored: 2026-09-16 Scored from commit: None\n+Status: Resolved Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0130, Phase 2 sync statement compatibility Resolved by: UBU-D0247 Last scored: 2026-09-16 Scored from commit: None\n \n Formerly UBU-QSYNC-002 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a78, \u00a715.\n \n ### Question\n \n What exact causality mechanism should Phase 2 use: hybrid logical clocks, Lamport clocks, vector\n clocks, per-object version counters, content-addressed logs, or a hybrid?\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0247`: Phase 2 uses a hybrid causality stack: content-addressed signed sync statements form the durable append-only log; every statement carries a per-origin hybrid logical clock tick for compact total-order tie breaking and queue progress; `causal_parents` preserve the statement DAG; `observed_versions` and `observed_policy_versions` are per-object/version preconditions used for deterministic conflict detection, not the sole clock.\n+\n+This rejects a pure Lamport clock because wall-time adjacency is useful for operator review, a pure vector clock because N-device and partial replicas make vectors too wide and leaky, and pure per-object counters because cross-object mutations and policy updates must retain causal edges.\n \n ---\ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -3709,3 +3709,31 @@ Consequences for this repository, which are the load-bearing part of this decisi\n 5. **Runs stay auditable.** `manifest.prompt_context` records the exact questions, decisions, and sections a prompt was built from, and `runs/<run-id>/snapshot/` still holds the full files. A reviewer can see both what the model was shown and what the repository contained.\n \n This does not change what is canonical. Accepted design state still exists only when a human operator commits to this repository.\n+\n+---\n+\n+## UBU-D0247: Phase 2 sync uses hybrid HLC, DAG, and content-addressed statements\n+\n+**Status:** Accepted \u2192 DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a78, \u00a715. Resolves `UBU-Q0140`.\n+\n+Phase 2 sync ordering uses a hybrid causality mechanism rather than selecting a single scalar clock.\n+\n+Canonical components:\n+\n+- The durable unit is the signed or integrity-protected `SyncStatement`; accepted statements are retained as an append-only, content-addressed log or bundle record.\n+- Each statement has a deterministic content address computed over its canonical signed payload, excluding replica-local metadata such as `received_time` and `admitted_time`.\n+- Each origin Device maintains a hybrid logical clock. The HLC tick is included in the statement payload and advances on local statement creation and on admitting remote causal parents.\n+- `causal_parents` define the statement DAG and are the authoritative happened-before evidence across Devices.\n+- `observed_versions` and `observed_policy_versions` are per-object preconditions used to detect stale writes, policy races, and deterministic conflicts; they are not a replacement for the statement DAG.\n+- Admitted-state application order is a deterministic topological traversal of available causal parents, with ties broken by HLC tick, origin Device ID, and sync statement ID.\n+\n+The HLC tick is an ordering aid and operator-review affordance, not an authority source. It never overrides missing causal parents, observed-version failures, policy failures, or invalid statement integrity. `effective_time`, `recorded_time`, `received_time`, and `admitted_time` retain their separate meanings and must not be collapsed into the causality clock.\n+\n+Rejected alternatives:\n+\n+- Pure Lamport clocks lose useful physical-time adjacency for review and diagnostics while still requiring deterministic tie breakers.\n+- Pure vector clocks are too large and privacy-leaky for N-device sync with partial, redacted, stale, or restricted replicas.\n+- Pure per-object counters cannot represent cross-object mutations, policy races, worker results, or conflict-resolution statements without an additional statement-level causal graph.\n+- Pure content-addressed bundles provide integrity and deduplication but do not, by themselves, encode happened-before ordering.\n+\n+This decision is design-compatible with direct peer, local LAN, removable-file, and encrypted indirect transports because the causality evidence lives in the signed statement payload and its content address rather than in any canonical server.\n",
    "commit_message": "Resolve Phase 2 sync causality mechanism",
    "validation_notes": [
      "Patch modifies only OPEN_QUESTIONS.md and DECISIONS.md, both allowed files.",
      "OPEN_QUESTIONS.md preserves the selected question's single-line metadata format and anchors the resolution under UBU-Q0140.",
      "Validated the unified diff with git apply --check against base commit 3bfc8e22fc26bccbb616a65622e6853dfe39099e."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0140"
    ],
    "decisions_added": [
      "UBU-D0247"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "ubu-q0140-hybrid-causality-v1",
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
