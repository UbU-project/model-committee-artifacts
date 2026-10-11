# Model-Committee Work Proposal Request

You are participating in the UbU `model-committee` process.

Your task is to produce one concrete work proposal as strict JSON.

Do not return prose outside the JSON object.

## Selected question

Question ID: `UBU-Q0128`  
Question title: `Embodied Audio Interaction`  
Repository working tree: `/home/sean/ubu-design`

HEAD commit (provenance only): `b14cc51502862a9d7f36a8404fe8b5c72f51f32f`

```markdown
## UBU-Q0128: Embodied Audio Interaction

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: Commute Mode, restricted-attention interaction, audio-only task capture, audio-safe messaging, media-state routing, exercise/rest/walking interaction modes Resolved by: None Last scored: 2026-10-11 Scored from commit: None

Depends on unregistered areas: Affect model, Compartment policy, audio I/O policy,
MCP/tool harness policy, device context model.

### Question

UbU may support Embodied Audio Interaction: audio-first or audio-only interaction for situations where hands, visuals, or intellectual bandwidth are restricted, including driving, bicycling, walking, running, jogging, stationary exercise, physical resting, cooking, housework, and other embodied contexts. What is the interaction model, policy envelope, context-routing architecture, and safety boundary for using UbU as a harness over STT, TTS, MCP-scoped LLM calls, media apps, and structured UbU state during these restricted-attention contexts?

### Subquestions

1. What activity contexts should Embodied Audio Interaction explicitly model: driving, bicycling, walking, running, jogging, stationary exercise, resting, cooking, housework, showering, transit, or user-defined modes?
2. What input and output constraints should be represented for each mode: hands availability, visual availability, locomotion risk, interruptability, working-memory budget, intellectual bandwidth, emotional bandwidth, maximum prompt length, maximum response length, and maximum number of choices?
3. How should UbU distinguish interaction mode from Compartment: for example, `Compartment: UbU Project / Public Outreach` and `Mode: Driving`, rather than making driving itself a Compartment?
4. What deterministic policy layer should sit between audio input, MCP/tool access, LLM interpretation, UbU state, and external actions?
5. How should UbU package scoped context for an LLM during audio-only use so that the model receives enough context for high performance without overexposing private, irrelevant, or unsafe data?
6. What structured JSON contract should audio-interaction LLM calls return: intent, entities, confidence, ambiguity, cognitive-load estimate, action risk, proposed action, required confirmation, defer reason, and safe-for-mode flag?
7. What actions are safe in each embodied mode: idea capture, task creation, message drafting, calendar review, media control, lightweight reflection, exercise coaching, original narration, or external sends?
8. What actions should be prohibited or deferred in high-risk modes such as driving or bicycling: high-stakes messages, irreversible deletion, financial actions, legal commitments, major scheduling changes, multi-party communication, or visually dependent review?
9. How should UbU handle speech recognition uncertainty, road noise, breath noise, interruptions, partial utterances, ambiguous references, and false continuity?
10. What confirmation grammar should UbU require for external actions in restricted-attention contexts, especially where a generic “yes” is too ambiguous?
11. How should UbU use affect information to decide whether the user is ready for intellectual interaction, should only capture ideas, should defer complex work, or should receive non-intellectual audio instead?
12. How should UbU detect and model user understandability during embodied contexts, including reduced recall, reduced intelligence-in-the-moment, lower working memory, or increased interruption probability?
13. How should UbU support audio-safe messaging: relationship-aware drafting, concise readback, wrong-recipient prevention, tone checking, review queues, and defer-until-parked policies?
14. How should UbU integrate with media apps without replacing them: music playback, podcast resume state, audiobook state, skip/rewind, queue, save, summarize, and “what did I miss?” interactions?
15. How should UbU interpret vague audio commands such as “go back,” “pause that,” “save this,” “what was that?”, or “remind me about this” based on current activity, media state, recent interruptions, and project context?
16. Should UbU expose an `InteractionBudget` object, and if so, what fields should it include for input channels, output channels, visual attention, manual control, locomotion risk, intellectual bandwidth, emotional bandwidth, social risk tolerance, max turn length, action permissions, and deferral policy?
17. How should Embodied Audio Interaction interact with local-first architecture, cross-device sync, car audio systems, headphones, watches, phones, and future wearable devices?
18. What audit trail should UbU preserve for audio interactions, especially when external actions were proposed, confirmed, deferred, or rejected?
19. How should UbU avoid becoming an attention-extraction system or productivity grinder, and instead act as an attention steward that sometimes recommends silence, music, rest, or non-intellectual content?
20. Given the primary camera-and-voice MVP release commitment in `UBU-D0292`, which restricted-attention primitives must be delivered first, and which richer media/narration integrations remain later work? Phase 3 is no longer a gate on the primary surface.

### Current direction

**Architectural answer and MVP scope (`UBU-D0292`, `UBU-D0293`).** The primary surface is local camera-and-voice perception: observation → reviewable candidate state → resolution against permitted persistent context → consequential clarification → CPU semantic/policy/conflict validation → operator admission → planning from admitted state. Model-time evidence never substitutes for planner-time semantics. Voice is ordinary interaction; forms and text remain available for precision, context, preference and accessibility. Capture is mode-bound, visible, source-scoped and pausable; continuous AV requires the existing explicit mode, Compartment review, routing disclosure and user approval. Raw-media minimization/deletion is distinct from append-only correction and evidence metadata. Intent and affect remain user-declared. These answers refine the already-licensed mechanism, rather than making a model the planner.

**Restricted-attention default (`UBU-D0309`).** DESIGN.md §21.1.2 specifies a controller-owned `InteractionBudget`, activity/Compartment separation, candidate capture and scoped media transport, high-risk deferral, interruption invalidation and fresh review after mode/Device changes. Model confidence or spoken confirmation cannot expand these permissions. Tuned mode profiles, exact typed output and confirmation grammar, richer integrations and naming remain open.

**Naming (absorbs delta §17.2).** Embodied Audio Interaction remains the recorded name for the restricted-attention audio subset. No name for the broader camera-and-voice surface has been selected; neither working proposal is adopted. Should this wider interaction model receive a distinct canonical name, or should the existing label remain only the narrower mode? Keep this as an operator naming choice, not a new decision record.

Embodied Audio Interaction is the recorded name for the restricted-attention audio subset. It generalizes the earlier Commute Mode concept beyond driving to any situation where audio interaction is available while hands, visuals, attention, or intellectual bandwidth are constrained. Driving and bicycling are the most safety-critical cases, but walking, running, gym use, physical resting, cooking, housework, and similar contexts share the same architectural pattern. The broader primary camera-and-voice surface is MVP release scope; its naming remains open above.

UbU should not merely provide “ChatGPT in the car” or generic voice chat. UbU should remain the harness, context controller, policy layer, and action authority. STT, TTS, MCP, LLMs, media apps, and device integrations should be subordinate components. UbU should decide what context is exposed, what actions are allowed, how much complexity the user can handle, when confirmation is required, and when work should be deferred.

The feature should distinguish agency audio, media-control audio, and generative-experience audio. UbU may allow complex processes in restricted-attention contexts only by decomposing them into bounded, audio-safe primitives such as capture, triage, short drafting, readback, review-queue creation, and low-risk execution. UbU should be conservative with external messages, irreversible actions, financial/legal commitments, and any action requiring visual review.

Affect and readiness remain user-declared or unknown. UbU may use those declarations to offer silence, rest or user-selected media, without diagnosing reduced intelligence or treating available audio time as an obligation to work. Original Narration remains the separate question `UBU-Q0129`.

### Resolution

Architecture and MVP-release priority are answered by `UBU-D0292` and `UBU-D0293`; the former Phase 3 release gate is superseded. `UBU-D0309` answers the initial restricted-attention permission envelope, controller-owned budget, high-risk deferral and interruption/review boundary. This question remains Open for quantitative mode profiles, the exact budget and LLM-output schemas, detailed external-action confirmation grammar, richer messaging/media/coaching integrations, device-specific behavior and the naming choice. The capture delivery sequence is `UBU-Q0183`; related evidence, latency, retention and hardware questions remain `UBU-Q0164`–`UBU-Q0182`. No generic voice confirmation authorizes a consequential external action.

---

```

