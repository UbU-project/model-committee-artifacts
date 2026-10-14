# Model-Committee Work Proposal Request

You are participating in the UbU `model-committee` process.

Your task is to produce one concrete work proposal as strict JSON.

Do not return prose outside the JSON object.

## Selected question

Question ID: `UBU-Q0160`  
Question title: `Which mobile compute API should the mobile GPU backend target?`  
Repository working tree: `/home/sean/ubu-design`

HEAD commit (provenance only): `c39ea0ad1c7d177fa161ac42318c663545a057fb`

```markdown
## UBU-Q0160: Which mobile compute API should the mobile GPU backend target?

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0156 Blocks: mobile GPU backend selection, mobile interaction/workload roadmap Resolved by: None Last scored: 2026-10-14 Scored from commit: None

Defining context: DESIGN.md §16.5, §16.10.6; PLANNING_KERNEL_CONTRACT.md §4; `UBU-D0126`, `UBU-D0256`, `UBU-D0283`, `UBU-D0290`.

### Question

Which compute API should implement the in-process mobile GPU backend while preserving CPU certification, provenance and the existing parity contract?

### Subquestions

1. How do Vulkan compute, OpenCL on Adreno, ExecuTorch and NNAPI compare on the actual supported device and driver stack?
2. Which candidate passes exact structural/hard-constraint parity and the named numeric tolerances against the CPU reference path?
3. What latency, memory, energy and sustained thermal behavior does each exhibit for deterministic skeletonization, hard checks, local repair recipes and a short-horizon branch cache?
4. Which APIs and operations are actually available on the selected LineageOS build, and what CPU fallback is required?

5. With `UBU-D0292` making the smartphone camera a primary MVP surface, does `UBU-D0290`'s stewardship execution-profile framing survive, or must mobile's product role and planning workload change? Distinguish where the user interacts from where full search executes: a primary mobile client does not by itself require a full mobile planner.
6. What local perception, voice, battery, privacy and CPU-certification requirements must the camera-first mobile client meet, and how are they sequenced with MVP capture and device synchronization (`UBU-Q0183`)?

### Current direction

The camera-first roadmap implication requires an operator decision. `UBU-D0290` is questioned, not amended; neither a broader mobile workload nor a compute API is selected. Existing parity measurements remain necessary for any selected planning backend, and perception-model suitability needs its own evidence.

No compute API is selected. `UBU-Q0156` settled the desktop invocation boundary; it does not select a mobile backend. This question is blocked on the on-device parity harness producing measurements. The gate is satisfied only once the stewardship workload has been measured against the CPU reference path on physically connected real hardware under the `UBU-D0283` parity rule. Emulator timings and desktop full-search benchmarks do not answer it.

### Resolution

Open pending that measurement gate; no hardware result is inferred from API availability alone.

---

```

## Patch baseline

The patch must apply to the **current working-tree files** in the repository above.
The selected question and context excerpts come from that working tree, which may
include uncommitted ranking and metadata updates.

- Set the JSON `base_commit` field to `c39ea0ad1c7d177fa161ac42318c663545a057fb` for provenance. It does not
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
- next decision id: `UBU-D0358`

`DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md` is read-only context and is not in the patch
allowlist: do not propose changes to it.

### OPEN_QUESTIONS.md — related questions

#### UBU-Q0156

```markdown
## UBU-Q0156: Phase 1b GPU engine invocation boundary

Status: Solved Priority: MVP blocker Phase: Phase 1b Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: Phase 1b GPU engine Resolved by: UBU-D0283 Last scored: 2026-09-21 Scored from commit: None

Defining context: DESIGN.md §16.10, DESIGN.md §16.3, PLANNING_KERNEL_CONTRACT.md §5, `UBU-D0275`, `UBU-D0279`, `UBU-D0281`.

### Question

How does the planning kernel invoke the desktop GPU engine in Phase 1b? The design text and the existing scaffold disagree.

### Subquestions

1. **Invocation.** DESIGN.md §16.10.1 specifies an in-process typed Python function call, not a subprocess or service. The existing scaffold (`ubu-planning-advisory-protocol`, `request_via_process`) spawns `python -m ubu_gpu_advisory.main` and exchanges JSON over stdin and stdout. Which applies? Weigh crash isolation, startup cost, packaging a Python runtime with the desktop app, the interpreter lock, and the security boundary.
2. **Boundary types.** The contract boundary is `PlanningRequest` and `PlanningResponse`, but the scaffold carries `ubu_core::worker::GpuAdvisoryRequest` and `GpuAdvisoryResponse`. Which types cross the boundary? The contract also expresses time windows as RFC 3339 timestamps, while the Rust kernel's `TimeWindow` uses integer coordinates (Unix seconds in the orchestrator). Which representation crosses the boundary?
3. **Framework.** Is PyTorch, as §16.10.1 specifies, confirmed for Phase 1b?
4. **Reproducibility.** GPU floating-point reduction is not bitwise deterministic, while the RNG seed exists so that Plans are reproducible for peer debugging. Is reproducibility required bit for bit, or within a stated tolerance, with CPU certification as the final authority?
5. **Parity testing.** How is the GPU engine tested against the CPU reference path and its goldens? Which stage outputs must match exactly (validity masks, feasibility) and which statistically (rollout probabilities)?
6. **Fallback and provenance.** How is the backend selected on machines without a GPU, and how does a Plan record which backend produced it?
7. **Chunked search.** The chunked search of `UBU-D0279` runs inside whichever invocation is chosen. Does batching every chunk into one call, or dispatching chunks separately, affect the invocation choice or the memory budget of each call?
8. **Streaming.** Interactive requests stream certified chunk results (`UBU-D0281`). A persistent worker process can emit one framed result per completed chunk depth, while an in-process call needs callbacks or an iterator across the language boundary. How does streaming bear on the invocation choice, and what framing carries the partial results?

### Current direction

The `PlanningRequest`/`PlanningResponse` boundary, CPU certification of every selected Plan, and the CPU fallback path are fixed by §16.10 and are not reopened. The Phase 1b planner inputs (segments as placement units, allowed ranges, Task value) enter through the existing `TaskSpec`, so they need no GPU-specific contract. The only pending `TaskSpec` change is the split policy of `UBU-Q0157`. The open decision is how the engine is invoked, which must be settled before implementation because the design and the scaffold contradict each other.

### Resolution

Resolved. See UBU-D0283.

---
```

### DECISIONS.md — cited decisions

#### UBU-D0126

```markdown
## UBU-D0126: Mobile planning is real-time stewardship, not full global optimization

**Status:** Accepted as mobile planning UX direction

A mobile device has the least computational headroom but the highest demand for immediate user-facing adaptation. Mobile UbU therefore should not be designed as the full global planner. Its role is real-time stewardship of Plan legitimacy, user agency, and next-action clarity.

Beyond short-horizon BFS precomputation, compact Calendars should support decision envelopes, protected/flexible/disposable region metadata, last-legitimate-Plan repair, precomputed repair recipes, fast local policy selection, progressive planning feedback, cached explanations, conflict severity levels, next-best-action mode, opportunistic idle/charging computation, optional remote assist without dependency, and uncertainty-aware UI.

The MVP-friendly subset is narrower: Task criticality, last legitimate Plan storage, simple repair rules, conflict severity levels, cached explanations, next-best-action mode, and basic decision envelopes.

---
```

#### UBU-D0256

```markdown
## UBU-D0256: Mobile stewardship metadata uses compact envelopes and bounded local repair

**Status:** Accepted → DESIGN.md §§16.5, 16.9. Resolves `UBU-Q0073`.

Mobile stewardship metadata is part of the Compact Calendar and exists to preserve current/next Task legitimacy on constrained devices without pretending to run full global optimization. The MVP record contains Task criticality, decision envelopes, shared conflict severity, cached explanation fragments, last-legitimate-Plan identity, simple repair recipe refs, next-best-action mode, recalculation triggers, and remote-assist eligibility. Learned local policy models and rich branch packaging remain post-MVP.

Task criticality has exactly three levels: `protected`, `flexible`, and `disposable`. Protected Tasks include Static Tasks, hard dependencies/preconditions, user commitments, legitimization support, recovery-critical work, and deadline-fragile work; mobile must not silently drop, shorten below minimum duration, or move them outside envelope. Flexible Tasks may be moved, split, or shortened inside envelope when constraints remain satisfied. Disposable Tasks may be omitted or replaced when doing so does not break downstream legitimacy.

Decision envelopes record the local movement boundary: earliest start, latest start, latest finish, movable windows, minimum and target duration, dependency/precondition refs, affected Static Task refs, affect/resource/location assumptions, repair budget, and staleness deadline. Local repairs are legitimate only while accepted Logs, Snapshots, External Events, elapsed time, and user actions keep the current/next Task inside those envelopes and do not invalidate dependencies, preconditions, affect assumptions, hard constraints, or coverage.

Conflict severity uses one Phase 1b vocabulary for mobile stewardship and projection conflicts: `advisory`, `minor`, `major`, and `blocking`. Advisory conflicts can be recorded as stale/degraded metadata; minor conflicts may be silently repaired inside envelopes; major conflicts require user choice or confirmation while preserving next-action explanation; blocking conflicts mean mobile cannot identify a legitimate current or next Task without clarification or higher-fidelity planning.

Cached explanation fragments are short, non-blaming strings with lineage refs for `why_now`, `matters_because`, `depends_on`, `supports`, `legitimacy_reason`, `if_skipped`, `repair_summary`, and `staleness_summary`. They should carry enough provenance to explain the current recommendation, repair, or clarification without copying private detail into more-disclosive replicas.

MVP repair recipes cover `late_task`, `skipped_task`, `fatigue_report`, `approaching_static_task`, and `missing_prerequisite`. They may condition on accepted local evidence and adjust only flexible/disposable work inside recorded envelopes, preserve Static Tasks and protected support, use next-best-action mode when no safe default exists, and produce diagnostics or clarification Tasks when required starting state is missing. They must not invent canonical state, Resources, Skills, Techniques, external facts, or user commitments.

The last legitimate Plan is stored by digest and replay cursors: Plan id/digest, generated/effective time, horizon, execution profile, legitimacy and hard-validation refs, default segment digest, UniverseState digest, Log/Snapshot/External Event cursors, Compact Calendar digest, protected Task refs, decision-envelope digest, and explanation-lineage digest. Mobile asks the user for protected changes, meaningful omissions, missing state, or major/blocking conflicts; silently repairs only explicit minor cases; and waits for desktop/cloud refinement when the issue is advisory, outside the reactive horizon, low-coverage but not current-action-critical, or requires global optimization.

---
```

#### UBU-D0275

```markdown
## UBU-D0275: Phase 1b runs through the switch, the planner precedes sync, and later phases are renumbered

**Status:** Accepted → DESIGN.md §4, §4.2, §4.7

Phase 1b is the phase in which mainline UbU reproduces the practical features of Quick UbU, the dogfooding user's working personal planner, so that Quick UbU can become legacy software. Its exit criterion is the switch: the dogfooding user's primary daily planning runs on mainline UbU. Until the switch, Quick UbU remains the primary tool and mainline is exercised against non-primary test data.

Live Phase 1b rehearsals established that the greedy baseline already meets the earlier practical switch bar. The desktop GPU backend remains in Phase 1b because the operator raised the planning expectation on 2026-10-05: automated results requiring minimal human-labor ordering, rather than the ordering work of the previous personal planner. This corrects the justification, not the scope. The switch has no scheduled date and waits on that planner. In addition to the feature ports, Phase 1b therefore includes decomposition into Containers (§9.4) whose ordered child Tasks are grouped into segments that stay together; an allowed time range on Tasks; Task value and priority inputs to Plan scoring; routines as evergreen-Objective recurrence that instantiates Tasks; partial placement when not every Dynamic Task fits the planning horizon; and the desktop GPU backend of §16.10, which was already the Phase 1 performance target and is delivered in Phase 1b together with its CPU reference path. Calendar-style recurrence (§7.4.1) moves to Phase 1b with this decision. The Phase 1b slice of recurrence and Task synthesis is carved out of `UBU-Q0125` when the Phase 1b planner questions are recorded.

The planner precedes multi-device sync, which remains Phase 2. Neither constrains the other. The planning engine is a pure function over `PlanningRequest` and `PlanningResponse` (§16.10.1) and never sees replicas, Devices, mutation envelopes, or Compartments; sync carries canonical state and the Plan artifacts already specified for Phase 1. Sync is not needed for the switch, because the dogfooding configuration is one desktop Device with Google Calendar as a projection surface, which is not a Device (`UBU-D0257`). Building the planner first shortens the path to the switch, starts full dogfooding of mainline sooner, and returns time to the user through more efficient daily planning.

Later phases are renumbered to remove lettered subdivisions:

| Label before `UBU-D0275` | Label after |
|---|---|
| Phase 3A | Phase 3 |
| Phase 3 | Phase 3 |
| Phase 3B | Phase 4 |
| Phase 4+ | Phase 5+ |

Phase 0, Phase 1, Phase 1b, Phase 2, Phase 3+, and Post-MVP keep their labels and meanings. Earlier tickets, commits, and external documents that use the old labels are read through this map.

A phase label names the phase that implements an item. Pulling an individual item forward, as the Phase 1b rule does, re-tags that item to the implementing phase and never renumbers phases. Build order follows the phase table in §4.7.

Consequences:

- The switch waits on the desktop GPU planner and CPU reference/certification path, and on ratification or retirement of every provisional subject root before the store becomes non-disposable (`UBU-D0291`). No switch date is set.

- `DESIGN.md` §4 gains a Phase 1b section (§4.2) and a phase map (§4.7); Phase 2 and Phase 3 move to §4.3 and §4.4, and Phase 4 and Phase 5+ receive §4.5 and §4.6.
- The `model-committee` phase vocabulary adds `Phase 4` and `Phase 5` so that Open Questions can carry the new labels.
- Phase 2 sync scope, the Device Sync and Compartment contract, and all Phase 1 and Phase 1b decisions are unchanged.

---
```

#### UBU-D0279

```markdown
## UBU-D0279: Phase 1b planning search is chunked at capacity-occupying Static Tasks

**Status:** Accepted → DESIGN.md §16.3, §16.10.4

Capacity-occupying Static Tasks, together with preserved or frozen placements, partition the planning horizon into chunks: the free intervals before, between, and after those fixed placements. Non-capacity Static Tasks do not bound chunks. Placement units are not preempted, so every atomic unit, whether a Dynamic Task or a compiled decomposition segment (`UBU-D0278`), lies entirely inside one chunk.

Candidate search has two levels. The first level assigns placement units to chunks. This is the coupled part of the problem:

- each unit occupies exactly one chunk;
- allowed time ranges (`UBU-D0276`) restrict which chunks a unit may use;
- dependencies and effects flow forward in time;
- affect and UniverseState evolve from chunk to chunk.

The second level orders the units assigned to each chunk. Given an assignment, chunks are independent, so their orderings are evaluated in parallel. Equivalently, the search sweeps the chunks breadth-first in time order. Each branch carries its unplaced units and its carried state, and branches are pruned at chunk boundaries, instead of being generated for the whole horizon and rejected late.

Fixed placements bound delay propagation, so Monte Carlo rollouts evaluate chunks separately. Each rollout draws the latent correlation-group factors once and shares them across all chunks, and it is feasible only if every chunk is feasible under those shared draws.

The CPU reference path and the GPU engine share these search semantics; the GPU engine adds breadth, not different meaning. The `PlanningRequest`/`PlanningResponse` contract, the stage boundaries of `PLANNING_KERNEL_CONTRACT.md` §5, and CPU certification of the selected Plan are unchanged. A chunk axis in tensor layouts is an implementation choice within the recommended tensor profile.

Chunks also scope runtime repair. The current chunk, up to the next fixed placement, is the unit of local repair, consistent with the §16.5 repair recipes, and compact Calendar packaging may carry alternatives per chunk.

A horizon with few fixed placements yields large chunks, and for those the within-chunk problem is the whole problem. Long availability blocks, such as working hours, are expressed as allowed ranges on the Tasks they admit, not as capacity-occupying Static Tasks, which would leave those Tasks nowhere to go. A unit longer than every chunk it may occupy cannot be placed. The remedies are decomposition split points, splittable Tasks (`UBU-Q0157`), and partial placement (`UBU-Q0155`).

Consequences:

- `DESIGN.md` §16.3 records chunked search, and §16.10.4 applies it to the GPU pipeline stages.
- `OPEN_QUESTIONS.md` updates `UBU-Q0154`, `UBU-Q0155`, and `UBU-Q0156` for chunked search and records splittable Tasks as `UBU-Q0157`.

---
```

#### UBU-D0281

```markdown
## UBU-D0281: Interactive planning streams certified chunk results; batch planning does not

**Status:** Accepted → DESIGN.md §16.3, §16.10.2

A planning request declares either interactive or batch delivery. Interactive delivery serves a user who wants to act now, for example after an interruption, without waiting for a long horizon to finish. The engine emits the results for each chunk depth as that depth completes, and the CPU certifies each chunk result on arrival; certifying chunks separately is sound because chunks are independent given their boundary state (`UBU-D0279`). The first chunk's plan therefore reaches the user almost immediately while later chunks are still being searched. A streamed chunk plan stays provisional until the user starts it (`UBU-D0280`) and is fixed afterwards. The final response completes the stream.

Batch delivery, such as an overnight run over a long horizon, returns only the final response and does not stream.

Streaming changes how an interactive response is delivered. It therefore amends `UBU-D0279`'s statement that the `PlanningRequest`/`PlanningResponse` contract is unchanged: the request gains a delivery mode, and the response gains a streamed partial-result form for interactive delivery. How that stream is framed across the engine boundary is settled together with the invocation boundary in `UBU-Q0156`.

Consequences:

- `DESIGN.md` §16.3 and §16.10.2 record interactive and batch delivery.
- `OPEN_QUESTIONS.md` adds a streaming subquestion to `UBU-Q0156` and records chunk-boundary outcome coverage as `UBU-Q0158`.

---
```

#### UBU-D0283

```markdown
## UBU-D0283: Phase 1b GPU engine is a persistent local Python worker over framed PlanningKernel messages

**Status:** Accepted → DESIGN.md §16.10, PLANNING_KERNEL_CONTRACT.md §1, §2, §4, §5. Resolves `UBU-Q0156`.

Phase 1b invokes the desktop GPU engine through a persistent local Python worker process, not through an in-process Rust-to-Python call and not through a network service. The CPU kernel starts and owns the worker as a child process for the duration of a planning session or bounded worker lifetime. The worker communicates over length-prefixed JSON frames on local pipes. Startup cost is amortized by worker reuse; crash isolation is explicit because a Python, CUDA, or PyTorch fault can terminate the child without corrupting the CPU kernel; and the interpreter lock remains inside the worker instead of entering the Rust desktop process. The worker has no listening socket and receives only the already-minimized planning payload, so it is an invocation boundary, not a new trust or sync boundary.

The semantic contract remains a pure planning function over `PlanningRequest` and `PlanningResponse`. Except for the explicitly approved internal `stage1-atomic-v1` profile below, `ubu_core::worker::GpuAdvisoryRequest` and `GpuAdvisoryResponse` may exist only as implementation envelopes for spawning, framing, cancellation, error reporting, and telemetry; they must carry exactly one `PlanningRequest`, `PlanningResponse`, or stream frame and must not become a second planning schema. Time values crossing the semantic boundary use the contract's RFC 3339 UTC timestamps. Rust may store Unix-second coordinates internally, and the Python/PyTorch implementation may lower timestamps to integer offsets or seconds inside tensors, but that lowering is implementation-local and is replayable from the request.

**Approved atomic Stage 1 exception.** `stage1-atomic-v1` is an internal stage handoff, carrying a minimized request plus CPU-derived topological order, Task/slot masks and a tagged sampling source, and returning padded structural arrays for CPU interpretation and certification. It is the bounded exception to the complete-object envelope restriction, not another public planning schema. The canonical `PlanningStreamFrame` is unchanged. CPU-owned request construction, backend selection, pure planning semantics, exact structural/hard-constraint parity and final CPU certification remain authoritative. This profile neither authorizes worker admission nor implements canonical chunk streaming or the full planning contract. The existing legacy invocation can transport a completed CPU answer; reaching a strategy/framework gate does not establish a completed tensor exchange or GPU planning parity.

PyTorch is confirmed for the Phase 1b GPU backend. The CPU reference path remains the built-in authoritative certification path and the no-GPU fallback. Backend selection is CPU-owned: use the GPU worker only when policy allows it, a compatible local Python/PyTorch/CUDA environment is available, and the compute budget justifies it; otherwise use the CPU reference path. Every returned response and committed Plan records backend provenance, including backend kind, invocation kind, engine version, framework, device summary when available, request id, RNG seed, and CPU certification status.

The RNG seed is required for replay and peer debugging, but GPU execution is not required to be bitwise deterministic across hardware, drivers, PyTorch versions, or reduction orders. Exact reproducibility is required for request construction, CPU reference certification, structural validity results, hard-constraint feasibility, masks, dependency ordering, and selected Plan certification. Floating-point scores, rollout frequencies, and probability intervals may match within a named tolerance profile. When GPU and CPU advisory scores disagree beyond tolerance, CPU certification and CPU reference goldens are the final authority.

Parity tests compare the GPU worker against the CPU reference path at each stage. Schema decoding, chunk partitioning, task-slot masks, dependency feasibility, hard-constraint feasibility, rejection classes, and CPU-certified selected Plan validity must match exactly. Floating-point stage scores, rollout probability estimates, probability intervals, and schedule-diversity scores are compared by documented absolute/relative tolerances or statistical acceptance tests tied to the rollout count and seed. Goldens include CPU-only fixtures so contributors and CI systems without a GPU can run the certification suite.

Chunked search stays inside one planning invocation for a whole request. The worker may batch chunks internally to fit memory, but the CPU does not dispatch each chunk as a separate semantic request. Keeping chunks inside one invocation preserves shared latent rollout draws, boundary-state propagation, and coherent streaming. The per-call memory budget is therefore the maximum active frontier and chunk batch chosen by the engine under `compute_budget`, not the sum of all chunks materialized at once.

Interactive delivery uses the same worker framing. A stream consists of zero or more `PlanningStreamFrame` objects with `frame_type = chunk_result`, each carrying one CPU-certifiable chunk-depth partial response, followed by exactly one `frame_type = final_response` carrying the final `PlanningResponse`. Batch delivery emits only the final response. Error and cancellation frames are transport outcomes; they do not certify a Plan.

Consequences:

- `DESIGN.md` §16.10 replaces the in-process typed-call requirement with a persistent local Python worker boundary and records fallback, provenance, reproducibility, chunking, and streaming consequences.
- `PLANNING_KERNEL_CONTRACT.md` records delivery mode, engine provenance, worker frame semantics, and parity expectations.
- `OPEN_QUESTIONS.md` marks `UBU-Q0156` solved.

---
```

