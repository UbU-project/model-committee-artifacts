# Model-Committee Work Proposal Request

You are participating in the UbU `model-committee` process.

Your task is to produce one concrete work proposal as strict JSON.

Do not return prose outside the JSON object.

## Selected question

Question ID: `UBU-Q0078`  
Question title: `Interruption, escalation, and discovery-mode consent policy`  
Repository working tree: `/home/sean/ubu-design`

HEAD commit (provenance only): `469480860e389949428ceafe5d7ea07a825cbe9e`

```markdown
## UBU-Q0078: Interruption, escalation, and discovery-mode consent policy

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0052, UBU-Q0077 Blocks: realtime UX, discovery mode, short-horizon reactive repair Resolved by: None Last scored: 2026-10-14 Scored from commit: None

### Question

When may UbU interrupt, pause, escalate, or ask for clarification based on realtime or inferred conditions?

### Subquestions

1. What user-visible modes are required: off, text-only, voice session, discovery mode, meeting/logging mode, local-only mode, cloud-assisted mode?
2. What conditions justify interruption: imminent deadline failure, blocked Task, safety issue, unexpected external event, significant affect shift, or user-requested monitoring?
3. How should interruption policy avoid becoming surveillance, nagging, or emotional paternalism?
4. How should UbU record user overrides of interruptions and use them for future calibration?
5. What UI indicators must show active capture, routing, retention, and review status?

### Current direction

Realtime and discovery modes must be explicit and bounded. Interruption should preserve user sovereignty and Plan legitimacy rather than maximizing engagement or compliance.

### Resolution

Open.

---

```

## Patch baseline

The patch must apply to the **current working-tree files** in the repository above.
The selected question and context excerpts come from that working tree, which may
include uncommitted ranking and metadata updates.

- Set the JSON `base_commit` field to `469480860e389949428ceafe5d7ea07a825cbe9e` for provenance. It does not
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
- next decision id: `UBU-D0350`

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

#### UBU-Q0031

```markdown
## UBU-Q0031: Log Structure and Fields

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 implementation Resolved by: UBU-D0071 Last scored: Never Scored from commit: None

Resolved. See UBU-D0071.

---
```

#### UBU-Q0050

```markdown
## UBU-Q0050: Minimum Phase 1 bootstrap interview and next-action focus UX

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 public demo, nontechnical onboarding, mock app prototype, Release Outreach Pipeline demo scripts Resolved by: UBU-D0129 Last scored: Never Scored from commit: None

Resolved. See UBU-D0129.

---
```

#### UBU-Q0052

```markdown
## UBU-Q0052: Discovery-mode action inference and override semantics

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0050 Blocks: Discovery mode, mobile sensor workflow, behavior reconciliation, habit-pattern inference Resolved by: UBU-D0201 Last scored: 2026-05-27 Scored from commit: None

Resolved. See UBU-D0201.

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

#### UBU-Q0066

```markdown
## UBU-Q0066: Minimal Phase 3 Message Context Envelope schema

Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0061, UBU-Q0062 Blocks: Phase 3 cross-user communication, message priority triage, UbU-to-UbU interoperability Resolved by: UBU-D0270 Last scored: 2026-09-18 Scored from commit: None

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

Resolved by `UBU-D0270`: Phase 3 uses a minimal `MessageContextEnvelope` with sender and receiver Identity refs, source metadata, body or body ref, message kind, topic, priority, interrupt recommendation, response expectation, optional deadline, receiver treatment hint, assumptions, ambiguities, provenance, confidence, Compartment refs, and disclosure policy. Local objects may be referenced only through envelope-scoped opaque handles or redacted labels, inferred metadata remains provenance-marked and non-authoritative, and receiver UbU instances may create interruption, communication-review, or Task-creation suggestions but not mutate state without policy or user approval.

---
```

#### UBU-Q0068

```markdown
## UBU-Q0068: Structured message extraction schema, validation, and model strategy

Status: Solved Priority: Post-MVP Phase: Phase 1b Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0066 Blocks: direct-message ingestion, group-chat ingestion, communication-to-Task conversion, Association evidence extraction Resolved by: UBU-D0271 Last scored: 2026-09-18 Scored from commit: None

### Question

How should UbU implement the Message Context Extractor that converts unstructured direct-message and group-chat text plus available metadata into strict UbU JSON candidate structures?

### Subquestions

1. What input bundle should the extractor receive: raw message, source system, channel type, channel purpose, sender, receiver, Identity mapping, Association mapping, timestamp, thread context, Relationship history, and Compartment policy?
2. What output schemas are required: message classification, candidate Task, Objective link, priority, interrupt recommendation, actionability, assumptions, ambiguities, response expectation, confidence, and provenance?
3. How should validators and repair loops handle malformed JSON, invalid enum values, missing required fields, and overconfident inferences?
4. How should UbU distinguish explicit facts, channel metadata, thread inference, Relationship inference, Association inference, model guesses, and user-confirmed corrections?
5. Which extractor outputs can be accepted automatically, and which require user review?
6. When should UbU consider fine-tuned, distilled, or adapter-trained extractor models instead of general LLMs with strict schemas?
7. What training data and correction logs are needed before custom extractor models become justified?

### Current direction

Start with schema-constrained general LLMs or local models plus validation, repair, confidence, and provenance. Specialized extractor models are attractive later for privacy, latency, cost, and reliability, but should follow schema stabilization and labeled examples.

### Resolution

Resolved by `UBU-D0271`: the Message Context Extractor is a schema-constrained extraction pipeline that receives a policy-filtered `MessageExtractionBundle`, emits strict candidate `MessageExtractionResult` JSON with per-field confidence and provenance, uses deterministic validation plus bounded repair, distinguishes explicit facts from metadata, thread, Relationship, Association, model, and user-correction sources, limits automatic acceptance to low-risk non-mutating facts, and defers custom extractor models until schemas and correction datasets justify them.

---
```

#### UBU-Q0077

```markdown
## UBU-Q0077: Realtime interaction session and candidate update schema

Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0025, UBU-Q0031, UBU-Q0068 Blocks: realtime model adapters, discovery mode, meeting capture, interruption handling, Phase 1b candidate/admitted state boundary Resolved by: UBU-D0272 Last scored: 2026-09-18 Scored from commit: None

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

Resolved by `UBU-D0272`: realtime interaction is represented by a lightweight `RealtimeInteractionSession` plus reviewable `RealtimeCandidateUpdate` records. Sessions describe the consented interaction context and routing boundary; candidate updates carry typed, provenance-bearing suggestions that remain outside admitted state until ordinary review or explicit policy admission. Phase 1b candidate types are tag, dependency, preference, decomposition, and clarification question, while interruption, task progress, affect, external condition, Plan deviation, Log candidate, Task candidate, and AssociationAttestation candidates are allowed only within their existing review and Compartment gates. Model-noticed elapsed time is evidence, not Task, Calendar, or Log truth. Raw audio, video, screen, keystroke, and sensitive Compartment capture is local-only or prohibited unless a separate explicit mode and policy grant allow more.

---
```

### DECISIONS.md — cited decisions

#### UBU-D0071

```markdown
## UBU-D0071: MVP Logs are append-only per-instance event records

**Status:** Accepted

Resolved question: `UBU-Q0031`.

MVP Logs use a shared append-only entry envelope with event-specific payloads. Required fields are `log_entry_id`, `schema_version`, `instance_id`, `recorded_at`, `effective_at`, `event_type`, `actor_identity_ref`, `recorded_by_device_ref`, `target_ref`, `result`, `event_payload`, and `provenance`. Optional fields include old/new values, reason, notes, confidence, related Plan references, external references, annotation/correction links, and idempotency keys.

MVP event types are `task_completed`, `task_failed`, `task_moot`, `external_event_observed`, `snapshot_observed`, `objective_transitioned`, `plan_realized`, `decision_recorded`, `recalculation_triggered`, worker mutation submission/application/rejection events, and Log annotation/correction events.

Log entries are immutable once written. An annotation or correction creates a new Log entry that points to the original entry; it does not modify or delete the original. Corrections supersede interpretation of the original entry for query views while preserving the historical record.

Canonical Logs are stored per UbU instance, with device references recorded on entries. Device-local queues may exist for transport and audit, especially in later multi-device sync. MVP retention is indefinite for canonical Logs; archival may move old entries to colder local storage while preserving queryability and integrity. Deletion or redaction is deferred except where required by Compartment retention invariants.

Automation Workers contribute to Logs through worker Identities by submitting events or mutation requests. The canonical instance validates authority and records applied or rejected worker contributions with provenance, confidence when available, evidence references when available, and idempotency keys.

**Consequences:**

- Logs can support audit, reconciliation, worker accountability, plan-vs-reality feedback, and correction without losing historical claims.
- Detailed event-specific payload schemas may be refined alongside Task, Snapshot, Objective transition, worker mutation, and recalculation-trigger schemas.
- Large-history search can rely on rebuildable indexes over the append-only Log rather than treating indexes as canonical state.

---
```

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

#### UBU-D0129

```markdown
## UBU-D0129: Phase 1 bootstrap and next-action UX uses explicit object-backed minimum loop

**Status:** Accepted → DESIGN.md §4

See DESIGN.md §4.

---
```

#### UBU-D0201

```markdown
## UBU-D0201: Discovery mode uses reviewable evidence and explicit override admission

**Status:** Accepted → DESIGN.md §§4.1.2, 12.2, 17.8

See DESIGN.md §§4.1.2, 12.2, 17.8.

---
```

#### UBU-D0265

```markdown
## UBU-D0265: Social identity is scoped context, not intrinsic judgment

**Status:** Accepted → DESIGN.md §18.1, §12. Resolves `UBU-Q0054`.

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

**Status:** Accepted → DESIGN.md §19.1. Resolves `UBU-Q0061`.

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

**Status:** Accepted → DESIGN.md §19.2. Resolves `UBU-Q0062`.

UbU represents claims about an Association as `AssociationAttestation` records: immutable, scoped claims with claim type, polarity or asserted value, subject Identity or Association refs, scope, effective interval, evidence refs, provenance, confidence, Compartment refs, disclosure policy, review status, and correction, dispute, revocation, or supersession links. Required claim types are membership, non-membership, role, authority, commitment, Objective, norm, governance rule, capability, reputation, relationship, priority, lifecycle, and dissolution. Attestations are partial assertions; omitted claims do not imply absence.

Attestation source kind is explicit. `user_authored` attestations are first-person or operator-entered claims and can become accepted records through ordinary review. `imported` attestations preserve outside-source provenance and authority scope. `worker_generated_candidate` and `llm_generated_candidate` records are candidates only until reviewed; LLM output is interpretation metadata, not evidence by itself. Generated attestations record source locator, excerpt or payload hash, observed timestamp or interval, import/capture time, tool/parser/schema version, model and prompt/template identifiers or digests, run id, source Compartment, redaction level, and any evidence item refs needed to audit the extraction.

Confidence and trust are separate from authority. Confidence estimates how well the attestation is supported by its evidence; trust or source class explains why a source may be relied on for a particular claim. Public records and legal filings may be high-confidence evidence for their filed contents or legal status but not for informal social reality; signed descriptors depend on signer identity and authority; chat logs and meeting notes are contextual evidence; LLM interpretations begin as low-authority candidates even when drawn from large corpora. Confidence must not override explicit user declarations or accepted correction records.

Review status distinguishes `candidate`, `accepted`, `rejected`, `disputed`, `annotated`, `superseded`, `revoked`, and `expired`. Affected Identities or Associations may add response attestations or dispute records that accept, reject, annotate, narrow, or supersede a claim, but they do not erase the original historical assertion. Corrected query views follow the latest accepted supersession or revocation path within scope while retaining disputes and minority or counter-claims as queryable records.

Disclosure policy is part of the attestation, not an afterthought. Private or permissioned attestations must carry Compartment refs, projection policy, allowed audience, redaction behavior, and whether the existence of the claim may be revealed. Exported or cross-user views must not leak membership, role, relationship, non-membership, or dispute facts through notifications, IDs, counts, search results, or reconciliation hints unless the disclosure policy and actor capability permit that exposure.

AssociationAttestations may support Association reconciliation, organizational introspection, pseudonymous reputation, and multi-user trust only as evidence-bearing reviewed claims. They do not grant capability, settle authority, publish private Association state, or create organization-mode legitimacy unless a separate accepted authority, governance, or capability record permits that effect.

---
```

#### UBU-D0270

```markdown
## UBU-D0270: Message Context Envelopes carry minimal triage metadata without private reconstruction

**Status:** Accepted → DESIGN.md §20.1. Resolves `UBU-Q0066`.

UbU introduces a Phase 3 `MessageContextEnvelope` as a small cross-user communication projection for useful priority, interruption, response, Task, Objective, and blocker triage. The envelope is not a SyncStatement, Relationship record, Association record, Objective disclosure, or proof of authority; it is a sender-scoped message plus bounded metadata that the receiver may inspect, review, and map into local suggestions.

The minimum envelope fields are `envelope_id`, `schema_version`, `sent_at` or observed timestamp, `sender_identity_ref`, `receiver_identity_ref`, `source_system`, optional `source_message_ref`, `body` or `body_ref`, `message_kind`, `topic`, `priority`, `interrupt_recommendation`, `response_expectation`, optional `deadline`, `receiver_treatment_hint`, `assumptions`, `ambiguities`, `provenance`, `confidence`, `compartment_ids`, and `disclosure_policy`. `message_kind` distinguishes at least request, question, status update, blocker, commitment, decision, and FYI. `receiver_treatment_hint` states whether the receiver may consider the message as a Task candidate, status update, blocker notice, commitment notice, or FYI, without granting authority to create accepted state.

Local-only objects may be referenced only through envelope-scoped opaque handles, redacted labels, or user-approved summaries. Sender-local Objective, Task, Relationship, Association, Compartment, or attestation ids are not exported as stable identifiers unless the disclosure policy explicitly permits projection to the receiver. Hidden context must not leak through ids, counts, provenance paths, topic labels, search hints, notification text, or reconciliation metadata.

A receiver's UbU may translate envelope metadata into candidate actions: high interrupt recommendations can create Calendar interruption or focus-review suggestions; response expectations and deadlines can create communication-review reminders; Task, blocker, or commitment hints can create Task-creation or dependency-review candidates. These mappings remain receiver-local suggestions with provenance and confidence and require ordinary user or policy approval before becoming accepted Tasks, Calendar changes, commitments, Relationship facts, AssociationAttestations, or Objective state.

Explicit sender-provided metadata and inferred metadata are distinguishable per field. Inferred priority, topic, interrupt recommendation, response expectation, deadline, assumptions, ambiguities, or treatment hints must record source evidence, inference mechanism or model/template identifier, timestamp, confidence, redaction level, and review status. Confidence is advisory and never overrides explicit user declarations, accepted corrections, Compartment policy, or receiver-side authority checks.

Minimum user controls are export preview, per-field redaction, Compartment and disclosure-policy validation, reveal-existence control, local-only reference stripping, provenance inspection, correction/revocation after send where transport permits, and receiver-side review before local mutation. Defaults should minimize disclosure: raw private context, Relationship state, Association state, Objective details, Task details, and Compartment names stay local unless the sender explicitly approves their projection.

---
```

#### UBU-D0271

```markdown
## UBU-D0271: Message Context Extractors produce validated candidate structures

**Status:** Accepted → DESIGN.md §20.4. Resolves `UBU-Q0068`.

UbU implements the `MessageContextExtractor` as a schema-constrained extraction pipeline that converts raw or referenced messages plus allowed metadata into candidate UbU JSON structures. The extractor receives a `MessageExtractionBundle` containing source system, channel type and purpose, source message and thread refs, observed timestamp, raw body or body ref, sender and receiver Identity mapping candidates, channel participants, Association mapping candidates, allowed thread context, allowed Relationship summaries, Compartment and disclosure policy, locale or timezone, and extraction run metadata. Private context is included only when policy permits it.

The extractor emits a `MessageExtractionResult` with schema version, run id, candidate `MessageContextEnvelope`, message classification, candidate Task or Objective-link suggestions, candidate AssociationAttestations where allowed, priority, interrupt recommendation, actionability, response expectation, optional deadline, assumptions, ambiguities, per-field confidence, per-field provenance, and review or auto-accept eligibility. Extractor outputs are candidates and do not create accepted Tasks, Objective state, Relationship facts, Association facts, commitments, Calendar changes, or disclosure rights by themselves.

All extractor schemas are strict JSON schemas with closed enums, required fields, explicit nullable or unknown values, and per-field provenance requirements for inferred data. Validators run deterministically before admission. Repair loops may correct malformed JSON, normalize enum values, or fill missing required fields with unknown and low confidence, but they must not invent evidence, raise confidence without new support, or bypass Compartment policy. Repeated validation failure creates a review item rather than accepted state.

Provenance distinguishes explicit message facts, source metadata, channel policy metadata, thread-context inference, Relationship inference, Association inference, model inference, and user-confirmed correction. Each inferred field records evidence refs or hashes, source Compartment, redaction level, model or template identifier, parser or schema version, timestamp, confidence, and review status. User-confirmed corrections supersede extractor inferences through append-only correction paths.

Automatic acceptance is limited to low-risk, non-mutating parse facts that are directly evidenced and policy-allowed, such as source refs, timestamps, message ids, and exact explicit metadata. Candidate Tasks, Objective links, AssociationAttestations, Relationship updates, commitments, high interrupt recommendations, priority escalation, deadlines, and any cross-Compartment or cross-user projection require user review or an explicit receiver-side policy grant.

UbU should begin with general schema-constrained LLMs or local models plus validation, repair, confidence calibration, and provenance inspection. Fine-tuned, distilled, or adapter-trained extractors become justified only after schemas stabilize, correction logs are available, and measured privacy, latency, cost, or reliability needs outweigh the operational cost. Custom models use the same schemas, validators, provenance, review gates, and authority limits as general models.

---
```

#### UBU-D0272

```markdown
## UBU-D0272: Realtime interaction emits reviewable candidate updates

**Status:** Accepted → DESIGN.md §21.1. Resolves `UBU-Q0077`.

UbU represents realtime interaction as a hybrid boundary: a `RealtimeInteractionSession` records the consented interaction context, enabled sources, backend or model, Device, Identity, Compartment, routing, retention, evidence, candidate refs, provenance, review status, and correction or revocation links; it is not itself a Task, Calendar event, canonical Log entry, or raw sensor stream. Session states align with discovery mode where applicable: `inactive`, `active`, `paused`, `ended`, and `pending_review`.

Realtime backends emit `RealtimeCandidateUpdate` records. The minimum candidate update records candidate id, schema version, optional session id, emitted time, effective instant or interval, candidate type, optional target ref, payload, evidence refs or hashes, per-field provenance, confidence, Compartment refs, redaction level, routing mode, review status, admission hint, idempotency key, and correction, rejection, revocation, or supersession links. Phase 1b requires `tag`, `dependency`, `preference`, `decomposition`, and `clarification_question` candidates. Later or separately gated candidates may include `interruption`, `task_progress`, `affect_signal`, `external_condition_change`, `plan_deviation`, `log_candidate`, `task_candidate`, and `association_attestation_candidate`.

Candidate updates are not admitted state. They may create advisory review items, explanations, clarification prompts, or local suggestions, but they do not mutate Tasks, Calendars, Logs, Snapshots, Preferences, Plans, Relationships, Associations, AssociationAttestations, or Objective state without user acceptance, validated admission, or an explicit local policy grant for that mutation class. AssociationAttestation candidates remain subject to the evidence, trust, disclosure, and review rules for AssociationAttestations; Phase 1b does not require realtime audio/video or association extraction.

Model-noticed elapsed time is evidence, not canonical semantics. A realtime model may report that time seems to have passed, a user may have been interrupted, or a planned interval may differ from observed activity, but planner-valid meaning is created only by accepted `plan_realized`, Task lifecycle, Calendar, Snapshot, or Log records. Unknown, private, rest, interruption, different-Task, and quick-note intervals preserve uncertainty and must not imply Preference changes, Objective failure, habit patterns, or moral judgment.

Mandatory provenance for realtime observations includes source kind, observation or capture interval, Device and backend refs, model or tool identifier, prompt/template or schema version where applicable, evidence refs or payload hashes, source Compartment, redaction level, processing route, confidence, and review status. Audio, video, screen, keystroke, raw message body, raw file, raw GPS, and similarly sensitive inputs require explicit source enablement, visible capture state, and Compartment approval before they can produce candidates.

Routing is conservative by default. Raw sensitive capture, `no_cloud_llm` Compartments, and externally restricted Compartments are local-only or prohibited according to policy. Cloud processing is permitted only for policy-approved, visible, intentionally provided or redacted inputs. Cross-user projection, external export, covert or broad background capture, hidden persuasion timing, and automatic external mutation remain prohibited unless a later explicit mode, Compartment policy, capability grant, and user approval authorize the exact behavior.

---
```

### DESIGN.md — referenced sections

#### DESIGN.md §4 — Release Phases

```markdown
## 4. Release Phases

Phase labels name the phase that implements an item. The phases, their build order, and the mapping from labels used before `UBU-D0275` are recorded in §4.7.

### 4.1 Phase 1: Single-user GitHub dogfooding

A single user uses UbU to coordinate development of UbU itself.

Primary goals:

- import or observe GitHub issues, PRs, reviews, CI events, and milestones;
- represent them in UbU’s Objective/Task/Calendar model;
- generate useful Plans;
- provide a minimal bootstrap interview that creates an initial dogfooding context, current or stale affect Snapshot, and prioritized Objective/Task seed set;
- provide a next-action focus mode that shows one recommended next Task with an inspectable explanation of why it matters now;
- update GitHub as a low-dimensional projection of UbU state;
- expose limitations and open questions through dogfooding.

The frozen Phase 1 implementation set is:

- local single-user `user_mode` instance for UbU-runs-UbU;
- bootstrap interview and explicit seed model creation;
- GitHub issue, PR, review, CI, milestone, and comment import from live data or approved fixtures;
- ExternalReference-style source links sufficient to trace imported GitHub objects;
- Objective, Preference, Task, Container, UniverseState, Snapshot, Plan, Calendar, Log, Identity, Relationship, Compartment, Automation Worker, External Event, and External Reference schemas only to the depth required by the dogfooding loop;
- schedulable Static and Dynamic Tasks with Objective links, durations, dependency/precondition/effect fields, active/completed/failed/moot lifecycle handling, and moot reason codes sufficient for the demo;
- lightweight UniverseState facts, affect Snapshot handling, accepted mutation vocabulary, and deterministic precondition evaluation;
- append-only per-instance Logs with correction, annotation, provenance, worker-submission, and recalculation-trigger entries;
- Plan and Calendar generation for a next work window with a default Plan and inspectable explanation;
- next-action focus mode with start, done, snooze, reject, decompose, explain-more, and feedback controls;
- derived risk and human-complete plan-quality reports covering deadline risk, dependency fragility, worker or automation bottlenecks, stale affect, affect-margin, destructive pressure, and post-plan depletion warnings;
- minimal Compartment guardrails for `local_only`, `no_cloud_llm`, `no_external_export`, allowed integration/device references, low-security labeling, and logged boundary decisions;
- Automation Worker identity, scoped capability grants, explicit assignment/status display, and mutation or projection request submission without direct canonical writes;
- Delegation Substrate-compatible Task formalization fields where needed for dogfooding, worker assignment, or solo self-reminder clarity, including purpose, executor type, authority, expected output, evidence, review, privacy scope, and escalation notes;
- clearly marked GitHub projection previews or human-approved writes for UbU-managed labels, comments, or blocks, plus reconciliation reporting;
- manually structured Release Outreach Pipeline work items and artifact records for release notes, screenshots, scripts, and contributor calls-to-action;
- manually structured EthConf outreach notes and follow-up artifacts that dogfood Association formation and organizational introspection without requiring full multi-user Association automation.