## Patch baseline

The patch must apply to the **current working-tree files** in the repository above.
The selected question and context excerpts come from that working tree, which may
include uncommitted ranking and metadata updates.

- Set the JSON `base_commit` field to `b14cc51502862a9d7f36a8404fe8b5c72f51f32f` for provenance. It does not
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
- next decision id: `UBU-D0310`

`DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md` is read-only context and is not in the patch
allowlist: do not propose changes to it.

### DECISIONS.md — cited decisions

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

#### UBU-D0309

```markdown
## UBU-D0309: Restricted-attention audio starts with a controller-enforced capture envelope

**Status:** Accepted → DESIGN.md §21.1.2. Partially answers `UBU-Q0128`; refines `UBU-D0292` without changing its MVP-release commitment or `UBU-D0293`'s provenance mapping.

Keep activity mode separate from Compartment and realtime source/routing mode. A session-local `InteractionBudget` bounds channels, attention, turn complexity, operation classes and deferral; UbU computes permissions from current policy and grants, rather than accepting a model's safety or confidence judgment. Intent, capacity and affect remain user-declared or unknown.

The initial restricted-attention profile permits user-initiated candidate capture, bounded acknowledgments, pause/stop/silence and explicitly scoped media transport. Driving, bicycling and uncertain high-risk activity defer consequential actions and prohibit drafting/coaching dialogue and visually dependent review. Spoken confirmation cannot override that restriction. Interruption and material context changes invalidate pending approval; returning to ordinary review never executes deferred work automatically.

Reuse existing evidence, candidate, ContextBundle, admission and append-only audit boundaries. Device handoff and offline failure cannot expand capture, disclosure or action authority. This specifies behavior, not implementation. Quantitative profiles, exact audio-output/confirmation schemas, richer integrations and broader-surface naming remain in `UBU-Q0128`; `UBU-Q0183` retains the release sequence.
```