#### UBU-D0290

```markdown
## UBU-D0290: Mobile is a stewardship execution profile with its own backend kind, validated by on-device parity

**Status:** Accepted → DESIGN.md §16.10.6; PLANNING_KERNEL_CONTRACT.md §4; OPEN_QUESTIONS.md `UBU-Q0160`. Refines `UBU-D0126`, `UBU-D0256` and `UBU-D0283`.

`UBU-D0283`'s persistent Python/PyTorch worker over local pipes is a desktop invocation mechanism, not a mobile backend definition. Mobile GPU execution is a new backend kind, not a port of that worker. What carries over is the pure `PlanningRequest` → `PlanningResponse` function, CPU-owned backend selection, CPU certification as final authority, and recorded provenance on responses and Plans. The authority and admission boundaries of `UBU-D0254` and `UBU-D0255` continue to apply.

A mobile backend runs in process on the device: provenance gains `backend_kind = mobile_cpu | mobile_gpu` and `invocation_kind = in_process_mobile`. `framework` is a free string naming the implementation's framework or compute API; the `pytorch` requirement applies only to the Phase 1b desktop GPU worker. No Python runtime is shipped on device.

The evaluated mobile workload is the stewardship set from `UBU-D0126` and DESIGN.md §16.5: deterministic skeletonization, exact hard-constraint checks, local repair recipes and a short-horizon branch cache. It is explicitly not the desktop's full chunked search. Mobile compute API selection remains undecided in `UBU-Q0160`, gated on on-device parity measurements against the CPU reference path rather than a preferred library.

`UBU-D0283`'s parity requirements apply unchanged: exact agreement on schema decoding, chunk partitioning, task-slot masks, dependency feasibility, hard-constraint feasibility, rejection classes and CPU-certified Plan validity; named absolute/relative tolerances or statistical acceptance criteria for floating-point scores and rollout estimates. Applicable stages are exercised by shared parity fixtures even though the evaluated workload is stewardship, not global search. A mobile-produced Plan is CPU-certified on the device before becoming user-visible, just as a desktop GPU candidate is certified before presentation.

Validation uses a physically connected device over `adb`, not an emulator. An AVD executes graphics through host hardware or software rendering; it cannot supply the target phone's driver, thermal, timing or vendor-extension evidence. A rooted LineageOS test device supplies the actual stack and local diagnostics without routing test data through a vendor telemetry service. Build capabilities and recovery paths must be checked by the device gate; root alone does not imply every diagnostic or reboot transport is available.

Consequences:

- `DESIGN.md` §16.10 gains a mobile execution-profile subsection while cloud and the full mobile planner remain deferred.
- `PLANNING_KERNEL_CONTRACT.md` §4 gains the provenance enum members and scopes framework identity to the backend.
- `OPEN_QUESTIONS.md` gains `UBU-Q0160`, dependent on `UBU-Q0156` and blocked on the real-device parity harness.
- `docs/MOBILE_DEVICE_TESTING.md` defines the pre-flight and the rule that no procedure may leave the phone needing a cable to recover.

---

---
```

#### UBU-D0292

```markdown
## UBU-D0292: Camera-and-voice perception is the primary MVP interaction surface

**Status:** Accepted → DESIGN.md §2.13, §4.2, §12.2, §21; MULTIMODAL_INTERACTION.md. Amends `UBU-D0132` and supersedes `UBU-Q0128`'s Phase 3 release gate.

Multimodal perception becomes the primary interaction surface without becoming an authoritative planner. The camera proposes candidate state; the kernel still plans; admission is still the operator's.

Local camera-and-voice interaction is part of the MVP release, rather than an optional post-MVP input feature. This extends release scope without retrospectively changing the frozen Phase 1 dogfooding implementation. The work belongs in the Phase 1b MVP-release backlog; `UBU-Q0183` asks for its exact delivery sequence, capture-mode coverage and relationship to the switch and device waves. A Phase 3 question cannot gate this release commitment. `UBU-Q0128` now carries Phase 1b and answers the architecture while retaining unresolved restricted-attention details and naming.

The camera is a perception layer: show the world, resolve candidate objects and facts against permitted persistent context, ask for consequential ambiguity, and admit only validated operator-approved updates. Voice is ordinary interaction; text and forms remain available for preference, accessibility, privacy, precision and context. Primary does not mean mandatory capture or always-on consent. Raw images in this primary path are processed on the operator's own hardware, without upload to a hosted model. Any later export is a separate policy-governed operator choice.

The observation/admission mechanism of `UBU-D0132` is preserved. Backend choice remains replaceable; neither confidence, conversational fluency nor observed elapsed time confers planning authority. Continuous AV refines the existing explicit-mode, Compartment-review, routing-disclosure and approval boundary in §12.2. `UBU-D0290` is unchanged: camera-first mobile interaction's roadmap implication is a question in `UBU-Q0160`, not an implied choice of mobile planner or compute API. Rich Resource/Skill/Technique examples retain their existing later-phase dependencies.

---
```

### DESIGN.md — referenced sections

#### DESIGN.md §2.13 — Realtime and agentic AI boundary

```markdown
### 2.13 Realtime and agentic AI boundary

Local camera-and-voice perception is the primary interaction surface and part of the MVP release (`UBU-D0292`, amending `UBU-D0132`). Realtime multimodal models remain replaceable perceptual/extraction backends: they may listen, speak, watch, transcribe, translate, detect interruptions, monitor Task progress, assist meeting capture, or generate candidate UI, but they do not become UbU's authoritative planner. Text and forms remain available, and primary interaction never implies continuous-capture consent.

UbU distinguishes **model-time awareness** from **planner-time semantics**. A model may notice elapsed time, silence, overlapping speech, or changed conditions; UbU decides whether those observations update a Task, Log, Snapshot, Calendar, Objective, or recalculation trigger.

Realtime/discovery functionality must be mode-bound and visible. The user should know whether UbU is off, text-only, in a voice session, in active Discovery mode, in meeting/logging mode, local-only, or cloud-assisted.
```

#### DESIGN.md §4 — Release Phases

```markdown
## 4. Release Phases

Phase labels name the phase that implements an item. The phases, their build order, and the mapping from labels used before `UBU-D0275` are recorded in §4.7.

### 4.1 Phase 1: Single-user GitHub dogfooding

A single user uses UbU to coordinate development of UbU itself.

Primary goals:

- import or observe GitHub issues, PRs, reviews, CI events, and milestones;
- represent them in UbU’s Objective/Task/Calendar model;
- generate useful Plans;
- provide a minimal bootstrap interview that creates an initial dogfooding context, current or stale affect Snapshot, and prioritized Objective/Task seed set;
- provide a next-action focus mode that shows one recommended next Task with an inspectable explanation of why it matters now;
- update GitHub as a low-dimensional projection of UbU state;
- expose limitations and open questions through dogfooding.

The frozen Phase 1 implementation set is:

- local single-user `user_mode` instance for UbU-runs-UbU;
- bootstrap interview and explicit seed model creation;
- GitHub issue, PR, review, CI, milestone, and comment import from live data or approved fixtures;
- ExternalReference-style source links sufficient to trace imported GitHub objects;
- Objective, Preference, Task, Container, UniverseState, Snapshot, Plan, Calendar, Log, Identity, Relationship, Compartment, Automation Worker, External Event, and External Reference schemas only to the depth required by the dogfooding loop;
- schedulable Static and Dynamic Tasks with Objective links, durations, dependency/precondition/effect fields, active/completed/failed/moot lifecycle handling, and moot reason codes sufficient for the demo;
- lightweight UniverseState facts, affect Snapshot handling, accepted mutation vocabulary, and deterministic precondition evaluation;
- append-only per-instance Logs with correction, annotation, provenance, worker-submission, and recalculation-trigger entries;
- Plan and Calendar generation for a next work window with a default Plan and inspectable explanation;
- next-action focus mode with start, done, snooze, reject, decompose, explain-more, and feedback controls;
- derived risk and human-complete plan-quality reports covering deadline risk, dependency fragility, worker or automation bottlenecks, stale affect, affect-margin, destructive pressure, and post-plan depletion warnings;
- minimal Compartment guardrails for `local_only`, `no_cloud_llm`, `no_external_export`, allowed integration/device references, low-security labeling, and logged boundary decisions;
- Automation Worker identity, scoped capability grants, explicit assignment/status display, and mutation or projection request submission without direct canonical writes;
- Delegation Substrate-compatible Task formalization fields where needed for dogfooding, worker assignment, or solo self-reminder clarity, including purpose, executor type, authority, expected output, evidence, review, privacy scope, and escalation notes;
- clearly marked GitHub projection previews or human-approved writes for UbU-managed labels, comments, or blocks, plus reconciliation reporting;
- manually structured Release Outreach Pipeline work items and artifact records for release notes, screenshots, scripts, and contributor calls-to-action;
- manually structured EthConf outreach notes and follow-up artifacts that dogfood Association formation and organizational introspection without requiring full multi-user Association automation.

Phase 2 explicitly defers:

- multi-device local-first sync;
- partial replication across Devices, Zones, or Compartments;
- secure cross-device Compartment propagation;
- sync conflict handling;
- cross-device worker or enclave coordination beyond the single local instance boundary.

Phase 3 is the bridge from the bootstrap MVP toward the full personal life-logistics product. It prioritizes minimal Resource/Skill-aware task readiness, private skill-tree foundations, and narrow Identity coordination that directly improves single-user life logistics. Phase 4 (§4.5) begins the full version 1.0 release track, and Phase 5+ (§4.6) continues it.

Phase 3 explicitly includes:

- minimal Resource objects and Task resource requirements sufficient to answer whether a Task is realistically ready to start or complete;
- minimal Skill objects and Task skill requirements sufficient to answer whether the user can perform a Task, needs a refresher, should learn a prerequisite, or should consider purchase/delegation;
- minimal cross-user contextual messaging between UbU instances;
- user-to-user Identity-mediated requests, status updates, questions, commitments, and blockers;
- bounded Message Context Envelopes carrying priority, interrupt recommendation, topic, response expectation, assumptions, ambiguities, provenance, and disclosure policy;
- legacy-message ingestion and limited upgrade paths where both parties use UbU;
- user-to-user Identity commitments, capabilities, and limited disclosure sufficient for narrow coordination;
- early Delegation Substrate use between Identities, if Phase 2/3 authority and privacy boundaries are in place.

Phase 3 still defers:

- full inventory management, full financial management, bank syncing, investment tracking, receipt OCR, tax categorization, and Quicken-like accounting workflows;
- public Skill Barter marketplace operation, payment/settlement flows, reputation markets, dispute resolution, and marketplace governance;
- shared global project truth between users;
- rich subjective Relationship-model exchange;
- full Association synchronization;
- multi-party governance, invitation, revocation, or trust protocols;
- broad personal-data ingestion beyond selected communication integrations.

Phase 1 keeps these abstractions documented for compatibility but does not implement them:

- Technique as a first-class planning object (minimal Technique-based Objective expansion in Phase 3, richer integration in Phase 4);
- Resource and Skill as first-class planning objects beyond lightweight compatibility placeholders (minimal readiness in Phase 3; richer integration in Phase 4);
- full Compact Calendar planner grammar and high-coverage transport format;
- complete Zone and Device system beyond the current local execution enclave;
- organization-mode and worker-mode web admin consoles;
- richer relationship-management, personal CRM, and longitudinal affect/growth models;
- full Release Outreach Pipeline video generation, rendering, and publication workflow;
- broad email, text-message, file, invoice, note, or personal-data ingestion outside narrow approved dogfooding fixtures;
- full realtime multimodal capture (Phase 1b MVP-release scope under `UBU-D0292`, with mandatory mode/device slices and delivery order set by `UBU-D0339` in §4.2); always-on assistant behavior remains outside this release commitment;
- public Skill Barter marketplace operation, payment/settlement flows, reputation markets, dispute resolution, and General Contractor subdelegation workflows;
- adaptive model-committee weighting, automatic patch application, GitHub mutation, and direct cloud-provider APIs.

Stop rule:

> A new abstraction may be added to Phase 1 only when it is required to complete the single-user GitHub dogfooding loop, enforce an accepted hard invariant, or avoid a known irreversible schema contradiction. Otherwise it must be documented as a Phase 2, Phase 3, or post-MVP concern and must not block Phase 1 implementation.

After `UBU-D0175`, new MVP blockers are allowed only when discovered during implementation of a concrete Phase 1 slice and accompanied by a blocker certificate naming the blocked object or file, failed acceptance criterion, unsafe fallback, minimum answer needed, and persistence impact. Phase 1 readiness is judged slice-by-slice rather than by global philosophical completion of the model.

### 4.1.0 Phase 1 readiness scoring

Phase 1 readiness is a derived, evidence-backed, human-reviewed signal. It reports `scope_freeze_readiness` and `mvp_readiness` from per-slice evidence, gates, and score caps rather than from raw open-question count or model confidence. Unresolved nonblocking questions do not reduce Phase 1 readiness after `UBU-D0175` unless they carry a valid blocker certificate. Model-committee may compute readiness reports and propose README readiness text, but it must not publish or update public readiness signals automatically. The accepted rubric is `UBU-D0189`.

The minimum dogfooding loop that proves UbU’s core model is:

1. bootstrap the operator, project context, available work window, constraints, and current or stale affect Snapshot;
2. import or load a curated UbU GitHub fixture with issues, PR/review/CI signals, milestone context, and source links;
3. map those inputs into Objectives, Tasks, External Events, Logs, UniverseState facts, and External References;
4. generate a Calendar with a default Plan for the next work window;
5. present one recommended next Task with an explanation of Objective value, dependencies, deadlines, affect constraints, worker status, and risk findings;
6. record completion, failure, snooze, rejection, decomposition, or override as Log evidence and trigger recalculation when appropriate;
7. preview or perform a human-approved UbU-managed GitHub projection update;
8. show a reconciliation or risk summary that makes the changed model inspectable.

### 4.1.1 Phase 1 public demo criteria

The Phase 1 public demo should use real UbU GitHub issues or a frozen fixture captured from real UbU GitHub data when live access is unsafe, unavailable, or non-reproducible.

The smallest persuasive demo is an end-to-end single-user dogfooding loop:

1. import a curated set of UbU issues, PR/review/CI signals, and milestone context;
2. map those inputs into Objectives, schedulable Tasks, External Events, and traceable source links;
3. generate a Calendar with a default Plan for the next work window;
4. show why the Plan was chosen, including dependencies and worker assignment or worker status;
5. show user-mode affect constraints by applying a current affect Snapshot or by creating an affect-collection Task when affect data is stale or missing;
6. write or preview clearly marked UbU-managed GitHub projection labels, comments, or managed blocks;
7. show a risk summary covering at least deadline risk, dependency fragility, and worker or automation bottlenecks.

The demo must not require Phase 2 sync, Phase 3 multi-user coordination, full RBAC, a complete Compact Calendar UI, or autonomous remote GitHub mutation. If live GitHub writes are unsafe for the public recording, a dry-run projection is acceptable only when it shows the exact payload that would be written after human approval.

### 4.1.2 Phase 1 user-facing loop

Phase 1 must include a minimal first-person UX loop for dogfooding. The loop is intentionally narrow: it collects explicit answers, creates explicit UbU objects, recommends one next Task, records what happened, and recalculates from Logs and current state.

1. **Bootstrap interview**
   - Ask exactly the minimum questions needed to seed the first dogfooding Plan:
     1. `What are you trying to move forward right now?`
     2. `Which project context should UbU use for this first session?`
     3. `How much usable time do you have for the next work window?`
     4. `Is there a deadline, meeting, release target, or external event that changes what matters today?`
     5. `What work should UbU consider first?`
     6. `What is already blocked, unavailable, or not worth recommending right now?`
     7. `How are your energy, stress, and mood right now?`
     8. `When choosing between useful work, what should UbU favor today?`
   - Record the project/context answer as UniverseState facts and source-linked Logs.
   - Record available time, static commitments, deadlines, and immediate constraints as UniverseState facts, Tasks, External Events, or Logs.
   - Record affect answers as a user-declared Snapshot, or create an affect-collection Task when the user skips or the data is stale.
   - Record favored tradeoffs as Preferences only when the user accepts an explicit Preference statement; otherwise retain them as Log notes or Objective annotations.
   - Seed a small Objective/Task set from user answers plus approved GitHub fixtures or configured imports.

2. **Minimum data for one recommendation**
   - A user or operator Identity.
   - At least one active Objective.
   - At least one active schedulable Task linked to that Objective with title and duration or work-window estimate.
   - A current work window or next available time.
   - Current or explicitly stale affect state.
   - Known hard blockers, deadlines, dependencies, and source references for the recommended Task when available.
   - If this minimum is missing, the next recommended Task should be a clarification, import, or affect-collection Task rather than pretending to optimize.

3. **Plan generation**
   - Generate a candidate default Plan from explicit modeled state.
   - Preserve inspectability of the full Plan.
   - Explain relevant Objective value, constraints, dependencies, deadlines, risk findings, worker status, and affect constraints.
   - Mark any fixture-backed, hardcoded, or mock recommendation path directly in the UI and run artifact.

4. **Calendar preview**
   - Treat Calendar preview as a notable recurring Task.
   - Default cadence is before the first recommended Task in a user-declared work window and after a material Calendar regeneration when UbU is about to rely on the new default Plan.
   - Let the user inspect the candidate Calendar or default Plan before relying on it.
   - Ask whether the Plan feels plausible, motivating, humane, and consistent with the user's current context.
   - Ask only lightweight model-repair questions about expected execution, perceived control, social pressure, and missing context; store raw psychological comments as review notes unless the user accepts a concrete canonical update.
   - Allow the user to correct Preferences, Objectives, Tasks, affect Snapshots, availability, or estimates before execution.
   - Let the user snooze, skip, or adjust Calendar-preview cadence through the cadence rule on its evergreen system Objective (§7.4.1).

5. **Next-action focus mode**
   - Present one recommended next Task as the default UI surface.
   - Show title, Objective link, estimated duration or work window, current status, and source/provenance label.
   - Show `why this matters now` with at least the served Objective, timing reason, main dependency or blocker status, affect/staleness status, and top risk or opportunity considered.
   - Show what UbU considered, including visible counts or links for candidate Tasks, blocked Tasks, hard constraints, affect constraints, worker/projection state, and imported or fixture sources.
   - Provide controls for start, done, snooze, reject, decompose, override, and explain more.
   - Keep full Plan inspection one action away through a Plan drawer, timeline, or inspector that shows ordered Tasks, assumptions, source links, and explanation fragments.
   - One-task focus reduces cognitive load; it must not hide the Plan, turn the recommendation into opaque automation, or imply that the user has committed to the default Plan.

6. **Feedback, Log review, and recalculation**
   - After `done`, ask whether completion matched the estimate, whether the modeled effect happened, and whether energy or stress changed enough to update the Snapshot.
   - After failure or blocked execution, ask what assumption failed: missing dependency, wrong duration, stale affect, interruption, unclear Task, changed priority, or other.
   - After snooze, ask for the next review time or condition and whether the reason is timing, affect, dependency, or user preference.
   - After rejection, ask whether the Task is wrong, not valuable now, blocked, too large, already done, duplicate, or should become moot.
   - After override, record the chosen Task or action, the reason if supplied, and whether the override should update Preferences, availability, Task estimates, Objective status, or only this Plan.
   - Treat Log review as a notable recurring Task. Default cadence is at the end of a work window or at the next startup when there are unreconciled deviations, with a weekly catch-up if routine review is skipped.
   - Store reviewed outcomes as canonical updates only when they change Logs, Snapshots, Preferences, Objectives, Tasks, or recalculation triggers; otherwise keep autonomy, competence, relatedness, attitude, subjective-norm, perceived-control, and expected-execution comments as noncanonical review notes or derived report inputs.
   - Record outcomes as Logs and trigger recalculation when the outcome changes Task status, availability, affect state, dependencies, Objective status, or user Preferences.
   - Treat failure, rejection, override, and deviation as model evidence, not user blame.

7. **Discovery mode when appropriate**
   - Allow the user to choose discovery mode at any time.
   - Show active, paused, ended, and pending-review state, enabled sources, routing or retention limits, and evidence count.
   - Provide pause, resume, exit, inspect, reject, delete, and review controls.
   - Collect only enabled Phase 1 evidence such as quick notes, user-selected current action, Calendar context, configured integration events, coarse motion or location category, focus state, and app-state category.
   - Treat inferred observations as reviewable evidence, not final truth; defer ambiguous interpretation until Log review or user-visible reconciliation.

The Phase 1 version may be simple and fixture-backed. Fixture behavior must be labeled as fixture behavior, mock recommendations must not be described as implemented planning, and public demos should show the exact boundary between explicit UbU objects, approved fixtures, and implemented planner behavior. The public demonstration pattern is bootstrap, one recommended next Task, explanation, user feedback, recalculation, and full-Plan inspection. It does not require broad email, text-message, file, invoice, note, or personal-data ingestion. It must not claim complete life-modeling, therapeutic authority, autonomous life coaching, complete planning automation, full privacy isolation, Phase 2 sync, or Phase 3 multi-user coordination. The purpose is to demonstrate the core UbU experience: one meaningful next action, with an explanation, grounded in explicit state.

### 4.1.3 Release Outreach Pipeline dogfooding

The Release Outreach Pipeline should become part of ordinary UbU-runs-UbU release management. A minor release should produce a release outreach package when the current project state contains enough user-visible, developer-visible, or contributor-visible change to justify public explanation.

The Phase 1 package is manual but structured. Minimum package artifacts are:

- `manifest.json` with release, source, Identity, Compartment, status, and hash metadata;
- `claim_register.json` with audience, support status, evidence refs, and review state for each public claim;
- `evidence_index.json` for source artifacts, hashes, selectors, and provenance;
- release-note, script, narration/caption, announcement, and publication-metadata drafts when relevant;
- `media_refs.json` for approved screenshots, UI-test exports, fixture captures, or recordings;
- `export_review.json`, `approvals.json`, and `publication_plan.json`;
- known-limitations, future-work, and contributor call-to-action notes tied to real artifacts.

Media provenance records must distinguish approved live captures, approved fixture/synthetic captures, UI-test exports, and mock/demo-only captures. Every claim in scripts, release notes, and public posts links to claim-register evidence and uses the support labels accepted in `UBU-D0180`.

Phase 1 dogfooding may generate text drafts, collect approved screenshots or recordings, assemble a publication plan, and record human approvals. It may use local renderers or LLM/script workers only as Automation Workers that produce candidate artifacts under capability grants.

Post-MVP work includes automatic UI capture generation, demo-flow export, script drafting from repository state, narration and caption generation, thumbnail generation, video rendering automation, platform upload, social posting, mailing-list publication, analytics feedback, and trusted auto-publication policies.

The full video-generation and publication pipeline remains future work. Phase 1 should generate reviewable communication artifacts before trying to automate external publication.

### 4.1.4 EthConf outreach as Association-introspection dogfooding

EthConf outreach is a Phase 1 dogfooding workflow for Association formation and organizational introspection.

Phase 1 represents the workflow with ordinary Objectives, Tasks, Logs, External Events, External References, Release Outreach package artifacts, public dogfooding review packages, and manually reviewed candidate AssociationAttestations. It does not require full Association automation, multi-user Association sync, automatic contact analysis, or broad message/archive ingestion.

Minimum evidence set:

- canonical design files and accepted decisions;
- public dogfooding artifacts such as model-committee review packages, selected patches, issue fixtures, release outreach packages, and public retrospectives;
- structured EthConf outreach notes with private details separated from redacted public summaries;
- follow-up Tasks, missed follow-ups, public replies, contributor/funder/design-partner next actions, and outcome Logs.

Minimum outputs:

- candidate AssociationAttestations about declared project mission, outreach commitments, observed follow-through, and evidence gaps;
- mission-alignment questions, especially whether outreach behavior supports the privacy-first self-governance, contributor-recruitment, workflow-discovery, and compatible-funding goals;
- priority-drift observations and commitment-mismatch notes;
- follow-up Tasks with owners, review dates, evidence refs, and public/private disclosure labels;
- redacted retrospective notes suitable for public dogfooding artifacts.

The core review prompt is: `Does UbU's actual outreach behavior prove, from reviewable evidence, that the project pursued its stated goals?` The answer may be yes, no, mixed, or insufficient evidence, and must cite evidence or gaps rather than model confidence alone.

