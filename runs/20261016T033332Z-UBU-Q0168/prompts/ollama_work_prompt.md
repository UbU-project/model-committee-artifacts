# Model-Committee Work Proposal Request

You are participating in the UbU `model-committee` process.

Your task is to produce one concrete work proposal as strict JSON.

Do not return prose outside the JSON object.

## Selected question

Question ID: `UBU-Q0168`  
Question title: `Snapshot, conversational, streamed and retrospective latency classes`  
Repository working tree: `/home/sean/ubu-design`

HEAD commit (provenance only): `1d922f99e6a0b46c86141d8eabbfdda8eb5dcf51`

```markdown
## UBU-Q0168: Snapshot, conversational, streamed and retrospective latency classes

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-15 Scored from commit: None

### Question

Which latency classes and degradation behavior are required for snapshots, conversational interaction, streamed supervision and retrospective analysis?

### Current direction

Delta §17.6. `UBU-D0325` defines the four latency classes and deadline, freshness, degradation and interruption semantics in DESIGN.md §21.1.4. Select actual user-visible deadlines, source/context-age limits, stream gap/backlog caps and battery/thermal conditions through revision-pinned local-device evidence. Capture latency remains distinct from planner streaming, admission and proof of execution. No measured performance numbers or supported-hardware claims are established. `UBU-D0335` adds deadline-boundary, stream-invalidation and interruption-race fixtures with separately bounded fallback, output quiescence and resource release; simulated timings do not select numerical profiles. `UBU-D0361` separates calibration from frozen acceptance trials and requires complete request/session outcome accounting with declared timely-useful and stream-coverage criteria.

`UBU-D0367` adds clock qualification for Device sleep/controller suspension and conservative source/worker timing mappings, with controller fixtures and deployment evidence or enforced mode exclusion. Numerical selection still requires the frozen real-device acceptance evidence above.

`UBU-D0374` adds per-scope observation-frontier checks for out-of-order stream results and coverage measured from eligible presentation intervals. Still-fresh older or duplicate updates cannot replace newer guidance or renew gap/coverage accounting; controller fixtures and deployment evidence remain required.

### Resolution

Open. `UBU-D0325` answers the class/degradation architecture; numerical profiles and measured acceptance remain unresolved. Close only after evidence for each enabled Device/backend/mode pins implementation and policy revisions, selects finite deadlines and freshness/resource ceilings, and demonstrates the specified ordinary, sustained-load, offline, interruption and stale-result behavior. `UBU-Q0177` retains secure hardware selection, `UBU-Q0128` retains quantitative restricted-attention profiles and `UBU-Q0183` retains delivery sequence. `UBU-D0335` makes controller boundary enforcement an explicit qualification gate; its fixtures still need passing implementation and real-device evidence. `UBU-D0361` additionally requires frozen per-condition acceptance limits and complete outcome/coverage reports; correct fallback alone cannot qualify ordinary usefulness. A parameterized contract alone is insufficient to close this question.

`UBU-D0367` further requires suspension-aware enforcement and bounded clock-mapping checks; its fixtures and deployment trials remain outstanding. This selects no numerical values or passing profile and does not close the question.

`UBU-D0374` further specifies stream presentation ordering and coverage accounting; its required enforcement fixtures and deployment trials remain outstanding. Numerical selection and measured acceptance remain Open.

---

```

## Patch baseline

The patch must apply to the **current working-tree files** in the repository above.
The selected question and context excerpts come from that working tree, which may
include uncommitted ranking and metadata updates.

- Set the JSON `base_commit` field to `1d922f99e6a0b46c86141d8eabbfdda8eb5dcf51` for provenance. It does not
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
- next decision id: `UBU-D0380`

`DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md` is read-only context and is not in the patch
allowlist: do not propose changes to it.

### DECISIONS.md — cited decisions

#### UBU-D0325

