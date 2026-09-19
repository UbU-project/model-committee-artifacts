# Model-Committee Work Proposal Request

You are participating in the UbU `model-committee` process.

Your task is to produce one concrete work proposal as strict JSON.

Do not return prose outside the JSON object.

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

- next question id: `UBU-Q0151`
- next decision id: `UBU-D0273`

`DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md` is read-only context and is not in the patch
allowlist: do not propose changes to it.

### OPEN_QUESTIONS.md — related questions

#### UBU-Q0025

```markdown
## UBU-Q0025: Snapshot Application Semantics

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 implementation Resolved by: UBU-D0100 Last scored: Never Scored from commit: None

Resolved. See UBU-D0100.

---
```

#### UBU-Q0054

```markdown
## UBU-Q0054: Social identity theory impact on Identity, role, group membership, and mode switching

Status: Solved Priority: Post-MVP Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: UBU-Q0025 Blocks: Post-MVP multi-user Identity modeling, group-membership modeling, mode-switching policy Resolved by: UBU-D0265 Last scored: 2026-09-18 Scored from commit: None

### Question

How should social identity theory affect UbU's Identity model, role model, group-membership model, and mode-switching semantics?

### Subquestions

1. How should UbU distinguish Identity as external presentation, permission boundary, social role, self-concept, group membership, and mode-switching context?
2. How should group membership affect Objectives, Preferences, disclosure, trust, and coordination behavior?
3. How should UbU represent in-group and out-group effects without hard-coding stereotypes or paternalistic judgments?
4. How should social identity interact with Compartments, Relationships, and organization-mode planning?
5. Which parts are needed for Phase 3 multi-user coordination and which remain later research?

### Current direction

Identity is already central to UbU. Social identity theory may require richer group-membership, salience, and role-switching semantics, but its impact is post-MVP and must not block Phase 1 implementation.

### Resolution

Resolved by `UBU-D0265`: social identity is modeled as scoped, inspectable claims linking Identity, Role, GroupMembership, SelfConcept, Compartment, and ModeContext without collapsing one into another. Group membership affects planning only through explicit accepted records or policy, cannot synthesize Preferences or stereotypes from category membership, and uses Snapshot-style provenance, confidence, correction, and revocation. Phase 3 needs minimal group, membership, role-assignment, and mode-context records; learned salience and deeper social-psychology modeling remain later research.

---
```

#### UBU-Q0055

```markdown
## UBU-Q0055: Social choice theory and collective decision legitimacy

Status: Solved Priority: Post-MVP Phase: Phase 1b Decision type: Governance Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0025 Blocks: Post-MVP organizational governance, multi-user decision procedures, collective legitimacy Resolved by: UBU-D0266 Last scored: 2026-09-18 Scored from commit: None

### Question

How should social choice theory affect UbU's model of collective decisions, organizational legitimacy, dissent, and exit?

### Subquestions

1. What collective decision procedures should UbU be able to represent?
2. How should UbU distinguish authority, consent, voting, delegation, consensus, veto, and exit rights?
3. How should collective Preferences or directives be aggregated without pretending they are one person's Preferences?
4. How should UbU preserve dissent, minority reports, or unresolved disagreement?
5. What minimum governance semantics are required for Phase 3 multi-user / Identity coordination?
6. Which social-choice impossibility or legitimacy problems should UbU surface rather than hide?

### Current direction

Social choice theory is post-MVP. UbU should eventually model collective decision legitimacy explicitly for organizations and multi-user coordination, but Phase 1 should remain single-user dogfooding.

### Resolution

Resolved by `UBU-D0266`: UbU represents collective decisions as provenance-bearing governance artifacts rather than synthetic personal Preferences. Organization and multi-user coordination may model authority, consent, voting, delegation, consensus, veto, dissent, minority reports, and exit rights, but Phase 1 and Phase 1b remain single-user and admin-equivalent; social-choice limits are surfaced as legitimacy diagnostics instead of hidden by aggregation.

---
```

#### UBU-Q0056

```markdown
## UBU-Q0056: Game theory, strategic interaction, and counterparty modeling

Status: Solved Priority: Post-MVP Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0025 Blocks: Strategic interaction modeling, counterparty incentives, post-MVP coordination protocols Resolved by: UBU-D0267 Last scored: 2026-09-18 Scored from commit: None

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

Resolved by `UBU-D0267`: UbU treats game theory as a boundary discipline. Phase 1 and Phase 1b represent ordinary strategic interaction through existing objects such as External Events, Techniques, Tasks, Logs, External References, Relationships, Identity, capability grants, Compartments, and projection policy. Explicit strategic-interaction and counterparty model objects are deferred to Phase 3+ / future Skill Barter or marketplace work, where they must remain hypothesis-based, provenance-bearing, confidence-scoped, reviewable, and unable to authorize commitments, disclosures, or adversarial optimization without explicit authority.

---
```

#### UBU-Q0061

```markdown
## UBU-Q0061: Association object model and lifecycle

Status: Solved Priority: Post-MVP Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0054, UBU-Q0055, UBU-Q0056 Blocks: Phase 3 multi-user coordination, Association reconciliation, future skill-barter systems Resolved by: UBU-D0268 Last scored: 2026-09-18 Scored from commit: None

### Question

Should UbU introduce a first-class Association object, and if so what minimum fields describe an Identity-scoped perceived coordination structure without pretending that membership, authority, or boundaries are globally objective?

### Subquestions

1. What distinguishes an Association from an Organization Identity, Relationship, or External Reference?
2. Can an Association exist entirely inside one user's `user_mode` model?
3. What minimum fields describe perceived members, roles, shared Objectives, commitments, lifecycle, norms, confidence, and disclosure policy?
4. How should UbU represent informal groups such as friend groups, parties, amateur leagues, FOSS projects, conference cohorts, and skill networks?
5. How does an Association become formal enough to justify `organization_mode`?
6. How are invitations, exits, revocations, dormancy, and dissolution represented?
7. Which parts are required for Phase 3, and which remain later research?

### Current direction

An Association is an Identity-scoped, perspective-bound model of emergent coordination. Legal entities and institutional records are External References or evidence, not the whole Association.

### Resolution

Resolved by `UBU-D0268`: UbU introduces an `Association` as an Identity-scoped, perspective-bound coordination structure that may exist entirely inside one user's `user_mode`. It is distinct from organization-mode actors, bilateral Relationships, and External References; records perceived participants, roles, shared Objectives, commitments, norms, lifecycle, confidence, provenance, and disclosure policy; and can graduate to `organization_mode` only when there is explicit durable authority, governance, shared operational state, and accepted projection policy. Invitations, exits, revocations, dormancy, and dissolution are append-only lifecycle or attestation events.

---
```

#### UBU-Q0062