Public artifacts expose only redacted summaries, evidence selectors, hashes, public links, reviewed claims, and approved follow-up Tasks. They must not expose private notes, raw contact details, unapproved conversation content, private funding terms, private relationship hypotheses, or Compartment-protected payloads.

Acceptance criteria: the workflow produces one evidence-backed retrospective, at least one candidate AssociationAttestation or explicit no-evidence finding, at least one mission-alignment question, any needed follow-up Tasks, an export review, and a public-safe redacted artifact that labels fixture/manual/LLM-assisted content. A human reviewer must be able to trace each public claim to evidence or to a recorded evidence gap.

### 4.1.5 Optional VoxPopuli EthConf demo

UbU may optionally demonstrate a **VoxPopuli** flow at EthConf if it does not displace higher-priority dogfooding, contributor, or funder deliverables. In this flow, a user speaks freely about what they wish would happen or what feels disorganized. An LLM-assisted extractor converts that natural-language input into candidate Objectives, Tasks, constraints, preferences, and planning assumptions for the user to inspect, correct, accept, or reject.

VoxPopuli is a trust-building outreach hook, not a replacement for the narrow Phase 1 bootstrap interview. It should demonstrate how LLMs can help structure abstract human concerns while UbU remains the explicit, inspectable planning system.

### 4.1.6 Phase 1 implementation priority order

Phase 1 implementation proceeds in this priority order:

1. Remove all in-memory orchestrator state (`MemoryState`) and fully implement `ubu_store` as the canonical local admission and persistence path for the running loop.
2. Log event vocabulary: correction, annotation, provenance, worker-submission, and recalculation-trigger entries.
3. UniverseState facts.
4. Affect Snapshot content, accepted mutation vocabulary, and deterministic precondition evaluation.
5. Structured bootstrap interview with seed Objective, Task, and Snapshot creation.
6. Plan and Calendar generation for a next work window with a default Plan and inspectable explanation.
7. Activate `ubu_github_adapter` as a functioning orchestrator dependency for import, projection preview, approved writes, and reconciliation.

The remaining §4.1 frozen-set items follow in bootstrap-dependency-driven ordering: persistence and vocabulary precede the features that must reference them.

**Phase 1 realization (`UBU-D0241`).** Step 3 (UniverseState facts) is foundationally realized, together with the accepted mutation vocabulary and deterministic precondition evaluation named in step 4, implemented as the §11 container and pure `ubu-core` semantics and persisted by `ubu-store`. The precondition, effect, and bootstrap-fact wiring of these facts into the loop is realized across Wiring-A/B/C (`UBU-D0242`): preconditions gate planning, a completed Task's effects mutate the facts, and the bootstrap records the initial facts under the `UBU-D0243` namespace convention. Step 4's affect-Snapshot content is realized by the affect observation route (`UBU-D0304`); step 5's interactive bootstrap interview remains.

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

### 4.3 Phase 2: Single-user multi-device synchronization

A single user runs UbU across multiple Devices / execution enclaves.

Phase 2 is in development from 2026-10-10 (`UBU-D0305`), and the switch of `UBU-D0275` follows its completion.

Phase 2 validates:

- local-first sync;
- partial replication;
- Zone and Compartment boundaries;
- device roles;
- conflict handling;
- user-owned worker execution on the user's laptop, desktop, or other trusted personal compute device.

Design rubric:

> Phase 2 should feel like multi-agent coordination, but all agents happen to agree.

A Phase 2 personal worker is not a mandatory cloud dependency. It is an optional execution backend for a user's own UbU instance. It may provide more CPU, GPU, memory, storage, network availability, or battery-independent runtime than the mobile device while preserving the user's practical control over the compute environment.

### 4.4 Phase 3: Minimal multi-user / Identity coordination

Multiple humans coordinate through explicit Identities, capabilities, limited disclosure, and commitments.

Phase 3 should enable coordination without surveillance or shared global truth.

### 4.5 Phase 4: Version 1.0 release track

Phase 4 begins the full version 1.0 release track. It expands Resource and Skill usability, Technique-database integration, DIY-versus-purchase/hire planning, and the user-facing capability graph without requiring the public Skill Barter marketplace.

### 4.6 Phase 5+: Version 1.0+ release track

Phase 5+ continues the full version 1.0+ track with mature inventory, financial-management extensions, public or federated Skill Barter, reputation/evidence, dispute workflows, and other multi-party marketplace features.

### 4.7 Phase map and build order

| Phase | Scope | Status |
|---|---|---|
| Phase 0 | ETHConf NYC demo | Complete |
| Phase 1 | Single-user GitHub dogfooding (§4.1) | Feature-complete |
| Phase 1b | Quick UbU merge through the switch, including the planner (§4.2) | Complete (closed by decision 2026-10-10, UBU-D0305; switch deferred until Phase 2 is complete) |
| Phase 2 | Single-user multi-device synchronization (§4.3) | In development (from 2026-10-10, UBU-D0305) |
| Phase 3 | Minimal multi-user / Identity coordination and Resource/Skill task readiness (§4.4) | Planned |
| Phase 4 | Version 1.0 release track (§4.5) | Planned |
| Phase 5+ | Version 1.0+ release track (§4.6) | Planned |

Phases are built in table order. A phase label names the phase that implements an item; pulling an item forward re-tags that item and never renumbers phases (`UBU-D0275`). Labels used before `UBU-D0275` map as follows: Phase 3A becomes Phase 3, Phase 3B becomes Phase 4, and Phase 4+ becomes Phase 5+.

---
```

#### DESIGN.md §12.2 — Discovery mode

```markdown
### 12.2 Discovery mode

**Discovery mode** is a user-selectable workflow state, not a fourth instance operating mode. It is off by default and may be started, paused, resumed, exited, or inspected by the user at any time. The UI must show active capture state, enabled sources, local/cloud routing status, retention limits, and pending-review evidence count.

Phase 1 session states are `inactive`, `active`, `paused`, `ended`, and `pending_review`. A minimal `DiscoveryEvidenceItem` is a reviewable candidate artifact with session ref, effective time or interval, source kind, signal kind, payload ref or redacted summary, confidence, Compartment or low-security label, provenance, and review status. Raw evidence is not final truth.

Allowed Phase 1 discovery inputs are intentionally narrow:

- explicit quick notes, user-selected current action, voice or text notes intentionally captured by the user, and manual start/stop markers;
- UbU app state, Task controls, Calendar or default Plan context, timers, focus state, and foreground app category or app identifier when explicitly enabled;
- configured integration events already allowed by Compartment, External Event, and External Reference policy;
- coarse motion category such as stationary, walking, transit, or driving;
- coarse user-defined location category or geofence event, not continuous raw location by default;
- device state needed for interpretation, such as screen on/off or network/offline state.

Phase 1 discovery excludes covert or broad capture by default: continuous microphone, camera, screen recording, keystroke logging, raw message bodies, raw file contents, raw GPS trails, and cross-Identity sharing are outside the default discovery input set. Any later use of those sources requires a separate explicit mode, Compartment review, routing disclosure, and user approval.

**AV refinement for the MVP interaction surface (`UBU-D0292`).** The primary camera-and-voice surface does not enable continuous sources implicitly. A consented capture session remains visible, source-scoped, pausable and inspectable under the clause above. Process raw AV locally on the operator's own hardware, minimize context and retain only material justified by its purpose. Selective clips, summaries, hashes or deletion are operator-controlled under Compartment policy; subsequent export needs its own allowed route and approval. No-cloud/no-export denials remain hard. Segmentation and bounded local interfaces follow §21.1.5 (`UBU-D0326`); raw-media retention defaults follow `UBU-D0327` below and operator controls follow `UBU-D0328`; recording a policy does not claim its enforcement has shipped.

**Default raw-media retention (`UBU-D0327`).** For an explicitly enabled continuous or near-continuous perception session, durable raw-media retention is off. Use a local memory-only rolling buffer with a maximum sample age of **60 seconds from acquisition**, measured on the original source monotonic clock. Release material earlier when its authorized processing/look-back purpose no longer needs it or a shorter policy limit applies. The finite byte and aggregate-resource limits required by §21.1.5 may shorten this window; it is a privacy ceiling, not a promised look-back duration or latency result. Pins, worker copies, queues, retries and decoded frames share the original deadline. No raw disk spool, debug payload, general cache, automatic backup or sync copy is authorized by this default. A Device/backend profile whose persistence paths cannot meet effective policy is ineligible.

On pause, end, source/Identity/Compartment change or revocation, invalidate affected reads and pending results immediately and release unsaved raw buffers and copies within §21.1.5's bounded cancellation grace, without exceeding the original age ceiling. Resume starts a fresh source epoch. Pending review, an unfinished model job and session metadata do not keep raw media alive; expired spans become explicitly unavailable.

A purposeful clip requires explicit operator approval before durable saving, bound to the selected source intervals, purpose, eligible capture Device, Compartment and current policy versions. A relevance suggestion or capture consent alone cannot approve it. Store only the bounded selected material in access-controlled encrypted local storage on that Device, on the operator's own hardware, with lineage and an absolute expiry. The default expiry is **24 hours after the earliest captured sample**, or an earlier source/Compartment deadline. Missing reliable capture-time mapping denies durable saving. Review, copying, reprocessing and restart never reset expiry. A longer finite lifetime requires a separate purpose-specific approval bound to the clip and current policies before expiry; expired material cannot be recovered by renewed approval. Durable saving remains unavailable if eligible storage, finite clip/quota limits or expiry enforcement are absent. Recheck current policy and Device eligibility on every access; capture-time approval is not continuing authority. Backup, another Device and export require their separately allowed route and approval; hard denials remain binding.

Expiry denies further reads and result release and cancels affected jobs. Remove expired app-controlled clip payloads, any clip-specific keys and reconstructive raw copies; if cleanup is interrupted, reconcile deadlines and complete pending cleanup before reopening capture or retained-media access after restart. Clock rollback or uncertain remaining lifetime must deny access rather than extend retention. Report scheduled, pending, failed and confirmed local deletion distinctly; a deadline or local unlink cannot establish remote deletion or forensic erasure. App shutdown may leave cleanup pending and must not be reported as completed deletion.

Do not automatically replace discarded raw media with a transcript, embedding, hash or summary archive. Separately retained minimized evidence needs its own allowed purpose and retention authority, inherits source disclosure restrictions, and may outlive raw media only when policy permits. Purge disallowed reconstructive derivatives and retain only permitted gap/deletion metadata. The append-only rules below continue to govern admitted claims. This is a policy decision, not implemented automated deletion or a physical-erasure guarantee; continuous capture and clip storage require the revision-pinned retention checks in `UBU-D0327` before enablement. Detailed operator controls follow `UBU-D0328` below.

Selective raw-media retention and append-only evidence are distinct. Discarding a raw stream or deleting an authorized clip does not edit an admitted historical claim in place: append minimized correction, revocation, deletion or tombstone metadata with the appropriate links. The immutable Log retains evidence about what was asserted, reviewed or withdrawn, not an automatic reconstructible copy of private raw media. A hash is not proof of an event and can itself leak correlation; policy must govern it. Later views can mark deleted evidence unavailable without inventing a replacement observation. `UBU-Q0162` tracks the reported Log-editability enforcement gap.

**Operator media controls (`UBU-D0328`, Phase 2).** Keep capture state and pause/stop controls available independently of model completion. A policy-filtered media inventory shows each permitted source/session, capture Device and processing route, purpose, source intervals and coverage gaps, transient raw buffer versus retained artifact, storage destination, effective retention rule/deadline and cleanup state. Inspect/replay only material still available and authorized; show expired/deleted evidence as unavailable. Show known permitted copies and export outcomes without claiming a complete inventory of other Devices or recipients. Counts, previews, filenames, source refs and Compartment labels obey current visibility policy; omit an item when even existence is denied.

Offer independent actions with no durable-save or export option selected by default. Show the selected source/artifact scope, destination, retention effects and any remaining permitted evidence before committing an action. Capture consent, relevance suggestions, model output and prior deletion authorize none of these actions implicitly.

| Control | Required behavior |
|---|---|
| Discard raw / Delete retained artifact | Remove the selected app-controlled payloads and affected disallowed copies/derivatives; invalidate their handles and pending reads/results, cancel affected work and report cleanup under `UBU-D0327`. Discard does not create a summary, transcript, embedding or hash. It acts on the displayed scope; Stop capture is separately available immediately. |
| Create summary | Use only selected, permitted local source spans with bounded analysis. Preview the exact proposed text with source/transform provenance, analyzed/missing coverage, attribution and uncertainty. It remains advisory evidence requiring ordinary review/admission; a summary cannot certify an event or effect. |
| Create hash | Make hashing an explicit optional operation. Show the algorithm/version, exact input representation and source scope; record lineage only as policy permits. A hash can disclose identity or correlation and is neither anonymization nor proof of the captured event. No mandatory media hash belongs in discard or audit records. |
| Save clip | Preview the actual selected sources, channels, intervals and any codec padding. Apply `UBU-D0327`'s eligible encrypted local storage, explicit version-bound approval and absolute expiry; use §21.1.5's bounded authorized read/output scope. Unavailable spans cannot be recovered or silently replaced with adjacent capture. |
| Export | Review the final output and metadata locally, then obtain a separate operation-bound approval for an allowed recipient and route through the authoritative Compartment/export gate. Include filenames, attachments, thumbnails, captions, timestamps, source refs and hashes in disclosure review; clipboard, share-sheet, file-copy and integration routes outside protected storage receive the same checks. |

Summary/hash previews have finite temporary leases under §21.1.5. Durable artifact retention requires an explicit separate save choice bound to the displayed result/source versions, actor/Device, purpose, eligible local destination and current source-policy versions, with the effective retention rule and any absolute expiry. Admitted claims and Task lore retain their existing source-history/lifecycle rules. The inventory shows the approved result and its own retention authority; permitted derivatives may outlive raw media, but raw discard never grants that authority. Review, copying and retries cannot renew original source or artifact deadlines. Combining actions must preserve each approval and its failure outcome; a failed derivation cannot silently retain raw media past expiry. No cloud fallback, training use, backup or sync permission follows.

Export approval binds the actor and eligible Device, immutable final artifact/payload versions, all contributing source restrictions and current policy versions, purpose, recipient, route, format/metadata, recipient retention conditions, finite approval expiry and idempotency key. A displayed finite batch may share approval; future captures and wildcard scopes cannot. State known recipient retention/deletion assurances and mark unknowns explicitly. If required protection cannot be established, deny the route. Deleting raw media, redacting text or selecting a hash never declassifies an output. Mixed-source outputs satisfy every source's restrictions; `local_only`, `no_cloud_llm`, `no_external_export` and allowed-Device/integration denials remain hard, including on derived content. UI approval cannot substitute for route authority or the existing export permit.

Recheck authority, visibility, policy, artifact availability and approval bindings on every read/result release and immediately before egress, serialized with revocation/cancellation. For streamed export, recheck before each released segment and stop unsent bytes on invalidation; emitted bytes cannot be recalled. Changed content, source restrictions, recipient, route, Device or relevant policy versions invalidates approval and requires fresh permitted review. Cancel invalidates unsent approval and queued export immediately; restart cannot replay it. Expiry of material required by the operation denies execution. Retention of a separately authorized derivative does not require recreating its raw source. Report completed, failed, cancelled and delivery-unknown outcomes distinctly; retries use current checks and ordinary idempotency/outcome rules, never another recipient or changed payload under old approval. Relevant stale-sync/projection checks in §1 and disclosure/tool rules in §§21.3–21.4 still apply.

Record permitted minimized action, approval/denial, policy, source/artifact lineage and cleanup outcome links through existing review/Log paths. Rejected/dismissed proposals follow §17.0 without a compulsory reason or Preference inference. Raw deletion leaves admitted claims subject to their existing append-only correction/redaction rules and marks their source unavailable; withdrawing a claim is a separate correction/revocation. Purge disallowed reconstructive derivatives, preserving only permitted correction/deletion/gap metadata. Distinguish scheduled, pending, failed and confirmed local cleanup; never imply remote recall or forensic erasure. `UBU-D0328`'s revision-pinned behavioral gates are required before exposing these controls or export routes; this specifies behavior, not shipped enforcement.

Admission rules:

- A user-confirmed actual action that reconciles a planned interval is recorded with `plan_realized`; if it completes, fails, or moots a Task, the corresponding Task lifecycle Log is also written.
- A user override of the current recommendation records `decision_recorded` with `decision_kind = system_recommendation_overridden`, `authority_source = user_override`, chosen action or Task when known, optional reason, and a `user_override` recalculation trigger when planner-relevant.
- A user-declared current state becomes a Snapshot only when it asserts observed state such as affect, availability, location category, or capacity.
- Preferences change only through explicit accepted pairwise Preference statements. Repeated behavior and inferred reasons are not Preferences.
- Task estimates, dependencies, status, Objective annotations, or Objective status change only after user acceptance or normal validated admission. Otherwise the item remains an unresolved review item.

Undetailed periods should be represented with uncertainty rather than filled in by inference. The review UI should allow `unknown`, `private`, `rest`, `interruption`, `planned Task`, `different Task`, `quick note`, `other`, and free-text correction. UbU must not infer Preference changes, Objective failure, habit patterns, or moral meaning from unknown or private time.

Before treating repeated behavior as a habit pattern, UbU must ask a clarification prompt such as: `I have seen this pattern more than once: [behavior] during [context]. Should UbU plan around it, help you change it, treat it as not a pattern, or leave it unresolved?` Valid answers are `endorse_and_plan_around`, `tolerate_but_review`, `unwanted_help_change`, `not_a_pattern`, and `leave_unresolved`.