```markdown
## UBU-D0325: Interaction latency classes bound waiting, freshness and degradation

**Status:** Accepted → DESIGN.md §21.1.4. Narrows `UBU-Q0168`; numerical profiles and measured local-device acceptance remain Open.

Adopt `snapshot_capture`, `conversational`, `streamed_supervision` and `retrospective` classes with controller-owned, versioned Device/backend/mode profiles. Separate acknowledgment, first useful result, total-job, context-age and stream-gap/backlog ceilings. Include queue time, enforce finite resource limits and monotonic deadlines, and expose a bounded fallback on expiry. Receipt, durable saving, candidate admission, CPU-certified planning and proof of execution remain distinct.

Recheck original evidence/context/version/policy bindings before current guidance, admission or dispatch. Offline, battery and thermal pressure defer or reduce permitted work and preserve manual routes and validated cached planning. Interruption invalidates pending output/action bindings immediately; late results cannot overwrite corrections, resume capture, use old approval or trigger duplicate/unknown external effects. Privacy, attention, corroboration and hard-validation rules continue to govern every class.

Require DESIGN.md §21.1.4's revision-pinned timings, sustained-load measurements and degradation fixtures before enabling each profile. No numerical latency, supported-hardware, safety-critical supervision or implementation claim is established, and no kernel API or wire-schema change is selected. `UBU-Q0168` remains Open until finite numerical ceilings and their measured behavior are reviewed.

---
```

#### UBU-D0335

```markdown
## UBU-D0335: Latency qualification includes deadline-boundary and invalidation fixtures

**Status:** Accepted → DESIGN.md §21.1.4. Narrows `UBU-Q0168`; numeric profiles and real-device acceptance remain Open.

Require reproducible controller fixtures around elapsed deadlines, stream freshness/gap/backlog limits and interruption races. An outstanding elapsed deadline expires at equality; release checks cannot rely on a timely timer callback. Separate per-window stream deadlines from the finite consented session, and require explicit restart/resume and fresh usable output before resumed live guidance.

Pin separate fallback, output-quiescence and resource-release bounds. Record policy-minimized expected/observed traces and run the actual controller with deterministic clock/adapter fixtures alongside revision-pinned real-device trials. Failed enforcement cannot be averaged away by a fast median or counted as useful completion.

These are qualification requirements, not measured results. `UBU-D0325` retains the four-class contract and `UBU-D0331` the hardware trial protocol. `UBU-Q0168` remains Open until finite numerical profiles and passing local-device evidence are reviewed; no hardware, kernel API or wire-schema change is selected.

---
```

#### UBU-D0361

```markdown
## UBU-D0361: Latency profiles require frozen acceptance trials and complete outcome accounting

**Status:** Accepted → DESIGN.md §21.1.4. Narrows `UBU-Q0168`; numerical profiles and measured acceptance remain Open.

Separate revision-pinned calibration evidence used to propose ceilings from qualification trials of the frozen Device/backend/mode profile. Before acceptance runs, pin the workload, useful-outcome rubric, trial counts and finite limits, including positive minimum timely-useful-outcome and stream-coverage proportions for each applicable ordinary operating condition. Changing limits, workloads or rubrics after observing acceptance results requires a new profile revision and affected qualification runs.

Account for every offered workload request, including refusals, timeouts, failures and retries, without counting fallback speed as useful completion. Distinguish successful interpretation, expected consequential clarification/unresolved output and degradation. Report conditional latency distributions with their denominators and missing outcomes; missing results cannot be assigned a successful latency. Stream evidence includes independent sessions, permitted windows and the fraction of declared active time covered by fresh usable guidance, including startup and coverage gaps.

DESIGN.md §21.1.4 supplies the accounting and pass/fail requirements alongside `UBU-D0331`/`UBU-D0336`'s endpoint/runtime trials and `UBU-D0335`'s enforcement fixtures. Correct bounded fallback alone cannot qualify an ordinary workload that misses its declared usefulness limit. This selects no numerical ceiling, passing implementation or supported hardware; `UBU-Q0168` stays Open pending operator-reviewed evidence.

---
```

