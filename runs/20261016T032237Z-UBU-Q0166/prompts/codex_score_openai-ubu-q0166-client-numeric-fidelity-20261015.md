# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0166`  
Question title: `Preserve perceptual qualifiers alongside the closed provenance enum`  
Base commit: `3d271b324ecf23946dd559281bc3059eceda6e9b`

```markdown
## UBU-Q0166: Preserve perceptual qualifiers alongside the closed provenance enum

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-15 Scored from commit: None

### Question

Which revision-pinned host mappings preserve the claim-level qualifications specified by `UBU-D0321` through validation, serialization, storage, review and authorized exchange, and does a failed mapping demonstrate a schema gap requiring separate versioned review?

### Current direction

Delta §17.4. `UBU-D0293` decides kind mapping; `UBU-D0320` decides observation/admission bindings; `UBU-D0321` specifies claim-level qualifier content, field/evidence links and fidelity fixtures. The remaining work is a revision-pinned host field/payload mapping and round-trip evidence from the actual validators, serializers, store/review and exchange paths. Measured remains an instrument/reading, derived a computation from facts, and proposed an unconfirmed candidate. Do not infer host compatibility from prose payload slots. A demonstrated gap requires separate versioned chain review; no schema, pin or migration change is made here. `UBU-D0323` records bounded candidate link-carrier checks and source-inspected downstream limitations. Full claims/qualifiers and resolved evidence revisions still need fixtures through the actual Discovery, normalization, store/review, admission and authorized-exchange paths. A rejected direct field mapping alone does not establish a schema gap.

`UBU-D0334` extends those probes to actual persistence/reload/replay and direct review API reads, with a minimal runtime reproduction of inline qualifier loss in `precondition_review::vet_result`. The rejected normalized-proposal alternative and this implementation loss do not establish that all compliant existing payload/ref mappings fail. Repair or validate that branch before relying on it; the full qualifier/evidence and policy-authorized paths remain the enablement gate.

`UBU-D0360` adds opaque string-reference controls that survive core round trips and the tested normalization branch. The first token's normalized output also survives direct storage/replay/API reads. The unresolved alternate token also receives native `ok`; no manifest or evidence revision is resolved. Treat these as carrier evidence only, not an approved field-provenance mapping or qualifier-preservation result. The inline loss remains unrepaired.

`UBU-D0366` exercises actual native enqueue and admission services. Both unresolved tokens can accompany an admitted synthetic precondition removal; known and unknown inline profiles are discarded before enqueue and their native removals also admit. The changed-precondition control rejects without an effect. These controls establish native behavior, not qualifier-aware admission: resolved content, profile/ref/policy validation and complete reviewed evidence links remain required.

`UBU-D0373` adds mounted Review component controls with a mocked API/client boundary. Retained known and unknown inline specimens yield identical article DOM to the baseline; opaque refs appear only as strings, and Admit invokes the mock in every control. This establishes a separate presentation limitation, without full qualifier content, actual admission or an end-to-end mapping.

### Resolution

Open. `UBU-D0321` answers preservation requirements; `UBU-D0323` records the initial synthetic carrier probes. `UBU-D0334` adds actual direct SQLite close/reopen/replay and review API controls, and reproduces qualifier loss in the tested precondition-review normalization branch despite a native `ok` result. This disqualifies that mapping through that branch, without establishing a governing schema gap. `UBU-D0360` additionally records retention of opaque reference strings through normalization and, for the first specimen, direct persistence/API controls. The alternate token remains unresolved; no manifest or qualifier content is resolved. Complete Discovery claim/qualifier mappings, adapter profile/ref validation, faithful normalization and rendered review, admission and authorized-exchange fixtures remain outstanding. Close only with the full pinned mapping and fixture evidence, or a minimally reproduced host-contract gap routed to a separate versioned change. No passing adapter or actual schema gap is established.

`UBU-D0366` adds actual controller enqueue/native-admission controls and native stale-value/old-candidate-version rejection. It demonstrates no resolved manifest or qualifier-preserving admission, and makes no repair or schema-gap claim. The full existing enablement gate remains outstanding; this question stays Open.

`UBU-D0373` upgrades the generic precondition-review card's UI source inspection to bounded mounted DOM/click evidence. It makes no repair or full-preservation claim; resolved qualifier presentation, actual review/admission and authorized exchange remain outstanding. This question stays Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "openai-ubu-q0166-client-numeric-fidelity-20261015",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0166",
    "base_commit": "3d271b324ecf23946dd559281bc3059eceda6e9b",
    "summary": "Record pinned client JSON precision loss and require independent exact-value fidelity checks; keep UBU-Q0166 open.",
    "rationale": "Actual client/plugin JavaScript probes preserve raw response text but round an exact synthetic quantity without an error. Comparing both sides through the same decoder masks the loss. This adds concrete host-path evidence while establishing neither a complete qualifier mapping nor a governing schema gap.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\nindex a395b0c..41d1042 100644\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5858,3 +5858,15 @@ Allow at most one terminal speech attempt per original turn. Preflight the compl\n Link outcome evidence, template and output disposition through existing minimized audit records; completed output proves neither hearing nor comprehension. Require \u00a721.1.2's storage-failure, unknown-effect, duplicate, interruption and rendering-boundary fixtures, plus the existing comprehension trials. This specifies rendering behavior, not implementation, measured usability or a driving-safety result. Both v1 schemas and MVP-release scope remain unchanged; `UBU-Q0128` stays Open for measured profiles and its other remaining slices.\n \n ---\n+\n+## UBU-D0379: Pinned client probes expose numeric precision loss during JSON decoding\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a721.1.1. Narrows `UBU-Q0166`; adds bounded JavaScript client evidence alongside `UBU-D0373`.\n+\n+Record the pinned client/plugin probes in DESIGN.md \u00a721.1.1. With native Tauri IPC mocked, the actual JavaScript HTTP plugin retains the supplied response text, while the actual advisory queue client decodes the synthetic integer `9007199254740993` as `9007199254740992` with status 200. Nearby integer, quoted-string and null controls retain their supplied values. Comparing both sides after the same JavaScript decoder falsely passes the changed-integer case.\n+\n+Include client decoding and numeric domains/encodings in the existing host-mapping gate. Compare exact source values using an oracle that does not first apply the decoder under test; preserve quantity meaning and units, not merely matching rounded outputs. A future adapter must demonstrate a lossless permitted representation through every required path or leave unsupported content unresolved before executable review/admission. The quoted-string control changes JSON type and approves neither an encoding nor native field coercion.\n+\n+These are numeric carrier controls with incomplete qualifications and unresolved evidence, not a complete companion mapping, deployed WebView result or admission/exchange test. No repair or governing schema gap is established; `UBU-Q0166` remains Open. `UBU-D0334`'s normalization loss and `UBU-D0373`'s presentation limitation remain. No schema, enum, FactProvenance field, pin, migration or producer authority changes here.\n+\n+---\ndiff --git a/DESIGN.md b/DESIGN.md\nindex 9a509d7..392307e 100644\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -3644,6 +3644,26 @@ This follow-up demonstrates a native-service enforcement limitation, not failure\n \n This upgrades only the generic precondition-review card's source-inspected presentation limitation to a mounted DOM/click result. It is independent of the earlier normalization loss because the mocked queue supplies the retained inline member directly. The specimens remain candidate link-carrier controls, without full claims, qualifications or resolved revisions. JSDOM supplies no browser-layout, assistive-technology, authenticated transport, policy, persistence or end-to-end evidence. No new schema/core validator or Rust build ran, and no UI or normalization repair was made. API retention and an enabled Admit control cannot qualify perceptual review. A future adapter still needs faithful permitted claim/qualifier presentation and binding checks through actual review/admission and authorized exchange under the existing gate. `UBU-Q0166` stays Open; no passing mapping or governing schema gap is established.\n \n+**Client numeric fidelity follow-up (`UBU-D0379`; 2026-10-15).** Exercised `ubu-ui@dda2312fd17a5dc330fef102f9def30ab88ac5bf`, with tracked files clean. An in-memory harness transpiled the actual `src/api/client.ts` and `src/api/endpoints.ts` as ES modules using TypeScript `5.9.3`, initialized `import.meta.env` to an empty object and evaluated the installed `@tauri-apps/plugin-http@2.6.1` JavaScript implementation. Node `v24.4.1` supplied Request/Response/stream primitives; only `@tauri-apps/api/core`'s native IPC and shell check were mocked. The UI lockfile SHA-256 remains `f17aa89a8b2770b7c28e1aa38fe5a9a1b71c938f58a6bd87cb3666fd448d330f`; the exercised plugin's `dist-js/index.js` SHA-256 is `ace2c179bb8db0e1d2c10d9970304d10c65a49908a5bf94402e538627d181f4c`. No native Tauri backend, deployed WebView or HTTP server ran.\n+\n+**Exact numeric carrier specimen.** Reuse `UBU-D0334`'s baseline candidate `C`, native `N` and inline `P`, before normalization: confidence absent, native proposal and inline payload initially `N`, with `payload.value.perceptual_qualifiers = P`. Extend only `P` with `claims: [{\"claim_key\":\"charge\",\"content\":{\"quantity\":Q,\"unit\":\"synthetic_count\"},\"qualifications\":[]}]`. Here `Q` is each exact JSON token below, inserted into the wire text before any JavaScript numeric decoding. Empty qualifications and unresolved synthetic refs make this a numeric carrier extension, not an approved claim placement or complete `perceptual-qualifiers/1` fixture. Supply the queue body `{\"state_category\":\"candidate_state\",\"candidates\":[{\"state_category\":\"candidate_state\",\"candidate\":C}],\"deferred_candidates\":[],\"target_titles\":{},\"review_intervals\":{}}`.\n+\n+For each specimen, mock native IPC `plugin:http|fetch` with a request resource id, `fetch_send` with status 200/JSON headers and `fetch_read_body` with the exact UTF-8 body plus the plugin's zero continuation byte, then its one-byte end marker. Use separate identical responses for the actual plugin's `fetch(...).text()` control and actual `orchestratorClient.advisoryQueue()`. Both issue GET `http://127.0.0.1:7878/advisory/queue`. Compare raw response text with supplied text, all other structured content with the baseline, and integer values with independently constructed `BigInt` expectations from the original tokens.\n+\n+| Wire `Q` token | Retrieved client quantity after decoding/reserialization | Exact value outcome |\n+|---|---|---|\n+| `9007199254740991` | Number `9007199254740991` | Preserved. |\n+| `9007199254740992` | Number `9007199254740992` | Preserved; this representable value alone cannot qualify neighboring integers. |\n+| `9007199254740993` | Number `9007199254740992` | Changed by one, despite status 200. |\n+| `\"9007199254740993\"` | String `\"9007199254740993\"` | String preserved; no numeric interpretation is established. |\n+| `null` | `null` | Preserved, without defaulting to zero. |\n+\n+All five raw-text controls preserve the complete supplied body; all client calls return status 200 and preserve every other structured value. The first observed numeric loss on this tested boundary is `src/api/client.ts::request` consuming `await response.json()`. The comparison `JSON.stringify(JSON.parse(wire)) === JSON.stringify(result.data)` nevertheless passes all five cases, including the changed integer: the same decoder rounds the expected input too. The independent integer comparison rejects that case. This tests exact numeric value, not incidental JSON whitespace or number formatting. Run-only harness source SHA-256 is `35e1e13bff733587bc5a869e32208ba1d417a8f91420c7d6652938b8fffcdb44`; compact results JSON SHA-256 is `9818c1e18f4d3b48e642971df988f3b6685a53ebd3c8d831d9b6642149c4b4fc`. No fixture files or host changes were committed.\n+\n+Extend the existing fidelity comparisons to the pinned client decoder and declared numeric domains/encodings, with exact expected values independent of a potentially lossy decoder. An adapter must preserve permitted quantity meaning, units and source distinctions through every required host path or keep unsupported content unresolved before executable review/admission. A string control cannot authorize silently changing numeric types in evidence or native proposals, and formatting an already-rounded value cannot recover its source. Any alternate encoding requires its own validated field/payload/ref mapping and complete round-trip evidence.\n+\n+These probes ran no canonical schema/core validator, Rust build, Discovery/evidence resolver, normalization, persistence, mounted review, actual admission or authorized exchange. They establish a bounded client-decoding limitation, not failure of every compliant existing representation or a governing schema gap. No mapping, codec or adapter is qualified, and earlier normalization/presentation failures remain unrepaired. `UBU-Q0166` stays Open under the full existing host-mapping gate.\n+\n **Supported operations.** Validate the native proposal against an already-supported candidate kind and ordinary admission operation before enqueueing an executable review proposal. Unresolved targets, unsupported object kinds and perceptual values without a supported adapter remain Discovery review evidence with the limitation visible; they cannot fall through to a generic mutation executor. `UniverseTarget` advice remains name-only under `UBU-D0296`: it cannot carry, default or admit a perceptual value. The operator may author a value through the ordinary assertion/measurement form linked to the evidence. Future perceptual value-candidate support requires its separately reviewed versioned adapter; this contract does not enable it. Intent, affect, Preferences, completion/effect claims and later-phase Resources/Skills/Techniques retain their existing source and admission gates.\n \n **Disclosure and session scope.** Evidence, candidates, alternatives, selectors, hashes and derivative review material inherit all supporting sources' disclosure/routing/retention restrictions; reference-only payloads do not waive them. Denied matches cannot leak identifiers, labels or existence through alternatives or counts. Reviewing permitted retained evidence is allowed after its capture session is paused/ended; it neither restarts capture nor transfers source consent or pending approval to another Device.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\nindex 99ffd94..b034f10 100644\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3251,31 +3251,35 @@ ## UBU-Q0166: Preserve perceptual qualifiers alongside the closed provenance enum\n ## UBU-Q0166: Preserve perceptual qualifiers alongside the closed provenance enum\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-15 Scored from commit: None\n \n ### Question\n \n Which revision-pinned host mappings preserve the claim-level qualifications specified by `UBU-D0321` through validation, serialization, storage, review and authorized exchange, and does a failed mapping demonstrate a schema gap requiring separate versioned review?\n \n ### Current direction\n \n Delta \u00a717.4. `UBU-D0293` decides kind mapping; `UBU-D0320` decides observation/admission bindings; `UBU-D0321` specifies claim-level qualifier content, field/evidence links and fidelity fixtures. The remaining work is a revision-pinned host field/payload mapping and round-trip evidence from the actual validators, serializers, store/review and exchange paths. Measured remains an instrument/reading, derived a computation from facts, and proposed an unconfirmed candidate. Do not infer host compatibility from prose payload slots. A demonstrated gap requires separate versioned chain review; no schema, pin or migration change is made here. `UBU-D0323` records bounded candidate link-carrier checks and source-inspected downstream limitations. Full claims/qualifiers and resolved evidence revisions still need fixtures through the actual Discovery, normalization, store/review, admission and authorized-exchange paths. A rejected direct field mapping alone does not establish a schema gap.\n \n `UBU-D0334` extends those probes to actual persistence/reload/replay and direct review API reads, with a minimal runtime reproduction of inline qualifier loss in `precondition_review::vet_result`. The rejected normalized-proposal alternative and this implementation loss do not establish that all compliant existing payload/ref mappings fail. Repair or validate that branch before relying on it; the full qualifier/evidence and policy-authorized paths remain the enablement gate.\n \n `UBU-D0360` adds opaque string-reference controls that survive core round trips and the tested normalization branch. The first token's normalized output also survives direct storage/replay/API reads. The unresolved alternate token also receives native `ok`; no manifest or evidence revision is resolved. Treat these as carrier evidence only, not an approved field-provenance mapping or qualifier-preservation result. The inline loss remains unrepaired.\n \n `UBU-D0366` exercises actual native enqueue and admission services. Both unresolved tokens can accompany an admitted synthetic precondition removal; known and unknown inline profiles are discarded before enqueue and their native removals also admit. The changed-precondition control rejects without an effect. These controls establish native behavior, not qualifier-aware admission: resolved content, profile/ref/policy validation and complete reviewed evidence links remain required.\n \n `UBU-D0373` adds mounted Review component controls with a mocked API/client boundary. Retained known and unknown inline specimens yield identical article DOM to the baseline; opaque refs appear only as strings, and Admit invokes the mock in every control. This establishes a separate presentation limitation, without full qualifier content, actual admission or an end-to-end mapping.\n \n+`UBU-D0379` adds actual JavaScript client/plugin probes with mocked native IPC: an exact synthetic integer changes during response JSON decoding, while raw text and string controls survive. Using the same decoder for input/output comparison masks the loss. Include independent exact-value checks and the client numeric domain in the pinned mapping; no encoding or adapter is approved.\n+\n ### Resolution\n \n Open. `UBU-D0321` answers preservation requirements; `UBU-D0323` records the initial synthetic carrier probes. `UBU-D0334` adds actual direct SQLite close/reopen/replay and review API controls, and reproduces qualifier loss in the tested precondition-review normalization branch despite a native `ok` result. This disqualifies that mapping through that branch, without establishing a governing schema gap. `UBU-D0360` additionally records retention of opaque reference strings through normalization and, for the first specimen, direct persistence/API controls. The alternate token remains unresolved; no manifest or qualifier content is resolved. Complete Discovery claim/qualifier mappings, adapter profile/ref validation, faithful normalization and rendered review, admission and authorized-exchange fixtures remain outstanding. Close only with the full pinned mapping and fixture evidence, or a minimally reproduced host-contract gap routed to a separate versioned change. No passing adapter or actual schema gap is established.\n \n `UBU-D0366` adds actual controller enqueue/native-admission controls and native stale-value/old-candidate-version rejection. It demonstrates no resolved manifest or qualifier-preserving admission, and makes no repair or schema-gap claim. The full existing enablement gate remains outstanding; this question stays Open.\n \n `UBU-D0373` upgrades the generic precondition-review card's UI source inspection to bounded mounted DOM/click evidence. It makes no repair or full-preservation claim; resolved qualifier presentation, actual review/admission and authorized exchange remain outstanding. This question stays Open.\n \n+`UBU-D0379` records a separate numeric-fidelity limitation at the tested client decoder and a comparison that falsely reports preservation when both sides use that decoder. The numeric carrier controls resolve no evidence and test no deployed WebView, native admission or exchange. Exact numeric representations and full qualifier preservation remain unqualified; this question stays Open.\n+\n ---\n \n ## UBU-Q0167: Consequence-sensitive corroboration before planning influence\n",
    "commit_message": "docs: record Q0166 client numeric fidelity probes",
    "validation_notes": [
      "Executed five numeric/string/null controls through the actual pinned UI client and HTTP plugin JavaScript with mocked native IPC; raw-text controls preserved all supplied bytes.",
      "Reproduced 9007199254740993 becoming 9007199254740992 with status 200. Independent BigInt comparison detected the loss; comparing both sides after JavaScript JSON parsing falsely passed.",
      "git apply --check passed against the current working-tree files; no cached/index-only validation was used.",
      "Preserved UBU-Q0166's single-line metadata and Open status. No code, schema, dependency pin or migration changes are proposed.",
      "No native Tauri/WebView, authenticated HTTP, complete qualifier mapping, evidence resolution, actual admission or authorized exchange was exercised."
    ],
    "new_questions_added": [],
    "questions_resolved": [],
    "decisions_added": [
      "UBU-D0379"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "openai-ubu-q0166-client-numeric-fidelity-20261015",
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