Discovery mode preserves sovereignty by remaining visible, opt-in, source-scoped, pausable, inspectable, and correctable. Users may reject, delete where retention policy permits, mark private/unknown, correct, defer, or accept evidence before it becomes canonical state. Compartment policy, `no_cloud_llm`, `no_external_export`, allowed-device, and allowed-integration denials remain hard boundaries.

---
```

#### DESIGN.md §16.3 — Planner grammar direction

```markdown
### 16.3 Planner grammar direction

Compact Calendar planning is no longer framed as a bare DFS process that directly produces the default Plan. The current architecture is:

1. Build the skeleton Plan from Static Tasks and dependency DAGs.
2. Legitimize the skeleton Plan into a minimally human-viable baseline.
3. Generate richer candidate Plans by adding optional Dynamic Tasks, alternative placements, and alternative Techniques for the same Objectives (§16.3.1).
4. Use semi-legitimization or full legitimization to reject unrealistic candidates.
5. Validate finalists against hard constraints.
6. Select the user-facing default Plan by Plan probability among deterministic candidate Plans.
7. Package compact repair metadata for runtime use.

DFS-like search may still be useful for candidate construction. BFS-like search may still be useful for near-term divergence. Greedy selection may still be useful as a deliberately unintelligent baseline. None of those algorithms is the complete planning architecture by itself.

Execution profiles are additive rather than mutually exclusive. The greedy mean-duration planner is the required MVP benchmark. Local and mobile profiles must support deterministic skeletonization, conservative legitimization, exact hard-constraint checks, local repair recipes, and a short-horizon branch cache. Desktop, worker, and hosted profiles may add DFS-like candidate construction, local search, solver-backed finalist validation, and GPU-friendly scoring or simulation. GPU or learned search may propose and score candidates, but exact or conservative validation certifies the selected Plan.

**Chunked search (`UBU-D0279`).** Phase 1b candidate construction partitions the horizon into chunks at capacity-occupying Static Tasks and other fixed placements. The search first assigns placement units to chunks. That is the coupled step, constrained by allowed ranges, dependencies, and state carried between chunks. It then orders each chunk's units independently and in parallel, sweeping chunks in time order and pruning at chunk boundaries. Rollouts evaluate chunks separately, sharing latent draws across chunks. The CPU reference path and the GPU engine share these semantics, and the current chunk is the unit of local repair.

**Splittable Tasks (`UBU-D0284`).** Atomic Tasks and compiled decomposition segments occupy one chunk. A splittable Task may assign at most one piece to each eligible chunk, and every piece must remain inside the Task's allowed range. The chunk-assignment level chooses how much work from the Task enters each chunk; ordinary within-chunk ordering then places that piece. The first piece has no resume overhead, while each later piece reserves the Task's constant `resume_overhead_seconds` before doing more work. Rollouts draw one total work duration for the Task, spend it across pieces, and count a continuation into a later eligible chunk as feasible only when `max_pieces`, window, dependencies, and carried state still permit it. Splittable work earns proportional scheduled-work value for completed sampled work inside the horizon, but completion, effects, and outgoing dependencies still wait for the last piece. Phase 1b needs no additional interruption input beyond the split policy; later stochastic interruption modeling can use the same policy to distinguish continuation from restart.

**Partial placement (`UBU-D0289`).** Candidate construction is best-effort for optional Dynamic work after Static placements, mandatory routine occurrences, baseline/support work, and prerequisites of placed work are protected. If capacity is short, optional units are omitted deterministically by lowest value, then latest deadline or `latest_finish`, then Task id. The response stays usable when at least one certifiable candidate remains, with omitted units reported as unplaced diagnostics; their dependents are deferred rather than independently scheduled. The CPU may make one policy-bounded horizon-extension attempt before deferral when extension is an allowed safe alternative, but it records whether extension was skipped, attempted, or exhausted. Under chunked search, unplaced reasons distinguish not enough total free capacity from no eligible chunk large enough for an atomic unit. Splittable Tasks may be partially placed across eligible chunks and carry remaining work forward, but completion, effects, and outgoing dependencies wait for the final piece. Unplaced diagnostics feed Plan explanation, risk reports, and next-action context; focus mode may recommend a triage action for unplaced work, but not the unplaced work itself.

**Sweep mechanics and streaming (`UBU-D0280`, `UBU-D0281`).** The sweep expands each branch into up to K alternatives per chunk and keeps the best B partial plans after each chunk depth. It merges branches that reach the same state, and assigns the most constrained units first. Interactive requests stream certified chunk results, so the user can act on the first chunk while later chunks are still being searched; a streamed chunk stays provisional until the user starts it. Batch requests return only the final response.

**Outcome continuations (`UBU-D0285`).** At a chunk boundary, an outcome state is the equivalence class over completed units, failed or omitted units when they affect reconstruction, splittable progress and remaining work, carried UniverseState/dependency/affect state, modeled external-event or interruption assumptions, and the next chunk cursor. Rollout frequencies under shared latent draws estimate each state's probability with recorded confidence. The sweep ranks boundary outcome states by probability and precomputes CPU-certified next-chunk continuations until `branch_coverage_target` is met inside `reactive_horizon_seconds` or the compute budget is exhausted. Decision alternatives and outcome continuations share that budget: current-chunk certification and the requested K alternatives take priority, then continuation work follows descending outcome probability with recovery-critical and deadline-fragile ties first. Deviations inside a chunk use decision envelopes and repair recipes, not intra-chunk precomputed branches. Interactive streaming may expose outcome continuations incrementally after the first certified chunk, with final coverage reported in the final response.

Plan probability is represented internally as probability metadata rather than only as a display scalar. The minimum record contains a display scalar, a log probability for stable computation, an optional probability interval, provenance over the modeled probabilistic inputs, and correlation-group or scenario references when inputs are not independent. Implementations may multiply probabilities only for inputs declared independent. Correlated or unknown relationships must use joint scenarios, shared random variables, correlation groups, or conservative intervals rather than pretending independence.
```

#### DESIGN.md §16.5 — Mobile stewardship metadata

```markdown
### 16.5 Mobile stewardship metadata

A compact Calendar should carry metadata that lets constrained devices preserve legitimacy without performing full global replanning. Useful metadata includes:

- protected, flexible, and disposable Task criticality;
- decision envelopes such as earliest start, latest finish, and movable windows;
- cached explanations for why a Task matters and what depends on it;
- conflict severity levels;
- last-legitimate-Plan references;
- simple repair recipes;
- recovery-critical and deadline-fragile markers;
- recalculation triggers and remote-assist eligibility.

The MVP-friendly subset is Task criticality, last legitimate Plan storage, simple repair rules, conflict severity levels, cached explanations, next-best-action mode, basic decision envelopes, and compact outcome-continuation refs for probable chunk-boundary outcomes. Unbounded rich branch packaging and learned local policy models are post-MVP by default.

**Phase 1b mobile stewardship schema (`UBU-D0256`).** A compact Calendar stores `mobile_stewardship_metadata` for each Task or Plan segment that mobile may recommend, move, omit, or explain. The record is compact and by reference where possible; it contains `criticality`, `decision_envelope`, `conflict_severity`, `explanation_fragments`, `repair_recipe_refs`, optional `outcome_continuation_refs`, and optional `recalculation_triggers` and `remote_assist_eligible`. Unbounded rich branch packaging, learned local policy, and global re-optimization remain outside MVP.

`outcome_continuation_refs` point to certified next-chunk continuations by boundary outcome digest, probability estimate, confidence summary, coverage scope, and stale-after value. Mobile uses them only after observed execution reaches the corresponding chunk boundary; inside the current chunk it still relies on decision envelopes, repair recipes, and escalation when local repair is no longer legitimate.

Task criticality is:

- `protected`: required for hard Calendar Logic, Static Tasks, prerequisites, dependencies, full legitimization support, recovery or safety, or a user-declared must-do commitment. Mobile must not silently drop it, shorten it below its minimum duration, or move it outside its envelope.
- `flexible`: legitimate to move, split, or shorten within its envelope when dependencies, preconditions, affect assumptions, and hard constraints remain satisfied. Mobile may silently repair flexible Tasks only when the resulting conflict severity is `minor`.
- `disposable`: optional or opportunistic work that may be skipped, replaced by an eligible next-best action, or left for desktop/cloud refinement when doing so does not break protected or flexible downstream commitments.

The MVP criticality object is `criticality: { level, reason_refs, recovery_critical, deadline_fragile, min_duration_seconds, user_confirmation_required, disposal_effect }`. `level` is one of `protected`, `flexible`, or `disposable`; `reason_refs` point to Objectives, Static Tasks, dependencies, preconditions, legitimization support, user commitments, or risk findings; `disposal_effect` is a short enum-or-string summary such as `none`, `lost_optional_value`, `missed_deadline_risk`, or `breaks_dependency`.

Task-level `allowed_time_range` is an admitted scheduling input for Dynamic Tasks. A decision envelope is later derived from a specific Plan placement, criticality, dependencies, preconditions, and repair policy so mobile can preserve or repair that Plan; it must not be treated as the source of the Task's allowed range.

The MVP decision envelope is `decision_envelope: { earliest_start, latest_start, latest_finish, movable_windows, min_duration_seconds, target_duration_seconds, dependency_refs, precondition_refs, affected_static_refs, affect_assumption_refs, resource_or_location_refs, repair_budget_seconds, stale_after }`. `movable_windows` is an ordered list of `{ start, end }` intervals in which the Task may begin or occupy time, depending on the Task's duration fields. Empty or absent movable windows mean the Task is fixed or must be handled by hard Calendar Logic rather than local motion.

The shared conflict severity vocabulary is:

- `advisory`: stale or degraded metadata exists, but the current and next recommendation remain legitimate.
- `minor`: a local repair inside recorded envelopes preserves all protected commitments and can be undone or inspected later.
- `major`: the current or next recommendation remains explainable, but the user must choose, confirm, or supply missing information before mobile changes a protected commitment or meaningful tradeoff.
- `blocking`: mobile cannot identify a legitimate current or next Task because a hard constraint, Static Task, precondition, dependency, affect legitimacy requirement, or required starting state is violated or unknown.

This severity vocabulary is also the Phase 1b projection-conflict vocabulary: projection-specific classes may map into these severities, but must not invent a parallel severity ladder without a later decision.

**Phase 1b projection conflict policy (`UBU-D0261`).** Projection reconciliation uses the sync conflict class `projection_conflict` with projection-specific quadrants: `known_object_edited`, `known_object_deleted`, `unknown_object_appeared`, and `expected_object_absent`. Automatic handling is allowed only when the result is deterministic, idempotent, within projection policy and decision envelopes, and does not choose between canonical user intent, protected Calendar ownership, or independently changed third-party truth.

The quadrant defaults are:

- `known_object_edited`: auto-resolvable only for non-authority metadata repair or for a permitted edit inside the object's explicit decision envelope. A duration edit to a flexible or disposable Dynamic Task may be `minor` when minimum duration, dependencies, affect assumptions, and Static Tasks remain valid; an edit that pins or moves a Dynamic Task to an external wall-clock time is `major` unless the projection policy already granted that editable window. Protected, Static, dependency-bearing, or precondition-affecting edits are `major` or `blocking`.
- `known_object_deleted`: never silently tombstones or removes the canonical object. If the object is already tombstoned, withdrawn from the projection window, or awaiting idempotent projection deletion, the absence may be `advisory`; otherwise deletion of an active canonical object is `major`, or `blocking` when current/next legitimacy depends on it.
- `unknown_object_appeared`: is contained as external evidence, import candidate, or projection diagnostic rather than admitted canonical state. It remains `advisory` or `minor` when it does not claim a user commitment or protected slot; it becomes `major` when accepting it would create or override a commitment, dependency, Static Task, or third-party-truth assertion.
- `expected_object_absent`: is `advisory` when explained by tombstone, policy/window rotation, or completed projection deletion; it may be `minor` when UbU can retry or recreate the projection idempotently without overriding external truth. It is `major` or `blocking` when the missing external object affects a protected commitment, a current/next recommendation, a required precondition, or Calendar validity.

Every automatic projection repair emits an auditable `projection_conflict_event` record with the conflict id, projection surface, quadrant, severity, canonical object ref when any, external object ref or redacted handle when allowed, observed object and policy versions, external delta digest, action taken, candidate refs when relevant, resulting mutation or projection-operation refs, and recorded time. If the action admits canonical state, it uses the ordinary mutation-envelope or tombstone path; otherwise it remains projection state, diagnostic state, or candidate metadata and is not a silent canonical mutation.

When an external delta touches the same object as a still-pending candidate, admission compares both against the candidate's observed versions before either is accepted. Exact duplicates collapse; deterministic non-overlapping deltas may update the pending candidate's provenance or candidate mutation proposal. Overlapping deltas quarantine or supersede the candidate and surface a `major` review diagnostic, escalating to `blocking` when current/next legitimacy depends on resolving it. External projection evidence cannot force a pending candidate into admitted state.

Cached explanation fragments are short, non-blaming strings with lineage refs. MVP fragments are `why_now`, `matters_because`, `depends_on`, `supports`, `legitimacy_reason`, `if_skipped`, `repair_summary`, and `staleness_summary`. Each fragment may reference Task ids, Objective refs, dependency/precondition refs, Static Task refs, risk finding refs, and compact Calendar provenance, but should avoid copying private details into more-disclosive replicas.

A repair recipe record is `repair_recipe: { trigger, applies_to_refs, eligible_when, steps, ask_policy, fallback, explanation_fragment_ref }`. MVP triggers are:

- `late_task`: condition on elapsed time and accepted Logs; keep protected Tasks and Static Tasks fixed, compress or drop only flexible/disposable work inside envelopes, then offer the next valid Dynamic Task or next-best action. Escalate to `major` or `blocking` if a protected latest finish, hard precondition, or Static Task would be violated.
- `skipped_task`: for disposable Tasks, record omission and continue; for flexible Tasks, reschedule within the envelope; for protected Tasks, ask before omission or create a blocking diagnostic if no legitimate alternate exists.
- `fatigue_report`: record the user report as a Log/Snapshot input, protect recovery-critical Tasks, reduce to protected and low-affect eligible work, and surface rest or clarification when affect assumptions no longer support the Plan.
- `approaching_static_task`: preserve the Static Task, insert required transition/setup/teardown if already modeled, stop or park the current Task when necessary, and ask when the remaining time cannot satisfy a protected current Task.
- `missing_prerequisite`: re-evaluate the deterministic precondition/dependency chain; if a modeled prerequisite Task is available, schedule or pull it forward inside its envelope; otherwise show a clarification Task or blocking prompt. Mobile must not invent a satisfied state, Resource, Skill, Technique, or external fact.

The last legitimate Plan record is `last_legitimate_plan: { plan_id, plan_digest, generated_at, effective_time, horizon, execution_profile, legitimacy_result_ref, hard_validation_ref, default_segment_digest, universe_state_digest, log_cursor, snapshot_refs, external_event_cursor, compact_calendar_digest, protected_task_refs, decision_envelope_digest, explanation_lineage_digest }`. Mobile compares current reality to this record by replaying accepted Logs, Snapshots, External Events, elapsed time, and user actions since the recorded cursors, then checking whether current and next Task placements remain inside decision envelopes with dependencies, preconditions, affect assumptions, Static Tasks, and coverage still valid.

Mobile may silently repair only for `minor` conflicts where the recipe is explicit, all changes stay inside decision envelopes, protected Tasks and support Tasks are preserved, failed preconditions are not hidden, and the explanation cache remains accurate enough for next-action display. Mobile asks the user for `major` conflicts, protected/discretionary tradeoffs, missing starting state, meaningful omission, or any repair that would change a user commitment. Mobile waits for desktop/cloud refinement or marks the Calendar stale when the issue is advisory, outside the reactive horizon, low coverage but not current-action-critical, or requires global optimization. It enters next-best-action mode when there is no safe default but there is at least one legitimate local Task or clarification Task to show.
```

#### DESIGN.md §16.9 — MVP Compact Calendar implementation

```markdown
### 16.9 MVP Compact Calendar implementation

The minimum Phase 1 Compact Calendar implementation stores the legitimized skeleton baseline, the default Plan, the active execution profile, coverage estimate, uncovered-mass estimate, effective coverage threshold, decision envelopes, Task criticality, conflict severity, cached explanation fragments, simple repair recipes, probability provenance, and last-legitimate-Plan record. It stores user-previewed, risk-report, and debug/reproducibility Plans only when those artifacts are actually generated or needed for audit.

High-probability near-term alternatives within the reactive horizon may be stored as materialized branches or as deterministic reconstruction instructions. Non-selected candidate Plans, expensive global repairs, and low-probability branches are reconstructed on demand from the compact grammar, current UniverseState, Logs, Snapshots, External Events, repair recipes, and cached provenance.

MVP planning does not require exhaustive optimal search, cloud compute, GPU execution, exact coverage proof, learned policy models, or a complete transport grammar for every post-MVP uncertainty source. The required implementation is a deterministic skeletonizer, minimum legitimizer, greedy baseline, bounded candidate expansion, hard-constraint validator, Plan probability/provenance record, compact default-Plan package, short-horizon repair cache or reconstruction path, and low-coverage recalculation trigger.
```

#### DESIGN.md §16.10 — GPU desktop execution backend

```markdown
### 16.10 GPU desktop execution backend

The Phase 1 performance target is a local desktop/laptop GPU backend, delivered in Phase 1b as part of the planner the switch requires (`UBU-D0275`). A small CPU reference path or fixture-backed deterministic path remains required for tests, CI, and contributors without GPU access. Cloud planner backends and the full mobile planner remain deferred beyond Phase 1. Mobile already has a smaller stewardship role; its execution profile and validation contract are specified in §16.10.6.

The implementation-facing contract for this section is `PLANNING_KERNEL_CONTRACT.md`. `DESIGN.md` defines the architectural intent; the contract file defines the Phase 1 schema and boundary details.

#### 16.10.1 Pure function contract

The GPU planning engine is a pure function. Its input is a `PlanningRequest`. Its output is a `PlanningResponse` containing ranked PlanCandidates, diagnostics, warnings, and probability-quality metadata. The engine has no canonical side effects, performs no UbU I/O, and holds no request-semantic mutable state. The CPU kernel owns all canonical state mutation. The GPU engine is advisory only: it proposes; the CPU kernel validates and commits.

In Phase 1b the engine is invoked through a persistent local Python worker process owned by the CPU kernel (`UBU-D0283`). The boundary is local length-prefixed JSON frames over pipes, not a network service. This replaces the earlier in-process typed-call direction so the Rust desktop kernel keeps crash isolation from Python, CUDA, and PyTorch failures, avoids embedding the Python interpreter and its lock in the Rust process, and amortizes Python startup by reusing the worker. The framework is PyTorch.

#### 16.10.2 CPU/GPU handoff contract

The CPU/GPU semantic boundary is defined by two typed objects: `PlanningRequest` and `PlanningResponse`, specified in `PLANNING_KERNEL_CONTRACT.md`. Worker-spawn envelopes such as `GpuAdvisoryRequest` and `GpuAdvisoryResponse` may wrap those objects for process management, framing, cancellation, and telemetry, but they do not define separate planning semantics. Time values crossing the semantic boundary use the contract's RFC 3339 UTC timestamps; integer Unix seconds and tensor offsets are implementation-local lowerings.

`UBU-D0283` now records the explicitly approved internal `stage1-atomic-v1` exception: a minimized request, CPU-derived order/masks and tagged sampling source cross the stage boundary; padded structural arrays return for CPU certification. The canonical `PlanningStreamFrame` and pure semantic boundary are unchanged. The atomic profile does not implement public chunk streaming or the full response contract, and a fallback gate is not evidence of successful tensor computation.

`UBU-D0297` specifies the future worker-derived duration stream: stateless Philox4x32-10 with seed/candidate/Task/draw addressing, an exact open-unit uniform and AS241/fdlibm transform. Both implementations and near-window golden vectors must be frozen and certified before activation. The current atomic profile still receives deterministic fixed/mode placement seconds from CPU; neither the new stream nor expanded Stage 1 stochastic behavior is implemented by this design ticket. Exact duration/window/mask parity remains distinct from the unchanged named numeric/statistical profiles for later-stage scores and rollouts.

`PlanningRequest` includes schema version, planner version, request ID, effective time, generated-at time, mode, RNG seed, time-window policy, horizon policy, compute budget, task graph with CPU-provided `topological_order`, UniverseState snapshot, AffectProfile, scoring policy, constraint policy, payload policy summary, and privacy/provenance payload-safety proof. Optional fields include external event assumptions, repair context, explanation request, and debug flags.

`PlanningResponse` returns ranked PlanCandidates plus diagnostics, rejection counts, warnings, probability-quality metadata, optional coverage estimate, optional compute telemetry, and stage funnel counts. `K=3` PlanCandidates is the Phase 1 default:

- highest-utility candidate;
- most-robust candidate;
- most-schedule-diverse candidate.

K is user-configurable to allow power users to tune computational resource use. The Phase 1 default rollout budget is `n_rollouts = 1000` per finalist unless overridden by the CPU kernel's compute budget.

Interactive requests may be answered as a stream of certified chunk results followed by the final `PlanningResponse`. Batch requests return only the final response (`UBU-D0281`). In Phase 1b the worker emits framed `PlanningStreamFrame` messages: zero or more `chunk_result` frames for interactive delivery, then exactly one `final_response` frame. Each chunk result is certified by the CPU kernel on arrival before it can become user-visible.

Backend selection is CPU-owned. If policy disables GPU use, no compatible local GPU/PyTorch runtime is available, the worker fails, or the compute budget does not justify worker dispatch, the CPU reference path is used. Responses and committed Plans record backend provenance, including backend kind, invocation kind, engine version, framework, device summary when available, request id, RNG seed, tolerance profile, and CPU certification status. GPU scores and rollout estimates are reproducible within the recorded tolerance profile, not bit for bit across hardware; CPU certification and CPU reference goldens are authoritative.