#### UBU-D0367

```markdown
## UBU-D0367: Latency clocks account for suspension and bounded mapping uncertainty

**Status:** Accepted → DESIGN.md §21.1.4. Narrows `UBU-Q0168`; numerical profiles and measured acceptance remain Open.

Pin each profile's clock basis, epochs, resolution and sleep/suspension behavior. Suspended time consumes the original elapsed, consent, freshness and stream-gap budgets. Demonstrate continued enforcement or a stop before unsupported conditions; checking on foreground return cannot retrospectively satisfy fallback, output-quiescence or resource-release bounds. Recheck time and generation before exposing queued output/actions or displayed live guidance on return, without rebasing deadlines.

Source/worker clock mappings require finite valid error bounds including drift. Establish release eligibility against the greatest possible elapsed time/age, retain elapsed-expiry equality and the declared evidence-age comparator, and reject unknown, expired or inconsistent mappings for live use. Receipt, transfer and remapping cannot refresh evidence or restart budgets; ordinary permitted historical review remains available.

Require §21.1.4's deterministic controller fixtures and pinned deployment suspension/return and applicable worker-route evidence, or demonstrated mode exclusion. Keep unplanned suspension gaps in the declared stream-coverage denominator. Failed required bounds withhold the affected mode. This selects no clock API, numerical values, passing implementation or supported hardware; `UBU-Q0168` remains Open pending operator-reviewed evidence.

---
```

#### UBU-D0374

```markdown
## UBU-D0374: Stream presentation cannot regress the observation frontier

**Status:** Accepted → DESIGN.md §21.1.4. Narrows `UBU-Q0168`; numerical profiles and measured acceptance remain Open.

Order live stream updates within each guidance scope by their original broker-bound observation windows, preserving source epochs and retry identity. Advance the presented frontier only on actual eligible presentation; a multi-source update must advance at least one required source and regress none. Duplicate or older updates cannot overwrite newer guidance or reset the gap clock even when still fresh. Newer eligible updates may bypass unfinished older work; queueing or completion cannot establish presentation order. All existing freshness, invalidation and explicit-resume checks remain required.

Measure live-guidance coverage as a union of eligible presentation intervals, clipped by age/gap expiry, invalidation and withdrawal. Rejected or duplicate callbacks and repeated wording cannot extend coverage; overlapping eligible intervals count once, and later analysis cannot fill earlier presentation gaps. Keep analyzed observation coverage distinct and retain the declared active-session denominator.

Require DESIGN.md §21.1.4's actual-controller reordered-window, duplicate/retry, failed-newer-window, overlap, multi-source and release-race fixtures plus pinned deployment evidence. These are enforcement and accounting requirements, not passing results, numerical values or a new wire schema. `UBU-Q0168` stays Open pending the existing frozen real-device acceptance trials and operator review.

---
```

### DESIGN.md — referenced sections

#### DESIGN.md §21.1.4 — Latency classes, freshness and interruption

