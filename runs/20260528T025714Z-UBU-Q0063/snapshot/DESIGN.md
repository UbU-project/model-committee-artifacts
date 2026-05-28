# UbU Design

Status: Draft / active pre-MVP dogfooding  
Repository: `ubu-design`  
Purpose: Canonical public design document for the UbU project

---

## 1. Overview

**UbU** is a privacy-first planning, coordination, and self-governance system for implementing life logistics.

UbU takes messy real-world inputs—tasks, calendar events, messages, external events, user preferences, physical/emotional state, Resource availability, Skill capability, integration data, realtime interaction streams, and agent or worker outputs—and turns them into explicit, inspectable, recalculable Plans.

UbU is not merely a task list or calendar application. Its core motivation is to help an individual human being plan and implement the logistics of an actual life: what matters, what must happen, what the world must contain, what the user is capable of doing, what can be learned, what should be bought or delegated, and what should happen next. Organizational coordination is an important emergent property of the same model, not the root purpose.

The first MVP is designed around **dogfooding**: using UbU to coordinate the design, development, release, and maintenance of UbU itself.

Recent LLM and agentic-AI changes reinforce UbU's core boundary: realtime, multimodal, tool-using, memory-bearing models are valuable interaction and extraction backends, but UbU remains the user-sovereign state-transition, planning, logging, privacy, and review layer.

For Phase 1, UbU must also be understandable as a first-person user experience. The minimal user-facing loop is: answer a small number of bootstrapping questions, allow UbU to construct an initial context model, receive one recommended next Task, inspect why that Task matters now, act or override, then let UbU learn from the result through Logs, Snapshots, and recalculation.

---

## 2. Core Principles

### 2.1 User sovereignty

The user is the final authority over what the user wants and what occurred.

UbU may recommend, infer, estimate, warn, and automate, but it must not override user sovereignty.

User overrides are authoritative, but not necessarily consistent. Two forms of consistency matter:

- **Logistical consistency**: hard consistency required by the data model and planner.
- **Philosophical consistency**: broader consistency between the user’s stated goals and actual behavior.

UbU may reject or require repair of logistical contradictions. Philosophical contradictions are allowed and may be surfaced later through reporting.

Example:

- A user cannot maintain an active preference cycle such as `A > B`, `B > C`, `C > A`.
- A user may state an Objective like “communicate more with X” and later ignore a message from X. That is philosophically meaningful, but not a data-model violation.

### 2.2 Explicit value

Value must be explicit in the UbU data model.

Value is attached to **Objectives** through user-defined **Preferences**. Numeric “utils” may be derived for plan scoring, but these are transient computational artifacts, not canonical facts about the user.

UbU should not expose derived utility as if it were the user's real value. User-facing value formation should rely on direct Preference statements, review, reflection, examples, and later correction.

LLMs may estimate or suggest value-related structures, but AI-generated value is non-canonical unless accepted through UbU’s explicit model.

#### 2.2.1 Preference calibration

Phase 1 may use preference-calibration examples during onboarding, Calendar preview, and Log review. These examples describe common situations, likely emotional costs, likely emotional rewards, and plausible tradeoffs so the user can make more thoughtful Preference judgments.

Examples are not canonical Preferences. They are prompts for self-reporting. A calibration example becomes canonical only if the user accepts, edits, or answers it in a way that UbU records as a Preference, Snapshot, Log entry, Objective annotation, or review note.

Preference calibration should reduce shallow, reactive, or situationally distorted guesses without manipulating the user into a hidden value model. The user remains the final authority over Preferences.

### 2.3 Explicit planning

Planning must remain explicit and constraint-based.

UbU should model:

- Tasks
- dependencies
- preconditions
- effects
- Objectives
- Preferences
- UniverseState
- Plans
- Calendars
- external events
- decision/recalculation triggers

LLMs may assist, but the canonical planning and decision logic must remain explicit and recalculable outside the LLM.

#### 2.3.1 Life logistics, Resources, and Skills

Real-life planning failures are often not time-allocation failures. They are readiness failures: the user lacks a document, tool, ingredient, credential, location, payment method, transportation option, quiet space, or embodied capability required to perform the Task.

UbU therefore treats external prerequisites and embodied capabilities as planning-relevant predicates:

- a **Resource** is something the user or another Identity must have access to or in the correct state;
- a **Skill** is a rust-prone capability held by an Identity;
- a **Technique** is a reusable procedure that can transform Resources, Skills, time, and attention into an Objective-serving result.

Resource and Skill modeling is delayed from Phase 1 only because of the bootstrapping process. It is not conceptually peripheral. A minimal Resource/Skill-aware task-readiness layer is a Phase 3 candidate because it directly serves the core personal life-logistics product.

### 2.4 Privacy-first architecture

UbU is designed around privacy, local-first operation, compartmentalized data, multiple Identities, and user-controlled sharing.

Sensitive data must not leak merely because structural planning metadata is useful.

### 2.5 Affect-aware planning

Human beings are not machines.

In `user_mode`, affective state is part of the planning problem. Energy, stress, tiredness, mood, and related human limitations are constraints that planning must respect.

UbU should not assume that users can simply execute arbitrary work because a schedule says they should.

### 2.5.1 Human-complete planning quality

UbU treats affect as a core part of planning, not a decorative wellness feature.

A plan that ignores fatigue, stress, boredom, motivation, emotional load, recovery, and dignity is not merely incomplete. It may become actively misleading.

A high-quality plan should:

- produce fast feedback about success or failure;
- make failure informative rather than humiliating;
- suggest revision after failure;
- respect the user’s dignity;
- respect emotional and physical limits;
- distinguish sustainable stretch from destructive pressure;
- help the user grow beyond current limitations when appropriate;
- remain recalculable when reality contradicts the model.

The goal is neither comfort-maximization nor coercive productivity. The goal is humane self-governance: disciplined action that respects the user’s emotional and physical reality.

For Phase 1, UbU models this as a derived `human_complete_plan_quality` assessment rather than a new first-class canonical object.

Phase 1 plan-quality analysis is derived from the candidate Plan, current affect Snapshot, Task duration and success estimates, modeled affect deltas, Log history, and user overrides. It may be cached with Plan/risk-report artifacts, but it is recalculable and non-canonical.

The minimum Phase 1 signals are:

- `feedback_latency`: expected time until the Plan produces observable evidence through Task completion, Task failure, a blocked precondition, an affect Snapshot, a user override, or an External Event.
- `checkpoint_coverage`: whether important work windows contain a near-term observation point instead of relying on end-of-day hindsight.
- `affect_margin`: how close the Plan comes to configured energy, stress, tiredness, mood, and recovery limits.
- `failure_pattern`: whether recent failures point to wrong estimates, missing dependencies, stale affect data, interruption, overload, or a changed Objective rather than a character judgment about the user.
- `stretch_pressure`: a derived label of `comfort`, `sustainable_stretch`, or `destructive_pressure`.
- `post_plan_state_delta`: whether completing the Plan is expected to leave the user better off, neutral, depleted, or at elevated risk despite nominal task completion.

Informative failure is failure that improves the model: it shortens future feedback loops, reveals an unmodeled constraint, updates duration or success estimates, or changes the next Plan. Humiliating or demoralizing failure is indicated by repeated overload, repeated late discovery, avoidable public exposure, dignity-risking wording or presentation, or a pattern where the same user-limits are ignored after they have been observed.

UbU should present failed execution as a failed plan assumption or changed world state unless the user explicitly records another interpretation. Suggested improvements should be framed as model repairs, constraint changes, smaller Tasks, added checkpoints, revised timing, recovery, clarification, delegation, or Objective reconsideration.

Sustainable stretch means a Plan asks for more than the user's current baseline while retaining near-term feedback, recovery margin, and a plausible improvement path. Destructive pressure means the Plan depends on overriding observed limits, lacks recovery, hides failure until too late, repeatedly requires success above observed capacity, or leaves the user worse off even if tasks are completed.

Post-MVP work may add richer growth models, personalized baseline learning, longitudinal dignity and morale trend reports, and UI-specific coaching language. Phase 1 only needs derived assessment, risk-report findings, recalculation triggers, and non-blaming revision suggestions.

### 2.6 Release Outreach Pipeline

The Release Outreach Pipeline is an accepted future feature bundle with the tagline:

> UbU should make every release explain itself.

Release management should not stop at code, tests, changelogs, and deployment artifacts. For UbU-runs-UbU, every meaningful minor release should create a reviewable release outreach package when there is enough change to explain.

A release outreach package is a derived artifact set attached to ordinary WorkItems, Logs, release artifacts, and export/projection gates. Phase 1 does not add a new canonical entity; it requires a structured package manifest, claim register, evidence/provenance index, reviewed text drafts, approved media references, privacy/export review, approval records, and a publication plan.

Release outreach must be evidence-bound. Each public claim should carry a support label such as `implemented_behavior`, `mock_or_fixture_behavior`, `future_plan`, or `speculative_goal`, plus evidence refs when evidence is claimed. Public artifacts must not represent planned, fixture-backed, mock, or speculative behavior as implemented behavior.

Screenshots, UI-test exports, fixture captures, and demo recordings are provenance-bearing evidence. They should record source workflow or worker, repo/build/test refs, fixture or live-data mode, visible Identity and Compartment exposure, redaction, hash, capture time, approval state, and the claims they support.

Publication is gated by default. UbU may draft, assemble, render, and prepare communication artifacts automatically, but video rendering, platform upload, public posting, or external channel mutation require explicit human approval unless a project has configured a narrow trusted auto-publication rule.

The Release Outreach Pipeline is not UbU-specific marketing glue. It is intended to generalize to project-management configurations through audience-specific communication Objectives for users, developers, maintainers, funders, internal stakeholders, customers, community members, or other audiences.

### 2.7 First-person legibility

UbU must be legible to users who do not think in programming, planning theory, or project-management jargon. The system may use explicit internal objects such as Objectives, Tasks, Plans, Logs, UniverseState, Compartments, and Snapshots, but the first user-facing experience should make the core loop obvious:

1. What matters?
2. What is true now?
3. What should I do next?
4. Why this action?
5. What happened when I tried?
6. What should change in the model?

For Phase 1, first-person legibility is implemented through a minimal bootstrap interview and next-action focus mode. These are UX requirements over existing core objects, not new canonical ontology objects.

Calendar preview and Log review are also first-person legibility requirements. They are notable Tasks that should run regularly so the user can inspect what UbU is about to recommend, compare planned behavior against actual behavior, correct mistaken assumptions, and decide whether Preferences, Objectives, Tasks, affect Snapshots, or estimates need revision.

### 2.8 Dogfooding

UbU should be useful for managing its own development.

The Phase 1 MVP should coordinate UbU’s GitHub issues, pull requests, reviews, CI events, release milestones, design questions, and contributor interactions.

### 2.9 LLM boundary

LLMs are useful but bounded.

LLMs may serve as:

- planning oracles,
- advisory systems,
- screenshot/UI interpreters,
- external workflow assistants,
- value-reflection assistants,
- document summarizers,
- automation helpers.

LLMs must not be the canonical real-time decision engine.

Short-turnaround execution decisions should be computed by UbU’s explicit decision logic.

---


### 2.10 Associations and organizational introspection

UbU models formal and informal organizations as **Associations**: Identity-scoped, perspective-bound coordination structures that emerge from Relationships, shared Objectives, commitments, norms, Logs, External References, and evidence.

An Association is not assumed to be a globally objective object with an authoritative member list. A legal entity, GitHub organization, Discord server, website, conference event, or skill network may provide strong External References, but it does not eliminate the need for perspective-bound modeling and attestations.

Organizational introspection is a first-class UbU feature. For individuals, UbU asks whether actual behavior matches stated Objectives and values. For Associations, UbU asks whether actual work, decisions, resource allocation, overrides, and undocumented structure match the Association's declared mission, values, and commitments.

LLMs may help extract candidate AssociationAttestations from permitted evidence-bearing records, but the generated claims remain reviewable, provenance-backed, confidence-scored, and disputable.

### 2.10.1 Extrospection and Relationship review

Extrospection is the Relationship-level sibling of introspection and organizational introspection. It exists because Relationships have **epistemic asymmetry**: UbU can model the user's own perspective from first-party declarations, behavior, affect history, and reflection, but it can only maintain evidence-backed hypotheses about another party's perspective.

Extrospection does not reveal, prove, or authoritatively attest to another person's inner state. It tests the user's counterparty-perspective hypotheses, declared Relationship scopes, trust assumptions, reciprocity assumptions, and boundary expectations against permitted Relationship evidence. Its UX should confront the user with evidence in tension, including supporting evidence, disconfirming evidence, ambiguity, and possible clarification paths, while preserving affect-aware framing and user sovereignty.

Extrospection findings are candidate, reviewable state. They may inform introspection when the evidence primarily concerns the user's own behavior, but they must not silently mutate durable Relationship models before user review.

### 2.10.2 Autonomy-first safeguards

Relationship, extrospection, and RelationshipScopeTransition safeguards should be wise default-on recommendations, not paternalistic hard gates. UbU may strongly recommend pre-action introspection, extrospection, TrustCalibrationAssessment, power/vulnerability review, pacing review, graceful nonachievement planning, and manipulation-risk review. The user may bypass or disable advisory safeguards unless doing so violates a structurally enforceable hard boundary, a self-imposed user-configured required gate, or an unavoidable external provider/platform/legal constraint.

UbU classifies safeguards into four governance categories:

- **Product hard boundaries**: structural invariants UbU can enforce by data model, routing, authorization, validation, storage, provenance, or append-only audit mechanics. These include Compartment denials, privacy/export policy, data-access authorization, EvidenceUsePolicy restrictions, identity and Compartment isolation, integration authorization, provenance integrity, audit-record integrity, and immutable Log/correction rules.
- **External hard constraints**: provider, platform, app-store, integration, or legal constraints that UbU must obey for a specific jurisdiction, distribution channel, or external surface. They are source-labeled constraints, not independent expressions of UbU philosophy.
- **User-configured required gates**: self-imposed user or project policy gates that may block a workflow while enabled. They are recorded separately from product invariants and external constraints so documentation does not imply that UbU itself treats the underlying behavioral judgment as mechanically certain.
- **Advisory behavioral safeguards**: fallible risk assessments such as manipulation, coercion, harassment, stalking, deception, power-asymmetry risk, rumination risk, relationship-transition ethics, or other behavioral misuse checks. These are generally default-on, uncertainty-transparent, user-overrideable, and introspection-relevant when bypassed.

Hard-boundary documentation must name the specific enforced invariant and its enforcement mechanism. Behavioral-safeguard documentation should use language such as recommend, warn, ask for review, require user-configured review, or refuse a provider-forbidden action. It must not imply that UbU can reliably prevent behavioral misuse, illegal conduct, or harm merely by classifying user intent.

### 2.11 Cloud LLM provider boundary

Cloud LLMs are optional execution providers, not the canonical UbU brain.

The core model should support provider-neutral routing across local model providers, user-configured BYOK cloud APIs, user-owned remote workers, optional UbUCorp managed gateways, and future compatible providers. External/cloud routing must remain governed by Compartment policy, context minimization, redaction where appropriate, cost policy, provenance, and user-visible disclosure.

Local-first operation remains the philosophical baseline. Cloud LLM use may improve performance and scalability, but it must not become a hidden dependency of the FOSS core or a way to bypass `no_cloud_llm` Compartments.

The minimum provider-neutral model is an `LLMProviderDescriptor` plus a routed invocation envelope. Provider descriptors identify `provider_id`, `provider_class`, `endpoint_or_worker_ref`, execution location, operator, region or jurisdiction when known, supported models, modalities, context window, structured-output and tool-use capabilities, safety behavior, retention/training/disclosure profile, cost and rate-limit metadata, credential reference kind, default-enabled state, and user-visible name. Provider classes are `local_process`, `byok_cloud_api`, `user_owned_worker`, `ubucorp_managed_gateway`, and `third_party_compatible`.

Provider adapters should expose the same narrow interface regardless of backend: list available models and capabilities, estimate cost and context fit, prepare a minimized request, invoke or stream completion, cancel when supported, and return usage, model, provider, and provenance metadata. The adapter interface normalizes execution; it does not erase trust-boundary differences.

An `LLMRouteRequest` records purpose, actor Identity, related Task/Objective/workflow refs, required capabilities, candidate provider policy, cost and latency limits, ContextBundle or source refs, required output schema when any, review requirement, and requested advisory use. An `LLMRouteDecision` records the selected provider and model, local/internal/external/cloud boundary classification, Compartment policy result, redaction and minimization summary, estimated cost, user-disclosure text, approval state, and Log refs for allowed or denied routing.

Compartment policy is evaluated before context assembly and again before provider invocation. Content from a `no_cloud_llm` Compartment cannot be sent to cloud provider classes, including BYOK cloud APIs, UbUCorp managed gateways, or third-party compatible cloud providers. Content from a `no_external_export` Compartment cannot be sent to external providers, remote workers, managed gateways, or third-party services except as redacted structural references that expose no protected payload. `local_only` content is limited to eligible local Devices and local providers. A capability grant, provider setting, or user approval cannot override these hard Compartment denials.

Cloud routing requires a minimized `ContextBundle` or equivalent envelope that records purpose, source object refs, Compartment refs, Identity and Association refs exposed, destination provider, model ref, data categories exposed, redaction policy, minimization notes, retention policy, user-visible summary, creation time, expiry when applicable, and downstream candidate refs. Raw payloads should be replaced with references, summaries, hashes, or redacted structural fields whenever the task can still be performed.

BYOK credentials are stored as scoped credential references, not embedded in provider descriptors, ContextBundles, prompts, Logs, or exported artifacts. A credential may be scoped by provider, account, model family, Identity, Compartment, workflow, cost budget, and Device or worker. The default storage target is the local instance or OS/device secret store; a user-owned worker may hold its own credential if explicitly configured. Credential rotation and revocation preserve audit continuity through credential version refs without exposing the secret.

UbUCorp managed inference is represented as one provider class, not as privileged protocol authority. It must use the same provider descriptor, routing envelope, Compartment gates, disclosure, cost controls, provenance, and output admission path as any other cloud provider. The FOSS core must remain useful when the UbUCorp provider is absent.

Before or at execution time, any workflow that uses a cloud or external LLM must disclose the provider, model or model class, operator, local-vs-cloud status, destination region when known, whether BYOK or UbUCorp-managed credentials are used, data categories and Compartments exposed, retention/training profile, estimated cost when available, and the fact that output is advisory until admitted through UbU validation and review. Users may grant per-run, session, workflow, or policy-based approval only when Compartment policy allows it.

---

### 2.12 Context-rich messaging and legacy communication upgrade

UbU should treat messages as planning-relevant communication events, not merely flat text strings.

A normal WhatsApp, SMS, Discord, IRC, Slack, Matrix, or email message often lacks explicit context. The context exists in the sender's intent, the recipient's Relationship history, the channel purpose, the Association involved, the current Objective, the implied Task, the expected response time, and the consequences of ignoring or delaying the message.

UbU should eventually support **context-rich asynchronous messaging** in which a message may carry a bounded metadata envelope: message kind, topic, related Objective or Task, related Association, priority, interrupt recommendation, response expectation, assumptions, ambiguities, provenance, and disclosure policy.

For Phase 3, a minimal version of this is a premier feature because it allows a receiving UbU instance to decide whether the message should interrupt an existing Plan, become a Task, update an Objective, or wait for a normal communication-review window.

Legacy-system integration can still provide value before native UbU-to-UbU messaging exists. UbU may ingest flat legacy messages and use a structured extraction layer to generate candidate planning state. When both parties have UbU, legacy transports may be upgraded by attaching, linking, or side-channeling a UbU Message Context Envelope while preserving the normal human-readable legacy message.

This metadata must be intentionally bounded. UbU should not leak private Relationship history, hidden Objectives, private Association assumptions, or sensitive Compartment contents simply because the receiver would benefit from richer context.


### 2.13 Realtime and agentic AI boundary

Realtime multimodal LLMs are optional interaction backends. They may listen, speak, watch, transcribe, translate, detect interruptions, monitor Task progress, assist meeting capture, or generate candidate UI, but they do not become UbU's authoritative planner.

UbU distinguishes **model-time awareness** from **planner-time semantics**. A model may notice elapsed time, silence, overlapping speech, or changed conditions; UbU decides whether those observations update a Task, Log, Snapshot, Calendar, Objective, or recalculation trigger.

Realtime/discovery functionality must be mode-bound and visible. The user should know whether UbU is off, text-only, in a voice session, in active Discovery mode, in meeting/logging mode, local-only, or cloud-assisted.

### 2.14 LLMs, structured outputs, and memory

LLMs are replaceable cognitive backends. They may interpret, summarize, propose, critique, classify, transcribe, generate fixtures, or call tools, but accepted state remains typed, scoped, provenance-bearing, and correctable.

Structured output is not sufficient for state admission. A schema-valid object is still a candidate until UbU validates semantics, Compartment policy, provenance, conflicts, and user-review requirements.

UbU memory should be explicit objects such as Preferences, Objectives, Identities, Associations, PlanningConstraints, TaskPatterns, AffectPatterns, Snapshots, Logs, External References, and Attestations. Vague assistant memory does not replace the UbU data model.

### 2.15 MCP-style integration boundary

UbU should be both an MCP-style client and an MCP-style server.

As a client, UbU may connect to tools, local services, repositories, calendars, email, files, model providers, and agent services. As a server, UbU may expose narrow capabilities such as candidate Task creation, Plan-summary reading, Log-candidate submission, clarification requests, Plan-repair proposals, Objective-status queries, and AssociationAttestation candidates.

Every tool surface is bounded by Identity, Compartment, capability grant, Objective scope, operation kind, time window, provenance, and review policy. External agents should usually submit candidates rather than mutate canonical state directly.

### 2.16 Delegation Substrate

The **Delegation Substrate** is the near-term model for preparing Tasks for delegation. It formalizes purpose, executor, authority, expected output, evidence, privacy scope, constraints, review, and failure/escalation path.

Delegation Substrate is useful even when the user performs a Task solo. A self-performed Task can still benefit from a clear statement of why the Task matters, what authority or context applies, what output is expected, and what evidence would prove completion.

The full Skill Barter marketplace is not an MVP requirement. MVP-relevant work should implement the primitives that make future delegation and marketplace features possible without operating a public marketplace.

### 2.17 General Contractor and Skill Barter direction

A **General Contractor** is an Identity, Agent, or Association delegated authority to coordinate multiple subordinate executors toward an Objective or Container of Tasks. The role is a first-class coordination specialization, not merely another worker label.

The **Skill Barter marketplace** is a future marketplace direction and EthConf outreach hook, especially for cypherpunk/privacy-oriented audiences. It signals voluntary coordination, sovereign identity, privacy, open markets, FOSS contribution, pseudonymous capability, and lawful user-controlled settlement references.

Skill Barter should connect to future Ethereum-aligned privacy technologies such as FHE, ZK, secure compute, private reputation, and selective disclosure while avoiding token-first, speculation-first, illicit-market, or exploitative labor framing.

### 2.18 Context assembly and background agency

Context assembly is a privacy-relevant act. UbU should track why a context bundle was assembled, which objects and Compartments it included, which Identities or Associations it exposed, where it was routed, how it was minimized, and which downstream candidate updates it supported.

Computer-use agents and background processes are high-risk actors because they may operate credentials, browsers, files, APIs, money, privacy budgets, and external systems. They require authority scopes, audit trails, prompt-injection exposure tracking, rollback or mitigation paths, and explicit notification/escalation policy.

The long-term UX should evolve into a **state-transition cockpit** rather than chat plus calendar. The Phase 1 one-next-Task loop remains the narrow proof; later UIs should help the user inspect and approve state transitions such as Plan repairs, Log corrections, Delegation Substrate packets, agent actions, AssociationAttestations, and external projections.

---

## 3. Model-committee dogfooding

UbU may use model-committee automation to help maintain its own design process.

The model committee is not the canonical decision engine. It is an advisory Automation Worker pattern that can:

- read the canonical design repo,
- identify unresolved questions,
- propose answers,
- critique alternatives,
- generate candidate changesets,
- run consistency checks,
- estimate MVP readiness,
- rank remaining open questions,
- identify follow-up questions.

Accepted design state exists only when committed to the canonical design repo.

Question rankings are derived planning metadata. They may guide the next committee run, but they are not canonical design truth.

Model-committee automation has three expected lifecycle modes:

1. **Pre-MVP:** design-freeze assistant.
2. **MVP dogfooding:** repo-maintenance and planning worker.
3. **Post-MVP:** continuous design-governance assistant.

The goal is to accelerate implementation, not to create unlimited pre-implementation design work.

Model-committee automation is bounded by the Phase 1 design automation stop rule in `UBU-D0175`. Broad pre-MVP design expansion no longer blocks Phase 1; further design work may block implementation only when it is required for a concrete implementation slice, an accepted hard invariant, a needed contract, or avoidance of an irreversible schema contradiction.

The first implementation of this process is intentionally constrained by the v0.1 restrictions recorded in `DECISIONS.md`. The accepted v0.2 direction expands the bootstrap loop with Claude Code CLI, frontier cross-scoring, disagreement flags, schema-native structured output, and operator-run artifact publication while preserving the advisory authority boundary.

### 3.0 Current dogfooding status

`model-committee v0.1` is the first runnable bootstrap artifact for UbU’s dogfooding process.

It is not the full UbU planner, but it exercises the recursive project-governance loop: parse canonical state, check consistency, select answerable work, generate candidate changesets, score them, validate patches, and produce reviewable artifacts.