#### 16.10.3 Tensor layout

All tensor batches use padded fixed-size shape `(N_CANDIDATES, MAX_PLANNING_TASKS, ...)` with an explicit boolean validity mask tensor marking valid task slots. Ragged tensors are not used in Phase 1.

`MAX_PLANNING_TASKS = 256` is the Phase 1 planning window ceiling, defined at a single site. Future premium or wide-horizon tiers may increase this ceiling by scalar configuration, subject to memory, scenario-count, correlation-matrix, validation-cost, and backend performance limits. Linear scaling is not assumed as a mathematical guarantee.

#### 16.10.4 Pipeline stages

The GPU engine has four first-class pipeline stages, with semantic boundaries specified in `PLANNING_KERNEL_CONTRACT.md §5`.

1. **Skeleton sampling** — parallel shifted-log-normal or fixed duration sampling across candidates with vectorized propagation over the CPU-provided topological order.

2. **`affect_legitimacy_filter`** — batch sigmoid affect-constraint evaluation across surviving candidates. This stage implements only the sigmoid affect-constraint portion of legitimization. Full legitimization in UbU design is broader: it makes a skeleton Plan human-viable by respecting affect, recovery, transition, rest, sustainability, and support constraints. The `affect_legitimacy_filter` GPU stage does not replace or subsume full legitimization.

3. **Value scoring** — parallel utility, approximate robustness, affect-margin, and schedule-diversity scoring across surviving candidates.

Phase 1b value scoring uses CPU-computed, request-local Task value/priority metadata derived from admitted pairwise Task Preferences (`UBU-D0277`, `UBU-D0282`). Objective labels, deadlines, routine cadence, and revealed-preference evidence may inform review, risk, urgency, or later policy work, but the GPU stage receives only the bounded transient values supplied in the `PlanningRequest`.

4. **Monte Carlo rollout** — joint scenario simulation for finalists using correlation-group Gaussian copula sampling with deterministic rollout seed derivation.

GPU search proposes candidates. Hard constraint certification and final Plan validity are performed by the CPU kernel using exact or conservative validation.

In Phase 1b the stages operate on chunked candidates (`UBU-D0279`, §16.3): skeleton sampling builds candidates chunk by chunk, and the Monte Carlo rollout shares its latent draws across chunks. Chunked search stays inside one planning invocation for the whole request; the worker may batch chunks internally to fit memory, but the CPU does not dispatch each chunk as a separate semantic request.

#### 16.10.5 Stochastic duration model

Task durations use either a fixed duration model or a shifted log-normal distribution. The stochastic model is parameterized by `(min_seconds, mode_seconds, p95_seconds)`.

- `min_seconds` is the optimistic lower support shift used by the planner distribution; it is a subjective best-case modeling prior, not a claim about physical impossibility.
- `mode_seconds` is the most likely duration.
- `p95_seconds` is the high-but-plausible 95th percentile, not a hard upper cap.

The shifted distribution is `D = min_seconds + LogNormal(mu, sigma)`, with the canonical conversion formula specified in `PLANNING_KERNEL_CONTRACT.md §3`. The planner may sample durations greater than `p95_seconds`; that is intentional.

Fixed known durations use a separate fixed-duration model and are represented as a delta distribution. Invalid stochastic triples are rejected by schema validation rather than silently repaired.

For lay audiences this is well described as a stop-light model: a sequence of independent delay sources where slowdowns stack asymmetrically and early arrivals are absorbed cheaply by forward-pulling the next eligible Dynamic Task.

#### 16.10.6 Mobile execution profile

Mobile is a real-time steward of Plan legitimacy (`UBU-D0126`, `UBU-D0256`, `UBU-D0290`). Its measured workload is deterministic skeletonization, exact hard-constraint checks, local repair recipes and a short-horizon branch cache from §16.5, explicitly not the full desktop chunked search.

The persistent Python worker in `UBU-D0283` is a desktop invocation mechanism. A mobile backend runs in process on the device; no Python runtime is shipped on device. The pure `PlanningRequest` → `PlanningResponse` semantic boundary, CPU-owned backend selection, CPU certification and recorded provenance remain unchanged. Mobile execution records `mobile_cpu` or `mobile_gpu`, `in_process_mobile`, and its own framework or compute API. The compute API is undecided pending `UBU-Q0160`'s measurement gate.

The parity rule from `UBU-D0283` applies unchanged: schema decoding, chunk partitioning, task-slot masks, dependency feasibility, hard-constraint feasibility, rejection classes and CPU-certified Plan validity agree exactly with the CPU reference path. Floating-point scores and rollout estimates use named tolerances or statistical criteria. Shared parity fixtures exercise the applicable stages; the workload benchmark remains stewardship. Every mobile-produced Plan is CPU-certified on the device before it is user-visible.

Validation uses a physically connected device over `adb`, not an AVD: emulator graphics use host rendering and cannot measure the phone's driver, vendor extensions, timing or thermal response. The rooted LineageOS device procedure and mandatory pre-flight are in `docs/MOBILE_DEVICE_TESTING.md`. No device procedure may leave the phone in a state that needs a cable to recover.

#### 16.10.7 Correlation groups and rollout matrix

Each stochastic-duration Task may carry `correlation_groups: [{group: str, strength: float}]`. In Phase 1, `strength` is a positive latent-factor loading in `[0, 1]`; negative correlations are deferred.

The CPU kernel constructs a deterministic positive semi-definite correlation matrix by normalizing per-Task positive factor loadings and computing `C = L * L^T + diag(1 - row_norm(L)^2)`. Multiple shared groups combine through the dot product of normalized loading vectors. Numeric jitter may be applied for floating-point error with explicit degraded diagnostics. Silent nearest-PSD projection is not a Phase 1 semantic repair.

Negative correlations are not rejected philosophically. They are deferred because signed pairwise-correlation declarations can create non-PSD matrices and confusing validation failures. Phase 2 should support anti-correlations through signed latent factors, explicit Cholesky-style parameterization, or a signed partial-correlation model.

The RNG seed is an explicit required input, ensuring plan generation is reproducible for peer debugging.

#### 16.10.8 Solver selection and deferred backend targets

Phase 1 does not require OR-Tools, Z3, CP-SAT, SMT, MaxSMT, or another external exact solver as a runtime dependency. The mandatory certification path is the built-in CPU kernel: dependency DAG validation, deterministic precondition and effect evaluation, skeleton validity, bounded contradiction diagnostics, full legitimization, hard Calendar Logic checks, provenance validation, payload-safety validation, and final Plan commit.

Solver and library candidates are evaluation targets:

- built-in CPU graph and precondition validator: required reference path and certification source;
- OR-Tools CP-SAT: optional finalist schedule-feasibility and contradiction-minimization experiment;
- Z3 or comparable SMT/MaxSMT: optional logical contradiction-diagnosis experiment;
- PyTorch or CPU local-search libraries: optional advisory candidate-optimization experiments;
- learned models: post-MVP advisory ranking or parameter estimation unless CPU admission certifies the output.

Phase 1 has no mobile GPU target. The required mobile fallback is CPU stewardship for current or next Task hard checks, cached last-legitimate Plan repair, decision envelopes, and simple repair recipes as defined by `UBU-D0256`.

Premium or cloud GPU planning is governed by the execution-provider boundary in `UBU-D0254`, with concrete products deferred to later provider work. Any future cloud backend must preserve the same typed `PlanningRequest`/`PlanningResponse` boundary, Compartment and export gating, payload minimization, provenance, and CPU certification on return.

See also: `UBU-D0166`, `UBU-D0167`, `UBU-D0168`, `UBU-D0169`, `UBU-D0170`, `UBU-D0171`, `UBU-D0172`, `UBU-D0173`, `UBU-D0174`, `UBU-D0212`, and `PLANNING_KERNEL_CONTRACT.md`.

---
```

#### DESIGN.md §21 — Agentic Interaction, MCP, and Delegation Substrate

```markdown
## 21. Agentic Interaction, MCP, and Delegation Substrate

This section records the post-realtime LLM and agentic-platform design direction. The core rule is that unstructured AI can observe, converse, propose, and act under authority, but UbU remains the canonical state-transition, review, privacy, and planning layer.

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

### 21.2 Structured-output admission pipeline

LLM, parser, worker, and agent outputs should follow an admission pipeline:

```text
output
  -> schema validation
  -> semantic validation
  -> Compartment and policy validation
  -> provenance and confidence attachment
  -> conflict detection
  -> user-review classification
  -> accepted canonical object or rejected candidate
```

This applies to structured message extraction, realtime observations, AssociationAttestations, Tasks, Log candidates, Plan repairs, Delegation Substrate packets, and agent outputs.

**Producer grammar rule (`UBU-D0299`).** Every shape offered to a producer must satisfy its unchanged semantic validator for the matching context. An intentional, documented and independently tested conservative subset is allowed; the producer grammar need not equal the full authoring validator. Schema membership is only the first gate, and does not confer canonical truth or authority.

#### 21.2.1 Phase 1b advisory candidate lifecycle

An `AdvisoryCandidate` is the first-class object form for Phase 1b review-queue proposals, including tag, dependency, preference, decomposition, clarification-question, and other advisory outputs before admission. It is `candidate_state`, not admitted state, until an admission decision creates the ordinary canonical mutation or review event.

The minimum record contains `advisory_candidate_id`, `schema_version`, `candidate_kind`, lifecycle state, object version, target or scope refs, normalized proposal shape, payload ref or redacted payload summary, source evidence refs or hashes, confidence, per-field provenance, proposed/effective time or interval, proposing actor or model/tool metadata, origin Device and execution-context provenance, idempotency key, optional suppression key, `compartment_ids`, a review-visible Compartment label or redacted label, disclosure and retention policy, review order metadata, and correction, rejection, deferral, resurfacing, supersession, admission, or archive links. Candidates are per-object records rather than aggregate queue blobs so each candidate can be versioned, replicated, redacted, and reviewed under its own Compartment policy.

The normalized proposal shape for a `Decomposition` candidate contains the target Task ref, proposed Container summary or inherited original-intent fields, ordered proposed child Tasks with stable proposed-child keys, titles, duration models, child-specific constraints and provenance, and split markers after child positions. The advisor proposes split markers only at natural pause points or where child-specific hard constraints require a boundary; review may edit child text, durations, constraints, order, and split markers before admission. Until admitted, these proposed segments remain candidate-state preview material and never become hidden planner constraints.

Lifecycle states are `proposed`, `deferred`, `resurfaced`, `admitted`, `rejected`, `superseded`, and `archived`. New candidates start as `proposed`; `proposed` candidates may be admitted, rejected, deferred, superseded, or archived. Deferred candidates may become `resurfaced`, rejected, superseded, or archived. Resurfaced candidates may be admitted, rejected, deferred again, superseded, or archived. Admitted, rejected, and superseded candidates remain historical records and may later be archived according to retention policy; archiving is not erasure unless a separate redaction or deletion policy permits it.

Unadmitted candidates may influence only review surfaces: ordering, grouping, explanations, deduplication diagnostics, clarification prompts, local suggestions, and explicitly labeled preview diffs. They must not mutate Tasks, tags, dependencies, decompositions, clarification answers, Preferences, Logs, Snapshots, Plans, Calendars, Relationships, Associations, projection state, capability grants, disclosure policy, external messages, or external actions. They must not become hidden planner constraints. Enforcement is by requiring admitted-state reducers, planners, projection writers, and mutation call sites to consume only admitted canonical objects or explicit preview inputs, while candidate reads remain scoped to review APIs that preserve candidate-state labels and Compartment policy.

Deferral preserves an unresolved proposal without treating it as false. A deferred candidate may resurface only on materially new evidence, user request, a policy-triggered review interval, a relevant accepted change to the candidate target or dependencies, or arrival of a clarification or external reference that directly changes reviewability. Resurfacing links the prior deferral, trigger evidence, and reason the old deferral no longer controls; it creates a review item and never admits the candidate automatically.

Rejection is durable correction metadata. A rejected candidate retains a privacy-minimized suppression record containing candidate kind, normalized proposal shape, target and scope shape, Compartment and redaction class, evidence hashes or source fingerprints, extractor/model/schema version, prompt or template digest when relevant, rejection reason or user correction, deciding actor Identity, `authority_source`, decided-at time, retention or purge policy, and the suppression key. The rejected payload may be redacted or purged under retention policy once the suppression record is sufficient; retaining the key must not make the rejected proposal accepted, exportable, more visible, or usable as evidence that the rejected content was true.

Admission, rejection, deferral, resurfacing, supersession, and archival decisions are first-class review events carrying actor Identity, `authority_source`, origin Device, observed candidate version, observed policy versions when policy is relied on, effective time, recorded time, idempotency key, and links to resulting canonical mutations or replacement candidates. Admission emits the ordinary sync-ready mutation envelope for the admitted object change; rejection emits the decision event and suppression record without creating the proposed canonical state.

Review order never changes state category. Proposed and resurfaced candidates may be prioritized ahead of deferred or archived records, but a contested region remains `candidate_state` or `pending_state` until an admission or conflict-resolution event is itself admitted. UI, APIs, derived previews, and reports must label candidate diffs as candidate or preview material and must not present them as `admitted_state` merely because they are high priority, old, repeated, or uncontested in the queue.

#### 21.2.2 Implemented precondition and vocabulary advice

`UBU-D0294` records admitted-precondition review: explicit replacement/clear candidates, stale-value checks, finite escalating holds, admission-reset escalation and the blocking-now seed cap. Rejecting a newly proposed precondition instead durably suppresses that proposal for that Task. These are separate dismissal semantics. Later field reviewers inherit the contract; only precondition review is implemented today.

`UBU-D0295` records the local precondition producer's title/optional-description and recorded-target-name inputs, with no fact values/provenance sent. Every leaf names an existing target, including `absent`; the controller validates before enqueue and admission. Precondition and vocabulary proposal producers consider at most 25 Tasks, return at most three proposals, and refuse when ten or more of their own kind are proposed/resurfaced. Deferred candidates do not count; a permitted batch can cross the refusal threshold, which is not a hard queue ceiling. `UBU-D0296` keeps UniverseTarget proposals name-only, with only operator-authored values admitted.

### 21.3 ContextBundle

A `ContextBundle` is a governed package of context assembled for a model, tool, worker, agent, or long-context review. It is the auditable boundary between UbU's explicit state and an execution backend.

Minimum fields:

- `context_bundle_id`;
- `schema_version`;
- `purpose`;
- `requested_operation`;
- `actor_identity_ref`;
- `related_task_refs`, `related_objective_refs`, or workflow refs when applicable;
- `route_request_ref` and `route_decision_ref` when the bundle is used for LLM routing;
- `destination_ref`, including provider/tool/worker, provider class, model or tool ref, and boundary classification;
- `source_refs`;
- `source_selectors`, time ranges, object ranges, or repository paths included;
- `source_snapshot_refs` or hashes sufficient to identify the assembled version;
- `compartment_refs`;
- `compartment_policy_results`;
- `identity_refs_exposed`;
- `association_refs_exposed`;
- `relationship_refs_exposed` when applicable;
- `data_categories_exposed`;
- `sensitivity_summary`;
- `token_or_size_estimate`;
- `redaction_policy`;
- `minimization_rules_applied`;
- `excluded_source_summary`;
- `prompt_injection_exposure_summary` when the bundle includes untrusted external text, webpages, messages, documents, or repository content;
- `retention_policy`;
- `training_or_secondary_use_policy`;
- `user_visible_summary`;
- `approval_state`;
- `approval_log_ref` when approval or denial was recorded;
- `created_at`;
- `expires_at`;
- `invocation_refs`;
- `supports_candidate_refs`;
- `exposure_summary_ref`;
- `log_refs`.

ContextBundles may contain raw payload only when the task requires it and policy allows it. The preferred representation is references, selectors, summaries, hashes, redacted excerpts, or structural fields that let the model perform the requested work without copying unnecessary private material. Repository and archive-scale bundles should record path or object selectors and excluded material, not merely a broad repository or archive name.

Compartment policy is checked before assembly and again before invocation. If any included source is denied for the destination, UbU must either exclude that source and record the exclusion or deny the bundle. A mixed-Compartment bundle inherits the most restrictive applicable route. User approval, capability grants, workflow settings, and provider preferences cannot override `no_cloud_llm`, `no_external_export`, `local_only`, or other hard Compartment denials.

User approval is required before routing a ContextBundle when the bundle sends protected payload, low-security un-compartmented content beyond structural references, Relationship evidence, Association evidence, private messages, broad repository or archive content, or other long-context bulk material to a cloud provider, external provider, remote worker, managed gateway, third-party tool, or different Identity/Association boundary. Approval is also required when the provider, model, operator, region, retention/training profile, data categories, minimization policy, or source scope differs materially from an already approved workflow template.

Policy-based approval is allowed only when a prior user-approved rule names the workflow, destination class, allowed Compartments or data categories, maximum source scope, minimization rule, retention profile, cost limits when relevant, expiry, and review requirement. The rule must still fail closed on hard Compartment denials or materially broader context than the rule allowed. Local-only deterministic processing or local model use may proceed without per-run approval when existing local policy permits it and the bundle does not cross a new disclosure boundary.

After invocation, UbU should create or update a context exposure summary. The summary records the actual provider/model/tool used, invocation refs, source counts and categories, Compartments, Identities, Associations, and Relationships exposed, raw-versus-summary/reference volume, redactions and exclusions, retention/training profile, prompt-injection exposure notes, approval and routing Log refs, downstream candidate refs, and any denied or omitted context that affected output quality. The summary is a review artifact and must not contain secrets or unnecessary raw payload.

ContextBundles are immutable after use. Corrections, narrower reruns, broader reruns, or provider changes create a new bundle or a superseding revision linked by Log entries. Candidate updates, AssociationAttestations, Log candidates, Plan repairs, Delegation Substrate packets, worker mutation requests, and other downstream outputs must carry `originating_context_bundle_refs` or equivalent provenance. Logs for approval, denial, invocation, output admission, and rejection should link back to the relevant ContextBundle.

### 21.4 MCP-style client and server boundary

UbU as an MCP-style client may call external tools and services. UbU as an MCP-style server may expose narrow tool surfaces to external agents.

Phase 1 server tools are query or candidate-submission tools only:

- `plan_summary.read`;
- `objective_status.query`;
- `task_candidate.create`;
- `log_candidate.submit`;
- `plan_repair.submit`;
- `clarification.request`;
- `delegation_packet.prepare`;
- `projection_preview.submit`;
- `association_attestation_candidate.submit` for fixtures or manually reviewed Association-introspection dogfooding.

Phase 1 server tools do not directly create, update, or delete canonical Objectives, Tasks, Logs, UniverseState, External References, Plans, Calendars, projection state, credentials, or external systems. Any write-like request becomes a candidate, worker-style mutation request, projection request, clarification request, or review artifact admitted by the canonical instance.

A minimum `MCPToolCapabilityGrant` includes grant ID, issuing parent instance, actor Identity, integration or agent identity, allowed tool names, operation kinds, payload schema refs, object refs or object-type scopes, Objective subtree refs, Task set or Container refs, Compartment constraints, Identity/Association disclosure constraints when applicable, integration or destination refs, time window or expiration, rate and cost limits, review requirement, idempotency/version requirement, audit policy, lifecycle status, and revocation refs. Compartment policy is a hard upper bound; a tool grant cannot override `local_only`, `no_cloud_llm`, `no_external_export`, allowed-integration, allowed-device, identity-isolation, or EvidenceUsePolicy denials.

Every external-agent write-like tool call uses a candidate envelope with tool call ID, capability grant ref, actor Identity, target refs, operation, payload, expected prior version when applicable, idempotency key, provenance, evidence refs, originating ContextBundle refs when any, Compartment policy result, confidence when relevant, and review requirement. The canonical instance validates schema, semantics, authority, Compartment policy, idempotency, expected version, assignment or workflow status, and review policy before applying, rejecting, or leaving the candidate pending.

Tool invocations that reach UbU are logged as append-only audit records with redacted argument summary or hash, policy decision, source and destination Identity, Compartment result, cost and rate counters when relevant, result refs, denial or failure reason when safe, and downstream candidate refs. Denied calls fail closed. Retries require the same idempotency key or an explicit superseding request. Rollback is append-only repair through compensating mutation, Log correction, supersession, Task moot transition, projection repair, or AgentAction mitigation metadata; prior invocation and candidate records are not rewritten.

The minimum dogfooding fixture is a local loopback MCP adapter over synthetic or redacted UbU project data. It exposes the Phase 1 server tools, exercises capability grants and Compartment denials, writes only candidate/review artifacts and audit Logs, and supports a dry-run client path for calling external tools without live external mutation.

### 21.5 Delegation Substrate

A Delegation Substrate packet prepares a Task for execution by making the work explicit enough to hand off, automate, review, or perform oneself with less ambiguity.

#### 21.5.1 Task brief and packet ownership

The MVP contract (`UBU-D0342`) keeps reusable intent on Task and a version-bound handoff in a separate `DelegationPacket`. Task remains the canonical owner of its Objective links, description, status, timing, dependencies, preconditions, effects, External References, and Compartment/privacy metadata.

Task may carry an optional `execution_brief` with `purpose`, `intended_executor_type` (default `self`), `expected_output`, `completion_criteria`, `required_evidence`, `self_authority_note`, `review_requirement`, and `escalation_notes`. It may also link `delegation_packet_refs`. Purpose explains why the work matters; expected output names the artifact or outcome and an optional output schema ref; completion criteria describe what must be checked; required evidence names the evidence kinds needed, rather than claiming evidence already exists. An explicit empty evidence list means an authorized completion declaration recorded in Logs is sufficient only where the applicable operation policy permits it.

