# Model-Committee Work Proposal Request

You are participating in the UbU `model-committee` process.

Your task is to produce one concrete work proposal as strict JSON.

Do not return prose outside the JSON object.

## Selected question

Question ID: `UBU-Q0177`  
Question title: `Minimum viable secure local hardware for multimodal interaction`  
Repository working tree: `/home/sean/ubu-design`

HEAD commit (provenance only): `a3b9e57b71e4d3a06aafab3c8cd2e71549507d5d`

```markdown
## UBU-Q0177: Minimum viable secure local hardware for multimodal interaction

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-15 Scored from commit: None

### Question

What measured local hardware profile can deliver the MVP multimodal experience securely and usefully?

### Current direction

Delta §17.15. Evaluate model quality, memory, latency, energy, sustained thermal behavior, endpoint security and local storage on supported devices. Distinguish primary mobile interaction from mobile planning workload (`UBU-Q0160`). No modern-phone universality or cloud-versus-local quality ranking is assumed.

`UBU-D0331` specifies revision-pinned Device-role profiles, predeclared usefulness/resource criteria, real-device cold/warm and sustained trials, endpoint/storage gates and a reviewed support matrix in DESIGN.md §1. This narrows the qualification process without selecting hardware, a mobile planner or a compute API. `UBU-D0336` adds isolation fixtures across inference jobs and measures the enforced isolation/reset costs within that profile.

`UBU-D0362` makes hardware-selection completeness explicit: the approved profile set must cover all mandatory capture/review slices in DESIGN.md §4.2 (`UBU-D0339`), with qualified endpoints, any worker routes and demonstrated shared-hardware workload limits.

`UBU-D0368` extends qualification to every enabled processing stage, including delegated OS services, with connected/offline, backend-loss and reconnection evidence for actual provider calls, egress restrictions and queued retries. The support matrix binds the complete pipeline and its resource/retention costs.

`UBU-D0375` makes qualification an enforced use-time binding: missing or mismatched deployment pins withhold dependent modes and invalidate affected active work. Changed bindings require affected rechecks and renewed operator approval.

### Resolution

Open. `UBU-D0331` answers the qualification protocol only. Close after actual real-device evidence demonstrates the declared MVP slices, numeric latency/resource profiles, endpoint and retention controls, sustained behavior and useful manual degradation, and the operator approves a scoped hardware/software support matrix. No measured minimum or supported Device is established by the protocol. `UBU-D0336` adds runtime-isolation gates; those fixtures also require passing deployment evidence before selection. `UBU-D0362` requires that selection to cover every mandatory §4.2 slice, with each endpoint and any inter-Device route qualified; partial coverage leaves this question Open.

`UBU-D0368` adds complete-route locality evidence; its required real-device trials remain outstanding and establish no supported configuration. `UBU-D0375` adds deployment-binding enforcement and change fixtures; those checks also require passing evidence and select no hardware.

---

```

## Patch baseline

The patch must apply to the **current working-tree files** in the repository above.
The selected question and context excerpts come from that working tree, which may
include uncommitted ranking and metadata updates.

- Set the JSON `base_commit` field to `a3b9e57b71e4d3a06aafab3c8cd2e71549507d5d` for provenance. It does not
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
- next decision id: `UBU-D0381`

`DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md` is read-only context and is not in the patch
allowlist: do not propose changes to it.

### DECISIONS.md — cited decisions

#### UBU-D0331

```markdown
## UBU-D0331: Local multimodal hardware support requires measured Device-profile qualification

**Status:** Accepted → DESIGN.md §1. Narrows `UBU-Q0177`; hardware selection and measured acceptance remain Open.

Adopt a revision-pinned qualification report for each physical hardware/software configuration and execution-enclave Device role. Freeze useful-outcome criteria and finite limits before real-device trials, then measure quality, cold/warm timings, aggregate memory/storage, energy and sustained thermal behavior alongside the existing interaction, retention and interruption gates. Require endpoint isolation, key/storage protection, persistence-path inspection and denial/cleanup fixtures; user ownership, nominal RAM and model size establish no secure supported minimum.

Require DESIGN.md §1's trial counts, sustained sessions, security evidence and fallback demonstrations before operator approval of a scoped support matrix. Missing required evidence or a failed mandatory gate leaves that slice unavailable. Passing grants no Device trust or Compartment authority. Record a tested sufficient profile with limitations, rather than an absolute hardware floor. `UBU-Q0160` retains mobile planning/API selection, `UBU-Q0168` numerical interaction profiles, `UBU-Q0183` release allocation and `UBU-Q0178` lower-cost reach.

These are required measurement and implementation checks, not passing results or a selected hardware configuration. `UBU-Q0177` remains Open until the operator reviews actual evidence and selects at least one complete local camera-and-voice configuration for the declared MVP slices.

---
```