This establishes active pre-MVP dogfooding while preserving the rule that accepted design state exists only when committed to the canonical design repository.

The current strategic emphasis is implementation-first Phase 1 dogfooding: use visible dogfooding, contributor recruitment, and prototype-funder discovery to accelerate the trunk of UbU rather than to expand design philosophy indefinitely.

### 3.1 v0.1 Codex-first provider model

`model-committee` v0.1 uses Codex CLI as the primary model provider.

The runtime calls `codex exec` for:

- primary work proposal generation;
- work scoring.

Codex outputs are schema-constrained. Prompts are passed through stdin. Final responses are written to JSON files. JSONL event streams and stderr output are preserved in run logs.

Codex is used only as a proposal and scoring provider in v0.1. It must not directly mutate canonical repo files. It produces JSON work proposals and score results. Patches are validated and selected by `model-committee`, then written as review artifacts.

Every runtime Codex call must pass `--skip-git-repo-check`.

`model-committee` does not pass deprecated `--disable web_search` flags. If Codex web search must be disabled, that is handled through Codex configuration or profile state outside `model-committee`.

Local Ollama models remain secondary work proposal providers. They provide local diversity, dissent, fallback, and offline review, but Codex is the required scoring provider for automatic patch selection in v0.1.

### 3.1.1 v0.2 Claude Code and cross-scoring provider model

`model-committee v0.2` adds Claude Code CLI as a second frontier provider for both work proposal generation and scoring.

The v0.2 provider model is not raw majority voting. It is cross-scored adversarial review:

- Codex may generate work proposals.
- Claude Code may generate work proposals.
- Claude Code scores Codex-authored proposals.
- Codex scores Claude-authored proposals.
- A provider's self-score may be preserved as diagnostic metadata, but it does not count as quorum evidence.

Claude Code should use schema-native structured output through `--json-schema`. The v0.2 target environment uses Claude Code CLI `2.1.146`, which supports that flag. Claude output parsing should therefore read the schema-conforming `structured_output` field from the CLI JSON envelope when `--output-format json` and `--json-schema` are used. Prompt-only JSON extraction is a compatibility fallback, not the primary path.

Claude Code is invoked as a subprocess provider. `model-committee` itself still must not call Anthropic APIs directly. The explicit boundary is that approved provider CLIs may perform their own network/API calls according to their configured upstream authentication and billing, while `model-committee` owns orchestration, prompts, schemas, validation, manifests, logs, and review artifacts.

Claude tool authority should be restricted explicitly. The default scripted schema-output call should use no tools, or `--tools Read` only when file inspection is required. `--allowedTools` alone is not a sandbox boundary because it controls permission prompts, not the total available tool set.

v0.2 manifests and reviews should include a score matrix with author provider, scorer provider, proposal ID, score, validity, rationale, risks, and required fixes. Disagreement is signal: a large score gap between Codex and Claude Code should not be hidden inside a single aggregate score.

v0.2 automated selection requires at least one valid work proposal, at least one valid cross-score from a different frontier provider, no hard validation failure on the selected patch, and no critical disagreement flag unless manually overridden outside automatic selection. Default thresholds are a frontier score gap of 25 or more points, selected score below 70, selected patch validation failure, or no valid frontier cross-score. A dedicated human-review-required exit code, provisionally `9`, should distinguish quorum/disagreement review from invalid selected patch failure.

`review.md` should include an operator-run final step for publishing a run directory to the sibling `../model-committee-artifacts` repository. `model-committee v0.2` should generate these commands but must not auto-push or mutate remote GitHub state.

### 3.1.2 Model-committee run log and provenance format

A `model-committee` run log is a filesystem run directory plus a manifest. The log is derived state, but it must be sufficient for a human reviewer or deterministic fake-provider test to reconstruct what canonical inputs, prompts, schemas, provider outputs, validations, scores, and selected artifacts existed at selection time.

The minimum v0.1/v0.2 run directory preserves:

- `manifest.json` with run identity, schema version, tool version, base commit, working-tree state, selected question metadata, provider configuration summaries, artifact paths and hashes, proposal records, validation results, score/quorum results, selection result, exit code, timestamps, and publication status.
- `inputs/` snapshots of `DESIGN.md`, `DECISIONS.md`, and `OPEN_QUESTIONS.md`, plus hashes and the base commit used for the run.
- `schemas/` copies or hashed references for every schema used to validate proposals, score results, manifests, provider metadata, and review artifacts.
- `prompts/` for every provider invocation, including role, phase, template or source hash, question ID, schema ref, provider ID, model name, and prompt text.
- `raw/` provider artifacts, including Codex JSON outputs, Codex JSONL event logs, Claude Code JSON envelopes, Ollama raw responses, stdout/stderr, timeout and exit-status records, and usage or cost metadata when available.
- `parsed/` normalized proposals and scores, including extracted Claude `structured_output` payloads, JSON validation results, parse errors, provider failure records, and repaired or rejected candidate payloads.
- `patches/` with one patch file per proposal, patch validation status, selected patch copy, and any mechanical validation diagnostics.
- `scores/` with the score matrix, cross-score records, self-score diagnostics when retained, disagreement flags, quorum decision, selected proposal ID, and human-review-required reason when applicable.
- top-level `selected.patch`, `commit_message.txt`, and `review.md`.

Provider invocation records must include `invocation_id`, provider ID, provider class, model name or alias, phase, command/argv shape with secrets redacted, timeout, start and end timestamps, exit status, stdout/stderr artifact paths, raw output path, parsed output path, schema path or hash, validation result, and failure class when applicable. They must not store API keys, bearer tokens, credential files, private environment dumps, or unredacted secrets.

v0.2 review artifacts should expose the score matrix, disagreement flags, quorum result, selected patch validation status, selected score, cross-score provenance, and the operator-run artifact-publication commands. The commands are part of the review artifact and must not be executed automatically.

### 3.1.3 Public dogfooding artifact policy

Public dogfooding artifacts should publish a sanitized review package, not the complete private run log by default. The public package is evidence that the loop is real and reviewable: canonical inputs led to provider proposals, validated patches, cross-scores, quorum or disagreement outcomes, human review, and a later canonical commit when accepted.

Default public artifacts are:

- `review.md`, including run ID, base commit, selected question, selected proposal summary, validation result, score matrix summary, quorum or disagreement result, failed-provider summary, and publication commands.
- `selected.patch` and `commit_message.txt` when a candidate exists.
- `manifest.public.json` or an equivalent redacted manifest containing hashes, provider/model IDs, artifact paths, scores, validation status, selected proposal ID, exit code, and links to the accepted commit, Issue, or PR when available.
- Score-matrix and validation summaries sufficient to reproduce the selection judgment without exposing full prompts or private reasoning.

The complete local run directory may be archived outside the design repo. Public publication must exclude secrets, private environment dumps, credentials, private chain-of-thought, unsafe provider text, unredacted raw prompts when they contain sensitive context, and raw provider logs that are not needed for public review. Redacted raw artifacts may be published only when they help diagnose a run and pass the same artifact-safety check.

Successful, failed, and human-review-required runs should all be publishable. Failed runs should be labeled by failure class, such as parse failure, invalid patch, no quorum, provider timeout, or critical disagreement, and should explain the next review action. This improves credibility when failures show bounded authority, preserved evidence, and no silent state mutation.

GitHub connection is by reference, not automatic mutation. A public artifact should name the related open question, Issue, PR, accepted decision, selected patch, and final commit when available. `model-committee` may generate links and operator instructions, but opening PRs, pushing artifacts, and accepting canonical design changes remain human actions.

### 3.2 Changeset-based work phase

Model-committee automation should make implementation explicit.

After a selected question or problem is chosen, models may be asked to produce concrete changesets. A changeset may be a documentation patch, code patch, schema patch, or other explicit repo-state transition artifact.

Other models, or a designated scoring model, then score those changesets. The best changeset is selected and written as reviewable artifacts such as:

- `selected.patch`
- `commit_message.txt`
- `review.md`
- run logs

This prevents implementation from being hidden inside an external editor agent and makes the committee loop applicable to documentation changes, code changes, bug fixes, and future UbU implementation work.

VS Code or another editor may still be used for human review, but it is not part of the canonical committee loop.

The accepted work-phase contract is intentionally mechanical. A work proposal is a schema-validated envelope containing provider identity, model name, selected question or problem ID, base commit, summary, rationale, changed-file list, a raw unified diff, suggested commit message, validation notes, declared new questions, declared resolved questions, declared decisions, and a human-review flag. The patch is the authoritative changeset; prose summaries are explanatory only.

Before scoring, `model-committee` validates the proposal envelope, base commit, allowed file set, referenced question and decision IDs, patch syntax, patch applicability against the recorded base snapshot, and any work-item-specific semantic checks. For current design-document work, the v0.1 writable set is limited to `DESIGN.md`, `DECISIONS.md`, and `OPEN_QUESTIONS.md`. Later code, schema, fixture, and bug-fix work generalizes by declaring an allowed path set plus validators, tests, and artifact expectations for the selected work item.

Only mechanically valid proposals are eligible for automatic selection. Invalid proposals are retained in logs with diagnostics, but they are not quorum evidence. If a selected proposal later fails patch validation, automatic selection fails rather than silently applying a different patch; the run writes review artifacts and uses the invalid-selected-patch failure path.

Work scoring considers at least correctness against the selected question or problem, consistency with accepted decisions, mechanical validity, minimality, reviewability, risk and reversibility, validation or test adequacy, maintainability, and whether the changeset preserves model-committee authority boundaries. Score records must name required fixes and validation assumptions. Self-scores may be retained as diagnostics, but only non-self scores count for quorum evidence. v0.1 uses Codex scoring over valid Codex and Ollama proposals; v0.2 requires frontier cross-scoring between Codex and Claude Code as described above.

`model-committee` v0.1 and v0.2 do not automatically apply patches, commit locally, push artifacts, open pull requests, or mutate GitHub. A human operator may apply `selected.patch`, inspect `review.md`, rerun appropriate checks, and create a normal local commit using `commit_message.txt` as a suggestion. Any future automatic local commit path requires a later accepted decision with explicit clean-worktree, validation, and approval rules.

### 3.3 Prioritized recursive loop

Model-committee runs use a prioritized recursive loop:

1. **System-wide consistency check** verifies that the current project state is coherent.
2. **Question/problem prioritization selection** chooses the next work item only after consistency is known.
3. **Work** produces and scores concrete changesets.

Each run should record loop state in its manifest or review artifact: base commit, canonical input hashes, consistency status, blocking failure IDs, warning IDs, selected loop mode, selected work item, provider/version baseline, and whether any derived scores were reused or invalidated.

System-wide consistency checks run before prioritization and before ordinary work. They are triggered by:

- a new base commit, merge, rebase, checkout, or human-applied patch that changes canonical files;
- a directive decision or other direct edit to `DECISIONS.md`;
- any edit to `DESIGN.md`, `DECISIONS.md`, `OPEN_QUESTIONS.md`, or `PLANNING_KERNEL_CONTRACT.md`;
- prompt, schema, validator, quorum, provider-weight, provider-config, or model-committee tool-version changes;
- enabled LLM model, model-alias, or provider capability changes;
- Codex CLI, Claude Code CLI, Ollama, or other approved provider CLI version or behavior changes;
- explicit operator request, scheduled audit, doctor run, or prior run ending with consistency, quorum, or validation failure;
- derived-document changes when derived-document consistency checks are enabled.

Hard consistency failures block ordinary prioritization and ordinary work. Hard failures include missing or unparseable canonical files, duplicate question or decision IDs, malformed question metadata, dependency references to missing questions, dependency cycles, invalid `Resolved by` references, solved/decomposed/deferred/superseded status contradictions, selected-work dependency violations, patch-base mismatch, forbidden-path proposals, and canonical input changes after the recorded consistency snapshot.

Warnings do not block prioritization when they cannot invalidate the selected work item. Warnings include stale derived documents, unscored or `TBD` ranking fields, optional provider failures when quorum remains satisfiable, nonblocking provider version drift, stale historical scores, and derived artifact publication gaps. Prioritization may run with warnings only when the warning list is recorded and stale scores are excluded from automatic selection.

When hard failures exist, the next selectable work is consistency repair, decomposition, or diagnostic work for those failures. A consistency failure should be converted into a problem record with stable failure key, severity, affected files or object IDs, evidence, suggested repair, and whether a human decision is required. If the repair is mechanical and inside the current allowed file set, model-committee may solicit repair patches. If the repair exposes an unresolved design choice, it may create or update an open question; new MVP blockers must satisfy the `UBU-D0175` blocker-certificate rule.

Ordinary work may proceed against a known inconsistency only when the selected work item repairs or narrows that inconsistency, declares the relevant failure IDs, avoids relying on invalid derived ranking, and reruns consistency after the candidate patch. It must not mix unrelated design answering with consistency repair unless both are necessary for the same failure.

LLM model updates and provider CLI updates do not retroactively change committed canonical design state or historical run logs. They do invalidate reusable derived rankings, readiness estimates, and score evidence that depend on the old provider baseline. Before automatic selection reuses any such artifact, model-committee must rerun consistency and, when the selected proposal depends on old scores, rescore or require human review.

Future integrated UbU Automation Worker behavior should map this loop to ordinary worker semantics: consistency checks are high-priority assigned work; repair outputs are mutation or patch candidates; canonical state changes only after parent validation and human repository review where required.

### 3.4 Consistency requirements

System-wide consistency checks should include both document consistency and logical consistency.

For `model-committee`, consistency checking should verify at least:

- canonical files exist and are parseable;
- question IDs are unique;
- decision IDs are unique;
- question dependencies point to existing questions;
- question dependencies do not contain cycles;
- `Resolved by` fields point to existing decisions or are explicitly unresolved;
- solved, decomposed, deferred, or superseded questions are marked consistently;
- question metadata satisfies the declared schema;
- derived documents do not contradict canonical files when derived-document checks are enabled.

A question should not be selected for ordinary answer/work execution if it has unresolved dependencies, unless those dependencies are answered in the same work item.

Blocked questions may still be selected for decomposition work when decomposition can produce replacement questions with fewer, simpler, or no dependencies.

### 3.5 Direct project directives

The UbU project may receive direct project-owner directives that are appended to `DECISIONS.md` as accepted decisions.

These directive decisions are treated as canonical once committed.

Directive decisions are a “word of God” mechanism for project governance, but they are still represented as explicit repo state rather than hidden chat context.

If a directive decision creates inconsistency, the next system-wide consistency check should detect it and convert the inconsistency into a problem report, question update, or work item.

### 3.6 Decomposition and design-burden reduction

Model-committee should not treat every increase in open-question count as failure.

A hard, ambiguous, or blocked question may be validly split into multiple simpler questions when the replacement questions are clearer, narrower, lower-risk, or easier to automatically answer.

This is especially valuable when a blocked question can be decomposed into replacement questions where at least one replacement question has fewer dependencies, simpler dependencies, or no dependencies.

The relevant metric is unresolved design burden, not merely raw open-question count.

For derived ranking, unresolved design burden combines dependency burden, ambiguity burden, automation burden, and implementation burden. Dependency burden includes unresolved dependency count, dependency depth, and whether a dependency blocks Phase 1 work. Ambiguity burden includes mixed decision types, vague scope, and missing acceptance criteria. Automation burden includes automation-likelihood, validator availability, risk, and human-involvement class. Implementation burden includes whether the question blocks a concrete Phase 1 slice, hard invariant, needed contract, or persistent schema choice.

A decomposition is valid when replacement questions preserve the parent intent, are individually narrower, include dependency and lineage metadata, and reduce total estimated burden. The strongest decomposition creates at least one immediately answerable replacement question.

A decomposition is bad when it creates more, harder, vaguer, more coupled, or lineage-free questions.

A decomposition is good when it exposes smaller answerable units and improves future automation.

Replacement questions should link to the original with `Decomposes: UBU-Qxxxx` or equivalent metadata once parser support exists. Until decomposed-status metadata is parser-supported, parent questions should remain open unless an accepted decision also resolves the parent; resolved parent questions are tombstoned normally.

### 3.7 Answerability-first prioritization

Question selection should use answerability as the first gate.

A question is eligible for ordinary work only if:

- it has no dependencies;
- all dependencies are solved;
- or all dependencies are being answered in the same work item.

Answerability is a hard gate for ordinary answering, not for decomposition work. For ordinary work, unresolved dependencies make the question ineligible unless included in the same work item. For decomposition selection, unresolved dependencies contribute to burden and can raise priority when replacement questions are expected to have fewer, simpler, or no dependencies. Dependency simplification improves ranking only through the decomposition path; it does not make the blocked parent ordinarily answerable.

After answerability is established, questions are ranked by:

1. automation-likelihood,
2. importance,
3. risk ascending.

Questions blocked by unresolved dependencies may be selected for decomposition rather than ordinary answering.

### 3.7.1 Automation coverage taxonomy

`Auto-choice eligibility` is a governance gate separate from answerability and automation-likelihood. The accepted values are:

- `Auto eligible`: automation may propose, score, and select a review candidate when dependencies, validators, and quorum pass. This does not bypass human repository review.
- `Human approval required`: automation may draft, decompose, score, and prepare review artifacts, but the selected result must be marked human-review-required before it can resolve the question.
- `Human only`: automation may summarize context, detect consistency issues, or list options, but it must not auto-select an answer.

Use the stricter category when classification is uncertain. A proposal may not lower a question's human-involvement category without human review.

### 3.8 v0.1 and v0.2 provider and network boundaries

`model-committee` v0.1 is a narrow local Python CLI.

It may communicate only with:

- local Ollama `base_url`;
- Codex CLI subprocesses.

v0.2 may also invoke Claude Code CLI as an explicitly configured subprocess provider.

`model-committee` itself must not call:

- GitHub;
- OpenAI APIs directly;
- Anthropic APIs directly;
- Gemini APIs;
- arbitrary HTTP URLs.

`httpx` may be used only for the configured Ollama `base_url`. Frontier cloud providers are accessed through approved CLI subprocess boundaries rather than direct API calls by `model-committee`.

This provider policy preserves the bootstrap architecture and prevents accidental API creep while allowing approved CLI providers to use their own upstream authentication and billing.

### 3.9 v0.1 and v0.2 testing and diagnostics

`model-committee` v0.1 should include fake provider mode for deterministic tests.

Fake provider mode should not call Codex or Ollama. It should load canned fixture responses, then run normal JSON validation, patch validation, scoring, selection, and artifact-writing logic.

`model-committee` v0.1 should also include a `doctor` command that checks the local environment:

- Python version;
- `git` availability;
- `codex` availability;
- required `codex exec` flags;
- Claude Code CLI availability when enabled;
- Claude Code schema-native structured-output support when enabled;
- Ollama reachability;
- configured Ollama model availability;
- required target repo files;
- runs directory writability.

A `version` command should print the installed `model-committee` version.

### 3.10 v0.1 and v0.2 authority boundary

`model-committee` has advisory authority only in both v0.1 and v0.2. It may produce derived analysis and review artifacts, but it must not directly create accepted design state. Accepted design state exists only after an ordinary human-reviewed repo change is committed to the canonical repo.

The following actions may be automated in v0.1:

- read `DESIGN.md`, `DECISIONS.md`, and `OPEN_QUESTIONS.md`;
- parse open questions and their metadata;
- run consistency checks over canonical files;
- rank answerable questions using answerability, automation-likelihood, importance, and risk;
- generate schema-constrained Codex work proposals and score results;
- collect Ollama work proposals as secondary proposal inputs;
- validate proposal JSON and candidate patches mechanically;
- select a patch only from mechanically valid proposals using a valid Codex score result;
- write run logs, review artifacts, selected patches, and commit-message suggestions.

v0.2 additionally may automate:

- invoking Claude Code CLI as a schema-native subprocess provider;
- generating and validating Claude Code work proposals and score results;
- cross-scoring Codex and Claude Code proposals;
- constructing score-matrix artifacts;
- detecting disagreement and quorum failures;
- writing human-review-required results when quorum, score, or disagreement thresholds fail;
- adding operator-run artifact-publication commands to `review.md`.

The following actions require human repository review in v0.1 and v0.2:

- accepting a proposed answer as design state;
- applying, editing, or committing any candidate patch to canonical files;
- changing question status, priority, dependencies, or resolved-by metadata in the canonical repo;
- treating readiness estimates as release, scope-freeze, or go/no-go decisions;
- converting a proposed open question into canonical `OPEN_QUESTIONS.md` state;
- changing provider weights, quorum rules, or network/provider boundaries.

The following actions are outside automatic authority:

- direct OpenAI, Anthropic, Gemini, GitHub, or arbitrary HTTP API calls by `model-committee` itself;
- auto-merge, auto-push, automatic PR creation, or GitHub mutation;
- automatic artifact publication;
- automatic patch application to canonical repo files;
- allowing Codex CLI, Claude Code CLI, Ollama, or any model provider to directly edit canonical repo files;
- internal manual override of failed scoring, quorum, disagreement, or patch-selection checks;
- readiness-score writes to derived public files such as `README.md`;
- deriving canonical user value, project directives, or release commitments from model output alone.

Provider outputs are weighted by configured trust weights, provider role, cross-score status, and observed reliability metadata, not by one-provider-one-vote counting. Static configured weights are sufficient for v0.1 and v0.2; adaptive weighting remains deferred.

Provider failures are logged as run events. A failure should record the provider, model, run phase, failure class, timeout or exit status when available, stderr/response artifact path when available, and whether quorum remained satisfied.

A valid v0.1 committee result requires at least one mechanically valid work proposal and a valid Codex score result. Two or more valid proposals are preferred but not required. Failed secondary providers do not invalidate a run when quorum is met.

A valid automated v0.2 committee result requires at least one valid work proposal, at least one valid cross-score from a different frontier provider, no hard validation failure on the selected patch, and no critical disagreement flag unless manually overridden outside automatic selection. Human-review-required results should use a distinct exit code, provisionally `9`.

Codex CLI and Claude Code CLI authority differ from direct API authority because they are invoked only as subprocess providers. Their outputs are proposal and scoring artifacts only. Direct OpenAI and Anthropic API authority is zero because direct cloud-provider API calls by `model-committee` are forbidden.

---

## 4. MVP Release Phases

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

Phase 3 is the bridge from the bootstrap MVP toward the full personal life-logistics product. It may be subdivided during implementation planning:

- **Phase 3A** should prioritize minimal Resource/Skill-aware task readiness, private skill-tree foundations, and narrow Identity coordination that directly improves single-user life logistics.
- **Phase 3B** begins the full version 1.0 release track. It should expand Resource and Skill usability, Technique-database integration, DIY-versus-purchase/hire planning, and the user-facing capability graph without requiring the public Skill Barter marketplace.
- **Phase 4+** continues the full version 1.0+ track with mature inventory, financial-management extensions, public or federated Skill Barter, reputation/evidence, dispute workflows, and other multi-party marketplace features.

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

- Technique as a first-class planning object;
- Resource and Skill as first-class planning objects beyond lightweight compatibility placeholders;
- full Compact Calendar planner grammar and high-coverage transport format;
- complete Zone and Device system beyond the current local execution enclave;
- organization-mode and worker-mode web admin consoles;
- richer relationship-management, personal CRM, and longitudinal affect/growth models;
- full Release Outreach Pipeline video generation, rendering, and publication workflow;
- broad email, text-message, file, invoice, note, or personal-data ingestion outside narrow approved dogfooding fixtures;
- full realtime multimodal capture, always-on assistant behavior, public Skill Barter marketplace operation, payment/settlement flows, reputation markets, dispute resolution, and General Contractor subdelegation workflows;
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
   - Let the user snooze, skip, or adjust Calendar-preview cadence through ordinary Task recurrence settings.

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

The UbU project should treat outreach as an attempt to form an ad hoc Association around the project: contributors, reviewers, funders, workflow informants, privacy/cypherpunk contacts, and possible design partners. Conversation notes, follow-up commitments, public artifacts, private/redacted observations, and missed follow-ups can become evidence for whether the project actually pursued its stated goals.

For Phase 1, this does not require full multi-user Association implementation. It can be represented through structured outreach Objectives, Tasks, Logs, External Events, External References, and reviewable candidate AssociationAttestations generated manually or by local/allowed LLM review.

The dogfooding question is: can UbU produce an evidence-backed retrospective showing whether UbU actually formed useful coordination around its own project?

### 4.1.5 Optional VoxPopuli EthConf demo

UbU may optionally demonstrate a **VoxPopuli** flow at EthConf if it does not displace higher-priority dogfooding, contributor, or funder deliverables. In this flow, a user speaks freely about what they wish would happen or what feels disorganized. An LLM-assisted extractor converts that natural-language input into candidate Objectives, Tasks, constraints, preferences, and planning assumptions for the user to inspect, correct, accept, or reject.

VoxPopuli is a trust-building outreach hook, not a replacement for the narrow Phase 1 bootstrap interview. It should demonstrate how LLMs can help structure abstract human concerns while UbU remains the explicit, inspectable planning system.

### 4.2 Phase 2: Single-user multi-device synchronization

A single user runs UbU across multiple Devices / execution enclaves.

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

### 4.3 Phase 3: Minimal multi-user / Identity coordination

Multiple humans coordinate through explicit Identities, capabilities, limited disclosure, and commitments.

Phase 3 should enable coordination without surveillance or shared global truth.

---

## 5. Operating Modes

Each UbU instance runs in exactly one mode. Modes are mutually exclusive and cannot change after initialization.

### 5.1 `user_mode`

A personal UbU instance operated for an autonomous human user.

Properties:

