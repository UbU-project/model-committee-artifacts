# Model-Committee Work Proposal Request

You are participating in the UbU `model-committee` process.

Your task is to produce one concrete work proposal as strict JSON.

Do not return prose outside the JSON object.

## Selected question

Question ID: `UBU-Q0097`  
Question title: `RelationshipScopeTransition step validation and counterparty autonomy`  
Repository working tree: `/home/sean/ubu-design`

HEAD commit (provenance only): `ea50d7b0f13e20accdb21945b4d1f1e7dc1ca176`

```markdown
## UBU-Q0097: RelationshipScopeTransition step validation and counterparty autonomy

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0095 Blocks: Technique generation, Step validation, autonomy-preserving relationship planning Resolved by: None Last scored: 2026-10-14 Scored from commit: None

### Question

How should UbU validate that every Step in a RelationshipScopeTransition is anchored to user-controlled behavior rather than counterparty mental state?

### Subquestions

1. What completion criteria are malformed because they require counterparty feelings, decisions, or identity change?
2. How should `counterparty_autonomy_acknowledgment` be represented?
3. What counts as authentic refusal, and when does it terminate an exploratory transition Objective?
4. How should one clarification after ambiguity differ from a retry strategy?
5. How should future reopening after refusal require materially new evidence and a new Objective?

### Current direction

All actionable Steps and Tasks must be user-controlled. Counterparty autonomy and valid refusal are explicit model assumptions.

### Resolution

Open.

---

```

## Patch baseline

The patch must apply to the **current working-tree files** in the repository above.
The selected question and context excerpts come from that working tree, which may
include uncommitted ranking and metadata updates.

- Set the JSON `base_commit` field to `ea50d7b0f13e20accdb21945b4d1f1e7dc1ca176` for provenance. It does not
  define the patch's file contents: do not use `git show HEAD:<path>` or the Git index
  as the original version when it differs from the working tree.
- Use exact working-tree lines for all removed and unchanged lines. Preserve existing
  metadata updates and exclude unrelated edits from the proposal.
- To generate the diff, compare unmodified working-tree copies with the proposed
  contents. Validate against the working tree with `git apply --check`; a successful
  `git apply --check --cached` checks the index and is insufficient.

## Canonical design context

The excerpts below are **not** the whole canonical files. They are the subset selected by
following references from the selected question: its dependencies, the decisions those
questions cite, and the sections those cite in turn.

Read them under these rules:

- **Absence is not non-existence.** Ids and sections you cannot see here still exist.
  Never conclude that a question is unasked, a decision unmade, or a section absent
  because it is not shown.
- **Do not renumber or reuse ids.** Allocate new entries from the next free ids below.
- **Patch context must match the real file.** Hunks are applied against the full files,
  not against these excerpts, and `git apply` locates them by surrounding context. Quote
  surrounding lines exactly as they appear here.
- **Scope your edits to what you can see.** Do not delete or rewrite text on the
  assumption that it duplicates something outside these excerpts.

Next free ids — use these for anything you add:

- next question id: `UBU-Q0191`
- next decision id: `UBU-D0347`

`DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md` is read-only context and is not in the patch
allowlist: do not propose changes to it.

### OPEN_QUESTIONS.md — related questions

#### UBU-Q0026

```markdown
## UBU-Q0026: Relationship Maintenance Modeling

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 implementation Resolved by: UBU-D0076 Last scored: Never Scored from commit: None

Resolved. See UBU-D0076.

---
```

#### UBU-Q0087

```markdown
## UBU-Q0087: Relationship declaration versioning and scope drift

Status: Solved Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0026 Blocks: Relationship model, extrospection, RelationshipScopeTransition Resolved by: UBU-D0344 Last scored: 2026-10-14 Scored from commit: None

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

Resolved by UBU-D0344. DESIGN.md §22.1.1 defines immutable declaration fields, scoped supersession/coexistence, effective-time and known-as-of views, evidence-based drift review, and explicit transition/declaration links. User review distinguishes intentional scope evolution from possible unnoticed creep without making counterparty claims. This resolves the Phase 2 data-model contract; implementation conformance, proactive thresholds, typed evidence policies and the full transition model remain separate work.

---
```

#### UBU-Q0094

```markdown
## UBU-Q0094: RelationshipScopeTransition model boundary and reverse references

Status: Solved Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0026, UBU-Q0087 Blocks: RelationshipScopeTransition, Objective targeting, Relationship history Resolved by: UBU-D0345 Last scored: 2026-10-14 Scored from commit: None

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

Resolved by UBU-D0345. DESIGN.md §§7.5, 22.1.1 and 22.3.1 define the versioned Objective target/owner boundary, retained policy-safe reverse references, separately admitted declaration changes, Log/EvidenceRef attribution, and MVP hooks versus post-MVP orchestration. Declined remains an outcome/history view independent of Objective status. This resolves the model contract; implementation conformance and the detailed outcome, taxonomy, Step validation, safeguards and typed evidence-policy questions remain separate work.

---
```

#### UBU-Q0095

```markdown
## UBU-Q0095: Process success and outcome observations in RelationshipScopeTransition

Status: Solved Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0094 Blocks: consent-dependent transitions, Objective status, graceful nonachievement Resolved by: UBU-D0346 Last scored: 2026-10-14 Scored from commit: None

### Question

How should UbU distinguish user-controlled process success from autonomous counterparty outcome observations?

### Subquestions

1. What `process_success_criteria` are valid for unilateral, exploratory, mutual, and retroactive clarification transitions?
2. What outcome observations should be enumerated: accepted, declined, ambiguous, insufficient evidence, no response, or others?
3. How should an Objective be terminal and process-successful when the desired mutual outcome is not achieved?
4. How should Objective status avoid treating refusal as user failure?
5. How should outcome observations update Relationship evidence and hypotheses?

### Current direction

Counterparty response is evidence, not user success or failure. Consent-dependent transitions can be process-successful even when declined.

### Resolution

Resolved by UBU-D0346. DESIGN.md §22.3.2 defines versioned user-controlled criteria for the four existing transition types, scoped response observations, evidence-bound process assessments and ordinary terminal-status mappings. Completed/met can coexist with refusal or unresolved outcomes; refusal is not user failure. Observations feed reviewed, scoped Relationship hypotheses while declaration changes remain separate explicit admissions. This resolves the Phase 2 data-model contract; implementation conformance, detailed Step/refusal validation, taxonomy, safeguards, pacing/support and typed evidence policies remain separate work.

---
```

### DECISIONS.md — cited decisions

#### UBU-D0076

```markdown
## UBU-D0076: Relationship maintenance uses Objectives, history, and risk reports

**Status:** Accepted

Resolved question: `UBU-Q0026`.

In MVP, Relationship remains a structured UniverseState payload between two Identities and stores only the minimal relationship state already accepted: identity refs, the user-controlled Identity, user-stated affect toward the other Identity, and inferred/speculated affect of the other Identity where applicable.

Communication cadence belongs to an evergreen Objective recurrence/reactivation rule, not to the Relationship payload. Interaction evidence belongs in Logs, External Events, Task history, or Compartment/external references depending on source and sensitivity. Message bodies, private notes, or integration-specific history should remain behind Compartment or external-storage references when sensitive.

"Maintain relationship with contributor X" is modeled as an evergreen Objective linked indirectly to the contributor Relationship or relationship-relevant UniverseState key. That Objective can generate Dynamic Tasks such as reply, review, check in, or follow up. Task effects and imported External Events update ordinary UniverseState facts or event markers used to evaluate the Objective; they do not require a special Relationship-maintenance object in Phase 1.

Neglect risk is a risk-report finding derived from the Objective recurrence, observed interaction history, and current Plan/Calendar state. It is not stored on Relationship and does not make risk a first-class MVP object.

GitHub contributor interactions may update relationship-relevant modeled state when they are imported as External Events and associated with the user's GitHub Identity and the contributor Identity. They may satisfy or reactivate a relationship-maintenance Objective, but they must not overwrite user-stated private affect fields or infer sensitive relationship facts as canonical without user acceptance.

For Phase 1, relationship maintenance is supported only through existing Objective, Task, Log, External Event, UniverseState, Identity, Relationship, and risk-report mechanisms. A dedicated relationship-management UI, special cadence schema on Relationship, and full personal CRM behavior are deferred.

**Consequences:**

- Relationship payload stays small and privacy-sensitive.
- Communication cadence remains schedulable and recalculable through Objective recurrence.
- GitHub dogfooding can model contributor follow-up without adding a new relationship subsystem to MVP.
- Future richer relationship-management features can refine cadence evidence and private notes without changing the MVP anchor model.

---
```

#### UBU-D0344