Phase 2 explicitly defers:

- multi-device local-first sync;
- partial replication across Devices, Zones, or Compartments;
- secure cross-device Compartment propagation;
- sync conflict handling;
- cross-device worker or enclave coordination beyond the single local instance boundary.

Phase 3 is the bridge from the bootstrap MVP toward the full personal life-logistics product. It prioritizes minimal Resource/Skill-aware task readiness, private skill-tree foundations, and narrow Identity coordination that directly improves single-user life logistics. Phase 4 (§4.5) begins the full version 1.0 release track, and Phase 5+ (§4.6) continues it.

Phase 3 explicitly includes:

- minimal Resource objects and Task resource requirements sufficient to answer whether a Task is realistically ready to start or complete;
- minimal Skill objects and Task skill requirements sufficient to answer whether the user can perform a Task, needs a refresher, should learn a prerequisite, or should consider purchase/delegation;
- minimal cross-user contextual messaging between UbU instances;
- user-to-user Identity-mediated requests, status updates, questions, commitments, and blockers;
- bounded Message Context Envelopes carrying priority, interrupt recommendation, topic, response expectation, assumptions, ambiguities, provenance, and disclosure policy;
- legacy-message ingestion and limited upgrade paths where both parties use UbU;
- user-to-user Identity commitments, capabilities, and limited disclosure sufficient for narrow coordination;
- early Delegation Substrate use between Identities, if Phase 2/3 authority and privacy boundaries are in place.

Phase 3 still defers:

- full inventory management, full financial management, bank syncing, investment tracking, receipt OCR, tax categorization, and Quicken-like accounting workflows;
- public Skill Barter marketplace operation, payment/settlement flows, reputation markets, dispute resolution, and marketplace governance;
- shared global project truth between users;
- rich subjective Relationship-model exchange;
- full Association synchronization;
- multi-party governance, invitation, revocation, or trust protocols;
- broad personal-data ingestion beyond selected communication integrations.

Phase 1 keeps these abstractions documented for compatibility but does not implement them:

- Technique as a first-class planning object (minimal Technique-based Objective expansion in Phase 3, richer integration in Phase 4);
- Resource and Skill as first-class planning objects beyond lightweight compatibility placeholders (minimal readiness in Phase 3; richer integration in Phase 4);
- full Compact Calendar planner grammar and high-coverage transport format;
- complete Zone and Device system beyond the current local execution enclave;
- organization-mode and worker-mode web admin consoles;
- richer relationship-management, personal CRM, and longitudinal affect/growth models;
- full Release Outreach Pipeline video generation, rendering, and publication workflow;
- broad email, text-message, file, invoice, note, or personal-data ingestion outside narrow approved dogfooding fixtures;
- full realtime multimodal capture (Phase 1b MVP-release scope under `UBU-D0292`, with mandatory mode/device slices and delivery order set by `UBU-D0339` in §4.2); always-on assistant behavior remains outside this release commitment;
- public Skill Barter marketplace operation, payment/settlement flows, reputation markets, dispute resolution, and General Contractor subdelegation workflows;
- adaptive model-committee weighting, automatic patch application, GitHub mutation, and direct cloud-provider APIs.

Stop rule:

> A new abstraction may be added to Phase 1 only when it is required to complete the single-user GitHub dogfooding loop, enforce an accepted hard invariant, or avoid a known irreversible schema contradiction. Otherwise it must be documented as a Phase 2, Phase 3, or post-MVP concern and must not block Phase 1 implementation.

After `UBU-D0175`, new MVP blockers are allowed only when discovered during implementation of a concrete Phase 1 slice and accompanied by a blocker certificate naming the blocked object or file, failed acceptance criterion, unsafe fallback, minimum answer needed, and persistence impact. Phase 1 readiness is judged slice-by-slice rather than by global philosophical completion of the model.

### 4.1.0 Phase 1 readiness scoring

Phase 1 readiness is a derived, evidence-backed, human-reviewed signal. It reports `scope_freeze_readiness` and `mvp_readiness` from per-slice evidence, gates, and score caps rather than from raw open-question count or model confidence. Unresolved nonblocking questions do not reduce Phase 1 readiness after `UBU-D0175` unless they carry a valid blocker certificate. Model-committee may compute readiness reports and propose README readiness text, but it must not publish or update public readiness signals automatically. The accepted rubric is `UBU-D0189`.

The minimum dogfooding loop that proves UbU’s core model is:

1. bootstrap the operator, project context, available work window, constraints, and current or stale affect Snapshot;
2. import or load a curated UbU GitHub fixture with issues, PR/review/CI signals, milestone context, and source links;
3. map those inputs into Objectives, Tasks, External Events, Logs, UniverseState facts, and External References;
4. generate a Calendar with a default Plan for the next work window;
5. present one recommended next Task with an explanation of Objective value, dependencies, deadlines, affect constraints, worker status, and risk findings;
6. record completion, failure, snooze, rejection, decomposition, or override as Log evidence and trigger recalculation when appropriate;
7. preview or perform a human-approved UbU-managed GitHub projection update;
8. show a reconciliation or risk summary that makes the changed model inspectable.

### 4.1.1 Phase 1 public demo criteria

The Phase 1 public demo should use real UbU GitHub issues or a frozen fixture captured from real UbU GitHub data when live access is unsafe, unavailable, or non-reproducible.

The smallest persuasive demo is an end-to-end single-user dogfooding loop:

1. import a curated set of UbU issues, PR/review/CI signals, and milestone context;
2. map those inputs into Objectives, schedulable Tasks, External Events, and traceable source links;
3. generate a Calendar with a default Plan for the next work window;
4. show why the Plan was chosen, including dependencies and worker assignment or worker status;
5. show user-mode affect constraints by applying a current affect Snapshot or by creating an affect-collection Task when affect data is stale or missing;
6. write or preview clearly marked UbU-managed GitHub projection labels, comments, or managed blocks;
7. show a risk summary covering at least deadline risk, dependency fragility, and worker or automation bottlenecks.

The demo must not require Phase 2 sync, Phase 3 multi-user coordination, full RBAC, a complete Compact Calendar UI, or autonomous remote GitHub mutation. If live GitHub writes are unsafe for the public recording, a dry-run projection is acceptable only when it shows the exact payload that would be written after human approval.

### 4.1.2 Phase 1 user-facing loop

Phase 1 must include a minimal first-person UX loop for dogfooding. The loop is intentionally narrow: it collects explicit answers, creates explicit UbU objects, recommends one next Task, records what happened, and recalculates from Logs and current state.

1. **Bootstrap interview**
   - Ask exactly the minimum questions needed to seed the first dogfooding Plan:
     1. `What are you trying to move forward right now?`
     2. `Which project context should UbU use for this first session?`
     3. `How much usable time do you have for the next work window?`
     4. `Is there a deadline, meeting, release target, or external event that changes what matters today?`
     5. `What work should UbU consider first?`
     6. `What is already blocked, unavailable, or not worth recommending right now?`
     7. `How are your energy, stress, and mood right now?`
     8. `When choosing between useful work, what should UbU favor today?`
   - Record the project/context answer as UniverseState facts and source-linked Logs.
   - Record available time, static commitments, deadlines, and immediate constraints as UniverseState facts, Tasks, External Events, or Logs.
   - Record affect answers as a user-declared Snapshot, or create an affect-collection Task when the user skips or the data is stale.
   - Record favored tradeoffs as Preferences only when the user accepts an explicit Preference statement; otherwise retain them as Log notes or Objective annotations.
   - Seed a small Objective/Task set from user answers plus approved GitHub fixtures or configured imports.

2. **Minimum data for one recommendation**
   - A user or operator Identity.
   - At least one active Objective.
   - At least one active schedulable Task linked to that Objective with title and duration or work-window estimate.
   - A current work window or next available time.
   - Current or explicitly stale affect state.
   - Known hard blockers, deadlines, dependencies, and source references for the recommended Task when available.
   - If this minimum is missing, the next recommended Task should be a clarification, import, or affect-collection Task rather than pretending to optimize.

3. **Plan generation**
   - Generate a candidate default Plan from explicit modeled state.
   - Preserve inspectability of the full Plan.
   - Explain relevant Objective value, constraints, dependencies, deadlines, risk findings, worker status, and affect constraints.
   - Mark any fixture-backed, hardcoded, or mock recommendation path directly in the UI and run artifact.

4. **Calendar preview**
   - Treat Calendar preview as a notable recurring Task.
   - Default cadence is before the first recommended Task in a user-declared work window and after a material Calendar regeneration when UbU is about to rely on the new default Plan.
   - Let the user inspect the candidate Calendar or default Plan before relying on it.
   - Ask whether the Plan feels plausible, motivating, humane, and consistent with the user's current context.
   - Ask only lightweight model-repair questions about expected execution, perceived control, social pressure, and missing context; store raw psychological comments as review notes unless the user accepts a concrete canonical update.
   - Allow the user to correct Preferences, Objectives, Tasks, affect Snapshots, availability, or estimates before execution.
   - Let the user snooze, skip, or adjust Calendar-preview cadence through the cadence rule on its evergreen system Objective (§7.4.1).

5. **Next-action focus mode**
   - Present one recommended next Task as the default UI surface.
   - Show title, Objective link, estimated duration or work window, current status, and source/provenance label.
   - Show `why this matters now` with at least the served Objective, timing reason, main dependency or blocker status, affect/staleness status, and top risk or opportunity considered.
   - Show what UbU considered, including visible counts or links for candidate Tasks, blocked Tasks, hard constraints, affect constraints, worker/projection state, and imported or fixture sources.
   - Provide controls for start, done, snooze, reject, decompose, override, and explain more.
   - Keep full Plan inspection one action away through a Plan drawer, timeline, or inspector that shows ordered Tasks, assumptions, source links, and explanation fragments.
   - One-task focus reduces cognitive load; it must not hide the Plan, turn the recommendation into opaque automation, or imply that the user has committed to the default Plan.

6. **Feedback, Log review, and recalculation**
   - After `done`, ask whether completion matched the estimate, whether the modeled effect happened, and whether energy or stress changed enough to update the Snapshot.
   - After failure or blocked execution, ask what assumption failed: missing dependency, wrong duration, stale affect, interruption, unclear Task, changed priority, or other.
   - After snooze, ask for the next review time or condition and whether the reason is timing, affect, dependency, or user preference.
   - After rejection, ask whether the Task is wrong, not valuable now, blocked, too large, already done, duplicate, or should become moot.
   - After override, record the chosen Task or action, the reason if supplied, and whether the override should update Preferences, availability, Task estimates, Objective status, or only this Plan.
   - Treat Log review as a notable recurring Task. Default cadence is at the end of a work window or at the next startup when there are unreconciled deviations, with a weekly catch-up if routine review is skipped.
   - Store reviewed outcomes as canonical updates only when they change Logs, Snapshots, Preferences, Objectives, Tasks, or recalculation triggers; otherwise keep autonomy, competence, relatedness, attitude, subjective-norm, perceived-control, and expected-execution comments as noncanonical review notes or derived report inputs.
   - Record outcomes as Logs and trigger recalculation when the outcome changes Task status, availability, affect state, dependencies, Objective status, or user Preferences.
   - Treat failure, rejection, override, and deviation as model evidence, not user blame.

7. **Discovery mode when appropriate**
   - Allow the user to choose discovery mode at any time.
   - Show active, paused, ended, and pending-review state, enabled sources, routing or retention limits, and evidence count.
   - Provide pause, resume, exit, inspect, reject, delete, and review controls.
   - Collect only enabled Phase 1 evidence such as quick notes, user-selected current action, Calendar context, configured integration events, coarse motion or location category, focus state, and app-state category.
   - Treat inferred observations as reviewable evidence, not final truth; defer ambiguous interpretation until Log review or user-visible reconciliation.

The Phase 1 version may be simple and fixture-backed. Fixture behavior must be labeled as fixture behavior, mock recommendations must not be described as implemented planning, and public demos should show the exact boundary between explicit UbU objects, approved fixtures, and implemented planner behavior. The public demonstration pattern is bootstrap, one recommended next Task, explanation, user feedback, recalculation, and full-Plan inspection. It does not require broad email, text-message, file, invoice, note, or personal-data ingestion. It must not claim complete life-modeling, therapeutic authority, autonomous life coaching, complete planning automation, full privacy isolation, Phase 2 sync, or Phase 3 multi-user coordination. The purpose is to demonstrate the core UbU experience: one meaningful next action, with an explanation, grounded in explicit state.

### 4.1.3 Release Outreach Pipeline dogfooding

The Release Outreach Pipeline should become part of ordinary UbU-runs-UbU release management. A minor release should produce a release outreach package when the current project state contains enough user-visible, developer-visible, or contributor-visible change to justify public explanation.

The Phase 1 package is manual but structured. Minimum package artifacts are:

- `manifest.json` with release, source, Identity, Compartment, status, and hash metadata;
- `claim_register.json` with audience, support status, evidence refs, and review state for each public claim;
- `evidence_index.json` for source artifacts, hashes, selectors, and provenance;
- release-note, script, narration/caption, announcement, and publication-metadata drafts when relevant;
- `media_refs.json` for approved screenshots, UI-test exports, fixture captures, or recordings;
- `export_review.json`, `approvals.json`, and `publication_plan.json`;
- known-limitations, future-work, and contributor call-to-action notes tied to real artifacts.

Media provenance records must distinguish approved live captures, approved fixture/synthetic captures, UI-test exports, and mock/demo-only captures. Every claim in scripts, release notes, and public posts links to claim-register evidence and uses the support labels accepted in `UBU-D0180`.

Phase 1 dogfooding may generate text drafts, collect approved screenshots or recordings, assemble a publication plan, and record human approvals. It may use local renderers or LLM/script workers only as Automation Workers that produce candidate artifacts under capability grants.

Post-MVP work includes automatic UI capture generation, demo-flow export, script drafting from repository state, narration and caption generation, thumbnail generation, video rendering automation, platform upload, social posting, mailing-list publication, analytics feedback, and trusted auto-publication policies.

The full video-generation and publication pipeline remains future work. Phase 1 should generate reviewable communication artifacts before trying to automate external publication.

### 4.1.4 EthConf outreach as Association-introspection dogfooding

EthConf outreach is a Phase 1 dogfooding workflow for Association formation and organizational introspection.

Phase 1 represents the workflow with ordinary Objectives, Tasks, Logs, External Events, External References, Release Outreach package artifacts, public dogfooding review packages, and manually reviewed candidate AssociationAttestations. It does not require full Association automation, multi-user Association sync, automatic contact analysis, or broad message/archive ingestion.

Minimum evidence set:

- canonical design files and accepted decisions;
- public dogfooding artifacts such as model-committee review packages, selected patches, issue fixtures, release outreach packages, and public retrospectives;
- structured EthConf outreach notes with private details separated from redacted public summaries;
- follow-up Tasks, missed follow-ups, public replies, contributor/funder/design-partner next actions, and outcome Logs.

Minimum outputs:

- candidate AssociationAttestations about declared project mission, outreach commitments, observed follow-through, and evidence gaps;
- mission-alignment questions, especially whether outreach behavior supports the privacy-first self-governance, contributor-recruitment, workflow-discovery, and compatible-funding goals;
- priority-drift observations and commitment-mismatch notes;
- follow-up Tasks with owners, review dates, evidence refs, and public/private disclosure labels;
- redacted retrospective notes suitable for public dogfooding artifacts.

The core review prompt is: `Does UbU's actual outreach behavior prove, from reviewable evidence, that the project pursued its stated goals?` The answer may be yes, no, mixed, or insufficient evidence, and must cite evidence or gaps rather than model confidence alone.

Public artifacts expose only redacted summaries, evidence selectors, hashes, public links, reviewed claims, and approved follow-up Tasks. They must not expose private notes, raw contact details, unapproved conversation content, private funding terms, private relationship hypotheses, or Compartment-protected payloads.

Acceptance criteria: the workflow produces one evidence-backed retrospective, at least one candidate AssociationAttestation or explicit no-evidence finding, at least one mission-alignment question, any needed follow-up Tasks, an export review, and a public-safe redacted artifact that labels fixture/manual/LLM-assisted content. A human reviewer must be able to trace each public claim to evidence or to a recorded evidence gap.

### 4.1.5 Optional VoxPopuli EthConf demo

UbU may optionally demonstrate a **VoxPopuli** flow at EthConf if it does not displace higher-priority dogfooding, contributor, or funder deliverables. In this flow, a user speaks freely about what they wish would happen or what feels disorganized. An LLM-assisted extractor converts that natural-language input into candidate Objectives, Tasks, constraints, preferences, and planning assumptions for the user to inspect, correct, accept, or reject.

VoxPopuli is a trust-building outreach hook, not a replacement for the narrow Phase 1 bootstrap interview. It should demonstrate how LLMs can help structure abstract human concerns while UbU remains the explicit, inspectable planning system.

### 4.1.6 Phase 1 implementation priority order

Phase 1 implementation proceeds in this priority order:

1. Remove all in-memory orchestrator state (`MemoryState`) and fully implement `ubu_store` as the canonical local admission and persistence path for the running loop.
2. Log event vocabulary: correction, annotation, provenance, worker-submission, and recalculation-trigger entries.
3. UniverseState facts.
4. Affect Snapshot content, accepted mutation vocabulary, and deterministic precondition evaluation.
5. Structured bootstrap interview with seed Objective, Task, and Snapshot creation.
6. Plan and Calendar generation for a next work window with a default Plan and inspectable explanation.
7. Activate `ubu_github_adapter` as a functioning orchestrator dependency for import, projection preview, approved writes, and reconciliation.

The remaining §4.1 frozen-set items follow in bootstrap-dependency-driven ordering: persistence and vocabulary precede the features that must reference them.

**Phase 1 realization (`UBU-D0241`).** Step 3 (UniverseState facts) is foundationally realized, together with the accepted mutation vocabulary and deterministic precondition evaluation named in step 4, implemented as the §11 container and pure `ubu-core` semantics and persisted by `ubu-store`. The precondition, effect, and bootstrap-fact wiring of these facts into the loop is realized across Wiring-A/B/C (`UBU-D0242`): preconditions gate planning, a completed Task's effects mutate the facts, and the bootstrap records the initial facts under the `UBU-D0243` namespace convention. Step 4's affect-Snapshot content is realized by the affect observation route (`UBU-D0304`); step 5's interactive bootstrap interview remains.

### 4.2 Phase 1b: Quick UbU merge through the switch

Phase 1b extends the Phase 1 system until it can replace Quick UbU, the separately developed personal planner that serves as the dogfooding user's working daily tool, so that Quick UbU becomes legacy software (`UBU-D0275`).

Exit criterion:

> The switch: the dogfooding user's primary daily planning runs on mainline UbU.

Phase 1b closed by decision on 2026-10-10 (`UBU-D0305`), with its planner condition discharged by the greedy Stage 1 certification under the CPU tensor-worker profile (`UBU-D0303`). The ratification gate of `UBU-D0291` was met with no provisional subject root; its registry remains the standing agenda, evaluated at the deferred switch. The switch is not in force and follows the completion of Phase 2.

Until the switch, Quick UbU remains the primary tool and mainline is exercised against non-primary test data. (The switch is deferred until Phase 2 is complete (`UBU-D0305`).)

**The Phase 1b rule.** Every Quick UbU feature merged into mainline adopts the restrictions, forms, and functional boundaries of the MVP design it corresponds to, even when that design's implementation phase is later. Open questions that block Phase 1b are answered during Phase 1b whatever phase they were previously tagged, and the items they pull forward are re-tagged Phase 1b.

Phase 1b includes:

- the Phase 1b foundations already decided, including sync-ready mutation envelopes, the Device registry, tombstones, redacted Handles, projection-conflict repair, Device-local credentials, the local advisory worker, advisory batches, and first-class advisory candidate review;
- the Quick UbU practical features, ported under the Phase 1b rule;
- the planner the switch requires: decomposition into Containers whose ordered child Tasks are grouped into segments that stay together (§9.4); an allowed time range on Tasks; Task value and priority inputs to Plan scoring; routines as evergreen-Objective recurrence that instantiates Tasks (§7.4.1); partial placement when not every Dynamic Task fits the planning horizon; and the desktop GPU backend with its CPU reference path (§16.10).

**MVP-release scope amendment (`UBU-D0292`, sequenced by `UBU-D0339`).** Local camera-and-voice interaction, including explicitly enabled realtime capture, remains in the Phase 1b MVP-release backlog. Phase 1b's closure does not establish release completion or implemented capture. `UBU-D0339` resolves `UBU-Q0183`'s scope allocation below; implementation proceeds alongside Phase 2 without reopening Phase 1b or changing `UBU-D0305`'s switch criteria. `UBU-Q0128` remains Phase 1b-origin work now tracked in Phase 2; Phase 3 does not gate this interaction surface. Mobile planning retains `UBU-D0290`'s stewardship workload and parity rules; any workload/API or stewardship amendment still requires `UBU-Q0160`.

**Capture delivery order (`UBU-D0339`).** Deliver the following slices in order. Rows 1–3 are mandatory before MVP release; exact hardware/software configurations require evidence-backed selection under `UBU-Q0177`. A Device is an execution enclave: qualify each capture, review and inference endpoint and any inter-Device route, even on shared physical hardware.

| Order | Required mode/device slice | Delivery boundary |
|---|---|---|
| 1 | Operator-owned laptop/desktop: `off`/`text_only`, bounded still-image capture and push-to-talk voice, with independently selectable speech output; `snapshot_capture` and `conversational` classes. | Establish §21.1.3's common concern-to-candidate, exact correction, review/admission and next-action loop, plus every ordinary voice and text/form workflow. Camera preview or transcription alone cannot pass. Keep the complete manual loop usable with capture and models disabled. |
| 2 | At least one qualified phone capture/review profile: bounded still images and push-to-talk voice through the same loop. | Inference may run on that phone or a separately qualified operator-owned laptop/desktop worker through an authenticated encrypted, policy-authorized route. Show the chosen route before acquisition/dispatch; worker loss preserves permitted drafts and useful text/forms. This allocates interaction, without selecting a mobile planner or compute API. |
| 3 | At least one qualified laptop/desktop profile: explicitly started, finite local realtime sessions in ordinary-attention `voice_session` and Task-scoped `discovery_mode`, using the `local_only_realtime` route. | Support microphone-only, camera-only and simultaneous ongoing audio/live-camera input; repeated one-shot captures alone cannot satisfy this slice. Demonstrate `conversational` and `streamed_supervision` classes, visible source state and gaps, pause/resume/stop, interruption and reviewable updates. Models propose evidence/candidates; only ordinary review/admission changes state. |