```markdown
## UBU-Q0062: AssociationAttestation provenance, trust, and dispute semantics

Status: Solved Priority: Post-MVP Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0061 Blocks: Association reconciliation, organizational introspection, pseudonymous reputation, multi-user trust Resolved by: UBU-D0269 Last scored: 2026-09-18 Scored from commit: None

### Question

How should UbU represent claims about Associations as attestations with provenance, confidence, disclosure policy, review status, and dispute or supersession semantics?

### Subquestions

1. What claim types are required: membership, non-membership, role, authority, commitment, objective, norm, governance rule, capability, reputation, relationship, priority, and dissolution?
2. What is the difference between a user-authored attestation, imported attestation, worker-generated candidate, and LLM-generated candidate?
3. What provenance is required for generated attestations: source locator, excerpt hash, timestamp range, model/prompt metadata, parser version, and Compartment?
4. How should an affected Identity or Association dispute, annotate, accept, reject, or supersede an attestation?
5. How should trust weights or confidence differ between public records, legal filings, signed descriptors, chat logs, meeting notes, and LLM interpretations?
6. How should private or permissioned attestations avoid leaking membership, role, or relationship facts?

### Current direction

LLM-generated AssociationAttestations are candidate claims. They must be reviewable, evidence-backed, confidence-scored, and never treated as authoritative social truth merely because they came from a large corpus.

### Resolution

Resolved by `UBU-D0269`: AssociationAttestations are immutable scoped claims with explicit claim type, provenance, evidence, confidence, Compartment/disclosure policy, review status, and append-only dispute, correction, revocation, or supersession links. User-authored and imported claims can become accepted through review, while worker and LLM outputs remain candidate-only until accepted. Trust is source- and claim-type-specific, confidence is not authority, and private attestations must not leak membership, role, relationship, non-membership, reputation, or dispute facts through projection or reconciliation surfaces.

---
```

### DECISIONS.md — cited decisions

#### UBU-D0100

```markdown
## UBU-D0100: Snapshots are immutable partial observed assertions

**Status:** Accepted → DESIGN.md §12

Snapshots are partial observed assertions over specific UniverseState fields. They are not full assertions of all UniverseState and do not imply that omitted fields are absent, unchanged, or unknown. Applying a Snapshot updates only the fields included in that Snapshot.

Snapshot records are immutable once accepted into the append-only Log. The original Snapshot observation is never edited or deleted as canonical history. Annotation, correction, and revocation are represented by later Log entries that point to the original Snapshot observation or Log entry.

MVP confidence is stored at both levels: a required snapshot-level confidence summarizes the observation as a whole, and optional per-field confidence overrides may be present when different observed dimensions have different reliability. If a field has no per-field confidence, it inherits the snapshot-level confidence. For affect Snapshots, the existing MVP rule remains: affect confidence may be treated globally across affect dimensions, with per-dimension confidence deferred unless a Snapshot explicitly provides per-field confidence.

Conflict resolution is field-local. Latest observed Snapshot data overrides simulated state for conflicting fields. User-declared Snapshots have top priority over sensor-derived, imported, inferred, or worker-submitted observations in MVP. If two user-declared Snapshots conflict on the same field, the latest `effective_at` or Snapshot timestamp wins unless a later correction supersedes it. For non-user observations, the application algorithm may use source priority, effective timestamp, and confidence, but confidence does not defeat an explicit user declaration in MVP.

A Snapshot can be corrected or revoked, but only through a new Log entry. Correction that replaces the observed state creates a new Snapshot and links it to the corrected Snapshot or Log entry. Revocation without replacement creates a correction/revocation Log entry that excludes the original Snapshot from corrected query views while preserving the historical claim.

---
```

#### UBU-D0265

```markdown
## UBU-D0265: Social identity is scoped context, not intrinsic judgment

**Status:** Accepted → DESIGN.md §1.1, §12. Resolves `UBU-Q0054`.

UbU adopts social identity theory as a modeling caution rather than as a stereotype engine. Identity, Role, GroupMembership, SelfConcept, Compartment, and ModeContext are distinct records. Identity carries presentation, accountability, and permission-bearing actor identity; Role captures situated expectations or authority; GroupMembership captures scoped membership in a group; SelfConcept captures user-declared first-person meaning; Compartment remains a privacy/routing boundary; ModeContext captures the currently salient planning or coordination lens. Cross-references are allowed, but no dimension implies another by default.

Group membership may affect Objectives, Preferences, disclosure, trust, and coordination only through accepted user statements, Relationship records, Compartment policy, capability grants, organizational rules, or reviewed observations. It may increase the salience of an existing Objective, select an appropriate presentation, add coordination obligations, or constrain sharing when tied to policy. It must not synthesize Preferences, loyalty, risk, trustworthiness, capability, or moral meaning from demographic or category membership.

In-group and out-group effects are represented as inspectable contextual hypotheses or user-declared concerns with source, scope, confidence, and correction path. Automated planning may use such claims only inside reviewable policy gates, and any action that would restrict options, disclose data, classify another person or group, or change coordination authority requires ordinary approval. Social-identity Snapshots and accepted records follow `UBU-D0100`: they are partial assertions, omissions carry no negative implication, and correction or revocation is append-only.

Phase 3 multi-user coordination needs minimal `Group`, `GroupMembership`, `RoleAssignment`, and `ModeContext` records with provenance, confidence, scope, optional Compartment refs, and review/correction support. Learned salience, norm conflict modeling, collective identity dynamics, and deeper social-psychology interpretation remain later research and must not block Phase 1 or Phase 1b implementation.

---
```

#### UBU-D0266

```markdown
## UBU-D0266: Collective decisions are governance artifacts, not synthetic personal Preferences

**Status:** Accepted → DESIGN.md §26.3. Resolves `UBU-Q0055`.

UbU adopts social choice theory as a legitimacy boundary for organization and multi-user coordination. A collective decision is represented as a governance artifact with a procedure, authority basis, scope, participants, evidence, and legitimacy diagnostics, not as a literal Preference held by a group mind or by every member.

Collective procedures should be able to distinguish authority, consent, voting, delegation, consensus, veto, abstention, dissent, appeal, and exit rights. A valid collective outcome records the rule used, who was eligible, what evidence was considered, what threshold or quorum applied, what authority source made the result binding, and what Compartment or disclosure policy governed participation.

Aggregated outputs become organization directives, policy statements, or coordination constraints with explicit `authority_source` and provenance. Individual Preferences, ballots, delegations, objections, and reasons remain separate source records. UbU may report that a procedure selected an option for a scope, but it must not collapse minority positions into consensus or pretend the outcome is one person's Preference.

Dissent, minority reports, unresolved objections, abstentions, veto use, appeals, and exit-right assertions remain durable, queryable records. Procedural validity may make a directive binding inside its scope, but it does not erase disagreement or prevent later correction, revocation, appeal, or exit analysis.

UbU should surface social-choice and legitimacy problems as diagnostics rather than hiding them behind aggregation. Examples include agenda dependence, cyclic majorities, strategic-voting incentives, delegation-chain fragility, quorum or participation defects, veto abuse, minority-right conflicts, and consent defects caused by high exit cost or unclear authority. Phase 1 and Phase 1b remain single-user and admin-equivalent; this decision preserves the semantics needed for later Phase 3 governance without blocking current implementation.

---
```

#### UBU-D0267