### DESIGN.md — referenced sections

#### DESIGN.md §2.13 — Realtime and agentic AI boundary

```markdown
### 2.13 Realtime and agentic AI boundary

Local camera-and-voice perception is the primary interaction surface and part of the MVP release (`UBU-D0292`, amending `UBU-D0132`). Realtime multimodal models remain replaceable perceptual/extraction backends: they may listen, speak, watch, transcribe, translate, detect interruptions, monitor Task progress, assist meeting capture, or generate candidate UI, but they do not become UbU's authoritative planner. Text and forms remain available, and primary interaction never implies continuous-capture consent.

UbU distinguishes **model-time awareness** from **planner-time semantics**. A model may notice elapsed time, silence, overlapping speech, or changed conditions; UbU decides whether those observations update a Task, Log, Snapshot, Calendar, Objective, or recalculation trigger.

Realtime/discovery functionality must be mode-bound and visible. The user should know whether UbU is off, text-only, in a voice session, in active Discovery mode, in meeting/logging mode, local-only, or cloud-assisted.
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

**MVP-release scope amendment (`UBU-D0292`).** Local camera-and-voice interaction, including explicitly enabled realtime capture, enters the Phase 1b release backlog. This is a release commitment, not a claim that capture is implemented or a retroactive change to frozen Phase 1. `UBU-Q0183` leaves the exact capture modes, supported devices and sequence relative to the switch and existing device waves to an operator roadmap decision. `UBU-D0305` amends the Phase 1b exit criterion; release completion cannot be inferred from closing Phase 1b alone. `UBU-Q0128` is re-tagged Phase 1b, so Phase 3 no longer gates the primary interaction surface. Always-on behavior is not required. Mobile interaction versus mobile planning workload remains open in `UBU-Q0160`, without amending `UBU-D0290`.

The richer household apprentice, maintenance triage, purchasing defense, Skill evidence and tacit-expertise examples depend on first-class Technique, Resource and Skill implementations. They are phased but unimplemented dependencies: Phase 3 supplies minimal readiness and Technique-based Objective expansion, and Phase 4 supplies richer integration. Evidence semantics, hardware suitability and scenario-specific scope remain unresolved. Camera-first MVP scope does not promise that those later planning objects or scenarios are MVP-ready.

Phase 1b defers multi-device sync and everything else assigned to Phase 2 and later.
```

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

**AV refinement for the MVP interaction surface (`UBU-D0292`).** The primary camera-and-voice surface does not enable continuous sources implicitly. A consented capture session remains visible, source-scoped, pausable and inspectable under the clause above. Process raw AV locally on the operator's own hardware, minimize context and retain only material justified by its purpose. Selective clips, summaries, hashes or deletion are operator-controlled under Compartment policy; subsequent export needs its own allowed route and approval. No-cloud/no-export denials remain hard. Exact defaults, segmentation and deletion mechanics remain open in `UBU-Q0169`–`UBU-Q0171`; recording a policy does not claim its enforcement has shipped.

Selective raw-media retention and append-only evidence are distinct. Discarding a raw stream or deleting an authorized clip does not edit an admitted historical claim in place: append minimized correction, revocation, deletion or tombstone metadata with the appropriate links. The immutable Log retains evidence about what was asserted, reviewed or withdrawn, not an automatic reconstructible copy of private raw media. A hash is not proof of an event and can itself leak correlation; policy must govern it. Later views can mark deleted evidence unavailable without inventing a replacement observation. `UBU-Q0162` tracks the reported Log-editability enforcement gap.

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