**Evidence and retention precede enablement.** Each slice needs revision-pinned passing §21.1.1 host-mapping, qualifier, corroboration and admission evidence for its enabled adapters (`UBU-Q0166`), §21.1.3 usefulness/access evidence including `UBU-D0338`'s affected user-tested workflows, §21.1.4 numerical and deadline-boundary results (`UBU-Q0168`), and §1's endpoint/runtime qualification (`UBU-Q0177`, `UBU-D0331`, `UBU-D0336`). Unsupported mappings remain unresolved; failed mandatory gates withhold the affected slice. Select at least one bounded camera-and-voice path from rows 1–2 under `UBU-Q0178`'s reviewed ownership/budget comparison (`UBU-D0332`, `UBU-D0337`), including CPU-only and existing-worker candidates and explicit no-worker gaps; manual fallback does not establish multimodal reach.

Before row 3 is exposed, demonstrate §21.1.5's broker isolation, finite aggregate limits, original-time/coverage bindings and bounded cancellation (`UBU-D0326`), plus §12.2's enforced memory-only raw buffer ceiling of 60 seconds, copy/cache/swap controls, expiry/restart cleanup and policy-filtered inspection/discard controls (`UBU-D0327`–`UBU-D0328`). No automatic derivative archive replaces discarded media. Retained minimized evidence/candidates need their own permitted purpose and retention authority. Optional summary/hash/clip/export controls stay unavailable until their separate retention, review and routing gates pass; capture consent never authorizes saving or exporting.

**Coordination with the switch and device wave.** Build and qualify these slices against non-primary test data while Phase 2 develops Device trust, Compartment policy, sync and personal-worker routes. Cross-Device inference waits for its endpoint/route gates; permitted candidate exchange uses the existing Phase 2 contract and carries no live source consent or approval. Exercise phone/desktop switching, corrections, partial replicas, stale policy, revocation and restart without stale admission, duplicate effects or implicit capture resumption. Phase 2's existing three-Device sync freeze evidence remains required independently; its third Device need not implement AV. Release integration and operator-reviewed acceptance follow Phase 2 completion and the deferred switch under `UBU-D0305`/`UBU-D0291`. A capture shortfall blocks MVP-release completion, without adding a switch criterion or reversing Phase 1b closure.

Publish the release support matrix by Device/backend/mode with enabled sources/classes, finite session/resource limits, measured results, user-tested access combinations, retention/security outcomes, approved cost scenarios and useful fallbacks. Phone streaming, meeting/multi-party logging, retrospective analysis, cloud-assisted interaction, other sensitive sources and restricted-attention interaction beyond immediate local controls are subsequent separately qualified slices, not prerequisites for this release minimum. Withhold unavailable processing/review under §21.1.2 while keeping immediate local pause/stop/silence available; always-on capture is outside the commitment. Resource/Skill/Technique-dependent scenarios retain their later phases.

The richer household apprentice, maintenance triage, purchasing defense, Skill evidence and tacit-expertise examples depend on first-class Technique, Resource and Skill implementations. They are phased but unimplemented dependencies: Phase 3 supplies minimal readiness and Technique-based Objective expansion, and Phase 4 supplies richer integration. Evidence semantics, hardware suitability and scenario-specific scope remain unresolved. Camera-first MVP scope does not promise that those later planning objects or scenarios are MVP-ready.

Phase 1b defers multi-device sync and everything else assigned to Phase 2 and later.

### 4.3 Phase 2: Single-user multi-device synchronization

A single user runs UbU across multiple Devices / execution enclaves.

Phase 2 is in development from 2026-10-10 (`UBU-D0305`), and the switch of `UBU-D0275` follows its completion.

Phase 2 validates:

- local-first sync;
- partial replication;
- Zone and Compartment boundaries;
- device roles;
- conflict handling;
- user-owned worker execution on the user's laptop, desktop, or other trusted personal compute device.

Design rubric:

> Phase 2 should feel like multi-agent coordination, but all agents happen to agree.

A Phase 2 personal worker is not a mandatory cloud dependency. It is an optional execution backend for a user's own UbU instance. It may provide more CPU, GPU, memory, storage, network availability, or battery-independent runtime than the mobile device while preserving the user's practical control over the compute environment.

### 4.4 Phase 3: Minimal multi-user / Identity coordination

Multiple humans coordinate through explicit Identities, capabilities, limited disclosure, and commitments.

Phase 3 should enable coordination without surveillance or shared global truth.

### 4.5 Phase 4: Version 1.0 release track

Phase 4 begins the full version 1.0 release track. It expands Resource and Skill usability, Technique-database integration, DIY-versus-purchase/hire planning, and the user-facing capability graph without requiring the public Skill Barter marketplace.

### 4.6 Phase 5+: Version 1.0+ release track

Phase 5+ continues the full version 1.0+ track with mature inventory, financial-management extensions, public or federated Skill Barter, reputation/evidence, dispute workflows, and other multi-party marketplace features.

### 4.7 Phase map and build order

| Phase | Scope | Status |
|---|---|---|
| Phase 0 | ETHConf NYC demo | Complete |
| Phase 1 | Single-user GitHub dogfooding (§4.1) | Feature-complete |
| Phase 1b | Quick UbU merge through the switch, including the planner (§4.2) | Complete (closed by decision 2026-10-10, UBU-D0305; switch deferred until Phase 2 is complete) |
| Phase 2 | Single-user multi-device synchronization (§4.3) | In development (from 2026-10-10, UBU-D0305) |
| Phase 3 | Minimal multi-user / Identity coordination and Resource/Skill task readiness (§4.4) | Planned |
| Phase 4 | Version 1.0 release track (§4.5) | Planned |
| Phase 5+ | Version 1.0+ release track (§4.6) | Planned |

Phases are built in table order. A phase label names the phase that implements an item; pulling an item forward re-tags that item and never renumbers phases (`UBU-D0275`). Labels used before `UBU-D0275` map as follows: Phase 3A becomes Phase 3, Phase 3B becomes Phase 4, and Phase 4+ becomes Phase 5+.

---
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

**AV refinement for the MVP interaction surface (`UBU-D0292`).** The primary camera-and-voice surface does not enable continuous sources implicitly. A consented capture session remains visible, source-scoped, pausable and inspectable under the clause above. Process raw AV locally on the operator's own hardware, minimize context and retain only material justified by its purpose. Selective clips, summaries, hashes or deletion are operator-controlled under Compartment policy; subsequent export needs its own allowed route and approval. No-cloud/no-export denials remain hard. Segmentation and bounded local interfaces follow §21.1.5 (`UBU-D0326`); raw-media retention defaults follow `UBU-D0327` below and operator controls follow `UBU-D0328`; recording a policy does not claim its enforcement has shipped.

**Default raw-media retention (`UBU-D0327`).** For an explicitly enabled continuous or near-continuous perception session, durable raw-media retention is off. Use a local memory-only rolling buffer with a maximum sample age of **60 seconds from acquisition**, measured on the original source monotonic clock. Release material earlier when its authorized processing/look-back purpose no longer needs it or a shorter policy limit applies. The finite byte and aggregate-resource limits required by §21.1.5 may shorten this window; it is a privacy ceiling, not a promised look-back duration or latency result. Pins, worker copies, queues, retries and decoded frames share the original deadline. No raw disk spool, debug payload, general cache, automatic backup or sync copy is authorized by this default. A Device/backend profile whose persistence paths cannot meet effective policy is ineligible.

On pause, end, source/Identity/Compartment change or revocation, invalidate affected reads and pending results immediately and release unsaved raw buffers and copies within §21.1.5's bounded cancellation grace, without exceeding the original age ceiling. Resume starts a fresh source epoch. Pending review, an unfinished model job and session metadata do not keep raw media alive; expired spans become explicitly unavailable.

A purposeful clip requires explicit operator approval before durable saving, bound to the selected source intervals, purpose, eligible capture Device, Compartment and current policy versions. A relevance suggestion or capture consent alone cannot approve it. Store only the bounded selected material in access-controlled encrypted local storage on that Device, on the operator's own hardware, with lineage and an absolute expiry. The default expiry is **24 hours after the earliest captured sample**, or an earlier source/Compartment deadline. Missing reliable capture-time mapping denies durable saving. Review, copying, reprocessing and restart never reset expiry. A longer finite lifetime requires a separate purpose-specific approval bound to the clip and current policies before expiry; expired material cannot be recovered by renewed approval. Durable saving remains unavailable if eligible storage, finite clip/quota limits or expiry enforcement are absent. Recheck current policy and Device eligibility on every access; capture-time approval is not continuing authority. Backup, another Device and export require their separately allowed route and approval; hard denials remain binding.

Expiry denies further reads and result release and cancels affected jobs. Remove expired app-controlled clip payloads, any clip-specific keys and reconstructive raw copies; if cleanup is interrupted, reconcile deadlines and complete pending cleanup before reopening capture or retained-media access after restart. Clock rollback or uncertain remaining lifetime must deny access rather than extend retention. Report scheduled, pending, failed and confirmed local deletion distinctly; a deadline or local unlink cannot establish remote deletion or forensic erasure. App shutdown may leave cleanup pending and must not be reported as completed deletion.

Do not automatically replace discarded raw media with a transcript, embedding, hash or summary archive. Separately retained minimized evidence needs its own allowed purpose and retention authority, inherits source disclosure restrictions, and may outlive raw media only when policy permits. Purge disallowed reconstructive derivatives and retain only permitted gap/deletion metadata. The append-only rules below continue to govern admitted claims. This is a policy decision, not implemented automated deletion or a physical-erasure guarantee; continuous capture and clip storage require the revision-pinned retention checks in `UBU-D0327` before enablement. Detailed operator controls follow `UBU-D0328` below.

Selective raw-media retention and append-only evidence are distinct. Discarding a raw stream or deleting an authorized clip does not edit an admitted historical claim in place: append minimized correction, revocation, deletion or tombstone metadata with the appropriate links. The immutable Log retains evidence about what was asserted, reviewed or withdrawn, not an automatic reconstructible copy of private raw media. A hash is not proof of an event and can itself leak correlation; policy must govern it. Later views can mark deleted evidence unavailable without inventing a replacement observation. `UBU-Q0162` tracks the reported Log-editability enforcement gap.

**Operator media controls (`UBU-D0328`, Phase 2).** Keep capture state and pause/stop controls available independently of model completion. A policy-filtered media inventory shows each permitted source/session, capture Device and processing route, purpose, source intervals and coverage gaps, transient raw buffer versus retained artifact, storage destination, effective retention rule/deadline and cleanup state. Inspect/replay only material still available and authorized; show expired/deleted evidence as unavailable. Show known permitted copies and export outcomes without claiming a complete inventory of other Devices or recipients. Counts, previews, filenames, source refs and Compartment labels obey current visibility policy; omit an item when even existence is denied.

Offer independent actions with no durable-save or export option selected by default. Show the selected source/artifact scope, destination, retention effects and any remaining permitted evidence before committing an action. Capture consent, relevance suggestions, model output and prior deletion authorize none of these actions implicitly.

| Control | Required behavior |
|---|---|
| Discard raw / Delete retained artifact | Remove the selected app-controlled payloads and affected disallowed copies/derivatives; invalidate their handles and pending reads/results, cancel affected work and report cleanup under `UBU-D0327`. Discard does not create a summary, transcript, embedding or hash. It acts on the displayed scope; Stop capture is separately available immediately. |
| Create summary | Use only selected, permitted local source spans with bounded analysis. Preview the exact proposed text with source/transform provenance, analyzed/missing coverage, attribution and uncertainty. It remains advisory evidence requiring ordinary review/admission; a summary cannot certify an event or effect. |
| Create hash | Make hashing an explicit optional operation. Show the algorithm/version, exact input representation and source scope; record lineage only as policy permits. A hash can disclose identity or correlation and is neither anonymization nor proof of the captured event. No mandatory media hash belongs in discard or audit records. |
| Save clip | Preview the actual selected sources, channels, intervals and any codec padding. Apply `UBU-D0327`'s eligible encrypted local storage, explicit version-bound approval and absolute expiry; use §21.1.5's bounded authorized read/output scope. Unavailable spans cannot be recovered or silently replaced with adjacent capture. |
| Export | Review the final output and metadata locally, then obtain a separate operation-bound approval for an allowed recipient and route through the authoritative Compartment/export gate. Include filenames, attachments, thumbnails, captions, timestamps, source refs and hashes in disclosure review; clipboard, share-sheet, file-copy and integration routes outside protected storage receive the same checks. |

Summary/hash previews have finite temporary leases under §21.1.5. Durable artifact retention requires an explicit separate save choice bound to the displayed result/source versions, actor/Device, purpose, eligible local destination and current source-policy versions, with the effective retention rule and any absolute expiry. Admitted claims and Task lore retain their existing source-history/lifecycle rules. The inventory shows the approved result and its own retention authority; permitted derivatives may outlive raw media, but raw discard never grants that authority. Review, copying and retries cannot renew original source or artifact deadlines. Combining actions must preserve each approval and its failure outcome; a failed derivation cannot silently retain raw media past expiry. No cloud fallback, training use, backup or sync permission follows.

Export approval binds the actor and eligible Device, immutable final artifact/payload versions, all contributing source restrictions and current policy versions, purpose, recipient, route, format/metadata, recipient retention conditions, finite approval expiry and idempotency key. A displayed finite batch may share approval; future captures and wildcard scopes cannot. State known recipient retention/deletion assurances and mark unknowns explicitly. If required protection cannot be established, deny the route. Deleting raw media, redacting text or selecting a hash never declassifies an output. Mixed-source outputs satisfy every source's restrictions; `local_only`, `no_cloud_llm`, `no_external_export` and allowed-Device/integration denials remain hard, including on derived content. UI approval cannot substitute for route authority or the existing export permit.

Recheck authority, visibility, policy, artifact availability and approval bindings on every read/result release and immediately before egress, serialized with revocation/cancellation. For streamed export, recheck before each released segment and stop unsent bytes on invalidation; emitted bytes cannot be recalled. Changed content, source restrictions, recipient, route, Device or relevant policy versions invalidates approval and requires fresh permitted review. Cancel invalidates unsent approval and queued export immediately; restart cannot replay it. Expiry of material required by the operation denies execution. Retention of a separately authorized derivative does not require recreating its raw source. Report completed, failed, cancelled and delivery-unknown outcomes distinctly; retries use current checks and ordinary idempotency/outcome rules, never another recipient or changed payload under old approval. Relevant stale-sync/projection checks in §1 and disclosure/tool rules in §§21.3–21.4 still apply.

Record permitted minimized action, approval/denial, policy, source/artifact lineage and cleanup outcome links through existing review/Log paths. Rejected/dismissed proposals follow §17.0 without a compulsory reason or Preference inference. Raw deletion leaves admitted claims subject to their existing append-only correction/redaction rules and marks their source unavailable; withdrawing a claim is a separate correction/revocation. Purge disallowed reconstructive derivatives, preserving only permitted correction/deletion/gap metadata. Distinguish scheduled, pending, failed and confirmed local cleanup; never imply remote recall or forensic erasure. `UBU-D0328`'s revision-pinned behavioral gates are required before exposing these controls or export routes; this specifies behavior, not shipped enforcement.

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

#### DESIGN.md §17.8 — Regular Log review

```markdown
### 17.8 Regular Log review

Log review is a notable recurring Task. Its purpose is to keep UbU's model aligned with the user's actual behavior, later reflection, and corrections. Default cadence is at the end of a work window or at the next startup when unreconciled plan/reality differences exist, with a weekly catch-up if routine review is skipped. The user may adjust cadence through the recurrence rule on its evergreen system Objective (§7.4.1), snooze, or skip controls.

A Log review Task may:

- reconcile DiscoveryEvidenceItems and undetailed or under-specified time periods by accepting, correcting, rejecting, deleting, marking private/unknown, or deferring them;
- ask whether a user override reflected a better local judgment, bad timing, missing preconditions, wrong duration estimate, stale affect data, social pressure, or changed Preference;
- compare planned Tasks against completed, failed, moot, snoozed, rejected, or unobserved Tasks;
- convert user-approved observations into corrected Logs, Snapshots, Preferences, Objective annotations, Task estimate updates, or recalculation triggers;
- keep raw autonomy, competence, relatedness, social pressure, attitude, subjective norms, perceived control, and expected-execution comments as review notes unless the user accepts a concrete canonical update;
- mark proposed habit patterns as user-endorsed, tolerated, unwanted, or unresolved.

Log review should not become a blame ritual. It is a model-maintenance Task for self-governance. The minimum Phase 1 annotation boundary is resolved by `UBU-D0181`.

**Phase 2 and post-MVP additions to the Log review report:**

- Friction annotations: when actual Task duration significantly exceeds planned duration, the review should prompt the user for an optional friction note explaining the cause. Friction notes feed the Bayesian duration learning system. See `UBU-Q0113`, `UBU-Q0116`.
- Streak surfacing: recurring Task completion chains are surfaced as motivational signals. See `UBU-Q0111`.
- Deferred-intent review: due `deferred_intent` Tasks surface on Log review or startup, with a default 30-day item interval and earlier review for material linked-Objective changes. The prompt offers promotion with explicit Preference commitment, keep deferred with a next review, permanent dismissal through `moot`, or Objective review; missed prompts never imply acceptance or dismissal. See §9.7 and `UBU-D0308`.
- Circadian affect pattern report: after sufficient Log history, time-of-day patterns in affect state may be surfaced as a standard report to inform planning priors.
```

#### DESIGN.md §18.1 — Social identity boundary

```markdown
### 18.1 Social identity boundary

Social identity theory affects UbU as a boundary discipline: an Identity may present externally, hold permissions, and own accountability, but it is not the same object as a social role, self-concept statement, group membership, Compartment, or temporary mode-switching context.

A `Role` records situational expectations or authority for an Identity. A `GroupMembership` records a scoped relation between an Identity and a group. A `SelfConcept` records user-declared first-person meaning. A `ModeContext` records the currently salient planning lens for recommendations or coordination. A `Compartment` remains the privacy, routing, retention, and export boundary. These dimensions may reference each other, but none of them implies another by default.

Group membership can affect Objectives, Preferences, disclosure, trust, and coordination only through explicit accepted records: user-declared statements, Relationship records, Compartment policy, capability grants, organizational rules, or review-accepted observations. Membership may raise the salience of an existing Objective, select an appropriate presentation, attach coordination obligations, or constrain sharing when tied to policy; it must not create inferred Preferences, loyalty judgments, trust scores, risks, capabilities, or stereotypes merely from category membership.

In-group and out-group effects are represented as inspectable contextual hypotheses or user-declared concerns, not as hard-coded judgments about people or groups. Any planner use must expose the source, scope, affected behavior, and correction path, and choices that would restrict options, disclose data, classify another person, or change coordination authority require the ordinary review and approval gates.

For Phase 3 multi-user coordination, UbU needs minimal `Group`, `GroupMembership`, `RoleAssignment`, and `ModeContext` records with provenance, confidence, scope, optional Compartment refs, and review/correction support. Learned salience, norm conflict modeling, collective identity dynamics, and social-psychology interpretation remain later research.

---
```

#### DESIGN.md §19.1 — Association object model

```markdown
### 19.1 Association object model

An `Association` is a first-class, Identity-scoped record of perceived coordination among people, Identities, projects, or groups. It is perspective-bound: it records what a modeling Identity believes, has observed, or is allowed to rely on, not an objective global fact about membership, authority, or group boundaries. An Association may exist entirely inside one user's `user_mode` model.

An Association is distinct from an Organization Identity, a Relationship, and an External Reference. An Organization Identity is an accountable actor with organization-mode authority or shared operational state; a Relationship is a scoped relation between particular Identities; and an External Reference points to outside artifacts such as registries, repositories, contracts, calendars, chats, or web pages. Legal entities, institutional records, rosters, and public project pages are evidence or references for an Association, not the Association itself.

A minimal Association record contains `association_id`, `perspective_identity_id`, optional `display_name`, `association_kind`, perceived participant or member claim refs, role claim refs, shared Objective refs, commitment refs, norm or rule refs, lifecycle state, lifecycle event refs, evidence and External Reference refs, confidence, Compartment refs, disclosure policy, provenance, review status, and correction or revocation links.

Membership, authority, roles, commitments, and boundaries are represented as scoped claims or linked attestations, not as absolute facts inferred from a roster or category label. Association records follow Snapshot-style partial assertion semantics: omitted participants, roles, commitments, or norms do not imply absence, and corrections or revocations are append-only.

Informal groups such as friend groups, parties, amateur leagues, FOSS projects, conference cohorts, mutual-aid circles, and skill networks are ordinary Associations when UbU needs to reason about coordination beyond a single bilateral Relationship. They may carry norms, expected roles, shared Objectives, commitments, and privacy rules when those are explicitly declared, observed, reviewed, or imported as evidence.

Lifecycle changes such as invitations, joins, exits, revocations, dormancy, reactivation, merges, splits, and dissolution are append-only lifecycle events or AssociationAttestation records. Disclosure and projection of Association state require Compartment and projection policy; a private perceived Association must not become visible to other participants merely because UbU models it.

An Association becomes formal enough to justify `organization_mode` only when it has explicit durable authority, governance or decision procedures, shared operational state, accountability for external actions, capability grants or external mutation rights, and accepted disclosure/projection boundaries. Until then, UbU treats it as a user-mode perspective model and does not grant collective authority, synthesize collective Preferences, or treat disputed membership as settled.

For Phase 3, UbU needs the minimal Association record, lifecycle state, links to GroupMembership and RoleAssignment claims, Objective and commitment references, Compartment/disclosure policy, confidence, provenance, and correction/revocation paths. Cross-user reconciliation, dispute semantics, public organizational introspection, pseudonymous reputation, norm conflict analysis, collective-identity dynamics, and Skill Barter governance remain later research or separately resolved questions.

Full Association implementation is not required for Phase 1. Phase 1 may represent EthConf/outreach dogfooding through existing Objectives, Tasks, Logs, Relationships, External Events, and External References, plus manually structured or fixture-backed candidate AssociationAttestations.
```

#### DESIGN.md §19.2 — AssociationAttestations

