# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0056`  
Question title: `Game theory, strategic interaction, and counterparty modeling`  
Base commit: `3cf0877469f87458f8b605bf1340efbbc3e3cbcf`

```markdown
## UBU-Q0056: Game theory, strategic interaction, and counterparty modeling

Status: Open Priority: Post-MVP Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0025 Blocks: Strategic interaction modeling, counterparty incentives, post-MVP coordination protocols Resolved by: None Last scored: 2026-09-18 Scored from commit: None

### Question

How should game theory affect UbU's model of strategic interaction, counterparty incentives, commitments, and adversarial or cooperative behavior?

### Subquestions

1. Which strategic patterns can be represented through External Events and Techniques?
2. Which patterns require explicit strategic-interaction objects or counterparty models?
3. How should UbU represent credible commitments, signaling, free-riding, principal-agent problems, bargaining, and trust-but-verify workflows?
4. How should strategic reasoning interact with Identity, capability grants, Compartments, and external projections?
5. How should UbU avoid overconfident game-theoretic recommendations when the counterparty model is speculative?
6. Which strategic-interaction features are useful for FOSS coordination, skilled contributors, grants, issue triage, bug bounties, and future Skill Barter systems?

### Current direction

Many strategic patterns can be represented through External Events and reusable Techniques. Deeper counterparty modeling and strategic-interaction primitives are post-MVP research areas and should not block Phase 1.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "codex-ubu-q0056-strategic-boundary-v1",
    "provider_id": "openai",
    "model_name": "gpt-5-codex",
    "question_id": "UBU-Q0056",
    "base_commit": "3cf0877469f87458f8b605bf1340efbbc3e3cbcf",
    "summary": "Resolve UBU-Q0056 by treating game theory as a boundary discipline: Phase 1/1b uses External Events, Techniques, Tasks, Logs, Relationships, Identity, capability grants, Compartments, and External References for ordinary strategic interaction, while explicit strategic-interaction and counterparty model objects are deferred to Phase 3+/Skill Barter contexts with strong epistemic and authority safeguards.",
    "rationale": "This keeps the MVP and Phase 1b model small while preserving a clear upgrade path for bargaining, commitments, trust-but-verify workflows, principal-agent problems, FOSS coordination, bug bounties, grants, and future marketplace/barter systems. It also prevents speculative counterparty models from becoming overconfident recommendations or hidden manipulation engines.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\nindex 09310c8..c496ef8 100644\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -4120,3 +4120,21 @@ Aggregated outputs become organization directives, policy statements, or coordin\n Dissent, minority reports, unresolved objections, abstentions, veto use, appeals, and exit-right assertions remain durable, queryable records. Procedural validity may make a directive binding inside its scope, but it does not erase disagreement or prevent later correction, revocation, appeal, or exit analysis.\n \n UbU should surface social-choice and legitimacy problems as diagnostics rather than hiding them behind aggregation. Examples include agenda dependence, cyclic majorities, strategic-voting incentives, delegation-chain fragility, quorum or participation defects, veto abuse, minority-right conflicts, and consent defects caused by high exit cost or unclear authority. Phase 1 and Phase 1b remain single-user and admin-equivalent; this decision preserves the semantics needed for later Phase 3 governance without blocking current implementation.\n+\n+---\n+\n+## UBU-D0267: Strategic interaction is advisory until backed by authority and evidence\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a726.4. Resolves `UBU-Q0056`.\n+\n+UbU adopts game theory as a boundary discipline for coordination, not as a hidden optimizer for manipulating counterparties. Strategic reasoning may identify incentives, commitment problems, signaling, bargaining structure, verification needs, free-riding risk, principal-agent concerns, and possible cooperation failures, but it remains advisory unless backed by accepted records and explicit authority.\n+\n+Most Phase 1 and Phase 1b strategic patterns should be represented through existing objects: External Events for observed counterparty actions and outside-world triggers; Techniques for reusable coordination workflows; Tasks and Logs for user-controlled actions and outcomes; External References for issue trackers, grants, bounties, agreements, or messages; Relationships and organization-mode UniverseState for accepted state; and Identity, capability grants, Compartments, and projection policy for authority, disclosure, and routing limits.\n+\n+Dedicated strategic-interaction or counterparty model objects are deferred to Phase 3+ and future Skill Barter/marketplace work. When introduced, they should be provenance-bearing hypothesis records with actor Identity refs, hypothesized incentives, strategic-situation type, commitment or verification mechanism, bargaining or principal-agent structure, expected consequence ranges, assumptions, evidence refs, confidence, Compartment refs, authority source, review status, and correction/revocation links.\n+\n+Counterparty models must not be treated as facts. A speculative model may create a review item, explanation, risk warning, or option comparison, but it must not classify trustworthiness, restrict another person's options, disclose private data, expand capability grants, authorize external mutation, or create a commitment without ordinary review and approval. Recommendations must expose uncertainty and preserve plausible non-strategic explanations when evidence is weak.\n+\n+Credible commitments, signaling, trust-but-verify workflows, and free-riding controls should be represented as explicit mechanisms such as milestones, staged disclosure, escrow or deposit requirements, acceptance criteria, verification Tasks, audit evidence, revocation paths, and graceful failure states. Payoff framing should remain user-facing consequences for Objectives and constraints, not an opaque utility maximizer.\n+\n+For FOSS coordination, skilled contributors, grants, issue triage, bug bounties, and future Skill Barter systems, the useful near-term surface is pragmatic coordination: ownership clarity, stale-promise warnings, verification checklists, review commitments, grant or bounty terms, visible projection state, and mismatch diagnostics. Phase 1 and Phase 1b must not implement adversarial strategic optimization, automatic counterparty scoring, hidden persuasion timing, or automated bargaining authority.\ndiff --git a/DESIGN.md b/DESIGN.md\nindex 9566cb2..19697f5 100644\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -3588,6 +3588,22 @@ Social-choice limitations should be surfaced as diagnostics rather than hidden.\n \n Phase 1 and Phase 1b do not implement general multi-user governance. They preserve room for Phase 3 by keeping organization-mode authority explicit, logging actor Identity and provenance, retaining dissent/correction paths, and avoiding any schema claim that an organization's Preference is literally a person's Preference.\n \n+### 26.4 Strategic interaction boundary\n+\n+Game theory is a boundary discipline for coordination, not a hidden optimizer for manipulating counterparties. UbU may help the user notice incentives, commitments, verification needs, asymmetric information, and likely coordination failures, but strategic analysis remains advisory unless backed by accepted records and explicit authority.\n+\n+Phase 1 and Phase 1b represent most strategic interaction with existing objects: External Events record observed counterparty actions or outside-world triggers; Techniques encode reusable coordination workflows; Tasks and Logs record the user's controlled actions and outcomes; External References point to issue trackers, grant calls, bounties, agreements, or messages; Relationships and organization-mode UniverseState capture accepted state; Identity, capability grants, Compartments, and projection policy constrain who may act, what may be disclosed, and where evidence may be sent.\n+\n+Explicit strategic-interaction records are deferred to Phase 3+ or future Skill Barter/marketplace work. When introduced, they should model the strategic situation, actor Identity refs, hypothesized incentives, commitment or verification mechanisms, bargaining or principal-agent structure, expected consequence ranges, assumptions, evidence refs, confidence, Compartment refs, authority source, review status, and correction/revocation links.\n+\n+Counterparty models are hypotheses, not facts. A speculative model may create a review item, explanation, or option comparison, but it must not become canonical state, restrict a person's options, classify trustworthiness, disclose private data, expand a capability grant, or create a commitment without review. Recommendations should expose uncertainty and offer non-strategic interpretations when evidence is weak.\n+\n+Credible commitments, signaling, free-riding controls, bargaining, and trust-but-verify workflows should be represented as user-visible mechanisms: milestones, escrow or deposit requirements, acceptance criteria, verification Tasks, audit evidence, staged disclosure, revocation paths, and graceful failure states. Payoff language should be framed as consequences for Objectives and constraints rather than as an opaque utility maximizer.\n+\n+For FOSS dogfooding, the useful Phase 1b surface is pragmatic coordination: issue triage, contributor follow-up, review commitments, release promises, grant deadlines, bug bounty terms, verification checklists, and visible projection state. UbU may warn about bottlenecks, ambiguous ownership, unverified claims, stale promises, and incentive mismatch, but it should not optimize adversarial pressure or persuasion.\n+\n+Phase 1 and Phase 1b must not implement adversarial strategic optimization, automatic counterparty scoring, hidden persuasion timing, or automated bargaining authority. Later multi-user, marketplace, bounty, grant, or Skill Barter features must pass through the same Identity, capability, Compartment, provenance, projection, and human-review boundaries as other external coordination.\n+\n ---\n \n ## 27. GitHub Dogfooding and Projection\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\nindex ce4a217..290785c 100644\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -489,7 +489,7 @@ Resolved by `UBU-D0266`: UbU represents collective decisions as provenance-beari\n \n ## UBU-Q0056: Game theory, strategic interaction, and counterparty modeling\n \n-Status: Open Priority: Post-MVP Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0025 Blocks: Strategic interaction modeling, counterparty incentives, post-MVP coordination protocols Resolved by: None Last scored: 2026-09-18 Scored from commit: None\n+Status: Solved Priority: Post-MVP Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0025 Blocks: Strategic interaction modeling, counterparty incentives, post-MVP coordination protocols Resolved by: UBU-D0267 Last scored: 2026-09-18 Scored from commit: None\n \n ### Question\n \n@@ -510,7 +510,7 @@ Many strategic patterns can be represented through External Events and reusable\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0267`: UbU treats game theory as a boundary discipline. Phase 1 and Phase 1b represent ordinary strategic interaction through existing objects such as External Events, Techniques, Tasks, Logs, External References, Relationships, Identity, capability grants, Compartments, and projection policy. Explicit strategic-interaction and counterparty model objects are deferred to Phase 3+ / future Skill Barter or marketplace work, where they must remain hypothesis-based, provenance-bearing, confidence-scoped, reviewable, and unable to authorize commitments, disclosures, or adversarial optimization without explicit authority.\n \n ---\n \n",
    "commit_message": "Resolve UBU-Q0056 strategic interaction boundary",
    "validation_notes": [
      "Patch is limited to allowlisted files: OPEN_QUESTIONS.md, DESIGN.md, and DECISIONS.md.",
      "OPEN_QUESTIONS.md hunk is anchored on the selected UBU-Q0056 heading and updates only that question's metadata and resolution text.",
      "Workspace is read-only in this run, so the patch was not applied; hunk ranges were prepared from the inspected file line numbers at base commit 3cf0877469f87458f8b605bf1340efbbc3e3cbcf.",
      "Patch required --recount normalization."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0056"
    ],
    "decisions_added": [
      "UBU-D0267"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "codex-ubu-q0056-strategic-boundary-v1",
    "patch_applies": true,
    "allowlist_passed": true,
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "error": null,
    "normalized_patch": "diff --git a/DECISIONS.md b/DECISIONS.md\nindex 09310c8..c496ef8 100644\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -4120,3 +4120,21 @@ Aggregated outputs become organization directives, policy statements, or coordin\n Dissent, minority reports, unresolved objections, abstentions, veto use, appeals, and exit-right assertions remain durable, queryable records. Procedural validity may make a directive binding inside its scope, but it does not erase disagreement or prevent later correction, revocation, appeal, or exit analysis.\n \n UbU should surface social-choice and legitimacy problems as diagnostics rather than hiding them behind aggregation. Examples include agenda dependence, cyclic majorities, strategic-voting incentives, delegation-chain fragility, quorum or participation defects, veto abuse, minority-right conflicts, and consent defects caused by high exit cost or unclear authority. Phase 1 and Phase 1b remain single-user and admin-equivalent; this decision preserves the semantics needed for later Phase 3 governance without blocking current implementation.\n+\n+---\n+\n+## UBU-D0267: Strategic interaction is advisory until backed by authority and evidence\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a726.4. Resolves `UBU-Q0056`.\n+\n+UbU adopts game theory as a boundary discipline for coordination, not as a hidden optimizer for manipulating counterparties. Strategic reasoning may identify incentives, commitment problems, signaling, bargaining structure, verification needs, free-riding risk, principal-agent concerns, and possible cooperation failures, but it remains advisory unless backed by accepted records and explicit authority.\n+\n+Most Phase 1 and Phase 1b strategic patterns should be represented through existing objects: External Events for observed counterparty actions and outside-world triggers; Techniques for reusable coordination workflows; Tasks and Logs for user-controlled actions and outcomes; External References for issue trackers, grants, bounties, agreements, or messages; Relationships and organization-mode UniverseState for accepted state; and Identity, capability grants, Compartments, and projection policy for authority, disclosure, and routing limits.\n+\n+Dedicated strategic-interaction or counterparty model objects are deferred to Phase 3+ and future Skill Barter/marketplace work. When introduced, they should be provenance-bearing hypothesis records with actor Identity refs, hypothesized incentives, strategic-situation type, commitment or verification mechanism, bargaining or principal-agent structure, expected consequence ranges, assumptions, evidence refs, confidence, Compartment refs, authority source, review status, and correction/revocation links.\n+\n+Counterparty models must not be treated as facts. A speculative model may create a review item, explanation, risk warning, or option comparison, but it must not classify trustworthiness, restrict another person's options, disclose private data, expand capability grants, authorize external mutation, or create a commitment without ordinary review and approval. Recommendations must expose uncertainty and preserve plausible non-strategic explanations when evidence is weak.\n+\n+Credible commitments, signaling, trust-but-verify workflows, and free-riding controls should be represented as explicit mechanisms such as milestones, staged disclosure, escrow or deposit requirements, acceptance criteria, verification Tasks, audit evidence, revocation paths, and graceful failure states. Payoff framing should remain user-facing consequences for Objectives and constraints, not an opaque utility maximizer.\n+\n+For FOSS coordination, skilled contributors, grants, issue triage, bug bounties, and future Skill Barter systems, the useful near-term surface is pragmatic coordination: ownership clarity, stale-promise warnings, verification checklists, review commitments, grant or bounty terms, visible projection state, and mismatch diagnostics. Phase 1 and Phase 1b must not implement adversarial strategic optimization, automatic counterparty scoring, hidden persuasion timing, or automated bargaining authority.\ndiff --git a/DESIGN.md b/DESIGN.md\nindex 9566cb2..19697f5 100644\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -3588,6 +3588,22 @@ Social-choice limitations should be surfaced as diagnostics rather than hidden.\n \n Phase 1 and Phase 1b do not implement general multi-user governance. They preserve room for Phase 3 by keeping organization-mode authority explicit, logging actor Identity and provenance, retaining dissent/correction paths, and avoiding any schema claim that an organization's Preference is literally a person's Preference.\n \n+### 26.4 Strategic interaction boundary\n+\n+Game theory is a boundary discipline for coordination, not a hidden optimizer for manipulating counterparties. UbU may help the user notice incentives, commitments, verification needs, asymmetric information, and likely coordination failures, but strategic analysis remains advisory unless backed by accepted records and explicit authority.\n+\n+Phase 1 and Phase 1b represent most strategic interaction with existing objects: External Events record observed counterparty actions or outside-world triggers; Techniques encode reusable coordination workflows; Tasks and Logs record the user's controlled actions and outcomes; External References point to issue trackers, grant calls, bounties, agreements, or messages; Relationships and organization-mode UniverseState capture accepted state; Identity, capability grants, Compartments, and projection policy constrain who may act, what may be disclosed, and where evidence may be sent.\n+\n+Explicit strategic-interaction records are deferred to Phase 3+ or future Skill Barter/marketplace work. When introduced, they should model the strategic situation, actor Identity refs, hypothesized incentives, commitment or verification mechanisms, bargaining or principal-agent structure, expected consequence ranges, assumptions, evidence refs, confidence, Compartment refs, authority source, review status, and correction/revocation links.\n+\n+Counterparty models are hypotheses, not facts. A speculative model may create a review item, explanation, or option comparison, but it must not become canonical state, restrict a person's options, classify trustworthiness, disclose private data, expand a capability grant, or create a commitment without review. Recommendations should expose uncertainty and offer non-strategic interpretations when evidence is weak.\n+\n+Credible commitments, signaling, free-riding controls, bargaining, and trust-but-verify workflows should be represented as user-visible mechanisms: milestones, escrow or deposit requirements, acceptance criteria, verification Tasks, audit evidence, staged disclosure, revocation paths, and graceful failure states. Payoff language should be framed as consequences for Objectives and constraints rather than as an opaque utility maximizer.\n+\n+For FOSS dogfooding, the useful Phase 1b surface is pragmatic coordination: issue triage, contributor follow-up, review commitments, release promises, grant deadlines, bug bounty terms, verification checklists, and visible projection state. UbU may warn about bottlenecks, ambiguous ownership, unverified claims, stale promises, and incentive mismatch, but it should not optimize adversarial pressure or persuasion.\n+\n+Phase 1 and Phase 1b must not implement adversarial strategic optimization, automatic counterparty scoring, hidden persuasion timing, or automated bargaining authority. Later multi-user, marketplace, bounty, grant, or Skill Barter features must pass through the same Identity, capability, Compartment, provenance, projection, and human-review boundaries as other external coordination.\n+\n ---\n \n ## 27. GitHub Dogfooding and Projection\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\nindex ce4a217..290785c 100644\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -489,7 +489,7 @@ Resolved by `UBU-D0266`: UbU represents collective decisions as provenance-beari\n \n ## UBU-Q0056: Game theory, strategic interaction, and counterparty modeling\n \n-Status: Open Priority: Post-MVP Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0025 Blocks: Strategic interaction modeling, counterparty incentives, post-MVP coordination protocols Resolved by: None Last scored: 2026-09-18 Scored from commit: None\n+Status: Solved Priority: Post-MVP Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0025 Blocks: Strategic interaction modeling, counterparty incentives, post-MVP coordination protocols Resolved by: UBU-D0267 Last scored: 2026-09-18 Scored from commit: None\n \n ### Question\n \n@@ -510,7 +510,7 @@ Many strategic patterns can be represented through External Events and reusable\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0267`: UbU treats game theory as a boundary discipline. Phase 1 and Phase 1b represent ordinary strategic interaction through existing objects such as External Events, Techniques, Tasks, Logs, External References, Relationships, Identity, capability grants, Compartments, and projection policy. Explicit strategic-interaction and counterparty model objects are deferred to Phase 3+ / future Skill Barter or marketplace work, where they must remain hypothesis-based, provenance-bearing, confidence-scoped, reviewable, and unable to authorize commitments, disclosures, or adversarial optimization without explicit authority.\n \n ---\n \n",
    "warnings": [
      "Patch required --recount normalization."
    ],
    "ordinary_error": "error: corrupt patch at line 89",
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
