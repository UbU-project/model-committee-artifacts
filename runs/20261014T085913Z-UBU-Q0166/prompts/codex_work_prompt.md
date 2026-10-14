# Model-Committee Work Proposal Request

You are participating in the UbU `model-committee` process.

Your task is to produce one concrete work proposal as strict JSON.

Do not return prose outside the JSON object.

## Selected question

Question ID: `UBU-Q0166`  
Question title: `Preserve perceptual qualifiers alongside the closed provenance enum`  
Repository working tree: `/home/sean/ubu-design`

HEAD commit (provenance only): `37e41e0e0bc060d7b5eaed151d4459c13f57432f`

```markdown
## UBU-Q0166: Preserve perceptual qualifiers alongside the closed provenance enum

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-14 Scored from commit: None

### Question

Which revision-pinned host mappings preserve the claim-level qualifications specified by `UBU-D0321` through validation, serialization, storage, review and authorized exchange, and does a failed mapping demonstrate a schema gap requiring separate versioned review?

### Current direction

Delta §17.4. `UBU-D0293` decides kind mapping; `UBU-D0320` decides observation/admission bindings; `UBU-D0321` specifies claim-level qualifier content, field/evidence links and fidelity fixtures. The remaining work is a revision-pinned host field/payload mapping and round-trip evidence from the actual validators, serializers, store/review and exchange paths. Measured remains an instrument/reading, derived a computation from facts, and proposed an unconfirmed candidate. Do not infer host compatibility from prose payload slots. A demonstrated gap requires separate versioned chain review; no schema, pin or migration change is made here. `UBU-D0323` records bounded candidate link-carrier checks and source-inspected downstream limitations. Full claims/qualifiers and resolved evidence revisions still need fixtures through the actual Discovery, normalization, store/review, admission and authorized-exchange paths. A rejected direct field mapping alone does not establish a schema gap.

`UBU-D0334` extends those probes to actual persistence/reload/replay and direct review API reads, with a minimal runtime reproduction of inline qualifier loss in `precondition_review::vet_result`. The rejected normalized-proposal alternative and this implementation loss do not establish that all compliant existing payload/ref mappings fail. Repair or validate that branch before relying on it; the full qualifier/evidence and policy-authorized paths remain the enablement gate.

### Resolution

Open. `UBU-D0321` answers preservation requirements; `UBU-D0323` records the initial synthetic carrier probes. `UBU-D0334` adds actual direct SQLite close/reopen/replay and review API controls, and reproduces qualifier loss in the tested precondition-review normalization branch despite a native `ok` result. This disqualifies that mapping through that branch, without establishing a governing schema gap. Complete Discovery claim/qualifier mappings, adapter profile/ref validation, faithful normalization and rendered review, admission and authorized-exchange fixtures remain outstanding. Close only with the full pinned mapping and fixture evidence, or a minimally reproduced host-contract gap routed to a separate versioned change. No passing adapter or actual schema gap is established.

---

```

## Patch baseline

The patch must apply to the **current working-tree files** in the repository above.
The selected question and context excerpts come from that working tree, which may
include uncommitted ranking and metadata updates.

- Set the JSON `base_commit` field to `37e41e0e0bc060d7b5eaed151d4459c13f57432f` for provenance. It does not
  define the patch's file contents: do not use `git show HEAD:<path>` or the Git index
  as the original version when it differs from the working tree.
- Use exact working-tree lines for all removed and unchanged lines. Preserve existing
  metadata updates and exclude unrelated edits from the proposal.
- To generate the diff, compare unmodified working-tree copies with the proposed
  contents. Validate against the working tree with `git apply --check`; a successful
  `git apply --check --cached` checks the index and is insufficient.

## Canonical design context

The excerpts below are **not** the whole canonical files. They are the subset selected by
following references from the selected question: its dependencies, the decisions those
questions cite, and the sections those cite in turn.

Read them under these rules:

- **Absence is not non-existence.** Ids and sections you cannot see here still exist.
  Never conclude that a question is unasked, a decision unmade, or a section absent
  because it is not shown.
- **Do not renumber or reuse ids.** Allocate new entries from the next free ids below.
- **Patch context must match the real file.** Hunks are applied against the full files,
  not against these excerpts, and `git apply` locates them by surrounding context. Quote
  surrounding lines exactly as they appear here.
- **Scope your edits to what you can see.** Do not delete or rewrite text on the
  assumption that it duplicates something outside these excerpts.

Next free ids — use these for anything you add:

- next question id: `UBU-Q0191`
- next decision id: `UBU-D0360`

`DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md` is read-only context and is not in the patch
allowlist: do not propose changes to it.

### DECISIONS.md — cited decisions

#### UBU-D0293

```markdown
## UBU-D0293: Perceptual evidence maps onto the existing provenance enum

**Status:** Accepted → DESIGN.md §11.2, §21.1; MULTIMODAL_INTERACTION.md. Refines `UBU-D0132` as amended by `UBU-D0292`.

Keep the closed `ProvenanceKind` vocabulary `asserted / measured / derived / proposed`. The perceptual vocabulary describes evidence and review operations, not an ordered confidence ladder and not new enum variants. The mapping is explicit and conditional on how a value was established:

| Perceptual term | Existing kind and admission meaning |
|---|---|
| `observed` | An unconfirmed visual interpretation is a `proposed` candidate. An actual instrument reading can support `measured`; a person's report of what they saw is `asserted`. Observation alone does not admit state. |
| `inferred` | An unconfirmed perceptual value is `proposed`. `derived` is reserved for a computation from other facts, with its inputs and derivation evidenced; a model's guess is not such a computation. |
| `operator_confirmed` | An admitted personal assertion is `asserted`; an operator recording an instrument reading may choose `measured`. Confirmation does not automatically make an inference a measurement. |
| `externally_verified` | Verification is an evidence qualifier. An attested statement maps to `asserted`, an instrument reading to `measured`, or a computation from facts to `derived`, according to the underlying source. Verification alone neither selects a kind nor admits a value. |
| perceptual `measured` | Map to enum `measured` only for an instrument or reading. The proposed ladder's strongest-confidence meaning is not adopted. |

The enum member is `ProvenanceKind::Proposed`, stored on `FactProvenance.kind`; `FactProvenance` is a struct, not an enum. It remains unused by the current advisory value policy: existing advisors propose no fact values (`UBU-D0296`). A future unconfirmed perceptual value is the first legitimate prospective use of that member. Such a value stays in candidate state; it is not a write into canonical UniverseState. This records a future mapping, not an implemented perception producer or permission for existing advisors to propose values.

Keep confidence, observation source, corroboration and verification qualifiers in existing candidate/evidence records, including `DiscoveryEvidenceItem`, and link admission to that evidence. Do not add fields to `FactProvenance`, whose current shape is only `kind` and `recorded_at`. Exact evidence sufficiency and admission profiles remain questions (`UBU-Q0165`, `UBU-Q0166`, `UBU-Q0167`, `UBU-Q0172`). No enum, schema, pin or migration changes here; `UBU-D0291`'s vocabulary governance is preserved.

---
```

#### UBU-D0320

```markdown
## UBU-D0320: Observation bindings reuse Discovery evidence and advisory admission

**Status:** Accepted → DESIGN.md §21.1.1. Resolves `UBU-Q0165`; specifies the payload/admission contract without enabling a perception producer or changing existing advisor authority.

Adopt the `observation-admission/1` binding profile and required fixtures in DESIGN.md §21.1.1. Capture and unresolved interpretations use existing `DiscoveryEvidenceItem` records; supported native proposals use per-object `AdvisoryCandidate` records, lifecycle/review events and ordinary CPU admission. Bind source/session/evidence/context refs, ambiguity alternatives, proposed-object/value dependencies, observed versions and policies, corrections and separately scoped replay keys. Unsupported or lossy mappings stay unresolved and require separate versioned review.

Operator review binds the exact diff, resolved targets and dependency batch. Admission rechecks those bindings and current authority/policy, then atomically records canonical changes, ordinary review/Log links and idempotency. Candidate/evidence storage never admits world state or authorizes projection/external action. Exact replay has one effect; changed-content key reuse rejects; stale results and corrections require fresh review and append-only history.

`FactProvenance` remains only `{kind, recorded_at}` under `UBU-D0293`, and `UniverseTarget` advice remains name-only under `UBU-D0296`. Qualifier fidelity, corroboration sufficiency, capture/retention mechanics and proof of effects remain under `UBU-Q0166`, `UBU-Q0167`, `UBU-Q0169`–`UBU-Q0172`. The fixtures are implementation gates, not reported passing results.

---
```

#### UBU-D0321

```markdown
## UBU-D0321: Perceptual qualifiers retain claim-level evidence and source history

**Status:** Accepted → DESIGN.md §21.1.1. Narrows `UBU-Q0166`; specifies preservation requirements alongside `UBU-D0293` and `UBU-D0320` without asserting host compatibility or enabling a producer.

Adopt the companion `perceptual-qualifiers/1` payload requirements over existing Discovery evidence payloads and advisory evidence/per-field slots. Preserve scoped claims, separately attributed observation/inference/confirmation/verification entries, confidence meaning, source basis, contradictions and source ancestry. Map each proposed field to exact evidence revisions and claim keys. Confirmation and verification create linked evidence rather than relabeling an original inference; admission links the reviewed candidate/field/evidence versions to ordinary review and canonical-effect records.

Keep `ProvenanceKind` and `{kind, recorded_at}` FactProvenance unchanged. Actual assertions, instrument readings and computations from facts determine admitted kind; confidence and verification labels confer no admission or external-action authority. `UniverseTarget` remains name-only. Preservation is bounded by current disclosure/retention policy, with explicit permitted gaps after purge or partial replication.

Require the revision-pinned host-path inventory and fidelity fixtures in DESIGN.md §21.1.1 before enabling an adapter. `UBU-Q0166` remains open for that compatibility evidence; no actual schema gap or passing mapping is claimed. Unsupported/lossy mappings stay unresolved. A demonstrated gap requires a separate reviewed versioned schema/type/store chain change; no schema, pin or migration changes here. Corroboration sufficiency and proof of effects remain `UBU-Q0167` and `UBU-Q0172`.

---
```

#### UBU-D0323

```markdown
## UBU-D0323: Candidate carrier probes leave perceptual host compatibility open

**Status:** Accepted → DESIGN.md §21.1.1. Narrows `UBU-Q0166`; records bounded revision-pinned representation evidence without enabling an adapter.

Record the pinned inventory, synthetic link specimen and actual schema/Rust outcomes in DESIGN.md §21.1.1. An inline candidate payload preserved that specimen; placing the structured specimen in string-valued `field_provenance` rejected. Generic schema/type validation also accepted an unknown qualifier profile, so it cannot substitute for the adapter's required profile and claim/version checks. These probes establish only candidate link representation, not full claim/qualifier preservation, evidence resolution or host-path compatibility.

A failed mapping alone does not demonstrate a schema gap. Report the first failing pinned path and exact rejected or changed content. Distinguish an unsuitable field choice, missing adapter validation or normalization/store/review defect from a governing host-contract restriction that no compliant existing payload/ref mapping can satisfy. Only the latter demonstrated gap warrants the separate versioned schema/type/store chain review required by `UBU-D0321`.

`UBU-Q0166` remains Open for complete mappings and actual normalization, evidence, store/review, admission and authorized-exchange fixtures. Source-inspected downstream limitations are not reported runtime outcomes. No schema, enum, FactProvenance field, dependency pin, migration or producer authority changes here; `UniverseTarget` remains name-only.

---
```

#### UBU-D0334

```markdown
## UBU-D0334: Pinned probes reproduce qualifier loss during precondition review normalization

**Status:** Accepted → DESIGN.md §21.1.1. Narrows `UBU-Q0166`; extends `UBU-D0323` with bounded runtime evidence without enabling an adapter.

Record the synthetic specimen, pinned builds and actual normalization, SQLite close/reopen/replay and review API controls in DESIGN.md §21.1.1. The inline link specimen survives direct candidate storage and API reads when normalization is bypassed. The tested `precondition_review::vet_result` branch instead returns `ok` while rebuilding the payload without that specimen; adding it to the closed native normalized proposal rejects. An unknown companion profile is also discarded by that branch rather than receiving the required adapter rejection.

These results disqualify the tested inline mapping through that normalization branch. They establish a current implementation loss, not a governing host-contract gap across all compliant existing payload/ref mappings. A future adapter using this path must demonstrate preservation or leave the interpretation unresolved before executable review; native `ok`, durable candidate storage and API retention cannot substitute for claim/version/profile validation or rendered qualifier review.

`UBU-Q0166` remains Open for complete Discovery claim/evidence mappings, adapter validation, faithful normalization and presentation, admission and authorized-exchange fixtures. Synthetic refs remain unresolved; pool close/reopen is not a process/crash-recovery test, and direct API calls test neither HTTP authorization nor disclosure middleware. No schema, enum, FactProvenance field, pin, migration or producer authority changes here; `UniverseTarget` remains name-only.

---
```

### DESIGN.md — referenced sections

#### DESIGN.md §11.2 — UniverseState values

```markdown
### 11.2 UniverseState values

MVP value discipline:

- fact keys are free-form strings;
- default keys should use a lightweight namespace convention;
- values are text / JSON-like payloads.

**Namespace convention (`UBU-D0243`, amended by `UBU-D0291`).** A target is `<collection>.<subject>(.<entity-path>)?.<predicate>`. The first segment after the collection is a permanent top-level taxonomy referenced by every fact, precondition and mutation, so it remains governed and reviewed. Its effective vocabulary is the governed set union one visible operator-authored provisional registry. The governed set remains `operator`, `project`, `github`, `affect`, `relationship`, and only a recorded amending decision extends it. Minting a provisional root is an explicit operator act, never a side effect of recording a fact and never available to an advisor; producers may use only the effective vocabulary. Every root is a snake_case singular noun naming an entity or domain, never an instance, attribute, provenance/source or reverse-DNS authority prefix. Predicates and entity paths retain their existing rules. `affect` retains its reserved meaning, and `UBU-D0242`'s intrinsic-affect rejection keys off `<subject> == affect` unchanged.

Every provisional root must be ratified by an amending decision or retired before the switch; the registry is that agenda. The store is disposable until the switch, making that the last cheap rename point before UniverseState keys, fact_provenance keys and Task precondition strings require rewriting without migration tooling. No root is proposed here. Registry, minting and validator/advisor enforcement are follow-up orchestrator work, not implemented by this ticket.

The Phase 1b Tasks form reads requirements in words and authors a single leaf. Boolean trees remain readable and clearable, and authorable through the existing routes; a tree editor is outside the form's scope.

A stricter ontology can be added later.

**Human authoring and target advice (`UBU-D0296`, `UBU-D0300`).** The UniverseState form offers `asserted` for a person's statement and `measured` for an instrument/reading; it does not offer `derived` or `proposed`. A UniverseTarget advisor proposes only a target name, without a value. Its admission requires the operator's own value and records an assertion through the ordinary mutation path. No current advisor suggests values.

**Perceptual provenance (`UBU-D0293`).** The closed enum remains `asserted / measured / derived / proposed`, not a confidence ladder. Unconfirmed visual observation or inference maps to a `proposed` candidate, never a canonical value. Personal confirmation maps to `asserted`, instrument/reading evidence to `measured`, and an actual computation from facts to `derived`. External verification qualifies the evidence and keeps the underlying source kind. Candidate/Discovery evidence carries observation, confidence and corroboration qualifiers; `FactProvenance` remains only `{kind, recorded_at}`. This future perceptual mapping does not implement value proposals or broaden the existing advisors' authority.
```

#### DESIGN.md §21.1 — Realtime interaction sessions and candidate updates