```markdown
### 19.2 AssociationAttestations

An **AssociationAttestation** is a claim by an Identity, human operator, Automation Worker, parser, or LLM-assisted workflow about an Association.

Possible claim types include:

- membership;
- non-membership;
- role;
- authority;
- commitment;
- objective;
- norm;
- governance rule;
- capability;
- reputation;
- relationship;
- operational priority;
- mission alignment;
- dissolution or dormancy.

LLM-generated AssociationAttestations are candidate claims. They require provenance, source references, confidence, review status, and disclosure policy. They must not be treated as authoritative social truth merely because they were generated from a large corpus.

An `AssociationAttestation` is an immutable, scoped claim about an Association, an Identity's relation to it, or a coordination artifact linked to it. Required claim types include `membership`, `non_membership`, `role`, `authority`, `commitment`, `objective`, `norm`, `governance_rule`, `capability`, `reputation`, `relationship`, `priority`, `lifecycle`, and `dissolution`; each records its subject refs, asserted value or polarity, scope, effective interval, evidence refs, confidence, Compartment refs, disclosure policy, provenance, review status, and dispute, correction, revocation, or supersession links.

Attestation source kind is explicit. `user_authored` attestations are first-person or operator-entered claims that may become accepted through ordinary review; `imported` attestations preserve outside-source provenance and authority scope; `worker_generated_candidate` and `llm_generated_candidate` records remain candidate claims until reviewed. Generated candidates must record source locator, excerpt or payload hash, observed timestamp or interval, capture/import time, tool/parser/schema version, model and prompt/template identifiers or digests, run id, source Compartment, redaction level, and evidence item refs sufficient to audit the extraction.

Confidence and trust are separate from authority. Confidence estimates support from evidence; source class explains why a source may be relied on for a claim type. Public records and legal filings can be strong evidence for filed contents or legal status without settling informal social reality; signed descriptors depend on signer identity and authority; chat logs and meeting notes are contextual evidence; LLM interpretations begin as low-authority candidates even when drawn from large corpora. Confidence never overrides explicit user declarations, accepted correction records, Compartment policy, or capability boundaries.

Review status distinguishes `candidate`, `accepted`, `rejected`, `disputed`, `annotated`, `superseded`, `revoked`, and `expired`. Affected Identities or Associations may add response attestations or dispute records that accept, reject, annotate, narrow, or supersede a claim, but they do not erase the original historical assertion. Corrected query views follow the latest accepted supersession or revocation path within scope while retaining disputes, annotations, minority reports, and counter-claims as queryable records.

Disclosure policy is part of the attestation. Private or permissioned attestations must carry Compartment refs, projection policy, allowed audience, redaction behavior, and whether the claim's existence may be revealed. Exported or cross-user views must not leak membership, role, relationship, non-membership, reputation, or dispute facts through notifications, IDs, counts, search results, reconciliation hints, or provenance fields unless the disclosure policy and actor capability permit that exposure.
```

#### DESIGN.md §20.1 — Message Context Envelope

```markdown
### 20.1 Message Context Envelope

A `MessageContextEnvelope` is a Phase 3 cross-user communication projection that carries enough context for triage without reconstructing the sender's private state. It is not a SyncStatement, Relationship record, Association record, Objective disclosure, or proof of authority; it is a message plus bounded metadata that the receiver may inspect and map into local suggestions.

The minimum envelope record contains `envelope_id`, `schema_version`, `sent_at` or observed timestamp, `sender_identity_ref`, `receiver_identity_ref`, `source_system`, optional `source_message_ref`, `body` or `body_ref`, `message_kind`, `topic`, `priority`, `interrupt_recommendation`, `response_expectation`, optional `deadline`, `receiver_treatment_hint`, `assumptions`, `ambiguities`, `provenance`, `confidence`, `compartment_ids`, and `disclosure_policy`. `message_kind` distinguishes at least `request`, `question`, `status_update`, `blocker`, `commitment`, `decision`, and `fyi`. `receiver_treatment_hint` states whether the receiver may consider the message as a Task candidate, status update, blocker notice, commitment notice, or FYI.

The envelope may reference local-only objects only through envelope-scoped opaque handles, redacted labels, or user-approved summaries. Sender-local Objective, Task, Relationship, Association, Compartment, or attestation ids are not exported as stable identifiers unless the disclosure policy explicitly permits that projection to the receiver. Hidden context must not leak through ids, counts, provenance paths, topic labels, search hints, notification text, or reconciliation metadata.

Sender intent is advisory unless backed by separate authority. A sender may ask the receiver to treat a message as a Task, blocker, commitment, status update, or FYI, but the receiver's UbU records only a candidate review item until the receiver accepts it or a receiver-side policy grants authority. The envelope must not create accepted Tasks, Calendar changes, commitments, Relationship facts, AssociationAttestations, or Objective state by itself.

A receiver's UbU may translate envelope metadata into local suggestions: high interrupt recommendations can create Calendar interruption or focus-review suggestions; response expectations and deadlines can create communication-review reminders; Task, blocker, or commitment hints can create Task-creation or dependency-review candidates. These mappings preserve envelope provenance and confidence and remain correctable, rejectable, and revocable according to local policy.

Provenance is per-field when metadata is inferred. Inferred priority, topic, interrupt recommendation, response expectation, deadline, assumptions, ambiguities, or treatment hints must record source evidence, inference mechanism or model/template identifier, timestamp, confidence, redaction level, and review status. Confidence is advisory and never overrides explicit user declarations, accepted corrections, Compartment policy, or receiver-side authority checks.

Minimum controls are export preview, per-field redaction, Compartment and disclosure-policy validation, reveal-existence control, local-only reference stripping, provenance inspection, correction/revocation after send where transport permits, and receiver-side review before local mutation. Defaults minimize disclosure: raw private context, Relationship state, Association state, Objective details, Task details, and Compartment names stay local unless the sender explicitly approves their projection.
```

#### DESIGN.md §20.4 — Message Context Extractor

```markdown
### 20.4 Message Context Extractor

A `MessageContextExtractor` is the local or policy-approved model pipeline that turns unstructured direct-message or group-chat text into strict candidate UbU JSON. It is an extraction and triage layer, not an authority layer: its outputs are reviewable candidates until accepted by user action or by an explicit local policy grant.

The extractor input is a `MessageExtractionBundle` containing raw body or `body_ref`, source system, channel type, channel purpose, source message and thread refs, observed timestamp, sender and receiver refs or mapping candidates, available Identity mapping evidence, Association mapping candidates, channel participant metadata, allowed thread context, allowed Relationship or history summaries, relevant Compartment and disclosure policy, locale or timezone, schema version, prompt or template version, and extraction run metadata. Raw message bodies, Relationship state, Association state, and private Objective or Task context are included only when the relevant Compartment policy permits that use.

The extractor output is a `MessageExtractionResult` containing schema version, run id, candidate `MessageContextEnvelope`, message classification, topic, candidate Task suggestions, candidate Objective-link suggestions, candidate AssociationAttestation suggestions where allowed, priority, interrupt recommendation, actionability, response expectation, optional deadline, assumptions, ambiguities, per-field confidence, per-field provenance, and review status or auto-accept eligibility. Suggested structures remain candidate records and do not mutate Tasks, Objectives, Relationships, Associations, Calendar entries, commitments, or admitted state by themselves.

Extractor schemas are strict JSON schemas with closed enums, required fields, explicit nullable or unknown values, and no unvalidated extra fields. A deterministic validator runs before any downstream use. A bounded repair loop may fix malformed JSON, normalize invalid enum values, or replace missing required inferences with unknown and low confidence, but it must not invent evidence, raise confidence without new support, or route around Compartment policy. Overconfident or unsupported inferences are downgraded and marked for review.

Per-field provenance distinguishes `explicit_message_fact`, `source_metadata`, `channel_policy_metadata`, `thread_context_inference`, `relationship_context_inference`, `association_context_inference`, `model_inference`, and `user_confirmed_correction`. Each inferred field records evidence refs or excerpt hashes, source Compartment, redaction level, model or template identifier, parser or schema version, timestamp, confidence, and review status. User-confirmed corrections supersede extractor claims through append-only correction paths rather than editing the original extraction result.

Automatic acceptance is limited to low-risk, directly evidenced, non-mutating parse facts such as source refs, timestamps, source message ids, and explicit sender-provided metadata that passes policy validation. Candidate Tasks, Objective links, AssociationAttestations, Relationship updates, commitments, priority escalation, high interrupt recommendations, deadlines, and cross-Compartment or cross-user projection require user review or a specific local policy grant.

UbU should start with schema-constrained general LLMs or local models plus validation, repair, confidence calibration, and provenance inspection. Fine-tuned, distilled, or adapter-trained extractor models become appropriate only after schemas stabilize and correction logs show enough repeated examples to justify the privacy, latency, cost, or reliability tradeoff. Custom models must preserve the same schemas, validators, provenance, review gates, and authority limits as general models.

Custom extractor training requires retained-with-consent training bundles, gold `MessageExtractionResult` labels, user corrections, rejected candidates, field-level provenance labels, ambiguity examples, no-action examples, hard negatives, redaction and Compartment labels, and evaluation sets split by source system, channel type, relationship context, and privacy class.

The initial extractor strategy should use general LLMs or local models constrained by schemas, validation, and repair loops. A specialized fine-tuned or distilled extractor model may become valuable later after UbU has stable schemas and corrected training examples.

The system should not require a new foundation model. The likely requirement is reliable structured extraction, not open-ended conversation.
```

#### DESIGN.md §21.1 — Realtime interaction sessions and candidate updates

```markdown
### 21.1 Realtime interaction sessions and candidate updates

A realtime interaction session is a bounded period in which UbU receives a continuous or semi-continuous stream from text, voice, video, sensors, tools, or a model provider.

Possible session modes include:

- `off`;
- `text_only`;
- `voice_session`;
- `meeting_logging`;
- `discovery_mode`;
- `local_only_realtime`;
- `cloud_assisted_realtime`.

Realtime sessions may produce candidate updates, but they do not directly mutate canonical state. Candidate update types include observed events, interruption signals, Task progress deltas, affect signals, Plan deviations, external condition changes, clarification questions, WorkItem candidates, LogEntry candidates, and AssociationAttestation candidates.

A realtime model's sense of elapsed time is evidence. Planner-time semantics are determined by UbU's explicit Task, Plan, Calendar, Log, Snapshot, and recalculation rules.

Realtime models are replaceable interaction backends for the primary MVP camera-and-voice surface (`UBU-D0292`), including conversation, capture, triage and short-horizon repair. UbU represents a realtime run as a lightweight `RealtimeInteractionSession`, not as a Task, Calendar event, canonical Log entry, or raw sensor stream. A session records the consented interaction context and source boundary; the model's outputs are `RealtimeCandidateUpdate` records until admitted through the ordinary planner, Log, Snapshot, Compartment, and review rules.

The minimum `RealtimeInteractionSession` record contains `interaction_session_id`, `schema_version`, `instance_id`, `actor_identity_ref`, `device_ref`, `started_at`, optional `ended_at`, `session_state`, `source_modes`, `backend_ref`, `model_or_tool_ref`, prompt or template version, enabled input sources, `compartment_ids`, `disclosure_policy`, retention or redaction policy, routing mode, evidence item refs, candidate update refs, provenance, review status, and correction or revocation links. Session states align with discovery mode where applicable: `inactive`, `active`, `paused`, `ended`, and `pending_review`.

The minimum `RealtimeCandidateUpdate` record contains `candidate_update_id`, `schema_version`, optional `interaction_session_id`, `emitted_at`, effective instant or interval, `candidate_type`, optional `target_ref`, payload, source evidence refs or excerpt hashes, per-field provenance, confidence, `compartment_ids`, redaction level, routing mode, review status, admission hint, idempotency key, and correction, rejection, revocation, or supersession links. The Phase 1b candidate types required for Quick UbU are `tag`, `dependency`, `preference`, `decomposition`, and `clarification_question`. Additional allowed candidate types are `interruption`, `task_progress`, `affect_signal`, `external_condition_change`, `plan_deviation`, `log_candidate`, `task_candidate`, and `association_attestation_candidate`, but each remains subject to its existing schema, authority, review, and Compartment gates.

A realtime candidate never mutates admitted state by itself. A tag, dependency, decomposition, preference, Task, Log, Snapshot, Calendar, Plan-deviation, or AssociationAttestation effect becomes canonical only after user acceptance, validated admission, or a specific local policy grant for that exact mutation class. Rejected, corrected, superseded, or revoked candidates remain auditable according to the candidate lifecycle rules.

Model-noticed elapsed time is represented as evidence or a candidate interval, not as planner-valid Task execution, Calendar occupancy, or Log truth. Planned time is reconciled only through accepted `plan_realized`, Task lifecycle, Calendar, Snapshot, or Log records. Unknown, private, rest, interruption, different-Task, and quick-note intervals preserve uncertainty and must not create Preference changes, Objective failures, habit claims, or moral meaning by inference.

Mandatory provenance for realtime observations includes source kind, capture or observation interval, device and backend refs, model or tool identifier, prompt or template digest where applicable, parser or schema version, evidence refs or payload hashes, source Compartment, redaction level, processing route, confidence, and review status. Audio, video, screen, keystroke, raw message, raw file, raw GPS, and similarly sensitive sources also require explicit source enablement and policy-visible capture state before they can produce candidates.

Routing defaults are conservative. Local-only processing is required for raw sensitive capture, `no_cloud_llm` Compartments, and sources whose policy denies external processing. Cloud processing is optional only for policy-approved, user-visible, redacted or intentionally provided inputs. Cross-user projection, external export, covert capture, broad background capture, hidden persuasion timing, and automatic external mutation are prohibited unless a later explicit mode, Compartment policy, capability grant, and user approval allow that exact behavior.

#### 21.1.1 Observation, context and closed-loop execution

The architecture is observation → candidate objects/facts → Identity and object resolution against permitted persistent context → candidate UniverseState/Resource implications → proposed preconditions or constraints → CPU semantic/policy/conflict validation and operator admission → planning or recalculation over admitted state. Ambiguous identity matches remain alternatives, not merged objects. The camera can suggest the relevance of an observed object to the user's life; it cannot infer intent, affect, Preference changes or ownership as self-certifying truth.

Confidence helps choose high-information clarification, not bypass admission. Corroboration is consequence-sensitive: escalate evidence and human review when an interpretation could change a consequential action; unknown or incompatible evidence stays unresolved. An observation consistent with completion is not proof that the modeled effect occurred. `UBU-D0293` defines source-kind mapping; `UBU-D0320` specifies observation bindings/admission; `UBU-D0321` specifies qualifier preservation; `UBU-D0324` specifies corroboration and review below. `UBU-Q0166` retains host-mapping evidence; `UBU-D0329` specifies completion/effect evidence in §17.10. Existing candidate/Discovery records are the reuse point, not newly implemented perception schemas.

The execution loop is **See → understand → plan → direct → observe → verify → learn → update**. Understanding proposes interpretations; planning remains the kernel's; direction presents an inspectable next action; new observations prompt reconciliation; verification uses the required evidence and operator review; learning proposes model corrections; only admitted updates close the loop. Inference never certifies itself by being repeated downstream. Technique guidance, observed Skill development and procedure-to-Technique capture depend on the later-phase objects and unresolved evidence/safety questions, not merely access to a camera. See [the interaction document](MULTIMODAL_INTERACTION.md) and [the security boundary](MULTIMODAL_SECURITY.md).

**Observation binding profile (`UBU-D0320`).** Reuse `DiscoveryEvidenceItem` (§12.2) for captured evidence and unresolved interpretations, and per-object `AdvisoryCandidate` records (§21.2.1) for supported proposals. One evidence item may support several candidates; accepting one interpretation never accepts the others. Store the following `observation-admission/1` bindings in their existing payload/evidence slots alongside the native normalized proposal. This is a payload contract, not a new first-class perception object, candidate-kind enum or `FactProvenance` extension. A producer's version-pinned adapter must validate its mapping to the host schemas; an unsupported or lossy mapping stays unresolved and needs separate versioned review under `UBU-Q0166`.

| Binding | Required content and interpretation |
|---|---|
| `profile` | Exactly `observation-admission/1`; retain the host record's own schema version separately. Unknown profiles cannot be admitted. |
| `sources` | Nonempty list of `{evidence_ref, evidence_version, source_ref, session_ref, session_version, selector, effective_start, effective_end}`. The controller supplies/resolves these refs; selectors locate a retained frame, span, reading or policy-permitted summary within that source, not model-invented evidence. Endpoints are timezone-bearing timestamps with start <= end; equal endpoints denote an instant. One-shot capture also has a session ref. The captured session revision establishes source enablement at acquisition time; it is not a live grant for later processing or execution. Evidence provenance retains origin Device, source/signal kind, capture enablement and route/policy refs, backend/model/tool, parser/schema version, template digest where applicable, confidence and permitted hashes. |
| `context` | `{context_bundle_ref, digest}` binding the exact permitted context used, or `null` only if no persistent context was consulted. Every context-based match must resolve to this bundle's supplied refs. A model cannot search denied context, mint a canonical ref, or substitute a fresh context for the one used. |
| `subjects` | List of `{observation_key, alternatives, selection}`; observation keys are unique within the capture and are not canonical entity ids. Each alternative is either `{object_ref, object_version}` or `{candidate_ref, candidate_version, proposed_object_key}`. An empty alternatives list means unknown; `selection: null` means unresolved. Selection is either null or `{alternative_index, review_event_ref}`; only explicit operator resolution selects an alternative. Confidence, coincident labels or repeated sightings cannot merge Identities/objects or establish ownership. |
| `proposal` and `claim_evidence` | The native normalized proposal (null for evidence without a supported proposal), plus `claim_evidence: [{field_path, evidence_refs}]` covering every proposed field change. Paths are nonempty arrays of native field names or zero-based array indices; evidence refs are nonempty lists drawn from `sources`. With no proposal, claim_evidence is empty. UniverseState implications use §11.3's `operation`/`target`/`payload` rules; proposed preconditions use §10.1. Unknown/omitted fields propose no write or deletion; a literal JSON null value must be an explicit claim. Source kinds follow `UBU-D0293`; confidence and corroboration stay in evidence, while `FactProvenance` remains `{kind, recorded_at}`. |
| `dependencies` | List of `{candidate_ref, candidate_version, proposal_path}` for proposed-object/value links used by the proposal; paths use the same field-path syntax. Object creations also name `proposed_object_key`. Such links are candidate-local, never canonical ids. Admission must resolve them to already-admitted records/values or changes in the same validated atomic batch; otherwise defer. Native schemas that cannot express these links retain the interpretation as Discovery evidence. |
| `observed_versions` | Ref-to-version map covering every evidence item, resolved target and context input relied on, plus referenced candidate dependencies. Where a current-state container has no internal version, bind its admitted envelope/frontier digest; do not invent UniverseState history. Creates bind a stable proposed-object key and an explicit create-only/expected-absence condition. Review also binds the candidate's own current version. |
| `observed_policy_versions` | Ref-to-version map of all relied-on capture, routing, Compartment, retention and admission policies. Current policy, not capture-time permission alone, governs review and admission. |
| `predecessors` | `{evidence: [{evidence_ref, evidence_version}], candidates: [{candidate_ref, candidate_version}], logs: [log_entry_id]}` using the host's correction/supersession links. Admitted-history corrections cite original Log ids under §17.3. Empty lists mean an original interpretation, not absence of earlier world events. |

**Perceptual qualifier preservation (`UBU-D0321`).** Use a companion `perceptual-qualifiers/1` payload in the existing evidence payload/ref and candidate evidence/per-field slots. Keep the host schema version and `observation-admission/1` bindings separate and unchanged. These are payload requirements, not new first-class objects, candidate kinds or `FactProvenance` fields. Reuse is conditional on a validated host mapping; an opaque payload slot described in prose is not proof that its schema, serializer and store preserve the content.

| Payload content | Required preservation and links |
|---|---|
| `profile` | Exactly `perceptual-qualifiers/1`. Unknown versions remain unresolved; adapters cannot silently discard or reinterpret them. |
| Evidence `claims` | List of `{claim_key, content, qualifications}`. Keys are stable and unique within an evidence revision, not canonical object ids. Content retains the exact permitted source statement or structured interpretation, including target/identity alternatives, attribution, negation, conditions, quantities, units and uncertainty. A summary alone is insufficient when it drops these distinctions. Evidence without a supported native proposal still retains its claims. |
| Claim `qualifications` | Separate entries for `observed`, `inferred`, `operator_confirmed` and `externally_verified`; several may concern the same claim. Each entry retains `{term, attribution, effective_start, effective_end, recorded_at, confidence, basis, related_claims}` and resolves to its enclosing evidence revision and the binding profile's source/session/selector. Attribution identifies the actual person, external source or model/tool and its applicable revision, using controller-resolved refs or explicit unknown metadata. Observation and recording times remain distinct. Additional relied-on sources join `sources` and `observed_versions`. These terms are evidence labels, never additions to `ProvenanceKind` or an ordered confidence rank. |
| Qualification `confidence` | Original score, scale, meaning/method and producing source/model revision, or explicit null when unreported. Preserve differing per-claim/per-source estimates; a candidate aggregate cannot replace them. Unreported is neither zero nor certainty, and confirmation/verification cannot synthesize a confidence score. |
| Qualification `basis` | Retain how the claim was established and the underlying kind under `UBU-D0293`. Observation cites the permitted frame/span/reading; inference retains model/tool revision, input refs, assumptions and unresolved alternatives. A personal confirmation retains the actual statement and speaker; an instrument reading retains instrument ref, reading, units, method and supplied accuracy/calibration qualifications. An actual derivation retains version-bound input facts/evidence, admitted envelope/frontier bindings for canonical inputs, rule/tool revision, parameters and result. Unreported optional details stay explicitly unknown; missing evidence of an actual instrument/reading or computation cannot justify `measured` or `derived`. |
| External-verification basis | Additionally retain verifier/source ref, exact claim and scope checked, verification time, method, result including inconclusive/disputed outcomes, and supporting evidence refs/versions. The verifier's attestation provenance is distinct from the checked claim's underlying kind. Verification of an interpretation neither changes its original kind nor supplies an admission event. |
| Qualification `related_claims` | List of `{evidence_ref, evidence_version, claim_key, relation}` locating claims supported, contradicted, confirmed, verified, computed from or sharing a source. Preserve source ancestry and dependency links so a repeated interpretation, copied report or reused frame cannot appear as independent corroboration. Unknown independence remains unknown; sufficiency follows `UBU-D0324` below. |
| Candidate `field_qualifiers` | List of `{field_path, claim_refs: [{evidence_ref, evidence_version, claim_key}]}` covering every path in `claim_evidence`. Use that binding's native field-path syntax; refs resolve to claims in its version-bound `sources`. Each mapped path has nonempty claim refs. One field may have conflicting or differently qualified evidence. Evidence-only records have no candidate field mapping. A partial acceptance binds only the reviewed subset and its supporting claims; other claims remain unaccepted. |

**Confirmation, review and retention.** A later confirmation, reading, derivation or verification creates separately attributed evidence and a linked candidate revision where needed; it never edits an earlier inference into a different source. Acceptance is an ordinary review event, distinct from a source confirmation. Bind the exact candidate revision, field mapping and evidence revisions to that event and its resulting mutation/Log/Snapshot refs through `UBU-D0320`'s atomic admission links. The companion payload is part of bound operation content; changing it follows the same revision, idempotency and fresh-review rules. Admitted kind follows the actual accepted assertion, reading or computation under `UBU-D0293`; `FactProvenance` remains `{kind, recorded_at}`. Review presents qualifiers, contradictions and attribution rather than only a confidence badge. `UniverseTarget` stays name-only, and unsupported value/computation paths stay evidence-only.

Losslessness means preservation of the permitted retained claims, qualifications and links through normalization, storage, review and authorized exchange; it does not require retaining raw media beyond policy. Redaction, purge or an incomplete Replica must indicate only permitted unavailability/incompleteness, never imply an absent qualifier was false or reconstruct disallowed content from retained derivatives. Inherit all source restrictions. Missing required bindings block admission under `UBU-D0320`; loss of evidence neither automatically retracts nor reaffirms admitted history.

**Host-mapping gate (`UBU-Q0166`).** Before enabling an adapter, pin the producer/adapter, host schemas, Rust/JSON serializers, store/review API and exchange revisions and policy configuration. Publish the exact host field paths or payload-ref mapping for every row above, with serialized fixture inputs and retrieved outputs. Compare structured values and all ids, versions, field paths, attributions, times, qualifiers and links after validation, serialization, persistence/reload, review and authorized exchange. Structural round-trip fidelity establishes preservation, not perceptual truth or evidence sufficiency. Exercise:

- a visual hypothesis with both observation and inference, differing field confidences and incompatible alternatives, with no canonical write;
- a later personal assertion, actual instrument reading and actual computation from versioned admitted facts, preserving each original interpretation and selecting only the source-appropriate admitted kind through supported paths;
- scoped external verification, inconclusive/disputed verification and two reports sharing one original source, retaining lineage without automatic admission or independent-corroboration claims;
- correction, partial acceptance, restart/replay, unavailable/purged evidence and policy-restricted partial exchange, retaining exact permitted links and exposing no denied content;
- unknown profile, unsupported host fields, serializer field dropping/defaulting and dangling claim/version refs, preventing admission rather than silently flattening qualifiers.

No passing host mapping or schema gap is asserted here. A mapping that cannot preserve this content stays unresolved with a policy-permitted diagnostic. A demonstrated gap needs a separately reviewed versioned schema/type/store chain change and any required pins/migration; this payload contract makes none.

**Bounded host evidence (`UBU-D0323`; 2026-10-12).** These inspected Phase 2 checkout revisions identify evaluated code, not dependency-pin changes. Tracked files were clean. Offline locked builds used rustc `1.99.0 (b940084d7 2026-09-28)`; the core lockfile supplies serde `1.0.229` and serde_json `1.0.152`, and the schema validator uses jsonschema `0.46.5`.

| Host path | Revision and observed carrier or limitation | Evidence scope |
|---|---|---|
| Candidate schema | `ubu-schemas@070d0a6f9a6ad7833dd523042925008316ce923b`, `schemas/core/advisory-candidate.schema.json`: `#/$defs/payload/oneOf/0/properties/value` accepts JSON; `#/properties/field_provenance/additionalProperties` requires strings. | Actual `tools/validate-fixtures` execution on the probes below. |
| Candidate Rust/JSON | `ubu-core@fa26bf678977f5e077229eb97a39cc1b8b960c9b`, `src/advisory_candidate.rs`: `CandidatePayload::Inline(serde_json::Value)` and `AdvisoryCandidate.field_provenance: BTreeMap<String, String>`. Its `schemas-ref` is the schema revision above. | Actual `AdvisoryCandidate` deserialization, `validate`, serialization, deserialization and reserialization; structured output comparison. |
| Candidate store/reload | `ubu-store@83a099f55220b1286ffb2dee3c9d19006e2090b5`, `src/candidates.rs::store_advisory_candidate` stores the serialized candidate in `advisory_candidates.payload_json`; `src/models/candidate_record.rs::CandidateRecord::candidate` deserializes it. | Source inspection only; no persistence/restart/replay fixture. |
| Normalization/review API | `ubu-orchestrator@9fddef16990ac89d07a168c75bf04b77c35b9bc3`, `src/api/advisory.rs::candidate_response` wraps stored JSON; `src/services/precondition_review.rs::vet_result` rebuilds accepted candidates' inline payload from `normalized_proposal`. | Source inspection only; added payload content cannot be assumed to survive that normalization path. |
| Review presentation | `ubu-ui@dda2312fd17a5dc330fef102f9def30ab88ac5bf`, `src/routes/Review.tsx`: the generic card presents confidence, evidence refs and normalized proposal without rendering `candidate.payload` qualifiers. | Source inspection only; API retention does not establish review fidelity. |