```markdown
## UBU-D0344: Relationship declarations preserve scoped history and reviewed scope evolution

**Status:** Accepted → DESIGN.md §22.1.1. Resolves `UBU-Q0087`; refines `UBU-D0153` and `UBU-D0154` under `UBU-D0076` and `UBU-D0273`.

For Phase 2, represent `ownDeclaredPerspectiveHistory` as immutable, provenance-bearing declarations with identity/Relationship refs, domain/context/dimensions, current-versus-desired participation, exact content or protected source refs, recorded and effective times, correction/withdrawal and scoped supersession links, optional transition/Objective links, and ordinary authority/privacy policy. Keep Phase 1 Relationship maintenance unchanged.

Explicit user-authorized supersession replaces only identified overlapping dimensions, contexts and intervals. Complementary declarations coexist; desired participation never replaces current participation. Corrections and withdrawals append records, preserving effective-time and known-as-of views. Concurrent incompatible replacements require ordinary admitted conflict resolution, with no timestamp or Device winner. Missing or withheld history establishes no absence, and retention/redaction rules govern history and derivatives.

Scope review compares behavior with the declaration applicable at the behavior's time, separately showing declaration changes, desired scope, affect and counterevidence. Reviewed intentional evolution and possible unnoticed creep are distinct review outcomes with uncertainty. A declaration establishes the user's perspective, not counterparty agreement or retrospective authorization. Findings follow `UBU-D0273` and cannot silently change declarations or become planner constraints.

Transitions bind baseline declaration versions and link any separately admitted resulting declaration to its Objective and evidence. Transition completion, refusal or process success cannot itself change current scope. DESIGN.md §22.1.1 defines enablement fixtures; this resolves the data-model contract, not implementation conformance. Proactive thresholds, typed evidence policies, full transition schema and process/outcome details remain with `UBU-Q0089`, `UBU-Q0092`, `UBU-Q0094` and `UBU-Q0095`.

---
```

#### UBU-D0345

```markdown
## UBU-D0345: Relationship scope transitions bind Objective attempts and preserve reverse history

**Status:** Accepted → DESIGN.md §§7.5, 22.1.1 and 22.3.1. Resolves `UBU-Q0094`; refines `UBU-D0154` under `UBU-D0076` and `UBU-D0344`, preserving `UBU-D0155` and §22.4's advisory boundary.

Represent each transition as a versioned attempt for one Relationship and user-controlled Identity, with one owning one-time Objective and reciprocal typed target/owner refs. Pin baseline declaration versions, scope and causal coverage; store desired scope, intent, process/outcome slots, review/evidence and resulting-declaration links on the transition. Objectives and ordinary Tasks retain planning and lifecycle authority.

Relationship exposes a policy-filtered, rebuildable `scope_transition_refs` index sourced from admitted transition links, including attempts without a scope change. Preserve refs across active and terminal Objective states. Declined is an evidence-backed outcome/history view, not an Objective status or process failure. Reopening requires a new transition/Objective pair with explicit lineage; ordinary version, replay and conflict rules govern linkage and revisions.

Only a separately admitted explicit user declaration/correction changes `ownDeclaredPerspectiveHistory`. Link it to the transition, Objective and permitted evidence without erasing scoped history. Transition-generated Logs and EvidenceRefs or evidence associations bind those same refs and source versions, retaining primary Task/Objective targets, source provenance and privacy restrictions. Completion, refusal or supersession cannot itself change current participation.

Keep Phase 1 maintenance and required payload unchanged. Reserve optional MVP target/history/provenance hooks; supported Phase 2 attempts must satisfy the core bindings. Dedicated UI and richer orchestration remain post-MVP, with detailed outcome, type, validation, safeguard and evidence-policy work in the existing questions. DESIGN.md §22.3.1 defines conformance fixtures; this resolves the model contract, not implementation acceptance.

---
```

#### UBU-D0346

```markdown
## UBU-D0346: Relationship transition process success is independent of counterparty response

**Status:** Accepted → DESIGN.md §22.3.2. Resolves `UBU-Q0095`; refines `UBU-D0155` within `UBU-D0344`, `UBU-D0345` and `UBU-D0273`, preserving §22.4's advisory boundary.

Version process criteria and assessments separately from response observations. Criteria evaluate user-controlled actions and checks for unilateral, exploratory, mutual and retroactive clarification attempts, including finite outcome-independent closure branches. They cannot require counterparty assent, feelings or performance. Consent evidence governs eligible branch selection without making assent a success requirement or granting action authority.

Use scoped, provenance-bearing observations with kinds `accepted`, `declined`, `withdrawn`, `ambiguous`, `insufficient_evidence` and `no_response`. Silence is neither refusal nor consent; no response requires evidenced channel/window coverage. Partial and conditional replies remain scoped, and acceptance does not prove implemented scope or continuing consent. Preserve earlier observations, corrections, uncertainty and counterevidence.

A reviewed `met` process assessment permits the owning one-time Objective to become `completed` regardless of the counterparty outcome. Stopping with unfulfilled or unassessable criteria uses `abandoned` with `not_met` or `undetermined`, without framing refusal as personal failure. Authentic refusal closes the exploratory attempt, retires remaining pursuit through ordinary admission and preserves history. Late evidence cannot reactivate it; renewed pursuit requires the existing new-attempt/Objective lineage rules.

Admitted observations become policy-permitted evidence for scoped hypothesis review, not automatic declarations, affect edits, planner constraints or claims about inner states. Declaration changes remain separate explicit user admissions. DESIGN.md §22.3.2 defines the record fields, status mappings and conformance fixtures; this accepts the Phase 2 data-model contract, not shipped orchestration or passing fixtures. Existing taxonomy, Step/refusal validation, safeguard, pacing/support and typed evidence-policy questions retain their remaining scope; Phase 1 maintenance is unchanged.

---
```

### DESIGN.md — referenced sections

#### DESIGN.md §7.5 — RelationshipScopeTransition Objectives

```markdown
### 7.5 RelationshipScopeTransition Objectives

A user may create an Objective to change the user's participation in a Relationship, invite or test a mutual scope change, or clarify whether another party is willing to enter a different scope. This is explicitly permissible self-governance. It is implemented through ordinary Objectives, Techniques, Steps, Tasks, Logs, reviews, and UniverseState mutations, not through a parallel relationship-planning system.

A Relationship scope transition Objective must target user-controlled behavior: disclosures, invitations, boundary-setting, availability changes, communication changes, clarification attempts, and accepted model updates. It must model counterparty response as uncertain and autonomous. For consent-dependent transitions, counterparty response is `outcome_observations`, not user success or failure.

`RelationshipScopeTransition` is a semantic target/reference for such Objectives. Identity, ownership and history bindings follow §22.3.1 (`UBU-D0345`); process criteria, observations and closure follow §22.3.2 (`UBU-D0346`). Additional review fields remain provisional where noted:

- `relationship_ref`;
- `transition_type`: `unilateral`, `exploratory`, `mutual`, or `retroactive_scope_clarification`;
- `current_scope` and `desired_scope`;
- `transition_intent`;
- `scope_history_context`;
- `user_controlled_changes`;
- `counterparty_response_hypotheses`;
- `process_success_criteria`;
- `outcome_observations`;
- `counterparty_autonomy_acknowledgment`;
- `power_asymmetry` and `power_asymmetry_basis`;
- `vulnerability_profile`;
- `prohibited_strategy_types`;
- `epistemic_transparency_requirements`;
- `affect_knowledge_firewall`;
- `pacing_constraints`;
- `graceful_nonachievement_path`;
- `review_checkpoints`;
- `extrospection_expectations`;
- `introspection_expectations`;
- `safeguard_policy`.

A Relationship keeps reverse references to active, completed, abandoned, declined, or superseded RelationshipScopeTransitions that affected it. The Relationship object remains primarily descriptive; the Objective carries the aspirational process.

---
```

#### DESIGN.md §22.1.1 — Declaration history and scope drift