Confidence helps choose high-information clarification, not bypass admission. Corroboration is consequence-sensitive: escalate evidence and human review when an interpretation could change a consequential action; unknown or incompatible evidence stays unresolved. An observation consistent with completion is not proof that the modeled effect occurred. `UBU-D0293` defines source-kind mapping; `UBU-Q0165`–`UBU-Q0167` and `UBU-Q0172` define remaining evidence-contract questions. Existing candidate/Discovery records are the reuse point, not newly implemented perception schemas.

The execution loop is **See → understand → plan → direct → observe → verify → learn → update**. Understanding proposes interpretations; planning remains the kernel's; direction presents an inspectable next action; new observations prompt reconciliation; verification uses the required evidence and operator review; learning proposes model corrections; only admitted updates close the loop. Inference never certifies itself by being repeated downstream. Technique guidance, observed Skill development and procedure-to-Technique capture depend on the later-phase objects and unresolved evidence/safety questions, not merely access to a camera. See [the interaction document](MULTIMODAL_INTERACTION.md) and [the security boundary](MULTIMODAL_SECURITY.md).

#### 21.1.2 Restricted-attention audio envelope

**Embodied Audio Interaction (`UBU-D0309`).** Activity mode is separate from the realtime session's source/routing mode and from `compartment_ids`: for example, `Mode: Driving` with `Compartment: UbU Project / Public Outreach`. Activity constrains interaction; Compartment policy constrains disclosure and authority. Neither grants permission supplied by the other.

The initial activity vocabulary covers driving, bicycling, walking, running (including jogging), stationary exercise, resting, cooking, housework, showering, passenger transit, user-defined and unknown contexts. Operator selection establishes the intended mode. Explicitly enabled coarse activity evidence may tighten restrictions, but never starts capture or relaxes them. Driving, bicycling and conflicting or uncertain high-risk activity use the high-risk envelope; stopping motion alone does not establish parked review readiness.

A controller-owned, session-local `InteractionBudget` records:

- budget id and version, session and Device refs, activity mode, source/freshness of context, applicable policy refs, `compartment_ids` and expiry;
- permitted input/output channels and audio endpoint, manual availability, visual attention, locomotion risk, interruptibility, working-memory budget, intellectual/emotional bandwidth and social-risk tolerance;
- finite maximum prompt and response lengths, turn duration, choices and clarification count, permitted operation classes, confirmation policy and deferral policy.

Capacity and affect are user-declared or unknown, never a model-certified assessment of intelligence or readiness. Unknown capacity uses the conservative profile; observations may reduce the budget, not expand permissions. Effective permissions are the intersection of the reviewed activity profile, current channel/attention limits, Compartment policy and scoped capability grants. Missing, expired or conflicting authority prevents model/tool dispatch; pause, stop and silence controls remain available. Numeric profile defaults and the exact serialized budget schema remain in `UBU-Q0128`; a profile cannot ship without finite caps.

**Initial permissions.** Every restricted-attention mode starts with user-initiated local candidate capture, a bounded acknowledgment, pause/stop/silence, and optional explicitly granted media transport (`pause`, `resume`, `skip`, `rewind`) against one fresh, unambiguous playback target. Capture produces existing evidence/candidate records, including Task proposals; it does not create an admitted Task, Snapshot, Preference or Calendar change. Media transport uses the current integration grant and policy checks; it never authorizes sharing, purchases or broader app access. Richer read-only and short-drafting profiles remain open.

Driving, bicycling and uncertain high-risk contexts additionally prohibit generated drafting, reflective/coaching dialogue, spoken menus and visually dependent review. Permit at most one brief clarification per request; an unresolved target or unanswered clarification leads to capture/deferral without retry prompts. External sends, financial actions, legal commitments, deletion, multi-party communication and scheduling changes are deferred. Neither a capability grant, a generic “yes”, an action-specific spoken confirmation nor a model's `safe_for_mode` claim overrides this envelope. UbU may remain silent or suggest user-selected existing media/rest; it must not repeatedly solicit work or queue review prompts while attention is restricted.

**Controller boundary.** Consented local STT yields evidence, then the harness assembles a purpose-limited `ContextBundle` under §21.3 and requests an interpretation/candidate through §21.2. Include only the permitted current referents, relevant admitted state and scoped playback/context versions needed for that operation. Model confidence, risk estimates and confirmation suggestions are advisory. Deterministic validation checks schema, semantics, current budget, Compartment route, grants, object versions, ambiguity and idempotency before invoking an allowlisted control or creating a review candidate. Recheck immediately before dispatch and speech output; a local processing route alone does not authorize reading private content aloud on a shared audio endpoint. Raw AV stays local under §12.2; an unavailable local backend never creates cloud-fallback authority.