- includes user affect modeling;
- represents personal Objectives and Preferences;
- may integrate with personal calendars, tasks, messages, sensors, and devices;
- supports personal Zones and Compartments.

### 5.2 `organization_mode`

An organizational UbU instance operated on behalf of a project or organization.

Important philosophical note:

An organization is not an autonomous human. Organization mode is shorthand for a human-operated planning system where the organization appears as a structure in UniverseState, directives, Identities, and Preferences.

Properties:

- does not model intrinsic affect;
- may use Preferences as organizational directives;
- may be run on a thin web/database host;
- may delegate work to Automation Workers;
- may expose a web UI;
- may eventually support classic user accounts and access control.

For MVP, all organization-mode users may be treated as admin-equivalent to avoid premature RBAC complexity.

### 5.3 `worker_mode`

A worker-mode instance performs delegated work assigned by another UbU instance.

A worker-mode instance is usually:

- one-device / one-enclave;
- daemon/service-oriented;
- administered through a web page and/or CLI;
- capable of running heavy compute or GPU workloads;
- externally represented as an Identity;
- assigned Tasks by a user-mode or organization-mode instance.

Worker mode may run on a local machine, a server, or a thin controller that starts/stops cloud compute resources.

---

## 6. Core Entity Summary

The core model includes:

- Objective
- Technique
- Preference
- WorkItem
- Task
- Container
- UniverseState
- Snapshot
- Plan
- Calendar
- Log
- Message Context Envelope
- Message Extraction Result
- Voice Profile Descriptor
- Identity
- Relationship
- RelationshipScopeTransition
- ExtrospectionFinding
- Zone
- Compartment
- Automation Worker
- External Event
- External Reference
- Association
- AssociationAttestation
- SharedAssociationDescriptor

Some entities are implemented in MVP. Others are documented now to avoid future contradiction.

---

## 7. Objectives

An **Objective** is a desired or maintained state of the universe.

Examples:

- “Release UbU MVP”
- “Review open pull requests”
- “Maintain relationship with contributor X”
- “Explore whether Alice is open to a romantic Relationship scope”
- “Transition Bob from close-friend scope to formal-acquaintance scope”
- “Collect affect information relevant to important usage”
- “Finish documentation draft”
- “Explain the latest release to users and contributors”

Objectives are the canonical anchor for value.

### 7.1 Objective modes

An Objective has one of two modes:

- `one_time`
- `evergreen`

#### One-time Objective

A one-time Objective is completed once and does not reactivate.

#### Evergreen Objective

An evergreen Objective may become satisfied and later become active again.

Example:

- “Maintain clean kitchen”
- “Keep relationship with X warm”
- “Collect affect information”
- “Manage GitHub issue queue”

### 7.2 MVP Objective fields

Required:

- `objective_id`
- `title_or_description`
- `mode`
- `status`

Optional:

- `notes`
- `tags`
- `linked_container_refs`

Derived / transient:

- `derived_util_cache`
- predicted satisfaction
- observed satisfaction

Not present in MVP:

- linked Techniques
- explicit satisfaction field
- required provenance

### 7.3 Objective statuses

Candidate MVP statuses:

- `active`
- `satisfied`
- `completed`
- `abandoned`
- `invalid`
- `superseded`

Mode rules:

- `completed` applies to one-time Objectives.
- `satisfied` applies to evergreen Objectives.
- `satisfied` is not a valid one-time Objective status.
- `completed` is not a valid evergreen Objective status.

One-time Objective transitions:

- `active` may transition to `completed`, `abandoned`, `invalid`, or `superseded`.
- `completed`, `abandoned`, and `superseded` are terminal for normal planning.
- `invalid` may be applied from any one-time Objective status when the Objective is discovered to be malformed, impossible, contradictory, or admitted by mistake.
- `superseded` may be applied from any one-time Objective status except `invalid` when a newer Objective or accepted design/source artifact replaces the Objective.
- One-time Objectives do not reactivate after `completed`; later renewed intent should use a new Objective or explicit supersession.

Evergreen Objective transitions:

- `active` may transition to `satisfied`, `abandoned`, `invalid`, or `superseded`.
- `satisfied` may transition to `active`, `abandoned`, `invalid`, or `superseded`.
- `abandoned` and `superseded` are terminal for normal planning.
- `invalid` may be applied from any evergreen Objective status when the Objective is discovered to be malformed, impossible, contradictory, or admitted by mistake.
- `superseded` may be applied from any evergreen Objective status except `invalid` when a newer Objective or accepted design/source artifact replaces the Objective.

Evergreen `satisfied` to `active` reactivation is a canonical transition only when the current accepted state changes through user declaration, authorized observation/import, or elapsed-time recurrence evaluation. Hypothetical Plan simulation may predict future reactivation, but does not by itself mutate canonical Objective status.

Every accepted canonical Objective status transition creates an `objective_transitioned` Log entry. Simulated or predicted status changes inside candidate Plans are not canonical transitions and do not create Objective transition Logs unless accepted as real state.

### 7.4 Evergreen recurrence

Evergreen Objectives may include recurrence/reactivation behavior.

For MVP:

- default recurrence type: `maintenance_time_decay`
- recurrence may be represented as a static timespan or simple PDF-like field
- user declarations or authorized observations may modify satisfaction state

### 7.5 RelationshipScopeTransition Objectives

A user may create an Objective to change the user's participation in a Relationship, invite or test a mutual scope change, or clarify whether another party is willing to enter a different scope. This is explicitly permissible self-governance. It is implemented through ordinary Objectives, Techniques, Steps, Tasks, Logs, reviews, and UniverseState mutations, not through a parallel relationship-planning system.

A Relationship scope transition Objective must target user-controlled behavior: disclosures, invitations, boundary-setting, availability changes, communication changes, clarification attempts, and accepted model updates. It must model counterparty response as uncertain and autonomous. For consent-dependent transitions, counterparty response is `outcome_observations`, not user success or failure.

`RelationshipScopeTransition` is a semantic target/reference for such Objectives. Provisional fields include:

- `relationship_id`;
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

## 8. Preferences and Value

A **Preference** is a relation between Objectives.

Value is derived from Preferences, not directly authored as an absolute scalar.

### 8.1 Preference object

MVP fields:

- `objective_a`
- `objective_b`
- `order`
- `acquired_method`
- `acquired_date`
- `enabled`

`order` may be:

- `a_preferred_to_b`
- `a_indifferent_to_b`

`acquired_method` may include:

- `user_defined`
- `llm_estimated`

More detailed method metadata may be added later.

### 8.2 Ordinal rankings

Ordinal UI input compiles immediately into pairwise Preference objects.

The original ordinal ranking may be retained in the log, but Preferences remain pairwise in the canonical model.

### 8.3 Indifference

Indifference is explicit.

Indifferent Objectives are assigned equal derived util values.

### 8.4 Preference contradictions

Preference cycles are logistical consistency errors.

Example:

- `A > B`
- `B > C`
- `C > A`

UbU must query the user for resolution with high priority.

### 8.5 Derived utils

Derived util values are transient and may be cached on Objectives.

They must be treated as volatile and recalculated when needed.

Default MVP direction:

- active Preferences form a DAG of Objectives;
- indifference creates equal-util levels;
- least-preferred level receives util `1.0`;
- each higher level is multiplied by `√2`.

Util derivation is typically local to the Objective subset being compared in a single planning process.

### 8.6 Preference calibration examples

A **PreferenceCalibrationExample** is a user-facing prompt or scenario used to help the user make better Preference judgments. It may describe a common emotional cost, common emotional reward, motivation ambiguity, social-pressure pattern, or tradeoff.

Preference calibration examples are MVP-important onboarding and review aids. They are not canonical value objects unless the user accepts or edits the resulting judgment into a Preference or other canonical entry.

The minimum Phase 1 calibration library has six example frames:

- `urgent_vs_important`: urgent external commitment versus important but nonurgent Objective;
- `emotional_cost_vs_visible_progress`: emotionally costly repair, clarification, or maintenance work versus easier visible progress;
- `social_pressure_vs_planned_objective`: socially pressured request versus planned user-chosen Objective;
- `recovery_vs_more_work`: recovery or rest versus another useful Task;
- `short_term_relief_vs_long_term_importance`: short-term relief or cleanup versus long-term capability, relationship, or project importance;
- `ambiguity_vs_decomposition`: decomposition of ambiguous work versus starting a larger unclear Task.

Each example identifies one or more calibration dimensions: `urgent_value`, `emotional_cost`, `social_pressure`, `recovery_value`, `long_term_importance`, and `ambiguity_value`.

Default presentation budget:

- Bootstrap interview: at most two examples after the user has named real Objectives or candidate Tasks.
- Calendar preview: at most one relevant example when the default Plan appears value-misaligned, socially pressured, recovery-fragile, or unusually urgent.
- Log review: at most one relevant example for an unreconciled rejection, override, failure, or repeated deviation unless the user asks for more.

Answers become canonical only through normal admission:

- explicit accepted pairwise Objective comparisons become Preferences;
- current affect, availability, capacity, or recovery reports become Snapshots or UniverseState facts when they describe observed current state;
- preview/review answers that explain outcomes, rejections, overrides, or calibration interactions become Logs, Log annotations, or Log corrections;
- comments about Objective importance or fit that do not declare pairwise order become Objective annotations or review notes;
- raw feelings, social-pressure comments, skipped examples, and unaccepted hypotheses remain noncanonical review notes.

Calibration wording must avoid leading the user:

- use the user's actual Objectives or Tasks when possible;
- present plausible costs and rewards for both sides;
- avoid preselected answers, hidden scoring, therapy, diagnosis, moral judgment, and "should" language;
- offer `neither`, `indifferent`, `not applicable`, edit, and free-text paths;
- disclose that examples are prompts for self-reporting, not UbU's values.

Candidate fields for a calibration example include:

- `example_id`
- `version`
- `situation_summary`
- `calibration_dimensions`
- `common_emotional_costs`
- `common_emotional_rewards`
- `possible_tradeoffs`
- `calibration_question`
- `resulting_object_refs`
- `source`
- `revision_status`

Example definitions are versioned derived content. UbU may revise or retire examples when user corrections, skips, edits, repeated non-use, or review feedback show that an example is confusing or leading. Historical Logs are not rewritten.

---

## 9. WorkItems, Tasks, and Containers

### 9.1 WorkItem

A **WorkItem** is the abstraction over concrete work-like entities.

A WorkItem may be:

- Task
- Container
- possible future subtype

### 9.2 Task

A **Task** is a schedulable WorkItem.

Tasks may be:

- Static Task
- Dynamic Task

#### Static Task

A Static Task has fixed start and end times.

Examples:

- meeting
- class
- appointment

Static Tasks are included directly in Plans.

#### Dynamic Task

A Dynamic Task has flexible scheduling.

Dynamic Tasks may have:

- duration or duration PDF
- dependencies
- preconditions
- effects
- Objective link
- success probability
- affect delta
- expected cost

### 9.3 MVP Task schedulability invariant

A Task is schedulable in MVP if it has:

- stable ID
- Objective link
- duration or duration PDF
- active status
- title

Permitted:

- dependencies may be empty
- earliest-start may be absent
- due may be absent
- cost may be absent
- effect may be absent in exceptional cases

Tasks with no known modeled effect may be reportable as low-utility or “meaningless” for plan scoring.

### 9.3.1 Evergreen gap-fillers

Phase 1 does not add an evergreen Task subtype. An evergreen gap-filler is an ordinary active Dynamic Task, usually serving an evergreen Objective, with a `gap_fill_policy` that lets the planner consider it when an otherwise discretionary gap exists.

Required `gap_fill_policy` fields:

- `eligible`: `true`;
- `minimum_useful_duration_seconds`;
- `maximum_useful_duration_seconds`;
- `cooldown_policy`, including an explicit `none_declared` value when no cooldown applies;
- `autonomy_mode`, which is `suggest_only` in Phase 1.

Optional `gap_fill_policy` fields:

- `preferred_duration_seconds` when different from the Task duration model;
- `location_scope`, such as `anywhere`, `current_location`, or a broad user-defined category;
- `required_context_refs`, such as required material, device, app, or integration state;
- `affect_suitability`, when the Task has narrower affect fit than normal Task affect constraints;
- `rank_hint`, for local tie-breaking among otherwise similar candidates.

Existing Task fields carry Objective link, title, status, duration, dependencies, preconditions, effects, recurrence, and provenance. Readiness should use ordinary preconditions when possible.

Recovery, meals, sleep, transition buffers, setup/teardown, and other legitimization support work are not gap-fillers by default. They protect human viability before optional gap work is considered.

### 9.4 Container

A **Container** groups WorkItems.

Containers are not directly scheduled in Plans. Plans contain Tasks.

A Container may represent:

- decomposed work
- preempted work
- Technique instantiation
- grouped child Tasks
- Super Automation workflow structure

Container completion is derived from child states.

A Container is complete when all child Tasks are either:

- completed
- moot

Task-to-Container mutation is a structural replacement, not an in-place type change. The original Task handle remains a historical Task handle and is not reused as the Container handle.

Mutation creates a new Container with a new `container_id`. The Container records `origin_task_ref`, `mutation_reason`, `mutation_log_ref`, child WorkItem refs, and lineage/provenance sufficient to trace the restructuring.

All child Tasks created by decomposition, preemption, retry, or worker expansion receive new Task handles. Existing child or continuation Tasks may be linked into the Container, but the mutation operation does not reassign the original Task handle to a child.

The original Task transitions to `moot` with reason code `replaced_by_new_plan_structure` when decomposition or regrouping preserves the underlying intent. If responsibility moves to another executor without decomposition, `delegated` may be more accurate; if a newer source artifact replaces the Task, `superseded` may be more accurate.

Fields that describe the original intent stay on the Container or its lineage metadata: title or summary, served Objective refs, parent/dependency context, external-reference lineage, Compartment/security labels, authority/provenance, and notes needed to explain why the work was split.

Fields that make a schedulable action executable belong on child Tasks: duration or duration PDF, preconditions, effects, executor/delegation fields, worker assignment/status, expected output, evidence requirements, and any child-specific dependencies or deadlines. Child Tasks may inherit or narrow Objective refs, Compartment refs, authority source, and External Reference context when valid, but they do not silently inherit stale status, completion evidence, or modeled effects that no longer apply.

External References are preserved by linking the external object to the new Container when it represents the larger work and to child Tasks only when the external object supports, evidences, or projects that specific child. The original Task's historical External References are not rewritten; new `supersedes`, `projection_of`, `supports`, or `evidence_for` references may be added according to the accepted External Reference model.

GitHub-linked Task decomposition should keep the GitHub Issue or PR traceable to the Container and add child-level External References only for actionable subwork that needs projection or reconciliation.

Automation/Super Automation expansion uses the same structural-replacement rule; worker-specific child Task details are in §24.1.2.

### 9.5 Moot

`moot` is a first-class terminal Task status.

It is functionally equivalent to completion for planning, but distinct for logs and reporting.

MVP moot reason codes are schema-controlled enum values:

- `externally_satisfied`
- `superseded`
- `delegated`
- `no_longer_relevant`
- `invalidated_by_universe_change`
- `replaced_by_new_plan_structure`
- `user_declared_moot`
- `automation_obsolete`
- `duplicate`

The MVP list is sufficient for Phase 1 and intentionally closed to free-form reason codes. New canonical reason codes require an explicit schema migration or accepted decision. Implementations may store human-readable notes on the `task_moot` Log entry, but the canonical `moot_reason_code` remains one of the enum values.

`duplicate` is included because duplicate work is common in imported GitHub/project queues and should be queryable separately from generic plan supersession.

`delegated` remains separate from `externally_satisfied`. `delegated` means the Task is no longer work for this executor because responsibility moved to another executor, worker, Identity, or delegation path. It does not by itself assert that the underlying Objective or required world state has been satisfied. `externally_satisfied` means the desired state became true through another action or event, regardless of delegation.

Use the narrowest accurate code:

- `externally_satisfied`: the required state is already true because of outside action or observation.
- `superseded`: this Task is replaced by a newer Task, Objective, decision, or source artifact.
- `delegated`: responsibility moved out of this Task's executor scope and will be tracked elsewhere.
- `no_longer_relevant`: the Task is no longer useful, but not because the world invalidated it.
- `invalidated_by_universe_change`: an external state change made the Task impossible or wrong.
- `replaced_by_new_plan_structure`: decomposition, regrouping, or planning restructure replaced this Task without changing the underlying intent.
- `user_declared_moot`: the user explicitly says the Task should be treated as moot and no more specific code is appropriate.
- `automation_obsolete`: a worker, automation path, or generated work item is obsolete because automation state changed.
- `duplicate`: the Task duplicates another active, completed, or canonical work item.

**Phase 2 status additions (not in Phase 1):**

`DEFERRED_INTENT` — the user has decided not to plan this Task now but wants to retain it for future consideration. Distinct from low-priority (which remains eligible for planning) and from cancelled (which is terminal). Tasks in `DEFERRED_INTENT` are invisible to the skeleton sampler. The Log review introspection cycle periodically surfaces them for review and structured dismissal. See `UBU-Q0117`.

`WAITING_FOR` — the user is personally waiting on a human response or external action before this Task can proceed. Carries an optional counterparty Identity reference and expected-by date. Distinct from a worker-delegated Task. Surfaces in the planning horizon as a blocked risk rather than a schedulable slot. See `UBU-Q0112`.

---

## 10. Task Preconditions and Effects

### 10.1 Preconditions

Preconditions are deterministic constraints over UniverseState.

If preconditions are unknown or partially modeled, they are treated as absent in MVP.

MVP preconditions use a recursive object with `all_of` and `any_of` arrays for simple AND/OR composition. A leaf predicate has `target`, `predicate`, and optional `expected` fields. `target` uses the same dotted UniverseState target convention as mutations, beginning with `facts`, `numeric_values`, `set_memberships`, or `event_markers`.

MVP predicates are:

- `equals`, for exact JSON-compatible equality;
- `member_of`, for checking that `expected` is present in a `set_memberships` target;
- `absent`, for checking that a target is not present or has been cleared.

Numeric comparisons are not in MVP, even for `numeric_values` targets. Numeric values may be checked only by equality or absence.

Preconditions may reference event markers, affect-related UniverseState keys in `user_mode`, and Relationship-relevant UniverseState keys when the containing Task is allowed to see those targets. Organization-mode and worker-mode validation must reject intrinsic-affect preconditions. Relationship-relevant and Compartment-protected targets remain subject to Compartment policy, mode rules, and user-acceptance requirements for private or inferred claims.

A failed precondition means the Task is blocked for planning and execution, not invalid. `unschedulable` is a derived planner result when no placement satisfies otherwise valid preconditions and Calendar Logic; `invalid` is reserved for malformed Tasks, malformed precondition schema, forbidden targets, or contradictions in canonical state.

### 10.2 Effects

A Task effect describes predicted mutation of UniverseState if the Task succeeds.

MVP effect object:

- scalar success probability
- mutation list

If success probability is `1` or `null`, the effect is assumed to occur when the Task completes.

If a Task fails, UniverseState is unchanged in MVP.

### 10.3 Duration and success probability

Duration uncertainty and success probability are distinct.

A Task may have:

- fixed duration
- duration PDF
- scalar success probability on the effect object

Task duration PDFs likely use seconds as the canonical time unit in MVP.

### 10.4 Resource preconditions and task readiness

A **Resource** is any physical, digital, legal, financial, informational, location, access-controlled, or tool-like thing whose state affects whether a Task can begin, continue, or complete. Examples include a whiteboard, a specific file, a queued podcast episode, a ladder, bread flour, a car, a passport, a software license, an API key, a warranty document, a payment method, or a quiet room.

Resource modeling is core to UbU's life-logistics purpose. It is delayed from Phase 1 because Phase 1 bootstraps the planning kernel through GitHub dogfooding, not because Resource is optional to the real product.

The Phase 3 Resource boundary should be a thin task-readiness layer, not a full inventory or financial-management product. The first user-facing question is:

> What must be true about the world before this Task is realistically executable?

A minimal Phase 3 Resource object may carry:

- `resource_id`;
- `name`;
- `kind` (`physical`, `digital`, `financial`, `access`, `location`, `information`, `tool`, `consumable`, `other`);
- `availability_state` (`available`, `missing`, `unavailable`, `unknown`, `needs_acquisition`, `needs_preparation`);
- `location_or_access_hint`;
- `owner_identity_ref` when relevant;
- `compartment_ref`;
- `notes`.

A Task may carry `resource_requirements[]` with:

- `resource_ref` or free-text placeholder;
- `requirement_type` (`required`, `helpful`, `blocks_start`, `blocks_completion`);
- `status` (`satisfied`, `unsatisfied`, `unknown`).

If a required Resource is missing, UbU may suggest or create prerequisite Tasks such as buying a part, finding a document, charging a battery, downloading a form, renewing a license, preparing a workspace, or checking whether a store or service is available. A Task requiring a Resource cannot be scheduled as ready unless the required Resource is in the required availability state in UniverseState.

A mature Resource model naturally extends into inventory control, subscriptions, household logistics, procurement, cost history, depreciation, budgeting, and Quicken-like financial management. These are Phase 3B/Phase 4+ full-version-1.0 features, not Phase 3A requirements. The distinctive UbU finance path is not to clone a ledger application first; it is to connect spending, ownership, maintenance, and acquisition to Objectives, Tasks, Techniques, Compartments, and capability growth.

### 10.5 Skill preconditions and capability readiness

A **Skill** is an Identity-owned capability that can satisfy a Task dependency. Skills are not static tags. They can be learned, tested, evidenced, improved through practice, and allowed to rust over time.

A minimal Skill object may carry:

- `skill_id`;
- `name`;
- `domain`;
- `description`;
- `owner_identity_ref`;
- `proficiency_level`;
- `confidence`;
- `verified_status`;
- `last_used_at`;
- `last_tested_at`;
- `rust_model`;
- `prerequisite_skill_refs[]`;
- `unlocks_technique_refs[]`;
- `evidence_refs[]`;
- `compartment_ref`.

A Task may carry `skill_requirements[]` with:

- `skill_ref`;
- `required_level`;
- `requirement_type` (`blocks_start`, `blocks_completion`, `improves_quality`, `reduces_risk`, `reduces_cost`);
- `acceptable_substitutes[]`.

Skill evidence may include self-declaration, completed Tasks, passed quizzes, supervised completions, credentials, portfolio artifacts, peer review, marketplace ratings, or External References. Self-declared evidence may be enough for private planning; public or barter-visible claims need stronger evidence, especially for high-risk or regulated domains.

Skill rust is part of the model. UbU should not assume that a Skill remains permanently available at the same level because it was once learned. Last use, last test, confidence, consequence of failure, and refresher requirements should influence whether a Task is ready, risky, or should be preceded by learning/practice.

### 10.6 Techniques, capability graphs, and DIY-versus-purchase planning

A **Technique** is a reusable procedure that can transform Resources, Skills, time, attention, and other prerequisites into an Objective-serving result. A Technique can describe how Resources are used (`consumed`, `borrowed`, `referenced`, `transformed`, or `produced`) and which Skills are required, reinforced, tested, or produced.

A large UbU-run or UbU-compatible Technique database can become a practical real-life capability graph. It can represent procedures such as replacing a faucet cartridge, filing an LLC annual report, cooking a basic meal, setting up encrypted backups, learning Rust ownership, preparing a ZK circuit review, or cleaning an apartment before guests arrive.

Technique requirements can create skill-tree behavior:

- prerequisites unlock new Techniques;
- learning Tasks can be scheduled to acquire missing Skills;
- completed DIY Tasks reinforce or test Skills;
- missing Skills can trigger a decision between learning, DIY, hiring, buying, bartering, or deferring.

This creates a natural DIY-versus-purchase/hire tradeoff. UbU can compare time cost, money cost, affect cost, Resource availability, Skill rust, risk, failure consequence, learning value, future task unlocks, and possible Skill Barter value.

The marketing hook is legitimate if kept grounded:

> UbU gamifies real life not by adding fake points, but by turning real Objectives, Resources, Skills, Techniques, constraints, and evidence into a capability tree the user can actually live inside.

See `UBU-Q0114`, `UBU-Q0115`, `UBU-Q0118`, `UBU-Q0119`, `UBU-Q0120`, and `UBU-Q0121`.

---

## 11. UniverseState

**UniverseState** is a first-class object representing the modeled state of the world relevant to UbU planning.

For MVP, UniverseState is a lightweight shell with loosely typed facts and events.

### 11.1 MVP UniverseState fields

Core shell:

- `universe_state_id`
- `timestamp` or valid-at instant
- `facts`
- `numeric_values`
- `set_memberships`
- `event_markers`
- `source_summary`
- `confidence_summary` optional

### 11.2 UniverseState values

MVP value discipline:

- fact keys are free-form strings;
- default keys should use a lightweight namespace convention;
- values are text / JSON-like payloads.

A stricter ontology can be added later.

### 11.3 Mutation vocabulary

MVP mutations support:

- `set_fact`
- `clear_fact`
- `increment_numeric`
- `decrement_numeric`
- `add_membership`
- `remove_membership`
- `append_event_marker`

MVP mutation items use a small deterministic schema. Required fields are `operation` and `target`; `payload` is required except for `clear_fact`; `note` is optional. Mutation items do not carry item-level confidence, source, or provenance in MVP. Those belong on the containing Task effect, Snapshot, Log entry, worker mutation request, External Reference, or other envelope.

