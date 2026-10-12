# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0142`  
Question title: `Device disconnection midway through a sync session`  
Base commit: `e736c1d19e398acdfec94c6a3df32e3896038e55`

```markdown
## UBU-Q0142: Device disconnection midway through a sync session

Status: Open Priority: Post-MVP Phase: Phase 2 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: Phase 2 sync session recovery Resolved by: None Last scored: 2026-10-11 Scored from commit: None

Formerly UBU-QSYNC-004 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §16, §17.

### Question

What should happen if a Device disconnects midway through a sync session?

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0142-openai-gpt-6-disconnect-recovery",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0142",
    "base_commit": "e736c1d19e398acdfec94c6a3df32e3896038e55",
    "summary": "Specify durable, idempotent Phase 2 sync recovery: preserve admitted changes, resume or safely discard staging, and distinguish local checkpoint completion from acknowledged delivery.",
    "rationale": "The existing contract already requires checkpoints, dependency safety, idempotency, and recoverable metadata. UBU-D0314 clarifies the remaining operational boundaries: crash-consistent incremental admission, scope-bound local completion, lost-acknowledgment recovery, fresh authority checks, and discard without loss of admitted or locally queued work. It preserves ordinary conflict review and Compartment privacy, and adds concrete Phase 2 certification cases.",
    "changed_files": [
      "DESIGN.md",
      "DECISIONS.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -32,6 +32,18 @@ For Phase 2, a single-live-session sync implementation is sufficient (`UBU-D0312\n \n The minimum indirect path for the first Phase 2 sync release is implemented, authenticated encrypted bundle export/import through a user-selected file store, with at least one documented external rclone or rsync copy workflow exercised before freeze (`UBU-D0313`). Manual export/copy/import is sufficient; built-in clients and background delivery are optional. Encryption and recipient authorization protect statements, payloads, and manifests before storage, while ordinary admission and Compartment policy govern every import.\n \n+**Interrupted sync recovery (`UBU-D0314`).** Maintain a durable, policy-protected recovery record throughout each exchange. Connection loss, interrupted import, or restart without an admitted final checkpoint for the declared exchange scope records `incomplete_sync_session`. End the failed live session and release its slot while preserving recovery records and queued work; other Devices may synchronize and ordinary policy-authorized offline work may continue against an explicitly stale local view.\n+\n+Fully verified, authorized statements whose required causal/admission dependencies are satisfied may be admitted incrementally. Persist each admission outcome, resulting state, and deduplication metadata as one crash-consistent unit. These admissions survive disconnect, retry, and discard, subject to existing redaction/purge policy. Partial statements, unverified payloads, and items with missing required statements or dependencies remain staged; they cannot be treated as admitted mutations or supply authoritative inputs to Plans or external projections.\n+\n+An integrity-protected `checkpoint` or equivalent authenticated manifest commit must bind the declared exchange scope and expected statement/payload inventory, or its content-addressed equivalent. Admit it only after all material required for this Replica's authorized replication level has been durably verified and the required dependency/admission checks have completed. Report completion only after that checkpoint admission is durable, and only for that local scope. A checkpoint cannot admit contested mutations or prove other Devices current. A lost acknowledgment leaves delivery unknown to the sender; replay cannot undo the recipient's durable admissions. Outgoing statements remain pending for an intended recipient until the sender durably records a matching authenticated acknowledgment of that recipient's durable checkpoint admission. Exchange acknowledgments directly or through encrypted return bundles; transmission, file-copy success, or local checkpoint creation is insufficient.\n+\n+The recovery record tracks exchange/bundle identity, source/recipient Device references, authenticated manifest reference if available, last locally admitted checkpoint, received/missing content references, and durable per-statement admission/idempotency outcomes. Reconstruct progress after restart from durable records. Recovery storage, dependency fetches, retention, and diagnostics obey current Zone/Compartment policy; restricted views reveal neither denied payload nor Compartment identity. Retention cleanup preserves permitted causality, deduplication, and incomplete/discard outcome metadata; missing content must be retransmitted rather than assumed received.\n+\n+At the next authorized connection or import, resume from durable progress, or replay the original statements if no usable cursor exists. Preserve original statement ids, content hashes, idempotency keys, causal parents, and observed versions. Recheck integrity, Device trust, actor authority, current routing/Compartment policy, object/policy versions, and dependencies before admitting pending material or disclosing retained content. Recovery records and old checkpoints grant no authority. Automatic recovery is allowed only for deterministic, idempotent, non-authority-changing cases under `UBU-D0248`; revoked Devices and review-required conflicts retain their ordinary rejection/review rules.\n+\n+Offer Resume/retry and Discard through the existing policy-safe incomplete/stale-sync diagnostic, showing the last completed scope and permitted pending counts. Discard removes unfinished incoming staging and its cursor while preserving already-admitted history, locally authored queued work, and still-required policy-permitted causality/idempotency metadata; record the discard outcome. It does not emit a tombstone or undo admitted mutations. A transient interruption alone requires no blocking merge prompt. Review-required conflicts use DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a717's blocking `manual_decision` diagnostic and admitted `conflict_resolution` statement. Apply DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a722's Plan invalidation/recalculation rules to admitted changes even while the exchange remains incomplete.\n+\n ---\n \n ## 2. Core Principles\ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -4974,3 +4974,21 @@ Before freeze, extend \u00a727's fixtures and `UBU-D0312`'s required three-Device di\n - Altered ciphertext/manifests, wrong or unauthorized recipient keys, stale object/policy versions, and revoked origin/recipient Devices, including restricted relays and `full`, `metadata_only`, and `none` replicas. Denied payload and Compartment identity remain inaccessible, conflicts follow ordinary review rules, and admitted changes trigger required Plan invalidation.\n \n Failure blocks the Phase 2 freeze. This decision records the required transport floor; implementation and certification evidence remain outstanding.\n+\n+---\n+\n+## UBU-D0314: Interrupted sync preserves durable admissions and resumes idempotently\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a71. Resolves `UBU-Q0142`; clarifies DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a716, \u00a717 under `UBU-D0248`, `UBU-D0312`, and `UBU-D0313`.\n+\n+The existing incomplete-sync safeguards become an explicit recovery process: retain a durable, policy-protected progress record; release the failed live-session slot; preserve independently admitted statements; and stage incomplete or dependency-blocked material. Completion requires durable admission of a checkpoint/manifest bound to the declared transfer scope and required inventory. Local completion and authenticated recipient acknowledgment are distinct; a lost acknowledgment permits idempotent replay, never rollback or inferred delivery.\n+\n+Resume or replay on the next authorized exchange using original statement identities and fresh admission/policy checks. Discard removes unfinished incoming staging, preserving admitted history, locally authored queued work, and required permitted causality/idempotency metadata. Existing redaction/purge rules still apply. Incomplete/stale diagnostics expose recovery options without leaking denied payload or Compartment identity; authority conflicts retain the existing blocking review flow. Admitted changes invalidate affected Plans even when the session is incomplete.\n+\n+Before Phase 2 freeze, extend \u00a727 and `UBU-D0312`/`UBU-D0313` recovery checks through direct and encrypted-bundle paths with:\n+\n+- Disconnect/restart before receipt, during statement/payload receipt, with missing causal dependencies, and during final checkpoint receipt: no false completion or dependent admission.\n+- Failure after an independent admission and after durable checkpoint admission but before acknowledgment: admitted effects survive restart, replay has no duplicate effects, and the sender preserves unknown-delivery work until authenticated acknowledgment.\n+- Resume/retry/discard after intervening offline writes, policy changes, and Device revocation, including restricted replicas: ordinary conflicts and Plan invalidation hold, discard preserves admitted/queued work, and neither recovery nor diagnostics disclose denied payload or Compartment identity.\n+\n+These are required implementation/certification checks, not evidence that they have passed. Failure blocks Phase 2 freeze.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -2544,15 +2544,15 @@ Resolved by `UBU-D0313`: the first Phase 2 sync release requires implemented enc\n ## UBU-Q0142: Device disconnection midway through a sync session\n \n-Status: Open Priority: Post-MVP Phase: Phase 2 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: Phase 2 sync session recovery Resolved by: None Last scored: 2026-10-11 Scored from commit: None\n+Status: Solved Priority: Post-MVP Phase: Phase 2 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: Phase 2 sync session recovery Resolved by: UBU-D0314 Last scored: 2026-10-11 Scored from commit: None\n \n Formerly UBU-QSYNC-004 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a716, \u00a717.\n \n ### Question\n \n What should happen if a Device disconnects midway through a sync session?\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0314`: disconnect or restart before the declared scope's durable checkpoint leaves a recoverable incomplete exchange. Fully admitted statements survive; incomplete/dependency-blocked material remains staged. Release the live-session slot, resume or replay original statements with current trust/policy/admission checks, and treat lost acknowledgment as unknown sender delivery. Discard removes unfinished incoming staging while preserving admitted history, locally authored queued work, and required permitted causality/idempotency metadata. Completion is local and scope-bound; diagnostics remain Compartment-safe and admitted changes still invalidate Plans. Phase 2 interruption/restart/replay/discard certification is required; implementation evidence remains outstanding.\n \n ---\n",
    "commit_message": "docs: define recovery for interrupted Phase 2 sync sessions",
    "validation_notes": [
      "Generated the unified diff with git diff --no-index over unmodified working-tree copies and proposed temporary copies; repository files were not modified.",
      "git apply --check --whitespace=error-all - passed against the current working tree, without --cached or --recount.",
      "Only DESIGN.md, DECISIONS.md, and the selected UBU-Q0142 block in OPEN_QUESTIONS.md change; DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md remains read-only context.",
      "Verified UBU-D0314 is unused; the selected question metadata changes only Status and Resolved by, preserving all ranking, phase, and provenance fields.",
      "The selected Resolution currently contains Open.; the patch replaces that exact working-tree text and anchors its combined hunk with the UBU-Q0142 heading and its own Resolution section.",
      "Checked consistency with the incomplete-sync safeguards and UBU-D0248, UBU-D0312, and UBU-D0313. Runtime interruption/certification cases are specified, not claimed executed; human approval remains required."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0142"
    ],
    "decisions_added": [
      "UBU-D0314"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0142-openai-gpt-6-disconnect-recovery",
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