```markdown
### 21.1 Realtime interaction sessions and candidate updates

A realtime interaction session is a bounded period in which UbU receives a continuous or semi-continuous stream from text, voice, video, sensors, tools, or a model provider.

Possible session modes include:

- `off`;
- `text_only`;
- `voice_session`;
- `meeting_logging`;
- `discovery_mode`;
- `local_only_realtime`;
- `cloud_assisted_realtime`.

Realtime sessions may produce candidate updates, but they do not directly mutate canonical state. Candidate update types include observed events, interruption signals, Task progress deltas, affect signals, Plan deviations, external condition changes, clarification questions, WorkItem candidates, LogEntry candidates, and AssociationAttestation candidates.

A realtime model's sense of elapsed time is evidence. Planner-time semantics are determined by UbU's explicit Task, Plan, Calendar, Log, Snapshot, and recalculation rules.

Realtime models are replaceable interaction backends for the primary MVP camera-and-voice surface (`UBU-D0292`), including conversation, capture, triage and short-horizon repair. UbU represents a realtime run as a lightweight `RealtimeInteractionSession`, not as a Task, Calendar event, canonical Log entry, or raw sensor stream. A session records the consented interaction context and source boundary; the model's outputs are `RealtimeCandidateUpdate` records until admitted through the ordinary planner, Log, Snapshot, Compartment, and review rules.

The minimum `RealtimeInteractionSession` record contains `interaction_session_id`, `schema_version`, `instance_id`, `actor_identity_ref`, `device_ref`, `started_at`, optional `ended_at`, `session_state`, `source_modes`, `backend_ref`, `model_or_tool_ref`, prompt or template version, enabled input sources, `compartment_ids`, `disclosure_policy`, retention or redaction policy, routing mode, evidence item refs, candidate update refs, provenance, review status, and correction or revocation links. Session states align with discovery mode where applicable: `inactive`, `active`, `paused`, `ended`, and `pending_review`.

The minimum `RealtimeCandidateUpdate` record contains `candidate_update_id`, `schema_version`, optional `interaction_session_id`, `emitted_at`, effective instant or interval, `candidate_type`, optional `target_ref`, payload, source evidence refs or excerpt hashes, per-field provenance, confidence, `compartment_ids`, redaction level, routing mode, review status, admission hint, idempotency key, and correction, rejection, revocation, or supersession links. The Phase 1b candidate types required for Quick UbU are `tag`, `dependency`, `preference`, `decomposition`, and `clarification_question`. Additional allowed candidate types are `interruption`, `task_progress`, `affect_signal`, `external_condition_change`, `plan_deviation`, `log_candidate`, `task_candidate`, and `association_attestation_candidate`, but each remains subject to its existing schema, authority, review, and Compartment gates.

A realtime candidate never mutates admitted state by itself. A tag, dependency, decomposition, preference, Task, Log, Snapshot, Calendar, Plan-deviation, or AssociationAttestation effect becomes canonical only after user acceptance, validated admission, or a specific local policy grant for that exact mutation class. Rejected, corrected, superseded, or revoked candidates remain auditable according to the candidate lifecycle rules.

Model-noticed elapsed time is represented as evidence or a candidate interval, not as planner-valid Task execution, Calendar occupancy, or Log truth. Planned time is reconciled only through accepted `plan_realized`, Task lifecycle, Calendar, Snapshot, or Log records. Unknown, private, rest, interruption, different-Task, and quick-note intervals preserve uncertainty and must not create Preference changes, Objective failures, habit claims, or moral meaning by inference.

Mandatory provenance for realtime observations includes source kind, capture or observation interval, device and backend refs, model or tool identifier, prompt or template digest where applicable, parser or schema version, evidence refs or payload hashes, source Compartment, redaction level, processing route, confidence, and review status. Audio, video, screen, keystroke, raw message, raw file, raw GPS, and similarly sensitive sources also require explicit source enablement and policy-visible capture state before they can produce candidates.

Routing defaults are conservative. Local-only processing is required for raw sensitive capture, `no_cloud_llm` Compartments, and sources whose policy denies external processing. Cloud processing is optional only for policy-approved, user-visible, redacted or intentionally provided inputs. Cross-user projection, external export, covert capture, broad background capture, hidden persuasion timing, and automatic external mutation are prohibited unless a later explicit mode, Compartment policy, capability grant, and user approval allow that exact behavior.

#### 21.1.1 Observation, context and closed-loop execution

The architecture is observation → candidate objects/facts → Identity and object resolution against permitted persistent context → candidate UniverseState/Resource implications → proposed preconditions or constraints → CPU semantic/policy/conflict validation and operator admission → planning or recalculation over admitted state. Ambiguous identity matches remain alternatives, not merged objects. The camera can suggest the relevance of an observed object to the user's life; it cannot infer intent, affect, Preference changes or ownership as self-certifying truth.

Confidence helps choose high-information clarification, not bypass admission. Corroboration is consequence-sensitive: escalate evidence and human review when an interpretation could change a consequential action; unknown or incompatible evidence stays unresolved. An observation consistent with completion is not proof that the modeled effect occurred. `UBU-D0293` defines source-kind mapping; `UBU-D0320` specifies observation bindings/admission; `UBU-D0321` specifies qualifier preservation; `UBU-D0324` specifies corroboration and review below. `UBU-Q0166` retains host-mapping evidence; `UBU-D0329` specifies completion/effect evidence in §17.10. Existing candidate/Discovery records are the reuse point, not newly implemented perception schemas.

The execution loop is **See → understand → plan → direct → observe → verify → learn → update**. Understanding proposes interpretations; planning remains the kernel's; direction presents an inspectable next action; new observations prompt reconciliation; verification uses the required evidence and operator review; learning proposes model corrections; only admitted updates close the loop. Inference never certifies itself by being repeated downstream. Technique guidance, observed Skill development and procedure-to-Technique capture depend on the later-phase objects and unresolved evidence/safety questions, not merely access to a camera. See [the interaction document](MULTIMODAL_INTERACTION.md) and [the security boundary](MULTIMODAL_SECURITY.md).

**Observation binding profile (`UBU-D0320`).** Reuse `DiscoveryEvidenceItem` (§12.2) for captured evidence and unresolved interpretations, and per-object `AdvisoryCandidate` records (§21.2.1) for supported proposals. One evidence item may support several candidates; accepting one interpretation never accepts the others. Store the following `observation-admission/1` bindings in their existing payload/evidence slots alongside the native normalized proposal. This is a payload contract, not a new first-class perception object, candidate-kind enum or `FactProvenance` extension. A producer's version-pinned adapter must validate its mapping to the host schemas; an unsupported or lossy mapping stays unresolved and needs separate versioned review under `UBU-Q0166`.

| Binding | Required content and interpretation |
|---|---|
| `profile` | Exactly `observation-admission/1`; retain the host record's own schema version separately. Unknown profiles cannot be admitted. |
| `sources` | Nonempty list of `{evidence_ref, evidence_version, source_ref, session_ref, session_version, selector, effective_start, effective_end}`. The controller supplies/resolves these refs; selectors locate a retained frame, span, reading or policy-permitted summary within that source, not model-invented evidence. Endpoints are timezone-bearing timestamps with start <= end; equal endpoints denote an instant. One-shot capture also has a session ref. The captured session revision establishes source enablement at acquisition time; it is not a live grant for later processing or execution. Evidence provenance retains origin Device, source/signal kind, capture enablement and route/policy refs, backend/model/tool, parser/schema version, template digest where applicable, confidence and permitted hashes. |
| `context` | `{context_bundle_ref, digest}` binding the exact permitted context used, or `null` only if no persistent context was consulted. Every context-based match must resolve to this bundle's supplied refs. A model cannot search denied context, mint a canonical ref, or substitute a fresh context for the one used. |
| `subjects` | List of `{observation_key, alternatives, selection}`; observation keys are unique within the capture and are not canonical entity ids. Each alternative is either `{object_ref, object_version}` or `{candidate_ref, candidate_version, proposed_object_key}`. An empty alternatives list means unknown; `selection: null` means unresolved. Selection is either null or `{alternative_index, review_event_ref}`; only explicit operator resolution selects an alternative. Confidence, coincident labels or repeated sightings cannot merge Identities/objects or establish ownership. |
| `proposal` and `claim_evidence` | The native normalized proposal (null for evidence without a supported proposal), plus `claim_evidence: [{field_path, evidence_refs}]` covering every proposed field change. Paths are nonempty arrays of native field names or zero-based array indices; evidence refs are nonempty lists drawn from `sources`. With no proposal, claim_evidence is empty. UniverseState implications use §11.3's `operation`/`target`/`payload` rules; proposed preconditions use §10.1. Unknown/omitted fields propose no write or deletion; a literal JSON null value must be an explicit claim. Source kinds follow `UBU-D0293`; confidence and corroboration stay in evidence, while `FactProvenance` remains `{kind, recorded_at}`. |
| `dependencies` | List of `{candidate_ref, candidate_version, proposal_path}` for proposed-object/value links used by the proposal; paths use the same field-path syntax. Object creations also name `proposed_object_key`. Such links are candidate-local, never canonical ids. Admission must resolve them to already-admitted records/values or changes in the same validated atomic batch; otherwise defer. Native schemas that cannot express these links retain the interpretation as Discovery evidence. |
| `observed_versions` | Ref-to-version map covering every evidence item, resolved target and context input relied on, plus referenced candidate dependencies. Where a current-state container has no internal version, bind its admitted envelope/frontier digest; do not invent UniverseState history. Creates bind a stable proposed-object key and an explicit create-only/expected-absence condition. Review also binds the candidate's own current version. |
| `observed_policy_versions` | Ref-to-version map of all relied-on capture, routing, Compartment, retention and admission policies. Current policy, not capture-time permission alone, governs review and admission. |
| `predecessors` | `{evidence: [{evidence_ref, evidence_version}], candidates: [{candidate_ref, candidate_version}], logs: [log_entry_id]}` using the host's correction/supersession links. Admitted-history corrections cite original Log ids under §17.3. Empty lists mean an original interpretation, not absence of earlier world events. |

**Perceptual qualifier preservation (`UBU-D0321`).** Use a companion `perceptual-qualifiers/1` payload in the existing evidence payload/ref and candidate evidence/per-field slots. Keep the host schema version and `observation-admission/1` bindings separate and unchanged. These are payload requirements, not new first-class objects, candidate kinds or `FactProvenance` fields. Reuse is conditional on a validated host mapping; an opaque payload slot described in prose is not proof that its schema, serializer and store preserve the content.

| Payload content | Required preservation and links |
|---|---|
| `profile` | Exactly `perceptual-qualifiers/1`. Unknown versions remain unresolved; adapters cannot silently discard or reinterpret them. |
| Evidence `claims` | List of `{claim_key, content, qualifications}`. Keys are stable and unique within an evidence revision, not canonical object ids. Content retains the exact permitted source statement or structured interpretation, including target/identity alternatives, attribution, negation, conditions, quantities, units and uncertainty. A summary alone is insufficient when it drops these distinctions. Evidence without a supported native proposal still retains its claims. |
| Claim `qualifications` | Separate entries for `observed`, `inferred`, `operator_confirmed` and `externally_verified`; several may concern the same claim. Each entry retains `{term, attribution, effective_start, effective_end, recorded_at, confidence, basis, related_claims}` and resolves to its enclosing evidence revision and the binding profile's source/session/selector. Attribution identifies the actual person, external source or model/tool and its applicable revision, using controller-resolved refs or explicit unknown metadata. Observation and recording times remain distinct. Additional relied-on sources join `sources` and `observed_versions`. These terms are evidence labels, never additions to `ProvenanceKind` or an ordered confidence rank. |
| Qualification `confidence` | Original score, scale, meaning/method and producing source/model revision, or explicit null when unreported. Preserve differing per-claim/per-source estimates; a candidate aggregate cannot replace them. Unreported is neither zero nor certainty, and confirmation/verification cannot synthesize a confidence score. |
| Qualification `basis` | Retain how the claim was established and the underlying kind under `UBU-D0293`. Observation cites the permitted frame/span/reading; inference retains model/tool revision, input refs, assumptions and unresolved alternatives. A personal confirmation retains the actual statement and speaker; an instrument reading retains instrument ref, reading, units, method and supplied accuracy/calibration qualifications. An actual derivation retains version-bound input facts/evidence, admitted envelope/frontier bindings for canonical inputs, rule/tool revision, parameters and result. Unreported optional details stay explicitly unknown; missing evidence of an actual instrument/reading or computation cannot justify `measured` or `derived`. |
| External-verification basis | Additionally retain verifier/source ref, exact claim and scope checked, verification time, method, result including inconclusive/disputed outcomes, and supporting evidence refs/versions. The verifier's attestation provenance is distinct from the checked claim's underlying kind. Verification of an interpretation neither changes its original kind nor supplies an admission event. |
| Qualification `related_claims` | List of `{evidence_ref, evidence_version, claim_key, relation}` locating claims supported, contradicted, confirmed, verified, computed from or sharing a source. Preserve source ancestry and dependency links so a repeated interpretation, copied report or reused frame cannot appear as independent corroboration. Unknown independence remains unknown; sufficiency follows `UBU-D0324` below. |
| Candidate `field_qualifiers` | List of `{field_path, claim_refs: [{evidence_ref, evidence_version, claim_key}]}` covering every path in `claim_evidence`. Use that binding's native field-path syntax; refs resolve to claims in its version-bound `sources`. Each mapped path has nonempty claim refs. One field may have conflicting or differently qualified evidence. Evidence-only records have no candidate field mapping. A partial acceptance binds only the reviewed subset and its supporting claims; other claims remain unaccepted. |

**Confirmation, review and retention.** A later confirmation, reading, derivation or verification creates separately attributed evidence and a linked candidate revision where needed; it never edits an earlier inference into a different source. Acceptance is an ordinary review event, distinct from a source confirmation. Bind the exact candidate revision, field mapping and evidence revisions to that event and its resulting mutation/Log/Snapshot refs through `UBU-D0320`'s atomic admission links. The companion payload is part of bound operation content; changing it follows the same revision, idempotency and fresh-review rules. Admitted kind follows the actual accepted assertion, reading or computation under `UBU-D0293`; `FactProvenance` remains `{kind, recorded_at}`. Review presents qualifiers, contradictions and attribution rather than only a confidence badge. `UniverseTarget` stays name-only, and unsupported value/computation paths stay evidence-only.

Losslessness means preservation of the permitted retained claims, qualifications and links through normalization, storage, review and authorized exchange; it does not require retaining raw media beyond policy. Redaction, purge or an incomplete Replica must indicate only permitted unavailability/incompleteness, never imply an absent qualifier was false or reconstruct disallowed content from retained derivatives. Inherit all source restrictions. Missing required bindings block admission under `UBU-D0320`; loss of evidence neither automatically retracts nor reaffirms admitted history.

**Host-mapping gate (`UBU-Q0166`).** Before enabling an adapter, pin the producer/adapter, host schemas, Rust/JSON serializers, store/review API and exchange revisions and policy configuration. Publish the exact host field paths or payload-ref mapping for every row above, with serialized fixture inputs and retrieved outputs. Compare structured values and all ids, versions, field paths, attributions, times, qualifiers and links after validation, serialization, persistence/reload, review and authorized exchange. Structural round-trip fidelity establishes preservation, not perceptual truth or evidence sufficiency. Exercise:

- a visual hypothesis with both observation and inference, differing field confidences and incompatible alternatives, with no canonical write;
- a later personal assertion, actual instrument reading and actual computation from versioned admitted facts, preserving each original interpretation and selecting only the source-appropriate admitted kind through supported paths;
- scoped external verification, inconclusive/disputed verification and two reports sharing one original source, retaining lineage without automatic admission or independent-corroboration claims;
- correction, partial acceptance, restart/replay, unavailable/purged evidence and policy-restricted partial exchange, retaining exact permitted links and exposing no denied content;
- unknown profile, unsupported host fields, serializer field dropping/defaulting and dangling claim/version refs, preventing admission rather than silently flattening qualifiers.

No passing host mapping or schema gap is asserted here. A mapping that cannot preserve this content stays unresolved with a policy-permitted diagnostic. A demonstrated gap needs a separately reviewed versioned schema/type/store chain change and any required pins/migration; this payload contract makes none.

**Bounded host evidence (`UBU-D0323`; 2026-10-12).** These inspected Phase 2 checkout revisions identify evaluated code, not dependency-pin changes. Tracked files were clean. Offline locked builds used rustc `1.99.0 (b940084d7 2026-09-28)`; the core lockfile supplies serde `1.0.229` and serde_json `1.0.152`, and the schema validator uses jsonschema `0.46.5`.

| Host path | Revision and observed carrier or limitation | Evidence scope |
|---|---|---|
| Candidate schema | `ubu-schemas@070d0a6f9a6ad7833dd523042925008316ce923b`, `schemas/core/advisory-candidate.schema.json`: `#/$defs/payload/oneOf/0/properties/value` accepts JSON; `#/properties/field_provenance/additionalProperties` requires strings. | Actual `tools/validate-fixtures` execution on the probes below. |
| Candidate Rust/JSON | `ubu-core@fa26bf678977f5e077229eb97a39cc1b8b960c9b`, `src/advisory_candidate.rs`: `CandidatePayload::Inline(serde_json::Value)` and `AdvisoryCandidate.field_provenance: BTreeMap<String, String>`. Its `schemas-ref` is the schema revision above. | Actual `AdvisoryCandidate` deserialization, `validate`, serialization, deserialization and reserialization; structured output comparison. |
| Candidate store/reload | `ubu-store@83a099f55220b1286ffb2dee3c9d19006e2090b5`, `src/candidates.rs::store_advisory_candidate` stores the serialized candidate in `advisory_candidates.payload_json`; `src/models/candidate_record.rs::CandidateRecord::candidate` deserializes it. | Source inspection only; no persistence/restart/replay fixture. |
| Normalization/review API | `ubu-orchestrator@9fddef16990ac89d07a168c75bf04b77c35b9bc3`, `src/api/advisory.rs::candidate_response` wraps stored JSON; `src/services/precondition_review.rs::vet_result` rebuilds accepted candidates' inline payload from `normalized_proposal`. | Source inspection only; added payload content cannot be assumed to survive that normalization path. |
| Review presentation | `ubu-ui@dda2312fd17a5dc330fef102f9def30ab88ac5bf`, `src/routes/Review.tsx`: the generic card presents confidence, evidence refs and normalized proposal without rendering `candidate.payload` qualifiers. | Source inspection only; API retention does not establish review fidelity. |

**Reproducible carrier specimen.** Start from `fixtures/valid/core/advisory-candidate/proposed-precondition.json` at the schema revision above, with its `schema_version: "1.0"` and native proposal unchanged. Let `P` be:

```json
{"profile":"perceptual-qualifiers/1","field_qualifiers":[{"field_path":["preconditions"],"claim_refs":[{"evidence_ref":"synthetic:description","evidence_version":"v1","claim_key":"charge"}]}]}
```

| Probe input relative to that fixture | Canonical schema validator | Actual core validation/JSON outcome |
|---|---|---|
| Unchanged baseline | Accepted. | Whole structured candidate unchanged after round trip. |
| Add `payload.value.perceptual_qualifiers = P` | Accepted. | Whole structured candidate and `P` unchanged after round trip. |
| Replace `field_provenance.preconditions` with the object `P` | Rejected. | Rejected: `invalid type: map, expected a string`. |
| Inline case with only `P.profile` changed to `perceptual-qualifiers/unknown` | Accepted by the generic carrier schema. | Accepted and preserved by the generic type layer; required adapter rejection remains untested. |

These are candidate link-carrier probes with unresolved synthetic refs, not the full qualifier/admission fixtures above. No producer/adapter, Discovery claim/evidence resolution, normalization, store/review, admission or authorized-exchange fixture was run; no policy-authorized exchange revision/configuration or complete host mapping is established. Unknown profiles still must remain unresolved before admission. The rejected direct field choice does not establish a schema gap when an inline representation remains available for testing. Report any failure at its pinned host stage with exact expected/retrieved content; distinguish mapping/implementation defects from a minimally reproduced host-contract restriction that a compliant existing payload/ref mapping cannot satisfy. `UBU-Q0166` stays Open for the remaining evidence, with no adapter enabled by these results.

**Normalization and persistence follow-up (`UBU-D0334`; 2026-10-13).** Reused the schema, core, store and orchestrator checkout revisions pinned in `UBU-D0323`; tracked files remained clean. A temporary synthetic harness called the actual public functions, linked to orchestrator libraries verified by `cargo build --locked --offline --lib`. The canonical validator was verified by `cargo build --locked --offline -p validate-fixtures`. Both builds used the rustc version above; this harness uses the orchestrator lockfile's serde `1.0.228`, serde_json `1.0.150`, sqlx `0.8.6` and tokio `1.52.3`, rather than the earlier standalone core probe's dependency versions. The schema tool still uses jsonschema `0.46.5`. No dependency pins changed.

**Minimal failing specimen.** Start from the same pinned `proposed-precondition.json` fixture used above. Remove its candidate-level `confidence`, retain all other host fields, and set both `normalized_proposal` and `payload.value` to `N`:

```json
{"operation":"clear_precondition","verdict":"remove","reason":"Synthetic carrier probe.","existing_precondition":{"target":"numeric_values.synthetic.orbital_teapot_charge","predicate":"at_least","expected":25},"blocked_now":false}
```

The baseline has no companion payload. The inline case adds the exact `P` above at `payload.value.perceptual_qualifiers`; the unknown-profile case changes only `P.profile` to `perceptual-qualifiers/unknown`. A fourth case copies `P` into `normalized_proposal.perceptual_qualifiers` as well. These remain synthetic candidate link specimens, without resolved evidence or complete claim qualifications.

For normalization, initialize an in-memory `AppState` with a fixed clock at `2026-09-19T09:00:00Z`, and admit only the fixture's target Task at version 1: title and description `Synthetic carrier probe.`, status `active`, preconditions `N.existing_precondition`, and ordinary user provenance. Author no UniverseState; the host supplies its default current view. Supply `precondition_review::Context` with `force: false`, one matching `ReviewTask` (the same description/tree, `existing_words` from `words`, null prior rejection reason), and empty `targets`. Obtain `submission(state, context, "synthetic-carrier-probe")`; construct an `ok` `LocalAdvisoryResult` carrying one specimen and the submission's authority/provider/policy/digest fields, with empty artifacts/diagnostics/telemetry and no deletion confirmation. Call the actual `precondition_review::vet_result` directly, without a producer/model call or enqueue/admission.

| Probe/path | Actual outcome and exact comparison |
|---|---|
| Canonical schema validation | Baseline, inline and unknown-profile cases accepted; the fourth case rejected by the closed native `normalized_proposal`. A structured `field_provenance.preconditions = P` control also rejected. `tools/validate-fixtures` reported three valid and two invalid fixtures with their expected outcomes. |
| Core Rust/JSON | All four cases deserialized, validated and retained the whole structured candidate through serialize/deserialize/reserialize. The fourth case's core acceptance does not override its schema rejection. |
| Baseline through `precondition_review::vet_result` | Status `ok`, one candidate, empty diagnostics. The host recomputed `blocked_now` to `true`, set `payload.value = normalized_proposal`, and set `suppression_key` to `review:sha256:57ea73a474832a19855231c2d99b66a4ed30b1eeccc4721fe20933e462ebee96`. Other structured fields were unchanged. |
| Inline and unknown-profile cases through the same function | The same native result as the baseline: `payload.value` is exactly `N` with `blocked_now: true`; the `perceptual_qualifiers` member is absent, rather than null or preserved. Status remains `ok`, one candidate, empty diagnostics. The expected retained member was exactly `P` (or its unknown-profile variant). The first observed loss on this tested route is `src/services/precondition_review.rs::vet_result` assigning `CandidatePayload::Inline(c.normalized_proposal.clone())`. |
| Fourth case through the same function | Status `malformed_result`, zero candidates, diagnostic code `advisory_malformed_result` and message `The model response was not a valid precondition review; no candidates were enqueued`. Adding a companion member to the closed native envelope is not a compliant workaround. |
| Direct store/reload control, bypassing normalization | Store the inline case with `store_advisory_candidate` in a temporary SQLite file using one ordinary user envelope. Close the pool, reconnect with `UbuStore::connect`, read via `get_advisory_candidate` and `CandidateRecord::candidate`, then retry the identical envelope/candidate. Each retrieved structured candidate equals the complete input, including `P`; one candidate and one mutation-ledger row, zero canonical objects and Logs. Changed payload with the same envelope key rejects with `idempotency_key_conflict`. This is connection close/reopen and replay, not process/crash restart. |
| Direct review API controls, bypassing normalization | Store the same inline case in the in-memory state; call `api::advisory::candidate` and `api::advisory::queue`. Both return the complete input candidate unchanged with `state_category: "candidate_state"`. These are handler calls, not authenticated HTTP or UI presentation tests. |

All normalization cases left the mutation ledger and every non-candidate table unchanged; the direct store/API controls also left canonical tables unchanged. The measured loss is specific to the tested normalization branch and mapping. The inline carrier remains schema-valid and survives the direct storage controls, so this does not establish a host-contract restriction that no compliant existing payload/ref mapping can satisfy. A future adapter using this branch must preserve the companion content and bound refs, or keep the interpretation unresolved before executable review; a native `ok` result cannot certify fidelity.

Complete Discovery claims/qualifications and resolved evidence revisions, required adapter profile/ref rejection, rendered review, admission, process/crash recovery, retention/partial acceptance and policy-authorized exchange remain untested. No complete mapping, passing adapter or actual schema gap is established; `UBU-Q0166` stays Open. Only a separately reproduced governing host-contract gap warrants the versioned chain review specified by `UBU-D0321`.

**Supported operations.** Validate the native proposal against an already-supported candidate kind and ordinary admission operation before enqueueing an executable review proposal. Unresolved targets, unsupported object kinds and perceptual values without a supported adapter remain Discovery review evidence with the limitation visible; they cannot fall through to a generic mutation executor. `UniverseTarget` advice remains name-only under `UBU-D0296`: it cannot carry, default or admit a perceptual value. The operator may author a value through the ordinary assertion/measurement form linked to the evidence. Future perceptual value-candidate support requires its separately reviewed versioned adapter; this contract does not enable it. Intent, affect, Preferences, completion/effect claims and later-phase Resources/Skills/Techniques retain their existing source and admission gates.

**Disclosure and session scope.** Evidence, candidates, alternatives, selectors, hashes and derivative review material inherit all supporting sources' disclosure/routing/retention restrictions; reference-only payloads do not waive them. Denied matches cannot leak identifiers, labels or existence through alternatives or counts. Reviewing permitted retained evidence is allowed after its capture session is paused/ended; it neither restarts capture nor transfers source consent or pending approval to another Device.