Targets are dotted strings. The first segment names the UniverseState collection being changed: `facts`, `numeric_values`, `set_memberships`, or `event_markers`. Remaining segments form the lightweight namespaced key inside that collection. Example targets include `facts.github.issue.14.pipeline_state`, `numeric_values.affect.energy`, `set_memberships.github.issue.14.labels`, and `event_markers.relationship.rel_123.interactions`.

Payload rules by operation:

- `set_fact`: any JSON-compatible value;
- `clear_fact`: no payload;
- `increment_numeric` / `decrement_numeric`: numeric delta payload;
- `add_membership` / `remove_membership`: JSON scalar member payload, usually a string;
- `append_event_marker`: JSON object payload describing the lightweight marker.

A Task effect's mutation list is unconditional once that effect succeeds. Conditional behavior belongs in Task preconditions, effect success probability, or planner branching, not in individual mutation items. Implementations should validate the full mutation list before applying it and apply valid items in list order.

Mutation targets may include affect-related UniverseState keys in `user_mode`; organization and worker modes must reject intrinsic-affect mutations. Mutation targets may include Relationship-relevant UniverseState keys, but inferred or worker-generated relationship facts remain subject to Compartment policy, mode rules, provenance/logging envelopes, and user acceptance where required. Task effects should not silently overwrite user-declared private affect or Relationship truths.

---

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

## 13. Affect

Affect is core in `user_mode`.

Affect belongs to UniverseState.

Affect is not intrinsic to organizations or machines.

### 13.1 MVP affect dimensions

MVP uses simplified user-reportable dimensions:

- energy / tiredness
- stress level
- mood

Values are reported in the range `0.0` to `1.0`.

### 13.2 Mood

Mood is represented as:

- categorical trinary state:
  - `happy`
  - `sad`
  - `angry`
- intensity scalar from `0.0` to `1.0`

`interested/bored` is an independent derived dimension.

### 13.3 Affect snapshots

Affect snapshots are ordinary UniverseState data produced by user query.

They include:

- timestamp
- source: `user`
- per-dimension values

### 13.4 Affect confidence

In MVP, affect confidence decays with age.

Low confidence is determined by algorithm configuration.

Confidence is global across affect dimensions in MVP and may become per-dimension later.

### 13.5 Affect collection Objective

UbU may include an evergreen high-value Objective such as:

> Collect affect information relevant to important usage.

When affect data is missing or stale, the planning algorithm may create direct UI survey Tasks.

### 13.6 Affect constraints

Affect is a constraint-satisfaction concern.

Burnout / affect exhaustion is modeled as a constraint violation rather than a first-class Risk object in MVP.

### 13.7 Affect constraint functional form

Affect constraints in the planning kernel use sigmoid-family functional form, parameterized per dimension. Piecewise-linear form is not used for the Phase 1 planning kernel.

Sigmoid is mandated because:

- UbU intends to eventually infer AffectProfile parameters from execution history; sigmoid parameters are directly fittable using standard logistic regression or MLE, whereas piecewise-linear fitting requires non-standard constrained optimization;
- sigmoid models gradual saturation at extremes, preventing hard-clipping planning artifacts near constraint boundaries that piecewise-linear produces;
- recovery curves follow an S-shape that sigmoid captures with two parameters and piecewise-linear approximates poorly with fewer than four hand-placed breakpoints.

For Phase 1 MVP, the sigmoid output is a constraint-satisfaction score in `[0, 1]`. It is not a literal success probability, not durable user preference, and not canonical utility. The full per-dimension schema — including `direction`, `location`, `scale`, `threshold`, freshness metadata, and bootstrap defaults — is specified in `PLANNING_KERNEL_CONTRACT.md §6`.

Phase 1 dimension directions are:

- `energy`: `higher_is_better`;
- `stress`: `lower_is_better`;
- `mood_intensity`: `lower_is_better`, where intensity means affective arousal or volatility rather than mood valence. Positive excitement and negative agitation can both consume planning capacity.

The Phase 1 `mood_intensity` direction is a simplification, not the long-term affect model. The fuller design treats mood intensity as task-dependent arousal/activation: routine tasks may favor low arousal, while creative, social, or physical tasks may require a higher optimal range. Phase 2 should add `bounded_optimum`, `optimal_low`, `optimal_high`, and task/category affect overrides.

Phase 1 must not require ordinary users to edit raw sigmoid parameters. The UX should ask qualitative calibration questions and map those answers into the internal schema. Conservative bootstrap defaults are allowed as temporary planning priors and must be marked for review. Advanced users may inspect and edit raw parameters.

Composite or cross-dimension affect interactions are post-MVP.

See also: `UBU-D0167`, `UBU-D0173`, `PLANNING_KERNEL_CONTRACT.md §6`.

---

## 14. External Events

An **External Event** is an instantaneous change in the universe.

External Events have no duration and can overlap Tasks.

External Events are not part of feasibility evaluation for an individual Plan in MVP.

Examples:

- GitHub issue comment
- PR opened
- CI failed
- user receives message
- train delay
- another user completes dependent work

External Events may trigger:

- logs
- Objective updates
- Tasks
- recalculation
- worker assignment

### 14.1 External References

An **External Reference** is the canonical many-to-many link between an outside object, record, registry, URL, repository, contract, chat room, payment rail, or other external artifact and a UbU object.

For Phase 1 GitHub dogfooding, External References are first-class objects. They are preferred over embedding all external links directly on Objectives, Tasks, or External Events. Small `external_refs` arrays may still appear on Logs or provenance payloads as convenience pointers, but they are not the authoritative external-link model.

MVP required fields:

- `external_reference_id`
- `external_system`
- `external_object_type`
- `external_object_id`
- `ubu_object_type`
- `ubu_object_id`
- `relation_type`
- `confidence`
- `created_by_identity_ref`
- `created_at`
- `last_verified_at`
- `sync_policy`
- `projection_policy`
- `provenance`

MVP target object types include Objective, Task, External Event, and Log entry. This allows one GitHub Issue to support multiple Objectives, one Objective to reference multiple GitHub Issues, a PR or CI run to become an External Event, and comments, reviews, or CI signals to be retained as evidence for Tasks or Objective updates.

MVP relation types should be a small enum sufficient for GitHub dogfooding: `represents`, `supports`, `evidence_for`, `source_event_for`, `projection_of`, `duplicate_of`, and `supersedes`. Later integrations may add relation types through explicit schema migration rather than free-form strings.

Duplicate detection uses a normalized uniqueness key over `external_system`, `external_object_type`, `external_object_id`, `ubu_object_type`, `ubu_object_id`, and `relation_type`. Importers should normalize repository identity, object numbers, node IDs, URLs, and delivery IDs before comparison. A repeated observation of the same normalized external reference updates verification metadata or logs an idempotent no-op; it does not create another external reference. Different relation types between the same objects are allowed when semantically distinct.

Automation Workers may not directly create or mutate canonical External References in Phase 1. A worker may submit an external-reference mutation request when its capability grants allow `mutation_request.submit` for the relevant scope. The canonical instance validates authority, Compartment/export policy, duplicate keys, expected prior version, and provenance before applying or rejecting the request and writing the corresponding Log entry.

---

## 15. Plans and Calendars

### 15.1 Plan

A **Plan** is a finite ordered set of Tasks from a start time to an end time, satisfying Calendar Logic.

A Plan includes:

- Static Tasks
- Dynamic Tasks
- external events

A Plan does not directly contain:

- Containers
- Objectives
- Techniques
- Recipes

Those are logical/model relations outside the calendar-view layer.

Once represented on a Calendar, a Plan is deterministic. It is best understood as a time-independent representation of predicted Actions, similar in shape to a future-facing Log projection. A Plan may eventually include predicted sensor states, expected UniverseState transitions, and other projected observations, but it does not internally evaluate its own probability after it has been materialized.

Plan optimization and default Plan selection are separate operations. Each candidate Plan should be individually optimized for value while satisfying constraints within the modeled world-branch that produced it. The planning algorithm models uncertain parameters such as duration distributions, Task success or failure, external events, interruptions, affect uncertainty, availability changes, and sensor predictions. A particular modeled combination of those parameters can produce a deterministic candidate Plan. UbU may then ascribe a **Plan probability** to that candidate Plan: the probability mass of the modeled branch that yields that deterministic Plan.

### 15.2 Calendar

A **Calendar** is a group of possible Plans.

A Calendar has a default Plan.

The default Plan is the current best recommendation, not a commitment.

The user controls what actually becomes historical log.

### 15.2.1 Default Plan selection

The default Plan is the Plan, or one of multiple tied Plans, with the highest Plan probability overall among the candidate Plans produced for the current Calendar scope.

This does not mean the default Plan is an unoptimized low-value routine. The Plan itself is already value-optimized within its deterministic branch while satisfying dependencies, deadlines, affect constraints, preconditions, and other Calendar Logic. The default selection step chooses among deterministic optimized Plans by Plan probability.

The default Plan is critical to UbU's user experience. Calendar preview must be able to show an ordered upcoming set of Tasks from the current time to the end of the Calendar scope, and each Task should have an explanation of why it appears there: Objective value, dependencies, deadlines, preconditions, affect constraints, worker status, risk findings, or other relevant scheduling reasons.

The ideal search over candidate Plans may be NP-hard or otherwise combinatorially expensive. Practical Compact Calendar implementations should therefore use bounded finite Task instances, time-delta configuration, pruning, greedy baselines, cached subplans, GPU-friendly batch scoring/simulation, device-specific resource limits, and other heuristics. Exact ideality is a north-star property, not a Phase 1 runtime promise.

### 15.2.2 Skeleton Plans and legitimization

The planning pipeline begins with a **skeleton Plan**. A skeleton Plan affixes all Static Tasks and recursively walks backward through dependency DAGs from terminal Static Tasks to dependency roots, then schedules root prerequisites before their dependents. The skeleton Plan is a causal/dependency foundation, not a complete optimized user Plan.

A dependency is a requirement that some state be true before a dependent Task begins. During skeletonization, UbU must check whether that state is already satisfied by the initial UniverseState before inserting prerequisite Tasks. If a viable skeleton Plan cannot be created, the planning model is blocked, contradictory, incomplete, or impossible enough that UbU should stop ordinary planning and ask the user for clarification with a concrete diagnostic explanation.

After skeletonization, UbU performs **legitimization**. Legitimization adds the minimum human-viability constraints and support Tasks needed to make the skeleton Plan plausibly executable by the user. Examples include affect constraints, breaks, recovery Tasks, meals, sleep, transition buffers, setup/teardown time, context-switch limits, and basic sustainability requirements.

The legitimized skeleton Plan is the baseline feasible Plan. Optional Dynamic Tasks, gap-filling work, and richer candidate Plans are compared against it by value, risk, fragility, and user-fit. If full legitimization is cheap, it may be used directly while testing candidate Plans. If it is expensive, UbU should use semi-legitimization heuristics before invoking full legitimization on finalists.

Minimum skeleton Plan representation records:

- Static Task placements with fixed start and end times;
- the dependency DAG frontier used for the current planning scope;
- prerequisite roots whose required state is not already true in the initial UniverseState;
- ordered prerequisite chains and the dependency or precondition refs that justify them;
- initial UniverseState assumptions, including known true, known false, assumed, and unresolved planner-relevant facts;
- unsatisfied dependency diagnostics with the failing Task, missing state, causal chain, and clarification alternatives when available.

Legitimization records the constraints it enforced and any support Tasks or buffers it inserted. The minimum legitimization output includes affect limits in `user_mode`, recovery, breaks, meals, sleep, rest, transition buffers, setup and teardown time, context-switch limits, slack thresholds, dependency-fragility thresholds, and a threshold result of `passed`, `failed`, or `needs_clarification`.

After legitimization, the minimum candidate Plan representation includes materialized Task placements, decision envelopes for movable Tasks, Plan probability metadata, value score, legitimacy threshold result or score, hard-validation status, and explanation lineage back to Objectives, dependencies, preconditions, affect constraints, worker status, risk findings, and probability inputs.

### 15.2.3 Planning horizon and early preparation

The internal planning horizon may exceed the user-visible Calendar window. A one-day visible Calendar may require look-ahead beyond the day to avoid cutting off dependency chains, Techniques, deadlines, or future preparation sequences.

Within reasonable detailed planning windows, such as one day to roughly one week, UbU may bias fragile prerequisite work earlier when that reduces the risk of future impossible choices. This protects users from being forced to make hard tradeoffs while tired, overwhelmed, interrupted, or otherwise emotionally compromised.

This is a bounded operational bias. It should not imply that all distant future work should be pulled forward. For short operational windows, time discounting may usually be treated as negligible unless explicit user Preferences say otherwise. Longer horizons require separate preference and temporal-discounting modeling.

### 15.3 Calendar Logic

Calendar Logic includes constraints such as:

- no overlapping Tasks;
- dependencies respected;
- temporal constraints respected;
- preconditions evaluated;
- affect constraints respected in user mode.

### 15.4 Gaps

Plans may contain gaps.

Gap time is discretionary. UbU may explain a gap as protected recovery, transition/setup time, user-declared free time, or a place where optional work could fit.

For Phase 1, evergreen gap-fillers are UI suggestions outside the default Plan. A suggestion may be shown only after Static Tasks, predetermined Dynamic Tasks, dependencies, preconditions, legitimization support, recovery, and transition buffers have already been respected.

The planner ranks eligible gap-fillers by:

- hard fit to the available gap;
- Objective-derived value and due cadence;
- readiness from preconditions, location, material, device, and integration state;
- affect suitability and expected affect delta;
- cooldown or frequency policy;
- setup and teardown cost;
- user recency, rejection, snooze, and override evidence.

Calendar preview, Log review, affect collection, review queues, light cleanup, reflection, meditation, and relationship-maintenance prompts can all be represented by ordinary Dynamic Tasks with `gap_fill_policy` when they satisfy the same rules.

A gap-filler must not displace recovery, meals, sleep, transition buffers, setup/teardown, Static Tasks, deadline-fragile prerequisites, or a user-declared desire to leave the gap open. Phase 1 `autonomy_mode` is `suggest_only`: accepting or starting a suggestion records ordinary Task execution evidence and may trigger recalculation, but the suggestion is not a prior user commitment.

Future versions may instantiate Gap Tasks, trusted auto-insertion policies, Technique-generated maintenance Tasks, richer Resource/Skill readiness, and stochastic Objective recurrence.

---

## 16. Compact Calendar

A compact Calendar is a transport/storage representation of a Calendar.

It is intended to efficiently represent a large set of possible Plans.

Compact Calendar support is considered important for MVP because it enables recursive self-analysis and transport between devices/workers.

### 16.1 Compact Calendar concept

A compact Calendar may include:

- Task set;
- Static Tasks;
- Dynamic Tasks;
- skeleton Plan metadata;
- legitimized skeleton baseline;
- ordering constraints;
- duration distributions;
- Task success probabilities;
- external-event trigger distributions;
- modeled interruption distributions;
- affect-state uncertainty and constraints;
- deterministic expansion grammar;
- probability provenance expressions and correlation-group references;
- expansion threshold(s);
- coverage value;
- default Plan reference or materialized default Plan;
- stored user-previewed, risk-report, or debug Plans when policy requires them;
- short-horizon branch cache;
- decision envelopes;
- protected, flexible, and disposable Task metadata;
- cached explanations and repair recipes;
- execution-mode metadata such as time delta, branch horizon, CPU/GPU resource limits, and privacy-routing limits.

### 16.2 Coverage

Coverage belongs to the compact serialization, not the abstract Calendar. It is an estimated regeneration metric in Phase 1, not an exact proof.

Coverage represents the modeled probability mass of possible futures covered by the compact representation within the recorded coverage scope, usually the reactive horizon. MVP coverage includes modeled duration uncertainty, Task success/failure uncertainty when success probabilities affect branch reconstruction, and modeled external-event or interruption assumptions. Objective recurrence uncertainty is not included in MVP coverage; evergreen recurrence is evaluated deterministically before Calendar generation or repair unless a later schema adds stochastic recurrence inputs.

A compact Calendar stores `coverage_estimate`, `uncovered_mass_estimate`, `coverage_scope`, `coverage_threshold_used`, `coverage_inputs_summary`, `probability_quality`, and warning or diagnostic refs when inputs are unmodeled, stale, or degraded.

The default Phase 1 regeneration threshold is `0.99` short-horizon branch coverage. The effective policy resolves Calendar-specific override first, then execution-profile or Device policy, then the global default. The effective threshold is stored with the compact Calendar for replayable regeneration decisions.

After time advances, coverage is recalculated by conditioning the compact Calendar on elapsed time and accepted Logs, Snapshots, External Events, and user actions, then discarding expired branch mass and re-estimating the remaining covered mass.

If recalculated coverage falls below the effective threshold, UbU records or batches a `low_compact_calendar_coverage` recalculation trigger and includes the finding in risk reporting. Immediate recalculation is required only when low coverage can affect the current or next recommended Task, hard feasibility, affect legitimacy, or the default Plan; otherwise marking the Calendar stale is sufficient. Low coverage does not directly create a canonical Task or worker assignment.

### 16.3 Planner grammar direction

Compact Calendar planning is no longer framed as a bare DFS process that directly produces the default Plan. The current architecture is:

1. Build the skeleton Plan from Static Tasks and dependency DAGs.
2. Legitimize the skeleton Plan into a minimally human-viable baseline.
3. Generate richer candidate Plans by adding optional Dynamic Tasks and alternative placements.
4. Use semi-legitimization or full legitimization to reject unrealistic candidates.
5. Validate finalists against hard constraints.
6. Select the user-facing default Plan by Plan probability among deterministic candidate Plans.
7. Package compact repair metadata for runtime use.

DFS-like search may still be useful for candidate construction. BFS-like search may still be useful for near-term divergence. Greedy selection may still be useful as a deliberately unintelligent baseline. None of those algorithms is the complete planning architecture by itself.

Execution profiles are additive rather than mutually exclusive. The greedy mean-duration planner is the required MVP benchmark. Local and mobile profiles must support deterministic skeletonization, conservative legitimization, exact hard-constraint checks, local repair recipes, and a short-horizon branch cache. Desktop, worker, and hosted profiles may add DFS-like candidate construction, local search, solver-backed finalist validation, and GPU-friendly scoring or simulation. GPU or learned search may propose and score candidates, but exact or conservative validation certifies the selected Plan.

Plan probability is represented internally as probability metadata rather than only as a display scalar. The minimum record contains a display scalar, a log probability for stable computation, an optional probability interval, provenance over the modeled probabilistic inputs, and correlation-group or scenario references when inputs are not independent. Implementations may multiply probabilities only for inputs declared independent. Correlated or unknown relationships must use joint scenarios, shared random variables, correlation groups, or conservative intervals rather than pretending independence.

### 16.4 Reactive short-horizon branch layer

The default Plan is not enough for real-time use because reality can diverge from the modeled branch. UbU therefore also needs a lightweight reactive layer, likely BFS-like, policy-based, or repair-recipe-based, that starts at the current instant and maintains a short-horizon cache or reconstruction path for high-probability near-term alternatives.

The reactive layer should cover near-term alternatives involving planned Tasks, duration variation, early completion, late completion, interruptions, relevant external events, affect shifts, user overrides, eligible evergreen gap-fill suggestions, and immediate recalculation triggers. The provisional default horizon is one hour. Mobile-only, low-power, or offline operation may shorten this horizon.

When early completion creates a gap before the next Static Task and no predetermined Dynamic Task can be pulled forward, the reactive layer may compute a small ranked suggestion set from eligible gap-fillers. It should preserve the protected baseline and ask before starting or materializing the suggested Task in Phase 1.

This layer exists to let the user's current Calendar and Log update quickly when something actually happens. It should not attempt to enumerate every possible future to the end of the Calendar scope.

### 16.5 Mobile stewardship metadata

A compact Calendar should carry metadata that lets constrained devices preserve legitimacy without performing full global replanning. Useful metadata includes:

- protected, flexible, and disposable Task criticality;
- decision envelopes such as earliest start, latest finish, and movable windows;
- cached explanations for why a Task matters and what depends on it;
- conflict severity levels;
- last-legitimate-Plan references;
- simple repair recipes;
- recovery-critical and deadline-fragile markers;
- recalculation triggers and remote-assist eligibility.

The MVP-friendly subset is Task criticality, last legitimate Plan storage, simple repair rules, conflict severity levels, cached explanations, next-best-action mode, and basic decision envelopes. Rich branch packaging and learned local policy models are post-MVP by default.

### 16.6 Early completion and forward-pull behavior

When a Task completes earlier than expected, the default behavior should be to pull the next valid Dynamic Task forward to the present time if dependencies, preconditions, affect constraints, location constraints, and other Calendar Logic allow it.

This supports a conservative duration bias: when uncertain, the scheduler may prefer duration estimates that are not overly optimistic. Early completion then becomes easy to absorb by starting the next valid Dynamic Task immediately rather than leaving useless gaps or requiring disruptive global replanning.

Static Tasks retain fixed start times. If no predetermined Dynamic Task can validly fill time before the next Static Task, the planner considers evergreen gap-fill suggestions under section 15.4 and `gap_fill_policy` rather than treating the gap as unexplained failure.

This forward-pull semantic is the natural counterpart to the log-normal duration model. Log-normal duration distributions are right-skewed: delays stack asymmetrically and early completions are relatively rare. The forward-pull repair handles early completion cheaply — no global replan, just pull the next eligible Dynamic Task forward — while late completion requires no special action since the distribution already biases toward longer-than-mean durations. For lay audiences, this is well described as a stop-light model: a sequence of independent delay sources where slowdowns accumulate and early green lights simply mean you move a little sooner. See also: `UBU-D0166`.

### 16.7 Adaptive granularity and offline operation

Compact Calendar runtime uses configurable execution profiles. The default Phase 1 presets are:

- `full_detail`: 60-second delta, 3600-second reactive horizon, `0.99` branch coverage target;
- `mobile_moderate`: 300-second delta, 3600-second reactive horizon, `0.99` branch coverage target;
- `mobile_low_power`: 900-second delta, 1800-second reactive horizon, `0.95` branch coverage target;
- `offline_steward`: 900-second delta, 1800-second reactive horizon, `0.95` branch coverage target, using precomputed branches and local repair metadata.

The one-, five-, and fifteen-minute deltas are presets, not schema constants. Calendar policy, Device policy, user settings, and runtime conditions may choose different positive deltas, horizons, coverage targets, and compute budgets. The effective values are stored with the compact Calendar or PlanningRequest so replay and explanation use the same policy inputs.

Switching to a coarser profile is allowed for `low_battery`, `low_power_mode`, `thermal_pressure`, `offline`, `expected_offline_window`, `heavy_workload`, `user_setting`, `external_worker_unavailable`, and `compute_budget_exceeded`. Returning to a finer profile is allowed when power, temperature, connectivity, worker availability, idle time, or user settings permit it. A switch records its reason in runtime or compact Calendar metadata; if it can affect the current or next recommendation, hard feasibility, affect legitimacy, or coverage threshold result, UbU records or batches a recalculation trigger using the relevant existing trigger kind.

When UbU switches to a coarser delta, the user-facing explanation must disclose the active profile, reason, effective delta, reactive horizon, coverage target or estimate, expected loss of precision, guarantees that still hold, and how to request more detailed planning or wait for a finer-capability backend.

Known offline windows trigger preparatory precomputation when connectivity and compute are available. The precompute package should cover the declared offline window plus the current reactive horizon when feasible, and should store the default Plan, last legitimate Plan, decision envelopes, Task criticality, cached explanations, simple repair recipes, and high-probability branch materialization or reconstruction instructions up to the effective coverage target. Precomputation is bounded by battery, thermal, user, Compartment, and compute-budget policy.

Unexpected offline operation degrades to cached compact Calendar state, local explicit algorithms, coarser deltas, reduced branch horizon and coverage target, simple repair recipes, and next-best-action mode. It must not pretend cloud, worker, or LLM-assisted analysis is still available.

Cached branches remain usable only while accepted Logs, Snapshots, External Events, elapsed time, and user actions stay inside their decision envelopes and while dependencies, preconditions, affect assumptions, and coverage remain valid. A branch expires when divergence leaves its envelope, a required dependency or precondition changes, a Static Task conflict appears, affect data becomes invalid for the Plan, coverage falls below the effective threshold, or the branch passes its recorded expiry.

The minimum mobile-only guarantee is the core UbU loop on one local device: preserve explicit state, show one recommended next Task or a clarification Task, explain why it matters now, honor Static Tasks and hard constraints conservatively, expose affect/staleness and coverage degradation, record user feedback in Logs, and run local repair for the current or next Task. Mobile may use coarser granularity, shallower branch coverage, reduced analysis depth, and fewer LLM-assisted features; it may not hide a mandatory external compute dependency.

### 16.8 GPU-aware execution backends and privacy boundary

The open-core planning loop must not depend on a hidden cloud service. Mobile-only UbU must preserve the core UbU experience, but it should do so as a real-time steward of Plan legitimacy, user agency, and next-action clarity rather than as the full global optimizer.

Cloud or external compute may improve performance, granularity, analysis depth, branch coverage, and expensive legitimization, but must not be a hidden mandatory dependency for the open-core planning loop. Solver selection should treat GPU suitability as a first-class criterion. CPU or conservative exact logic should certify skeleton validity, hard constraints, contradiction diagnostics, and explanations; GPU-capable search, simulation, scoring, and learned-model inference should handle large candidate sets, stochastic robustness, affect scoring, and premium cloud planning where available.

Possible execution backends include:

- local mobile execution;
- local desktop or laptop execution;
- Phase 2 user-owned worker devices;
- dedicated user-owned worker appliances;
- UbU corporate hosted compute;
- third-party UbU-compatible compute providers;
- future privacy-preserving compute services such as practical FHE-backed or comparable encrypted computation.

The FOSS planner and Compact Calendar representation should be backend-agnostic. Execution providers are interchangeable through explicit APIs, capability grants, Compartment policy, provenance, and user-visible routing decisions. GPU search may propose candidate Plans, but hard constraints and final Plan validity must be certified by exact or conservative validation. Cloud planning payloads should minimize PII and transmit only the structured timing, dependency, value, constraint, criticality, and legitimacy information needed for the selected provider.

### 16.9 MVP Compact Calendar implementation

The minimum Phase 1 Compact Calendar implementation stores the legitimized skeleton baseline, the default Plan, the active execution profile, coverage estimate, uncovered-mass estimate, effective coverage threshold, decision envelopes, Task criticality, cached explanation fragments, probability provenance, and last-legitimate-Plan reference. It stores user-previewed, risk-report, and debug/reproducibility Plans only when those artifacts are actually generated or needed for audit.

High-probability near-term alternatives within the reactive horizon may be stored as materialized branches or as deterministic reconstruction instructions. Non-selected candidate Plans, expensive global repairs, and low-probability branches are reconstructed on demand from the compact grammar, current UniverseState, Logs, Snapshots, External Events, repair recipes, and cached provenance.

MVP planning does not require exhaustive optimal search, cloud compute, GPU execution, exact coverage proof, learned policy models, or a complete transport grammar for every post-MVP uncertainty source. The required implementation is a deterministic skeletonizer, minimum legitimizer, greedy baseline, bounded candidate expansion, hard-constraint validator, Plan probability/provenance record, compact default-Plan package, short-horizon repair cache or reconstruction path, and low-coverage recalculation trigger.

### 16.10 GPU desktop execution backend

The Phase 1 performance target is a local desktop/laptop GPU backend. A small CPU reference path or fixture-backed deterministic path remains required for tests, CI, and contributors without GPU access. Mobile and cloud planner backends are deferred beyond Phase 1.

The implementation-facing contract for this section is `PLANNING_KERNEL_CONTRACT.md`. `DESIGN.md` defines the architectural intent; the contract file defines the Phase 1 schema and boundary details.

#### 16.10.1 Pure function contract

The GPU planning engine is a pure function. Its input is a `PlanningRequest`. Its output is a `PlanningResponse` containing ranked PlanCandidates, diagnostics, warnings, and probability-quality metadata. The engine has no side effects, performs no I/O, and holds no mutable state. The CPU kernel owns all canonical state mutation. The GPU engine is advisory only: it proposes; the CPU kernel validates and commits.

The engine is invoked as a typed Python function call, not a subprocess or service. The framework is PyTorch.

#### 16.10.2 CPU/GPU handoff contract

The CPU/GPU boundary is defined by two typed objects: `PlanningRequest` and `PlanningResponse`, specified in `PLANNING_KERNEL_CONTRACT.md`.

`PlanningRequest` includes schema version, planner version, request ID, effective time, generated-at time, mode, RNG seed, time-window policy, horizon policy, compute budget, task graph with CPU-provided `topological_order`, UniverseState snapshot, AffectProfile, scoring policy, constraint policy, payload policy summary, and privacy/provenance payload-safety proof. Optional fields include external event assumptions, repair context, explanation request, and debug flags.

`PlanningResponse` returns ranked PlanCandidates plus diagnostics, rejection counts, warnings, probability-quality metadata, optional coverage estimate, optional compute telemetry, and stage funnel counts. `K=3` PlanCandidates is the Phase 1 default:

- highest-utility candidate;
- most-robust candidate;
- most-schedule-diverse candidate.

K is user-configurable to allow power users to tune computational resource use. The Phase 1 default rollout budget is `n_rollouts = 1000` per finalist unless overridden by the CPU kernel's compute budget.

#### 16.10.3 Tensor layout

All tensor batches use padded fixed-size shape `(N_CANDIDATES, MAX_PLANNING_TASKS, ...)` with an explicit boolean validity mask tensor marking valid task slots. Ragged tensors are not used in Phase 1.

`MAX_PLANNING_TASKS = 256` is the Phase 1 planning window ceiling, defined at a single site. Future premium or wide-horizon tiers may increase this ceiling by scalar configuration, subject to memory, scenario-count, correlation-matrix, validation-cost, and backend performance limits. Linear scaling is not assumed as a mathematical guarantee.

#### 16.10.4 Pipeline stages

The GPU engine has four first-class pipeline stages, with semantic boundaries specified in `PLANNING_KERNEL_CONTRACT.md §5`.

1. **Skeleton sampling** — parallel shifted-log-normal or fixed duration sampling across candidates with vectorized propagation over the CPU-provided topological order.

2. **`affect_legitimacy_filter`** — batch sigmoid affect-constraint evaluation across surviving candidates. This stage implements only the sigmoid affect-constraint portion of legitimization. Full legitimization in UbU design is broader: it makes a skeleton Plan human-viable by respecting affect, recovery, transition, rest, sustainability, and support constraints. The `affect_legitimacy_filter` GPU stage does not replace or subsume full legitimization.

3. **Value scoring** — parallel utility, approximate robustness, affect-margin, and schedule-diversity scoring across surviving candidates.

4. **Monte Carlo rollout** — joint scenario simulation for finalists using correlation-group Gaussian copula sampling with deterministic rollout seed derivation.

GPU search proposes candidates. Hard constraint certification and final Plan validity are performed by the CPU kernel using exact or conservative validation.

#### 16.10.5 Stochastic duration model

Task durations use either a fixed duration model or a shifted log-normal distribution. The stochastic model is parameterized by `(min_seconds, mode_seconds, p95_seconds)`.

- `min_seconds` is the optimistic lower support shift used by the planner distribution; it is a subjective best-case modeling prior, not a claim about physical impossibility.
- `mode_seconds` is the most likely duration.
- `p95_seconds` is the high-but-plausible 95th percentile, not a hard upper cap.

The shifted distribution is `D = min_seconds + LogNormal(mu, sigma)`, with the canonical conversion formula specified in `PLANNING_KERNEL_CONTRACT.md §3`. The planner may sample durations greater than `p95_seconds`; that is intentional.

Fixed known durations use a separate fixed-duration model and are represented as a delta distribution. Invalid stochastic triples are rejected by schema validation rather than silently repaired.

For lay audiences this is well described as a stop-light model: a sequence of independent delay sources where slowdowns stack asymmetrically and early arrivals are absorbed cheaply by forward-pulling the next eligible Dynamic Task.

#### 16.10.6 Correlation groups and rollout matrix

Each stochastic-duration Task may carry `correlation_groups: [{group: str, strength: float}]`. In Phase 1, `strength` is a positive latent-factor loading in `[0, 1]`; negative correlations are deferred.

The CPU kernel constructs a deterministic positive semi-definite correlation matrix by normalizing per-Task positive factor loadings and computing `C = L * L^T + diag(1 - row_norm(L)^2)`. Multiple shared groups combine through the dot product of normalized loading vectors. Numeric jitter may be applied for floating-point error with explicit degraded diagnostics. Silent nearest-PSD projection is not a Phase 1 semantic repair.

Negative correlations are not rejected philosophically. They are deferred because signed pairwise-correlation declarations can create non-PSD matrices and confusing validation failures. Phase 2 should support anti-correlations through signed latent factors, explicit Cholesky-style parameterization, or a signed partial-correlation model.

The RNG seed is an explicit required input, ensuring plan generation is reproducible for peer debugging.

See also: `UBU-D0166`, `UBU-D0167`, `UBU-D0168`, `UBU-D0169`, `UBU-D0170`, `UBU-D0171`, `UBU-D0172`, `UBU-D0173`, `UBU-D0174`, and `PLANNING_KERNEL_CONTRACT.md`.

---

## 17. Logs

A **Log** is the canonical record of what actually happened in the universe.

A Log consists of timestamped entries recording:

- Task completion or failure
- External Events that occurred
- Observed Snapshots (user-declared or sensor-derived state)
- Objective status transitions
- Plan realizations
- Decisions made and by whom
- System recalculations

Logs are immutable once written, but may be annotated or corrected through new entries.

### 17.0 Counterfactual logging requirement

The Log records user-facing choices that did not become the selected path. A counterfactual decision is recorded with the existing `decision_recorded` event type whenever UbU presents a meaningful option, warning, proposal, alert, or recommendation and the user rejects, dismisses, overrides, bypasses, or ignores it.

Minimum MVP `decision_kind` values:

- `plan_candidate_rejected`
- `task_suggestion_dismissed`
- `suggestion_declined`
- `system_recommendation_overridden`
- `worker_proposal_declined`
- `safeguard_advisory_bypassed`
- `alert_dismissed_or_ignored`

Minimum `event_payload` fields:

- `decision_kind`
- `presented_ref` or `presented_candidate_summary`
- `presented_at`
- `decision_surface`
- `available_actions`
- `user_action`
- `chosen_ref` when the user chose another option
- `rejected_ref` or `rejected_refs`
- `reason_capture`
- `decision_context`
- `preference_inference_allowed`

`presented_candidate_summary` is a compact, redacted snapshot sufficient for review when the full candidate is transient or expensive to retain. It records identifiers, title or label, rank or recommendation status, score or explanation fragments when shown, and source or provenance refs; it must not embed denied Compartment payload.

`reason_capture` records optional reason evidence without making explanation mandatory. It contains `reason_source` (`user_stated`, `user_selected_code`, `system_inferred`, or `none_given`), optional `reason_code`, optional `reason_text`, and optional `confidence` for inferred reasons. If the user skips the prompt, UbU records `none_given`. System-inferred reasons are review notes, not canonical Preferences.

`decision_context` records the system state needed to understand the choice: related Calendar or Plan refs, current recommendation ref, Task or Objective refs, active Identity or acting-as context beyond `actor_identity_ref` when relevant, active Compartment or policy result refs, Snapshot or UniverseState refs, risk-report refs, worker assignment or proposal refs, safeguard or alert refs, and source External References. Context should use refs, hashes, and summaries rather than copied payload whenever possible.

Counterfactual entries are append-only Log entries. They are corrected or annotated only through later `log_correction_added` or `log_annotation_added` entries. A later accepted Preference, Task update, Objective update, safeguard policy change, or worker reassignment may cite the counterfactual Log ref but does not rewrite it.

Preference inference may consume only uncorrected counterfactual entries and must distinguish user-stated, selected-code, inferred, and missing reasons. Absence of a counterfactual entry is not evidence of user acceptance.

### 17.1 MVP Log entry fields

MVP Log entries use a shared event envelope. Required fields:

- `log_entry_id`
- `schema_version`
- `instance_id`
- `recorded_at`
- `effective_at`
- `event_type`
- `actor_identity_ref`
- `recorded_by_device_ref`
- `target_ref`
- `result`
- `event_payload`
- `provenance`

`recorded_at` is when UbU accepted the entry. `effective_at` is when the modeled event occurred and may be earlier for imported or reconstructed events. `target_ref` identifies the primary canonical object or imported External Event. `result` is one of `success`, `failure`, `partial`, or `not_applicable`. `event_payload` is JSON-compatible event-specific data.

Optional fields:

- `old_value`
- `new_value`
- `reason`
- `notes`
- `confidence`
- `related_plan_ref`
- `external_refs`
- `annotation_of`
- `correction_of`
- `idempotency_key`

`old_value` and `new_value` are used for mutation-like events and may be absent for observational, external-only, or compartment-sensitive records. `provenance` records the source kind and source reference; `confidence` is stored when the entry is inferred, sensor-derived, imported, or worker-submitted.

### 17.2 MVP Log event types

MVP event types are:

- `task_completed`
- `task_failed`
- `task_moot`
- `external_event_observed`
- `snapshot_observed`
- `objective_transitioned`
- `pipeline_state_transitioned`
- `plan_realized`
- `decision_recorded`
- `recalculation_triggered`
- `worker_assignment_updated`
- `worker_mutation_submitted`
- `worker_mutation_applied`
- `worker_mutation_rejected`
- `log_annotation_added`
- `log_correction_added`

All event types share the required envelope. Event-specific details live in `event_payload` and may be validated by event-specific schemas.

### 17.3 Immutability, annotation, and correction

Log entries are append-only. Annotation and correction never modify the original entry. They create a new Log entry with `event_type` set to `log_annotation_added` or `log_correction_added`, and with `annotation_of` or `correction_of` pointing to the original `log_entry_id`.

Corrections supersede interpretation of the original entry but do not erase the original historical claim. Later queries should resolve corrected views by following correction links.

### 17.4 Storage, retention, and query

Canonical Logs are stored per UbU instance. Device-local Logs or queues may exist as transport and audit buffers, but canonical history belongs to the instance; each entry records `recorded_by_device_ref` when known.

MVP retention is indefinite for canonical Logs. Archival may move old entries to colder local storage, but must preserve queryability and integrity. Deletion or redaction policies are governed by Compartment retention rules and remain post-MVP except where required by a hard Compartment invariant.

MVP query/search is index-backed over `recorded_at`, `effective_at`, `event_type`, `actor_identity_ref`, `target_ref`, `external_refs`, and correction/annotation links. Large histories should be queried by time-windowed, cursor-paginated APIs. Derived search indexes may be rebuilt from the append-only Log.

### 17.5 Automation Worker contributions

Automation Workers contribute to Logs through their worker Identity. A worker may submit events or mutation requests, but the canonical instance validates authority and writes the canonical Log entry. A worker submission first creates a `worker_mutation_submitted` or operation-specific submission record when it enters the canonical instance. Accepted worker mutations create applied entries; invalid, unauthorized, stale, or policy-denied attempts create rejected entries. Requests that require human or policy review remain candidates until an authorized actor accepts or rejects them.

Worker-supplied `idempotency_key`, provenance, evidence references, and confidence metadata are retained when available so duplicate submissions and stale worker results can be detected.

### 17.6 Log vs Plan

- **Plan**: a possible ordered sequence of future Tasks, representing prediction
- **Log**: a timestamped record of what actually occurred, representing reality

Plans are inherently multiple. Logs are singular and authoritative for the specific timeline that occurred.

### 17.7 Log and Feedback

Logs enable feedback loops. A Log records the gap between prediction and reality, enabling better future planning.

The comparison of Logs against Plans is essential for:

- detecting estimation errors
- updating probability models
- surfacing unmodeled constraints
- improving forecasting algorithms

### 17.8 Regular Log review

Log review is a notable recurring Task. Its purpose is to keep UbU's model aligned with the user's actual behavior, later reflection, and corrections. Default cadence is at the end of a work window or at the next startup when unreconciled plan/reality differences exist, with a weekly catch-up if routine review is skipped. The user may adjust cadence through ordinary Task recurrence, snooze, or skip controls.

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
- Deferred-intent review: Tasks in `DEFERRED_INTENT` status are periodically surfaced with a structured dismissal prompt. Tasks that are no longer attainable or desirable should be dismissed rather than accumulating indefinitely. See `UBU-Q0117`.
- Circadian affect pattern report: after sufficient Log history, time-of-day patterns in affect state may be surfaced as a standard report to inform planning priors.

### 17.9 Authority source metadata

`authority_source` is a coarse enum describing the authority/source path for an accepted object, projection state, or candidate mutation. It does not replace actor Identity, capability grants, External References, expected-prior-version checks, review policy, or Log provenance.

Required in Phase 1:

- organization-mode accepted `Objective`, `Preference`, and `Task` records;
- `pipeline_state` projection-state records;
- worker assignment records;
- worker mutation requests;
- external projection requests, previews, and applied projection records;
- Delegation Substrate packets when present.

It is not required on ordinary user-mode `Objective`, `Preference`, or `Task` records created directly by the user. It is still required in user mode for worker, projection, pipeline-state, and mutation-request records that use those envelopes.

MVP values are:

- `human_admin`: admin-equivalent human operator or human-approved import/projection action;
- `automation_worker`: Automation Worker submission under a capability grant;
- `github_event`: normalized GitHub event, webhook, fixture, or reconciliation observation;
- `project_policy`: accepted project or organization policy/directive;
- `imported_config`: bootstrap, fixture, or configured import source;
- `llm_advisory`: model-generated candidate/advice; never sufficient authority without admission provenance;
- `user_override`: user-mode explicit override of the current recommendation, Plan, Task choice, or modeled state.

`authority_source` values are schema-controlled in MVP. Specific Identity, source object, and evidence path belong in surrounding fields such as `actor_identity_ref`, `created_by_identity_ref`, `capability_grant_ref`, `source_refs`, `external_reference_refs`, `provenance`, and the append-only Log entry.

---

## 18. Identities

An **Identity** is the external-facing communication and authorization surface.

Humans may have multiple Identities.

Organizations and workers are also represented through Identities externally.

UbU interacts with outside agents through Identities, not directly through metaphysical assumptions about humans.

Examples:

- personal identity
- family identity
- GitHub identity
- pseudonymous identity
- worker identity
- organization identity


---

## 19. Associations

An **Association** is an Identity-scoped, perspective-bound model of an emergent group-like coordination pattern.

Associations may be formal or informal, temporary or durable, economic or non-economic, intimate or impersonal, public or pseudonymous. Examples include:

- friend groups;
- parties and event cohorts;
- amateur sports leagues;
- FOSS projects;
- contributor crews;
- skill networks;
- private or public marketplaces;
- nonprofits;
- companies;
- DAO-like projects;
- conference outreach networks.

An Association is not an objective interpersonal object by default. UbU should not assume that an Association has one globally authoritative membership list, role list, boundary, or decision authority. Instead, an Association is modeled from the perspective of a particular Identity and supported by evidence, attestations, Relationships, shared Objectives, commitments, norms, Logs, and External References.

A legal entity filing, GitHub organization, website, governance document, Discord server, IRC log, board meeting note, contract, or payment address may be a strong External Reference. It is still not the whole Association. It proves or supports a limited claim about an external record, not every social fact about the group.

### 19.1 Association fields, provisional

A future Association object may include:

- `association_id`
- `perceived_by_identity_ref`
- `label`
- `description`
- `association_kind`
- `lifecycle_state`
- `compartment_ref`
- `perceived_member_refs`
- `perceived_objective_refs`
- `perceived_norms`
- `perceived_commitment_refs`
- `relationship_refs`
- `external_reference_refs`
- `evidence_refs`
- `confidence`

Full Association implementation is not required for Phase 1. Phase 1 may represent EthConf/outreach dogfooding through existing Objectives, Tasks, Logs, Relationships, External Events, and External References, plus manually structured or fixture-backed candidate AssociationAttestations.

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

### 19.3 SharedAssociationDescriptor

A **SharedAssociationDescriptor** is a future coordination artifact that multiple Identities may sign, accept, or reference.

It may describe a shared label, purpose, declared mission, governance rules, public references, contribution paths, membership or participation criteria, disclosure policy, and other declared facts. It is not a God's-eye Association. It is a shared artifact that different Identities may adopt, dispute, supersede, or interpret differently.

### 19.4 Organizational introspection

**Organizational introspection** is the process of reviewing an Association's evidence-bearing records and asking whether actual behavior matches declared goals.

Possible inputs include:

- README and governance documents;
- issue trackers and pull requests;
- release notes and roadmaps;
- meeting notes and board minutes;
- public or permissioned Discord, Matrix, Slack, IRC, or forum logs;
- calendar events;
- grant or milestone records;
- outreach notes and follow-up records.

Outputs should be reviewable artifacts:

- evidence-backed AssociationAttestations;
- mission-alignment questions;
- priority-drift warnings;
- undocumented-role observations;
- commitment-mismatch reports;
- follow-up Tasks;
- public or redacted retrospective artifacts.

The organizational-introspection hook for outreach is: `Can your project prove from its records that it is actually committed to achieving its stated goals?` In design terms, this means operational proof through auditable evidence trails and reviewed attestations, not mathematical certainty or an LLM truth oracle.

---


## 20. Contextual Messaging and Message Extraction

A **Message Context Envelope** is a bounded metadata wrapper around a human-readable message or external message reference.

It exists because ordinary legacy messages often under-specify the planning context needed by the receiver. A flat text request such as `can you grab milk?` may imply a household Objective, a grocery Task, a route constraint, a known Relationship, a typical milk type, a soft deadline, and a low-to-medium interrupt level. Legacy systems usually transmit only the text and a small amount of transport metadata.

A future Message Context Envelope may include:

- `message_id`;
- `source_system`;
- `transport_reference` or External Reference;
- `sender_identity_ref`;
- `receiver_identity_ref` or intended audience;
- `channel_ref` or Association reference;
- `raw_body` or content reference;
- `message_kind`;
- `topic`;
- `objective_refs`;
- `task_refs` or candidate Task description;
- `priority`;
- `interrupt_recommendation`;
- `response_expectation`;
- `deadline_or_review_window`;
- `assumptions`;
- `ambiguities`;
- `provenance`;
- `confidence`;
- `disclosure_policy`;
- `compartment_ref`.

A **Message Extraction Result** is a candidate interpretation produced by an Automation Worker, parser, or LLM-assisted extractor from raw or semi-structured communication. It should distinguish:

- facts explicit in the message;
- facts explicit in channel or transport metadata;
- facts inferred from thread context;
- facts inferred from Relationship history;
- facts inferred from Association context;
- model guesses;
- user-confirmed corrections.

Message extraction is advisory until accepted into canonical state. The extractor may propose Tasks, Events, Objective updates, Relationship observations, AssociationAttestations, or communication-review items, but it should not silently mutate canonical state.

### 20.1 Native UbU-to-UbU contextual messaging

Native UbU-to-UbU communication should allow a sender to disclose selected context intentionally. The receiving instance can then use that context to triage the message against the current Plan.

A minimal Phase 3 implementation should support enough metadata for priority and interrupt decisions. Richer subjective Relationship context, detailed Association reconciliation, and multi-party trust semantics may remain post-MVP.

The future inter-instance protocol should be a generic envelope family, not a separate protocol for every feature. Message delivery, worker assignment, status reporting, mutation-request submission, capability-grant exchange, External Event submission, and recalculation requests should share Identity, capability, Compartment, provenance, idempotency, and review semantics.

Phase 1 worker communication is a narrow worker API profile of that direction, not the full Phase 3 protocol. It may use simpler local or parent-specific endpoints, but its request, status, mutation, projection, and Log-submission shapes should intentionally avoid contradicting the future protocol.

Phase 3 multi-user coordination can then extend the same envelope family with Message Context Envelopes and user-to-user Identity commitments while preserving bounded disclosure. Native user messages are one protocol payload type; they do not get permission to bypass worker-style authority, validation, or admission rules.

### 20.2 Legacy communication adapters

Legacy adapters may ingest or emit messages through systems such as WhatsApp, SMS, email, Discord, IRC, Slack, Matrix, or similar systems when permitted by the user and the service boundary.

If only one party has UbU, the message remains a flat legacy input and UbU may locally extract candidate structure. If both parties have UbU, the instances may upgrade the exchange by attaching, linking, side-channeling, or otherwise associating a Message Context Envelope with the legacy message.

This is an interoperability bridge, not a reason to leak hidden local state. Each field must be filtered through Identity, Compartment, disclosure, and recipient policy.

### 20.3 Structured extraction model strategy

The initial extractor strategy should use general LLMs or local models constrained by schemas, validation, and repair loops. A specialized fine-tuned or distilled extractor model may become valuable later after UbU has stable schemas and corrected training examples.

The system should not require a new foundation model. The likely requirement is reliable structured extraction, not open-ended conversation.

### 20.4 Personalized TTS and voice descriptors

A **Voice Profile Descriptor** is a future optional communication metadata object that may describe pronunciation, cadence, voice style, or a reference to an approved local voice profile.

The intended use is expressive and accessibility-oriented rendering: a receiver may convert text into spoken audio approximating the sender's usual style without receiving a noisy full audio recording. This can compress communication from audio to text plus metadata.

Voice descriptors are sensitive. They must be opt-in, policy-governed, clearly separated from authentication, and protected against deceptive impersonation.

---


## 21. Agentic Interaction, MCP, and Delegation Substrate

This section records the post-realtime LLM and agentic-platform design direction. The core rule is that unstructured AI can observe, converse, propose, and act under authority, but UbU remains the canonical state-transition, review, privacy, and planning layer.

### 21.1 Realtime interaction sessions

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

### 21.2 Structured-output admission pipeline

LLM, parser, worker, and agent outputs should follow an admission pipeline:

```text
output
  -> schema validation
  -> semantic validation
  -> Compartment and policy validation
  -> provenance and confidence attachment
  -> conflict detection
  -> user-review classification
  -> accepted canonical object or rejected candidate
```

This applies to structured message extraction, realtime observations, AssociationAttestations, Tasks, Log candidates, Plan repairs, Delegation Substrate packets, and agent outputs.

### 21.3 ContextBundle

A `ContextBundle` is a governed package of context assembled for a model, tool, worker, agent, or long-context review. It is the auditable boundary between UbU's explicit state and an execution backend.

Minimum fields:

- `context_bundle_id`;
- `schema_version`;
- `purpose`;
- `requested_operation`;
- `actor_identity_ref`;
- `related_task_refs`, `related_objective_refs`, or workflow refs when applicable;
- `route_request_ref` and `route_decision_ref` when the bundle is used for LLM routing;
- `destination_ref`, including provider/tool/worker, provider class, model or tool ref, and boundary classification;
- `source_refs`;
- `source_selectors`, time ranges, object ranges, or repository paths included;
- `source_snapshot_refs` or hashes sufficient to identify the assembled version;
- `compartment_refs`;
- `compartment_policy_results`;
- `identity_refs_exposed`;
- `association_refs_exposed`;
- `relationship_refs_exposed` when applicable;
- `data_categories_exposed`;
- `sensitivity_summary`;
- `token_or_size_estimate`;
- `redaction_policy`;
- `minimization_rules_applied`;
- `excluded_source_summary`;
- `prompt_injection_exposure_summary` when the bundle includes untrusted external text, webpages, messages, documents, or repository content;
- `retention_policy`;
- `training_or_secondary_use_policy`;
- `user_visible_summary`;
- `approval_state`;
- `approval_log_ref` when approval or denial was recorded;
- `created_at`;
- `expires_at`;
- `invocation_refs`;
- `supports_candidate_refs`;
- `exposure_summary_ref`;
- `log_refs`.

ContextBundles may contain raw payload only when the task requires it and policy allows it. The preferred representation is references, selectors, summaries, hashes, redacted excerpts, or structural fields that let the model perform the requested work without copying unnecessary private material. Repository and archive-scale bundles should record path or object selectors and excluded material, not merely a broad repository or archive name.

Compartment policy is checked before assembly and again before invocation. If any included source is denied for the destination, UbU must either exclude that source and record the exclusion or deny the bundle. A mixed-Compartment bundle inherits the most restrictive applicable route. User approval, capability grants, workflow settings, and provider preferences cannot override `no_cloud_llm`, `no_external_export`, `local_only`, or other hard Compartment denials.

User approval is required before routing a ContextBundle when the bundle sends protected payload, low-security un-compartmented content beyond structural references, Relationship evidence, Association evidence, private messages, broad repository or archive content, or other long-context bulk material to a cloud provider, external provider, remote worker, managed gateway, third-party tool, or different Identity/Association boundary. Approval is also required when the provider, model, operator, region, retention/training profile, data categories, minimization policy, or source scope differs materially from an already approved workflow template.

Policy-based approval is allowed only when a prior user-approved rule names the workflow, destination class, allowed Compartments or data categories, maximum source scope, minimization rule, retention profile, cost limits when relevant, expiry, and review requirement. The rule must still fail closed on hard Compartment denials or materially broader context than the rule allowed. Local-only deterministic processing or local model use may proceed without per-run approval when existing local policy permits it and the bundle does not cross a new disclosure boundary.

After invocation, UbU should create or update a context exposure summary. The summary records the actual provider/model/tool used, invocation refs, source counts and categories, Compartments, Identities, Associations, and Relationships exposed, raw-versus-summary/reference volume, redactions and exclusions, retention/training profile, prompt-injection exposure notes, approval and routing Log refs, downstream candidate refs, and any denied or omitted context that affected output quality. The summary is a review artifact and must not contain secrets or unnecessary raw payload.

ContextBundles are immutable after use. Corrections, narrower reruns, broader reruns, or provider changes create a new bundle or a superseding revision linked by Log entries. Candidate updates, AssociationAttestations, Log candidates, Plan repairs, Delegation Substrate packets, worker mutation requests, and other downstream outputs must carry `originating_context_bundle_refs` or equivalent provenance. Logs for approval, denial, invocation, output admission, and rejection should link back to the relevant ContextBundle.

### 21.4 MCP-style client and server boundary

UbU as an MCP-style client may call external tools and services. UbU as an MCP-style server may expose narrow tool surfaces to external agents.

Phase 1 server tools are query or candidate-submission tools only:

- `plan_summary.read`;
- `objective_status.query`;
- `task_candidate.create`;
- `log_candidate.submit`;
- `plan_repair.submit`;
- `clarification.request`;
- `delegation_packet.prepare`;
- `projection_preview.submit`;
- `association_attestation_candidate.submit` for fixtures or manually reviewed Association-introspection dogfooding.

Phase 1 server tools do not directly create, update, or delete canonical Objectives, Tasks, Logs, UniverseState, External References, Plans, Calendars, projection state, credentials, or external systems. Any write-like request becomes a candidate, worker-style mutation request, projection request, clarification request, or review artifact admitted by the canonical instance.

A minimum `MCPToolCapabilityGrant` includes grant ID, issuing parent instance, actor Identity, integration or agent identity, allowed tool names, operation kinds, payload schema refs, object refs or object-type scopes, Objective subtree refs, Task set or Container refs, Compartment constraints, Identity/Association disclosure constraints when applicable, integration or destination refs, time window or expiration, rate and cost limits, review requirement, idempotency/version requirement, audit policy, lifecycle status, and revocation refs. Compartment policy is a hard upper bound; a tool grant cannot override `local_only`, `no_cloud_llm`, `no_external_export`, allowed-integration, allowed-device, identity-isolation, or EvidenceUsePolicy denials.

Every external-agent write-like tool call uses a candidate envelope with tool call ID, capability grant ref, actor Identity, target refs, operation, payload, expected prior version when applicable, idempotency key, provenance, evidence refs, originating ContextBundle refs when any, Compartment policy result, confidence when relevant, and review requirement. The canonical instance validates schema, semantics, authority, Compartment policy, idempotency, expected version, assignment or workflow status, and review policy before applying, rejecting, or leaving the candidate pending.

Tool invocations that reach UbU are logged as append-only audit records with redacted argument summary or hash, policy decision, source and destination Identity, Compartment result, cost and rate counters when relevant, result refs, denial or failure reason when safe, and downstream candidate refs. Denied calls fail closed. Retries require the same idempotency key or an explicit superseding request. Rollback is append-only repair through compensating mutation, Log correction, supersession, Task moot transition, projection repair, or AgentAction mitigation metadata; prior invocation and candidate records are not rewritten.

The minimum dogfooding fixture is a local loopback MCP adapter over synthetic or redacted UbU project data. It exposes the Phase 1 server tools, exercises capability grants and Compartment denials, writes only candidate/review artifacts and audit Logs, and supports a dry-run client path for calling external tools without live external mutation.

### 21.5 Delegation Substrate

A Delegation Substrate packet prepares a Task for execution by making the work explicit enough to hand off, automate, review, or perform oneself with less ambiguity.

Provisional fields:

- `delegation_packet_id`;
- `task_ref` or `objective_ref`;
- `purpose`;
- `executor_type` (`self`, `local_agent`, `remote_agent`, `tool`, `human_identity`, `association`, `general_contractor`, `external_provider`);
- `executor_ref` when known;
- `authority_source`;
- `authority_scope`;
- `expected_output`;
- `required_evidence`;
- `review_requirement`;
- `privacy_scope`;
- `compartment_refs`;
- `allowed_tools`;
- `timebox_or_deadline`;
- `failure_path`;
- `escalation_path`;
- `status`.

Status values may include `not_delegated`, `prepared`, `proposed`, `offered`, `accepted`, `in_progress`, `delivered`, `reviewed`, `rejected`, `completed`, and `cancelled`.

When `executor_type` is `self`, the packet acts as a self-reminder and execution aid. It does not imply external handoff.

### 21.6 General Contractor

A General Contractor coordinates multiple subordinate executors to satisfy an Objective or complete a Container of Tasks.

A General Contractor may be a human Identity, agent, or Association. The role may decompose work, assign subtasks, monitor status, request evidence, and submit candidate updates, but only within explicit authority and Compartment bounds.

Open details include subdelegation, authority propagation, budget constraints, evidence requirements, review rights, and failure/escalation semantics.

### 21.7 Skill Barter marketplace direction

The Skill Barter marketplace is a future specialization of Association coordination, Delegation Substrate, Resource/Skill readiness, and Technique evidence. It is not a Phase 1 public marketplace.

The private Skill model should come first. A user must be able to model, learn, test, refresh, and use Skills for their own life logistics before UbU exposes those Skills to other Identities. Once private skill evidence and Delegation Substrate packets are useful, Skill Barter can become a voluntary exchange layer rather than a generic labor marketplace.

A mature Skill Barter system may support privacy-preserving skilled-work exchange, pseudonymous capability claims, reputation without unnecessary doxxing, scoped work agreements, private or selective-disclosure evidence, lawful user-controlled settlement references, dispute workflows, and human/agentic executor discovery.

For EthConf and cypherpunk outreach, Skill Barter is useful as a future-direction signal: UbU can preserve cryptocurrency-native values of voluntary coordination, sovereign identity, privacy, FOSS development, open markets, economic self-sufficiency, and lifelong skill acquisition without becoming token-first or speculation-first.

The preferred framing is an open, user-sovereign skill economy or a self-reinforcing ecosystem for skill acquisition, task execution, and voluntary exchange. Avoid calling it a closed ecosystem, because that implies platform captivity rather than user sovereignty.

### 21.8 AgentAction and BackgroundProcess

An `AgentAction` is a bounded action by an agent or tool against an external surface. A `BackgroundProcess` is a recurring or conditional process that may run without occupying user Calendar time.

Provisional shared fields:

- `actor_identity_ref`;
- `agent_or_tool_ref`;
- `trigger`;
- `schedule_or_condition`;
- `authority_scope`;
- `credential_refs`;
- `compartment_refs`;
- `external_surface_refs`;
- `irreversible_side_effect`;
- `prompt_injection_exposure`;
- `compute_or_cost_budget`;
- `notification_policy`;
- `rollback_or_mitigation_path`;
- `completion_evidence`;
- `failure_escalation_policy`.

These objects are not equivalent to ordinary user Calendar events. They may consume compute, credentials, money, privacy budget, or external authority without consuming the user's direct time.

### 21.9 State-transition cockpit

The long-term UX should present the current state transition and its evidence, constraints, expected effects, authority, privacy scope, and available actions.

Phase 1 proves this through one recommended next Task and an inspectable explanation. Later cockpit surfaces may include message triage, Log review, Plan repair, Delegation Substrate packets, agent approvals, AssociationAttestation review, organizational introspection, and projection publication.


## 22. Relationships

A **Relationship** is structured UniverseState data representing the relationship between Identities as modeled by a particular UbU instance. Relationships are not globally objective social truth. They are perspective-bound, Compartment-governed, evidence-backed models that help the user reason about communication, obligations, trust, scope, boundaries, and maintenance.

MVP Relationship data remains intentionally narrow:

- identity refs for the relevant parties;
- at least one Identity controlled by the user;
- user-stated affect or stance toward the other Identity where the user chooses to record it;
- inferred/speculated hypotheses about the other Identity's stance only as candidate, reviewable, evidence-backed claims;
- ordinary references to Logs, External Events, Tasks, Compartments, or External References when source policy permits.

Communication and maintenance metadata are not stored directly on Relationship in MVP. Cadence policy for "keep this relationship warm" lives on an evergreen Objective's recurrence/reactivation rule. Observed interactions live in Logs, External Events, Task history, or Compartment/external references, depending on source and sensitivity.

Objectives attach to Relationships indirectly through UniverseState or explicitly through `RelationshipScopeTransition` targets. A relationship-maintenance Objective points at the Relationship or relationship-relevant UniverseState key, and its Tasks mutate ordinary UniverseState or append events. A scope-transition Objective points at a `RelationshipScopeTransition` target and uses ordinary Techniques, Steps, and Tasks to plan user-controlled actions.

Neglect risk is not stored on Relationship in MVP. It is a risk-report finding derived from the maintenance Objective's recurrence rule plus observed interaction history and current Plan state.

### 22.1 Epistemic asymmetry and perspective decomposition

Relationships have **epistemic asymmetry**. The user has first-person access to the user's own declarations and experiences, but even the user's self-model can conflict with observed behavior, affect history, and later reflections. The counterparty's perspective is never known directly by UbU; it is represented only as evidence-backed hypotheses about how the counterparty may understand, value, or scope the relationship.

The richer post-MVP Relationship model should lazily decompose the user's side rather than treating `ownPerspective` as a monolith:

- `ownDeclaredPerspectiveHistory`;
- `ownObservedBehaviorRefs`;
- `ownAffectHistoryRefs`;
- `ownReflections`.

`ownDeclaredPerspectiveHistory` is versioned. A changed declaration is meaningful evidence, not a silent overwrite. It may legitimize, supersede, or conflict with observed scope drift.

The counterparty side should use domain- and scope-tagged hypotheses rather than a single `speculatedPerspective`:

- `claim`;
- `domain`;
- `scope`;
- `evidence_refs`;
- `counterevidence_refs`;
- `confidence`;
- `status`;
- `last_reviewed`.

Domain examples include `technical_collaboration`, `emotional_availability`, `professional_boundary`, `money_and_value`, `authority`, `reciprocity`, `trust`, `intimacy`, and `conflict_style`. UbU may detect internal tension between hypotheses as a finding about the user's model, not as a claim about the counterparty's inner state.

### 22.2 Extrospection

**Extrospection** is relationship-scoped, evidence-backed review of the user's counterparty-perspective hypotheses, Relationship scope declarations, trust boundaries, reciprocity assumptions, affective impact, and functionality assumptions. It reuses the attestation pattern from introspection and AssociationAttestation, but its attestation target is weaker: the user's hypothesis about another party, not the other party's actual perspective and not a value the other party necessarily declared.

Extrospection should confront the user with evidence in tension rather than produce authoritative resolution. A review may surface supporting evidence, disconfirming evidence, ambiguity, insufficient evidence, clarification prompts, possible model updates, and possible introspection handoffs. Disconfirming evidence requires a confirming-evidence search when permitted by the corpus; if no confirming evidence is found, UbU should say so rather than fabricate balance.

Durable extrospection records should prefer Compartment-aware `EvidenceRef`s, selectors, summaries, redaction policy, retention policy, and EvidenceUsePolicy over copied private excerpts. Raw excerpts may be displayed or retained only when source policy permits. Evidence from old interactions should decay in salience unless reactivated by new evidence, user review, or a current Objective. Cross-Relationship inference is prohibited by default.

Extrospection assessments should be separated rather than collapsed into a single health score:

- `ScopeAssessment`: is the relationship operating within the declared or intended scope?
- `BoundaryAssessment`: are privacy, trust, money, labor, intimacy, authority, and Identity boundaries being respected?
- `ReciprocityAssessment`: are obligations, benefits, attention, and effort consistent with the declared relationship type?
- `TrustCalibrationAssessment`: is the user's level of trust evidence-calibrated, including over-trust and under-trust?
- `AffectiveAssessment`: how does the relationship affect the user?
- `FunctionalityAssessment`: is the relationship serving its intended role without unsustainable cost or boundary violation?

`ScopeAssessment` logically precedes the other assessments because a wrong or stale scope can make other evaluations use the wrong frame. Negative affect is not equivalent to dysfunction; UbU should distinguish hedonic valence from normative alignment. A user may record `userAcceptedDiscomfort` when discomfort has been reviewed and accepted as value-aligned, with reopening only when new evidence, changed capacity, or changed values justify it.

Extrospection findings use an explicit lifecycle:

- `candidate`;
- `deferred`;
- `resurfaced`;
- `reviewed_accepted`;
- `reviewed_rejected`;
- `superseded`;
- `archived`.

Unreviewed findings must not silently update durable Relationship state. Rejected findings should remain durable enough to suppress repeated bad framings. Deferred findings may resurface when materially new evidence accumulates. High-frequency extrospection sessions without model updates may trigger a rumination advisory, but such behavioral-risk detection is advisory by default.

### 22.3 RelationshipScopeTransition

Users may deliberately create Objectives to change their own participation in a Relationship, invite or test mutual scope changes, or clarify whether another party is willing to enter a different scope. Examples include exploring whether a friend is open to a romantic scope, downscoping a close friendship to a formal-acquaintance scope, formalizing an observed professional collaboration, or clarifying an ambiguous drift.

The valid target is not control over another person's feelings, decisions, or Identity. UbU may plan ethical user-controlled actions such as disclosures, invitations, boundary-setting, availability changes, communication changes, and clarification attempts. Counterparty response remains autonomous evidence.

`transition_type` distinguishes:

- `unilateral`: the user changes the user's own availability, disclosure, communication, or participation;
- `exploratory`: the user makes a disclosure, invitation, or proposal to test whether the counterparty is open to a scope change;
- `mutual`: both parties have already indicated interest and the transition is being coordinated;
- `retroactive_scope_clarification`: the relationship has drifted or become ambiguous and the user seeks explicit clarification of the operating scope.

Consent-dependent or mutual transitions separate `process_success_criteria` from `outcome_observations`. The user can reach a terminal process-successful state even if the counterparty declines. Valid outcome observations include `accepted`, `declined`, `ambiguous`, `insufficient_evidence`, and `no_response`.

All actionable Steps and Tasks in a RelationshipScopeTransition must be anchored to user-controlled behavior. A Step is malformed if its completion criterion requires a counterparty mental state, feeling, or decision. An exploratory transition should have a single disclosure or proposal, one clarification if ambiguous, and termination of the current Objective on authentic refusal. A future re-opening requires materially new evidence, a new Objective, and stronger review.

Prohibited strategy types should be typed rather than freeform. The taxonomy includes:

- `artificial_urgency`;
- `false_scarcity`;
- `vulnerability_exploitation`;
- `incremental_boundary_erosion`;
- `strategic_information_withholding`;
- `emotional_state_manipulation`;
- `leveraging_privileged_affect_knowledge`;
- `manufactured_dependency`;
- `social_pressure_via_third_parties`;
- `retaliation_or_threat`;
- `coercive_leverage`;
- `deceptive_self_presentation`;
- `repeated_pursuit_after_refusal`.

The affect-knowledge firewall is central: extrospection-derived affect knowledge may be used for harm avoidance, not persuasion optimization. For example, UbU may advise that a stressed counterparty should not be pressured with a scope-change disclosure now; it should not recommend that the user's request is more likely to succeed because the counterparty is vulnerable.

`counterparty_autonomy_acknowledgment` records that the counterparty may decline, decline is valid, decline is not user failure, and an authentic refusal terminates the current exploratory Objective. A graceful nonachievement path should define a valid terminal state, relationship model update options, counterparty-hypothesis review, introspection handoff, affective support, and cooling period. Immediate factual logging may occur after an emotionally intense transition attempt; deeper interpretive extrospection should generally wait until after the interaction concludes.

Romantic, professional, and dependency-heavy transitions may have stronger default advisory safeguards because they can involve asymmetric vulnerability or power. `power_asymmetry`, `power_asymmetry_basis`, `vulnerability_profile`, and `pacing_constraints` should be represented explicitly. Pacing constraints may include minimum time between Steps, mandatory observation periods, cooling periods after decline, evidence required before the next Step, and maximum prompt frequency.

### 22.4 Advisory safeguards and hard boundaries

Relationship-transition review gates are default-on advisory safeguards unless the user has configured them as self-imposed required gates or an external constraint makes them unavoidable. UbU should recommend pre-action introspection, extrospection, TrustCalibrationAssessment, power/vulnerability review, pacing review, graceful nonachievement planning, and manipulation-risk review where relevant. The user may bypass or disable such safeguards. Bypass decisions are introspection-relevant evidence and may surface as candidate findings about conflicts between revealed behavior and declared values.

Hard boundaries are narrower. They cover structurally enforceable product invariants and explicit policies: Compartment boundaries, privacy policy, data-access authorization, EvidenceUsePolicy restrictions, export prohibitions, identity/Compartment isolation, provenance integrity, audit-record integrity, integration authorization, and unavoidable provider/platform/legal constraints. Provider, platform, app-store, integration, and legal constraints should be recorded as external constraints with source, scope, affected workflow, and enforcement path when known.

Behavioral-risk checks such as manipulation, coercion, harassment, stalking, deception, power-asymmetry risk, rumination risk, or relationship-transition ethics are fallible classifier judgments. Treating them as hard gates would risk false positives that override legitimate user autonomy, false negatives that create misplaced trust, and a false impression that UbU can reliably prevent misuse or illegal behavior. They should generally be default-on, uncertainty-aware, user-overrideable advisory checks with introspection consequences when bypassed.

A user-configured required gate for a RelationshipScopeTransition is not reclassified as a product hard boundary. It is a self-imposed policy gate that may block the current workflow while enabled and should record the configuring Identity, scope, review requirement, bypass or disablement authority, and relevant Log refs.

Relationship documentation must not claim that UbU prevents coercion, harassment, stalking, manipulation, or illegal behavior. It may claim that UbU preserves structural privacy and authorization boundaries, surfaces advisory behavioral-risk findings, records bypasses as introspection-relevant evidence, and refuses actions only when a product hard boundary, user-required gate, or external constraint applies.

---

## 23. Zones, Devices, and Compartments

### 23.1 Device

A **Device** is an execution enclave, not necessarily physical hardware.

Examples:

- OS user profile
- container
- VM
- secure enclave

One physical machine may host multiple Devices.

### 23.2 Zone

A **Zone** is a workspace-like UbU instance context.

A Device belongs to one Zone.

A Zone may have many Devices.

Zones maintain explicit allowlists/denylists of Compartments, defaulting to deny.

### 23.3 Compartment

A **Compartment** is a first-class data containment object.

Compartments enforce hard invariants that the user cannot casually override.

Hard invariants may include:

- storage backend constraints
- device eligibility constraints
- identity disclosure constraints
- export/integration constraints
- retention constraints
- audit constraints

#### Phase 1 Compartment baseline

Phase 1 implements Compartments as explicit classification and routing guardrails, not as the complete Phase 2 multi-device containment system. The minimum Phase 1 promise is:

- Compartment-marked content stores a `compartment_ref` and is handled through references rather than embedded in ordinary WorkItem structure.
- A Compartment may declare `local_only`, `no_cloud_llm`, `no_external_export`, `allowed_integration_refs`, and `allowed_device_refs` policies.
- Content in a `no_cloud_llm` Compartment must not be sent to cloud LLM routes. Content in a `no_external_export` Compartment must not be exported, projected, or handed to workers except as redacted structural references.
- Other Compartment-marked content may cross a boundary only through an allowed route and a user-visible action. Boundary-crossing attempts that reach UbU are recorded in Logs as allowed or denied.
- In Phase 1, device eligibility is limited to the current single local Device / execution enclave and configured allowlists. Hardware attestation, cross-device partial replication enforcement, and cryptographic isolation are not MVP claims.
- Phase 1 does not promise automated retention deletion or redaction for Compartment content. Retention policies may be recorded, but enforcement beyond append-only Log retention is post-MVP unless separately implemented and disclosed.

Phase 1 public messaging must describe this as a minimal Compartment guardrail layer. It must not claim complete privacy isolation, complete retention enforcement, secure multi-device Compartments, or protection from a malicious local administrator.

### 23.4 Sensitive content

WorkItems must remain structurally usable without dereferencing sensitive content.

Sensitive Compartment data should be referenced by link/reference, not embedded in WorkItem structure.

Un-compartmented content is treated as low-security and may require per-integration prompts.

In Phase 1, un-compartmented content is explicitly labeled `security_level: low`. Low-security content may be used by configured integrations or cloud LLM-backed Automation Workers after a user-visible integration or worker action, but UbU must not market that content as compartment-protected.

### 23.5 Phase 1 local-first and cloud LLM disclosure

Phase 1 can honestly claim that canonical planning state, Logs, and source-linked project model data live in the local single-user UbU instance by default. It cannot claim Phase 2 local-first sync, conflict handling, partial replication, or secure multi-device Compartment propagation.

Phase 1 can honestly claim that cloud LLM usage is optional, explicit, and advisory. Cloud LLMs may be used by configured Automation Workers, Super Automation flows, or bootstrap tools such as model-committee, but they are not the canonical planner and must not receive `no_cloud_llm` Compartment content. When a workflow uses cloud LLMs on un-compartmented low-security content, the UI or run artifact must disclose that routing before or at execution time.

---

## 24. Automation Workers and Super Automation

### 24.1 Automation Worker

An **Automation Worker** is a worker-mode UbU instance or compatible execution unit that performs delegated work.

Automation Workers may:

- run LLM queries;
- process photos or screenshots;
- label GitHub events;
- perform external API actions;
- run document analysis;
- submit authorized mutations to a canonical UbU instance.

A worker-mode instance is externally represented as an Identity.

### 24.1.1 Worker identity and capabilities

A worker Identity may receive only explicit capability grants. The Identity stores stable external identity metadata and credential references; authority lives in separate capability-grant objects attached to that Identity and issued by a specific parent UbU instance. A grant names allowed actions, scope limits, lifecycle state, issuance time, expiration time when present, and audit/provenance metadata.

Phase 1 capability verbs are:

- `task.read`;
- `objective.read`;
- `universe_state.read_subset`;
- `external_event.append`;
- `snapshot.submit`;
- `mutation_request.submit`;
- `recalculation.request`;
- `projection.github.request_update`.

Workers do not directly mutate canonical Task, Objective, UniverseState, `pipeline_state`, or GitHub projection state in Phase 1. They may submit mutation requests or projection update requests, and the canonical instance validates those requests, writes accepted changes, and logs applied or rejected outcomes. Task creation, Objective creation, Task mutation, Objective mutation, `pipeline_state` mutation, and direct external writes are represented through `mutation_request.submit` or a narrower projection request rather than through direct write authority.

Worker endpoints are the Phase 1 worker profile of the future inter-instance protocol. They should carry the same envelope concerns the later protocol will need: parent instance, worker Identity, capability grant reference, payload kind, target refs, expected prior version or idempotency key where relevant, provenance, Compartment/export decision, and review requirement. Phase 1 does not need to expose native user-to-user messaging or generic remote instance discovery through this API.

Capability grants may be scoped by object ID, Objective subtree, Task set, Compartment, external integration, operation kind, and time window. Compartment policy is a hard upper bound on worker authority: a capability grant cannot authorize access, export, or worker handoff that the relevant Compartment forbids.