These fields are optional for ordinary solo capture and do not change §9.3's schedulability invariant. Missing fields remain visibly unknown. Before automated dispatch, purpose, expected output, completion criteria, an explicit evidence rule, and review requirement must be resolved. Description prose and brief fields supply no hidden planner constraints: any typed timing, dependency, precondition, or effect change needs ordinary Task admission.

A packet binds one Task. An Objective-only or Container-level preparation is a preview until ordinary admission creates or selects an executable Task; multi-executor work uses ordinary child Tasks and Container lineage. Packets contain a frozen brief snapshot and versioned source refs, not a second editable Task description or Task lifecycle. Task edits never silently rewrite a dispatched packet. Sensitive details in briefs and packets use policy-gated source refs rather than ordinary WorkItem payload.

#### 21.5.2 Minimum packet schema

All rows below are required on an admitted packet. Lists are explicitly empty when no applicable sources, grants, tools, or routes exist; optional limits may be absent. Draft candidates may remain incomplete and cannot execute.

| Fields | Meaning and requirement |
| --- | --- |
| `delegation_packet_id`, `schema_version`, `packet_version`, `task_ref`, `expected_task_version` | Stable packet identity and the admitted Task version being prepared. |
| `execution_brief_snapshot`, `source_refs` | Resolved brief plus versioned input/evidence refs or hashes. Protected source payload is accessed through policy-gated context, not embedded in ordinary Task structure. |
| `executor_type`, `executor_ref` | Execution class and concrete Identity, worker, tool, or provider endpoint ref; the ref may be null during preparation, but must resolve before dispatch. |
| `authority_source`, `authority_scope`, `capability_grant_refs`, `allowed_tools` | Who authorizes the work, permitted operations and object/integration scope, current grants, and tool allowlist. Worker dispatch requires active parent-issued grants covering its operations. A solo self-authority note is descriptive; it grants no tool or external-action authority. |
| `review_requirement` | `{ mode, reviewer_identity_ref?, policy_ref? }`, with mode `self_review`, `human_review`, or `policy_review`. Human review requires a reviewer ref; policy review requires a previously admitted operation-scoped policy ref. Worker output defaults to human review; self review is for solo work. The effective rule is also recorded in the brief snapshot and cannot weaken Task or operation review requirements. |
| `privacy_scope`, `compartment_refs`, `originating_context_bundle_refs`, `route_decision_refs` | Source classification, allowed disclosure/destination scope, retention-policy refs, and context/routing provenance; each field inherits applicable source restrictions. Lists may be empty for an un-compartmented local self-reminder; boundary-crossing work requires the applicable ContextBundle and policy/route decisions. Un-compartmented content is explicitly `security_level: low`. |
| `execution_limits` | `{ max_elapsed_seconds?, max_calls?, deliver_by?, cost_limit? }`; automated work requires positive finite elapsed-time and call limits. `deliver_by` is an optional UTC output deadline. This record does not replace Task timing constraints or the assignment's lease/heartbeat deadline. |
| `failure_path`, `escalation_path` | Stop/clarify/review behavior on invalid output, denial, timeout, or unmet criteria, and the responsible reviewing Identity. Default: stop and return the unresolved work to the assigning user for review. Retry uses §25.1.2; escalation grants no contact, broader context, reassignment, or subdelegation authority. |
| `created_by_identity_ref`, `created_at`, `provenance`, `idempotency_key`, `status`, `supersedes_packet_ref` | Authorship, audit/version lineage, deduplication, and preparation state; supersedes ref is null for the first revision. |

Packet `status` is preparation state only: `not_delegated` for an admitted self-reminder, `prepared` for an admitted non-self preparation, and `superseded` or `cancelled` when retired through ordinary admission. Preparing a packet does not offer or assign work. An invocation binds the packet id/version; used packet content is immutable. Changes require a new revision, fresh bindings and any renewed approval. Retirement is recorded in append-only history; retention, redaction, and purge follow source policy.

Execution status, lease, delivery, and assignment outcome remain in §25.1.2's assignment record, linked to the packet id/version. Cancelling a preparation does not silently cancel an active assignment; the parent must admit and log its assignment transition. Actual output/evidence and its admission or rejection remain linked candidates and Logs, not mutable claims in the brief.

#### 21.5.3 Executor types and self-reminders

Preserve `self`, `local_agent`, `remote_agent`, `tool`, `human_identity`, `association`, `general_contractor`, and `external_provider`. The type describes intended execution, not authority or trust. Agent types distinguish execution location; cloud inference used by a local agent still needs §2.11 provider routing. An external provider references its descriptor/endpoint, while any assigned worker remains a separately identified actor under a parent-issued grant.

Phase 1 implements self-reminders and explicitly configured agent/tool/provider routes only to the depth required by dogfooding. Worker execution uses the existing assignment contract regardless of executor type. Human-Identity and Association handoff, General Contractor coordination/subdelegation, discovery, bidding, settlement, and marketplace lifecycles remain future work; preparing those types cannot dispatch unsupported execution.

For `self`, no separate packet is required: display the Task brief as "For me", with purpose, expected result, evidence/checklist, constraints, and escalation notes. An optional saved packet uses `not_delegated`, resolves to the user's Identity, and creates no offer, worker lease, notification, recurring reminder, or external disclosure. Starting and completing the Task use ordinary Task/Log admission. A user-selected switch to a worker requires a prepared packet, policy checks, disclosure/approval where required, and explicit parent assignment.

#### 21.5.4 Dispatch, review, and Phase 1 dogfooding

`delegation_packet.prepare` produces a candidate under §21.4; it cannot dispatch work. Before context assembly and again before invocation, the parent validates packet/Task versions, current grants and their versions, allowed operations/tools, executor eligibility, ContextBundle bindings, Compartment policy, disclosure approval, limits, and review policy. Approval binds the packet revision, source versions, destination and relevant policy/grant versions; material change invalidates it. Capability grants, packet notes and user approval cannot override hard Compartment, Identity-isolation, or EvidenceUsePolicy denials. Credentials remain scoped secret references in the existing provider/integration stores, never packet payloads.

Outputs carry packet id/version, assignment ref when applicable, originating ContextBundle refs, evidence/provenance, idempotency key, and expected target versions through §21.2/§25.1.3 admission. Receipt, delivery, or assignment completion alone does not complete a Task, admit modeled effects, or satisfy an Objective. Those changes need separately validated evidence and authorized canonical admission. Assignment failure/expiry leaves work unresolved under §25.1.2; duplicate, stale, revoked, and denied submissions cannot silently change it. Handoff alone never automatically marks the Task moot.

The Phase 1 slice is the optional Task brief, versioned packets for assigned work, worker Identity/grant and assignment links, minimized context and policy decisions, candidate output review, and append-only audit Logs. GitHub dogfooding reuses External References for Issue/PR inputs and returns analysis, patch artifacts, reconciliation reports, or projection previews. Live writes still require §27.3 approval; model-committee retains §3.10's advisory boundary and cannot apply canonical patches or mutate GitHub.

Before claiming this slice implemented, require fixtures covering a solo brief without handoff, explicit worker assignment and delivery pending review, duplicate/stale/revoked or expired submissions, Compartment-denied handoff, permitted disclosed cloud routing, unsupported executor rejection, timeout/escalation without authority expansion, and GitHub preview without live mutation. These are acceptance requirements, not passing runtime evidence.

### 21.6 General Contractor

A General Contractor coordinates multiple subordinate executors to satisfy an Objective or complete a Container of Tasks.

A General Contractor may be a human Identity, agent, or Association. The role may decompose work, assign subtasks, monitor status, request evidence, and submit candidate updates, but only within explicit authority and Compartment bounds.

#### 21.6.1 Coordination mandate

The post-MVP contract (`UBU-D0343`) represents the role as a versioned `CoordinationMandate`, distinct from an executor type, Task, packet, or capability grant. A `general_contractor` packet still binds one executable coordination Task and resolves its executor to the actual actor; a `coordination_mandate_ref` binds the role and mandate version. Objective/Container scope does not make an Objective or Container executable.

| Required fields | Meaning |
| --- | --- |
| `mandate_id`, `schema_version`, `mandate_version`, `root_mandate_ref`, `parent_mandate_ref` | Stable identity and versioned lineage; root points to itself and parent is null at the root. |
| `contractor_actor_ref`, `accountable_identity_ref`, `originating_identity_ref`, `originating_association_ref`, `authority_source_refs` | Concrete executor, accountable actor, and issuing principal with accepted authority evidence; Association ref is null when absent. |
| `coordination_task_ref`, `coordination_packet_ref`, `objective_refs`, `container_refs` | Versioned work scope and the coordination packet id/version; lists may be explicitly empty. |
| `capability_grant_refs`, `coordination_operations`, `subdelegation_policy` | Versioned grants, explicit permitted coordination operations (decompose, select executors, request assignments/grants, collect evidence, report status, submit candidates), and delegable operations, executor/destination scopes, and maximum delegation depth. Empty delegation scope grants no subdelegation. |
| `privacy_scope`, `compartment_refs`, `context_policy_refs`, `settlement_constraint_refs` | Applicable source, disclosure, retention, tool/route and work-agreement restrictions; settlement refs may be empty and grant no payment authority. |
| `aggregate_limits`, `budget_refs`, `allocation_refs` | Finite expiry, total descendant Task/attempt count, active-execution count and aggregate automated call/time ceilings; finite cost ceilings per currency/resource, including explicit zero where spending is forbidden. |
| `review_requirement`, `reserved_rights`, `reporting_rule`, `failure_path`, `escalation_path` | Named reviewer or admitted scoped review policy, retained inspection/pause/revocation rights, finite report cadence or milestone deadlines, and responsible escalation Identity. |
| `created_by_identity_ref`, `created_at`, `provenance`, `idempotency_key`, `status`, `supersedes_mandate_ref` | Audit and revision lineage; lifecycle is derived from append-only admission/transition Logs. |

Mandate states are `proposed`, `active`, `paused`, `revoked`, `expired`, and `completed`. Only an active, currently authorized mandate can support dispatch; preparation or a role label grants no authority. Activation requires originator approval bound to the mandate, packet, source, grant and policy versions. For an Association, accepted governance evidence must identify the issuing/reviewing Identities and their authority under §§19.1 and 26.3; perceived membership or a roster cannot authorize action for participants.

#### 21.6.2 Subdelegation and execution links

Subdelegation is denied by default. An approved policy may permit direct subordinate execution at depth one; another contractor requires an explicitly larger finite depth and its own admitted mandate. For every child, effective authority is the intersection of the root mandate, every ancestor's delegable scope, the child mandate/packet, current capability grants and operation policy. Apply all source restrictions and the strongest applicable review requirements. A child cannot outlive an ancestor, add tools/destinations, weaken evidence/review, or enlarge a budget. Authority from another mandate cannot be combined to widen this chain.

The contractor may propose decomposition, executor selection, reassignment or child grants only for explicitly permitted operations. The authorizing UbU instance admits Tasks, links, assignments and narrowed grants through ordinary validation; the contractor cannot mint credentials or self-authorize a grant. Grant issuance and each dispatch recheck the complete versioned authority chain, source/Task versions, current policy, executor eligibility and allocations. Missing, stale, paused, expired or revoked ancestors deny dispatch. A stale or partial view unable to establish current chain or allocation eligibility cannot dispatch the affected work. Reassignment requires a new executor/packet binding and cancellation or expiry of the old execution slot.

Each admitted `SubdelegationLink` records its id/version, root and parent mandate versions, parent coordination Task/packet, child Task/version, child packet id/version, child executor/accountable Identity, grant refs, allocation refs, assignment or execution/commitment ref, optional child mandate ref, provenance, idempotency key and Log refs. Each child has its own Task and packet. Multiple executors use separate child Tasks or named assignment roles; one active executor owns each slot. Authority links must be acyclic and have exactly one parent; ordinary Task dependencies may cross branches but confer no authority.

Tasks and Containers retain §25.1.2's ordinary structure, including Task-to-Container replacement where applicable. Workers retain its assignment/lease/retry semantics. Human or Association execution additionally requires an accepted scoped work commitment naming the accountable Identity and withdrawal/escalation terms; an offer or mandate cannot impose acceptance. Tool/provider execution binds its invocation to the child packet and grants. Unsupported executor or commitment protocols remain unavailable.

#### 21.6.3 Shared limits, privacy and settlement

All coordination overhead, descendants, retries, reassignment attempts and nested contractors consume the root's aggregate limits as well as their branch limits. Revisions and retries retain cumulative counters and original deadlines; they cannot reset them. A limit increase or renewal requires fresh originator approval and current bindings.

Reserve child cost/resource/call/time allowances atomically against the root and branch budgets before dispatch: consumed plus outstanding reservations must remain within every ceiling. Allocation records bind the mandate/packet versions, quantity and currency/resource unit, reservation id, idempotency key and consumption/release evidence. Concurrent siblings, replay and restart cannot spend an allowance twice. Unknown charges/outcomes keep their reservation until reconciled; release only unused allowance established by evidence. Missing enforceable bounds, unavailable reservation serialization or insufficient allowance denies the affected dispatch. Budget authority never authorizes a commitment, payment or external write.

Each subordinate gets a separately minimized, destination-bound ContextBundle and route decision under §§2.11 and 21.3–21.5; access to the parent's context is not inherited automatically. Derived reports and evidence retain every contributing source's restrictions. Hard Compartment, Identity-isolation and EvidenceUsePolicy denials remain binding on contractors, descendants, logs and review views. An Association principal or reviewer sees only policy-permitted information, including metadata; omit identifiers, counts or even existence when denied.

Propagate accepted work-agreement and settlement restrictions through versioned refs, including permitted counterparties, amounts/units, milestones, dispute/review and settlement destinations when applicable. No payment, escrow release, bargaining, agreement revision or transfer follows from coordinating work or reporting completion. Such actions need separate explicit operation authority and current approval through a supported route; unresolved Skill Barter/settlement workflows remain `UBU-Q0065` work.

#### 21.6.4 Evidence, review and bounded failure

A status report is an immutable candidate carrying report id, reporter Identity, mandate/version, as-of time, child Task/packet and assignment/link refs, claimed status, evidence/provenance, ContextBundle refs, unresolved failures, budget observations and coverage/omission notes where disclosure permits. Preserve subordinate attribution and distinguish reported, validated and admitted outcomes. Contractor summaries cannot replace source evidence or turn missing/withheld reports into success. Corrections or withdrawals append linked records.

The originating user or authorized Association reviewers retain inspection of permitted lineage, grants, limits, evidence and audit outcomes; pause, revocation, rejection, correction and approved reassignment paths; and final acceptance of the aggregate result. Final review defaults to that user or a named Association reviewer; any policy review must be previously admitted and operation-scoped. The contractor cannot weaken these rights or approve its own aggregate result merely by holding the role. Child delivery or assignment completion never alone completes the parent Task or satisfies an Objective; ordinary evidence, version and canonical-admission checks remain required.

Pause, revocation, expiry or exhausted limits invalidate new dispatch and unreleased results across the affected subtree. The authorizing instance records explicit cancellation/expiry transitions for live assignments and requests cancellation of active invocations; it never treats packet retirement as assignment cancellation. Recheck the chain before each privileged action, disclosure/result release and output admission, serialized with invalidation. Late output remains quarantined candidate evidence; use needs fresh authority and ordinary review. Revocation ends UbU execution/disclosure authority without erasing accepted commitments, incurred obligations or dispute rights. Already disclosed data or irreversible external effects cannot be recalled by revocation and require explicit mitigation/dispute records.

Denial, missed reports, timeout, disputed evidence or failed executors stop affected automatic work and return unresolved scope to the named reviewer. Independent branches may continue only within current authority and reservations. Escalation cannot broaden disclosure, contact another party, spend more, replace an executor or renew authority implicitly. Finite depth, descendant/attempt, concurrency, call/time, cost and expiry bounds apply transitively; no automatic renewal, recursive self-appointment or unbounded retry is permitted.

#### 21.6.5 Enablement boundary

This resolves the post-MVP role/data-model contract, not implementation conformance. Phase 1 retains §21.5's configured execution subset and cannot dispatch `general_contractor` packets. Association governance, human coordination and marketplace operation still require their supported later-phase protocols.

Before enabling a supported contractor path, require fixtures for a valid two-executor chain; no delegation permission, excess depth and cycle rejection; child scope/tool/review/Compartment widening denial; concurrent sibling reservation contention and duplicate/restart accounting; cumulative retry/nested overhead and uncertain-charge handling; human/Association acceptance and authority defects; source-linked partial/withheld reports; independent final review; and ancestor revocation/expiry during dispatch, release and admission, including stale versions, cancellation failure and late output. Fixtures must preserve unresolved parent work and prevent authority expansion on failure. These are acceptance requirements, not passing runtime evidence.

### 21.7 Skill Barter marketplace direction

The Skill Barter marketplace is a future specialization of Association coordination, Delegation Substrate, Resource/Skill readiness, and Technique evidence. It is not a Phase 1 public marketplace.

The private Skill model should come first. A user must be able to model, learn, test, refresh, and use Skills for their own life logistics before UbU exposes those Skills to other Identities. Once private skill evidence and Delegation Substrate packets are useful, Skill Barter can become a voluntary exchange layer rather than a generic labor marketplace.

A mature Skill Barter system may support privacy-preserving skilled-work exchange, pseudonymous capability claims, reputation without unnecessary doxxing, scoped work agreements, private or selective-disclosure evidence, lawful user-controlled settlement references, dispute workflows, and human/agentic executor discovery.

For EthConf and cypherpunk outreach, Skill Barter is useful as a future-direction signal: UbU can preserve cryptocurrency-native values of voluntary coordination, sovereign identity, privacy, FOSS development, open markets, economic self-sufficiency, and lifelong skill acquisition without becoming token-first or speculation-first.

The preferred framing is an open, user-sovereign skill economy or a self-reinforcing ecosystem for skill acquisition, task execution, and voluntary exchange. Avoid calling it a closed ecosystem, because that implies platform captivity rather than user sovereignty.

### 21.8 AgentAction and BackgroundProcess

An `AgentAction` is one bounded attempted action by an agent, model, worker, or tool against an external or privileged surface. A `BackgroundProcess` is the durable controller object that decides when zero or more `AgentAction`s should be proposed, run, retried, cancelled, or escalated without necessarily occupying user Calendar time.

`AgentAction` fields describe a single dispatch or candidate dispatch:

- `agent_action_id`;
- optional `background_process_ref`;
- `actor_identity_ref` and `agent_or_tool_ref`;
- purpose, related Objective/Task/workflow refs, and originating ContextBundle refs;
- target surface, operation kind, payload summary or hash, expected prior external version, and idempotency key;
- authority source, capability grant refs, credential or secret-capability refs by opaque handle and version, Compartment refs, Identity/Association disclosure constraints, and provider/tool route decision refs;
- prompt-injection exposure assessment and containment policy;
- compute, rate, money, and privacy-budget estimate plus actual usage when known;
- side-effect class, reversibility, dry-run/candidate status, review requirement, and approval state;
- result status, external result refs, downstream candidate refs, completion evidence, Log refs, and failure or denial reason when safe;
- rollback or mitigation metadata for any external effect that cannot simply be ignored.

`BackgroundProcess` fields describe standing permission and scheduling policy:

- `background_process_id`, owner Identity, purpose, lifecycle status, created/updated/disabled refs, and policy version;
- trigger spec, recurrence or polling rule, event subscriptions, manual-start affordance, run window, deadline when any, expiry, concurrency limit, retry and backoff policy, cancellation policy, and maximum catch-up behavior after downtime;
- allowed action kinds, target surfaces, provider/tool classes, capability grants, credential handles, Compartments, and ContextBundle templates;
- compute, rate, money, and privacy-budget caps per action, per period, and lifetime;
- prompt-injection risk ceiling, required isolation mode, review threshold, output-admission policy, and candidate-update policy;
- notification policy, quiet-hours behavior, escalation path, failure threshold, and required user-visible summary;
- Calendar projection policy, audit/logging policy, retained evidence refs, and cleanup or mitigation obligations.

**Phase 1b advisory batch runner (`UBU-D0264`).** The minimum Phase 1b advisory batch runner is a narrow `BackgroundProcess` whose allowed action kind is local advisory candidate generation through the `LocalAdvisorySubmission`/`LocalAdvisoryResult` boundary. Its durable process record contains `background_process_id`, owner Identity, purpose, lifecycle status, operation kind, eligibility filter, interval or manual-start policy, run window, concurrency limit of one unless explicitly raised, per-run and per-object pass caps, retry/backoff and cancellation policy, maximum catch-up behavior, `WorkerAuthority` ref, secret-capability refs by id and version, Compartment and ContextBundle template refs, compute/rate/money/privacy caps, prompt-injection ceiling, output-admission policy, notification/escalation policy, Calendar projection policy of `none` by default, audit policy, and retained evidence refs. Dispatch rechecks lifecycle, eligibility, object versions, Compartment policy, credential version, capability grants, budgets, and prompt-injection ceiling immediately before each object attempt.

Per-object progress is canonical process state, not a single aggregate blob. A `BackgroundProcessObjectState` is keyed by background process, target object ref, operation kind, and policy or prompt/template version when that version affects eligibility. It records lifecycle status, observed object version, eligibility reason, pass count, success and failure counts, last attempted/succeeded/failed times, last result status, next eligible time, last `AgentAction`/worker-result refs, candidate refs when any, and safe diagnostic refs. Updating this state uses ordinary mutation-envelope/idempotency rules so later sync can version and replicate each object's progress independently.