**Replay and corrections.** The controller assigns stable evidence ids and capture keys scoped to source/session and source item or span. It assigns separate candidate-creation and review-operation idempotency keys, each bound to its immutable originating operation rather than later lifecycle state. Exact retries, restart and exchange preserve ids, versions, normalized content and keys; the same key with different bound content is rejected, never treated as a harmless duplicate. Distinct observations are not deduplicated by their meaning or time alone. Equivalent repeated proposals use §21.2.1's suppression/deferral rules rather than resetting rejected/deferred state. New material evidence or a correction creates a linked revision/replacement with a new key. Superseded versions and late interpretations cannot overwrite an operator correction or use an old approval. Corrections to admitted claims append §17.3 entries and any separately admitted replacement Snapshot/mutation; they never rewrite the original. Raw-media purge marks supporting evidence unavailable and obeys existing retention policy; it neither fabricates replacement evidence nor automatically retracts or reaffirms a claim.

**Review and admission.** Present the source/session and observation time, permitted evidence or an explicit unavailable/redacted indicator, proposed-versus-current diff, source kind, uncertainty/contradictions, alternative matches, dependencies and resulting planning implications. Show the exact fields and batch being accepted; accepting an evidence item alone does not authorize every linked proposal. Offer resolve/edit, accept, reject, defer and private/unknown through existing Discovery/advisory review; edits record the operator's source, create a new bound revision and require fresh review. A permitted partial acceptance creates a bound revision or separate candidate for its exact subset and coherent dependency batch; unaccepted interpretations remain unresolved. If required evidence or target bindings cannot be established, retain an unresolved review item. Restricted attention defers unavailable review under §21.1.2.

At acceptance, the CPU rechecks the exact candidate/target/dependency versions and evidence/context/source revisions, including whether the latter remain available and unsuperseded, dependency bindings, current authority and Device trust, current source/disclosure/admission policy and native semantic/conflict rules. Changed bindings invalidate approval; require fresh review rather than silently rebasing. Source confirmation records new evidence of the actual assertion, reading or derivation; it does not relabel the original inference as measured or derived. Commit the ordinary canonical change, review decision with actor/`authority_source`/Device/times/observed versions, resulting Log/Snapshot refs, candidate admission links and idempotency outcome as one crash-consistent unit. Rejection/deferral records only ordinary review metadata, not the proposed world state. Recalculation follows admitted planner-relevant changes. Candidate persistence, successful extraction, high confidence and queue order confer no canonical, projection or external-action authority; external execution requires its own current checks and approval.

**Consequence-sensitive corroboration (`UBU-D0324`).** Assess each proposed claim against its intended planning/execution use, including downstream effects if it is wrong. The operator reviews the consequence class and intended scope. Review records the exact target, value, effective time, proposed diff, affected preconditions/capacity/commitments, credible error consequences, reversibility and uncertainty. Bound ordinary use by explicit operator cost/reversibility limits where relevant. Material financial or external commitments, safety-sensitive decisions, disclosure, irreversible changes, consequential identity/ownership attribution, and claims that release capacity or unlock dependent execution require consequential review. Unknown consequence or an unbounded downstream use takes that review path; model confidence cannot establish that a use is ordinary. No automatic confidence cutoff is selected.

| Intended use | Minimum evidence and human review |
|---|---|
| Clarification or labelled hypothesis | Permitted evidence may order questions or explain alternatives without admission. It cannot satisfy a precondition, release capacity, establish completion/effects, certify a Plan or dispatch an action. An operator may separately admit a verification Task without affirming the disputed claim. |
| Bounded, reversible ordinary planning change | Show the exact claim, source, uncertainty and proposed change; obtain explicit operator confirmation and ordinary version-bound admission. One actual operator assertion can suffice; no second source is required merely to clarify a low-consequence detail. Confirmation is separately attributed evidence under `UBU-D0321`, even when collected in the same interaction as acceptance. |
| Consequential planning or execution use | Each decision-critical perceptual claim needs at least one relevant corroborating source established independently of the original interpretation, plus explicit operator review of sufficiency and the resulting change. Check identity, time, scope, method, source reliability and limitations; resolve decision-relevant contradictions and alternatives. Further verification or qualified review required by the applicable workflow still applies. A corroborating source is necessary, never automatically sufficient. |

Independent corroboration requires an identifiable acquisition or knowledge basis that does not depend on the original model interpretation or merely copy its source. A direct operator inspection, independently obtained source record or actual instrument reading may qualify for the exact property checked; another Device or model is not required. Approving the same image, repeated frames without a distinct verification basis, another model interpreting the same input, copied reports, vote counts, high confidence and downstream summaries/Plans supply no independent corroboration. Operator confirmation states what the person actually knows and how; simple acceptance of model prose supplies no new source. Unknown independence is not counted as independent, and an independent but stale, irrelevant or inconclusive report does not satisfy the claim.

**Unresolved evidence and sovereign authoring.** Missing, stale, policy-withheld, incompatible or insufficient evidence keeps the affected candidate/use unresolved. Show only permitted uncertainty and offer clarify, obtain evidence, edit/narrow, defer, reject or a verification Task. Do not average contradictions, infer false/free/completed from absence, or silently choose a source. Unrelated supported fields may proceed only as a separately reviewed coherent subset. An authorized operator may separately author an assertion, correction or supported explicit planning assumption through ordinary admission, recording the actual source and remaining uncertainty; this does not relabel the visual inference as corroborated or waive native semantics, required workflow evidence, authority or privacy rules. Unsupported assumption/value paths remain evidence-only.

**Assessment bindings and later use.** Retain the reviewed use/scope, consequences, supporting and conflicting claim refs/versions, independence basis, unresolved issues and sufficiency rationale with existing candidate/evidence payloads and ordinary review/Log links; preserve `FactProvenance` as `{kind, recorded_at}`. Apply the check in host admission and Plan-input/execution preparation. Ordinary acceptance is not approval for a later consequential use. A changed use, material new contradiction, source correction/unavailability, or changed candidate/target/context/policy binding invalidates the applicable assessment and approval before affected Plan reuse or execution. Use the ordinary append-only correction and conflict paths; invalidate affected Plans and recalculate from admitted corrections without rewriting history or letting late model output undo an operator correction. An adapter unable to retain or enforce these links remains disabled for that use under `UBU-Q0166`.

Evidence review/admission neither grants capture/export capability nor approves an external action. Immediately before execution, recheck current admitted inputs, sufficiency for that operation, Plan legitimacy, authority, Device trust, Compartment/export policy and the operation's own scoped approval. A review may present both decisions, but must record and bind them separately. Apparent completion remains evidence-only until §17.10's lifecycle/effect evidence requirements under `UBU-D0329` are met.

**Required fixtures.** Before enabling a producer/adapter, pin its revision, host schemas and policy configuration, and serialize the following scenarios through the real validators, review storage and admission reducer. Fixture labels below denote distinct stable schema-valid ids. Record exact input payloads/versions, decisions, candidate/evidence links, expected canonical diffs/Logs and replay outcomes. These are required acceptance cases, not passing implementation results.

| Fixture input and action | Required outcome |
|---|---|
| Evidence `E1@v1`, session `S1@v1`, frame selector `frame:12`, `proposal: null`; evidence payload contains a visual hypothesis `{"operation":"set_fact","target":"facts.project.ubu.build_result","payload":"passed"}`; no value adapter or confirmation | Discovery review evidence only; no UniverseState, Snapshot, Task, Plan, Calendar or projection effect. Existing UniverseTarget advice still contains no value. |
| Supported precondition proposal `C1@v1` for `T1@v7` (prior precondition absent) uses `E1@v1`, context `B1`, policy `P1@v3` and a recorded target; capture session S1 has ended. Proposes `{"target":"facts.project.ubu.build_result","predicate":"equals","expected":"passed"}`, operator edits expected to `"reviewed"` and accepts the bound revision | Only T1's precondition becomes that leaf with expected `"reviewed"`; the fact's value is unchanged. Ordinary decision/Log links are admitted once. Review restarts no capture; repeat submission, restart and exchange produce no second effect. |
| Observation key `person:1` has permitted alternatives `I1@v2` and `I2@v4`, selection null, confidence 0.99; operator leaves unresolved or selects `I2` | Unresolved creates no merge/ownership/fact; selection affects only the newly reviewed proposal bound to `I2@v4`, without accepting other interpretations. |
| A proposal references proposed-object key `o1` from `C2@v1`, or a value from `C3@v1`; the dependency is pending, rejected, or admitted | No phantom object, dangling value link or planner use of a candidate value. Pending/rejected dependency prevents dependent admission; an approved atomic dependency batch or fresh review against admitted refs/values resolves it exactly once. |
| Between presentation and acceptance, `T1@v7` becomes `v8`, evidence/context is superseded, policy changes, or the originating/acting Device is revoked | No stale/unauthorized admission. Preserve only permitted review material and require the ordinary fresh-review, rejection or conflict path. |
| Duplicate `E1/C1` with identical keys/content; then the same key with changed value/target; then a genuinely new observation `E2` | Exact replay collapses; changed-content key reuse rejects; `E2` remains distinct and cannot automatically revive a rejection or satisfy corroboration by repetition. |
| Crash before or after the canonical-change/review/idempotency transaction; retry the same approval operation | Either no effect or the complete single admitted effect and its linked review/Log/outcome; never an orphan mutation, false admission acknowledgment or duplicate effect. |
| Operator corrects `E1/C1` before a delayed result arrives, and separately corrects an already-admitted claim | Late result cannot restore the superseded claim. Unadmitted replacement needs fresh review; admitted correction appends links and any accepted replacement without editing historical records. |
| Supporting raw evidence is purged, or a partial Replica lacks/denies a required source/target; manual text replaces capture | Show only permitted unavailable/incomplete metadata and no denied identifiers or Compartment labels. Missing bindings block that admission; a new operator-authored submission uses its actual source and fresh bindings without recreating deleted media or widening disclosure. |
| Missing required binding, unknown profile/schema, model-invented ref, malformed native proposal or hard routing/Compartment denial | No dispatch/admission; only policy-permitted evidence and an unresolved/rejected diagnostic through existing machinery. |
| Explicit operator assertion, instrument reading and actual derivation through supported ordinary authoring/computation paths; high-confidence apparent Task completion without the required effect evidence | Admitted kinds follow `UBU-D0293` with unchanged FactProvenance fields and retained original candidate links; unsupported derivation paths remain evidence-only. Apparent completion alone creates no lifecycle/effect update, inferred affect/intent or Preference. |

**Corroboration acceptance cases.** In addition to the fixtures above, pin the host admission, Plan-input and execution paths and demonstrate the following. These are enablement gates, not reported passing results.

| Case | Required outcome |
|---|---|
| A bounded ordinary edit with an explicit operator assertion; the same claim later supports a material commitment | Ordinary review may admit the first use without a second source. The later use requires a new consequential assessment and scoped review; prior acceptance is insufficient. |
| Consequential inference with high confidence, two models using one frame, copied reports or unknown lineage; then an independently obtained matching reading/inspection | The first set cannot satisfy independent corroboration. The new evidence preserves ancestry and may support a freshly bound review, without automatic admission or execution. |
| Independent but stale/inconclusive evidence, contradictory target/value/time, denied evidence or unresolved identity | The affected claim/use remains unresolved with policy-safe alternatives; no source voting, capacity release, precondition satisfaction or dependent dispatch. |
| New contradiction or operator correction after acceptance, changed use/versions/policy, and delayed model output | The old assessment/approval cannot authorize affected Plan reuse or execution; admitted corrections append history and trigger recalculation, and the late result cannot overwrite them. |
| Sufficient evidence and admitted candidate, but no external-action approval or a revoked grant; apparent completion without effect evidence | No external dispatch or inferred completion/effect. Evidence sufficiency cannot replace action authorization or §17.10's completion/effect requirements. |

`UBU-D0321` specifies qualifier content and fidelity gates; `UBU-D0324` specifies consequence-sensitive corroboration and review; `UBU-Q0166` retains revision-pinned host-mapping evidence and demonstrated schema-gap review. `UBU-D0326` specifies segmentation and scoped local analysis in §21.1.5; `UBU-D0327` and `UBU-D0328` specify raw-media retention and operator controls in §12.2; `UBU-D0329` specifies completion/effect evidence in §17.10. This contract freezes bindings, reuse, corroboration and admission behavior without claiming the remaining questions or implementation acceptance are complete.

#### 21.1.2 Restricted-attention audio envelope

**Embodied Audio Interaction (`UBU-D0309`).** Activity mode is separate from the realtime session's source/routing mode and from `compartment_ids`: for example, `Mode: Driving` with `Compartment: UbU Project / Public Outreach`. Activity constrains interaction; Compartment policy constrains disclosure and authority. Neither grants permission supplied by the other.

The initial activity vocabulary covers driving, bicycling, walking, running (including jogging), stationary exercise, resting, cooking, housework, showering, passenger transit, user-defined and unknown contexts. Operator selection establishes the intended mode. Explicitly enabled coarse activity evidence may tighten restrictions, but never starts capture or relaxes them. Driving, bicycling and conflicting or uncertain high-risk activity use the high-risk envelope; stopping motion alone does not establish parked review readiness.

A controller-owned, session-local `InteractionBudget` records:

- budget id and version, session and Device refs, activity mode, source/freshness of context, applicable policy refs, `compartment_ids` and expiry;
- permitted input/output channels and audio endpoint, manual availability, visual attention, locomotion risk, interruptibility, working-memory budget, intellectual/emotional bandwidth and social-risk tolerance;
- finite maximum prompt and response lengths, turn duration, choices and clarification count, permitted operation classes, confirmation policy and deferral policy.

Capacity and affect are user-declared or unknown, never a model-certified assessment of intelligence or readiness. Unknown capacity uses the conservative profile; observations may reduce the budget, not expand permissions. Effective permissions are the intersection of the reviewed activity profile, current channel/attention limits, Compartment policy and scoped capability grants. Missing, expired or conflicting authority prevents model/tool dispatch; pause, stop and silence controls remain available. Numeric profile defaults remain in `UBU-Q0128`; the exact initial-envelope serialization is specified below (`UBU-D0311`). A profile cannot ship without finite caps.

