# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0066`  
Question title: `Minimal Phase 3 Message Context Envelope schema`  
Base commit: `69ad3c7479877b7da91311ab728099a9c3963cf7`

```markdown
## UBU-Q0066: Minimal Phase 3 Message Context Envelope schema

Status: Open Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0061, UBU-Q0062 Blocks: Phase 3 cross-user communication, message priority triage, UbU-to-UbU interoperability Resolved by: None Last scored: 2026-09-18 Scored from commit: None

### Question

What is the minimum Message Context Envelope schema required for Phase 3 cross-user communication to support useful priority, interrupt, Task, Objective, and response triage without leaking excessive private context?

### Subquestions

1. Which fields are required for Phase 3: sender Identity, receiver Identity, source system, raw body, message kind, topic, priority, interrupt recommendation, response expectation, deadline, assumptions, ambiguities, provenance, confidence, disclosure policy, and Compartment?
2. Which fields may reference local-only objects without disclosing hidden identifiers or private Objective/Task details?
3. How should the sender specify whether the receiver may treat the message as a Task, status update, blocker, commitment, or FYI?
4. How should a receiver's UbU convert envelope metadata into Calendar interruption, communication review, or Task-creation suggestions?
5. What provenance and confidence markings are required when metadata is inferred rather than explicitly provided?
6. What minimum user controls prevent accidental disclosure of Relationship, Association, Objective, or Compartment state?

### Current direction

Phase 3 should include a small context-rich messaging envelope as a premier feature. The first useful version should prioritize message kind, topic, priority, interrupt recommendation, response expectation, assumptions, ambiguities, and provenance over deep relationship reconstruction.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "openai-gpt-5-codex-UBU-Q0066-20260919",
    "provider_id": "openai",
    "model_name": "gpt-5-codex",
    "question_id": "UBU-Q0066",
    "base_commit": "69ad3c7479877b7da91311ab728099a9c3963cf7",
    "summary": "Resolve UBU-Q0066 by adding UBU-D0270 and DESIGN.md \u00a71.3 for a minimal Phase 3 Message Context Envelope that supports triage while preserving Compartment and disclosure boundaries.",
    "rationale": "The proposal keeps the envelope intentionally small: enough sender-provided and receiver-inferred metadata to support priority, interruption, response, and Task-review workflows, while treating local Objectives, Tasks, Relationships, Associations, Compartments, and hidden identifiers as non-exportable unless explicitly projected. It aligns with existing Snapshot, Association, and AssociationAttestation semantics by requiring provenance, confidence, partial-assertion behavior, correction paths, and review before receiver-side mutation.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\nindex bd0d30a..4706040 100644\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -4176,3 +4176,21 @@ Review status distinguishes `candidate`, `accepted`, `rejected`, `disputed`, `an\n Disclosure policy is part of the attestation, not an afterthought. Private or permissioned attestations must carry Compartment refs, projection policy, allowed audience, redaction behavior, and whether the existence of the claim may be revealed. Exported or cross-user views must not leak membership, role, relationship, non-membership, or dispute facts through notifications, IDs, counts, search results, or reconciliation hints unless the disclosure policy and actor capability permit that exposure.\n \n AssociationAttestations may support Association reconciliation, organizational introspection, pseudonymous reputation, and multi-user trust only as evidence-bearing reviewed claims. They do not grant capability, settle authority, publish private Association state, or create organization-mode legitimacy unless a separate accepted authority, governance, or capability record permits that effect.\n+\n+---\n+\n+## UBU-D0270: Message Context Envelopes carry minimal triage metadata without private reconstruction\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a71.3. Resolves `UBU-Q0066`.\n+\n+UbU introduces a Phase 3 `MessageContextEnvelope` as a small cross-user communication projection for useful priority, interruption, response, Task, Objective, and blocker triage. The envelope is not a SyncStatement, Relationship record, Association record, Objective disclosure, or proof of authority; it is a sender-scoped message plus bounded metadata that the receiver may inspect, review, and map into local suggestions.\n+\n+The minimum envelope fields are `envelope_id`, `schema_version`, `sent_at` or observed timestamp, `sender_identity_ref`, `receiver_identity_ref`, `source_system`, optional `source_message_ref`, `body` or `body_ref`, `message_kind`, `topic`, `priority`, `interrupt_recommendation`, `response_expectation`, optional `deadline`, `receiver_treatment_hint`, `assumptions`, `ambiguities`, `provenance`, `confidence`, `compartment_ids`, and `disclosure_policy`. `message_kind` distinguishes at least request, question, status update, blocker, commitment, decision, and FYI. `receiver_treatment_hint` states whether the receiver may consider the message as a Task candidate, status update, blocker notice, commitment notice, or FYI, without granting authority to create accepted state.\n+\n+Local-only objects may be referenced only through envelope-scoped opaque handles, redacted labels, or user-approved summaries. Sender-local Objective, Task, Relationship, Association, Compartment, or attestation ids are not exported as stable identifiers unless the disclosure policy explicitly permits projection to the receiver. Hidden context must not leak through ids, counts, provenance paths, topic labels, search hints, notification text, or reconciliation metadata.\n+\n+A receiver's UbU may translate envelope metadata into candidate actions: high interrupt recommendations can create Calendar interruption or focus-review suggestions; response expectations and deadlines can create communication-review reminders; Task, blocker, or commitment hints can create Task-creation or dependency-review candidates. These mappings remain receiver-local suggestions with provenance and confidence and require ordinary user or policy approval before becoming accepted Tasks, Calendar changes, commitments, Relationship facts, AssociationAttestations, or Objective state.\n+\n+Explicit sender-provided metadata and inferred metadata are distinguishable per field. Inferred priority, topic, interrupt recommendation, response expectation, deadline, assumptions, ambiguities, or treatment hints must record source evidence, inference mechanism or model/template identifier, timestamp, confidence, redaction level, and review status. Confidence is advisory and never overrides explicit user declarations, accepted corrections, Compartment policy, or receiver-side authority checks.\n+\n+Minimum user controls are export preview, per-field redaction, Compartment and disclosure-policy validation, reveal-existence control, local-only reference stripping, provenance inspection, correction/revocation after send where transport permits, and receiver-side review before local mutation. Defaults should minimize disclosure: raw private context, Relationship state, Association state, Objective details, Task details, and Compartment names stay local unless the sender explicitly approves their projection.\ndiff --git a/DESIGN.md b/DESIGN.md\nindex df0f625..ad81ccf 100644\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -68,6 +68,22 @@ An Association becomes formal enough to justify `organization_mode` only when it\n \n For Phase 3, UbU needs the minimal Association record, lifecycle state, links to GroupMembership and RoleAssignment claims, Objective and commitment references, Compartment/disclosure policy, confidence, provenance, and correction/revocation paths. Cross-user reconciliation, dispute semantics, public organizational introspection, pseudonymous reputation, norm conflict analysis, collective-identity dynamics, and Skill Barter governance remain later research or separately resolved questions.\n \n+### 1.3 Message Context Envelope\n+\n+A `MessageContextEnvelope` is a Phase 3 cross-user communication projection that carries enough context for triage without reconstructing the sender's private state. It is not a SyncStatement, Relationship record, Association record, Objective disclosure, or proof of authority; it is a message plus bounded metadata that the receiver may inspect and map into local suggestions.\n+\n+The minimum envelope record contains `envelope_id`, `schema_version`, `sent_at` or observed timestamp, `sender_identity_ref`, `receiver_identity_ref`, `source_system`, optional `source_message_ref`, `body` or `body_ref`, `message_kind`, `topic`, `priority`, `interrupt_recommendation`, `response_expectation`, optional `deadline`, `receiver_treatment_hint`, `assumptions`, `ambiguities`, `provenance`, `confidence`, `compartment_ids`, and `disclosure_policy`. `message_kind` distinguishes at least `request`, `question`, `status_update`, `blocker`, `commitment`, `decision`, and `fyi`. `receiver_treatment_hint` states whether the receiver may consider the message as a Task candidate, status update, blocker notice, commitment notice, or FYI.\n+\n+The envelope may reference local-only objects only through envelope-scoped opaque handles, redacted labels, or user-approved summaries. Sender-local Objective, Task, Relationship, Association, Compartment, or attestation ids are not exported as stable identifiers unless the disclosure policy explicitly permits that projection to the receiver. Hidden context must not leak through ids, counts, provenance paths, topic labels, search hints, notification text, or reconciliation metadata.\n+\n+Sender intent is advisory unless backed by separate authority. A sender may ask the receiver to treat a message as a Task, blocker, commitment, status update, or FYI, but the receiver's UbU records only a candidate review item until the receiver accepts it or a receiver-side policy grants authority. The envelope must not create accepted Tasks, Calendar changes, commitments, Relationship facts, AssociationAttestations, or Objective state by itself.\n+\n+A receiver's UbU may translate envelope metadata into local suggestions: high interrupt recommendations can create Calendar interruption or focus-review suggestions; response expectations and deadlines can create communication-review reminders; Task, blocker, or commitment hints can create Task-creation or dependency-review candidates. These mappings preserve envelope provenance and confidence and remain correctable, rejectable, and revocable according to local policy.\n+\n+Provenance is per-field when metadata is inferred. Inferred priority, topic, interrupt recommendation, response expectation, deadline, assumptions, ambiguities, or treatment hints must record source evidence, inference mechanism or model/template identifier, timestamp, confidence, redaction level, and review status. Confidence is advisory and never overrides explicit user declarations, accepted corrections, Compartment policy, or receiver-side authority checks.\n+\n+Minimum controls are export preview, per-field redaction, Compartment and disclosure-policy validation, reveal-existence control, local-only reference stripping, provenance inspection, correction/revocation after send where transport permits, and receiver-side review before local mutation. Defaults minimize disclosure: raw private context, Relationship state, Association state, Objective details, Task details, and Compartment names stay local unless the sender explicitly approves their projection.\n+\n ---\n \n ## 2. Core Principles\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\nindex d4aafd2..0740e5a 100644\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -688,7 +688,7 @@ Open.\n \n ## UBU-Q0066: Minimal Phase 3 Message Context Envelope schema\n \n-Status: Open Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0061, UBU-Q0062 Blocks: Phase 3 cross-user communication, message priority triage, UbU-to-UbU interoperability Resolved by: None Last scored: 2026-09-18 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0061, UBU-Q0062 Blocks: Phase 3 cross-user communication, message priority triage, UbU-to-UbU interoperability Resolved by: UBU-D0270 Last scored: 2026-09-18 Scored from commit: None\n \n ### Question\n \n@@ -709,7 +709,7 @@ Phase 3 should include a small context-rich messaging envelope as a premier feat\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0270`: Phase 3 uses a minimal `MessageContextEnvelope` with sender and receiver Identity refs, source metadata, body or body ref, message kind, topic, priority, interrupt recommendation, response expectation, optional deadline, receiver treatment hint, assumptions, ambiguities, provenance, confidence, Compartment refs, and disclosure policy. Local objects may be referenced only through envelope-scoped opaque handles or redacted labels, inferred metadata remains provenance-marked and non-authoritative, and receiver UbU instances may create interruption, communication-review, or Task-creation suggestions but not mutate state without policy or user approval.\n \n ---\n \n",
    "commit_message": "Resolve UBU-Q0066 message context envelope schema",
    "validation_notes": [
      "Line anchors were checked against the workspace at base commit 69ad3c7479877b7da91311ab728099a9c3963cf7.",
      "Patch is limited to DESIGN.md, DECISIONS.md, and OPEN_QUESTIONS.md as allowed.",
      "OPEN_QUESTIONS.md preserves the single-line metadata format and anchors the resolution in the UBU-Q0066 block.",
      "Patch required --recount normalization."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0066"
    ],
    "decisions_added": [
      "UBU-D0270"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "openai-gpt-5-codex-UBU-Q0066-20260919",
    "patch_applies": true,
    "allowlist_passed": true,
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "error": null,
    "normalized_patch": "diff --git a/DECISIONS.md b/DECISIONS.md\nindex bd0d30a..4706040 100644\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -4176,3 +4176,21 @@ Review status distinguishes `candidate`, `accepted`, `rejected`, `disputed`, `an\n Disclosure policy is part of the attestation, not an afterthought. Private or permissioned attestations must carry Compartment refs, projection policy, allowed audience, redaction behavior, and whether the existence of the claim may be revealed. Exported or cross-user views must not leak membership, role, relationship, non-membership, or dispute facts through notifications, IDs, counts, search results, or reconciliation hints unless the disclosure policy and actor capability permit that exposure.\n \n AssociationAttestations may support Association reconciliation, organizational introspection, pseudonymous reputation, and multi-user trust only as evidence-bearing reviewed claims. They do not grant capability, settle authority, publish private Association state, or create organization-mode legitimacy unless a separate accepted authority, governance, or capability record permits that effect.\n+\n+---\n+\n+## UBU-D0270: Message Context Envelopes carry minimal triage metadata without private reconstruction\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a71.3. Resolves `UBU-Q0066`.\n+\n+UbU introduces a Phase 3 `MessageContextEnvelope` as a small cross-user communication projection for useful priority, interruption, response, Task, Objective, and blocker triage. The envelope is not a SyncStatement, Relationship record, Association record, Objective disclosure, or proof of authority; it is a sender-scoped message plus bounded metadata that the receiver may inspect, review, and map into local suggestions.\n+\n+The minimum envelope fields are `envelope_id`, `schema_version`, `sent_at` or observed timestamp, `sender_identity_ref`, `receiver_identity_ref`, `source_system`, optional `source_message_ref`, `body` or `body_ref`, `message_kind`, `topic`, `priority`, `interrupt_recommendation`, `response_expectation`, optional `deadline`, `receiver_treatment_hint`, `assumptions`, `ambiguities`, `provenance`, `confidence`, `compartment_ids`, and `disclosure_policy`. `message_kind` distinguishes at least request, question, status update, blocker, commitment, decision, and FYI. `receiver_treatment_hint` states whether the receiver may consider the message as a Task candidate, status update, blocker notice, commitment notice, or FYI, without granting authority to create accepted state.\n+\n+Local-only objects may be referenced only through envelope-scoped opaque handles, redacted labels, or user-approved summaries. Sender-local Objective, Task, Relationship, Association, Compartment, or attestation ids are not exported as stable identifiers unless the disclosure policy explicitly permits projection to the receiver. Hidden context must not leak through ids, counts, provenance paths, topic labels, search hints, notification text, or reconciliation metadata.\n+\n+A receiver's UbU may translate envelope metadata into candidate actions: high interrupt recommendations can create Calendar interruption or focus-review suggestions; response expectations and deadlines can create communication-review reminders; Task, blocker, or commitment hints can create Task-creation or dependency-review candidates. These mappings remain receiver-local suggestions with provenance and confidence and require ordinary user or policy approval before becoming accepted Tasks, Calendar changes, commitments, Relationship facts, AssociationAttestations, or Objective state.\n+\n+Explicit sender-provided metadata and inferred metadata are distinguishable per field. Inferred priority, topic, interrupt recommendation, response expectation, deadline, assumptions, ambiguities, or treatment hints must record source evidence, inference mechanism or model/template identifier, timestamp, confidence, redaction level, and review status. Confidence is advisory and never overrides explicit user declarations, accepted corrections, Compartment policy, or receiver-side authority checks.\n+\n+Minimum user controls are export preview, per-field redaction, Compartment and disclosure-policy validation, reveal-existence control, local-only reference stripping, provenance inspection, correction/revocation after send where transport permits, and receiver-side review before local mutation. Defaults should minimize disclosure: raw private context, Relationship state, Association state, Objective details, Task details, and Compartment names stay local unless the sender explicitly approves their projection.\ndiff --git a/DESIGN.md b/DESIGN.md\nindex df0f625..ad81ccf 100644\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -68,6 +68,22 @@ An Association becomes formal enough to justify `organization_mode` only when it\n \n For Phase 3, UbU needs the minimal Association record, lifecycle state, links to GroupMembership and RoleAssignment claims, Objective and commitment references, Compartment/disclosure policy, confidence, provenance, and correction/revocation paths. Cross-user reconciliation, dispute semantics, public organizational introspection, pseudonymous reputation, norm conflict analysis, collective-identity dynamics, and Skill Barter governance remain later research or separately resolved questions.\n \n+### 1.3 Message Context Envelope\n+\n+A `MessageContextEnvelope` is a Phase 3 cross-user communication projection that carries enough context for triage without reconstructing the sender's private state. It is not a SyncStatement, Relationship record, Association record, Objective disclosure, or proof of authority; it is a message plus bounded metadata that the receiver may inspect and map into local suggestions.\n+\n+The minimum envelope record contains `envelope_id`, `schema_version`, `sent_at` or observed timestamp, `sender_identity_ref`, `receiver_identity_ref`, `source_system`, optional `source_message_ref`, `body` or `body_ref`, `message_kind`, `topic`, `priority`, `interrupt_recommendation`, `response_expectation`, optional `deadline`, `receiver_treatment_hint`, `assumptions`, `ambiguities`, `provenance`, `confidence`, `compartment_ids`, and `disclosure_policy`. `message_kind` distinguishes at least `request`, `question`, `status_update`, `blocker`, `commitment`, `decision`, and `fyi`. `receiver_treatment_hint` states whether the receiver may consider the message as a Task candidate, status update, blocker notice, commitment notice, or FYI.\n+\n+The envelope may reference local-only objects only through envelope-scoped opaque handles, redacted labels, or user-approved summaries. Sender-local Objective, Task, Relationship, Association, Compartment, or attestation ids are not exported as stable identifiers unless the disclosure policy explicitly permits that projection to the receiver. Hidden context must not leak through ids, counts, provenance paths, topic labels, search hints, notification text, or reconciliation metadata.\n+\n+Sender intent is advisory unless backed by separate authority. A sender may ask the receiver to treat a message as a Task, blocker, commitment, status update, or FYI, but the receiver's UbU records only a candidate review item until the receiver accepts it or a receiver-side policy grants authority. The envelope must not create accepted Tasks, Calendar changes, commitments, Relationship facts, AssociationAttestations, or Objective state by itself.\n+\n+A receiver's UbU may translate envelope metadata into local suggestions: high interrupt recommendations can create Calendar interruption or focus-review suggestions; response expectations and deadlines can create communication-review reminders; Task, blocker, or commitment hints can create Task-creation or dependency-review candidates. These mappings preserve envelope provenance and confidence and remain correctable, rejectable, and revocable according to local policy.\n+\n+Provenance is per-field when metadata is inferred. Inferred priority, topic, interrupt recommendation, response expectation, deadline, assumptions, ambiguities, or treatment hints must record source evidence, inference mechanism or model/template identifier, timestamp, confidence, redaction level, and review status. Confidence is advisory and never overrides explicit user declarations, accepted corrections, Compartment policy, or receiver-side authority checks.\n+\n+Minimum controls are export preview, per-field redaction, Compartment and disclosure-policy validation, reveal-existence control, local-only reference stripping, provenance inspection, correction/revocation after send where transport permits, and receiver-side review before local mutation. Defaults minimize disclosure: raw private context, Relationship state, Association state, Objective details, Task details, and Compartment names stay local unless the sender explicitly approves their projection.\n+\n ---\n \n ## 2. Core Principles\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\nindex d4aafd2..0740e5a 100644\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -688,7 +688,7 @@ Open.\n \n ## UBU-Q0066: Minimal Phase 3 Message Context Envelope schema\n \n-Status: Open Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0061, UBU-Q0062 Blocks: Phase 3 cross-user communication, message priority triage, UbU-to-UbU interoperability Resolved by: None Last scored: 2026-09-18 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0061, UBU-Q0062 Blocks: Phase 3 cross-user communication, message priority triage, UbU-to-UbU interoperability Resolved by: UBU-D0270 Last scored: 2026-09-18 Scored from commit: None\n \n ### Question\n \n@@ -709,7 +709,7 @@ Phase 3 should include a small context-rich messaging envelope as a premier feat\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0270`: Phase 3 uses a minimal `MessageContextEnvelope` with sender and receiver Identity refs, source metadata, body or body ref, message kind, topic, priority, interrupt recommendation, response expectation, optional deadline, receiver treatment hint, assumptions, ambiguities, provenance, confidence, Compartment refs, and disclosure policy. Local objects may be referenced only through envelope-scoped opaque handles or redacted labels, inferred metadata remains provenance-marked and non-authoritative, and receiver UbU instances may create interruption, communication-review, or Task-creation suggestions but not mutate state without policy or user approval.\n \n ---\n \n",
    "warnings": [
      "Patch required --recount normalization."
    ],
    "ordinary_error": "error: corrupt patch at line 27",
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
