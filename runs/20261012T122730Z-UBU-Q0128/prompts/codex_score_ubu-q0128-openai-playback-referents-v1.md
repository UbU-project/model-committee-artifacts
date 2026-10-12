# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0128`  
Question title: `Embodied Audio Interaction`  
Base commit: `47c766e5ffea71c99f177a80f55df1488d026659`

```markdown
## UBU-Q0128: Embodied Audio Interaction

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: Commute Mode, restricted-attention interaction, audio-only task capture, audio-safe messaging, media-state routing, exercise/rest/walking interaction modes Resolved by: None Last scored: 2026-10-12 Scored from commit: None

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

**Restricted-attention default (`UBU-D0309`).** DESIGN.md §21.1.2 specifies a controller-owned `InteractionBudget`, activity/Compartment separation, candidate capture and scoped media transport, high-risk deferral, interruption invalidation and fresh review after mode/Device changes. Model confidence or spoken confirmation cannot expand these permissions. Quantitative mode profiles, external-action confirmation grammar, richer integrations and naming remain open.

**Initial typed interpretation (`UBU-D0310`).** DESIGN.md §21.1.2 defines `AudioInterpretationV1` for the existing capture/control envelope: closed action shapes, evidence/context aliases, advisory confidence/load/risk/confirmation fields, semantic consistency checks and controller-owned request bindings. Stale, malformed or duplicate model results cannot authorize speech or repeated media transport. This answers the initial-envelope portion of subquestion 6; richer interpretation shapes remain open.

**Serialized budget (`UBU-D0311`).** DESIGN.md §21.1.2 defines the closed controller-issued `InteractionBudgetV1`, including versioned context/authority bindings, declared-or-unknown readiness, channel/attention bounds, finite byte/time/choice caps and fixed review/deferral policies. Semantic checks, monotonic deadlines and turn-owned counters prevent stale snapshots or budget renewal from restoring authority. This answers the initial-envelope serialization portion of subquestions 2 and 16; numerical profile values and richer envelopes remain open.

**Naming (absorbs delta §17.2).** Embodied Audio Interaction remains the recorded name for the restricted-attention audio subset. No name for the broader camera-and-voice surface has been selected; neither working proposal is adopted. Should this wider interaction model receive a distinct canonical name, or should the existing label remain only the narrower mode? Keep this as an operator naming choice, not a new decision record.

Embodied Audio Interaction is the recorded name for the restricted-attention audio subset. It generalizes the earlier Commute Mode concept beyond driving to any situation where audio interaction is available while hands, visuals, attention, or intellectual bandwidth are constrained. Driving and bicycling are the most safety-critical cases, but walking, running, gym use, physical resting, cooking, housework, and similar contexts share the same architectural pattern. The broader primary camera-and-voice surface is MVP release scope; its naming remains open above.

UbU should not merely provide “ChatGPT in the car” or generic voice chat. UbU should remain the harness, context controller, policy layer, and action authority. STT, TTS, MCP, LLMs, media apps, and device integrations should be subordinate components. UbU should decide what context is exposed, what actions are allowed, how much complexity the user can handle, when confirmation is required, and when work should be deferred.

The feature should distinguish agency audio, media-control audio, and generative-experience audio. UbU may allow complex processes in restricted-attention contexts only by decomposing them into bounded, audio-safe primitives such as capture, triage, short drafting, readback, review-queue creation, and low-risk execution. UbU should be conservative with external messages, irreversible actions, financial/legal commitments, and any action requiring visual review.

Affect and readiness remain user-declared or unknown. UbU may use those declarations to offer silence, rest or user-selected media, without diagnosing reduced intelligence or treating available audio time as an obligation to work. Original Narration remains the separate question `UBU-Q0129`.

### Resolution

Architecture and MVP-release priority are answered by `UBU-D0292` and `UBU-D0293`; the former Phase 3 release gate is superseded. `UBU-D0309` answers the initial restricted-attention permission envelope, controller-owned budget, high-risk deferral and interruption/review boundary. `UBU-D0310` adds the exact initial-envelope LLM-output schema, semantic checks and request/freshness binding. `UBU-D0311` specifies the exact initial-envelope budget serialization, units, revision/freshness checks and turn-owned limits. This question remains Open for quantitative mode profiles, richer LLM-output extensions, detailed external-action confirmation grammar, richer messaging/media/coaching integrations, device-specific behavior and the naming choice. The capture delivery sequence is `UBU-Q0183`; related evidence, latency, retention and hardware questions remain `UBU-Q0164`–`UBU-Q0182`. No generic voice confirmation authorizes a consequential external action.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0128-openai-playback-referents-v1",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0128",
    "base_commit": "47c766e5ffea71c99f177a80f55df1488d026659",
    "summary": "Define deterministic playback-target selection and vague-command routing within the existing restricted-attention audio envelope.",
    "rationale": "The v1 contracts require a fresh unambiguous playback target but leave selection and commands such as 'pause that' and 'go back' underspecified. A controller-owned referent policy, control-revision binding and explicit deferral table reduce wrong-target actions without extending permissions. This partially answers subquestions 9, 14 and 15; UBU-Q0128 remains Open.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5106,3 +5106,15 @@ Adopt the companion `perceptual-qualifiers/1` payload requirements over existing\n Keep `ProvenanceKind` and `{kind, recorded_at}` FactProvenance unchanged. Actual assertions, instrument readings and computations from facts determine admitted kind; confidence and verification labels confer no admission or external-action authority. `UniverseTarget` remains name-only. Preservation is bounded by current disclosure/retention policy, with explicit permitted gaps after purge or partial replication.\n \n Require the revision-pinned host-path inventory and fidelity fixtures in DESIGN.md \u00a721.1.1 before enabling an adapter. `UBU-Q0166` remains open for that compatibility evidence; no actual schema gap or passing mapping is claimed. Unsupported/lossy mappings stay unresolved. A demonstrated gap requires a separate reviewed versioned schema/type/store chain change; no schema, pin or migration changes here. Corroboration sufficiency and proof of effects remain `UBU-Q0167` and `UBU-Q0172`.\n+\n+---\n+\n+## UBU-D0322: Restricted-attention media commands bind an explicit playback referent\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a721.1.2. Partially answers `UBU-Q0128` subquestions 9, 14 and 15; refines `UBU-D0309`\u2013`UBU-D0311` without widening their initial envelope.\n+\n+Resolve audio domain and target before exposing a playback alias. Explicit names require a unique permitted match; implicit media references require a fresh session-selected target without competing audio. Local pause/stop/silence stops UbU output immediately and cannot infer a second app effect. Bind integration/target, item/queue/control revision, endpoint, freshness, authority and bounded step semantics; dispatch cannot fall back to a global active-player command.\n+\n+Adopt the vague-command routing table and revision-pinned acceptance cases in DESIGN.md \u00a721.1.2. Unresolved references and unsupported readback, save, summary or reminder effects remain evidence/review items. Clarification cannot reset a turn or survive interruption. Media apps retain playback ownership; no automatic resume, content retrieval or transport replay follows an interruption or unknown outcome.\n+\n+This specifies behavior, not an implemented adapter or validated driving safety. `UBU-Q0128` remains Open for quantitative profiles, richer interpretation/action shapes, external-action confirmation grammar, messaging/media/coaching integration, device-specific behavior and naming. `UBU-Q0183` retains the release sequence.\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -3413,6 +3413,26 @@ Driving, bicycling and uncertain high-risk contexts additionally prohibit genera\n \n **Controller boundary.** Consented local STT yields evidence, then the harness assembles a purpose-limited `ContextBundle` under \u00a721.3 and requests an interpretation/candidate through \u00a721.2. Include only the permitted current referents, relevant admitted state and scoped playback/context versions needed for that operation. Model confidence, risk estimates and confirmation suggestions are advisory. Deterministic validation checks schema, semantics, current budget, Compartment route, grants, object versions, ambiguity and idempotency before invoking an allowlisted control or creating a review candidate. Recheck immediately before dispatch and speech output; a local processing route alone does not authorize reading private content aloud on a shared audio endpoint. Raw AV stays local under \u00a712.2; an unavailable local backend never creates cloud-fallback authority.\n \n+**Playback referents and vague commands (`UBU-D0322`).** For the existing transport envelope, resolve the audio domain and target before offering a playback alias. This is controller-owned request context, not a new canonical object or an expansion of `AudioInterpretationV1`. An explicit target must resolve uniquely within the permitted integration scope. An implicit media reference requires one user-selected playback target in this session, current integration evidence that it remains the target, and no competing UbU speech or other permitted audio source. Foreground-app status, most-recent playback, project relevance and model confidence cannot select a target. Clear implicit referents on interruption, material mode/Compartment or endpoint change, restart or Device handoff; retained/audit copies cannot restore them. If the available permitted evidence cannot establish uniqueness, clarify or defer without inspecting denied apps or revealing their existence.\n+\n+Bind the selected integration/target, item and queue/control revision, playing/paused state, Device/output endpoint, current grant/policy versions, observation time, finite freshness deadline and the operation's integration-declared step semantics in the existing request binding. A paused target can support explicit resume while its binding remains fresh; selecting it grants no new playback or disclosure authority. Ordinary position advancement within the same item does not change the control revision; item/queue replacement, seek, playback-state change, endpoint switch or loss of reliable state does. Changes before dispatch invalidate the pending interpretation. A dispatched control's confirmed transition records its outcome and invalidates other outstanding bindings; its acknowledgment still rechecks the current budget, endpoint and disclosure permission. Transport must address the bound integration and target with its expected control revision, rather than a global command to whichever player is active at dispatch. An adapter unable to enforce that binding cannot enable transport. Constrain producer branches to the routing decision below and recheck it at dispatch; a model's action label cannot supply a missing target or operation.\n+\n+| Utterance in the initial envelope | Controller routing |\n+|---|---|\n+| Recognized local pause/stop/silence, or `pause that` while UbU is speaking | Take the immediate local session-control path and stop UbU output. Do not pause another app as an inferred second effect. Consented speech input or local controls remain required. |\n+| `pause that` with UbU silent and exactly one fresh selected media referent | Propose `media_transport/pause` for that alias, subject to the existing budget and dispatch checks. With an unknown or competing referent, use the one permitted target clarification or defer. |\n+| Explicit `pause/resume/skip/rewind [target]` | Resolve the named target uniquely and bind that request; do not change the session's default target implicitly. Skip/rewind permit only the adapter's declared single bounded step. Unsupported quantities or semantics defer. |\n+| `go back` | Keep unresolved: a prior track, a seek, UbU readback and UI navigation have different effects. A current player alone does not choose the operation. The v1 clarification vocabulary cannot clarify this operation; defer for ordinary review or a fresh explicit command. |\n+| `save this` | Retain only the authorized utterance as unresolved capture evidence. Do not bookmark/download media or treat a deictic reference as captured content. A self-contained dictated note may use the existing evidence-bound capture action. |\n+| `what was that?` or `what did I miss?` | Defer readback, replay and summarization requests; v1 has no such action. Do not substitute a rewind or speak generated content. |\n+| `remind me about this` | Retain the authorized utterance as unresolved evidence for note/Task review, with its missing referent visible. Do not admit a Task, schedule a reminder or fetch content to fill the gap. |\n+\n+Target clarification uses only `identify_playback_target` and the remaining turn budget; no spoken menu or generic `yes` selects an app. Its answer must uniquely identify a permitted target. Bind the original command and clarification evidence within the same uninterrupted turn, retaining its original counters/deadline; an interruption cannot resume that exchange. A new explicit request needs fresh evidence and bindings. Partial/uncertain STT or mixed supported/unsupported requests cannot yield media dispatch; retain authorized unresolved evidence. Immediate local pause/stop/silence remains available.\n+\n+Media apps retain playback/position ownership. UbU records only policy-permitted target/control metadata and confirmed outcomes needed for routing and audit; transport grants do not permit transcript, catalog or library retrieval. Do not issue app transport automatically when UbU speaks, is interrupted or reconnects. An unknown dispatch outcome is reconciled through permitted state reads and audit metadata, never by replaying the transport or claiming success from silence. State reconciliation alone does not prove a relative command succeeded. A subsequent effect requires a fresh user request.\n+\n+Before enabling this routing slice, pin the adapter/controller and policy revisions and exercise: two players or no reliable target; UbU speech overlapping media; an explicitly named app differing from the selected default; denied-source metadata; paused-target resume; ordinary position advancement versus item/queue/seek changes between interpretation and dispatch; stale/offline state and endpoint/Device handoff; ambiguous `go back`, `save this` and reminder references; and duplicate or unknown skip/rewind outcomes. Require no wrong-target, inferred second or repeated effect, no denied-context leakage, no canonical planning-state change without ordinary admission and no false success acknowledgment. These are acceptance requirements, not passing results. This answers initial-envelope referent routing in `UBU-Q0128` subquestions 9, 14 and 15; richer media operations, numerical profiles and device-specific integration remain open.\n+\n **Initial typed interpretation (`UBU-D0310`).** `AudioInterpretationV1` is the closed JSON result for the initial envelope only. All keys are required; `null` or `unknown` represents an unavailable estimate. References are opaque aliases supplied in this request, not model-authored object identifiers. This is the schema template; the controller instantiates a conservative producer grammar under \u00a721.2. Matching it does not establish permission. Apply the budget's finite response-size cap before parsing one complete JSON object; reject duplicate keys, non-finite numbers and trailing content.\n \n ```json\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -2138,60 +2138,62 @@\n ## UBU-Q0128: Embodied Audio Interaction\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: Commute Mode, restricted-attention interaction, audio-only task capture, audio-safe messaging, media-state routing, exercise/rest/walking interaction modes Resolved by: None Last scored: 2026-10-12 Scored from commit: None\n \n Depends on unregistered areas: Affect model, Compartment policy, audio I/O policy,\n MCP/tool harness policy, device context model.\n \n ### Question\n \n UbU may support Embodied Audio Interaction: audio-first or audio-only interaction for situations where hands, visuals, or intellectual bandwidth are restricted, including driving, bicycling, walking, running, jogging, stationary exercise, physical resting, cooking, housework, and other embodied contexts. What is the interaction model, policy envelope, context-routing architecture, and safety boundary for using UbU as a harness over STT, TTS, MCP-scoped LLM calls, media apps, and structured UbU state during these restricted-attention contexts?\n \n ### Subquestions\n \n 1. What activity contexts should Embodied Audio Interaction explicitly model: driving, bicycling, walking, running, jogging, stationary exercise, resting, cooking, housework, showering, transit, or user-defined modes?\n 2. What input and output constraints should be represented for each mode: hands availability, visual availability, locomotion risk, interruptability, working-memory budget, intellectual bandwidth, emotional bandwidth, maximum prompt length, maximum response length, and maximum number of choices?\n 3. How should UbU distinguish interaction mode from Compartment: for example, `Compartment: UbU Project / Public Outreach` and `Mode: Driving`, rather than making driving itself a Compartment?\n 4. What deterministic policy layer should sit between audio input, MCP/tool access, LLM interpretation, UbU state, and external actions?\n 5. How should UbU package scoped context for an LLM during audio-only use so that the model receives enough context for high performance without overexposing private, irrelevant, or unsafe data?\n 6. What structured JSON contract should audio-interaction LLM calls return: intent, entities, confidence, ambiguity, cognitive-load estimate, action risk, proposed action, required confirmation, defer reason, and safe-for-mode flag?\n 7. What actions are safe in each embodied mode: idea capture, task creation, message drafting, calendar review, media control, lightweight reflection, exercise coaching, original narration, or external sends?\n 8. What actions should be prohibited or deferred in high-risk modes such as driving or bicycling: high-stakes messages, irreversible deletion, financial actions, legal commitments, major scheduling changes, multi-party communication, or visually dependent review?\n 9. How should UbU handle speech recognition uncertainty, road noise, breath noise, interruptions, partial utterances, ambiguous references, and false continuity?\n 10. What confirmation grammar should UbU require for external actions in restricted-attention contexts, especially where a generic \u201cyes\u201d is too ambiguous?\n 11. How should UbU use affect information to decide whether the user is ready for intellectual interaction, should only capture ideas, should defer complex work, or should receive non-intellectual audio instead?\n 12. How should UbU detect and model user understandability during embodied contexts, including reduced recall, reduced intelligence-in-the-moment, lower working memory, or increased interruption probability?\n 13. How should UbU support audio-safe messaging: relationship-aware drafting, concise readback, wrong-recipient prevention, tone checking, review queues, and defer-until-parked policies?\n 14. How should UbU integrate with media apps without replacing them: music playback, podcast resume state, audiobook state, skip/rewind, queue, save, summarize, and \u201cwhat did I miss?\u201d interactions?\n 15. How should UbU interpret vague audio commands such as \u201cgo back,\u201d \u201cpause that,\u201d \u201csave this,\u201d \u201cwhat was that?\u201d, or \u201cremind me about this\u201d based on current activity, media state, recent interruptions, and project context?\n 16. Should UbU expose an `InteractionBudget` object, and if so, what fields should it include for input channels, output channels, visual attention, manual control, locomotion risk, intellectual bandwidth, emotional bandwidth, social risk tolerance, max turn length, action permissions, and deferral policy?\n 17. How should Embodied Audio Interaction interact with local-first architecture, cross-device sync, car audio systems, headphones, watches, phones, and future wearable devices?\n 18. What audit trail should UbU preserve for audio interactions, especially when external actions were proposed, confirmed, deferred, or rejected?\n 19. How should UbU avoid becoming an attention-extraction system or productivity grinder, and instead act as an attention steward that sometimes recommends silence, music, rest, or non-intellectual content?\n 20. Given the primary camera-and-voice MVP release commitment in `UBU-D0292`, which restricted-attention primitives must be delivered first, and which richer media/narration integrations remain later work? Phase 3 is no longer a gate on the primary surface.\n \n ### Current direction\n \n **Architectural answer and MVP scope (`UBU-D0292`, `UBU-D0293`).** The primary surface is local camera-and-voice perception: observation \u2192 reviewable candidate state \u2192 resolution against permitted persistent context \u2192 consequential clarification \u2192 CPU semantic/policy/conflict validation \u2192 operator admission \u2192 planning from admitted state. Model-time evidence never substitutes for planner-time semantics. Voice is ordinary interaction; forms and text remain available for precision, context, preference and accessibility. Capture is mode-bound, visible, source-scoped and pausable; continuous AV requires the existing explicit mode, Compartment review, routing disclosure and user approval. Raw-media minimization/deletion is distinct from append-only correction and evidence metadata. Intent and affect remain user-declared. These answers refine the already-licensed mechanism, rather than making a model the planner.\n \n **Restricted-attention default (`UBU-D0309`).** DESIGN.md \u00a721.1.2 specifies a controller-owned `InteractionBudget`, activity/Compartment separation, candidate capture and scoped media transport, high-risk deferral, interruption invalidation and fresh review after mode/Device changes. Model confidence or spoken confirmation cannot expand these permissions. Quantitative mode profiles, external-action confirmation grammar, richer integrations and naming remain open.\n \n **Initial typed interpretation (`UBU-D0310`).** DESIGN.md \u00a721.1.2 defines `AudioInterpretationV1` for the existing capture/control envelope: closed action shapes, evidence/context aliases, advisory confidence/load/risk/confirmation fields, semantic consistency checks and controller-owned request bindings. Stale, malformed or duplicate model results cannot authorize speech or repeated media transport. This answers the initial-envelope portion of subquestion 6; richer interpretation shapes remain open.\n \n **Serialized budget (`UBU-D0311`).** DESIGN.md \u00a721.1.2 defines the closed controller-issued `InteractionBudgetV1`, including versioned context/authority bindings, declared-or-unknown readiness, channel/attention bounds, finite byte/time/choice caps and fixed review/deferral policies. Semantic checks, monotonic deadlines and turn-owned counters prevent stale snapshots or budget renewal from restoring authority. This answers the initial-envelope serialization portion of subquestions 2 and 16; numerical profile values and richer envelopes remain open.\n \n+**Playback referents (`UBU-D0322`).** DESIGN.md \u00a721.1.2 specifies explicit versus session-selected media targets, separation from UbU speech, version-bound dispatch, a vague-command routing table and required stale/replay fixtures. This answers the initial-envelope referent-routing portion of subquestions 9, 14 and 15 without enabling richer media operations. Numerical profiles, richer integrations, external-action confirmation grammar and naming remain open.\n+\n **Naming (absorbs delta \u00a717.2).** Embodied Audio Interaction remains the recorded name for the restricted-attention audio subset. No name for the broader camera-and-voice surface has been selected; neither working proposal is adopted. Should this wider interaction model receive a distinct canonical name, or should the existing label remain only the narrower mode? Keep this as an operator naming choice, not a new decision record.\n \n Embodied Audio Interaction is the recorded name for the restricted-attention audio subset. It generalizes the earlier Commute Mode concept beyond driving to any situation where audio interaction is available while hands, visuals, attention, or intellectual bandwidth are constrained. Driving and bicycling are the most safety-critical cases, but walking, running, gym use, physical resting, cooking, housework, and similar contexts share the same architectural pattern. The broader primary camera-and-voice surface is MVP release scope; its naming remains open above.\n \n UbU should not merely provide \u201cChatGPT in the car\u201d or generic voice chat. UbU should remain the harness, context controller, policy layer, and action authority. STT, TTS, MCP, LLMs, media apps, and device integrations should be subordinate components. UbU should decide what context is exposed, what actions are allowed, how much complexity the user can handle, when confirmation is required, and when work should be deferred.\n \n The feature should distinguish agency audio, media-control audio, and generative-experience audio. UbU may allow complex processes in restricted-attention contexts only by decomposing them into bounded, audio-safe primitives such as capture, triage, short drafting, readback, review-queue creation, and low-risk execution. UbU should be conservative with external messages, irreversible actions, financial/legal commitments, and any action requiring visual review.\n \n Affect and readiness remain user-declared or unknown. UbU may use those declarations to offer silence, rest or user-selected media, without diagnosing reduced intelligence or treating available audio time as an obligation to work. Original Narration remains the separate question `UBU-Q0129`.\n \n ### Resolution\n \n-Architecture and MVP-release priority are answered by `UBU-D0292` and `UBU-D0293`; the former Phase 3 release gate is superseded. `UBU-D0309` answers the initial restricted-attention permission envelope, controller-owned budget, high-risk deferral and interruption/review boundary. `UBU-D0310` adds the exact initial-envelope LLM-output schema, semantic checks and request/freshness binding. `UBU-D0311` specifies the exact initial-envelope budget serialization, units, revision/freshness checks and turn-owned limits. This question remains Open for quantitative mode profiles, richer LLM-output extensions, detailed external-action confirmation grammar, richer messaging/media/coaching integrations, device-specific behavior and the naming choice. The capture delivery sequence is `UBU-Q0183`; related evidence, latency, retention and hardware questions remain `UBU-Q0164`\u2013`UBU-Q0182`. No generic voice confirmation authorizes a consequential external action.\n+Architecture and MVP-release priority are answered by `UBU-D0292` and `UBU-D0293`; the former Phase 3 release gate is superseded. `UBU-D0309` answers the initial restricted-attention permission envelope, controller-owned budget, high-risk deferral and interruption/review boundary. `UBU-D0310` adds the exact initial-envelope LLM-output schema, semantic checks and request/freshness binding. `UBU-D0311` specifies the exact initial-envelope budget serialization, units, revision/freshness checks and turn-owned limits. `UBU-D0322` adds initial-envelope audio-domain/playback referent selection, vague-command routing, control-revision checks and adapter acceptance requirements; it enables no richer action. This question remains Open for quantitative mode profiles, richer LLM-output extensions, detailed external-action confirmation grammar, richer messaging/media/coaching integrations, device-specific behavior and the naming choice. The capture delivery sequence is `UBU-Q0183`; related evidence, latency, retention and hardware questions remain `UBU-Q0164`\u2013`UBU-Q0182`. No generic voice confirmation authorizes a consequential external action.\n \n ---\n \n",
    "commit_message": "docs: bind restricted-attention media commands to explicit referents",
    "validation_notes": [
      "Generated from current working-tree contents with git diff --no-index; normalized paths and limited OPEN_QUESTIONS.md context to the UBU-Q0128 block.",
      "git apply --check --whitespace=error passed against the current working tree.",
      "Validated the closed JSON field set, field types, ID patterns and patch file allowlist.",
      "Preserved every OPEN_QUESTIONS.md metadata line and all content outside UBU-Q0128; no repository files were written.",
      "UBU-D0322 was unused; the selected question remains Open with Resolved by: None.",
      "Adapter fixtures are proposed acceptance requirements; no implementation, hardware validation or driving-safety result is claimed."
    ],
    "new_questions_added": [],
    "questions_resolved": [],
    "decisions_added": [
      "UBU-D0322"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0128-openai-playback-referents-v1",
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
