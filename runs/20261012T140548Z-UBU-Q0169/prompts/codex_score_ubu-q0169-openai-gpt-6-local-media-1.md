# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0169`  
Question title: `Local AV segmentation and bounded agent/tool interfaces`  
Base commit: `9f312041296214e0e279ebfd61b4ae5a5ab1342d`

```markdown
## UBU-Q0169: Local AV segmentation and bounded agent/tool interfaces

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-12 Scored from commit: None

### Question

How are local audio/video streams segmented, and what scoped interfaces expose frame extraction, event detection and relevant-section isolation?

### Current direction

Delta §17.7. Decide buffering, timestamps, cancellation, source boundaries, access grants and retention of intermediate artifacts. Tools receive only purpose-required media/context; interpreted scene text is data, never a capability grant. No MCP or agent interface is implemented by this record.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0169-openai-gpt-6-local-media-1",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0169",
    "base_commit": "9f312041296214e0e279ebfd61b4ae5a5ab1342d",
    "summary": "Define bounded local AV segmentation and purpose-scoped frame extraction, event detection and section isolation.",
    "rationale": "A local broker separates capture consent from per-job access. Immutable source chunks, explicit time and coverage provenance, scoped grants, bounded resources and artifact leases constrain analysis without granting models authority. Existing questions retain numerical acceptance, raw-retention defaults, operator controls and implementation evidence.",
    "changed_files": [
      "DESIGN.md",
      "DECISIONS.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -2073,11 +2073,11 @@ Allowed Phase 1 discovery inputs are intentionally narrow:\n - coarse user-defined location category or geofence event, not continuous raw location by default;\n - device state needed for interpretation, such as screen on/off or network/offline state.\n \n Phase 1 discovery excludes covert or broad capture by default: continuous microphone, camera, screen recording, keystroke logging, raw message bodies, raw file contents, raw GPS trails, and cross-Identity sharing are outside the default discovery input set. Any later use of those sources requires a separate explicit mode, Compartment review, routing disclosure, and user approval.\n \n-**AV refinement for the MVP interaction surface (`UBU-D0292`).** The primary camera-and-voice surface does not enable continuous sources implicitly. A consented capture session remains visible, source-scoped, pausable and inspectable under the clause above. Process raw AV locally on the operator's own hardware, minimize context and retain only material justified by its purpose. Selective clips, summaries, hashes or deletion are operator-controlled under Compartment policy; subsequent export needs its own allowed route and approval. No-cloud/no-export denials remain hard. Exact defaults, segmentation and deletion mechanics remain open in `UBU-Q0169`\u2013`UBU-Q0171`; recording a policy does not claim its enforcement has shipped.\n+**AV refinement for the MVP interaction surface (`UBU-D0292`).** The primary camera-and-voice surface does not enable continuous sources implicitly. A consented capture session remains visible, source-scoped, pausable and inspectable under the clause above. Process raw AV locally on the operator's own hardware, minimize context and retain only material justified by its purpose. Selective clips, summaries, hashes or deletion are operator-controlled under Compartment policy; subsequent export needs its own allowed route and approval. No-cloud/no-export denials remain hard. Segmentation and bounded local interfaces follow \u00a721.1.5 (`UBU-D0326`); raw-media retention defaults and operator discard/export controls remain open in `UBU-Q0170`\u2013`UBU-Q0171`; recording a policy does not claim its enforcement has shipped.\n \n Selective raw-media retention and append-only evidence are distinct. Discarding a raw stream or deleting an authorized clip does not edit an admitted historical claim in place: append minimized correction, revocation, deletion or tombstone metadata with the appropriate links. The immutable Log retains evidence about what was asserted, reviewed or withdrawn, not an automatic reconstructible copy of private raw media. A hash is not proof of an event and can itself leak correlation; policy must govern it. Later views can mark deleted evidence unavailable without inventing a replacement observation. `UBU-Q0162` tracks the reported Log-editability enforcement gap.\n \n Admission rules:\n \n@@ -3346,11 +3346,11 @@ Evidence review/admission neither grants capture/export capability nor approves\n | Consequential inference with high confidence, two models using one frame, copied reports or unknown lineage; then an independently obtained matching reading/inspection | The first set cannot satisfy independent corroboration. The new evidence preserves ancestry and may support a freshly bound review, without automatic admission or execution. |\n | Independent but stale/inconclusive evidence, contradictory target/value/time, denied evidence or unresolved identity | The affected claim/use remains unresolved with policy-safe alternatives; no source voting, capacity release, precondition satisfaction or dependent dispatch. |\n | New contradiction or operator correction after acceptance, changed use/versions/policy, and delayed model output | The old assessment/approval cannot authorize affected Plan reuse or execution; admitted corrections append history and trigger recalculation, and the late result cannot overwrite them. |\n | Sufficient evidence and admitted candidate, but no external-action approval or a revoked grant; apparent completion without effect evidence | No external dispatch or inferred completion/effect. Evidence sufficiency cannot replace action authorization or `UBU-Q0172`'s remaining proof requirements. |\n \n-`UBU-D0321` specifies qualifier content and fidelity gates; `UBU-D0324` specifies consequence-sensitive corroboration and review; `UBU-Q0166` retains revision-pinned host-mapping evidence and demonstrated schema-gap review. `UBU-Q0169`\u2013`UBU-Q0171` retain capture/retention mechanics; `UBU-Q0172` retains proof-of-effect semantics. This contract freezes bindings, reuse, corroboration and admission behavior without claiming the remaining questions or implementation acceptance are complete.\n+`UBU-D0321` specifies qualifier content and fidelity gates; `UBU-D0324` specifies consequence-sensitive corroboration and review; `UBU-Q0166` retains revision-pinned host-mapping evidence and demonstrated schema-gap review. `UBU-D0326` specifies segmentation and scoped local analysis in \u00a721.1.5; `UBU-Q0170`\u2013`UBU-Q0171` retain raw-media retention defaults and operator controls; `UBU-Q0172` retains proof-of-effect semantics. This contract freezes bindings, reuse, corroboration and admission behavior without claiming the remaining questions or implementation acceptance are complete.\n \n #### 21.1.2 Restricted-attention audio envelope\n \n **Embodied Audio Interaction (`UBU-D0309`).** Activity mode is separate from the realtime session's source/routing mode and from `compartment_ids`: for example, `Mode: Driving` with `Compartment: UbU Project / Public Outreach`. Activity constrains interaction; Compartment policy constrains disclosure and authority. Neither grants permission supplied by the other.\n \n@@ -3617,10 +3617,50 @@ Offline operation uses available permitted local backends and manual routes; no\n \n **Measured enablement gate (`UBU-Q0168`).** Select numeric ceilings only from revision-pinned evidence on each proposed Device/backend/mode; record OS/runtime/model/template, implementation and policy revisions, source/context sizes, load and instrumentation. Measure controller acceptance, acknowledgment, first useful result, completion, observation age at presentation, stream gaps and stop/output-quiescence separately from admission, first CPU-certified planner chunk and final planner response. Report sample counts, median and tail distributions, misses, errors/degraded outcomes, memory/storage, battery/energy where observable, and sustained thermal behavior; unavailable metrics stay explicit. Report successful interpretations separately from unresolved/degraded outcomes; latency alone establishes neither correctness nor usefulness.\n \n Exercise cold and warm operation, competing work and sustained streams; offline/backend loss, low battery and thermal/resource exhaustion; interruption during acquisition/inference/output; queue overload and deadline expiry; source/context changes, corrections, revocation, restart/handoff and delayed/duplicate results. Demonstrate bounded waiting/backlog, working manual fallback, truthful saved/coverage/freshness status, no stale admission/dispatch and no hidden rerouting. Publish the selected finite ceilings and demonstrated fallback for each enabled class; an unmeasured class remains unavailable. Hardware selection remains `UBU-Q0177`, restricted-attention numeric profiles remain `UBU-Q0128`, and delivery sequence remains `UBU-Q0183`. `UBU-Q0168` stays Open for these numerical profiles and measured acceptance; this contract asserts no implementation, supported hardware or achieved latency.\n \n+#### 21.1.5 Local AV segmentation and scoped analysis\n+\n+**Architecture contract (`UBU-D0326`).** Adopt a controller-owned local media broker with the logical adapter profile `local-media/1`. Session consent enables only named sources on eligible execution-enclave Devices on the operator's own hardware; sharing hardware grants no access between enclaves. Capture controls remain visible under \u00a712.2 and \u00a721.1.3. Models and agents can request analysis of supplied media spans, but cannot start/resume capture, enumerate sources, renew consent or enlarge a grant. This profile specifies an interface boundary, not an implemented MCP service, frozen wire-schema change or new planner entity.\n+\n+**Segmentation and buffering.** Maintain a separate bounded transient ring for each consented source epoch. Seal immutable chunks at the first configured duration or byte ceiling, or earlier on pause, stop or a source/consent/Identity/Compartment boundary. Bounded chunks are the storage/read units; utterances and detected events are separate span references that may cross chunks only within authorized continuous coverage. A detector's proposed boundary cannot extend acquisition or retention. One-shot requests end acquisition at their requested bound; they never become continuous sessions.\n+\n+Before enabling a source, pin a controller-owned Device/backend/mode profile with positive finite chunk duration/bytes, raw-buffer age/bytes, total raw/decoded/intermediate memory, queue count/age, concurrency/calls, decode work, frame dimensions/count, event/span/result sizes, job deadline, cancellation grace and artifact/scratch lifetime. Use \u00a721.1.4's measured deadline/resource gate and current source-retention policy; missing limits deny acquisition/dispatch. Include pinned chunks and worker copies in aggregate accounting. Evict only eligible unpinned transient material and record coverage gaps; pins cannot extend retention or exceed quotas. If eligible eviction cannot satisfy limits, pause acquisition and cancel/defer affected work visibly. Never spill to unapproved storage, silently discard acknowledged durable evidence or reroute to cloud processing.\n+\n+**Timeline and coverage.** Each sealed chunk/span manifest binds session, source, Device, source epoch, sequence, immutable content/version reference, half-open interval `[start, end)`, actual sample/frame positions, capture provenance, policy/retention versions and permitted gaps. Preserve media sample/PTS offsets separately from acquisition monotonic offsets, with their clock mapping, wall-time anchor and uncertainty when known. Capture elapsed time and live deadlines use a local monotonic clock; historical intervals retain their original time basis. Ingestion, interpretation or emission times cannot replace observation time. Restart, source replacement and clock discontinuity create new epochs. Unknown mappings stay unknown: do not align different sources or Devices by timestamp equality, sequence or nominal frame rate. Audio/video alignment requires explicit permitted clock mappings and uncertainty. All selectors state the source epoch, time basis and units; reject nonfinite, reversed or out-of-range bounds. Missing, dropped, expired or sampled-out intervals never establish silence, no event or a continuous observation.\n+\n+**Access and invocation.** Effective access is the intersection of current session/source consent, actor and adapter Device eligibility, capability grant, declared Task/Objective/workflow purpose, source policy and finite profile. The grant names allowed operations, sources/epochs and read intervals, context selectors, recipient adapter, expiry and aggregate work/output limits. Opaque handles are audience- and scope-bound references, not authority by possession; recheck current policy on every dereference. Broker-mediated reads and worker isolation must prevent ambient camera/microphone, unrelated filesystem/context, network and credential access. Physical locality alone does not satisfy this boundary.\n+\n+Bind each request to its id, actor/grant and versions, session/source epochs, pinned manifest/evidence/context versions, operation/finite selectors, requested read/output intervals, minimized ContextBundle, adapter/model/profile revisions, policy versions, job generation, monotonic deadline and idempotency key. Check these before reading, invoking and releasing each result. Requests may select only supplied permitted aliases; arbitrary paths, URLs, device discovery, executable instructions and unbounded selectors are unavailable. Current denials fail closed with policy-safe errors that do not reveal denied existence or Compartment identity. An exact retry shares the original budget/deadline and idempotency outcome; changed-content key reuse rejects. A retry cannot reacquire expired media or widen scope; expired result artifacts return unavailable rather than a cached disclosure.\n+\n+A bounded result envelope echoes the request/generation and original manifest bindings, reports complete/partial/unavailable/cancelled/denied status, analyzed/missing coverage, permitted artifact refs/expiry, work used and transform/tool/model provenance. Validate size, profile, references, intervals and bindings before release; malformed or oversized results reject without repair into a usable output. Errors and metadata obey the same disclosure boundary as media.\n+\n+| Logical operation | Required bounded input | Output and boundary |\n+|---|---|---|\n+| `media.frames.extract` | Pinned source spans plus explicit timestamps or sampling interval, finite frame count, dimensions and optional crop within the grant. | Selected frame payloads/leased handles with original sample positions, actual observation times, transform provenance and gaps. No fabricated/interpolated frames or reads outside the authorized read scope. |\n+| `media.events.detect` | Pinned audio/video spans, an allowed detector/label set and finite window/work/event limits; only purpose-required context. | Advisory event hypotheses with source spans, detector/model revision, confidence/uncertainty and analyzed/missing coverage. No detection means only no reported match in analyzed coverage; it is not evidence of real-world absence, completion or an effect. |\n+| `media.sections.isolate` | Explicit subintervals or validated event-span references within already authorized sources, with finite section count, duration and bytes. | A manifest of selected spans and, when authorized, a leased local clip handle. Relevance is a proposed selection, not a capability grant; suggestions cannot fetch adjacent or new sources outside the original scope. |\n+\n+Codec dependency frames or audio padding may be decoded internally only within the declared authorized read scope and decode budget; return only the requested output interval and disclose any permitted padding/coverage limitation. Otherwise return unavailable/partial coverage rather than silently broadening the read. Multi-source results carry every parent span and satisfy the intersection of their disclosure/retention policies. Overlapping windows and derived copies preserve sample lineage and cannot count as independent corroboration.\n+\n+**Cancellation and revocation.** Serialize pause/stop, source or policy change, grant expiry/revocation, job-generation invalidation, reads and result release in the controller. Invalidation immediately prevents further reads and acceptance of late callbacks; stop affected acquisition and request backend cancellation without waiting for acknowledgment. After the finite cancellation grace, terminate the isolated worker if needed and reclaim its leases/resources. An adapter unable to enforce isolation and bounded release remains unavailable. Resume/restart/handoff uses fresh local consent and bindings; synchronized candidates and persisted handles never restore live access. Later review of permitted retained evidence is a new bounded job, not revival of the cancelled one.\n+\n+**Intermediate artifacts and audit.** Decoded frames/audio, crops, thumbnails, temporary clips, transcripts, embeddings, prompts and model caches inherit source restrictions. Scratch expires on completion/cancellation; returned temporary artifacts have a finite non-renewing result lease bounded by applicable source policy. Any permitted disk scratch is policy-protected, inventoried with expiry and cleaned before reuse after restart; there is no implicit durable spool, training use or general-purpose cache. Explicit or already-scoped policy authorization is required to retain selected evidence beyond its temporary lease, through ordinary Discovery/candidate storage with lineage and its own allowed purpose/retention. Reads and retries cannot silently renew raw/intermediate retention.\n+\n+Revocation/purge invalidates disallowed handles and removes app-controlled disallowed copies and reconstructive derivatives under current policy. Already-admitted history follows ordinary append-only correction/redaction rules; raw expiry does not edit a historical claim or prove it false. Record permitted minimized invocation, source-span, policy, budget, gap, cancellation/cleanup and candidate links rather than mandatory raw media, transcripts or hashes. Scene/OCR/transcript text is untrusted data, never an instruction, permission or grant. Results use \u00a721.1.1's Discovery/advisory bindings and ordinary review/admission; they cannot directly mutate canonical state, certify a Plan or authorize external action. Export or a different Device/recipient requires its separately permitted route and approval under \u00a7\u00a721.3\u201321.4.\n+\n+**Enablement evidence.** Before exposing an adapter, pin implementation, OS/runtime/backend/model and policy/profile revisions and demonstrate:\n+\n+- Variable frame rate, audio/video clock uncertainty, timestamp discontinuity, chunk-crossing events, overlapping samples and dropped/expired spans preserve original times, lineage and explicit analyzed/missing coverage.\n+- Forged/foreign/expired handles, wrong purpose/actor/Device, mixed-policy sources, unbounded selectors, codec dependencies outside scope and hostile scene/transcript instructions expose no unauthorized media/context or tool capability.\n+- Sustained capture, slow consumers, oversized decode/output, pinned-buffer exhaustion and disk pressure respect aggregate limits, pause/defer visibly, preserve permitted saved evidence and leave manual/stop controls usable without cloud fallback.\n+- Cancellation/revocation during read, inference and result delivery; delayed/duplicate callbacks; source changes, restart and handoff produce no stale release, revived consent or duplicate candidate effects, and release workers/artifacts within the configured bound.\n+- Normal completion, expiry, crash/restart and policy purge remove disallowed scratch/caches/derivatives; separately retained evidence remains correctly policy-bound and ordinary review is required before canonical effects.\n+\n+These are required gates, not reported passing tests or physical-erasure claims. `UBU-Q0169`'s architecture is answered; numeric latency/resource acceptance remains `UBU-Q0168`, default raw-media retention remains `UBU-Q0170`, and operator discard/summary/hash/clip/export controls remain `UBU-Q0171`. Continuous capture cannot be enabled with unresolved effective retention policy. Host fidelity, proof-of-effect semantics and delivery sequence retain their existing gates.\n+\n ### 21.2 Structured-output admission pipeline\n \n LLM, parser, worker, and agent outputs should follow an admission pipeline:\n \n ```text\ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5152,5 +5152,19 @@ Require the pinned behavioral gates in DESIGN.md \u00a721.1.1 before enabling these\n Adopt `snapshot_capture`, `conversational`, `streamed_supervision` and `retrospective` classes with controller-owned, versioned Device/backend/mode profiles. Separate acknowledgment, first useful result, total-job, context-age and stream-gap/backlog ceilings. Include queue time, enforce finite resource limits and monotonic deadlines, and expose a bounded fallback on expiry. Receipt, durable saving, candidate admission, CPU-certified planning and proof of execution remain distinct.\n \n Recheck original evidence/context/version/policy bindings before current guidance, admission or dispatch. Offline, battery and thermal pressure defer or reduce permitted work and preserve manual routes and validated cached planning. Interruption invalidates pending output/action bindings immediately; late results cannot overwrite corrections, resume capture, use old approval or trigger duplicate/unknown external effects. Privacy, attention, corroboration and hard-validation rules continue to govern every class.\n \n Require DESIGN.md \u00a721.1.4's revision-pinned timings, sustained-load measurements and degradation fixtures before enabling each profile. No numerical latency, supported-hardware, safety-critical supervision or implementation claim is established, and no kernel API or wire-schema change is selected. `UBU-Q0168` remains Open until finite numerical ceilings and their measured behavior are reviewed.\n+\n+---\n+\n+## UBU-D0326: Local AV uses bounded source chunks and capability-scoped analysis\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a721.1.5. Resolves `UBU-Q0169`'s architecture contract; complements `UBU-D0292`, `UBU-D0320` and `UBU-D0325` without enabling a producer or MCP service.\n+\n+Adopt the controller-owned `local-media/1` broker boundary. Segment each consented source epoch into immutable duration/byte-bounded chunks backed by a finite transient ring; preserve actual sample times, clock mappings/uncertainty, source lineage and explicit gaps. Event/utterance spans may reference multiple authorized chunks without widening capture or retention. Finite profiles cover queues, aggregate raw/decoded/intermediate resources, decode/output, work/deadlines and cleanup; overload pauses/degrades visibly instead of spilling or silently extending retention.\n+\n+Expose only `media.frames.extract`, `media.events.detect` and `media.sections.isolate` over supplied, purpose-required spans/context. Current consent, actor/Device eligibility, scoped grants and Compartment policy intersect on every read and result release. Handles confer no authority; isolated local workers have no ambient source, storage, network or credential access. Agent requests, relevance suggestions and scene/OCR/transcript text cannot acquire sources, expand grants or authorize effects.\n+\n+Serialize cancellation/revocation with access and result acceptance, invalidate pending generations immediately and bound worker/resource release. Keep intermediates under finite leases, clean disallowed scratch/caches/derivatives on expiry, cancellation, purge and restart, and require separate allowed evidence-retention or export authority. Results remain Discovery/advisory evidence subject to existing fidelity, corroboration, review and admission rules; admitted history retains ordinary correction/redaction semantics.\n+\n+Require \u00a721.1.5's pinned segmentation, access-denial, resource, interruption and artifact-lifecycle fixtures before enablement. This resolves an architectural contract, not implementation, host compatibility, measured performance or physical deletion. `UBU-Q0168` retains numeric latency/resource acceptance, `UBU-Q0170` raw-media retention defaults, `UBU-Q0171` operator controls, `UBU-Q0166` host fidelity, `UBU-Q0172` proof-of-effect semantics and `UBU-Q0183` delivery sequence.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3207,23 +3207,23 @@ Open. `UBU-D0325` answers the class/degradation architecture; numerical profiles\n \n ---\n \n ## UBU-Q0169: Local AV segmentation and bounded agent/tool interfaces\n \n-Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-12 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: UBU-D0326 Last scored: 2026-10-12 Scored from commit: None\n \n ### Question\n \n How are local audio/video streams segmented, and what scoped interfaces expose frame extraction, event detection and relevant-section isolation?\n \n ### Current direction\n \n-Delta \u00a717.7. Decide buffering, timestamps, cancellation, source boundaries, access grants and retention of intermediate artifacts. Tools receive only purpose-required media/context; interpreted scene text is data, never a capability grant. No MCP or agent interface is implemented by this record.\n+Delta \u00a717.7. `UBU-D0326` specifies source-epoch chunking, bounded local buffering, original timestamps/coverage, purpose-scoped frame/event/section operations, revocation/cancellation and intermediate-artifact leases in DESIGN.md \u00a721.1.5. Raw-media retention defaults and operator controls remain `UBU-Q0170`\u2013`UBU-Q0171`; numeric latency/resource acceptance remains `UBU-Q0168`. No MCP or agent interface is implemented by this record.\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0326`: DESIGN.md \u00a721.1.5 defines the `local-media/1` broker architecture, immutable bounded source chunks, clock/coverage provenance, capability- and purpose-scoped analysis, current-policy access checks, cancellation/revocation and intermediate-artifact lifecycle, with required enablement fixtures. Results remain Discovery/advisory evidence; interpreted media cannot grant capabilities or admit state. This resolves the architecture, not implementation or measured acceptance; `UBU-Q0168`, `UBU-Q0170`\u2013`UBU-Q0171`, host-fidelity and proof-of-effect gates retain their existing scope.\n \n ---\n \n ## UBU-Q0170: Default raw-media retention for consented perception\n \n",
    "commit_message": "docs: define bounded local AV segmentation and analysis interfaces",
    "validation_notes": [
      "Generated the diff from current working-tree contents and proposed contents, preserving the working-tree baseline rather than using HEAD or the index.",
      "git apply --check --whitespace=error passed against the current working tree; repository files remain unchanged.",
      "Schema constraints and all five hunk ranges were validated; applying the hunks in memory reproduced the proposed contents.",
      "Only UBU-Q0169's metadata, current direction and own Resolution change in OPEN_QUESTIONS.md; ranking/scoring metadata and all other question blocks are preserved.",
      "UBU-D0326 and DESIGN.md \u00a721.1.5 were unused; no new questions or frozen contract/schema changes are proposed.",
      "No AV implementation or hardware behavior is claimed; documented fixtures and numerical/retention gates remain required before enablement."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0169"
    ],
    "decisions_added": [
      "UBU-D0326"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0169-openai-gpt-6-local-media-1",
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