```markdown
#### 22.1.1 Declaration history and scope drift

**Phase 2 contract (`UBU-D0344`).** Lazily attach declaration records or policy-protected references through `ownDeclaredPerspectiveHistory`; Phase 1's minimal payload and Objective-based maintenance remain under `UBU-D0076`. Each admitted declaration records the user's own perspective. Imported or generated interpretations remain candidates until the user confirms their meaning through ordinary admission.

Each immutable record carries:

- `declaration_id`, `schema_version`, `relationship_ref`, and the user-controlled `declaring_identity_ref`;
- `domain`, explicit `scope`/context conditions, the dimensions asserted (such as boundaries, availability or obligations), and `assertion_role`: `current_participation` or `desired_participation`;
- exact user-stated content or a policy-protected `content_ref`, with source/version provenance; optional rationale and permitted `evidence_refs`;
- `recorded_at` and a half-open effective interval `[effective_from, effective_until)`, allowing an open end; unknown effective time stays explicit rather than defaulting to recording time;
- `record_kind`: `statement`, `correction` or `withdrawal`; `supersedes_refs` naming prior declaration ids and the affected dimensions, context and interval; optional `transition_ref` and `objective_ref`;
- ordinary actor/Device, authority, admission/Log or SyncStatement provenance, observed source versions and causal references, plus Compartment, disclosure, retention, redaction and evidence-use policy references.

A new statement coexists with compatible complementary statements or declarations for different contexts, dimensions or intervals. Supersession requires an explicit, user-authorized replacement or withdrawal of identified records for the same Relationship and declaring Identity. Only the named overlap is superseded; unaffected dimensions and intervals remain applicable. Desired participation cannot supersede current participation. Editing content, scope, times or links appends a new record. Withdrawal ends applicability without asserting the opposite. Reject cyclic or invalid supersession references, stage unavailable required references, and deduplicate replay through ordinary admission.

Derive applicability from admitted records rather than updating an old record's content, end time or active flag. Offer both effective-time and known-as-of-history views. Known-as-of queries use the Replica's admitted causal frontier/checkpoint; recording timestamps are provenance, not cross-Device ordering authority. A late correction or retrospective clarification may change today's interpretation of an earlier interval, but preserves what was declared and known before the correction was recorded. A new declaration does not erase observed behavior, establish counterparty consent, authorize disclosure, or retrospectively remove a boundary violation.

Incompatible overlapping declarations without explicit resolution, including concurrent replacements of the same prior declaration, require ordinary conflict review. Never select a winner by wall-clock time, arrival order or Device. Preserve both source records; the contested interpretation remains pending/candidate until an admitted resolution establishes applicability under DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §§9 and 17. Recheck authority, relevant declaration/frontier versions and policy before admission. Partial, stale or restricted views label permitted coverage and uncertainty; unavailable history cannot prove absence of a declaration or absence of drift. Diagnostics and links must not reveal denied content or identifiers.

Compare declaration changes separately from observed scope divergence. A `ScopeAssessment` compares admitted current-participation declarations with permitted behavior for the same dimensions, context and effective interval, using the declaration applicable when the behavior occurred. Desired participation is shown separately. Unknown effective times or ambiguous scope selectors require clarification before an authoritative comparison. A drift finding binds the compared declaration ids/versions, comparison interval and scope, behavior/evidence and counterevidence refs, relevant affect/reflection refs, source provenance, coverage limits, uncertainty and review status. Search for supporting as well as disconfirming evidence where permitted; missing or conflicting evidence yields an explicit unresolved comparison. Affect may motivate review, but negative affect alone establishes neither scope change nor dysfunction. Agreement between declarations and behavior cannot establish the counterparty's perspective.

Distinguish reviewed scope evolution from possible unnoticed scope creep through explicit user review: ask whether the observed change was intended, when it began, and which dimensions the user now endorses. A user-confirmed declaration or scoped review annotation can document intentional evolution of the user's participation. Expansion without matching reviewed intent is only a candidate discrepancy, not proof of unconscious intent or wrongdoing. Reviewing evolution preserves earlier discrepancies and uncertainty about mutual scope. Under `UBU-D0273`, accepting a finding admits a separate scoped observation, annotation or correction; changing the declaration baseline additionally requires an explicit user declaration/correction. Rejection retains the existing privacy-minimized suppression record. Findings cannot silently update scope or become hidden planner constraints. Proactive evidence-count, recency and salience thresholds remain `UBU-Q0089`; typed evidence-policy enums remain `UBU-Q0092`.

A deliberate transition binds its `scope_history_context` to baseline declaration ids/versions and records a separate desired scope. Any resulting declaration explicitly links the transition, Objective and permitted review/Log evidence; Relationship reverse references retain transitions across active, completed, abandoned, declined and superseded states. Starting, completing or declining an Objective never automatically changes current participation. A process-successful invitation with a refusal leaves the current declaration intact unless the user separately declares a change in the user's own participation. Retrospective clarification records its effective interval and later recording time. Transition identity, Objective ownership, reverse references and provenance follow §22.3.1 (`UBU-D0345`); process/outcome details follow §22.3.2 (`UBU-D0346`).

Before claiming implementation conformance, require fixtures for complementary scopes, partial supersession at interval boundaries, withdrawal, future/desired declarations, late correction with both historical views, concurrent offline replacements and replay, behavior/affect disagreement, incomplete or denied evidence, and completed/declined transitions without automatic declaration changes. Verify preserved source history, policy-safe views and ordinary review/admission outcomes. History retention follows current policy; authorized redaction/purge removes disallowed dependent content and leaves only permitted gap metadata.
```

#### DESIGN.md §22.3.1 — Objective target, reverse history and provenance

```markdown
#### 22.3.1 Objective target, reverse history and provenance

**Phase 2 model boundary (`UBU-D0345`).** One admitted `RelationshipScopeTransition` represents one deliberate attempt concerning one Relationship and its user-controlled Identity, owned by one `one_time` Objective. The Objective carries a typed `target_ref` to the transition; the transition carries the reciprocal `objective_ref`. Ordinary Objectives, Techniques, Steps and Tasks own value, recurrence, execution and scheduling. Transition records hold the attempt's scope, intent, review context and provenance.

The versioned transition record carries:

- `transition_id`, `schema_version`, `relationship_ref`, `acting_identity_ref` and `objective_ref`; the acting Identity must be the Relationship's user-controlled side;
- `transition_type`, scoped `transition_intent`, `desired_scope` and `user_controlled_changes`, with protected content refs where needed; the existing type vocabulary and review payload remain under §§7.5 and 22.3;
- `scope_history_context`: baseline declaration ids/versions (an empty set requires an explicit unknown/coverage reason), domain/context/dimensions and effective interval, the admitted causal frontier/checkpoint used, and permitted coverage/uncertainty; `current_scope` is a derived, policy-safe view of that baseline;
- separate `process_success_criteria` and `outcome_observations` slots, permitted review/Log and `evidence_refs`, and derived `resulting_declaration_refs` identifying separately admitted declarations;
- optional `supersedes_transition_ref` or `reopens_transition_ref` with reason and permitted supporting evidence; these identify earlier attempts, not declaration supersession;
- ordinary object-version, actor/Device, effective/recorded-time, authority, admission/Log or SyncStatement, causal and idempotency provenance, plus Compartment, disclosure, retention, redaction and evidence-use policy refs.

Admit the reciprocal Objective/transition linkage as a coherent batch; stage it if required dependencies are unavailable. Stable refs identify objects, while review, planning and admission bind exact observed versions. Ownership, Relationship and acting Identity cannot be rebound to a different attempt. Scope, intent, baseline and evidence changes create admitted revisions retaining prior versions. Missing baseline history stays explicitly unknown; a changed applicable declaration requires renewed review before relying on the old baseline, never silent rebasing.

**Relationship reverse history.** Lazily expose `scope_transition_refs` as a policy-filtered index of admitted transitions whose `relationship_ref` names this Relationship. Attach the reverse association at attempt admission, including attempts that produce no scope change. Its source is the admitted transition linkage; a materialized index is rebuildable and carries no independent lifecycle or scope authority. Relationship retains its descriptive Identity, affect, declaration and observation histories; transition intent, criteria, safeguards and scheduling do not move onto Relationship. Concurrent attempts for different contexts may coexist; incompatible admitted changes use ordinary conflict review.

Retain stable reverse refs through active, completed, abandoned, superseded and invalid Objective states, subject to current retention policy. Lifecycle views follow the owning Objective's §7.3 status and append-only `objective_transitioned` Logs. `declined` is a separate outcome/history filter supported by an admitted scoped response observation, not an Objective status or proof of process failure; it can coexist with a terminal process-successful Objective. Never derive refusal from silence or missing evidence. A superseding attempt records the prior owning Objective's ordinary §7.3 supersession transition and retains both reverse refs. Terminal attempts do not reactivate; reopening uses a new transition and new Objective with lineage, materially new evidence and the existing stronger-review rule. Reject cross-Relationship/Identity lineage and lineage cycles. Detailed criteria and terminal-status mappings follow §22.3.2 (`UBU-D0346`).

**Declaration changes.** A transition may propose a §22.1.1 declaration; only a separate explicit user declaration/correction admitted under `UBU-D0344` changes `ownDeclaredPerspectiveHistory`. Bind the new declaration to `transition_ref`, `objective_ref` and permitted review/Log evidence; validate matching Relationship and declaring/acting Identity on those links. Derive the transition's `resulting_declaration_refs` index from those admitted records. Preserve effective/recorded times and scoped supersession/coexistence. Desired scope, Task or Objective completion, abandonment, refusal and transition supersession cannot themselves replace current participation. A completed attempt may have no resulting declaration; a declined attempt may still accompany a separately declared change to the user's own participation.

**Logs and evidence.** Use §17's append-only envelope and appropriate existing event types. Keep a Task or Objective event's primary `target_ref`; its `event_payload` additionally binds `transition_ref`, owning `objective_ref`, `relationship_ref`, relevant observed versions and permitted Step/Task, declaration and evidence refs. Record admissions, revisions and corrections with ordinary provenance and idempotency, preserving original events. Transition-generated EvidenceRefs, or transition-scoped associations for reused evidence, bind the same transition/Objective/Relationship refs to source id/version, selector or interval, purpose, provenance and applicable policy. Linking evidence does not rewrite its source or make generated summaries independent evidence. Adding links to existing Logs uses annotations/corrections.

Target links, reverse indexes, Logs, EvidenceRefs, declarations and cached views inherit source restrictions. Recheck current authority, object/source versions and policy before admission, read, sync or export. Partial or denied views cannot prove absence of an attempt or declaration; omit denied identifiers, counts and existence. Authorized redaction/purge removes disallowed dependent content and leaves only permitted gap/causal metadata. Concurrent incompatible revisions or links require ordinary admitted conflict resolution under DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §§9 and 17, with no wall-clock, arrival-order or Device winner.

**Delivery boundary.** MVP hooks are optional typed target/reverse-reference and provenance bindings through ordinary Objective, Task and Log mechanisms. Phase 1's required Relationship payload and maintenance cadence remain under `UBU-D0076`; ordinary Relationships need no transition object or empty history array. A supported Phase 2 attempt must meet this core contract before admission. Dedicated relationship-management UI, automated multi-stage orchestration and personal CRM behavior are post-MVP work. Process/outcome semantics follow §22.3.2 (`UBU-D0346`). `UBU-Q0096`, `UBU-Q0097`, `UBU-Q0098` and `UBU-Q0099` retain their detailed taxonomy, validation and safeguard scope; typed evidence policies remain `UBU-Q0092`.

Before claiming implementation conformance, require fixtures for reciprocal linkage and rejected mismatched owners, reverse-index rebuild/replay without duplicates, all retained lifecycle views including a completed process with a declined outcome and no declaration change, explicit resulting declarations and retrospective correction, stale baselines, concurrent offline revisions, lineage cycles and reopening with a new Objective, Task/Log/EvidenceRef attribution, and denied/partial views plus derivative purge. This accepts the model boundary, not passing fixtures or shipped transition orchestration.
```

