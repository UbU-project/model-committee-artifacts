# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0146`  
Question title: `Sync-state warnings without the one-device-at-a-time anti-pattern`  
Base commit: `372efa919eb54a9868552636b986bdf0bff5c280`

```markdown
## UBU-Q0146: Sync-state warnings without the one-device-at-a-time anti-pattern

Status: Open Priority: Post-MVP Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: Phase 2 sync-state surfacing Resolved by: None Last scored: 2026-10-11 Scored from commit: None

Formerly UBU-QSYNC-008 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §23.

### Question

What sync-state warnings are necessary to prevent the one-device-powered-on-at-a-time anti-pattern
without becoming annoying or paternalistic?

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "ubu-q0146-openai-gpt-6-sync-state-warnings-20261011",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0146",
    "base_commit": "372efa919eb54a9868552636b986bdf0bff5c280",
    "summary": "Resolve Phase 2 sync-warning UX with passive scoped status, contextual freshness warnings, durable advisory suppression, and an explicit stale-state choice before external projection.",
    "rationale": "Concurrent Device use is already supported. Warnings should explain evidence-backed freshness and delivery limits when they matter, without treating offline work or another powered-on Device as an error. Operation-bound stale-state acceptance preserves user choice while leaving integrity, authority, privacy, and blocking conflict rules intact.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -4992,3 +4992,19 @@ Before Phase 2 freeze, extend \u00a727 and `UBU-D0312`/`UBU-D0313` recovery checks t\n - Resume/retry/discard after intervening offline writes, policy changes, and Device revocation, including restricted replicas: ordinary conflicts and Plan invalidation hold, discard preserves admitted/queued work, and neither recovery nor diagnostics disclose denied payload or Compartment identity.\n \n These are required implementation/certification checks, not evidence that they have passed. Failure blocks Phase 2 freeze.\n+\n+---\n+\n+## UBU-D0315: Phase 2 sync warnings preserve concurrent Device use and surface relevant uncertainty\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a71. Resolves `UBU-Q0146`; applies DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a710, \u00a717, \u00a723 under `UBU-D0248`, `UBU-D0312`, and `UBU-D0314`.\n+\n+Phase 2 uses one scoped, non-blocking sync status, contextual Plan freshness and visibility explanations, and a per-operation or reviewed-batch stale-state choice before external projection. Concurrent Device use and ordinary offline work remain normal. Checkpoints prove only their completed local scope; pending exchange, unconfirmed delivery, and unknown peer freshness are distinct. Dismissal/snooze survives restart, with recurrence tied to material permitted changes or user-selected reminders. Existing blocking conflict, policy, and authority checks remain mandatory. Privacy governs text, counts, urgency, and recurrence.\n+\n+Before Phase 2 freeze, demonstrate:\n+\n+- Three Devices used concurrently with offline mutations and serialized direct/encrypted-bundle exchanges: no power-off/single-primary instructions, modal merely for peer absence, or false global-freshness status after copy success, interruption, or lost acknowledgment.\n+- Repeated view opens, aging, and restart preserve discoverable status and advisory suppression until material change or an explicitly chosen reminder/snooze deadline; relevant input changes resurface explanations. External projection offers exchange, a logged version-bound stale-state choice when eligible, or cancellation, with one choice per reviewed batch.\n+- Partial/restricted replicas expose only permitted diagnostics, including counts and recurrence; intentional visibility limits survive successful exchange. Stale-state acceptance or dismissal cannot bypass policy denial or required `manual_decision` review.\n+\n+These are required behavior checks, not evidence of implementation or completed certification.\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -44,6 +44,18 @@ At the next authorized connection or import, resume from durable progress, or re\n \n Offer Resume/retry and Discard through the existing policy-safe incomplete/stale-sync diagnostic, showing the last completed scope and permitted pending counts. Discard removes unfinished incoming staging and its cursor while preserving already-admitted history, locally authored queued work, and still-required policy-permitted causality/idempotency metadata; record the discard outcome. It does not emit a tombstone or undo admitted mutations. A transient interruption alone requires no blocking merge prompt. Review-required conflicts use DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a717's blocking `manual_decision` diagnostic and admitted `conflict_resolution` statement. Apply DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a722's Plan invalidation/recalculation rules to admitted changes even while the exchange remains incomplete.\n \n+**Sync-state warnings (`UBU-D0315`).** Multiple Devices may remain powered on and perform policy-authorized offline work simultaneously; only live sync exchanges serialize under `UBU-D0312`. Explain this once in Phase 2 onboarding. Ordinary offline use, Device switching, peer absence, and elapsed time alone cause no modal warning or recurring notification. Time-based reminders are opt-in with a user-selected cadence.\n+\n+Keep one compact, non-blocking status for the active permitted sync scope, with details on demand: last durably admitted checkpoint and its scope/time (or \"No completed exchange yet\"), permitted locally authored pending counts split into awaiting exchange and awaiting recipient acknowledgment, staged incoming work, and incomplete-session state with Resume/retry and Discard per `UBU-D0314`. Say \"Delivery unconfirmed\" for a missing acknowledgment. File-copy success and a completed local exchange never justify \"All Devices are up to date\". A disconnected peer's changes are unknown unless policy-permitted evidence establishes pending state; age alone proves neither new changes nor their absence.\n+\n+Show an inline Plan/Calendar freshness explanation where relevant inputs may be missing or stale: \"This Plan uses this Device's current view; other Devices may have changes.\" Identify confirmed remote pending changes only when supported by permitted evidence. Locally authored outgoing work is saved local state, not evidence that the local Plan is stale. Label protected blocks and policy-withheld items separately from exchange failures; retrying sync cannot expand visibility.\n+\n+Before external projection whose relevant permitted inputs have known pending changes, an incomplete exchange, or unknown peer freshness, present an inline review: \"Other Device changes may be missing. Synchronize before projection, or use this Device's current view.\" Offer Sync/exchange (including export/import), Continue with current view, and Cancel. Continue requires explicit stale-state acceptance once per projection operation or reviewed batch, bound to the Device, operation, payload/input and policy versions, and permitted scope. Record it under \u00a717.0 using `decision_recorded` with `decision_kind: safeguard_advisory_bypassed`; recheck the bindings before execution. This accepts freshness uncertainty only. Current authority, Device trust, integrity, Compartment/export policy, Plan-legitimacy requirements, and `UBU-D0248`'s blocking `manual_decision`/`conflict_resolution` rules still govern eligibility. If those rules forbid execution, omit Continue. Mere interruption or missing peer contact creates no blocking merge prompt.\n+\n+Coalesce advisory explanations by Device, permitted scope, condition, and material evidence. Offer Dismiss until material change or Snooze until a user-selected time; persist the local choice across views and restart while keeping status discoverable. Reopen for material changes affecting that warning, such as newly learned permitted pending statements or changed relevant Plan/input versions; snooze may also reopen at its chosen deadline. A new projection operation needs its own scoped choice. Age or restart alone cannot reopen a dismissed advisory; opt-in reminders may follow their chosen cadence. Dismissal needs no justification and is logged under \u00a717.0 without turning the warning into a synchronized Task. Advisory suppression cannot hide a required blocking conflict diagnostic.\n+\n+Generate visible text, counts, urgency, and reappearance only from evidence or derived warnings allowed at this Replica's replication level. Restricted notices use DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a723's \"A protected object cannot sync to this Device.\" without denied payload, source object identifiers, or Compartment ids/labels; omit object-specific notices when even existence is denied. Warnings preserve intentional partial replication and never recommend powering off other Devices or switching to a single primary Device.\n+\n ---\n \n ## 2. Core Principles\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -2614,22 +2614,22 @@ Resolved by `UBU-D0250`: Phase 2 worker Devices use a scoped request/response pr\n \n ---\n \n ## UBU-Q0146: Sync-state warnings without the one-device-at-a-time anti-pattern\n \n-Status: Open Priority: Post-MVP Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: Phase 2 sync-state surfacing Resolved by: None Last scored: 2026-10-11 Scored from commit: None\n+Status: Solved Priority: Post-MVP Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: Phase 2 sync-state surfacing Resolved by: UBU-D0315 Last scored: 2026-10-11 Scored from commit: None\n \n Formerly UBU-QSYNC-008 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a723.\n \n ### Question\n \n What sync-state warnings are necessary to prevent the one-device-powered-on-at-a-time anti-pattern\n without becoming annoying or paternalistic?\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0315`: Phase 2 uses scoped non-blocking sync status, evidence-based Plan freshness and visibility explanations, and an explicit operation/batch-bound stale-state choice before external projection. Concurrent powered-on Devices and ordinary offline work remain normal. Advisory dismissal/snooze persists across restart; only material permitted changes or user-selected reminder/snooze deadlines resurface it. Completion/delivery claims stay scoped and evidence-based, diagnostics obey replication/privacy policy, and stale-state acceptance cannot bypass authority checks or blocking conflict review.\n \n ---\n \n ## UBU-Q0147: Schedulable syncable manual-review Task and the causal regress\n \n",
    "commit_message": "docs: resolve UBU-Q0146 with scoped Phase 2 sync warnings",
    "validation_notes": [
      "Read current working-tree sections and checked consistency with UBU-D0248, UBU-D0312, UBU-D0313, UBU-D0314 and the read-only sync contract.",
      "Generated unified hunks with git diff --no-index against unchanged working-tree files and proposed contents held in memory; normalized path headers and omitted optional index headers.",
      "git apply --check --whitespace=error passed against the current working tree; no index-based validation was used.",
      "Verified one selected-question hunk contains its exact heading, metadata, and own Resolution; preserved all other metadata and question blocks.",
      "Only DECISIONS.md, DESIGN.md and OPEN_QUESTIONS.md differ in the proposal; working-tree files remain unchanged. New behavior checks are requirements, not completed implementation tests.",
      "Human approval is required for this Product decision; the Solved/Accepted updates are proposed for that approval."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0146"
    ],
    "decisions_added": [
      "UBU-D0315"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "ubu-q0146-openai-gpt-6-sync-state-warnings-20261011",
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