A worker may be revoked by disabling or deleting its capability grants, rotating or invalidating credentials, and rejecting later submissions from that grant. Revocation does not rewrite prior Logs. Credential rotation creates a new credential version linked to the same worker Identity and capability grants unless the grant is also changed.

A single worker-mode instance may serve multiple parent organization-mode or user-mode instances only through separate parent-specific capability grants, credentials, and audit trails. Cross-parent data sharing is forbidden unless each parent explicitly grants a route and all relevant Compartment policies allow it. Workers may operate for user-mode instances, but affect and other personal data require especially narrow read-subset grants and must respect `no_cloud_llm`, `no_external_export`, and low-security disclosure rules.

### 24.1.2 Worker assignment lifecycle

Phase 1 worker work discovery is explicit assignment by the parent UbU instance. A worker may poll a parent-specific assignment inbox or receive pushed notifications, but the inbox returns only work that the parent has already assigned or offered to that worker Identity. Worker check-ins may advertise health, availability, local resource state, and granted capability metadata so the parent can choose an eligible worker; they are not an open task-claim or marketplace mechanism.

A worker assignment is a scoped execution lease that binds one Task or Delegation Substrate packet to one worker Identity for one active executor slot. The minimum assignment record includes `assignment_id`, parent instance ref, `task_ref`, worker Identity ref, capability grant ref, assigned-by Identity or authority source, assignment status, lease or heartbeat deadline, expected output summary, review requirement, idempotency key, and provenance. The assignment may reference Compartment decisions or ContextBundles when the work handoff exposes protected or low-security content.

The minimum Phase 1 assignment statuses are `offered`, `accepted`, `in_progress`, `clarification_requested`, `delivered`, `completed`, `rejected_by_worker`, `cancelled`, `expired`, and `failed`. `delivered` means the worker has submitted output or evidence for parent review; `completed` means the canonical instance accepted the assignment outcome or otherwise determined that no further worker action is required. Task status remains canonical on the Task itself and is not replaced by assignment status.

Multiple workers may observe the same Task only through explicit read grants, observer roles, or separate review assignments. Observation does not confer execution authority. In Phase 1, only one worker may hold the active execution lease for a given executor slot on a Task. If apparently parallel work is needed, UbU should model separate child Tasks or separate named assignment roles rather than allowing anonymous workers to compete for the same Task. Competitive worker claiming, work stealing, and marketplace-style bidding are deferred.

Every assignment lifecycle transition is logged. Phase 1 extends the Log event-type set with `worker_assignment_updated`; its payload records the assignment id, old and new status when applicable, worker Identity, Task ref, capability grant ref, lease deadline, reason, evidence refs, idempotency key, and whether recalculation was requested. Assignment events that affect the current or next recommended Task, worker bottleneck risk, or Plan feasibility create or batch a `recalculation_triggered` Log entry using the existing trigger taxonomy.

Workers may reject assignments by returning `rejected_by_worker` with a reason and evidence when relevant. Rejection does not mutate the Task directly. The canonical instance decides whether to reassign, ask the user, mark the Task blocked, revise the Delegation Substrate packet, or leave the Task available for manual work.

If a worker disappears mid-Task, the parent marks the assignment `expired` after the heartbeat or lease deadline, logs the transition, and treats the Task as still unresolved unless separate accepted evidence proves completion or mootness. Late worker submissions after expiration are stale by default and must pass expected-prior-version, idempotency, authority, and review checks before they can affect canonical state. The parent may then reassign the Task, create a retry or repair Task, request clarification, or surface the worker bottleneck in risk reporting. Retry construction follows the worker retry semantics below.

Workers may request clarification by submitting `clarification_requested` status or an authorized mutation/request payload. The canonical instance may convert that into a clarification Task, user prompt, revised assignment packet, or rejection of the request.

#### Automation child Task structure

An Automation/Super Automation Task that expands into multiple canonical children is structurally replaced by a separate Container. The Task itself is not reused as the Container handle. The original Task usually becomes `moot` with reason code `replaced_by_new_plan_structure`.

Automation child Tasks are ordinary Tasks, normally Dynamic Tasks. Phase 1 does not define an automation-step subtype. A child may be Static only when it independently has fixed start and end times under ordinary Static Task rules.

Workflow-level automation metadata belongs on Container lineage and provenance. Child-specific automation metadata belongs on ordinary Task delegation fields, Delegation Substrate packets when present, worker assignments, capability-grant refs, worker mutation request refs, originating ContextBundle refs, External References, and Logs.

Workers may propose child Tasks or the restructuring Container only through authorized mutation requests. The canonical instance validates scope, expected prior version, Compartment/export policy, idempotency, review policy, and External Reference changes before admitting canonical children.

Retries use the worker retry semantics below: a failed child stays failed, and retry work is a new sibling Task under the same Container or lineage with retry metadata.

#### Worker retry semantics

A failed worker-created child Task is not mutated back to `active`. The failed attempt remains a failed Task with its own assignment, evidence, and Logs. When retry is warranted, UbU creates a new retry sibling Task with a new `task_id` under the same parent Container or lineage when one exists, linked by `retry_of_task_ref`, `retry_root_task_ref`, `attempt_number`, `retry_policy_summary`, and failure or evidence refs.

Worker assignment failure on an existing Task may leave the underlying Task unresolved when no attempt Task was created. In that case retry or repair work is represented by a new assignment or retry/repair Task; accepted completion evidence or mootness is required before the underlying work stops being planned.

The effective retry policy resolves from Task or Delegation Substrate packet, then parent Container or Objective, then worker or integration default, then global Phase 1 default. Worker and integration defaults cannot exceed capability grants, Compartment/export policy, assignment leases, idempotency, expected-prior-version, or review policy.

The Phase 1 global default is at most three total attempts per retry root, including the initial attempt. No automatic retry is created for policy denials, authorization failures, Compartment/export denials, invalid mutation requests, missing required human review, or stale expected-prior-version conflicts.

Each retry attempt has its own assignment, idempotency key, status transitions, and Task outcome Logs. Failure is logged with `task_failed`, assignment changes with `worker_assignment_updated`, worker request failures with `worker_mutation_rejected` when applicable, and retry-sibling admission through the normal Task creation or mutation-request admission path. Retry metadata must keep the attempt chain queryable without rewriting prior attempts.

When attempts are exhausted, UbU stops automatic retry creation, leaves the latest attempt failed unless another accepted lifecycle transition applies, and routes the parent work to human review, clarification, reassignment, repair, or a moot/supersession decision through ordinary admission rules.

Worker failure affects derived reports and recalculation. Failed, expired, rejected, repeated, or exhausted worker attempts feed `worker_or_automation_bottleneck`, may increase dependency fragility or deadline risk, and create or batch recalculation triggers when they can affect the current or next recommended Task, Plan feasibility, or risk-report validity.

### 24.1.3 Worker mutation request schema

Phase 1 worker submissions use three bounded payload families:

- event or observation submissions, such as authorized External Event, Snapshot, Log-candidate, status, clarification, or recalculation requests;
- mutation requests, which are the normal worker path for changing canonical UbU objects;
- proposed patch artifacts, which are reviewable evidence or an explicit patch-style mutation request for supported textual or external artifacts, never direct canonical edits.

A worker mutation request is a candidate state-transition envelope. It is not a canonical write until the parent instance validates it, applies it when policy allows, and writes the corresponding Log entries. The minimum request fields are `mutation_request_id`, `schema_version`, `parent_instance_ref`, `worker_identity_ref`, `capability_grant_ref`, `authority_source`, `submitted_at`, `target_ref`, `operation`, `operation_payload` or `new_value`, `reason`, `evidence_refs`, `provenance`, `idempotency_key`, and either `expected_prior_version` or an explicit no-prior-version reason for create-only requests. Optional fields include `assignment_ref`, `effective_at`, `old_value_observed`, `originating_context_bundle_refs`, `compartment_refs`, `compartment_policy_result`, `confidence`, `review_requirement`, `batch_id`, `batch_atomicity`, and `supersedes_request_ref`.

Phase 1 mutation operations are closed and typed. They include UniverseState mutation-list requests using the accepted mutation vocabulary; Task lifecycle transition requests, including completion, failure, moot, and estimate or delegation-field updates; Objective transition requests; External Reference create, verify, supersede, or duplicate-link requests; `pipeline_state` projection-state requests; Task or Objective candidate creation requests; Task-to-Container restructuring requests; and Log correction or annotation requests. New operation kinds require a schema migration or accepted decision. Projection writes, GitHub API actions, and external side effects remain separate projection or AgentAction requests and do not become direct canonical mutations.

Valid requests are not always applied immediately. A request may be applied immediately only when schema validation, semantic validation, capability scope, Compartment/export policy, idempotency, expected-prior-version checks, assignment lease status, and operation review policy all pass and the request's `review_requirement` allows automatic application. Otherwise a valid request remains a submitted candidate awaiting human or policy review. Invalid, unauthorized, stale, duplicate-conflicting, or policy-denied requests are logged as `worker_mutation_rejected` with a sanitized reason and evidence references when safe.

Batches are allowed when the envelope declares a `batch_id` and `batch_atomicity`. `all_or_nothing` batches validate the whole batch before application and either apply every item in declared order or reject the batch without partial canonical mutation. `independent_items` batches may apply valid items and reject invalid items separately, but this is not atomic. MVP implementations may restrict atomic batches to one parent instance and one validation transaction.

Workers may request creation of new Objectives, Tasks, External References, child Tasks, or Containers only as mutation requests or candidates. They do not receive direct create authority in Phase 1. Objective creation should require review by default unless a later policy grants a very narrow auto-create path. Task creation may be auto-applied only when the grant, assignment, operation kind, target scope, and review policy explicitly allow it.

Worker mutations are reversible only through append-only repair. UbU does not rewrite or delete the original request or applied Log entry. Reversal uses a compensating mutation, Log correction, superseding object, Task moot transition, or projection repair. Requests should include enough observed old value, evidence, and expected-prior-version metadata to make compensation possible when the operation is logically reversible. Irreversible external side effects require projection or AgentAction mitigation metadata rather than ordinary mutation reversal.

Stale overwrite prevention is mandatory. The canonical instance compares `expected_prior_version` or equivalent target version metadata with current canonical state before applying mutation requests. A mismatch rejects the request or converts it into a conflict candidate for review; it must not silently overwrite newer state. Idempotency keys prevent duplicate application, assignment leases prevent expired worker work from being accepted accidentally, and capability-grant versions prevent revoked or changed authority from authorizing late submissions.

### 24.1.4 Model-committee worker pattern

A model-committee worker is an Automation Worker pattern that reads canonical project state, runs configured provider workflows against a selected open question or problem, produces candidate changesets, scores candidate changesets, and writes reviewable artifacts.

In v0.1, this means Codex CLI plus local Ollama proposal workflows. In v0.2, this expands to Codex and Claude Code frontier-provider cross-scoring, with local providers remaining useful for diversity and fallback.

In both versions, model-committee is still an external bootstrap tool rather than a fully integrated UbU worker-mode runtime component.

### 24.2 Worker mode

Worker mode:

- is one of the three mutually exclusive UbU modes;
- is usually daemon/service-oriented;
- may expose an admin web UI;
- may use CLI settings;
- often runs on GPU-capable hardware;
- may control cloud compute resources.

### 24.2.1 Worker-mode public UX

Worker-mode public UX is an administrative console for the worker operator, not the primary planning UI.

The first worker-mode screen should show:

- connection and identity status for the parent UbU instance;
- current service health and last check-in;
- active assignment, assignment queue, and retry/error state;
- granted capabilities and their scopes;
- recent Log submissions, mutation requests, and rejection reasons;
- local resource status, including GPU availability and cloud compute state when applicable.

The worker UI may expose detailed logs and operational controls, but it should default to summarized, redacted views. It must not expose Compartment-protected payloads, broader organization planning state, or personal affect data merely because the worker executed related work.

No worker-mode web admin UI is required for Phase 1. Phase 1 only needs enough visible worker assignment/status information for the GitHub dogfooding demo, which may be shown in the user-mode dogfooding UI, CLI output, or run artifacts.

### 24.3 Super Automation

**Super Automation** is a product/UX pattern, not the technical name of the device.

Super Automation uses Automation Workers, local services, or external APIs to abstract away difficult external interactions from the user.

It may include:

- screenshot interpretation;
- user-mediated UI actions;
- GitHub processing;
- document parsing;
- API work;
- LLM-assisted workflow execution.

---

## 25. Organization Mode

Organization mode is an instance-wide option.

It is isolated from personal user-mode installations.

Organization mode:

- does not model intrinsic affect;
- may disable affect dimensions on Relationship objects;
- may use organizational Preferences as directives;
- may use authority-source metadata/logging;
- may delegate work to Automation Workers;
- may expose a web interface.

For MVP, roles/RBAC may be omitted and all users treated as admin-equivalent.

### 25.1 Organization-mode object rules

Organization mode uses the shared core object model unless a field or behavior is intrinsically personal-affect-specific. Objective, Preference, Task, Container, UniverseState, Snapshot, Plan, Calendar, Log, Identity, Relationship, Compartment, Automation Worker, External Event, External Reference, and deferred Association-related objects are available in organization mode to the depth needed by the instance's project-planning workflow.

Intrinsic affect fields are structurally absent from organization-mode objects where a mode-specific schema is used. Where a shared storage schema contains affect-capable fields for implementation convenience, those fields are disabled in organization mode and validation must reject organization-created intrinsic-affect values rather than merely ignoring them. Organization mode may still store non-intrinsic human-related project constraints, availability facts, risk-report findings, or externally supplied structural signals when those signals are allowed by source authority, Compartment policy, and consent/export rules.

Relationship objects may exist in organization mode without affect dimensions. An organization-mode Relationship represents structured UniverseState between Identities, such as contributor, maintainer, worker, project, vendor, or integration relationships. It may carry non-affect project metadata through ordinary UniverseState facts, External Events, Logs, External References, candidate AssociationAttestations, Objectives, or Tasks, but it must not claim the organization has a private emotional state toward another Identity.

Organization-created Objectives, Preferences, and Tasks require `authority_source` metadata as defined in `DESIGN.md` section 17.9.

Before RBAC exists, organization-mode users are represented as human operator Identities with admin-equivalent authority over the organization-mode instance. This is an MVP authority simplification, not a claim that all future organization users have the same role. Actions still write Logs with actor Identity and provenance so later RBAC can be introduced without erasing earlier history.

Future personal `user_mode` instances may provide limited signals to an organization-mode instance only through explicit user-controlled sharing, Identity-mediated authorization, Compartment/export checks, and clear provenance. Phase 1 does not implement cross-instance personal-to-organization sharing. Later designs should prefer structural signals such as availability, commitment status, task completion, or user-approved projection summaries rather than raw affect, private relationship state, or broad personal context.

### 25.2 Organization-mode public UX

Organization-mode public UX is a project operations dashboard for human operators of an organization or project instance.

The first organization-mode screen should show:

- pipeline state for imported/project WorkItems and Objectives;
- risk summary for current Plans, including deadline, dependency, and worker bottleneck risk;
- worker assignments, worker health, and pending mutation requests;
- GitHub projection and reconciliation status;
- recent decision, projection, and worker Log entries that need operator attention.

Organization mode may expose pipeline state, risk reports, worker assignments, and GitHub projection status as first-class public UI concepts. It should not expose personal user-mode affect surfaces, and admin-equivalent MVP assumptions must be labeled when no full RBAC is implemented.

No organization-mode web admin UI is required for Phase 1. The Phase 1 public demo may show organization-like project coordination data, but the required UI is the single-user dogfooding surface defined by the Phase 1 demo criteria, not a general organization console.

---

## 26. GitHub Dogfooding and Projection

GitHub is a projection of UbU state, not the source of truth.

UbU should be able to use GitHub as an interface for FOSS contributors while maintaining canonical state internally.

### 26.1 GitHub object mapping

Provisional mapping:

- GitHub Issue → one or more Objectives
- Objective → one or more GitHub Issues
- GitHub PR → External Event and/or analysis Objective
- Review request → Task
- CI failure → External Event and/or analysis Objective
- Milestone/release → Objective with Calendar detail
- Contributor comment → External Event and/or analysis Objective

Contributor interactions may also be associated with a GitHub Identity and relationship-maintenance Objective when the user has explicitly modeled that contributor relationship. These imported events may update relationship-relevant UniverseState or satisfy/reactivate the maintenance Objective, but they do not directly rewrite private Relationship affect fields without user acceptance.

### 26.2 Pipeline state

`pipeline_state` is generic projection-scoped workflow/project-management state. It is not GitHub-specific, although Phase 1 uses it first for GitHub dogfooding and managed-label projection.

`pipeline_state` is not stored directly on `Objective` in Phase 1. It is stored as projection metadata keyed by one projection plus one target Objective. One Objective may therefore have multiple current pipeline states for multiple projections, such as GitHub issue labels, a future organization board, or a local release workflow.

MVP projection-state record:

- `pipeline_state_id`
- `pipeline_projection_id`
- `target_ref` (`Objective` only in Phase 1)
- `pipeline_state`
- `authority_source`
- `source_refs`
- `external_reference_refs`
- `updated_at`
- `version`
- `provenance`

A projection has at most one current `pipeline_state` for a given Objective. History is reconstructed from Logs, not by storing multiple historical states on the Objective.

Phase 1 `pipeline_state` enum:

- `unlabeled`
- `invalid`
- `under_specified`
- `valid_unprioritized`
- `unassigned`
- `in_process_awaiting_pr`
- `awaiting_review`
- `awaiting_ci`
- `complete`

Enum semantics:

- `unlabeled`: no accepted projection label/state has been assigned.
- `invalid`: the projected work item is malformed, duplicate-invalid, impossible, or not admissible as planned work.
- `under_specified`: more triage or clarification is required before prioritization.
- `valid_unprioritized`: valid work exists but has not been ranked or selected for execution.
- `unassigned`: valid prioritized work has no executor or active assignment.
- `in_process_awaiting_pr`: work is assigned or in progress and the next expected external projection is a PR or comparable artifact.
- `awaiting_review`: implementation or artifact exists and needs human, maintainer, or worker review.
- `awaiting_ci`: review or merge path is waiting on CI or comparable automated verification.
- `complete`: the projection workflow is complete; this does not by itself prove `Objective.status = completed` or `satisfied`.

Automation Workers do not directly mutate `pipeline_state`. A worker may submit a `pipeline_state` projection-state mutation request when its capability grant permits `mutation_request.submit` for the target projection and Objective. The canonical instance validates authority, expected prior version, Compartment/export policy, External References, idempotency, and review policy before applying or rejecting the request.

Every accepted `pipeline_state` transition creates a `pipeline_state_transitioned` Log entry with old state, new state, projection id, target Objective ref, actor or authority source, reason, effective time, provenance, source refs, external refs, and any requested projection update. Rejected, stale, or unauthorized worker requests are logged through worker mutation rejection events.

### 26.3 GitHub projection

GitHub projection is a deliberately low-dimensional projection of canonical UbU state. Phase 1 may read any authorized GitHub object needed for import or reconciliation, but it writes only the following managed surfaces:

- `ubu:`-prefixed labels or another explicitly configured UbU-owned label prefix;
- issue or PR body blocks delimited by UbU-managed HTML comments;
- UbU-authored comments that include a stable projection identifier and managed-marker text.

Milestones and assignees are preview-only by default in Phase 1. UbU may include them in a projection payload, but a live milestone or assignee write requires explicit human approval for that operation or a later accepted decision that narrows the automation rule.

PR statuses, CI checks, branch protection, repository settings, issue titles, and unmarked issue or PR body text are read/import-only in Phase 1. UbU does not set PR status checks or mutate arbitrary repository metadata as part of MVP projection.

Managed labels should encode only projection-owned facts, such as the projected `pipeline_state` once that projection state is accepted. UbU must not remove, rewrite, or reinterpret non-`ubu:` labels as canonical UbU state.

Managed body blocks and comments must carry enough metadata to reconcile them later: projection id, source UbU object refs or External Reference refs, generated-at time, projection schema version, and a short human-readable statement that the block or comment is managed by UbU.

A human-approved live write and a dry-run preview use the same projection payload shape. Public demos may use dry-run projection when live GitHub mutation is unsafe, but the preview must show the exact labels, blocks, comments, milestones, or assignee changes that would be requested after approval. Projection requests, dry-run previews, and applied projection records carry `authority_source` plus actor/provenance fields so their source path is visible during reconciliation.

GitHub edits do not directly override canonical UbU state. A human GitHub edit, including an edit to a UbU-managed label, block, or comment, is imported as a GitHub External Event and may create drift, a mutation candidate, a projection repair request, a Task candidate, or a recalculation trigger after validation. Canonical UbU state changes only through UbU's normal admission path and Logs.

Automation Workers may submit `projection.github.request_update` requests or reconciliation artifacts when granted authority, but they do not directly mutate canonical UbU state or bypass human approval rules for live GitHub writes.

#### 26.3.1 GitHub token custody

Phase 1 GitHub credentials are secret material held by the execution actor that performs a GitHub API call. The default MVP path is worker-held credentials: an assigned worker stores its GitHub token in its OS/device secret store, and the canonical UbU instance stores only credential refs, scope metadata, capability grants, projection requests, External References, results, and Logs.

The canonical instance may hold a local GitHub credential only when it is itself the writer for a human-approved local projection. It does not need to see worker-held tokens and must not store raw GitHub secrets in Objectives, Tasks, ContextBundles, Logs, run artifacts, projection payloads, or External References.

Phase 1 prefers a dedicated GitHub App installation, fine-grained bot token, or equivalent service identity for UbU-managed projection writes. A maintainer-owned token is acceptable for single-user dogfooding when explicitly configured and logged as that operator's credential. Individual contributor tokens are not shared with a parent instance or other workers; they may be used only by that contributor's own authorized instance or worker.

GitHub credentials must be repository-scoped where GitHub supports it. Task-level authority is enforced by UbU capability grants, assignment leases, projection payload allowlists, idempotency keys, and review policy rather than by assuming GitHub can mint a unique token for each Task. Short-lived or narrower per-Task tokens may be used when available, but MVP security does not depend on them.

Live GitHub writes are not treated as confirmed solely from the write response. The writer or canonical instance re-reads the managed GitHub surface, records the observed version or timestamp, and reconciles it against the expected projection. Until verification succeeds, the projection is not marked verified and remains eligible for reconciliation reporting.

The MVP security assumption is cooperative, operator-administered local and worker enclaves with least-privilege GitHub credentials, explicit capability grants, human approval for live writes, append-only audit, and revocation/rotation. Phase 1 does not protect a token from a malicious local administrator or compromised worker that legitimately holds that token.

### 26.4 GitHub reconciliation

Missed GitHub updates are expected in MVP. Reconciliation is therefore part of the normal GitHub dogfooding loop, not an exceptional recovery path.

The canonical UbU instance owns reconciliation admission. It may perform manual sync, polling, fixture replay, or webhook intake itself. Automation Workers may poll GitHub, receive webhooks, compute candidate reconciliation reports, or submit worker requests, but the canonical instance validates authority, Compartment/export policy, idempotency, and expected prior version before applying canonical changes or logging accepted events.

The reconciliation report compares at least:

- GitHub issues and PRs against External References that link them to UbU Objectives, Tasks, External Events, or Logs;
- UbU-managed labels against the expected projection of `pipeline_state` or other accepted projection metadata;
- UbU-managed body blocks and comments against projection records and source UbU object refs;
- GitHub comments, reviews, PR changes, CI runs, and milestone changes against imported External Events and append-only Log entries;
- expected projection payloads against live GitHub state, including missing, stale, duplicate, manually edited, or foreign-owned fields.

Each report item should include external object identity, matched UbU object refs, External Reference refs, observed GitHub version or timestamp, expected projection version when any, drift classification, severity, provenance, and recommended handling.

MVP drift classes are:

- `missing_external_reference`;
- `missing_external_event_log`;
- `stale_managed_projection`;
- `manual_edit_to_managed_surface`;
- `unexpected_ubu_label_or_block`;
- `foreign_edit_outside_ubu_surface`;
- `projection_request_failed`;
- `github_object_missing_or_inaccessible`.

Drift creates a reconciliation report first. It may recommend repair Tasks, mutation requests, projection update requests, External Reference verification, or recalculation. It must not silently create canonical repair Tasks or perform live GitHub writes in Phase 1; those require human approval or a later explicitly accepted policy.

Planner-relevant GitHub reconciliation findings create or batch `recalculation_triggered` Log entries with trigger kind `github_update`. Low-priority drift that does not affect the current Plan may mark projection or report caches stale instead of recalculating immediately.

### 26.5 GitHub event triage

GitHub event triage converts authorized GitHub observations into External Events, Logs, candidate Objectives, candidate Tasks, recalculation triggers, worker assignments, and projection requests. A GitHub observation is not canonical domain state merely because GitHub emitted it. The importer first normalizes and deduplicates the observation, then records accepted unique observations through the normal admission path.

Log-only cases in Phase 1 are narrow:

- duplicate webhook deliveries, fixture events, worker submissions, or reconciliation observations whose idempotency key already exists;
- reconciliation checks that verify an external object or managed projection is unchanged;
- events outside the configured repository, object, milestone, or fixture import scope;
- projection write acknowledgements that are already represented by an accepted projection Log entry and do not reveal new GitHub state.

Every other unique in-scope GitHub observation creates an External Event and an `external_event_observed` Log entry with source External References where available. This includes issue, PR, comment, label, review, CI, and milestone changes, even when the only immediate planning effect is to mark a report cache stale.

