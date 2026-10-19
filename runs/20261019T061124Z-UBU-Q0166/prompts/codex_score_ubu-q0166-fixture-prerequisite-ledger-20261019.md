# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `anthropic`

## Selected question

Question ID: `UBU-Q0166`  
Question title: `Preserve perceptual qualifiers alongside the closed provenance enum`  
Base commit: `cb5b99f5cb73b217e2b1a6620fd51ed2df6f47e6`

```markdown
## UBU-Q0166: Preserve perceptual qualifiers alongside the closed provenance enum

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-19 Scored from commit: None

### Question

Which revision-pinned host mappings preserve the claim-level qualifications specified by `UBU-D0321` through validation, serialization, storage, review and authorized exchange, and does a failed mapping demonstrate a schema gap requiring separate versioned review?

### Current direction

Delta §17.4. `UBU-D0293` decides kind mapping; `UBU-D0320` decides observation/admission bindings; `UBU-D0321` specifies claim-level qualifier content, field/evidence links and fidelity fixtures. The remaining work is a revision-pinned host field/payload mapping and round-trip evidence from the actual validators, serializers, store/review and exchange paths. Measured remains an instrument/reading, derived a computation from facts, and proposed an unconfirmed candidate. Do not infer host compatibility from prose payload slots. A demonstrated gap requires separate versioned chain review; no schema, pin or migration change is made here. `UBU-D0323` records bounded candidate link-carrier checks and source-inspected downstream limitations. Full claims/qualifiers and resolved evidence revisions still need fixtures through the actual Discovery, normalization, store/review, admission and authorized-exchange paths. A rejected direct field mapping alone does not establish a schema gap.

`UBU-D0334` extends those probes to actual persistence/reload/replay and direct review API reads, with a minimal runtime reproduction of inline qualifier loss in `precondition_review::vet_result`. The rejected normalized-proposal alternative and this implementation loss do not establish that all compliant existing payload/ref mappings fail. Repair or validate that branch before relying on it; the full qualifier/evidence and policy-authorized paths remain the enablement gate.

`UBU-D0360` adds opaque string-reference controls that survive core round trips and the tested normalization branch. The first token's normalized output also survives direct storage/replay/API reads. The unresolved alternate token also receives native `ok`; no manifest or evidence revision is resolved. Treat these as carrier evidence only, not an approved field-provenance mapping or qualifier-preservation result. The inline loss remains unrepaired.

`UBU-D0366` exercises actual native enqueue and admission services. Both unresolved tokens can accompany an admitted synthetic precondition removal; known and unknown inline profiles are discarded before enqueue and their native removals also admit. The changed-precondition control rejects without an effect. These controls establish native behavior, not qualifier-aware admission: resolved content, profile/ref/policy validation and complete reviewed evidence links remain required.

`UBU-D0373` adds mounted Review component controls with a mocked API/client boundary. Retained known and unknown inline specimens yield identical article DOM to the baseline; opaque refs appear only as strings, and Admit invokes the mock in every control. This establishes a separate presentation limitation, without full qualifier content, actual admission or an end-to-end mapping.

`UBU-D0379` adds actual JavaScript client/plugin probes with mocked native IPC: an exact synthetic integer changes during response JSON decoding, while raw text and string controls survive. Using the same decoder for input/output comparison masks the loss. Include independent exact-value checks and the client numeric domain in the pinned mapping; no encoding or adapter is approved.

`UBU-D0385` supplies a compact evidence index and retained client request-binding reproduction in §21.1.1. The index preserves specimen and harness boundaries; synthetic version rounding establishes no native admission or practical stale approval. Next investigate a complete Discovery evidence revision and resolved candidate/field links, then adapter rejection and actual service/review paths under the existing policy-bounded mapping gate.

`UBU-D0386` records a source-only Discovery host-path inventory at the same five revisions. No dedicated evidence implementation or complete revision resolver was located in its bounded tracked-source scope. Generic carriers remain unqualified; identify the concrete evidence write/read/resolution and candidate field-to-claim paths before the complete fixture run. This finding establishes no governing schema gap.

### Resolution

Open. `UBU-D0321` answers preservation requirements; `UBU-D0323` records the initial synthetic carrier probes. `UBU-D0334` adds actual direct SQLite close/reopen/replay and review API controls, and reproduces qualifier loss in the tested precondition-review normalization branch despite a native `ok` result. This disqualifies that mapping through that branch, without establishing a governing schema gap. `UBU-D0360` additionally records retention of opaque reference strings through normalization and, for the first specimen, direct persistence/API controls. The alternate token remains unresolved; no manifest or qualifier content is resolved. Complete Discovery claim/qualifier mappings, adapter profile/ref validation, faithful normalization and rendered review, admission and authorized-exchange fixtures remain outstanding. Close only with the full pinned mapping and fixture evidence, or a minimally reproduced host-contract gap routed to a separate versioned change. No passing adapter or actual schema gap is established.

`UBU-D0366` adds actual controller enqueue/native-admission controls and native stale-value/old-candidate-version rejection. It demonstrates no resolved manifest or qualifier-preserving admission, and makes no repair or schema-gap claim. The full existing enablement gate remains outstanding; this question stays Open.

`UBU-D0373` upgrades the generic precondition-review card's UI source inspection to bounded mounted DOM/click evidence. It makes no repair or full-preservation claim; resolved qualifier presentation, actual review/admission and authorized exchange remain outstanding. This question stays Open.

`UBU-D0379` records a separate numeric-fidelity limitation at the tested client decoder and a comparison that falsely reports preservation when both sides use that decoder. The numeric carrier controls resolve no evidence and test no deployed WebView, native admission or exchange. Exact numeric representations and full qualifier preservation remain unqualified; this question stays Open.

`UBU-D0385` retains the request-binding probe source/results and indexes the existing evidence without replacing closure criteria. No complete qualifier mapping, repair, passing adapter or governing schema gap is established; this question stays Open.

`UBU-D0386` bounds the unidentified Discovery implementation path to its inspected scope; it executes no complete Discovery fixture and approves no carrier mapping. The full existing gate remains outstanding; this question stays Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0166-fixture-prerequisite-ledger-20261019",
    "provider_id": "anthropic",
    "model_name": "claude-fable-5-1",
    "question_id": "UBU-Q0166",
    "base_commit": "cb5b99f5cb73b217e2b1a6620fd51ed2df6f47e6",
    "summary": "Add UBU-D0388, a fixture prerequisite ledger for UBU-Q0166. The ledger in DESIGN.md \u00a721.1.1 names every host path that UBU-D0321's host-mapping gate requires and records its disposition at the UBU-D0323 pinned revisions as present, lossy, absent or unexecuted, citing only the decision whose specimen produced each result. It gates the complete fixture run on supplying the absent paths (adapter validation, Discovery evidence writer, revision-addressed resolver) through separately reviewed host changes recorded at their resulting revisions, requires lossy paths to be repaired or avoided with evidence, and stops recording further synthetic carrier variants that change no ledger row. Absent or lossy rows are prerequisites or defects, not a governing schema gap. UBU-Q0166 stays Open with unchanged closure criteria.",
    "rationale": "Nine bounded decisions (UBU-D0321 through UBU-D0386) have characterized every reachable generic carrier path and each ends with the same disposition: no passing mapping and no governing gap. UBU-D0386 established that the remaining fixture cannot run because the pinned hosts contain no Discovery evidence writer, revision-addressed reader/resolver or companion-profile adapter, and that absence is not a schema gap. Further synthetic probes on already-characterized paths cannot change closure and produce non-combinable evidence (UBU-D0385). The most useful next step is therefore to convert the accumulated results into one auditable prerequisite ledger with an explicit scheduling rule: the complete fixture run waits for the absent paths to be supplied by separately reviewed host work and recorded at pinned revisions, lossy paths must be repaired or avoided with evidence, and new evidence is recorded only when it changes a row. This narrows the question without inventing a profile, grammar, encoding, repair or schema change, keeps UBU-D0321's gate and UBU-D0323's gap test intact, and prevents further low-value probe churn. No new question is allocated because the implementation work belongs to separately reviewed host changes rather than to this mapping question.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -3478,3 +3478,15 @@\n This is source inspection, with no new validator, round-trip, service or exchange execution. `UBU-Q0166` stays Open under `UBU-D0321`'s existing gate. No mapping, repair, governing gap, schema, enum, FactProvenance field, pin, migration or adapter enablement is established.\n \n+---\n+\n+## UBU-D0388: Fixture prerequisite ledger gates further perceptual host-mapping evidence\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a721.1.1. Narrows `UBU-Q0166`; follows `UBU-D0386` by recording the disposition of every required host path at the pinned revisions, without enabling an adapter.\n+\n+Record the ledger in DESIGN.md \u00a721.1.1. It names each path that `UBU-D0321`'s host-mapping gate requires and marks it `present`, `lossy`, `absent` or `unexecuted` at the `UBU-D0323` revisions, citing only the decision whose specimen and harness produced that result. Adapter profile/ref validation, a Discovery evidence writer and a revision-addressed evidence reader/resolver are `absent`; precondition-review normalization, the generic review card and the client decoder are `lossy`; authenticated HTTP, crash recovery, correction/partial acceptance, purge and authorized exchange are `unexecuted`. The ledger indexes recorded results and executes nothing new; rows cannot be combined into an end-to-end result.\n+\n+Schedule the complete fixture run only when no required row is `absent` and each `lossy` row is repaired at a new pinned revision or avoided by the proposed mapping with its own round-trip evidence. Missing paths are supplied by separately reviewed host changes outside this question, including any versioned schema/type/store chain review they need, and are recorded in the ledger at their resulting revisions before use; changed revisions re-establish affected rows rather than copying superseded cells. Until then, record further `UBU-Q0166` evidence only where it changes a row; additional synthetic carrier variants on already-characterized rows are not recorded.\n+\n+An `absent` or `lossy` row is an implementation prerequisite or defect, not a governing host-contract gap; that route still needs a minimally reproduced restriction that no compliant existing payload/ref mapping can satisfy. Closure criteria are unchanged. No profile, canonical object, candidate kind, reference grammar, numeric encoding, repair, schema, enum, FactProvenance field, pin, migration, producer authority or adapter enablement is selected; `UBU-Q0166` stays Open.\n+\n ---\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -2890,3 +2890,25 @@\n Before the next fidelity run, identify the concrete evidence writer, revision-addressed reader/resolver, current policy boundary and candidate/field/claim host paths for one complete permitted specimen. Supply missing implementation through separately reviewed work and recheck affected revisions before relying on it. Then run the existing profile/ref rejection, normalization, service, rendered-review, recovery and authorized-exchange fixtures. No new profile, canonical object, candidate kind, reference grammar or closure criterion is selected. `UBU-Q0166` stays Open; the previously recorded normalization, presentation and client limitations remain unrepaired, and no governing schema gap is demonstrated.\n \n+**Fixture prerequisite ledger (`UBU-D0388`; 2026-10-19).** The ledger names each host path that `UBU-D0321`'s host-mapping gate requires and records its disposition at the five checkout revisions pinned in `UBU-D0323` and rechecked by `UBU-D0386`. Dispositions are `present` (an identified path retained the cited specimen), `lossy` (an identified path reproduced content loss, omission or a false pass), `absent` (no path located in `UBU-D0386`'s inspected tracked-source scope) and `unexecuted` (no fixture has exercised the path). Each row inherits only its cited decision's specimen, harness and boundary: `present` establishes carrier retention for that specimen, not complete qualifier fidelity, and rows cannot be combined into an end-to-end result. This is bookkeeping over recorded results, not a new execution, mapping or closure rule.\n+\n+| Required path | Pinned representation or function | Disposition | Source |\n+|---|---|---|---|\n+| Canonical schema validation of the companion carrier | `tools/validate-fixtures` over `schemas/core/advisory-candidate.schema.json` | `present`; generic acceptance supplies no profile, claim-key or version check | `UBU-D0323`, `UBU-D0334`, `UBU-D0366` |\n+| Candidate Rust/JSON round trip | `AdvisoryCandidate` deserialization, `validate` and reserialization | `present` for the inline member, string evidence refs and field-provenance strings; a structured field-provenance value rejects | `UBU-D0323`, `UBU-D0334`, `UBU-D0360` |\n+| Adapter validation of the `perceptual-qualifiers/1` profile, claim/evidence/field refs, versions and policy | None located; the generic layers accept an unknown profile and the tested normalization branch discards it without rejection | `absent` | `UBU-D0323`, `UBU-D0334`, `UBU-D0366`, `UBU-D0386` |\n+| Evidence write of a `DiscoveryEvidenceItem` revision carrying claims and qualifications | None located; `object_type_from_str` has no Discovery case | `absent` | `UBU-D0386` |\n+| Revision-addressed evidence read and `{evidence_ref, evidence_version, claim_key}` resolution | None located; both synthetic tokens receive native `ok` without dereference | `absent` | `UBU-D0360`, `UBU-D0366`, `UBU-D0386` |\n+| Current policy boundary for evidence disclosure and retention | Not identified for evidence records; direct handler calls bypass HTTP authorization and disclosure middleware | `unexecuted` | `UBU-D0334`, `UBU-D0386` |\n+| Candidate normalization | `precondition_review::vet_result` | `lossy` for the inline member, discarded with native `ok`; `present` for string refs; other native candidate kinds are `unexecuted` | `UBU-D0334`, `UBU-D0360` |\n+| Enqueue and native admission | `advisory_service::run_advisory`, `admit_candidate` | `present` for string refs; the inline member is absent after enqueue; no companion validation, and decision links bind only the Task version | `UBU-D0366` |\n+| Candidate persistence, reload and exact replay | `store_advisory_candidate`, `UbuStore::connect`, `get_advisory_candidate`, `CandidateRecord::candidate` | `present` for stored candidate JSON across pool close/reopen; process/crash recovery `unexecuted` | `UBU-D0334`, `UBU-D0360` |\n+| Review API reads | `api::advisory::candidate`, `api::advisory::queue` | `present` for stored candidate JSON as direct handler calls; authenticated HTTP `unexecuted` | `UBU-D0334`, `UBU-D0360` |\n+| Client decoding and admission request binding | `src/api/client.ts::request`, `admitAdvisory` | `lossy` beyond the safe-integer domain; strings, null and in-domain integers `present` | `UBU-D0379`, `UBU-D0385` |\n+| Rendered qualifier review | `src/routes/Review.tsx` generic card | `lossy`: companion content is not rendered and Admit stays enabled; refs appear as literal strings | `UBU-D0373` |\n+| Correction, partial acceptance, purge/partial Replica and authorized exchange | Existing \u00a717.3, \u00a712.2 and DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md rules; no qualifier fixture run | `unexecuted` | `UBU-D0321` gate |\n+\n+**Ledger rules.** Schedule the complete fixture run of `UBU-D0321`'s gate only when no required row is `absent` and every `lossy` row is either repaired at a new pinned revision or avoided by the proposed mapping with its own round-trip evidence. Supply `absent` rows through separately reviewed host changes outside this question, including any versioned schema/type/store chain review they need, and record each resulting revision in the ledger before relying on it. A row changes only with a recorded revision and re-executed evidence; superseded cells are not copied forward (`UBU-D0385`). Until the run is scheduled, record further `UBU-Q0166` evidence only where it changes a row: a newly identified path, a changed disposition at a recorded revision, a first result for an `unexecuted` row, or a mapping that avoids a `lossy` path with its own evidence. Additional synthetic carrier variants on already-characterized rows add no closure evidence and are not recorded.\n+\n+An `absent` or `lossy` row is an implementation prerequisite or defect, never a governing host-contract gap; that route still requires a minimally reproduced restriction that no compliant existing payload/ref mapping can satisfy (`UBU-D0323`). Closure is unchanged: the full pinned mapping with fixture evidence at the ledger's recorded revisions, or that separately routed gap. No profile, canonical object, candidate kind, reference grammar, numeric encoding, repair, schema, enum, `FactProvenance` field, pin, migration, producer authority or adapter enablement is selected here; `UBU-Q0166` stays Open.\n+\n **Supported operations.** Validate the native proposal against an already-supported candidate kind and ordinary admission operation before enqueueing an executable review proposal. Unresolved targets, unsupported object kinds and perceptual values without a supported adapter remain Discovery review evidence with the limitation visible; they cannot fall through to a generic mutation executor. `UniverseTarget` advice remains name-only under `UBU-D0296`: it cannot carry, default or admit a perceptual value. The operator may author a value through the ordinary assertion/measurement form linked to the evidence. Future perceptual value-candidate support requires its separately reviewed versioned adapter; this contract does not enable it. Intent, affect, Preferences, completion/effect claims and later-phase Resources/Skills/Techniques retain their existing source and admission gates.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -4050,3 +4050,5 @@\n `UBU-D0386` records a source-only Discovery host-path inventory at the same five revisions. No dedicated evidence implementation or complete revision resolver was located in its bounded tracked-source scope. Generic carriers remain unqualified; identify the concrete evidence write/read/resolution and candidate field-to-claim paths before the complete fixture run. This finding establishes no governing schema gap.\n \n+`UBU-D0388` records a fixture prerequisite ledger in \u00a721.1.1 at the pinned revisions: adapter profile/ref validation, a Discovery evidence writer and a revision-addressed evidence reader/resolver are absent; precondition-review normalization, the generic review card and the client decoder are lossy; authenticated HTTP, crash recovery, correction/partial acceptance, purge and authorized exchange are unexecuted. Schedule the complete fixture run only once no required row is absent and each lossy row is repaired or avoided with evidence; missing paths come from separately reviewed host changes recorded at their resulting revisions. Record further evidence here only where it changes a ledger row. Absent or lossy rows are prerequisites or defects, not a governing schema gap.\n+\n ### Resolution\n@@ -4066,3 +4068,5 @@\n `UBU-D0386` bounds the unidentified Discovery implementation path to its inspected scope; it executes no complete Discovery fixture and approves no carrier mapping. The full existing gate remains outstanding; this question stays Open.\n \n+`UBU-D0388` adds the prerequisite ledger and its scheduling rule; it executes nothing new, selects no mapping, grammar, encoding or repair, and establishes no governing schema gap. Closure criteria are unchanged, and the complete fixture run remains unscheduled until the absent paths are supplied and recorded; this question stays Open.\n+\n ---\n",
    "commit_message": "UMC: docs: record fixture prerequisite ledger for UBU-Q0166\n\nAdd UBU-D0388: a ledger in DESIGN.md \u00a721.1.1 marking each host path\nrequired by UBU-D0321's host-mapping gate as present, lossy, absent or\nunexecuted at the UBU-D0323 pinned revisions. The complete fixture run\nis scheduled only once absent paths are supplied by separately reviewed\nhost changes and lossy paths are repaired or avoided with evidence;\nfurther synthetic carrier variants that change no row are not recorded.\nAbsent or lossy rows are prerequisites or defects, not a governing\nschema gap. UBU-Q0166 stays Open with unchanged closure criteria.",
    "validation_notes": [
      "No file-read, search or shell tool was exposed in this session, so the patch was constructed from the supplied working-tree excerpts and `git apply --check` could not be run here; run it against the working tree before merging.",
      "Hunk line numbers are estimates; every context line is quoted verbatim from the excerpts so `git apply` can locate the hunks by content with an offset.",
      "DECISIONS.md inserts UBU-D0388 immediately after UBU-D0386, anchored on its closing paragraph and reusing its `---` rule as trailing context, so nothing is assumed about what follows; if UBU-D0387 already follows UBU-D0386, move the UBU-D0388 block after it to keep numerical order.",
      "OPEN_QUESTIONS.md edits are confined to the UBU-Q0166 block (Current direction and its own Resolution section); the metadata line is unchanged and Status remains Open.",
      "The ledger restates dispositions already recorded in UBU-D0323 through UBU-D0386 and claims no new execution, pin, schema, host change, repair or adapter enablement.",
      "No new question is allocated; missing host implementation is routed to separately reviewed host changes as UBU-D0386 already directs."
    ],
    "new_questions_added": [],
    "questions_resolved": [],
    "decisions_added": [
      "UBU-D0388"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0166-fixture-prerequisite-ledger-20261019",
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