#### UBU-D0336

```markdown
## UBU-D0336: Local multimodal qualification includes isolation across inference jobs

**Status:** Accepted → DESIGN.md §1. Narrows `UBU-Q0177`; secure hardware selection and measured acceptance remain Open.

Extend `UBU-D0331`'s endpoint evidence with paired synthetic camera-and-voice jobs under distinct grants. Test source-dependent context, caches, buffers, handles and scratch across completion, cancellation/revocation, restart and resource pressure, plus proposed concurrency. Require observed runtime access/release boundaries and an authorized positive control; missing a canary in generated prose or a model's refusal cannot establish isolation.

Warm processes and shared read-only weights confer no prior-context authority. Apply §21.1.5's artifact lifecycle and fresh broker checks; an unverified reuse path requires qualification of an isolated per-job runtime or remains unavailable. Measure cold/warm and sustained performance with those protections active and include their time, energy and aggregate resource costs in the support matrix.

These are additional qualification fixtures, not passing tests or a selected configuration. `UBU-Q0177` remains Open for real-device evidence and operator-approved support; existing Device trust, Compartment, retention and endpoint threat-envelope boundaries remain in force.

---
```

#### UBU-D0339

```markdown
## UBU-D0339: MVP capture ships bounded desktop and phone slices before consented local AV streaming

**Status:** Accepted → DESIGN.md §4.2 and §21.1.3; OPEN_QUESTIONS.md `UBU-Q0183`. Resolves delivery scope under `UBU-D0292`; preserves `UBU-D0305`'s switch criteria and `UBU-D0290`'s mobile stewardship profile.

Deliver the Phase 1b-origin MVP-release backlog in this order: the laptop/desktop common manual/review loop with bounded still images and push-to-talk voice; the same bounded capture/review loop on at least one qualified phone, using same-Device inference or an explicitly permitted personal worker; then finite, explicitly started local laptop/desktop streaming in ordinary-attention voice sessions and Task-scoped Discovery. All three slices precede MVP release. The streaming slice includes microphone-only, camera-only and simultaneous ongoing audio/live-camera input, with useful candidates, visible gaps and immediate controls; snapshots alone cannot fulfill it.

Require existing revision-pinned host fidelity/admission, access/usefulness, numerical latency/resource, endpoint/runtime and retention gates for every enabled profile. Implement broker isolation, memory-only raw-media expiry and inspection/discard before exposing streaming. Any retained evidence needs its own authority; optional summary/hash/clip/export controls require their separate gates. Approve at least one bounded path through the existing lower-cost ownership/budget comparison. Actual hardware, numerical profiles and cost selection remain `UBU-Q0177`, `UBU-Q0168` and `UBU-Q0178` work; this decision supplies no passing evidence or supported configuration.

Develop alongside Phase 2 on non-primary test data. Cross-Device routes and candidate exchange use existing trust, Compartment and sync gates, preserving fresh local consent and review; the existing three-Device sync freeze requirement does not require AV on every Device. Release integration/acceptance follows Phase 2 completion and the deferred switch. Missing capture evidence holds the MVP release, without adding a switch condition or reopening Phase 1b. Phone capture allocates no broader mobile planning workload or compute API; `UBU-Q0160` retains any stewardship amendment.

Phone streaming, meeting/multi-party logging, retrospective or cloud-assisted interaction, other sensitive sources and restricted-attention interaction beyond immediate local controls may follow as separately qualified slices. Always-on consent is never inferred, and Phase 3 supplies no capture release gate. Publish exact supported Device/backend/mode combinations, limitations and fallbacks. This resolves scope allocation and sequence only; implementation and operator-reviewed release acceptance remain outstanding.

---
```

#### UBU-D0362

