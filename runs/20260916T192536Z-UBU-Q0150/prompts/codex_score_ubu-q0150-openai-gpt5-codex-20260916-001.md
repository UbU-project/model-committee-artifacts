# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0150`  
Question title: `Redacted object and Compartment handle stability scope`  
Base commit: `cc9181c72201abb1e0e28c0086bd3f8529975f80`

```markdown
## UBU-Q0150: Redacted object and Compartment handle stability scope

Status: Open Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0133, Phase 2 redaction identity Resolved by: None Last scored: 2026-09-16 Scored from commit: None

Formerly UBU-QSYNC-012 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §10, §12.

### Question

Should redacted object/Compartment handles be stable per Device, per sync session, per Calendar
window, or per object version, and how much correlation risk is acceptable?

### Resolution

Open.

```

## Candidate proposals

```json
[
  {
    "proposal_id": "ubu-q0150-openai-gpt5-codex-20260916-001",
    "provider_id": "openai",
    "model_name": "gpt-5-codex",
    "question_id": "UBU-Q0150",
    "base_commit": "cc9181c72201abb1e0e28c0086bd3f8529975f80",
    "summary": "Resolve UBU-Q0150 by defining redacted handles as local rotating aliases: object handles are stable only for one receiving Device, one redacted source-object version, and one projection window; Compartment grouping handles are omitted by default and, when allowed, are stable only for one Device/window/policy epoch.",
    "rationale": "This preserves the hard invariant that restricted Devices never learn real Compartment identity while still allowing limited local UI continuity and de-duplication. It accepts only bounded correlation inside one authorized restricted projection window and rejects durable cross-Device, cross-window, cross-version, or Compartment-pattern correlation.",
    "changed_files": [
      "DECISIONS.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\nindex dca936a..783fb7f 100644\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -3870,3 +3870,19 @@ Recovery is a separate custody path, not a backdoor replication exception. A rec\n Sync statements may reference secret capability IDs, version IDs, envelope IDs, rotation records, and revocation records. They must not carry reusable bearer tokens, raw local encryption keys, or worker credentials as plaintext sync content.\n \n This decision intentionally leaves concrete cryptographic algorithms, OS keychain integrations, hardware enclave support, and provider-specific refresh-token mechanics to implementation design, while fixing the custody semantics needed to keep Phase 3 integrations inside the Compartment model.\n+\n+---\n+\n+## UBU-D0253: Redacted handles are local rotating aliases\n+\n+**Status:** Accepted \u2192 DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a710, \u00a712. Resolves `UBU-Q0150`.\n+\n+Redacted object and Compartment handles are privacy-preserving aliases for a restricted Replica. They are never real object IDs, `compartment_id` values, Compartment labels, or encodings from which a restricted Device can recover or recognize those identities.\n+\n+For `redacted_object_ref`, the default stability scope is the receiving Device, the redacted source-object version, and the declared projection window. The handle may remain stable across retries or repeated sync of that same version within that same window so the restricted Device can de-duplicate and render consistently, but it rotates when the source object version, redaction level, policy epoch, target Device authority, or projection window changes.\n+\n+For `compartment_ref`, the default is omission. If policy explicitly permits restricted grouping, the grouping handle is stable only for the receiving Device, declared projection window, and policy epoch. It rotates across Devices, across projection windows, and across policy epochs, and it must be omitted whenever the fact of grouping would itself leak sensitive context.\n+\n+All redacted handles must be generated as high-entropy random aliases or keyed aliases whose key is unavailable to the restricted Device. Handles must carry no semantic prefix, reason code, sequence, compartment vocabulary, or stable global identifier.\n+\n+The acceptable correlation risk is limited to local correlation needed for one authorized restricted Device to maintain UI continuity and de-duplication inside one projection window. Durable cross-Device, cross-window, cross-version, or Compartment-pattern correlation is not acceptable for redacted replicas.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\nindex b7f8ec6..1d6d4d2 100644\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -2674,14 +2674,14 @@ Resolved by `UBU-D0252`: Phase 3 treats external tokens, local encryption keys,\n ## UBU-Q0150: Redacted object and Compartment handle stability scope\n \n-Status: Open Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0133, Phase 2 redaction identity Resolved by: None Last scored: 2026-09-16 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0133, Phase 2 redaction identity Resolved by: UBU-D0253 Last scored: 2026-09-16 Scored from commit: None\n \n Formerly UBU-QSYNC-012 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a710, \u00a712.\n \n ### Question\n \n Should redacted object/Compartment handles be stable per Device, per sync session, per Calendar\n window, or per object version, and how much correlation risk is acceptable?\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0253`: Redacted object handles are local rotating aliases stable only for the receiving Device, the redacted source-object version, and the declared projection window, with rotation on version, redaction, policy, authority, or window changes. Compartment grouping handles are omitted by default; when policy permits them, they are stable only for the receiving Device, projection window, and policy epoch. Acceptable correlation is limited to local UI continuity and de-duplication inside one authorized restricted projection window; durable cross-Device, cross-window, cross-version, or Compartment-pattern correlation is not acceptable.\n",
    "commit_message": "Resolve redacted handle stability scope",
    "validation_notes": [
      "Patch validated with git apply --check against base commit cc9181c72201abb1e0e28c0086bd3f8529975f80.",
      "Patch modifies only DECISIONS.md and OPEN_QUESTIONS.md.",
      "OPEN_QUESTIONS.md preserves the single-line metadata format and anchors the change in the UBU-Q0150 block."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0150"
    ],
    "decisions_added": [
      "UBU-D0253"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "ubu-q0150-openai-gpt5-codex-20260916-001",
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