**Serialized budget (`UBU-D0311`).** `InteractionBudgetV1` is the controller's immutable snapshot for the initial envelope, not an LLM result, capability grant, capture consent or portable approval. All keys are required and all objects are closed. Reject duplicate keys, trailing content, non-finite numbers and unknown schema versions without repair. References identify controller artifacts or existing records; referenced versions use opaque revision tokens such as `v12`, preserving existing version conventions. `budget_version` and `interaction_epoch` are session-local integer counters. The model receives only a purpose-limited projection of effective limits through its ContextBundle, never authority to edit or return this object.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object", "additionalProperties": false,
  "required": ["schema_version", "budget_id", "budget_version", "session_ref", "device_ref", "interaction_epoch", "profile", "activity_mode", "context", "compartment_ids", "policy_versions", "grant_versions", "input_channels", "output_channels", "audio_endpoint", "readiness_declaration", "attention", "limits", "allowed_operations", "confirmation_policy", "deferral_policy", "issued_at_ms", "expires_at_ms"],
  "properties": {
    "schema_version": {"const": 1},
    "budget_id": {"$ref": "#/$defs/ref"},
    "budget_version": {"type": "integer", "minimum": 1, "maximum": 9007199254740991},
    "session_ref": {"$ref": "#/$defs/ref"},
    "device_ref": {"$ref": "#/$defs/ref"},
    "interaction_epoch": {"$ref": "#/$defs/uint"},
    "profile": {"$ref": "#/$defs/versioned_ref"},
    "activity_mode": {"enum": ["driving", "bicycling", "walking", "running", "stationary_exercise", "resting", "cooking", "housework", "showering", "passenger_transit", "user_defined", "unknown"]},
    "context": {
      "type": "object", "additionalProperties": false,
      "required": ["source", "evidence_versions", "observed_at_ms", "expires_at_ms"],
      "properties": {
        "source": {"enum": ["operator_selected", "operator_and_coarse_evidence", "coarse_evidence", "unknown"]},
        "evidence_versions": {"$ref": "#/$defs/ref_set"},
        "observed_at_ms": {"$ref": "#/$defs/uint"},
        "expires_at_ms": {"$ref": "#/$defs/uint"}
      }
    },
    "compartment_ids": {"type": "array", "maxItems": 32, "uniqueItems": true, "items": {"$ref": "#/$defs/ref"}},
    "policy_versions": {"$ref": "#/$defs/ref_set", "minItems": 1},
    "grant_versions": {"$ref": "#/$defs/ref_set"},
    "input_channels": {"type": "array", "minItems": 1, "maxItems": 2, "uniqueItems": true, "items": {"enum": ["speech", "local_controls"]}},
    "output_channels": {"type": "array", "maxItems": 1, "uniqueItems": true, "items": {"const": "speech"}},
    "audio_endpoint": {"oneOf": [{"type": "null"}, {"$ref": "#/$defs/versioned_ref"}]},
    "readiness_declaration": {"oneOf": [{"type": "null"}, {"$ref": "#/$defs/versioned_ref"}]},
    "attention": {
      "type": "object", "additionalProperties": false,
      "required": ["manual_availability", "visual_attention", "locomotion_risk", "interruptibility", "working_memory", "intellectual_bandwidth", "emotional_bandwidth", "social_risk_tolerance"],
      "properties": {
        "manual_availability": {"enum": ["none", "limited", "available", "unknown"]},
        "visual_attention": {"enum": ["none", "glance", "available", "unknown"]},
        "locomotion_risk": {"enum": ["low", "high", "unknown"]},
        "interruptibility": {"enum": ["none", "brief", "available", "unknown"]},
        "working_memory": {"$ref": "#/$defs/capacity"},
        "intellectual_bandwidth": {"$ref": "#/$defs/capacity"},
        "emotional_bandwidth": {"$ref": "#/$defs/capacity"},
        "social_risk_tolerance": {"$ref": "#/$defs/capacity"}
      }
    },
    "limits": {
      "type": "object", "additionalProperties": false,
      "required": ["max_capture_ms", "max_stt_utf8_bytes", "max_model_input_utf8_bytes", "max_model_output_utf8_bytes", "max_spoken_prompt_utf8_bytes", "max_spoken_response_utf8_bytes", "max_speech_ms", "max_turn_ms", "max_choices", "max_clarifications"],
      "properties": {
        "max_capture_ms": {"$ref": "#/$defs/uint"},
        "max_stt_utf8_bytes": {"$ref": "#/$defs/uint"},
        "max_model_input_utf8_bytes": {"$ref": "#/$defs/uint"},
        "max_model_output_utf8_bytes": {"$ref": "#/$defs/uint"},
        "max_spoken_prompt_utf8_bytes": {"$ref": "#/$defs/uint"},
        "max_spoken_response_utf8_bytes": {"$ref": "#/$defs/uint"},
        "max_speech_ms": {"$ref": "#/$defs/uint"},
        "max_turn_ms": {"$ref": "#/$defs/uint"},
        "max_choices": {"$ref": "#/$defs/uint"},
        "max_clarifications": {"$ref": "#/$defs/uint"}
      }
    },
    "allowed_operations": {"type": "array", "minItems": 3, "maxItems": 8, "uniqueItems": true, "items": {"enum": ["capture_candidate", "session_pause", "session_stop", "session_silence", "media_pause", "media_resume", "media_skip", "media_rewind"]}},
    "confirmation_policy": {"const": "candidate_review_only"},
    "deferral_policy": {"const": "ordinary_review_no_auto_dispatch"},
    "issued_at_ms": {"$ref": "#/$defs/uint"},
    "expires_at_ms": {"$ref": "#/$defs/uint"}
  },
  "$defs": {
    "ref": {"type": "string", "minLength": 1, "maxLength": 128},
    "uint": {"type": "integer", "minimum": 0, "maximum": 9007199254740991},
    "capacity": {"enum": ["none", "low", "available", "unknown"]},
    "versioned_ref": {
      "type": "object", "additionalProperties": false, "required": ["ref", "version"],
      "properties": {"ref": {"$ref": "#/$defs/ref"}, "version": {"$ref": "#/$defs/ref"}}
    },
    "ref_set": {"type": "array", "maxItems": 32, "uniqueItems": true, "items": {"$ref": "#/$defs/versioned_ref"}}
  }
}
```

**Budget semantics.** JSON shape validation is followed by these controller checks; no missing value is defaulted into permission:

- The budget must be the current controller-issued id/version for this session, Device and interaction epoch. Referenced profile, context evidence, policies, grants and endpoint revisions must match current permitted records; each ref set has at most one version per ref. Include every policy/grant used to determine this envelope; metadata overflow fails closed rather than omitting restrictions. Context sources and readiness declarations must match the kind of evidence referenced. `user_defined` requires an explicitly reviewed profile, rather than a free-form label selecting permissions. Coarse evidence may only maintain or tighten the operator-selected mode. Unknown or conflicting activity cannot relax the high-risk envelope.
- Times are integer milliseconds since the Unix epoch. Require `context.observed_at_ms <= issued_at_ms <= now`, and `now < min(expires_at_ms, context.expires_at_ms)`; profile, policy and grant expiry further shorten validity. Bind the remaining lifetime and turn deadline to the local monotonic clock. Clock discontinuity or restart invalidates the snapshot; wall-clock rollback cannot extend it. A new Device recomputes a budget under its own current policy.
- Attention fields are engagement bounds, not diagnostic scores. Known working-memory, intellectual/emotional bandwidth or social-risk tolerance requires the referenced, fresh user declaration; otherwise those fields are `unknown` and the conservative profile supplies finite limits. Profiles may tighten declared limits, never infer improved capacity. Driving, bicycling, high/unknown locomotion risk or conflicting high-risk evidence requires the high-risk envelope, `manual_availability = none`, `visual_attention = none`, `max_choices = 0` and `max_clarifications <= 1`. Zero choices prohibits menus, not one permitted brief reference clarification.
- Every limit is an explicit nonnegative integer; zero disables the corresponding resource. Positive values must be within the reviewed profile and current policy, channel and grant ceilings. Quantitative profile values remain in `UBU-Q0128`; absent ceilings prevent dispatch. These wire bounds are not usability defaults or evidence of driving safety.
- Counters belong to the original user turn, outside the model result. `max_capture_ms` and `max_stt_utf8_bytes` bound total authorized user audio and STT text for that turn. Model byte caps cover the complete normalized invocation (instructions, context, grammar and tool declarations) and complete outputs, cumulatively across that turn; apply the output cap before parsing. Spoken prompt/response byte caps and `max_speech_ms` cover cumulative controller prompts/acknowledgments, measured in UTF-8 bytes and elapsed speech time. `max_turn_ms` runs from the first user audio frame through final speech/dispatch, including waits and clarification; renewal or clarification never resets counters or revives an invalid binding. Exceeding a cap stops that processing/output path and leaves only already-authorized unresolved evidence for review, without truncating text into an executable interpretation or retrying automatically.
- `local_controls` and all three session-control operations are mandatory; immediate local pause/stop/silence remains available even without a valid budget. Speech input still needs the session's explicit source consent. Speech output requires a non-null fresh endpoint, nonzero speech limits and current disclosure permission; a known endpoint is not proof of privacy. Empty output channels mean silence. Zero model limits prevent model invocation; zero clarification allowance removes that branch from `AudioInterpretationV1`.
- `allowed_operations` is only a cached intersection of the initial envelope and current authority, rechecked at dispatch. `capture_candidate` maps to either existing capture variant; session and media names map to their exact v1 operations. Media requires the scoped integration grant and fresh request-bound playback target. `candidate_review_only` leaves capture unadmitted while permitted session/media controls need no admission; `ordinary_review_no_auto_dispatch` retains unsupported or prohibited requests for fresh ordinary review. No operation admits canonical state or authorizes consequential external execution.

Issue a superseding budget/version and invalidate request bindings on material context, mode, Device, endpoint, profile, policy or grant change, or on expiry. Synchronized/audit copies cannot restore a live budget, microphone consent or pending approval. Before enabling this contract, exercise missing/extra fields, non-integer/negative limits, stale revisions, replay after restart/handoff, clock rollback, missing endpoint permission, unknown readiness and high-risk menu/clarification violations. This specifies the serialization and enforcement boundary; numeric profiles, richer actions and device-specific tuning remain open.

**Initial permissions.** Every restricted-attention mode starts with user-initiated local candidate capture, a bounded acknowledgment, pause/stop/silence, and optional explicitly granted media transport (`pause`, `resume`, `skip`, `rewind`) against one fresh, unambiguous playback target. Capture produces existing evidence/candidate records, including Task proposals; it does not create an admitted Task, Snapshot, Preference or Calendar change. Media transport uses the current integration grant and policy checks; it never authorizes sharing, purchases or broader app access. Richer read-only and short-drafting profiles remain open.

Driving, bicycling and uncertain high-risk contexts additionally prohibit generated drafting, reflective/coaching dialogue, spoken menus and visually dependent review. Permit at most one brief clarification per request; an unresolved target or unanswered clarification leads to capture/deferral without retry prompts. External sends, financial actions, legal commitments, deletion, multi-party communication and scheduling changes are deferred. Neither a capability grant, a generic “yes”, an action-specific spoken confirmation nor a model's `safe_for_mode` claim overrides this envelope. UbU may remain silent or suggest user-selected existing media/rest; it must not repeatedly solicit work or queue review prompts while attention is restricted.

**Controller boundary.** Consented local STT yields evidence, then the harness assembles a purpose-limited `ContextBundle` under §21.3 and requests an interpretation/candidate through §21.2. Include only the permitted current referents, relevant admitted state and scoped playback/context versions needed for that operation. Model confidence, risk estimates and confirmation suggestions are advisory. Deterministic validation checks schema, semantics, current budget, Compartment route, grants, object versions, ambiguity and idempotency before invoking an allowlisted control or creating a review candidate. Recheck immediately before dispatch and speech output; a local processing route alone does not authorize reading private content aloud on a shared audio endpoint. Raw AV stays local under §12.2; an unavailable local backend never creates cloud-fallback authority.

**Audio routes and peripherals (`UBU-D0359`).** Bind input and output independently. The existing `InteractionBudgetV1.audio_endpoint` ref/version resolves to a controller-owned output-route record; microphone identity remains in session/source consent and request bindings. These local artifacts add no model-output field or canonical object. A qualified route record must establish:

| Binding | Required content |
|---|---|
| Route identity | Owning execution-enclave Device, adapter/profile revisions, controller route-generation token tied to observed platform state, exact input source or complete output sink set, observation time and finite freshness ceiling. A friendly name, pairing record or default-device setting cannot establish current route identity. |
| Disclosure scope | Current source/Compartment policy versions, operator-selected output route and declared audience, and permitted content scope. Treat car speakers, phone/watch speakers and undeclared audiences as shared or unknown. Headphones alone cannot establish privacy; every sink in a mirrored route must be permitted. |
| Enforced controls | Demonstrated source/stream binding, prevention of automatic microphone/speaker fallback, finite queued-audio bounds and measured stop-to-output-quiescence/resource-release ceilings under §§21.1.4–21.1.5. Missing route observability or enforceable bounds leaves the affected audio direction unavailable. |

Attaching a microphone, headset or car speaker does not itself register a UbU Device. A phone/watch/car app executing UbU work in another enclave requires its own Device trust, qualified endpoint and authenticated policy-authorized route. Pairing, proximity and ownership supply no capture, processing, storage or disclosure authority. Peripheral/platform buffering and copies remain within the qualified route's retention and resource envelope; an accessory cannot become an implicit inference backend.

Before acquisition, verify the selected input source against current source-scoped consent and pin the stream to it. Inability to prevent silent microphone fallback disables speech acquisition. Before speech, and before releasing each bounded speech segment, verify the selected output route/generation, complete sink set, current disclosure permission and remaining budget. Bind the stream to that route; do not send to a platform default that can migrate to another speaker. The adapter must prevent queued audio from falling back to an unreviewed sink, including before a delayed route-change callback. If it cannot, speech on that profile remains unavailable. Speech content and endpoint metadata inherit source restrictions; a neutral acknowledgment also requires a permitted output route.

Disconnect, input/output replacement, mirroring change, loss of reliable route state or a platform audio-focus interruption invalidates affected turn/output bindings locally. Stop affected acquisition and speech, discard unsent speech queues and invalidate pending interpretations, playback referents and confirmation through the existing epoch rules. Do not splice microphones into one utterance, infer an action from its partial transcript, switch to speaker, resume old speech, replay transport or resume an interrupted turn automatically. Audio focus, ducking or a system call establishes no media-control request. Already released samples cannot be recalled; report quiescence against the measured bound rather than claiming instant physical silence.

Reconnection, even under the same friendly name, needs fresh route evidence and explicit user selection/resumption. Recompute the budget and permissions; a changed input source also needs fresh source-scoped consent. A new user request uses fresh bindings and retains existing unresolved evidence only where authorized. A cross-Device handoff transfers permitted candidates through ordinary exchange, never a live microphone, partial executable turn, queued speech or approval. Keep local Pause/Stop/Silence usable and preserve permitted saved evidence; indicate unavailable audio through an eligible remaining surface without requiring visual interaction while attention is restricted.

Before enabling a route, pin Device/OS/adapter/controller and policy/profile revisions and exercise queued and active speech during headset unplug, car disconnect/reconnect, same-name endpoint replacement, speaker/default-route changes, mirrored output and delayed/duplicate route callbacks. Also exercise input fallback during an utterance, platform audio-focus loss, source/output revocation, offline operation and phone/watch handoff. Require no unconsented acquisition, release to an unreviewed sink, post-invalidation release, partial-utterance dispatch, automatic resumption or repeated effect; demonstrate bounded quiescence and retained-evidence/manual-control behavior. Include permitted-route controls so silence alone cannot pass. These are required qualification cases, not passing results, supported accessories or a driving-safety claim. This answers the input/output route and handoff subset of `UBU-Q0128` subquestion 17; numeric profiles and platform-specific qualification remain open.

**Playback referents and vague commands (`UBU-D0322`).** For the existing transport envelope, resolve the audio domain and target before offering a playback alias. This is controller-owned request context, not a new canonical object or an expansion of `AudioInterpretationV1`. An explicit target must resolve uniquely within the permitted integration scope. An implicit media reference requires one user-selected playback target in this session, current integration evidence that it remains the target, and no competing UbU speech or other permitted audio source. Foreground-app status, most-recent playback, project relevance and model confidence cannot select a target. Clear implicit referents on interruption, material mode/Compartment or endpoint change, restart or Device handoff; retained/audit copies cannot restore them. If the available permitted evidence cannot establish uniqueness, clarify or defer without inspecting denied apps or revealing their existence.

Bind the selected integration/target, item and queue/control revision, playing/paused state, Device/output endpoint, current grant/policy versions, observation time, finite freshness deadline and the operation's integration-declared step semantics in the existing request binding. A paused target can support explicit resume while its binding remains fresh; selecting it grants no new playback or disclosure authority. Ordinary position advancement within the same item does not change the control revision; item/queue replacement, seek, playback-state change, endpoint switch or loss of reliable state does. Changes before dispatch invalidate the pending interpretation. A dispatched control's confirmed transition records its outcome and invalidates other outstanding bindings; its acknowledgment still rechecks the current budget, endpoint and disclosure permission. Transport must address the bound integration and target with its expected control revision, rather than a global command to whichever player is active at dispatch. An adapter unable to enforce that binding cannot enable transport. Constrain producer branches to the routing decision below and recheck it at dispatch; a model's action label cannot supply a missing target or operation.

| Utterance in the initial envelope | Controller routing |
|---|---|
| Recognized local pause/stop/silence, or `pause that` while UbU is speaking | Take the immediate local session-control path and stop UbU output. Do not pause another app as an inferred second effect. Consented speech input or local controls remain required. |
| `pause that` with UbU silent and exactly one fresh selected media referent | Propose `media_transport/pause` for that alias, subject to the existing budget and dispatch checks. With an unknown or competing referent, use the one permitted target clarification or defer. |
| Explicit `pause/resume/skip/rewind [target]` | Resolve the named target uniquely and bind that request; do not change the session's default target implicitly. Skip/rewind permit only the adapter's declared single bounded step. Unsupported quantities or semantics defer. |
| `go back` | Keep unresolved: a prior track, a seek, UbU readback and UI navigation have different effects. A current player alone does not choose the operation. The v1 clarification vocabulary cannot clarify this operation; defer for ordinary review or a fresh explicit command. |
| `save this` | Retain only the authorized utterance as unresolved capture evidence. Do not bookmark/download media or treat a deictic reference as captured content. A self-contained dictated note may use the existing evidence-bound capture action. |
| `what was that?` or `what did I miss?` | Defer readback, replay and summarization requests; v1 has no such action. Do not substitute a rewind or speak generated content. |
| `remind me about this` | Retain the authorized utterance as unresolved evidence for note/Task review, with its missing referent visible. Do not admit a Task, schedule a reminder or fetch content to fill the gap. |

Target clarification uses only `identify_playback_target` and the remaining turn budget; no spoken menu or generic `yes` selects an app. Its answer must uniquely identify a permitted target. Bind the original command and clarification evidence within the same uninterrupted turn, retaining its original counters/deadline; an interruption cannot resume that exchange. A new explicit request needs fresh evidence and bindings. Partial/uncertain STT or mixed supported/unsupported requests cannot yield media dispatch; retain authorized unresolved evidence. Immediate local pause/stop/silence remains available.

Media apps retain playback/position ownership. UbU records only policy-permitted target/control metadata and confirmed outcomes needed for routing and audit; transport grants do not permit transcript, catalog or library retrieval. Do not issue app transport automatically when UbU speaks, is interrupted or reconnects. An unknown dispatch outcome is reconciled through permitted state reads and audit metadata, never by replaying the transport or claiming success from silence. State reconciliation alone does not prove a relative command succeeded. A subsequent effect requires a fresh user request.

Before enabling this routing slice, pin the adapter/controller and policy revisions and exercise: two players or no reliable target; UbU speech overlapping media; an explicitly named app differing from the selected default; denied-source metadata; paused-target resume; ordinary position advancement versus item/queue/seek changes between interpretation and dispatch; stale/offline state and endpoint/Device handoff; ambiguous `go back`, `save this` and reminder references; and duplicate or unknown skip/rewind outcomes. Require no wrong-target, inferred second or repeated effect, no denied-context leakage, no canonical planning-state change without ordinary admission and no false success acknowledgment. These are acceptance requirements, not passing results. This answers initial-envelope referent routing in `UBU-Q0128` subquestions 9, 14 and 15; richer media operations, numerical profiles and device-specific integration remain open.

**Initial typed interpretation (`UBU-D0310`).** `AudioInterpretationV1` is the closed JSON result for the initial envelope only. All keys are required; `null` or `unknown` represents an unavailable estimate. References are opaque aliases supplied in this request, not model-authored object identifiers. This is the schema template; the controller instantiates a conservative producer grammar under §21.2. Matching it does not establish permission. Apply the budget's finite response-size cap before parsing one complete JSON object; reject duplicate keys, non-finite numbers and trailing content.

```json
{
  "type": "object", "additionalProperties": false,
  "required": ["schema_version", "request_id", "intent", "entities", "confidence", "ambiguity", "cognitive_load_estimate", "action_risk", "proposed_action", "required_confirmation", "clarification_request", "defer_reason", "safe_for_mode"],
  "properties": {
    "schema_version": {"const": 1},
    "request_id": {"$ref": "#/$defs/ref"},
    "intent": {"enum": ["capture", "session_control", "media_transport", "clarify", "defer", "no_op"]},
    "entities": {"type": "array", "maxItems": 4, "items": {"$ref": "#/$defs/entity"}},
    "confidence": {"type": ["number", "null"], "minimum": 0, "maximum": 1},
    "ambiguity": {"type": "array", "maxItems": 6, "uniqueItems": true, "items": {"enum": ["uncertain_stt", "partial_utterance", "unresolved_reference", "multiple_targets", "stale_context", "other"]}},
    "cognitive_load_estimate": {"enum": ["minimal", "low", "moderate", "high", "unknown"]},
    "action_risk": {"enum": ["none", "low", "high", "unknown"]},
    "proposed_action": {"oneOf": [{"type": "null"}, {"$ref": "#/$defs/capture"}, {"$ref": "#/$defs/session_control"}, {"$ref": "#/$defs/media_transport"}]},
    "required_confirmation": {"enum": ["none", "clarification", "ordinary_review"]},
    "clarification_request": {"enum": [null, "repeat_request", "identify_playback_target"]},
    "defer_reason": {"enum": [null, "unsupported_operation", "mode_prohibits", "budget_exceeded", "policy_denied", "ambiguous_input", "stale_context", "user_requested"]},
    "safe_for_mode": {"type": ["boolean", "null"]}
  },
  "allOf": [
    {"oneOf": [
      {"properties":{"intent":{"const":"capture"},"proposed_action":{"$ref":"#/$defs/capture"},"required_confirmation":{"const":"ordinary_review"},"ambiguity":{"maxItems":0},"clarification_request":{"type":"null"},"defer_reason":{"type":"null"}}},
      {"properties":{"intent":{"const":"session_control"},"proposed_action":{"$ref":"#/$defs/session_control"},"required_confirmation":{"const":"none"},"ambiguity":{"maxItems":0},"clarification_request":{"type":"null"},"defer_reason":{"type":"null"}}},
      {"properties":{"intent":{"const":"media_transport"},"proposed_action":{"$ref":"#/$defs/media_transport"},"required_confirmation":{"const":"none"},"ambiguity":{"maxItems":0},"clarification_request":{"type":"null"},"defer_reason":{"type":"null"}}},
      {"properties":{"intent":{"const":"clarify"},"proposed_action":{"type":"null"},"required_confirmation":{"const":"clarification"},"clarification_request":{"not":{"type":"null"}},"defer_reason":{"type":"null"}}},
      {"properties":{"intent":{"const":"defer"},"proposed_action":{"type":"null"},"required_confirmation":{"const":"ordinary_review"},"clarification_request":{"type":"null"},"defer_reason":{"not":{"type":"null"}}}},
      {"properties":{"intent":{"const":"no_op"},"proposed_action":{"type":"null"},"required_confirmation":{"const":"none"},"ambiguity":{"maxItems":0},"clarification_request":{"type":"null"},"defer_reason":{"type":"null"}}}
    ]}
  ],
  "$defs": {
    "ref": {"type": "string", "minLength": 1, "maxLength": 128},
    "entity": {
      "type": "object", "additionalProperties": false, "required": ["role", "context_ref"],
      "properties": {"role": {"enum": ["subject", "playback_target"]}, "context_ref": {"$ref": "#/$defs/ref"}}
    },
    "capture": {
      "type": "object", "additionalProperties": false, "required": ["kind", "candidate_kind", "evidence_span_ref"],
      "properties": {"kind": {"const": "capture_candidate"}, "candidate_kind": {"enum": ["note", "task_candidate"]}, "evidence_span_ref": {"$ref": "#/$defs/ref"}}
    },
    "session_control": {
      "type": "object", "additionalProperties": false, "required": ["kind", "operation"],
      "properties": {"kind": {"const": "session_control"}, "operation": {"enum": ["pause", "stop", "silence"]}}
    },
    "media_transport": {
      "type": "object", "additionalProperties": false, "required": ["kind", "operation", "playback_ref"],
      "properties": {"kind": {"const": "media_transport"}, "operation": {"enum": ["pause", "resume", "skip", "rewind"]}, "playback_ref": {"$ref": "#/$defs/ref"}}
    }
  }
}
```

Before offering the grammar, the controller constrains `request_id` to the invocation's id, replaces each context/evidence/playback reference site with an enum of supplied aliases, and removes action branches or media operations outside the current envelope and grants. An empty alias set forces the related entity list empty or removes the related action branch; no remaining clarification allowance means the `clarify` branch is unavailable. Uncertain or partial STT permits only clarification or deferral, while the already-authorized evidence may remain unresolved. Static intent/action consistency is encoded in the schema; dynamic semantic and policy checks still run at dispatch.

Semantic validation repeats these constraints and checks current context:

- `capture`, `session_control` and `media_transport` require the matching non-null action kind, empty `ambiguity`, null `clarification_request` and null `defer_reason`. Capture requires `ordinary_review`; session and media controls require `none`. These labels never perform admission or confirmation.
- `clarify` requires a non-null `clarification_request`, `required_confirmation = clarification`, and null action and defer reason. `defer` requires a non-null defer reason, `ordinary_review`, and null action and clarification request. `no_op` requires null action, clarification request and defer reason, and `none`. Nonempty ambiguity permits only `clarify` or `defer`.
- Entity aliases must occur in the permitted ContextBundle and match their declared role; they suggest associations without resolving Identity or ownership. Capture must cite a supplied STT evidence span belonging to the current utterance. The controller retains that evidence as an existing unresolved candidate; a model cannot supply replacement note text, admit a Task, or invent evidence. `note` routes to existing quick-note/Discovery evidence and `task_candidate` to existing Task proposals; these routing tags do not extend candidate-type or provenance enums.
- Media transport must name the one fresh playback alias whose integration, target and state version the controller bound to this request. `skip` and `rewind` each mean one integration-declared bounded step; requested quantities or other unsupported arguments defer rather than being silently approximated. The result cannot contain arbitrary tool names, destinations, credentials, message payloads or executable code.

**Request binding.** Outside the producer result, the controller binds each invocation to its request id, session, Device, STT evidence/version, ContextBundle, budget and policy versions, scoped grants, playback target/version when applicable, session interaction epoch, finite deadline and idempotency key. The echoed `request_id` must match that invocation's original binding; it cannot select another request. Interruption or a material mode, Device, endpoint, target, payload, policy or grant change invalidates outstanding bindings and advances the epoch. The controller serializes invalidation, result acceptance and dispatch decisions. Late results cannot speak, dispatch or supply a new clarification; a fresh request needs fresh evidence and current context.

The controller rechecks the binding and operation permissions before both speech and dispatch. Confidence, load, risk, confirmation and `safe_for_mode` are advisory; the controller may tighten or defer, never widen the envelope. Load describes the proposed interaction, never the user's intelligence, affect or capacity. A clarification uses a controller-owned bounded template and consumes the request's remaining clarification allowance. Acknowledgments report only confirmed capture/control outcomes; a capture acknowledgment means that a review artifact was retained, without confirming its content. The model result is never spoken directly. Recognized pause/stop/silence controls take an immediate local path without waiting for an LLM; local stop/silence controls remain available if model processing fails.

Reject malformed, truncated, oversized, unknown-version, extra-field, forged-reference or semantically inconsistent results without repairing them into executable actions. Preserve only already-authorized evidence as an unresolved review item, without automatic model retries or repeated prompts. Duplicate results reuse the original idempotency key and are ignored after the first acceptance, including speech and clarification; an uncertain media outcome requires state reconciliation and a fresh user request, never automatic repetition of a relative skip/rewind.

Before enabling v1, include negative fixtures for unknown action kinds, foreign evidence/playback aliases, inconsistent intent/action pairs, late replies after interruption or handoff, and duplicate/unknown media outcomes. Valid capture stays in candidate state; every consequential external request remains deferred under the initial envelope. This specifies a contract, not a shipped schema or implementation. Numeric mode profiles, external-action confirmation grammar and richer output extensions remain in `UBU-Q0128`.

**Interruption and deferral.** Partial/uncertain STT, ambiguous recipients or “this/that” references, stale media state and false conversational continuity never select an action by confidence alone. Ask at most the budgeted clarification count, then retain an unresolved candidate or defer. Interruption cancels unfinished speech and pending confirmation; it cannot complete an utterance or replay an action. A mode, target, payload, policy or Device change invalidates pending approval. Leaving a restricted mode only makes a deferred item available for ordinary review: it never dispatches it. Any later consequential action needs fresh admission under current authority and confirmation bound to the exact operation, recipient, payload/version and expiry; the ordinary-review single-recipient message slice follows below, while other external-action grammars remain open.

**Deferred message confirmation (`UBU-D0333`).** The controller profile `audio-send-review/1` defines an additional confirmation requirement for an already-permitted, ordinary-attention review of one plain-text message to one recipient. It enables no sending workflow and adds no operation to `AudioInterpretationV1` or `InteractionBudgetV1`. Every external send remains deferred in the initial restricted-attention envelope. Group/multi-party sends, attachments, financial/legal commitments, high-stakes messages and unknown consequences are outside this profile.

Returning to ordinary attention requires explicit operator selection and a fresh eligible review context; stationary motion, reconnection or a Device change is insufficient. Open the deferred item only on user request, with permitted evidence and a newly reviewed recipient and draft. Missing references stay unresolved. The captured request, an accepted Task/note, a draft, a tone suggestion and earlier spoken approval authorize no send.

**Complete review and binding.** Resolve one permitted recipient and exact destination; never select by relationship closeness, recent contact or model confidence. Present the sending Identity/account, recipient's distinguishing label and actual address/number, integration/route and complete final message. An abbreviated readback cannot approve omitted content. Audio-only review requires a revision-pinned rendering that preserves decision-relevant spelling, punctuation, numbers and other literal content; unsupported rendering or content exceeding the finite review budget defers to an eligible ordinary review surface. Do not silently shorten content, split it into unbounded turns or read it on an unauthorized endpoint. Review of the draft and authorization of its external send remain separate decisions.

Before a confirmation prompt, bind the review id/version, actor and authenticated session, Device, interaction epoch, activity/readiness and review-profile versions, input source and output endpoint revisions, recipient ref/version and exact destination, sending account/integration/route versions, immutable complete payload/version, source/context/policy/grant versions, completed review presentation, finite monotonic deadline and the originating send's idempotency key. Store these in the controller's existing review/AgentAction bindings, outside model output. A reviewed Device/profile must supply finite input, readback, prompt, speech and total-review limits; missing limits or bindings prevent the prompt. Budget renewal, replay and restart cannot extend the deadline or complete an interrupted readback.

**English confirmation grammar.** Instantiate exactly one permitted token sequence for the current review:

```text
confirm send to <recipient_phrase> code <digit_word> <digit_word> <digit_word> <digit_word>
```

`recipient_phrase` is the controller-reviewed, unique spoken label for that exact recipient/destination, shown or spoken during complete review; it is not an LLM alias or a way to choose a recipient during confirmation. Each `digit_word` is one of `zero one two three four five six seven eight nine`. The controller samples a fresh four-digit code, does not reuse it within the review session and keeps at most one live challenge; code exhaustion disables this confirmation route. For example, `confirm send to Morgan mobile code four seven two six` matches only the currently bound message. The code associates a reply with a review; it is not actor authentication, a secret capability or proof of comprehension.

The controller generates the expected phrase, never the model or message body. Match the complete final STT token sequence after only ASCII case-folding, collapsing ASCII whitespace and removing one terminal sentence mark (`.`, `?` or `!`). Require spelled-out digit words; do not repair digits, synonyms, approximate names or homophones into a match. This English profile permits no other locale grammar. Generic `yes`, `send it`, a partial phrase, extra instructions, an incorrect code, uncertain STT or an ambiguous speaker/source cannot approve. Input must be consented and explicitly initiated for this same uninterrupted review exchange after presentation/prompt output has ended. Prior recordings/transcripts, TTS echo, media and model/tool output cannot supply confirmation; input ownership must be established independently of phrase matching or this route remains unavailable.

Stop/cancel/silence immediately invalidates the challenge. Interruption, expiry, incomplete presentation or a material change to any bound actor/session, Device, mode, endpoint, recipient, account, route, payload, context, policy or grant invalidates it as well. A new user-requested exchange needs fresh review and a new challenge, with no automatic retry prompt. Matching text records approval evidence only; serialize cancellation, approval consumption and dispatch, recheck all current bindings and authority immediately before egress, and persist the consumed approval and ordinary attempt record before external dispatch. Duplicate callbacks cannot create another attempt. Failed or unknown delivery follows ordinary outcome reconciliation/idempotency rules in §21.8, never blind replay or a success acknowledgment inferred from silence. Fresh approval cannot bypass unresolved delivery reconciliation.

Reuse existing review, evidence, AgentAction and append-only Log links for permitted recipient/payload versions, presentation completion, challenge id/epoch, confirmation evidence, policy checks, invalidation/consumption and delivery outcome. No mandatory raw recording, full transcript, message-body copy or retained plaintext code is added. Synchronized review artifacts carry no live challenge, microphone consent or execution approval.

Before enabling this confirmation route in a separately released sending workflow, pin controller/STT/TTS/adapter, Device/profile, rendering and policy revisions and demonstrate:

- A fully reviewed permitted message and exact fresh phrase yield one bound attempt; generic assent, wrong/partial code, approximate recipient, uncertain speech, TTS/media echo and an unowned source yield none.
- Same-name contacts, changed destination/account/route or payload, incomplete/oversized readback and denied endpoint/context require fresh review or deferral without wrong-recipient disclosure or dispatch.
- Stop/interruption/revocation between presentation, confirmation and egress, expiry, restart/handoff and delayed/duplicate callbacks produce no stale or repeated attempt; unknown delivery remains explicitly unknown and is reconciled before any retry.
- Driving, bicycling and every other initial restricted-attention mode still defer external sends even with a grant and a correctly spoken phrase.

These are required acceptance cases, not passing implementation results or authentication guarantees. This answers the ordinary-review single-recipient send slice of `UBU-Q0128` subquestions 10, 13 and 18. Other external-action grammars, future restricted-mode execution permissions and richer messaging integrations remain open.

Audit records link the session, evidence/candidate, budget and policy versions, ContextBundle, resolved target/version, policy decision and reason, approval/deferral/rejection, invocation/result and idempotency key. They retain minimized summaries or permitted refs, not a mandatory recording or full transcript. Raw-media retention remains under §12.2 and `UBU-Q0170`–`UBU-Q0171`. Device handoff rechecks capture, context, output route and authority; synchronized candidates never carry live microphone consent or pending execution approval.

**Release boundary.** Before enabling a restricted-attention slice, demonstrate that interruption stops output, uncertain references do not dispatch, high-risk sends remain deferred despite confirmation, mode/Device changes do not execute queued work, and offline operation preserves local-only denials. This is a design contract, not an implementation or a claim of validated driving safety. `UBU-Q0183` retains the delivery sequence and device allocation under `UBU-D0292`; richer media/narration and profile tuning remain later answers, without restoring a Phase 3 gate.

#### 21.1.3 MVP interaction coverage and alternatives

**Product contract (`UBU-D0319`).** Realize `UBU-D0292` through a common planning/review surface: showing a concrete concern and speaking about it produces a reviewable proposal or an explicit unresolved result with a useful next step. A camera preview, transcript or conversation alone does not satisfy delivery. Text and forms are directly reachable before capture or permission requests, without a full inventory or later-phase Resource/Skill/Technique setup.

**Source choice.** Camera, microphone and speech output are independently selectable; opening the surface starts none of them. Before acquisition, disclose purpose, active Identity/Compartment, enabled sources, local route and effective retention limits. Keep capture state, pause/stop, silence and switch-to-text controls available during capture and processing. Permission to use hardware is not session consent. A remembered surface choice is a user-controlled UI setting, not an inferred planner Preference; declining capture must not cause recurring permission prompts. Guided acquisition explains the missing information, offers another view, and allows stop or an alternative.

**Guided acquisition (`UBU-D0338`).** Offer optional help for a user-selected target, such as acquiring a label. Explain the detail needed and give one short positioning or lighting instruction at a time through the user's selected usable output: text, optional speech or supported assistive technology. Make direction references explicit and let the user repeat or skip a cue. Nonvisual use must not require interpreting the preview. Distinguish an observed framing problem from a suggestion; use only current permitted source evidence and keep uncertain or unreadable details explicit. Present extracted names and values for exact correction during ordinary candidate review; framing success and model confidence cannot establish factual correctness. Each enabled profile declares finite guidance-time and retry limits. Stop immediately on request; at its limit or when useful feedback is unavailable, offer another eligible view, manual entry or deferral without restarting capture. Guidance starts no source, renews no consent and extends no media deadline.

**Access across routes.** Make source controls, guidance, exact-value correction, uncertainty, candidate diffs and accept/reject/defer actions usable through the supported keyboard, screen-reader and switch-access routes. Expose control names, roles, state, focus and errors; no essential action depends solely on color, a pointer gesture, speech or seeing the preview. Apply the continuity rules below when changing channels. Permit text/forms with camera, microphone and speech output disabled and models unavailable. Let users choose channels and assistance without disclosing a diagnosis or inferring disability, intent or planner Preferences. Speak private source details only through a currently permitted user-selected output route; otherwise offer private text or defer. Apply §21.1.2's attention budget to guidance and review: unavailable safe attention or output defers affected interaction, and an access setting cannot widen action permissions.

**Ordinary-attention coverage.** All routes use the same admitted-state operations, validation and review authority; provenance follows the actual source. Camera input is useful where observation is relevant, rather than required for every operation.

| Workflow | Camera/voice route | Text/form route |
|---|---|---|
| First useful concern or quick capture | Show relevant context and describe the concern; propose a note or Task with uncertainty visible. | Type the concern or enter minimal note/Task fields without a media attachment. |
| Clarify or correct | Ask about consequential ambiguity; let the operator identify the target, correct interpretation or leave it unresolved. | Select permitted existing targets and edit exact names, values, dates, durations or constraints; retain unknown/private or unresolved answers without inventing field values. |
| Next action and explanation | Request the next legitimate Task and inspect or hear why it is recommended. | Read the same Task, explanation, constraints and permitted evidence, or the reason no legitimate recommendation is available. |
| Review a proposal | Discuss its source, uncertainty, exact proposed diff and effects; explicitly accept, correct, reject or defer the identified item. | Inspect the same candidate and diff, edit fields, and accept, reject or defer without speaking or capturing media. |
| Act, override and reconcile | Report action, override or progress; observations remain candidates for actual-versus-planned reconciliation. | Use Task controls and interval/status entry, including unknown/private time, under the existing Log, Snapshot and recalculation rules. |
| State a Preference | Speak an explicit pairwise statement and review it before acceptance. | Enter and accept the same pairwise statement; channel choice or repeated behavior creates no planner Preference. |

**Continuity and degradation.** Changing surfaces preserves permitted user-authored drafts and retained candidate/version/evidence links, distinguishes operator corrections from model interpretations, and shows unsaved input. It neither admits a candidate nor duplicates an operation, starts another source, changes Compartment or extends raw-media retention. Changed source/context bindings invalidate pending interpretations and approvals; late results cannot overwrite manual corrections. Missing, declined, revoked, noisy or poorly framed sources and failed or over-budget local backends leave the remaining routes useful. Explain the limitation without claiming recognition and offer text/forms; do not retry capture repeatedly or reroute local AV to cloud processing. With camera and microphone disabled and models unavailable, manual capture/editing, existing Plan inspection, candidate review and action/override recording remain usable through ordinary validation. Forms expose precision on demand without requiring internal-ontology entry; labels, errors, focus and review actions work with keyboard and screen reader, with speech output optional.

**Acceptance evidence.** Before claiming the primary surface delivered, attach revision-pinned scenario results on the qualified hardware profiles required by §4.2 (`UBU-D0339`) and selected under `UBU-Q0177`, identifying enabled sources/backends, policy configuration, outcomes and permitted candidate/admission references. Demonstrate:

- A shown concern plus voice uses a visible detail and permitted context to produce a useful candidate or consequential clarification, followed by operator correction, admission and a legitimate next action or explicit planning limitation. Exercise the other voice workflows and repeat every table row using only text/forms with both capture sources disabled. Equivalent accepted meanings produce matching canonical effects and recalculation, with source-appropriate provenance.
- Camera/microphone absence, denial or revocation; noisy speech or unsuccessful framing; offline/local-model failure and resource exhaustion allow manual completion without lost permitted input, fabricated observations, false saved-state acknowledgments or cloud fallback.
- Keyboard-only and screen-reader completion of text/form entry, exact-value correction and review with speech output disabled; guided acquisition can be stopped or replaced without repeated unexplained capture requests.
- Start, pause, resume, stop and revocation accurately control acquisition and visible state. Raw AV stays local; context and output respect Compartment denials, and export requires its separate authority. Retention behavior matches the approved policy rather than a documentation claim.
- Switching surfaces during a delayed interpretation, duplicate submission/replay, and material context, policy or Device change cause neither stale admission nor duplicate mutations. Preserved candidates carry no live capture consent or pending execution approval across Devices.
- Ambiguous objects, high-confidence interpretations and apparent completion remain unresolved/candidate evidence until required review and validation. Unknown/private intervals and channel choices create no inferred intent, affect, Preference, habit or completion claim. Restricted attention obeys §21.1.2: defer unavailable review rather than demand visual/form interaction or widen permissions.

**User-tested access evidence (`UBU-D0338`).** Before enabling or claiming these guided-access workflows, extend the acceptance evidence with consented user sessions involving people who use the proposed access routes and experience the relevant vision, hearing, speech or dexterity constraints; developer simulations alone are insufficient. Predeclare participant/condition coverage, tasks, Device/software/assistive-technology versions, finite time/attempt and assistance-burden limits, and useful-outcome criteria. Include label framing with blur, glare and cropping; guidance without visual preview; unavailable speech or hearing; limited camera handling; keyboard, screen-reader and switch access; capture declined for privacy; and restricted attention in safe simulations. Use matched tasks to compare guidance, unguided capture and the user's preferred alternative where usable; report unavailable routes and user-reported burden without forcing capture. Repeat entry, exact correction and review with capture and speech output disabled. Exercise private-output denial, stop, retry exhaustion and switching during a delayed interpretation.

Report completion, correction, unresolved, rejected and failed outcomes, time, retries and required assistance by tested condition and route; distinguish useful manual fallback from successful guided acquisition. Require reachable source controls and exit, preserved permitted drafts, faithful uncertainty/review, ordinary admission and no unauthorized acquisition or disclosure. A failed mandatory access/privacy gate or missed declared usefulness limit withholds the affected workflow for that combination; keep its fallback and remaining gap visible. Publish policy-safe results and limitations alongside the support matrix, with explicit participant consent for research retention separate from capture permission and no compulsory diagnosis or raw-media retention. Public access claims name only the tested workflows and combinations; passing one route or population sample cannot establish universal accessibility. These are release evidence requirements, not passing results; hardware qualification, numerical profiles and delivery allocation retain their existing questions.

This resolves interaction coverage and acceptance, not implementation. `UBU-D0339` sets capture-mode/device-role allocation and delivery sequence in §4.2; `UBU-Q0177` retains measured hardware selection and `UBU-Q0160` retains the mobile workload/API choice; `UBU-Q0128` retains numeric restricted-attention profiles and richer actions; `UBU-Q0166`–`UBU-Q0172` retain exact evidence, corroboration, latency, segmentation and retention contracts. Apply the existing evidence, policy and attention gates to each enabled slice. No particular backend, hardware support, latency target or later-phase planning object is established here.

#### 21.1.4 Latency classes, freshness and interruption

**Class contract (`UBU-D0325`).** Use four interaction latency classes. A snapshot capture is a bounded one-shot input; it does not itself admit a canonical Snapshot. Receipt, permitted durable saving, interpretation, candidate admission, planner certification and verification of an executed effect are separate outcomes. A receipt may say "Received" or "Processing"; "Saved" requires the relevant durable write, and neither message implies admission or execution.

| Class | Clock and useful result | Deadline/degradation behavior |
|---|---|---|
| `snapshot_capture` | From controller acceptance of a one-shot capture request through acquisition, interpretation and presentation of a reviewable candidate or explicit unresolved result. | End acquisition and cancel unfinished interpretation at the job deadline. Preserve permitted saved evidence/drafts and show any unsaved input; offer an explicit retry or text/form entry. |
| `conversational` | From acceptance of a completed user turn to the first substantive response or clarification, with a separate total turn deadline. | A missed response deadline ends waiting with an unavailable/deferred explanation; stop pending speech and interpretation. A new request uses fresh bindings and budgets. Restricted attention uses §21.1.2 rather than demanding visual review. |
| `streamed_supervision` | From each permitted observation window to a usable candidate update, with a maximum observation age at presentation and a maximum gap since the last usable update. | On excessive age, gap or backlog, withdraw live guidance and show "Updates delayed/paused". Bound acquisition and queued work; expose permitted coverage gaps rather than replaying old observations as current supervision. This class supplies no safety-critical monitoring guarantee. |
| `retrospective` | From job enqueue to a reviewable report over a pinned historical interval/source manifest, with bounded progress intervals and a total job deadline. | Yield to interactive work. On exhaustion, pause at a permitted durable checkpoint under §21.8 where supported, otherwise stop with retained sources. Label analyzed and missing scope; explicit continuation rechecks current policy and bindings. Partial work cannot claim full coverage. |

**Deadline policy.** Before enabling a class on a Device/backend/mode, require a controller-owned versioned profile with finite local-acknowledgment, first-useful-result and total-job ceilings, applicable source/context-age ceilings, and, for streams, update-gap and backlog ceilings. Enabled work uses positive elapsed ceilings with acknowledgment <= first useful result <= total job. Declare finite concurrency, calls/work, prompt/result, memory/buffer and storage limits plus battery/thermal pause and recovery conditions. Missing or unsupported limits prevent that class's dispatch while manual routes and stop controls remain available. These are policy requirements, not new wire-schema fields or measured defaults.

Bind elapsed deadlines to the local monotonic clock at the table's start event; include queueing, acquisition where applicable, preprocessing, generation, validation and presentation. Progress text, tokens, retries and intermediate frames cannot reset deadlines or count as a substantive result. Before waiting, show the active response/wait limit and fallback in the permitted channel; expiry produces the named degraded outcome and cancels outstanding work. The effective limits are the intersection of that profile, remaining session consent, current policy and §21.1.2's attention/turn budget where applicable. They may tighten during work; continuing beyond them requires a new explicit request and fresh bindings. Restart or clock discontinuity invalidates active jobs rather than extending their deadlines.

**Freshness and authority.** Bind results to their original observation interval, source/session/context and target versions, policy versions and controller job/turn generation. Current guidance/dispatch requires a live generation and applicable age checks; unknown or uncertain age cannot establish freshness. Review/admission rechecks evidence, context, targets and current policy under ordinary review bindings. Reopening retained material establishes fresh review bindings rather than reviving a cancelled job. New generation timestamps and repeated summaries cannot refresh old evidence. Historical analysis may use permitted older evidence within its declared interval; applying findings to current state still requires current evidence and admission. Stale/superseded results remain labelled historical material only where retention permits, and cannot overwrite corrections, satisfy current preconditions or use expired approvals.

Capture updates and planner streaming have separate timing traces. A planner chunk can reach the user only after CPU certification of its current request under PLANNING_KERNEL_CONTRACT.md §4; incomplete/final transport status does not establish an executed effect. Slow processing cannot waive evidence, hard validation, Compartment policy or `UBU-D0324`'s corroboration requirements.

**Offline, battery and thermal degradation.** Keep local pause/stop/silence, visible source state, permitted draft handling and the manual admitted-state loop independent of model/network completion. Defer retrospective work first, reduce streaming rate or analysis scope within a declared profile, then offer bounded one-shot capture or text/forms; pause capture/analysis when limits cannot be met. Disclose the active class, reason, retained/unsaved work and freshness limitation without leaking denied source existence. Resource pressure never extends media retention, silently evicts accepted input/history or starts another source.

Offline operation uses available permitted local backends and manual routes; no hidden cloud fallback. Cached Plan guidance is usable only after §16.5–§16.7's legitimacy/envelope/freshness checks. If those checks fail, show the planning limitation or eligible clarification rather than an obsolete next action. Returning connectivity, power or temperature may permit a finer compute profile, but cannot restart paused/stopped acquisition, speech or queued actions without current consent and fresh controller bindings.

**Interruption.** Pause, stop, silence, cancellation, a superseding turn, source/Compartment or Device change, and consent revocation invalidate the affected pending output/action bindings locally without waiting for a backend acknowledgment. Stop the affected acquisition/output and request cancellation; reject late callbacks for live guidance or dispatch even while the backend finishes. Require bounded cancellation/resource release under its permitted lifecycle; an adapter without that bound cannot enable the class. Preserve already-admitted state, permitted durable sources/drafts and ordinary idempotency outcomes. Interrupted work does not establish an effect or authorize external-action replay; existing outcome/idempotency rules still apply. Reviewing retained candidates later neither resumes capture nor restores approval; restart/handoff requires new local bindings and ordinary review.

**Deadline-boundary qualification (`UBU-D0335`).** An outstanding elapsed deadline expires when the controller's monotonic time is at or beyond its bound. Recheck time and current generation when releasing live output or dispatching; a delayed timer callback cannot make an expired result eligible. Serialize interruption/invalidation with release: an already-observed cancellation or superseding turn wins over a queued result. Pin separate finite expiry-to-visible-fallback, local-stop-to-output-quiescence and cancellation-to-resource-release ceilings; backend acknowledgment cannot delay local invalidation.

For streams, start each permitted observation window's processing clock when that window closes, before queueing, using its mapped controller-monotonic timestamp; an unknown mapping cannot establish live freshness. Bound the acquisition window and consented session separately. Before the first usable update, the first-useful-result deadline bounds waiting. Afterward, only presentation of a fresh, valid usable update resets the update-gap clock; progress, stale results and dropped windows do not. Crossing an age, gap or backlog limit invalidates affected pending live output and withdraws displayed guidance. Resumption needs explicit restart/resume, a fresh generation, current consent and a fresh usable update before showing live guidance.

Run the following fixtures against the actual controller with a deterministic clock and delayed/duplicate adapter callbacks, then demonstrate the corresponding behavior in the pinned real-device trials. Place events immediately before, exactly at and immediately after each applicable elapsed deadline; for age/backlog ceilings, exercise both equality and exceedance under the declared profile comparator.

| Class | Required boundary fixture | Required disposition |
|---|---|---|
| `snapshot_capture` | Queueing/acquisition consumes the budget; interpretation or retry returns at/after expiry. | Acquisition ends, late live output rejects, permitted saved input survives and unsaved input is identified; retry cannot renew the original deadline. |
| `conversational` | Tokens precede a missed useful-response deadline; silence or a superseding turn races with queued speech/results. | Tokens never satisfy the useful-result gate; old speech/results cannot resume or overwrite the new turn, and the bounded fallback remains available. |
| `streamed_supervision` | No first update; then stale/duplicate updates, a gap and queued/in-flight overload, followed by apparent backend recovery. | Bound initial waiting and aggregate backlog; withdraw live guidance, expose permitted coverage gaps and reject old updates without automatic resumption. |
| `retrospective` | Progress/checkpoint activity and competing interactive work straddle total-job expiry. | Progress/yielding cannot renew the deadline; pause at a permitted checkpoint or stop, retaining sources and truthful partial coverage. |

Each fixture report binds profile/implementation/policy revisions, minimized job/generation and source-epoch aliases, clock basis/resolution, relevant event offsets, queued/in-flight high-water marks, and expected versus observed disposition. Include acknowledgment, useful presentation, invalidation, fallback, output-quiescence and resource-release offsets where applicable. Diagnostics inherit source restrictions and do not require retained media or a new Log/wire schema. Release of expired/cancelled live output, stale admission/dispatch, renewal of the same job's deadline, unauthorized resumption or loss of permitted saved input fails that profile's enforcement gate; aggregate timing percentiles cannot excuse it. Later ordinary review of retained candidates with fresh bindings remains permitted. Deterministic fixtures establish controller behavior only, not achieved latency or hardware support.

**Measured enablement gate (`UBU-Q0168`).** Select numeric ceilings only from revision-pinned evidence on each proposed Device/backend/mode; record OS/runtime/model/template, implementation and policy revisions, source/context sizes, load and instrumentation. Measure controller acceptance, acknowledgment, first useful result, completion, observation age at presentation, stream gaps and stop/output-quiescence separately from admission, first CPU-certified planner chunk and final planner response. Report sample counts, median and tail distributions, misses, errors/degraded outcomes, memory/storage, battery/energy where observable, and sustained thermal behavior; unavailable metrics stay explicit. Report successful interpretations separately from unresolved/degraded outcomes; latency alone establishes neither correctness nor usefulness.

Exercise cold and warm operation, competing work and sustained streams; offline/backend loss, low battery and thermal/resource exhaustion; interruption during acquisition/inference/output; queue overload and deadline expiry; source/context changes, corrections, revocation, restart/handoff and delayed/duplicate results. Demonstrate bounded waiting/backlog, working manual fallback, truthful saved/coverage/freshness status, no stale admission/dispatch and no hidden rerouting. Publish the selected finite ceilings and demonstrated fallback for each enabled class; an unmeasured class remains unavailable. Hardware selection remains `UBU-Q0177`, restricted-attention numeric profiles remain `UBU-Q0128`, and delivery sequence remains `UBU-Q0183`. `UBU-Q0168` stays Open for these numerical profiles and measured acceptance; this contract asserts no implementation, supported hardware or achieved latency.

#### 21.1.5 Local AV segmentation and scoped analysis

**Architecture contract (`UBU-D0326`).** Adopt a controller-owned local media broker with the logical adapter profile `local-media/1`. Session consent enables only named sources on eligible execution-enclave Devices on the operator's own hardware; sharing hardware grants no access between enclaves. Capture controls remain visible under §12.2 and §21.1.3. Models and agents can request analysis of supplied media spans, but cannot start/resume capture, enumerate sources, renew consent or enlarge a grant. This profile specifies an interface boundary, not an implemented MCP service, frozen wire-schema change or new planner entity.

**Segmentation and buffering.** Maintain a separate bounded transient ring for each consented source epoch. Seal immutable chunks at the first configured duration or byte ceiling, or earlier on pause, stop or a source/consent/Identity/Compartment boundary. Bounded chunks are the storage/read units; utterances and detected events are separate span references that may cross chunks only within authorized continuous coverage. A detector's proposed boundary cannot extend acquisition or retention. One-shot requests end acquisition at their requested bound; they never become continuous sessions.

Before enabling a source, pin a controller-owned Device/backend/mode profile with positive finite chunk duration/bytes, raw-buffer age/bytes, total raw/decoded/intermediate memory, queue count/age, concurrency/calls, decode work, frame dimensions/count, event/span/result sizes, job deadline, cancellation grace and artifact/scratch lifetime. Use §21.1.4's measured deadline/resource gate and current source-retention policy; missing limits deny acquisition/dispatch. Include pinned chunks and worker copies in aggregate accounting. Evict only eligible unpinned transient material and record coverage gaps; pins cannot extend retention or exceed quotas. If eligible eviction cannot satisfy limits, pause acquisition and cancel/defer affected work visibly. Never spill to unapproved storage, silently discard acknowledged durable evidence or reroute to cloud processing.

**Timeline and coverage.** Each sealed chunk/span manifest binds session, source, Device, source epoch, sequence, immutable content/version reference, half-open interval `[start, end)`, actual sample/frame positions, capture provenance, policy/retention versions and permitted gaps. Preserve media sample/PTS offsets separately from acquisition monotonic offsets, with their clock mapping, wall-time anchor and uncertainty when known. Capture elapsed time and live deadlines use a local monotonic clock; historical intervals retain their original time basis. Ingestion, interpretation or emission times cannot replace observation time. Restart, source replacement and clock discontinuity create new epochs. Unknown mappings stay unknown: do not align different sources or Devices by timestamp equality, sequence or nominal frame rate. Audio/video alignment requires explicit permitted clock mappings and uncertainty. All selectors state the source epoch, time basis and units; reject nonfinite, reversed or out-of-range bounds. Missing, dropped, expired or sampled-out intervals never establish silence, no event or a continuous observation.

**Access and invocation.** Effective access is the intersection of current session/source consent, actor and adapter Device eligibility, capability grant, declared Task/Objective/workflow purpose, source policy and finite profile. The grant names allowed operations, sources/epochs and read intervals, context selectors, recipient adapter, expiry and aggregate work/output limits. Opaque handles are audience- and scope-bound references, not authority by possession; recheck current policy on every dereference. Broker-mediated reads and worker isolation must prevent ambient camera/microphone, unrelated filesystem/context, network and credential access. Physical locality alone does not satisfy this boundary.

Bind each request to its id, actor/grant and versions, session/source epochs, pinned manifest/evidence/context versions, operation/finite selectors, requested read/output intervals, minimized ContextBundle, adapter/model/profile revisions, policy versions, job generation, monotonic deadline and idempotency key. Check these before reading, invoking and releasing each result. Requests may select only supplied permitted aliases; arbitrary paths, URLs, device discovery, executable instructions and unbounded selectors are unavailable. Current denials fail closed with policy-safe errors that do not reveal denied existence or Compartment identity. An exact retry shares the original budget/deadline and idempotency outcome; changed-content key reuse rejects. A retry cannot reacquire expired media or widen scope; expired result artifacts return unavailable rather than a cached disclosure.

A bounded result envelope echoes the request/generation and original manifest bindings, reports complete/partial/unavailable/cancelled/denied status, analyzed/missing coverage, permitted artifact refs/expiry, work used and transform/tool/model provenance. Validate size, profile, references, intervals and bindings before release; malformed or oversized results reject without repair into a usable output. Errors and metadata obey the same disclosure boundary as media.

| Logical operation | Required bounded input | Output and boundary |
|---|---|---|
| `media.frames.extract` | Pinned source spans plus explicit timestamps or sampling interval, finite frame count, dimensions and optional crop within the grant. | Selected frame payloads/leased handles with original sample positions, actual observation times, transform provenance and gaps. No fabricated/interpolated frames or reads outside the authorized read scope. |
| `media.events.detect` | Pinned audio/video spans, an allowed detector/label set and finite window/work/event limits; only purpose-required context. | Advisory event hypotheses with source spans, detector/model revision, confidence/uncertainty and analyzed/missing coverage. No detection means only no reported match in analyzed coverage; it is not evidence of real-world absence, completion or an effect. |
| `media.sections.isolate` | Explicit subintervals or validated event-span references within already authorized sources, with finite section count, duration and bytes. | A manifest of selected spans and, when authorized, a leased local clip handle. Relevance is a proposed selection, not a capability grant; suggestions cannot fetch adjacent or new sources outside the original scope. |

Codec dependency frames or audio padding may be decoded internally only within the declared authorized read scope and decode budget; return only the requested output interval and disclose any permitted padding/coverage limitation. Otherwise return unavailable/partial coverage rather than silently broadening the read. Multi-source results carry every parent span and satisfy the intersection of their disclosure/retention policies. Overlapping windows and derived copies preserve sample lineage and cannot count as independent corroboration.

**Cancellation and revocation.** Serialize pause/stop, source or policy change, grant expiry/revocation, job-generation invalidation, reads and result release in the controller. Invalidation immediately prevents further reads and acceptance of late callbacks; stop affected acquisition and request backend cancellation without waiting for acknowledgment. After the finite cancellation grace, terminate the isolated worker if needed and reclaim its leases/resources. An adapter unable to enforce isolation and bounded release remains unavailable. Resume/restart/handoff uses fresh local consent and bindings; synchronized candidates and persisted handles never restore live access. Later review of permitted retained evidence is a new bounded job, not revival of the cancelled one.

**Intermediate artifacts and audit.** Decoded frames/audio, crops, thumbnails, temporary clips, transcripts, embeddings, prompts and model caches inherit source restrictions. Scratch expires on completion/cancellation; returned temporary artifacts have a finite non-renewing result lease bounded by applicable source policy. Any permitted disk scratch is policy-protected, inventoried with expiry and cleaned before reuse after restart; there is no implicit durable spool, training use or general-purpose cache. Explicit or already-scoped policy authorization is required to retain selected evidence beyond its temporary lease, through ordinary Discovery/candidate storage with lineage and its own allowed purpose/retention. Reads and retries cannot silently renew raw/intermediate retention.

Revocation/purge invalidates disallowed handles and removes app-controlled disallowed copies and reconstructive derivatives under current policy. Already-admitted history follows ordinary append-only correction/redaction rules; raw expiry does not edit a historical claim or prove it false. Record permitted minimized invocation, source-span, policy, budget, gap, cancellation/cleanup and candidate links rather than mandatory raw media, transcripts or hashes. Scene/OCR/transcript text is untrusted data, never an instruction, permission or grant. Results use §21.1.1's Discovery/advisory bindings and ordinary review/admission; they cannot directly mutate canonical state, certify a Plan or authorize external action. Export or a different Device/recipient requires its separately permitted route and approval under §§21.3–21.4.

**Enablement evidence.** Before exposing an adapter, pin implementation, OS/runtime/backend/model and policy/profile revisions and demonstrate:

- Variable frame rate, audio/video clock uncertainty, timestamp discontinuity, chunk-crossing events, overlapping samples and dropped/expired spans preserve original times, lineage and explicit analyzed/missing coverage.
- Forged/foreign/expired handles, wrong purpose/actor/Device, mixed-policy sources, unbounded selectors, codec dependencies outside scope and hostile scene/transcript instructions expose no unauthorized media/context or tool capability.
- Sustained capture, slow consumers, oversized decode/output, pinned-buffer exhaustion and disk pressure respect aggregate limits, pause/defer visibly, preserve permitted saved evidence and leave manual/stop controls usable without cloud fallback.
- Cancellation/revocation during read, inference and result delivery; delayed/duplicate callbacks; source changes, restart and handoff produce no stale release, revived consent or duplicate candidate effects, and release workers/artifacts within the configured bound.
- Normal completion, expiry, crash/restart and policy purge remove disallowed scratch/caches/derivatives; separately retained evidence remains correctly policy-bound and ordinary review is required before canonical effects.

These are required gates, not reported passing tests or physical-erasure claims. `UBU-Q0169`'s architecture is answered; numeric latency/resource acceptance remains `UBU-Q0168`, default raw-media retention follows §12.2 (`UBU-D0327`), and operator discard/summary/hash/clip/export controls follow §12.2 (`UBU-D0328`). Continuous capture cannot be enabled with unresolved effective retention policy. Host fidelity, proof-of-effect semantics and delivery sequence retain their existing gates.

#### 21.1.6 Realtime guidance stops and professional review

**Release envelope (`UBU-D0330`).** Camera access, fluency, confidence and prior assisted success establish neither domain competence nor safe conditions. Before enabling procedural direction, declare a controller-owned, versioned envelope of supported workflows/steps, required context, evidence/freshness and Skill/Resource prerequisites, finite clarification limits, verification/fallback conditions and appropriate professional roles. Missing envelopes and unclassified consequences withhold procedural direction. Models cannot add permitted steps or weaken requirements.

MVP procedural direction is limited to explicitly released, bounded reversible ordinary steps. Individualized clinical diagnosis/treatment, hazardous electrical/gas/structural/machinery procedures, professional legal/financial determinations and safety-critical monitoring are outside this release envelope. Scheduling, recording user-provided information and preparing questions for a professional retain their ordinary planning/review paths. Rich Technique execution and any later domain expansion require their own implemented objects, qualified domain review and release evidence.

**Stop conditions.** Check eligibility before the first and each subsequent step, before output/dispatch, and on material observation, context, policy or attention changes. Withhold the affected step whenever a required condition cannot be established from permitted evidence:

| Condition | Required response |
|---|---|
| Unsupported domain/step or required professional judgment | Stop procedural direction and identify the appropriate domain professional, such as a clinician or qualified trade specialist. More camera input cannot authorize diagnosis, on-site assessment or specialist oversight. |
| Missing context or hazardous uncertainty | Missing/ambiguous target identity, measurements/units, site state, Skill/tools, safety prerequisites, source coverage or freshness remain unknown, including withheld, occluded or expired data. A bounded non-invasive clarification is allowed only within the released workflow and attention budget. If consequential uncertainty cannot be resolved safely, require relevant professional assessment before affected guidance; ordinary low-risk ambiguity may instead defer to manual entry. |
| Contradictory observations, unexpected change or failed verification | Pause the affected guidance; preserve conflicting sources and the unmet condition. Do not average away disagreement or use confidence, repeated frames or model agreement as verification. A safety-relevant conflict that remains unresolved, or a professional-stop fallback, requires appropriate professional review. |
| Possible immediate serious harm | A user report or observed warning may require urgent escalation. Withdraw procedural guidance and give a concise message to seek appropriate local emergency assistance now, without diagnosis, waiting for more images or diagnostic questioning. Uncertain location must not produce an invented contact number. |
| Stop/revocation, unavailable safe attention or controller/source failure | User stop, invalid consent/policy/bindings, unavailable private output, forbidden restricted-attention interaction, delayed/lost sources, backend failure or exhausted limits stop affected work under §§21.1.2–21.1.5. A technical failure alone does not require a professional; hazardous unresolved prerequisites do. |

Hazard judgments are fallible. A stop explains the reported/observed concern or missing requirement without claiming a diagnosis, proven danger or guaranteed detection. No warning detected does not mean safe. This bounds the guidance service; it neither controls the user's physical actions nor turns §22.4's advisory behavioral judgments into product hard constraints.

**Visible stop behavior.** The controller immediately invalidates affected job/step approvals, stops queued speech/procedural output, pauses affected live acquisition/analysis and requests bounded cancellation without waiting for the model. Reject late callbacks and procedural replay. Use the existing paused/ended session lifecycle, not a new canonical state. Show "Guidance stopped", the permitted reason and affected scope, what remains unknown, whether capture is paused, and the appropriate next route. Preserve permitted drafts/evidence and distinguish saved from unsaved input. Offer End, ordinary manual planning/notes or operator-initiated professional contact. Restricted attention permits only its brief output envelope; defer menus and visual review.

Escalation confers no contact, disclosure, export or action authority. Calls, evidence sharing and other external actions still require their separate current permissions and operation-bound review; a stop never admits completion, a world-state correction or a professional handoff. Minimal expert-handoff packets remain `UBU-Q0175`.

**Fresh resumption.** Dismissing a notice, accepting risk, changing channels, rephrasing, restart or Device handoff cannot supply missing eligibility. Resume only after a fresh explicit request, renewed local consent/bindings and review establishing that each stop reason is resolved within the released envelope. Where qualified review is required, retain the reviewer's attributed identity/role and qualification basis, exact findings, inspected evidence, scope/time and conditions; a model's claimed expert opinion cannot satisfy it. Unknown qualifications or unsupported advice remain unresolved. A corrected false-positive signal may clear through ordinary evidence review. Recheck current context/evidence/policy versions before the next step; professional advice cannot expand the release envelope. Unrelated manual workflows retain their normal gates.

**Evidence and enablement.** Reuse existing session, Discovery/candidate and review/Log links for envelope/version, affected step, triggering evidence/time, unknowns/contradictions, stop reason, presented options, required professional role and resolution/resumption bindings. Source restrictions apply to diagnostics and records; do not reveal denied content, identifiers or existence. Record operator dismissal/override through §17.0 without inferring Preferences or requiring justification. No new kernel field, provenance kind or Log event is selected.

Before enabling a guidance workflow, pin controller/backend/model, envelope and policy revisions and demonstrate:

- Ordinary ambiguity safely clarifies or defers; high confidence, missing/occluded/stale context, contradictory readings and failed verification withhold affected steps and route hazardous uncertainty to professional review.
- Unsupported domains, unknown consequences and absent professional qualifications stay unavailable despite disclaimers, risk acceptance or model-supplied clearance; possible urgent harm receives brief emergency escalation without additional diagnostic acquisition.
- Stop/revocation during acquisition, generation and speech cancels locally; delayed callbacks, retries, channel changes, restart and handoff cannot resume instructions or execute queued effects.
- A reviewed correction or scoped qualified assessment permits only a freshly requested eligible step; changed evidence/context/policy invalidates that review. Manual alternatives remain usable.
- Restricted attention and policy-denied evidence remain protected in prompts, notices and audit; escalation causes no automatic call, upload, completion or evidence export.

These are required behavioral gates, not passing results or guarantees of safety. `UBU-Q0174` is answered as a process contract; measured timing, host fidelity, expert packets and release sequencing retain their existing questions and gates.

#### 21.1.7 Interruption, escalation and session consent

**Product contract (`UBU-D0350`, Phase 2).** Apply this policy to realtime/discovery notices, clarification and short-horizon repair alongside `UBU-D0201`/`UBU-D0272` and §§21.1.2–21.1.6. A model may propose an interruption; the controller decides whether to queue it, present it, ask a bounded question or stop an affected service. Presentation permission grants no canonical mutation, Plan adoption, external action, disclosure or additional capture authority.

**Modes and consent.** Display interaction mode separately from processing route, activity/attention and Compartment. The existing mode vocabulary has these operational meanings:

| Selection | Consent boundary |
|---|---|
| `off` | Realtime/discovery acquisition, model interaction and unsolicited interaction output are off. Manual planning/review and required inline operation checks remain available. |
| `text_only` | Use intentionally submitted text/forms; microphone, camera and sensor acquisition remain disabled. |
| `voice_session` | Explicitly start a finite session with named sources and independently selected speech output. The name enables neither camera nor background microphone capture. |
| `discovery_mode` | Explicitly select a Task/workflow purpose, finite duration and permitted sources; observations remain reviewable evidence under §12.2. |
| `meeting_logging` | Show as unavailable until separately released participant-consent, source, retention and disclosure gates pass. One operator's consent cannot authorize other participants or cross-user projection. |
| `local_only_realtime` | Show the named qualified endpoint and eligible local route. Another execution-enclave Device requires its own trust, authenticated route and source-policy checks; ownership alone grants no access. |
| `cloud_assisted_realtime` | Show as unavailable until separately qualified for the exact policy-approved input and destination. No implicit cloud fallback or raw-AV export follows; MVP raw AV remains local. |

Before acquisition, bind explicit consent to actor, capture Device, source selectors, session/source epoch, Task/workflow purpose, Compartment and policy versions, named processing endpoints/route, retention limits and a finite session deadline. Hardware permission, opening a surface, remembered settings and silence are insufficient. Adding a source, widening purpose or changing Identity, Compartment, Device or route requires fresh permitted review/consent; policy tightening invalidates affected work immediately. Resume is explicit and starts a fresh source epoch. Restart, handoff, recovered connectivity and synchronized records never restore live consent. Delivery sequence and qualification remain §4.2 (`UBU-D0339`).

Pause stops affected acquisition, analysis and output; Stop ends the session. Silence stops speech immediately without implying that capture has stopped: show any sources still active and keep Pause/Stop reachable. These controls operate locally without a model, network or valid interaction budget. Pending review may remain after capture ends, within its own retention authority.

**Interruption permission.** Unsolicited realtime/discovery alerts are off by default. A user may enable a versioned local rule for a named condition and scope. Bind the rule to actor/Device, Task/Objective/workflow, permitted evidence/source refs, trigger predicate and uncertainty treatment, freshness limit, allowed presentation channel/endpoint, quiet periods, finite aggregate alert/clarification ceilings, a finite response window when recording nonresponse, cooldown, expiry and allowed resurfacing conditions. Include a finite session/work-window cap across producers, not just a cap per detector. Missing limits or authority permits only policy-allowed review queuing. Capture consent and a sender's interrupt recommendation supply no such rule. Models cannot change these bindings or urgency.

A response/clarification within the current explicit user request uses its remaining interaction budget. Unsolicited delivery additionally needs the rule above. In both cases recheck current consent where relevant, permitted evidence and policy versions, target/context bindings, freshness, attention budget, output privacy, suppression and remaining limits immediately before presentation. Unknown attention uses §21.1.2's conservative envelope; an interruption category cannot widen it.

| Condition | Permitted response |
|---|---|
| Imminent deadline failure | An enabled rule may notify when permitted current admitted Task/commitment data and a labelled risk calculation cross its reviewed lead-time/slack threshold. Explain the threatened outcome, uncertainty and remaining repair option; lateness or Plan deviation alone does not justify repeated prompting. |
| Blocked Task or loss of Plan legitimacy | For the current/selected work, show the failed precondition or relevant blocker and offer review/repair. Withhold an ineligible recommendation or dispatch regardless of alert settings; unrelated blocked Tasks stay in ordinary review unless separately monitored. |
| Unexpected external event | An enabled rule may notify of an evidenced material change to selected work or commitments. Imported/inferred claims retain their candidate/uncertainty labels; notifying never admits the claimed event or changes the Plan. |
| Significant inferred affect shift | Affect inference alone justifies no unsolicited interruption, emotional label or capacity/Preference change. A separately requested, scoped check-in may ask for a user declaration within its finite budget; declining or leaving state unknown ends that prompt. |
| User-requested monitoring | Evaluate only the named predicate, sources, interval and delivery rule. Show freshness and gaps; absence of an alert proves neither coverage nor safety. No safety-critical monitoring guarantee or broader surveillance follows. |
| Clarification | Ask only for a decision-relevant missing detail in the current request or an enabled monitored condition, within the remaining finite clarification allowance. Offer unknown/private, defer or manual entry; unanswered or ambiguous input never becomes consent. |
| Guidance hazard or professional stop | Apply §21.1.6: stop affected procedural guidance and route unresolved hazardous prerequisites to qualified review. Possible immediate serious harm in a permitted active workflow receives its concise emergency-assistance notice only through a currently allowed output/attention envelope. It starts no monitoring, contact, export or additional acquisition. |

A stop explanation within a currently requested workflow is a service response, not permission for background alerts. Required controller/service stops do not wait for an alert grant: invalid consent, policy, bindings, freshness, evidence prerequisites or resource limits stop the affected processing/guidance under §§21.1.2–21.1.6. Quiet/silence and dismissal cannot permit an otherwise forbidden operation. Keep its policy-safe inline status discoverable without recurring speech or modal prompts; no urgent label overrides revoked output, silence, denied disclosure or unavailable attention. A stop constrains UbU's service, not the user's physical actions or Task lifecycle.

A notice offers an explanation and a next choice, not a command. Repair candidates stay outside admitted state; recalculation uses admitted inputs and CPU certification, and relying on a replacement default Plan follows ordinary Plan/Calendar review. Neither an interruption nor failure to respond authorizes Task completion/failure, Calendar changes, automatic Plan switching or external escalation.

**Delivery and suppression.** Prefer review queuing, then an allowed turn/Task boundary and the least disruptive enabled channel. Immediate service invalidation remains independent. Coalesce notices by Device, permitted scope, condition episode and materially relevant evidence; new model/template versions, rewritten wording, age, restart and repeated observations do not create a new episode. A resolved condition must have permitted evidence before a later recurrence can form a new episode. Aggregate budgets also limit genuinely new conditions.

Offer Dismiss until material change, Snooze until a user-selected time/condition, Disable this rule and Review later, without a compulsory reason. Persist minimized local suppression across views/restart under retention policy, alongside ordinary candidate rejection/deferral under §21.2.1. Material change can reopen a dismissal only within the existing rule and limits; snooze follows the user's selected reopening condition. Expiry, unavailable evidence or exhausted budgets never create retry prompts or new capture. Nonresponse never increases frequency, urgency, volume, channel or audience. Mandatory eligibility/conflict checks remain visible at the affected operation; dismissing their notice does not resolve the blocker.

**Overrides and calibration.** Record a presented notice's dismissal, snooze, disablement or nonresponse through §17.0's existing `decision_recorded`/`alert_dismissed_or_ignored` path; a bypass or changed action uses its existing applicable decision kind. Link the permitted notice/candidate/version, trigger/evidence and policy versions, presentation time/channel, selected control, suppression scope/deadline and any separately admitted change. Explicit override records identify the actor and `authority_source = user_override`. Reasons remain optional with `none_given` when absent; `preference_inference_allowed = false` for these records. Record nonresponse only after established presentation and the rule's finite response window, distinguishing it from unknown delivery; it proves no attention, assent or rejection of the underlying claim.

During user-requested review, permitted minimized history may support proposals to reduce false positives, improve timing or revise explicit monitoring rules. Show uncertainty, corrections, unanswered notices and user-reported burden; do not optimize compliance, engagement, inferred emotional susceptibility or surveillance. Calibration cannot silently expand sources, channels, thresholds, frequency or authority. Policy changes need explicit acceptance as interaction settings; planner Preferences still require their separate accepted statements. Monitoring/override history grants no training or secondary-use consent.

**Visible status.** Keep a compact, accessible indicator of actual session/source state and pending review, with details on demand: active Identity/visible Compartment, each enabled source and capture Device, named processing endpoint/route, session expiry, effective raw-buffer/artifact retention deadlines and cleanup state, current delayed/paused coverage, enabled alert rule/quiet state and remaining limits. Distinguish transient capture, authorized saved evidence, unresolved candidates and admitted updates; show receipt/saving/admission separately. Policy-filter counts, labels, reasons and even existence. Pause/Stop/Silence, switch to text, inspect permitted evidence and reject/defer controls remain usable independently of processing; retention/deletion actions follow §12.2.

**Enablement evidence.** Before exposing interruption behavior, pin controller/adapter, Device/mode, policy/rule and finite profile revisions. Exercise:

- Opt-in deadline/blocker/event rules with current versus stale/denied evidence; absent consent, missing limits and affect-only inference produce no unsolicited delivery, mutation or capture.
- Repeated/rewritten candidates, many distinct detectors, unanswered notices, dismiss/snooze/disable and restart respect coalescing, aggregate ceilings and durable suppression; a permitted material change or chosen snooze deadline reopens only under the current rule.
- Stop/Silence/revocation and deadline expiry racing with queued speech/results, source changes and Device handoff invalidate locally; late callbacks cannot speak, resume capture, reuse approval or overwrite corrections.
- Restricted attention and private-output denial defer unavailable prompts; an active-workflow professional/urgent stop supplies no automatic call, upload, diagnosis, Task update or expanded monitoring.
- Overrides require no reason and create only the correct minimized decision/suppression records; nonresponse/unknown delivery stays distinct, and calibration creates no inferred Preference or silently broader policy.
- Disabled alerts preserve required operation checks and useful manual review/repair; displayed capture, route, retention, review and pause/silence state match actual behavior without denied-context leakage.

These are acceptance requirements, not passing implementation or safety evidence. `UBU-Q0128` retains numerical restricted-attention profiles; `UBU-Q0168` retains measured timing/resource ceilings. Existing host, retention, endpoint and mode-release gates remain required; this policy adds no kernel field, Log event type or capture-release commitment.
```