```markdown
## UBU-D0362: MVP hardware selection requires coverage of every mandatory capture slice

**Status:** Accepted → DESIGN.md §1. Narrows `UBU-Q0177`; measured hardware selection remains Open.

Apply `UBU-D0331`/`UBU-D0336` to a support set covering every mandatory §4.2 slice allocated by `UBU-D0339`: bounded laptop/desktop camera-and-voice, bounded phone capture/review with its selected inference route, and finite laptop/desktop realtime audio/video. Map each slice to exact capture, review and inference Device profiles and any inter-Device route, with revision-pinned passing whole-route evidence and operator approval bound to those revisions. Qualify proposed workloads on shared physical hardware or demonstrate enforced mode exclusions.

Operator-approved passing configurations may establish scoped partial support. Keep failed or missing slices and endpoint/route gates visible; close `UBU-Q0177` only after every mandatory slice has an approved evidence-backed configuration. A bounded camera-and-voice path cannot substitute for the realtime slice.

This clarifies completeness within the existing release scope. It supplies no measured result, supported minimum or mobile planner/API selection and changes no switch or release commitment.

---
```

#### UBU-D0368

```markdown
## UBU-D0368: Local multimodal qualification covers the complete processing route

**Status:** Accepted → DESIGN.md §1. Narrows `UBU-Q0177`; measured hardware selection remains Open.

Extend `UBU-D0331`/`UBU-D0336` to every enabled source-processing stage, including OS-delegated speech, vision and speech-output services. Pin each stage's implementation/model, execution endpoint, effective permissions, persistence paths and allowed destinations. Qualify the complete route with deployed restrictions active under connected, Internet-blocked, backend-loss and reconnection conditions; inspect provider/service calls, network attempts and queued retries. Include denied-egress and applicable authorized-worker controls. An offline success, local API label or absent plaintext canary alone cannot prove connected-mode locality.

Publish the pipeline manifest and pass/fail/missing evidence with `UBU-D0362`'s support matrix, including every stage's enforced resource and retention costs. Unknown required paths withhold the affected mode. Separate package/model acquisition and independently authorized sync/export from source processing; they grant no cloud-fallback or source-telemetry authority. This supplies no passing route, supported hardware or mobile planner/API selection. `UBU-Q0177` remains Open for real-device evidence and operator approval of complete mandatory-slice coverage.

---
```

#### UBU-D0375

```markdown
## UBU-D0375: Qualified multimodal modes remain bound to the tested deployment

**Status:** Accepted → DESIGN.md §1. Narrows `UBU-Q0177`; measured hardware selection remains Open.

Enforce `UBU-D0331`/`UBU-D0336`/`UBU-D0368`'s required deployment pins before source acquisition and inference dispatch across the complete route. Missing or mismatched bindings withhold dependent modes. Pin artifacts and enforcement settings through each job or detect changes before further source access/release; recheck qualification bindings at result release and invalidate affected active work under the existing interruption and retention rules. A changed shared component affects every dependent route.

Changed bindings require affected rechecks and renewed operator approval. Preserve permitted saved state, immediate controls and useful manual completion; restored support cannot resume capture or replay work automatically. Require DESIGN.md §1's matching, changed-component, worker, restart and dispatch/release-race fixtures against actual controller access and output behavior. Include enforcement costs in the existing cold/warm and sustained measurements and support matrix.

This makes qualification enforceable during use without selecting hardware, an update mechanism, a mobile planner or a compute API. It supplies no passing implementation or deployment evidence; `UBU-Q0177` remains Open pending complete mandatory-slice evidence and operator approval.

---
```

### DESIGN.md — referenced sections

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

**MVP hardware support coverage (`UBU-D0362`).** Select a set of qualified hardware/software profiles that collectively covers every mandatory capture/review slice in §4.2 (`UBU-D0339`): bounded laptop/desktop camera-and-voice, bounded phone capture/review with its selected inference route, and finite laptop/desktop realtime audio/video. For each required slice, the support matrix identifies the exact capture, review and inference Device profiles, any inter-Device route, enabled sources/classes, declared concurrency and session limits, passing whole-route evidence and operator approval bound to those revisions. Shared physical hardware may cover several slices; qualify the proposed combined workload or enforce and demonstrate the declared mode exclusions under `UBU-D0331`/`UBU-D0336`.

Record pass, fail or missing evidence for each mandatory slice and each required endpoint/route gate. Operator-approved passing configurations may establish support for their qualified slices while incomplete coverage remains visible. Close `UBU-Q0177` only after the operator approves a complete support set with passing evidence for every mandatory slice; a bounded camera-and-voice result alone cannot establish the realtime slice. This clarifies hardware-selection completeness under the existing release scope and supplies no measured result or mobile planner/API selection.

