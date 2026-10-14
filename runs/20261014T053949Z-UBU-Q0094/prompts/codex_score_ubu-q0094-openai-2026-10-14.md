# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0094`  
Question title: `RelationshipScopeTransition model boundary and reverse references`  
Base commit: `ad0c5f8d7390f714bd7c707cb4060d15190a287a`

```markdown
## UBU-Q0094: RelationshipScopeTransition model boundary and reverse references

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0026, UBU-Q0087 Blocks: RelationshipScopeTransition, Objective targeting, Relationship history Resolved by: None Last scored: 2026-10-14 Scored from commit: None

### Question

How should RelationshipScopeTransition remain an Objective target while preserving reverse references from Relationship history?

### Subquestions

1. What fields belong on `RelationshipScopeTransition` versus `Relationship`?
2. How should active, completed, abandoned, declined, and superseded transitions be referenced from Relationship?
3. How should a transition update `ownDeclaredPerspectiveHistory`?
4. How should transition-generated Logs and EvidenceRefs link back to the Objective?
5. Which parts are MVP hooks versus post-MVP full implementation?

### Current direction

RelationshipScopeTransition is a process target for Objectives. Relationship remains descriptive but keeps reverse references to transitions that affected it.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0094-openai-2026-10-14",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0094",
    "base_commit": "ad0c5f8d7390f714bd7c707cb4060d15190a287a",
    "summary": "Define RelationshipScopeTransition as a versioned Objective target with retained Relationship reverse references and explicit declaration, Log and evidence provenance.",
    "rationale": "One owning Objective per attempt preserves ordinary planning and lifecycle authority. Derived reverse indexes retain unsuccessful and terminal attempts without changing declared scope. Separate admitted declarations and attributed evidence preserve historical meaning, counterparty autonomy and Compartment policy while leaving detailed outcome and safeguard semantics to existing questions.",
    "changed_files": [
      "DESIGN.md",
      "DECISIONS.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -1421,9 +1421,9 @@ A user may create an Objective to change the user's participation in a Relations\n \n A Relationship scope transition Objective must target user-controlled behavior: disclosures, invitations, boundary-setting, availability changes, communication changes, clarification attempts, and accepted model updates. It must model counterparty response as uncertain and autonomous. For consent-dependent transitions, counterparty response is `outcome_observations`, not user success or failure.\n \n-`RelationshipScopeTransition` is a semantic target/reference for such Objectives. Provisional fields include:\n+`RelationshipScopeTransition` is a semantic target/reference for such Objectives. Identity, ownership and history bindings follow \u00a722.3.1 (`UBU-D0345`); additional process/review fields remain provisional where noted:\n \n-- `relationship_id`;\n+- `relationship_ref`;\n - `transition_type`: `unilateral`, `exploratory`, `mutual`, or `retroactive_scope_clarification`;\n - `current_scope` and `desired_scope`;\n - `transition_intent`;\n@@ -4286,7 +4286,7 @@ Compare declaration changes separately from observed scope divergence. A `ScopeA\n \n Distinguish reviewed scope evolution from possible unnoticed scope creep through explicit user review: ask whether the observed change was intended, when it began, and which dimensions the user now endorses. A user-confirmed declaration or scoped review annotation can document intentional evolution of the user's participation. Expansion without matching reviewed intent is only a candidate discrepancy, not proof of unconscious intent or wrongdoing. Reviewing evolution preserves earlier discrepancies and uncertainty about mutual scope. Under `UBU-D0273`, accepting a finding admits a separate scoped observation, annotation or correction; changing the declaration baseline additionally requires an explicit user declaration/correction. Rejection retains the existing privacy-minimized suppression record. Findings cannot silently update scope or become hidden planner constraints. Proactive evidence-count, recency and salience thresholds remain `UBU-Q0089`; typed evidence-policy enums remain `UBU-Q0092`.\n \n-A deliberate transition binds its `scope_history_context` to baseline declaration ids/versions and records a separate desired scope. Any resulting declaration explicitly links the transition, Objective and permitted review/Log evidence; Relationship reverse references retain transitions across active, completed, abandoned, declined and superseded states. Starting, completing or declining an Objective never automatically changes current participation. A process-successful invitation with a refusal leaves the current declaration intact unless the user separately declares a change in the user's own participation. Retrospective clarification records its effective interval and later recording time. The full transition schema/lifecycle remains `UBU-Q0094`, and process/outcome details remain `UBU-Q0095`.\n+A deliberate transition binds its `scope_history_context` to baseline declaration ids/versions and records a separate desired scope. Any resulting declaration explicitly links the transition, Objective and permitted review/Log evidence; Relationship reverse references retain transitions across active, completed, abandoned, declined and superseded states. Starting, completing or declining an Objective never automatically changes current participation. A process-successful invitation with a refusal leaves the current declaration intact unless the user separately declares a change in the user's own participation. Retrospective clarification records its effective interval and later recording time. Transition identity, Objective ownership, reverse references and provenance follow \u00a722.3.1 (`UBU-D0345`); process/outcome details remain `UBU-Q0095`.\n \n Before claiming implementation conformance, require fixtures for complementary scopes, partial supersession at interval boundaries, withdrawal, future/desired declarations, late correction with both historical views, concurrent offline replacements and replay, behavior/affect disagreement, incomplete or denied evidence, and completed/declined transitions without automatic declaration changes. Verify preserved source history, policy-safe views and ordinary review/admission outcomes. History retention follows current policy; authorized redaction/purge removes disallowed dependent content and leaves only permitted gap metadata.\n \n@@ -4360,6 +4360,35 @@ The affect-knowledge firewall is central: extrospection-derived affect knowledge\n \n Romantic, professional, and dependency-heavy transitions may have stronger default advisory safeguards because they can involve asymmetric vulnerability or power. `power_asymmetry`, `power_asymmetry_basis`, `vulnerability_profile`, and `pacing_constraints` should be represented explicitly. Pacing constraints may include minimum time between Steps, mandatory observation periods, cooling periods after decline, evidence required before the next Step, and maximum prompt frequency.\n \n+#### 22.3.1 Objective target, reverse history and provenance\n+\n+**Phase 2 model boundary (`UBU-D0345`).** One admitted `RelationshipScopeTransition` represents one deliberate attempt concerning one Relationship and its user-controlled Identity, owned by one `one_time` Objective. The Objective carries a typed `target_ref` to the transition; the transition carries the reciprocal `objective_ref`. Ordinary Objectives, Techniques, Steps and Tasks own value, recurrence, execution and scheduling. Transition records hold the attempt's scope, intent, review context and provenance.\n+\n+The versioned transition record carries:\n+\n+- `transition_id`, `schema_version`, `relationship_ref`, `acting_identity_ref` and `objective_ref`; the acting Identity must be the Relationship's user-controlled side;\n+- `transition_type`, scoped `transition_intent`, `desired_scope` and `user_controlled_changes`, with protected content refs where needed; the existing type vocabulary and review payload remain under \u00a7\u00a77.5 and 22.3;\n+- `scope_history_context`: baseline declaration ids/versions (an empty set requires an explicit unknown/coverage reason), domain/context/dimensions and effective interval, the admitted causal frontier/checkpoint used, and permitted coverage/uncertainty; `current_scope` is a derived, policy-safe view of that baseline;\n+- separate `process_success_criteria` and `outcome_observations` slots, permitted review/Log and `evidence_refs`, and derived `resulting_declaration_refs` identifying separately admitted declarations;\n+- optional `supersedes_transition_ref` or `reopens_transition_ref` with reason and permitted supporting evidence; these identify earlier attempts, not declaration supersession;\n+- ordinary object-version, actor/Device, effective/recorded-time, authority, admission/Log or SyncStatement, causal and idempotency provenance, plus Compartment, disclosure, retention, redaction and evidence-use policy refs.\n+\n+Admit the reciprocal Objective/transition linkage as a coherent batch; stage it if required dependencies are unavailable. Stable refs identify objects, while review, planning and admission bind exact observed versions. Ownership, Relationship and acting Identity cannot be rebound to a different attempt. Scope, intent, baseline and evidence changes create admitted revisions retaining prior versions. Missing baseline history stays explicitly unknown; a changed applicable declaration requires renewed review before relying on the old baseline, never silent rebasing.\n+\n+**Relationship reverse history.** Lazily expose `scope_transition_refs` as a policy-filtered index of admitted transitions whose `relationship_ref` names this Relationship. Attach the reverse association at attempt admission, including attempts that produce no scope change. Its source is the admitted transition linkage; a materialized index is rebuildable and carries no independent lifecycle or scope authority. Relationship retains its descriptive Identity, affect, declaration and observation histories; transition intent, criteria, safeguards and scheduling do not move onto Relationship. Concurrent attempts for different contexts may coexist; incompatible admitted changes use ordinary conflict review.\n+\n+Retain stable reverse refs through active, completed, abandoned, superseded and invalid Objective states, subject to current retention policy. Lifecycle views follow the owning Objective's \u00a77.3 status and append-only `objective_transitioned` Logs. `declined` is a separate outcome/history filter supported by an admitted scoped response observation, not an Objective status or proof of process failure; it can coexist with a terminal process-successful Objective. Never derive refusal from silence or missing evidence. A superseding attempt records the prior owning Objective's ordinary \u00a77.3 supersession transition and retains both reverse refs. Terminal attempts do not reactivate; reopening uses a new transition and new Objective with lineage, materially new evidence and the existing stronger-review rule. Reject cross-Relationship/Identity lineage and lineage cycles. Detailed criteria and terminal-status mappings remain `UBU-Q0095`.\n+\n+**Declaration changes.** A transition may propose a \u00a722.1.1 declaration; only a separate explicit user declaration/correction admitted under `UBU-D0344` changes `ownDeclaredPerspectiveHistory`. Bind the new declaration to `transition_ref`, `objective_ref` and permitted review/Log evidence; validate matching Relationship and declaring/acting Identity on those links. Derive the transition's `resulting_declaration_refs` index from those admitted records. Preserve effective/recorded times and scoped supersession/coexistence. Desired scope, Task or Objective completion, abandonment, refusal and transition supersession cannot themselves replace current participation. A completed attempt may have no resulting declaration; a declined attempt may still accompany a separately declared change to the user's own participation.\n+\n+**Logs and evidence.** Use \u00a717's append-only envelope and appropriate existing event types. Keep a Task or Objective event's primary `target_ref`; its `event_payload` additionally binds `transition_ref`, owning `objective_ref`, `relationship_ref`, relevant observed versions and permitted Step/Task, declaration and evidence refs. Record admissions, revisions and corrections with ordinary provenance and idempotency, preserving original events. Transition-generated EvidenceRefs, or transition-scoped associations for reused evidence, bind the same transition/Objective/Relationship refs to source id/version, selector or interval, purpose, provenance and applicable policy. Linking evidence does not rewrite its source or make generated summaries independent evidence. Adding links to existing Logs uses annotations/corrections.\n+\n+Target links, reverse indexes, Logs, EvidenceRefs, declarations and cached views inherit source restrictions. Recheck current authority, object/source versions and policy before admission, read, sync or export. Partial or denied views cannot prove absence of an attempt or declaration; omit denied identifiers, counts and existence. Authorized redaction/purge removes disallowed dependent content and leaves only permitted gap/causal metadata. Concurrent incompatible revisions or links require ordinary admitted conflict resolution under DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a7\u00a79 and 17, with no wall-clock, arrival-order or Device winner.\n+\n+**Delivery boundary.** MVP hooks are optional typed target/reverse-reference and provenance bindings through ordinary Objective, Task and Log mechanisms. Phase 1's required Relationship payload and maintenance cadence remain under `UBU-D0076`; ordinary Relationships need no transition object or empty history array. A supported Phase 2 attempt must meet this core contract before admission. Dedicated relationship-management UI, automated multi-stage orchestration and personal CRM behavior are post-MVP work. `UBU-Q0095`, `UBU-Q0096`, `UBU-Q0097`, `UBU-Q0098` and `UBU-Q0099` retain their detailed outcome, taxonomy, validation and safeguard scope; typed evidence policies remain `UBU-Q0092`.\n+\n+Before claiming implementation conformance, require fixtures for reciprocal linkage and rejected mismatched owners, reverse-index rebuild/replay without duplicates, all retained lifecycle views including a completed process with a declined outcome and no declaration change, explicit resulting declarations and retrospective correction, stale baselines, concurrent offline revisions, lineage cycles and reopening with a new Objective, Task/Log/EvidenceRef attribution, and denied/partial views plus derivative purge. This accepts the model boundary, not passing fixtures or shipped transition orchestration.\n+\n ### 22.4 Advisory safeguards and hard boundaries\n \n Relationship-transition review gates are default-on advisory safeguards unless the user has configured them as self-imposed required gates or an external constraint makes them unavoidable. UbU should recommend pre-action introspection, extrospection, TrustCalibrationAssessment, power/vulnerability review, pacing review, graceful nonachievement planning, and manipulation-risk review where relevant. The user may bypass or disable such safeguards. Bypass decisions are introspection-relevant evidence and may surface as candidate findings about conflicts between revealed behavior and declared values.\ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5410,3 +5410,17 @@ Scope review compares behavior with the declaration applicable at the behavior's\n Transitions bind baseline declaration versions and link any separately admitted resulting declaration to its Objective and evidence. Transition completion, refusal or process success cannot itself change current scope. DESIGN.md \u00a722.1.1 defines enablement fixtures; this resolves the data-model contract, not implementation conformance. Proactive thresholds, typed evidence policies, full transition schema and process/outcome details remain with `UBU-Q0089`, `UBU-Q0092`, `UBU-Q0094` and `UBU-Q0095`.\n \n ---\n+\n+## UBU-D0345: Relationship scope transitions bind Objective attempts and preserve reverse history\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a7\u00a77.5, 22.1.1 and 22.3.1. Resolves `UBU-Q0094`; refines `UBU-D0154` under `UBU-D0076` and `UBU-D0344`, preserving `UBU-D0155` and \u00a722.4's advisory boundary.\n+\n+Represent each transition as a versioned attempt for one Relationship and user-controlled Identity, with one owning one-time Objective and reciprocal typed target/owner refs. Pin baseline declaration versions, scope and causal coverage; store desired scope, intent, process/outcome slots, review/evidence and resulting-declaration links on the transition. Objectives and ordinary Tasks retain planning and lifecycle authority.\n+\n+Relationship exposes a policy-filtered, rebuildable `scope_transition_refs` index sourced from admitted transition links, including attempts without a scope change. Preserve refs across active and terminal Objective states. Declined is an evidence-backed outcome/history view, not an Objective status or process failure. Reopening requires a new transition/Objective pair with explicit lineage; ordinary version, replay and conflict rules govern linkage and revisions.\n+\n+Only a separately admitted explicit user declaration/correction changes `ownDeclaredPerspectiveHistory`. Link it to the transition, Objective and permitted evidence without erasing scoped history. Transition-generated Logs and EvidenceRefs or evidence associations bind those same refs and source versions, retaining primary Task/Objective targets, source provenance and privacy restrictions. Completion, refusal or supersession cannot itself change current participation.\n+\n+Keep Phase 1 maintenance and required payload unchanged. Reserve optional MVP target/history/provenance hooks; supported Phase 2 attempts must satisfy the core bindings. Dedicated UI and richer orchestration remain post-MVP, with detailed outcome, type, validation, safeguard and evidence-policy work in the existing questions. DESIGN.md \u00a722.3.1 defines conformance fixtures; this resolves the model contract, not implementation acceptance.\n+\n+---\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -1344,27 +1344,27 @@ Open.\n \n ## UBU-Q0094: RelationshipScopeTransition model boundary and reverse references\n \n-Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0026, UBU-Q0087 Blocks: RelationshipScopeTransition, Objective targeting, Relationship history Resolved by: None Last scored: 2026-10-14 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0026, UBU-Q0087 Blocks: RelationshipScopeTransition, Objective targeting, Relationship history Resolved by: UBU-D0345 Last scored: 2026-10-14 Scored from commit: None\n \n ### Question\n \n How should RelationshipScopeTransition remain an Objective target while preserving reverse references from Relationship history?\n \n ### Subquestions\n \n 1. What fields belong on `RelationshipScopeTransition` versus `Relationship`?\n 2. How should active, completed, abandoned, declined, and superseded transitions be referenced from Relationship?\n 3. How should a transition update `ownDeclaredPerspectiveHistory`?\n 4. How should transition-generated Logs and EvidenceRefs link back to the Objective?\n 5. Which parts are MVP hooks versus post-MVP full implementation?\n \n ### Current direction\n \n RelationshipScopeTransition is a process target for Objectives. Relationship remains descriptive but keeps reverse references to transitions that affected it.\n \n ### Resolution\n \n-Open.\n+Resolved by UBU-D0345. DESIGN.md \u00a7\u00a77.5, 22.1.1 and 22.3.1 define the versioned Objective target/owner boundary, retained policy-safe reverse references, separately admitted declaration changes, Log/EvidenceRef attribution, and MVP hooks versus post-MVP orchestration. Declined remains an outcome/history view independent of Objective status. This resolves the model contract; implementation conformance and the detailed outcome, taxonomy, Step validation, safeguards and typed evidence-policy questions remain separate work.\n \n ---\n \n",
    "commit_message": "docs: define RelationshipScopeTransition ownership and reverse history",
    "validation_notes": [
      "Generated unified hunks with git diff --no-index against current working-tree contents; repository files were left unchanged.",
      "git apply --check passed against the current working tree, including uncommitted OPEN_QUESTIONS.md metadata updates.",
      "Verified that only UBU-Q0094's question block changes; its metadata remains one line with only Status and Resolved by updated.",
      "UBU-D0345 was unused. Implementation conformance fixtures are specified but were not run; this proposal changes documentation only."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0094"
    ],
    "decisions_added": [
      "UBU-D0345"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0094-openai-2026-10-14",
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
