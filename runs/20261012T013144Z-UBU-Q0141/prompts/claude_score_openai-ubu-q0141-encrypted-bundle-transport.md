# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0141`  
Question title: `Minimum acceptable encrypted indirect sync transport`  
Base commit: `d8ce0c79bb8ebfe3c9c77adf87cbdc1dd317fcd1`

```markdown
## UBU-Q0141: Minimum acceptable encrypted indirect sync transport

Status: Open Priority: Post-MVP Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: Phase 2 sync transport Resolved by: None Last scored: 2026-10-11 Scored from commit: None

Formerly UBU-QSYNC-003 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §8, §27.

### Question

What is the minimum acceptable encrypted indirect sync transport? Should encrypted rclone/rsync-
compatible storage be an MVP-supported path or only design-compatible?

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "openai-ubu-q0141-encrypted-bundle-transport",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0141",
    "base_commit": "d8ce0c79bb8ebfe3c9c77adf87cbdc1dd317fcd1",
    "summary": "Require implemented encrypted bundle store-and-forward for the first Phase 2 sync release, with one certified user-managed rclone or rsync workflow and optional built-in clients.",
    "rationale": "UBU-D0312 already requires encrypted store-and-forward evidence before Phase 2 freeze. A working file-based bundle path makes that requirement concrete without requiring a managed relay or integrated transport client. Encryption before storage, policy-scoped recipient access, authenticated completion, and ordinary statement admission preserve privacy and transport-independent authority. The earlier single-Device MVP keeps its existing scope.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -4955,3 +4955,22 @@ Before final Phase 2 freeze, record topology research comparing serialized direc\n - Partial-replica fixtures covering `full`, `metadata_only`, and `none`, including policy changes and restricted relay/recipient Devices. Forwarding, conflict diagnostics, and redacted views must preserve payload and Compartment-identity denial under current policy.\n \n These are required research and implementation checks, not a claim that they have passed. Failure blocks the Phase 2 freeze until the topology or delivery implementation is corrected. Higher session concurrency or optional mesh may later be added while preserving the same statement and authority semantics.\n+\n+---\n+\n+## UBU-D0313: Phase 2 requires encrypted bundle store-and-forward with one certified file-copy workflow\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a71. Resolves `UBU-Q0141`; specifies the indirect transport minimum under DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a78, \u00a727 and `UBU-D0312`.\n+\n+The first Phase 2 sync release must implement export and import of immutable encrypted SyncStatement bundles through a user-selected file store. Manual export/copy/import is sufficient. Before Phase 2 freeze, at least one documented external rclone or rsync copy workflow must be exercised with the actual exporter and importer through a user-controlled store. Schema-only compatibility or fixture-only bundles do not satisfy this gate. Built-in clients, background delivery, and additional backends are optional; unimplemented transports remain design-compatible and must be labeled accordingly. The earlier single-Device MVP retains its existing scope.\n+\n+Encrypt and authenticate bundle contents on an authorized exporting Device before shared-store writes, including statements, referenced payloads, and manifest/checkpoint metadata. Recipient/key scope must enforce Device, Zone, Compartment, routing, and retention policy; storage access or a common store password must not grant a restricted Device denied payload or Compartment identity. Once an exporter admits revocation, new exports must exclude that Device from decryption grants; previously delivered content retains the existing best-effort exposure/deletion limits. Decryption keys stay in authorized Device custody or policy-authorized encrypted recipient envelopes, never plaintext in the store. Outer filenames and transport diagnostics must expose no plaintext object or Compartment ids or labels. TLS or provider-side encryption alone is insufficient, and encryption does not authorize a forbidden export route. Document visible ciphertext sizes and transfer timing without claiming traffic-analysis protection or retroactive deletion of previously disclosed content.\n+\n+Forwarding preserves original statement identity, authorship, causal parents, observed object/policy versions, and idempotency. Import uses the same integrity, Device trust, policy, admission, conflict, and Plan-invalidation rules as direct sync. Store ordering, modification times, overwrites, deletions, and copy success cannot admit, supersede, or tombstone canonical state. Completion requires an admitted checkpoint or equivalent authenticated manifest commit; partial receipt must not admit dependent payloads without required statements. Incomplete delivery remains detectable and recoverable, repeated import is idempotent, and queued statements and causal dependencies remain recoverable across sessions subject to policy, as required by `UBU-D0312`.\n+\n+Before freeze, extend \u00a727's fixtures and `UBU-D0312`'s required three-Device direct/store-and-forward research and implementation checks with:\n+\n+- An actual export/store/import round trip through the documented copy workflow, including reordered and duplicate bundles, interrupted/retried delivery, missing dependencies/checkpoints, an unavailable store, and a late rejoining Device. Equivalent policy-permitted statements must yield equivalent admitted state; copy success must never be reported as admission or global freshness.\n+- Altered ciphertext/manifests, wrong or unauthorized recipient keys, stale object/policy versions, and revoked origin/recipient Devices, including restricted relays and `full`, `metadata_only`, and `none` replicas. Denied payload and Compartment identity remain inaccessible, conflicts follow ordinary review rules, and admitted changes trigger required Plan invalidation.\n+\n+Failure blocks the Phase 2 freeze. This decision records the required transport floor; implementation and certification evidence remain outstanding.\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -30,6 +30,8 @@ For Phase 1, UbU must also be understandable as a first-person user experience.\n \n For Phase 2, a single-live-session sync implementation is sufficient (`UBU-D0312`), with arbitrary N Devices, concurrent offline mutations, partial replicas, and transport-independent integrity-protected statements. Direct exchanges, imported bundles, and user-selected encrypted stores share the same causality, policy, and conflict rules; no Device or transport becomes a permanent canonical authority. Full automatic peer mesh is optional. DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a726's topology research remains a freeze gate, with at least three Devices demonstrating serialized direct exchange and encrypted store-and-forward, recoverable delivery, deterministic admission/conflict handling, and Compartment-safe partial replication as specified by `UBU-D0312`.\n \n+The minimum indirect path for the first Phase 2 sync release is implemented, authenticated encrypted bundle export/import through a user-selected file store, with at least one documented external rclone or rsync copy workflow exercised before freeze (`UBU-D0313`). Manual export/copy/import is sufficient; built-in clients and background delivery are optional. Encryption and recipient authorization protect statements, payloads, and manifests before storage, while ordinary admission and Compartment policy govern every import.\n+\n ---\n \n ## 2. Core Principles\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -2526,18 +2526,18 @@ This rejects a pure Lamport clock because wall-time adjacency is useful for oper\n \n ## UBU-Q0141: Minimum acceptable encrypted indirect sync transport\n \n-Status: Open Priority: Post-MVP Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: Phase 2 sync transport Resolved by: None Last scored: 2026-10-11 Scored from commit: None\n+Status: Solved Priority: Post-MVP Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: Phase 2 sync transport Resolved by: UBU-D0313 Last scored: 2026-10-11 Scored from commit: None\n \n Formerly UBU-QSYNC-003 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a78, \u00a727.\n \n ### Question\n \n What is the minimum acceptable encrypted indirect sync transport? Should encrypted rclone/rsync-\n compatible storage be an MVP-supported path or only design-compatible?\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0313`: the first Phase 2 sync release requires implemented encrypted bundle export/import over a user-selected file store, with at least one documented rclone or rsync copy workflow certified before freeze. Manual export/copy/import suffices; built-in clients and additional backends are optional, and unimplemented transports remain design-compatible. Authenticated encryption, policy-scoped recipient access, and ordinary statement admission/completion rules are mandatory. The earlier single-Device MVP retains its scope, and `UBU-D0312`'s three-Device research gate remains required. Implementation and certification evidence remain outstanding.\n \n ---\n \n",
    "commit_message": "docs: require certified encrypted bundle transport for Phase 2",
    "validation_notes": [
      "Generated path-normalized git diff --no-index output by comparing each unmodified working-tree file with proposed contents supplied on stdin; HEAD/index contents were not used as patch baselines.",
      "git apply --check --whitespace=error - passed against the current working tree; all unified hunk ranges were independently checked without --recount.",
      "The OPEN_QUESTIONS.md edit is one hunk containing the UBU-Q0141 heading and its own Resolution section. Only Status and Resolved by change in its single-line metadata; existing scoring fields are preserved.",
      "UBU-D0313 is unused in the current working tree. The proposal preserves UBU-D0312 topology research and three-Device certification gates; no new questions are needed.",
      "Only DECISIONS.md, DESIGN.md, and OPEN_QUESTIONS.md appear in the patch. No workspace files were modified; git status --short remains clean. Runtime transport and cryptographic certification are proposed release requirements, not completed tests."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0141"
    ],
    "decisions_added": [
      "UBU-D0313"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "openai-ubu-q0141-encrypted-bundle-transport",
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