**Complete local processing route (`UBU-D0368`).** For each proposed §4.2 slice, extend the qualification profile with a stage-by-stage manifest of all enabled capture/preprocessing, OCR/vision, speech recognition, embeddings, inference and optional speech-output components, including delegated OS services. Bind each applicable stage to its implementation/model pins, execution Device/profile, effective permissions, input/intermediate copies, persistence paths and allowed destinations. Source-processing traffic stays on qualified endpoints and any selected authenticated encrypted, policy-authorized worker route. Keep package/model acquisition and separately authorized sync/export distinct; none authorizes cloud inference, hidden fallback or source-bearing telemetry.

On the exact deployment with its egress restrictions active, exercise the frozen useful-outcome fixtures with Internet access available and blocked; preserve the selected permitted worker link in the latter condition. Also exercise unavailable local components, queued retries and restored connectivity. Inspect actual provider/service calls and network attempts/destinations for source content and intermediates, including services outside the main model process. Include a controlled denied-egress probe and, when selected, an authorized worker-transfer control to verify observation and enforcement. A successful networking-disabled run, a local API label or absence of a plaintext canary in encrypted traffic cannot establish connected-mode locality. Undeclared remote processing, unauthorized transmission or an unobserved required path withholds the affected mode; backend loss preserves permitted saved input and the existing manual route without remote substitution.

Attach policy-safe manifest and observation evidence with pass/fail/missing stage dispositions to the support matrix. Measure the complete pipeline with its deployed restrictions active under the existing cold/warm and sustained trials, including every enabled stage's aggregate resource and retention costs. Apply affected rechecks when a stage, delegated provider, destination or enforcement configuration changes. These requirements supply no passing deployment or supported hardware; `UBU-Q0177` stays Open pending the existing complete-support-set evidence and operator approval.

**Qualification bindings at use (`UBU-D0375`).** Bind enabled multimodal modes to the operator-approved profile and complete pipeline manifest. Before source acquisition and inference dispatch, compare the effective deployment with its required pins under `UBU-D0331`/`UBU-D0336`/`UBU-D0368` across every execution endpoint, including actual model identity, delegated providers, effective permissions and retention/egress controls. Record how each required binding is verified. Missing or mismatched required bindings withhold every dependent mode; a stable pathname, backend label or past passing result cannot establish a match.

During each job, pin verified artifacts and enforcement settings or detect changes and invalidate affected work before further source access or result release. Recheck the job's qualification bindings at release. On missing or mismatched bindings, apply existing interruption, cancellation and retention rules: stop affected acquisition/output, reject late live results, preserve permitted saved state and keep immediate controls and the manual loop available. A shared component change affects all dependent routes. Changed bindings require affected rechecks and renewed operator approval before use; restored support never resumes capture or replays work automatically.

Require actual-controller fixtures for an approved matching route, model replacement at the same path, changed delegated-provider/permission/egress settings, worker-profile mismatch, restart with stale approval and change between dispatch and result release. Verify observed source access, output suppression and dependent-mode unavailability, including useful manual completion. Repeat the existing cold/warm and sustained measurements with these controls active and publish their costs, failures and evidence in the support matrix. These requirements establish no supported deployment; `UBU-Q0177` remains Open.

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

**First-use setup and acquisition recovery (`UBU-D0363`).** For each eligible route in `UBU-D0332`/`UBU-D0337`'s comparison, add one first-use setup trial and one interrupted package/model acquisition followed by its documented recovery on the exact candidate hardware. Before trials, freeze the starting OS/security state, available storage and package/model inventory, installation path, connectivity/data allowance and any required helper Device or human assistance. Declare finite transferred-byte, first-use elapsed-time and active-effort limits alongside the existing cash, energy and storage budgets. Distinguish already-installed and packages/models-absent starting states; preinstalled trial artifacts cannot establish the latter's entry budget.

Run setup through the first useful camera-and-voice outcome under the frozen rubric. Measure all transferred bytes including failed transfers and retries, operator/helper effort, wall time, energy and peak storage including downloads, unpacking, installed packages/weights and recovery copies. Bind results to package digests and the exercised instructions. Apply `UBU-D0331`'s installation/model-integrity checks before activation; incomplete or unverified acquisitions cannot become usable profiles. Record the interrupted-acquisition recovery separately; a supported full re-download is permissible when its measured costs and limits pass.