#### DESIGN.md §22.3.2 — Process success, response observations and terminal closure

```markdown
#### 22.3.2 Process success, response observations and terminal closure

**Phase 2 data-model contract (`UBU-D0346`).** Refine `UBU-D0155` within §22.3.1's attempt/Objective bindings. Process success evaluates the user's actions against admitted criteria; response observations describe scoped evidence about an autonomous counterparty. Neither dimension substitutes for the other, and completing the process does not assert that the desired mutual scope was achieved.

**Process criteria.** `process_success_criteria` is a nonempty, versioned set of criteria with `criterion_id`, `scope`, a user-controlled `action_or_check`, `applicability_condition`/`closure_branch`, and permitted `evidence_requirements`. Bind criteria and assessments to the attempt's exact baseline and criteria versions. User attestations may supply evidence where ordinary policy permits; Task completion satisfies only criteria supported by its admitted evidence. Distinguish a communication attempt from verified delivery or receipt. A criterion may require checking available consent evidence before acting, but cannot require obtaining assent, a response, a feeling or an Identity change. Such a check supplies no action authority: applicable authorization/policy still governs consent-dependent actions, and an unresolved-outcome closure branch remains available.

Each attempt specifies finite closure conditions and an authorized way to stop without claiming unmet criteria were satisfied; where waiting is relevant, bind a user-selected observation window. Define response-dependent branches before relying on them; do not retrospectively waive unfulfilled user actions to award success. Branch selection may depend on admitted response evidence, but every branch's required action remains user-controlled. Behavioral reviews retain §22.4's advisory or user-required policy; this contract creates no new classifier-based hard gate.

| Existing transition type | Valid process criteria |
| --- | --- |
| `unilateral` | Decide and carry out the user's own scoped availability, disclosure, communication or participation change; communicate or record it where the admitted criteria require. Counterparty approval is unnecessary for the user's own participation and supplies no authority over the counterparty. |
| `exploratory` | Make the scoped disclosure/proposal, record permitted response evidence or uncertainty, and close the attempt. Preserve §22.3's single proposal and at most one clarification after ambiguity; a refusal branch requires ending pursuit, not obtaining a different answer. |
| `mutual` | Check the scope and currency of reported agreement before each consent-dependent action, carry out the user's own coordination commitments where eligible, and record results. Provide a closure branch when agreement is withheld, withdrawn or unresolved; another party's promised performance is never a user completion criterion. |
| `retroactive_scope_clarification` | Identify the disputed dimensions and historical interval, make any applicable clarification attempt, and record what remains known or unresolved. A separately chosen declaration/correction follows §22.1.1; agreement with the user's historical interpretation and retrospective authorization are never required or implied. |

**Response observation records.** Append `outcome_observations` with stable observation ids, the §22.3.1 transition/Objective/Relationship and source-version bindings, attributed responding Identity, proposal/scope version, domain/context/dimensions and conditions, exact content or protected source refs, evidence and counterevidence refs, effective/observed and recorded times, coverage/uncertainty, and review/admission provenance. Unknown times and uncertain attribution remain explicit. Corrections or scoped replacements name earlier records and preserve them under current retention policy. Distinguish an attributed user report from a direct or authenticated imported statement; a generated interpretation remains candidate and a summary is not independent evidence.

The `observation_kind` vocabulary is:

- `accepted`: evidence of expressed assent to the identified proposal, scope and conditions at that time; it is neither blanket/continuing consent nor proof that the scope change was implemented;
- `declined`: evidence of expressed refusal of that proposal or identified part of it; it does not establish rejection of the user or the whole Relationship;
- `withdrawn`: evidence that earlier expressed assent was retracted for identified scope, linking the earlier observation; earlier assent remains historical evidence;
- `ambiguous`: available response material supports multiple readings that cannot yet be resolved;
- `insufficient_evidence`: permitted evidence cannot support a response classification, including unavailable, unreliable or incomplete coverage;
- `no_response`: no response was observed in named channels during a declared finite interval, supported by a known communication attempt and evidenced response-observation coverage. Record delivery/receipt status separately, including unknown; this classification cannot prove the message was read. Incomplete or withheld response coverage requires `insufficient_evidence`.

Silence, elapsed time, affect and missing records never establish acceptance, refusal or withdrawal. Split partial/conditional responses into scoped observations rather than labeling an entire proposal accepted. Unilateral attempts may have no applicable response observation; an empty history is not `no_response`. Later responses append evidence without erasing earlier uncertainty or unanswered intervals. Preserve incompatible observations and counterevidence; current scoped interpretations require ordinary review/conflict resolution, never a timestamp, arrival-order or Device winner.

**Assessment and Objective status.** A versioned `process_assessment` on the transition records an assessment id, `result` (`met`, `not_met` or `undetermined`), bound criteria/baseline versions, selected closure branch and its basis, per-criterion judgments, permitted evidence refs, coverage/uncertainty, and authority/review/Log provenance. `met` requires evidence for every applicable criterion, including closure. `not_met` requires evidence that at least one applicable user-controlled criterion was unfulfilled. Otherwise, missing evidence for any applicable criterion yields `undetermined`. Inapplicable branches record their scoped reason. Assessments append revisions and cannot silently change the criteria.

Use §7.3's existing one-time statuses. Admit `active` → `completed` with a reviewed `met` assessment, even with `declined`, `withdrawn`, `ambiguous`, `insufficient_evidence` or `no_response` observations. If the user stops before satisfying applicable process criteria, use `abandoned` with the actual assessment and reason; neither label is a judgment of personal failure. `invalid` remains for a malformed/impossible process Objective or mistaken admission, and `superseded` for ordinary replacement. A counterparty refusal alone supplies neither invalidity nor a `task_failed` event.

An admitted authentic refusal ends the current exploratory attempt under §22.3: use `completed` when its refusal-closure criteria are met, otherwise `abandoned` with `not_met` or `undetermined`. Retire remaining pursuit Tasks through ordinary admission and invalidate/recalculate affected Plans. Log terminal transitions with the assessment, criteria versions, scoped observations and closure reason under §22.3.1. A graceful nonachievement path records this closure branch and optional later reflection/review; emotional recovery or acceptance of an interpretation is not a completion requirement. Late evidence, including withdrawn assent, may change action eligibility and trigger ordinary Plan invalidation, but cannot reactivate a terminal attempt or rewrite its historical assessment. Renewed pursuit follows §22.3.1's new Objective/transition and lineage rules.

**Relationship evidence and hypotheses.** Link admitted observations into the Relationship's permitted evidence history through §22.3.1's associations. They support or challenge only domain-, scope- and time-matched counterparty hypotheses. Propose hypothesis revisions with evidence, counterevidence, confidence/uncertainty and review provenance under `UBU-D0273`; unreviewed interpretations cannot silently become durable Relationship state or planner constraints. An observation establishes what was reported or expressed, not privileged knowledge of feelings or motives. Process success and refusal cannot overwrite affect or declarations. Current participation changes only through §22.1.1's separate explicit user declaration/correction. All observations, assessments, links and derivatives inherit §22.3.1's admission, concurrency, Compartment and purge rules.

Before claiming implementation conformance, require fixtures for all four types and rejection of assent-dependent success criteria; completed/met with scoped refusal and no declaration change; abandoned with independently unfulfilled criteria; ambiguous versus insufficient versus covered silence; conditional/partial assent and later withdrawal; retrospective clarification without retrospective consent; late responses without reactivation; stale criteria/evidence, replay and concurrent conflicting observations; and denied coverage plus derivative purge. These are required checks, not passing implementation evidence. `UBU-Q0096`–`UBU-Q0099` retain taxonomy, detailed Step/refusal validation, advisory safeguards and pacing/support policy; typed evidence policies remain `UBU-Q0092`.
```

