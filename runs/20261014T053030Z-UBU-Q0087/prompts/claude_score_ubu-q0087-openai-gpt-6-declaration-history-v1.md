# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0087`  
Question title: `Relationship declaration versioning and scope drift`  
Base commit: `3082787323c189d6be5efa9949c6feea38625149`

```markdown
## UBU-Q0087: Relationship declaration versioning and scope drift

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0026 Blocks: Relationship model, extrospection, RelationshipScopeTransition Resolved by: None Last scored: 2026-10-14 Scored from commit: None

### Question

How should user-declared Relationship scope and perspective be versioned over time, and how should declaration drift relate to observed scope drift?

### Subquestions

1. What fields belong in `ownDeclaredPerspectiveHistory`?
2. When does a new user declaration supersede an older declaration versus coexist with it?
3. How should scope drift be detected when user declarations, observed behavior, and affect history disagree?
4. How should a deliberate RelationshipScopeTransition be linked to declaration history?
5. How should UbU distinguish legitimate scope evolution from unnoticed scope creep?

### Current direction

Relationship declarations should be historical. A changed declaration is evidence and should not silently erase the prior model.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0087-openai-gpt-6-declaration-history-v1",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0087",
    "base_commit": "3082787323c189d6be5efa9949c6feea38625149",
    "summary": "Define immutable Relationship declarations, scoped supersession, temporal drift review and explicit transition links; resolve UBU-Q0087 through UBU-D0344.",
    "rationale": "Separate current participation from desired scope, preserve earlier declarations and behavior, and require explicit user review for scope evolution. Causal conflict handling and Compartment-safe evidence prevent inferred drift or transition completion from silently rewriting the user's perspective. Existing Phase 1 maintenance and related open questions retain their scope.",
    "changed_files": [
      "DESIGN.md",
      "DECISIONS.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DESIGN.md b/DESIGN.md\nindex f5b5da4..3124a0e 100644\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -4248,7 +4248,7 @@ The richer post-MVP Relationship model should lazily decompose the user's side r\n - `ownAffectHistoryRefs`;\n - `ownReflections`.\n \n-`ownDeclaredPerspectiveHistory` is versioned. A changed declaration is meaningful evidence, not a silent overwrite. It may legitimize, supersede, or conflict with observed scope drift.\n+`ownDeclaredPerspectiveHistory` preserves admitted user declarations as history. Phase 2 uses \u00a722.1.1's scoped records and review rules; new declarations preserve earlier declarations and observed evidence.\n \n The counterparty side should use domain- and scope-tagged hypotheses rather than a single `speculatedPerspective`:\n \n@@ -4263,6 +4263,33 @@ The counterparty side should use domain- and scope-tagged hypotheses rather than\n \n Domain examples include `technical_collaboration`, `emotional_availability`, `professional_boundary`, `money_and_value`, `authority`, `reciprocity`, `trust`, `intimacy`, and `conflict_style`. UbU may detect internal tension between hypotheses as a finding about the user's model, not as a claim about the counterparty's inner state.\n \n+#### 22.1.1 Declaration history and scope drift\n+\n+**Phase 2 contract (`UBU-D0344`).** Lazily attach declaration records or policy-protected references through `ownDeclaredPerspectiveHistory`; Phase 1's minimal payload and Objective-based maintenance remain under `UBU-D0076`. Each admitted declaration records the user's own perspective. Imported or generated interpretations remain candidates until the user confirms their meaning through ordinary admission.\n+\n+Each immutable record carries:\n+\n+- `declaration_id`, `schema_version`, `relationship_ref`, and the user-controlled `declaring_identity_ref`;\n+- `domain`, explicit `scope`/context conditions, the dimensions asserted (such as boundaries, availability or obligations), and `assertion_role`: `current_participation` or `desired_participation`;\n+- exact user-stated content or a policy-protected `content_ref`, with source/version provenance; optional rationale and permitted `evidence_refs`;\n+- `recorded_at` and a half-open effective interval `[effective_from, effective_until)`, allowing an open end; unknown effective time stays explicit rather than defaulting to recording time;\n+- `record_kind`: `statement`, `correction` or `withdrawal`; `supersedes_refs` naming prior declaration ids and the affected dimensions, context and interval; optional `transition_ref` and `objective_ref`;\n+- ordinary actor/Device, authority, admission/Log or SyncStatement provenance, observed source versions and causal references, plus Compartment, disclosure, retention, redaction and evidence-use policy references.\n+\n+A new statement coexists with compatible complementary statements or declarations for different contexts, dimensions or intervals. Supersession requires an explicit, user-authorized replacement or withdrawal of identified records for the same Relationship and declaring Identity. Only the named overlap is superseded; unaffected dimensions and intervals remain applicable. Desired participation cannot supersede current participation. Editing content, scope, times or links appends a new record. Withdrawal ends applicability without asserting the opposite. Reject cyclic or invalid supersession references, stage unavailable required references, and deduplicate replay through ordinary admission.\n+\n+Derive applicability from admitted records rather than updating an old record's content, end time or active flag. Offer both effective-time and known-as-of-history views. Known-as-of queries use the Replica's admitted causal frontier/checkpoint; recording timestamps are provenance, not cross-Device ordering authority. A late correction or retrospective clarification may change today's interpretation of an earlier interval, but preserves what was declared and known before the correction was recorded. A new declaration does not erase observed behavior, establish counterparty consent, authorize disclosure, or retrospectively remove a boundary violation.\n+\n+Incompatible overlapping declarations without explicit resolution, including concurrent replacements of the same prior declaration, require ordinary conflict review. Never select a winner by wall-clock time, arrival order or Device. Preserve both source records; the contested interpretation remains pending/candidate until an admitted resolution establishes applicability under DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a7\u00a79 and 17. Recheck authority, relevant declaration/frontier versions and policy before admission. Partial, stale or restricted views label permitted coverage and uncertainty; unavailable history cannot prove absence of a declaration or absence of drift. Diagnostics and links must not reveal denied content or identifiers.\n+\n+Compare declaration changes separately from observed scope divergence. A `ScopeAssessment` compares admitted current-participation declarations with permitted behavior for the same dimensions, context and effective interval, using the declaration applicable when the behavior occurred. Desired participation is shown separately. Unknown effective times or ambiguous scope selectors require clarification before an authoritative comparison. A drift finding binds the compared declaration ids/versions, comparison interval and scope, behavior/evidence and counterevidence refs, relevant affect/reflection refs, source provenance, coverage limits, uncertainty and review status. Search for supporting as well as disconfirming evidence where permitted; missing or conflicting evidence yields an explicit unresolved comparison. Affect may motivate review, but negative affect alone establishes neither scope change nor dysfunction. Agreement between declarations and behavior cannot establish the counterparty's perspective.\n+\n+Distinguish reviewed scope evolution from possible unnoticed scope creep through explicit user review: ask whether the observed change was intended, when it began, and which dimensions the user now endorses. A user-confirmed declaration or scoped review annotation can document intentional evolution of the user's participation. Expansion without matching reviewed intent is only a candidate discrepancy, not proof of unconscious intent or wrongdoing. Reviewing evolution preserves earlier discrepancies and uncertainty about mutual scope. Under `UBU-D0273`, accepting a finding admits a separate scoped observation, annotation or correction; changing the declaration baseline additionally requires an explicit user declaration/correction. Rejection retains the existing privacy-minimized suppression record. Findings cannot silently update scope or become hidden planner constraints. Proactive evidence-count, recency and salience thresholds remain `UBU-Q0089`; typed evidence-policy enums remain `UBU-Q0092`.\n+\n+A deliberate transition binds its `scope_history_context` to baseline declaration ids/versions and records a separate desired scope. Any resulting declaration explicitly links the transition, Objective and permitted review/Log evidence; Relationship reverse references retain transitions across active, completed, abandoned, declined and superseded states. Starting, completing or declining an Objective never automatically changes current participation. A process-successful invitation with a refusal leaves the current declaration intact unless the user separately declares a change in the user's own participation. Retrospective clarification records its effective interval and later recording time. The full transition schema/lifecycle remains `UBU-Q0094`, and process/outcome details remain `UBU-Q0095`.\n+\n+Before claiming implementation conformance, require fixtures for complementary scopes, partial supersession at interval boundaries, withdrawal, future/desired declarations, late correction with both historical views, concurrent offline replacements and replay, behavior/affect disagreement, incomplete or denied evidence, and completed/declined transitions without automatic declaration changes. Verify preserved source history, policy-safe views and ordinary review/admission outcomes. History retention follows current policy; authorized redaction/purge removes disallowed dependent content and leaves only permitted gap metadata.\n+\n ### 22.2 Extrospection\n \n **Extrospection** is relationship-scoped, evidence-backed review of the user's counterparty-perspective hypotheses, Relationship scope declarations, trust boundaries, reciprocity assumptions, affective impact, and functionality assumptions. It reuses the attestation pattern from introspection and AssociationAttestation, but its attestation target is weaker: the user's hypothesis about another party, not the other party's actual perspective and not a value the other party necessarily declared.\ndiff --git a/DECISIONS.md b/DECISIONS.md\nindex 2532743..18ca9cf 100644\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5394,3 +5394,19 @@ Subdelegation defaults to denied. Each admitted child Task/packet/execution link\n Account for contractor overhead, descendants, retries and nested coordination against shared root/branch ceilings using atomic reservations and cumulative counters. Reports retain source attribution and remain candidate evidence. Originators retain policy-permitted inspection, pause, revocation and final review; delivery or contractor summaries cannot establish Task/Objective success or authorize settlement.\n \n Invalidation applies transitively at dispatch, privileged action, disclosure, result release and admission, with explicit assignment cancellation and honest handling of late output and irreversible effects. DESIGN.md \u00a721.6 specifies schema, lifecycle, propagation and enablement fixtures. Phase 1 remains unchanged; this decision accepts a post-MVP data-model contract, not shipped contractor coordination, Association governance or marketplace/settlement protocols.\n+\n+---\n+\n+## UBU-D0344: Relationship declarations preserve scoped history and reviewed scope evolution\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a722.1.1. Resolves `UBU-Q0087`; refines `UBU-D0153` and `UBU-D0154` under `UBU-D0076` and `UBU-D0273`.\n+\n+For Phase 2, represent `ownDeclaredPerspectiveHistory` as immutable, provenance-bearing declarations with identity/Relationship refs, domain/context/dimensions, current-versus-desired participation, exact content or protected source refs, recorded and effective times, correction/withdrawal and scoped supersession links, optional transition/Objective links, and ordinary authority/privacy policy. Keep Phase 1 Relationship maintenance unchanged.\n+\n+Explicit user-authorized supersession replaces only identified overlapping dimensions, contexts and intervals. Complementary declarations coexist; desired participation never replaces current participation. Corrections and withdrawals append records, preserving effective-time and known-as-of views. Concurrent incompatible replacements require ordinary admitted conflict resolution, with no timestamp or Device winner. Missing or withheld history establishes no absence, and retention/redaction rules govern history and derivatives.\n+\n+Scope review compares behavior with the declaration applicable at the behavior's time, separately showing declaration changes, desired scope, affect and counterevidence. Reviewed intentional evolution and possible unnoticed creep are distinct review outcomes with uncertainty. A declaration establishes the user's perspective, not counterparty agreement or retrospective authorization. Findings follow `UBU-D0273` and cannot silently change declarations or become planner constraints.\n+\n+Transitions bind baseline declaration versions and link any separately admitted resulting declaration to its Objective and evidence. Transition completion, refusal or process success cannot itself change current scope. DESIGN.md \u00a722.1.1 defines enablement fixtures; this resolves the data-model contract, not implementation conformance. Proactive thresholds, typed evidence policies, full transition schema and process/outcome details remain with `UBU-Q0089`, `UBU-Q0092`, `UBU-Q0094` and `UBU-Q0095`.\n+\n+---\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\nindex fc790ca..04567b0 100644\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -1160,27 +1160,27 @@ Open.\n \n ## UBU-Q0087: Relationship declaration versioning and scope drift\n \n-Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0026 Blocks: Relationship model, extrospection, RelationshipScopeTransition Resolved by: None Last scored: 2026-10-14 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0026 Blocks: Relationship model, extrospection, RelationshipScopeTransition Resolved by: UBU-D0344 Last scored: 2026-10-14 Scored from commit: None\n \n ### Question\n \n How should user-declared Relationship scope and perspective be versioned over time, and how should declaration drift relate to observed scope drift?\n \n ### Subquestions\n \n 1. What fields belong in `ownDeclaredPerspectiveHistory`?\n 2. When does a new user declaration supersede an older declaration versus coexist with it?\n 3. How should scope drift be detected when user declarations, observed behavior, and affect history disagree?\n 4. How should a deliberate RelationshipScopeTransition be linked to declaration history?\n 5. How should UbU distinguish legitimate scope evolution from unnoticed scope creep?\n \n ### Current direction\n \n Relationship declarations should be historical. A changed declaration is evidence and should not silently erase the prior model.\n \n ### Resolution\n \n-Open.\n+Resolved by UBU-D0344. DESIGN.md \u00a722.1.1 defines immutable declaration fields, scoped supersession/coexistence, effective-time and known-as-of views, evidence-based drift review, and explicit transition/declaration links. User review distinguishes intentional scope evolution from possible unnoticed creep without making counterparty claims. This resolves the Phase 2 data-model contract; implementation conformance, proactive thresholds, typed evidence policies and the full transition model remain separate work.\n \n ---\n \n",
    "commit_message": "docs: define relationship declaration history and scope drift",
    "validation_notes": [
      "Generated the diff from current working-tree contents and proposed contents; repository files remain unmodified.",
      "git apply --check passed against the working tree without --cached or --recount.",
      "UBU-Q0087's heading and Resolution share one hunk; its existing scores, dates, dependencies and single-line metadata format are preserved.",
      "Only allowlisted documentation changes are proposed; UBU-D0344 was unused and no new question ids were allocated.",
      "Implementation fixtures are specified, not executed; this data-model decision requires human approval."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0087"
    ],
    "decisions_added": [
      "UBU-D0344"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0087-openai-gpt-6-declaration-history-v1",
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