Include mandatory helper resources and connectivity payments in the applicable entry/period totals, with recovery cost and its assumed frequency and uncertainty explicit in period estimates. Keep setup costs separate from `UBU-D0337`'s run-related totals and count only rubric-qualified multimodal outcomes. Publish evidence and pass/fail/unknown for each setup limit in the existing cost/support matrix. Failed or unknown required setup limits prevent selection for the affected starting-state/ownership scenario; passing setup alone cannot qualify inference or close `UBU-Q0178`. This specifies evidence collection without selecting a provisioning or maintenance-automation architecture.

**Secure-upkeep feasibility (`UBU-D0369`).** Extend `UBU-D0332`/`UBU-D0337`/`UBU-D0363`'s comparison with one documented OS or runtime security-update cycle for each execution endpoint on each eligible route's exact hardware, plus interrupted update acquisition followed by its supported recovery. Before trials, freeze starting/destination versions, maintained-security eligibility, update instructions, available storage, connectivity/data allowance and required helper resources. Declare finite transferred-byte, route-downtime and active-effort limits within the scenario's period cash, energy and storage budgets. First-use installation alone cannot establish secure-upkeep feasibility.

Measure all attempts through the first useful camera-and-voice outcome after installation/model-integrity checks and affected `UBU-D0331`/`UBU-D0336`/`UBU-D0368` requalification. Include downloaded bytes and retries, payments when due, operator/helper effort, energy, elapsed time with the affected multimodal route unavailable, and peak storage for coexisting installed, staged and recovery artifacts. Separate upkeep, recovery and requalification from first-use and run totals; include each cost exactly once in whole-route schedules and preserve `UBU-D0337`'s useful-outcome denominator. Recovery preserves permitted saved state and rechecks current Device/source/Compartment permissions and retention before resuming; reverting to a build whose maintained-security eligibility has ended cannot count as successful recovery.

Publish evidence and pass/fail/unknown for each required upkeep limit by ownership/horizon scenario in the existing cost/support matrix. Period estimates disclose update/recovery frequency and uncertainty rather than treating the trial as a guarantee of future update availability. Failed or unknown required limits prevent lower-cost selection for the affected scenario; evaluate shorter horizons or budgeted replacement separately instead of pricing unsupported operation as a saving. Preserve the explicit no-worker gap. This adds evidence for secure upkeep and selects no maintenance-automation architecture or measured path; `UBU-Q0178` remains Open pending operator-reviewed comparative results.

**Period workload feasibility (`UBU-D0376`).** Extend `UBU-D0332`/`UBU-D0337`'s comparison by freezing a demand schedule for each ownership/budget scenario before acceptance: a positive required number of rubric-qualified camera-and-voice outcomes per budget period, offered-request count/timing including bursts, finite completion and active-effort limits, and capture/review/inference endpoint availability and competing-use windows. Apply the same demand and frozen rubric to candidate routes and the named lower-cost comparator. Ownership alone cannot establish worker availability.

Exercise a predeclared finite demand window covering the claimed bursts and availability boundaries on the exact route with its deployed qualification controls active. Reuse `UBU-D0331`'s trials where schedule/load match without reducing minimum counts or sustained durations. Apply `UBU-D0361`'s offered-request/retry accounting and `UBU-D0337`'s multimodal denominator: each requested outcome counts at most once, and manual-only fallback contributes none. Record attained useful outcomes, unmet requests, queue wait/backlog, total workflow time and all-attempt costs. Refusals, unavailable endpoints and unfinished work remain in offered demand; waiting cannot extend freshness, raw-media retention or job deadlines.

Publish observed-window outcomes separately from period projections in the existing cost/support matrix. State projection assumptions and uncertainty for demand, endpoint availability, competing use and secure-upkeep downtime; projected completions are not observed outcomes. Record pass/fail/unknown for the required period useful-outcome target alongside every existing budget and mandatory gate. Selection requires both workload and whole-route budget passes for the same scenario; missing capacity evidence prevents selection. Lower demand, longer waits or additional hardware define separately evaluated scenarios. Preserve the explicit no-worker gap. This adds workload feasibility to cost acceptance and establishes no measured configuration, price or population reach; `UBU-Q0178` remains Open.

