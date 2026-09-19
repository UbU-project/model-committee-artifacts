# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0077`  
Question title: `Realtime interaction session and candidate update schema`  
Base commit: `ef2593f0f9ee9be4ba16b661e144b81d936d2a43`

```markdown
## UBU-Q0077: Realtime interaction session and candidate update schema

Status: Open Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0025, UBU-Q0031, UBU-Q0068 Blocks: realtime model adapters, discovery mode, meeting capture, interruption handling, Phase 1b candidate/admitted state boundary Resolved by: None Last scored: 2026-09-18 Scored from commit: None

### Question

What is the minimum schema for realtime interaction sessions and realtime-derived candidate updates?

### Subquestions

1. Should realtime sessions be represented as Tasks, Events, Logs, sensor streams, `InteractionSession` objects, or a hybrid?
2. What candidate update types are required: interruption, task progress, affect signal, external condition change, clarification question, Plan deviation, Log candidate, Task candidate, or AssociationAttestation candidate?
3. How should UbU distinguish elapsed time noticed by a model from planner-valid Task, Calendar, or Log semantics?
4. What provenance and confidence metadata is mandatory for audio/video/text-derived observations?
5. Which realtime features are local-only, cloud-optional, or prohibited under sensitive Compartments?

### Current direction

Realtime models are optional interaction backends. They emit candidate updates. The planner, Logs, Compartment policy, and user-review rules decide what becomes canonical.

Phase 1b re-scope: this question is pulled forward from Phase 2 under the Phase 1b rule, which requires every Quick UbU feature merged into mainline to adopt the restrictions, forms, and functional boundaries of the MVP design it corresponds to, even when that design's implementation phase is beyond Phase 1. Phase 1b emits artifacts whose shape depends on this answer, so it is answered now. Phase 1b scope: the advisory review queue is the candidate/admitted boundary in practice, so the candidate-update schema and its provenance and confidence metadata are required now, independently of any realtime audio or video backend. Candidate update types needed in Phase 1b are tag, dependency, preference, decomposition, and clarification question. Subquestion 4's audio/video provenance and the AssociationAttestation candidate type are not Phase 1b concerns, since the message crawler and the association model are deferred. Candidate lifecycle and durable rejection are decomposed into UBU-Q0138.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0077-openai-gpt-5-codex-20260919",
    "provider_id": "openai",
    "model_name": "gpt-5-codex",
    "question_id": "UBU-Q0077",
    "base_commit": "ef2593f0f9ee9be4ba16b661e144b81d936d2a43",
    "summary": "Resolve UBU-Q0077 by adding a minimal realtime session and candidate-update schema, then marking realtime outputs as reviewable candidates rather than admitted state.",
    "rationale": "The proposal keeps realtime models as optional interaction backends while preserving UbU's existing sovereignty, Log, Snapshot, Message Extractor, candidate/admitted, and Compartment boundaries. It answers the Phase 1b need for tag, dependency, preference, decomposition, and clarification candidates without prematurely admitting audio/video capture or association extraction semantics.",
    "changed_files": [
      "OPEN_QUESTIONS.md",
      "DESIGN.md",
      "DECISIONS.md"
    ],
    "patch": "diff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -932,7 +932,7 @@\n \n ## UBU-Q0077: Realtime interaction session and candidate update schema\n \n-Status: Open Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0025, UBU-Q0031, UBU-Q0068 Blocks: realtime model adapters, discovery mode, meeting capture, interruption handling, Phase 1b candidate/admitted state boundary Resolved by: None Last scored: 2026-09-18 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0025, UBU-Q0031, UBU-Q0068 Blocks: realtime model adapters, discovery mode, meeting capture, interruption handling, Phase 1b candidate/admitted state boundary Resolved by: UBU-D0272 Last scored: 2026-09-18 Scored from commit: None\n \n ### Question\n \n@@ -954,7 +954,7 @@\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0272`: realtime interaction is represented by a lightweight `RealtimeInteractionSession` plus reviewable `RealtimeCandidateUpdate` records. Sessions describe the consented interaction context and routing boundary; candidate updates carry typed, provenance-bearing suggestions that remain outside admitted state until ordinary review or explicit policy admission. Phase 1b candidate types are tag, dependency, preference, decomposition, and clarification question, while interruption, task progress, affect, external condition, Plan deviation, Log candidate, Task candidate, and AssociationAttestation candidates are allowed only within their existing review and Compartment gates. Model-noticed elapsed time is evidence, not Task, Calendar, or Log truth. Raw audio, video, screen, keystroke, and sensitive Compartment capture is local-only or prohibited unless a separate explicit mode and policy grant allow more.\n \n ---\n \ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -101,6 +101,22 @@\n UbU should start with schema-constrained general LLMs or local models plus validation, repair, confidence calibration, and provenance inspection. Fine-tuned, distilled, or adapter-trained extractor models become appropriate only after schemas stabilize and correction logs show enough repeated examples to justify the privacy, latency, cost, or reliability tradeoff. Custom models must preserve the same schemas, validators, provenance, review gates, and authority limits as general models.\n \n Custom extractor training requires retained-with-consent training bundles, gold `MessageExtractionResult` labels, user corrections, rejected candidates, field-level provenance labels, ambiguity examples, no-action examples, hard negatives, redaction and Compartment labels, and evaluation sets split by source system, channel type, relationship context, and privacy class.\n+\n+### 1.5 Realtime interaction sessions and candidate updates\n+\n+Realtime models are optional interaction backends for conversation, capture, triage, and short-horizon repair. UbU represents a realtime run as a lightweight `RealtimeInteractionSession`, not as a Task, Calendar event, canonical Log entry, or raw sensor stream. A session records the consented interaction context and source boundary; the model's outputs are `RealtimeCandidateUpdate` records until admitted through the ordinary planner, Log, Snapshot, Compartment, and review rules.\n+\n+The minimum `RealtimeInteractionSession` record contains `interaction_session_id`, `schema_version`, `instance_id`, `actor_identity_ref`, `device_ref`, `started_at`, optional `ended_at`, `session_state`, `source_modes`, `backend_ref`, `model_or_tool_ref`, prompt or template version, enabled input sources, `compartment_ids`, `disclosure_policy`, retention or redaction policy, routing mode, evidence item refs, candidate update refs, provenance, review status, and correction or revocation links. Session states align with discovery mode where applicable: `inactive`, `active`, `paused`, `ended`, and `pending_review`.\n+\n+The minimum `RealtimeCandidateUpdate` record contains `candidate_update_id`, `schema_version`, optional `interaction_session_id`, `emitted_at`, effective instant or interval, `candidate_type`, optional `target_ref`, payload, source evidence refs or excerpt hashes, per-field provenance, confidence, `compartment_ids`, redaction level, routing mode, review status, admission hint, idempotency key, and correction, rejection, revocation, or supersession links. The Phase 1b candidate types required for Quick UbU are `tag`, `dependency`, `preference`, `decomposition`, and `clarification_question`. Additional allowed candidate types are `interruption`, `task_progress`, `affect_signal`, `external_condition_change`, `plan_deviation`, `log_candidate`, `task_candidate`, and `association_attestation_candidate`, but each remains subject to its existing schema, authority, review, and Compartment gates.\n+\n+A realtime candidate never mutates admitted state by itself. A tag, dependency, decomposition, preference, Task, Log, Snapshot, Calendar, Plan-deviation, or AssociationAttestation effect becomes canonical only after user acceptance, validated admission, or a specific local policy grant for that exact mutation class. Rejected, corrected, superseded, or revoked candidates remain auditable according to the candidate lifecycle rules.\n+\n+Model-noticed elapsed time is represented as evidence or a candidate interval, not as planner-valid Task execution, Calendar occupancy, or Log truth. Planned time is reconciled only through accepted `plan_realized`, Task lifecycle, Calendar, Snapshot, or Log records. Unknown, private, rest, interruption, different-Task, and quick-note intervals preserve uncertainty and must not create Preference changes, Objective failures, habit claims, or moral meaning by inference.\n+\n+Mandatory provenance for realtime observations includes source kind, capture or observation interval, device and backend refs, model or tool identifier, prompt or template digest where applicable, parser or schema version, evidence refs or payload hashes, source Compartment, redaction level, processing route, confidence, and review status. Audio, video, screen, keystroke, raw message, raw file, raw GPS, and similarly sensitive sources also require explicit source enablement and policy-visible capture state before they can produce candidates.\n+\n+Routing defaults are conservative. Local-only processing is required for raw sensitive capture, `no_cloud_llm` Compartments, and sources whose policy denies external processing. Cloud processing is optional only for policy-approved, user-visible, redacted or intentionally provided inputs. Cross-user projection, external export, covert capture, broad background capture, hidden persuasion timing, and automatic external mutation are prohibited unless a later explicit mode, Compartment policy, capability grant, and user approval allow that exact behavior.\n \n ---\n \ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -4212,3 +4212,21 @@\n Automatic acceptance is limited to low-risk, non-mutating parse facts that are directly evidenced and policy-allowed, such as source refs, timestamps, message ids, and exact explicit metadata. Candidate Tasks, Objective links, AssociationAttestations, Relationship updates, commitments, high interrupt recommendations, priority escalation, deadlines, and any cross-Compartment or cross-user projection require user review or an explicit receiver-side policy grant.\n \n UbU should begin with general schema-constrained LLMs or local models plus validation, repair, confidence calibration, and provenance inspection. Fine-tuned, distilled, or adapter-trained extractors become justified only after schemas stabilize, correction logs are available, and measured privacy, latency, cost, or reliability needs outweigh the operational cost. Custom models use the same schemas, validators, provenance, review gates, and authority limits as general models.\n+\n+---\n+\n+## UBU-D0272: Realtime interaction emits reviewable candidate updates\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a71.5. Resolves `UBU-Q0077`.\n+\n+UbU represents realtime interaction as a hybrid boundary: a `RealtimeInteractionSession` records the consented interaction context, enabled sources, backend or model, Device, Identity, Compartment, routing, retention, evidence, candidate refs, provenance, review status, and correction or revocation links; it is not itself a Task, Calendar event, canonical Log entry, or raw sensor stream. Session states align with discovery mode where applicable: `inactive`, `active`, `paused`, `ended`, and `pending_review`.\n+\n+Realtime backends emit `RealtimeCandidateUpdate` records. The minimum candidate update records candidate id, schema version, optional session id, emitted time, effective instant or interval, candidate type, optional target ref, payload, evidence refs or hashes, per-field provenance, confidence, Compartment refs, redaction level, routing mode, review status, admission hint, idempotency key, and correction, rejection, revocation, or supersession links. Phase 1b requires `tag`, `dependency`, `preference`, `decomposition`, and `clarification_question` candidates. Later or separately gated candidates may include `interruption`, `task_progress`, `affect_signal`, `external_condition_change`, `plan_deviation`, `log_candidate`, `task_candidate`, and `association_attestation_candidate`.\n+\n+Candidate updates are not admitted state. They may create advisory review items, explanations, clarification prompts, or local suggestions, but they do not mutate Tasks, Calendars, Logs, Snapshots, Preferences, Plans, Relationships, Associations, AssociationAttestations, or Objective state without user acceptance, validated admission, or an explicit local policy grant for that mutation class. AssociationAttestation candidates remain subject to the evidence, trust, disclosure, and review rules for AssociationAttestations; Phase 1b does not require realtime audio/video or association extraction.\n+\n+Model-noticed elapsed time is evidence, not canonical semantics. A realtime model may report that time seems to have passed, a user may have been interrupted, or a planned interval may differ from observed activity, but planner-valid meaning is created only by accepted `plan_realized`, Task lifecycle, Calendar, Snapshot, or Log records. Unknown, private, rest, interruption, different-Task, and quick-note intervals preserve uncertainty and must not imply Preference changes, Objective failure, habit patterns, or moral judgment.\n+\n+Mandatory provenance for realtime observations includes source kind, observation or capture interval, Device and backend refs, model or tool identifier, prompt/template or schema version where applicable, evidence refs or payload hashes, source Compartment, redaction level, processing route, confidence, and review status. Audio, video, screen, keystroke, raw message body, raw file, raw GPS, and similarly sensitive inputs require explicit source enablement, visible capture state, and Compartment approval before they can produce candidates.\n+\n+Routing is conservative by default. Raw sensitive capture, `no_cloud_llm` Compartments, and externally restricted Compartments are local-only or prohibited according to policy. Cloud processing is permitted only for policy-approved, visible, intentionally provided or redacted inputs. Cross-user projection, external export, covert or broad background capture, hidden persuasion timing, and automatic external mutation remain prohibited unless a later explicit mode, Compartment policy, capability grant, and user approval authorize the exact behavior.\n",
    "commit_message": "Resolve realtime interaction candidate schema",
    "validation_notes": [
      "Generated a unified diff against the visible workspace context for OPEN_QUESTIONS.md, DESIGN.md, and DECISIONS.md.",
      "Ran git apply --check successfully against the current workspace state.",
      "No files outside the allowlist are modified; DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md is left unchanged."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0077"
    ],
    "decisions_added": [
      "UBU-D0272"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0077-openai-gpt-5-codex-20260919",
    "patch_applies": true,
    "allowlist_passed": true,
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
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
- Do not score your own provider's proposal unless explicitly asked for diagnostic self-score.

Return only JSON.