```markdown
## UBU-D0267: Strategic interaction is advisory until backed by authority and evidence

**Status:** Accepted → DESIGN.md §26.4. Resolves `UBU-Q0056`.

UbU adopts game theory as a boundary discipline for coordination, not as a hidden optimizer for manipulating counterparties. Strategic reasoning may identify incentives, commitment problems, signaling, bargaining structure, verification needs, free-riding risk, principal-agent concerns, and possible cooperation failures, but it remains advisory unless backed by accepted records and explicit authority.

Most Phase 1 and Phase 1b strategic patterns should be represented through existing objects: External Events for observed counterparty actions and outside-world triggers; Techniques for reusable coordination workflows; Tasks and Logs for user-controlled actions and outcomes; External References for issue trackers, grants, bounties, agreements, or messages; Relationships and organization-mode UniverseState for accepted state; and Identity, capability grants, Compartments, and projection policy for authority, disclosure, and routing limits.

Dedicated strategic-interaction or counterparty model objects are deferred to Phase 3+ and future Skill Barter/marketplace work. When introduced, they should be provenance-bearing hypothesis records with actor Identity refs, hypothesized incentives, strategic-situation type, commitment or verification mechanism, bargaining or principal-agent structure, expected consequence ranges, assumptions, evidence refs, confidence, Compartment refs, authority source, review status, and correction/revocation links.

Counterparty models must not be treated as facts. A speculative model may create a review item, explanation, risk warning, or option comparison, but it must not classify trustworthiness, restrict another person's options, disclose private data, expand capability grants, authorize external mutation, or create a commitment without ordinary review and approval. Recommendations must expose uncertainty and preserve plausible non-strategic explanations when evidence is weak.

Credible commitments, signaling, trust-but-verify workflows, and free-riding controls should be represented as explicit mechanisms such as milestones, staged disclosure, escrow or deposit requirements, acceptance criteria, verification Tasks, audit evidence, revocation paths, and graceful failure states. Payoff framing should remain user-facing consequences for Objectives and constraints, not an opaque utility maximizer.

For FOSS coordination, skilled contributors, grants, issue triage, bug bounties, and future Skill Barter systems, the useful near-term surface is pragmatic coordination: ownership clarity, stale-promise warnings, verification checklists, review commitments, grant or bounty terms, visible projection state, and mismatch diagnostics. Phase 1 and Phase 1b must not implement adversarial strategic optimization, automatic counterparty scoring, hidden persuasion timing, or automated bargaining authority.

---
```

#### UBU-D0268

```markdown
## UBU-D0268: Associations are perspective-bound coordination structures

**Status:** Accepted → DESIGN.md §1.2. Resolves `UBU-Q0061`.

UbU introduces an `Association` as a first-class, Identity-scoped record of perceived coordination among people, Identities, projects, or groups. An Association is perspective-bound: it records what a modeling Identity believes, has observed, or is allowed to rely on, not an objective global fact about who belongs, who has authority, or where the boundary of the group truly is. An Association may exist entirely inside one user's `user_mode` model.

An Association is distinct from an Organization Identity, a Relationship, and an External Reference. An Organization Identity is an accountable actor with organization-mode authority or shared operational state; a Relationship is a scoped relation between particular Identities; and an External Reference points to outside artifacts such as registries, repositories, contracts, calendars, chats, or web pages. Legal entities, institutional records, rosters, and public project pages are evidence or references for an Association, not the Association itself.

The minimum Association model records `association_id`, `perspective_identity_id`, optional `display_name`, `association_kind`, perceived participant or member claim refs, role claim refs, shared Objective refs, commitment refs, norm or rule refs, lifecycle state, lifecycle event refs, evidence and External Reference refs, confidence, Compartment refs, disclosure policy, provenance, review status, and correction or revocation links. Membership, authority, roles, commitments, and boundaries should be represented as scoped claims or linked attestations, not as absolute facts inferred from a roster or category label.

Informal groups such as friend groups, parties, amateur leagues, FOSS projects, conference cohorts, mutual-aid circles, and skill networks are ordinary Associations when UbU needs to reason about coordination beyond a single bilateral Relationship. They do not require legal identity, shared infrastructure, or organization-mode state. They may still carry norms, expected roles, shared Objectives, commitments, and privacy rules when those are explicitly declared, observed, reviewed, or imported as evidence.

Invitations, joins, exits, revocations, dormancy, reactivation, merges, splits, and dissolution are append-only lifecycle events or AssociationAttestation records. They update corrected query views without erasing historical claims. Disclosure and projection of Association state require Compartment and projection policy; a private perceived Association must not become visible to other participants merely because UbU models it.

An Association becomes formal enough to justify `organization_mode` only when it has explicit durable authority, governance or decision procedures, shared operational state, accountability for external actions, capability grants or external mutation rights, and accepted disclosure/projection boundaries. Until those conditions are accepted, UbU treats it as a user-mode perspective model and does not grant collective authority, synthesize collective Preferences, or treat disputed membership as settled.

For Phase 3, UbU needs the minimal Association record, lifecycle state, links to GroupMembership and RoleAssignment claims, Objective and commitment references, Compartment/disclosure policy, confidence, provenance, and correction/revocation paths. Cross-user reconciliation, dispute semantics, public organizational introspection, pseudonymous reputation, norm conflict analysis, collective-identity dynamics, and Skill Barter governance remain later research or separately resolved questions.

---
```

#### UBU-D0269

```markdown
## UBU-D0269: AssociationAttestations are evidence-backed scoped claims

**Status:** Accepted → DESIGN.md §1.2. Resolves `UBU-Q0062`.

UbU represents claims about an Association as `AssociationAttestation` records: immutable, scoped claims with claim type, polarity or asserted value, subject Identity or Association refs, scope, effective interval, evidence refs, provenance, confidence, Compartment refs, disclosure policy, review status, and correction, dispute, revocation, or supersession links. Required claim types are membership, non-membership, role, authority, commitment, Objective, norm, governance rule, capability, reputation, relationship, priority, lifecycle, and dissolution. Attestations are partial assertions; omitted claims do not imply absence.

Attestation source kind is explicit. `user_authored` attestations are first-person or operator-entered claims and can become accepted records through ordinary review. `imported` attestations preserve outside-source provenance and authority scope. `worker_generated_candidate` and `llm_generated_candidate` records are candidates only until reviewed; LLM output is interpretation metadata, not evidence by itself. Generated attestations record source locator, excerpt or payload hash, observed timestamp or interval, import/capture time, tool/parser/schema version, model and prompt/template identifiers or digests, run id, source Compartment, redaction level, and any evidence item refs needed to audit the extraction.

Confidence and trust are separate from authority. Confidence estimates how well the attestation is supported by its evidence; trust or source class explains why a source may be relied on for a particular claim. Public records and legal filings may be high-confidence evidence for their filed contents or legal status but not for informal social reality; signed descriptors depend on signer identity and authority; chat logs and meeting notes are contextual evidence; LLM interpretations begin as low-authority candidates even when drawn from large corpora. Confidence must not override explicit user declarations or accepted correction records.

Review status distinguishes `candidate`, `accepted`, `rejected`, `disputed`, `annotated`, `superseded`, `revoked`, and `expired`. Affected Identities or Associations may add response attestations or dispute records that accept, reject, annotate, narrow, or supersede a claim, but they do not erase the original historical assertion. Corrected query views follow the latest accepted supersession or revocation path within scope while retaining disputes and minority or counter-claims as queryable records.

Disclosure policy is part of the attestation, not an afterthought. Private or permissioned attestations must carry Compartment refs, projection policy, allowed audience, redaction behavior, and whether the existence of the claim may be revealed. Exported or cross-user views must not leak membership, role, relationship, non-membership, or dispute facts through notifications, IDs, counts, search results, or reconciliation hints unless the disclosure policy and actor capability permit that exposure.

AssociationAttestations may support Association reconciliation, organizational introspection, pseudonymous reputation, and multi-user trust only as evidence-bearing reviewed claims. They do not grant capability, settle authority, publish private Association state, or create organization-mode legitimacy unless a separate accepted authority, governance, or capability record permits that effect.

---
```