---
```

#### DESIGN.md §4.2 — Phase 1b: Quick UbU merge through the switch

```markdown
### 4.2 Phase 1b: Quick UbU merge through the switch

Phase 1b extends the Phase 1 system until it can replace Quick UbU, the separately developed personal planner that serves as the dogfooding user's working daily tool, so that Quick UbU becomes legacy software (`UBU-D0275`).

Exit criterion:

> The switch: the dogfooding user's primary daily planning runs on mainline UbU.

Phase 1b closed by decision on 2026-10-10 (`UBU-D0305`), with its planner condition discharged by the greedy Stage 1 certification under the CPU tensor-worker profile (`UBU-D0303`). The ratification gate of `UBU-D0291` was met with no provisional subject root; its registry remains the standing agenda, evaluated at the deferred switch. The switch is not in force and follows the completion of Phase 2.

Until the switch, Quick UbU remains the primary tool and mainline is exercised against non-primary test data. (The switch is deferred until Phase 2 is complete (`UBU-D0305`).)

**The Phase 1b rule.** Every Quick UbU feature merged into mainline adopts the restrictions, forms, and functional boundaries of the MVP design it corresponds to, even when that design's implementation phase is later. Open questions that block Phase 1b are answered during Phase 1b whatever phase they were previously tagged, and the items they pull forward are re-tagged Phase 1b.

Phase 1b includes:

- the Phase 1b foundations already decided, including sync-ready mutation envelopes, the Device registry, tombstones, redacted Handles, projection-conflict repair, Device-local credentials, the local advisory worker, advisory batches, and first-class advisory candidate review;
- the Quick UbU practical features, ported under the Phase 1b rule;
- the planner the switch requires: decomposition into Containers whose ordered child Tasks are grouped into segments that stay together (§9.4); an allowed time range on Tasks; Task value and priority inputs to Plan scoring; routines as evergreen-Objective recurrence that instantiates Tasks (§7.4.1); partial placement when not every Dynamic Task fits the planning horizon; and the desktop GPU backend with its CPU reference path (§16.10).

**MVP-release scope amendment (`UBU-D0292`, sequenced by `UBU-D0339`).** Local camera-and-voice interaction, including explicitly enabled realtime capture, remains in the Phase 1b MVP-release backlog. Phase 1b's closure does not establish release completion or implemented capture. `UBU-D0339` resolves `UBU-Q0183`'s scope allocation below; implementation proceeds alongside Phase 2 without reopening Phase 1b or changing `UBU-D0305`'s switch criteria. `UBU-Q0128` remains Phase 1b-origin work now tracked in Phase 2; Phase 3 does not gate this interaction surface. Mobile planning retains `UBU-D0290`'s stewardship workload and parity rules; any workload/API or stewardship amendment still requires `UBU-Q0160`.

**Capture delivery order (`UBU-D0339`).** Deliver the following slices in order. Rows 1–3 are mandatory before MVP release; exact hardware/software configurations require evidence-backed selection under `UBU-Q0177`. A Device is an execution enclave: qualify each capture, review and inference endpoint and any inter-Device route, even on shared physical hardware.

| Order | Required mode/device slice | Delivery boundary |
|---|---|---|
| 1 | Operator-owned laptop/desktop: `off`/`text_only`, bounded still-image capture and push-to-talk voice, with independently selectable speech output; `snapshot_capture` and `conversational` classes. | Establish §21.1.3's common concern-to-candidate, exact correction, review/admission and next-action loop, plus every ordinary voice and text/form workflow. Camera preview or transcription alone cannot pass. Keep the complete manual loop usable with capture and models disabled. |
| 2 | At least one qualified phone capture/review profile: bounded still images and push-to-talk voice through the same loop. | Inference may run on that phone or a separately qualified operator-owned laptop/desktop worker through an authenticated encrypted, policy-authorized route. Show the chosen route before acquisition/dispatch; worker loss preserves permitted drafts and useful text/forms. This allocates interaction, without selecting a mobile planner or compute API. |
| 3 | At least one qualified laptop/desktop profile: explicitly started, finite local realtime sessions in ordinary-attention `voice_session` and Task-scoped `discovery_mode`, using the `local_only_realtime` route. | Support microphone-only, camera-only and simultaneous ongoing audio/live-camera input; repeated one-shot captures alone cannot satisfy this slice. Demonstrate `conversational` and `streamed_supervision` classes, visible source state and gaps, pause/resume/stop, interruption and reviewable updates. Models propose evidence/candidates; only ordinary review/admission changes state. |