```markdown
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

**Clock qualification (`UBU-D0367`).** Pin the elapsed-clock API, units/resolution, boot/source epochs and sleep/suspension behavior in each profile. Count Device sleep and controller suspension against the original elapsed deadlines, consent lifetime, applicable evidence ages and stream gaps. Demonstrate continued enforcement in permitted operating conditions, or enforce stopping affected work before an unsupported condition. A mode unable to establish elapsed time or meet its fallback, output-quiescence and resource-release bounds remains unavailable for that condition; checking only on return cannot establish those bounds. Before queued output/actions or displayed live guidance become available on return, recheck time and generation. Expired or invalidated work requires an explicit new request or permitted continuation with fresh bindings; returning to the foreground cannot rebase its deadline.

For source-to-controller and inter-Device clock mappings under §21.1.5, pin the epochs, finite maximum timing-error bound including drift, and validity interval. Evaluate applicable elapsed deadlines and evidence-age ceilings against the greatest elapsed time/age consistent with the mapping and clock resolution. Keep elapsed-deadline expiry at equality and the profile's declared evidence-age comparator. Unknown, expired or inconsistent mappings cannot establish live eligibility. Equal timestamps, worker receipt, transfer or remapping cannot replace the original start/observation event or renew budgets. Permitted historical use and later review retain their existing rules.

Extend `UBU-D0335`'s actual-controller fixtures with a paused clock while an independent fixture clock advances past deadlines/gaps, queued callbacks on foreground return, wall-clock jumps, and mapped-age bounds wholly within, touching and crossing the declared ceiling. Demonstrate Device sleep/controller suspension and applicable worker-route timing on the pinned deployment, or the mode's enforced exclusion. Record independent elapsed evidence, clock/mapping revisions and uncertainty bounds with expected/observed outcomes; simulated elapsed time supplies no deployment performance evidence. Suspension cannot postpone the separate fallback/quiescence/release measurements or remove unplanned gaps from declared active stream-time accounting under `UBU-D0361`. Failed required bounds withhold the affected mode; this selects no clock API, numerical profile or passing Device.

**Freshness and authority.** Bind results to their original observation interval, source/session/context and target versions, policy versions and controller job/turn generation. Current guidance/dispatch requires a live generation and applicable age checks; unknown age or uncertainty that cannot prove compliance with the applicable ceiling cannot establish freshness. Review/admission rechecks evidence, context, targets and current policy under ordinary review bindings. Reopening retained material establishes fresh review bindings rather than reviving a cancelled job. New generation timestamps and repeated summaries cannot refresh old evidence. Historical analysis may use permitted older evidence within its declared interval; applying findings to current state still requires current evidence and admission. Stale/superseded results remain labelled historical material only where retention permits, and cannot overwrite corrections, satisfy current preconditions or use expired approvals.

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

**Profile selection and outcome accounting (`UBU-D0361`).** Keep revision-pinned calibration runs used to propose numeric ceilings separate from acceptance evidence. Before acceptance runs, freeze the candidate profile, scenario/source-size/load envelope, useful-outcome rubric, trial counts and pass/fail rules. Include a positive minimum proportion of timely useful outcomes for each ordinary cold, warm and sustained-load condition and, for streams, a positive minimum fraction of declared active time covered by fresh usable guidance, alongside the finite latency, freshness, resource and degradation limits already required. Retain `UBU-D0331`'s minimum trials/durations and `UBU-D0336`'s deployed isolation costs. Acceptance observations cannot justify widening ceilings, lowering usefulness requirements or removing failed cases; changes require a new profile revision and affected qualification runs.

For each predeclared cohort, reconcile all offered workload requests with controller-accepted requests, refusals before dispatch and terminal outcomes. Count an original request once for the timely-useful proportion; retries consume its original elapsed/work budgets and add no extra successes or denominator entries. Keep successful interpretations, corrected results, consequential clarification/explicit unresolved results, expiry, interruption, errors and manual/degraded fallback distinguishable. Clarification or unresolved output counts as useful only when it meets the frozen scenario rubric, and never becomes a successful interpretation. Receipt, progress or a fast unavailable explanation cannot count as useful completion. Fault/interruption cohorts test their predeclared degraded disposition separately from ordinary usefulness; cases cannot move cohorts after their outcomes are known.

Report the timely-useful numerator over all offered requests in each ordinary cohort, plus refusal, miss, error and degraded counts. Latency percentiles name their observed-outcome population and denominator; absent useful output has no successful completion time and stays visible as a missing outcome. Report time to fallback separately, rather than substituting it into useful-result timings. For streams, report independent session counts, all permitted observation windows and dropped/missing/stale updates, startup waiting, longest gap, backlog high-water marks and the proportion of declared active-session time with fresh usable guidance. Include startup and delayed/paused coverage in that time denominator; planned inactive periods are declared before trials. Many correlated windows in one session cannot establish independent-session coverage.

Publish a per-class, per-condition acceptance table linking the frozen profile and trial plan to counts, distributions, coverage, each limit and pass/fail/unknown. An unknown required metric or missed mandatory limit withholds the affected profile; pooled results cannot hide a failed condition. Passing cancellation/fallback fixtures cannot compensate for missed ordinary usefulness, and rapid ordinary results cannot compensate for failed enforcement, privacy or retention gates. Publish calibration results separately and obtain operator review before enablement. This adds an evidence-selection gate, not measured values or a new wire schema; `UBU-Q0168` remains Open.

**Stream presentation order (`UBU-D0374`).** Within each scope of streamed guidance and live generation, the controller binds observation windows before dispatch to the broker's original source epochs, sequence/sample positions and intervals under §21.1.5. For each required source, the presented observation frontier is the latest presented window end in that source's original ordered sample timeline; no frontier is presented initially. Retries preserve window identity; callback arrival, worker completion and model timestamps cannot define observation order. At release, an update must advance at least one required source's frontier and regress none, while satisfying every required source's freshness and ordinary release checks. An identical window cannot advance the frontier through regenerated wording. Different sources/epochs have no shared sequence order; unknown ordering cannot establish live eligibility, and source-epoch changes use the existing invalidation and fresh-generation rules.

Reject duplicate or regressing updates for current guidance even when their elapsed deadlines and age limits still pass. They cannot overwrite newer guidance or reset the update-gap clock. A newer eligible update may bypass unfinished older windows without waiting for an ordered backlog; queueing or completion alone never advances the presented frontier. Before any useful presentation, an older window may still qualify if a newer window failed or remains pending and all original limits and bindings hold. Existing withdrawal, explicit-resume and historical-review rules continue to apply.

Count live-guidance coverage as the union of intervals during which the scoped guidance is available as current and remains fresh and usable, beginning at actual eligible presentation and ending at the earliest applicable age/gap expiry, invalidation or withdrawal. Overlapping updates cannot multiply covered time; rejected or duplicate callbacks cannot extend it, and later analysis cannot backfill time before presentation. Keep `UBU-D0361`'s declared active-session denominator and report analyzed observation coverage separately.

Extend `UBU-D0335`'s actual-controller fixtures with two still-fresh windows A then B whose results return B then A; duplicate/retried B callbacks; a failed or pending B followed by eligible A before the first result; overlapping windows; and multi-source updates that advance one required source while regressing another, including equal timestamps across sources. Assert eligible presentations and frontier changes, rejected live releases, unchanged gap deadlines on rejection and exact covered-time intervals. Include release races with expiry and interruption, then demonstrate the behavior in the pinned deployment trials. Incorrect ordering or inflated coverage fails the affected stream profile; simulated timings select no numerical profile. Record minimized window/frontier aliases and expected/observed outcomes in the existing fixture reports without a new canonical object or wire-schema field.
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
- When editing `OPEN_QUESTIONS.md`, anchor hunks with the selected question heading `## UBU-Q0168: Snapshot, conversational, streamed and retrospective latency classes` and its own `### Resolution` section. Do not use a repeated `### Resolution` heading from an earlier or later question as the edit location.
- If resolving the selected question, replace the `Unresolved.` text under that selected question's `### Resolution` section and update that same question's metadata line. Do not insert selected-question resolution text into any other question block.
- Use the existing `UBU-Qxxxx` and `UBU-Dxxxx` numbering conventions.
- Prefer minimal, auditable changesets.
- If the selected question is blocked, propose decomposition only if it produces replacement questions with fewer, simpler, or no dependencies.
- If the selected question is already partially resolved, narrow or clarify it rather than pretending it is fully unresolved.

Return only JSON.