#### DESIGN.md §1 — Overview

```markdown
## 1. Overview

**UbU** is a privacy-first planning, coordination, and self-governance system for implementing life logistics.

UbU takes messy real-world inputs—tasks, calendar events, messages, external events, user preferences, physical/emotional state, Resource availability, Skill capability, integration data, realtime interaction streams, and agent or worker outputs—and turns them into explicit, inspectable, recalculable Plans.

UbU is not merely a task list or calendar application. Its core motivation is to help an individual human being plan and implement the logistics of an actual life: what matters, what must happen, what the world must contain, what the user is capable of doing, what can be learned, what should be bought or delegated, and what should happen next. Organizational coordination is an important emergent property of the same model, not the root purpose.

The canonical UbU use-case formula is:

> **Objective + Current State + Constraints + Resources + Skills + Techniques + Preferences + External Options → Legitimate Plan**

The canonical use-case statement: UbU helps an individual user solve everyday life problems by transforming a desired outcome into a legitimate, executable plan. It does this by modeling the current state of the user's life, the desired state, required Tasks, available and missing Resources, available and missing Skills, reusable Techniques, affect and energy constraints, financial tradeoffs, public or marketplace options, expert-guided alternatives, and evidence from execution. UbU's purpose is not merely to schedule time, but to help the user determine what path can realistically move life from the current state toward the desired state.

The deeper purpose is to help people build the capabilities, resources, routines, and relationships needed to actually live the life they choose — not just to schedule it.

**Access within ordinary life logistics (`UBU-D0338`).** Present guided camera acquisition and voice/text/form choices as ordinary ways to capture concerns, review proposals and act on Plans. Keep UbU's primary identity as a privacy-first life-logistics system; access features are available without a disability label or special onboarding category. Describe concrete benefits and limitations for tested workflows and Device/assistive-technology combinations. Camera availability alone establishes no accessibility claim.

The first MVP is designed around **dogfooding**: using UbU to coordinate the design, development, release, and maintenance of UbU itself.

Recent LLM and agentic-AI changes reinforce UbU's core boundary: realtime, multimodal, tool-using, memory-bearing models are valuable interaction and extraction backends, but UbU remains the user-sovereign state-transition, planning, logging, privacy, and review layer.

**Advisory generation policy (`UBU-D0341`).** For the MVP/Phase 2 advisory boundary, the operator configures sampling behavior in the selected inference runtime. UbU adds no generic decode Settings or application-selected temperature, top-p/top-k or repetition penalties, hidden request overrides or automatic retry tuning. Existing model/endpoint selection, timeout and review seed/ceiling behavior remain; the review seed provides no cross-model or cross-runtime reproducibility guarantee.

UbU owns bounded validator-safe producer grammars (`UBU-D0302`), finite request/output and job budgets, cancellation, validation and ordinary candidate review/admission. Required grammar and resource/output-limit fields may be sent to the runtime without transferring these contracts to sampler policy. A runtime unable to meet them leaves the affected operation unavailable with an explicit limitation and eligible retry/manual route. Abort, timeout and invalid output preserve saved state and admit no generated state changes. Retries retain current authority/input bindings and finite call/time budgets; failure cannot relax validation or authorize another route.

Document each supported adapter's required grammar/limit behavior and runtime sampling configuration instructions. Pin observable sampling configuration or its digest in qualified Device/backend profiles and label unknown effective defaults explicitly. Configuration changes require affected qualification checks under `UBU-D0331`/`UBU-D0336` where applicable. Before claiming conformance, require adapter fixtures that inspect requests for absence of application-selected sampler overrides while preserving existing review seed/ceiling behavior and required grammar/limit fields; exercise abort, timeout and invalid-output paths for preserved saved state, bounded retries and unchanged review/admission requirements. These are required checks, not passing runtime evidence or a diagnosis of the Vocabulary abort.

For Phase 1, UbU must also be understandable as a first-person user experience. The minimal user-facing loop is: answer a small number of bootstrapping questions, allow UbU to construct an initial context model, receive one recommended next Task, inspect why that Task matters now, act or override, then let UbU learn from the result through Logs, Snapshots, and recalculation.

For Phase 2, a single-live-session sync implementation is sufficient (`UBU-D0312`), with arbitrary N Devices, concurrent offline mutations, partial replicas, and transport-independent integrity-protected statements. Direct exchanges, imported bundles, and user-selected encrypted stores share the same causality, policy, and conflict rules; no Device or transport becomes a permanent canonical authority. Full automatic peer mesh is optional. DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §26's topology research remains a freeze gate, with at least three Devices demonstrating serialized direct exchange and encrypted store-and-forward, recoverable delivery, deterministic admission/conflict handling, and Compartment-safe partial replication as specified by `UBU-D0312`.

The minimum indirect path for the first Phase 2 sync release is implemented, authenticated encrypted bundle export/import through a user-selected file store, with at least one documented external rclone or rsync copy workflow exercised before freeze (`UBU-D0313`). Manual export/copy/import is sufficient; built-in clients and background delivery are optional. Encryption and recipient authorization protect statements, payloads, and manifests before storage, while ordinary admission and Compartment policy govern every import.

**Interrupted sync recovery (`UBU-D0314`).** Maintain a durable, policy-protected recovery record throughout each exchange. Connection loss, interrupted import, or restart without an admitted final checkpoint for the declared exchange scope records `incomplete_sync_session`. End the failed live session and release its slot while preserving recovery records and queued work; other Devices may synchronize and ordinary policy-authorized offline work may continue against an explicitly stale local view.

Fully verified, authorized statements whose required causal/admission dependencies are satisfied may be admitted incrementally. Persist each admission outcome, resulting state, and deduplication metadata as one crash-consistent unit. These admissions survive disconnect, retry, and discard, subject to existing redaction/purge policy. Partial statements, unverified payloads, and items with missing required statements or dependencies remain staged; they cannot be treated as admitted mutations or supply authoritative inputs to Plans or external projections.

An integrity-protected `checkpoint` or equivalent authenticated manifest commit must bind the declared exchange scope and expected statement/payload inventory, or its content-addressed equivalent. Admit it only after all material required for this Replica's authorized replication level has been durably verified and the required dependency/admission checks have completed. Report completion only after that checkpoint admission is durable, and only for that local scope. A checkpoint cannot admit contested mutations or prove other Devices current. A lost acknowledgment leaves delivery unknown to the sender; replay cannot undo the recipient's durable admissions. Outgoing statements remain pending for an intended recipient until the sender durably records a matching authenticated acknowledgment of that recipient's durable checkpoint admission. Exchange acknowledgments directly or through encrypted return bundles; transmission, file-copy success, or local checkpoint creation is insufficient.

The recovery record tracks exchange/bundle identity, source/recipient Device references, authenticated manifest reference if available, last locally admitted checkpoint, received/missing content references, and durable per-statement admission/idempotency outcomes. Reconstruct progress after restart from durable records. Recovery storage, dependency fetches, retention, and diagnostics obey current Zone/Compartment policy; restricted views reveal neither denied payload nor Compartment identity. Retention cleanup preserves permitted causality, deduplication, and incomplete/discard outcome metadata; missing content must be retransmitted rather than assumed received.

At the next authorized connection or import, resume from durable progress, or replay the original statements if no usable cursor exists. Preserve original statement ids, content hashes, idempotency keys, causal parents, and observed versions. Recheck integrity, Device trust, actor authority, current routing/Compartment policy, object/policy versions, and dependencies before admitting pending material or disclosing retained content. Recovery records and old checkpoints grant no authority. Automatic recovery is allowed only for deterministic, idempotent, non-authority-changing cases under `UBU-D0248`; revoked Devices and review-required conflicts retain their ordinary rejection/review rules.

Offer Resume/retry and Discard through the existing policy-safe incomplete/stale-sync diagnostic, showing the last completed scope and permitted pending counts. Discard removes unfinished incoming staging and its cursor while preserving already-admitted history, locally authored queued work, and still-required policy-permitted causality/idempotency metadata; record the discard outcome. It does not emit a tombstone or undo admitted mutations. A transient interruption alone requires no blocking merge prompt. Review-required conflicts use DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §17's blocking `manual_decision` diagnostic and admitted `conflict_resolution` statement. Apply DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §22's Plan invalidation/recalculation rules to admitted changes even while the exchange remains incomplete.

**Sync-state warnings (`UBU-D0315`).** Multiple Devices may remain powered on and perform policy-authorized offline work simultaneously; only live sync exchanges serialize under `UBU-D0312`. Explain this once in Phase 2 onboarding. Ordinary offline use, Device switching, peer absence, and elapsed time alone cause no modal warning or recurring notification. Time-based reminders are opt-in with a user-selected cadence.

Keep one compact, non-blocking status for the active permitted sync scope, with details on demand: last durably admitted checkpoint and its scope/time (or "No completed exchange yet"), permitted locally authored pending counts split into awaiting exchange and awaiting recipient acknowledgment, staged incoming work, and incomplete-session state with Resume/retry and Discard per `UBU-D0314`. Say "Delivery unconfirmed" for a missing acknowledgment. File-copy success and a completed local exchange never justify "All Devices are up to date". A disconnected peer's changes are unknown unless policy-permitted evidence establishes pending state; age alone proves neither new changes nor their absence.

Show an inline Plan/Calendar freshness explanation where relevant inputs may be missing or stale: "This Plan uses this Device's current view; other Devices may have changes." Identify confirmed remote pending changes only when supported by permitted evidence. Locally authored outgoing work is saved local state, not evidence that the local Plan is stale. Label protected blocks and policy-withheld items separately from exchange failures; retrying sync cannot expand visibility.

Before external projection whose relevant permitted inputs have known pending changes, an incomplete exchange, or unknown peer freshness, present an inline review: "Other Device changes may be missing. Synchronize before projection, or use this Device's current view." Offer Sync/exchange (including export/import), Continue with current view, and Cancel. Continue requires explicit stale-state acceptance once per projection operation or reviewed batch, bound to the Device, operation, payload/input and policy versions, and permitted scope. Record it under §17.0 using `decision_recorded` with `decision_kind: safeguard_advisory_bypassed`; recheck the bindings before execution. This accepts freshness uncertainty only. Current authority, Device trust, integrity, Compartment/export policy, Plan-legitimacy requirements, and `UBU-D0248`'s blocking `manual_decision`/`conflict_resolution` rules still govern eligibility. If those rules forbid execution, omit Continue. Mere interruption or missing peer contact creates no blocking merge prompt.

Coalesce advisory explanations by Device, permitted scope, condition, and material evidence. Offer Dismiss until material change or Snooze until a user-selected time; persist the local choice across views and restart while keeping status discoverable. Reopen for material changes affecting that warning, such as newly learned permitted pending statements or changed relevant Plan/input versions; snooze may also reopen at its chosen deadline. A new projection operation needs its own scoped choice. Age or restart alone cannot reopen a dismissed advisory; opt-in reminders may follow their chosen cadence. Dismissal needs no justification and is logged under §17.0 without turning the warning into a synchronized Task. Advisory suppression cannot hide a required blocking conflict diagnostic.

Generate visible text, counts, urgency, and reappearance only from evidence or derived warnings allowed at this Replica's replication level. Restricted notices use DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §23's "A protected object cannot sync to this Device." without denied payload, source object identifiers, or Compartment ids/labels; omit object-specific notices when even existence is denied. Warnings preserve intentional partial replication and never recommend powering off other Devices or switching to a single primary Device.

**Future schedulable manual review (`UBU-D0316`).** A later versioned extension may admit a shared Dynamic review Task with independent, coherent scheduling inputs and a protected conflict-episode identity. Its target is evidence to review, not admitted contested effects. Place it only in validated unaffected Calendar capacity; otherwise keep it unplaced. The diagnostic/prompt must still admit a permitted source-conflict `conflict_resolution` without review Task creation, placement, or completion. Conflicts in the Task, its scheduling/projection artifacts, or resolution workflow terminate at that diagnostic and never spawn review-of-review Tasks. Task lifecycle reconciliation follows resolution; snooze or deletion cannot clear the underlying blocker. Current authority and source privacy govern every step. The first Phase 2 sync release retains §17's diagnostic-only model until this extension's versioned contracts and behavioral gates pass.

**Task lore after Clarify (`UBU-D0317`).** After a Clarify round, persist the operator's submitted answers through ordinary admission independently of synthesis, then offer one additional local synthesis job. The Task description is readable narrative. Keep the pre-existing description, subsequent operator edits, exact questions/answers and their necessary context as separate durable per-Task source records with stable ids, versions, actor/Device/session/round provenance, effective/recorded times, hashes and privacy/retention policy. Versioned context refs are sufficient only while the referenced context remains retrievable. Corrections append records with explicit supersession links; they never edit originals. Accepted model prose is a derived revision, not independent evidence. Raw audio/video retention is not required.

The local model returns an `AdvisoryCandidate` containing proposed prose, model/template provenance, the complete source manifest/digest and a source-to-prose coverage map. Every answer, qualification, original lore statement, correction and unresolved issue must map to prose spans; every factual prose assertion must map back to sources. Equivalent repetitions may share wording. Superseded assertions retain a temporal account of the old and corrected meanings; disagreements, unknowns, declined answers and open questions remain explicit. Preserve attribution, negation, conditions, quantities and uncertainty. Unsupported inferences remain separate labelled suggestions, not synthesized lore.

Review shows the description diff and a source-by-source checklist with exact source text beside its mapped prose. Deterministic checks require complete source/span coverage, valid references, finite bounds and current bindings; structural coverage and another model's opinion cannot establish semantic fidelity. Explicit operator approval confirms readable prose preserves all source meanings and invents no facts or agreement. Offer edit, regenerate or keep the prior description; edited prose needs renewed coverage review. Recheck authority, policy, Task-description/source/candidate versions and source digest before atomically admitting the description revision and its provenance. Concurrent changes invalidate approval and use ordinary conflict review. Failure, rejection or model unavailability preserves the prior description and saved answers. Lore approval does not admit typed planner constraints or authorize external action.

Later Clarify uses admitted prose plus the source manifest, unresolved issues and correction links. Supply exact question/context/answer records for topics being revisited and make remaining permitted originals retrievable. Check the originals before treating a topic as settled; absence from a prompt is not absence of knowledge. Missing records make context explicitly incomplete. A partial Replica unable to establish the full source frontier may offer a scoped preview but cannot replace the Task description.

Declare finite storage, prompt, result and job time/call limits. Retain source history and admitted revision/provenance while the Task is retained; archival must remain queryable. Chunk long inputs along complete source records and track coverage across chunks through the final prose. If complete reviewable prose cannot fit, stop and keep saved sources rather than truncate or drop knowledge; no cloud fallback. At storage exhaustion, pause Clarify and show any unsaved input; never claim it saved or silently evict history. Rejected candidate payloads follow the ordinary minimized retention lifecycle.

Source records, synthesized prose, coverage maps and subsequent context inherit all source disclosure, routing and retention restrictions; local processing does not waive them. Reject synthesis into a Task whose destination policy cannot protect its sources. Recheck policy on every read, sync and export. Authorized redaction/purge also removes disallowed content from dependent prose, revisions and cached context; prevent reconstruction from retained derivatives. Keep only permitted minimized gap metadata, disclose incompleteness without revealing denied existence, and never claim lossless retention beyond that scope.

**Recurring-busy workaround retirement (`UBU-D0318`).** Timed expanded recurrence instances already capture as individual Tasks. Retire a temporary manual capacity blocker only for an operator-reviewed source/series and planning scope with verified replacement occupancy. Full series-level import is not a prerequisite when expanded instances cover that scope; unsupported inputs, missed/on-demand capture and incomplete coverage retain explicit residual blocking.

Record a policy-permitted coverage manifest binding the implementation revision, source/series references, capture window/cursor and completeness evidence, source and admitted input versions, planning horizon, expected busy UTC intervals, occurrence-to-Task lineage, and blocker references. Every relevant busy occurrence must map to one active capacity-bearing representation; record how any uncovered part of the blocker remains protected. Titles or coincident times do not establish occurrence identity. Repeated capture, restart and replay preserve lineage without duplicate Tasks. Moves, cancellations and series changes supersede prior active reservations through ordinary admission while preserving logged history; absence from an incomplete response cannot establish cancellation.

Fixed busy commitments use capacity-occupying Static Tasks at validated UTC start/end times and bound chunks under `UBU-D0279`. Capture counts, Calendar display and Task existence alone do not prove occupancy. Verify the CPU reference path and each enabled planner backend against expected occupied/free intervals: flexible placements cannot enter busy time, and a source occurrence, its projection and its temporary blocker cannot consume capacity twice. Distinct commitments that genuinely overlap retain their conflict rather than being deduplicated by time.

Before removal, show the exact blocker replacements and residual intervals and obtain explicit operator approval bound to the manifest and current versions. Apply them through ordinary admission as one batch, or an equivalent serialized handoff that prevents Plan certification/projection of intermediate missing or doubled occupancy. Invalidate affected Plans and certify recalculation before reuse or projection. Failure or stale inputs preserve protection and require fresh review. Retire only the identified workaround, preserving source commitments and history. Advancing the horizon or receiving material source changes requires renewed coverage checks before using affected capacity; missing, stale or policy-withheld data never proves free time. Evidence and diagnostics obey current Compartment policy; this process neither grants capture/export authority nor bypasses existing sync/projection checks.

**Measured local multimodal hardware (`UBU-D0331`).** Qualify an exact physical-hardware/software profile, separately identifying every execution-enclave Device and its capture, review, inference and optional planning roles. Pin hardware/RAM, OS build and security-update posture, drivers/runtime, model and weights digest, quantization, context/media sizes, storage/key configuration, UbU/template and policy revisions. Declare enabled sources, latency classes, concurrency, supported session duration and operating conditions. Primary mobile interaction does not select a full mobile planner or compute API (`UBU-Q0160`); capture-mode/device allocation and delivery sequence remain `UBU-Q0183`.

Before testing, freeze the scenario set, useful-outcome rubric and finite acceptance limits for quality, latency/freshness, memory/storage, battery/thermal behavior and cancellation. Use §21.1.3's ordinary interaction and text/form workflows, with synthetic or expressly consented fixtures covering visible details, exact names/values, noisy speech, ambiguity, corrections and unsupported requests. Review grounding, preserved uncertainty, fabricated assertions, correction burden and useful candidate-to-admission/next-action outcomes; transcripts, token rates and plausible conversation alone cannot pass. Count accepted, corrected, unresolved, rejected and failed outcomes separately over all trials. Numeric interaction profiles follow §21.1.4 and remain `UBU-Q0168`'s measured work.

For each proposed Device/backend/mode, run at least 30 cold-start and 100 warm trials using the pinned fixtures, plus at least 30 minutes of its proposed concurrent capture/inference/UI workload and the full maximum supported session if longer. Measure the complete capture-to-review route, including any authorized worker transfer. Record ambient conditions, charging/battery state, competing load, instrumentation and repeat procedure. Report §21.1.4's separate timings with sample counts, median, p95, maximum and misses/errors; include peak aggregate CPU/accelerator memory, model loading, buffers/caches and concurrent manual/planning work at the declared state/context limits. Report model/state/approved-artifact storage, peak scratch and required free-space reserve, energy/battery measurements and uncertainty, temperature/throttling and performance over time. Unavailable metrics stay explicit; missing evidence for a required limit prevents qualification. A short successful run cannot establish sustained or longer-session support.

Endpoint qualification must demonstrate a maintained security-update posture, installation/model integrity checks, OS access isolation, protected Device keys and access-controlled encrypted persistent storage with documented key custody and recovery. State the threat envelope: locked-device loss, unauthorized unprivileged processes, unintended persistence and unauthorized egress; do not claim protection from a compromised OS or malicious local administrator. Exercise source/Device revocation, lock/restart, denied reads/egress, storage exhaustion and interrupted cleanup. Inventory raw/intermediate/retained copies, OS/runtime swap, hibernation, crash dumps, caches and backups; enforce §12.2 and §21.1.5 on every applicable path or disable the affected mode. Encryption alone cannot satisfy transient-media retention. A rooted/debug planning-parity device supplies no endpoint-security evidence by itself; hardware attestation remains optional under DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §26, and cleanup establishes no forensic-erasure guarantee.

Publish a policy-safe support matrix linking the exact profile, enabled workflows/classes, measured usable-memory/free-storage envelope, quality and timing results, sustained operating duration, security/retention outcomes and demonstrated fallback. Test offline/model loss, low battery, thermal/resource pressure, stop/silence, delayed results and manual completion without hidden cloud routing or loss of permitted saved state. A worker on another Device needs its own qualified endpoint/profile and existing route/Compartment checks; a same-Device worker remains an execution context unless separately admitted under §23.1. Ownership grants no disclosure authority. Only profiles passing all declared limits and mandatory security/retention gates may be operator-approved for those slices; material profile changes require affected rechecks. Label the selected configuration a tested sufficient profile among the tested candidates, with failures and limitations visible; infer no universal phone minimum or local-versus-cloud quality ranking. `UBU-Q0177` stays Open until a reviewed evidence-backed profile is selected; a protocol alone cannot close it. Lower-cost reach remains `UBU-Q0178`.

**Inference runtime reuse qualification (`UBU-D0336`).** Extend the endpoint gate to the deployed runtime's effective process permissions and source-dependent state across jobs. Inventory prompt/KV caches, embeddings, decoded media, buffers, leased handles and scratch separately from verified read-only model weights. A warm process or shared weights grant no access to prior job context. Enforce §21.1.5's lifecycle before reuse; separately authorized retained artifacts enter a new job only through fresh broker access and current bindings. If safe reuse cannot be demonstrated, qualify an isolated per-job runtime or leave the affected mode unavailable.

Run paired synthetic camera-and-voice jobs A/B under distinct grants, with B denied A's source and context. Put a unique synthetic detail in A and request it from B without supplying it. Exercise both job orders, ordinary completion, cancellation and source/policy revocation, crash/restart and resource pressure; include overlapping jobs only where concurrency is proposed. Inspect B's actual supplied context, runtime reads, result/artifact releases, cache/handle lookups and diagnostics for unauthorized A content or identifiers. Probe ambient filesystem, credential, camera/microphone and network access through the runtime's actual interfaces under deployment permissions; model refusal or a configured permission declaration alone cannot establish denial. Include an authorized control supplying the synthetic detail to B through the broker to verify the detector and permitted access path. Absence of a detail in generated prose alone cannot prove isolation.

Record pinned fixture/grant versions, expected and observed access/release outcomes, runtime permission observations, reset/termination behavior and policy-safe evidence links in the qualification report. Every enabled reuse/concurrency mode must demonstrate the allowed control and zero unauthorized reads/releases in these fixtures; unobserved mandatory paths remain unqualified. Repeat the cold/warm and sustained workload measurements with the enforced isolation, reset and cache restrictions active, accounting for their time, energy and aggregate memory/storage costs. Publish limitations beside the support matrix. These fixtures supply no passing runtime, hardware selection or forensic-erasure result.

**Economically accessible local paths (`UBU-D0332`).** Compare two candidate routes for a bounded camera-and-voice slice under `UBU-D0331`:

- **Existing single Device:** bounded still-image capture and push-to-talk audio, with smaller/quantized or task-specific local models capable of CPU inference, finite context and serialized inference jobs. Evaluate supported older hardware without assuming that age, nominal RAM or model size establishes eligibility.
- **Existing personal worker:** capture and review on one Device, with bounded inference on an existing operator-owned laptop/desktop worker. Qualify both endpoints and the complete authenticated encrypted transfer route. Current Device trust, Compartment/source permissions and original consent, freshness, retention and job limits govern every copy; ownership or physical proximity grants no authority. Worker loss leaves a qualified same-Device route or useful text/forms.

Each candidate uses legally locally runnable software/weights with disclosed licenses, installation downloads and fees, and requires no hosted inference, paid API credential or recurring model-service subscription. Dedicated accelerators and continuous streaming are optional for this evaluated slice; `UBU-Q0183` retains overall release scope and sequencing. Demonstrate same-Device operation with networking disabled and worker operation with Internet access blocked. Ordinary manual entry, exact correction, candidate review, admitted-state inspection and action recording remain available with models absent under §21.1.3.

Before dispatch, show the selected route, enabled modalities, measured latency class, finite wait/context/media limits and fallback. Reduce resolution, frame count or context only within a qualified profile, retaining transform provenance and explicit missing coverage. Slower processing cannot extend raw-media deadlines or freshness, silently omit necessary detail, or weaken uncertainty, corroboration, review, admission or Plan certification. Unsupported work returns an explicit limitation and an eligible retry or manual route. Switching models, endpoints or sources requires fresh bindings and applicable consent; cost/resource pressure grants no routing authority.

Extend `UBU-D0331`'s frozen useful-outcome rubric and real-device trials with a paired comparison of these routes on the same permitted §21.1.3 fixtures and declared workload. Include at least one CPU-only inference candidate and one existing-worker candidate; unavailable or ineligible candidates remain visible. Require a visible camera detail plus speech to produce a grounded reviewable candidate, exact correction/admission and a useful next action or explicit planning limitation. Report successful, corrected, unresolved, rejected and failed trials separately, plus total workflow time and operator setup, maintenance and correction effort. A text-only fallback cannot establish multimodal reach.

Declare the comparison baseline, usage period/workload and evaluated cash, effort, energy and storage budgets before trials. Report incremental outlay for already-owned hardware separately from acquisition/refurbishment and replacement assumptions. Include necessary software/model fees, download/connectivity, measured workload energy, storage, secure upkeep and accessories for the whole route, including a worker. Price observations state date, region and currency; running-cost estimates state usage period, workload, rates and uncertainty. Distinguish measured outcomes, quoted prices and estimates; unknown required costs prevent a claim that the budget is met. Use "lower-cost" only for a specified comparator and matched useful workload, with accuracy, latency and effort tradeoffs beside cash savings. User-supplied budget scenarios establish an evaluated envelope, without inferring individual affordability or population reach.

Publish costs, effort, qualified workflows/classes and failures alongside `UBU-D0331`'s support matrix. Select a path only after the operator reviews evidence that it passes the declared usefulness, resource, cost and mandatory security/retention limits. If no path qualifies, retain useful manual operation and the explicit multimodal gap. `UBU-Q0178` remains Open until an evidence-backed lower-cost path is selected; this protocol establishes no price, device minimum or supported configuration. `UBU-Q0177` and `UBU-Q0168` retain hardware and numeric-profile acceptance; CPU inference here selects no mobile planner or compute API under `UBU-Q0160`.

**Entry cash and useful-outcome accounting (`UBU-D0337`).** Extend `UBU-D0332`'s comparison with explicit ownership scenarios: capture/review Device only, or capture/review Device plus an existing worker. Include the Device-only scenario even if its worker route is unavailable. Evaluate acquisition/replacement separately; an already-owned worker's zero acquisition outlay cannot be used in a scenario that must buy one.

Before trials, declare finite entry-cash, cash-per-budget-period and total-horizon ceilings alongside the existing effort, energy and storage budgets. Entry cash includes every mandatory payment before the first useful multimodal outcome. Show subsequent payments in the period when due, including secure upkeep and necessary replacement under the declared assumptions. Amortization or unconfirmed resale proceeds cannot make a due payment fit. State the security-support and replacement assumptions over that horizon; continued use after an endpoint's declared maintained-security eligibility ends cannot be priced as a saving. Unknown required payments prevent a budget pass.

For each measured workload, report all-attempt totals for run-related cash, energy and active operator minutes, including failures and retries, and divide each total by the number of useful multimodal outcomes. That denominator includes only successful or successfully corrected camera-and-voice workflows meeting the frozen rubric; keep those outcome counts separate. Manual-only completion after multimodal failure remains fallback and cannot enter the denominator. With zero qualifying outcomes, report per-outcome values as undefined and the workload as unqualified. Period estimates retain the observed outcome mix and disclose uncertainty; fixed entry, upkeep and replacement costs remain in whole-route totals.

Add ownership prerequisites, entry/period payment schedules and ceilings, all-attempt totals, outcome denominators and per-outcome values to the cost/support matrix, with evidence links and pass/fail/unknown for each budget and mandatory gate. Select only for scenarios whose whole-route budgets and `UBU-D0331`/`UBU-D0336` qualification gates pass. Keep any no-worker multimodal gap visible. This extension selects no measured configuration or cost envelope; `UBU-Q0178` stays Open pending operator-reviewed results.

---
```

### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md — referenced sections

#### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §4 — Core definitions

```markdown
## 4. Core definitions

### Device **(net-new)**

A `Device` is a UbU execution enclave, not merely physical hardware. Examples: phone,
laptop, desktop, home server, browser session, removable sync-bundle importer, personal
GPU worker, temporary recovery environment.

### Zone **(net-new)**

A `Zone` is a user-controlled replication and work-context boundary — not a server.
Examples: personal Zone, household Zone, UbU project Zone, work/client Zone, experimental
Zone.

### Compartment

A `Compartment` is an existing UbU model noun (already referenced in the Phase 0 cutlist
as a deferred Phase 1+ model). In Phase 0 it surfaces only as `compartment_ids:
list[str]` and `redaction_level: str` on `PrivacyAndProvenance`. Phase 2 introduces a
**Compartment policy object** (replication/routing/retention/export) that references
compartments by the same `compartment_ids` convention. The policy object itself is
net-new; the compartment id vocabulary is not.

### SyncStatement **(net-new)**

A `SyncStatement` is a signed or integrity-protected state-change envelope exchangeable
through many transports.

### Replica **(net-new)**

A `Replica` is the local state view held by a Device. It may be full, partial, redacted,
stale, or restricted.

### Admitted state

Admitted state is the deterministic result of applying accepted sync statements, policy
updates, corrections, and conflict-resolution statements according to this contract's
causality and conflict rules. See §9 for the precise determinism claim.

---
```