Run control is event-sourced. Each run records append-only audit events such as `background_process_run_started`, `object_attempt_started`, `object_attempt_succeeded`, `object_attempt_failed`, `run_interrupted`, `run_resumed`, `run_cancelled`, and `run_completed`, with checkpoint cursor, object counts, budget use, interruption reason, actor or signal source when safe, and Log refs. Resumption reads durable object state and the last run event; in-memory cursors are only caches. A graceful interrupt records the checkpoint before exit, and retry uses the same per-object idempotency keys rather than reprocessing successful objects.

The Phase 1b advisory process creates candidate artifacts only; it does not occupy Calendar time merely because it consumes compute, credentials, or wall-clock duration. Operator surfaces show elapsed time, compute/resource use, credential/capability versions used, candidate counts, per-object pass counts, failure counts, and last-run status in process state or summaries. Calendar entries are created only for user attention, review deadlines, reminders, or escalation Tasks, not for the unattended compute itself.

A batch over operator-authored lore plus external message content is never below `medium` prompt-injection exposure. It is `high` whenever untrusted external text is mixed with private context, can influence candidate content materially, or could affect tool choice, routing, credential use, or arguments without containment. Phase 1b containment is source separation, instruction stripping or quoting for untrusted text, narrowed ContextBundles, allowlisted no-network local advisory calls, candidate-only writes, credential brokering by the controller, result schema validation, and human review for admitted state. `Critical` exposure fails closed; Phase 1b does not permit advisory batch content to drive external mutation, spending, credential disclosure, or policy expansion.

Default notification is summary-only for a clean run that creates candidates or for a clean no-op with no eligible work. The process must notify or escalate when dispatch is denied by policy or credential state, the run fails, every attempted object fails, malformed results dominate the run, budgets are exhausted, prompt-injection exposure exceeds the ceiling, or a configured no-output threshold is reached. For shutdown chaining, exit success means only that the process reached a terminal clean state: `completed`, `completed_no_eligible_work`, or `completed_no_candidate_output` with no escalation-required failures. A run that attempted eligible objects and all failed, was interrupted before checkpoint, exceeded policy, or ended with unknown result exits nonzero and records escalation; power-off automation must not treat that as successful work completion.

Prompt-injection exposure is scored before dispatch and updated after execution. The score combines source trust, whether the agent consumed untrusted webpages, messages, documents, tool outputs, or mixed-Compartment content, the amount of raw private context exposed, whether untrusted text can influence tool choice or arguments, and the authority at risk: credentials, spending, external mutation, privacy export, or user-facing commitments. The minimum qualitative bands are `low`, `medium`, `high`, and `critical`. `High` exposure requires containment such as instruction stripping, source separation, allowlisted tools, candidate-only writes, narrowed ContextBundles, or human review. `Critical` exposure fails closed unless a specific approved policy permits the run and the action remains inside hard Compartment, credential, cost, and review gates.

External side effects carry a side-effect class: `read_only`, `local_candidate_only`, `externally_reversible`, `compensating_action_possible`, `externally_irreversible`, or `unknown`. Anything outside `read_only` or `local_candidate_only` must record expected external prior state when available, idempotency keys, evidence to reconcile the result, and the review gate that admitted the effect. `Compensating_action_possible`, `externally_irreversible`, and `unknown` actions require mitigation metadata before dispatch: mitigation owner, user notification rule, cancellation or revocation path, compensating operation when one exists, external support/contact path when relevant, deadline for mitigation, and the Log or candidate records that should be written if mitigation is needed.

Background processes are Calendar items only when they reserve or require the user's attention, represent a user commitment to another party, or make the user's availability materially unavailable. Compute-only work, polling, sync, advisory analysis, candidate generation, projection reconciliation, and scheduled external checks remain separate process state. They may surface Calendar overlays, deadlines, reminders, or review Tasks, but those projections do not become user time blocks unless the user must attend, decide, travel, meet, or be unavailable.

All AgentAction and BackgroundProcess mutations remain candidate/admission workflows unless an accepted integration policy gives narrower automatic authority. Dispatch rechecks current Compartment policy, credential version, capability grant, budget, lifecycle status, and expected external version immediately before use. Logs are append-only: failed, denied, cancelled, mitigated, and superseded actions are recorded rather than rewritten.

### 21.9 State-transition cockpit

The long-term UX should present the current state transition and its evidence, constraints, expected effects, authority, privacy scope, and available actions.

Phase 1 proves this through one recommended next Task and an inspectable explanation. Later cockpit surfaces may include message triage, Log review, Plan repair, Delegation Substrate packets, agent approvals, AssociationAttestation review, organizational introspection, and projection publication.

#### 21.9.1 First-person frame and shared inspector

**Product contract (`UBU-D0351`, Phase 2).** Keep one current choice as the default surface, phrased around the user's purpose: what I am moving forward, why now, and what this choice would change. Open a focused review surface from a Task, message, review item or user request; keep the next Task and full Plan reachable without requiring a dashboard tour. Use the same summary, inspector and action area across workflows. Chat, voice and forms reach these same controls and admission paths.

Show the acting Identity, current Device/local Replica scope, permitted Compartment or `security_level: low` label, and relevant freshness or incomplete-view limits. Link to the existing sync/session indicators; opening review starts no capture. Filter the entire surface before display or model context assembly, including counts, badges, evidence selectors, labels and notifications. If existence is denied, omit the item rather than showing a revealing placeholder. A visible unavailable/redacted indicator is allowed only when policy permits it.

The compact summary states the purpose, current state, proposed change, main constraint or uncertainty, expected consequence and exact decision requested. Expand these inspection groups on demand, with decision-critical denials, uncertainty and irreversible effects visible before approval:

| Inspector group | Required presentation |
| --- | --- |
| Current and proposed state | Read-only current admitted values beside the exact proposed field/artifact diff, affected scope and versions. Omission proposes no change; unknown and private stay explicit. Distinguish a derived Plan preview from a proposal to mutate its input objects. |
| Evidence | Source links/selectors, observation and recording times, attribution, explicit assertion versus inference, per-field qualifiers/confidence when supplied, contradictions and permitted evidence gaps. Model prose, repetition and confidence confer no authority. Apply §21.1.1's corroboration rules where relevant. |
| Constraints | Relevant dependencies, preconditions, timing, availability, affect/staleness, risk findings and policy limits, with their source and whether they are hard checks or advisory findings. Show the reason a recommendation or action is unavailable. |
| Expected effects | Separate the immediate admitted change, predicted planning consequences and any external action. Label assumptions and uncertain outcomes; Task/worker completion or an accepted recommendation does not prove a modeled effect occurred. |
| Authority and disclosure | Who proposes, who reviews and who executes; applicable authority/grants and current denials; recipient, provider/route, exposed data and relevant cost/retention limits. Acceptance of a claim, Plan choice, packet preparation and external-action authorization remain separately identified decisions. |
| Recovery | What can be cancelled before dispatch, what can be corrected afterward, mitigation owner/path and any irreversible or unknown effect. Local repair appends a correction, supersession or compensating mutation; external mitigation is a separately authorized action. Never promise erasure, recall or restoration that the adapter cannot establish. |

This is a presentation contract over existing records, review events and operation bindings, not a new canonical transition object, lifecycle enum or planning-kernel input.

#### 21.9.2 Candidate and admitted state

Use explicit text and structural separation: `Current (admitted on this Device)` and `Proposed (not applied)`, with a labelled diff region. A draft or candidate can be displayed beside current state but cannot replace its values in the ordinary Task, Calendar or Objective view. Previews remain labelled through drawers, timelines, search and reports. Use redundant text, icons/borders and accessible state descriptions; color alone cannot distinguish state.

Display each record's actual lifecycle from §21.2.1 or its native contract, including deferred, rejected, disputed, superseded or revoked states. Queue order, confidence, age and an uncontested appearance never change state category. Pending/conflicted sync material is not admitted input. Admitted state is this Replica's policy-scoped view, not a claim that all Devices agree or that an accepted social claim is universal truth.

Keep saving, admission and execution outcomes separate: `Draft saved`, `Accepted update`, `Awaiting review`, `Delivery unconfirmed` and a verified external outcome appear only when the corresponding durable/result evidence exists. A CPU-certified Plan preview is still a recommendation, not a commitment or proof of execution. Show the resulting diff and linked review/Log outcome after success; on failure or uncertainty preserve permitted drafts and report the actual outcome without a success badge.

Offer Inspect, Edit/resolve, Accept the displayed change, Reject, Defer and Cancel where supported. Name consequential controls for their operation, such as `Accept Task changes`, `Use reviewed Plan` or `Approve this send`; avoid a generic approval that hides multiple effects. A permitted batch lists every included change and dependency, with nothing preselected for unrelated admission. Partial acceptance requires a coherent separately bound subset. Rejection/deferral needs no compulsory explanation and creates no inferred Preference.

Edits produce a new bound proposal revision and renewed review. Immediately before admission or dispatch, the controller rechecks candidate, target, evidence/context, dependency and relevant policy/grant versions, current authority, Device eligibility, native semantics and idempotency. Material changes invalidate approval; show the changed scope for fresh review rather than silently rebasing. Reviewing retained candidates after restart or Device handoff conveys no live capture consent or execution approval. Cancel invalidates pending approval/unsent work; already-dispatched effects follow their actual reconciliation and mitigation path.

#### 21.9.3 Core workflow surfaces

All surfaces use the shared inspector, with the workflow-specific decision below. Unsupported operations remain visibly unavailable or evidence-only; a polished screen cannot enable them.

| Surface | Focus and decision boundary |
| --- | --- |
| Next Task | One legitimate recommended Task, served Objective, why now, duration, readiness and risk; preserve §4.1.2's start/done/snooze/reject/decompose/override controls and full-Plan inspection. Starting is distinct from completion and verified effects. |
| Message reply | Incoming source and explicit/inferred metadata beside the exact reply, sending Identity/account, recipient/destination and disclosed fields. Saving or accepting a local Task/draft never authorizes send; external send uses its own released workflow and operation-bound approval. |
| Log review | Planned versus observed intervals and original entries beside proposed annotations/corrections, including unknown/private time. Accept an explicit correction or separately reviewed Snapshot/Task change; preserve original history and avoid inferred Preferences or blame. |
| Plan repair | Failed assumptions/blockers, current Plan legitimacy and candidate repair with changed ordering, placements, omissions and risks. Review input corrections separately from choosing a CPU-certified replacement Plan; no silent default switching or external dispatch. |
| Delegation packet | Show the Task brief as `For me` or the frozen packet/version, executor, output/evidence requirements, limits and failure path. Preparing/saving grants no handoff; assignment, delivery and output admission remain distinct under §21.5. |
| Agent action approval | Exact target, operation/payload, grant, route, prompt-injection exposure, cost and side-effect/recovery class. Bind authorization to that action and current versions; show attempted, confirmed, failed or unknown outcomes without treating output as admitted state. |
| AssociationAttestation review | Scoped claim, perspective, attribution, evidence/counterclaims, confidence versus authority, and disclosure policy. Accept, narrow, dispute or supersede through §19.2; acceptance grants no capability or publication authority. |
| Organizational introspection | Reviewed mission/commitment evidence versus observed behavior, dissent, uncertainty and gaps. Findings and suggested follow-up Tasks are separate candidates; approval cannot synthesize collective Preferences, settle disputed authority or publish the report. |
| Projection publication | Exact final payload/artifacts and metadata, audience/destination, managed scope, source/input versions and redactions. Preview/export review, operation approval, dispatch and verified reconciliation are distinct; §1's stale-view choice and §27.3's managed-write rules still apply. |

The Phase 2 shell extends available Task, Log, candidate, Plan, worker and projection workflows. Message/Identity coordination, full Association/governance/introspection and other richer surfaces retain their existing Phase 3+ or separately released gates. Phase 1's manual/fixture-backed outreach review remains possible without full Association automation. This decision enables no new ingestion, capture, executor, sending or publication route.

#### 21.9.4 Bounded generative presentation

Generation may supply candidate explanations, evidence summaries, clarification wording, checklists and comparisons, or suggest an arrangement of known inspector widgets. Use a versioned, closed declarative widget vocabulary with finite layout/content limits and deterministic validation. Generated content is labelled with source/model provenance and remains inspectable against the permitted underlying records; an explanation cannot invent evidence or promote a candidate.

The controller owns navigation, state labels, exact diffs, authority/denial text and approval/reject/cancel controls. Generated layouts cannot hide or reorder these required review elements, change their meaning, preselect admission, create executable controls, call tools or supply scripts/HTML. Unknown widgets, invalid references, lost qualifiers or ungrounded summaries fall back to the fixed inspector with original permitted content. Generative presentation uses the same ContextBundle, provider routing and Compartment gates as other advisory output.

Keep the fixed text/form inspector usable with models, camera, microphone and speech output disabled. Preserve permitted drafts, exact corrections and version links when changing surfaces; late generation cannot overwrite user edits. Support keyboard and screen-reader inspection and controls, predictable focus and accessible errors. Apply §21.1.2's attention envelope: defer unavailable review rather than compressing consequential approval into a generic spoken assent.

#### 21.9.5 Phase 1 minimum

The Phase 1 cockpit seed is the already frozen §4.1.2 loop: one Task with title, Objective, duration/work window, status and provenance; a compact why-now explanation covering timing, dependency/blocker, affect/staleness and top risk/opportunity; visible worker/projection state where relevant; and one-action access to the full Plan, considered alternatives/constraints and permitted evidence. Explain what each control records, distinguish recommendation from commitment and predicted effects from observed outcomes, and expose the existing review/correction and projection-preview paths. Label fixture/mock behavior explicitly.

The richer inspector, generative layout and later workflow inventory do not add a Phase 1 blocker, require a new bootstrap interview or reopen the frozen feature set. Broader cockpit implementation proceeds only for supported slices under their existing release gates.

#### 21.9.6 Acceptance evidence

Before claiming a cockpit slice delivered, pin UI/controller, schema/adapter and policy revisions and exercise these cases through the real supported review/admission paths:

- From one next Task, reach its Plan, constraints, evidence and proposed change, then return without losing context. In user review, ask what is current, what is proposed, who acts, what approval changes and what recovery is possible; report comprehension gaps and tested scope.
- Preview, edit, reject, defer and cancel admit none of the proposed world-state effects. Exact bound acceptance produces only the displayed coherent changes and linked review/Log outcomes once; duplicate submission/replay adds no effect.
- Target/evidence/policy changes, revocation, delayed model results, restart and Device handoff cannot use stale approval or overwrite corrections. Current partial/stale Replica limits stay visible without implying global agreement.
- A denied-existence item leaks no count, label, identifier, notification or generated context. An allowed evidence gap stays explicitly unavailable; confidence and repeated claims bypass no gate.
- Log repair preserves the original entry; packet preparation dispatches nothing; delivery does not admit output or complete a Task; publication preview causes no external write. Unknown external outcomes remain unknown and follow reconciliation rather than blind retry.
- Invalid/generated layouts cannot hide diffs, uncertainty, denials or action scope. Model/capture failure and keyboard/screen-reader use preserve useful fixed review, exact editing and cancellation; restricted attention defers prohibited review.
- Cancellation before dispatch prevents unsent work; after dispatch the UI reports remaining effects and the actual compensating/irreversible limits rather than claiming undo.

These are product acceptance requirements, not passing implementation evidence. Existing per-operation corroboration, host fidelity, sync, access, retention and release requirements continue to govern each slice.
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

### PLANNING_KERNEL_CONTRACT.md — referenced sections

#### PLANNING_KERNEL_CONTRACT.md §1 — Versioning and envelope rules

```markdown
## 1. Versioning and envelope rules

All request and response objects must carry explicit versions so that planner artifacts remain replayable after schema evolution.

### Required envelope fields

- `schema_version`: semantic contract version string. Phase 1 baseline: `planning-kernel-contract/0.1`; Phase 1b split-policy and partial-placement additions share `planning-kernel-contract/0.2`.
- `planner_version`: implementation version string for the planner backend that produced or consumed the object.
- `request_id`: stable identifier generated by the CPU kernel before dispatch. Enables audit, deduplication, and replay.
- `effective_time`: logical planning start time in RFC 3339 / ISO 8601 UTC form.
- `generated_at`: wall-clock generation time in RFC 3339 / ISO 8601 UTC form.

`effective_time` is the time from which the Plan should be evaluated. It may differ from `generated_at` when replaying a prior request, repairing a Plan from a recorded Log point, or testing fixtures. The planning engine treats `effective_time` as authoritative.

Phase 1b invokes the PyTorch GPU implementation through a persistent local Python worker process owned by the CPU kernel (`UBU-D0283`). Worker transport uses length-prefixed JSON frames over local pipes. The frame envelope is allowed to carry process-management fields such as frame type, cancellation, error details, and telemetry, but planning semantics are only the typed contract objects in this file. The worker has no listening socket and is not a service boundary.

Timestamps crossing this contract use RFC 3339 / ISO 8601 UTC strings. Implementations may lower them to Unix seconds, planning ticks, or tensor offsets after decoding, but those lowered coordinates are not the cross-language semantic representation.

---
```

#### PLANNING_KERNEL_CONTRACT.md §2 — `PlanningRequest`

```markdown
## 2. `PlanningRequest`

`PlanningRequest` is the complete CPU-to-planning-engine handoff object for Phase 1.

### Required fields

- `schema_version`
- `planner_version`
- `request_id`
- `effective_time`
- `generated_at`
- `mode`: enum, one of `fresh_generation`, `repair`.
- `delivery_mode`: enum, one of `interactive`, `batch`. Interactive delivery permits chunk-result stream frames; batch delivery returns only the final response.
- `rng_seed`: integer seed. Required for reproducibility and peer debugging.
- `time_window`:
  - `start_time`: RFC 3339 / ISO 8601 UTC timestamp.
  - `end_time`: RFC 3339 / ISO 8601 UTC timestamp.
  - `planning_delta_seconds`: positive integer. Phase 1 default is `60`.
- `horizon_policy`:
  - `reactive_horizon_seconds`: positive integer. Phase 1 default is `3600`.
  - `branch_coverage_target`: float in `(0, 1]`. The CPU supplies the effective value after Calendar, execution-profile, Device, and global policy resolution. It applies to the modeled probability mass of certified chunk-boundary outcome continuations within `reactive_horizon_seconds`, not to the count of K decision alternatives. Phase 1 default is `0.99`.
- `compute_budget`:
  - `max_planning_tasks`: positive integer. Phase 1 default is `256`.
  - `n_candidates`: positive integer chosen by the CPU kernel.
  - `n_rollouts`: non-negative integer chosen by the CPU kernel. Phase 1 default is `1000` per finalist.
  - `k_candidates`: positive integer. Phase 1 default is `3`.
  - `max_wall_clock_ms`: optional positive integer budget.
- `task_graph`:
  - `tasks`: ordered list of `TaskSpec` objects.
  - `dependency_edges`: list of directed edges `{before_task_id, after_task_id}`.
  - `topological_order`: CPU-provided topological task ID list. The CPU validates the DAG and the GPU engine does not perform its own topological sort.
- `universe_state_snapshot`:
  - `snapshot_id`.
  - `snapshot_effective_time`.
  - `state_refs` or compact state facts needed by the planner.
  - `payload_policy_summary`: compact CPU-generated summary of the Compartment, redaction, and disclosure policies applied during context assembly.
- `affect_profile`: required Phase 1 profile using the schema in §6.
- `scoring_policy`:
  - `utility_weight`.
  - `robustness_weight`.
  - `affect_margin_weight`.
  - `schedule_diversity_weight`.
  - omitted weights default to the CPU kernel's Phase 1 defaults. All weights must be non-negative; zero is valid to disable a scoring dimension.
- `constraint_policy`:
  - `hard_constraints_always_required`: must be `true`.
  - `affect_constraint_mode`: enum, one of `enforce`, `warn_only`. Phase 1 default is `enforce` in user-facing contexts and `warn_only` in non-user-facing test fixtures or explicit onboarding/debug contexts.
- `privacy_and_provenance`:
  - `compartment_ids`: list of compartment IDs represented in the request.
  - `redaction_level`: enum or policy label.
  - `payload_safety_proof`: CPU-generated assertion that no denied Compartment data, forbidden identity disclosure, export-banned data, or integration-prohibited data is included.
  - `payload_verified_safe`: derived boolean that must be true before dispatch; it is not a substitute for `payload_safety_proof`.

### Optional fields

- `external_event_assumptions`: list of structured stochastic or fixed external event assumptions. Defaults to empty.
- `repair_context`: required when `mode = repair`, absent otherwise.
  - `prior_plan_id`
  - `last_legitimate_plan_ref`
  - `observed_divergence_refs`
  - `repair_scope`: enum, one of `local`, `remaining_window`, `full_window`.
- `explanation_request`:
  - `include_rejection_summary`: boolean.
  - `include_score_breakdown`: boolean.
  - `include_user_facing_fragments`: boolean.
- `debug_flags`: implementation-specific diagnostic toggles. Must not change canonical semantics.

### Deferred fields

The Phase 1 contract deliberately defers mobile GPU targets, cloud GPU provider metadata, encrypted-compute metadata, cross-user coordination payloads, realtime stream state, and premium wide-horizon planning-provider negotiation. These may be added later without changing the Phase 1 CPU/GPU authority boundary.

Solver/library identity is not part of `PlanningRequest`. Optional OR-Tools, SMT/MaxSMT, local-search, mobile GPU, cloud GPU, or learned-model backends must preserve this request/response contract and remain advisory until CPU certification. Backend choice is recorded on `PlanningResponse.engine_provenance`, not requested as a semantic planner input.

### Phase 2 note: controller-resolved horizons and timing Preferences

`UBU-D0349` and DESIGN.md §15.2.3 govern controller horizon selection. The controller supplies the effective detailed scheduling interval through existing `time_window` timestamps; `horizon_policy.reactive_horizon_seconds` remains independent. Visible scope, future dependency-frontier findings and horizon-selection reasons stay in controller/Calendar metadata. No unversioned request fields or new schema version are introduced here. Existing task-count and compute limits still apply, including the default `256`-Task ceiling; horizon growth cannot silently truncate required work.

Current §5 scoring retains `UBU-D0282`'s pairwise Task-Preference value mapping and `priority = 1.0`, with no additional elapsed-time discount factor or early-preparation value boost. The current schema cannot express a placement-dependent discount curve. When an accepted timing Preference cannot be represented by ordinary admitted comparisons, the controller offers user choice and reports that automatic timing comparison is unavailable. Such scoring requires a later versioned semantic extension with CPU/reference/backend parity checks, rather than changing `TaskSpec.value`, repurposing scoring weights or sending hidden fields. This note establishes policy compatibility, not implemented Phase 2 behavior.

### Phase 3 note: Objective expansion is pre-kernel; `scoring_policy` carries trade-off weights

The Phase 3 Objective-to-Task expansion and technicalizing stage (DESIGN.md §15.2.1.1) runs on the CPU side **before** this contract is invoked. The kernel continues to receive a fully materialized `task_graph`; it does not expand Objectives, select Techniques, or consult any advisory model. This preserves the fixed `task_graph` input, the CPU-provided `topological_order`, the absence of Objectives/Techniques from `PlanningRequest`, and the kernel's determinism and no-I/O guarantee. Alternative Technique instantiations enter as separate candidates (either enumerated CPU-side and scored independently, or, if a later schema adds them, as mutually-exclusive choice-group nodes the kernel selects among deterministically — see `UBU-Q0125` subquestion 3).

Phase 1b routine instantiation (`UBU-D0286`) is also a pre-kernel CPU responsibility, but it is direct materialization rather than Technique expansion. The kernel receives only the instantiated Static or Dynamic routine Tasks, their ordinary dependency edges when any, and each Dynamic routine Task's concrete UTC `window` after recurrence and local time-of-day rules have been resolved.

The existing `scoring_policy` weights (`utility_weight`, `robustness_weight`, `affect_margin_weight`, `schedule_diversity_weight`) are the parameterization point for the user's trade-off preferences in multi-Technique outcome comparison (DESIGN.md §16.3.1). A user's revealed choice may **propose** an update to these weights for explicit user acceptance; the kernel itself neither learns nor mutates them, consistent with `UBU-D0217`.

---
```