**Reproducible carrier specimen.** Start from `fixtures/valid/core/advisory-candidate/proposed-precondition.json` at the schema revision above, with its `schema_version: "1.0"` and native proposal unchanged. Let `P` be:

```json
{"profile":"perceptual-qualifiers/1","field_qualifiers":[{"field_path":["preconditions"],"claim_refs":[{"evidence_ref":"synthetic:description","evidence_version":"v1","claim_key":"charge"}]}]}
```

| Probe input relative to that fixture | Canonical schema validator | Actual core validation/JSON outcome |
|---|---|---|
| Unchanged baseline | Accepted. | Whole structured candidate unchanged after round trip. |
| Add `payload.value.perceptual_qualifiers = P` | Accepted. | Whole structured candidate and `P` unchanged after round trip. |
| Replace `field_provenance.preconditions` with the object `P` | Rejected. | Rejected: `invalid type: map, expected a string`. |
| Inline case with only `P.profile` changed to `perceptual-qualifiers/unknown` | Accepted by the generic carrier schema. | Accepted and preserved by the generic type layer; required adapter rejection remains untested. |

These are candidate link-carrier probes with unresolved synthetic refs, not the full qualifier/admission fixtures above. No producer/adapter, Discovery claim/evidence resolution, normalization, store/review, admission or authorized-exchange fixture was run; no policy-authorized exchange revision/configuration or complete host mapping is established. Unknown profiles still must remain unresolved before admission. The rejected direct field choice does not establish a schema gap when an inline representation remains available for testing. Report any failure at its pinned host stage with exact expected/retrieved content; distinguish mapping/implementation defects from a minimally reproduced host-contract restriction that a compliant existing payload/ref mapping cannot satisfy. `UBU-Q0166` stays Open for the remaining evidence, with no adapter enabled by these results.

**Normalization and persistence follow-up (`UBU-D0334`; 2026-10-13).** Reused the schema, core, store and orchestrator checkout revisions pinned in `UBU-D0323`; tracked files remained clean. A temporary synthetic harness called the actual public functions, linked to orchestrator libraries verified by `cargo build --locked --offline --lib`. The canonical validator was verified by `cargo build --locked --offline -p validate-fixtures`. Both builds used the rustc version above; this harness uses the orchestrator lockfile's serde `1.0.228`, serde_json `1.0.150`, sqlx `0.8.6` and tokio `1.52.3`, rather than the earlier standalone core probe's dependency versions. The schema tool still uses jsonschema `0.46.5`. No dependency pins changed.

**Minimal failing specimen.** Start from the same pinned `proposed-precondition.json` fixture used above. Remove its candidate-level `confidence`, retain all other host fields, and set both `normalized_proposal` and `payload.value` to `N`:

```json
{"operation":"clear_precondition","verdict":"remove","reason":"Synthetic carrier probe.","existing_precondition":{"target":"numeric_values.synthetic.orbital_teapot_charge","predicate":"at_least","expected":25},"blocked_now":false}
```

The baseline has no companion payload. The inline case adds the exact `P` above at `payload.value.perceptual_qualifiers`; the unknown-profile case changes only `P.profile` to `perceptual-qualifiers/unknown`. A fourth case copies `P` into `normalized_proposal.perceptual_qualifiers` as well. These remain synthetic candidate link specimens, without resolved evidence or complete claim qualifications.

For normalization, initialize an in-memory `AppState` with a fixed clock at `2026-09-19T09:00:00Z`, and admit only the fixture's target Task at version 1: title and description `Synthetic carrier probe.`, status `active`, preconditions `N.existing_precondition`, and ordinary user provenance. Author no UniverseState; the host supplies its default current view. Supply `precondition_review::Context` with `force: false`, one matching `ReviewTask` (the same description/tree, `existing_words` from `words`, null prior rejection reason), and empty `targets`. Obtain `submission(state, context, "synthetic-carrier-probe")`; construct an `ok` `LocalAdvisoryResult` carrying one specimen and the submission's authority/provider/policy/digest fields, with empty artifacts/diagnostics/telemetry and no deletion confirmation. Call the actual `precondition_review::vet_result` directly, without a producer/model call or enqueue/admission.

| Probe/path | Actual outcome and exact comparison |
|---|---|
| Canonical schema validation | Baseline, inline and unknown-profile cases accepted; the fourth case rejected by the closed native `normalized_proposal`. A structured `field_provenance.preconditions = P` control also rejected. `tools/validate-fixtures` reported three valid and two invalid fixtures with their expected outcomes. |
| Core Rust/JSON | All four cases deserialized, validated and retained the whole structured candidate through serialize/deserialize/reserialize. The fourth case's core acceptance does not override its schema rejection. |
| Baseline through `precondition_review::vet_result` | Status `ok`, one candidate, empty diagnostics. The host recomputed `blocked_now` to `true`, set `payload.value = normalized_proposal`, and set `suppression_key` to `review:sha256:57ea73a474832a19855231c2d99b66a4ed30b1eeccc4721fe20933e462ebee96`. Other structured fields were unchanged. |
| Inline and unknown-profile cases through the same function | The same native result as the baseline: `payload.value` is exactly `N` with `blocked_now: true`; the `perceptual_qualifiers` member is absent, rather than null or preserved. Status remains `ok`, one candidate, empty diagnostics. The expected retained member was exactly `P` (or its unknown-profile variant). The first observed loss on this tested route is `src/services/precondition_review.rs::vet_result` assigning `CandidatePayload::Inline(c.normalized_proposal.clone())`. |
| Fourth case through the same function | Status `malformed_result`, zero candidates, diagnostic code `advisory_malformed_result` and message `The model response was not a valid precondition review; no candidates were enqueued`. Adding a companion member to the closed native envelope is not a compliant workaround. |
| Direct store/reload control, bypassing normalization | Store the inline case with `store_advisory_candidate` in a temporary SQLite file using one ordinary user envelope. Close the pool, reconnect with `UbuStore::connect`, read via `get_advisory_candidate` and `CandidateRecord::candidate`, then retry the identical envelope/candidate. Each retrieved structured candidate equals the complete input, including `P`; one candidate and one mutation-ledger row, zero canonical objects and Logs. Changed payload with the same envelope key rejects with `idempotency_key_conflict`. This is connection close/reopen and replay, not process/crash restart. |
| Direct review API controls, bypassing normalization | Store the same inline case in the in-memory state; call `api::advisory::candidate` and `api::advisory::queue`. Both return the complete input candidate unchanged with `state_category: "candidate_state"`. These are handler calls, not authenticated HTTP or UI presentation tests. |

All normalization cases left the mutation ledger and every non-candidate table unchanged; the direct store/API controls also left canonical tables unchanged. The measured loss is specific to the tested normalization branch and mapping. The inline carrier remains schema-valid and survives the direct storage controls, so this does not establish a host-contract restriction that no compliant existing payload/ref mapping can satisfy. A future adapter using this branch must preserve the companion content and bound refs, or keep the interpretation unresolved before executable review; a native `ok` result cannot certify fidelity.

Complete Discovery claims/qualifications and resolved evidence revisions, required adapter profile/ref rejection, rendered review, admission, process/crash recovery, retention/partial acceptance and policy-authorized exchange remain untested. No complete mapping, passing adapter or actual schema gap is established; `UBU-Q0166` stays Open. Only a separately reproduced governing host-contract gap warrants the versioned chain review specified by `UBU-D0321`.

**Supported operations.** Validate the native proposal against an already-supported candidate kind and ordinary admission operation before enqueueing an executable review proposal. Unresolved targets, unsupported object kinds and perceptual values without a supported adapter remain Discovery review evidence with the limitation visible; they cannot fall through to a generic mutation executor. `UniverseTarget` advice remains name-only under `UBU-D0296`: it cannot carry, default or admit a perceptual value. The operator may author a value through the ordinary assertion/measurement form linked to the evidence. Future perceptual value-candidate support requires its separately reviewed versioned adapter; this contract does not enable it. Intent, affect, Preferences, completion/effect claims and later-phase Resources/Skills/Techniques retain their existing source and admission gates.

**Disclosure and session scope.** Evidence, candidates, alternatives, selectors, hashes and derivative review material inherit all supporting sources' disclosure/routing/retention restrictions; reference-only payloads do not waive them. Denied matches cannot leak identifiers, labels or existence through alternatives or counts. Reviewing permitted retained evidence is allowed after its capture session is paused/ended; it neither restarts capture nor transfers source consent or pending approval to another Device.

**Replay and corrections.** The controller assigns stable evidence ids and capture keys scoped to source/session and source item or span. It assigns separate candidate-creation and review-operation idempotency keys, each bound to its immutable originating operation rather than later lifecycle state. Exact retries, restart and exchange preserve ids, versions, normalized content and keys; the same key with different bound content is rejected, never treated as a harmless duplicate. Distinct observations are not deduplicated by their meaning or time alone. Equivalent repeated proposals use §21.2.1's suppression/deferral rules rather than resetting rejected/deferred state. New material evidence or a correction creates a linked revision/replacement with a new key. Superseded versions and late interpretations cannot overwrite an operator correction or use an old approval. Corrections to admitted claims append §17.3 entries and any separately admitted replacement Snapshot/mutation; they never rewrite the original. Raw-media purge marks supporting evidence unavailable and obeys existing retention policy; it neither fabricates replacement evidence nor automatically retracts or reaffirms a claim.

**Review and admission.** Present the source/session and observation time, permitted evidence or an explicit unavailable/redacted indicator, proposed-versus-current diff, source kind, uncertainty/contradictions, alternative matches, dependencies and resulting planning implications. Show the exact fields and batch being accepted; accepting an evidence item alone does not authorize every linked proposal. Offer resolve/edit, accept, reject, defer and private/unknown through existing Discovery/advisory review; edits record the operator's source, create a new bound revision and require fresh review. A permitted partial acceptance creates a bound revision or separate candidate for its exact subset and coherent dependency batch; unaccepted interpretations remain unresolved. If required evidence or target bindings cannot be established, retain an unresolved review item. Restricted attention defers unavailable review under §21.1.2.

At acceptance, the CPU rechecks the exact candidate/target/dependency versions and evidence/context/source revisions, including whether the latter remain available and unsuperseded, dependency bindings, current authority and Device trust, current source/disclosure/admission policy and native semantic/conflict rules. Changed bindings invalidate approval; require fresh review rather than silently rebasing. Source confirmation records new evidence of the actual assertion, reading or derivation; it does not relabel the original inference as measured or derived. Commit the ordinary canonical change, review decision with actor/`authority_source`/Device/times/observed versions, resulting Log/Snapshot refs, candidate admission links and idempotency outcome as one crash-consistent unit. Rejection/deferral records only ordinary review metadata, not the proposed world state. Recalculation follows admitted planner-relevant changes. Candidate persistence, successful extraction, high confidence and queue order confer no canonical, projection or external-action authority; external execution requires its own current checks and approval.

**Consequence-sensitive corroboration (`UBU-D0324`).** Assess each proposed claim against its intended planning/execution use, including downstream effects if it is wrong. The operator reviews the consequence class and intended scope. Review records the exact target, value, effective time, proposed diff, affected preconditions/capacity/commitments, credible error consequences, reversibility and uncertainty. Bound ordinary use by explicit operator cost/reversibility limits where relevant. Material financial or external commitments, safety-sensitive decisions, disclosure, irreversible changes, consequential identity/ownership attribution, and claims that release capacity or unlock dependent execution require consequential review. Unknown consequence or an unbounded downstream use takes that review path; model confidence cannot establish that a use is ordinary. No automatic confidence cutoff is selected.

| Intended use | Minimum evidence and human review |
|---|---|
| Clarification or labelled hypothesis | Permitted evidence may order questions or explain alternatives without admission. It cannot satisfy a precondition, release capacity, establish completion/effects, certify a Plan or dispatch an action. An operator may separately admit a verification Task without affirming the disputed claim. |
| Bounded, reversible ordinary planning change | Show the exact claim, source, uncertainty and proposed change; obtain explicit operator confirmation and ordinary version-bound admission. One actual operator assertion can suffice; no second source is required merely to clarify a low-consequence detail. Confirmation is separately attributed evidence under `UBU-D0321`, even when collected in the same interaction as acceptance. |
| Consequential planning or execution use | Each decision-critical perceptual claim needs at least one relevant corroborating source established independently of the original interpretation, plus explicit operator review of sufficiency and the resulting change. Check identity, time, scope, method, source reliability and limitations; resolve decision-relevant contradictions and alternatives. Further verification or qualified review required by the applicable workflow still applies. A corroborating source is necessary, never automatically sufficient. |

Independent corroboration requires an identifiable acquisition or knowledge basis that does not depend on the original model interpretation or merely copy its source. A direct operator inspection, independently obtained source record or actual instrument reading may qualify for the exact property checked; another Device or model is not required. Approving the same image, repeated frames without a distinct verification basis, another model interpreting the same input, copied reports, vote counts, high confidence and downstream summaries/Plans supply no independent corroboration. Operator confirmation states what the person actually knows and how; simple acceptance of model prose supplies no new source. Unknown independence is not counted as independent, and an independent but stale, irrelevant or inconclusive report does not satisfy the claim.

**Unresolved evidence and sovereign authoring.** Missing, stale, policy-withheld, incompatible or insufficient evidence keeps the affected candidate/use unresolved. Show only permitted uncertainty and offer clarify, obtain evidence, edit/narrow, defer, reject or a verification Task. Do not average contradictions, infer false/free/completed from absence, or silently choose a source. Unrelated supported fields may proceed only as a separately reviewed coherent subset. An authorized operator may separately author an assertion, correction or supported explicit planning assumption through ordinary admission, recording the actual source and remaining uncertainty; this does not relabel the visual inference as corroborated or waive native semantics, required workflow evidence, authority or privacy rules. Unsupported assumption/value paths remain evidence-only.

**Assessment bindings and later use.** Retain the reviewed use/scope, consequences, supporting and conflicting claim refs/versions, independence basis, unresolved issues and sufficiency rationale with existing candidate/evidence payloads and ordinary review/Log links; preserve `FactProvenance` as `{kind, recorded_at}`. Apply the check in host admission and Plan-input/execution preparation. Ordinary acceptance is not approval for a later consequential use. A changed use, material new contradiction, source correction/unavailability, or changed candidate/target/context/policy binding invalidates the applicable assessment and approval before affected Plan reuse or execution. Use the ordinary append-only correction and conflict paths; invalidate affected Plans and recalculate from admitted corrections without rewriting history or letting late model output undo an operator correction. An adapter unable to retain or enforce these links remains disabled for that use under `UBU-Q0166`.

Evidence review/admission neither grants capture/export capability nor approves an external action. Immediately before execution, recheck current admitted inputs, sufficiency for that operation, Plan legitimacy, authority, Device trust, Compartment/export policy and the operation's own scoped approval. A review may present both decisions, but must record and bind them separately. Apparent completion remains evidence-only until §17.10's lifecycle/effect evidence requirements under `UBU-D0329` are met.

**Required fixtures.** Before enabling a producer/adapter, pin its revision, host schemas and policy configuration, and serialize the following scenarios through the real validators, review storage and admission reducer. Fixture labels below denote distinct stable schema-valid ids. Record exact input payloads/versions, decisions, candidate/evidence links, expected canonical diffs/Logs and replay outcomes. These are required acceptance cases, not passing implementation results.

| Fixture input and action | Required outcome |
|---|---|
| Evidence `E1@v1`, session `S1@v1`, frame selector `frame:12`, `proposal: null`; evidence payload contains a visual hypothesis `{"operation":"set_fact","target":"facts.project.ubu.build_result","payload":"passed"}`; no value adapter or confirmation | Discovery review evidence only; no UniverseState, Snapshot, Task, Plan, Calendar or projection effect. Existing UniverseTarget advice still contains no value. |
| Supported precondition proposal `C1@v1` for `T1@v7` (prior precondition absent) uses `E1@v1`, context `B1`, policy `P1@v3` and a recorded target; capture session S1 has ended. Proposes `{"target":"facts.project.ubu.build_result","predicate":"equals","expected":"passed"}`, operator edits expected to `"reviewed"` and accepts the bound revision | Only T1's precondition becomes that leaf with expected `"reviewed"`; the fact's value is unchanged. Ordinary decision/Log links are admitted once. Review restarts no capture; repeat submission, restart and exchange produce no second effect. |
| Observation key `person:1` has permitted alternatives `I1@v2` and `I2@v4`, selection null, confidence 0.99; operator leaves unresolved or selects `I2` | Unresolved creates no merge/ownership/fact; selection affects only the newly reviewed proposal bound to `I2@v4`, without accepting other interpretations. |
| A proposal references proposed-object key `o1` from `C2@v1`, or a value from `C3@v1`; the dependency is pending, rejected, or admitted | No phantom object, dangling value link or planner use of a candidate value. Pending/rejected dependency prevents dependent admission; an approved atomic dependency batch or fresh review against admitted refs/values resolves it exactly once. |
| Between presentation and acceptance, `T1@v7` becomes `v8`, evidence/context is superseded, policy changes, or the originating/acting Device is revoked | No stale/unauthorized admission. Preserve only permitted review material and require the ordinary fresh-review, rejection or conflict path. |
| Duplicate `E1/C1` with identical keys/content; then the same key with changed value/target; then a genuinely new observation `E2` | Exact replay collapses; changed-content key reuse rejects; `E2` remains distinct and cannot automatically revive a rejection or satisfy corroboration by repetition. |
| Crash before or after the canonical-change/review/idempotency transaction; retry the same approval operation | Either no effect or the complete single admitted effect and its linked review/Log/outcome; never an orphan mutation, false admission acknowledgment or duplicate effect. |
| Operator corrects `E1/C1` before a delayed result arrives, and separately corrects an already-admitted claim | Late result cannot restore the superseded claim. Unadmitted replacement needs fresh review; admitted correction appends links and any accepted replacement without editing historical records. |
| Supporting raw evidence is purged, or a partial Replica lacks/denies a required source/target; manual text replaces capture | Show only permitted unavailable/incomplete metadata and no denied identifiers or Compartment labels. Missing bindings block that admission; a new operator-authored submission uses its actual source and fresh bindings without recreating deleted media or widening disclosure. |
| Missing required binding, unknown profile/schema, model-invented ref, malformed native proposal or hard routing/Compartment denial | No dispatch/admission; only policy-permitted evidence and an unresolved/rejected diagnostic through existing machinery. |
| Explicit operator assertion, instrument reading and actual derivation through supported ordinary authoring/computation paths; high-confidence apparent Task completion without the required effect evidence | Admitted kinds follow `UBU-D0293` with unchanged FactProvenance fields and retained original candidate links; unsupported derivation paths remain evidence-only. Apparent completion alone creates no lifecycle/effect update, inferred affect/intent or Preference. |

**Corroboration acceptance cases.** In addition to the fixtures above, pin the host admission, Plan-input and execution paths and demonstrate the following. These are enablement gates, not reported passing results.

| Case | Required outcome |
|---|---|
| A bounded ordinary edit with an explicit operator assertion; the same claim later supports a material commitment | Ordinary review may admit the first use without a second source. The later use requires a new consequential assessment and scoped review; prior acceptance is insufficient. |
| Consequential inference with high confidence, two models using one frame, copied reports or unknown lineage; then an independently obtained matching reading/inspection | The first set cannot satisfy independent corroboration. The new evidence preserves ancestry and may support a freshly bound review, without automatic admission or execution. |
| Independent but stale/inconclusive evidence, contradictory target/value/time, denied evidence or unresolved identity | The affected claim/use remains unresolved with policy-safe alternatives; no source voting, capacity release, precondition satisfaction or dependent dispatch. |
| New contradiction or operator correction after acceptance, changed use/versions/policy, and delayed model output | The old assessment/approval cannot authorize affected Plan reuse or execution; admitted corrections append history and trigger recalculation, and the late result cannot overwrite them. |
| Sufficient evidence and admitted candidate, but no external-action approval or a revoked grant; apparent completion without effect evidence | No external dispatch or inferred completion/effect. Evidence sufficiency cannot replace action authorization or §17.10's completion/effect requirements. |

