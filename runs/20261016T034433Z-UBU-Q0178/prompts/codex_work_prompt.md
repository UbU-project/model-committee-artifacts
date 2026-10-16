# Model-Committee Work Proposal Request

You are participating in the UbU `model-committee` process.

Your task is to produce one concrete work proposal as strict JSON.

Do not return prose outside the JSON object.

## Selected question

Question ID: `UBU-Q0178`  
Question title: `Economically accessible local compute paths`  
Repository working tree: `/home/sean/ubu-design`

HEAD commit (provenance only): `a805125baedaf9c0cf1c88bd6204f75f29ef6f30`

```markdown
## UBU-Q0178: Economically accessible local compute paths

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-15 Scored from commit: None

### Question

Which lower-cost local compute paths prevent economically precarious users from being excluded from the multimodal experience?

### Current direction

Delta §17.16. Consider smaller local models, bounded snapshots, CPU fallback, reduced latency classes and operator-controlled trusted hardware without a hidden hosted-model dependency. Measure the useful tradeoffs before claiming reach or costs. Compute accessibility is part of the mission, not a promised device minimum.

`UBU-D0332` specifies a comparison of bounded CPU-capable single-Device inference and an existing personal worker, with whole-route cost/effort budgets and the qualification gates in DESIGN.md §1. Actual useful lower-cost profiles and cost envelopes remain unselected. `UBU-D0337` adds explicit ownership scenarios, entry/period cash limits and all-attempt accounting per useful multimodal outcome; the no-worker scenario and any remaining gap must stay visible. `UBU-D0363` adds first-use setup and interrupted package/model acquisition trials, accounting for transferred data, helper resources and peak installation/recovery storage by starting-state/ownership scenario. `UBU-D0369` adds measured secure-upkeep and interrupted-update recovery, including route downtime and affected requalification costs within each ownership/horizon scenario. `UBU-D0376` adds a frozen period demand schedule and endpoint-availability assumptions, coupling budget acceptance to useful multimodal workload delivery.

### Resolution

Open. `UBU-D0332` answers the candidate-path and cost-evidence contract; `UBU-D0337` adds entry/period cash and useful-outcome accounting only. `UBU-D0363` adds setup and acquisition-recovery evidence; `UBU-D0369` adds secure-upkeep and interrupted-update recovery evidence. No measured path is selected. Close after operator-reviewed comparative real-device results select at least one qualified lower-cost camera-and-voice path for explicit ownership scenarios within declared entry, period and total cash, effort, energy and storage budgets, with useful end-to-end outcomes and mandatory security/retention gates met. Keep unqualified ownership scenarios and any no-worker gap explicit. A protocol, smaller model or text-only fallback cannot establish economically accessible multimodal reach.

`UBU-D0376` additionally requires the declared period useful-outcome target to pass under the evaluated availability and competing-use assumptions; a budget pass alone cannot qualify a route.

---

```

## Patch baseline

The patch must apply to the **current working-tree files** in the repository above.
The selected question and context excerpts come from that working tree, which may
include uncommitted ranking and metadata updates.

- Set the JSON `base_commit` field to `a805125baedaf9c0cf1c88bd6204f75f29ef6f30` for provenance. It does not
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
- next decision id: `UBU-D0382`

`DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md` is read-only context and is not in the patch
allowlist: do not propose changes to it.

### DECISIONS.md — cited decisions

#### UBU-D0332

```markdown
## UBU-D0332: Lower-cost multimodal access requires measured local paths and cost envelopes

**Status:** Accepted → DESIGN.md §1. Narrows `UBU-Q0178`; measured lower-cost selection remains Open.

Compare bounded camera-and-voice inference on an existing supported CPU-capable Device with capture/review plus an existing operator-owned worker. Use smaller/quantized or task-specific local models, bounded inputs and only qualified latency classes. Require useful text/form continuity and no hosted inference, paid API credential or recurring model-service subscription. Preserve current Device, Compartment, source-retention, review and planning gates on both routes.

Extend `UBU-D0331`'s real-device qualification with the same useful-workflow fixtures, at least one CPU-only and one worker candidate, and predeclared comparison baselines, workloads and cash/effort/energy/storage budgets. Report whole-route incremental and replacement costs, dated regional prices, measured energy, estimated running costs and operator effort, including unavailable candidates and failed trials. A manual-only fallback or smaller model cannot establish affordable multimodal reach.

Require operator-reviewed evidence and a scoped cost/support matrix before selecting a lower-cost path. `UBU-Q0178` stays Open pending that selection; `UBU-Q0177`, `UBU-Q0168`, `UBU-Q0160` and `UBU-Q0183` retain their existing hardware, numeric-profile, mobile-planning and release-allocation scope. No measured configuration or affordability claim is established here.

---
```