#### PLANNING_KERNEL_CONTRACT.md §4 — `PlanningResponse`

```markdown
## 4. `PlanningResponse`

`PlanningResponse` is the planning-engine-to-CPU return object.

### Required fields

- `schema_version`
- `planner_version`
- `request_id`
- `rng_seed_echo`
- `generated_at`
- `status`: enum, one of `ok`, `partial`, `rejected`, `engine_error`. `partial` means at least one PlanCandidate is returned but optional Dynamic work is reported in `diagnostics.unplaced_tasks`.
- `engine_provenance`:
  - `backend_kind`: enum, one of `cpu_reference`, `gpu_worker`, `mobile_cpu`, `mobile_gpu`.
  - `invocation_kind`: enum, one of `in_process_cpu`, `persistent_python_worker`, `in_process_mobile`.
  - `engine_version`: implementation version string.
  - `framework`: optional free string; required as `pytorch` only for the Phase 1b desktop GPU worker. A mobile backend names its own framework or compute API.
  - `framework_version`: optional string.
  - `device_summary`: optional compact local hardware/runtime summary, for example GPU model, `cpu`, or mobile SoC/GPU such as `Snapdragon 8 Elite / Adreno 830`.
  - `tolerance_profile`: optional name of the numeric tolerance profile used for parity checks.
  - `cpu_certification_status`: enum, one of `not_yet_certified`, `certified`, `rejected_by_cpu`.
- `plan_candidates`: ranked list of `PlanCandidate` objects.
- `diagnostics`:
  - `candidate_counts_by_stage`
  - `n_skeleton_candidates`
  - `n_after_affect_filter`
  - `n_after_value_scoring`
  - `n_finalists_rollout`
  - `rejection_counts_by_reason`
  - `warnings`
  - `skeleton_failure_diagnostics`: list of `SkeletonFailureDiagnostic` objects. Empty when skeletonization and prerequisite validation do not fail; required when `status = rejected` because no skeleton baseline exists.
  - `unplaced_tasks`: list of `UnplacedTaskDiagnostic` objects. Empty when every eligible optional Dynamic Task is fully placed; required when `status = partial`. Mandatory routine occurrences and other required baseline work are not selected for omission; inability to place them remains a blocking skeleton or legitimacy diagnostic.
  - `probability_quality`: enum, one of `full`, `degraded_numeric_jitter`, `degraded_independence`, `not_estimated`.
  - `coverage_scope`: optional enum, one of `reactive_horizon`, `full_window`, `repair_scope`.
  - `coverage_estimate`: optional numeric branch-coverage estimate.
  - `uncovered_mass_estimate`: optional numeric estimate, normally `max(0, 1 - coverage_estimate)` for the same scope.
  - `coverage_threshold_used`: optional numeric threshold used for regeneration decisions.
  - `coverage_below_threshold`: optional boolean.
  - `coverage_confidence`: optional object summarizing probability estimation method, rollout count, confidence level, and lower/upper bounds for the coverage estimate.
  - `outcome_continuation_summary`: optional compact counts and digests for precomputed chunk-boundary outcome states, including covered outcome count, uncovered sampled outcome count, and whether the target was budget-limited.
  - `compute_telemetry`: optional backend timing and resource summary, including `duration_ms` when available.

### `SkeletonFailureDiagnostic`

A skeleton failure diagnostic is the bounded payload for halted or degraded skeleton generation. Minimum fields:

- `diagnostic_id`
- `severity`: enum, one of `blocking`, `warning`.
- `failure_class`: enum, one of `missing_starting_state`, `impossible_dependency`, `cyclic_dependency`, `static_task_collision`, `insufficient_calendar_window`, `unavailable_resource`, `blocked_external_event`, `unknown_precondition`.
- `primary_task_ref`: optional when the failure is Calendar-wide.
- `affected_task_refs`: list of Task refs.
- `missing_or_conflicting_state`: object with `target`, `predicate`, optional `expected`, optional `observed`, and `state_status` enum, one of `missing`, `known_false`, `conflicting`, `unresolved`, `denied_by_policy`.
- `causal_chain`: ordered list of compact fragments with Task ref, dependency or precondition ref, required state, optional producer Task ref, and status.
- `time_window_refs`: list of relevant Calendar or deadline refs.
- `static_task_conflict_refs`: list of Static Task refs for fixed-time conflicts.
- `initial_universe_state_ref`: optional UniverseState or Snapshot ref.
- `source_refs`: list of provenance or External Reference refs.
- `external_event_refs`: list of relevant External Event refs.
- `safe_alternatives`: list of alternatives, each with `action`, `label`, `requires_user_input`, `resulting_change_summary`, and optional candidate mutation or repair ref.
- `prompt_policy`: enum, one of `immediate_blocking_prompt`, `planning_warning`.
- `user_facing_summary`: short non-blaming explanation text.

Allowed `safe_alternatives.action` values are `provide_starting_state`, `mark_state_already_satisfied`, `add_prerequisite_task`, `relax_deadline`, `extend_planning_horizon`, `remove_or_moot_task`, `choose_alternate_technique_or_task_path`, `wait_for_external_event`, and `manual_decision`.

### `UnplacedTaskDiagnostic`

An unplaced Task diagnostic reports optional Dynamic work that was omitted, deferred, or only partially placed while the rest of the Plan proceeds. Minimum fields:

- `diagnostic_id`
- `task_ref`
- `reason`: enum, one of `insufficient_total_capacity`, `no_eligible_chunk_large_enough`, `outside_allowed_window`, `omitted_lower_value`, `deferred_dependency`, `unsupported_split_policy`, `horizon_extension_limit`.
- `deferred_by_task_refs`: list of Task refs that caused `deferred_dependency`, empty otherwise.
- `affected_dependent_task_refs`: list of dependent Task refs also deferred by this diagnostic.
- `eligible_chunk_refs`: list of chunks or deterministic chunk-range identifiers considered for the Task.
- `placed_piece_count`: optional integer for splittable Tasks; absent for atomic Tasks.
- `remaining_work_seconds_estimate`: optional numeric estimate for splittable carry-forward work.
- `selection_rank`: optional object recording the omission ordering inputs: `value`, optional `deadline_or_latest_finish`, and `task_id`.
- `horizon_extension`: enum, one of `not_applicable`, `skipped_by_policy`, `attempted_exhausted`.
- `safe_alternatives`: list of alternatives, each with `action`, `label`, `requires_user_input`, `resulting_change_summary`, and optional candidate mutation or repair ref. Unplaced-task action values are `decompose_task`, `reprioritize_task`, `extend_planning_horizon`, `relax_task_window`, `remove_or_moot_task`, and `manual_decision`.
- `user_facing_summary`: short non-blaming explanation text for risk reports, Plan explanation, and next-action triage.

### `PlanCandidate`

Each `PlanCandidate` must carry:

- `candidate_id`
- `rank`
- `candidate_role`: enum, one of `highest_utility`, `most_robust`, `most_schedule_diverse`, `other`.
- `schedule`: ordered list of planned Task placements. Each placement carries:
  - `task_id`
  - `piece_index`: 1-based integer; atomic placements use `1`.
  - `piece_count`: positive integer; atomic placements use `1`.
  - start/end or equivalent occupied interval fields required by the implementation profile.
- `score_summary`:
  - `utility_score`
  - `robustness_score`
  - `affect_margin_score`
  - `schedule_diversity_score`
  - `total_score`
- `feasibility_summary`:
  - `hard_constraints_assumed_satisfied_by_engine`: boolean. This is advisory only.
  - `affect_feasible`: boolean.
  - `minimum_affect_score`.
  - `violated_affect_dimensions`.
- `semi_legitimization_summary`: optional cheap prevalidation summary:
  - `result`: enum, one of `passes_cheap_checks`, `reject_obvious`, `needs_full_legitimization`.
  - `affect_budget_ok`: optional boolean.
  - `slack_preserved`: optional boolean.
  - `dependency_fragility_ok`: optional boolean.
  - `user_mode_compatible`: optional boolean.
  - `local_repair_viable`: optional boolean.
  - `legitimacy_delta_estimate`: optional numeric estimate relative to the legitimized skeleton baseline.
- `probability_summary`:
  - `display_probability`: optional scalar.
  - `log_probability`: optional numeric value for stable computation.
  - `probability_interval`: optional interval.
  - `provenance_refs`: list of stochastic-input references.
- `explanation_fragments`: optional list of user-facing explanation fragments.
- `validation_hints`: optional CPU-consumable hints. These must not be treated as certification.

The CPU kernel must validate any returned candidate before canonical Plan commit. The committed Plan records the response's `engine_provenance` together with the CPU certification result.

### `PlanningStreamFrame`

Interactive delivery is a sequence of worker frames for a single `PlanningRequest`: zero or more chunk results followed by exactly one final response. Batch delivery emits only the final response.

Common frame fields:

- `schema_version`
- `request_id`
- `frame_index`: zero-based monotonically increasing integer within the request.
- `frame_type`: enum, one of `chunk_result`, `final_response`, `engine_error`, `cancelled`.

A `chunk_result` frame carries:

- `chunk_depth`: positive integer sweep depth completed.
- `chunk_id` or deterministic chunk range identifier.
- `partial_response`: a CPU-certifiable partial `PlanningResponse` form containing only candidates and diagnostics whose placements end at or before the completed chunk depth.

Chunk-result partial responses may include outcome-continuation summaries and references for already certified boundary outcomes at or before the completed chunk depth. Later frames may increase coverage; they must not remove a continuation that the user has started executing except through the ordinary post-start pruning and repair rules.

A `final_response` frame carries exactly one complete `PlanningResponse`. `engine_error` and `cancelled` frames are transport outcomes and do not certify a Plan. The CPU may surface a streamed chunk only after CPU certification of that frame's partial response.

---

The `UBU-D0290` mobile provenance members are additive on a contract-only field;
no implementation reads `engine_provenance` yet. They introduce no version bump.
The Phase 1 baseline remains `planning-kernel-contract/0.1`, and the already
recorded Phase 1b split-policy/partial-placement version remains
`planning-kernel-contract/0.2` (`UBU-D0289`).
```

#### PLANNING_KERNEL_CONTRACT.md §5 — GPU pipeline stage boundaries

```markdown
## 5. GPU pipeline stage boundaries

The following stage-boundary contracts are design artifacts. Exact tensor dtypes, device placement, batching mechanics, and PyTorch implementation classes belong in `model-committee`, but their semantic inputs and outputs are fixed here. Chunked search is executed inside one planning invocation for the whole request; the worker may batch chunks internally to fit memory, but separate chunks are not separate semantic requests.

### Phase 1 recommended tensor profile

The Phase 1 PyTorch implementation should use padded fixed-size tensors and masks using stable implementation-facing names. This profile is recommended for tests and code generation, but exact dtype and class layout remain implementation details.

Common names:

- `task_index`: padded task-slot index within `MAX_PLANNING_TASKS`.
- `validity_mask`: boolean mask for occupied task slots or valid candidates.
- `start_time_offsets`: planned start offsets from `time_window.start_time`, in planning ticks or seconds as declared by the implementation profile.
- `piece_index`: 1-based planned piece index per placement, with `1` for atomic placements.
- `piece_count`: total planned piece count for that Task in the candidate, with `1` for atomic placements.
- `duration_samples`: sampled duration tensor.
- `feasible_mask`: boolean candidate feasibility mask after stage-local checks.
- `surviving_indices`: compacted indices of candidates that survive a filter stage.
- `feasibility_scores`: per-candidate or per-candidate/per-dimension satisfaction scores.
- `composite_scores`: stage-3 aggregate scores before rollout.
- `top_k_indices`: finalist indices selected for rollout.
- `probability_interval_low` / `probability_interval_high`: lower and upper probability interval outputs when estimable.

### Stage 1: `skeleton_sampling`

Consumes:

- CPU-provided `topological_order` and validity masks;
- dependency edge data or adjacency tensors;
- duration-model parameters;
- split-policy parameters;
- time-window and planning-delta parameters;
- candidate seeds derived from `rng_seed`.

Produces:

- candidate schedule tensor or equivalent structured schedule batch;
- `start_time_offsets`;
- `duration_samples`;
- candidate `validity_mask`;
- per-placement `piece_index` and `piece_count` metadata;
- dependency-slack summary;
- `dependency_feasibility`;
- `hard_constraint_feasibility`;
- rejection/failure code per invalid candidate.

`dependency_feasibility` is a boolean over request dependency edges whose dependent Task is placed in the candidate. It is `true` exactly when every such prerequisite is present in the candidate and finishes at or before its dependent placement starts. If no such edges exist, it is `true`; the other hard-constraint checks still apply. A candidate retaining a dependent while omitting any required prerequisite reports `dependency_feasibility = false` and `hard_constraint_feasibility = false` and is rejected as infeasible, not assigned an incomplete feasibility result (`UBU-D0340`).

Detect missing prerequisites before placement or dependency-slack lookups. Record the missing-prerequisite reason in Stage 1's existing per-candidate rejection/failure output; do not fabricate prerequisite completion or numeric slack, or abort otherwise evaluable candidates with a lookup error. Unfinished internal construction cannot be certified as a feasible Stage 1 output. Permitted optional-work omission defers affected dependents rather than retaining them without prerequisites (`UBU-D0289`).

`hard_constraint_feasibility` is the conjunction of:

- plan validity: a non-empty candidate, no Task placed twice, every placement with `start < end`, and every dependency present in the candidate and earlier in its declared placement order;
- dependency feasibility by time, as reported by `dependency_feasibility`;
- containment: every placement inside the plan window, inside its Task window when declared, and starting exactly at its Static anchor when declared;
- pairwise disjointness: no two placements overlap in time. Overlap means `start < other.end && end > other.start`; touching endpoints are allowed.

Shared CPU/GPU goldens must cover a retained dependent with an absent prerequisite (both feasibility fields `false`, with a missing-prerequisite rejection reason), one of multiple prerequisites absent, prerequisite completion exactly at dependent start (dependency feasibility `true`), prerequisite completion after dependent start (dependency feasibility `false`), dependency-free work, and valid independent work with an entire optional prerequisite/dependent chain omitted (dependency feasibility `true`). Compare both boolean fields and rejection classes exactly, and include a mixed valid/invalid batch to verify that a missing prerequisite does not abort other candidates.

`feasible_mask` is the Stage 2 name, not Stage 1's `hard_constraint_feasibility`. A field named in an exact-parity clause must be enumerated at the stage that produces it (`UBU-D0301`).

### Stage 2: `affect_legitimacy_filter`

Consumes:

- stage-1 candidate schedules;
- candidate validity masks;
- AffectProfile tensors;
- predicted or declared affect state needed by the Phase 1 filter.

Produces:

- per-candidate per-dimension affect satisfaction scores in `[0, 1]`;
- per-candidate aggregate minimum affect score;
- affect-margin score;
- `feasible_mask`;
- `surviving_indices`;
- violated-dimension codes.

This stage implements only the sigmoid affect-constraint portion of legitimization. Full legitimization remains broader than this GPU stage.

### Stage 3: `value_scoring`

Consumes:

- surviving candidate schedules;
- Task value/priority metadata;
- scoring policy weights;
- affect-margin outputs;
- schedule-diversity policy.

Phase 1b `TaskSpec.value` is a CPU-computed float in `[0.1, 1.0]` derived from admitted explicit Task prioritization for the request. Given dense priority buckets ordered best to worst, bucket position `p`, and bucket count `m`, value is `1.0` when `m = 1` and otherwise `1.0 - 0.9 * p / (m - 1)`. Tied Tasks share `p`; unranked eligible Tasks use `0.1`. In Phase 1b the CPU sends `TaskSpec.priority` as `1.0`, because Stage 3 utility multiplies value by priority; the normalized priority rank stays in the orchestrator's explanation and diagnostic records (`UBU-D0282`). Mandatory routine occurrences are outside this range: they are not layered and are sent with `TaskSpec.value = 0.0` (`UBU-D0288`).

The canonical persisted input is the set of admitted pairwise Task Preferences, layered into dense buckets at request time (`UBU-D0009`, `UBU-D0282`). The bounded value and normalized priority are transient request metadata and must not be persisted as utility. Routine instances enter Stage 3 as mandatory TaskSpecs after recurrence instantiation: they carry no priority, must be placed, and are never selected for omission (`UBU-D0288`). Objective labels, deadlines, missed occurrences, and cadence do not rewrite value/priority unless admitted prioritization changes the underlying rank.

Produces:

- utility score;
- approximate robustness score before finalist rollout;
- affect-margin score;
- schedule-diversity score;
- `composite_scores`;
- `top_k_indices` or finalist selection set for Monte Carlo rollout.

### Stage 4: `monte_carlo_rollout`

Consumes:

- top-K or top-M finalist candidates;
- shifted-log-normal duration parameters;
- correlation matrix from §7;
- external event assumptions;
- rollout count and rollout seed stream.

Produces:

- feasibility frequency by candidate;
- outcome distribution summaries;
- worst-case or lower-percentile robustness estimate;
- probability summaries and intervals when estimable, including `probability_interval_low` and `probability_interval_high` where supported;
- rollout diagnostics and degradation warnings.

Stage 4 uses deterministic rollout seed derivation from the request seed. Phase 1 may use `rng_seed + 3` as the stage-4 rollout seed stream convention until named substreams are introduced. GPU floating-point rollout summaries are reproducible within the recorded tolerance profile, not bitwise across hardware, drivers, PyTorch versions, or reduction orders.

For splittable Tasks, Stage 4 reuses the single sampled total work duration for every piece of that Task in a rollout and accounts for deterministic resume overhead before each resumed piece. A rollout remains feasible when unfinished sampled work can continue in a later planned or repair-eligible piece inside the Task window and `max_pieces`; it is infeasible when remaining work, overhead, fixed placements, dependency state, or the allowed range prevent completion under the candidate's policy. Partial scheduled-work value is proportional to completed sampled work for splittable Tasks, while effects and outgoing dependencies require completion of the last piece.

### CPU/GPU parity expectations

Parity tests compare the GPU worker with the CPU reference path and CPU-only goldens. Schema decoding, chunk partitioning, task-slot `validity_mask`, dependency feasibility, hard-constraint feasibility, rejection classes, and CPU-certified selected Plan validity must match exactly. Floating-point scores, rollout frequencies, probability intervals, and schedule-diversity scores match by documented absolute/relative tolerances or statistical acceptance tests tied to rollout count and seed. CPU certification is the final authority for any selected Plan.

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
- When editing `OPEN_QUESTIONS.md`, anchor hunks with the selected question heading `## UBU-Q0160: Which mobile compute API should the mobile GPU backend target?` and its own `### Resolution` section. Do not use a repeated `### Resolution` heading from an earlier or later question as the edit location.
- If resolving the selected question, replace the `Unresolved.` text under that selected question's `### Resolution` section and update that same question's metadata line. Do not insert selected-question resolution text into any other question block.
- Use the existing `UBU-Qxxxx` and `UBU-Dxxxx` numbering conventions.
- Prefer minimal, auditable changesets.
- If the selected question is blocked, propose decomposition only if it produces replacement questions with fewer, simpler, or no dependencies.
- If the selected question is already partially resolved, narrow or clarify it rather than pretending it is fully unresolved.

Return only JSON.
