# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `codex`

## Selected question

Question ID: `UBU-Q0084`  
Question title: `Computer-use AgentAction and BackgroundProcess model`  
Base commit: `b9ea1362d86344b7cb6476c888dbe72171f75f8c`

```markdown
## UBU-Q0084: Computer-use AgentAction and BackgroundProcess model

Status: Open Priority: MVP important Phase: Phase 1b Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0031, UBU-Q0032, UBU-Q0060, UBU-Q0079 Blocks: background agents, scheduled agents, computer-use automation, prompt-injection controls Resolved by: None Last scored: 2026-09-18 Scored from commit: None

### Question

How should UbU model computer-use agents and background processes that consume compute, credentials, money, privacy budget, or external authority without necessarily occupying user Calendar time?

### Subquestions

1. What fields belong in `AgentAction` versus `BackgroundProcess`?
2. How should triggers, recurrence, schedules, cost budgets, notification policy, and escalation be represented?
3. How should prompt-injection exposure be scored when agents consume webpages, messages, documents, or tool outputs?
4. What rollback or mitigation metadata is required for irreversible side effects?
5. Which background processes belong in the Calendar, and which should remain separate from user time blocking?

### Current direction

Computer-use and background agents are high-risk external actors. They require authority scopes, audit trails, rollback or mitigation paths, prompt-injection handling, and candidate-update semantics.

Phase 1b split: the advisory background-process subset of this question is decomposed into UBU-Q0137 and answered under the Phase 1b rule, because Phase 1b's unattended advisory batch runner is a real BackgroundProcess that consumes compute and external credentials outside Calendar time. Computer-use `AgentAction` semantics, irreversible-side-effect rollback metadata, and general prompt-injection scoring remain open here and are not answered by Phase 1b.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "codex-ubu-q0084-d0263",
    "provider_id": "codex",
    "model_name": "gpt-5-codex",
    "question_id": "UBU-Q0084",
    "base_commit": "b9ea1362d86344b7cb6476c888dbe72171f75f8c",
    "summary": "Resolve UBU-Q0084 by accepting a split model: BackgroundProcess records durable unattended policy and scheduling, while AgentAction records each concrete external dispatch with authority, prompt-injection, budget, side-effect, evidence, and mitigation metadata.",
    "rationale": "The existing provisional section listed shared fields but did not distinguish standing process policy from individual external acts. This proposal makes the boundary explicit, carries forward the accepted Log, ContextBundle, credential, Compartment, MCP capability, candidate-update, and append-only mitigation constraints, and answers which background work should project into Calendar time.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -4050,3 +4050,23 @@ Reusable secret values must never appear in canonical objects, mutation payloads,\n Enforcement is at egress and serialization. Secret material is represented inside trusted code as a typed secret reference or secret-store handle whose value cannot be rendered into ordinary JSON payloads. The projection dispatcher, worker submission builder, log/diagnostic writer, export/backup pipeline, and diagnostic-bundle builder share a clearance filter that rejects secret-bearing fields, scans declared payload classes for forbidden secret types, and fails closed when a value is not classified. Diagnostics may record that a credential was missing, revoked, expired, or denied by policy, but not the credential value or enough lookup material to recover it.\n \n Phase 1b exports and backups exclude the local secret store by default. They may include redacted secret capability metadata only when policy permits structural disclosure and must mark dependent projection/advisory queues as needing reauthorization on restore. This leaves cross-Device replication, recovery wrapping, rotation propagation, local encryption-key custody, and concrete cryptographic or OS-keychain choices to `UBU-D0252` and later implementation design.\n+\n+---\n+\n+## UBU-D0263: Computer-use agents use separate AgentAction and BackgroundProcess records\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a721.8. Resolves `UBU-Q0084`.\n+\n+UbU models external computer-use work with two records rather than treating every autonomous run as Calendar time. A `BackgroundProcess` is durable standing policy for when work may run; an `AgentAction` is one attempted dispatch, candidate dispatch, retry, cancellation, or mitigation step under that policy.\n+\n+An `AgentAction` records action identity, actor and agent/tool refs, purpose, related work refs, target surface, operation kind, payload summary or hash, expected external version, idempotency key, authority source, capability and credential-handle refs, Compartment and disclosure constraints, route decision refs, prompt-injection assessment, estimated and actual budget use, side-effect class, review/approval state, result refs, completion evidence, downstream candidate refs, Log refs, failure reason when safe, and rollback or mitigation metadata.\n+\n+A `BackgroundProcess` records process identity, owner, purpose, lifecycle status, trigger and recurrence policy, run windows, event subscriptions, concurrency limits, retry/backoff and cancellation policy, catch-up behavior, allowed action kinds and target surfaces, provider/tool classes, capability grants, credential handles, Compartments, ContextBundle templates, compute/rate/money/privacy budget caps, prompt-injection ceiling, isolation mode, review threshold, output-admission policy, notification policy, escalation path, Calendar projection policy, audit policy, retained evidence, and cleanup obligations.\n+\n+Prompt-injection exposure is scored before dispatch and revised after execution. The score considers source trust, untrusted webpages/messages/documents/tool outputs, mixed-Compartment context, raw private-context volume, whether untrusted text can influence tool choice or arguments, and the authority at risk: credentials, spending, external mutation, privacy export, or user-facing commitments. The minimum bands are `low`, `medium`, `high`, and `critical`; high exposure requires containment and critical exposure fails closed unless a specific approved policy permits the run inside hard gates.\n+\n+External side effects are classified as `read_only`, `local_candidate_only`, `externally_reversible`, `compensating_action_possible`, `externally_irreversible`, or `unknown`. Anything beyond read-only or local candidate generation needs expected prior state when available, idempotency, reconciliation evidence, and the review gate that admitted it. Irreversible, unknown, or compensation-only effects require mitigation owner, notification rule, cancellation or revocation path, compensating operation when available, external contact path when relevant, mitigation deadline, and Log or candidate records to write if mitigation is needed.\n+\n+Background processes enter the Calendar only when they reserve or require user attention, represent a user commitment, or make the user materially unavailable. Compute-only work, polling, sync, advisory analysis, candidate generation, projection reconciliation, and scheduled external checks remain separate process state, though they may create Calendar overlays, deadlines, reminders, or review Tasks.\n+\n+Dispatch rechecks current Compartment policy, credential version, capability grant, budget, lifecycle status, and expected external version immediately before use. Logs remain append-only; failed, denied, cancelled, mitigated, and superseded actions are recorded rather than rewritten.\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -3028,25 +3028,37 @@ The preferred framing is an open, user-sovereign skill economy or a self-reinfor\n ### 21.8 AgentAction and BackgroundProcess\n \n-An `AgentAction` is a bounded action by an agent or tool against an external surface. A `BackgroundProcess` is a recurring or conditional process that may run without occupying user Calendar time.\n+An `AgentAction` is one bounded attempted action by an agent, model, worker, or tool against an external or privileged surface. A `BackgroundProcess` is the durable controller object that decides when zero or more `AgentAction`s should be proposed, run, retried, cancelled, or escalated without necessarily occupying user Calendar time.\n \n-Provisional shared fields:\n+`AgentAction` fields describe a single dispatch or candidate dispatch:\n \n-- `actor_identity_ref`;\n-- `agent_or_tool_ref`;\n-- `trigger`;\n-- `schedule_or_condition`;\n-- `authority_scope`;\n-- `credential_refs`;\n-- `compartment_refs`;\n-- `external_surface_refs`;\n-- `irreversible_side_effect`;\n-- `prompt_injection_exposure`;\n-- `compute_or_cost_budget`;\n-- `notification_policy`;\n-- `rollback_or_mitigation_path`;\n-- `completion_evidence`;\n-- `failure_escalation_policy`.\n+- `agent_action_id`;\n+- optional `background_process_ref`;\n+- `actor_identity_ref` and `agent_or_tool_ref`;\n+- purpose, related Objective/Task/workflow refs, and originating ContextBundle refs;\n+- target surface, operation kind, payload summary or hash, expected prior external version, and idempotency key;\n+- authority source, capability grant refs, credential or secret-capability refs by opaque handle and version, Compartment refs, Identity/Association disclosure constraints, and provider/tool route decision refs;\n+- prompt-injection exposure assessment and containment policy;\n+- compute, rate, money, and privacy-budget estimate plus actual usage when known;\n+- side-effect class, reversibility, dry-run/candidate status, review requirement, and approval state;\n+- result status, external result refs, downstream candidate refs, completion evidence, Log refs, and failure or denial reason when safe;\n+- rollback or mitigation metadata for any external effect that cannot simply be ignored.\n \n-These objects are not equivalent to ordinary user Calendar events. They may consume compute, credentials, money, privacy budget, or external authority without consuming the user's direct time.\n+`BackgroundProcess` fields describe standing permission and scheduling policy:\n+\n+- `background_process_id`, owner Identity, purpose, lifecycle status, created/updated/disabled refs, and policy version;\n+- trigger spec, recurrence or polling rule, event subscriptions, manual-start affordance, run window, deadline when any, expiry, concurrency limit, retry and backoff policy, cancellation policy, and maximum catch-up behavior after downtime;\n+- allowed action kinds, target surfaces, provider/tool classes, capability grants, credential handles, Compartments, and ContextBundle templates;\n+- compute, rate, money, and privacy-budget caps per action, per period, and lifetime;\n+- prompt-injection risk ceiling, required isolation mode, review threshold, output-admission policy, and candidate-update policy;\n+- notification policy, quiet-hours behavior, escalation path, failure threshold, and required user-visible summary;\n+- Calendar projection policy, audit/logging policy, retained evidence refs, and cleanup or mitigation obligations.\n+\n+Prompt-injection exposure is scored before dispatch and updated after execution. The score combines source trust, whether the agent consumed untrusted webpages, messages, documents, tool outputs, or mixed-Compartment content, the amount of raw private context exposed, whether untrusted text can influence tool choice or arguments, and the authority at risk: credentials, spending, external mutation, privacy export, or user-facing commitments. The minimum qualitative bands are `low`, `medium`, `high`, and `critical`. `High` exposure requires containment such as instruction stripping, source separation, allowlisted tools, candidate-only writes, narrowed ContextBundles, or human review. `Critical` exposure fails closed unless a specific approved policy permits the run and the action remains inside hard Compartment, credential, cost, and review gates.\n+\n+External side effects carry a side-effect class: `read_only`, `local_candidate_only`, `externally_reversible`, `compensating_action_possible`, `externally_irreversible`, or `unknown`. Anything outside `read_only` or `local_candidate_only` must record expected external prior state when available, idempotency keys, evidence to reconcile the result, and the review gate that admitted the effect. `Compensating_action_possible`, `externally_irreversible`, and `unknown` actions require mitigation metadata before dispatch: mitigation owner, user notification rule, cancellation or revocation path, compensating operation when one exists, external support/contact path when relevant, deadline for mitigation, and the Log or candidate records that should be written if mitigation is needed.\n+\n+Background processes are Calendar items only when they reserve or require the user's attention, represent a user commitment to another party, or make the user's availability materially unavailable. Compute-only work, polling, sync, advisory analysis, candidate generation, projection reconciliation, and scheduled external checks remain separate process state. They may surface Calendar overlays, deadlines, reminders, or review Tasks, but those projections do not become user time blocks unless the user must attend, decide, travel, meet, or be unavailable.\n+\n+All AgentAction and BackgroundProcess mutations remain candidate/admission workflows unless an accepted integration policy gives narrower automatic authority. Dispatch rechecks current Compartment policy, credential version, capability grant, budget, lifecycle status, and expected external version immediately before use. Logs are append-only: failed, denied, cancelled, mitigated, and superseded actions are recorded rather than rewritten.\n \n ### 21.9 State-transition cockpit\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -1081,7 +1081,7 @@ Resolved. See UBU-D0162.\n ## UBU-Q0084: Computer-use AgentAction and BackgroundProcess model\n \n-Status: Open Priority: MVP important Phase: Phase 1b Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0031, UBU-Q0032, UBU-Q0060, UBU-Q0079 Blocks: background agents, scheduled agents, computer-use automation, prompt-injection controls Resolved by: None Last scored: 2026-09-18 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: UBU-Q0031, UBU-Q0032, UBU-Q0060, UBU-Q0079 Blocks: background agents, scheduled agents, computer-use automation, prompt-injection controls Resolved by: UBU-D0263 Last scored: 2026-09-18 Scored from commit: None\n \n ### Question\n \n How should UbU model computer-use agents and background processes that consume compute, credentials, money, privacy budget, or external authority without necessarily occupying user Calendar time?\n@@ -1101,7 +1101,7 @@ Computer-use and background agents are high-risk external actors. They require a\n Phase 1b split: the advisory background-process subset of this question is decomposed into UBU-Q0137 and answered under the Phase 1b rule, because Phase 1b's unattended advisory batch runner is a real BackgroundProcess that consumes compute and external credentials outside Calendar time. Computer-use `AgentAction` semantics, irreversible-side-effect rollback metadata, and general prompt-injection scoring remain open here and are not answered by Phase 1b.\n \n ### Resolution\n \n-Open.\n+Resolved. See UBU-D0263.\n \n ---\n",
    "commit_message": "Resolve AgentAction and BackgroundProcess model",
    "validation_notes": [
      "Patch modifies only DECISIONS.md, DESIGN.md, and OPEN_QUESTIONS.md.",
      "OPEN_QUESTIONS.md preserves the single-line metadata format and anchors the resolution in the UBU-Q0084 block.",
      "Validated with git apply --check against the provided workspace context."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0084"
    ],
    "decisions_added": [
      "UBU-D0263"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "codex-ubu-q0084-d0263",
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