Phase 1 event-class defaults are:

- `issue_opened`: Creates an External Event, creates or links an issue-backed Objective when the issue represents durable work, and creates an issue-triage or clarification Task when the issue is under-specified.
- `issue_commented`: Creates an External Event; creates a response, clarification, review, or follow-up Task only when the comment asks for action, supplies blocker information, changes acceptance evidence, or affects contributor follow-up.
- `issue_labeled`: Creates an External Event; non-`ubu:` labels are evidence only, while UbU-managed labels are checked for projection drift and may create projection repair recommendations.
- `issue_closed`: Creates an External Event; may transition linked Tasks to completed or moot only after admission validates that the modeled effect is satisfied, otherwise it creates a reconciliation or clarification Task.
- `pr_opened`: Creates an External Event, links the PR to existing work when possible, and usually creates a review or PR-triage Task.
- `pr_updated`: Creates an External Event; creates a re-review, CI-wait, or projection-update Task only when the changed commits, body, title, base branch, or mergeability affect active work.
- `review_requested`: Creates an External Event and a review Task for the requested reviewer or eligible worker path.
- `review_submitted`: Creates an External Event; approvals may unblock work, while change requests or blocking comments create follow-up Tasks.
- `ci_failed`: Creates an External Event and a failure-analysis or fix Task, and may request Automation Worker assignment when an eligible worker has an explicit assignment path.
- `ci_passed`: Creates an External Event; usually logs or unblocks existing Tasks and may trigger projection update, but creates a new Task only when a next action such as merge review is required.
- `milestone_changed`: Creates an External Event; may create or update a release Objective, deadline fact, Calendar constraint, or release-planning Task.

GitHub Objective and Task creation is capped by admission rules:

- A GitHub event creates a new Objective only when it introduces durable desired state, release scope, accepted project work, or an investigation that cannot be represented as a Task under an existing Objective.
- An analysis Objective is a one-time Objective tagged `analysis`, linked to at least one parent Objective or source External Reference, and must include a crisp question, termination condition, owner or review path, source refs, and duplicate key.
- An event is appended to an existing Objective when the source object is already represented, shares the same durable desired state, or supplies evidence, blocker, status, acceptance, scope, deadline, or contributor-follow-up information for that Objective.
- A unique in-scope event is merely logged only in the sense that it creates the External Event and Log required by this section but no Objective or Task. This is the default for evidence-only labels, routine comments, status churn, CI transitions that confirm known state, and low-priority observations that only mark caches stale.
- A Task is created only for a concrete next action with an executor path or review owner and an actionable completion criterion: triage, clarification, review, fix, projection repair, reconciliation, release planning, merge readiness, contributor follow-up, or bounded analysis. If an active equivalent Task already exists, the event is appended as evidence instead.

Analysis Objective duplicate detection uses a normalized key over parent Objective refs, analysis kind or failure class, normalized source refs, affected artifact or GitHub object, unresolved question text, and active or terminal status. If a matching active Objective exists, the new event is appended. If a matching terminal Objective exists and the new evidence materially reopens the question, UbU creates a superseding analysis Objective linked by `supersedes`; otherwise it logs duplicate or idempotent evidence.

Analysis Objectives do not receive independent user value in Phase 1. They derive instrumental scheduling value from parent Objective refs and accepted Preferences, capped by the parent scope. An analysis Objective without a valid parent or durable source link is under-specified and should create a clarification or review Task rather than compete as high-value work.

Analysis Objectives close automatically when their termination condition is satisfied by accepted evidence. They transition to `completed` when the analysis question is answered or the required artifact is accepted; they transition to `abandoned`, `invalid`, or `superseded`, or cause related Tasks to become `moot` with accepted reason codes, when the source work disappears, duplicates another work item, becomes obsolete, or is replaced by a newer plan structure. All closures are logged.

Noise-control defaults:

- one GitHub delivery may create at most one new Objective and one immediate Task unless a human-approved decomposition or worker mutation request creates a Container and child Tasks;
- low-priority evidence is batched into reconciliation, Calendar preview, Log review, or risk-report refresh instead of creating prompt-facing Tasks;
- generated analysis work needs a review window, owner or worker assignment path, and stale-after policy;
- repeated equivalent events update External References, Logs, verification metadata, or cache staleness rather than creating more Objectives or Tasks.

A GitHub event triggers recalculation with trigger kind `github_update` when it can affect the current or next recommended Task, a dependency or precondition, Task lifecycle state, Objective status, deadline or milestone constraints, worker assignment need, projection state, or risk-report validity. Low-priority observations that do not affect the current Plan may mark Calendar, projection, explanation, or report caches stale.

Automation Worker assignment is never caused by raw GitHub data alone. The GitHub event must first create or update an admitted Task or Delegation Substrate packet, and the parent UbU instance must explicitly assign eligible worker work under the accepted worker-assignment model.

GitHub projection updates are triggered by accepted canonical UbU state changes or reconciliation drift, not by treating GitHub as source of truth. A GitHub event may recommend a projection update request or preview; live writes still follow the managed-surface and approval rules.

Duplicate detection uses the strongest available normalized key: GitHub delivery ID when present; otherwise repository identity, event class, action, external object type and ID, actor, source timestamp, object version such as commit SHA or CI run attempt, and the affected UbU target. External Reference uniqueness and Log `idempotency_key` checks prevent duplicate External Events, Tasks, Objectives, and projection requests.

Missed events are reconstructed during reconciliation by comparing live GitHub objects, timelines, comments, reviews, CI runs, milestones, and managed projection surfaces against External References, imported External Events, Logs, and projection records. Reconstructed events receive GitHub-derived effective timestamps, reconciliation provenance, confidence metadata when needed, and synthetic idempotency keys. If GitHub exposes only final state, UbU records the observed state transition or drift finding rather than fabricating a precise sequence of unseen events.

---

## 27. Risk Reporting

Risk is not a first-class object in MVP.

Risk reports are derived, recalculable analyses over Calendars, Plans, Tasks, Logs, Snapshots, External Events, worker status, External References, and compact Calendar metadata. They do not create canonical Risk objects, and Plan scoring does not include an explicit risk penalty in MVP.

The Phase 1 MVP report set is:

- `p90_completion_time`
- `critical_path`
- `deadline_miss_probability`
- `affect_constraint_violation_probability`
- `low_compact_calendar_coverage_warning`
- `dependency_fragility`
- `worker_or_automation_bottleneck`
- `stale_affect_warning`
- `destructive_pressure_warning`
- `post_plan_depletion_warning`

Additional findings may be computed when the inputs already exist, but they are not required for Phase 1: `relationship_maintenance_neglect_warning`, `preference_uncertainty`, `repeated_override_or_deviation_pattern`, `motivation_mismatch`, `stale_or_missing_discovery_mode_reconciliation`, and `calendar_preview_or_log_review_overdue`.

Risk reports are computed on demand from the current Calendar or Plan. Implementations may cache a risk-report artifact with the Calendar, Plan, run artifact, or release package for auditability and UI performance, but the cache is derived state and must be invalidated or marked stale when relevant Logs, Snapshots, Tasks, worker status, External Events, compact Calendar coverage, or recalculation triggers change.

Automation Workers may compute or refresh risk-report artifacts when granted appropriate read capability, but worker output is advisory until admitted by the canonical instance. Workers may submit report artifacts, report-refresh requests, mutation requests, or projection requests according to their grants; they do not create canonical Risk objects or directly mutate Tasks.

A risk report may recommend or request Tasks. It must not silently create canonical Tasks in MVP. Human-approved or policy-approved follow-up Tasks may include clarification, affect collection, dependency repair, worker retry/escalation, Calendar regeneration, Log review, Calendar preview, or GitHub projection/reconciliation work.

Worker retry state is an input to `worker_or_automation_bottleneck`: active retry chains, repeated failure classes, retry exhaustion, expired leases, and stale late submissions should be surfaced when they threaten current work or release readiness.

Risk reports are part of release ceremonies for UbU-runs-UbU. A release readiness or release outreach package should include, at minimum, deadline risk, critical path, dependency fragility, worker/automation bottleneck, and low coverage warnings when the inputs exist. Public release artifacts should cite only reviewable report summaries and must not expose sensitive Compartment payloads.

The PERT-superiority demonstration should not claim mathematical replacement of every PERT use. UbU supersedes PERT for its own MVP demo by showing that schedule risk is not only a duration network: it also includes explicit dependency/precondition state, affect constraints, worker status, compact Calendar coverage, recalculation from Logs and Snapshots, and actionable next Task or Plan repair recommendations.

Burnout / affect exhaustion is modeled as a constraint violation.

Affect-related risk reports should name plan fragility, not user blame. Phase 1 reports may include affect constraint violation probability, destructive-pressure warnings, stale-affect warnings, repeated late-failure warnings, and post-plan depletion warnings. These findings are derived from Plans, Logs, Snapshots, and risk-report analysis rather than first-class Risk objects.

---

## 28. Recalculation Triggers

Recalculation triggers are explicit records that tell UbU when a Calendar, Plan, risk-report cache, explanation cache, or next-action recommendation may no longer reflect current state. A trigger does not mutate domain state by itself; it points at the Log entry, Snapshot, External Event, worker request, or clock condition that changed the planner's inputs.

Phase 1 triggers are:

- `task_completed`
- `task_failed`
- `task_moot`
- `observed_snapshot`
- `affect_confidence_decay`
- `external_event`
- `github_update`
- `user_override`
- `calendar_preview_due`
- `log_review_due`
- `discovery_mode_reconciliation_due`
- `elapsed_time`
- `low_compact_calendar_coverage`
- `worker_request`

Phase 1 user controls such as snooze, reject, decompose, and override should be represented through ordinary Task, Log, or user-override payloads rather than separate trigger kinds unless a later implementation proves the distinction is necessary.

Phase 2 triggers are limited to sync and personal-worker conditions that can invalidate the local Calendar: `sync_state_changed`, `device_reconnected`, `remote_worker_status_changed`, and `offline_window_changed`. These are deferred with multi-device local-first sync and user-owned worker coordination.

Post-MVP trigger families include native cross-user message arrival, AssociationAttestation review, broad legacy-message ingestion, background AgentAction policy events, external compute budget or provider changes, and other high-level agentic or multi-user coordination events. They should be modeled through the same trigger envelope when implemented, but they do not block Phase 1.

Accepted Phase 1 triggers are logged with Log event type `recalculation_triggered`. The Log payload should include `trigger_kind`, `scope`, `target_refs`, `source_event_refs`, `requested_action`, `batch_key`, `reason`, and optional `idempotency_key`. `requested_action` is either `recalculate_now` or `mark_calendar_stale`.

Automation Workers do not create canonical triggers directly. A worker with `recalculation.request` may submit a request. The canonical instance validates the worker capability, target scope, Compartment/export policy, source evidence, and idempotency key, then records either an accepted `recalculation_triggered` entry or a rejected worker mutation/request Log entry.

Triggers may be batched when they share the same Calendar or Plan scope and no member requires a user-visible immediate repair before the batch window closes. Batching should preserve the individual source references and reasons in the logged payload. A short event-loop batch is acceptable for multiple imported GitHub events, worker updates, or low-priority stale markers.

Immediate recalculation is required when the trigger can change the current or next recommended Task, hard feasibility, Task lifecycle state, dependency or precondition truth, Objective status, affect legitimacy, worker assignment needed for current work, or compact Calendar coverage below the configured minimum. The default Phase 1 immediate triggers are `task_completed`, `task_failed`, `task_moot`, `user_override`, planner-relevant `observed_snapshot`, planner-relevant `external_event`, planner-relevant `github_update`, `low_compact_calendar_coverage`, and accepted urgent `worker_request`.

Stale marking is sufficient when the trigger only means cached Plans, Calendars, explanations, or reports should be refreshed before later reliance. The default stale-only triggers are `calendar_preview_due`, `log_review_due`, `discovery_mode_reconciliation_due`, `elapsed_time` outside the reactive repair envelope, `affect_confidence_decay` that has not yet crossed a configured planning threshold or current affect constraint, and low-priority external or GitHub observations that do not affect the current Plan.

`elapsed_time` should not create one Log entry per clock tick. It is materialized only when crossing a modeled boundary such as Task start/end, static commitment proximity, review due time, stale-affect threshold, reactive horizon expiry, offline precompute boundary, or compact Calendar expiration.

---

## 29. Current Major Open Questions

Unresolved questions are tracked in [`OPEN_QUESTIONS.md`](OPEN_QUESTIONS.md).

## 30. Design Process

The GitHub repository is the canonical public design process for UbU.

Chat discussions and private notes may generate proposals, but accepted decisions become official only when reflected in this repository.

Design changes should be recorded as:

- updates to `DESIGN.md`,
- accepted decisions in `DECISIONS.md`,
- unresolved issues in `OPEN_QUESTIONS.md`,
- or GitHub Issues linked from `OPEN_QUESTIONS.md`.

The project should prefer explicit decisions over hidden assumptions.


### Current strategic posture

UbU has moved from pure design preparation into active pre-MVP dogfooding.

The project should now prioritize:

- visible `model-committee` dogfooding output;
- recruitment of a small core cohort of serious, self-directed contributors;
- prototype-funder discovery among privacy-sensitive independent knowledge workers;
- conversion of outreach into concrete next actions;
- implementation of trunk features needed by the personal self-governance product.

Further broad philosophical elaboration should generally be subordinated to implementation, recruitment, dogfooding, or market discovery.

Public-facing materials should avoid implying that there is only one meaningful contributor role. They should welcome several serious contributors while still distinguishing committed work from passive interest.

Public materials should also avoid negative metaphors when criticizing planning systems that fail to model affect. Preferred terms include affect-blind, emotionally incomplete, human-incomplete, mechanistic planning, or affect-insensitive planning.

### Public recruitment language baseline

Public recruitment copy should say that UbU is looking for a small core cohort of serious, self-directed contributors who want to help build an inspectable, privacy-first planning kernel in public. It should make clear that several contributors are welcome, and that commitment means taking a bounded public path from concrete context to reviewed work, not competing for one privileged role.

Reusable public copy:

> UbU is looking for a small core cohort of serious, self-directed contributors. Good first contributions include workflow examples, design review, synthetic or redacted fixtures, parser or validation tests, model-committee artifact review, and narrow implementation-ready issues. If the work keeps the planning kernel inspectable, privacy-first, and useful for dogfooding, there is room for multiple contributors to earn bounded ownership over subsystems.

Public materials may name the staged roles already used by onboarding: workflow informant, design reviewer, fixture/test contributor, implementation contributor, and bounded module owner. Prototype-funder or design-partner leads may also be routed through outreach, but should not be presented as a privileged contributor rank.

Avoid language such as `the co-builder`, `highest-priority contact`, `one founding slot`, `competing for a role`, `exclusive inner circle`, `prove you belong`, `hand-picked elite`, or any phrasing that treats passive interest as worthless. Casual interest should be welcomed when it can become a workflow example, design feedback, contributor lead, prototype-funder lead, or public issue.

ETHConf and similar follow-up should route each person into one concrete next action: send a workflow example, review a model-committee artifact, comment on a public design question, contribute a fixture or test, take a narrow implementation-ready issue, introduce a serious contributor, or discuss a trunk-compatible prototype sponsorship.


### Release Outreach Pipeline baseline

Public and project-facing release communication should be treated as a first-class planning output. The standard tagline is:

> UbU should make every release explain itself.

For UbU itself, a minor release should normally produce a release outreach package when there is enough change to explain. The package should be grounded in current repository state, release notes, accepted decisions, closed issues, automated UI screenshots, scripted demo recordings, test fixtures, known limitations, and future-plan labels.

The public-audience script should explain what changed and why it matters without assuming deep computer knowledge. The developer-facing segment should briefly show dogfooding, current implementation artifacts, and concrete ways serious contributors can help. This lets a single release package serve users, nontechnical supporters, FOSS developers, project maintainers, design partners, and prototype funders.

Release outreach automation should draft and assemble artifacts before it publishes them. Human or policy approval is required before public upload, announcement, or mutation of external channels unless a project explicitly configures a narrow trusted auto-publication rule. Export must respect Compartment, Identity, and public-projection boundaries.

### First technical essay baseline

The first public technical essay should use `The Planning Kernel: What Task Managers Leave Out` as the working title. It should lead with a precise problem claim: list, calendar, board, and opaque assistant tools are useful projections, but they are not enough for real planning unless objectives, state transitions, dependencies, constraints, logs, uncertainty, affect, and recalculation are explicit.

The essay should avoid accusatory title language such as `your task manager is lying to you`, negative metaphors for affect-blind systems, generic startup brochure copy, grandiose inevitability claims, privacy overclaims, scarcity contributor framing, and claims that LLMs are the planner. It should explain LLMs as advisory tools that help interpret, summarize, propose, critique, and generate fixtures, while canonical planning remains inspectable and recalculable. It should point to `model-committee` as visible dogfooding and request one concrete planning-kernel workflow example that can become a public issue, fixture, interview, or prototype-funder conversation.

### Long-arc public narrative baseline

UbU may publicly say it is a long-running personal and technical project whose architecture predates the current LLM moment, and that recent LLM capabilities make the plan more practical by improving interpretation, summarization, proposal generation, fixture creation, and advisory automation.

Public copy should share only enough personal history to explain commitment, design maturity, and why the project exists. It should avoid memoir, destiny claims, and claims that UbU was inevitable. The preferred framing is: `I have worked on this problem for a long time; the toolchain is now good enough to build and test a narrow, inspectable version in public.`

Durable significance should be stated as a possibility to test through dogfooding, contributors, and prototype-funder discovery. Preferred language includes `could matter for a long time`, `worth building carefully`, and `a serious attempt at privacy-first self-governance software`. Avoid `will change the world`, `inevitable`, `once-in-a-generation`, `solves coordination`, `the future of work`, `the only real solution`, `world-historical`, and similar totalizing claims.

The narrative belongs mainly in the first technical essay, selected outreach, talks, and interviews. README-level use should be short and point back to current dogfooding and contribution paths, not long personal history.

### Contributor onboarding baseline

Committed-contributor onboarding should be public, bounded, and fixture-first. The minimum path is: read the project overview/core principles, model-committee dogfooding, Phase 1 demo, GitHub dogfooding, open-core, Compartment, and design-process sections; run the no-private-access local smoke command for the relevant repo; make a small public contribution; then earn broader ownership through reviewed work.

For `model-committee`, the first command should be `uv run model-committee doctor`, followed by a fake-provider or fixture-backed run/test when available. The first task should touch a synthetic or redacted fixture, parser or validation test, model-committee artifact review, public workflow example, or narrow implementation-ready issue with explicit acceptance criteria.

Contributors should understand the core boundaries before implementation: canonical state lives only in committed canonical files; model-committee is advisory; v0.1 provider/network and no-auto-apply boundaries are hard; GitHub is a projection; LLMs are advisory; Compartment and low-security rules constrain data routing; and the open-core planning kernel must remain inspectable and self-hostable.

Contributor progression runs from workflow informant to design reviewer to fixture/test contributor to implementation contributor to bounded module owner. Work that requires private project-owner context is not onboarding-ready; it should be converted into a public issue, fixture, design note, or open question with sensitive details redacted or synthesized.

### Commercial funding red lines

Funding is acceptable only when it accelerates trunk capabilities needed by the personal self-governance product and preserves the user's authority over objectives, logs, plans, affect data, compartments, and external projections.

Acceptable prototype funding may pay for planning-kernel, local-first, privacy, Compartment, affect-aware planning, GitHub/calendar/task ingestion, Log, risk-report, worker, or projection features that generalize to the open core.

Incompatible funding includes requests for:

- surveillance-style project management, activity scoring, keystroke or screen monitoring, or always-on productivity telemetry;
- manager-first dashboards that rank, pressure, or compare contributors instead of helping a person or project operator govern commitments explicitly;
- centralized productivity telemetry across users, teams, clients, or contributors without explicit user-controlled sharing;
- generic enterprise dashboards that make UbU a conventional reporting layer rather than a self-governance planner;
- features that expose affect, private relationship data, Compartment payloads, or low-security personal context to managers, funders, or customers as a condition of use;
- token-first, speculation-first, governance-token, DAO-dashboard, or financialized coordination work that would reposition UbU as a crypto product;
- custom branches whose value depends on one funder's private workflow and would not become trunk capability.

Funding terms are incompatible when they require:

- funder control over the product roadmap, canonical design process, licensing boundary, or accepted decisions;
- exclusive ownership, assignment, or proprietary relicensing of open-core planning-kernel work;
- a closed-source replacement for functionality that public contributors need for ordinary dogfooding;
- confidentiality terms that prevent honest public explanation of architectural constraints, privacy limits, or contributor-facing behavior;
- urgency, deliverables, or support obligations that materially displace Phase 1 implementation, recruitment, or public dogfooding.

Crypto, tokenization, or DAO-specific requests should be rejected when they are token-first or speculation-first, and deferred when they are merely integration-specific rather than needed for the personal self-governance trunk.

Paid work should fund trunk features by default. When a funded prototype needs customer-specific configuration, that configuration should remain outside the core boundary unless it exposes a reusable open-core abstraction.

### Prototype-funder workflow baseline

The first fundable independent-consultant prototype is a local commitment-risk and weekly-planning workflow, not a general inbox assistant or enterprise dashboard.

The prototype should answer one buyer question: whether the consultant is about to miss, overrun, or quietly under-service a client commitment this week, and what to do next without exposing private client content.

Minimum inputs are manual client/project declarations, manually entered or imported deadlines, task declarations, current availability, optional calendar busy/free blocks, optional GitHub issue/PR references, and an affect Snapshot. Email, notes, invoices, and full message bodies are excluded from the first prototype unless represented by user-approved structural references.

The primary output is a compartment-aware weekly commitment-risk review plus a next-day default Plan. It should show client/project compartments, deadlines, dependency and overcommitment risk, blocked or stale commitments, affect/energy constraints, and the exact structural data used. It should not expose one client's payload in another client's view.

The prototype remains on the trunk when every funded feature maps to reusable open-core objects: Objectives, Tasks, Logs, Compartments, Calendar/Plan generation, risk reports, optional GitHub/calendar ingestion, and user-visible import/projection boundaries. Customer-specific templates, connector credentials, private examples, and support may remain outside the core.

Prototype-funder discovery should test paid design-partner structures before custom consulting: a small prepaid discovery/review session, a larger prepaid prototype sponsorship, or a design-partner retainer whose deliverable is trunk functionality plus customer-specific configuration. Exact pricing is market-discovery data rather than canonical product policy, but offers that cannot fund reusable trunk work should be rejected or renegotiated.

### Open-core and FOSS boundary

UbU treats the planning kernel and contributor-facing integration surface as the open core. A public contributor must be able to inspect, run, modify, and self-host the core system needed for ordinary single-user and project dogfooding without depending on private replacement components.

The definitely open-source surface includes:

- canonical data model schemas and migrations for Objectives, Preferences, WorkItems, Tasks, Containers, UniverseState, Snapshots, Plans, Calendars, Logs, Identities, Relationships, Zones, Compartments, External Events, External References, Association-related schemas when implemented, worker assignments, mutation requests, and projection state;
- explicit planner and Calendar-generation logic required for Phase 1 dogfooding, including constraint evaluation, recalculation triggers, compact Calendar serialization needed for transport or analysis, and MVP risk-report generation;
- GitHub import, triage, projection, reconciliation, fixture/demo tooling, and the managed-label/comment/block formats used for FOSS collaboration;
- worker-mode runtime surfaces required for delegated work, including Identity/capability checks, assignment, status, mutation request, and audit/log contribution APIs;
- local-first storage and sync protocols when implemented;
- the Super Automation extension/API boundary needed for third-party workers, connectors, or local services.

Private or commercial code may exist only outside that core boundary. Acceptable private areas include hosted-service operations, managed cloud infrastructure, paid support/packaging, premium hosted worker capacity, enterprise administration/compliance layers, proprietary connectors to closed third-party systems, and short-lived experimental prototypes that are not required for the public dogfooding loop.

Private experiments must not become hidden mandatory dependencies for public contributors. If an experimental component becomes necessary for the advertised open-source workflow, UbU must either open it before relying on it publicly or explicitly narrow the public promise.

Implementation repositories should use OSI-approved licenses. The default license for core implementation repos is MPL-2.0 so modifications to core files remain shareable while integrations can be built without relicensing unrelated code. Stronger copyleft may be considered for network-hosted service code, and permissive licensing may be used for small examples, SDK stubs, or interoperability fixtures when that better serves adoption.

Contributor trust requires clear labeling of each repository or package as open core, private experiment, premium hosted service, or external connector. UbU should not recruit FOSS contributors around a capability that depends on an undisclosed private substitute for the open implementation.

### Canonical and derived project documents

For UbU design governance:

- `DESIGN.md`, `DECISIONS.md`, and `OPEN_QUESTIONS.md` are canonical design inputs.
- `README.md`, `OUTREACH.md`, `PM_BRIEF.md`, `FUNDER_BRIEF.md`, `SOVEREIGN_COORDINATION.md`, and `ORG_INTROSPECTION_BRIEF.md` are derived public-facing projections.

Question-answering uses canonical files. Consistency checking includes derived files and patches them when they fall out of sync.

### Directive decisions

Direct project-owner directives may be appended to `DECISIONS.md` as accepted decisions.

Directive decisions are canonical once committed.

They may override provisional decisions, create new questions, close questions, split questions, or require consistency repairs.

The model-committee process must treat directive decisions as authoritative input during the next system-wide consistency check.