### DESIGN.md — referenced sections

#### DESIGN.md §1.1 — Social identity boundary

```markdown
### 1.1 Social identity boundary

Social identity theory affects UbU as a boundary discipline: an Identity may present externally, hold permissions, and own accountability, but it is not the same object as a social role, self-concept statement, group membership, Compartment, or temporary mode-switching context.

A `Role` records situational expectations or authority for an Identity. A `GroupMembership` records a scoped relation between an Identity and a group. A `SelfConcept` records user-declared first-person meaning. A `ModeContext` records the currently salient planning lens for recommendations or coordination. A `Compartment` remains the privacy, routing, retention, and export boundary. These dimensions may reference each other, but none of them implies another by default.

Group membership can affect Objectives, Preferences, disclosure, trust, and coordination only through explicit accepted records: user-declared statements, Relationship records, Compartment policy, capability grants, organizational rules, or review-accepted observations. Membership may raise the salience of an existing Objective, select an appropriate presentation, attach coordination obligations, or constrain sharing when tied to policy; it must not create inferred Preferences, loyalty judgments, trust scores, risks, capabilities, or stereotypes merely from category membership.

In-group and out-group effects are represented as inspectable contextual hypotheses or user-declared concerns, not as hard-coded judgments about people or groups. Any planner use must expose the source, scope, affected behavior, and correction path, and choices that would restrict options, disclose data, classify another person, or change coordination authority require the ordinary review and approval gates.

For Phase 3 multi-user coordination, UbU needs minimal `Group`, `GroupMembership`, `RoleAssignment`, and `ModeContext` records with provenance, confidence, scope, optional Compartment refs, and review/correction support. Learned salience, norm conflict modeling, collective identity dynamics, and social-psychology interpretation remain later research.
```

#### DESIGN.md §1.2 — Association object model

```markdown
### 1.2 Association object model

An `Association` is a first-class, Identity-scoped record of perceived coordination among people, Identities, projects, or groups. It is perspective-bound: it records what a modeling Identity believes, has observed, or is allowed to rely on, not an objective global fact about membership, authority, or group boundaries. An Association may exist entirely inside one user's `user_mode` model.

An Association is distinct from an Organization Identity, a Relationship, and an External Reference. An Organization Identity is an accountable actor with organization-mode authority or shared operational state; a Relationship is a scoped relation between particular Identities; and an External Reference points to outside artifacts such as registries, repositories, contracts, calendars, chats, or web pages. Legal entities, institutional records, rosters, and public project pages are evidence or references for an Association, not the Association itself.

A minimal Association record contains `association_id`, `perspective_identity_id`, optional `display_name`, `association_kind`, perceived participant or member claim refs, role claim refs, shared Objective refs, commitment refs, norm or rule refs, lifecycle state, lifecycle event refs, evidence and External Reference refs, confidence, Compartment refs, disclosure policy, provenance, review status, and correction or revocation links.

Membership, authority, roles, commitments, and boundaries are represented as scoped claims or linked attestations, not as absolute facts inferred from a roster or category label. Association records follow Snapshot-style partial assertion semantics: omitted participants, roles, commitments, or norms do not imply absence, and corrections or revocations are append-only.

Informal groups such as friend groups, parties, amateur leagues, FOSS projects, conference cohorts, mutual-aid circles, and skill networks are ordinary Associations when UbU needs to reason about coordination beyond a single bilateral Relationship. They may carry norms, expected roles, shared Objectives, commitments, and privacy rules when those are explicitly declared, observed, reviewed, or imported as evidence.

Lifecycle changes such as invitations, joins, exits, revocations, dormancy, reactivation, merges, splits, and dissolution are append-only lifecycle events or AssociationAttestation records. Disclosure and projection of Association state require Compartment and projection policy; a private perceived Association must not become visible to other participants merely because UbU models it.

An `AssociationAttestation` is an immutable, scoped claim about an Association, an Identity's relation to it, or a coordination artifact linked to it. Required claim types include `membership`, `non_membership`, `role`, `authority`, `commitment`, `objective`, `norm`, `governance_rule`, `capability`, `reputation`, `relationship`, `priority`, `lifecycle`, and `dissolution`; each records its subject refs, asserted value or polarity, scope, effective interval, evidence refs, confidence, Compartment refs, disclosure policy, provenance, review status, and dispute, correction, revocation, or supersession links.

Attestation source kind is explicit. `user_authored` attestations are first-person or operator-entered claims that may become accepted through ordinary review; `imported` attestations preserve outside-source provenance and authority scope; `worker_generated_candidate` and `llm_generated_candidate` records remain candidate claims until reviewed. Generated candidates must record source locator, excerpt or payload hash, observed timestamp or interval, capture/import time, tool/parser/schema version, model and prompt/template identifiers or digests, run id, source Compartment, redaction level, and evidence item refs sufficient to audit the extraction.

Confidence and trust are separate from authority. Confidence estimates support from evidence; source class explains why a source may be relied on for a claim type. Public records and legal filings can be strong evidence for filed contents or legal status without settling informal social reality; signed descriptors depend on signer identity and authority; chat logs and meeting notes are contextual evidence; LLM interpretations begin as low-authority candidates even when drawn from large corpora. Confidence never overrides explicit user declarations, accepted correction records, Compartment policy, or capability boundaries.

Review status distinguishes `candidate`, `accepted`, `rejected`, `disputed`, `annotated`, `superseded`, `revoked`, and `expired`. Affected Identities or Associations may add response attestations or dispute records that accept, reject, annotate, narrow, or supersede a claim, but they do not erase the original historical assertion. Corrected query views follow the latest accepted supersession or revocation path within scope while retaining disputes, annotations, minority reports, and counter-claims as queryable records.

Disclosure policy is part of the attestation. Private or permissioned attestations must carry Compartment refs, projection policy, allowed audience, redaction behavior, and whether the claim's existence may be revealed. Exported or cross-user views must not leak membership, role, relationship, non-membership, reputation, or dispute facts through notifications, IDs, counts, search results, reconciliation hints, or provenance fields unless the disclosure policy and actor capability permit that exposure.

An Association becomes formal enough to justify `organization_mode` only when it has explicit durable authority, governance or decision procedures, shared operational state, accountability for external actions, capability grants or external mutation rights, and accepted disclosure/projection boundaries. Until then, UbU treats it as a user-mode perspective model and does not grant collective authority, synthesize collective Preferences, or treat disputed membership as settled.

For Phase 3, UbU needs the minimal Association record, lifecycle state, links to GroupMembership and RoleAssignment claims, Objective and commitment references, Compartment/disclosure policy, confidence, provenance, and correction/revocation paths. Cross-user reconciliation, dispute semantics, public organizational introspection, pseudonymous reputation, norm conflict analysis, collective-identity dynamics, and Skill Barter governance remain later research or separately resolved questions.
```