## JSON Schema

Your output must satisfy this schema:

```json
{
  "type": "object",
  "additionalProperties": false,
  "required": [
    "proposal_id",
    "provider_id",
    "model_name",
    "question_id",
    "base_commit",
    "summary",
    "rationale",
    "changed_files",
    "patch",
    "commit_message",
    "validation_notes",
    "new_questions_added",
    "questions_resolved",
    "decisions_added",
    "requires_human_review"
  ],
  "properties": {
    "proposal_id": {
      "type": "string"
    },
    "provider_id": {
      "type": "string"
    },
    "model_name": {
      "type": "string"
    },
    "question_id": {
      "type": "string",
      "pattern": "^UBU-Q[0-9]{4}$"
    },
    "base_commit": {
      "type": "string"
    },
    "summary": {
      "type": "string"
    },
    "rationale": {
      "type": "string"
    },
    "changed_files": {
      "type": "array",
      "items": {
        "type": "string",
        "enum": [
          "DECISIONS.md",
          "DESIGN.md",
          "OPEN_QUESTIONS.md",
          "PLANNING_KERNEL_CONTRACT.md"
        ]
      }
    },
    "patch": {
      "type": "string"
    },
    "commit_message": {
      "type": "string"
    },
    "validation_notes": {
      "type": "array",
      "items": {
        "type": "string"
      }
    },
    "new_questions_added": {
      "type": "array",
      "items": {
        "type": "string",
        "pattern": "^UBU-Q[0-9]{4}$"
      }
    },
    "questions_resolved": {
      "type": "array",
      "items": {
        "type": "string",
        "pattern": "^UBU-Q[0-9]{4}$"
      }
    },
    "decisions_added": {
      "type": "array",
      "items": {
        "type": "string",
        "pattern": "^UBU-D[0-9]{4}$"
      }
    },
    "requires_human_review": {
      "type": "boolean"
    }
  }
}
```

