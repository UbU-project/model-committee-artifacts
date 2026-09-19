# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0088`  
Question title: `Extrospection finding lifecycle and durable rejection semantics`  
Base commit: `e652dc4fcfb46284cfe5f39da3f38739cf67ad37`

```markdown
## UBU-Q0088: Extrospection finding lifecycle and durable rejection semantics

Status: Open Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0062 Blocks: extrospection review, candidate/canonical distinction, state-transition cockpit Resolved by: None Last scored: 2026-09-18 Scored from commit: None

### Question

How should candidate, deferred, resurfaced, accepted, rejected, superseded, and archived extrospection findings behave?

### Subquestions

1. What state transitions are valid for an `ExtrospectionFinding`?
2. What model influence is prohibited before user review?
3. How should deferred findings resurface when new evidence accumulates?
4. What data should a rejected finding retain to suppress repeated bad framings?
5. How should accepted findings mutate Relationship state, if at all?

### Current direction

Unreviewed findings must not silently update durable state. Rejection is a first-class correction path, not merely dismissal.

Phase 1b split: the candidate-lifecycle and durable-rejection subset of this question is decomposed into UBU-Q0138 and answered under the Phase 1b rule, because Phase 1b's advisory review queue already implements durable rejection as dedup-against-rejected history and must express each candidate as a first-class labeled object. `ExtrospectionFinding`-specific state transitions and the effect of accepted findings on Relationship state remain open here, since extrospection and the association model are deferred.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "ubu-q0088-gpt5-codex-d0273",
    "provider_id": "openai",
    "model_name": "gpt-5-codex",
    "question_id": "UBU-Q0088",
    "base_commit": "e652dc4fcfb46284cfe5f39da3f38739cf67ad37",
    "summary": "Resolve UBU-Q0088 by defining ExtrospectionFinding as an evidence-backed review candidate with explicit lifecycle states, durable rejection keys, resurfacing rules, and constrained Relationship effects.",
    "rationale": "The existing design already states that extrospection findings are reviewable candidates and that AssociationAttestations use immutable, evidence-backed review status. This proposal makes the remaining extrospection-specific semantics explicit: unaccepted findings can only shape review surfaces, rejected findings retain privacy-minimized suppression metadata, resurfacing requires materially new evidence or user/policy triggers, and accepted findings mutate Relationship state only through ordinary append-only admitted records rather than direct overwrite.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -1187,27 +1187,27 @@\n ## UBU-Q0088: Extrospection finding lifecycle and durable rejection semantics\n \n-Status: Open Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0062 Blocks: extrospection review, candidate/canonical distinction, state-transition cockpit Resolved by: None Last scored: 2026-09-18 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0062 Blocks: extrospection review, candidate/canonical distinction, state-transition cockpit Resolved by: UBU-D0273 Last scored: 2026-09-18 Scored from commit: None\n \n ### Question\n \n How should candidate, deferred, resurfaced, accepted, rejected, superseded, and archived extrospection findings behave?\n \n ### Subquestions\n \n 1. What state transitions are valid for an `ExtrospectionFinding`?\n 2. What model influence is prohibited before user review?\n 3. How should deferred findings resurface when new evidence accumulates?\n 4. What data should a rejected finding retain to suppress repeated bad framings?\n 5. How should accepted findings mutate Relationship state, if at all?\n \n ### Current direction\n \n Unreviewed findings must not silently update durable state. Rejection is a first-class correction path, not merely dismissal.\n \n Phase 1b split: the candidate-lifecycle and durable-rejection subset of this question is decomposed into UBU-Q0138 and answered under the Phase 1b rule, because Phase 1b's advisory review queue already implements durable rejection as dedup-against-rejected history and must express each candidate as a first-class labeled object. `ExtrospectionFinding`-specific state transitions and the effect of accepted findings on Relationship state remain open here, since extrospection and the association model are deferred.\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0273`: `ExtrospectionFinding` records use the lifecycle states `candidate`, `deferred`, `resurfaced`, `accepted`, `rejected`, `superseded`, and `archived`. Unaccepted findings may affect only review ordering, explanations, and local suggestions, not durable Relationship, Association, Preference, trust, capability, projection, or external-action state. Deferred findings resurface only on materially new evidence, user request, policy-triggered review, or relevant accepted Relationship change. Rejected findings retain privacy-minimized suppression keys, normalized claim shape, evidence hashes, rejection reason, actor, authority source, and retention policy so repeated bad framings can be suppressed without treating the rejected claim as truth. Accepted findings do not directly rewrite Relationship state; they admit separate scoped Relationship observations, annotations, Snapshots, Logs, or AssociationAttestations through ordinary authority, provenance, Compartment, and correction rules.\n \n ---\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -328,9 +328,23 @@\n Extrospection is the Relationship-level sibling of introspection and organizational introspection. It exists because Relationships have **epistemic asymmetry**: UbU can model the user's own perspective from first-party declarations, behavior, affect history, and reflection, but it can only maintain evidence-backed hypotheses about another party's perspective.\n \n Extrospection does not reveal, prove, or authoritatively attest to another person's inner state. It tests the user's counterparty-perspective hypotheses, declared Relationship scopes, trust assumptions, reciprocity assumptions, and boundary expectations against permitted Relationship evidence. Its UX should confront the user with evidence in tension, including supporting evidence, disconfirming evidence, ambiguity, and possible clarification paths, while preserving affect-aware framing and user sovereignty.\n \n Extrospection findings are candidate, reviewable state. They may inform introspection when the evidence primarily concerns the user's own behavior, but they must not silently mutate durable Relationship models before user review.\n \n+An `ExtrospectionFinding` is an evidence-backed candidate over Relationship, Association, message, Log, Snapshot, External Reference, or user-provided evidence. The minimum record carries finding id, Relationship or affected Identity refs, finding kind, normalized claim shape, evidence refs or hashes, supporting and disconfirming notes, assumptions and ambiguities, confidence, Compartment refs, disclosure policy, provenance, review status, surfaced-at time, and correction, rejection, revocation, supersession, or archive links.\n+\n+Lifecycle states are `candidate`, `deferred`, `resurfaced`, `accepted`, `rejected`, `superseded`, and `archived`. New findings begin as `candidate`; a candidate may be accepted, rejected, deferred, superseded by a replacement candidate, or archived as no longer useful. A deferred finding becomes `resurfaced` only when materially new evidence, a user request, a policy-triggered review interval, or an accepted Relationship change makes the prior deferral worth reviewing again. Accepted, rejected, and superseded findings remain historical records and may later be archived according to retention policy, but archiving is not erasure unless a separate deletion or redaction rule permits it.\n+\n+Unreviewed, deferred, resurfaced, rejected, and superseded findings are not admitted state. They may influence review ordering, explanations, clarification prompts, risk warnings, and local candidate suggestions, but they must not mutate Relationship state, Association state, Preferences, trust or reputation scores, capability grants, disclosure policy, projection state, external messages, or external actions. They also must not be used as hidden planner constraints that treat the counterparty hypothesis as settled fact.\n+\n+Deferred findings resurface by linking the old finding, the new evidence or trigger, and the reason the prior deferral no longer controls. Resurfacing preserves the earlier decision and presents the combined evidence as a review item rather than quietly promoting the old candidate.\n+\n+Rejected findings retain a durable, privacy-minimized rejection key sufficient to suppress repeated bad framings: normalized finding kind, subject and scope, polarity or asserted value shape, evidence hashes or source fingerprints, model or extractor version, prompt/template digest when relevant, Compartment and redaction class, rejection reason or user correction, deciding actor, authority source, decided-at time, and retention or purge policy. The rejection record must not make the rejected claim true, exportable, or more widely visible merely because it is retained for deduplication.\n+\n+Accepted findings do not rewrite a Relationship object in place. Acceptance admits a separate scoped Relationship observation, annotation, Snapshot, Log entry, AssociationAttestation, or correction record with its own authority, provenance, confidence, Compartment, and effective interval. Corrected query views may use those admitted records under the ordinary Snapshot and attestation rules, but counterparty-perspective claims remain hypotheses unless backed by an accepted source with authority for the claim.\n+\n+Superseded findings point to the replacement finding or admitted record and leave the older framing queryable for audit, correction, and rejection-dedup purposes. Supersession is the ordinary path for narrowing an overbroad finding, replacing unsupported framing with a better one, or updating a finding after Relationship evidence changes.\n+\n ### 2.10.2 Autonomy-first safeguards\n \n Relationship, extrospection, and RelationshipScopeTransition safeguards should be wise default-on recommendations, not paternalistic hard gates. UbU may strongly recommend pre-action introspection, extrospection, TrustCalibrationAssessment, power/vulnerability review, pacing review, graceful nonachievement planning, and manipulation-risk review. The user may bypass or disable advisory safeguards unless doing so violates a structurally enforceable hard boundary, a self-imposed user-configured required gate, or an unavoidable external provider/platform/legal constraint.\ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -4228,5 +4228,25 @@\n Model-noticed elapsed time is evidence, not canonical semantics. A realtime model may report that time seems to have passed, a user may have been interrupted, or a planned interval may differ from observed activity, but planner-valid meaning is created only by accepted `plan_realized`, Task lifecycle, Calendar, Snapshot, or Log records. Unknown, private, rest, interruption, different-Task, and quick-note intervals preserve uncertainty and must not imply Preference changes, Objective failure, habit patterns, or moral judgment.\n \n Mandatory provenance for realtime observations includes source kind, observation or capture interval, Device and backend refs, model or tool identifier, prompt/template or schema version where applicable, evidence refs or payload hashes, source Compartment, redaction level, processing route, confidence, and review status. Audio, video, screen, keystroke, raw message body, raw file, raw GPS, and similarly sensitive inputs require explicit source enablement, visible capture state, and Compartment approval before they can produce candidates.\n \n Routing is conservative by default. Raw sensitive capture, `no_cloud_llm` Compartments, and externally restricted Compartments are local-only or prohibited according to policy. Cloud processing is permitted only for policy-approved, visible, intentionally provided or redacted inputs. Cross-user projection, external export, covert or broad background capture, hidden persuasion timing, and automatic external mutation remain prohibited unless a later explicit mode, Compartment policy, capability grant, and user approval authorize the exact behavior.\n+\n+---\n+\n+## UBU-D0273: Extrospection findings are durable review candidates, not Relationship truth\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a72.10.1. Resolves `UBU-Q0088`.\n+\n+An `ExtrospectionFinding` is an evidence-backed review candidate about a Relationship or affected Identity, not a fact about another person's inner state. The record carries a finding id, affected refs, finding kind, normalized claim shape, evidence refs or hashes, supporting and disconfirming notes, assumptions, ambiguities, confidence, Compartment and disclosure policy, provenance, surfaced-at time, review status, and correction, rejection, revocation, supersession, or archive links.\n+\n+The valid lifecycle states are `candidate`, `deferred`, `resurfaced`, `accepted`, `rejected`, `superseded`, and `archived`. Findings begin as candidates. Candidates may be accepted, rejected, deferred, superseded by a replacement, or archived. Deferred findings may resurface only when materially new evidence, a user request, a policy-triggered review interval, or an accepted Relationship change makes review newly relevant. Accepted, rejected, and superseded findings remain historical records and may later be archived under retention policy.\n+\n+Before acceptance, extrospection findings may influence only review surfaces: ordering, explanation, clarification prompts, risk warnings, and local candidate suggestions. They must not mutate Relationship state, Association state, Preferences, trust or reputation scores, capability grants, disclosure policy, projection state, external messages, or external actions, and they must not become hidden planner constraints that treat a counterparty hypothesis as settled fact.\n+\n+Deferral preserves uncertainty rather than hiding it. A resurfaced finding links the prior deferral, the new evidence or trigger, and the reason resurfacing is justified, so the user can see both the old decision and the new support. Resurfacing creates a review item; it does not promote the old candidate automatically.\n+\n+Rejection is durable correction metadata, not mere dismissal. A rejected finding retains a privacy-minimized suppression key: normalized finding kind, subject and scope, polarity or asserted-value shape, evidence hashes or source fingerprints, model or extractor version, prompt/template digest when relevant, Compartment and redaction class, rejection reason or user correction, deciding actor, authority source, decided-at time, and retention or purge policy. Retaining this key suppresses repeated bad framings without making the rejected claim accepted, exportable, or more visible.\n+\n+Accepting an extrospection finding admits a separate scoped Relationship observation, annotation, Snapshot, Log entry, AssociationAttestation, or correction record through ordinary authority, provenance, confidence, Compartment, and effective-interval rules. The finding itself remains provenance for the decision and does not rewrite the Relationship object in place. Counterparty-perspective claims remain hypotheses unless backed by an accepted source with authority for that claim.\n+\n+Supersession is the ordinary path for narrowing an overbroad finding, replacing unsupported framing, or updating after Relationship evidence changes. Superseded findings point to the replacement finding or admitted record and remain queryable for audit, correction, and rejection-deduplication.\n",
    "commit_message": "Resolve extrospection finding lifecycle semantics",
    "validation_notes": [
      "Read OPEN_QUESTIONS.md, DESIGN.md, and DECISIONS.md around UBU-Q0088, DESIGN \u00a72.10.1, and the latest decision ids.",
      "Patch is scoped to the allowed canonical files and uses UBU-D0273 as the next free decision id.",
      "Workspace is read-only, so the patch was constructed but not applied or checked with git apply."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0088"
    ],
    "decisions_added": [
      "UBU-D0273"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "ubu-q0088-gpt5-codex-d0273",
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
