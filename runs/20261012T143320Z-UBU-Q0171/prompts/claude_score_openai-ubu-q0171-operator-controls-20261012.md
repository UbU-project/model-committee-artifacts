# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0171`  
Question title: `Operator controls over discard, summarize, hash, clip and export`  
Base commit: `a3843f72dab3ce719eb0a533a57d1098d4b041ed`

```markdown
## UBU-Q0171: Operator controls over discard, summarize, hash, clip and export

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-12 Scored from commit: None

### Question

How does the operator inspect and control raw-AV discard, summaries, hashes, clips and any permitted subsequent export?

### Current direction

Delta §17.9. Controls must show capture state, destination and retention effects, obey hard Compartment denials and preserve minimized correction/deletion metadata. Hashes and summaries can still expose sensitive context; no blanket permission to export them follows from deleting raw media.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "openai-UBU-Q0171-operator-controls-20261012",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0171",
    "base_commit": "a3843f72dab3ce719eb0a533a57d1098d4b041ed",
    "summary": "Define operator inspection, independent AV artifact actions and separately approved, policy-gated export.",
    "rationale": "Existing retention rules answer how long raw media may live. These controls make scope, destinations and retention effects inspectable, prevent summaries or hashes from bypassing source policy, and invalidate stale export approvals while preserving minimized correction/deletion history.",
    "changed_files": [
      "DESIGN.md",
      "DECISIONS.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -2075,7 +2075,7 @@ Allowed Phase 1 discovery inputs are intentionally narrow:\n \n Phase 1 discovery excludes covert or broad capture by default: continuous microphone, camera, screen recording, keystroke logging, raw message bodies, raw file contents, raw GPS trails, and cross-Identity sharing are outside the default discovery input set. Any later use of those sources requires a separate explicit mode, Compartment review, routing disclosure, and user approval.\n \n-**AV refinement for the MVP interaction surface (`UBU-D0292`).** The primary camera-and-voice surface does not enable continuous sources implicitly. A consented capture session remains visible, source-scoped, pausable and inspectable under the clause above. Process raw AV locally on the operator's own hardware, minimize context and retain only material justified by its purpose. Selective clips, summaries, hashes or deletion are operator-controlled under Compartment policy; subsequent export needs its own allowed route and approval. No-cloud/no-export denials remain hard. Segmentation and bounded local interfaces follow \u00a721.1.5 (`UBU-D0326`); raw-media retention defaults follow `UBU-D0327` below, while operator discard/export controls remain open in `UBU-Q0171`; recording a policy does not claim its enforcement has shipped.\n+**AV refinement for the MVP interaction surface (`UBU-D0292`).** The primary camera-and-voice surface does not enable continuous sources implicitly. A consented capture session remains visible, source-scoped, pausable and inspectable under the clause above. Process raw AV locally on the operator's own hardware, minimize context and retain only material justified by its purpose. Selective clips, summaries, hashes or deletion are operator-controlled under Compartment policy; subsequent export needs its own allowed route and approval. No-cloud/no-export denials remain hard. Segmentation and bounded local interfaces follow \u00a721.1.5 (`UBU-D0326`); raw-media retention defaults follow `UBU-D0327` below and operator controls follow `UBU-D0328`; recording a policy does not claim its enforcement has shipped.\n \n **Default raw-media retention (`UBU-D0327`).** For an explicitly enabled continuous or near-continuous perception session, durable raw-media retention is off. Use a local memory-only rolling buffer with a maximum sample age of **60 seconds from acquisition**, measured on the original source monotonic clock. Release material earlier when its authorized processing/look-back purpose no longer needs it or a shorter policy limit applies. The finite byte and aggregate-resource limits required by \u00a721.1.5 may shorten this window; it is a privacy ceiling, not a promised look-back duration or latency result. Pins, worker copies, queues, retries and decoded frames share the original deadline. No raw disk spool, debug payload, general cache, automatic backup or sync copy is authorized by this default. A Device/backend profile whose persistence paths cannot meet effective policy is ineligible.\n \n@@ -2085,10 +2085,30 @@ A purposeful clip requires explicit operator approval before durable saving, bou\n \n Expiry denies further reads and result release and cancels affected jobs. Remove expired app-controlled clip payloads, any clip-specific keys and reconstructive raw copies; if cleanup is interrupted, reconcile deadlines and complete pending cleanup before reopening capture or retained-media access after restart. Clock rollback or uncertain remaining lifetime must deny access rather than extend retention. Report scheduled, pending, failed and confirmed local deletion distinctly; a deadline or local unlink cannot establish remote deletion or forensic erasure. App shutdown may leave cleanup pending and must not be reported as completed deletion.\n \n-Do not automatically replace discarded raw media with a transcript, embedding, hash or summary archive. Separately retained minimized evidence needs its own allowed purpose and retention authority, inherits source disclosure restrictions, and may outlive raw media only when policy permits. Purge disallowed reconstructive derivatives and retain only permitted gap/deletion metadata. The append-only rules below continue to govern admitted claims. This is a policy decision, not implemented automated deletion or a physical-erasure guarantee; continuous capture and clip storage require the revision-pinned retention checks in `UBU-D0327` before enablement. Detailed operator controls remain `UBU-Q0171`.\n+Do not automatically replace discarded raw media with a transcript, embedding, hash or summary archive. Separately retained minimized evidence needs its own allowed purpose and retention authority, inherits source disclosure restrictions, and may outlive raw media only when policy permits. Purge disallowed reconstructive derivatives and retain only permitted gap/deletion metadata. The append-only rules below continue to govern admitted claims. This is a policy decision, not implemented automated deletion or a physical-erasure guarantee; continuous capture and clip storage require the revision-pinned retention checks in `UBU-D0327` before enablement. Detailed operator controls follow `UBU-D0328` below.\n \n Selective raw-media retention and append-only evidence are distinct. Discarding a raw stream or deleting an authorized clip does not edit an admitted historical claim in place: append minimized correction, revocation, deletion or tombstone metadata with the appropriate links. The immutable Log retains evidence about what was asserted, reviewed or withdrawn, not an automatic reconstructible copy of private raw media. A hash is not proof of an event and can itself leak correlation; policy must govern it. Later views can mark deleted evidence unavailable without inventing a replacement observation. `UBU-Q0162` tracks the reported Log-editability enforcement gap.\n \n+**Operator media controls (`UBU-D0328`, Phase 2).** Keep capture state and pause/stop controls available independently of model completion. A policy-filtered media inventory shows each permitted source/session, capture Device and processing route, purpose, source intervals and coverage gaps, transient raw buffer versus retained artifact, storage destination, effective retention rule/deadline and cleanup state. Inspect/replay only material still available and authorized; show expired/deleted evidence as unavailable. Show known permitted copies and export outcomes without claiming a complete inventory of other Devices or recipients. Counts, previews, filenames, source refs and Compartment labels obey current visibility policy; omit an item when even existence is denied.\n+\n+Offer independent actions with no durable-save or export option selected by default. Show the selected source/artifact scope, destination, retention effects and any remaining permitted evidence before committing an action. Capture consent, relevance suggestions, model output and prior deletion authorize none of these actions implicitly.\n+\n+| Control | Required behavior |\n+|---|---|\n+| Discard raw / Delete retained artifact | Remove the selected app-controlled payloads and affected disallowed copies/derivatives; invalidate their handles and pending reads/results, cancel affected work and report cleanup under `UBU-D0327`. Discard does not create a summary, transcript, embedding or hash. It acts on the displayed scope; Stop capture is separately available immediately. |\n+| Create summary | Use only selected, permitted local source spans with bounded analysis. Preview the exact proposed text with source/transform provenance, analyzed/missing coverage, attribution and uncertainty. It remains advisory evidence requiring ordinary review/admission; a summary cannot certify an event or effect. |\n+| Create hash | Make hashing an explicit optional operation. Show the algorithm/version, exact input representation and source scope; record lineage only as policy permits. A hash can disclose identity or correlation and is neither anonymization nor proof of the captured event. No mandatory media hash belongs in discard or audit records. |\n+| Save clip | Preview the actual selected sources, channels, intervals and any codec padding. Apply `UBU-D0327`'s eligible encrypted local storage, explicit version-bound approval and absolute expiry; use \u00a721.1.5's bounded authorized read/output scope. Unavailable spans cannot be recovered or silently replaced with adjacent capture. |\n+| Export | Review the final output and metadata locally, then obtain a separate operation-bound approval for an allowed recipient and route through the authoritative Compartment/export gate. Include filenames, attachments, thumbnails, captions, timestamps, source refs and hashes in disclosure review; clipboard, share-sheet, file-copy and integration routes outside protected storage receive the same checks. |\n+\n+Summary/hash previews have finite temporary leases under \u00a721.1.5. Durable artifact retention requires an explicit separate save choice bound to the displayed result/source versions, actor/Device, purpose, eligible local destination and current source-policy versions, with the effective retention rule and any absolute expiry. Admitted claims and Task lore retain their existing source-history/lifecycle rules. The inventory shows the approved result and its own retention authority; permitted derivatives may outlive raw media, but raw discard never grants that authority. Review, copying and retries cannot renew original source or artifact deadlines. Combining actions must preserve each approval and its failure outcome; a failed derivation cannot silently retain raw media past expiry. No cloud fallback, training use, backup or sync permission follows.\n+\n+Export approval binds the actor and eligible Device, immutable final artifact/payload versions, all contributing source restrictions and current policy versions, purpose, recipient, route, format/metadata, recipient retention conditions, finite approval expiry and idempotency key. A displayed finite batch may share approval; future captures and wildcard scopes cannot. State known recipient retention/deletion assurances and mark unknowns explicitly. If required protection cannot be established, deny the route. Deleting raw media, redacting text or selecting a hash never declassifies an output. Mixed-source outputs satisfy every source's restrictions; `local_only`, `no_cloud_llm`, `no_external_export` and allowed-Device/integration denials remain hard, including on derived content. UI approval cannot substitute for route authority or the existing export permit.\n+\n+Recheck authority, visibility, policy, artifact availability and approval bindings on every read/result release and immediately before egress, serialized with revocation/cancellation. For streamed export, recheck before each released segment and stop unsent bytes on invalidation; emitted bytes cannot be recalled. Changed content, source restrictions, recipient, route, Device or relevant policy versions invalidates approval and requires fresh permitted review. Cancel invalidates unsent approval and queued export immediately; restart cannot replay it. Expiry of material required by the operation denies execution. Retention of a separately authorized derivative does not require recreating its raw source. Report completed, failed, cancelled and delivery-unknown outcomes distinctly; retries use current checks and ordinary idempotency/outcome rules, never another recipient or changed payload under old approval. Relevant stale-sync/projection checks in \u00a71 and disclosure/tool rules in \u00a7\u00a721.3\u201321.4 still apply.\n+\n+Record permitted minimized action, approval/denial, policy, source/artifact lineage and cleanup outcome links through existing review/Log paths. Rejected/dismissed proposals follow \u00a717.0 without a compulsory reason or Preference inference. Raw deletion leaves admitted claims subject to their existing append-only correction/redaction rules and marks their source unavailable; withdrawing a claim is a separate correction/revocation. Purge disallowed reconstructive derivatives, preserving only permitted correction/deletion/gap metadata. Distinguish scheduled, pending, failed and confirmed local cleanup; never imply remote recall or forensic erasure. `UBU-D0328`'s revision-pinned behavioral gates are required before exposing these controls or export routes; this specifies behavior, not shipped enforcement.\n+\n Admission rules:\n \n - A user-confirmed actual action that reconciles a planned interval is recorded with `plan_realized`; if it completes, fails, or moots a Task, the corresponding Task lifecycle Log is also written.\n@@ -3358,7 +3378,7 @@ Evidence review/admission neither grants capture/export capability nor approves\n | New contradiction or operator correction after acceptance, changed use/versions/policy, and delayed model output | The old assessment/approval cannot authorize affected Plan reuse or execution; admitted corrections append history and trigger recalculation, and the late result cannot overwrite them. |\n | Sufficient evidence and admitted candidate, but no external-action approval or a revoked grant; apparent completion without effect evidence | No external dispatch or inferred completion/effect. Evidence sufficiency cannot replace action authorization or `UBU-Q0172`'s remaining proof requirements. |\n \n-`UBU-D0321` specifies qualifier content and fidelity gates; `UBU-D0324` specifies consequence-sensitive corroboration and review; `UBU-Q0166` retains revision-pinned host-mapping evidence and demonstrated schema-gap review. `UBU-D0326` specifies segmentation and scoped local analysis in \u00a721.1.5; `UBU-D0327` specifies raw-media retention defaults in \u00a712.2; `UBU-Q0171` retains operator controls and `UBU-Q0172` retains proof-of-effect semantics. This contract freezes bindings, reuse, corroboration and admission behavior without claiming the remaining questions or implementation acceptance are complete.\n+`UBU-D0321` specifies qualifier content and fidelity gates; `UBU-D0324` specifies consequence-sensitive corroboration and review; `UBU-Q0166` retains revision-pinned host-mapping evidence and demonstrated schema-gap review. `UBU-D0326` specifies segmentation and scoped local analysis in \u00a721.1.5; `UBU-D0327` and `UBU-D0328` specify raw-media retention and operator controls in \u00a712.2; `UBU-Q0172` retains proof-of-effect semantics. This contract freezes bindings, reuse, corroboration and admission behavior without claiming the remaining questions or implementation acceptance are complete.\n \n #### 21.1.2 Restricted-attention audio envelope\n \n@@ -3667,7 +3687,7 @@ Revocation/purge invalidates disallowed handles and removes app-controlled disal\n - Cancellation/revocation during read, inference and result delivery; delayed/duplicate callbacks; source changes, restart and handoff produce no stale release, revived consent or duplicate candidate effects, and release workers/artifacts within the configured bound.\n - Normal completion, expiry, crash/restart and policy purge remove disallowed scratch/caches/derivatives; separately retained evidence remains correctly policy-bound and ordinary review is required before canonical effects.\n \n-These are required gates, not reported passing tests or physical-erasure claims. `UBU-Q0169`'s architecture is answered; numeric latency/resource acceptance remains `UBU-Q0168`, default raw-media retention follows \u00a712.2 (`UBU-D0327`), and operator discard/summary/hash/clip/export controls remain `UBU-Q0171`. Continuous capture cannot be enabled with unresolved effective retention policy. Host fidelity, proof-of-effect semantics and delivery sequence retain their existing gates.\n+These are required gates, not reported passing tests or physical-erasure claims. `UBU-Q0169`'s architecture is answered; numeric latency/resource acceptance remains `UBU-Q0168`, default raw-media retention follows \u00a712.2 (`UBU-D0327`), and operator discard/summary/hash/clip/export controls follow \u00a712.2 (`UBU-D0328`). Continuous capture cannot be enabled with unresolved effective retention policy. Host fidelity, proof-of-effect semantics and delivery sequence retain their existing gates.\n \n ### 21.2 Structured-output admission pipeline\n \ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5186,3 +5186,23 @@ Before enabling continuous capture or durable clip saving, pin implementation, D\n - Purge removes disallowed app-controlled copies and reconstructive derivatives while admitted claims retain permitted append-only links and unavailable-source gaps. Summaries/hashes do not bypass source policy, and local cleanup never claims unverified remote or physical erasure.\n \n These are required behavioral gates, not passing results. `UBU-Q0171` retains detailed operator controls, `UBU-Q0168` measured latency/resource acceptance, `UBU-Q0177` hardware eligibility and `UBU-Q0162` the Log-editability enforcement gap. This decision changes no Phase 1 discovery default, wire schema, kernel API or existing release sequence.\n+\n+---\n+\n+## UBU-D0328: Operator AV controls separate discard, derivative retention and export approval\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a712.2. Resolves `UBU-Q0171`'s operator-control contract; complements `UBU-D0292`, `UBU-D0326` and `UBU-D0327` without enabling capture or an export route.\n+\n+Provide a policy-filtered inventory of capture, available media/derivatives, destinations, retention effects and cleanup/export outcomes. Discard, summary, optional hash, clip saving and export are independent choices. Derivative retention needs its own explicit purpose/destination/retention approval; no substitute archive or export permission follows from capture consent or raw deletion. Summaries remain advisory and hashes can expose correlation.\n+\n+Each permitted export requires review of final content/metadata and operation-bound approval, current source-policy intersection and the authoritative export gate. Bind actor/Device, immutable output, contributing restrictions, destination/route, retention conditions, finite expiry and idempotency. Changes invalidate approval; cancellation/restart cannot replay it. Hard Compartment denials apply to derivatives, previews, logs and every egress route. Preserve permitted append-only correction/deletion/gap metadata, with truthful local versus remote deletion and delivery status.\n+\n+Before enabling these controls, pin implementation, Device/OS/runtime/backend and policy/profile revisions and demonstrate:\n+\n+- Capture/route/retention and artifact inventory remain accurate with models unavailable; pause/stop/discard stay usable. Restricted replicas leak neither denied existence nor payload, identifiers or Compartment labels through previews, counts or diagnostics.\n+- Independent actions, declined saves, failed derivation, source expiry and mixed-source policy enforce bounded preview/retention without automatic summaries/hashes, deadline renewal or hidden copies. Clip selection and padding obey the authorized scope.\n+- No-cloud/no-export/local-only and Device/integration denials hold for raw media, summaries, hashes and metadata on clipboard, share, file and tool routes; hostile media/model text grants no control. Required recipient protections fail closed when unknown or unavailable.\n+- Policy/authority/output/recipient changes between preview, approval, read and egress reject stale approval. Cancellation, delayed callbacks, crash/restart and duplicate/unknown-delivery retries cannot release disallowed content or replay an unsent approval.\n+- Discard and interrupted/failed purge distinguish pending and confirmed local cleanup, remove disallowed reconstructive derivatives and preserve only permitted historical links/gaps without claiming remote recall or erasure.\n+\n+These are required fixtures, not passing results. This resolves controls as a design contract, not implementation or physical deletion; `UBU-Q0168`, `UBU-Q0177` and `UBU-Q0162` retain measured performance, hardware eligibility and Log-enforcement scope. No Phase 1 default, wire schema, kernel API or existing authority gate changes.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3243,23 +3243,23 @@ Resolved by `UBU-D0327`: DESIGN.md \u00a712.2 defines transient raw-buffer age and r\n \n ---\n \n ## UBU-Q0171: Operator controls over discard, summarize, hash, clip and export\n \n-Status: Open Priority: MVP important Phase: Phase 2 Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-12 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 2 Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: UBU-D0328 Last scored: 2026-10-12 Scored from commit: None\n \n ### Question\n \n How does the operator inspect and control raw-AV discard, summaries, hashes, clips and any permitted subsequent export?\n \n ### Current direction\n \n-Delta \u00a717.9. Controls must show capture state, destination and retention effects, obey hard Compartment denials and preserve minimized correction/deletion metadata. Hashes and summaries can still expose sensitive context; no blanket permission to export them follows from deleting raw media.\n+Delta \u00a717.9. `UBU-D0328` specifies policy-filtered capture/artifact inspection and independent discard, summary, optional hash, clip and export controls in DESIGN.md \u00a712.2. Derivative retention needs its own explicit purpose/destination/retention approval; permitted exports require final content/metadata review, operation-bound approval and current authority/policy checks. Hard denials and minimized append-only correction/deletion metadata remain binding.\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0328`: DESIGN.md \u00a712.2 defines inspection, independent artifact actions, separately approved derivative retention and export, version-bound review, cancellation/restart, policy-safe audit and truthful cleanup/delivery status. Summaries and hashes inherit source restrictions; raw deletion never grants export or edits admitted claims. Revision-pinned denial, stale-approval, lifecycle and egress fixtures are required before enablement. This resolves the operator-control contract, not implementation or physical erasure; `UBU-Q0168`, `UBU-Q0177` and `UBU-Q0162` retain measured-performance, hardware and Log-enforcement scope.\n \n ---\n \n ## UBU-Q0172: Evidence compatible with completion versus proof of an effect\n \n",
    "commit_message": "docs: define AV artifact and export controls (UBU-Q0171)",
    "validation_notes": [
      "Unified diff generated from unchanged current working-tree files and proposed contents; repository files were not modified.",
      "git apply --check --whitespace=error - passed against the current working tree; no --cached or --recount was used.",
      "Only UBU-Q0171 changed in OPEN_QUESTIONS.md; its single-line metadata is preserved except Status: Solved and Resolved by: UBU-D0328. Its heading and own Resolution section anchor one hunk.",
      "UBU-D0328 was unused; only the three listed allowlisted files change. Existing ranking/scoring metadata and all other question/decision blocks are preserved.",
      "Design-only proposal: enablement fixtures are requirements, not executed tests or implementation claims. Security decision requires human review."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0171"
    ],
    "decisions_added": [
      "UBU-D0328"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "openai-UBU-Q0171-operator-controls-20261012",
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
