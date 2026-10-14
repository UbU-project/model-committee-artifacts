# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0101`  
Question title: `Advisory safeguard uncertainty, override, and introspection consequences`  
Base commit: `4ceb0ef3797c1cbf3e7a7edd33835404105037e1`

```markdown
## UBU-Q0101: Advisory safeguard uncertainty, override, and introspection consequences

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0100 Blocks: safeguard UX, introspection, relationship-transition UX Resolved by: None Last scored: 2026-10-14 Scored from commit: None

### Question

How should advisory behavioral safeguards represent uncertainty, false-positive risk, user override, and introspection consequences without becoming paternalistic gates?

### Subquestions

1. How should UbU communicate classifier uncertainty and false-positive/false-negative risk?
2. What UI should allow review, revision, bypass-once, disablement, or user-configured required gates?
3. How should bypass decisions become candidate introspection evidence without becoming soft coercion?
4. How should safeguard disablement interact with declared values around respect, care, autonomy, safety, and trust?
5. When should repeated bypasses trigger review of the user's values or preferences?

### Current direction

Advisory safeguards should be default-on, user-aware, uncertainty-transparent, overrideable, and introspection-relevant when bypassed.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "openai-ubu-q0101-advisory-safeguard-contract",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0101",
    "base_commit": "4ceb0ef3797c1cbf3e7a7edd33835404105037e1",
    "summary": "Define uncertainty disclosures, scoped override controls, optional values-linked introspection, and bounded review of repeated safeguard bypasses.",
    "rationale": "UBU-D0165 and UBU-D0354 already establish advisory safeguards and scoped decision Logs. This proposal completes the product contract by making fallibility and user controls explicit, requiring evidence and separate authority for introspection, and preventing repeated bypasses or disablement from becoming coercive values judgments.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\nindex 80545c1..1c47d7e 100644\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5576,3 +5576,17 @@ Bind a graceful nonachievement plan to actual process criteria and terminal mapp\n Phase 1 maintenance is unchanged. DESIGN.md \u00a722.3.6 specifies required model/enablement fixtures, not passing implementation evidence; richer orchestration and general safeguard UX retain their existing delivery boundaries.\n \n ---\n+\n+## UBU-D0356: Advisory safeguards expose fallibility and preserve scoped user control\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a722.4. Resolves `UBU-Q0101`; refines `UBU-D0165` and `UBU-D0354` without changing hard boundaries.\n+\n+Present advisory behavioral findings with analyzed/missing coverage, evidence and counterevidence, uncertainty and explicit false-positive/false-negative limitations. Distinguish no concern identified from unavailable or disabled checks. Numeric probabilities/error rates require applicable evaluation evidence; classifier confidence supplies no intent or safety verdict.\n+\n+Offer review/correction/revision, scoped Continue once, explicit disablement/re-enablement, self-imposed required review, and dismissal/snooze. Bind decisions and policy changes to current authority and versions. Required-gate exceptions retain their rule authority; advisory controls never waive product boundaries, external constraints, consent or evidence-use restrictions.\n+\n+Record choices through existing decision Logs with optional reasons and no Preference inference. Disablement does not revise declared values. Optional values-conflict candidates require current introspection authority, an applicable declared value and behavioral evidence beyond bypass alone; exact state changes need separate admission. No compulsory introspection, punitive Task or compliance-based privilege/Plan change follows.\n+\n+Repeated bypasses support requested review of warning quality/configuration. Unsolicited review reminders require an explicitly enabled, scoped finite count/window and attention budget; they prove no inconsistency. Persist suppression, deduplicate operations and honor rejected claims across surfaces. DESIGN.md \u00a722.4 specifies required enablement fixtures, not passing implementation evidence; broader proactive extrospection thresholds remain `UBU-Q0089`.\n+\n+---\ndiff --git a/DESIGN.md b/DESIGN.md\nindex d9b1b3e..710d8b9 100644\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -4843,16 +4843,60 @@ Before claiming conformance, require fixtures for every basis code, both advanta\n \n ### 22.4 Advisory safeguards and hard boundaries\n \n-Relationship-transition review gates are default-on advisory safeguards unless the user has configured them as self-imposed required gates or an external constraint makes them unavoidable. UbU should recommend pre-action introspection, extrospection, TrustCalibrationAssessment, power/vulnerability review, pacing review, graceful nonachievement planning, and manipulation-risk review where relevant. The user may bypass or disable such safeguards. Bypass decisions are introspection-relevant evidence and may surface as candidate findings about conflicts between revealed behavior and declared values.\n+Relationship-transition review gates are default-on advisory safeguards unless the user has configured them as self-imposed required gates or an external constraint makes them unavoidable. UbU should recommend pre-action introspection, extrospection, TrustCalibrationAssessment, power/vulnerability review, pacing review, graceful nonachievement planning, and manipulation-risk review where relevant. The user may bypass or disable such safeguards. Bypass decisions may contribute to optional, authorized introspection candidates; bypass alone establishes no conflict between behavior and declared values.\n \n Hard boundaries are narrower. They cover structurally enforceable product invariants and explicit policies: Compartment boundaries, privacy policy, data-access authorization, EvidenceUsePolicy restrictions, export prohibitions, identity/Compartment isolation, provenance integrity, audit-record integrity, integration authorization, and unavoidable provider/platform/legal constraints. Provider, platform, app-store, integration, and legal constraints should be recorded as external constraints with source, scope, affected workflow, and enforcement path when known.\n \n-Behavioral-risk checks such as manipulation, coercion, harassment, stalking, deception, power-asymmetry risk, rumination risk, or relationship-transition ethics are fallible classifier judgments. Treating them as hard gates would risk false positives that override legitimate user autonomy, false negatives that create misplaced trust, and a false impression that UbU can reliably prevent misuse or illegal behavior. They should generally be default-on, uncertainty-aware, user-overrideable advisory checks with introspection consequences when bypassed.\n+Behavioral-risk checks such as manipulation, coercion, harassment, stalking, deception, power-asymmetry risk, rumination risk, or relationship-transition ethics are fallible classifier judgments. Treating them as hard gates would risk false positives that override legitimate user autonomy, false negatives that create misplaced trust, and a false impression that UbU can reliably prevent misuse or illegal behavior. They should generally be default-on, uncertainty-aware, user-overrideable advisory checks with optional authorized introspection review when bypassed.\n \n A user-configured required gate for a RelationshipScopeTransition is not reclassified as a product hard boundary. It is a self-imposed policy gate that may block the current workflow while enabled and should record the configuring Identity, scope, review requirement, bypass or disablement authority, and relevant Log refs.\n \n Relationship documentation must not claim that UbU prevents coercion, harassment, stalking, manipulation, or illegal behavior. It may claim that UbU preserves structural privacy and authorization boundaries, surfaces advisory behavioral-risk findings, records bypasses as introspection-relevant evidence, and refuses actions only when a product hard boundary, user-required gate, or external constraint applies.\n \n+#### 22.4.1 Uncertainty and the presented finding\n+\n+**Phase 2 product contract (`UBU-D0356`).** Apply this contract to advisory behavioral safeguards in Relationship review, extrospection, introspection and transition workflows. Use existing AdvisoryCandidate, checkpoint, policy and Log envelopes; introduce no new kernel constraint or automatic state mutation.\n+\n+Bind each presented assessment to the check/rule and version, governance category, affected operation or reviewed batch, input/evidence/policy versions, model or reviewer provenance, analyzed scope, missing coverage, supporting and disconfirming evidence, and uncertainty. Distinguish a suspected concern, a user-reviewed interpretation, no concern identified within analyzed coverage, and an unavailable or incomplete assessment. Disabled or unperformed checks have no result; missing evidence never becomes a clean assessment.\n+\n+Give a short, neutral explanation of what may be concerning, the permitted basis and counterevidence, unknowns, and an optional alternative. State that a check can flag legitimate conduct or miss concerning conduct, and that no finding guarantees safety. Model confidence is neither proof of intent nor a probability of harm. Numeric risk probabilities or false-positive/false-negative rates require linked, applicable calibration/evaluation evidence with its population, workflow, version, coverage and uncertainty; unavailable rates remain unknown. Evaluated aggregate error rates establish no verdict about this action.\n+\n+#### 22.4.2 Review, continuation and configuration\n+\n+Advisory findings create no automatic delay, Task dependency, planner constraint or execution veto. A flagged, unavailable or disabled assessment leaves otherwise eligible work available through ordinary authorization. Present the category and any actual blocker separately from the advisory finding. Use concise inline review with details on demand, neutral labels and usable text/assistive-technology controls. Do not require a justification, countdown, repeated confirmation or introspection completion for an ordinary advisory choice.\n+\n+| Control | Effect |\n+| --- | --- |\n+| Review / revise | Inspect permitted evidence, correct or dispute the finding, revise the affected action, or defer/cancel it. Correction preserves source and review history through ordinary candidate and Log correction paths. |\n+| Continue once | Explicitly bypass the advisory for the exact operation or reviewed batch. This acknowledges the presented uncertainty; it neither endorses the finding nor completes the recommended review. |\n+| Disable this check | Show and explicitly confirm the selected check, Identity/workflow/project scope and duration (until re-enabled or a selected expiry), then admit the authorized policy change. Keep effective disabled status and Re-enable discoverable; preserve history. |\n+| Configure required review | Explicitly admit a self-imposed rule with configuring Identity, scope, inspectable completion condition and exception/disablement authority under \u00a722.3.5. Manual review must remain eligible without a model; a favorable classifier verdict is not a completion condition. |\n+| Dismiss / snooze | Suppress presentation until material change or a user-selected deadline, respectively. This changes notification state, not action authorization or a required gate. |\n+\n+Continue once is available only when independent product boundaries, required-gate policy and external constraints permit execution. If blocked, show the permitted reason/source and offer review, an authorized scoped exception or configuration change when available, and cancel. A required-gate exception is labeled separately from advisory continuation; no advisory control disables a hard boundary.\n+\n+Bind continuation and policy changes to current authority and relevant operation/input/evidence/policy versions; recheck before execution/admission. Material changes affecting the decision invalidate that one-time choice, not unrelated choices or an admitted disabled mode. Unknown required bindings and incompatible concurrent changes use ordinary pending/conflict handling. Bypass supplies no consent, export/data-use authority, conformance or safety certification.\n+\n+#### 22.4.3 Decision history, declared values and optional introspection\n+\n+Use \u00a717.0's `decision_recorded` with `safeguard_advisory_bypassed` for explicit continuation, authorized required-gate exceptions and disablement/relaxation, distinguishing the action and old/new rule versions under \u00a722.3.5. Bind the actual presented options/finding/coverage, affected operation, actor, current versions, exception authority when applicable and suppression scope/deadline when relevant. Dismissal/snooze uses `alert_dismissed_or_ignored`; correcting or rejecting a finding retains its independent candidate disposition. User-mode explicit overrides carry `authority_source: user_override`. Reasons are optional with `none_given`; infer no reason when none is supplied and set `preference_inference_allowed: false`. Nonresponse, unknown delivery and absent Logs are never bypass, completed review or assent.\n+\n+Disablement changes the admitted safeguard configuration, not the user's declared respect, care, autonomy, safety or trust values. The user need not renounce a value to disable a check. A declared value alone cannot silently create a required gate or re-enable a disabled rule. Show existing explicit value-linked rules when configuring the check; any rule, value, Preference, Objective or Task revision needs separate explicit review/admission.\n+\n+Bypass history may contribute to an optional per-object introspection candidate only with current purpose-specific authority for every source and derivative. A proposed values conflict must identify an applicable user-declared value/version, independently permitted behavioral evidence and its scope/time, supporting and disconfirming evidence, uncertainty and alternative explanations such as a false positive, changed circumstances or competing priorities. Bypass or repetition alone proves no conflict, motive or Preference. Keep the recommendation, the user's choice and any observed outcome separate; the warning is not independent corroboration of misconduct.\n+\n+Offer inspect, correct, reject, defer and optional review of warning quality/configuration or declared values. Opening review admits no claim. Exact observations or changes need ordinary separate admission; generate no compulsory review Task, blame label, compliance score, punitive Plan/ranking change, privilege reduction or external escalation from an override. Follow \u00a7\u00a721.2.1 and 2.10.1.1 for linked candidate dispositions and suppression. Reads, generation, display, sync and export retain current evidence-use, Compartment, disclosure, retention and ContextBundle rules, including denied existence and derivative purge; audit retention itself grants no introspection or training authority.\n+\n+#### 22.4.4 Repetition and suppression\n+\n+Repeated bypasses are discoverable during user-requested, authorized review; they create no default unsolicited values-review prompt. A separately enabled review-reminder rule may use a user-selected finite count/window for named checks and scope, with finite cadence/attention limits under \u00a7\u00a721.1.7 and 22.3.6. Count distinct bound operations, not replayed Logs, repeated clicks, copied Tasks or Device/view changes. Show the permitted window/count and history gaps; partial or disabled coverage cannot establish a complete behavioral pattern. Threshold crossing offers review of warning quality or configuration; a values-conflict candidate still needs \u00a722.4.3's declared baseline and behavioral evidence. No universal bypass count establishes inconsistency.\n+\n+Coalesce equivalent findings and review offers by check, subject/scope and material evidence. Persist permitted dismissal, snooze, disablement and candidate rejection across views/restart within their admitted scope; do not evade them with new wording, model version, copied work or an introspection handoff. Reopen a dismissal/rejected claim only for relevant material evidence or a user request, explaining the change; snooze follows its chosen deadline. A disabled check requires authorized re-enablement or its explicitly selected expiry. When an advisory is presented, Continue once covers only its bound operation/batch. Suppression changes notice presentation only; it supplies no consent, required-gate exception or external-action approval. Nonresponse and repeated bypasses cannot increase urgency, frequency, audience or channels. Mandatory blocker diagnostics remain available. Broader proactive extrospection and rumination thresholds remain `UBU-Q0089`.\n+\n+#### 22.4.5 Delivery and enablement evidence\n+\n+This resolves the Phase 2 product/UX contract; dedicated orchestration and passing implementation evidence remain separate. Before enabling these controls or claiming conformance, require paired legitimate-context/flagged and concerning-context/missed fixtures; incomplete, disabled and uncalibrated results; all controls with model absence, optional reasons and unchanged canonical values; advisory versus required/authorized-exception versus product/external denial; stale/materially changed and concurrent bindings; replay and copied/cross-Device operations without duplicate counts or prompts; dismissal/snooze/rejection/disablement persistence and authorized re-enablement; opt-in reminder limits, nonresponse and incomplete history; and authorized versus denied introspection/evidence routing, denied existence and derivative purge. Verify exact decision Logs and no compulsory Tasks, silent Preferences or punitive effects. These are required checks, not passing results or a claim that UbU prevents behavioral harm.\n+\n ---\n \n ## 23. Zones, Devices, and Compartments\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\nindex 9acf4e8..249fa89 100644\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -1508,27 +1508,27 @@ Resolved. See UBU-D0165.\n \n ## UBU-Q0101: Advisory safeguard uncertainty, override, and introspection consequences\n \n-Status: Open Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0100 Blocks: safeguard UX, introspection, relationship-transition UX Resolved by: None Last scored: 2026-10-14 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0100 Blocks: safeguard UX, introspection, relationship-transition UX Resolved by: UBU-D0356 Last scored: 2026-10-14 Scored from commit: None\n \n ### Question\n \n How should advisory behavioral safeguards represent uncertainty, false-positive risk, user override, and introspection consequences without becoming paternalistic gates?\n \n ### Subquestions\n \n 1. How should UbU communicate classifier uncertainty and false-positive/false-negative risk?\n 2. What UI should allow review, revision, bypass-once, disablement, or user-configured required gates?\n 3. How should bypass decisions become candidate introspection evidence without becoming soft coercion?\n 4. How should safeguard disablement interact with declared values around respect, care, autonomy, safety, and trust?\n 5. When should repeated bypasses trigger review of the user's values or preferences?\n \n ### Current direction\n \n Advisory safeguards should be default-on, user-aware, uncertainty-transparent, overrideable, and introspection-relevant when bypassed.\n \n ### Resolution\n \n-Open.\n+Resolved by UBU-D0356. DESIGN.md \u00a722.4 defines uncertainty and false-positive/false-negative disclosure, review/revision and scoped continuation, explicit disablement and self-imposed required gates, optional authorized values-linked introspection, and repetition/suppression rules with opt-in bounded reminders. Bypass or disablement alone proves no values conflict or Preference and causes no compulsory or punitive state change. This resolves the Phase 2 product/UX contract; implementation conformance and broader proactive extrospection thresholds under UBU-Q0089 remain separate work.\n \n ---\n \n",
    "commit_message": "docs: define advisory safeguard uncertainty and override UX",
    "validation_notes": [
      "Generated the diff from unmodified current working-tree copies and proposed temporary copies, preserving the selected question's ranking and scoring metadata.",
      "git apply --check passed against the working tree; git apply --check --whitespace=error-all also passed.",
      "The OPEN_QUESTIONS.md hunk includes UBU-Q0101's heading and its own Resolution section; only its status, resolving decision and resolution text change.",
      "Verified all three repository files remained byte-for-byte unchanged. No implementation conformance or classifier calibration results are claimed.",
      "Human approval is required to adopt UBU-D0356 and resolve UBU-Q0101."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0101"
    ],
    "decisions_added": [
      "UBU-D0356"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "openai-ubu-q0101-advisory-safeguard-contract",
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
