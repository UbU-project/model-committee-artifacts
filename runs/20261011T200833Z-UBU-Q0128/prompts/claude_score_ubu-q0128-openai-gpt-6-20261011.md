# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0128`  
Question title: `Embodied Audio Interaction`  
Base commit: `ab0668ab7395276fc6ecfae4aaf86312160a23e4`

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

**Naming (absorbs delta §17.2).** Embodied Audio Interaction remains the recorded name for the restricted-attention audio subset. No name for the broader camera-and-voice surface has been selected; neither working proposal is adopted. Should this wider interaction model receive a distinct canonical name, or should the existing label remain only the narrower mode? Keep this as an operator naming choice, not a new decision record.

Embodied Audio Interaction is the recorded name for the restricted-attention audio subset. It generalizes the earlier Commute Mode concept beyond driving to any situation where audio interaction is available while hands, visuals, attention, or intellectual bandwidth are constrained. Driving and bicycling are the most safety-critical cases, but walking, running, gym use, physical resting, cooking, housework, and similar contexts share the same architectural pattern. The broader primary camera-and-voice surface is MVP release scope; its naming remains open above.

UbU should not merely provide “ChatGPT in the car” or generic voice chat. UbU should remain the harness, context controller, policy layer, and action authority. STT, TTS, MCP, LLMs, media apps, and device integrations should be subordinate components. UbU should decide what context is exposed, what actions are allowed, how much complexity the user can handle, when confirmation is required, and when work should be deferred.

The feature should distinguish agency audio, media-control audio, and generative-experience audio. UbU may allow complex processes in restricted-attention contexts only by decomposing them into bounded, audio-safe primitives such as capture, triage, short drafting, readback, review-queue creation, and low-risk execution. UbU should be conservative with external messages, irreversible actions, financial/legal commitments, and any action requiring visual review.

The affect model is central. UbU may know that the user is not in the right state for intellectual work and may instead recommend silence, music, podcasts, audiobooks, or original narration. This makes UbU an attention steward rather than a productivity maximizer.

### Resolution