**Interruption and deferral.** Partial/uncertain STT, ambiguous recipients or “this/that” references, stale media state and false conversational continuity never select an action by confidence alone. Ask at most the budgeted clarification count, then retain an unresolved candidate or defer. Interruption cancels unfinished speech and pending confirmation; it cannot complete an utterance or replay an action. A mode, target, payload, policy or Device change invalidates pending approval. Leaving a restricted mode only makes a deferred item available for ordinary review: it never dispatches it. Any later consequential action needs fresh admission under current authority and confirmation bound to the exact operation, recipient, payload/version and expiry; the detailed audio confirmation grammar remains open.

Audit records link the session, evidence/candidate, budget and policy versions, ContextBundle, resolved target/version, policy decision and reason, approval/deferral/rejection, invocation/result and idempotency key. They retain minimized summaries or permitted refs, not a mandatory recording or full transcript. Raw-media retention remains under §12.2 and `UBU-Q0170`–`UBU-Q0171`. Device handoff rechecks capture, context, output route and authority; synchronized candidates never carry live microphone consent or pending execution approval.

**Release boundary.** Before enabling a restricted-attention slice, demonstrate that interruption stops output, uncertain references do not dispatch, high-risk sends remain deferred despite confirmation, mode/Device changes do not execute queued work, and offline operation preserves local-only denials. This is a design contract, not an implementation or a claim of validated driving safety. `UBU-Q0183` retains the delivery sequence and device allocation under `UBU-D0292`; richer media/narration and profile tuning remain later answers, without restoring a Phase 3 gate.

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

Provisional fields:

- `delegation_packet_id`;
- `task_ref` or `objective_ref`;
- `purpose`;
- `executor_type` (`self`, `local_agent`, `remote_agent`, `tool`, `human_identity`, `association`, `general_contractor`, `external_provider`);
- `executor_ref` when known;
- `authority_source`;
- `authority_scope`;
- `expected_output`;
- `required_evidence`;
- `review_requirement`;
- `privacy_scope`;
- `compartment_refs`;
- `allowed_tools`;
- `timebox_or_deadline`;
- `failure_path`;
- `escalation_path`;
- `status`.

Status values may include `not_delegated`, `prepared`, `proposed`, `offered`, `accepted`, `in_progress`, `delivered`, `reviewed`, `rejected`, `completed`, and `cancelled`.

When `executor_type` is `self`, the packet acts as a self-reminder and execution aid. It does not imply external handoff.

### 21.6 General Contractor

A General Contractor coordinates multiple subordinate executors to satisfy an Objective or complete a Container of Tasks.

A General Contractor may be a human Identity, agent, or Association. The role may decompose work, assign subtasks, monitor status, request evidence, and submit candidate updates, but only within explicit authority and Compartment bounds.

Open details include subdelegation, authority propagation, budget constraints, evidence requirements, review rights, and failure/escalation semantics.

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

The first MVP is designed around **dogfooding**: using UbU to coordinate the design, development, release, and maintenance of UbU itself.

Recent LLM and agentic-AI changes reinforce UbU's core boundary: realtime, multimodal, tool-using, memory-bearing models are valuable interaction and extraction backends, but UbU remains the user-sovereign state-transition, planning, logging, privacy, and review layer.

For Phase 1, UbU must also be understandable as a first-person user experience. The minimal user-facing loop is: answer a small number of bootstrapping questions, allow UbU to construct an initial context model, receive one recommended next Task, inspect why that Task matters now, act or override, then let UbU learn from the result through Logs, Snapshots, and recalculation.

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
- When editing `OPEN_QUESTIONS.md`, anchor hunks with the selected question heading `## UBU-Q0128: Embodied Audio Interaction` and its own `### Resolution` section. Do not use a repeated `### Resolution` heading from an earlier or later question as the edit location.
- If resolving the selected question, replace the `Unresolved.` text under that selected question's `### Resolution` section and update that same question's metadata line. Do not insert selected-question resolution text into any other question block.
- Use the existing `UBU-Qxxxx` and `UBU-Dxxxx` numbering conventions.
- Prefer minimal, auditable changesets.
- If the selected question is blocked, propose decomposition only if it produces replacement questions with fewer, simpler, or no dependencies.
- If the selected question is already partially resolved, narrow or clarify it rather than pretending it is fully unresolved.

Return only JSON.
