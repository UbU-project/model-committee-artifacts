# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0166`  
Question title: `Preserve perceptual qualifiers alongside the closed provenance enum`  
Base commit: `7f27411212ca68ff14369579895854bd0d5a3961`

```markdown
## UBU-Q0166: Preserve perceptual qualifiers alongside the closed provenance enum

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-13 Scored from commit: None

### Question

Which revision-pinned host mappings preserve the claim-level qualifications specified by `UBU-D0321` through validation, serialization, storage, review and authorized exchange, and does a failed mapping demonstrate a schema gap requiring separate versioned review?

### Current direction

Delta §17.4. `UBU-D0293` decides kind mapping; `UBU-D0320` decides observation/admission bindings; `UBU-D0321` specifies claim-level qualifier content, field/evidence links and fidelity fixtures. The remaining work is a revision-pinned host field/payload mapping and round-trip evidence from the actual validators, serializers, store/review and exchange paths. Measured remains an instrument/reading, derived a computation from facts, and proposed an unconfirmed candidate. Do not infer host compatibility from prose payload slots. A demonstrated gap requires separate versioned chain review; no schema, pin or migration change is made here. `UBU-D0323` records bounded candidate link-carrier checks and source-inspected downstream limitations. Full claims/qualifiers and resolved evidence revisions still need fixtures through the actual Discovery, normalization, store/review, admission and authorized-exchange paths. A rejected direct field mapping alone does not establish a schema gap.

### Resolution

Open. `UBU-D0321` answers preservation requirements; `UBU-D0323` records only a synthetic candidate link-carrier round trip, rejection by string-valued field_provenance and unknown-profile acceptance by generic host layers. Complete claim/qualifier mappings, required adapter validation and downstream fixture outcomes remain outstanding. Close only with the full pinned mapping and fixture evidence, or a minimally reproduced host-contract gap routed to a separate versioned change. No passing adapter or actual schema gap is established.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "openai-ubu-q0166-d0334-normalization-evidence",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0166",
    "base_commit": "7f27411212ca68ff14369579895854bd0d5a3961",
    "summary": "Record actual qualifier loss during pinned precondition-review normalization, with successful direct storage/reload/replay and review API controls; keep Q0166 open.",
    "rationale": "The existing question is partially answered. Runtime probes now show that the tested inline specimen survives candidate storage and API retrieval but is silently discarded by precondition_review::vet_result while returning ok. A closed normalized-proposal alternative rejects. This narrows the failing mapping without claiming a schema gap, complete qualifier preservation or an enabled adapter.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\nindex e6ca808..8f9578c 100644\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5268,3 +5268,15 @@ Require the exact controller-instantiated English phrase `confirm send to <recip\n Invalidate on interruption or changed bindings; serialize cancellation, approval consumption and dispatch, persist the ordinary attempt before egress and apply current authority and outcome reconciliation. Preserve minimized existing review/evidence/AgentAction/Log links without compulsory recordings or transferable live approval. Require \u00a721.1.2's revision-pinned negative and replay fixtures before enabling this route in a separately released sending workflow.\n \n This specifies confirmation behavior, not an implemented or newly permitted sending action. `AudioInterpretationV1` and `InteractionBudgetV1` remain unchanged; every initial restricted-attention mode still defers external sends despite a correct phrase. `UBU-Q0128` remains Open for numeric profiles, other external-action grammars, richer actions/integrations, device-specific behavior and naming.\n+\n+---\n+\n+## UBU-D0334: Pinned probes reproduce qualifier loss during precondition review normalization\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a721.1.1. Narrows `UBU-Q0166`; extends `UBU-D0323` with bounded runtime evidence without enabling an adapter.\n+\n+Record the synthetic specimen, pinned builds and actual normalization, SQLite close/reopen/replay and review API controls in DESIGN.md \u00a721.1.1. The inline link specimen survives direct candidate storage and API reads when normalization is bypassed. The tested `precondition_review::vet_result` branch instead returns `ok` while rebuilding the payload without that specimen; adding it to the closed native normalized proposal rejects. An unknown companion profile is also discarded by that branch rather than receiving the required adapter rejection.\n+\n+These results disqualify the tested inline mapping through that normalization branch. They establish a current implementation loss, not a governing host-contract gap across all compliant existing payload/ref mappings. A future adapter using this path must demonstrate preservation or leave the interpretation unresolved before executable review; native `ok`, durable candidate storage and API retention cannot substitute for claim/version/profile validation or rendered qualifier review.\n+\n+`UBU-Q0166` remains Open for complete Discovery claim/evidence mappings, adapter validation, faithful normalization and presentation, admission and authorized-exchange fixtures. Synthetic refs remain unresolved; pool close/reopen is not a process/crash-recovery test, and direct API calls test neither HTTP authorization nor disclosure middleware. No schema, enum, FactProvenance field, pin, migration or producer authority changes here; `UniverseTarget` remains name-only.\ndiff --git a/DESIGN.md b/DESIGN.md\nindex 6a52490..9fc4836 100644\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -3401,6 +3401,32 @@ No passing host mapping or schema gap is asserted here. A mapping that cannot pr\n \n These are candidate link-carrier probes with unresolved synthetic refs, not the full qualifier/admission fixtures above. No producer/adapter, Discovery claim/evidence resolution, normalization, store/review, admission or authorized-exchange fixture was run; no policy-authorized exchange revision/configuration or complete host mapping is established. Unknown profiles still must remain unresolved before admission. The rejected direct field choice does not establish a schema gap when an inline representation remains available for testing. Report any failure at its pinned host stage with exact expected/retrieved content; distinguish mapping/implementation defects from a minimally reproduced host-contract restriction that a compliant existing payload/ref mapping cannot satisfy. `UBU-Q0166` stays Open for the remaining evidence, with no adapter enabled by these results.\n \n+**Normalization and persistence follow-up (`UBU-D0334`; 2026-10-13).** Reused the schema, core, store and orchestrator checkout revisions pinned in `UBU-D0323`; tracked files remained clean. A temporary synthetic harness called the actual public functions, linked to orchestrator libraries verified by `cargo build --locked --offline --lib`. The canonical validator was verified by `cargo build --locked --offline -p validate-fixtures`. Both builds used the rustc version above; this harness uses the orchestrator lockfile's serde `1.0.228`, serde_json `1.0.150`, sqlx `0.8.6` and tokio `1.52.3`, rather than the earlier standalone core probe's dependency versions. The schema tool still uses jsonschema `0.46.5`. No dependency pins changed.\n+\n+**Minimal failing specimen.** Start from the same pinned `proposed-precondition.json` fixture used above. Remove its candidate-level `confidence`, retain all other host fields, and set both `normalized_proposal` and `payload.value` to `N`:\n+\n+```json\n+{\"operation\":\"clear_precondition\",\"verdict\":\"remove\",\"reason\":\"Synthetic carrier probe.\",\"existing_precondition\":{\"target\":\"numeric_values.synthetic.orbital_teapot_charge\",\"predicate\":\"at_least\",\"expected\":25},\"blocked_now\":false}\n+```\n+\n+The baseline has no companion payload. The inline case adds the exact `P` above at `payload.value.perceptual_qualifiers`; the unknown-profile case changes only `P.profile` to `perceptual-qualifiers/unknown`. A fourth case copies `P` into `normalized_proposal.perceptual_qualifiers` as well. These remain synthetic candidate link specimens, without resolved evidence or complete claim qualifications.\n+\n+For normalization, initialize an in-memory `AppState` with a fixed clock at `2026-09-19T09:00:00Z`, and admit only the fixture's target Task at version 1: title and description `Synthetic carrier probe.`, status `active`, preconditions `N.existing_precondition`, and ordinary user provenance. Author no UniverseState; the host supplies its default current view. Supply `precondition_review::Context` with `force: false`, one matching `ReviewTask` (the same description/tree, `existing_words` from `words`, null prior rejection reason), and empty `targets`. Obtain `submission(state, context, \"synthetic-carrier-probe\")`; construct an `ok` `LocalAdvisoryResult` carrying one specimen and the submission's authority/provider/policy/digest fields, with empty artifacts/diagnostics/telemetry and no deletion confirmation. Call the actual `precondition_review::vet_result` directly, without a producer/model call or enqueue/admission.\n+\n+| Probe/path | Actual outcome and exact comparison |\n+|---|---|\n+| Canonical schema validation | Baseline, inline and unknown-profile cases accepted; the fourth case rejected by the closed native `normalized_proposal`. A structured `field_provenance.preconditions = P` control also rejected. `tools/validate-fixtures` reported three valid and two invalid fixtures with their expected outcomes. |\n+| Core Rust/JSON | All four cases deserialized, validated and retained the whole structured candidate through serialize/deserialize/reserialize. The fourth case's core acceptance does not override its schema rejection. |\n+| Baseline through `precondition_review::vet_result` | Status `ok`, one candidate, empty diagnostics. The host recomputed `blocked_now` to `true`, set `payload.value = normalized_proposal`, and set `suppression_key` to `review:sha256:57ea73a474832a19855231c2d99b66a4ed30b1eeccc4721fe20933e462ebee96`. Other structured fields were unchanged. |\n+| Inline and unknown-profile cases through the same function | The same native result as the baseline: `payload.value` is exactly `N` with `blocked_now: true`; the `perceptual_qualifiers` member is absent, rather than null or preserved. Status remains `ok`, one candidate, empty diagnostics. The expected retained member was exactly `P` (or its unknown-profile variant). The first observed loss on this tested route is `src/services/precondition_review.rs::vet_result` assigning `CandidatePayload::Inline(c.normalized_proposal.clone())`. |\n+| Fourth case through the same function | Status `malformed_result`, zero candidates, diagnostic code `advisory_malformed_result` and message `The model response was not a valid precondition review; no candidates were enqueued`. Adding a companion member to the closed native envelope is not a compliant workaround. |\n+| Direct store/reload control, bypassing normalization | Store the inline case with `store_advisory_candidate` in a temporary SQLite file using one ordinary user envelope. Close the pool, reconnect with `UbuStore::connect`, read via `get_advisory_candidate` and `CandidateRecord::candidate`, then retry the identical envelope/candidate. Each retrieved structured candidate equals the complete input, including `P`; one candidate and one mutation-ledger row, zero canonical objects and Logs. Changed payload with the same envelope key rejects with `idempotency_key_conflict`. This is connection close/reopen and replay, not process/crash restart. |\n+| Direct review API controls, bypassing normalization | Store the same inline case in the in-memory state; call `api::advisory::candidate` and `api::advisory::queue`. Both return the complete input candidate unchanged with `state_category: \"candidate_state\"`. These are handler calls, not authenticated HTTP or UI presentation tests. |\n+\n+All normalization cases left the mutation ledger and every non-candidate table unchanged; the direct store/API controls also left canonical tables unchanged. The measured loss is specific to the tested normalization branch and mapping. The inline carrier remains schema-valid and survives the direct storage controls, so this does not establish a host-contract restriction that no compliant existing payload/ref mapping can satisfy. A future adapter using this branch must preserve the companion content and bound refs, or keep the interpretation unresolved before executable review; a native `ok` result cannot certify fidelity.\n+\n+Complete Discovery claims/qualifications and resolved evidence revisions, required adapter profile/ref rejection, rendered review, admission, process/crash recovery, retention/partial acceptance and policy-authorized exchange remain untested. No complete mapping, passing adapter or actual schema gap is established; `UBU-Q0166` stays Open. Only a separately reproduced governing host-contract gap warrants the versioned chain review specified by `UBU-D0321`.\n+\n **Supported operations.** Validate the native proposal against an already-supported candidate kind and ordinary admission operation before enqueueing an executable review proposal. Unresolved targets, unsupported object kinds and perceptual values without a supported adapter remain Discovery review evidence with the limitation visible; they cannot fall through to a generic mutation executor. `UniverseTarget` advice remains name-only under `UBU-D0296`: it cannot carry, default or admit a perceptual value. The operator may author a value through the ordinary assertion/measurement form linked to the evidence. Future perceptual value-candidate support requires its separately reviewed versioned adapter; this contract does not enable it. Intent, affect, Preferences, completion/effect claims and later-phase Resources/Skills/Techniques retain their existing source and admission gates.\n \n **Disclosure and session scope.** Evidence, candidates, alternatives, selectors, hashes and derivative review material inherit all supporting sources' disclosure/routing/retention restrictions; reference-only payloads do not waive them. Denied matches cannot leak identifiers, labels or existence through alternatives or counts. Reviewing permitted retained evidence is allowed after its capture session is paused/ended; it neither restarts capture nor transfers source consent or pending approval to another Device.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\nindex fe68eec..ba46c47 100644\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3178,29 +3178,31 @@ Resolved by `UBU-D0320`: DESIGN.md \u00a721.1.1 defines the `observation-admission/1\n \n ## UBU-Q0166: Preserve perceptual qualifiers alongside the closed provenance enum\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-13 Scored from commit: None\n \n ### Question\n \n Which revision-pinned host mappings preserve the claim-level qualifications specified by `UBU-D0321` through validation, serialization, storage, review and authorized exchange, and does a failed mapping demonstrate a schema gap requiring separate versioned review?\n \n ### Current direction\n \n Delta \u00a717.4. `UBU-D0293` decides kind mapping; `UBU-D0320` decides observation/admission bindings; `UBU-D0321` specifies claim-level qualifier content, field/evidence links and fidelity fixtures. The remaining work is a revision-pinned host field/payload mapping and round-trip evidence from the actual validators, serializers, store/review and exchange paths. Measured remains an instrument/reading, derived a computation from facts, and proposed an unconfirmed candidate. Do not infer host compatibility from prose payload slots. A demonstrated gap requires separate versioned chain review; no schema, pin or migration change is made here. `UBU-D0323` records bounded candidate link-carrier checks and source-inspected downstream limitations. Full claims/qualifiers and resolved evidence revisions still need fixtures through the actual Discovery, normalization, store/review, admission and authorized-exchange paths. A rejected direct field mapping alone does not establish a schema gap.\n \n+`UBU-D0334` extends those probes to actual persistence/reload/replay and direct review API reads, with a minimal runtime reproduction of inline qualifier loss in `precondition_review::vet_result`. The rejected normalized-proposal alternative and this implementation loss do not establish that all compliant existing payload/ref mappings fail. Repair or validate that branch before relying on it; the full qualifier/evidence and policy-authorized paths remain the enablement gate.\n+\n ### Resolution\n \n-Open. `UBU-D0321` answers preservation requirements; `UBU-D0323` records only a synthetic candidate link-carrier round trip, rejection by string-valued field_provenance and unknown-profile acceptance by generic host layers. Complete claim/qualifier mappings, required adapter validation and downstream fixture outcomes remain outstanding. Close only with the full pinned mapping and fixture evidence, or a minimally reproduced host-contract gap routed to a separate versioned change. No passing adapter or actual schema gap is established.\n+Open. `UBU-D0321` answers preservation requirements; `UBU-D0323` records the initial synthetic carrier probes. `UBU-D0334` adds actual direct SQLite close/reopen/replay and review API controls, and reproduces qualifier loss in the tested precondition-review normalization branch despite a native `ok` result. This disqualifies that mapping through that branch, without establishing a governing schema gap. Complete Discovery claim/qualifier mappings, adapter profile/ref validation, faithful normalization and rendered review, admission and authorized-exchange fixtures remain outstanding. Close only with the full pinned mapping and fixture evidence, or a minimally reproduced host-contract gap routed to a separate versioned change. No passing adapter or actual schema gap is established.\n \n ---\n \n ## UBU-Q0167: Consequence-sensitive corroboration before planning influence\n \n Status: Solved Priority: MVP important Phase: Phase 2 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: UBU-D0324 Last scored: 2026-10-12 Scored from commit: None\n \n ### Question\n \n How should consequence and uncertainty determine corroboration and human review before a visual interpretation can influence planning or execution?\n \n ### Current direction\n \n",
    "commit_message": "docs: record pinned qualifier normalization failure and persistence controls",
    "validation_notes": [
      "Generated the diff from unmodified current working-tree copies and proposed copies; git apply --check passed against /home/sean/ubu-design without applying the patch.",
      "Only DECISIONS.md, DESIGN.md and the UBU-Q0166 block in OPEN_QUESTIONS.md change. All question metadata lines, including existing ranking and scoring fields, remain unchanged; UBU-Q0166 stays Open with Resolved by: None.",
      "Confirmed clean pinned schema/core/store/orchestrator/UI checkouts. Offline locked orchestrator-library and canonical-validator builds passed; the harness used actual compiled host functions and the orchestrator lockfile versions recorded in the proposal.",
      "Canonical validator: three accepted specimens and two expected rejections. Actual core Rust/JSON: all four normalization specimens structurally preserved before host normalization.",
      "Actual vet_result: baseline, inline and unknown-profile specimens returned ok; the latter two lost the companion member. Adding P to normalized_proposal returned malformed_result with no candidates. Normalization changed no mutation-ledger or non-candidate table rows.",
      "Direct SQLite storage, connection close/reopen and identical replay preserved the whole inline candidate; one candidate/ledger row, zero canonical objects/Logs. Changed-content key reuse rejected. Direct candidate and queue API handlers preserved the same input candidate.",
      "No repository source or user data was changed. Probes used temporary synthetic data with unresolved refs; no Discovery resolution, full claim qualifiers, rendered review, HTTP authorization, admission, process/crash recovery, policy enforcement or authorized exchange result is claimed."
    ],
    "new_questions_added": [],
    "questions_resolved": [],
    "decisions_added": [
      "UBU-D0334"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "openai-ubu-q0166-d0334-normalization-evidence",
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