#### UBU-D0337

```markdown
## UBU-D0337: Lower-cost comparisons expose entry cash and cost per useful multimodal outcome

**Status:** Accepted → DESIGN.md §1. Narrows `UBU-Q0178`; measured lower-cost selection remains Open.

Extend `UBU-D0332` with explicit Device-only and Device-plus-existing-worker ownership scenarios, keeping unavailable routes visible and acquisition/replacement assumptions separate. Predeclare entry-cash, cash-per-budget-period and total-horizon ceilings. Report mandatory payments when due, including secure upkeep and necessary replacement; amortization, assumed worker ownership and unconfirmed resale cannot conceal an entry or period budget failure.

Report run-related cash, energy and active operator effort across all attempts and retries, plus each total per useful camera-and-voice outcome under the frozen rubric. Separate successful and successfully corrected counts; manual-only fallback cannot inflate the denominator, and zero useful outcomes yields undefined per-outcome values and no qualified workload. Keep fixed costs in whole-route totals and observed failure/correction burdens in period estimates.

Publish scenario-specific payment schedules, outcome denominators and budget/gate results in the cost/support matrix. Selection requires operator-reviewed evidence with `UBU-D0331` and `UBU-D0336` protections active and applies only to the qualifying ownership/budget scenarios. This adds accounting requirements, not passing trials, prices, population reach or a selected path. `UBU-Q0178` remains Open; existing hardware, numeric-profile, mobile-planning and release-allocation questions retain their scope.

---
```

#### UBU-D0363

```markdown
## UBU-D0363: Lower-cost paths require first-use setup and acquisition-recovery evidence

**Status:** Accepted → DESIGN.md §1. Narrows `UBU-Q0178`; measured lower-cost selection remains Open.

Extend `UBU-D0332`/`UBU-D0337` with one first-use setup trial and one interrupted package/model acquisition followed by recovery for each eligible candidate route on its exact hardware. Freeze the starting inventory, installation path, connectivity/data allowance, required helper resources and finite data, elapsed-time and active-effort limits. Measure through the first useful camera-and-voice outcome under the existing frozen rubric. Keep already-installed and packages/models-absent starting states distinct; a prepared inference trial cannot establish access from absent packages.

Report all transferred bytes and retries, operator/helper effort, energy and peak download/unpacking/installation/recovery storage. Apply existing installation/model-integrity gates before activation. Include helper and connectivity costs in the applicable entry/period totals; show recovery cost separately and disclose its assumed frequency and uncertainty in period estimates. Publish setup-limit results and evidence in the existing cost/support matrix; failed or unknown required setup limits prevent selection for the affected starting-state/ownership scenario.

This adds evidence for the existing setup-cost obligation, not a provisioning or maintenance-automation architecture, passing result, price or selected configuration. Preserve `UBU-D0331`/`UBU-D0336`, useful-outcome accounting and the explicit no-worker gap. `UBU-Q0178` remains Open pending operator-reviewed comparative results.

---
```

#### UBU-D0369

```markdown
## UBU-D0369: Lower-cost paths require measured secure-upkeep feasibility

**Status:** Accepted → DESIGN.md §1. Narrows `UBU-Q0178`; measured lower-cost selection remains Open.

Extend `UBU-D0332`/`UBU-D0337`/`UBU-D0363` with one documented OS or runtime security-update cycle per execution endpoint on each eligible route's exact hardware, plus interrupted update acquisition and supported recovery. Freeze starting/destination versions, security eligibility, instructions, storage, connectivity and helper prerequisites, with finite data, downtime and effort limits. Measure through a useful camera-and-voice outcome after integrity checks and affected `UBU-D0331`/`UBU-D0336`/`UBU-D0368` requalification.

Account for every attempt, payment, transferred byte, helper/operator effort, energy, route downtime and peak installed/staged/recovery storage. Keep upkeep, recovery and requalification separate from setup and run totals while including each cost exactly once in the whole-route payment schedule. Recovery preserves permitted saved state and current permission/retention gates; an ineligible prior build cannot establish successful recovery.

Publish evidence and pass/fail/unknown upkeep-limit results by ownership/horizon scenario in the existing cost/support matrix. Period estimates disclose update/recovery frequency and uncertainty. Failed or unknown required limits prevent selection for that scenario; shorter horizons or budgeted replacement require separate evaluation. These trials establish no future update availability, maintenance automation, passing configuration or selected path. Keep the no-worker gap explicit and `UBU-Q0178` Open pending operator-reviewed comparative evidence.

---
```

