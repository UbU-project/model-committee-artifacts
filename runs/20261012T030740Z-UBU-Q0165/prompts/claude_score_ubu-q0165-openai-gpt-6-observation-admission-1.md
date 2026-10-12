# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0165`  
Question title: `Observation-to-candidate contract and admission reuse`  
Base commit: `2ee88e3dd27155ac8ea0a7323da02c2cdca8fde9`

```markdown
## UBU-Q0165: Observation-to-candidate contract and admission reuse

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-11 Scored from commit: None

### Question

What exact observation-to-candidate contract reuses existing advisory and Discovery admission machinery without introducing automatic canonical mutation?

### Current direction

Delta §17.3. Specify source/session refs, identity ambiguity, candidate object/value links, observed versions, evidence, idempotency, corrections and review presentation. The architectural pipeline is answered in DESIGN.md §21.1.1; concrete payload coverage and fixtures are unresolved. Do not invent new FactProvenance fields.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0165-openai-gpt-6-observation-admission-1",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0165",
    "base_commit": "2ee88e3dd27155ac8ea0a7323da02c2cdca8fde9",
    "summary": "Specify observation payload bindings, Discovery/advisory reuse and review-bound admission, with required acceptance fixtures.",
    "rationale": "The architectural pipeline is already decided; its concrete bindings and admission cases remain unspecified. This profile makes those boundaries auditable while preserving existing candidate lifecycles, name-only UniverseTarget advice and FactProvenance fields. Unsupported interpretations remain evidence for review, and related qualifier, corroboration, retention and effect-proof questions retain their scope.",
    "changed_files": [
      "DESIGN.md",
      "DECISIONS.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DESIGN.md b/DESIGN.md\nindex 14c09f3..0000000 100644\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -3225,12 +3225,54 @@ Routing defaults are conservative. Local-only processing is required for raw sen\n #### 21.1.1 Observation, context and closed-loop execution\n \n The architecture is observation \u2192 candidate objects/facts \u2192 Identity and object resolution against permitted persistent context \u2192 candidate UniverseState/Resource implications \u2192 proposed preconditions or constraints \u2192 CPU semantic/policy/conflict validation and operator admission \u2192 planning or recalculation over admitted state. Ambiguous identity matches remain alternatives, not merged objects. The camera can suggest the relevance of an observed object to the user's life; it cannot infer intent, affect, Preference changes or ownership as self-certifying truth.\n \n-Confidence helps choose high-information clarification, not bypass admission. Corroboration is consequence-sensitive: escalate evidence and human review when an interpretation could change a consequential action; unknown or incompatible evidence stays unresolved. An observation consistent with completion is not proof that the modeled effect occurred. `UBU-D0293` defines source-kind mapping; `UBU-Q0165`\u2013`UBU-Q0167` and `UBU-Q0172` define remaining evidence-contract questions. Existing candidate/Discovery records are the reuse point, not newly implemented perception schemas.\n+Confidence helps choose high-information clarification, not bypass admission. Corroboration is consequence-sensitive: escalate evidence and human review when an interpretation could change a consequential action; unknown or incompatible evidence stays unresolved. An observation consistent with completion is not proof that the modeled effect occurred. `UBU-D0293` defines source-kind mapping; `UBU-D0320` specifies the observation binding/admission contract below; `UBU-Q0166`, `UBU-Q0167` and `UBU-Q0172` retain qualifier fidelity, corroboration and proof-of-effect questions. Existing candidate/Discovery records are the reuse point, not newly implemented perception schemas.\n \n The execution loop is **See \u2192 understand \u2192 plan \u2192 direct \u2192 observe \u2192 verify \u2192 learn \u2192 update**. Understanding proposes interpretations; planning remains the kernel's; direction presents an inspectable next action; new observations prompt reconciliation; verification uses the required evidence and operator review; learning proposes model corrections; only admitted updates close the loop. Inference never certifies itself by being repeated downstream. Technique guidance, observed Skill development and procedure-to-Technique capture depend on the later-phase objects and unresolved evidence/safety questions, not merely access to a camera. See [the interaction document](MULTIMODAL_INTERACTION.md) and [the security boundary](MULTIMODAL_SECURITY.md).\n \n+**Observation binding profile (`UBU-D0320`).** Reuse `DiscoveryEvidenceItem` (\u00a712.2) for captured evidence and unresolved interpretations, and per-object `AdvisoryCandidate` records (\u00a721.2.1) for supported proposals. One evidence item may support several candidates; accepting one interpretation never accepts the others. Store the following `observation-admission/1` bindings in their existing payload/evidence slots alongside the native normalized proposal. This is a payload contract, not a new first-class perception object, candidate-kind enum or `FactProvenance` extension. A producer's version-pinned adapter must validate its mapping to the host schemas; an unsupported or lossy mapping stays unresolved and needs separate versioned review under `UBU-Q0166`.\n+\n+| Binding | Required content and interpretation |\n+|---|---|\n+| `profile` | Exactly `observation-admission/1`; retain the host record's own schema version separately. Unknown profiles cannot be admitted. |\n+| `sources` | Nonempty list of `{evidence_ref, evidence_version, source_ref, session_ref, session_version, selector, effective_start, effective_end}`. The controller supplies/resolves these refs; selectors locate a retained frame, span, reading or policy-permitted summary within that source, not model-invented evidence. Endpoints are timezone-bearing timestamps with start <= end; equal endpoints denote an instant. One-shot capture also has a session ref. The captured session revision establishes source enablement at acquisition time; it is not a live grant for later processing or execution. Evidence provenance retains origin Device, source/signal kind, capture enablement and route/policy refs, backend/model/tool, parser/schema version, template digest where applicable, confidence and permitted hashes. |\n+| `context` | `{context_bundle_ref, digest}` binding the exact permitted context used, or `null` only if no persistent context was consulted. Every context-based match must resolve to this bundle's supplied refs. A model cannot search denied context, mint a canonical ref, or substitute a fresh context for the one used. |\n+| `subjects` | List of `{observation_key, alternatives, selection}`; observation keys are unique within the capture and are not canonical entity ids. Each alternative is either `{object_ref, object_version}` or `{candidate_ref, candidate_version, proposed_object_key}`. An empty alternatives list means unknown; `selection: null` means unresolved. Selection is either null or `{alternative_index, review_event_ref}`; only explicit operator resolution selects an alternative. Confidence, coincident labels or repeated sightings cannot merge Identities/objects or establish ownership. |\n+| `proposal` and `claim_evidence` | The native normalized proposal (null for evidence without a supported proposal), plus `claim_evidence: [{field_path, evidence_refs}]` covering every proposed field change. Paths are nonempty arrays of native field names or zero-based array indices; evidence refs are nonempty lists drawn from `sources`. With no proposal, claim_evidence is empty. UniverseState implications use \u00a711.3's `operation`/`target`/`payload` rules; proposed preconditions use \u00a710.1. Unknown/omitted fields propose no write or deletion; a literal JSON null value must be an explicit claim. Source kinds follow `UBU-D0293`; confidence and corroboration stay in evidence, while `FactProvenance` remains `{kind, recorded_at}`. |\n+| `dependencies` | List of `{candidate_ref, candidate_version, proposal_path}` for proposed-object/value links used by the proposal; paths use the same field-path syntax. Object creations also name `proposed_object_key`. Such links are candidate-local, never canonical ids. Admission must resolve them to already-admitted records/values or changes in the same validated atomic batch; otherwise defer. Native schemas that cannot express these links retain the interpretation as Discovery evidence. |\n+| `observed_versions` | Ref-to-version map covering every evidence item, resolved target and context input relied on, plus referenced candidate dependencies. Where a current-state container has no internal version, bind its admitted envelope/frontier digest; do not invent UniverseState history. Creates bind a stable proposed-object key and an explicit create-only/expected-absence condition. Review also binds the candidate's own current version. |\n+| `observed_policy_versions` | Ref-to-version map of all relied-on capture, routing, Compartment, retention and admission policies. Current policy, not capture-time permission alone, governs review and admission. |\n+| `predecessors` | `{evidence: [{evidence_ref, evidence_version}], candidates: [{candidate_ref, candidate_version}], logs: [log_entry_id]}` using the host's correction/supersession links. Admitted-history corrections cite original Log ids under \u00a717.3. Empty lists mean an original interpretation, not absence of earlier world events. |\n+\n+**Supported operations.** Validate the native proposal against an already-supported candidate kind and ordinary admission operation before enqueueing an executable review proposal. Unresolved targets, unsupported object kinds and perceptual values without a supported adapter remain Discovery review evidence with the limitation visible; they cannot fall through to a generic mutation executor. `UniverseTarget` advice remains name-only under `UBU-D0296`: it cannot carry, default or admit a perceptual value. The operator may author a value through the ordinary assertion/measurement form linked to the evidence. Future perceptual value-candidate support requires its separately reviewed versioned adapter; this contract does not enable it. Intent, affect, Preferences, completion/effect claims and later-phase Resources/Skills/Techniques retain their existing source and admission gates.\n+\n+**Disclosure and session scope.** Evidence, candidates, alternatives, selectors, hashes and derivative review material inherit all supporting sources' disclosure/routing/retention restrictions; reference-only payloads do not waive them. Denied matches cannot leak identifiers, labels or existence through alternatives or counts. Reviewing permitted retained evidence is allowed after its capture session is paused/ended; it neither restarts capture nor transfers source consent or pending approval to another Device.\n+\n+**Replay and corrections.** The controller assigns stable evidence ids and capture keys scoped to source/session and source item or span. It assigns separate candidate-creation and review-operation idempotency keys, each bound to its immutable originating operation rather than later lifecycle state. Exact retries, restart and exchange preserve ids, versions, normalized content and keys; the same key with different bound content is rejected, never treated as a harmless duplicate. Distinct observations are not deduplicated by their meaning or time alone. Equivalent repeated proposals use \u00a721.2.1's suppression/deferral rules rather than resetting rejected/deferred state. New material evidence or a correction creates a linked revision/replacement with a new key. Superseded versions and late interpretations cannot overwrite an operator correction or use an old approval. Corrections to admitted claims append \u00a717.3 entries and any separately admitted replacement Snapshot/mutation; they never rewrite the original. Raw-media purge marks supporting evidence unavailable and obeys existing retention policy; it neither fabricates replacement evidence nor automatically retracts or reaffirms a claim.\n+\n+**Review and admission.** Present the source/session and observation time, permitted evidence or an explicit unavailable/redacted indicator, proposed-versus-current diff, source kind, uncertainty/contradictions, alternative matches, dependencies and resulting planning implications. Show the exact fields and batch being accepted; accepting an evidence item alone does not authorize every linked proposal. Offer resolve/edit, accept, reject, defer and private/unknown through existing Discovery/advisory review; edits record the operator's source, create a new bound revision and require fresh review. A permitted partial acceptance creates a bound revision or separate candidate for its exact subset and coherent dependency batch; unaccepted interpretations remain unresolved. If required evidence or target bindings cannot be established, retain an unresolved review item. Restricted attention defers unavailable review under \u00a721.1.2.\n+\n+At acceptance, the CPU rechecks the exact candidate/target/dependency versions and evidence/context/source revisions, including whether the latter remain available and unsuperseded, dependency bindings, current authority and Device trust, current source/disclosure/admission policy and native semantic/conflict rules. Changed bindings invalidate approval; require fresh review rather than silently rebasing. Source confirmation records new evidence of the actual assertion, reading or derivation; it does not relabel the original inference as measured or derived. Commit the ordinary canonical change, review decision with actor/`authority_source`/Device/times/observed versions, resulting Log/Snapshot refs, candidate admission links and idempotency outcome as one crash-consistent unit. Rejection/deferral records only ordinary review metadata, not the proposed world state. Recalculation follows admitted planner-relevant changes. Candidate persistence, successful extraction, high confidence and queue order confer no canonical, projection or external-action authority; external execution requires its own current checks and approval.\n+\n+**Required fixtures.** Before enabling a producer/adapter, pin its revision, host schemas and policy configuration, and serialize the following scenarios through the real validators, review storage and admission reducer. Fixture labels below denote distinct stable schema-valid ids. Record exact input payloads/versions, decisions, candidate/evidence links, expected canonical diffs/Logs and replay outcomes. These are required acceptance cases, not passing implementation results.\n+\n+| Fixture input and action | Required outcome |\n+|---|---|\n+| Evidence `E1@v1`, session `S1@v1`, frame selector `frame:12`, `proposal: null`; evidence payload contains a visual hypothesis `{\"operation\":\"set_fact\",\"target\":\"facts.project.ubu.build_result\",\"payload\":\"passed\"}`; no value adapter or confirmation | Discovery review evidence only; no UniverseState, Snapshot, Task, Plan, Calendar or projection effect. Existing UniverseTarget advice still contains no value. |\n+| Supported precondition proposal `C1@v1` for `T1@v7` (prior precondition absent) uses `E1@v1`, context `B1`, policy `P1@v3` and a recorded target; capture session S1 has ended. Proposes `{\"target\":\"facts.project.ubu.build_result\",\"predicate\":\"equals\",\"expected\":\"passed\"}`, operator edits expected to `\"reviewed\"` and accepts the bound revision | Only T1's precondition becomes that leaf with expected `\"reviewed\"`; the fact's value is unchanged. Ordinary decision/Log links are admitted once. Review restarts no capture; repeat submission, restart and exchange produce no second effect. |\n+| Observation key `person:1` has permitted alternatives `I1@v2` and `I2@v4`, selection null, confidence 0.99; operator leaves unresolved or selects `I2` | Unresolved creates no merge/ownership/fact; selection affects only the newly reviewed proposal bound to `I2@v4`, without accepting other interpretations. |\n+| A proposal references proposed-object key `o1` from `C2@v1`, or a value from `C3@v1`; the dependency is pending, rejected, or admitted | No phantom object, dangling value link or planner use of a candidate value. Pending/rejected dependency prevents dependent admission; an approved atomic dependency batch or fresh review against admitted refs/values resolves it exactly once. |\n+| Between presentation and acceptance, `T1@v7` becomes `v8`, evidence/context is superseded, policy changes, or the originating/acting Device is revoked | No stale/unauthorized admission. Preserve only permitted review material and require the ordinary fresh-review, rejection or conflict path. |\n+| Duplicate `E1/C1` with identical keys/content; then the same key with changed value/target; then a genuinely new observation `E2` | Exact replay collapses; changed-content key reuse rejects; `E2` remains distinct and cannot automatically revive a rejection or satisfy corroboration by repetition. |\n+| Crash before or after the canonical-change/review/idempotency transaction; retry the same approval operation | Either no effect or the complete single admitted effect and its linked review/Log/outcome; never an orphan mutation, false admission acknowledgment or duplicate effect. |\n+| Operator corrects `E1/C1` before a delayed result arrives, and separately corrects an already-admitted claim | Late result cannot restore the superseded claim. Unadmitted replacement needs fresh review; admitted correction appends links and any accepted replacement without editing historical records. |\n+| Supporting raw evidence is purged, or a partial Replica lacks/denies a required source/target; manual text replaces capture | Show only permitted unavailable/incomplete metadata and no denied identifiers or Compartment labels. Missing bindings block that admission; a new operator-authored submission uses its actual source and fresh bindings without recreating deleted media or widening disclosure. |\n+| Missing required binding, unknown profile/schema, model-invented ref, malformed native proposal or hard routing/Compartment denial | No dispatch/admission; only policy-permitted evidence and an unresolved/rejected diagnostic through existing machinery. |\n+| Explicit operator assertion, instrument reading and actual derivation through supported ordinary authoring/computation paths; high-confidence apparent Task completion without the required effect evidence | Admitted kinds follow `UBU-D0293` with unchanged FactProvenance fields and retained original candidate links; unsupported derivation paths remain evidence-only. Apparent completion alone creates no lifecycle/effect update, inferred affect/intent or Preference. |\n+\n+`UBU-Q0166` retains qualifier-fidelity/schema-gap checks; `UBU-Q0167` retains consequence-sensitive evidence sufficiency; `UBU-Q0169`\u2013`UBU-Q0171` retain capture/retention mechanics; `UBU-Q0172` retains proof-of-effect semantics. This contract freezes bindings, reuse and admission behavior without claiming those questions or implementation acceptance are complete.\n+\n #### 21.1.2 Restricted-attention audio envelope\n \n **Embodied Audio Interaction (`UBU-D0309`).** Activity mode is separate from the realtime session's source/routing mode and from `compartment_ids`: for example, `Mode: Driving` with `Compartment: UbU Project / Public Outreach`. Activity constrains interaction; Compartment policy constrains disclosure and authority. Neither grants permission supplied by the other.\n \n@@ -3447,9 +3489,9 @@ Audit records link the session, evidence/candidate, budget and policy versions,\n - Start, pause, resume, stop and revocation accurately control acquisition and visible state. Raw AV stays local; context and output respect Compartment denials, and export requires its separate authority. Retention behavior matches the approved policy rather than a documentation claim.\n - Switching surfaces during a delayed interpretation, duplicate submission/replay, and material context, policy or Device change cause neither stale admission nor duplicate mutations. Preserved candidates carry no live capture consent or pending execution approval across Devices.\n - Ambiguous objects, high-confidence interpretations and apparent completion remain unresolved/candidate evidence until required review and validation. Unknown/private intervals and channel choices create no inferred intent, affect, Preference, habit or completion claim. Restricted attention obeys \u00a721.1.2: defer unavailable review rather than demand visual/form interaction or widen permissions.\n \n-This resolves interaction coverage and acceptance, not implementation. `UBU-Q0183` retains capture-mode/device allocation and delivery sequence; `UBU-Q0160` retains the mobile workload/API choice; `UBU-Q0128` retains numeric restricted-attention profiles and richer actions; `UBU-Q0165`\u2013`UBU-Q0172` retain exact evidence, corroboration, latency, segmentation and retention contracts. Apply the existing evidence, policy and attention gates to each enabled slice. No particular backend, hardware support, latency target or later-phase planning object is established here.\n+This resolves interaction coverage and acceptance, not implementation. `UBU-Q0183` retains capture-mode/device allocation and delivery sequence; `UBU-Q0160` retains the mobile workload/API choice; `UBU-Q0128` retains numeric restricted-attention profiles and richer actions; `UBU-Q0166`\u2013`UBU-Q0172` retain exact evidence, corroboration, latency, segmentation and retention contracts. Apply the existing evidence, policy and attention gates to each enabled slice. No particular backend, hardware support, latency target or later-phase planning object is established here.\n \n ### 21.2 Structured-output admission pipeline\n \n LLM, parser, worker, and agent outputs should follow an admission pipeline:\ndiff --git a/DECISIONS.md b/DECISIONS.md\nindex 7bc0c88..0000000 100644\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5082,3 +5082,15 @@ Require visible source controls, optional speech output, keyboard/screen-reader\n Before claiming delivery, attach the revision-pinned real-hardware scenario evidence specified in DESIGN.md \u00a721.1.3, including camera-and-voice usefulness, text/form completion, denied/failed sources, accessibility, capture/route enforcement, stale/duplicate results and uncertainty/restricted-attention boundaries.\n \n This resolves the product contract, not implementation or release acceptance. Exact capture slices and sequence remain `UBU-Q0183`; mobile workload/API, richer audio profiles, evidence, latency and retention questions retain their existing scope and gates.\n+\n+---\n+\n+## UBU-D0320: Observation bindings reuse Discovery evidence and advisory admission\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a721.1.1. Resolves `UBU-Q0165`; specifies the payload/admission contract without enabling a perception producer or changing existing advisor authority.\n+\n+Adopt the `observation-admission/1` binding profile and required fixtures in DESIGN.md \u00a721.1.1. Capture and unresolved interpretations use existing `DiscoveryEvidenceItem` records; supported native proposals use per-object `AdvisoryCandidate` records, lifecycle/review events and ordinary CPU admission. Bind source/session/evidence/context refs, ambiguity alternatives, proposed-object/value dependencies, observed versions and policies, corrections and separately scoped replay keys. Unsupported or lossy mappings stay unresolved and require separate versioned review.\n+\n+Operator review binds the exact diff, resolved targets and dependency batch. Admission rechecks those bindings and current authority/policy, then atomically records canonical changes, ordinary review/Log links and idempotency. Candidate/evidence storage never admits world state or authorizes projection/external action. Exact replay has one effect; changed-content key reuse rejects; stale results and corrections require fresh review and append-only history.\n+\n+`FactProvenance` remains only `{kind, recorded_at}` under `UBU-D0293`, and `UniverseTarget` advice remains name-only under `UBU-D0296`. Qualifier fidelity, corroboration sufficiency, capture/retention mechanics and proof of effects remain under `UBU-Q0166`, `UBU-Q0167`, `UBU-Q0169`\u2013`UBU-Q0172`. The fixtures are implementation gates, not reported passing results.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\nindex f889b0e..0000000 100644\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3112,17 +3112,17 @@ Resolved by `UBU-D0319`: the primary camera-and-voice surface covers the core pl\n ## UBU-Q0165: Observation-to-candidate contract and admission reuse\n \n-Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-11 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: UBU-D0320 Last scored: 2026-10-11 Scored from commit: None\n \n ### Question\n \n What exact observation-to-candidate contract reuses existing advisory and Discovery admission machinery without introducing automatic canonical mutation?\n \n ### Current direction\n \n-Delta \u00a717.3. Specify source/session refs, identity ambiguity, candidate object/value links, observed versions, evidence, idempotency, corrections and review presentation. The architectural pipeline is answered in DESIGN.md \u00a721.1.1; concrete payload coverage and fixtures are unresolved. Do not invent new FactProvenance fields.\n+Delta \u00a717.3. The architectural pipeline is answered in DESIGN.md \u00a721.1.1. `UBU-D0320` specifies source/session and context bindings, identity alternatives, object/value links, observed versions, evidence, idempotency, corrections, review and required fixtures. Qualifier fidelity, corroboration and proof-of-effect semantics remain `UBU-Q0166`, `UBU-Q0167` and `UBU-Q0172`. Do not invent new FactProvenance fields.\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0320`: DESIGN.md \u00a721.1.1 defines the `observation-admission/1` payload binding profile over existing DiscoveryEvidenceItem and AdvisoryCandidate records, supported-operation gating, explicit version-bound review, ordinary atomic admission, replay/suppression and append-only correction, plus required fixtures. Unsupported or unresolved interpretations stay review evidence; `UniverseTarget` remains name-only and `FactProvenance` keeps `{kind, recorded_at}`. This resolves the contract, not implemented support or passing fixture evidence; `UBU-Q0166`, `UBU-Q0167` and `UBU-Q0169`\u2013`UBU-Q0172` remain open.\n \n ---\n",
    "commit_message": "docs: define observation-to-candidate admission contract",
    "validation_notes": [
      "Generated with git diff --no-index from current working-tree originals and in-memory proposed contents; repository files remain unchanged.",
      "git apply --check --whitespace=error passed against /home/sean/ubu-design's working tree.",
      "Verified hunk counts, allowlisted paths, and a single question hunk beginning at UBU-Q0165's heading and containing its own Resolution section.",
      "Preserved scoring metadata and every other question block; related evidence and retention questions remain Open.",
      "Acceptance fixtures are specified implementation gates; behavioral implementation tests were not run."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0165"
    ],
    "decisions_added": [
      "UBU-D0320"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0165-openai-gpt-6-observation-admission-1",
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
