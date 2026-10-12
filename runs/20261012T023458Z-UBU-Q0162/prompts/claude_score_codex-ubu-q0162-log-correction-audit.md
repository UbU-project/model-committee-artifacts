# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `codex`

## Selected question

Question ID: `UBU-Q0162`  
Question title: `Enforce append-only Log correction rather than in-place editing`  
Base commit: `c13175653923461f78b785d07b5e0cba7a28d2ae`

```markdown
## UBU-Q0162: Enforce append-only Log correction rather than in-place editing

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-11 Scored from commit: None

### Question

Where does the operator-reported editable Log violate the existing append-only contract, and what minimum implementation correction closes it?

### Current direction

Append-only Logs and linked corrections are already design rules, not a newly adopted requirement. Audit edit paths and distinguish annotation, corrected query views and compensating entries from changing admitted history. Preserve original assertions, correction/revocation links and privacy-minimized media-deletion metadata. The affected implementation paths and remediation ticket are unresolved; this documentation ticket changes no store or UI behavior.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "codex-UBU-Q0162-log-correction-audit",
    "provider_id": "codex",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0162",
    "base_commit": "c13175653923461f78b785d07b5e0cba7a28d2ae",
    "summary": "Specify the Log edit-path audit, minimum linked-correction repair and evidence required to close UBU-Q0162.",
    "rationale": "Append-only Logs are already required by UBU-D0071 and DESIGN.md \u00a7\u00a712.1 and 17.3. The reported offending path and remediation ticket remain unidentified. This proposal makes the existing question actionable while keeping it Open until implementation evidence supports closure.",
    "changed_files": [
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\nindex c4cd28b..0000000 100644\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3037,27 +3037,46 @@ Resolved by `UBU-D0317`: Task descriptions become readable, explicitly reviewed\n ## UBU-Q0162: Enforce append-only Log correction rather than in-place editing\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-11 Scored from commit: None\n \n ### Question\n \n Where does the operator-reported editable Log violate the existing append-only contract, and what minimum implementation correction closes it?\n \n ### Current direction\n \n Append-only Logs and linked corrections are already design rules, not a newly adopted requirement. Audit edit paths and distinguish annotation, corrected query views and compensating entries from changing admitted history. Preserve original assertions, correction/revocation links and privacy-minimized media-deletion metadata. The affected implementation paths and remediation ticket are unresolved; this documentation ticket changes no store or UI behavior.\n \n+### Audit and minimum repair\n+\n+Record a privacy-safe reproduction identifying the running repository/revision, reported control or command, affected entry reference, and admitted record before and after the operation. Trace that operation through its UI/CLI, handler, admission and store writer; inventory other application-owned update, delete, replacement, import and replay paths that can reach the same records. Attach actual file/symbol/route references to the remediation ticket. An editable draft, changed corrected view, rebuilt index or reserialized storage file alone is not evidence that admitted history changed.\n+\n+The minimum repair uses the existing Log envelope and event types:\n+\n+- Route added notes through `log_annotation_added` with `annotation_of`; route withdrawal or replacement of an assertion through `log_correction_added` with `correction_of`. Each is a separately admitted entry with a fresh `log_entry_id`, correcting actor/Device, provenance, admission time and idempotency key. Annotation or semantic correction keeps the original envelope, assertion and existing links unchanged. Generic edit/delete paths must reject attempts to change admitted history, including its notes and provenance; an operator's edit authority does not waive this invariant.\n+- Resolve current query views through admitted correction/revocation links while retaining policy-permitted historical views. A replacement Snapshot creates a new Snapshot and links it under DESIGN.md \u00a712.1. Correcting a Log assertion does not automatically undo Task effects, UniverseState mutations or external actions; any compensating transition needs its own ordinary admission and authority.\n+- Reuse current authority, policy, observed-version and idempotency checks (`UBU-D0258`). Commit the new entry, admission outcome and any required canonical compensation atomically; invalidate affected derived state through ordinary recalculation. Stale or competing corrections use the ordinary conflict rules (`UBU-D0248`), not an unconditional overwrite or timestamp-only choice.\n+- Keep policy-authorized media deletion, redaction and purge distinct from semantic correction. Preserve only permitted minimized correction/revocation/deletion links and outcome metadata under DESIGN.md \u00a712.2 and the existing Compartment rules. Neither originals nor correction payloads, indexes or derived views may retain or reconstruct content whose policy requires removal. Deletion must not fabricate replacement evidence or imply that an unconfirmed remote purge completed.\n+\n+### Closure evidence\n+\n+Close this question only with a revision-pinned reproduction, an implementation remediation ticket naming the affected paths, and passing behavioral checks showing:\n+\n+- Annotation, correction and revocation leave the admitted original and prior links unchanged, create the appropriate linked entries, and expose both corrected meaning and permitted historical assertions. Snapshot replacement and required compensation remain explicit separate records.\n+- Application-owned direct and generic edit/delete bypasses fail; duplicate retry/replay adds no second entry or repeated compensation; stale, unauthorized and conflicting requests cannot rewrite history. Interrupted admission leaves no partial correction/compensation result.\n+- Query, import/replay and relevant planning inputs respect the admitted links; authorized media deletion leaves evidence unavailable with policy-safe metadata and no recoverable denied copy in dependent views.\n+\n ### Resolution\n \n-Open.\n+Open. Append-only correction is already decided. The reported offending path, concrete remediation ticket and passing implementation evidence remain outstanding; this audit and repair specification changes no store or UI behavior.\n \n ---\n \n ## UBU-Q0163: Retire the uncaptured recurring-busy capacity workaround\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-11 Scored from commit: None\n \n ### Question\n \n What verified capture/occupancy behavior allows removal of the temporary manual blocking Static event used for uncaptured recurring busy time during the Phase 1b-to-Phase 2 transition?\n \n ### Current direction\n",
    "commit_message": "docs: specify Q0162 Log correction audit and closure evidence",
    "validation_notes": [
      "Generated with git diff --no-index against the current working-tree file, supplying proposed content on stdin and normalizing only destination labels.",
      "git apply --check --whitespace=error - passed against /home/sean/ubu-design.",
      "Only UBU-Q0162's body changes; its single-line metadata, scores, status, neighboring questions and existing IDs remain unchanged.",
      "Checked consistency with UBU-D0071, UBU-D0248, UBU-D0258 and DESIGN.md \u00a7\u00a712.1, 12.2 and 17.3.",
      "Documentation only: no defect reproduction, remediation ticket creation or passing runtime checks are claimed.",
      "Repository files remained unchanged during generation and validation."
    ],
    "new_questions_added": [],
    "questions_resolved": [],
    "decisions_added": [],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "codex-UBU-Q0162-log-correction-audit",
    "patch_applies": true,
    "allowlist_passed": true,
    "changed_files": [
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