#### UBU-D0376

```markdown
## UBU-D0376: Lower-cost selection must meet the declared period workload

**Status:** Accepted → DESIGN.md §1. Narrows `UBU-Q0178`; measured lower-cost selection remains Open.

Extend `UBU-D0332`/`UBU-D0337` with a frozen demand schedule for each ownership/budget scenario: a positive required count of useful camera-and-voice outcomes per budget period, offered-request timing including bursts, finite completion/active-effort limits, and endpoint availability and competing-use windows. Apply the same demand and frozen useful-outcome rubric to candidate routes and the named cost comparator. Owning a worker cannot establish that it is available when needed.

Exercise a predeclared finite demand window covering the claimed bursts and availability boundaries on the exact route with deployed qualification controls active. Reuse existing qualification trials where schedule/load match, preserving their minimum counts and sustained durations. Use `UBU-D0361`'s offered-request/retry accounting and `UBU-D0337`'s stricter multimodal outcome denominator. Report attained useful outcomes, unmet requests, queue wait/backlog, total workflow time and all-attempt costs; refusals, unavailable endpoints and unfinished work cannot disappear from the demand record. Existing freshness, retention and job deadlines still apply.

Publish observed-window results separately from period projections, with availability, competing-use and upkeep-downtime assumptions and uncertainty. Add pass/fail/unknown workload-target results beside the existing budget/gate results. A route qualifies only when both the required useful workload and whole-route budgets pass for that scenario; missing capacity evidence prevents selection. Reduced demand, longer waits or additional hardware require a separately evaluated scenario. Keep the no-worker gap visible and `UBU-Q0178` Open pending operator-reviewed comparative evidence; this selects no measured path or cost envelope.

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

**Observed multimodal resource release (`UBU-D0381`).** Extend `UBU-D0331`/`UBU-D0336` qualification to the actual lifecycle of source-dependent buffers, pending backend submissions and runtime/worker handles after timeout, stop, expiry or revocation. Local invalidation still blocks further reads and result release immediately. A cancelled status, cancellation acknowledgment, process exit or lost worker link alone cannot establish complete-route cleanup or resource release. Bind outstanding work and buffers to the original job generation; prevent their reuse until backend quiescence and required cleanup are demonstrated. Verified read-only weights retain `UBU-D0336`'s separate treatment.

Keep unresolved resources charged to whole-route memory/storage/concurrency limits, including any separately qualified fallback. Withhold further dispatch through the affected runtime instance while release is unconfirmed; immediate controls and useful manual completion must remain schedulable. This containment cannot extend source/intermediate retention or the existing finite cancellation-to-resource-release ceiling. A failed or unobserved required release/retention bound prevents qualification. For a disconnected worker, exercise endpoint-local enforcement and report remote release as unconfirmed until permitted endpoint evidence is available.

Extend the existing distinct-grant fixtures with delayed backend completion, repeated cancellation, teardown failure and late callbacks/writes while a subsequent job is requested. Freeze expected dispositions and observe actual dispatch, buffer ownership and denied reuse, not just generated output. Record invalidation, backend quiescence, cleanup and resource-release offsets separately, with pending-resource high-water marks. Controller injections alone supply no deployed backend-release evidence; demonstrate the supported lifecycle on the exact physical endpoints. Include unresolved work, teardown and any fallback in the existing cold/warm and sustained resource measurements and support matrix. These are runtime-lifecycle observations, not forensic-erasure guarantees or passing hardware evidence; `UBU-Q0177` remains Open.

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
- When editing `OPEN_QUESTIONS.md`, anchor hunks with the selected question heading `## UBU-Q0178: Economically accessible local compute paths` and its own `### Resolution` section. Do not use a repeated `### Resolution` heading from an earlier or later question as the edit location.
- If resolving the selected question, replace the `Unresolved.` text under that selected question's `### Resolution` section and update that same question's metadata line. Do not insert selected-question resolution text into any other question block.
- Use the existing `UBU-Qxxxx` and `UBU-Dxxxx` numbering conventions.
- Prefer minimal, auditable changesets.
- If the selected question is blocked, propose decomposition only if it produces replacement questions with fewer, simpler, or no dependencies.
- If the selected question is already partially resolved, narrow or clarify it rather than pretending it is fully unresolved.

Return only JSON.
