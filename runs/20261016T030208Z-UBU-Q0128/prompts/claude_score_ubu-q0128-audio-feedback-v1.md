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
Base commit: `8ec7ca7d7b97e1bed58e6ece46e385ba1b328f89`

```markdown
## UBU-Q0128: Embodied Audio Interaction

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: Commute Mode, restricted-attention interaction, audio-only task capture, audio-safe messaging, media-state routing, exercise/rest/walking interaction modes Resolved by: None Last scored: 2026-10-15 Scored from commit: None

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

**Speech evidence (`UBU-D0372`).** DESIGN.md §21.1.2 specifies `audio-stt-evidence/1`: whole-utterance acquisition/completion bindings, immutable transcript sealing, controller quality flags and revision-race handling before interpretation. Provisional or changed STT text cannot authorize media transport; clarification retains distinct evidence and original turn limits. This answers only the input-finalization/revision slice of subquestion 9; recognition quality, source ownership and platform qualification remain open.

**Interpretation context (`UBU-D0371`).** DESIGN.md §21.1.2 specifies `audio-context/1`: an operation-specific allowlist for current-turn STT evidence, minimal explicit subject aliases and scoped playback metadata. The controller owns reads; prior dialogue, broader private state and backend tools remain outside the initial interpretation. This answers the initial-envelope context-selection slice of subquestion 5; measured quality and richer context profiles remain open.

**Serialized budget (`UBU-D0311`).** DESIGN.md §21.1.2 defines the closed controller-issued `InteractionBudgetV1`, including versioned context/authority bindings, declared-or-unknown readiness, channel/attention bounds, finite byte/time/choice caps and fixed review/deferral policies. Semantic checks, monotonic deadlines and turn-owned counters prevent stale snapshots or budget renewal from restoring authority. This answers the initial-envelope serialization portion of subquestions 2 and 16; numerical profile values and richer envelopes remain open.

**Playback referents (`UBU-D0322`).** DESIGN.md §21.1.2 specifies explicit versus session-selected media targets, separation from UbU speech, version-bound dispatch, a vague-command routing table and required stale/replay fixtures. This answers the initial-envelope referent-routing portion of subquestions 9, 14 and 15 without enabling richer media operations. Numerical profiles, richer integrations, external-action confirmation grammar and naming remain open.

**Deferred message confirmation (`UBU-D0333`).** DESIGN.md §21.1.2 specifies `audio-send-review/1`: user-requested ordinary review of one permitted plain-text message, complete recipient/payload presentation, an exact controller-bound single-use send phrase, current authority and interruption/replay checks. This answers only the ordinary-review single-recipient send slice of subquestions 10, 13 and 18. It enables no sending workflow, changes neither v1 audio schema and leaves every initial restricted-attention send deferred. Other external-action grammars and richer messaging remain open.

**Device route boundary (`UBU-D0359`).** DESIGN.md §21.1.2 binds microphone consent separately from a versioned complete speech-output route, requires prevention of automatic input/speaker fallback, and invalidates affected turns on route or audio-focus loss. Reconnection and handoff need explicit resumption and fresh local bindings. This answers the input/output route and handoff subset of subquestion 17; accessory support and numerical/platform qualification remain open.

**Numerical profile evidence (`UBU-D0365`).** DESIGN.md §21.1.2 defines calibration and frozen, condition-specific acceptance for existing budget limits, acknowledgment comprehension, primary-activity performance and session-long burden. Safe simulations include matched baselines, maximum-length speech, noise, interruption and unknown readiness; no productivity or interaction-frequency floor applies. This answers the profile-selection method only; measured values, passing combinations and real driving/bicycling qualification remain open.

**Naming (absorbs delta §17.2).** Embodied Audio Interaction remains the recorded name for the restricted-attention audio subset. No name for the broader camera-and-voice surface has been selected; neither working proposal is adopted. Should this wider interaction model receive a distinct canonical name, or should the existing label remain only the narrower mode? Keep this as an operator naming choice, not a new decision record.

Embodied Audio Interaction is the recorded name for the restricted-attention audio subset. It generalizes the earlier Commute Mode concept beyond driving to any situation where audio interaction is available while hands, visuals, attention, or intellectual bandwidth are constrained. Driving and bicycling are the most safety-critical cases, but walking, running, gym use, physical resting, cooking, housework, and similar contexts share the same architectural pattern. The broader primary camera-and-voice surface is MVP release scope; its naming remains open above.

UbU should not merely provide “ChatGPT in the car” or generic voice chat. UbU should remain the harness, context controller, policy layer, and action authority. STT, TTS, MCP, LLMs, media apps, and device integrations should be subordinate components. UbU should decide what context is exposed, what actions are allowed, how much complexity the user can handle, when confirmation is required, and when work should be deferred.

The feature should distinguish agency audio, media-control audio, and generative-experience audio. UbU may allow complex processes in restricted-attention contexts only by decomposing them into bounded, audio-safe primitives such as capture, triage, short drafting, readback, review-queue creation, and low-risk execution. UbU should be conservative with external messages, irreversible actions, financial/legal commitments, and any action requiring visual review.

Affect and readiness remain user-declared or unknown. UbU may use those declarations to offer silence, rest or user-selected media, without diagnosing reduced intelligence or treating available audio time as an obligation to work. Original Narration remains the separate question `UBU-Q0129`.

### Resolution

Architecture and MVP-release priority are answered by `UBU-D0292` and `UBU-D0293`; the former Phase 3 release gate is superseded. `UBU-D0309` answers the initial restricted-attention permission envelope, controller-owned budget, high-risk deferral and interruption/review boundary. `UBU-D0310` adds the exact initial-envelope LLM-output schema, semantic checks and request/freshness binding. `UBU-D0311` specifies the exact initial-envelope budget serialization, units, revision/freshness checks and turn-owned limits. `UBU-D0322` adds initial-envelope audio-domain/playback referent selection, vague-command routing, control-revision checks and adapter acceptance requirements; it enables no richer action. `UBU-D0333` adds complete ordinary review and a single-use, operation-bound English send phrase for one already-permitted plain-text message to one recipient; it enables no sending workflow or restricted-mode execution. `UBU-D0359` adds qualified input/output route bindings, prevention of automatic audio fallback, route-loss invalidation and fresh reconnection/handoff requirements; it selects no accessory or measured profile. `UBU-D0365` defines numerical-profile selection through comprehension, primary-activity and cumulative-burden trials; it supplies no measured values or passing activity profile. `UBU-D0371` bounds initial interpretation context with an operation-specific allowlist, controller-only reads and required input/access fixtures; it supplies no measured quality or richer profile. `UBU-D0372` adds whole-utterance STT completion, sealed evidence revisions, controller quality disposition and duplicate/conflicting callback rules; it supplies no measured recognition or input-owner guarantee. This question remains Open for quantitative mode profiles, richer LLM-output extensions, other external-action confirmation grammars, richer messaging/media/coaching integrations, platform-specific qualification and remaining device behavior, and the naming choice. The capture delivery sequence is `UBU-Q0183`; related evidence, latency, retention and hardware questions remain `UBU-Q0164`–`UBU-Q0182`. No generic voice confirmation authorizes a consequential external action.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0128-audio-feedback-v1",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0128",
    "base_commit": "8ec7ca7d7b97e1bed58e6ece46e385ba1b328f89",
    "summary": "Specify controller-owned spoken outcome feedback for the initial restricted-attention envelope.",
    "rationale": "Existing contracts require bounded acknowledgments but leave their exact wording and outcome selection implicit. Fixed phrases with durable-save and operation-correlated effect guards distinguish retained review work, confirmed controls and uncertainty without model-written speech or repeated feedback. This narrows the partially answered question while preserving its open measured profiles and broader product choices.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5846,3 +5846,15 @@ Retain ownership of in-flight resources until completion or demonstrated safe te\n Require \u00a716.10.6's delayed-completion, readback/cancellation-race, repeated-timeout and stale-callback fixtures plus policy-safe physical-device observations. Injected callbacks establish controller behavior only, not driver recovery. Publish pending versus released resources and actual outcomes under the existing accounting rules. Failed required bounds withhold the candidate configuration. The in-process boundary, planning schemas, parity rule and release/switch criteria remain unchanged; `UBU-Q0160` stays Open pending real-device measurements and operator review.\n \n ---\n+\n+## UBU-D0378: Restricted-attention acknowledgments bind controller outcomes\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a721.1.2. Partially answers `UBU-Q0128` subquestions 9, 12 and 18; refines `UBU-D0310` under `UBU-D0359` and `UBU-D0365` without widening the initial envelope.\n+\n+Adopt `audio-feedback/1` for controller-rendered terminal acknowledgments. Fixed English phrases distinguish a durably retained review artifact, a failed or uncertain save, a confirmed scoped media effect, a failed or unsent control and an uncertain effect. Model intent, confidence, dispatch acceptance and coincident playback state cannot select a success phrase. Saving evidence of a media attempt cannot establish that attempt's effect.\n+\n+Allow at most one terminal speech attempt per original turn. Preflight the complete phrase against remaining byte, rendered-speech and elapsed limits, then recheck current bindings and output disclosure before release. Suppress unavailable output without shortening qualifications, replaying speech or renewing the turn. Pause/Stop/Silence remains immediate and receives no spoken acknowledgment under this profile.\n+\n+Link outcome evidence, template and output disposition through existing minimized audit records; completed output proves neither hearing nor comprehension. Require \u00a721.1.2's storage-failure, unknown-effect, duplicate, interruption and rendering-boundary fixtures, plus the existing comprehension trials. This specifies rendering behavior, not implementation, measured usability or a driving-safety result. Both v1 schemas and MVP-release scope remain unchanged; `UBU-Q0128` stays Open for measured profiles and its other remaining slices.\n+\n+---\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -3984,6 +3984,34 @@ Reject malformed, truncated, oversized, unknown-version, extra-field, forged-ref\n \n Before enabling v1, include negative fixtures for unknown action kinds, foreign evidence/playback aliases, inconsistent intent/action pairs, late replies after interruption or handoff, and duplicate/unknown media outcomes. Valid capture stays in candidate state; every consequential external request remains deferred under the initial envelope. This specifies a contract, not a shipped schema or implementation. Numeric mode profiles, external-action confirmation grammar and richer output extensions remain in `UBU-Q0128`.\n \n+**Spoken outcome feedback (`UBU-D0378`).** Adopt the controller rendering profile `audio-feedback/1` for optional terminal acknowledgments in the initial restricted-attention envelope. The controller selects from the fixed English phrases below using its actual operation route and recorded outcome; model intent, confirmation advice and generated wording select none. Existing clarification templates remain separate and consume the same original turn budget. This profile adds no progress, readback or retry speech.\n+\n+| Controller outcome | Exact permitted phrase | Required basis |\n+|---|---|---|\n+| Capture or deferred request retained | `Saved for review.` | A policy-permitted durable commit identifies the exact review artifact/version and original capture key. Receipt, an in-memory queue, raw audio or an audit row alone cannot establish this outcome; neither content correctness nor canonical admission is claimed. |\n+| Capture/review write definitely failed | `Not saved.` | The controller establishes that no review artifact committed for that key and no write remains pending. A timeout alone cannot establish failure. |\n+| Capture/review write outcome unknown | `Save unconfirmed.` | Commit status cannot be established. Reconcile the original key before any resubmission; feedback authorizes no retry. |\n+| Scoped media effect confirmed | `Pause confirmed.`, `Resume confirmed.`, `Skip confirmed.` or `Rewind confirmed.`, matching the dispatched operation | The qualified adapter supplies operation-correlated effect evidence for the exact target, expected control revision and attempt. Command receipt/acceptance, silence or a matching state read alone cannot prove the effect, including a relative skip/rewind. |\n+| Media control failed without effect | `Playback control failed.` | Operation-correlated evidence establishes the dispatched attempt ended without effect and no execution remains pending. A generic adapter error is insufficient. |\n+| Media control definitely unsent | `Playback control not sent.` | The controller establishes no dispatch occurred. This says nothing about playback changes caused elsewhere. |\n+| Media effect unestablished | `Playback control unconfirmed.` | A dispatch may have occurred but its effect is unknown, including an error without proof of no effect. Preserve ordinary reconciliation and never replay transport to obtain a success phrase. |\n+| Rejected before selecting a supported operation route | `Request not completed.` | Neither a save nor a control was attempted. Use only a policy-permitted status; do not speak a denied reason or source detail. |\n+\n+Deferred work uses the capture/review rows only when that storage operation was actually attempted. Once a media route is selected, use its confirmed, failed, unsent or unconfirmed row; retaining its evidence/audit does not turn an unknown effect into capture success. No-op and local Pause/Stop/Silence produce no terminal speech under this profile. Local controls act immediately without waiting for rendering. No phrase adds a Task title, transcript, recipient, media label or other dynamic source text. Even these fixed status phrases need current output-route and disclosure permission.\n+\n+Before release, render the complete phrase within the qualified resource bounds and establish that its full UTF-8 length and rendered duration fit the remaining cumulative prompt/response and speech limits and original turn deadline. Recheck the live epoch, outcome binding, route generation and disclosure permission before each bounded output segment. If the complete phrase cannot fit, suppress it; do not drop `for review` or `unconfirmed`, accelerate speech outside the qualified profile or split across renewed turns. Interruption can still cut playback short: discard unsent samples and never continue or replay the phrase automatically. A failed acknowledgment does not undo a committed artifact or confirmed effect.\n+\n+Serialize terminal-feedback selection and attempt consumption with turn invalidation. Mark the attempt before releasing its first sample; permit at most one such attempt per original turn, including after duplicate callbacks, write reconciliation or a late positive media result. Unavailable output is not queued for reconnection, restart, handoff or later ordinary review. Preserve permitted outcomes for user-requested review without reviving the old turn. Existing audit links record the outcome evidence/artifact revision, template/profile, turn/epoch and route bindings, and whether output was suppressed, partial, reported complete or unknown. No mandatory recording, transcript copy or new canonical object/wire field is added. TTS completion establishes neither actual hearing, understanding nor approval.\n+\n+Before enabling this rendering profile, pin controller, storage/transport adapter, TTS/template, Device/route and policy revisions and exercise the actual outcome selector and output path:\n+\n+- A committed capture and retained deferral use the review phrase without admitting state; definite write rejection and an ambiguous commit use their respective failure/unknown phrases. Crash/restart and original-key reconciliation cause neither a false saved claim nor duplicate artifact/speech.\n+- Each confirmed scoped media operation supplies a positive control. A proven no-effect failure and a definitely unsent control use their separate phrases. Adapter acceptance, a timeout/error and a matching state caused by another actor supply no success proof; relative commands remain unconfirmed without operation-correlated evidence. Saved audit/evidence does not mask that uncertainty or trigger transport replay.\n+- Duplicate/late callbacks and Stop/Silence, expiry, policy/route change or handoff before/during output produce no stale, repeated or resumed speech. Confirmed storage/effect outcomes survive suppressed or interrupted feedback where policy permits.\n+- Exercise exact remaining byte/time boundaries, maximum rendered duration and insufficient space for the complete qualified phrase. No truncation or unqualified speech-rate change can pass. Apply `UBU-D0365`'s user comprehension trials to these exact renderings, checking the distinction between retained review work and admitted/executed effects and keeping missing answers explicit.\n+\n+These are required fixtures, not passing implementation or comprehension results. Other languages need separately versioned and qualified renderings. This answers the initial-envelope outcome-feedback slice of `UBU-Q0128` subquestions 9, 12 and 18; both v1 schemas, action permissions and \u00a74.2's MVP-release minimum remain unchanged.\n+\n **Interruption and deferral.** Partial/uncertain STT, ambiguous recipients or \u201cthis/that\u201d references, stale media state and false conversational continuity never select an action by confidence alone. Ask at most the budgeted clarification count, then retain an unresolved candidate or defer. Interruption cancels unfinished speech and pending confirmation; it cannot complete an utterance or replay an action. A mode, target, payload, policy or Device change invalidates pending approval. Leaving a restricted mode only makes a deferred item available for ordinary review: it never dispatches it. Any later consequential action needs fresh admission under current authority and confirmation bound to the exact operation, recipient, payload/version and expiry; the ordinary-review single-recipient message slice follows below, while other external-action grammars remain open.\n \n **Deferred message confirmation (`UBU-D0333`).** The controller profile `audio-send-review/1` defines an additional confirmation requirement for an already-permitted, ordinary-attention review of one plain-text message to one recipient. It enables no sending workflow and adds no operation to `AudioInterpretationV1` or `InteractionBudgetV1`. Every external send remains deferred in the initial restricted-attention envelope. Group/multi-party sends, attachments, financial/legal commitments, high-stakes messages and unknown consequences are outside this profile.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -2203,6 +2203,8 @@ ## UBU-Q0128: Embodied Audio Interaction\n \n ### Resolution\n \n+`UBU-D0378` adds `audio-feedback/1`: fixed controller-selected English outcome phrases, durable-save and operation-correlated media-effect guards, one terminal speech attempt per original turn, complete-rendering budget checks and minimized output-disposition audit links. Interrupted or missing feedback proves no hearing or comprehension and causes no replay. This answers only the initial outcome-feedback slice of subquestions 9, 12 and 18; measured usability, other languages and the remaining scope below stay Open.\n+\n Architecture and MVP-release priority are answered by `UBU-D0292` and `UBU-D0293`; the former Phase 3 release gate is superseded. `UBU-D0309` answers the initial restricted-attention permission envelope, controller-owned budget, high-risk deferral and interruption/review boundary. `UBU-D0310` adds the exact initial-envelope LLM-output schema, semantic checks and request/freshness binding. `UBU-D0311` specifies the exact initial-envelope budget serialization, units, revision/freshness checks and turn-owned limits. `UBU-D0322` adds initial-envelope audio-domain/playback referent selection, vague-command routing, control-revision checks and adapter acceptance requirements; it enables no richer action. `UBU-D0333` adds complete ordinary review and a single-use, operation-bound English send phrase for one already-permitted plain-text message to one recipient; it enables no sending workflow or restricted-mode execution. `UBU-D0359` adds qualified input/output route bindings, prevention of automatic audio fallback, route-loss invalidation and fresh reconnection/handoff requirements; it selects no accessory or measured profile. `UBU-D0365` defines numerical-profile selection through comprehension, primary-activity and cumulative-burden trials; it supplies no measured values or passing activity profile. `UBU-D0371` bounds initial interpretation context with an operation-specific allowlist, controller-only reads and required input/access fixtures; it supplies no measured quality or richer profile. `UBU-D0372` adds whole-utterance STT completion, sealed evidence revisions, controller quality disposition and duplicate/conflicting callback rules; it supplies no measured recognition or input-owner guarantee. This question remains Open for quantitative mode profiles, richer LLM-output extensions, other external-action confirmation grammars, richer messaging/media/coaching integrations, platform-specific qualification and remaining device behavior, and the naming choice. The capture delivery sequence is `UBU-Q0183`; related evidence, latency, retention and hardware questions remain `UBU-Q0164`\u2013`UBU-Q0182`. No generic voice confirmation authorizes a consequential external action.\n \n ---\n",
    "commit_message": "docs: bind restricted-attention feedback to recorded outcomes",
    "validation_notes": [
      "Generated unified diffs from current working-tree files and proposed in-memory contents; no repository files were modified.",
      "git apply --check --whitespace=error - passed against the working tree.",
      "Verified required JSON fields/types, accurate hunk ranges, selected-question scope and unchanged single-line metadata; UBU-Q0128 remains Open.",
      "Only the three listed allowed files change; UBU-D0378 was unused. Existing JSON contracts, action permissions and MVP-release scope remain unchanged.",
      "Documentation proposal only: specified behavioral fixtures and comprehension trials were not run; no measured usability or driving-safety result is claimed."
    ],
    "new_questions_added": [],
    "questions_resolved": [],
    "decisions_added": [
      "UBU-D0378"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0128-audio-feedback-v1",
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