#### DESIGN.md §1 — Overview

```markdown
## 1. Overview

**UbU** is a privacy-first planning, coordination, and self-governance system for implementing life logistics.

UbU takes messy real-world inputs—tasks, calendar events, messages, external events, user preferences, physical/emotional state, Resource availability, Skill capability, integration data, realtime interaction streams, and agent or worker outputs—and turns them into explicit, inspectable, recalculable Plans.

UbU is not merely a task list or calendar application. Its core motivation is to help an individual human being plan and implement the logistics of an actual life: what matters, what must happen, what the world must contain, what the user is capable of doing, what can be learned, what should be bought or delegated, and what should happen next. Organizational coordination is an important emergent property of the same model, not the root purpose.

The canonical UbU use-case formula is:

> **Objective + Current State + Constraints + Resources + Skills + Techniques + Preferences + External Options → Legitimate Plan**

The canonical use-case statement: UbU helps an individual user solve everyday life problems by transforming a desired outcome into a legitimate, executable plan. It does this by modeling the current state of the user's life, the desired state, required Tasks, available and missing Resources, available and missing Skills, reusable Techniques, affect and energy constraints, financial tradeoffs, public or marketplace options, expert-guided alternatives, and evidence from execution. UbU's purpose is not merely to schedule time, but to help the user determine what path can realistically move life from the current state toward the desired state.

The deeper purpose is to help people build the capabilities, resources, routines, and relationships needed to actually live the life they choose — not just to schedule it.

**Access within ordinary life logistics (`UBU-D0338`).** Present guided camera acquisition and voice/text/form choices as ordinary ways to capture concerns, review proposals and act on Plans. Keep UbU's primary identity as a privacy-first life-logistics system; access features are available without a disability label or special onboarding category. Describe concrete benefits and limitations for tested workflows and Device/assistive-technology combinations. Camera availability alone establishes no accessibility claim.

The first MVP is designed around **dogfooding**: using UbU to coordinate the design, development, release, and maintenance of UbU itself.

Recent LLM and agentic-AI changes reinforce UbU's core boundary: realtime, multimodal, tool-using, memory-bearing models are valuable interaction and extraction backends, but UbU remains the user-sovereign state-transition, planning, logging, privacy, and review layer.

**Advisory generation policy (`UBU-D0341`).** For the MVP/Phase 2 advisory boundary, the operator configures sampling behavior in the selected inference runtime. UbU adds no generic decode Settings or application-selected temperature, top-p/top-k or repetition penalties, hidden request overrides or automatic retry tuning. Existing model/endpoint selection, timeout and review seed/ceiling behavior remain; the review seed provides no cross-model or cross-runtime reproducibility guarantee.

UbU owns bounded validator-safe producer grammars (`UBU-D0302`), finite request/output and job budgets, cancellation, validation and ordinary candidate review/admission. Required grammar and resource/output-limit fields may be sent to the runtime without transferring these contracts to sampler policy. A runtime unable to meet them leaves the affected operation unavailable with an explicit limitation and eligible retry/manual route. Abort, timeout and invalid output preserve saved state and admit no generated state changes. Retries retain current authority/input bindings and finite call/time budgets; failure cannot relax validation or authorize another route.

Document each supported adapter's required grammar/limit behavior and runtime sampling configuration instructions. Pin observable sampling configuration or its digest in qualified Device/backend profiles and label unknown effective defaults explicitly. Configuration changes require affected qualification checks under `UBU-D0331`/`UBU-D0336` where applicable. Before claiming conformance, require adapter fixtures that inspect requests for absence of application-selected sampler overrides while preserving existing review seed/ceiling behavior and required grammar/limit fields; exercise abort, timeout and invalid-output paths for preserved saved state, bounded retries and unchanged review/admission requirements. These are required checks, not passing runtime evidence or a diagnosis of the Vocabulary abort.

For Phase 1, UbU must also be understandable as a first-person user experience. The minimal user-facing loop is: answer a small number of bootstrapping questions, allow UbU to construct an initial context model, receive one recommended next Task, inspect why that Task matters now, act or override, then let UbU learn from the result through Logs, Snapshots, and recalculation.

For Phase 2, a single-live-session sync implementation is sufficient (`UBU-D0312`), with arbitrary N Devices, concurrent offline mutations, partial replicas, and transport-independent integrity-protected statements. Direct exchanges, imported bundles, and user-selected encrypted stores share the same causality, policy, and conflict rules; no Device or transport becomes a permanent canonical authority. Full automatic peer mesh is optional. DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §26's topology research remains a freeze gate, with at least three Devices demonstrating serialized direct exchange and encrypted store-and-forward, recoverable delivery, deterministic admission/conflict handling, and Compartment-safe partial replication as specified by `UBU-D0312`.

The minimum indirect path for the first Phase 2 sync release is implemented, authenticated encrypted bundle export/import through a user-selected file store, with at least one documented external rclone or rsync copy workflow exercised before freeze (`UBU-D0313`). Manual export/copy/import is sufficient; built-in clients and background delivery are optional. Encryption and recipient authorization protect statements, payloads, and manifests before storage, while ordinary admission and Compartment policy govern every import.

**Interrupted sync recovery (`UBU-D0314`).** Maintain a durable, policy-protected recovery record throughout each exchange. Connection loss, interrupted import, or restart without an admitted final checkpoint for the declared exchange scope records `incomplete_sync_session`. End the failed live session and release its slot while preserving recovery records and queued work; other Devices may synchronize and ordinary policy-authorized offline work may continue against an explicitly stale local view.

Fully verified, authorized statements whose required causal/admission dependencies are satisfied may be admitted incrementally. Persist each admission outcome, resulting state, and deduplication metadata as one crash-consistent unit. These admissions survive disconnect, retry, and discard, subject to existing redaction/purge policy. Partial statements, unverified payloads, and items with missing required statements or dependencies remain staged; they cannot be treated as admitted mutations or supply authoritative inputs to Plans or external projections.

An integrity-protected `checkpoint` or equivalent authenticated manifest commit must bind the declared exchange scope and expected statement/payload inventory, or its content-addressed equivalent. Admit it only after all material required for this Replica's authorized replication level has been durably verified and the required dependency/admission checks have completed. Report completion only after that checkpoint admission is durable, and only for that local scope. A checkpoint cannot admit contested mutations or prove other Devices current. A lost acknowledgment leaves delivery unknown to the sender; replay cannot undo the recipient's durable admissions. Outgoing statements remain pending for an intended recipient until the sender durably records a matching authenticated acknowledgment of that recipient's durable checkpoint admission. Exchange acknowledgments directly or through encrypted return bundles; transmission, file-copy success, or local checkpoint creation is insufficient.

The recovery record tracks exchange/bundle identity, source/recipient Device references, authenticated manifest reference if available, last locally admitted checkpoint, received/missing content references, and durable per-statement admission/idempotency outcomes. Reconstruct progress after restart from durable records. Recovery storage, dependency fetches, retention, and diagnostics obey current Zone/Compartment policy; restricted views reveal neither denied payload nor Compartment identity. Retention cleanup preserves permitted causality, deduplication, and incomplete/discard outcome metadata; missing content must be retransmitted rather than assumed received.

At the next authorized connection or import, resume from durable progress, or replay the original statements if no usable cursor exists. Preserve original statement ids, content hashes, idempotency keys, causal parents, and observed versions. Recheck integrity, Device trust, actor authority, current routing/Compartment policy, object/policy versions, and dependencies before admitting pending material or disclosing retained content. Recovery records and old checkpoints grant no authority. Automatic recovery is allowed only for deterministic, idempotent, non-authority-changing cases under `UBU-D0248`; revoked Devices and review-required conflicts retain their ordinary rejection/review rules.

Offer Resume/retry and Discard through the existing policy-safe incomplete/stale-sync diagnostic, showing the last completed scope and permitted pending counts. Discard removes unfinished incoming staging and its cursor while preserving already-admitted history, locally authored queued work, and still-required policy-permitted causality/idempotency metadata; record the discard outcome. It does not emit a tombstone or undo admitted mutations. A transient interruption alone requires no blocking merge prompt. Review-required conflicts use DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §17's blocking `manual_decision` diagnostic and admitted `conflict_resolution` statement. Apply DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §22's Plan invalidation/recalculation rules to admitted changes even while the exchange remains incomplete.

**Sync-state warnings (`UBU-D0315`).** Multiple Devices may remain powered on and perform policy-authorized offline work simultaneously; only live sync exchanges serialize under `UBU-D0312`. Explain this once in Phase 2 onboarding. Ordinary offline use, Device switching, peer absence, and elapsed time alone cause no modal warning or recurring notification. Time-based reminders are opt-in with a user-selected cadence.

Keep one compact, non-blocking status for the active permitted sync scope, with details on demand: last durably admitted checkpoint and its scope/time (or "No completed exchange yet"), permitted locally authored pending counts split into awaiting exchange and awaiting recipient acknowledgment, staged incoming work, and incomplete-session state with Resume/retry and Discard per `UBU-D0314`. Say "Delivery unconfirmed" for a missing acknowledgment. File-copy success and a completed local exchange never justify "All Devices are up to date". A disconnected peer's changes are unknown unless policy-permitted evidence establishes pending state; age alone proves neither new changes nor their absence.

Show an inline Plan/Calendar freshness explanation where relevant inputs may be missing or stale: "This Plan uses this Device's current view; other Devices may have changes." Identify confirmed remote pending changes only when supported by permitted evidence. Locally authored outgoing work is saved local state, not evidence that the local Plan is stale. Label protected blocks and policy-withheld items separately from exchange failures; retrying sync cannot expand visibility.

Before external projection whose relevant permitted inputs have known pending changes, an incomplete exchange, or unknown peer freshness, present an inline review: "Other Device changes may be missing. Synchronize before projection, or use this Device's current view." Offer Sync/exchange (including export/import), Continue with current view, and Cancel. Continue requires explicit stale-state acceptance once per projection operation or reviewed batch, bound to the Device, operation, payload/input and policy versions, and permitted scope. Record it under §17.0 using `decision_recorded` with `decision_kind: safeguard_advisory_bypassed`; recheck the bindings before execution. This accepts freshness uncertainty only. Current authority, Device trust, integrity, Compartment/export policy, Plan-legitimacy requirements, and `UBU-D0248`'s blocking `manual_decision`/`conflict_resolution` rules still govern eligibility. If those rules forbid execution, omit Continue. Mere interruption or missing peer contact creates no blocking merge prompt.

Coalesce advisory explanations by Device, permitted scope, condition, and material evidence. Offer Dismiss until material change or Snooze until a user-selected time; persist the local choice across views and restart while keeping status discoverable. Reopen for material changes affecting that warning, such as newly learned permitted pending statements or changed relevant Plan/input versions; snooze may also reopen at its chosen deadline. A new projection operation needs its own scoped choice. Age or restart alone cannot reopen a dismissed advisory; opt-in reminders may follow their chosen cadence. Dismissal needs no justification and is logged under §17.0 without turning the warning into a synchronized Task. Advisory suppression cannot hide a required blocking conflict diagnostic.

Generate visible text, counts, urgency, and reappearance only from evidence or derived warnings allowed at this Replica's replication level. Restricted notices use DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §23's "A protected object cannot sync to this Device." without denied payload, source object identifiers, or Compartment ids/labels; omit object-specific notices when even existence is denied. Warnings preserve intentional partial replication and never recommend powering off other Devices or switching to a single primary Device.

**Future schedulable manual review (`UBU-D0316`).** A later versioned extension may admit a shared Dynamic review Task with independent, coherent scheduling inputs and a protected conflict-episode identity. Its target is evidence to review, not admitted contested effects. Place it only in validated unaffected Calendar capacity; otherwise keep it unplaced. The diagnostic/prompt must still admit a permitted source-conflict `conflict_resolution` without review Task creation, placement, or completion. Conflicts in the Task, its scheduling/projection artifacts, or resolution workflow terminate at that diagnostic and never spawn review-of-review Tasks. Task lifecycle reconciliation follows resolution; snooze or deletion cannot clear the underlying blocker. Current authority and source privacy govern every step. The first Phase 2 sync release retains §17's diagnostic-only model until this extension's versioned contracts and behavioral gates pass.

**Task lore after Clarify (`UBU-D0317`).** After a Clarify round, persist the operator's submitted answers through ordinary admission independently of synthesis, then offer one additional local synthesis job. The Task description is readable narrative. Keep the pre-existing description, subsequent operator edits, exact questions/answers and their necessary context as separate durable per-Task source records with stable ids, versions, actor/Device/session/round provenance, effective/recorded times, hashes and privacy/retention policy. Versioned context refs are sufficient only while the referenced context remains retrievable. Corrections append records with explicit supersession links; they never edit originals. Accepted model prose is a derived revision, not independent evidence. Raw audio/video retention is not required.

The local model returns an `AdvisoryCandidate` containing proposed prose, model/template provenance, the complete source manifest/digest and a source-to-prose coverage map. Every answer, qualification, original lore statement, correction and unresolved issue must map to prose spans; every factual prose assertion must map back to sources. Equivalent repetitions may share wording. Superseded assertions retain a temporal account of the old and corrected meanings; disagreements, unknowns, declined answers and open questions remain explicit. Preserve attribution, negation, conditions, quantities and uncertainty. Unsupported inferences remain separate labelled suggestions, not synthesized lore.

Review shows the description diff and a source-by-source checklist with exact source text beside its mapped prose. Deterministic checks require complete source/span coverage, valid references, finite bounds and current bindings; structural coverage and another model's opinion cannot establish semantic fidelity. Explicit operator approval confirms readable prose preserves all source meanings and invents no facts or agreement. Offer edit, regenerate or keep the prior description; edited prose needs renewed coverage review. Recheck authority, policy, Task-description/source/candidate versions and source digest before atomically admitting the description revision and its provenance. Concurrent changes invalidate approval and use ordinary conflict review. Failure, rejection or model unavailability preserves the prior description and saved answers. Lore approval does not admit typed planner constraints or authorize external action.

Later Clarify uses admitted prose plus the source manifest, unresolved issues and correction links. Supply exact question/context/answer records for topics being revisited and make remaining permitted originals retrievable. Check the originals before treating a topic as settled; absence from a prompt is not absence of knowledge. Missing records make context explicitly incomplete. A partial Replica unable to establish the full source frontier may offer a scoped preview but cannot replace the Task description.

Declare finite storage, prompt, result and job time/call limits. Retain source history and admitted revision/provenance while the Task is retained; archival must remain queryable. Chunk long inputs along complete source records and track coverage across chunks through the final prose. If complete reviewable prose cannot fit, stop and keep saved sources rather than truncate or drop knowledge; no cloud fallback. At storage exhaustion, pause Clarify and show any unsaved input; never claim it saved or silently evict history. Rejected candidate payloads follow the ordinary minimized retention lifecycle.

Source records, synthesized prose, coverage maps and subsequent context inherit all source disclosure, routing and retention restrictions; local processing does not waive them. Reject synthesis into a Task whose destination policy cannot protect its sources. Recheck policy on every read, sync and export. Authorized redaction/purge also removes disallowed content from dependent prose, revisions and cached context; prevent reconstruction from retained derivatives. Keep only permitted minimized gap metadata, disclose incompleteness without revealing denied existence, and never claim lossless retention beyond that scope.

**Recurring-busy workaround retirement (`UBU-D0318`).** Timed expanded recurrence instances already capture as individual Tasks. Retire a temporary manual capacity blocker only for an operator-reviewed source/series and planning scope with verified replacement occupancy. Full series-level import is not a prerequisite when expanded instances cover that scope; unsupported inputs, missed/on-demand capture and incomplete coverage retain explicit residual blocking.

Record a policy-permitted coverage manifest binding the implementation revision, source/series references, capture window/cursor and completeness evidence, source and admitted input versions, planning horizon, expected busy UTC intervals, occurrence-to-Task lineage, and blocker references. Every relevant busy occurrence must map to one active capacity-bearing representation; record how any uncovered part of the blocker remains protected. Titles or coincident times do not establish occurrence identity. Repeated capture, restart and replay preserve lineage without duplicate Tasks. Moves, cancellations and series changes supersede prior active reservations through ordinary admission while preserving logged history; absence from an incomplete response cannot establish cancellation.

Fixed busy commitments use capacity-occupying Static Tasks at validated UTC start/end times and bound chunks under `UBU-D0279`. Capture counts, Calendar display and Task existence alone do not prove occupancy. Verify the CPU reference path and each enabled planner backend against expected occupied/free intervals: flexible placements cannot enter busy time, and a source occurrence, its projection and its temporary blocker cannot consume capacity twice. Distinct commitments that genuinely overlap retain their conflict rather than being deduplicated by time.

Before removal, show the exact blocker replacements and residual intervals and obtain explicit operator approval bound to the manifest and current versions. Apply them through ordinary admission as one batch, or an equivalent serialized handoff that prevents Plan certification/projection of intermediate missing or doubled occupancy. Invalidate affected Plans and certify recalculation before reuse or projection. Failure or stale inputs preserve protection and require fresh review. Retire only the identified workaround, preserving source commitments and history. Advancing the horizon or receiving material source changes requires renewed coverage checks before using affected capacity; missing, stale or policy-withheld data never proves free time. Evidence and diagnostics obey current Compartment policy; this process neither grants capture/export authority nor bypasses existing sync/projection checks.

**Measured local multimodal hardware (`UBU-D0331`).** Qualify an exact physical-hardware/software profile, separately identifying every execution-enclave Device and its capture, review, inference and optional planning roles. Pin hardware/RAM, OS build and security-update posture, drivers/runtime, model and weights digest, quantization, context/media sizes, storage/key configuration, UbU/template and policy revisions. Declare enabled sources, latency classes, concurrency, supported session duration and operating conditions. Primary mobile interaction does not select a full mobile planner or compute API (`UBU-Q0160`); capture-mode/device allocation and delivery sequence remain `UBU-Q0183`.

Before testing, freeze the scenario set, useful-outcome rubric and finite acceptance limits for quality, latency/freshness, memory/storage, battery/thermal behavior and cancellation. Use §21.1.3's ordinary interaction and text/form workflows, with synthetic or expressly consented fixtures covering visible details, exact names/values, noisy speech, ambiguity, corrections and unsupported requests. Review grounding, preserved uncertainty, fabricated assertions, correction burden and useful candidate-to-admission/next-action outcomes; transcripts, token rates and plausible conversation alone cannot pass. Count accepted, corrected, unresolved, rejected and failed outcomes separately over all trials. Numeric interaction profiles follow §21.1.4 and remain `UBU-Q0168`'s measured work.

For each proposed Device/backend/mode, run at least 30 cold-start and 100 warm trials using the pinned fixtures, plus at least 30 minutes of its proposed concurrent capture/inference/UI workload and the full maximum supported session if longer. Measure the complete capture-to-review route, including any authorized worker transfer. Record ambient conditions, charging/battery state, competing load, instrumentation and repeat procedure. Report §21.1.4's separate timings with sample counts, median, p95, maximum and misses/errors; include peak aggregate CPU/accelerator memory, model loading, buffers/caches and concurrent manual/planning work at the declared state/context limits. Report model/state/approved-artifact storage, peak scratch and required free-space reserve, energy/battery measurements and uncertainty, temperature/throttling and performance over time. Unavailable metrics stay explicit; missing evidence for a required limit prevents qualification. A short successful run cannot establish sustained or longer-session support.

Endpoint qualification must demonstrate a maintained security-update posture, installation/model integrity checks, OS access isolation, protected Device keys and access-controlled encrypted persistent storage with documented key custody and recovery. State the threat envelope: locked-device loss, unauthorized unprivileged processes, unintended persistence and unauthorized egress; do not claim protection from a compromised OS or malicious local administrator. Exercise source/Device revocation, lock/restart, denied reads/egress, storage exhaustion and interrupted cleanup. Inventory raw/intermediate/retained copies, OS/runtime swap, hibernation, crash dumps, caches and backups; enforce §12.2 and §21.1.5 on every applicable path or disable the affected mode. Encryption alone cannot satisfy transient-media retention. A rooted/debug planning-parity device supplies no endpoint-security evidence by itself; hardware attestation remains optional under DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §26, and cleanup establishes no forensic-erasure guarantee.

Publish a policy-safe support matrix linking the exact profile, enabled workflows/classes, measured usable-memory/free-storage envelope, quality and timing results, sustained operating duration, security/retention outcomes and demonstrated fallback. Test offline/model loss, low battery, thermal/resource pressure, stop/silence, delayed results and manual completion without hidden cloud routing or loss of permitted saved state. A worker on another Device needs its own qualified endpoint/profile and existing route/Compartment checks; a same-Device worker remains an execution context unless separately admitted under §23.1. Ownership grants no disclosure authority. Only profiles passing all declared limits and mandatory security/retention gates may be operator-approved for those slices; material profile changes require affected rechecks. Label the selected configuration a tested sufficient profile among the tested candidates, with failures and limitations visible; infer no universal phone minimum or local-versus-cloud quality ranking. `UBU-Q0177` stays Open until a reviewed evidence-backed profile is selected; a protocol alone cannot close it. Lower-cost reach remains `UBU-Q0178`.

**Inference runtime reuse qualification (`UBU-D0336`).** Extend the endpoint gate to the deployed runtime's effective process permissions and source-dependent state across jobs. Inventory prompt/KV caches, embeddings, decoded media, buffers, leased handles and scratch separately from verified read-only model weights. A warm process or shared weights grant no access to prior job context. Enforce §21.1.5's lifecycle before reuse; separately authorized retained artifacts enter a new job only through fresh broker access and current bindings. If safe reuse cannot be demonstrated, qualify an isolated per-job runtime or leave the affected mode unavailable.

Run paired synthetic camera-and-voice jobs A/B under distinct grants, with B denied A's source and context. Put a unique synthetic detail in A and request it from B without supplying it. Exercise both job orders, ordinary completion, cancellation and source/policy revocation, crash/restart and resource pressure; include overlapping jobs only where concurrency is proposed. Inspect B's actual supplied context, runtime reads, result/artifact releases, cache/handle lookups and diagnostics for unauthorized A content or identifiers. Probe ambient filesystem, credential, camera/microphone and network access through the runtime's actual interfaces under deployment permissions; model refusal or a configured permission declaration alone cannot establish denial. Include an authorized control supplying the synthetic detail to B through the broker to verify the detector and permitted access path. Absence of a detail in generated prose alone cannot prove isolation.

Record pinned fixture/grant versions, expected and observed access/release outcomes, runtime permission observations, reset/termination behavior and policy-safe evidence links in the qualification report. Every enabled reuse/concurrency mode must demonstrate the allowed control and zero unauthorized reads/releases in these fixtures; unobserved mandatory paths remain unqualified. Repeat the cold/warm and sustained workload measurements with the enforced isolation, reset and cache restrictions active, accounting for their time, energy and aggregate memory/storage costs. Publish limitations beside the support matrix. These fixtures supply no passing runtime, hardware selection or forensic-erasure result.

**Economically accessible local paths (`UBU-D0332`).** Compare two candidate routes for a bounded camera-and-voice slice under `UBU-D0331`:

- **Existing single Device:** bounded still-image capture and push-to-talk audio, with smaller/quantized or task-specific local models capable of CPU inference, finite context and serialized inference jobs. Evaluate supported older hardware without assuming that age, nominal RAM or model size establishes eligibility.
- **Existing personal worker:** capture and review on one Device, with bounded inference on an existing operator-owned laptop/desktop worker. Qualify both endpoints and the complete authenticated encrypted transfer route. Current Device trust, Compartment/source permissions and original consent, freshness, retention and job limits govern every copy; ownership or physical proximity grants no authority. Worker loss leaves a qualified same-Device route or useful text/forms.

Each candidate uses legally locally runnable software/weights with disclosed licenses, installation downloads and fees, and requires no hosted inference, paid API credential or recurring model-service subscription. Dedicated accelerators and continuous streaming are optional for this evaluated slice; `UBU-Q0183` retains overall release scope and sequencing. Demonstrate same-Device operation with networking disabled and worker operation with Internet access blocked. Ordinary manual entry, exact correction, candidate review, admitted-state inspection and action recording remain available with models absent under §21.1.3.

Before dispatch, show the selected route, enabled modalities, measured latency class, finite wait/context/media limits and fallback. Reduce resolution, frame count or context only within a qualified profile, retaining transform provenance and explicit missing coverage. Slower processing cannot extend raw-media deadlines or freshness, silently omit necessary detail, or weaken uncertainty, corroboration, review, admission or Plan certification. Unsupported work returns an explicit limitation and an eligible retry or manual route. Switching models, endpoints or sources requires fresh bindings and applicable consent; cost/resource pressure grants no routing authority.

Extend `UBU-D0331`'s frozen useful-outcome rubric and real-device trials with a paired comparison of these routes on the same permitted §21.1.3 fixtures and declared workload. Include at least one CPU-only inference candidate and one existing-worker candidate; unavailable or ineligible candidates remain visible. Require a visible camera detail plus speech to produce a grounded reviewable candidate, exact correction/admission and a useful next action or explicit planning limitation. Report successful, corrected, unresolved, rejected and failed trials separately, plus total workflow time and operator setup, maintenance and correction effort. A text-only fallback cannot establish multimodal reach.

Declare the comparison baseline, usage period/workload and evaluated cash, effort, energy and storage budgets before trials. Report incremental outlay for already-owned hardware separately from acquisition/refurbishment and replacement assumptions. Include necessary software/model fees, download/connectivity, measured workload energy, storage, secure upkeep and accessories for the whole route, including a worker. Price observations state date, region and currency; running-cost estimates state usage period, workload, rates and uncertainty. Distinguish measured outcomes, quoted prices and estimates; unknown required costs prevent a claim that the budget is met. Use "lower-cost" only for a specified comparator and matched useful workload, with accuracy, latency and effort tradeoffs beside cash savings. User-supplied budget scenarios establish an evaluated envelope, without inferring individual affordability or population reach.

Publish costs, effort, qualified workflows/classes and failures alongside `UBU-D0331`'s support matrix. Select a path only after the operator reviews evidence that it passes the declared usefulness, resource, cost and mandatory security/retention limits. If no path qualifies, retain useful manual operation and the explicit multimodal gap. `UBU-Q0178` remains Open until an evidence-backed lower-cost path is selected; this protocol establishes no price, device minimum or supported configuration. `UBU-Q0177` and `UBU-Q0168` retain hardware and numeric-profile acceptance; CPU inference here selects no mobile planner or compute API under `UBU-Q0160`.

**Entry cash and useful-outcome accounting (`UBU-D0337`).** Extend `UBU-D0332`'s comparison with explicit ownership scenarios: capture/review Device only, or capture/review Device plus an existing worker. Include the Device-only scenario even if its worker route is unavailable. Evaluate acquisition/replacement separately; an already-owned worker's zero acquisition outlay cannot be used in a scenario that must buy one.

Before trials, declare finite entry-cash, cash-per-budget-period and total-horizon ceilings alongside the existing effort, energy and storage budgets. Entry cash includes every mandatory payment before the first useful multimodal outcome. Show subsequent payments in the period when due, including secure upkeep and necessary replacement under the declared assumptions. Amortization or unconfirmed resale proceeds cannot make a due payment fit. State the security-support and replacement assumptions over that horizon; continued use after an endpoint's declared maintained-security eligibility ends cannot be priced as a saving. Unknown required payments prevent a budget pass.

For each measured workload, report all-attempt totals for run-related cash, energy and active operator minutes, including failures and retries, and divide each total by the number of useful multimodal outcomes. That denominator includes only successful or successfully corrected camera-and-voice workflows meeting the frozen rubric; keep those outcome counts separate. Manual-only completion after multimodal failure remains fallback and cannot enter the denominator. With zero qualifying outcomes, report per-outcome values as undefined and the workload as unqualified. Period estimates retain the observed outcome mix and disclose uncertainty; fixed entry, upkeep and replacement costs remain in whole-route totals.

Add ownership prerequisites, entry/period payment schedules and ceilings, all-attempt totals, outcome denominators and per-outcome values to the cost/support matrix, with evidence links and pass/fail/unknown for each budget and mandatory gate. Select only for scenarios whose whole-route budgets and `UBU-D0331`/`UBU-D0336` qualification gates pass. Keep any no-worker multimodal gap visible. This extension selects no measured configuration or cost envelope; `UBU-Q0178` stays Open pending operator-reviewed results.

---
```

### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md — referenced sections

#### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §4 — Core definitions

```markdown
## 4. Core definitions

### Device **(net-new)**

A `Device` is a UbU execution enclave, not merely physical hardware. Examples: phone,
laptop, desktop, home server, browser session, removable sync-bundle importer, personal
GPU worker, temporary recovery environment.

### Zone **(net-new)**

A `Zone` is a user-controlled replication and work-context boundary — not a server.
Examples: personal Zone, household Zone, UbU project Zone, work/client Zone, experimental
Zone.

### Compartment

A `Compartment` is an existing UbU model noun (already referenced in the Phase 0 cutlist
as a deferred Phase 1+ model). In Phase 0 it surfaces only as `compartment_ids:
list[str]` and `redaction_level: str` on `PrivacyAndProvenance`. Phase 2 introduces a
**Compartment policy object** (replication/routing/retention/export) that references
compartments by the same `compartment_ids` convention. The policy object itself is
net-new; the compartment id vocabulary is not.

### SyncStatement **(net-new)**

A `SyncStatement` is a signed or integrity-protected state-change envelope exchangeable
through many transports.

### Replica **(net-new)**

A `Replica` is the local state view held by a Device. It may be full, partial, redacted,
stale, or restricted.

### Admitted state

Admitted state is the deterministic result of applying accepted sync statements, policy
updates, corrections, and conflict-resolution statements according to this contract's
causality and conflict rules. See §9 for the precise determinism claim.

---
```