`UBU-D0321` specifies qualifier content and fidelity gates; `UBU-D0324` specifies consequence-sensitive corroboration and review; `UBU-Q0166` retains revision-pinned host-mapping evidence and demonstrated schema-gap review. `UBU-D0326` specifies segmentation and scoped local analysis in §21.1.5; `UBU-D0327` and `UBU-D0328` specify raw-media retention and operator controls in §12.2; `UBU-D0329` specifies completion/effect evidence in §17.10. This contract freezes bindings, reuse, corroboration and admission behavior without claiming the remaining questions or implementation acceptance are complete.

#### 21.1.2 Restricted-attention audio envelope

**Embodied Audio Interaction (`UBU-D0309`).** Activity mode is separate from the realtime session's source/routing mode and from `compartment_ids`: for example, `Mode: Driving` with `Compartment: UbU Project / Public Outreach`. Activity constrains interaction; Compartment policy constrains disclosure and authority. Neither grants permission supplied by the other.

The initial activity vocabulary covers driving, bicycling, walking, running (including jogging), stationary exercise, resting, cooking, housework, showering, passenger transit, user-defined and unknown contexts. Operator selection establishes the intended mode. Explicitly enabled coarse activity evidence may tighten restrictions, but never starts capture or relaxes them. Driving, bicycling and conflicting or uncertain high-risk activity use the high-risk envelope; stopping motion alone does not establish parked review readiness.

A controller-owned, session-local `InteractionBudget` records:

- budget id and version, session and Device refs, activity mode, source/freshness of context, applicable policy refs, `compartment_ids` and expiry;
- permitted input/output channels and audio endpoint, manual availability, visual attention, locomotion risk, interruptibility, working-memory budget, intellectual/emotional bandwidth and social-risk tolerance;
- finite maximum prompt and response lengths, turn duration, choices and clarification count, permitted operation classes, confirmation policy and deferral policy.

Capacity and affect are user-declared or unknown, never a model-certified assessment of intelligence or readiness. Unknown capacity uses the conservative profile; observations may reduce the budget, not expand permissions. Effective permissions are the intersection of the reviewed activity profile, current channel/attention limits, Compartment policy and scoped capability grants. Missing, expired or conflicting authority prevents model/tool dispatch; pause, stop and silence controls remain available. Numeric profile defaults remain in `UBU-Q0128`; the exact initial-envelope serialization is specified below (`UBU-D0311`). A profile cannot ship without finite caps.