## Requirements

- Return exactly one JSON object.
- Do not include hidden reasoning, `<think>` tags, markdown fences, or explanatory text outside the JSON object.
- All fields are required.
- Arrays may be empty.
- The patch must be a full git-diff-style patch.
- The patch may modify only:
  - `DESIGN.md`
  - `DECISIONS.md`
  - `OPEN_QUESTIONS.md`
  - `PLANNING_KERNEL_CONTRACT.md`
- Do not modify `README.md`, `OUTREACH.md`, hidden files, scripts, code files, or generated logs.
- Preserve the single-line metadata format in `OPEN_QUESTIONS.md`.
- Allowed values for `Status:` are `Archived`, `Decomposed`, `Deferred`, `Open`, `Solved`, `Superseded`.
- When resolving a question, set its metadata to `Status: Solved` and set `Resolved by:` to the resolving decision id(s).
- Never write `Status: Resolved`; `Resolved by:` is a separate metadata field.
- The `patch` string must be a raw unified diff as produced by `git diff`; do not wrap it in markdown fences or prose.
- Every file diff must start with `diff --git a/<path> b/<path>`, followed by `--- a/<path>` and `+++ b/<path>`.
- Every hunk must include accurate `@@ -old_start,old_count +new_start,new_count @@` ranges and enough unchanged context for `git apply --check` to apply without `--recount`.
- When editing `OPEN_QUESTIONS.md`, anchor hunks with the selected question heading `## UBU-Q0166: Preserve perceptual qualifiers alongside the closed provenance enum` and its own `### Resolution` section. Do not use a repeated `### Resolution` heading from an earlier or later question as the edit location.
- If resolving the selected question, replace the `Unresolved.` text under that selected question's `### Resolution` section and update that same question's metadata line. Do not insert selected-question resolution text into any other question block.
- Use the existing `UBU-Qxxxx` and `UBU-Dxxxx` numbering conventions.
- Prefer minimal, auditable changesets.
- If the selected question is blocked, propose decomposition only if it produces replacement questions with fewer, simpler, or no dependencies.
- If the selected question is already partially resolved, narrow or clarify it rather than pretending it is fully unresolved.

Return only JSON.