#### DESIGN.md §12 — Snapshots

```markdown
## 12. Snapshots

A **Snapshot** is an observed state update.

User-declared and sensor-derived observations use the same object type.

Snapshots are partial observed assertions over specific UniverseState fields. They are not full-state replacements and do not assert anything about omitted fields.

MVP snapshot fields:

- `snapshot_id`
- `timestamp` or valid-at instant
- `source`
- per-dimension or per-field values
- snapshot-level confidence
- optional per-field confidence overrides
- correction or revocation status derived from Log correction links

### 12.1 Snapshot precedence rule

- Latest observed snapshot overrides simulation on conflicting fields.
- User-declared snapshots are top-priority observations.
- Confidence is stored, but does not override explicit user declaration in MVP.
- Between conflicting user-declared Snapshots for the same field, latest effective timestamp wins unless a later Log correction says otherwise.
- Between conflicting non-user observations, source priority, effective timestamp, and confidence may be used by the Snapshot application algorithm.

Snapshot records are immutable once accepted into the append-only Log. A Snapshot may be corrected or revoked only by a later Log correction entry that points to the original Snapshot observation. A correction that asserts replacement state creates a new Snapshot and links it to the corrected Snapshot or Log entry; a revocation without replacement simply removes the original Snapshot from corrected query views while preserving the historical claim.

Social identity observations, such as group membership, role activation, self-concept statements, or mode-context salience, follow the same partial-assertion rule. A Snapshot or accepted record may assert one scoped social-identity fact without implying that omitted memberships, roles, or contexts are absent, inactive, or unknown.

Corrections and revocations remain append-only. A later user correction can withdraw, narrow, or replace a social-identity claim without editing the original historical assertion.

### 12.2 Discovery mode

**Discovery mode** is a user-selectable workflow state, not a fourth instance operating mode. It is off by default and may be started, paused, resumed, exited, or inspected by the user at any time. The UI must show active capture state, enabled sources, local/cloud routing status, retention limits, and pending-review evidence count.

Phase 1 session states are `inactive`, `active`, `paused`, `ended`, and `pending_review`. A minimal `DiscoveryEvidenceItem` is a reviewable candidate artifact with session ref, effective time or interval, source kind, signal kind, payload ref or redacted summary, confidence, Compartment or low-security label, provenance, and review status. Raw evidence is not final truth.

Allowed Phase 1 discovery inputs are intentionally narrow:

- explicit quick notes, user-selected current action, voice or text notes intentionally captured by the user, and manual start/stop markers;
- UbU app state, Task controls, Calendar or default Plan context, timers, focus state, and foreground app category or app identifier when explicitly enabled;
- configured integration events already allowed by Compartment, External Event, and External Reference policy;
- coarse motion category such as stationary, walking, transit, or driving;
- coarse user-defined location category or geofence event, not continuous raw location by default;
- device state needed for interpretation, such as screen on/off or network/offline state.

Phase 1 discovery excludes covert or broad capture by default: continuous microphone, camera, screen recording, keystroke logging, raw message bodies, raw file contents, raw GPS trails, and cross-Identity sharing are outside the default discovery input set. Any later use of those sources requires a separate explicit mode, Compartment review, routing disclosure, and user approval.

Admission rules:

- A user-confirmed actual action that reconciles a planned interval is recorded with `plan_realized`; if it completes, fails, or moots a Task, the corresponding Task lifecycle Log is also written.
- A user override of the current recommendation records `decision_recorded` with `decision_kind = system_recommendation_overridden`, `authority_source = user_override`, chosen action or Task when known, optional reason, and a `user_override` recalculation trigger when planner-relevant.
- A user-declared current state becomes a Snapshot only when it asserts observed state such as affect, availability, location category, or capacity.
- Preferences change only through explicit accepted pairwise Preference statements. Repeated behavior and inferred reasons are not Preferences.
- Task estimates, dependencies, status, Objective annotations, or Objective status change only after user acceptance or normal validated admission. Otherwise the item remains an unresolved review item.

Undetailed periods should be represented with uncertainty rather than filled in by inference. The review UI should allow `unknown`, `private`, `rest`, `interruption`, `planned Task`, `different Task`, `quick note`, `other`, and free-text correction. UbU must not infer Preference changes, Objective failure, habit patterns, or moral meaning from unknown or private time.

Before treating repeated behavior as a habit pattern, UbU must ask a clarification prompt such as: `I have seen this pattern more than once: [behavior] during [context]. Should UbU plan around it, help you change it, treat it as not a pattern, or leave it unresolved?` Valid answers are `endorse_and_plan_around`, `tolerate_but_review`, `unwanted_help_change`, `not_a_pattern`, and `leave_unresolved`.

Discovery mode preserves sovereignty by remaining visible, opt-in, source-scoped, pausable, inspectable, and correctable. Users may reject, delete where retention policy permits, mark private/unknown, correct, defer, or accept evidence before it becomes canonical state. Compartment policy, `no_cloud_llm`, `no_external_export`, allowed-device, and allowed-integration denials remain hard boundaries.

---
```

#### DESIGN.md §26.3 — Collective decision legitimacy

```markdown
### 26.3 Collective decision legitimacy

Social choice theory is a boundary discipline for organization and multi-user coordination. UbU must not treat a committee, group, or Association as if it had one unqualified personal Preference. A collective outcome is a decision artifact with a procedure, authority basis, scope, participants, evidence, and legitimacy notes.

Future collective decision records should distinguish `authority`, `consent`, `vote`, `delegation`, `consensus`, `veto`, `abstention`, `dissent`, and `exit` rather than flattening them into one approval flag. Minimum fields include decision id, Association/Group or organization scope, actor Identity refs, role or eligibility rules, procedure kind, proposal refs, ballot or consent evidence refs, quorum/threshold rule, outcome, effective interval, authority_source, Compartment refs, provenance, review status, and correction/revocation links.

Aggregated outputs become organization directives, policy statements, or coordination constraints with their own authority_source. Individual Preferences, ballots, delegations, objections, and reasons remain separate source records. UbU may summarize an outcome as binding for a scope, but it must not synthesize a shared Preference or erase minority positions merely because a procedure selected an option.

Dissent is durable state. Minority reports, unresolved objections, abstentions, veto use, appeal paths, and recorded exit rights should remain queryable and may affect risk reporting, legitimacy explanations, and future planning even when the collective decision is procedurally valid.

Social-choice limitations should be surfaced as diagnostics rather than hidden. Examples include agenda dependence, cyclic majorities, strategic-voting incentives, delegation-chain fragility, quorum or participation defects, veto abuse, minority-right conflicts, and consent defects caused by high exit cost or unclear authority. These diagnostics should create review items or warnings before UbU treats a collective directive as legitimate for sensitive coordination.

Phase 1 and Phase 1b do not implement general multi-user governance. They preserve room for Phase 3 by keeping organization-mode authority explicit, logging actor Identity and provenance, retaining dissent/correction paths, and avoiding any schema claim that an organization's Preference is literally a person's Preference.
```