**Serialized budget (`UBU-D0311`).** `InteractionBudgetV1` is the controller's immutable snapshot for the initial envelope, not an LLM result, capability grant, capture consent or portable approval. All keys are required and all objects are closed. Reject duplicate keys, trailing content, non-finite numbers and unknown schema versions without repair. References identify controller artifacts or existing records; referenced versions use opaque revision tokens such as `v12`, preserving existing version conventions. `budget_version` and `interaction_epoch` are session-local integer counters. The model receives only a purpose-limited projection of effective limits through its ContextBundle, never authority to edit or return this object.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object", "additionalProperties": false,
  "required": ["schema_version", "budget_id", "budget_version", "session_ref", "device_ref", "interaction_epoch", "profile", "activity_mode", "context", "compartment_ids", "policy_versions", "grant_versions", "input_channels", "output_channels", "audio_endpoint", "readiness_declaration", "attention", "limits", "allowed_operations", "confirmation_policy", "deferral_policy", "issued_at_ms", "expires_at_ms"],
  "properties": {
    "schema_version": {"const": 1},
    "budget_id": {"$ref": "#/$defs/ref"},
    "budget_version": {"type": "integer", "minimum": 1, "maximum": 9007199254740991},
    "session_ref": {"$ref": "#/$defs/ref"},
    "device_ref": {"$ref": "#/$defs/ref"},
    "interaction_epoch": {"$ref": "#/$defs/uint"},
    "profile": {"$ref": "#/$defs/versioned_ref"},
    "activity_mode": {"enum": ["driving", "bicycling", "walking", "running", "stationary_exercise", "resting", "cooking", "housework", "showering", "passenger_transit", "user_defined", "unknown"]},
    "context": {
      "type": "object", "additionalProperties": false,
      "required": ["source", "evidence_versions", "observed_at_ms", "expires_at_ms"],
      "properties": {
        "source": {"enum": ["operator_selected", "operator_and_coarse_evidence", "coarse_evidence", "unknown"]},
        "evidence_versions": {"$ref": "#/$defs/ref_set"},
        "observed_at_ms": {"$ref": "#/$defs/uint"},
        "expires_at_ms": {"$ref": "#/$defs/uint"}
      }
    },
    "compartment_ids": {"type": "array", "maxItems": 32, "uniqueItems": true, "items": {"$ref": "#/$defs/ref"}},
    "policy_versions": {"$ref": "#/$defs/ref_set", "minItems": 1},
    "grant_versions": {"$ref": "#/$defs/ref_set"},
    "input_channels": {"type": "array", "minItems": 1, "maxItems": 2, "uniqueItems": true, "items": {"enum": ["speech", "local_controls"]}},
    "output_channels": {"type": "array", "maxItems": 1, "uniqueItems": true, "items": {"const": "speech"}},
    "audio_endpoint": {"oneOf": [{"type": "null"}, {"$ref": "#/$defs/versioned_ref"}]},
    "readiness_declaration": {"oneOf": [{"type": "null"}, {"$ref": "#/$defs/versioned_ref"}]},
    "attention": {
      "type": "object", "additionalProperties": false,
      "required": ["manual_availability", "visual_attention", "locomotion_risk", "interruptibility", "working_memory", "intellectual_bandwidth", "emotional_bandwidth", "social_risk_tolerance"],
      "properties": {
        "manual_availability": {"enum": ["none", "limited", "available", "unknown"]},
        "visual_attention": {"enum": ["none", "glance", "available", "unknown"]},
        "locomotion_risk": {"enum": ["low", "high", "unknown"]},
        "interruptibility": {"enum": ["none", "brief", "available", "unknown"]},
        "working_memory": {"$ref": "#/$defs/capacity"},
        "intellectual_bandwidth": {"$ref": "#/$defs/capacity"},
        "emotional_bandwidth": {"$ref": "#/$defs/capacity"},
        "social_risk_tolerance": {"$ref": "#/$defs/capacity"}
      }
    },
    "limits": {
      "type": "object", "additionalProperties": false,
      "required": ["max_capture_ms", "max_stt_utf8_bytes", "max_model_input_utf8_bytes", "max_model_output_utf8_bytes", "max_spoken_prompt_utf8_bytes", "max_spoken_response_utf8_bytes", "max_speech_ms", "max_turn_ms", "max_choices", "max_clarifications"],
      "properties": {
        "max_capture_ms": {"$ref": "#/$defs/uint"},
        "max_stt_utf8_bytes": {"$ref": "#/$defs/uint"},
        "max_model_input_utf8_bytes": {"$ref": "#/$defs/uint"},
        "max_model_output_utf8_bytes": {"$ref": "#/$defs/uint"},
        "max_spoken_prompt_utf8_bytes": {"$ref": "#/$defs/uint"},
        "max_spoken_response_utf8_bytes": {"$ref": "#/$defs/uint"},
        "max_speech_ms": {"$ref": "#/$defs/uint"},
        "max_turn_ms": {"$ref": "#/$defs/uint"},
        "max_choices": {"$ref": "#/$defs/uint"},
        "max_clarifications": {"$ref": "#/$defs/uint"}
      }
    },
    "allowed_operations": {"type": "array", "minItems": 3, "maxItems": 8, "uniqueItems": true, "items": {"enum": ["capture_candidate", "session_pause", "session_stop", "session_silence", "media_pause", "media_resume", "media_skip", "media_rewind"]}},
    "confirmation_policy": {"const": "candidate_review_only"},
    "deferral_policy": {"const": "ordinary_review_no_auto_dispatch"},
    "issued_at_ms": {"$ref": "#/$defs/uint"},
    "expires_at_ms": {"$ref": "#/$defs/uint"}
  },
  "$defs": {
    "ref": {"type": "string", "minLength": 1, "maxLength": 128},
    "uint": {"type": "integer", "minimum": 0, "maximum": 9007199254740991},
    "capacity": {"enum": ["none", "low", "available", "unknown"]},
    "versioned_ref": {
      "type": "object", "additionalProperties": false, "required": ["ref", "version"],
      "properties": {"ref": {"$ref": "#/$defs/ref"}, "version": {"$ref": "#/$defs/ref"}}
    },
    "ref_set": {"type": "array", "maxItems": 32, "uniqueItems": true, "items": {"$ref": "#/$defs/versioned_ref"}}
  }
}
```

**Budget semantics.** JSON shape validation is followed by these controller checks; no missing value is defaulted into permission:

- The budget must be the current controller-issued id/version for this session, Device and interaction epoch. Referenced profile, context evidence, policies, grants and endpoint revisions must match current permitted records; each ref set has at most one version per ref. Include every policy/grant used to determine this envelope; metadata overflow fails closed rather than omitting restrictions. Context sources and readiness declarations must match the kind of evidence referenced. `user_defined` requires an explicitly reviewed profile, rather than a free-form label selecting permissions. Coarse evidence may only maintain or tighten the operator-selected mode. Unknown or conflicting activity cannot relax the high-risk envelope.
- Times are integer milliseconds since the Unix epoch. Require `context.observed_at_ms <= issued_at_ms <= now`, and `now < min(expires_at_ms, context.expires_at_ms)`; profile, policy and grant expiry further shorten validity. Bind the remaining lifetime and turn deadline to the local monotonic clock. Clock discontinuity or restart invalidates the snapshot; wall-clock rollback cannot extend it. A new Device recomputes a budget under its own current policy.
- Attention fields are engagement bounds, not diagnostic scores. Known working-memory, intellectual/emotional bandwidth or social-risk tolerance requires the referenced, fresh user declaration; otherwise those fields are `unknown` and the conservative profile supplies finite limits. Profiles may tighten declared limits, never infer improved capacity. Driving, bicycling, high/unknown locomotion risk or conflicting high-risk evidence requires the high-risk envelope, `manual_availability = none`, `visual_attention = none`, `max_choices = 0` and `max_clarifications <= 1`. Zero choices prohibits menus, not one permitted brief reference clarification.
- Every limit is an explicit nonnegative integer; zero disables the corresponding resource. Positive values must be within the reviewed profile and current policy, channel and grant ceilings. Quantitative profile values remain in `UBU-Q0128`; absent ceilings prevent dispatch. These wire bounds are not usability defaults or evidence of driving safety.
- Counters belong to the original user turn, outside the model result. `max_capture_ms` and `max_stt_utf8_bytes` bound total authorized user audio and STT text for that turn. Model byte caps cover the complete normalized invocation (instructions, context, grammar and tool declarations) and complete outputs, cumulatively across that turn; apply the output cap before parsing. Spoken prompt/response byte caps and `max_speech_ms` cover cumulative controller prompts/acknowledgments, measured in UTF-8 bytes and elapsed speech time. `max_turn_ms` runs from the first user audio frame through final speech/dispatch, including waits and clarification; renewal or clarification never resets counters or revives an invalid binding. Exceeding a cap stops that processing/output path and leaves only already-authorized unresolved evidence for review, without truncating text into an executable interpretation or retrying automatically.
- `local_controls` and all three session-control operations are mandatory; immediate local pause/stop/silence remains available even without a valid budget. Speech input still needs the session's explicit source consent. Speech output requires a non-null fresh endpoint, nonzero speech limits and current disclosure permission; a known endpoint is not proof of privacy. Empty output channels mean silence. Zero model limits prevent model invocation; zero clarification allowance removes that branch from `AudioInterpretationV1`.
- `allowed_operations` is only a cached intersection of the initial envelope and current authority, rechecked at dispatch. `capture_candidate` maps to either existing capture variant; session and media names map to their exact v1 operations. Media requires the scoped integration grant and fresh request-bound playback target. `candidate_review_only` leaves capture unadmitted while permitted session/media controls need no admission; `ordinary_review_no_auto_dispatch` retains unsupported or prohibited requests for fresh ordinary review. No operation admits canonical state or authorizes consequential external execution.

Issue a superseding budget/version and invalidate request bindings on material context, mode, Device, endpoint, profile, policy or grant change, or on expiry. Synchronized/audit copies cannot restore a live budget, microphone consent or pending approval. Before enabling this contract, exercise missing/extra fields, non-integer/negative limits, stale revisions, replay after restart/handoff, clock rollback, missing endpoint permission, unknown readiness and high-risk menu/clarification violations. This specifies the serialization and enforcement boundary; numeric profiles, richer actions and device-specific tuning remain open.

**Initial permissions.** Every restricted-attention mode starts with user-initiated local candidate capture, a bounded acknowledgment, pause/stop/silence, and optional explicitly granted media transport (`pause`, `resume`, `skip`, `rewind`) against one fresh, unambiguous playback target. Capture produces existing evidence/candidate records, including Task proposals; it does not create an admitted Task, Snapshot, Preference or Calendar change. Media transport uses the current integration grant and policy checks; it never authorizes sharing, purchases or broader app access. Richer read-only and short-drafting profiles remain open.

Driving, bicycling and uncertain high-risk contexts additionally prohibit generated drafting, reflective/coaching dialogue, spoken menus and visually dependent review. Permit at most one brief clarification per request; an unresolved target or unanswered clarification leads to capture/deferral without retry prompts. External sends, financial actions, legal commitments, deletion, multi-party communication and scheduling changes are deferred. Neither a capability grant, a generic “yes”, an action-specific spoken confirmation nor a model's `safe_for_mode` claim overrides this envelope. UbU may remain silent or suggest user-selected existing media/rest; it must not repeatedly solicit work or queue review prompts while attention is restricted.

**Controller boundary.** Consented local STT yields evidence, then the harness assembles a purpose-limited `ContextBundle` under §21.3 and requests an interpretation/candidate through §21.2. Include only the permitted current referents, relevant admitted state and scoped playback/context versions needed for that operation. Model confidence, risk estimates and confirmation suggestions are advisory. Deterministic validation checks schema, semantics, current budget, Compartment route, grants, object versions, ambiguity and idempotency before invoking an allowlisted control or creating a review candidate. Recheck immediately before dispatch and speech output; a local processing route alone does not authorize reading private content aloud on a shared audio endpoint. Raw AV stays local under §12.2; an unavailable local backend never creates cloud-fallback authority.

**Playback referents and vague commands (`UBU-D0322`).** For the existing transport envelope, resolve the audio domain and target before offering a playback alias. This is controller-owned request context, not a new canonical object or an expansion of `AudioInterpretationV1`. An explicit target must resolve uniquely within the permitted integration scope. An implicit media reference requires one user-selected playback target in this session, current integration evidence that it remains the target, and no competing UbU speech or other permitted audio source. Foreground-app status, most-recent playback, project relevance and model confidence cannot select a target. Clear implicit referents on interruption, material mode/Compartment or endpoint change, restart or Device handoff; retained/audit copies cannot restore them. If the available permitted evidence cannot establish uniqueness, clarify or defer without inspecting denied apps or revealing their existence.

Bind the selected integration/target, item and queue/control revision, playing/paused state, Device/output endpoint, current grant/policy versions, observation time, finite freshness deadline and the operation's integration-declared step semantics in the existing request binding. A paused target can support explicit resume while its binding remains fresh; selecting it grants no new playback or disclosure authority. Ordinary position advancement within the same item does not change the control revision; item/queue replacement, seek, playback-state change, endpoint switch or loss of reliable state does. Changes before dispatch invalidate the pending interpretation. A dispatched control's confirmed transition records its outcome and invalidates other outstanding bindings; its acknowledgment still rechecks the current budget, endpoint and disclosure permission. Transport must address the bound integration and target with its expected control revision, rather than a global command to whichever player is active at dispatch. An adapter unable to enforce that binding cannot enable transport. Constrain producer branches to the routing decision below and recheck it at dispatch; a model's action label cannot supply a missing target or operation.

| Utterance in the initial envelope | Controller routing |
|---|---|
| Recognized local pause/stop/silence, or `pause that` while UbU is speaking | Take the immediate local session-control path and stop UbU output. Do not pause another app as an inferred second effect. Consented speech input or local controls remain required. |
| `pause that` with UbU silent and exactly one fresh selected media referent | Propose `media_transport/pause` for that alias, subject to the existing budget and dispatch checks. With an unknown or competing referent, use the one permitted target clarification or defer. |
| Explicit `pause/resume/skip/rewind [target]` | Resolve the named target uniquely and bind that request; do not change the session's default target implicitly. Skip/rewind permit only the adapter's declared single bounded step. Unsupported quantities or semantics defer. |
| `go back` | Keep unresolved: a prior track, a seek, UbU readback and UI navigation have different effects. A current player alone does not choose the operation. The v1 clarification vocabulary cannot clarify this operation; defer for ordinary review or a fresh explicit command. |
| `save this` | Retain only the authorized utterance as unresolved capture evidence. Do not bookmark/download media or treat a deictic reference as captured content. A self-contained dictated note may use the existing evidence-bound capture action. |
| `what was that?` or `what did I miss?` | Defer readback, replay and summarization requests; v1 has no such action. Do not substitute a rewind or speak generated content. |
| `remind me about this` | Retain the authorized utterance as unresolved evidence for note/Task review, with its missing referent visible. Do not admit a Task, schedule a reminder or fetch content to fill the gap. |

Target clarification uses only `identify_playback_target` and the remaining turn budget; no spoken menu or generic `yes` selects an app. Its answer must uniquely identify a permitted target. Bind the original command and clarification evidence within the same uninterrupted turn, retaining its original counters/deadline; an interruption cannot resume that exchange. A new explicit request needs fresh evidence and bindings. Partial/uncertain STT or mixed supported/unsupported requests cannot yield media dispatch; retain authorized unresolved evidence. Immediate local pause/stop/silence remains available.

Media apps retain playback/position ownership. UbU records only policy-permitted target/control metadata and confirmed outcomes needed for routing and audit; transport grants do not permit transcript, catalog or library retrieval. Do not issue app transport automatically when UbU speaks, is interrupted or reconnects. An unknown dispatch outcome is reconciled through permitted state reads and audit metadata, never by replaying the transport or claiming success from silence. State reconciliation alone does not prove a relative command succeeded. A subsequent effect requires a fresh user request.

Before enabling this routing slice, pin the adapter/controller and policy revisions and exercise: two players or no reliable target; UbU speech overlapping media; an explicitly named app differing from the selected default; denied-source metadata; paused-target resume; ordinary position advancement versus item/queue/seek changes between interpretation and dispatch; stale/offline state and endpoint/Device handoff; ambiguous `go back`, `save this` and reminder references; and duplicate or unknown skip/rewind outcomes. Require no wrong-target, inferred second or repeated effect, no denied-context leakage, no canonical planning-state change without ordinary admission and no false success acknowledgment. These are acceptance requirements, not passing results. This answers initial-envelope referent routing in `UBU-Q0128` subquestions 9, 14 and 15; richer media operations, numerical profiles and device-specific integration remain open.

**Initial typed interpretation (`UBU-D0310`).** `AudioInterpretationV1` is the closed JSON result for the initial envelope only. All keys are required; `null` or `unknown` represents an unavailable estimate. References are opaque aliases supplied in this request, not model-authored object identifiers. This is the schema template; the controller instantiates a conservative producer grammar under §21.2. Matching it does not establish permission. Apply the budget's finite response-size cap before parsing one complete JSON object; reject duplicate keys, non-finite numbers and trailing content.

```json
{
  "type": "object", "additionalProperties": false,
  "required": ["schema_version", "request_id", "intent", "entities", "confidence", "ambiguity", "cognitive_load_estimate", "action_risk", "proposed_action", "required_confirmation", "clarification_request", "defer_reason", "safe_for_mode"],
  "properties": {
    "schema_version": {"const": 1},
    "request_id": {"$ref": "#/$defs/ref"},
    "intent": {"enum": ["capture", "session_control", "media_transport", "clarify", "defer", "no_op"]},
    "entities": {"type": "array", "maxItems": 4, "items": {"$ref": "#/$defs/entity"}},
    "confidence": {"type": ["number", "null"], "minimum": 0, "maximum": 1},
    "ambiguity": {"type": "array", "maxItems": 6, "uniqueItems": true, "items": {"enum": ["uncertain_stt", "partial_utterance", "unresolved_reference", "multiple_targets", "stale_context", "other"]}},
    "cognitive_load_estimate": {"enum": ["minimal", "low", "moderate", "high", "unknown"]},
    "action_risk": {"enum": ["none", "low", "high", "unknown"]},
    "proposed_action": {"oneOf": [{"type": "null"}, {"$ref": "#/$defs/capture"}, {"$ref": "#/$defs/session_control"}, {"$ref": "#/$defs/media_transport"}]},
    "required_confirmation": {"enum": ["none", "clarification", "ordinary_review"]},
    "clarification_request": {"enum": [null, "repeat_request", "identify_playback_target"]},
    "defer_reason": {"enum": [null, "unsupported_operation", "mode_prohibits", "budget_exceeded", "policy_denied", "ambiguous_input", "stale_context", "user_requested"]},
    "safe_for_mode": {"type": ["boolean", "null"]}
  },
  "allOf": [
    {"oneOf": [
      {"properties":{"intent":{"const":"capture"},"proposed_action":{"$ref":"#/$defs/capture"},"required_confirmation":{"const":"ordinary_review"},"ambiguity":{"maxItems":0},"clarification_request":{"type":"null"},"defer_reason":{"type":"null"}}},
      {"properties":{"intent":{"const":"session_control"},"proposed_action":{"$ref":"#/$defs/session_control"},"required_confirmation":{"const":"none"},"ambiguity":{"maxItems":0},"clarification_request":{"type":"null"},"defer_reason":{"type":"null"}}},
      {"properties":{"intent":{"const":"media_transport"},"proposed_action":{"$ref":"#/$defs/media_transport"},"required_confirmation":{"const":"none"},"ambiguity":{"maxItems":0},"clarification_request":{"type":"null"},"defer_reason":{"type":"null"}}},
      {"properties":{"intent":{"const":"clarify"},"proposed_action":{"type":"null"},"required_confirmation":{"const":"clarification"},"clarification_request":{"not":{"type":"null"}},"defer_reason":{"type":"null"}}},
      {"properties":{"intent":{"const":"defer"},"proposed_action":{"type":"null"},"required_confirmation":{"const":"ordinary_review"},"clarification_request":{"type":"null"},"defer_reason":{"not":{"type":"null"}}}},
      {"properties":{"intent":{"const":"no_op"},"proposed_action":{"type":"null"},"required_confirmation":{"const":"none"},"ambiguity":{"maxItems":0},"clarification_request":{"type":"null"},"defer_reason":{"type":"null"}}}
    ]}
  ],
  "$defs": {
    "ref": {"type": "string", "minLength": 1, "maxLength": 128},
    "entity": {
      "type": "object", "additionalProperties": false, "required": ["role", "context_ref"],
      "properties": {"role": {"enum": ["subject", "playback_target"]}, "context_ref": {"$ref": "#/$defs/ref"}}
    },
    "capture": {
      "type": "object", "additionalProperties": false, "required": ["kind", "candidate_kind", "evidence_span_ref"],
      "properties": {"kind": {"const": "capture_candidate"}, "candidate_kind": {"enum": ["note", "task_candidate"]}, "evidence_span_ref": {"$ref": "#/$defs/ref"}}
    },
    "session_control": {
      "type": "object", "additionalProperties": false, "required": ["kind", "operation"],
      "properties": {"kind": {"const": "session_control"}, "operation": {"enum": ["pause", "stop", "silence"]}}
    },
    "media_transport": {
      "type": "object", "additionalProperties": false, "required": ["kind", "operation", "playback_ref"],
      "properties": {"kind": {"const": "media_transport"}, "operation": {"enum": ["pause", "resume", "skip", "rewind"]}, "playback_ref": {"$ref": "#/$defs/ref"}}
    }
  }
}
```

Before offering the grammar, the controller constrains `request_id` to the invocation's id, replaces each context/evidence/playback reference site with an enum of supplied aliases, and removes action branches or media operations outside the current envelope and grants. An empty alias set forces the related entity list empty or removes the related action branch; no remaining clarification allowance means the `clarify` branch is unavailable. Uncertain or partial STT permits only clarification or deferral, while the already-authorized evidence may remain unresolved. Static intent/action consistency is encoded in the schema; dynamic semantic and policy checks still run at dispatch.

Semantic validation repeats these constraints and checks current context:

- `capture`, `session_control` and `media_transport` require the matching non-null action kind, empty `ambiguity`, null `clarification_request` and null `defer_reason`. Capture requires `ordinary_review`; session and media controls require `none`. These labels never perform admission or confirmation.
- `clarify` requires a non-null `clarification_request`, `required_confirmation = clarification`, and null action and defer reason. `defer` requires a non-null defer reason, `ordinary_review`, and null action and clarification request. `no_op` requires null action, clarification request and defer reason, and `none`. Nonempty ambiguity permits only `clarify` or `defer`.
- Entity aliases must occur in the permitted ContextBundle and match their declared role; they suggest associations without resolving Identity or ownership. Capture must cite a supplied STT evidence span belonging to the current utterance. The controller retains that evidence as an existing unresolved candidate; a model cannot supply replacement note text, admit a Task, or invent evidence. `note` routes to existing quick-note/Discovery evidence and `task_candidate` to existing Task proposals; these routing tags do not extend candidate-type or provenance enums.
- Media transport must name the one fresh playback alias whose integration, target and state version the controller bound to this request. `skip` and `rewind` each mean one integration-declared bounded step; requested quantities or other unsupported arguments defer rather than being silently approximated. The result cannot contain arbitrary tool names, destinations, credentials, message payloads or executable code.

**Request binding.** Outside the producer result, the controller binds each invocation to its request id, session, Device, STT evidence/version, ContextBundle, budget and policy versions, scoped grants, playback target/version when applicable, session interaction epoch, finite deadline and idempotency key. The echoed `request_id` must match that invocation's original binding; it cannot select another request. Interruption or a material mode, Device, endpoint, target, payload, policy or grant change invalidates outstanding bindings and advances the epoch. The controller serializes invalidation, result acceptance and dispatch decisions. Late results cannot speak, dispatch or supply a new clarification; a fresh request needs fresh evidence and current context.

The controller rechecks the binding and operation permissions before both speech and dispatch. Confidence, load, risk, confirmation and `safe_for_mode` are advisory; the controller may tighten or defer, never widen the envelope. Load describes the proposed interaction, never the user's intelligence, affect or capacity. A clarification uses a controller-owned bounded template and consumes the request's remaining clarification allowance. Acknowledgments report only confirmed capture/control outcomes; a capture acknowledgment means that a review artifact was retained, without confirming its content. The model result is never spoken directly. Recognized pause/stop/silence controls take an immediate local path without waiting for an LLM; local stop/silence controls remain available if model processing fails.

Reject malformed, truncated, oversized, unknown-version, extra-field, forged-reference or semantically inconsistent results without repairing them into executable actions. Preserve only already-authorized evidence as an unresolved review item, without automatic model retries or repeated prompts. Duplicate results reuse the original idempotency key and are ignored after the first acceptance, including speech and clarification; an uncertain media outcome requires state reconciliation and a fresh user request, never automatic repetition of a relative skip/rewind.

Before enabling v1, include negative fixtures for unknown action kinds, foreign evidence/playback aliases, inconsistent intent/action pairs, late replies after interruption or handoff, and duplicate/unknown media outcomes. Valid capture stays in candidate state; every consequential external request remains deferred under the initial envelope. This specifies a contract, not a shipped schema or implementation. Numeric mode profiles, external-action confirmation grammar and richer output extensions remain in `UBU-Q0128`.

**Interruption and deferral.** Partial/uncertain STT, ambiguous recipients or “this/that” references, stale media state and false conversational continuity never select an action by confidence alone. Ask at most the budgeted clarification count, then retain an unresolved candidate or defer. Interruption cancels unfinished speech and pending confirmation; it cannot complete an utterance or replay an action. A mode, target, payload, policy or Device change invalidates pending approval. Leaving a restricted mode only makes a deferred item available for ordinary review: it never dispatches it. Any later consequential action needs fresh admission under current authority and confirmation bound to the exact operation, recipient, payload/version and expiry; the ordinary-review single-recipient message slice follows below, while other external-action grammars remain open.

**Deferred message confirmation (`UBU-D0333`).** The controller profile `audio-send-review/1` defines an additional confirmation requirement for an already-permitted, ordinary-attention review of one plain-text message to one recipient. It enables no sending workflow and adds no operation to `AudioInterpretationV1` or `InteractionBudgetV1`. Every external send remains deferred in the initial restricted-attention envelope. Group/multi-party sends, attachments, financial/legal commitments, high-stakes messages and unknown consequences are outside this profile.

Returning to ordinary attention requires explicit operator selection and a fresh eligible review context; stationary motion, reconnection or a Device change is insufficient. Open the deferred item only on user request, with permitted evidence and a newly reviewed recipient and draft. Missing references stay unresolved. The captured request, an accepted Task/note, a draft, a tone suggestion and earlier spoken approval authorize no send.

**Complete review and binding.** Resolve one permitted recipient and exact destination; never select by relationship closeness, recent contact or model confidence. Present the sending Identity/account, recipient's distinguishing label and actual address/number, integration/route and complete final message. An abbreviated readback cannot approve omitted content. Audio-only review requires a revision-pinned rendering that preserves decision-relevant spelling, punctuation, numbers and other literal content; unsupported rendering or content exceeding the finite review budget defers to an eligible ordinary review surface. Do not silently shorten content, split it into unbounded turns or read it on an unauthorized endpoint. Review of the draft and authorization of its external send remain separate decisions.

Before a confirmation prompt, bind the review id/version, actor and authenticated session, Device, interaction epoch, activity/readiness and review-profile versions, input source and output endpoint revisions, recipient ref/version and exact destination, sending account/integration/route versions, immutable complete payload/version, source/context/policy/grant versions, completed review presentation, finite monotonic deadline and the originating send's idempotency key. Store these in the controller's existing review/AgentAction bindings, outside model output. A reviewed Device/profile must supply finite input, readback, prompt, speech and total-review limits; missing limits or bindings prevent the prompt. Budget renewal, replay and restart cannot extend the deadline or complete an interrupted readback.

**English confirmation grammar.** Instantiate exactly one permitted token sequence for the current review:

```text
confirm send to <recipient_phrase> code <digit_word> <digit_word> <digit_word> <digit_word>
```

`recipient_phrase` is the controller-reviewed, unique spoken label for that exact recipient/destination, shown or spoken during complete review; it is not an LLM alias or a way to choose a recipient during confirmation. Each `digit_word` is one of `zero one two three four five six seven eight nine`. The controller samples a fresh four-digit code, does not reuse it within the review session and keeps at most one live challenge; code exhaustion disables this confirmation route. For example, `confirm send to Morgan mobile code four seven two six` matches only the currently bound message. The code associates a reply with a review; it is not actor authentication, a secret capability or proof of comprehension.

The controller generates the expected phrase, never the model or message body. Match the complete final STT token sequence after only ASCII case-folding, collapsing ASCII whitespace and removing one terminal sentence mark (`.`, `?` or `!`). Require spelled-out digit words; do not repair digits, synonyms, approximate names or homophones into a match. This English profile permits no other locale grammar. Generic `yes`, `send it`, a partial phrase, extra instructions, an incorrect code, uncertain STT or an ambiguous speaker/source cannot approve. Input must be consented and explicitly initiated for this same uninterrupted review exchange after presentation/prompt output has ended. Prior recordings/transcripts, TTS echo, media and model/tool output cannot supply confirmation; input ownership must be established independently of phrase matching or this route remains unavailable.

Stop/cancel/silence immediately invalidates the challenge. Interruption, expiry, incomplete presentation or a material change to any bound actor/session, Device, mode, endpoint, recipient, account, route, payload, context, policy or grant invalidates it as well. A new user-requested exchange needs fresh review and a new challenge, with no automatic retry prompt. Matching text records approval evidence only; serialize cancellation, approval consumption and dispatch, recheck all current bindings and authority immediately before egress, and persist the consumed approval and ordinary attempt record before external dispatch. Duplicate callbacks cannot create another attempt. Failed or unknown delivery follows ordinary outcome reconciliation/idempotency rules in §21.8, never blind replay or a success acknowledgment inferred from silence. Fresh approval cannot bypass unresolved delivery reconciliation.

Reuse existing review, evidence, AgentAction and append-only Log links for permitted recipient/payload versions, presentation completion, challenge id/epoch, confirmation evidence, policy checks, invalidation/consumption and delivery outcome. No mandatory raw recording, full transcript, message-body copy or retained plaintext code is added. Synchronized review artifacts carry no live challenge, microphone consent or execution approval.

Before enabling this confirmation route in a separately released sending workflow, pin controller/STT/TTS/adapter, Device/profile, rendering and policy revisions and demonstrate:

- A fully reviewed permitted message and exact fresh phrase yield one bound attempt; generic assent, wrong/partial code, approximate recipient, uncertain speech, TTS/media echo and an unowned source yield none.
- Same-name contacts, changed destination/account/route or payload, incomplete/oversized readback and denied endpoint/context require fresh review or deferral without wrong-recipient disclosure or dispatch.
- Stop/interruption/revocation between presentation, confirmation and egress, expiry, restart/handoff and delayed/duplicate callbacks produce no stale or repeated attempt; unknown delivery remains explicitly unknown and is reconciled before any retry.
- Driving, bicycling and every other initial restricted-attention mode still defer external sends even with a grant and a correctly spoken phrase.

These are required acceptance cases, not passing implementation results or authentication guarantees. This answers the ordinary-review single-recipient send slice of `UBU-Q0128` subquestions 10, 13 and 18. Other external-action grammars, future restricted-mode execution permissions and richer messaging integrations remain open.

Audit records link the session, evidence/candidate, budget and policy versions, ContextBundle, resolved target/version, policy decision and reason, approval/deferral/rejection, invocation/result and idempotency key. They retain minimized summaries or permitted refs, not a mandatory recording or full transcript. Raw-media retention remains under §12.2 and `UBU-Q0170`–`UBU-Q0171`. Device handoff rechecks capture, context, output route and authority; synchronized candidates never carry live microphone consent or pending execution approval.

**Release boundary.** Before enabling a restricted-attention slice, demonstrate that interruption stops output, uncertain references do not dispatch, high-risk sends remain deferred despite confirmation, mode/Device changes do not execute queued work, and offline operation preserves local-only denials. This is a design contract, not an implementation or a claim of validated driving safety. `UBU-Q0183` retains the delivery sequence and device allocation under `UBU-D0292`; richer media/narration and profile tuning remain later answers, without restoring a Phase 3 gate.

#### 21.1.3 MVP interaction coverage and alternatives

**Product contract (`UBU-D0319`).** Realize `UBU-D0292` through a common planning/review surface: showing a concrete concern and speaking about it produces a reviewable proposal or an explicit unresolved result with a useful next step. A camera preview, transcript or conversation alone does not satisfy delivery. Text and forms are directly reachable before capture or permission requests, without a full inventory or later-phase Resource/Skill/Technique setup.

**Source choice.** Camera, microphone and speech output are independently selectable; opening the surface starts none of them. Before acquisition, disclose purpose, active Identity/Compartment, enabled sources, local route and effective retention limits. Keep capture state, pause/stop, silence and switch-to-text controls available during capture and processing. Permission to use hardware is not session consent. A remembered surface choice is a user-controlled UI setting, not an inferred planner Preference; declining capture must not cause recurring permission prompts. Guided acquisition explains the missing information, offers another view, and allows stop or an alternative.

**Guided acquisition (`UBU-D0338`).** Offer optional help for a user-selected target, such as acquiring a label. Explain the detail needed and give one short positioning or lighting instruction at a time through the user's selected usable output: text, optional speech or supported assistive technology. Make direction references explicit and let the user repeat or skip a cue. Nonvisual use must not require interpreting the preview. Distinguish an observed framing problem from a suggestion; use only current permitted source evidence and keep uncertain or unreadable details explicit. Present extracted names and values for exact correction during ordinary candidate review; framing success and model confidence cannot establish factual correctness. Each enabled profile declares finite guidance-time and retry limits. Stop immediately on request; at its limit or when useful feedback is unavailable, offer another eligible view, manual entry or deferral without restarting capture. Guidance starts no source, renews no consent and extends no media deadline.

**Access across routes.** Make source controls, guidance, exact-value correction, uncertainty, candidate diffs and accept/reject/defer actions usable through the supported keyboard, screen-reader and switch-access routes. Expose control names, roles, state, focus and errors; no essential action depends solely on color, a pointer gesture, speech or seeing the preview. Apply the continuity rules below when changing channels. Permit text/forms with camera, microphone and speech output disabled and models unavailable. Let users choose channels and assistance without disclosing a diagnosis or inferring disability, intent or planner Preferences. Speak private source details only through a currently permitted user-selected output route; otherwise offer private text or defer. Apply §21.1.2's attention budget to guidance and review: unavailable safe attention or output defers affected interaction, and an access setting cannot widen action permissions.

**Ordinary-attention coverage.** All routes use the same admitted-state operations, validation and review authority; provenance follows the actual source. Camera input is useful where observation is relevant, rather than required for every operation.

| Workflow | Camera/voice route | Text/form route |
|---|---|---|
| First useful concern or quick capture | Show relevant context and describe the concern; propose a note or Task with uncertainty visible. | Type the concern or enter minimal note/Task fields without a media attachment. |
| Clarify or correct | Ask about consequential ambiguity; let the operator identify the target, correct interpretation or leave it unresolved. | Select permitted existing targets and edit exact names, values, dates, durations or constraints; retain unknown/private or unresolved answers without inventing field values. |
| Next action and explanation | Request the next legitimate Task and inspect or hear why it is recommended. | Read the same Task, explanation, constraints and permitted evidence, or the reason no legitimate recommendation is available. |
| Review a proposal | Discuss its source, uncertainty, exact proposed diff and effects; explicitly accept, correct, reject or defer the identified item. | Inspect the same candidate and diff, edit fields, and accept, reject or defer without speaking or capturing media. |
| Act, override and reconcile | Report action, override or progress; observations remain candidates for actual-versus-planned reconciliation. | Use Task controls and interval/status entry, including unknown/private time, under the existing Log, Snapshot and recalculation rules. |
| State a Preference | Speak an explicit pairwise statement and review it before acceptance. | Enter and accept the same pairwise statement; channel choice or repeated behavior creates no planner Preference. |

**Continuity and degradation.** Changing surfaces preserves permitted user-authored drafts and retained candidate/version/evidence links, distinguishes operator corrections from model interpretations, and shows unsaved input. It neither admits a candidate nor duplicates an operation, starts another source, changes Compartment or extends raw-media retention. Changed source/context bindings invalidate pending interpretations and approvals; late results cannot overwrite manual corrections. Missing, declined, revoked, noisy or poorly framed sources and failed or over-budget local backends leave the remaining routes useful. Explain the limitation without claiming recognition and offer text/forms; do not retry capture repeatedly or reroute local AV to cloud processing. With camera and microphone disabled and models unavailable, manual capture/editing, existing Plan inspection, candidate review and action/override recording remain usable through ordinary validation. Forms expose precision on demand without requiring internal-ontology entry; labels, errors, focus and review actions work with keyboard and screen reader, with speech output optional.

**Acceptance evidence.** Before claiming the primary surface delivered, attach revision-pinned scenario results on the qualified hardware profiles required by §4.2 (`UBU-D0339`) and selected under `UBU-Q0177`, identifying enabled sources/backends, policy configuration, outcomes and permitted candidate/admission references. Demonstrate:

- A shown concern plus voice uses a visible detail and permitted context to produce a useful candidate or consequential clarification, followed by operator correction, admission and a legitimate next action or explicit planning limitation. Exercise the other voice workflows and repeat every table row using only text/forms with both capture sources disabled. Equivalent accepted meanings produce matching canonical effects and recalculation, with source-appropriate provenance.
- Camera/microphone absence, denial or revocation; noisy speech or unsuccessful framing; offline/local-model failure and resource exhaustion allow manual completion without lost permitted input, fabricated observations, false saved-state acknowledgments or cloud fallback.
- Keyboard-only and screen-reader completion of text/form entry, exact-value correction and review with speech output disabled; guided acquisition can be stopped or replaced without repeated unexplained capture requests.
- Start, pause, resume, stop and revocation accurately control acquisition and visible state. Raw AV stays local; context and output respect Compartment denials, and export requires its separate authority. Retention behavior matches the approved policy rather than a documentation claim.
- Switching surfaces during a delayed interpretation, duplicate submission/replay, and material context, policy or Device change cause neither stale admission nor duplicate mutations. Preserved candidates carry no live capture consent or pending execution approval across Devices.
- Ambiguous objects, high-confidence interpretations and apparent completion remain unresolved/candidate evidence until required review and validation. Unknown/private intervals and channel choices create no inferred intent, affect, Preference, habit or completion claim. Restricted attention obeys §21.1.2: defer unavailable review rather than demand visual/form interaction or widen permissions.

**User-tested access evidence (`UBU-D0338`).** Before enabling or claiming these guided-access workflows, extend the acceptance evidence with consented user sessions involving people who use the proposed access routes and experience the relevant vision, hearing, speech or dexterity constraints; developer simulations alone are insufficient. Predeclare participant/condition coverage, tasks, Device/software/assistive-technology versions, finite time/attempt and assistance-burden limits, and useful-outcome criteria. Include label framing with blur, glare and cropping; guidance without visual preview; unavailable speech or hearing; limited camera handling; keyboard, screen-reader and switch access; capture declined for privacy; and restricted attention in safe simulations. Use matched tasks to compare guidance, unguided capture and the user's preferred alternative where usable; report unavailable routes and user-reported burden without forcing capture. Repeat entry, exact correction and review with capture and speech output disabled. Exercise private-output denial, stop, retry exhaustion and switching during a delayed interpretation.

Report completion, correction, unresolved, rejected and failed outcomes, time, retries and required assistance by tested condition and route; distinguish useful manual fallback from successful guided acquisition. Require reachable source controls and exit, preserved permitted drafts, faithful uncertainty/review, ordinary admission and no unauthorized acquisition or disclosure. A failed mandatory access/privacy gate or missed declared usefulness limit withholds the affected workflow for that combination; keep its fallback and remaining gap visible. Publish policy-safe results and limitations alongside the support matrix, with explicit participant consent for research retention separate from capture permission and no compulsory diagnosis or raw-media retention. Public access claims name only the tested workflows and combinations; passing one route or population sample cannot establish universal accessibility. These are release evidence requirements, not passing results; hardware qualification, numerical profiles and delivery allocation retain their existing questions.

This resolves interaction coverage and acceptance, not implementation. `UBU-D0339` sets capture-mode/device-role allocation and delivery sequence in §4.2; `UBU-Q0177` retains measured hardware selection and `UBU-Q0160` retains the mobile workload/API choice; `UBU-Q0128` retains numeric restricted-attention profiles and richer actions; `UBU-Q0166`–`UBU-Q0172` retain exact evidence, corroboration, latency, segmentation and retention contracts. Apply the existing evidence, policy and attention gates to each enabled slice. No particular backend, hardware support, latency target or later-phase planning object is established here.

#### 21.1.4 Latency classes, freshness and interruption

**Class contract (`UBU-D0325`).** Use four interaction latency classes. A snapshot capture is a bounded one-shot input; it does not itself admit a canonical Snapshot. Receipt, permitted durable saving, interpretation, candidate admission, planner certification and verification of an executed effect are separate outcomes. A receipt may say "Received" or "Processing"; "Saved" requires the relevant durable write, and neither message implies admission or execution.

| Class | Clock and useful result | Deadline/degradation behavior |
|---|---|---|
| `snapshot_capture` | From controller acceptance of a one-shot capture request through acquisition, interpretation and presentation of a reviewable candidate or explicit unresolved result. | End acquisition and cancel unfinished interpretation at the job deadline. Preserve permitted saved evidence/drafts and show any unsaved input; offer an explicit retry or text/form entry. |
| `conversational` | From acceptance of a completed user turn to the first substantive response or clarification, with a separate total turn deadline. | A missed response deadline ends waiting with an unavailable/deferred explanation; stop pending speech and interpretation. A new request uses fresh bindings and budgets. Restricted attention uses §21.1.2 rather than demanding visual review. |
| `streamed_supervision` | From each permitted observation window to a usable candidate update, with a maximum observation age at presentation and a maximum gap since the last usable update. | On excessive age, gap or backlog, withdraw live guidance and show "Updates delayed/paused". Bound acquisition and queued work; expose permitted coverage gaps rather than replaying old observations as current supervision. This class supplies no safety-critical monitoring guarantee. |
| `retrospective` | From job enqueue to a reviewable report over a pinned historical interval/source manifest, with bounded progress intervals and a total job deadline. | Yield to interactive work. On exhaustion, pause at a permitted durable checkpoint under §21.8 where supported, otherwise stop with retained sources. Label analyzed and missing scope; explicit continuation rechecks current policy and bindings. Partial work cannot claim full coverage. |

**Deadline policy.** Before enabling a class on a Device/backend/mode, require a controller-owned versioned profile with finite local-acknowledgment, first-useful-result and total-job ceilings, applicable source/context-age ceilings, and, for streams, update-gap and backlog ceilings. Enabled work uses positive elapsed ceilings with acknowledgment <= first useful result <= total job. Declare finite concurrency, calls/work, prompt/result, memory/buffer and storage limits plus battery/thermal pause and recovery conditions. Missing or unsupported limits prevent that class's dispatch while manual routes and stop controls remain available. These are policy requirements, not new wire-schema fields or measured defaults.

Bind elapsed deadlines to the local monotonic clock at the table's start event; include queueing, acquisition where applicable, preprocessing, generation, validation and presentation. Progress text, tokens, retries and intermediate frames cannot reset deadlines or count as a substantive result. Before waiting, show the active response/wait limit and fallback in the permitted channel; expiry produces the named degraded outcome and cancels outstanding work. The effective limits are the intersection of that profile, remaining session consent, current policy and §21.1.2's attention/turn budget where applicable. They may tighten during work; continuing beyond them requires a new explicit request and fresh bindings. Restart or clock discontinuity invalidates active jobs rather than extending their deadlines.

**Freshness and authority.** Bind results to their original observation interval, source/session/context and target versions, policy versions and controller job/turn generation. Current guidance/dispatch requires a live generation and applicable age checks; unknown or uncertain age cannot establish freshness. Review/admission rechecks evidence, context, targets and current policy under ordinary review bindings. Reopening retained material establishes fresh review bindings rather than reviving a cancelled job. New generation timestamps and repeated summaries cannot refresh old evidence. Historical analysis may use permitted older evidence within its declared interval; applying findings to current state still requires current evidence and admission. Stale/superseded results remain labelled historical material only where retention permits, and cannot overwrite corrections, satisfy current preconditions or use expired approvals.

Capture updates and planner streaming have separate timing traces. A planner chunk can reach the user only after CPU certification of its current request under PLANNING_KERNEL_CONTRACT.md §4; incomplete/final transport status does not establish an executed effect. Slow processing cannot waive evidence, hard validation, Compartment policy or `UBU-D0324`'s corroboration requirements.

**Offline, battery and thermal degradation.** Keep local pause/stop/silence, visible source state, permitted draft handling and the manual admitted-state loop independent of model/network completion. Defer retrospective work first, reduce streaming rate or analysis scope within a declared profile, then offer bounded one-shot capture or text/forms; pause capture/analysis when limits cannot be met. Disclose the active class, reason, retained/unsaved work and freshness limitation without leaking denied source existence. Resource pressure never extends media retention, silently evicts accepted input/history or starts another source.

Offline operation uses available permitted local backends and manual routes; no hidden cloud fallback. Cached Plan guidance is usable only after §16.5–§16.7's legitimacy/envelope/freshness checks. If those checks fail, show the planning limitation or eligible clarification rather than an obsolete next action. Returning connectivity, power or temperature may permit a finer compute profile, but cannot restart paused/stopped acquisition, speech or queued actions without current consent and fresh controller bindings.

**Interruption.** Pause, stop, silence, cancellation, a superseding turn, source/Compartment or Device change, and consent revocation invalidate the affected pending output/action bindings locally without waiting for a backend acknowledgment. Stop the affected acquisition/output and request cancellation; reject late callbacks for live guidance or dispatch even while the backend finishes. Require bounded cancellation/resource release under its permitted lifecycle; an adapter without that bound cannot enable the class. Preserve already-admitted state, permitted durable sources/drafts and ordinary idempotency outcomes. Interrupted work does not establish an effect or authorize external-action replay; existing outcome/idempotency rules still apply. Reviewing retained candidates later neither resumes capture nor restores approval; restart/handoff requires new local bindings and ordinary review.

**Deadline-boundary qualification (`UBU-D0335`).** An outstanding elapsed deadline expires when the controller's monotonic time is at or beyond its bound. Recheck time and current generation when releasing live output or dispatching; a delayed timer callback cannot make an expired result eligible. Serialize interruption/invalidation with release: an already-observed cancellation or superseding turn wins over a queued result. Pin separate finite expiry-to-visible-fallback, local-stop-to-output-quiescence and cancellation-to-resource-release ceilings; backend acknowledgment cannot delay local invalidation.

For streams, start each permitted observation window's processing clock when that window closes, before queueing, using its mapped controller-monotonic timestamp; an unknown mapping cannot establish live freshness. Bound the acquisition window and consented session separately. Before the first usable update, the first-useful-result deadline bounds waiting. Afterward, only presentation of a fresh, valid usable update resets the update-gap clock; progress, stale results and dropped windows do not. Crossing an age, gap or backlog limit invalidates affected pending live output and withdraws displayed guidance. Resumption needs explicit restart/resume, a fresh generation, current consent and a fresh usable update before showing live guidance.

Run the following fixtures against the actual controller with a deterministic clock and delayed/duplicate adapter callbacks, then demonstrate the corresponding behavior in the pinned real-device trials. Place events immediately before, exactly at and immediately after each applicable elapsed deadline; for age/backlog ceilings, exercise both equality and exceedance under the declared profile comparator.

| Class | Required boundary fixture | Required disposition |
|---|---|---|
| `snapshot_capture` | Queueing/acquisition consumes the budget; interpretation or retry returns at/after expiry. | Acquisition ends, late live output rejects, permitted saved input survives and unsaved input is identified; retry cannot renew the original deadline. |
| `conversational` | Tokens precede a missed useful-response deadline; silence or a superseding turn races with queued speech/results. | Tokens never satisfy the useful-result gate; old speech/results cannot resume or overwrite the new turn, and the bounded fallback remains available. |
| `streamed_supervision` | No first update; then stale/duplicate updates, a gap and queued/in-flight overload, followed by apparent backend recovery. | Bound initial waiting and aggregate backlog; withdraw live guidance, expose permitted coverage gaps and reject old updates without automatic resumption. |
| `retrospective` | Progress/checkpoint activity and competing interactive work straddle total-job expiry. | Progress/yielding cannot renew the deadline; pause at a permitted checkpoint or stop, retaining sources and truthful partial coverage. |

Each fixture report binds profile/implementation/policy revisions, minimized job/generation and source-epoch aliases, clock basis/resolution, relevant event offsets, queued/in-flight high-water marks, and expected versus observed disposition. Include acknowledgment, useful presentation, invalidation, fallback, output-quiescence and resource-release offsets where applicable. Diagnostics inherit source restrictions and do not require retained media or a new Log/wire schema. Release of expired/cancelled live output, stale admission/dispatch, renewal of the same job's deadline, unauthorized resumption or loss of permitted saved input fails that profile's enforcement gate; aggregate timing percentiles cannot excuse it. Later ordinary review of retained candidates with fresh bindings remains permitted. Deterministic fixtures establish controller behavior only, not achieved latency or hardware support.

**Measured enablement gate (`UBU-Q0168`).** Select numeric ceilings only from revision-pinned evidence on each proposed Device/backend/mode; record OS/runtime/model/template, implementation and policy revisions, source/context sizes, load and instrumentation. Measure controller acceptance, acknowledgment, first useful result, completion, observation age at presentation, stream gaps and stop/output-quiescence separately from admission, first CPU-certified planner chunk and final planner response. Report sample counts, median and tail distributions, misses, errors/degraded outcomes, memory/storage, battery/energy where observable, and sustained thermal behavior; unavailable metrics stay explicit. Report successful interpretations separately from unresolved/degraded outcomes; latency alone establishes neither correctness nor usefulness.

Exercise cold and warm operation, competing work and sustained streams; offline/backend loss, low battery and thermal/resource exhaustion; interruption during acquisition/inference/output; queue overload and deadline expiry; source/context changes, corrections, revocation, restart/handoff and delayed/duplicate results. Demonstrate bounded waiting/backlog, working manual fallback, truthful saved/coverage/freshness status, no stale admission/dispatch and no hidden rerouting. Publish the selected finite ceilings and demonstrated fallback for each enabled class; an unmeasured class remains unavailable. Hardware selection remains `UBU-Q0177`, restricted-attention numeric profiles remain `UBU-Q0128`, and delivery sequence remains `UBU-Q0183`. `UBU-Q0168` stays Open for these numerical profiles and measured acceptance; this contract asserts no implementation, supported hardware or achieved latency.

#### 21.1.5 Local AV segmentation and scoped analysis

**Architecture contract (`UBU-D0326`).** Adopt a controller-owned local media broker with the logical adapter profile `local-media/1`. Session consent enables only named sources on eligible execution-enclave Devices on the operator's own hardware; sharing hardware grants no access between enclaves. Capture controls remain visible under §12.2 and §21.1.3. Models and agents can request analysis of supplied media spans, but cannot start/resume capture, enumerate sources, renew consent or enlarge a grant. This profile specifies an interface boundary, not an implemented MCP service, frozen wire-schema change or new planner entity.

**Segmentation and buffering.** Maintain a separate bounded transient ring for each consented source epoch. Seal immutable chunks at the first configured duration or byte ceiling, or earlier on pause, stop or a source/consent/Identity/Compartment boundary. Bounded chunks are the storage/read units; utterances and detected events are separate span references that may cross chunks only within authorized continuous coverage. A detector's proposed boundary cannot extend acquisition or retention. One-shot requests end acquisition at their requested bound; they never become continuous sessions.

Before enabling a source, pin a controller-owned Device/backend/mode profile with positive finite chunk duration/bytes, raw-buffer age/bytes, total raw/decoded/intermediate memory, queue count/age, concurrency/calls, decode work, frame dimensions/count, event/span/result sizes, job deadline, cancellation grace and artifact/scratch lifetime. Use §21.1.4's measured deadline/resource gate and current source-retention policy; missing limits deny acquisition/dispatch. Include pinned chunks and worker copies in aggregate accounting. Evict only eligible unpinned transient material and record coverage gaps; pins cannot extend retention or exceed quotas. If eligible eviction cannot satisfy limits, pause acquisition and cancel/defer affected work visibly. Never spill to unapproved storage, silently discard acknowledged durable evidence or reroute to cloud processing.

**Timeline and coverage.** Each sealed chunk/span manifest binds session, source, Device, source epoch, sequence, immutable content/version reference, half-open interval `[start, end)`, actual sample/frame positions, capture provenance, policy/retention versions and permitted gaps. Preserve media sample/PTS offsets separately from acquisition monotonic offsets, with their clock mapping, wall-time anchor and uncertainty when known. Capture elapsed time and live deadlines use a local monotonic clock; historical intervals retain their original time basis. Ingestion, interpretation or emission times cannot replace observation time. Restart, source replacement and clock discontinuity create new epochs. Unknown mappings stay unknown: do not align different sources or Devices by timestamp equality, sequence or nominal frame rate. Audio/video alignment requires explicit permitted clock mappings and uncertainty. All selectors state the source epoch, time basis and units; reject nonfinite, reversed or out-of-range bounds. Missing, dropped, expired or sampled-out intervals never establish silence, no event or a continuous observation.

**Access and invocation.** Effective access is the intersection of current session/source consent, actor and adapter Device eligibility, capability grant, declared Task/Objective/workflow purpose, source policy and finite profile. The grant names allowed operations, sources/epochs and read intervals, context selectors, recipient adapter, expiry and aggregate work/output limits. Opaque handles are audience- and scope-bound references, not authority by possession; recheck current policy on every dereference. Broker-mediated reads and worker isolation must prevent ambient camera/microphone, unrelated filesystem/context, network and credential access. Physical locality alone does not satisfy this boundary.

Bind each request to its id, actor/grant and versions, session/source epochs, pinned manifest/evidence/context versions, operation/finite selectors, requested read/output intervals, minimized ContextBundle, adapter/model/profile revisions, policy versions, job generation, monotonic deadline and idempotency key. Check these before reading, invoking and releasing each result. Requests may select only supplied permitted aliases; arbitrary paths, URLs, device discovery, executable instructions and unbounded selectors are unavailable. Current denials fail closed with policy-safe errors that do not reveal denied existence or Compartment identity. An exact retry shares the original budget/deadline and idempotency outcome; changed-content key reuse rejects. A retry cannot reacquire expired media or widen scope; expired result artifacts return unavailable rather than a cached disclosure.

A bounded result envelope echoes the request/generation and original manifest bindings, reports complete/partial/unavailable/cancelled/denied status, analyzed/missing coverage, permitted artifact refs/expiry, work used and transform/tool/model provenance. Validate size, profile, references, intervals and bindings before release; malformed or oversized results reject without repair into a usable output. Errors and metadata obey the same disclosure boundary as media.

| Logical operation | Required bounded input | Output and boundary |
|---|---|---|
| `media.frames.extract` | Pinned source spans plus explicit timestamps or sampling interval, finite frame count, dimensions and optional crop within the grant. | Selected frame payloads/leased handles with original sample positions, actual observation times, transform provenance and gaps. No fabricated/interpolated frames or reads outside the authorized read scope. |
| `media.events.detect` | Pinned audio/video spans, an allowed detector/label set and finite window/work/event limits; only purpose-required context. | Advisory event hypotheses with source spans, detector/model revision, confidence/uncertainty and analyzed/missing coverage. No detection means only no reported match in analyzed coverage; it is not evidence of real-world absence, completion or an effect. |
| `media.sections.isolate` | Explicit subintervals or validated event-span references within already authorized sources, with finite section count, duration and bytes. | A manifest of selected spans and, when authorized, a leased local clip handle. Relevance is a proposed selection, not a capability grant; suggestions cannot fetch adjacent or new sources outside the original scope. |

Codec dependency frames or audio padding may be decoded internally only within the declared authorized read scope and decode budget; return only the requested output interval and disclose any permitted padding/coverage limitation. Otherwise return unavailable/partial coverage rather than silently broadening the read. Multi-source results carry every parent span and satisfy the intersection of their disclosure/retention policies. Overlapping windows and derived copies preserve sample lineage and cannot count as independent corroboration.

**Cancellation and revocation.** Serialize pause/stop, source or policy change, grant expiry/revocation, job-generation invalidation, reads and result release in the controller. Invalidation immediately prevents further reads and acceptance of late callbacks; stop affected acquisition and request backend cancellation without waiting for acknowledgment. After the finite cancellation grace, terminate the isolated worker if needed and reclaim its leases/resources. An adapter unable to enforce isolation and bounded release remains unavailable. Resume/restart/handoff uses fresh local consent and bindings; synchronized candidates and persisted handles never restore live access. Later review of permitted retained evidence is a new bounded job, not revival of the cancelled one.

**Intermediate artifacts and audit.** Decoded frames/audio, crops, thumbnails, temporary clips, transcripts, embeddings, prompts and model caches inherit source restrictions. Scratch expires on completion/cancellation; returned temporary artifacts have a finite non-renewing result lease bounded by applicable source policy. Any permitted disk scratch is policy-protected, inventoried with expiry and cleaned before reuse after restart; there is no implicit durable spool, training use or general-purpose cache. Explicit or already-scoped policy authorization is required to retain selected evidence beyond its temporary lease, through ordinary Discovery/candidate storage with lineage and its own allowed purpose/retention. Reads and retries cannot silently renew raw/intermediate retention.

Revocation/purge invalidates disallowed handles and removes app-controlled disallowed copies and reconstructive derivatives under current policy. Already-admitted history follows ordinary append-only correction/redaction rules; raw expiry does not edit a historical claim or prove it false. Record permitted minimized invocation, source-span, policy, budget, gap, cancellation/cleanup and candidate links rather than mandatory raw media, transcripts or hashes. Scene/OCR/transcript text is untrusted data, never an instruction, permission or grant. Results use §21.1.1's Discovery/advisory bindings and ordinary review/admission; they cannot directly mutate canonical state, certify a Plan or authorize external action. Export or a different Device/recipient requires its separately permitted route and approval under §§21.3–21.4.

**Enablement evidence.** Before exposing an adapter, pin implementation, OS/runtime/backend/model and policy/profile revisions and demonstrate:

- Variable frame rate, audio/video clock uncertainty, timestamp discontinuity, chunk-crossing events, overlapping samples and dropped/expired spans preserve original times, lineage and explicit analyzed/missing coverage.
- Forged/foreign/expired handles, wrong purpose/actor/Device, mixed-policy sources, unbounded selectors, codec dependencies outside scope and hostile scene/transcript instructions expose no unauthorized media/context or tool capability.
- Sustained capture, slow consumers, oversized decode/output, pinned-buffer exhaustion and disk pressure respect aggregate limits, pause/defer visibly, preserve permitted saved evidence and leave manual/stop controls usable without cloud fallback.
- Cancellation/revocation during read, inference and result delivery; delayed/duplicate callbacks; source changes, restart and handoff produce no stale release, revived consent or duplicate candidate effects, and release workers/artifacts within the configured bound.
- Normal completion, expiry, crash/restart and policy purge remove disallowed scratch/caches/derivatives; separately retained evidence remains correctly policy-bound and ordinary review is required before canonical effects.

These are required gates, not reported passing tests or physical-erasure claims. `UBU-Q0169`'s architecture is answered; numeric latency/resource acceptance remains `UBU-Q0168`, default raw-media retention follows §12.2 (`UBU-D0327`), and operator discard/summary/hash/clip/export controls follow §12.2 (`UBU-D0328`). Continuous capture cannot be enabled with unresolved effective retention policy. Host fidelity, proof-of-effect semantics and delivery sequence retain their existing gates.

#### 21.1.6 Realtime guidance stops and professional review

**Release envelope (`UBU-D0330`).** Camera access, fluency, confidence and prior assisted success establish neither domain competence nor safe conditions. Before enabling procedural direction, declare a controller-owned, versioned envelope of supported workflows/steps, required context, evidence/freshness and Skill/Resource prerequisites, finite clarification limits, verification/fallback conditions and appropriate professional roles. Missing envelopes and unclassified consequences withhold procedural direction. Models cannot add permitted steps or weaken requirements.

MVP procedural direction is limited to explicitly released, bounded reversible ordinary steps. Individualized clinical diagnosis/treatment, hazardous electrical/gas/structural/machinery procedures, professional legal/financial determinations and safety-critical monitoring are outside this release envelope. Scheduling, recording user-provided information and preparing questions for a professional retain their ordinary planning/review paths. Rich Technique execution and any later domain expansion require their own implemented objects, qualified domain review and release evidence.

**Stop conditions.** Check eligibility before the first and each subsequent step, before output/dispatch, and on material observation, context, policy or attention changes. Withhold the affected step whenever a required condition cannot be established from permitted evidence:

| Condition | Required response |
|---|---|
| Unsupported domain/step or required professional judgment | Stop procedural direction and identify the appropriate domain professional, such as a clinician or qualified trade specialist. More camera input cannot authorize diagnosis, on-site assessment or specialist oversight. |
| Missing context or hazardous uncertainty | Missing/ambiguous target identity, measurements/units, site state, Skill/tools, safety prerequisites, source coverage or freshness remain unknown, including withheld, occluded or expired data. A bounded non-invasive clarification is allowed only within the released workflow and attention budget. If consequential uncertainty cannot be resolved safely, require relevant professional assessment before affected guidance; ordinary low-risk ambiguity may instead defer to manual entry. |
| Contradictory observations, unexpected change or failed verification | Pause the affected guidance; preserve conflicting sources and the unmet condition. Do not average away disagreement or use confidence, repeated frames or model agreement as verification. A safety-relevant conflict that remains unresolved, or a professional-stop fallback, requires appropriate professional review. |
| Possible immediate serious harm | A user report or observed warning may require urgent escalation. Withdraw procedural guidance and give a concise message to seek appropriate local emergency assistance now, without diagnosis, waiting for more images or diagnostic questioning. Uncertain location must not produce an invented contact number. |
| Stop/revocation, unavailable safe attention or controller/source failure | User stop, invalid consent/policy/bindings, unavailable private output, forbidden restricted-attention interaction, delayed/lost sources, backend failure or exhausted limits stop affected work under §§21.1.2–21.1.5. A technical failure alone does not require a professional; hazardous unresolved prerequisites do. |

Hazard judgments are fallible. A stop explains the reported/observed concern or missing requirement without claiming a diagnosis, proven danger or guaranteed detection. No warning detected does not mean safe. This bounds the guidance service; it neither controls the user's physical actions nor turns §22.4's advisory behavioral judgments into product hard constraints.

**Visible stop behavior.** The controller immediately invalidates affected job/step approvals, stops queued speech/procedural output, pauses affected live acquisition/analysis and requests bounded cancellation without waiting for the model. Reject late callbacks and procedural replay. Use the existing paused/ended session lifecycle, not a new canonical state. Show "Guidance stopped", the permitted reason and affected scope, what remains unknown, whether capture is paused, and the appropriate next route. Preserve permitted drafts/evidence and distinguish saved from unsaved input. Offer End, ordinary manual planning/notes or operator-initiated professional contact. Restricted attention permits only its brief output envelope; defer menus and visual review.

Escalation confers no contact, disclosure, export or action authority. Calls, evidence sharing and other external actions still require their separate current permissions and operation-bound review; a stop never admits completion, a world-state correction or a professional handoff. Minimal expert-handoff packets remain `UBU-Q0175`.

**Fresh resumption.** Dismissing a notice, accepting risk, changing channels, rephrasing, restart or Device handoff cannot supply missing eligibility. Resume only after a fresh explicit request, renewed local consent/bindings and review establishing that each stop reason is resolved within the released envelope. Where qualified review is required, retain the reviewer's attributed identity/role and qualification basis, exact findings, inspected evidence, scope/time and conditions; a model's claimed expert opinion cannot satisfy it. Unknown qualifications or unsupported advice remain unresolved. A corrected false-positive signal may clear through ordinary evidence review. Recheck current context/evidence/policy versions before the next step; professional advice cannot expand the release envelope. Unrelated manual workflows retain their normal gates.

**Evidence and enablement.** Reuse existing session, Discovery/candidate and review/Log links for envelope/version, affected step, triggering evidence/time, unknowns/contradictions, stop reason, presented options, required professional role and resolution/resumption bindings. Source restrictions apply to diagnostics and records; do not reveal denied content, identifiers or existence. Record operator dismissal/override through §17.0 without inferring Preferences or requiring justification. No new kernel field, provenance kind or Log event is selected.

Before enabling a guidance workflow, pin controller/backend/model, envelope and policy revisions and demonstrate:

- Ordinary ambiguity safely clarifies or defers; high confidence, missing/occluded/stale context, contradictory readings and failed verification withhold affected steps and route hazardous uncertainty to professional review.
- Unsupported domains, unknown consequences and absent professional qualifications stay unavailable despite disclaimers, risk acceptance or model-supplied clearance; possible urgent harm receives brief emergency escalation without additional diagnostic acquisition.
- Stop/revocation during acquisition, generation and speech cancels locally; delayed callbacks, retries, channel changes, restart and handoff cannot resume instructions or execute queued effects.
- A reviewed correction or scoped qualified assessment permits only a freshly requested eligible step; changed evidence/context/policy invalidates that review. Manual alternatives remain usable.
- Restricted attention and policy-denied evidence remain protected in prompts, notices and audit; escalation causes no automatic call, upload, completion or evidence export.

These are required behavioral gates, not passing results or guarantees of safety. `UBU-Q0174` is answered as a process contract; measured timing, host fidelity, expert packets and release sequencing retain their existing questions and gates.
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
- When editing `OPEN_QUESTIONS.md`, anchor hunks with the selected question heading `## UBU-Q0078: Interruption, escalation, and discovery-mode consent policy` and its own `### Resolution` section. Do not use a repeated `### Resolution` heading from an earlier or later question as the edit location.
- If resolving the selected question, replace the `Unresolved.` text under that selected question's `### Resolution` section and update that same question's metadata line. Do not insert selected-question resolution text into any other question block.
- Use the existing `UBU-Qxxxx` and `UBU-Dxxxx` numbering conventions.
- Prefer minimal, auditable changesets.
- If the selected question is blocked, propose decomposition only if it produces replacement questions with fewer, simpler, or no dependencies.
- If the selected question is already partially resolved, narrow or clarify it rather than pretending it is fully unresolved.

Return only JSON.