## JSON Schema

Your output must satisfy this schema:

```json
{
  "type": "object",
  "additionalProperties": false,
  "required": [
    "proposal_id",
    "provider_id",
    "model_name",
    "question_id",
    "base_commit",
    "summary",
    "rationale",
    "changed_files",
    "patch",
    "commit_message",
    "validation_notes",
    "new_questions_added",
    "questions_resolved",
    "decisions_added",
    "requires_human_review"
  ],
  "properties": {
    "proposal_id": {
      "type": "string"
    },
    "provider_id": {
      "type": "string"
    },
    "model_name": {
      "type": "string"
    },
    "question_id": {
      "type": "string",
      "pattern": "^UBU-Q[0-9]{4}$"
    },
    "base_commit": {
      "type": "string"
    },
    "summary": {
      "type": "string"
    },
    "rationale": {
      "type": "string"
    },
    "changed_files": {
      "type": "array",
      "items": {
        "type": "string",
        "enum": [
          "DECISIONS.md",
          "DESIGN.md",
          "OPEN_QUESTIONS.md",
          "PLANNING_KERNEL_CONTRACT.md"
        ]
      }
    },
    "patch": {
      "type": "string"
    },
    "commit_message": {
      "type": "string"
    },
    "validation_notes": {
      "type": "array",
      "items": {
        "type": "string"
      }
    },
    "new_questions_added": {
      "type": "array",
      "items": {
        "type": "string",
        "pattern": "^UBU-Q[0-9]{4}$"
      }
    },
    "questions_resolved": {
      "type": "array",
      "items": {
        "type": "string",
        "pattern": "^UBU-Q[0-9]{4}$"
      }
    },
    "decisions_added": {
      "type": "array",
      "items": {
        "type": "string",
        "pattern": "^UBU-D[0-9]{4}$"
      }
    },
    "requires_human_review": {
      "type": "boolean"
    }
  }
}
```