#### DESIGN.md §26.4 — Strategic interaction boundary

```markdown
### 26.4 Strategic interaction boundary

Game theory is a boundary discipline for coordination, not a hidden optimizer for manipulating counterparties. UbU may help the user notice incentives, commitments, verification needs, asymmetric information, and likely coordination failures, but strategic analysis remains advisory unless backed by accepted records and explicit authority.

Phase 1 and Phase 1b represent most strategic interaction with existing objects: External Events record observed counterparty actions or outside-world triggers; Techniques encode reusable coordination workflows; Tasks and Logs record the user's controlled actions and outcomes; External References point to issue trackers, grant calls, bounties, agreements, or messages; Relationships and organization-mode UniverseState capture accepted state; Identity, capability grants, Compartments, and projection policy constrain who may act, what may be disclosed, and where evidence may be sent.

Explicit strategic-interaction records are deferred to Phase 3+ or future Skill Barter/marketplace work. When introduced, they should model the strategic situation, actor Identity refs, hypothesized incentives, commitment or verification mechanisms, bargaining or principal-agent structure, expected consequence ranges, assumptions, evidence refs, confidence, Compartment refs, authority source, review status, and correction/revocation links.

Counterparty models are hypotheses, not facts. A speculative model may create a review item, explanation, or option comparison, but it must not become canonical state, restrict a person's options, classify trustworthiness, disclose private data, expand a capability grant, or create a commitment without review. Recommendations should expose uncertainty and offer non-strategic interpretations when evidence is weak.

Credible commitments, signaling, free-riding controls, bargaining, and trust-but-verify workflows should be represented as user-visible mechanisms: milestones, escrow or deposit requirements, acceptance criteria, verification Tasks, audit evidence, staged disclosure, revocation paths, and graceful failure states. Payoff language should be framed as consequences for Objectives and constraints rather than as an opaque utility maximizer.

For FOSS dogfooding, the useful Phase 1b surface is pragmatic coordination: issue triage, contributor follow-up, review commitments, release promises, grant deadlines, bug bounty terms, verification checklists, and visible projection state. UbU may warn about bottlenecks, ambiguous ownership, unverified claims, stale promises, and incentive mismatch, but it should not optimize adversarial pressure or persuasion.

Phase 1 and Phase 1b must not implement adversarial strategic optimization, automatic counterparty scoring, hidden persuasion timing, or automated bargaining authority. Later multi-user, marketplace, bounty, grant, or Skill Barter features must pass through the same Identity, capability, Compartment, provenance, projection, and human-review boundaries as other external coordination.

---
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

The first MVP is designed around **dogfooding**: using UbU to coordinate the design, development, release, and maintenance of UbU itself.

Recent LLM and agentic-AI changes reinforce UbU's core boundary: realtime, multimodal, tool-using, memory-bearing models are valuable interaction and extraction backends, but UbU remains the user-sovereign state-transition, planning, logging, privacy, and review layer.

For Phase 1, UbU must also be understandable as a first-person user experience. The minimal user-facing loop is: answer a small number of bootstrapping questions, allow UbU to construct an initial context model, receive one recommended next Task, inspect why that Task matters now, act or override, then let UbU learn from the result through Logs, Snapshots, and recalculation.

### 1.1 Social identity boundary

Social identity theory affects UbU as a boundary discipline: an Identity may present externally, hold permissions, and own accountability, but it is not the same object as a social role, self-concept statement, group membership, Compartment, or temporary mode-switching context.

A `Role` records situational expectations or authority for an Identity. A `GroupMembership` records a scoped relation between an Identity and a group. A `SelfConcept` records user-declared first-person meaning. A `ModeContext` records the currently salient planning lens for recommendations or coordination. A `Compartment` remains the privacy, routing, retention, and export boundary. These dimensions may reference each other, but none of them implies another by default.

Group membership can affect Objectives, Preferences, disclosure, trust, and coordination only through explicit accepted records: user-declared statements, Relationship records, Compartment policy, capability grants, organizational rules, or review-accepted observations. Membership may raise the salience of an existing Objective, select an appropriate presentation, attach coordination obligations, or constrain sharing when tied to policy; it must not create inferred Preferences, loyalty judgments, trust scores, risks, capabilities, or stereotypes merely from category membership.

In-group and out-group effects are represented as inspectable contextual hypotheses or user-declared concerns, not as hard-coded judgments about people or groups. Any planner use must expose the source, scope, affected behavior, and correction path, and choices that would restrict options, disclose data, classify another person, or change coordination authority require the ordinary review and approval gates.

For Phase 3 multi-user coordination, UbU needs minimal `Group`, `GroupMembership`, `RoleAssignment`, and `ModeContext` records with provenance, confidence, scope, optional Compartment refs, and review/correction support. Learned salience, norm conflict modeling, collective identity dynamics, and social-psychology interpretation remain later research.

### 1.2 Association object model

An `Association` is a first-class, Identity-scoped record of perceived coordination among people, Identities, projects, or groups. It is perspective-bound: it records what a modeling Identity believes, has observed, or is allowed to rely on, not an objective global fact about membership, authority, or group boundaries. An Association may exist entirely inside one user's `user_mode` model.

An Association is distinct from an Organization Identity, a Relationship, and an External Reference. An Organization Identity is an accountable actor with organization-mode authority or shared operational state; a Relationship is a scoped relation between particular Identities; and an External Reference points to outside artifacts such as registries, repositories, contracts, calendars, chats, or web pages. Legal entities, institutional records, rosters, and public project pages are evidence or references for an Association, not the Association itself.

A minimal Association record contains `association_id`, `perspective_identity_id`, optional `display_name`, `association_kind`, perceived participant or member claim refs, role claim refs, shared Objective refs, commitment refs, norm or rule refs, lifecycle state, lifecycle event refs, evidence and External Reference refs, confidence, Compartment refs, disclosure policy, provenance, review status, and correction or revocation links.

Membership, authority, roles, commitments, and boundaries are represented as scoped claims or linked attestations, not as absolute facts inferred from a roster or category label. Association records follow Snapshot-style partial assertion semantics: omitted participants, roles, commitments, or norms do not imply absence, and corrections or revocations are append-only.

Informal groups such as friend groups, parties, amateur leagues, FOSS projects, conference cohorts, mutual-aid circles, and skill networks are ordinary Associations when UbU needs to reason about coordination beyond a single bilateral Relationship. They may carry norms, expected roles, shared Objectives, commitments, and privacy rules when those are explicitly declared, observed, reviewed, or imported as evidence.

Lifecycle changes such as invitations, joins, exits, revocations, dormancy, reactivation, merges, splits, and dissolution are append-only lifecycle events or AssociationAttestation records. Disclosure and projection of Association state require Compartment and projection policy; a private perceived Association must not become visible to other participants merely because UbU models it.

An `AssociationAttestation` is an immutable, scoped claim about an Association, an Identity's relation to it, or a coordination artifact linked to it. Required claim types include `membership`, `non_membership`, `role`, `authority`, `commitment`, `objective`, `norm`, `governance_rule`, `capability`, `reputation`, `relationship`, `priority`, `lifecycle`, and `dissolution`; each records its subject refs, asserted value or polarity, scope, effective interval, evidence refs, confidence, Compartment refs, disclosure policy, provenance, review status, and dispute, correction, revocation, or supersession links.

Attestation source kind is explicit. `user_authored` attestations are first-person or operator-entered claims that may become accepted through ordinary review; `imported` attestations preserve outside-source provenance and authority scope; `worker_generated_candidate` and `llm_generated_candidate` records remain candidate claims until reviewed. Generated candidates must record source locator, excerpt or payload hash, observed timestamp or interval, capture/import time, tool/parser/schema version, model and prompt/template identifiers or digests, run id, source Compartment, redaction level, and evidence item refs sufficient to audit the extraction.

Confidence and trust are separate from authority. Confidence estimates support from evidence; source class explains why a source may be relied on for a claim type. Public records and legal filings can be strong evidence for filed contents or legal status without settling informal social reality; signed descriptors depend on signer identity and authority; chat logs and meeting notes are contextual evidence; LLM interpretations begin as low-authority candidates even when drawn from large corpora. Confidence never overrides explicit user declarations, accepted correction records, Compartment policy, or capability boundaries.

Review status distinguishes `candidate`, `accepted`, `rejected`, `disputed`, `annotated`, `superseded`, `revoked`, and `expired`. Affected Identities or Associations may add response attestations or dispute records that accept, reject, annotate, narrow, or supersede a claim, but they do not erase the original historical assertion. Corrected query views follow the latest accepted supersession or revocation path within scope while retaining disputes, annotations, minority reports, and counter-claims as queryable records.

Disclosure policy is part of the attestation. Private or permissioned attestations must carry Compartment refs, projection policy, allowed audience, redaction behavior, and whether the claim's existence may be revealed. Exported or cross-user views must not leak membership, role, relationship, non-membership, reputation, or dispute facts through notifications, IDs, counts, search results, reconciliation hints, or provenance fields unless the disclosure policy and actor capability permit that exposure.

An Association becomes formal enough to justify `organization_mode` only when it has explicit durable authority, governance or decision procedures, shared operational state, accountability for external actions, capability grants or external mutation rights, and accepted disclosure/projection boundaries. Until then, UbU treats it as a user-mode perspective model and does not grant collective authority, synthesize collective Preferences, or treat disputed membership as settled.

For Phase 3, UbU needs the minimal Association record, lifecycle state, links to GroupMembership and RoleAssignment claims, Objective and commitment references, Compartment/disclosure policy, confidence, provenance, and correction/revocation paths. Cross-user reconciliation, dispute semantics, public organizational introspection, pseudonymous reputation, norm conflict analysis, collective-identity dynamics, and Skill Barter governance remain later research or separately resolved questions.

### 1.3 Message Context Envelope

A `MessageContextEnvelope` is a Phase 3 cross-user communication projection that carries enough context for triage without reconstructing the sender's private state. It is not a SyncStatement, Relationship record, Association record, Objective disclosure, or proof of authority; it is a message plus bounded metadata that the receiver may inspect and map into local suggestions.

The minimum envelope record contains `envelope_id`, `schema_version`, `sent_at` or observed timestamp, `sender_identity_ref`, `receiver_identity_ref`, `source_system`, optional `source_message_ref`, `body` or `body_ref`, `message_kind`, `topic`, `priority`, `interrupt_recommendation`, `response_expectation`, optional `deadline`, `receiver_treatment_hint`, `assumptions`, `ambiguities`, `provenance`, `confidence`, `compartment_ids`, and `disclosure_policy`. `message_kind` distinguishes at least `request`, `question`, `status_update`, `blocker`, `commitment`, `decision`, and `fyi`. `receiver_treatment_hint` states whether the receiver may consider the message as a Task candidate, status update, blocker notice, commitment notice, or FYI.

The envelope may reference local-only objects only through envelope-scoped opaque handles, redacted labels, or user-approved summaries. Sender-local Objective, Task, Relationship, Association, Compartment, or attestation ids are not exported as stable identifiers unless the disclosure policy explicitly permits that projection to the receiver. Hidden context must not leak through ids, counts, provenance paths, topic labels, search hints, notification text, or reconciliation metadata.

Sender intent is advisory unless backed by separate authority. A sender may ask the receiver to treat a message as a Task, blocker, commitment, status update, or FYI, but the receiver's UbU records only a candidate review item until the receiver accepts it or a receiver-side policy grants authority. The envelope must not create accepted Tasks, Calendar changes, commitments, Relationship facts, AssociationAttestations, or Objective state by itself.

A receiver's UbU may translate envelope metadata into local suggestions: high interrupt recommendations can create Calendar interruption or focus-review suggestions; response expectations and deadlines can create communication-review reminders; Task, blocker, or commitment hints can create Task-creation or dependency-review candidates. These mappings preserve envelope provenance and confidence and remain correctable, rejectable, and revocable according to local policy.

Provenance is per-field when metadata is inferred. Inferred priority, topic, interrupt recommendation, response expectation, deadline, assumptions, ambiguities, or treatment hints must record source evidence, inference mechanism or model/template identifier, timestamp, confidence, redaction level, and review status. Confidence is advisory and never overrides explicit user declarations, accepted corrections, Compartment policy, or receiver-side authority checks.

Minimum controls are export preview, per-field redaction, Compartment and disclosure-policy validation, reveal-existence control, local-only reference stripping, provenance inspection, correction/revocation after send where transport permits, and receiver-side review before local mutation. Defaults minimize disclosure: raw private context, Relationship state, Association state, Objective details, Task details, and Compartment names stay local unless the sender explicitly approves their projection.

### 1.4 Message Context Extractor

A `MessageContextExtractor` is the local or policy-approved model pipeline that turns unstructured direct-message or group-chat text into strict candidate UbU JSON. It is an extraction and triage layer, not an authority layer: its outputs are reviewable candidates until accepted by user action or by an explicit local policy grant.

The extractor input is a `MessageExtractionBundle` containing raw body or `body_ref`, source system, channel type, channel purpose, source message and thread refs, observed timestamp, sender and receiver refs or mapping candidates, available Identity mapping evidence, Association mapping candidates, channel participant metadata, allowed thread context, allowed Relationship or history summaries, relevant Compartment and disclosure policy, locale or timezone, schema version, prompt or template version, and extraction run metadata. Raw message bodies, Relationship state, Association state, and private Objective or Task context are included only when the relevant Compartment policy permits that use.

The extractor output is a `MessageExtractionResult` containing schema version, run id, candidate `MessageContextEnvelope`, message classification, topic, candidate Task suggestions, candidate Objective-link suggestions, candidate AssociationAttestation suggestions where allowed, priority, interrupt recommendation, actionability, response expectation, optional deadline, assumptions, ambiguities, per-field confidence, per-field provenance, and review status or auto-accept eligibility. Suggested structures remain candidate records and do not mutate Tasks, Objectives, Relationships, Associations, Calendar entries, commitments, or admitted state by themselves.

Extractor schemas are strict JSON schemas with closed enums, required fields, explicit nullable or unknown values, and no unvalidated extra fields. A deterministic validator runs before any downstream use. A bounded repair loop may fix malformed JSON, normalize invalid enum values, or replace missing required inferences with unknown and low confidence, but it must not invent evidence, raise confidence without new support, or route around Compartment policy. Overconfident or unsupported inferences are downgraded and marked for review.

Per-field provenance distinguishes `explicit_message_fact`, `source_metadata`, `channel_policy_metadata`, `thread_context_inference`, `relationship_context_inference`, `association_context_inference`, `model_inference`, and `user_confirmed_correction`. Each inferred field records evidence refs or excerpt hashes, source Compartment, redaction level, model or template identifier, parser or schema version, timestamp, confidence, and review status. User-confirmed corrections supersede extractor claims through append-only correction paths rather than editing the original extraction result.

Automatic acceptance is limited to low-risk, directly evidenced, non-mutating parse facts such as source refs, timestamps, source message ids, and explicit sender-provided metadata that passes policy validation. Candidate Tasks, Objective links, AssociationAttestations, Relationship updates, commitments, priority escalation, high interrupt recommendations, deadlines, and cross-Compartment or cross-user projection require user review or a specific local policy grant.

UbU should start with schema-constrained general LLMs or local models plus validation, repair, confidence calibration, and provenance inspection. Fine-tuned, distilled, or adapter-trained extractor models become appropriate only after schemas stabilize and correction logs show enough repeated examples to justify the privacy, latency, cost, or reliability tradeoff. Custom models must preserve the same schemas, validators, provenance, review gates, and authority limits as general models.

Custom extractor training requires retained-with-consent training bundles, gold `MessageExtractionResult` labels, user corrections, rejected candidates, field-level provenance labels, ambiguity examples, no-action examples, hard negatives, redaction and Compartment labels, and evaluation sets split by source system, channel type, relationship context, and privacy class.

### 1.5 Realtime interaction sessions and candidate updates

Realtime models are optional interaction backends for conversation, capture, triage, and short-horizon repair. UbU represents a realtime run as a lightweight `RealtimeInteractionSession`, not as a Task, Calendar event, canonical Log entry, or raw sensor stream. A session records the consented interaction context and source boundary; the model's outputs are `RealtimeCandidateUpdate` records until admitted through the ordinary planner, Log, Snapshot, Compartment, and review rules.

The minimum `RealtimeInteractionSession` record contains `interaction_session_id`, `schema_version`, `instance_id`, `actor_identity_ref`, `device_ref`, `started_at`, optional `ended_at`, `session_state`, `source_modes`, `backend_ref`, `model_or_tool_ref`, prompt or template version, enabled input sources, `compartment_ids`, `disclosure_policy`, retention or redaction policy, routing mode, evidence item refs, candidate update refs, provenance, review status, and correction or revocation links. Session states align with discovery mode where applicable: `inactive`, `active`, `paused`, `ended`, and `pending_review`.

The minimum `RealtimeCandidateUpdate` record contains `candidate_update_id`, `schema_version`, optional `interaction_session_id`, `emitted_at`, effective instant or interval, `candidate_type`, optional `target_ref`, payload, source evidence refs or excerpt hashes, per-field provenance, confidence, `compartment_ids`, redaction level, routing mode, review status, admission hint, idempotency key, and correction, rejection, revocation, or supersession links. The Phase 1b candidate types required for Quick UbU are `tag`, `dependency`, `preference`, `decomposition`, and `clarification_question`. Additional allowed candidate types are `interruption`, `task_progress`, `affect_signal`, `external_condition_change`, `plan_deviation`, `log_candidate`, `task_candidate`, and `association_attestation_candidate`, but each remains subject to its existing schema, authority, review, and Compartment gates.

A realtime candidate never mutates admitted state by itself. A tag, dependency, decomposition, preference, Task, Log, Snapshot, Calendar, Plan-deviation, or AssociationAttestation effect becomes canonical only after user acceptance, validated admission, or a specific local policy grant for that exact mutation class. Rejected, corrected, superseded, or revoked candidates remain auditable according to the candidate lifecycle rules.

Model-noticed elapsed time is represented as evidence or a candidate interval, not as planner-valid Task execution, Calendar occupancy, or Log truth. Planned time is reconciled only through accepted `plan_realized`, Task lifecycle, Calendar, Snapshot, or Log records. Unknown, private, rest, interruption, different-Task, and quick-note intervals preserve uncertainty and must not create Preference changes, Objective failures, habit claims, or moral meaning by inference.

Mandatory provenance for realtime observations includes source kind, capture or observation interval, device and backend refs, model or tool identifier, prompt or template digest where applicable, parser or schema version, evidence refs or payload hashes, source Compartment, redaction level, processing route, confidence, and review status. Audio, video, screen, keystroke, raw message, raw file, raw GPS, and similarly sensitive sources also require explicit source enablement and policy-visible capture state before they can produce candidates.

Routing defaults are conservative. Local-only processing is required for raw sensitive capture, `no_cloud_llm` Compartments, and sources whose policy denies external processing. Cloud processing is optional only for policy-approved, user-visible, redacted or intentionally provided inputs. Cross-user projection, external export, covert capture, broad background capture, hidden persuasion timing, and automatic external mutation are prohibited unless a later explicit mode, Compartment policy, capability grant, and user approval allow that exact behavior.

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
- The `patch` string must be a raw unified diff as produced by `git diff`; do not wrap it in markdown fences or prose.
- Every file diff must start with `diff --git a/<path> b/<path>`, followed by `--- a/<path>` and `+++ b/<path>`.
- Every hunk must include accurate `@@ -old_start,old_count +new_start,new_count @@` ranges and enough unchanged context for `git apply --check` to apply without `--recount`.
- When editing `OPEN_QUESTIONS.md`, anchor hunks with the selected question heading `## UBU-Q0088: Extrospection finding lifecycle and durable rejection semantics` and its own `### Resolution` section. Do not use a repeated `### Resolution` heading from an earlier or later question as the edit location.
- If resolving the selected question, replace the `Unresolved.` text under that selected question's `### Resolution` section and update that same question's metadata line. Do not insert selected-question resolution text into any other question block.
- Use the existing `UBU-Qxxxx` and `UBU-Dxxxx` numbering conventions.
- Prefer minimal, auditable changesets.
- If the selected question is blocked, propose decomposition only if it produces replacement questions with fewer, simpler, or no dependencies.
- If the selected question is already partially resolved, narrow or clarify it rather than pretending it is fully unresolved.

Return only JSON.