Architecture and MVP-release priority are answered by `UBU-D0292` and `UBU-D0293`; the former Phase 3 release gate is superseded. This question remains Open for the naming choice and the detailed restricted-attention policies above. The capture delivery sequence is `UBU-Q0183`; related evidence, latency, retention and hardware questions are `UBU-Q0164`–`UBU-Q0182`. No generic voice confirmation authorizes a consequential external action.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0128-openai-gpt-6-20261011",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0128",
    "base_commit": "ab0668ab7395276fc6ecfae4aaf86312160a23e4",
    "summary": "Adopt a controller-enforced restricted-attention capture envelope, with session-local InteractionBudget, high-risk deferral and fresh review after interruption or context changes.",
    "rationale": "UBU-D0292 and UBU-D0293 already settle the perception architecture and release commitment. This proposal narrows the remaining policy work by defining initial permissions and deterministic enforcement without granting models planning or action authority. UBU-Q0128 remains Open for profile tuning, exact schemas, richer integrations and naming.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\nindex 0aa21ae..a6c21f3 100644\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -4901,3 +4901,15 @@ The CPU excludes deferred work and dependents with unsatisfied required dependen\n Default review interval is 30 days, surfaced at Log review or startup when due; a chosen date delays ordinary review and a material admitted Objective change can surface it earlier. Dates never reactivate work. The four actions are promote, keep deferred with a future review, dismiss permanently through the existing narrow `moot_reason_code`, and review the linked Objective while retaining deferral. Keep, skip, and Objective review do not discharge an unresolved Preference requirement; the review cycle continues without forced expiry or inferred acceptance.\n \n Entry, edits, exit, and review decisions are version-checked, idempotent, and append-only logged under \u00a79.7; terminal exits also retain their ordinary outcome Log. Attention requests are coalesced per episode and respect existing Compartment, authority, worker-admission, recalculation, and device-sync rules. This decision specifies Phase 2 behavior and makes no implementation claim.\n+\n+---\n+\n+## UBU-D0309: Restricted-attention audio starts with a controller-enforced capture envelope\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a721.1.2. Partially answers `UBU-Q0128`; refines `UBU-D0292` without changing its MVP-release commitment or `UBU-D0293`'s provenance mapping.\n+\n+Keep activity mode separate from Compartment and realtime source/routing mode. A session-local `InteractionBudget` bounds channels, attention, turn complexity, operation classes and deferral; UbU computes permissions from current policy and grants, rather than accepting a model's safety or confidence judgment. Intent, capacity and affect remain user-declared or unknown.\n+\n+The initial restricted-attention profile permits user-initiated candidate capture, bounded acknowledgments, pause/stop/silence and explicitly scoped media transport. Driving, bicycling and uncertain high-risk activity defer consequential actions and prohibit drafting/coaching dialogue and visually dependent review. Spoken confirmation cannot override that restriction. Interruption and material context changes invalidate pending approval; returning to ordinary review never executes deferred work automatically.\n+\n+Reuse existing evidence, candidate, ContextBundle, admission and append-only audit boundaries. Device handoff and offline failure cannot expand capture, disclosure or action authority. This specifies behavior, not implementation. Quantitative profiles, exact audio-output/confirmation schemas, richer integrations and broader-surface naming remain in `UBU-Q0128`; `UBU-Q0183` retains the release sequence.\ndiff --git a/DESIGN.md b/DESIGN.md\nindex 36507f0..9b6c7d0 100644\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -3180,6 +3180,32 @@ Confidence helps choose high-information clarification, not bypass admission. Co\n \n The execution loop is **See \u2192 understand \u2192 plan \u2192 direct \u2192 observe \u2192 verify \u2192 learn \u2192 update**. Understanding proposes interpretations; planning remains the kernel's; direction presents an inspectable next action; new observations prompt reconciliation; verification uses the required evidence and operator review; learning proposes model corrections; only admitted updates close the loop. Inference never certifies itself by being repeated downstream. Technique guidance, observed Skill development and procedure-to-Technique capture depend on the later-phase objects and unresolved evidence/safety questions, not merely access to a camera. See [the interaction document](MULTIMODAL_INTERACTION.md) and [the security boundary](MULTIMODAL_SECURITY.md).\n \n+#### 21.1.2 Restricted-attention audio envelope\n+\n+**Embodied Audio Interaction (`UBU-D0309`).** Activity mode is separate from the realtime session's source/routing mode and from `compartment_ids`: for example, `Mode: Driving` with `Compartment: UbU Project / Public Outreach`. Activity constrains interaction; Compartment policy constrains disclosure and authority. Neither grants permission supplied by the other.\n+\n+The initial activity vocabulary covers driving, bicycling, walking, running (including jogging), stationary exercise, resting, cooking, housework, showering, passenger transit, user-defined and unknown contexts. Operator selection establishes the intended mode. Explicitly enabled coarse activity evidence may tighten restrictions, but never starts capture or relaxes them. Driving, bicycling and conflicting or uncertain high-risk activity use the high-risk envelope; stopping motion alone does not establish parked review readiness.\n+\n+A controller-owned, session-local `InteractionBudget` records:\n+\n+- budget id and version, session and Device refs, activity mode, source/freshness of context, applicable policy refs, `compartment_ids` and expiry;\n+- permitted input/output channels and audio endpoint, manual availability, visual attention, locomotion risk, interruptibility, working-memory budget, intellectual/emotional bandwidth and social-risk tolerance;\n+- finite maximum prompt and response lengths, turn duration, choices and clarification count, permitted operation classes, confirmation policy and deferral policy.\n+\n+Capacity and affect are user-declared or unknown, never a model-certified assessment of intelligence or readiness. Unknown capacity uses the conservative profile; observations may reduce the budget, not expand permissions. Effective permissions are the intersection of the reviewed activity profile, current channel/attention limits, Compartment policy and scoped capability grants. Missing, expired or conflicting authority prevents model/tool dispatch; pause, stop and silence controls remain available. Numeric profile defaults and the exact serialized budget schema remain in `UBU-Q0128`; a profile cannot ship without finite caps.\n+\n+**Initial permissions.** Every restricted-attention mode starts with user-initiated local candidate capture, a bounded acknowledgment, pause/stop/silence, and optional explicitly granted media transport (`pause`, `resume`, `skip`, `rewind`) against one fresh, unambiguous playback target. Capture produces existing evidence/candidate records, including Task proposals; it does not create an admitted Task, Snapshot, Preference or Calendar change. Media transport uses the current integration grant and policy checks; it never authorizes sharing, purchases or broader app access. Richer read-only and short-drafting profiles remain open.\n+\n+Driving, bicycling and uncertain high-risk contexts additionally prohibit generated drafting, reflective/coaching dialogue, spoken menus and visually dependent review. Permit at most one brief clarification per request; an unresolved target or unanswered clarification leads to capture/deferral without retry prompts. External sends, financial actions, legal commitments, deletion, multi-party communication and scheduling changes are deferred. Neither a capability grant, a generic \u201cyes\u201d, an action-specific spoken confirmation nor a model's `safe_for_mode` claim overrides this envelope. UbU may remain silent or suggest user-selected existing media/rest; it must not repeatedly solicit work or queue review prompts while attention is restricted.\n+\n+**Controller boundary.** Consented local STT yields evidence, then the harness assembles a purpose-limited `ContextBundle` under \u00a721.3 and requests an interpretation/candidate through \u00a721.2. Include only the permitted current referents, relevant admitted state and scoped playback/context versions needed for that operation. Model confidence, risk estimates and confirmation suggestions are advisory. Deterministic validation checks schema, semantics, current budget, Compartment route, grants, object versions, ambiguity and idempotency before invoking an allowlisted control or creating a review candidate. Recheck immediately before dispatch and speech output; a local processing route alone does not authorize reading private content aloud on a shared audio endpoint. Raw AV stays local under \u00a712.2; an unavailable local backend never creates cloud-fallback authority.\n+\n+**Interruption and deferral.** Partial/uncertain STT, ambiguous recipients or \u201cthis/that\u201d references, stale media state and false conversational continuity never select an action by confidence alone. Ask at most the budgeted clarification count, then retain an unresolved candidate or defer. Interruption cancels unfinished speech and pending confirmation; it cannot complete an utterance or replay an action. A mode, target, payload, policy or Device change invalidates pending approval. Leaving a restricted mode only makes a deferred item available for ordinary review: it never dispatches it. Any later consequential action needs fresh admission under current authority and confirmation bound to the exact operation, recipient, payload/version and expiry; the detailed audio confirmation grammar remains open.\n+\n+Audit records link the session, evidence/candidate, budget and policy versions, ContextBundle, resolved target/version, policy decision and reason, approval/deferral/rejection, invocation/result and idempotency key. They retain minimized summaries or permitted refs, not a mandatory recording or full transcript. Raw-media retention remains under \u00a712.2 and `UBU-Q0170`\u2013`UBU-Q0171`. Device handoff rechecks capture, context, output route and authority; synchronized candidates never carry live microphone consent or pending execution approval.\n+\n+**Release boundary.** Before enabling a restricted-attention slice, demonstrate that interruption stops output, uncertain references do not dispatch, high-risk sends remain deferred despite confirmation, mode/Device changes do not execute queued work, and offline operation preserves local-only denials. This is a design contract, not an implementation or a claim of validated driving safety. `UBU-Q0183` retains the delivery sequence and device allocation under `UBU-D0292`; richer media/narration and profile tuning remain later answers, without restoring a Phase 3 gate.\n+\n ### 21.2 Structured-output admission pipeline\n \n LLM, parser, worker, and agent outputs should follow an admission pipeline:\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\nindex a9ee7ad..dd74f29 100644\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -2138,54 +2138,56 @@\n ## UBU-Q0128: Embodied Audio Interaction\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: Commute Mode, restricted-attention interaction, audio-only task capture, audio-safe messaging, media-state routing, exercise/rest/walking interaction modes Resolved by: None Last scored: 2026-10-11 Scored from commit: None\n \n Depends on unregistered areas: Affect model, Compartment policy, audio I/O policy,\n MCP/tool harness policy, device context model.\n \n ### Question\n \n UbU may support Embodied Audio Interaction: audio-first or audio-only interaction for situations where hands, visuals, or intellectual bandwidth are restricted, including driving, bicycling, walking, running, jogging, stationary exercise, physical resting, cooking, housework, and other embodied contexts. What is the interaction model, policy envelope, context-routing architecture, and safety boundary for using UbU as a harness over STT, TTS, MCP-scoped LLM calls, media apps, and structured UbU state during these restricted-attention contexts?\n \n ### Subquestions\n \n 1. What activity contexts should Embodied Audio Interaction explicitly model: driving, bicycling, walking, running, jogging, stationary exercise, resting, cooking, housework, showering, transit, or user-defined modes?\n 2. What input and output constraints should be represented for each mode: hands availability, visual availability, locomotion risk, interruptability, working-memory budget, intellectual bandwidth, emotional bandwidth, maximum prompt length, maximum response length, and maximum number of choices?\n 3. How should UbU distinguish interaction mode from Compartment: for example, `Compartment: UbU Project / Public Outreach` and `Mode: Driving`, rather than making driving itself a Compartment?\n 4. What deterministic policy layer should sit between audio input, MCP/tool access, LLM interpretation, UbU state, and external actions?\n 5. How should UbU package scoped context for an LLM during audio-only use so that the model receives enough context for high performance without overexposing private, irrelevant, or unsafe data?\n 6. What structured JSON contract should audio-interaction LLM calls return: intent, entities, confidence, ambiguity, cognitive-load estimate, action risk, proposed action, required confirmation, defer reason, and safe-for-mode flag?\n 7. What actions are safe in each embodied mode: idea capture, task creation, message drafting, calendar review, media control, lightweight reflection, exercise coaching, original narration, or external sends?\n 8. What actions should be prohibited or deferred in high-risk modes such as driving or bicycling: high-stakes messages, irreversible deletion, financial actions, legal commitments, major scheduling changes, multi-party communication, or visually dependent review?\n 9. How should UbU handle speech recognition uncertainty, road noise, breath noise, interruptions, partial utterances, ambiguous references, and false continuity?\n 10. What confirmation grammar should UbU require for external actions in restricted-attention contexts, especially where a generic \u201cyes\u201d is too ambiguous?\n 11. How should UbU use affect information to decide whether the user is ready for intellectual interaction, should only capture ideas, should defer complex work, or should receive non-intellectual audio instead?\n 12. How should UbU detect and model user understandability during embodied contexts, including reduced recall, reduced intelligence-in-the-moment, lower working memory, or increased interruption probability?\n 13. How should UbU support audio-safe messaging: relationship-aware drafting, concise readback, wrong-recipient prevention, tone checking, review queues, and defer-until-parked policies?\n 14. How should UbU integrate with media apps without replacing them: music playback, podcast resume state, audiobook state, skip/rewind, queue, save, summarize, and \u201cwhat did I miss?\u201d interactions?\n 15. How should UbU interpret vague audio commands such as \u201cgo back,\u201d \u201cpause that,\u201d \u201csave this,\u201d \u201cwhat was that?\u201d, or \u201cremind me about this\u201d based on current activity, media state, recent interruptions, and project context?\n 16. Should UbU expose an `InteractionBudget` object, and if so, what fields should it include for input channels, output channels, visual attention, manual control, locomotion risk, intellectual bandwidth, emotional bandwidth, social risk tolerance, max turn length, action permissions, and deferral policy?\n 17. How should Embodied Audio Interaction interact with local-first architecture, cross-device sync, car audio systems, headphones, watches, phones, and future wearable devices?\n 18. What audit trail should UbU preserve for audio interactions, especially when external actions were proposed, confirmed, deferred, or rejected?\n 19. How should UbU avoid becoming an attention-extraction system or productivity grinder, and instead act as an attention steward that sometimes recommends silence, music, rest, or non-intellectual content?\n 20. Given the primary camera-and-voice MVP release commitment in `UBU-D0292`, which restricted-attention primitives must be delivered first, and which richer media/narration integrations remain later work? Phase 3 is no longer a gate on the primary surface.\n \n ### Current direction\n \n **Architectural answer and MVP scope (`UBU-D0292`, `UBU-D0293`).** The primary surface is local camera-and-voice perception: observation \u2192 reviewable candidate state \u2192 resolution against permitted persistent context \u2192 consequential clarification \u2192 CPU semantic/policy/conflict validation \u2192 operator admission \u2192 planning from admitted state. Model-time evidence never substitutes for planner-time semantics. Voice is ordinary interaction; forms and text remain available for precision, context, preference and accessibility. Capture is mode-bound, visible, source-scoped and pausable; continuous AV requires the existing explicit mode, Compartment review, routing disclosure and user approval. Raw-media minimization/deletion is distinct from append-only correction and evidence metadata. Intent and affect remain user-declared. These answers refine the already-licensed mechanism, rather than making a model the planner.\n \n+**Restricted-attention default (`UBU-D0309`).** DESIGN.md \u00a721.1.2 specifies a controller-owned `InteractionBudget`, activity/Compartment separation, candidate capture and scoped media transport, high-risk deferral, interruption invalidation and fresh review after mode/Device changes. Model confidence or spoken confirmation cannot expand these permissions. Tuned mode profiles, exact typed output and confirmation grammar, richer integrations and naming remain open.\n+\n **Naming (absorbs delta \u00a717.2).** Embodied Audio Interaction remains the recorded name for the restricted-attention audio subset. No name for the broader camera-and-voice surface has been selected; neither working proposal is adopted. Should this wider interaction model receive a distinct canonical name, or should the existing label remain only the narrower mode? Keep this as an operator naming choice, not a new decision record.\n \n Embodied Audio Interaction is the recorded name for the restricted-attention audio subset. It generalizes the earlier Commute Mode concept beyond driving to any situation where audio interaction is available while hands, visuals, attention, or intellectual bandwidth are constrained. Driving and bicycling are the most safety-critical cases, but walking, running, gym use, physical resting, cooking, housework, and similar contexts share the same architectural pattern. The broader primary camera-and-voice surface is MVP release scope; its naming remains open above.\n \n UbU should not merely provide \u201cChatGPT in the car\u201d or generic voice chat. UbU should remain the harness, context controller, policy layer, and action authority. STT, TTS, MCP, LLMs, media apps, and device integrations should be subordinate components. UbU should decide what context is exposed, what actions are allowed, how much complexity the user can handle, when confirmation is required, and when work should be deferred.\n \n The feature should distinguish agency audio, media-control audio, and generative-experience audio. UbU may allow complex processes in restricted-attention contexts only by decomposing them into bounded, audio-safe primitives such as capture, triage, short drafting, readback, review-queue creation, and low-risk execution. UbU should be conservative with external messages, irreversible actions, financial/legal commitments, and any action requiring visual review.\n \n-The affect model is central. UbU may know that the user is not in the right state for intellectual work and may instead recommend silence, music, podcasts, audiobooks, or original narration. This makes UbU an attention steward rather than a productivity maximizer.\n+Affect and readiness remain user-declared or unknown. UbU may use those declarations to offer silence, rest or user-selected media, without diagnosing reduced intelligence or treating available audio time as an obligation to work. Original Narration remains the separate question `UBU-Q0129`.\n \n ### Resolution\n \n-Architecture and MVP-release priority are answered by `UBU-D0292` and `UBU-D0293`; the former Phase 3 release gate is superseded. This question remains Open for the naming choice and the detailed restricted-attention policies above. The capture delivery sequence is `UBU-Q0183`; related evidence, latency, retention and hardware questions are `UBU-Q0164`\u2013`UBU-Q0182`. No generic voice confirmation authorizes a consequential external action.\n+Architecture and MVP-release priority are answered by `UBU-D0292` and `UBU-D0293`; the former Phase 3 release gate is superseded. `UBU-D0309` answers the initial restricted-attention permission envelope, controller-owned budget, high-risk deferral and interruption/review boundary. This question remains Open for quantitative mode profiles, the exact budget and LLM-output schemas, detailed external-action confirmation grammar, richer messaging/media/coaching integrations, device-specific behavior and the naming choice. The capture delivery sequence is `UBU-Q0183`; related evidence, latency, retention and hardware questions remain `UBU-Q0164`\u2013`UBU-Q0182`. No generic voice confirmation authorizes a consequential external action.\n \n ---\n \n",
    "commit_message": "design: bound restricted-attention audio capture and deferral",
    "validation_notes": [
      "Generated with git diff --no-index from temporary copies of the current working-tree files and proposed contents.",
      "git apply --check --verbose succeeded against the working tree for all three files; no --cached or --recount was used.",
      "The OPEN_QUESTIONS.md hunk contains UBU-Q0128's heading and its own Resolution section; its single-line metadata, Phase 2 tag and scoring fields are unchanged.",
      "UBU-D0309 was unused; no questions are marked Solved or added. Only the three listed allowlisted files differ.",
      "Repository files remain unchanged. This specifies proposed behavior; runtime enforcement and driving safety have not been tested."
    ],
    "new_questions_added": [],
    "questions_resolved": [],
    "decisions_added": [
      "UBU-D0309"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0128-openai-gpt-6-20261011",
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