**Evidence and retention precede enablement.** Each slice needs revision-pinned passing §21.1.1 host-mapping, qualifier, corroboration and admission evidence for its enabled adapters (`UBU-Q0166`), §21.1.3 usefulness/access evidence including `UBU-D0338`'s affected user-tested workflows, §21.1.4 numerical and deadline-boundary results (`UBU-Q0168`), and §1's endpoint/runtime qualification (`UBU-Q0177`, `UBU-D0331`, `UBU-D0336`). Unsupported mappings remain unresolved; failed mandatory gates withhold the affected slice. Select at least one bounded camera-and-voice path from rows 1–2 under `UBU-Q0178`'s reviewed ownership/budget comparison (`UBU-D0332`, `UBU-D0337`), including CPU-only and existing-worker candidates and explicit no-worker gaps; manual fallback does not establish multimodal reach.

Before row 3 is exposed, demonstrate §21.1.5's broker isolation, finite aggregate limits, original-time/coverage bindings and bounded cancellation (`UBU-D0326`), plus §12.2's enforced memory-only raw buffer ceiling of 60 seconds, copy/cache/swap controls, expiry/restart cleanup and policy-filtered inspection/discard controls (`UBU-D0327`–`UBU-D0328`). No automatic derivative archive replaces discarded media. Retained minimized evidence/candidates need their own permitted purpose and retention authority. Optional summary/hash/clip/export controls stay unavailable until their separate retention, review and routing gates pass; capture consent never authorizes saving or exporting.

**Coordination with the switch and device wave.** Build and qualify these slices against non-primary test data while Phase 2 develops Device trust, Compartment policy, sync and personal-worker routes. Cross-Device inference waits for its endpoint/route gates; permitted candidate exchange uses the existing Phase 2 contract and carries no live source consent or approval. Exercise phone/desktop switching, corrections, partial replicas, stale policy, revocation and restart without stale admission, duplicate effects or implicit capture resumption. Phase 2's existing three-Device sync freeze evidence remains required independently; its third Device need not implement AV. Release integration and operator-reviewed acceptance follow Phase 2 completion and the deferred switch under `UBU-D0305`/`UBU-D0291`. A capture shortfall blocks MVP-release completion, without adding a switch criterion or reversing Phase 1b closure.

Publish the release support matrix by Device/backend/mode with enabled sources/classes, finite session/resource limits, measured results, user-tested access combinations, retention/security outcomes, approved cost scenarios and useful fallbacks. Phone streaming, meeting/multi-party logging, retrospective analysis, cloud-assisted interaction, other sensitive sources and restricted-attention interaction beyond immediate local controls are subsequent separately qualified slices, not prerequisites for this release minimum. Withhold unavailable processing/review under §21.1.2 while keeping immediate local pause/stop/silence available; always-on capture is outside the commitment. Resource/Skill/Technique-dependent scenarios retain their later phases.

The richer household apprentice, maintenance triage, purchasing defense, Skill evidence and tacit-expertise examples depend on first-class Technique, Resource and Skill implementations. They are phased but unimplemented dependencies: Phase 3 supplies minimal readiness and Technique-based Objective expansion, and Phase 4 supplies richer integration. Evidence semantics, hardware suitability and scenario-specific scope remain unresolved. Camera-first MVP scope does not promise that those later planning objects or scenarios are MVP-ready.

Phase 1b defers multi-device sync and everything else assigned to Phase 2 and later.
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
- When editing `OPEN_QUESTIONS.md`, anchor hunks with the selected question heading `## UBU-Q0177: Minimum viable secure local hardware for multimodal interaction` and its own `### Resolution` section. Do not use a repeated `### Resolution` heading from an earlier or later question as the edit location.
- If resolving the selected question, replace the `Unresolved.` text under that selected question's `### Resolution` section and update that same question's metadata line. Do not insert selected-question resolution text into any other question block.
- Use the existing `UBU-Qxxxx` and `UBU-Dxxxx` numbering conventions.
- Prefer minimal, auditable changesets.
- If the selected question is blocked, propose decomposition only if it produces replacement questions with fewer, simpler, or no dependencies.
- If the selected question is already partially resolved, narrow or clarify it rather than pretending it is fully unresolved.

Return only JSON.
