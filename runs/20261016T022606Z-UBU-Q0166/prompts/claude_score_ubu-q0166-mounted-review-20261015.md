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
Base commit: `c33836bfc4141a45bee394c3f344b8ee9a63c469`

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

### Resolution

Open. `UBU-D0321` answers preservation requirements; `UBU-D0323` records the initial synthetic carrier probes. `UBU-D0334` adds actual direct SQLite close/reopen/replay and review API controls, and reproduces qualifier loss in the tested precondition-review normalization branch despite a native `ok` result. This disqualifies that mapping through that branch, without establishing a governing schema gap. `UBU-D0360` additionally records retention of opaque reference strings through normalization and, for the first specimen, direct persistence/API controls. The alternate token remains unresolved; no manifest or qualifier content is resolved. Complete Discovery claim/qualifier mappings, adapter profile/ref validation, faithful normalization and rendered review, admission and authorized-exchange fixtures remain outstanding. Close only with the full pinned mapping and fixture evidence, or a minimally reproduced host-contract gap routed to a separate versioned change. No passing adapter or actual schema gap is established.

`UBU-D0366` adds actual controller enqueue/native-admission controls and native stale-value/old-candidate-version rejection. It demonstrates no resolved manifest or qualifier-preserving admission, and makes no repair or schema-gap claim. The full existing enablement gate remains outstanding; this question stays Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "ubu-q0166-mounted-review-20261015",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0166",
    "base_commit": "c33836bfc4141a45bee394c3f344b8ee9a63c469",
    "summary": "Record pinned mounted-review controls showing invisible inline qualifier links and literal-only reference presentation; keep UBU-Q0166 Open.",
    "rationale": "The actual Review component omits retained inline companions independently of normalization loss. Bounded DOM and click evidence narrows the presentation gap without claiming complete qualifier preservation, actual admission or a governing schema gap.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5786,3 +5786,15 @@ Bind interpretation and dispatch to the sealed revision. Ignore identical duplic\n This supplies an input-finalization contract, not measured recognition quality, input-owner authentication or driving-safety evidence. Both v1 schemas, action permissions and MVP-release scope remain unchanged. `UBU-Q0128` stays Open for numerical profiles, measured/platform qualification, richer actions/integrations, remaining device behavior and naming.\n \n ---\n+\n+## UBU-D0373: Mounted review controls expose a separate qualifier-presentation limitation\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a721.1.1. Narrows `UBU-Q0166`; extends `UBU-D0323`'s UI source inspection with bounded mounted-component evidence.\n+\n+Record the pinned JSDOM controls in DESIGN.md \u00a721.1.1. Supplying retained known or unknown inline companion specimens directly to the actual Review component produces the same article DOM as the baseline. Opaque reference specimens appear only as literal evidence-ref strings. A changed native reason is visibly rendered, while all six controls leave Admit enabled and invoke the mocked client method with the candidate id/version.\n+\n+This demonstrates a presentation limitation even when normalization is bypassed. The mocked API/client boundary exercises no authenticated HTTP, actual admission, evidence resolution or disclosure enforcement; it cannot combine with earlier native-service probes into an end-to-end result. Full claims/qualifications are not supplied, and no mapping, UI repair or schema gap is established.\n+\n+Require the existing resolved-content, faithful-review and admission gates before enabling a perceptual adapter. `UBU-Q0166` remains Open; `UBU-D0334`'s inline normalization loss remains unrepaired. No schema, enum, FactProvenance field, pin, migration or producer authority changes here.\n+\n+---\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -3613,6 +3613,19 @@ Enqueue changes only candidate storage and its single ledger row, leaving every\n \n This follow-up demonstrates a native-service enforcement limitation, not failure of every compliant existing payload/ref mapping or a governing schema gap. Full Discovery claims/qualifications, evidence resolution, policy-bound adapter validation, rendered review, qualifier-aware admission links, process/crash recovery, corrections/partial acceptance/retention and authorized exchange remain outstanding. No producer/adapter or policy-authorized exchange configuration is qualified; `UBU-Q0166` stays Open and `UBU-D0334`'s inline loss remains unrepaired.\n \n+**Mounted review follow-up (`UBU-D0373`; 2026-10-15).** Exercised `ubu-ui@dda2312fd17a5dc330fef102f9def30ab88ac5bf`, with tracked files clean before and after the probe. An in-memory harness transpiled the actual `src/routes/Review.tsx` and imported presentation components to CommonJS and mounted them with real React hooks in JSDOM. Runtime: Node `v24.4.1`, React/React DOM `19.2.7`, TypeScript `5.9.3`, JSDOM `26.1.0`; installed versions matched the UI lockfile (SHA-256 `f17aa89a8b2770b7c28e1aa38fe5a9a1b71c938f58a6bd87cb3666fd448d330f`). The `../api/client` module was mocked; no real transport or controller service ran.\n+\n+**Inputs and boundary.** Use `UBU-D0334`'s exact baseline input, `N` and `P`, before normalization: confidence absent, both native proposal and inline payload initially `N`. Separately mount the baseline, inline `P`, unknown-profile inline `P`, and both exact `UBU-D0360` reference specimens. A sixth positive control changes only `reason` in both native proposal and inline payload to `Synthetic visible reason control.`. For each fresh mount, fix `Date.now()` at `2026-09-19T09:00:00Z`; mock `advisoryQueue` with that single candidate, empty deferred candidates, target title `Synthetic carrier probe.` and no `review_intervals`; mock `listTasks(\"active\")` with an empty task list. Flush effects with `act`, compare the proposal article's `outerHTML` and `textContent`, then click its Admit button and record mocked client calls. No manifest or evidence content is supplied or resolved.\n+\n+| Mounted control | Observed rendering and click outcome |\n+|---|---|\n+| Baseline, known inline profile and unknown inline profile | All three articles have identical complete DOM, including `Evidence refs: synthetic:description`, with no companion profile or field/claim mapping rendered. Article `outerHTML` SHA-256 is `00c007629815bd0e8df5270fc2b85326f140673d17ff82602507142e5f8fadfa` in all three cases. Keeping the inline member in the mocked queue response therefore does not repair this presentation path. |\n+| Each unresolved reference token | Text equals the baseline except for appending `, synthetic:qualifier-manifest@v1` or `, synthetic:qualifier-manifest@missing` to the evidence-ref line. Neither token produces resolved content or a missing-reference diagnostic. |\n+| Changed-reason positive control | The article renders `Model's reason: Synthetic visible reason control.`; its DOM differs from the baseline. This checks that native proposal content reaches the mounted card. |\n+| All six controls | Admit is enabled. One click invokes the mocked method exactly once as `admitAdvisory(\"advcand_018f3c8e9b2a7c4d8f1e2a3b4c5d6e7f\", 1)`, followed by queue/task reload. Before the click the only mocked calls are `advisoryQueue()` and `listTasks(\"active\")`. This is a client-method invocation, not an HTTP request or successful native admission. |\n+\n+This upgrades only the generic precondition-review card's source-inspected presentation limitation to a mounted DOM/click result. It is independent of the earlier normalization loss because the mocked queue supplies the retained inline member directly. The specimens remain candidate link-carrier controls, without full claims, qualifications or resolved revisions. JSDOM supplies no browser-layout, assistive-technology, authenticated transport, policy, persistence or end-to-end evidence. No new schema/core validator or Rust build ran, and no UI or normalization repair was made. API retention and an enabled Admit control cannot qualify perceptual review. A future adapter still needs faithful permitted claim/qualifier presentation and binding checks through actual review/admission and authorized exchange under the existing gate. `UBU-Q0166` stays Open; no passing mapping or governing schema gap is established.\n+\n **Supported operations.** Validate the native proposal against an already-supported candidate kind and ordinary admission operation before enqueueing an executable review proposal. Unresolved targets, unsupported object kinds and perceptual values without a supported adapter remain Discovery review evidence with the limitation visible; they cannot fall through to a generic mutation executor. `UniverseTarget` advice remains name-only under `UBU-D0296`: it cannot carry, default or admit a perceptual value. The operator may author a value through the ordinary assertion/measurement form linked to the evidence. Future perceptual value-candidate support requires its separately reviewed versioned adapter; this contract does not enable it. Intent, affect, Preferences, completion/effect claims and later-phase Resources/Skills/Techniques retain their existing source and admission gates.\n \n **Disclosure and session scope.** Evidence, candidates, alternatives, selectors, hashes and derivative review material inherit all supporting sources' disclosure/routing/retention restrictions; reference-only payloads do not waive them. Denied matches cannot leak identifiers, labels or existence through alternatives or counts. Reviewing permitted retained evidence is allowed after its capture session is paused/ended; it neither restarts capture nor transfers source consent or pending approval to another Device.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3239,25 +3239,29 @@\n ## UBU-Q0166: Preserve perceptual qualifiers alongside the closed provenance enum\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-15 Scored from commit: None\n \n ### Question\n \n Which revision-pinned host mappings preserve the claim-level qualifications specified by `UBU-D0321` through validation, serialization, storage, review and authorized exchange, and does a failed mapping demonstrate a schema gap requiring separate versioned review?\n \n ### Current direction\n \n Delta \u00a717.4. `UBU-D0293` decides kind mapping; `UBU-D0320` decides observation/admission bindings; `UBU-D0321` specifies claim-level qualifier content, field/evidence links and fidelity fixtures. The remaining work is a revision-pinned host field/payload mapping and round-trip evidence from the actual validators, serializers, store/review and exchange paths. Measured remains an instrument/reading, derived a computation from facts, and proposed an unconfirmed candidate. Do not infer host compatibility from prose payload slots. A demonstrated gap requires separate versioned chain review; no schema, pin or migration change is made here. `UBU-D0323` records bounded candidate link-carrier checks and source-inspected downstream limitations. Full claims/qualifiers and resolved evidence revisions still need fixtures through the actual Discovery, normalization, store/review, admission and authorized-exchange paths. A rejected direct field mapping alone does not establish a schema gap.\n \n `UBU-D0334` extends those probes to actual persistence/reload/replay and direct review API reads, with a minimal runtime reproduction of inline qualifier loss in `precondition_review::vet_result`. The rejected normalized-proposal alternative and this implementation loss do not establish that all compliant existing payload/ref mappings fail. Repair or validate that branch before relying on it; the full qualifier/evidence and policy-authorized paths remain the enablement gate.\n \n `UBU-D0360` adds opaque string-reference controls that survive core round trips and the tested normalization branch. The first token's normalized output also survives direct storage/replay/API reads. The unresolved alternate token also receives native `ok`; no manifest or evidence revision is resolved. Treat these as carrier evidence only, not an approved field-provenance mapping or qualifier-preservation result. The inline loss remains unrepaired.\n \n `UBU-D0366` exercises actual native enqueue and admission services. Both unresolved tokens can accompany an admitted synthetic precondition removal; known and unknown inline profiles are discarded before enqueue and their native removals also admit. The changed-precondition control rejects without an effect. These controls establish native behavior, not qualifier-aware admission: resolved content, profile/ref/policy validation and complete reviewed evidence links remain required.\n \n+`UBU-D0373` adds mounted Review component controls with a mocked API/client boundary. Retained known and unknown inline specimens yield identical article DOM to the baseline; opaque refs appear only as strings, and Admit invokes the mock in every control. This establishes a separate presentation limitation, without full qualifier content, actual admission or an end-to-end mapping.\n+\n ### Resolution\n \n Open. `UBU-D0321` answers preservation requirements; `UBU-D0323` records the initial synthetic carrier probes. `UBU-D0334` adds actual direct SQLite close/reopen/replay and review API controls, and reproduces qualifier loss in the tested precondition-review normalization branch despite a native `ok` result. This disqualifies that mapping through that branch, without establishing a governing schema gap. `UBU-D0360` additionally records retention of opaque reference strings through normalization and, for the first specimen, direct persistence/API controls. The alternate token remains unresolved; no manifest or qualifier content is resolved. Complete Discovery claim/qualifier mappings, adapter profile/ref validation, faithful normalization and rendered review, admission and authorized-exchange fixtures remain outstanding. Close only with the full pinned mapping and fixture evidence, or a minimally reproduced host-contract gap routed to a separate versioned change. No passing adapter or actual schema gap is established.\n \n `UBU-D0366` adds actual controller enqueue/native-admission controls and native stale-value/old-candidate-version rejection. It demonstrates no resolved manifest or qualifier-preserving admission, and makes no repair or schema-gap claim. The full existing enablement gate remains outstanding; this question stays Open.\n \n+`UBU-D0373` upgrades the generic precondition-review card's UI source inspection to bounded mounted DOM/click evidence. It makes no repair or full-preservation claim; resolved qualifier presentation, actual review/admission and authorized exchange remain outstanding. This question stays Open.\n+\n ---\n",
    "commit_message": "docs: record mounted perceptual qualifier review controls",
    "validation_notes": [
      "git apply --check passed against the unmodified current working tree; the patch adds 29 lines across three allowed files.",
      "Six JSDOM mounts exercised the actual Review component and presentation components with mocked API/client calls; DOM comparisons, native-reason positive control and Admit-call assertions passed.",
      "UI revision and clean tracked files verified; installed React, React DOM, TypeScript and JSDOM versions matched the hashed lockfile.",
      "Q0166 metadata remains one line with Status: Open and Resolved by: None; no neighboring question was edited.",
      "These controls exercise no authenticated HTTP, actual native admission, resolved claim/evidence revisions or authorized exchange; no fresh canonical validator or Rust build ran."
    ],
    "new_questions_added": [],
    "questions_resolved": [],
    "decisions_added": [
      "UBU-D0373"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "ubu-q0166-mounted-review-20261015",
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