## Requirements

- Return exactly one JSON object.
- Do not include hidden reasoning, `<think>` tags, markdown fences, or explanatory text outside the JSON object.
- All fields are required.
- Arrays may be empty.
- The patch must be a full git-diff-style patch.
- The patch may modify only:
  - `DESIGN.md`
  - `DECISIONS.md`
  - `OPEN_QUESTIONS.md`
  - `PLANNING_KERNEL_CONTRACT.md`
- Do not modify `README.md`, `OUTREACH.md`, hidden files, scripts, code files, or generated logs.
- Preserve the single-line metadata format in `OPEN_QUESTIONS.md`.
- Allowed values for `Status:` are `Archived`, `Decomposed`, `Deferred`, `Open`, `Solved`, `Superseded`.
- When resolving a question, set its metadata to `Status: Solved` and set `Resolved by:` to the resolving decision id(s).
- Never write `Status: Resolved`; `Resolved by:` is a separate metadata field.
- The `patch` string must be a raw unified diff as produced by `git diff`; do not wrap it in markdown fences or prose.
- Every file diff must start with `diff --git a/<path> b/<path>`, followed by `--- a/<path>` and `+++ b/<path>`.
- Every hunk must include accurate `@@ -old_start,old_count +new_start,new_count @@` ranges and enough unchanged context for `git apply --check` to apply without `--recount`.
- When editing `OPEN_QUESTIONS.md`, anchor hunks with the selected question heading `## UBU-Q0097: RelationshipScopeTransition step validation and counterparty autonomy` and its own `### Resolution` section. Do not use a repeated `### Resolution` heading from an earlier or later question as the edit location.
- If resolving the selected question, replace the `Unresolved.` text under that selected question's `### Resolution` section and update that same question's metadata line. Do not insert selected-question resolution text into any other question block.
- Use the existing `UBU-Qxxxx` and `UBU-Dxxxx` numbering conventions.
- Prefer minimal, auditable changesets.
- If the selected question is blocked, propose decomposition only if it produces replacement questions with fewer, simpler, or no dependencies.
- If the selected question is already partially resolved, narrow or clarify it rather than pretending it is fully unresolved.

Return only JSON.
