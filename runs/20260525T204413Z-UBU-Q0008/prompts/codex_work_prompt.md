# Model-Committee Work Proposal Request

You are participating in the UbU `model-committee` process.

Your task is to produce one concrete work proposal as strict JSON.

Do not return prose outside the JSON object.

## Selected question

Question ID: `UBU-Q0008`  
Question title: `Worker Assignment Model`  
Base commit: `620d9d0fb9994916928e61b2dcd8a58b2918406e`

```markdown
## UBU-Q0008: Worker Assignment Model

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0007 Blocks: Phase 1 implementation Resolved by: Unresolved Last scored: Never Scored from commit: None

Workers need a way to discover or receive work.

### Question

1. Does an organization-mode instance explicitly assign Tasks to workers?
2. Or do workers poll for Tasks matching their capabilities?
3. Can multiple workers observe the same Task?
4. Can multiple workers compete for the same Task?
5. What happens if a worker disappears mid-Task?
6. Is assignment itself logged?
7. Can workers reject assignments?
8. Can workers request clarification Tasks?
9. Can workers spawn child Tasks?

### Current MVP leaning

Explicit assignment is likely simplest.

### Resolution

Unresolved.

---

```

## Canonical design files

### DESIGN.md

```markdown
# UbU Design

Status: Draft / active pre-MVP dogfooding  
Repository: `ubu-design`  
Purpose: Canonical public design document for the UbU project

---

## 1. Overview

**UbU** is a privacy-first planning, coordination, and self-governance system.

UbU takes messy real-world inputs—tasks, calendar events, messages, external events, user preferences, physical/emotional state, integration data, realtime interaction streams, and agent or worker outputs—and turns them into explicit, inspectable, recalculable Plans.

UbU is not merely a task list or calendar application. It is intended to become a personal and organizational planning kernel: a system that models desired outcomes, constraints, risks, dependencies, available time, and human limitations, then helps determine what should happen next.

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

Release management should not stop at code, tests, changelogs, and deployment artifacts. For UbU-runs-UbU, every meaningful minor release should also create reviewable explanation artifacts for the audiences affected by the release. These artifacts may include user-facing release notes, developer-facing release notes, screenshots, scripted UI-demo captures, short video scripts, narration text, captions, YouTube descriptions, thumbnail concepts, public posts, known-limitations summaries, and contributor calls-to-action.

The pipeline is a communication Objective implemented through ordinary UbU primitives: Objectives, Techniques, WorkItems, Logs, Automation Worker outputs, release artifacts, and export/projection gates. A later implementation may add specialized schemas for release artifacts or communication objectives, but the design requirement does not depend on a new first-class MVP entity.

Release outreach must be evidence-bound. Generated claims should be traceable to implemented features, accepted design decisions, closed issues, release notes, automated UI screenshots, demo recordings, or clearly labeled future plans. The pipeline should not let AI-generated video scripts drift into unsupported hype.

Publication is gated by default. UbU may draft, assemble, render, and prepare release communication artifacts automatically, but uploading to YouTube, publishing posts, sending announcements, or mutating external public channels should require explicit human approval unless a project has configured a narrow trusted auto-publication rule.

The Release Outreach Pipeline is not UbU-specific marketing glue. It is intended to generalize to project-management configurations: open-source projects, research groups, internal teams, product teams, community projects, and personal projects may all define communication Objectives that explain progress to their relevant audiences.

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

Hard boundaries should be limited to product invariants UbU can actually enforce: Compartment boundaries, privacy policy, data-access authorization, EvidenceUsePolicy restrictions, export prohibitions, identity/Compartment isolation, provenance integrity, audit-record integrity, integration authorization, and unavoidable external constraints. Behavioral-risk checks such as manipulation, coercion, harassment, stalking, deception, power-asymmetry risk, rumination risk, or relationship-transition ethics are fallible classifier judgments and should generally be advisory, transparent about uncertainty, user-overrideable, and introspection-relevant when bypassed.

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

Model-committee automation must be bounded by a stop rule. It should recommend further design work only when that work has greater expected value than beginning or continuing implementation.

The first implementation of this process is intentionally constrained by the v0.1 restrictions recorded in `DECISIONS.md`. The accepted v0.2 direction expands the bootstrap loop with Claude Code CLI, frontier cross-scoring, disagreement flags, schema-native structured output, and operator-run artifact publication while preserving the advisory authority boundary.

### 3.0 Current dogfooding status

`model-committee v0.1` is the first runnable bootstrap artifact for UbU’s dogfooding process.

It is not the full UbU planner, but it exercises the recursive project-governance loop: parse canonical state, check consistency, select answerable work, generate candidate changesets, score them, validate patches, and produce reviewable artifacts.

This establishes active pre-MVP dogfooding while preserving the rule that accepted design state exists only when committed to the canonical design repository.

The current strategic emphasis is to use visible dogfooding, contributor recruitment, and prototype-funder discovery to accelerate the trunk of UbU rather than to expand design philosophy indefinitely.

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

### 3.3 Prioritized recursive loop

Model-committee automation is expected to evolve into a prioritized recursive loop:

1. **System-wide consistency check** verifies that the current project state is coherent.
2. **Question/problem prioritization selection** chooses the next work item only after consistency is known.
3. **Work** produces and scores concrete changesets.

Consistency has the highest priority because an inconsistent project state invalidates future planning.

Prioritization is second because it selects the next intended state transition.

Work is third because it should implement only a selected and justified transition.

This structure is intended to make `model-committee` an early dogfooding example of UbU coordinating its own development.

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

A decomposition is bad when it creates more, harder, vaguer, or more coupled questions.

A decomposition is good when it exposes smaller answerable units and improves future automation.

### 3.7 Answerability-first prioritization

Question selection should use answerability as the first gate.

A question is eligible for ordinary work only if:

- it has no dependencies;
- all dependencies are solved;
- or all dependencies are being answered in the same work item.

After answerability is established, questions are ranked by:

1. automation-likelihood,
2. importance,
3. risk ascending.

Questions blocked by unresolved dependencies may be selected for decomposition rather than ordinary answering.

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

Phase 3 explicitly includes:

- minimal cross-user contextual messaging between UbU instances;
- user-to-user Identity-mediated requests, status updates, questions, commitments, and blockers;
- bounded Message Context Envelopes carrying priority, interrupt recommendation, topic, response expectation, assumptions, ambiguities, provenance, and disclosure policy;
- legacy-message ingestion and limited upgrade paths where both parties use UbU;
- user-to-user Identity commitments, capabilities, and limited disclosure sufficient for narrow coordination;
- early Delegation Substrate use between Identities, if Phase 2/3 authority and privacy boundaries are in place.

Phase 3 still defers:

- shared global project truth between users;
- rich subjective Relationship-model exchange;
- full Association synchronization;
- multi-party governance, invitation, revocation, or trust protocols;
- broad personal-data ingestion beyond selected communication integrations.

Phase 1 keeps these abstractions documented for compatibility but does not implement them:

- Technique as a first-class planning object;
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
   - Treat Calendar preview as a notable Task that should run regularly.
   - Let the user inspect the candidate Calendar or default Plan before relying on it.
   - Ask whether the Plan feels plausible, motivating, humane, and consistent with the user's current context.
   - Allow the user to correct Preferences, Objectives, Tasks, affect Snapshots, availability, or estimates before execution.

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
   - Record outcomes as Logs and trigger recalculation when the outcome changes Task status, availability, affect state, dependencies, Objective status, or user Preferences.
   - Treat failure, rejection, override, and deviation as model evidence, not user blame.

7. **Discovery mode when appropriate**
   - Allow the user to choose discovery mode at any time.
   - Use discovery mode to collect or preserve evidence about what the user is doing or did, especially from mobile sensors or configured integrations.
   - Defer ambiguous interpretation until later Log review or user-visible reconciliation.

The Phase 1 version may be simple and fixture-backed. Fixture behavior must be labeled as fixture behavior, mock recommendations must not be described as implemented planning, and public demos should show the exact boundary between explicit UbU objects, approved fixtures, and implemented planner behavior. The public demonstration pattern is bootstrap, one recommended next Task, explanation, user feedback, recalculation, and full-Plan inspection. It does not require broad email, text-message, file, invoice, note, or personal-data ingestion. It must not claim complete life-modeling, therapeutic authority, autonomous life coaching, complete planning automation, full privacy isolation, Phase 2 sync, or Phase 3 multi-user coordination. The purpose is to demonstrate the core UbU experience: one meaningful next action, with an explanation, grounded in explicit state.

### 4.1.3 Release Outreach Pipeline dogfooding

The Release Outreach Pipeline should become part of ordinary UbU-runs-UbU release management. A minor release should produce a release outreach package when the current project state contains enough user-visible, developer-visible, or contributor-visible change to justify public explanation.

A release outreach package may include:

- a public release-note summary;
- a developer release-note summary;
- a short user-facing video script;
- a developer-facing video segment or call-to-action;
- screenshot and screen-recording references from automated UI runs, fixtures, or explicitly approved mock data;
- captions or narration text;
- YouTube title, description, and chapter outline;
- social or mailing-list announcement drafts;
- known limitations and future-work notes;
- a contributor next-action list tied to real issues or artifacts.

The package should record provenance. Each claim should identify whether it came from an implemented feature, accepted design decision, closed issue, test fixture, UI capture, release note, or future-plan label. Public artifacts should not represent planned or mock behavior as implemented behavior.

The Phase 1 minimum may be manual but structured: the project operator can write or approve the release notes, screenshots, and script while UbU records the artifact set as a WorkItem sequence. Later phases should automate screenshot capture from UI tests, demo-flow export, script drafting, voice/narration preparation, caption generation, video-render-plan generation, publication metadata, and review gates.

The full video-generation pipeline is future work. Phase 1 should preserve the model boundary: generate reviewable communication artifacts before trying to automate external publication.


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

Candidate fields for a calibration example include:

- `example_id`
- `situation_summary`
- `common_emotional_costs`
- `common_emotional_rewards`
- `possible_tradeoffs`
- `calibration_question`
- `resulting_object_refs`
- `source`

Open details are tracked in `UBU-Q0051`.

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

GitHub-linked Task decomposition should keep the GitHub Issue or PR traceable to the Container and add child-level External References only for actionable subwork that needs projection or reconciliation. Automation Worker child Tasks are ordinary child Tasks with worker/delegation metadata; workers may propose this restructuring only through authorized mutation requests, and the canonical instance validates and logs the applied or rejected mutation.

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

**Discovery mode** is a user-selectable workflow state, not a fourth instance operating mode. A user may choose discovery mode at any time to let UbU gather or preserve evidence about actual behavior for later reconciliation.

Discovery mode is especially important for a mobile app because embedded sensors, location categories, motion state, app activity, calendar context, and explicit quick notes may help reconstruct what happened during undetailed or under-specified time periods.

Discovery mode must preserve user sovereignty. Sensor-derived, integration-derived, or inferred observations are evidence, not final truth. Ambiguous periods should be reconciled through later Log review, user clarification, or correction before UbU treats them as stable habit patterns, Preference changes, or Objective evidence.

Open details are tracked in `UBU-Q0052`.

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

Gap time is discretionary.

For MVP, gap suggestions are UI suggestions outside the Plan. Future versions may instantiate Gap Tasks.

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

Coverage belongs to the compact serialization, not the abstract Calendar.

Coverage represents the probability mass of possible futures covered by the compact representation.

The provisional short-horizon branch coverage target is `0.99` probability mass. This should be treated as a configurable heuristic, not as a mathematical guarantee for every device, scope, or resource mode.

After time advances, coverage is recalculated by conditioning the compact Calendar on elapsed time and accepted Logs, Snapshots, External Events, and user actions, then discarding expired branch mass and re-estimating the remaining covered mass.

If recalculated coverage falls below the configured threshold, UbU records a `low_compact_calendar_coverage` recalculation trigger or marks the Calendar stale according to trigger policy. Exact coverage proof is not an MVP requirement.

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

The reactive layer should cover near-term alternatives involving planned Tasks, duration variation, early completion, late completion, interruptions, relevant external events, affect shifts, user overrides, and immediate recalculation triggers. The provisional default horizon is one hour. Mobile-only, low-power, or offline operation may shorten this horizon.

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

Static Tasks retain fixed start times. If no predetermined Dynamic Task can validly fill time before the next Static Task, the planner should consider gap-filling suggestions. The design of evergreen Dynamic Tasks or evergreen Objective-maintenance Tasks is still open and tracked separately.

### 16.7 Adaptive granularity and offline operation

Compact Calendar planning should support configurable time deltas.

Provisional defaults:

- full-detail planning delta: `1 minute`;
- mobile moderate delta: `5 minutes`;
- mobile low-power or offline delta: `15 minutes`;
- reactive branch horizon: `1 hour`;
- short-horizon branch coverage target: `0.99` probability mass.

A one-minute delta aligns with common calendar behavior and should be available when compute resources permit it. Mobile-only operation may use coarser granularity to preserve battery, reduce memory and CPU/GPU load, and keep local recalculation responsive. Offline or low-power mode may dynamically switch to a coarser delta, such as fifteen minutes, while preserving the core UbU experience.

Known offline windows, such as flights or planned disconnection, should trigger preparatory precomputation while connectivity and compute are available. If the offline state is unexpected, UbU should gracefully degrade to cached state, local explicit algorithms, coarser deltas, reduced branch depth, and mobile stewardship metadata.

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

The minimum Phase 1 Compact Calendar implementation stores the legitimized skeleton baseline, the default Plan, the active execution profile, coverage estimate, decision envelopes, Task criticality, cached explanation fragments, probability provenance, and last-legitimate-Plan reference. It stores user-previewed, risk-report, and debug/reproducibility Plans only when those artifacts are actually generated or needed for audit.

High-probability near-term alternatives within the reactive horizon may be stored as materialized branches or as deterministic reconstruction instructions. Non-selected candidate Plans, expensive global repairs, and low-probability branches are reconstructed on demand from the compact grammar, current UniverseState, Logs, Snapshots, External Events, repair recipes, and cached provenance.

MVP planning does not require exhaustive optimal search, cloud compute, GPU execution, exact coverage proof, learned policy models, or a complete transport grammar for every post-MVP uncertainty source. The required implementation is a deterministic skeletonizer, minimum legitimizer, greedy baseline, bounded candidate expansion, hard-constraint validator, Plan probability/provenance record, compact default-Plan package, short-horizon repair cache or reconstruction path, and low-coverage recalculation trigger.

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
- `plan_realized`
- `decision_recorded`
- `recalculation_triggered`
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

Automation Workers contribute to Logs through their worker Identity. A worker may submit events or mutation requests, but the canonical instance validates authority and writes the canonical Log entry. Accepted worker mutations create applied entries; invalid or unauthorized attempts create rejected entries.

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

Log review is a notable Task that should run regularly. Its purpose is to keep UbU's model aligned with the user's actual behavior, later reflection, and corrections.

A Log review Task may:

- reconcile undetailed or under-specified time periods;
- ask whether a user override reflected a better local judgment, bad timing, missing preconditions, wrong duration estimate, stale affect data, social pressure, or changed Preference;
- compare planned Tasks against completed, failed, moot, snoozed, rejected, or unobserved Tasks;
- convert user-approved observations into corrected Logs, Snapshots, Preferences, Objective annotations, Task estimate updates, or recalculation triggers;
- mark proposed habit patterns as user-endorsed, tolerated, unwanted, or unresolved.

Log review should not become a blame ritual. It is a model-maintenance Task for self-governance. Open details are tracked in `UBU-Q0053`.

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

A `ContextBundle` is a governed package of context sent to a model, tool, worker, or agent.

Provisional fields:

- `context_bundle_id`;
- `purpose`;
- `source_refs`;
- `compartment_refs`;
- `identity_refs_exposed`;
- `association_refs_exposed`;
- `destination_provider_ref`;
- `model_or_tool_ref`;
- `redaction_policy`;
- `minimization_notes`;
- `retention_policy`;
- `user_visible_summary`;
- `created_at`;
- `expires_at`;
- `supports_candidate_refs`.

Context assembly is not a low-level implementation detail. It is a privacy decision that must respect Compartment and Identity boundaries.

### 21.4 MCP-style client and server boundary

UbU as an MCP-style client may call external tools and services. UbU as an MCP-style server may expose narrow tool surfaces to external agents.

Safe tool surfaces should prefer candidate submission and queries over direct mutation. Examples:

- `task_candidate.create`;
- `log_candidate.submit`;
- `plan_summary.read`;
- `plan_repair.submit`;
- `objective_status.query`;
- `clarification.request`;
- `association_attestation_candidate.submit`;
- `delegation_packet.prepare`;
- `projection_preview.submit`.

Capability grants should scope these operations by object, Objective subtree, Task set, Compartment, Identity, integration, operation kind, time window, cost budget, and review requirement.

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

The Skill Barter marketplace is a future specialization of Association coordination and Delegation Substrate, not a Phase 1 public marketplace.

It may eventually support privacy-preserving skilled-work exchange, pseudonymous capability claims, reputation without unnecessary doxxing, scoped work agreements, private or selective-disclosure evidence, lawful user-controlled settlement references, and human/agentic executor discovery.

For EthConf and cypherpunk outreach, Skill Barter is useful as a future-direction signal: UbU can preserve cryptocurrency-native values of voluntary coordination, sovereign identity, privacy, FOSS development, and open markets without becoming token-first or speculation-first.

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

Hard boundaries are narrower. They cover structurally enforceable product invariants and explicit policies: Compartment boundaries, privacy policy, data-access authorization, EvidenceUsePolicy restrictions, export prohibitions, identity/Compartment isolation, provenance integrity, audit-record integrity, integration authorization, and unavoidable provider/platform/legal constraints.

Behavioral-risk checks such as manipulation, coercion, harassment, stalking, deception, power-asymmetry risk, rumination risk, or relationship-transition ethics are fallible classifier judgments. Treating them as hard gates would risk false positives that override legitimate user autonomy, false negatives that create misplaced trust, and a false impression that UbU can reliably prevent misuse or illegal behavior. They should generally be default-on, uncertainty-aware, user-overrideable advisory checks with introspection consequences when bypassed.

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

Organization-created Objectives, Preferences, and Tasks require `authority_source` metadata. For MVP, `authority_source` may be coarse and may identify an admin-equivalent operator Identity, imported project policy, imported external event, Automation Worker submission, or human-approved projection/import action. The detailed authority-source vocabulary remains tracked by `UBU-Q0013`.

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

`pipeline_state` is distinct from `Objective.status`.

Candidate pipeline states:

- `unlabeled`
- `invalid`
- `under_specified`
- `valid_unprioritized`
- `unassigned`
- `in_process_awaiting_pr`
- `awaiting_review`
- `awaiting_ci`
- `complete`

### 26.3 GitHub projection

UbU may project data to GitHub through:

- labels
- issue body blocks
- comments
- milestones
- assignees

Recommended MVP rule:

> UbU should only write clearly marked UbU-managed labels, comments, or blocks, and treat all other GitHub edits as external events.

### 26.4 GitHub reconciliation

Missed GitHub updates are expected in MVP.

A reconciliation report should compare GitHub state against UbU state.

Possible comparisons:

- GitHub issues vs UbU Objectives
- GitHub labels vs `pipeline_state`
- GitHub comments vs event log
- PRs / CI vs external events

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

```

### DECISIONS.md

```markdown
# UbU Decisions

Status: Draft  
Purpose: Lightweight architectural and data-model decision log for the UbU project.

This file records accepted design decisions so they do not need to be rediscovered from chat history. It is not a full specification. Details belong in `DESIGN.md`; unresolved matters belong in `OPEN_QUESTIONS.md`.

---


## UBU-D0001: GitHub repository is canonical for public design

**Status:** Accepted → DESIGN.md §30

The public GitHub repository is the canonical public design process for UbU.

Private chats, notes, and external documents may generate proposals, but accepted decisions become official only when reflected in the repository.

---

## UBU-D0002: Start public design documentation with four Markdown files

**Status:** Accepted → DESIGN.md §30

The initial public design repo should remain simple and LLM-friendly.

Initial files:

- `README.md`
- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

---

## UBU-D0003: UbU is a privacy-first planning and coordination system

**Status:** Accepted → DESIGN.md §1

UbU is not merely a task list, calendar, or project-management dashboard.

UbU is a system for converting messy real-world inputs into explicit, inspectable, recalculable plans.

Inputs may include:

- Tasks
- calendar events
- messages
- external events
- user preferences
- physical state
- affective state

---

## UBU-D0004: User sovereignty is foundational

**Status:** Accepted → DESIGN.md §2.1

The user is the final authority over what the user wants and what occurred.

UbU may recommend, infer, estimate, warn, and automate, but it must not override user sovereignty.

---

## UBU-D0005: Distinguish logistical consistency from philosophical consistency

**Status:** Accepted → DESIGN.md §2.1

UbU distinguishes:

- **Logistical consistency:** hard consistency required by the data model and planner.
- **Philosophical consistency:** broader consistency between stated user goals and actual behavior.

Logistical contradictions must be rejected, repaired, or resolved. Philosophical inconsistencies may be logged and reported later.

**Example:**

A cyclic active preference relation is a logistical contradiction. Ignoring a message despite an Objective to communicate more with someone is a philosophical inconsistency.

---

## UBU-D0006: LLMs are bounded assistants, not the canonical decision engine

**Status:** Accepted → DESIGN.md §2.7

LLMs may assist UbU, but they are not the canonical real-time planner or decision engine.

LLMs may serve as:

- planning oracles,
- advisory systems,
- UI/screenshot interpreters,
- external workflow helpers,
- document interpreters,
- value-reflection assistants.

---

## UBU-D0007: Value is attached to Objectives

**Status:** Accepted → DESIGN.md §7

From a data-model perspective, value is anchored to Objectives.

Tasks may derive value from the Objectives they serve, but Tasks are not the canonical source of value.

---

## UBU-D0008: Preference is a first-class relation object

**Status:** Accepted → DESIGN.md §8.1

Objective value is derived from Preference objects.

MVP Preference fields:

- `objective_a`
- `objective_b`
- `order`
- `acquired_method`
- `acquired_date`
- `enabled`

`order` may represent:

- Objective A preferred to Objective B

---

## UBU-D0009: Ordinal rankings compile into pairwise Preferences

**Status:** Accepted → DESIGN.md §8.2

If the user provides an ordinal ranking, UbU compiles it immediately into pairwise Preference objects.

The original ordinal input may be retained in the log, but the canonical value model uses pairwise Preferences.

---

## UBU-D0010: Preference cycles are logistical consistency errors

**Status:** Accepted → DESIGN.md §8.4

Active cyclic Preferences are not allowed.

Example:

- A preferred to B
- B preferred to C
- C preferred to A

This is a logistical contradiction.

---

## UBU-D0011: Derived utils are transient computational artifacts

**Status:** Accepted → DESIGN.md §8.5

Numeric utility values may be derived from Preferences, but they are transient and volatile.

They may be cached on Objectives, but must be recalculated when needed.

---

## UBU-D0012: Default util spacing uses √2 per preference level

**Status:** Accepted as MVP default, subject to future revision → DESIGN.md §8.5

The default MVP util derivation assigns:

- `1.0` to the lowest preference level,
- each higher level multiplied by `√2`.

Indifferent Objectives are placed at the same level and receive equal util values.

---

## UBU-D0013: UbU has three mutually exclusive operating modes

**Status:** Accepted → DESIGN.md §5

Each UbU instance runs in exactly one mode:

- `user_mode`
- `organization_mode`
- `worker_mode`

The mode is chosen at initialization and cannot change afterward.

---

## UBU-D0014: User mode models intrinsic human affect

**Status:** Accepted → DESIGN.md §5.1

`user_mode` is the mode for an autonomous human user.

Only user mode models intrinsic human affect.

---

## UBU-D0015: Organization mode does not model intrinsic affect

**Status:** Accepted → DESIGN.md §5.2

`organization_mode` represents an organization/project planning instance, but an organization is not a human.

Organization mode does not model intrinsic affect.

---

## UBU-D0016: Worker mode is a special UbU mode for delegated work

**Status:** Accepted → DESIGN.md §5.3

`worker_mode` is a one-device or one-enclave UbU mode used by Automation Workers.

Worker mode may run as:

- a daemon/service,
- a local workstation process,
- a GPU-capable device,
- a thin cloud-control server,
- or another specialized execution environment.

---

## UBU-D0017: Automation Worker is the technical term; Super Automation is a UX/product pattern

**Status:** Accepted → DESIGN.md §24.1

An **Automation Worker** is the technical execution entity.

**Super Automation** is a product/UX pattern in which UbU abstracts away difficult external interaction by using local services, Automation Workers, APIs, screenshots, photos, or LLM processing.

---

## UBU-D0018: GitHub is a projection of UbU, not the source of truth

**Status:** Accepted → DESIGN.md §26

For dogfooding, GitHub is treated as a low-dimensional projection of canonical UbU state.

UbU is the source of truth.

---

## UBU-D0019: GitHub projection requires reconciliation

**Status:** Accepted → DESIGN.md §26.4

Missed GitHub updates are expected in MVP.

UbU should support a reconciliation report comparing GitHub state to UbU state.

---

## UBU-D0020: Objective status and pipeline state are separate

**Status:** Accepted → DESIGN.md §26.2

`Objective.status` is the canonical UbU lifecycle status.

`pipeline_state` is a workflow/project-management status, such as a GitHub issue pipeline state.

---

## UBU-D0021: One GitHub object may map to many UbU objects and vice versa

**Status:** Accepted → DESIGN.md §26.1

GitHub ↔ UbU association is many-to-many.

Examples:

- One GitHub Issue may map to many Objectives.
- One Objective may map to many GitHub Issues.
- PRs, comments, reviews, and CI runs may associate with Objectives or Tasks.

---

## UBU-D0022: Objective has minimal MVP fields

**Status:** Accepted → DESIGN.md §7.2

MVP Objective required fields:

- `objective_id`
- `title_or_description`
- `mode`
- `status`

MVP Objective optional fields:

- `notes`
- `tags`
- `linked_container_refs`

Derived/transient Objective data:

---

## UBU-D0023: Objective modes are one-time or evergreen

**Status:** Accepted → DESIGN.md §7.1

Objectives have mode:

- `one_time`
- `evergreen`

One-time Objectives complete once and do not reactivate.

Evergreen Objectives can become satisfied and later active again.

---

## UBU-D0024: Objective satisfaction is derived in MVP

**Status:** Accepted → DESIGN.md §7.3

Objective satisfaction is not stored directly on Objective in MVP.

It is inferred from:

- Task effects on UniverseState,
- observed Snapshots,
- user declarations,
- other modeled state.

---

## UBU-D0025: WorkItems include Tasks and Containers

**Status:** Accepted → DESIGN.md §9.1

A WorkItem is the abstraction over work-like entities.

A WorkItem may be:

- Task
- Container
- future subtype

---

## UBU-D0026: Plans contain Tasks, not Containers

**Status:** Accepted → DESIGN.md §15.1

Plans contain an ordered array of Tasks.

Plans do not directly contain:

- Containers
- Objectives
- Techniques
- Recipes

---

## UBU-D0027: Static Tasks appear in Plans

**Status:** Accepted → DESIGN.md §9.2

Static Tasks are included directly in Plans.

---

## UBU-D0028: MVP Task schedulability invariant

**Status:** Accepted → DESIGN.md §9.3

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

---

## UBU-D0029: Task preconditions are deterministic UniverseState constraints in MVP

**Status:** Accepted → DESIGN.md §10.1

Task preconditions are deterministic constraints over UniverseState.

MVP preconditions support:

- equality checks
- membership checks
- absence checks
- simple AND/OR logic

Numeric comparisons are not in MVP.

---

## UBU-D0030: Task effects mutate UniverseState

**Status:** Accepted → DESIGN.md §10.2

A Task effect describes predicted mutation of UniverseState if the Task succeeds.

MVP effect object contains:

- scalar success probability
- mutation list

If success probability is `1` or `null`, the effect is assumed to occur when the Task completes.

If a Task fails, UniverseState is unchanged in MVP.

---

## UBU-D0031: Duration uncertainty and effect success probability are distinct

**Status:** Accepted → DESIGN.md §10.3

A Task may have:

- fixed duration or duration PDF,
- scalar success probability on the effect object.

These are separate.

---

## UBU-D0032: UniverseState is first-class

**Status:** Accepted → DESIGN.md §11

UniverseState is a first-class data object.

MVP UniverseState is a lightweight shell with loosely typed facts and events.

Core fields:

- `universe_state_id`
- `timestamp` or valid-at instant
- `facts`
- `numeric_values`
- `set_memberships`
- `event_markers`
- `source_summary`

---

## UBU-D0033: UniverseState uses lightweight free-form keys in MVP

**Status:** Accepted → DESIGN.md §11.2

MVP UniverseState keys are free-form strings.

Default keys should use a lightweight namespace convention, but enforcement is post-MVP.

Values may be text / JSON-like payloads.

---

## UBU-D0034: UniverseState mutation vocabulary

**Status:** Accepted → DESIGN.md §11.3

MVP mutations support:

- `set_fact`
- `clear_fact`
- `increment_numeric`
- `decrement_numeric`
- `add_membership`
- `remove_membership`
- `append_event_marker`

Exact mutation-item schema remains open.

---

## UBU-D0035: Snapshots are observed state updates

**Status:** Accepted → DESIGN.md §12

A Snapshot is an observed update to UniverseState.

User-declared and sensor-derived observations use the same object type.

MVP snapshot fields include:

- `snapshot_id`
- `timestamp`
- `source`
- values
- confidence

---

## UBU-D0036: Latest observed snapshot overrides simulation on conflict

**Status:** Accepted → DESIGN.md §12.1

MVP precedence rule:

- latest observed snapshot overrides simulation on conflicting fields;
- user-declared snapshots are treated as top-priority observations;
- confidence is stored but does not defeat explicit user declaration.

---

## UBU-D0037: Affect belongs to UniverseState in user mode

**Status:** Accepted → DESIGN.md §13

Affect is part of UniverseState in `user_mode`.

Affect is not intrinsic to organizations or machines.

---

## UBU-D0038: MVP affect dimensions

**Status:** Accepted → DESIGN.md §13.1

MVP affect dimensions:

- energy / tiredness
- stress level
- mood

Values are user-queryable.

Energy/tiredness and stress may be scored from `0.0` to `1.0`.

Mood is represented as:

- categorical trinary: `happy`, `sad`, `angry`
- intensity scalar from `0.0` to `1.0`

Interested/bored is independent and derived.

---

## UBU-D0039: Affect confidence decays by age in MVP

**Status:** Accepted → DESIGN.md §13.4

In MVP, affect confidence decreases over time due to staleness.

The threshold/frequency is an algorithm configuration setting.

---

## UBU-D0040: Affect collection is modeled through an evergreen Objective

**Status:** Accepted → DESIGN.md §13.5

UbU may include an evergreen Objective:

> Collect affect information relevant to important usage.

When affect data is missing or stale, the planning algorithm may create UI survey Tasks.

---

## UBU-D0041: External Events are instantaneous

**Status:** Accepted → DESIGN.md §14

An External Event is an instantaneous change in the universe.

External Events have no duration and may overlap Tasks.

They are not part of feasibility evaluation for an individual Plan in MVP.

---

## UBU-D0042: Calendar is a set of possible Plans

**Status:** Accepted → DESIGN.md §15.2

A Calendar is a set of possible Plans.

A Calendar has a default Plan.

The default Plan is a current best recommendation, not a user commitment.

---

## UBU-D0043: Compact Calendar coverage belongs to compact serialization

**Status:** Accepted → DESIGN.md §16.2

Coverage is a property of a compact Calendar representation, not of the abstract Calendar itself.

Coverage represents the probability mass of possible futures covered by the compact representation.

---

## UBU-D0044: Compact Calendar should prefer deterministic planner grammar over opaque PRNG seeds

**Status:** Provisional, refined by `UBU-D0108`, `UBU-D0123`, `UBU-D0124`, and `UBU-D0125` → DESIGN.md §16.3

A compact Calendar may not require PRNG seeds. Earlier design notes used deterministic DFS expansion as the candidate example. The current direction is broader: compact Calendar reconstruction should be based on deterministic, inspectable planner grammar and stored planning metadata, including skeleton Plan structure, legitimization state, candidate lineage, coverage, and repair metadata where appropriate.

DFS-like expansion may still be one implementation technique, but it is not the full architecture.

---

## UBU-D0045: Identity is the external-facing interaction surface

**Status:** Accepted → DESIGN.md §18

UbU interacts with outside agents through Identities.

A human may have multiple Identities.

Organizations and worker-mode instances are externally represented as Identities.

---

## UBU-D0046: Relationship is structured UniverseState data

**Status:** Accepted → DESIGN.md §22

A Relationship is structured UniverseState payload data representing the relationship between two Identities.

MVP Relationship data includes:

- two identity refs;
- one identity controlled by the user;
- user-stated affect state toward the other identity;
- inferred/speculated affect state of the other identity toward the user.

---

## UBU-D0047: Device means execution enclave

**Status:** Accepted → DESIGN.md §23.1

A Device is an execution enclave, not physical hardware.

Examples:

- OS profile
- container
- VM
- secure enclave

---

## UBU-D0048: Zone is a workspace-like instance context

**Status:** Accepted → DESIGN.md §23.2

A Zone is a workspace-like UbU context.

A Device belongs to exactly one Zone.

A Zone may have multiple Devices.

Zones maintain explicit allowlists/denylists of Compartments, defaulting to deny.

---

## UBU-D0049: Compartment is first-class

**Status:** Accepted → DESIGN.md §23.3

A Compartment is a first-class data containment object.

Compartments enforce hard invariants, such as:

- storage backend constraints
- device eligibility constraints
- identity disclosure constraints
- export/integration constraints
- retention constraints
- audit constraints

---

## UBU-D0050: Sensitive content is referenced, not embedded

**Status:** Accepted → DESIGN.md §23.4

If content is specific to a Compartment, a WorkItem should refer to it through a Compartment-scoped reference.

WorkItems must remain structurally usable without dereferencing sensitive content.

---

## UBU-D0051: Risk is reportable, not first-class in MVP

**Status:** Accepted → DESIGN.md §27

Risk is not a first-class object in MVP.

Risk is handled through reports derived from Calendar/Plan analysis.

Examples:

- P90 completion time
- critical path
- deadline miss probability
- affect constraint violation probability
- low coverage warning
- dependency fragility
- worker failure / bottleneck

---

## UBU-D0052: Moot is first-class terminal Task status

**Status:** Accepted → DESIGN.md §9.5

`moot` is a terminal Task status.

It is functionally equivalent to completion for planning, but distinct for logging/reporting.

Moot requires a reason code.

Candidate reason codes remain open.

---

## UBU-D0053: GitHub managed state should be clearly marked

**Status:** Accepted as MVP direction → DESIGN.md §26.3

UbU should avoid fighting manual GitHub edits.

MVP direction:

> UbU writes only clearly marked UbU-managed labels, comments, or blocks, and treats other GitHub edits as external events.

---

## UBU-D0054: The Phase 1 MVP target is dogfooding

**Status:** Accepted → DESIGN.md §4.1

The first MVP should help coordinate the UbU project itself.

Phase 1 focuses on single-user GitHub dogfooding before multi-device sync or multi-user coordination.

---

## UBU-D0055: Scope freeze is now a priority

**Status:** Accepted → DESIGN.md §30

The data-model discussion has reached the point of diminishing private returns.

Further unresolved questions should become public GitHub Issues when possible.

---

## UBU-D0056: File authority model for model-committee runs

**Status:** Accepted → DESIGN.md §30

The canonical source files for model-committee question-answering are:

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

Derived public-facing projections of the canonical design state currently include:

- `README.md`
- `OUTREACH.md`
- `PM_BRIEF.md`
- `FUNDER_BRIEF.md`
- `SOVEREIGN_COORDINATION.md`

---

## UBU-D0057: Model-committee automation is advisory and repo-driven

**Status:** Accepted → DESIGN.md §3

The model-committee process may read the canonical design repo, identify unresolved questions, generate proposed answers, run consistency checks, estimate MVP readiness, score open questions, and propose patches.

The model committee does not maintain its own durable question registry. `OPEN_QUESTIONS.md` remains the canonical unresolved-question queue.

---

## UBU-D0058: Model-committee v0.1 is intentionally constrained

**Status:** Accepted → DESIGN.md §3

The first model-committee implementation should be a narrow local Python CLI, not a general model-governance platform.

v0.1 must implement:

- parse `OPEN_QUESTIONS.md`
- run consistency checks
- rank answerable questions
- generate Codex work prompt
- launch Codex CLI work provider
- run configured Ollama work models sequentially by priority
- import and validate candidate work proposals
- mechanically validate patches

---

## UBU-D0059: Model committee outputs are weighted by capability and observed reliability

**Status:** Accepted → DESIGN.md §3

Model-committee synthesis should not use equal one-model-one-vote aggregation. Models may differ in reasoning quality, context length, instruction-following, access tier, cost, and observed reliability.

Premium or deeper cloud models may receive higher default trust weights than free-tier or smaller local models.

---

## UBU-D0060: Open questions are selected by answerability, automation-likelihood, importance, and risk

**Status:** Accepted → DESIGN.md §3

After consistency checks, the model-committee process should score every open question for:

- answerability,
- automation-likelihood,
- importance,
- risk.

Question selection uses answerability as the first gate.

A question should not be selected for ordinary answer/work execution if it has unresolved dependencies, unless those dependencies are answered in the same work item.

Blocked questions may be selected for decomposition work when decomposition can produce replacement questions with fewer, simpler, or no dependencies.

After answerability is established, questions are ranked by automation-likelihood, then importance, then risk ascending.

---

## UBU-D0061: DECISIONS.md is a bounded decision-memory index

**Status:** Accepted

`DECISIONS.md` is not an unbounded historical transcript. It is a bounded decision-memory index used to preserve accepted constraints and prevent settled questions from being rediscovered.

Full decision history is preserved by git history and model-committee logs.

---

## UBU-D0062: Model-committee is a bootstrap UbU dogfooding workload

**Status:** Accepted → DESIGN.md §3

The model-committee project should begin as a constrained external bootstrap tool, but should move toward the mainline UbU implementation as soon as the mainline codebase can host it cleanly.

---

## UBU-D0063: Model-committee work is changeset-based

**Status:** Accepted → DESIGN.md §3.2

The model-committee process should not rely on a hidden external editor, VS Code agent, or other opaque implementation actor to apply accepted answers.

After the conceptual answer phase, the committee should run an explicit work phase in which models produce concrete changesets.

The work phase is followed by work scoring, work selection, local patch application in later versions, and review artifact generation before consistency checks run.

---

## UBU-D0064: Model-committee v0.1 uses a provisional filesystem log format

**Status:** Accepted → DESIGN.md §3

For v0.1, the model-committee implementation should define a provisional filesystem log format in code.

The final log and provenance format may be refined later by the model-committee process itself.

---

## UBU-D0065: Model-committee v0.1 uses provisional quorum and provider-failure rules

**Status:** Accepted → DESIGN.md §3

For v0.1, model-committee runs should use simple provisional quorum and provider-failure rules.

These rules are sufficient to begin implementation and may be refined later.

Provisional quorum rules:

- Minimum valid work proposals: 1.
- Preferred valid work proposals: 2 or more.
- A Codex work proposal should be attempted before selecting a patch.
- Ollama work proposals should be attempted sequentially by configured priority.
- If Ollama responses finish within timeout and validate, they are included in Codex scoring.
- If an Ollama provider fails, the failure is logged and the run may continue if Codex produces at least one valid work proposal.
- If no valid work proposal exists, the run writes logs and exits nonzero.

---

## UBU-D0066: Model-committee follows a prioritized recursive loop

**Status:** Accepted → DESIGN.md §3.3

The model-committee project should be developed as a bootstrap version of UbU’s future self-automation and dogfooding loop.

The loop has three prioritized modes:

1. system-wide consistency check,
2. question/problem prioritization selection,
3. work.

System-wide consistency checks have highest priority and should run after every merge, UbU directive change, LLM model update, or other state-changing event that could invalidate the project model.

Question/problem prioritization runs after consistency checks and selects the next work item by scoring answerability, automation-likelihood, importance, and risk.

Work is lowest priority and should normally run only after the current project state is coherent and the next work item has been selected.

---

## UBU-D0067: Directive decisions may be appended directly

**Status:** Accepted → DESIGN.md §30

The UbU project may receive direct project-owner directives that are appended to `DECISIONS.md` as accepted decisions without first passing through the ordinary model-committee question-answering loop.

These directive decisions are treated as canonical once committed to `DECISIONS.md`.

---

## UBU-D0068: Model-committee may decompose hard questions into easier questions

**Status:** Accepted → DESIGN.md §3.6

The model-committee process should not treat every increase in open-question count as a failure.

A work proposal may validly split, narrow, or restate a hard question into multiple simpler questions.

---

## UBU-D0069: Model-committee v0.1 uses Codex CLI as the primary model provider

**Status:** Accepted → DESIGN.md §3.1

`model-committee` v0.1 uses Codex CLI as the primary model provider for work proposal generation and work scoring.

The runtime calls `codex exec` with schema-constrained output. Prompts are passed through stdin. Final responses are written to JSON files. JSONL event streams and stderr are preserved in run logs.

Every runtime Codex call must pass `--skip-git-repo-check`.

`model-committee` does not pass deprecated `--disable web_search` flags. If web search must be disabled, that is handled through Codex configuration or profile state outside `model-committee`.

Codex must not directly modify repository files in v0.1. It produces JSON work proposals and score results. Patches are validated and selected by `model-committee`, then written as review artifacts.

---

## UBU-D0070: Model-committee v0.1 authority boundary is explicit

**Status:** Accepted → DESIGN.md §3.10

`model-committee` v0.1 has authority to produce derived analysis, candidate answers, candidate questions, readiness estimates, consistency reports, provider scores, candidate changesets, and review artifacts. It has no authority to create accepted design state without an ordinary committed change to the canonical repo.

Automated actions allowed in v0.1:

- reading canonical design files;
- parsing and scoring open questions;
- running consistency checks;
- generating Codex and Ollama work proposals;
- mechanically validating structured outputs and patches;
- invoking Codex CLI for required scoring;
- selecting a mechanically valid patch from valid proposals when Codex scoring succeeds;
- writing filesystem run logs and review artifacts.

Human review is required for:

- accepting answers, patches, or new questions into canonical design state;
- applying or committing patches to `DESIGN.md`, `DECISIONS.md`, or `OPEN_QUESTIONS.md`;
- closing, decomposing, superseding, archiving, or reprioritizing questions in the canonical repo;
- treating readiness scores as scope-freeze, release, or implementation go/no-go decisions;
- changing provider weights, quorum rules, or provider/network boundaries.

Forbidden actions in v0.1:

- direct OpenAI, Anthropic, Gemini, GitHub, or arbitrary HTTP API calls;
- auto-merge, auto-push, automatic PR creation, or remote GitHub mutation;
- automatic patch application to canonical repo files;
- direct canonical-file edits by Codex CLI, Ollama, or any model provider;
- internal manual override that bypasses failed Codex scoring or patch validation;
- updating derived readiness signals in `README.md` or `OUTREACH.md`;
- treating model output as project-owner directives, canonical user value, or release commitment.

Provider outputs are weighted by configured trust weights and observed reliability metadata rather than raw vote count. Static configured weights are sufficient in v0.1. Adaptive reliability weighting is deferred.

Provider failures are logged as run events. Each failure should preserve the provider ID, model name when known, run phase, failure class, timeout or exit status when available, stderr or raw response artifact path when available, and whether the run still met quorum.

A valid v0.1 committee result requires at least one mechanically valid work proposal and a valid Codex score result. A Codex work proposal should be attempted before patch selection. Two or more valid work proposals are preferred but not required. Failed secondary providers do not invalidate the run when quorum is met.

Codex CLI has subprocess-provider authority only. It may produce schema-constrained proposal and scoring artifacts through `codex exec`, but those artifacts are non-canonical until validated, selected, reviewed by a human, and committed. Direct OpenAI API authority is zero in v0.1 because direct OpenAI API calls are forbidden.

---


## UBU-D0071: MVP Logs are append-only per-instance event records

**Status:** Accepted

Resolved question: `UBU-Q0031`.

MVP Logs use a shared append-only entry envelope with event-specific payloads. Required fields are `log_entry_id`, `schema_version`, `instance_id`, `recorded_at`, `effective_at`, `event_type`, `actor_identity_ref`, `recorded_by_device_ref`, `target_ref`, `result`, `event_payload`, and `provenance`. Optional fields include old/new values, reason, notes, confidence, related Plan references, external references, annotation/correction links, and idempotency keys.

MVP event types are `task_completed`, `task_failed`, `task_moot`, `external_event_observed`, `snapshot_observed`, `objective_transitioned`, `plan_realized`, `decision_recorded`, `recalculation_triggered`, worker mutation submission/application/rejection events, and Log annotation/correction events.

Log entries are immutable once written. An annotation or correction creates a new Log entry that points to the original entry; it does not modify or delete the original. Corrections supersede interpretation of the original entry for query views while preserving the historical record.

Canonical Logs are stored per UbU instance, with device references recorded on entries. Device-local queues may exist for transport and audit, especially in later multi-device sync. MVP retention is indefinite for canonical Logs; archival may move old entries to colder local storage while preserving queryability and integrity. Deletion or redaction is deferred except where required by Compartment retention invariants.

Automation Workers contribute to Logs through worker Identities by submitting events or mutation requests. The canonical instance validates authority and records applied or rejected worker contributions with provenance, confidence when available, evidence references when available, and idempotency keys.

**Consequences:**

- `UBU-Q0031` is resolved for Phase 1 MVP.
- Logs can support audit, reconciliation, worker accountability, plan-vs-reality feedback, and correction without losing historical claims.
- Detailed event-specific payload schemas may be refined alongside Task, Snapshot, Objective transition, worker mutation, and recalculation-trigger schemas.
- Large-history search can rely on rebuildable indexes over the append-only Log rather than treating indexes as canonical state.

---


## UBU-D0072: Phase 1 public demo is an end-to-end GitHub dogfooding loop

**Status:** Accepted

Resolved question: `UBU-Q0030`.

The Phase 1 public demo must prove that UbU can coordinate UbU's own development in a single-user dogfooding loop. It should use real UbU GitHub issues, PR/review/CI events, and milestone context when possible; if live access is unsafe, unavailable, or non-reproducible, a frozen fixture captured from real UbU GitHub data may be used with fixture provenance shown.

The smallest persuasive demo imports a curated issue set, maps it to Objectives and schedulable Tasks, generates a Calendar with a default Plan, explains the chosen Plan, displays at least one risk report, respects a user-mode affect Snapshot or stale-affect collection Task, shows Automation Worker assignment/status for delegated analysis or GitHub projection work, and writes or previews clearly marked UbU-managed GitHub projection labels/comments/blocks.

Live GitHub mutation is not required in a public recording. A dry-run projection is acceptable if it uses the same projection payloads and validation path that would be written after human approval. The demo must not depend on Phase 2 multi-device sync, Phase 3 multi-user coordination, full RBAC, fully settled Compact Calendar planner grammar, or autonomous remote GitHub mutation.

**Consequences:**

- `UBU-Q0030` is resolved for Phase 1 release criteria.
- The demo bar is end-to-end behavior, not complete implementation of every unresolved Phase 1 design question.
- Remaining GitHub projection, worker authority, risk-reporting, affect, and Compact Calendar details may still be refined in their dedicated open questions.
- Public messaging should describe any fixture, dry-run, or human-approval boundary explicitly.

---


## UBU-D0073: Core UbU planning and contributor surfaces are open source

**Status:** Accepted

Resolved question: `UBU-Q0029`.

UbU uses an open-core strategy, but the open core must include the planning kernel and the contributor-facing integration surface. A public contributor must be able to inspect, run, modify, and self-host the core system needed for ordinary single-user and project dogfooding without depending on private replacement components.

The definitely open-source surface includes data model schemas and migrations; the explicit planner and Calendar-generation logic required for Phase 1 dogfooding; GitHub import, triage, projection, reconciliation, fixture/demo tooling, and managed-label/comment/block formats; worker-mode runtime surfaces required for delegated work; compact Calendar serialization needed for transport or analysis; local-first storage and sync protocols when implemented; and the Super Automation extension/API boundary needed for third-party workers, connectors, or local services.

Private or commercial code may exist outside that boundary. Acceptable private areas include hosted-service operations, managed cloud infrastructure, paid support and packaging, premium hosted worker capacity, enterprise administration or compliance layers, proprietary connectors to closed third-party systems, and short-lived experimental prototypes that are not required for the public dogfooding loop.

Private experiments must not become hidden mandatory dependencies for public contributors. If an experimental component becomes necessary for the advertised open-source workflow, UbU must either open it before relying on it publicly or explicitly narrow the public promise. Repositories and packages should be labeled as open core, private experiment, premium hosted service, or external connector so contributors can tell where their work fits.

Implementation repositories should use OSI-approved licenses. The default license for core implementation repos is MPL-2.0 so modifications to core files remain shareable while integrations can be built without relicensing unrelated code. Stronger copyleft may be considered for network-hosted service code, and permissive licensing may be used for small examples, SDK stubs, or interoperability fixtures when that better serves adoption. No contributor agreement should grant unilateral proprietary relicensing of core contributions unless a later accepted decision explicitly changes that rule.

**Consequences:**

- `UBU-Q0029` is resolved for Phase 1 governance.
- The commercial boundary is service, operations, premium capacity, enterprise layers, proprietary closed-system connectors, and private experiments, not a hidden replacement for the planning kernel.
- Public contributor messaging should avoid claims that require private components.
- Future implementation repos need license files and package labels consistent with this boundary.

---


## UBU-D0074: Phase 1 privacy promise is a minimal Compartment guardrail layer

**Status:** Accepted

Resolved question: `UBU-Q0028`.

Phase 1 implements Compartments as metadata-backed classification and routing guardrails, not as the full multi-device containment system intended for later phases. A Phase 1 Compartment can mark content as local-only, disallow cloud LLM routing, disallow external export, declare allowed integrations, and declare allowed Devices within the limits of the single local Device model.

The hard Phase 1 invariants are narrow: `no_cloud_llm` Compartment content must not be sent to cloud LLM routes; `no_external_export` Compartment content must not be exported, projected, or handed to workers except as redacted structural references; Compartment-marked content crosses boundaries only through an allowed route and user-visible action; and boundary-crossing attempts that reach UbU are recorded in Logs as allowed or denied. Sensitive Compartment content is referenced rather than embedded in ordinary WorkItem structure.

Phase 1 does not promise complete privacy isolation, cryptographic isolation, hardware attestation, secure multi-device partial replication, protection from a malicious local administrator, or automated retention deletion/redaction. Retention policies may be recorded, but enforcement beyond append-only Log retention is post-MVP unless a specific implementation later adds and discloses it.

Un-compartmented content is explicitly labeled `security_level: low`. Low-security content is not compartment-protected and may be routed to configured integrations or cloud LLM-backed Automation Workers only through a user-visible integration or worker action.

Phase 1 may claim local-first operation only in the limited sense that canonical planning state, Logs, and source-linked project model data live in the local single-user UbU instance by default. It must not claim local-only operation, Phase 2 sync, conflict handling, partial replication, or secure multi-device Compartment propagation. Phase 1 may claim cloud LLM usage is optional, explicit, integration-scoped, and advisory; cloud LLMs are not the canonical planner and must not receive `no_cloud_llm` Compartment content.

**Consequences:**

- `UBU-Q0028` is resolved for Phase 1 MVP messaging and minimum implementation promises.
- Public messaging must distinguish privacy-first architecture from the narrower implemented Phase 1 privacy baseline.
- Full Compartment enforcement across sync, retention, device eligibility, and cryptographic isolation remains future work unless separately accepted and implemented.
- Implementations must label un-compartmented content as low-security rather than implying default protection.

---


## UBU-D0075: Organization and worker web admin UIs are post-MVP public surfaces

**Status:** Accepted

Resolved question: `UBU-Q0027`.

Organization-mode public UX is a project operations dashboard. The first screen should emphasize pipeline state, Plan/risk summaries, worker assignments and health, pending worker mutation requests, GitHub projection/reconciliation status, and recent decision/projection/worker Log entries requiring attention. It should avoid personal affect UX and should label admin-equivalent operation when full RBAC is absent.

Worker-mode public UX is an operator console for a worker instance. The first screen should emphasize connection and Identity status, service health and last check-in, active assignment and assignment queue, granted capability scopes, recent Log or mutation submissions, rejection/error state, and local resource status such as GPU availability or cloud compute state where applicable.

Both UIs may expose logs, queues, capability grants, resource status, pipeline state, risk reports, worker assignments, and GitHub projection status when the data is relevant to that mode. These views must default to summarized and redacted operational information and must not leak Compartment-protected payloads, broader planning state, or personal affect data through worker/admin convenience views.

No organization-mode or worker-mode web admin UI is required for Phase 1. Phase 1 requires only the single-user GitHub dogfooding surface and enough visible worker assignment/status information to satisfy the public demo criteria; that information may appear in the user-mode dogfooding UI, CLI output, or run artifacts.

**Consequences:**

- `UBU-Q0027` is resolved as a Post-MVP product direction rather than a Phase 1 implementation requirement.
- Phase 1 public UX scope remains focused on the single-user dogfooding loop.
- Future organization and worker admin UIs have a stable default first-screen direction without forcing RBAC or full admin products into MVP.
- Admin views must respect Compartment boundaries and mode boundaries.


---


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

- `UBU-Q0026` is resolved for Phase 1 modeling.
- Relationship payload stays small and privacy-sensitive.
- Communication cadence remains schedulable and recalculable through Objective recurrence.
- GitHub dogfooding can model contributor follow-up without adding a new relationship subsystem to MVP.
- Future richer relationship-management features can refine cadence evidence and private notes without changing the MVP anchor model.

---

## UBU-D0077: Model-committee v0.1 marks active pre-MVP dogfooding

**Status:** Accepted

`model-committee v0.1` marks a transition from pure design preparation into active pre-MVP dogfooding.

The project now has a runnable bootstrap artifact that can help process UbU design questions, generate reviewable changesets, and make the dogfooding loop visible.

This does not make `model-committee` the full UbU planner. It is a constrained bootstrap workload and advisory Automation Worker pattern.

**Consequences:**

- Public messaging should no longer imply that UbU is only an ideal design without runnable project machinery.
- The next strategic bottleneck is no longer the absence of a runnable skeleton.
- The next strategic bottlenecks are contributor conversion, visible dogfooding output, first-use workflow selection, and prototype-funder discovery.
- Further design writing should generally be subordinated to implementation, recruitment, dogfooding, or market discovery.
- `README.md` and `OUTREACH.md` should describe active pre-MVP dogfooding while preserving the canonical authority of `DESIGN.md`, `DECISIONS.md`, and `OPEN_QUESTIONS.md`.

---

## UBU-D0078: Recruitment targets a small core cohort, not a single co-builder

**Status:** Accepted

UbU’s near-term recruitment goal is to attract a small core cohort of serious, self-directed contributors.

The project should avoid public language implying that there is only one meaningful contributor role or that interested developers are competing for a single privileged position.

Popularity is not the goal, but multiple committed builders could materially accelerate UbU toward sustained full-time development.

**Consequences:**

- Public outreach should welcome several serious contributors.
- Contributor language should emphasize self-direction, seriousness, alignment, and concrete work rather than scarcity.
- The project should prefer committed contributors who can own bounded subsystems over broad passive interest.
- Casual interest remains useful when it produces workflow examples, design feedback, contributor leads, or prototype-funder leads.
- Recruitment success should be measured by concrete follow-up, contributions, design review, prototype funding, or subsystem ownership rather than raw attention.

---

## UBU-D0079: Independent knowledge workers are the first commercial beachhead hypothesis

**Status:** Accepted

UbU will treat privacy-sensitive independent knowledge workers as a leading commercial beachhead hypothesis.

This includes independent technical consultants, freelance developers, security researchers, solo founders, independent academics, technical writers, and similar workers who manage complex multi-client work privately.

This market is compatible with UbU’s north-star vision because these users experience personal self-governance, confidentiality, dependency, deadline, attention, and affect constraints directly.

Prototype funding from this market is acceptable only when it funds trunk features needed by the full personal self-governance product.

**Consequences:**

- Commercial discovery should include interviews with independent knowledge workers, not only FOSS maintainers or Ethereum teams.
- Prototype-funder outreach should test whether these users would fund or prepay for concrete core functionality.
- Acceptable funded work includes the planning kernel, local-first data model, privacy/Compartment model, affect-aware planning, GitHub/calendar/task ingestion, Logs, and recalculable Plans.
- Funding that requires surveillance, generic enterprise dashboards, centralized productivity telemetry, or manager-first reporting is incompatible unless a later accepted decision changes the project boundary.
- This decision is a market hypothesis, not a commitment to abandon FOSS or personal self-governance.

---

## UBU-D0080: Ethereum and FOSS outreach are recruitment and validation channels

**Status:** Accepted

Ethereum, FOSS, protocol, and autonomous developer communities are valuable early outreach channels because they expose UbU to high-autonomy contributors, complex coordination failures, anti-surveillance norms, and developer-tooling expectations.

These communities are not necessarily the first commercial buyer market.

Their primary near-term role is recruitment, workflow discovery, dogfooding validation, contributor-sustainability research, and project-management pain discovery.

**Consequences:**

- ETHConf and similar events should be treated as discovery and recruitment opportunities, not as proof that UbU is primarily a crypto project.
- FOSS outreach should seek concrete workflow examples, contributor candidates, design partners, and maintainers willing to discuss real coordination failures.
- Ethereum/protocol outreach should avoid token-first positioning.
- Project-management positioning should remain subordinate to personal self-governance and contributor sovereignty.
- UbU should not chase generic crypto speculation communities.

---

## UBU-D0081: Affect-aware planning is a core humane-planning requirement

**Status:** Accepted

UbU treats affect-aware planning as a core requirement, not a secondary wellness feature.

Planning systems that ignore fatigue, stress, boredom, motivation, emotional load, recovery, and dignity can produce plans that are technically organized but humanly unrealistic or harmful.

UbU should provide fast feedback about plan success or failure, suggest improvement after failure, respect emotional and physical limits, and still help users grow beyond current limitations.

The goal is neither comfort-maximization nor coercive productivity. The goal is humane self-governance: disciplined action that respects the user’s emotional and physical reality.

**Consequences:**

- Affect belongs to the core planning model in `user_mode`.
- Plans should be evaluated not only for logistical feasibility but also for human realism.
- A plan that repeatedly fails due to affect, fatigue, stress, boredom, or overload should cause model revision rather than user-blaming.
- Risk reports should eventually identify affect-related plan fragility.
- Public messaging may describe conventional planning tools as affect-blind, emotionally incomplete, human-incomplete, or mechanistic.

---

## UBU-D0082: High-quality plans need feedback, dignity, limits, and growth pressure

**Status:** Accepted

A UbU plan is not high-quality merely because it is internally consistent or time-feasible.

A high-quality plan should:

- produce fast feedback about success or failure;
- make failure informative rather than humiliating;
- suggest revision after failure;
- respect the user’s dignity;
- respect emotional and physical limits;
- distinguish sustainable stretch from destructive pressure;
- help the user grow beyond current limitations when appropriate;
- remain recalculable when reality contradicts the model.

This invariant is especially important in `user_mode`, where affect belongs to UniverseState.

**Consequences:**

- Plan evaluation should eventually consider whether a plan supports learning, adaptation, and sustainable self-improvement.
- Failure should generally produce improved modeling, revised constraints, or new suggested actions rather than moralized blame.
- UbU should push users toward growth only within a humane and feedback-sensitive planning loop.
- A plan that leaves the user worse off despite achieving nominal task completion should be considered suspect.
- This decision should inform `DESIGN.md` planning-quality language and future UI/UX choices.

---

## UBU-D0083: Technical essay should present a problem-first planning-kernel thesis

**Status:** Accepted

UbU should publish a problem-first technical essay that explains the planning-kernel thesis without reducing UbU to a product brochure.

The essay should argue that ordinary task managers, calendars, project boards, and opaque AI assistants fail to model real work because they do not represent objectives, state transitions, dependencies, constraints, logs, uncertainty, affect, and human limitations explicitly.

The essay should emphasize that most existing planning tools are affect-blind. They model tasks, deadlines, statuses, and calendar blocks, but not the emotional and physical reality of the person who must execute the plan.

The essay should explain why LLMs are useful but advisory: they can interpret, summarize, propose, and critique, but canonical planning must remain explicit, inspectable, and recalculable.

The essay should point to `model-committee` as UbU’s first visible dogfooding loop and end with a concrete contribution, interview, or prototype-funder request.

**Consequences:**

- The essay should lead with the problem, not with a feature list.
- The essay should avoid generic startup marketing language.
- The essay should invite concrete next actions from developers, maintainers, autonomous-team leads, and prototype funders.
- `OUTREACH.md` may summarize the essay project, but the essay itself may live outside the canonical design files.

---

## UBU-D0084: Public narrative may use moderated long-arc framing

**Status:** Accepted

UbU may publicly describe its long intellectual development history and the fact that modern LLMs have made the project newly viable.

This narrative may be used to attract contributors who want to work on software with durable significance.

Public framing should remain grounded, humble, and specific. It should avoid unrealistic claims, inevitability claims, or overstated promises about changing the world.

**Consequences:**

- Outreach may describe UbU as a long-running personal and technical project whose time may now be arriving.
- The narrative may help explain why the project has a mature design before it has a mature product.
- The narrative should be used to communicate commitment and depth, not destiny or superiority.
- Contributor recruitment should connect the long arc to concrete present work.

---

## UBU-D0085: Avoid exclusionary and scarcity-implying public framing

**Status:** Accepted

UbU public materials should avoid language that unintentionally makes interested contributors feel unwelcome, replaceable, or in competition for a single meaningful role.

UbU should also avoid accusatory language when describing planning systems that fail to model affect.

Preferred terms include:

- affect-blind;
- emotionally incomplete;
- human-incomplete;
- mechanistic planning;
- machine-like planning;
- non-humane planning;
- affect-insensitive planning.

**Consequences:**

- Public recruitment language should refer to a small core cohort or several serious contributors rather than a single highest-priority co-builder slot.
- Public language may still distinguish between serious contributors and passive interest.
- The project may strongly criticize mechanistic planning systems without using negative metaphors.
- Derived public-facing files should be updated when they imply scarcity or exclusion.
- Canonical design files should remain precise and humane in how they discuss affect, dignity, limits, and growth.

---

## UBU-D0086: Public outreach should convert interest into concrete next actions

**Status:** Accepted

UbU should not treat popularity as achievement.

Public outreach is successful when it produces concrete next actions that accelerate the project toward a working self-governance product.

Concrete next actions include:

- code contributions;
- test fixtures;
- design review;
- workflow examples;
- maintainer interviews;
- prototype-funder conversations;
- implementation-ready issues;
- review of `model-committee` run artifacts;
- sustained subsystem ownership;
- credible paths toward full-time development of core features.

---

## UBU-D0087: Commercial funding must preserve self-governance

**Status:** Accepted

UbU may accept funding, consulting, sponsorship, prepayment, or commercial prototype work only when the work accelerates trunk capabilities needed by the personal self-governance product and preserves user sovereignty, contributor sovereignty, privacy, and the open-core boundary.

A funding offer is compatible when it pays for reusable core work such as the planning kernel, local-first data model, privacy and Compartment guardrails, affect-aware planning, GitHub/calendar/task ingestion, Logs, recalculable Plans, risk reports, worker assignment/status, projection/reconciliation, or other features that become ordinary trunk capability.

Incompatible requested features include:

- surveillance-style project management, activity scoring, keystroke or screen monitoring, or always-on productivity telemetry;
- manager-first dashboards that rank, pressure, compare, or discipline contributors instead of supporting explicit self-governance and project coordination;
- centralized productivity telemetry across users, teams, clients, or contributors without explicit user-controlled sharing;
- generic enterprise reporting dashboards that pull UbU away from the planning kernel and toward conventional management software;
- features that expose affect, relationship state, Compartment payloads, or low-security personal context to managers, funders, or customers as a condition of use;
- token-first, speculation-first, governance-token, DAO-dashboard, or financialized coordination work that would reposition UbU as a crypto product;
- bespoke custom branches whose value depends on one funder's private workflow and does not produce reusable trunk capability.

Incompatible funding terms include:

- funder veto or control over the roadmap, canonical design process, accepted decisions, licensing boundary, or contributor access;
- exclusive ownership, assignment, or unilateral proprietary relicensing of open-core work;
- private replacement components for functionality that public contributors need for ordinary dogfooding;
- confidentiality terms that prevent honest public explanation of architecture, privacy limits, funded influence, or contributor-facing behavior;
- required pivot away from personal self-governance, privacy-first architecture, or the open-core planning kernel.

Acceptable prototype funding must default to trunk-first implementation. Customer-specific configuration, connectors, hosting, packaging, support, or compliance work may remain commercial when it stays outside the open-core boundary and does not become required for public dogfooding.

Crypto, tokenization, and DAO-specific requests should be rejected when they are token-first or speculation-first. They should be deferred when they are merely integration-specific and not required for the personal self-governance trunk. Ethereum and FOSS communities remain useful for recruitment, validation, and workflow discovery, not as a reason to reposition UbU as a crypto product.

Paid work should be evaluated with a simple rule: if the funded deliverable would make the open personal self-governance trunk better for independent knowledge workers and future public dogfooding, it may be considered; if it creates a private branch, surveillance surface, or funder-controlled roadmap, it should be rejected or renegotiated.

**Consequences:**

- `UBU-Q0048` is resolved for Phase 1 commercial red-line policy.
- Prototype-funder discovery can proceed with a clear accept/reject screen.
- Commercial discovery should prioritize independent knowledge-worker workflows that fund core planning, privacy, affect, worker, and projection capabilities.
- Enterprise opportunities are acceptable only when they preserve contributor sovereignty and remain subordinate to the planning-kernel trunk.
- Ethereum or DAO-related opportunities must not drive token-first positioning.
- Governance review remains required for ambiguous funding terms, especially exclusivity, IP assignment, confidentiality, roadmap control, and custom-branch obligations.

---

## UBU-D0088: Committed-contributor onboarding is public, bounded, and fixture-first

**Status:** Accepted

The minimum onboarding path for serious, self-directed contributors is a public, bounded path from context to a small verified contribution. It must not depend on private project-owner chats, unstated roadmap knowledge, private calendars, private GitHub data, or credentials.

A contributor should first read:

- the project overview and core principles in `DESIGN.md`;
- the model-committee dogfooding, Phase 1 public demo, GitHub dogfooding, design-process, open-core, and Phase 1 Compartment sections of `DESIGN.md`;
- the accepted decisions for open-core boundary, minimal Compartment promise, small core cohort recruitment, concrete outreach actions, and commercial self-governance red lines;
- the specific public issue, fixture, or model-committee artifact they intend to touch.

The first command should be a no-private-access local smoke check. For `model-committee`, the expected first command is `uv run model-committee doctor`, followed by a fake-provider or fixture-backed test/run when the implementation repo supports it. Equivalent first commands for later modules must avoid private tokens and should make missing optional providers or credentials visible as diagnostics rather than hidden prerequisites.

The first contribution should be one of: a synthetic or redacted workflow example, a fixture, a parser/validation test, review of a `model-committee` run artifact, documentation tied to a specific open question, or a narrow implementation-ready issue with explicit acceptance criteria. New contributors should not begin with hidden-roadmap work, direct GitHub mutation, Compartment-sensitive payloads, private-context reconstruction, broad planner rewrites, or module ownership.

Before implementation work, contributors must understand these boundaries: canonical design state lives only in committed canonical files; `model-committee` is advisory; v0.1 provider/network and no-auto-apply limits are hard; GitHub is a projection, not the source of truth; LLMs are advisory, not canonical planners; Compartment and low-security-content promises constrain data routing; the planning kernel and contributor-facing integration surface are open core; user sovereignty and mode boundaries are non-negotiable.

Contributor progression is staged:

---

## UBU-D0089: First prototype-funder workflow is commitment-risk review plus next-day plan

**Status:** Accepted → DESIGN.md §4

The smallest fundable workflow for privacy-sensitive independent technical consultants is a local client commitment-risk review plus next-day Plan. It addresses the pain of discovering too late that multi-client commitments, deadlines, dependencies, energy, and unavailable time no longer fit.

The first prototype should not require full inbox, notes, invoice, or message-body ingestion. Required inputs are manual client/project declarations, current commitments/deadlines, tasks or work items, current availability, compartment labels, and a current or stale affect Snapshot. Optional inputs are calendar busy/free blocks and GitHub issue/PR references when the consultant explicitly connects them. Email, notes, invoices, and private client documents may be represented only as user-approved structural references or later explicit connectors that obey Compartment rules.

The primary output is a compartment-aware weekly commitment-risk report with a next-day default Plan. The report should show each client/project compartment at a structural level, identify overcommitment, deadline risk, stale or blocked work, cross-client dependency pressure, and affect/energy constraint risk, then propose the next concrete work window. The output must be inspectable and recalculable from explicit data; it must not rank client value secretly or expose one client's sensitive payload in another client view.

A daily plan alone is too generic for the first funded prototype. A client-compartment view alone is too static. A weekly review alone may be too passive. The smallest valuable bundle is the weekly risk review plus the next-day Plan because it converts private commitments into immediate action without requiring broad surveillance-style ingestion.

Paid prototype work remains on the trunk only if the implementation uses reusable open-core structures: Objectives for client/project outcomes, Tasks for commitments, Compartments for client separation, Logs for declarations and actuals, Calendar/Plan generation for schedule recommendations, risk reports for commitment and dependency pressure, and optional GitHub/calendar ingestion through user-visible routes. Customer-specific templates, data imports, hosting, credentials, and support may remain commercial configuration.

Prototype-funder discovery should test prepaid design-partner structures, not open-ended bespoke consulting. Plausible offers to test are: a paid discovery/review session, a prepaid prototype sponsorship that funds an implementation slice, or a monthly design-partner retainer. As a working hypothesis, discovery can test roughly USD 250-750 for a focused review, USD 2,000-10,000 for a prototype sponsorship, and USD 1,000-3,000/month for a limited design-partner retainer. These are discovery hypotheses, not permanent pricing policy.

---

## UBU-D0090: Long-arc public narrative stays modest and testable

**Status:** Accepted

UbU may say that it grew out of a long personal and technical effort to understand planning, self-governance, privacy, and humane coordination. Public materials should include only enough of that history to explain commitment, design maturity, and why the project has more structure than a fresh prototype.

Modern LLMs should be described as changing the feasibility frontier for UbU, not as making success automatic. The grounded claim is that LLMs now make interpretation, summarization, proposal generation, fixture creation, review, and advisory automation cheap enough to support an explicit planning kernel. LLMs remain bounded assistants; they do not replace UbU's canonical planner, user sovereignty, or inspectable data model.

The long-arc narrative should communicate seriousness and durable possibility. Preferred public phrasing includes `long-running personal and technical project`, `the tools may finally be good enough to build the first narrow version`, `could matter for a long time if the dogfooding and contributor work prove it`, and `worth building carefully in public`.

The detailed version belongs in the first technical essay, selected outreach, talks, and interviews. README-level use should stay short and connect immediately to active dogfooding, concrete contribution paths, and prototype-funder discovery.

Avoid language that implies destiny, superiority, inevitability, or guaranteed world-historical impact. Avoid claims such as `will change the world`, `inevitable`, `once-in-a-generation`, `the future of work`, `solves coordination`, `the only real solution`, `decades ahead`, `everyone will need this`, and `LLMs make this inevitable`.

---

## UBU-D0091: First technical essay makes a precise planning-kernel request

**Status:** Accepted

The first public technical essay should make one central claim: ordinary task managers, calendars, project boards, and opaque AI assistants are useful projections, but they are not enough for real planning because they usually do not model objectives, state transitions, dependencies, constraints, logs, uncertainty, affect, and recalculation as explicit objects.

The working title should be:

> The Planning Kernel: What Task Managers Leave Out

The previously proposed title, `The Planning Kernel: Why Your Task Manager Is Lying to You`, should not be used as the working public title. It is memorable, but it implies accusation and undermines the humane, problem-first tone.

The essay should explain affect-blind planning as human-incomplete planning. Fatigue, stress, boredom, motivation, emotional load, recovery, and dignity are planning constraints. They are not excuses, decorative wellness features, or moral failures. A plan that ignores them can be internally organized while still being unrealistic for the human who must execute it.

The essay should distinguish UbU from adjacent tools as follows:

- task managers record intended work;
- calendars allocate time;
- project boards track workflow status;
- opaque AI assistants can generate plausible suggestions;

---

## UBU-D0092: Plan quality includes fast feedback, dignity, limits, and humane stretch

**Status:** Accepted → DESIGN.md §2.5.1

Phase 1 models human-complete plan quality as derived analysis over Plans, Tasks, Logs, Snapshots, affect constraints, and risk reports. It does not add a first-class canonical PlanQuality object in MVP. Any cached assessment is advisory and recalculable.

A candidate Plan should be checked for:

- fast feedback: important work has an observable checkpoint soon enough to revise before large loss;
- checkpoint coverage: success, failure, blocked preconditions, user overrides, affect Snapshots, or External Events can reveal whether the Plan is still accurate;
- affect margin: scheduled work does not consume energy, stress, mood, or recovery capacity beyond configured user-mode limits;
- dignity preservation: failure presentation does not moralize the user, expose avoidable embarrassment, or reuse wording that frames observed limits as character flaws;
- non-blaming revision: failed execution should suggest model repair, smaller Tasks, added checkpoints, updated estimates, recovery, clarification, delegation, or Objective reconsideration;
- humane stretch: the Plan may exceed the current baseline when feedback is close, recovery margin remains, and the expected post-plan state is not worse;
- destructive pressure: the Plan depends on overriding observed limits, lacks recovery, hides failure until too late, or repeatedly requires performance above observed capacity.

Task failure remains an ordinary Log result, but the interpretation of that failure should be plan-centered. UbU should treat failure as evidence about estimates, constraints, dependencies, affect state, interruptions, or Objective fit unless the user explicitly records another interpretation.

---

## UBU-D0093: Public recruitment invites several serious contributors into bounded paths

**Status:** Accepted

UbU's public recruitment copy should invite a small core cohort of serious, self-directed contributors, not a single co-builder, founding slot, or privileged insider. Seriousness is shown by concrete public follow-up: a workflow example, design review, fixture/test contribution, model-committee artifact review, narrow implementation-ready issue, or repeated reviewed work on a bounded subsystem.

Baseline public copy:

> UbU is looking for a small core cohort of serious, self-directed contributors. Good first contributions include workflow examples, design review, synthetic or redacted fixtures, parser or validation tests, model-committee artifact review, and narrow implementation-ready issues. If the work keeps the planning kernel inspectable, privacy-first, and useful for dogfooding, there is room for multiple contributors to earn bounded ownership over subsystems.

Public materials may name the contributor stages from onboarding: workflow informant, design reviewer, fixture/test contributor, implementation contributor, and bounded module owner. They may also route prototype funders and design partners into discovery, but prototype funding is not a contributor rank and must remain governed by the self-governance funding red lines.

The distinction between serious contribution and passive interest should be concrete rather than exclusionary. Passive interest is welcome when it produces workflow examples, design feedback, useful introductions, prototype-funder leads, public issue comments, or future contributor candidates. Serious contributor language should emphasize bounded public work, tests or fixtures, reviewed artifacts, and eventual subsystem ownership.

Avoid public phrases that imply scarcity or competition for one role, including `the co-builder`, `highest-priority contact`, `one founding slot`, `only serious builder`, `competing for a role`, `exclusive inner circle`, `prove you belong`, `hand-picked elite`, and similar language. If `co-builder` is ever used informally, it should not be singular or described as the single highest-priority outcome.

ETHConf and similar follow-up should classify people by their most useful next concrete action: send one workflow example, review a model-committee artifact, comment on a public design question, contribute a fixture/test, take a narrow implementation-ready issue, introduce a serious contributor, or discuss a trunk-compatible prototype sponsorship. Enthusiasm alone is not validation until it becomes one of these follow-ups.

---

## UBU-D0094: Release Outreach Pipeline makes releases explain themselves

**Status:** Accepted

UbU accepts the Release Outreach Pipeline feature bundle, with the tagline:

> UbU should make every release explain itself.

Release management in UbU should treat public and project-facing explanation artifacts as first-class release outputs. In addition to code, tests, changelogs, builds, and deployment artifacts, a meaningful release may require user-facing release notes, developer-facing release notes, screenshots, scripted UI-demo captures, video scripts, narration text, captions, YouTube metadata, public posts, known-limitations summaries, and contributor calls-to-action.

For UbU-runs-UbU, each minor release should normally produce a release outreach package when there is enough change to explain. The package should be derived from current repo state, release notes, accepted decisions, closed issues, updated design files, automated UI screenshots, scripted demo recordings, test fixtures, and explicitly labeled future plans.

Generated outreach must be evidence-bound. Claims about implemented behavior should be traceable to implemented features, accepted decisions, closed issues, release notes, test artifacts, screenshots, demo recordings, or other recorded evidence. Mock behavior, future plans, and speculative goals must be labeled as such.

The pipeline should support a fast public explanation path for nontechnical or lightly technical users and a sharper developer call-to-action path for serious contributors. A video can show the simple user-facing loop - bootstrap, one next task, explanation, feedback, and humane relationship or goal prompts - then close by showing UbU dogfooding its own GitHub issues, design docs, release tasks, and contributor needs.

The feature generalizes beyond UbU's own outreach. Project-management configurations should be able to define communication Objectives for users, developers, maintainers, funders, internal stakeholders, customers, community members, or other audiences. Release communication should become ordinary project work, not an afterthought.

Publication is gated by default. UbU may draft, assemble, render, and prepare outreach artifacts automatically, but external publication requires explicit human approval unless a project has configured a narrow trusted auto-publication policy. The pipeline must respect Compartment, Identity, export, and public-projection boundaries, and it must not leak private planning notes, contributor communications, personal data, or sensitive screenshots into public artifacts.

The implementation should be staged:

- Phase 0: manually structured release notes, screenshot lists, scripts, and calls-to-action.

---

## UBU-D0095: Worker authority uses scoped capability grants

**Status:** Accepted → DESIGN.md §24

Automation Worker authority is represented through explicit capability grants associated with a worker Identity. The Identity is the external-facing actor and credential subject; the capability grant is the authoritative object that says what the worker may do for a specific parent UbU instance.

Phase 1 capability verbs are `task.read`, `objective.read`, `universe_state.read_subset`, `external_event.append`, `snapshot.submit`, `mutation_request.submit`, `recalculation.request`, and `projection.github.request_update`. Direct creation or mutation of Tasks, Objectives, UniverseState, `pipeline_state`, or GitHub projection state is not granted to workers in Phase 1. Workers submit mutation or projection requests, and the canonical instance validates, applies, rejects, and logs the result.

Capability grants may be scoped by object ID, Objective subtree, Task set, Compartment, external integration, operation kind, and time window. Compartment policy is a hard upper bound on any worker grant: a grant cannot authorize cloud LLM routing, external export, worker handoff, or payload disclosure that the relevant Compartment forbids.

Workers can be revoked by disabling or deleting grants and invalidating or rotating credentials. Revocation affects future access and submissions only; prior Log entries remain append-only history. Credential rotation creates a new credential version for the worker Identity while preserving audit continuity unless the underlying grants are changed.

A worker may serve multiple organization-mode or user-mode parent instances only through separate parent-specific grants, credentials, and audit trails. Cross-parent data sharing is forbidden unless each parent explicitly grants the route and all relevant Compartment policies allow it. User-mode workers are allowed, but access to affect or other personal data must use narrow read-subset grants and obey Compartment and low-security disclosure rules.

---

## UBU-D0096: Phase 1 requires bootstrap interview and next-action focus UX

**Status:** Accepted → DESIGN.md §4.1

UbU Phase 1 must include a minimal first-person user-facing loop, not merely an internal planner, GitHub importer, model-committee loop, or project-state analyzer.

Phase 1 requires two user-facing UX primitives:

1. **Bootstrap interview**: UbU begins by asking a small number of questions that help form an initial model of the user, current context, important Objectives, relevant constraints, current or stale affect Snapshot, and immediate dogfooding/project context.

2. **Next-action focus mode**: UbU can present one recommended next Task at a time, with an explanation of why that Task matters now. The full Plan remains inspectable, but the default user experience may reduce immediate cognitive load by showing a single next action.

The Phase 1 next-action screen should include, at minimum:

- the recommended Task;
- estimated duration or work window when available;
- why this Task matters now;
- what inputs or constraints UbU considered;
- current affect or stale-affect status when relevant;

---

## UBU-D0097: Phase 1 MVP scope is frozen around single-user GitHub dogfooding

**Status:** Accepted → DESIGN.md §4.1

Phase 1 is frozen as the minimum single-user `user_mode` implementation that proves UbU can coordinate UbU's own development through explicit state, an inspectable Plan, one recommended next action, feedback, recalculation, and bounded GitHub projection.

The exact Phase 1 feature set is:

- local single-user UbU instance for UbU-runs-UbU;
- bootstrap interview, current or stale affect Snapshot handling, and initial Objective/Task seed creation;
- live or fixture-backed GitHub import for issues, PRs, reviews, CI events, milestones, comments, and source links;
- ExternalReference-style mapping between GitHub objects and UbU Objectives, Tasks, External Events, and Logs;
- MVP Objective, Preference, Task, Container, UniverseState, Snapshot, Plan, Calendar, Log, Identity, Relationship, Compartment, Automation Worker, External Event, External Reference, and deferred Association-related schemas only to the depth required for the dogfooding loop;
- schedulable Static and Dynamic Tasks with Objective links, duration, dependency/precondition/effect fields, lifecycle status, and moot handling;
- lightweight UniverseState mutation and precondition evaluation sufficient for Task effects, affect, relationship-relevant facts, and GitHub/project facts;
- append-only per-instance Logs with provenance, correction, annotation, worker-submission, and recalculation-trigger entries;

---

## UBU-D0098: GitHub external links use first-class External References

**Status:** Accepted → DESIGN.md §19

GitHub-to-UbU links are represented by first-class `ExternalReference` objects rather than by embedding all many-to-many source links directly on core objects. Lightweight `external_refs` fields may still appear on Log entries, provenance payloads, or import artifacts as convenience references, but they are not the authoritative external-link model.

MVP `ExternalReference` required fields are `external_reference_id`, `external_system`, `external_object_type`, `external_object_id`, `ubu_object_type`, `ubu_object_id`, `relation_type`, `confidence`, `created_by_identity_ref`, `created_at`, `last_verified_at`, `sync_policy`, `projection_policy`, and `provenance`.

Phase 1 external references may target Objectives, Tasks, External Events, and Log entries. This supports GitHub Issues mapping to multiple Objectives, Objectives mapping to multiple Issues, PRs and CI runs being retained as External Events, and comments or reviews acting as evidence for Tasks, Objective transitions, reconciliation, or projection decisions.

MVP relation types are `represents`, `supports`, `evidence_for`, `source_event_for`, `projection_of`, `duplicate_of`, and `supersedes`. Relation types are schema-controlled enums in MVP, not free-form labels.

Duplicate detection uses a normalized uniqueness key over `external_system`, `external_object_type`, `external_object_id`, `ubu_object_type`, `ubu_object_id`, and `relation_type`. Importers must normalize equivalent GitHub identifiers such as repository name, issue or PR number, node ID, URL, comment ID, run ID, and webhook delivery ID before comparing. Reobserving the same external reference updates verification metadata or creates an idempotent no-op Log entry rather than duplicating the external reference. Distinct relation types between the same objects are allowed.

Automation Workers cannot directly create or mutate canonical External References in Phase 1. They may submit external-reference mutation requests only through authorized `mutation_request.submit` capability grants. The canonical instance validates authority, Compartment/export policy, duplicate keys, expected prior version, and provenance, then logs applied or rejected outcomes.

---

## UBU-D0099: Psychological theory inputs are calibration, discovery, preview, and review layers

**Status:** Accepted → DESIGN.md §2.2.1

UbU should not expose internally computed utility values as user-facing truth. Derived utils remain transient computational artifacts used for scheduling, ranking, and comparison. User-facing value remains grounded in explicit Preferences, user declarations, Logs, Snapshots, review, and later correction.

Psychological and philosophical decision-theory inputs should usually enter Phase 1 through calibration, discovery, Calendar preview, Log review, reports, and reusable Tasks rather than through a large new psychology ontology.

Prospect-theory implications are accepted as preference-calibration requirements, not as an exposed utility model. UbU may show default preference examples and common-situation emotional-value examples during onboarding, Calendar preview, or Log review so the user can make more thoughtful Preference statements. These examples are grounding aids. They do not become canonical Preferences unless accepted by the user.

Temporal discounting is usually part of Preference for short-horizon Phase 1 planning. Long-horizon Objectives may later require explicit future-self or commitment-device modeling, but this is not a Phase 1 blocker.

Habit-related behavior should be handled through discovery mode, Logs, Snapshots, configured integrations, sensor-derived observations, and later clarification prompts. Discovery mode is a user-selectable workflow state that the user may choose at any time, especially from a mobile app with useful embedded sensors. Discovery mode supports later Log review and UbU-directed reconciliation for undetailed or under-specified time periods.

The Calendar is advisory. User overrides remain authoritative and should be treated as model evidence, not disobedience. Repeated overrides may trigger review, preference recalibration, Task decomposition, Objective reconsideration, or habit-pattern hypotheses only after appropriate user-visible reconciliation.

Calendar preview and Log review are notable Tasks that should run on a regular basis. They help verify whether UbU is correctly modeling the user's intended behavior, actual behavior, affective constraints, and preference judgments. These review Tasks should remain inspectable, interruptible, and adjustable by the user.

Self-determination theory and theory of planned behavior should primarily inform Calendar preview, Log review, reporting annotations, and optional user comments about motivation, autonomy, competence, relatedness, attitude, subjective norms, perceived control, and expected execution. Detailed modeling of those constructs remains open unless required by a concrete MVP workflow.

Narrative identity is partly addressed through Objectives and Reports. Social identity theory, social choice theory, and game theory are important post-MVP open-question areas and should be tracked explicitly without blocking Phase 1 implementation.

---

## UBU-D0100: Snapshots are immutable partial observed assertions

**Status:** Accepted → DESIGN.md §12

Snapshots are partial observed assertions over specific UniverseState fields. They are not full assertions of all UniverseState and do not imply that omitted fields are absent, unchanged, or unknown. Applying a Snapshot updates only the fields included in that Snapshot.

Snapshot records are immutable once accepted into the append-only Log. The original Snapshot observation is never edited or deleted as canonical history. Annotation, correction, and revocation are represented by later Log entries that point to the original Snapshot observation or Log entry.

MVP confidence is stored at both levels: a required snapshot-level confidence summarizes the observation as a whole, and optional per-field confidence overrides may be present when different observed dimensions have different reliability. If a field has no per-field confidence, it inherits the snapshot-level confidence. For affect Snapshots, the existing MVP rule remains: affect confidence may be treated globally across affect dimensions, with per-dimension confidence deferred unless a Snapshot explicitly provides per-field confidence.

Conflict resolution is field-local. Latest observed Snapshot data overrides simulated state for conflicting fields. User-declared Snapshots have top priority over sensor-derived, imported, inferred, or worker-submitted observations in MVP. If two user-declared Snapshots conflict on the same field, the latest `effective_at` or Snapshot timestamp wins unless a later correction supersedes it. For non-user observations, the application algorithm may use source priority, effective timestamp, and confidence, but confidence does not defeat an explicit user declaration in MVP.

A Snapshot can be corrected or revoked, but only through a new Log entry. Correction that replaces the observed state creates a new Snapshot and links it to the corrected Snapshot or Log entry. Revocation without replacement creates a correction/revocation Log entry that excludes the original Snapshot from corrected query views while preserving the historical claim.

---

## UBU-D0101: Organization mode uses shared objects without intrinsic affect

**Status:** Accepted → DESIGN.md §25

Organization mode uses the shared UbU core object model except where fields or behavior are intrinsically personal-affect-specific. Objective, Preference, Task, Container, UniverseState, Snapshot, Plan, Calendar, Log, Identity, Relationship, Compartment, Automation Worker, External Event, External Reference, and deferred Association-related objects are available in organization mode to the depth required by the organization or project planning workflow.

Organization mode does not model intrinsic affect. In mode-specific schemas, intrinsic affect fields are absent. In shared implementation schemas that contain affect-capable fields for storage or migration convenience, those fields are disabled in organization mode and validators must reject organization-created intrinsic-affect values rather than silently treating them as unused.

Relationship objects may exist in organization mode without affect dimensions. Organization-mode Relationships represent structured UniverseState between Identities such as contributors, maintainers, workers, projects, vendors, integrations, or external organizations. Non-affect relationship-relevant project information belongs in ordinary UniverseState facts, External Events, Logs, External References, candidate AssociationAttestations, Objectives, Tasks, or risk reports. Organization mode must not claim that the organization has a private emotional state toward another Identity.

Organization-created Objectives, Preferences, and Tasks require `authority_source` metadata. In MVP this metadata may be coarse, but it must identify the authority path for the object, such as an admin-equivalent operator Identity, imported project policy, imported external event, Automation Worker submission, or human-approved import/projection action. The detailed vocabulary remains open in `UBU-Q0013`.

Before RBAC exists, organization-mode human users are represented as operator Identities with admin-equivalent authority over the instance. This is an MVP simplification only. Their actions must still be logged with actor Identity and provenance so future RBAC can be introduced without rewriting history.

Organization-mode instances may later receive limited signals from personal user-mode instances only through explicit user-controlled sharing, Identity-mediated authorization, Compartment/export checks, and clear provenance. Phase 1 does not implement personal-to-organization cross-instance sharing. Future designs should prefer structural signals such as availability, commitment status, task completion, or user-approved projection summaries rather than raw affect, private relationship state, or broad personal context.

---

## UBU-D0102: Phase 1 bootstrap and next-action UX stays narrow and inspectable

**Status:** Accepted → DESIGN.md §4.1

The Phase 1 bootstrap interview and next-action focus UX are the first user-facing proof that UbU is more than an internal planner, GitHub importer, or automation loop. They must stay narrow, inspectable, and honest about implemented capability.

The bootstrap interview should ask only the minimum useful set of questions needed to seed the current user, context, available work window, important Objectives, relevant constraints, and current or stale affect Snapshot. It should not imply broad personal-data ingestion, therapeutic authority, complete life modeling, or autonomous coaching unless those capabilities have been separately implemented and disclosed.

The next-action focus mode should present one recommended Task at a time with a clear explanation of why that Task matters now. The full Plan must remain inspectable. One-task focus is a cognitive-load reduction pattern, not permission to hide the planner, omit Calendar preview, or turn UbU into opaque automation.

This decision is compatible with the Compact Calendar planning architecture. The default Plan supplies the inspectable ordered context behind the next-action recommendation, while reactive recalculation keeps the recommendation responsive when reality diverges. The planning layer should support the narrow UX rather than expanding Phase 1 scope beyond the single-user dogfooding loop.

---

## UBU-D0107: Default Plan selection uses Plan probability over deterministic optimized Plans

**Status:** Accepted → DESIGN.md §15

Plan optimization and default Plan selection are distinct operations.

Each Plan represented on a Calendar is deterministic. Once materialized, it is a time-independent representation of predicted Actions/Tasks, similar in shape to a future-facing Log projection. A Plan may eventually include predicted sensor states and expected UniverseState transitions, but it does not internally evaluate its own probability after representation.

Each candidate Plan should be individually optimized for value while satisfying the constraints applicable to the modeled branch that produced it. These constraints include dependencies, deadlines, preconditions, Calendar Logic, affect constraints in `user_mode`, and user Preferences.

The planning algorithm models probabilistic parameters such as Task duration distributions, Task success or failure, external events, interruptions, availability changes, affect uncertainty, and sensor predictions. A particular modeled combination of those probabilistic inputs yields a deterministic candidate Plan. UbU may ascribe a **Plan probability** to that candidate Plan: the probability mass of the branch or parameter combination that yields it.

The default Plan is the deterministic candidate Plan, or one of multiple equal candidate Plans, with the highest Plan probability overall.

---

## UBU-D0108: Skeletonization and legitimization precede default Plan candidate search

**Status:** Accepted as provisional planning architecture → DESIGN.md §15

This decision supersedes the narrower framing that a DFS or DFS-like process directly produces the full default Plan as the conceptual foundation of Compact Calendar planning. DFS-like, BFS-like, greedy, local-search, GPU-parallel, or solver-backed methods may still be used as implementation techniques, but the planning architecture now begins with explicit skeletonization and legitimization.

The planner first creates a **skeleton Plan**. The skeleton Plan affixes Static Tasks, walks backward through dependency DAGs from terminal Static Tasks to dependency roots, and schedules prerequisites before dependents. It is a dependency-valid causal foundation, not an optimized or fully human-viable Plan.

The planner then performs **legitimization**: the process of adding the minimum constraints and support Tasks required to make the skeleton Plan plausibly executable by a real user. This includes affect, recovery, transition, rest, sustainability, and similar human-viability constraints.

The **legitimized skeleton Plan** is the minimum feasible baseline against which richer candidate Plans are compared. Candidate Plan generation, Plan probability assignment, optional Task filling, value optimization, and reactive branch construction occur after this baseline exists.

A short-horizon reactive layer, likely BFS-like or policy/repair based, remains necessary for near-term divergence involving duration variation, early completion, late completion, interruptions, external events, affect shifts, user overrides, and recalculation triggers. The provisional reactive branch horizon remains one hour, with a provisional short-horizon coverage target of `0.99` probability mass. These are configurable heuristics, not immutable design law.

---

## UBU-D0109: Early completion pulls the next valid Dynamic Task forward

**Status:** Accepted as provisional runtime behavior → DESIGN.md §15

When a Task completes earlier than expected, UbU should normally pull the next valid Dynamic Task forward to the current time, provided that dependencies, preconditions, affect constraints, location constraints, Calendar Logic, and user-visible policy allow it.

This supports conservative duration estimates. If the scheduler avoids overly optimistic durations, then early completion is easy to absorb: the next Dynamic Task can begin immediately instead of forcing a disruptive full replanning cycle.

Static Tasks keep fixed start times. If the next Static Task is not yet available and all predetermined Dynamic Tasks before it have been completed or blocked, UbU may offer gap-filling suggestions. The exact evergreen Task model remains open.

---

## UBU-D0110: Compact Calendar planning supports adaptive time granularity

**Status:** Accepted as provisional default policy → DESIGN.md §16

Compact Calendar planning should support configurable time deltas. The provisional defaults are:

- full-detail planning delta: `1 minute`;
- mobile moderate delta: `5 minutes`;
- mobile low-power or offline delta: `15 minutes`;
- reactive branch horizon: `1 hour`;
- short-horizon branch coverage target: `0.99` probability mass.

A one-minute delta aligns with common calendar behavior and should be available when resources permit. Mobile-only, low-power, or offline modes may use coarser deltas to preserve battery, reduce computation, and keep local recalculation responsive.

Known offline windows, such as flights or planned disconnection, should trigger preparatory precomputation when connectivity and compute resources are available. Unexpected offline operation should degrade gracefully to cached explicit state, local planning, coarser deltas, and reduced branch depth.

---

## UBU-D0111: External compute is optional and backend-agnostic for the open-core planning loop

**Status:** Accepted

Cloud or external compute may improve performance, granularity, analysis depth, stochastic branch coverage, and expensive legitimization, but it must not be a hidden mandatory dependency for the open-core planning loop.

The FOSS core should be indifferent to the execution backend. Planning work may run on a mobile device, a local desktop or laptop, a Phase 2 user-owned worker, a dedicated user-owned appliance, an UbU corporate hosted service, a third-party compatible provider, or a future privacy-preserving compute backend.

Execution-provider selection should treat GPU suitability as a first-class criterion. Many practical devices have much more parallel arithmetic capacity in GPU-like hardware than in CPU-bound exact solvers. UbU should therefore prefer hybrid algorithms in which CPU or conservative exact logic certifies skeleton validity, hard constraints, and explanations, while GPU-capable search, simulation, scoring, and learned-model inference evaluate large candidate sets, robustness, affect load, and premium cloud planning. GPU search may propose; exact or conservative validation must certify.

External execution must be mediated by explicit APIs, capability grants, Compartment policy, provenance, user-visible routing decisions, and privacy controls. PII stripping, encryption, redaction, compartment-aware payload minimization, and future privacy-preserving compute such as practical FHE-backed or comparable encrypted computation are strategic directions, but not Phase 1 guarantees unless implemented and disclosed. Cloud planning payloads should transmit the structured timing, dependency, value, constraint, criticality, and legitimacy information needed by the algorithm while stripping or compartmentalizing unnecessary personal detail. Compact Calendar results should return small references, IDs, decision envelopes, explanations, and repair metadata rather than raw personal context where possible.

---

## UBU-D0112: Cloud LLM integration is provider-neutral, local-first, and policy-routed

**Status:** Accepted

UbU's premier LLM execution path remains local-first use of a user-controlled local model provider such as `ollama`, but cloud LLMs are accepted as optional execution providers when the user, Compartment policy, cost policy, and task sensitivity allow them.

The core architecture should model LLM execution through a provider-neutral routing layer rather than treating a local `ollama` call as the only conceptual interface. Supported or planned provider classes include local model providers, user-configured BYOK cloud APIs, optional UbUCorp managed gateways, user-owned remote workers, and future compatible third-party providers.

Cloud LLM execution is not semantically equivalent to local execution. It crosses an external trust boundary and must be governed by Compartment policy, context minimization, redaction where appropriate, cost controls, provenance, visible routing decisions, and output validation. Cloud LLM output remains advisory unless transformed into canonical UbU state through explicit accepted objects and Logs.

UbUCorp may offer managed hosted inference, model routing, support, integrations, enterprise controls, and commercial provider relationships. That convenience service must not become a hidden dependency of the FOSS core, the open-core planning loop, or the UbU protocol. A commercial exclusive provider relationship for UbUCorp's hosted service is acceptable only if the core remains local-capable, provider-neutral, BYOK-capable, and self-hostable.

---

## UBU-D0113: Associations are Identity-scoped perceived coordination structures

**Status:** Accepted → DESIGN.md §19

UbU adopts **Association** as the canonical term for the earlier social-formation concept. An Association is an Identity-scoped model of an emergent group-like coordination pattern. It may represent a friend group, party, amateur league, FOSS project, contributor crew, skill network, marketplace, nonprofit, company, DAO-like project, or other formal or informal group.

Associations are not assumed to have globally objective membership, authority, boundaries, or interpersonal identity. By default, an Association exists as a local, perspective-bound model perceived by an Identity and supported by evidence. Other Identities may maintain overlapping but non-identical models of what they call the same Association.

UbU should represent Association facts through local perception, AssociationAttestations, Relationships, shared Objectives, commitments, norms, Logs, and External References. Legal entities, corporate filing numbers, GitHub organizations, websites, event pages, chat rooms, contracts, payment addresses, and other institutional or external records are External References or evidence, not the complete social reality of the Association.

A future SharedAssociationDescriptor may provide a signed or reviewable shared artifact that multiple Identities can reference, but it is not a God's-eye truth object. It is a coordination artifact that can be accepted, disputed, superseded, or interpreted differently by different Identities.

---

## UBU-D0114: Organizational introspection is a first-class UbU feature

**Status:** Accepted

UbU should treat organizational introspection as a first-class feature, not merely as a side effect of project management or future Association modeling.

Organizational introspection is UbU's ability to analyze evidence-bearing records from an Association, such as documentation, meeting notes, issue trackers, pull requests, governance discussions, chat logs, board minutes, public Discord or IRC history, outreach notes, and other permitted records, then generate reviewable, provenance-backed AssociationAttestations and feedback questions about the Association's actual behavior.

The goal is to help an Association inspect whether its real priorities, overrides, undocumented roles, decision paths, commitments, and work patterns are consistent with its declared mission, values, objectives, and public claims. Generated claims are candidate attestations with evidence, confidence, provenance, review status, and disclosure policy. They are not authoritative social truth.

This feature mirrors personal UbU introspection. For a person, UbU asks whether actual behavior matches stated values and Objectives. For an Association, UbU asks whether actual work, decisions, and resource allocation match declared mission and commitments.

---

## UBU-D0115: EthConf outreach uses one core pitch plus lightweight audience lanes

**Status:** Accepted

EthConf outreach should not splinter into many separate campaigns. The project should use one core UbU thesis with lightweight audience variants for FOSS contributors, prototype funders, general EthConf attendees, and a modest cypherpunk/privacy-builder lane.

The shared thesis is that UbU is local-first, user-sovereign AI planning and coordination infrastructure. It can run locally for privacy, use optional cloud LLM execution when policy allows, and eventually help Associations coordinate through explicit Identities, commitments, evidence, and bounded disclosure.

The cypherpunk/privacy lane should frame **Skill Barter marketplace** as a future specialization of Association modeling: lawful, privacy-preserving skill barter and skilled-work coordination through pseudonymous Identities, scoped work agreements, reputation without unnecessary doxxing, commitments, and user-chosen settlement references where lawful. It must not be framed as an illicit marketplace, sanctions-evasion tool, tax-evasion tool, or token-first product.

---

## UBU-D0116: EthConf outreach dogfoods Association formation and organizational introspection

**Status:** Accepted

UbU's EthConf outreach is itself an Association-forming workflow. The project owner is attempting to form an ad hoc Association around the UbU project through conversations with contributors, reviewers, funders, workflow informants, privacy/cypherpunk builders, and possible design partners.

This outreach should be treated as dogfooding, not mere marketing. Notes, follow-ups, contact classifications, public artifacts, private/redacted observations, and subsequent commitments can be modeled as Objectives, Tasks, Logs, Relationships, External Events, External References, and candidate AssociationAttestations.

The outreach process should later be submitted to LLM-assisted or manually reviewed organizational introspection. UbU should ask whether the actual outreach behavior proves that the project pursued its stated goals: recruiting serious contributors, pressure-testing the design, finding workflow examples, identifying compatible funding, and preserving the privacy-first self-governance mission.

---

## UBU-D0117: Context-rich cross-user messaging is a premier Phase 3 feature

**Status:** Accepted → DESIGN.md §20

Phase 3 minimal multi-user coordination should include a limited but meaningful form of cross-user contextual messaging. This should be treated as a premier Phase 3 feature, not merely as an incidental transport detail.

A UbU message is not only flat text. It is a communication event that may carry structured planning context. Even a minimal Phase 3 message envelope can help the receiving UbU instance decide whether a message should interrupt the current Plan, be deferred to a later review window, become a Task, update an Objective, or remain ordinary communication history.

A minimal Phase 3 **Message Context Envelope** should include only fields that are safe, useful, and policy-permitted, such as:

- sender and receiver Identity references or external references;
- source system or transport;
- raw or human-readable message body;
- message kind, such as request, status update, question, commitment, blocker, reminder, or FYI;
- topic or associated Objective/Task/Association references when disclosure policy allows them;
- requested response kind and any explicit deadline;
- priority and interrupt recommendation;

---

## UBU-D0118: Legacy messaging adapters may upgrade flat messages into UbU contextual messages

**Status:** Accepted as product and interoperability direction → DESIGN.md §20

UbU should be able to ingest and, where permitted, send through legacy communication systems such as WhatsApp, SMS, email, Discord, IRC, Slack, Matrix, or similar systems.

When only one side uses UbU, the legacy message remains a flat external input. UbU may scan, classify, summarize, and convert it into local candidate Tasks, Events, Logs, Relationship observations, or Association evidence, subject to user permission and integration policy.

When both sides use UbU, the two instances should be able to translate the legacy exchange into a UbU-native contextual message format. Depending on the transport and policy, this may happen through a side channel, an attached envelope, an agreed encoding, a linkable External Reference, or another adapter-specific mechanism. The raw legacy text remains the user-visible message; the UbU envelope supplies structured context.

This creates a practical bridge from legacy systems into richer UbU-to-UbU communication without requiring the rest of the world to abandon existing messaging platforms first.

---

## UBU-D0119: Structured message extraction is a bounded LLM-assisted normalization layer

**Status:** Accepted as architectural direction → DESIGN.md §20

UbU will ingest large volumes of unstructured direct-message and group-chat text from legacy systems. A dedicated **Message Context Extractor** should normalize those inputs into strict UbU JSON structures.

The extractor should take raw message text plus available metadata, such as source system, channel type, channel purpose, sender, receiver, Identity mapping, Association mapping, timestamp, thread context, Relationship context, and Compartment policy. It should output bounded structures describing message kind, topic, priority, interrupt level, candidate Tasks, Objective links, assumptions, ambiguities, actionability, response expectation, confidence, and provenance.

The extractor is not the canonical planner and must not silently mutate canonical state. Its outputs are candidate interpretations that pass through schema validation, provenance marking, confidence scoring, repair loops, and user or policy acceptance where required.

Phase 3 and early post-MVP implementations should begin with general LLMs constrained by strict schemas, grammar-constrained output where practical, validation, and repair. Fine-tuned, distilled, or adapter-trained extractor models may become attractive after UbU has stable schemas and enough corrected examples. Training a new foundation model from scratch is not currently justified.

---

## UBU-D0120: Personalized voice/TTS descriptors are optional consent-gated communication metadata

**Status:** Accepted as future product direction, not Phase 1 scope

UbU may eventually support optional voice or pronunciation descriptors so that a receiving UbU instance can render text messages using text-to-speech that approximates the sender's usual voice, pronunciation, cadence, or expressive style.

This can be viewed as a communication-compression feature: instead of sending a noisy audio recording, a user may send text plus a voice descriptor or voice-profile reference, allowing the receiver's device to synthesize a locally rendered spoken version. The same idea may also help accessibility, hands-free interaction, language learning, and more expressive asynchronous updates.

Voice data is sensitive. A voice descriptor or voice profile must be opt-in, revocable where practical, policy-governed, and clearly separated from authentication. UbU should not treat a synthesized voice as proof that a human actually spoke the words. Implementations should consider visible disclosure, watermarking or provenance markers, anti-impersonation controls, and restrictions on cross-context reuse.

---

## UBU-D0121: Messaging interoperability can support value-led viral outreach without coercive growth loops

**Status:** Accepted as outreach/product direction

Legacy messaging integration can create a natural adoption path. A UbU user interacting with a non-UbU contact may receive immediate value from local extraction, prioritization, task creation, and planning integration. If both people install UbU, they can exchange richer structured context and reduce ambiguity, missed requests, and priority confusion.

This is a plausible viral outreach mechanism: the product becomes more useful when counterparties also use it. However, UbU should not use manipulative dark patterns, spammy invitations, forced signatures, or guilt-based prompts. The adoption message should be value-led: richer coordination, clearer priority, less lost context, and better respect for both users' time.

---

## UBU-D0122: Greedy mean-duration planning is a benchmark, not the canonical planner

**Status:** Accepted as baseline algorithm policy → DESIGN.md §15

UbU may define a deliberately unintelligent greedy baseline planner for comparison. The baseline inserts Static Tasks first, places modeled External Events at their expected mean-point start time where applicable, ranks Dynamic Tasks by temporary value-per-minute from the Preference DAG divided by expected mean duration, and fills time from the earliest available slot forward while checking only local viability. It does not backtrack and should be expected to be inefficient, brittle, and strategically myopic.

This baseline is useful because later planners can be evaluated against a simple deterministic reference: total value, missed deadlines, affect burden, dependency failures, fragility under interruption, and explanation quality should improve over the baseline.

---

## UBU-D0123: Calendar planning begins from explicit UniverseState assumptions

**Status:** Accepted as provisional planning-model requirement → DESIGN.md §15

Every Calendar planning run must begin from an initial UniverseState. For MVP, the preferred case is a deterministic initial UniverseState in which planner-relevant starting facts are known, assumed, or explicitly unresolved. A future probabilistic initial UniverseState may assign probabilities to known possible starting states, but that mode is more complex and likely beyond MVP.

Skeleton Plan generation must check whether dependencies are already satisfied in the initial UniverseState before inserting prerequisite Tasks. A dependency does not automatically mean that a new Task should be scheduled; it means that a required state must be true before the dependent Task begins.

If the planner cannot create a viable skeleton Plan, normal planning should halt and UbU should immediately present a diagnostic explanation. The user should see the failed dependency, conflicting Static Task, impossible timing, cyclic dependency, missing state, or unavailable resource, plus selectable alternatives where possible.

---

## UBU-D0124: Legitimization makes skeleton Plans human-viable before optional optimization

**Status:** Accepted as provisional planning architecture → DESIGN.md §15

**Legitimization** is the planning phase that takes a skeleton Plan and adds the minimum additional constraints and support Tasks required to make the Plan plausibly executable by the user. It converts a dependency-valid but potentially unrealistic skeleton Plan into a minimally human-viable Plan.

Legitimization may add or enforce affect constraints, recovery Tasks, breaks, meals, rest, sleep, transition buffers, setup/teardown time, context-switch limits, motivation constraints, and other human sustainability requirements. It is distinct from later optimization: it asks whether the Plan could be realistically performed, not whether all available value has been added.

The legitimized skeleton Plan is the baseline feasible Plan. Optional Dynamic Tasks, gap-filling work, and higher-value candidate Plans should be compared against it.

If full legitimization is cheap, it can be used as a frequent validity oracle while adding and removing optional Tasks. If full legitimization is expensive, UbU needs semi-legitimization heuristics such as affect-budget estimates, slack preservation, dependency-fragility scoring, user-mode compatibility checks, local repair checks, and legitimacy-delta estimates before invoking full legitimization on finalist candidates.

---

## UBU-D0125: Compact Calendar search is GPU-aware hybrid planning

**Status:** Accepted as implementation direction → DESIGN.md §16

UbU should not assume that the central planning engine is a CPU-heavy exact solver. Solver/library selection should treat GPU suitability as a first-class criterion across mobile devices, laptops/desktops, user-owned workers, and cloud providers.

The preferred architecture is hybrid. CPU or conservative exact logic handles dependency graph traversal, skeleton validity, hard constraints, contradiction diagnosis, final validation, and explanation. GPU-capable methods handle large candidate expansion, stochastic scenario simulation, affect scoring, robustness scoring, learned-model inference, and premium cloud planning.

Existing CPU-oriented solvers such as CP-SAT, SMT/MaxSMT, or local-search systems may remain useful for prototypes, exact finalist validation, contradiction explanation, and desktop/server experiments. They should not become the only planning path if GPU-friendly search, simulation, or scoring better matches available compute.

---

## UBU-D0126: Mobile planning is real-time stewardship, not full global optimization

**Status:** Accepted as mobile planning UX direction

A mobile device has the least computational headroom but the highest demand for immediate user-facing adaptation. Mobile UbU therefore should not be designed as the full global planner. Its role is real-time stewardship of Plan legitimacy, user agency, and next-action clarity.

Beyond short-horizon BFS precomputation, compact Calendars should support decision envelopes, protected/flexible/disposable region metadata, last-legitimate-Plan repair, precomputed repair recipes, fast local policy selection, progressive planning feedback, cached explanations, conflict severity levels, next-best-action mode, opportunistic idle/charging computation, optional remote assist without dependency, and uncertainty-aware UI.

The MVP-friendly subset is narrower: Task criticality, last legitimate Plan storage, simple repair rules, conflict severity levels, cached explanations, next-best-action mode, and basic decision envelopes.

---

## UBU-D0127: VoxPopuli is an optional EthConf demo, not a Phase 1 replacement

**Status:** Accepted as optional outreach/demo concept

UbU may demonstrate an optional **VoxPopuli** flow at EthConf: before structured bootstrap questions, a user speaks freely about what they wish would happen, what feels disorganized, or what kind of planning help they want. An LLM-assisted extractor converts that natural-language input into candidate Objectives, Tasks, constraints, preferences, and planning assumptions for the user to inspect, correct, accept, or reject.

The value of this flow is demonstration and trust-building. It shows that LLMs can help turn abstract human concerns into explicit structured planning material, while UbU remains the inspectable planning system that consumes accepted structures.

This must not override the Phase 1 narrow bootstrap requirement. VoxPopuli is optional, experimental, and useful for EthConf/public demonstrations only if it does not displace higher-priority dogfooding, contributor, or funder deliverables.

---

## UBU-D0128: Planning horizons may exceed visible Calendar windows and should front-load fragile prerequisites

**Status:** Accepted as provisional planning architecture → DESIGN.md §15

The user-visible Calendar window and the internal planning horizon do not have to be identical. If the user asks for a one-day Calendar, UbU may need to reason beyond that visible window to avoid cutting dependency chains, Techniques, deadlines, or preparation sequences at the boundary.

Within reasonable detailed planning windows, such as one day to roughly one week, UbU may use a bias toward completing fragile prerequisite work as early as reasonably viable. This is intended to reduce last-minute impossible choices, especially when later interruptions, affect deterioration, external events, or user overrides would otherwise threaten a tight dependency chain.

This early-preparation bias is not unlimited. Beyond a reasonable detailed horizon, preparation may be too premature or speculative. Long-horizon preference and temporal-discounting questions remain separate from short-horizon operational planning. For short operational windows, UbU may treat time discounting as negligible by default, while still respecting explicit user Preferences and current affect.

---

## UBU-D0129: Phase 1 bootstrap and next-action UX uses explicit object-backed minimum loop

**Status:** Accepted → DESIGN.md §4

The minimum Phase 1 bootstrap interview asks only enough to recommend one useful dogfooding Task from explicit state. The required questions are:

1. `What are you trying to move forward right now?`
2. `Which project context should UbU use for this first session?`
3. `How much usable time do you have for the next work window?`
4. `Is there a deadline, meeting, release target, or external event that changes what matters today?`
5. `What work should UbU consider first?`
6. `What is already blocked, unavailable, or not worth recommending right now?`
7. `How are your energy, stress, and mood right now?`
8. `When choosing between useful work, what should UbU favor today?`

The answers map into existing Phase 1 objects. Project context, availability, deadlines, constraints, fixture/import source, and hard blockers become UniverseState facts, External Events, Tasks, External References, and Log entries as appropriate. Affect answers become a user-declared Snapshot; skipped or stale affect data creates or prioritizes an affect-collection Task. Initial work answers seed Objectives and Tasks. Tradeoff answers become Preferences only when UbU presents an explicit Preference statement and the user accepts it; otherwise they remain Log notes, Objective annotations, or noncanonical preview/review evidence.

---

## UBU-D0130: UniverseState mutations use dotted targets and envelope-level provenance

**Status:** Accepted → DESIGN.md §11.3

MVP UniverseState mutation items use dotted string targets. The first target segment names the UniverseState collection: `facts`, `numeric_values`, `set_memberships`, or `event_markers`. Remaining segments form the lightweight namespaced key within that collection. Examples include `facts.github.issue.14.pipeline_state`, `numeric_values.affect.energy`, `set_memberships.github.issue.14.labels`, and `event_markers.relationship.rel_123.interactions`.

A mutation item has required `operation` and `target` fields. `payload` is required for all operations except `clear_fact`. `note` is optional for human-readable context. Mutation items do not carry item-level `confidence`, `source`, or `provenance` in MVP; those belong on the containing Task effect, Snapshot, Log entry, worker mutation request, External Reference, import artifact, or projection envelope.

Payload rules are operation-specific. `set_fact` accepts any JSON-compatible value. `clear_fact` has no payload. `increment_numeric` and `decrement_numeric` require numeric delta payloads. `add_membership` and `remove_membership` require JSON scalar member payloads, usually strings. `append_event_marker` requires a JSON object payload representing the lightweight marker.

Mutation lists are unconditional once the containing Task effect succeeds. Per-item conditions are not part of the MVP mutation schema; conditional behavior belongs in Task preconditions, effect success probability, planner branching, or worker/request validation. Implementations should validate the full mutation list before application and apply valid items in list order.

Mutation targets may include affect UniverseState keys in `user_mode`, subject to Snapshot precedence and user sovereignty rules. Organization-mode and worker-mode validators must reject intrinsic-affect mutations. Mutation targets may include Relationship-relevant UniverseState keys, including relationship interaction markers and maintenance facts, but they must respect Compartment policy, mode rules, provenance/logging envelopes, and user-acceptance requirements for private or inferred relationship claims. Task effects, workers, imports, and LLM-assisted flows must not silently overwrite user-declared private affect or Relationship truths.

---

## UBU-D0131: Task preconditions use recursive all_of/any_of predicates

**Status:** Accepted → DESIGN.md §10.1

MVP Task preconditions use a recursive boolean object with `all_of` and `any_of` arrays for simple AND/OR composition. A precondition node may be a group node or a leaf predicate. A group node contains `all_of` or `any_of`, each holding one or more precondition nodes. A leaf predicate contains `target`, `predicate`, and optional `expected`.

Precondition targets use the same dotted UniverseState target convention accepted for mutations. The first segment names the UniverseState collection: `facts`, `numeric_values`, `set_memberships`, or `event_markers`. Remaining segments form the lightweight namespaced key inside that collection.

MVP predicates are `equals`, `member_of`, and `absent`. `equals` compares the target value to a JSON-compatible `expected` value. `member_of` checks whether the JSON scalar `expected` value is present in the target set membership. `absent` checks that the target is not present or has been cleared, and does not use `expected`. Numeric comparisons such as greater-than, less-than, ranges, thresholds, and arithmetic expressions are not in MVP.

Preconditions may reference `event_markers` for deterministic marker presence or equality checks. Preconditions may reference affect-related UniverseState keys in `user_mode`, subject to Snapshot precedence and user sovereignty rules; organization-mode and worker-mode validators must reject intrinsic-affect preconditions. Preconditions may reference Relationship-relevant UniverseState keys, including relationship interaction markers and maintenance facts, but private or inferred relationship claims remain subject to Compartment policy, mode rules, provenance/logging envelopes, and user acceptance where required.

A failed precondition makes the Task blocked for planning and execution. It does not make the Task invalid. `unschedulable` is a derived planner result when an otherwise valid Task cannot be placed in the current Calendar scope while satisfying preconditions and Calendar Logic. `invalid` is reserved for malformed Tasks, malformed precondition schema, forbidden targets, or canonical-state contradictions. Unknown, unavailable, or partially modeled preconditions are treated as absent in MVP unless the user or importer explicitly records a deterministic predicate.

---

## UBU-D0132: Realtime multimodal LLMs are optional interaction backends, not authoritative planners

**Status:** Accepted → DESIGN.md §21

Realtime multimodal LLMs may power live voice, video, interruption detection, meeting capture, discovery mode, pronunciation or activity feedback, and short-horizon task monitoring. They are interaction backends and perceptual/extraction aids, not the authoritative UbU planner.

Realtime model output must enter UbU as structured, provenance-bearing candidate updates. Examples include `ObservedEvent`, `UserInterruption`, `TaskProgressDelta`, `AffectSignal`, `PlanDeviation`, `ExternalConditionChange`, `ClarificationQuestion`, `WorkItemCandidate`, `LogEntryCandidate`, and `AssociationAttestationCandidate`.

UbU distinguishes **model-time awareness** from **planner-time semantics**. A realtime model may notice elapsed time, silence, overlapping speech, interruption, or a changing audiovisual scene. UbU's planner remains responsible for deciding whether that observation changes Task state, Calendar validity, Plan legitimacy, Logs, Objectives, or recalculation triggers.

Realtime operation requires explicit user-visible modes, such as passive/off, text-only, voice session, active discovery mode, meeting/logging mode, high-privacy local-only mode, and cloud-assisted mode. Continuous capture must not become covert surveillance or an implied authorization to mutate canonical state.

---

## UBU-D0133: LLMs are replaceable cognitive backends and structured outputs are candidate updates

**Status:** Accepted → DESIGN.md §21

UbU should treat LLMs as replaceable cognitive backends. They may interpret, summarize, classify, translate, transcribe, propose structures, critique plans, call tools, generate UI drafts, and assist extraction. They do not own canonical truth, canonical value, canonical memory, or canonical planning state.

Strict schemas and structured outputs are necessary but not sufficient. A JSON object that satisfies a schema may still be semantically wrong, policy-invalid, Compartment-invalid, dependency-invalid, affect-invalid, stale, maliciously influenced, or unsupported by evidence.

The admission pipeline is therefore:

```text
LLM output
  -> schema validation
  -> semantic validation
  -> policy and Compartment validation
  -> provenance attachment
  -> conflict detection

---

## UBU-D0134: Context assembly is a privacy-relevant governed act

**Status:** Accepted

As LLM context windows grow, UbU must not assume that sending more context is automatically better. Context assembly is itself a privacy-relevant action that may cross Identity, Compartment, Association, provider, and retention boundaries.

A future `ContextBundle` should describe why context was assembled and what it exposed:

```text
ContextBundle
  - purpose
  - source object references
  - Compartments included
  - Identities exposed
  - Association references exposed
  - provider or model destination

---

## UBU-D0135: UbU should be both an MCP-style client and an MCP-style server with capability boundaries

**Status:** Accepted → DESIGN.md §21

UbU should support MCP-style integration boundaries in both directions.

As a client, UbU can connect to external tools and data sources such as GitHub, calendars, email, local scripts, file stores, home automation, and model/agent services.

As a server, UbU may expose narrow, scoped, reviewable affordances to outside agents, such as:

- create candidate Task;
- read a current Plan summary;
- append or submit a Log candidate;
- request user clarification;
- submit a proposed Plan repair;
- query Objective status;
- submit a candidate AssociationAttestation;

---

## UBU-D0136: Delegated agency is first-class in planning

**Status:** Accepted → DESIGN.md §21

UbU Tasks may be performed by the user, a local agent, a cloud agent, a tool, another human Identity, an Association, or a delegated coordinator. The executor is part of the planning problem and must not be hidden inside an opaque automation step.

A delegated Task should distinguish:

- Task intent;
- authorization source;
- executor;
- observer;
- reviewer;
- granted authority;
- expected output;
- completion evidence;

---

## UBU-D0137: Delegation Substrate is the near-term model for preparing Task delegation

**Status:** Accepted → DESIGN.md §21

Use **Delegation Substrate** as the near-term tool/model for preparing Task delegation. It is not the full Skill Barter marketplace. It is the explicit representation that makes a Task ready to be performed, reviewed, or handed off by clarifying purpose, executor, authority, expected output, evidence, constraints, and completion criteria.

The Delegation Substrate is also valuable for Tasks the user intends to perform solo. Formalizing the same fields can act as a self-reminder of how and why the Task needs to be performed, what evidence would show completion, what authority is being used, and what constraints matter.

A minimum Delegation Substrate packet may include:

```text
DelegationPacket
  - Task or Objective reference
  - intended executor type
  - purpose / why this matters
  - expected output
  - authority granted or self-authority note

---

## UBU-D0138: General Contractor is a first-class delegated coordination role

**Status:** Accepted → DESIGN.md §21

A **General Contractor** is an Identity, Agent, or Association delegated authority to coordinate multiple subordinate executors in order to satisfy an Objective or complete a Container of Tasks, subject to explicit authority, budget, privacy, review, evidence, and escalation constraints.

This role is distinct from an ordinary executor. A General Contractor may decompose work, assign subtasks, supervise human or agent executors, collect evidence, report status, and submit candidate updates. UbU must still model the General Contractor's authority as bounded and reviewable.

---

## UBU-D0139: Skill Barter marketplace is an outreach and future-market direction, not a Phase 1 marketplace commitment

**Status:** Accepted

The **Skill Barter marketplace** should be presented as a future marketplace direction and EthConf NYC outreach hook, especially for cypherpunk and privacy-oriented audiences. It signals that UbU preserves much of the original cryptocurrency ethos: voluntary coordination, sovereign identity, privacy, open markets, FOSS development, pseudonymous capability, and user-controlled settlement references where lawful.

Skill Barter can attract younger developers with drive, time, and interest in FOSS contribution by showing that UbU is not merely another productivity app. It is a coordination substrate that could eventually support privacy-preserving skilled-work exchange among human and agentic executors.

A mature Skill Barter marketplace would naturally create demand for FHE, ZK, secure compute, private reputation, private escrow-like commitments, selective disclosure, and other high-privacy technologies compatible with the future Ethereum ecosystem. This should remain an architectural and outreach signal, not a token-first or speculation-first positioning.

---

## UBU-D0140: Computer-use and background agents require authority, audit, rollback, and prompt-injection handling

**Status:** Accepted → DESIGN.md §21

Computer-use agents and background agents are high-risk external actors, not ordinary pure functions. They may operate browsers, accounts, files, credentials, APIs, and external systems where prompt injection, irreversible side effects, disclosure, or stale assumptions can cause harm.

Future `AgentAction` or `BackgroundProcess` models should record:

```text
AgentAction / BackgroundProcess
  - executor Identity or agent reference
  - trigger or schedule
  - authority scope
  - credentials or integrations used
  - Compartment and Identity scope
  - external surface touched

---

## UBU-D0141: Local/on-device inference is a first-class execution tier

**Status:** Accepted

UbU should treat local and on-device inference as a first-class execution tier, not merely a fallback for unavailable cloud models. Local inference supports privacy, offline use, lower marginal cost, responsiveness, and user sovereignty.

A provisional execution hierarchy is:

```text
Tier 0: deterministic local code
Tier 1: small local/on-device model
Tier 2: user-owned desktop or worker model
Tier 3: user-selected cloud model via BYOK
Tier 4: optional UbUCorp-managed hosted service
```

Local models may handle first-pass extraction, privacy classification, simple message triage, candidate Task/Log creation, affect journaling summaries, and compartment routing. Cloud or larger models may be reserved for high-complexity planning, deep design review, organizational introspection over large archives, difficult ambiguity resolution, and expensive multimodal reasoning when policy allows.

---

## UBU-D0142: UbU UX should evolve into a state-transition cockpit

**Status:** Accepted

UbU should not be framed as chat plus calendar. The long-term UX should become a **state-transition cockpit**: a user interface that presents the current WorkItem, relevant state, candidate transitions, constraints, explanations, evidence, and review controls.

The Phase 1 one-next-Task UX remains the narrow proof. Later interfaces may adapt the visible control surface to the current state transition:

- reply to a message;
- choose or start the next Task;
- review a Log discrepancy;
- repair a Plan conflict;
- inspect a Delegation Substrate packet;
- approve or reject an agent action;
- review an AssociationAttestation;
- run organizational introspection;

---

## UBU-D0143: Community-specific EthConf briefs are derived presentation layers

**Status:** Accepted

UbU should maintain a small set of community-specific derived documents for EthConf and adjacent outreach when the audience has a materially different trust barrier, motivation, or call to action.

The durable derived audience documents are:

- `README.md` for upcoming technical contributors and technically serious readers who need the project entry point;
- `OUTREACH.md` for FOSS maintainers and general software engineers who may become technical contributors;
- `PM_BRIEF.md` for project leads, technical PMs, protocol leads, release coordinators, and people coordinating autonomous contributors;
- `FUNDER_BRIEF.md` for grantmakers, sponsors, hackathon judges, aligned funders, and prototype funders;
- `SOVEREIGN_COORDINATION.md` for cypherpunks, privacy engineers, Ethereum privacy builders, FHE/ZK/secure-compute researchers, and sovereign-coordination audiences;
- `ORG_INTROSPECTION_BRIEF.md` for mission-driven projects, FOSS maintainers, nonprofits, DAOs, foundations, and teams that want evidence-backed mission alignment.

These files are presentation layers. They must not introduce new design authority. Their source of truth remains `DESIGN.md`, `DECISIONS.md`, and `OPEN_QUESTIONS.md`.

---

## UBU-D0144: Inter-instance protocol is a generic envelope family with worker API profiles

**Status:** Accepted

UbU should have one generic inter-instance protocol family for communication between UbU instances, worker-mode instances, and future compatible peers. The protocol is a family of typed envelopes with shared authority, provenance, Compartment, identity, idempotency, versioning, and review semantics, not one undifferentiated endpoint that treats every payload as the same kind of action.

The protocol should eventually support at least these payload families:

- contextual messages and Message Context Envelopes;
- worker assignments and assignment responses;
- status updates and check-ins;
- mutation requests and projection requests;
- capability grant issuance, acknowledgement, rotation, and revocation;
- External Event submission;
- Snapshot or Log candidate submission where authorized;
- recalculation requests;

---

## UBU-D0145: Phase 1 risk reports are derived artifacts, not canonical risk objects

**Status:** Accepted → DESIGN.md §27

Risk reports in MVP are derived, recalculable analyses over Calendars, Plans, Tasks, Logs, Snapshots, External Events, worker status, External References, and compact Calendar metadata. Risk remains reportable state, not a first-class canonical object.

The Phase 1 MVP risk-report set is:

- `p90_completion_time`;
- `critical_path`;
- `deadline_miss_probability`;
- `affect_constraint_violation_probability`;
- `low_compact_calendar_coverage_warning`;
- `dependency_fragility`;
- `worker_or_automation_bottleneck`;
- `stale_affect_warning`;
- `destructive_pressure_warning`;
- `post_plan_depletion_warning`.

Additional findings may be computed when source data is already available, but they are not required Phase 1 reports: relationship-maintenance neglect, preference uncertainty, repeated override or deviation patterns, motivation mismatch, stale or missing discovery-mode reconciliation, and overdue Calendar preview or Log review.

Risk reports are computed on demand from the current Calendar or Plan. Implementations may cache a risk-report artifact with a Calendar, Plan, run artifact, release package, or UI view for auditability and performance. Cached reports are derived state and must be invalidated or marked stale when relevant Tasks, Logs, Snapshots, External Events, worker status, compact Calendar coverage, or recalculation triggers change.

Automation Workers may compute or refresh risk-report artifacts only through explicit capability grants. Worker-produced risk reports are advisory until admitted by the canonical instance. Workers may submit report artifacts, refresh requests, mutation requests, or projection requests according to authority; they do not create canonical Risk objects or directly mutate Tasks.

Risk reports may recommend follow-up Tasks or submit Task candidates through the normal request/review path, but they must not silently create canonical Tasks in MVP. Typical follow-ups include clarification, affect collection, dependency repair, worker retry or escalation, Calendar regeneration, Log review, Calendar preview, and GitHub projection or reconciliation.

Risk reporting is part of UbU-runs-UbU release ceremonies. Release readiness and Release Outreach Pipeline packages should include deadline risk, critical path, dependency fragility, worker or automation bottleneck, and low coverage warnings when inputs exist, while respecting Compartment, Identity, export, and public-projection boundaries.

UbU's PERT-superiority demonstration should be precise: Phase 1 should not claim to replace every PERT use. It should show that UbU handles the planning dimensions PERT leaves out: explicit preconditions and UniverseState, affect constraints, worker status, compact Calendar coverage, recalculation from Logs and Snapshots, and actionable Plan repair or next-Task recommendations.

---

## UBU-D0146: Phase 1 recalculation triggers use logged trigger records

**Status:** Accepted → DESIGN.md §28

Recalculation triggers are explicit event-like records that tell UbU when a Calendar, Plan, risk-report cache, explanation cache, or next-action recommendation may no longer reflect the current modeled state. A trigger is not a separate canonical domain mutation by itself; it references the Log entry, Snapshot, External Event, worker request, or clock condition that changed the planner's inputs.

The Phase 1 trigger kinds are:

- `task_completed`;
- `task_failed`;
- `task_moot`;
- `observed_snapshot`;
- `affect_confidence_decay`;
- `external_event`;
- `github_update`;
- `user_override`;
- `calendar_preview_due`;
- `log_review_due`;
- `discovery_mode_reconciliation_due`;
- `elapsed_time`;
- `low_compact_calendar_coverage`;
- `worker_request`.

Phase 1 user feedback controls such as snooze, reject, decompose, and override should be represented through ordinary Task, Log, or user-override payloads rather than separate trigger kinds unless implementation evidence shows that a distinct trigger kind is required.

Phase 2 trigger kinds are limited to sync and personal-worker conditions that can invalidate a local Calendar: `sync_state_changed`, `device_reconnected`, `remote_worker_status_changed`, and `offline_window_changed`. These belong with multi-device local-first sync and user-owned worker coordination.

Post-MVP trigger families include native cross-user message arrival, AssociationAttestation review, broad legacy-message ingestion, background AgentAction policy events, external compute budget or provider changes, and other high-level agentic or multi-user coordination events.

Accepted Phase 1 triggers are logged using the existing Log event type `recalculation_triggered`. The event payload should include `trigger_kind`, `scope`, `target_refs`, `source_event_refs`, `requested_action`, `batch_key`, `reason`, and optional `idempotency_key`. `requested_action` is either `recalculate_now` or `mark_calendar_stale`.

Automation Workers cannot directly create canonical triggers. A worker with the `recalculation.request` capability may submit a recalculation request. The canonical instance validates capability, target scope, Compartment/export policy, source evidence, and idempotency before recording an accepted `recalculation_triggered` Log entry or a rejected worker request/mutation Log entry.

Triggers may be batched when they share the same Calendar or Plan scope and no trigger in the batch requires user-visible immediate repair before the batch window closes. Batching must preserve each source reference and reason. A short event-loop batch is acceptable for multiple imported GitHub events, worker updates, or stale markers.

Immediate recalculation is required when the trigger can change the current or next recommended Task, hard feasibility, Task lifecycle state, dependency or precondition truth, Objective status, affect legitimacy, worker assignment needed for current work, or compact Calendar coverage below the configured minimum.

Default Phase 1 immediate triggers are:

- `task_completed`, `task_failed`, and `task_moot` for current, planned, dependency-relevant, or Objective-relevant Tasks;
- `user_override`;
- planner-relevant `observed_snapshot`;
- planner-relevant `external_event`;
- planner-relevant `github_update`;
- `low_compact_calendar_coverage`;
- urgent accepted `worker_request`.

Stale marking is sufficient when the trigger only means cached Plans, Calendars, explanations, or reports must be refreshed before later reliance. Default stale-only triggers are:

- `calendar_preview_due`;
- `log_review_due`;
- `discovery_mode_reconciliation_due`;
- `elapsed_time` outside the reactive repair envelope;
- `affect_confidence_decay` that has not crossed a configured planning threshold or current affect constraint;
- low-priority `external_event` or `github_update` observations that do not affect the current Plan.

`elapsed_time` is materialized only when crossing a modeled boundary such as Task start or end, Static Task proximity, review due time, stale-affect threshold, reactive horizon expiry, offline precompute boundary, or compact Calendar expiration. It does not create continuous clock-tick Log entries.

---

## UBU-D0147: Moot reason codes are a closed MVP enum

**Status:** Accepted → DESIGN.md §9.5

`moot` remains a first-class terminal Task status that is functionally equivalent to completion for planning but distinct for logs, reporting, audit, and user review. Every Task transition to `moot` requires a reason code.

The Phase 1 MVP moot reason-code enum is:

- `externally_satisfied`;
- `superseded`;
- `delegated`;
- `no_longer_relevant`;
- `invalidated_by_universe_change`;
- `replaced_by_new_plan_structure`;
- `user_declared_moot`;
- `automation_obsolete`;
- `duplicate`.

This list is sufficient for MVP. It covers externally completed work, supersession, delegation, user-directed closure, world-state invalidation, plan-structure replacement, obsolete automation output, and duplicate imported or generated work. Additional nuance belongs in the `task_moot` Log entry's notes, reason text, provenance, External References, or correction/annotation entries rather than in ad hoc canonical codes.

`duplicate` is included because duplicate Tasks are common when importing GitHub Issues, PRs, comments, fixtures, worker outputs, or decomposed work. Duplicate closure must remain queryable and auditable rather than being collapsed into generic supersession.

`delegated` is separate from `externally_satisfied`. `delegated` means this Task is no longer assigned to this executor because responsibility moved to another executor, worker, Identity, Association, or delegation path. It does not assert that the underlying Objective or required world state is already satisfied. `externally_satisfied` means the required state became true through another action, observation, import, or external event.

Reason codes are enum-only in MVP. Implementations may accept human-readable notes and implementation-local diagnostic labels, but canonical `moot_reason_code` values must be one of the accepted enum values. New canonical reason codes require an explicit schema migration or accepted decision.

Selection guidance:

- use `externally_satisfied` when the required state is already true;
- use `superseded` when a newer Task, Objective, decision, or source artifact replaces this Task;
- use `delegated` when responsibility moved to another executor and will be tracked elsewhere;
- use `no_longer_relevant` when the Task is no longer useful but not invalidated by the world;
- use `invalidated_by_universe_change` when an external state change made the Task wrong or impossible;
- use `replaced_by_new_plan_structure` when decomposition, regrouping, or restructuring replaced this Task while preserving the underlying intent;
- use `user_declared_moot` when the user explicitly says the Task is moot and no more specific code applies;
- use `automation_obsolete` when worker or automation state makes generated work obsolete;
- use `duplicate` when another active, completed, or canonical work item already represents the same work.

---

## UBU-D0148: Task-to-Container mutation preserves Task identity as history

**Status:** Accepted → DESIGN.md §9.4

A Task does not literally change type into a Container by reusing its handle. Task-to-Container mutation is modeled as a structural replacement: the original Task remains an immutable historical Task record, a new Container is created with a new `container_id`, and the original Task transitions to `moot` when the restructuring replaces it for planning.

The Container records lineage back to the original Task through fields or metadata such as `origin_task_ref`, `mutation_reason`, `mutation_log_ref`, child WorkItem refs, and provenance. The original Task handle remains valid for Logs, External References, Plan history, projection history, and audit. It must not be reused as the Container handle.

All new child Tasks created during decomposition, preemption, retry, or worker expansion receive new Task handles. Existing Tasks may be grouped under the Container by reference when appropriate, but grouping does not rewrite their handles. A continuation child may inherit user-facing title context from the original Task, but it is still a new Task with its own lifecycle.

Intent-level fields stay on the Container or lineage metadata: title or summary, served Objective refs, parent or dependency context, external-reference lineage, Compartment/security labels, authority/provenance, and explanatory notes about why the work was split. Action-level fields belong on child Tasks: duration or duration PDF, preconditions, effects, executor/delegation fields, worker assignment/status, expected output, evidence requirements, child-specific dependencies, and child-specific deadlines.

Child Tasks may inherit or narrow Objective refs, Compartment refs, authority source, relevant External Reference context, and non-sensitive notes when those remain valid for the child. They do not silently inherit stale status, completion evidence, modeled effects, worker status, or projection state from the original Task.

The default original-Task lifecycle transition for decomposition or regrouping is `moot` with reason code `replaced_by_new_plan_structure`. Use narrower accepted moot codes when more accurate: `delegated` when responsibility moves out of the executor scope without decomposition, `superseded` when a newer Task, Objective, decision, or source artifact replaces the Task, and `duplicate` when another canonical work item already represents the same work.

External IDs are preserved through External References, not by reusing WorkItem handles. When an external object such as a GitHub Issue represents the whole unit of work, it should link to the Container. When the same external object supports or evidences a specific child Task, child-level External References may also be created with the appropriate relation type. The original Task's historical External References remain intact; new references express the replacement or child relationship rather than rewriting history.

For GitHub-linked Tasks, decomposition should keep the GitHub Issue or PR traceable to the Container and project only clearly marked child-level status when needed. GitHub projection and reconciliation use External References to decide whether the external object represents the Container, a child Task, or both.

Automation Worker child Tasks are ordinary child Tasks grouped under a Container, with worker/delegation metadata and capability boundaries. Workers may propose Task-to-Container restructuring only through authorized mutation requests. The canonical instance validates authority, expected prior version, Compartment/export policy, External Reference changes, and idempotency, then writes applied or rejected Log entries.

---

## UBU-D0149: Objective status transitions are mode-specific and logged

**Status:** Accepted → DESIGN.md §7.3

Objective status transitions are constrained by Objective mode. One-time Objectives and evergreen Objectives share the same status enum, but not every status is valid for every mode.

For one-time Objectives, valid statuses are `active`, `completed`, `abandoned`, `invalid`, and `superseded`. `satisfied` is not valid for one-time Objectives. Legal one-time transitions are:

```text
active -> completed
active -> abandoned
active -> invalid
active -> superseded
completed -> invalid
completed -> superseded
abandoned -> invalid
abandoned -> superseded
superseded -> invalid
```

`completed`, `abandoned`, and `superseded` are terminal for normal one-time planning. A completed one-time Objective does not reactivate. If the user later wants the same kind of outcome again, UbU should model that as a new Objective or as an explicit supersession, not as `completed -> active`.

For evergreen Objectives, valid statuses are `active`, `satisfied`, `abandoned`, `invalid`, and `superseded`. `completed` is not valid for evergreen Objectives. Legal evergreen transitions are:

```text
active -> satisfied
active -> abandoned
active -> invalid
active -> superseded
satisfied -> active
satisfied -> abandoned
satisfied -> invalid
satisfied -> superseded
abandoned -> invalid
abandoned -> superseded
superseded -> invalid
```

`abandoned` and `superseded` are terminal for normal evergreen planning. If an abandoned or superseded evergreen concern later becomes relevant again, UbU should usually create or select a new Objective rather than silently reactivating the old one.

`invalid` may occur from any Objective status and is terminal. It means the Objective record should not participate in normal planning because it is malformed, impossible, contradictory, forbidden by mode rules, or admitted by mistake. `invalid` is not a user preference to stop pursuing an otherwise valid Objective; that is `abandoned`.

`superseded` may occur from any non-`invalid` Objective status. It means a newer Objective, decision, import result, or source artifact has replaced the Objective for planning and traceability. Supersession should preserve lineage to the replacement Objective or source when known.

`abandoned` does not occur from any state. It is legal only from `active` one-time Objectives and from `active` or `satisfied` evergreen Objectives. It represents a user-authoritative or authority-source-authorized decision to stop pursuing an otherwise valid Objective.

Evergreen `satisfied -> active` reactivation is a canonical transition only when the accepted current state changes through user declaration, authorized observation/import, or elapsed-time recurrence evaluation. Hypothetical Plan simulation may predict that an evergreen Objective will become active in a future branch, but simulation alone does not mutate canonical Objective status.

Every accepted canonical Objective status transition creates an append-only Log entry with event type `objective_transitioned`, including old status, new status, actor or authority source, reason, effective time, and provenance when available. Simulated status changes inside candidate Plans or risk reports are predictions, not canonical transitions, and do not create `objective_transitioned` Logs unless accepted as actual state.

---

## UBU-D0150: Model-committee v0.2 adopts schema-native Claude Code cross-scoring

**Status:** Accepted → DESIGN.md §3

`model-committee v0.2` extends the v0.1 bootstrap loop by adding Claude Code CLI as a second frontier provider for both proposal generation and scoring.

This is a contract-level version change, not a small provider addition. v0.1 remains the narrow baseline, but v0.2 updates the dogfooding architecture to make disagreement between independent frontier providers visible, reviewable, and useful.

v0.2 scope includes:

- Claude Code CLI as a second frontier work and score provider;
- schema-native Claude Code structured output using `--json-schema`;
- cross-scoring between frontier providers;
- a first-class score matrix in run manifests and review artifacts;
- disagreement flags in `review.md`;
- a Claude Code config block;
- updated quorum rules;
- `doctor` checks for Claude availability and structured-output support;
- final operator-run artifact-publication instructions targeting `../model-committee-artifacts`.

v0.2 remains out of scope for:

- multi-turn Claude Code sessions;
- GitHub API integration;
- adaptive model weights;
- full Association automation;
- UbU planning-kernel work;
- automatic artifact push, merge, PR creation, or canonical design-state mutation.

Claude Code should be invoked as a subprocess provider, not through direct Anthropic API calls made by `model-committee` itself. The provider/network policy distinction is explicit:

- `model-committee` must not directly call Anthropic APIs;
- `model-committee` may invoke approved external CLI subprocesses when explicitly enabled by configuration;
- those provider CLIs may perform their own network/API calls according to their upstream authentication, billing, and policy configuration;
- every provider invocation must be logged with provider ID, model name or alias, argv shape, timeout, exit code, stdout/stderr artifact paths, schema-validation result, and relevant usage/cost metadata when available.

The installed Claude Code CLI version for the v0.2 target environment is confirmed as `2.1.146`, and that version supports `--json-schema`. Therefore Claude Code should use schema-native structured output as the primary path rather than Ollama-style JSON extraction.

The Claude Code provider should parse validated structured output from the CLI JSON envelope. When `--output-format json` and `--json-schema` are used, the schema-conforming payload should be read from `structured_output` rather than from free-form text. Prompt-embedded JSON extraction is only a compatibility fallback if a future environment lacks working schema-native output.

Claude tool authority must be restricted explicitly. For scripted schema-output runs, the default should be no tools or the narrowest necessary tool set. If file inspection is needed, use `--tools Read`. Do not rely on `--allowedTools` alone as a sandbox boundary, because allowing a tool without prompting is not the same as restricting the available tool set.

Cross-scoring rules:

- Codex scores Claude-authored proposals.
- Claude Code scores Codex-authored proposals.
- A provider's self-score may be retained as diagnostic metadata, but it does not count as quorum evidence.
- Local/Ollama providers remain useful for diversity, dissent, fallback, and offline review, but they do not replace the required frontier cross-score unless a later decision expands quorum policy.
- Score results should be stored as matrix entries with `proposal_id`, `author_provider`, `scorer_provider`, score, validity, rationale, risks, and required fixes.

v0.2 automated selection requires:

- at least one valid work proposal;
- at least one valid cross-score from a different frontier provider;
- no hard validation failures on the selected patch;
- no critical disagreement flag unless an explicit manual override mechanism is used outside automatic selection.

Default disagreement and review thresholds:

- frontier score gap of 25 or more points: human review required;
- selected score below 70: human review required;
- any selected patch validation failure: no selection;
- no valid cross-score from a different frontier provider: no automated selection.

v0.2 should add a distinct human-review-required exit code, provisionally `9`, for quorum or disagreement outcomes that are not the same failure mode as v0.1's invalid selected patch exit code `7`.

The generated `review.md` should include a final operator step to publish the run artifact to the sibling artifact repository:

```bash
RUN_ID="<run-id>"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```

These commands are instructions for a human operator or separately authorized release process. `model-committee v0.2` should generate them, not execute them automatically.

---

## UBU-D0151: Compact Calendar MVP grammar uses skeleton, legitimization, bounded candidates, and repair metadata

**Status:** Accepted → DESIGN.md §§15.2.2, 16

Resolved question: `UBU-Q0016`.

Compact Calendar planning for Phase 1 uses a staged explicit grammar rather than a bare DFS grammar.

The minimum skeleton Plan representation includes:

- Static Task placements with fixed start and end times;
- dependency DAG frontier for the current planning scope;
- prerequisite roots whose required state is not already true in the initial UniverseState;
- ordered prerequisite chains with dependency and precondition references;
- initial UniverseState assumptions, including known true, known false, assumed, and unresolved planner-relevant facts;
- unsatisfied dependency diagnostics with failing Task, missing state, causal chain, and clarification alternatives where available.

Skeleton generation must check the initial UniverseState before inserting a prerequisite Task. A dependency requires a state, not necessarily a new Task. If skeletonization cannot produce a viable baseline, ordinary planning stops and UbU reports a concrete diagnostic instead of continuing into optimization.

Legitimization adds the minimum human-viability constraints and support work required to make the skeleton plausible for execution:

- affect constraints in `user_mode`;
- recovery, break, meal, sleep, and rest requirements when applicable;
- transition buffers;
- setup and teardown time;
- context-switch limits;
- slack and dependency-fragility thresholds.

The legitimized skeleton baseline is the minimum feasible Plan. Support work inserted by legitimization is represented as ordinary Tasks or buffers with explanation lineage, not as hidden planner magic.

The minimum candidate Plan representation after legitimization includes:

- materialized Task placements;
- decision envelopes for movable Tasks;
- Plan probability metadata;
- value score;
- legitimacy threshold result or score;
- hard-validation status;
- explanation lineage back to Objectives, dependencies, preconditions, affect constraints, worker status, risk findings, and probability inputs.

Execution profiles are allowed as follows:

- `baseline`: greedy mean-duration planning is required as a benchmark and fallback reference.
- `mobile_local`: deterministic skeletonization, conservative legitimization, exact hard-constraint checks, local repair recipes, basic decision envelopes, cached explanations, and short-horizon BFS-like branch or repair reconstruction are required.
- `desktop_or_worker`: DFS-like candidate construction, local search, larger branch horizons, cached subplans, and solver-backed finalist validation are allowed.
- `solver_validation`: CP-SAT, SMT/MaxSMT, local-search, or comparable exact/conservative solvers may validate finalists, diagnose contradictions, or certify hard constraints.
- `gpu_or_hosted`: GPU-friendly scoring, stochastic simulation, affect scoring, robustness scoring, and learned-model inference may propose or rank candidates, but exact or conservative validation must certify selected Plans.

Search methods are implementation techniques, not semantic authority. DFS-like, BFS-like, greedy, solver-backed, GPU-friendly, and local repair methods may be combined as long as hard constraints, explanations, and selected-Plan validity remain inspectable.

Plan probability is represented as probability metadata with:

- a scalar display probability;
- a log probability for stable computation;
- an optional probability interval;
- a provenance expression over modeled probabilistic inputs;
- correlation-group, scenario, joint-distribution, or shared-random-variable references when inputs are not independent.

Implementations must not multiply probabilistic inputs unless independence is explicitly declared by the model. Correlated or unknown relationships must use joint scenarios, shared random variables, correlation groups, or conservative intervals rather than pretending independence. MVP may be conservative and mark correlation unknown instead of producing false precision.

Compact Calendar encoding should include, when present in the planning scope:

- skeleton Plan metadata;
- legitimized skeleton baseline;
- ordering constraints;
- duration PDFs;
- Task success probabilities;
- external-event and interruption distributions;
- affect constraints;
- decision envelopes;

---

## UBU-D0152: Extrospection is first-class Relationship review

**Status:** Accepted → DESIGN.md §§2.10.1, 22

Extrospection is a first-class Relationship review feature. It applies the evidence-backed attestation pattern used by introspection and AssociationAttestation to the user's Relationship model.

Relationships have **epistemic asymmetry**: the user has first-person access to the user's own declarations and experiences, while UbU can only maintain evidence-backed hypotheses about another party's perspective. Extrospection must therefore target the user's counterparty-perspective hypotheses, Relationship scopes, trust assumptions, reciprocity assumptions, affective impact, and functionality assumptions. It must not claim authoritative access to another person's inner state.

Extrospection should present evidence in tension rather than deliver an oracle verdict. It may surface confirming evidence, disconfirming evidence, ambiguity, insufficient evidence, clarification prompts, possible model updates, and introspection handoffs. The goal is evidence-backed confrontation and self-governance, not counterparty prosecution.

---

## UBU-D0153: Relationship perspectives, extrospection findings, and evidence policy are structured and reviewable

**Status:** Accepted → DESIGN.md §§22.1, 22.2

The richer Relationship model should lazily decompose the user's side into:

- `ownDeclaredPerspectiveHistory`;
- `ownObservedBehaviorRefs`;
- `ownAffectHistoryRefs`;
- `ownReflections`.

User declarations about a Relationship are versioned over time, not overwritten as a single timeless field.

Counterparty perspective should be represented as domain- and scope-tagged `counterpartyPerspectiveHypotheses`, not as a monolithic `speculatedPerspective`. UbU may detect internal tension among those hypotheses as a finding about the user's model consistency, not as a claim about the counterparty.

Extrospection assessments should be separated into `ScopeAssessment`, `BoundaryAssessment`, `ReciprocityAssessment`, `TrustCalibrationAssessment`, `AffectiveAssessment`, and `FunctionalityAssessment`. `ScopeAssessment` logically precedes the others. Negative affect is not equivalent to dysfunction, and reviewed value-aligned discomfort may be recorded as `userAcceptedDiscomfort`.

Extrospection findings use the lifecycle `candidate`, `deferred`, `resurfaced`, `reviewed_accepted`, `reviewed_rejected`, `superseded`, and `archived`. Unreviewed findings must not silently update durable Relationship state. Rejected findings should be retained enough to suppress repeated bad framings.

Durable evidence should prefer Compartment-aware `EvidenceRef`s, selectors, summaries, typed retention policy, typed redaction policy, and EvidenceUsePolicy over copied private excerpts. Cross-Relationship inference is prohibited by default. Evidence should decay in salience unless reactivated by new evidence, user review, or a current Objective.

---

## UBU-D0154: RelationshipScopeTransition is an Objective target, not a separate planning system

**Status:** Accepted → DESIGN.md §§7.5, 22.3

Users may explicitly create Objectives to change their own participation in a Relationship, invite or test a mutual scope change, or clarify whether another party is willing to enter a different scope.

`RelationshipScopeTransition` is a semantic target/reference for such Objectives. It is implemented through ordinary Objectives, Techniques, Steps, Tasks, Logs, extrospection reviews, introspection reviews, and UniverseState mutations. UbU should not create a parallel relationship-planning system.

The Relationship object remains primarily descriptive, but it keeps reverse references to active, completed, abandoned, declined, or superseded RelationshipScopeTransitions that affected it. This prevents declaration history from appearing to change spontaneously when a scope change resulted from a deliberate Objective.

Valid RelationshipScopeTransition work targets user-controlled behavior: disclosures, invitations, boundary-setting, availability changes, communication changes, and clarification attempts. Counterparty feelings, decisions, and Identity remain autonomous and uncertain.

---

## UBU-D0155: RelationshipScopeTransition separates process success from outcome and blocks optimized persuasion semantics

**Status:** Accepted → DESIGN.md §§7.5, 22.3

Consent-dependent and mutual RelationshipScopeTransitions must separate `process_success_criteria` from `outcome_observations`. A transition attempt can be process-successful even when the counterparty declines. Counterparty response is evidence, not the user's success or failure.

The `transition_type` enum is provisionally:

- `unilateral`;
- `exploratory`;
- `mutual`;
- `retroactive_scope_clarification`.

All actionable Steps and Tasks must be anchored to user-controlled behavior. A Step is malformed if its completion criterion requires a counterparty mental state, feeling, or decision.

RelationshipScopeTransition Techniques should satisfy epistemic transparency: the counterparty should receive accurate information sufficient for autonomous decision-making. Exploratory transitions follow a single-iteration rule: one disclosure or proposal, one clarification if ambiguous, and termination of the current Objective on authentic refusal. Future reopening requires materially new evidence, a new Objective, and stronger review.

The prohibited-strategy taxonomy is typed and includes artificial urgency, false scarcity, vulnerability exploitation, incremental boundary erosion, strategic information withholding, emotional state manipulation, leveraging privileged affect knowledge, manufactured dependency, social pressure through third parties, retaliation or threat, coercive leverage, deceptive self-presentation, and repeated pursuit after refusal.

The affect-knowledge firewall is canonical: extrospection-derived affect knowledge may be used for harm avoidance, not persuasion optimization. Romantic, professional, dependency-heavy, and power-asymmetric transitions may carry stronger default advisory safeguards, including power/vulnerability checks, pacing constraints, and graceful nonachievement paths.

---

## UBU-D0156: Relationship safeguards are default-on advisory unless structural boundaries apply

**Status:** Accepted → DESIGN.md §§2.10.2, 22.4, 23.3

Relationship-transition ethical reasoning, extrospection checks, introspection checks, manipulation-risk review, TrustCalibrationAssessment, power/vulnerability checks, pacing checks, and graceful nonachievement planning are wise default-on safeguards. Users should generally use them. However, UbU's prime directive is to respect the user's autonomy and decisions, so these behavioral-risk safeguards must not be unconditional hard gates by default.

Hard boundaries should be limited to structurally enforceable product invariants and explicit policies, including:

- Compartment boundaries;
- privacy policy;
- data-access authorization;
- EvidenceUsePolicy restrictions;
- export prohibitions;
- identity/Compartment isolation;
- provenance integrity;
- audit-record integrity;
- integration authorization;
- unavoidable provider/platform/legal constraints.

Behavioral-risk checks such as manipulation, coercion, harassment, stalking, deception, romantic/professional power asymmetry, rumination, and relationship-transition ethics depend on fallible classification. Treating them as hard gates risks false positives that block legitimate actions, false negatives that create misplaced trust, and a false impression that UbU can reliably prevent misuse or illegal behavior.

Behavioral safeguards should generally be default-on, user-aware, uncertainty-transparent, user-overrideable advisory checks with introspection consequences when bypassed. A user may configure some advisory checks as self-imposed required gates. Bypassing or disabling recommended safeguards is itself introspection-relevant evidence about revealed priorities and possible conflicts with declared values.

---

## UBU-D0157: LLM provider routing uses provider descriptors and policy-gated route decisions

**Status:** Accepted → DESIGN.md §2.11

Resolved question: `UBU-Q0060`.

UbU resolves external/cloud LLM routing through a provider-neutral descriptor plus a policy-gated route decision. Local Ollama-style providers, user-configured BYOK cloud APIs, user-owned remote workers, optional UbUCorp managed inference, and future compatible providers are all represented as LLM providers, but their trust boundaries remain explicit.

The minimum `LLMProviderDescriptor` records `provider_id`, `provider_class`, `endpoint_or_worker_ref`, execution location, operator, region or jurisdiction when known, supported models, modalities, context window, structured-output and tool-use capability, safety behavior, retention/training/disclosure profile, cost and rate-limit metadata, credential reference kind, default-enabled state, and user-visible name. The Phase 1 provider classes are `local_process`, `byok_cloud_api`, `user_owned_worker`, `ubucorp_managed_gateway`, and `third_party_compatible`.

Every provider adapter exposes the same narrow interface: list available models and capabilities, estimate cost and context fit, prepare a minimized request, invoke or stream completion, cancel when supported, and return usage, provider, model, and provenance metadata. The adapter interface normalizes execution mechanics only. It must not hide whether a request stays local, goes to a user-owned worker, or crosses a cloud/provider boundary.

An `LLMRouteRequest` records purpose, actor Identity, related Task/Objective/workflow refs, required capabilities, candidate provider policy, cost and latency limits, ContextBundle or source refs, required output schema when any, review requirement, and requested advisory use. An `LLMRouteDecision` records selected provider and model, boundary classification, Compartment policy result, minimization and redaction summary, estimated cost, disclosure text, approval state, and Log refs for allowed or denied routing.

Compartment policy is a hard upper bound on routing. `no_cloud_llm` content cannot be sent to cloud provider classes, including BYOK cloud APIs, UbUCorp managed gateways, or third-party compatible cloud providers. `no_external_export` content cannot be sent to external providers, remote workers, managed gateways, or third-party services except as redacted structural references that expose no protected payload. `local_only` content is limited to eligible local Devices and local providers. A capability grant, provider preference, workflow setting, or user click cannot override these denials.

Cloud routing requires context minimization before invocation. The route should use a `ContextBundle` or equivalent envelope that records purpose, source refs, Compartment refs, Identity and Association refs exposed, destination provider, model ref, data categories exposed, redaction policy, minimization notes, retention policy, user-visible summary, creation time, expiry when applicable, and downstream candidate refs. Raw payload should be replaced by references, summaries, hashes, or redacted structural fields whenever the task can still be performed.

BYOK credentials are credential references, not provider metadata payload. Secrets must not appear in provider descriptors, prompts, ContextBundles, Logs, or exported artifacts. Credential references may be scoped by provider, account, model family, Identity, Compartment, workflow, cost budget, and Device or worker. The default storage target is the local instance or OS/device secret store; a user-owned worker may hold a credential only when explicitly configured. Rotation and revocation preserve audit continuity through credential version refs without exposing the secret.

UbUCorp managed inference is represented as a provider class, not as a mandatory dependency or privileged protocol path. It uses the same descriptor, route request, route decision, Compartment gates, disclosure, provenance, cost controls, and output admission pipeline as any other cloud provider. The FOSS core must remain local-capable, BYOK-capable, self-hostable, and useful when UbUCorp managed inference is unavailable.

Any workflow using a cloud or external LLM must disclose, before or at execution time, the provider, model or model class, operator, local-vs-cloud status, destination region when known, whether BYOK or UbUCorp-managed credentials are used, data categories and Compartments exposed, retention/training profile, estimated cost when available, and that output is advisory until admitted through UbU validation and review. Approval may be per-run, per-session, per-workflow, or policy-based only when Compartment policy allows it.

**Consequences:**

- `UBU-Q0060` is resolved for Phase 1 architecture.
- LLM routing can proceed without privileging Ollama, UbUCorp, or any single cloud API as the canonical interface.
- `UBU-Q0079`, `UBU-Q0080`, `UBU-Q0083`, and `UBU-Q0084` may depend on this routing boundary while still refining MCP tools, Delegation Substrate fields, ContextBundle governance, and background-agent policy.
- Provider-specific SDK details, exact secret-store implementation, and long-context bundle review UX may evolve without changing the minimum boundary.

```

### OPEN_QUESTIONS.md

```markdown
# UbU Open Questions

Status: Draft  
Purpose: Public list of unresolved design questions for the UbU project.

Solved questions appear as compact tombstones. Open questions include full metadata, question body, subquestions, and current direction. The question-metadata format, allowed sentinel values, and question-selection policy are defined in `DECISIONS.md` (see UBU-D0060 and UBU-D0061).

---


## UBU-Q0001: Phase 1 MVP Scope Freeze

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Scope Auto-choice eligibility: Human only Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 implementation Resolved by: UBU-D0097 Last scored: Never Scored from commit: None

Resolved. See UBU-D0097.

---

## UBU-Q0002: GitHub Projection and Reconciliation

Status: Open Priority: MVP blocker Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0003 Blocks: Phase 1 GitHub dogfooding Resolved by: Unresolved Last scored: Never Scored from commit: None

### Question

1. Which GitHub fields does UbU write?
   - labels
   - issue body blocks
   - comments
   - milestones
   - assignees
   - PR statuses
2. Should UbU write only clearly marked `ubu:` labels and managed blocks?
3. Which GitHub edits are treated as external events?
4. Can any GitHub edit override UbU state?
5. How does UbU detect missed GitHub updates?
   - polling
   - webhooks
   - manual sync
   - worker-driven reconciliation
6. What does the reconciliation report compare?
   - GitHub Issues vs UbU Objectives
   - GitHub labels vs `pipeline_state`
   - GitHub comments vs event log
   - PRs / CI runs vs External Events
7. Does drift create a report only, or also Tasks to repair drift?
8. Does GitHub reconciliation run in the main UbU instance or an Automation Worker?

### Current direction

UbU should write only clearly marked UbU-managed labels, comments, or blocks, and treat other GitHub edits as External Events.

### Resolution

Unresolved.

---

## UBU-Q0003: GitHub ↔ UbU External Reference Model

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 GitHub dogfooding Resolved by: UBU-D0098 Last scored: Never Scored from commit: None

Resolved. See UBU-D0098.

---

## UBU-Q0004: Pipeline State

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0003 Blocks: Phase 1 implementation Resolved by: Unresolved Last scored: Never Scored from commit: None

`Objective.status` is the canonical UbU lifecycle status. `pipeline_state` is workflow/project-management state.

### Question

1. Is `pipeline_state` generic or GitHub-specific?
2. Can one Objective have multiple pipeline states for multiple projections?
3. Is `pipeline_state` stored on Objective directly or in projection metadata?
4. What is the MVP pipeline enum?
5. Can Automation Workers mutate `pipeline_state`?
6. Does every pipeline state transition create an event/log entry?

### Candidate MVP enum

```text
unlabeled
invalid
under_specified
valid_unprioritized
unassigned
in_process_awaiting_pr
awaiting_review
awaiting_ci
complete
```

### Resolution

Unresolved.

---

## UBU-Q0005: GitHub Event Triage Rules

Status: Open Priority: MVP blocker Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0002 Blocks: Phase 1 implementation Resolved by: Unresolved Last scored: Never Scored from commit: None

GitHub events must be interpreted into UbU events, Tasks, Objectives, or recalculation triggers.

### Question

1. Which GitHub events are logged only?
2. Which GitHub events create External Events?
3. Which GitHub events create Objectives?
4. Which GitHub events create Tasks?
5. Which GitHub events trigger recalculation?
6. Which GitHub events trigger Automation Worker assignment?
7. Which GitHub events trigger GitHub projection updates?
8. How are duplicate GitHub events detected?
9. How are missed events reconstructed during reconciliation?

### Candidate MVP event classes

```text
issue_opened
issue_commented
issue_labeled
issue_closed
pr_opened
pr_updated
review_requested
review_submitted
ci_failed
ci_passed
milestone_changed
```

### Resolution

Unresolved.

---

## UBU-Q0006: Objective and Task Explosion Control

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0005 Blocks: Phase 1 implementation Resolved by: Unresolved Last scored: Never Scored from commit: None

GitHub events may create analysis Objectives or Tasks. This could generate excessive noise.

### Question

1. When does a GitHub event deserve a new Objective?
2. When is it appended to an existing Objective?
3. When is it merely logged?
4. When does it create a Task?
5. How are duplicate analysis Objectives detected?
6. Do analysis Objectives inherit value from parent Objectives?
7. Do analysis Objectives automatically become moot or completed after resolution?
8. How does UbU prevent its own management process from becoming too noisy?

### Resolution

Unresolved.

---

## UBU-Q0007: Automation Worker Identity and Capabilities

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 implementation Resolved by: UBU-D0095 Last scored: Never Scored from commit: None

Resolved. See UBU-D0095.

---

## UBU-Q0008: Worker Assignment Model

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0007 Blocks: Phase 1 implementation Resolved by: Unresolved Last scored: Never Scored from commit: None

Workers need a way to discover or receive work.

### Question

1. Does an organization-mode instance explicitly assign Tasks to workers?
2. Or do workers poll for Tasks matching their capabilities?
3. Can multiple workers observe the same Task?
4. Can multiple workers compete for the same Task?
5. What happens if a worker disappears mid-Task?
6. Is assignment itself logged?
7. Can workers reject assignments?
8. Can workers request clarification Tasks?
9. Can workers spawn child Tasks?

### Current MVP leaning

Explicit assignment is likely simplest.

### Resolution

Unresolved.

---

## UBU-Q0009: Worker Mutation Request Schema

Status: Open Priority: MVP blocker Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0007 Blocks: Phase 1 implementation Resolved by: Unresolved Last scored: Never Scored from commit: None

Automation Workers need a bounded way to submit changes back to canonical UbU state.

### Question

1. Does a worker submit:
   - event records,
   - mutation requests,
   - proposed patches,
   - or all three?
2. What fields are required?
   - worker Identity
   - authority source
   - target object
   - operation
   - expected prior version
   - new value
   - reason
   - evidence reference
   - timestamp
   - idempotency key
3. Are valid mutations applied immediately?
4. Are invalid mutation attempts logged?
5. Can mutation requests be batched atomically?
6. Can workers request creation of new Objectives or Tasks?
7. Are worker mutations reversible?
8. How does UbU prevent stale worker mutations from overwriting newer canonical state?

### Resolution

Unresolved.

---

## UBU-Q0010: GitHub Token Custody

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0007 Blocks: Phase 1 implementation Resolved by: Unresolved Last scored: Never Scored from commit: None

Automation Workers may interact with GitHub using access tokens.

### Question

1. Are GitHub tokens stored:
   - on the central UbU instance,
   - only on worker-mode instances,
   - or both?
2. Are tokens tied to:
   - bot accounts,
   - maintainer accounts,
   - individual contributor accounts?
3. Can tokens be scoped per repository?
4. Can tokens be scoped per Task?
5. Does the central UbU instance ever see the token?
6. Does UbU verify GitHub writes by re-reading GitHub?
7. What is the MVP security assumption?

### Current leaning

For MVP, workers may own their GitHub tokens. The canonical UbU instance stores external refs/results, not necessarily the token.

### Resolution

Unresolved.

---

## UBU-Q0011: Inter-Instance Protocol

Status: Solved Priority: Post-MVP Phase: Phase 3 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 3 implementation Resolved by: UBU-D0144 Last scored: Never Scored from commit: None

Resolved. See UBU-D0144.

---

## UBU-Q0012: Organization Mode Object Rules

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 implementation Resolved by: UBU-D0101 Last scored: Never Scored from commit: None

Resolved. See UBU-D0101.

---

## UBU-Q0013: Authority Source Vocabulary

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0012 Blocks: Phase 1 implementation Resolved by: Unresolved Last scored: Never Scored from commit: None

Organizational directives and worker updates need authority/source metadata.

### Question

1. Which objects carry `authority_source`?
   - Preference
   - Objective
   - Task
   - pipeline state
   - worker assignment
   - external projection
   - mutation request
2. Is `authority_source` required only in organization mode?
3. What are MVP authority-source values?

### Candidate values

```text
human_admin
automation_worker
github_event
project_policy
imported_config
llm_advisory
user_override
```

### Resolution

Unresolved.

---

## UBU-Q0014: UniverseState Mutation Schema

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 implementation Resolved by: UBU-D0130 Last scored: Never Scored from commit: None

Resolved. See UBU-D0130.

---

## UBU-Q0015: Task Precondition Schema

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 implementation Resolved by: UBU-D0131 Last scored: Never Scored from commit: None

Resolved. See UBU-D0131.

---

## UBU-Q0016: Compact Calendar planner grammar and execution profile

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0014, UBU-Q0015 Blocks: Phase 1 implementation Resolved by: UBU-D0151 Last scored: Never Scored from commit: None

Compact Calendar support is important for recursive self-analysis, future sync/transport, and fast recalculation. The current direction is no longer a bare DFS grammar. The planner architecture begins with skeleton Plan generation, then legitimization, then candidate Plan expansion and validation, with reactive mobile stewardship around the selected Plan.

### Question

1. What is the minimum skeleton Plan representation?
   - Static Task placement
   - dependency DAG frontier
   - prerequisite roots
   - ordered prerequisite chains
   - initial UniverseState assumptions
   - unsatisfied dependency diagnostics
2. What does legitimization add?
   - affect constraints
   - recovery Tasks
   - transition buffers
   - sleep/food/rest requirements
   - setup/teardown time
   - context-switch limits
   - slack and fragility thresholds
3. What is the minimum candidate Plan representation after legitimization?
   - materialized Tasks
   - decision envelopes
   - Plan probability provenance
   - value score
   - legitimacy score or threshold result
   - explanation lineage
4. Which search methods are allowed in each execution profile?
   - greedy baseline
   - DFS-like candidate construction
   - BFS-like near-term branch construction
   - solver-backed exact validation
   - GPU-friendly candidate scoring/simulation
   - local repair recipes
5. How is Plan probability represented?
   - scalar probability
   - log probability
   - probability interval
   - provenance expression over probabilistic inputs
6. How does the implementation avoid naïve independence assumptions when probabilities are correlated?
7. Does Compact Calendar encode:
   - skeleton Plan metadata
   - legitimized skeleton baseline
   - ordering constraints
   - duration PDFs
   - success probabilities
   - external-event distributions
   - interruption distributions
   - affect constraints
   - decision envelopes
   - protected/flexible/disposable Task metadata
   - cached explanations
   - execution-mode metadata such as time delta, branch horizon, GPU/CPU resource limits, and privacy-routing limits
8. Which concrete Plans are stored?
   - the legitimized skeleton baseline
   - the default Plan
   - user-previewed Plans
   - risk-report Plans
   - debug/reproducibility Plans
   - high-probability near-term Plans within a horizon
9. Which Plans or repairs are reconstructed on demand?
10. How is coverage recalculated after time advances?
11. What is the minimum MVP implementation?

### Current direction

The planner should first create a skeleton Plan from Static Tasks and dependency DAGs, then legitimize that skeleton Plan into a minimally human-viable baseline. Candidate Plans are then generated, semi-legitimized or fully legitimized, validated, and compared. DFS-like search may be one candidate-construction strategy, but it is no longer the full conceptual foundation. BFS-like branch caching remains useful for near-term divergence, but mobile UX should also use decision envelopes, cached explanations, criticality metadata, last-legitimate-Plan repair, and conflict severity.

The ideal search may be NP-hard or otherwise combinatorially expensive, but finite Task instances, bounded time scope, configurable deltas, pruning, greedy baselines, GPU-friendly scoring/simulation, cached subplans, and adaptive execution modes should make practical approximations tractable.

### Resolution

Resolved. See UBU-D0151.

---

## UBU-Q0017: Compact Calendar Coverage and Regeneration

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0016 Blocks: Phase 1 implementation Resolved by: Unresolved Last scored: Never Scored from commit: None

Coverage belongs to compact Calendar serialization.

### Question

1. Is coverage exact or estimated in MVP?
2. Does coverage include:
   - duration uncertainty
   - Task success/failure uncertainty
   - external-event uncertainty
   - Objective recurrence uncertainty
3. Does a compact Calendar store coverage value only, or also uncovered mass?
4. What is the default regeneration threshold?
5. Is the threshold global, device-specific, Calendar-specific, or all three?
6. Does low coverage create:
   - report only,
   - recalculation trigger,
   - Task,
   - worker assignment?

### Current direction

The provisional short-horizon branch coverage target is `0.99` probability mass. Coverage and regeneration thresholds may be device-specific, Calendar-specific, or execution-mode-specific. This is not final.

### Resolution

Unresolved.

---

## UBU-Q0018: Risk Reporting Primitives

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 implementation Resolved by: UBU-D0145 Last scored: Never Scored from commit: None

Resolved. See UBU-D0145.

---

## UBU-Q0019: Recalculation Trigger Taxonomy

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 implementation Resolved by: UBU-D0146 Last scored: Never Scored from commit: None

Resolved. See UBU-D0146.

---

## UBU-Q0020: Automation Worker Retry Semantics

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0008 Blocks: Phase 1 implementation Resolved by: Unresolved Last scored: Never Scored from commit: None

Automation Workers may fail or produce failed child Tasks.

### Question

1. If a worker-created child Task fails, does UbU:
   - mutate the same Task,
   - create retry sibling,
   - mark failed then create replacement,
   - mark moot?
2. Is retry policy defined by:
   - Task
   - Objective
   - Worker
   - integration type
3. How are retry attempts logged?
4. How are repeated failures prevented from creating infinite loops?
5. Does a worker failure affect risk reports?

### Current leaning

Failed attempt + retry sibling is likely best for auditability.

### Resolution

Unresolved.

---

## UBU-Q0021: Automation Worker Parent / Child Structure

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0008 Blocks: Phase 1 implementation Resolved by: Unresolved Last scored: Never Scored from commit: None

Automation Workers may create canonical child Tasks.

### Question

1. Is the outer Automation/Super Automation Task also the Container?
2. Or is a separate Container created under it?
3. Are child Tasks ordinary Dynamic Tasks?
4. Or are child Tasks a dedicated automation-step subtype?
5. If child Tasks are ordinary Dynamic Tasks, how is automation-specific metadata stored?
6. How are retries connected to the original child Task?

### Resolution

Unresolved.

---

## UBU-Q0022: Moot Reason-Code Taxonomy

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 implementation Resolved by: UBU-D0147 Last scored: Never Scored from commit: None

Resolved. See UBU-D0147.

---

## UBU-Q0023: Container Mutation Semantics

Status: Solved Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 2 implementation Resolved by: UBU-D0148 Last scored: Never Scored from commit: None

Resolved. See UBU-D0148.

---

## UBU-Q0024: Objective Status Transition Table

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 implementation Resolved by: UBU-D0149 Last scored: Never Scored from commit: None

Resolved. See UBU-D0149.

---

## UBU-Q0025: Snapshot Application Semantics

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 implementation Resolved by: UBU-D0100 Last scored: Never Scored from commit: None

Resolved. See UBU-D0100.

---

## UBU-Q0026: Relationship Maintenance Modeling

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 implementation Resolved by: UBU-D0076 Last scored: Never Scored from commit: None

Resolved. See UBU-D0076.

---

## UBU-Q0027: Organization Mode and Worker Mode Public UX

Status: Solved Priority: Post-MVP Phase: Post-MVP Decision type: Product Auto-choice eligibility: Human only Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Post-MVP product Resolved by: UBU-D0075 Last scored: Never Scored from commit: None

Resolved. See UBU-D0075.

---

## UBU-Q0028: Minimum Privacy / Compartment Promise for MVP

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 implementation Resolved by: UBU-D0074 Last scored: Never Scored from commit: None

Resolved. See UBU-D0074.

---

## UBU-Q0029: Open-Core / FOSS Contribution Boundary

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Governance Auto-choice eligibility: Human only Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 implementation Resolved by: UBU-D0073 Last scored: Never Scored from commit: None

Resolved. See UBU-D0073.

---

## UBU-Q0030: Phase 1 Public Demo Criteria

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Product Auto-choice eligibility: Human only Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 release Resolved by: UBU-D0072 Last scored: Never Scored from commit: None

Resolved. See UBU-D0072.

---

## UBU-Q0031: Log Structure and Fields

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 implementation Resolved by: UBU-D0071 Last scored: Never Scored from commit: None

Resolved. See UBU-D0071.

---

## UBU-Q0032: Model Committee Process and Authority

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Model-committee correctness Resolved by: UBU-D0057, UBU-D0058, UBU-D0059, UBU-D0060, UBU-D0065, UBU-D0069, UBU-D0070, UBU-D0150 Last scored: Never Scored from commit: None

Resolved. See UBU-D0057, UBU-D0058, UBU-D0059, UBU-D0060, UBU-D0065, UBU-D0069, UBU-D0070, UBU-D0150.

---

## UBU-Q0033: Phase 1 MVP Readiness Scoring Rubric

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0001 Blocks: README readiness signal Resolved by: Unresolved Last scored: Never Scored from commit: None

### Question

How should UbU estimate how close the project is to Phase 1 scope freeze and MVP readiness?

### Resolution

Unresolved.

---

## UBU-Q0034: Design Automation Stop Rule

Status: Open Priority: MVP blocker Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0001 Blocks: Phase 1 scope freeze Resolved by: Unresolved Last scored: Never Scored from commit: None

### Question

When should UbU stop answering additional pre-MVP design questions and begin coding the MVP?

### Subquestions

1. What categories of questions may block Phase 1?
2. What categories must be deferred even if unresolved?
3. What MVP readiness score is sufficient to begin implementation?
4. What maximum rate of new MVP-blocker creation is acceptable?
5. How should the value of answering another question be compared against coding?
6. Can the model committee recommend that no further pre-MVP design work is justified?

### Resolution

Unresolved.

---

## UBU-Q0035: Automation Coverage Taxonomy

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0032 Blocks: Question-ranking accuracy Resolved by: Unresolved Last scored: Never Scored from commit: None

### Question

How should remaining design work be classified by automation eligibility?

### Resolution

Unresolved.

---

## UBU-Q0036: Committee Log and Provenance Format

Status: Open Priority: MVP blocker Phase: Phase 1 Decision type: Process Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0032 Blocks: model-committee logging Resolved by: UBU-D0064 Last scored: Never Scored from commit: None

### Question

What minimum files and fields must a model-committee run log preserve?

### Current direction

v0.1 should use the provisional filesystem log format defined in `UBU-D0064`. The provisional format includes canonical file snapshots, schema files, Codex prompts, Ollama prompts, Codex JSON outputs, Codex JSONL event logs, provider stderr, parsed proposals, patch files, selected patch, review artifact, and commit message. v0.2 should extend that format with Claude Code prompts, raw Claude CLI JSON envelopes, schema-native `structured_output` payloads, provider invocation metadata, score-matrix entries, disagreement flags, quorum results, and operator-run artifact-publication instructions. The final log/provenance format remains open and may be refined after the first working v0.2 implementation exists.

### Resolution

Partially resolved by `UBU-D0064`; final log/provenance requirements remain open.

---

## UBU-Q0037: Post-MVP Question Lifecycle

Status: Open Priority: Post-MVP Phase: Post-MVP Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0032 Blocks: None Resolved by: Unresolved Last scored: Never Scored from commit: None

### Question

How should post-MVP design questions be preserved, re-ranked, reopened, or retired after the first MVP release?

### Resolution

Unresolved.

---

## UBU-Q0038: Changeset-Based Work Phase

Status: Open Priority: MVP blocker Phase: Phase 1 Decision type: Process Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0032 Blocks: model-committee work execution Resolved by: UBU-D0063, UBU-D0069 Last scored: Never Scored from commit: None

### Question

How should the model-committee work phase represent, score, select, apply, and commit concrete changesets?

### Subquestions

1. What format should work proposals use?
2. What validation is required before work scoring?
3. May models score their own work?
4. What criteria should work scoring use?
5. When may a selected changeset be committed locally?
6. What files may be modified in v0.1?
7. How does the work phase generalize from design questions to code changes and bug fixes?
8. How should Codex CLI schema-constrained proposals fit into the work phase?
9. How should Ollama secondary proposals be included in Codex scoring?
10. What should happen when Codex scoring selects a mechanically invalid proposal?

### Current direction

The work phase should produce explicit patch-style changesets, score those changesets, select the best one when quorum is satisfied, and create reviewable artifacts. v0.1 uses Codex CLI as the primary schema-constrained work and scoring provider, with Ollama as secondary proposal providers. v0.2 adds Claude Code CLI as a schema-native frontier provider and requires cross-scoring: Codex scores Claude proposals and Claude scores Codex proposals. Self-scores may be diagnostic but do not count as quorum evidence. v0.2 writes or updates `selected.patch`, `commit_message.txt`, `review.md`, score-matrix artifacts, disagreement flags, and logs. Remote GitHub mutation, automatic patch application, automatic artifact push, and automatic PR creation remain out of scope.

### Resolution

Partially resolved by `UBU-D0063`, `UBU-D0069`, and `UBU-D0150`; detailed scoring and validation rules may be refined after implementation.

---

## UBU-Q0039: Prioritized Recursive Loop Semantics

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0032 Blocks: model-committee loop semantics Resolved by: UBU-D0066 Last scored: Never Scored from commit: None

### Question

How should model-committee represent and enforce the prioritized recursive loop of consistency, prioritization, and work?

### Subquestions

1. What exact events trigger a system-wide consistency check?
2. What consistency failures should block ordinary work?
3. How are consistency failures converted into questions or work items?
4. When may prioritization run if consistency has warnings but no hard failures?
5. When may work proceed against a known inconsistency?
6. How should this loop map into future UbU Automation Worker behavior?
7. How should LLM model updates trigger re-checks or re-scoring?
8. How should Codex CLI provider updates trigger re-checks or re-scoring?

### Current direction

System-wide consistency has highest priority, question/problem prioritization has second priority, and work has third priority. Work should not proceed against a known-inconsistent state unless the selected work item repairs that inconsistency.

### Resolution

Partially resolved by `UBU-D0066`; detailed trigger and blocking semantics remain open.

---

## UBU-Q0040: Question Decomposition and Design Burden Scoring

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0039 Blocks: model-committee prioritization accuracy Resolved by: UBU-D0068 Last scored: Never Scored from commit: None

### Question

How should model-committee distinguish harmful question proliferation from valuable decomposition of hard questions into simpler, more automatable questions?

### Subquestions

1. How is unresolved design burden measured?
2. How is automation difficulty compared between an original question and its replacements?
3. When may a question be marked decomposed rather than solved?
4. What metadata should link replacement questions to the original question?
5. How should work scoring reward valid decomposition?
6. How should the stop rule account for decomposition that increases question count but lowers total difficulty?
7. When is dependency-reducing decomposition preferable to waiting for all dependencies to be answered?
8. How should the system score decompositions that produce at least one immediately answerable replacement question?
9. How should dependency simplification affect question ranking?
10. How should answerability be computed from dependency metadata?
11. When should a blocked question be selected for decomposition rather than skipped?
12. Should answerability be a hard gate or a weighted score?

### Current direction

Increasing the number of open questions is acceptable when a hard or blocked question is split into simpler, clearer, lower-risk, or more automatable questions. This is especially valuable when at least one replacement question has fewer dependencies, simpler dependencies, or no dependencies, allowing the system to make progress without waiting for the full original dependency chain.

Question selection should use answerability as the first gate. Ordinary work should only run on questions whose dependencies are resolved, absent, or handled in the same work item. Blocked questions may still be selected for decomposition if the decomposition is expected to produce replacement questions with fewer, simpler, or no dependencies.

### Resolution

Partially resolved by `UBU-D0068`; detailed scoring mechanics remain open.

---


## UBU-Q0041: Public recruitment language for a small core cohort

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Product Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Contributor recruitment Resolved by: UBU-D0078, UBU-D0085, UBU-D0093 Last scored: Never Scored from commit: None

Resolved. See UBU-D0078, UBU-D0085, UBU-D0093.

---

## UBU-Q0042: Feedback, dignity, emotional limits, and growth pressure

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Affect-aware planning quality Resolved by: UBU-D0081, UBU-D0082, UBU-D0092 Last scored: Never Scored from commit: None

Resolved. See UBU-D0081, UBU-D0082, UBU-D0092.

---

## UBU-Q0043: First technical essay claim, avoidances, and request

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Product Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Public outreach Resolved by: UBU-D0083, UBU-D0091 Last scored: Never Scored from commit: None

Resolved. See UBU-D0083, UBU-D0091.

---

## UBU-Q0044: Long-arc project narrative without grandiosity

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Product Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Public narrative Resolved by: UBU-D0084, UBU-D0090 Last scored: Never Scored from commit: None

Resolved. See UBU-D0084, UBU-D0090.

---

## UBU-Q0045: Smallest prototype-funder workflow for independent technical consultants

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Prototype-funder discovery Resolved by: UBU-D0079, UBU-D0089 Last scored: Never Scored from commit: None

Resolved. See UBU-D0079, UBU-D0089.

---

## UBU-Q0046: Public dogfooding artifacts for contributor credibility

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0036, UBU-Q0038 Blocks: Contributor recruitment Resolved by: UBU-D0077, UBU-D0086 Last scored: Never Scored from commit: None

### Question

What public dogfooding artifacts should `model-committee` expose to make UbU credible and actionable to potential contributors?

### Subquestions

1. Which run artifacts should be committed, linked, summarized, or ignored?
2. How should selected patches, review notes, and commit messages be presented?
3. How should public artifacts avoid leaking private reasoning or unsafe provider outputs?
4. What would convince a developer that the loop is real and reviewable?
5. How should artifacts connect to GitHub Issues or PRs?
6. How should failed or partially successful runs be shown without damaging credibility?

### Current direction

The project should show the dogfooding loop, not merely describe it. Artifacts should demonstrate that open questions become model-assisted proposals, reviewable patches, decisions, and updated canonical state. v0.2 review artifacts should also show cross-model score matrices, quorum outcomes, disagreement flags, and the operator-run publication command for copying the run to `../model-committee-artifacts`.

### Resolution

Partially resolved by `UBU-D0077`, `UBU-D0086`, and `UBU-D0150`; exact long-term artifact publication policy remains open, but v0.2 has a concrete operator-run publication path for model-committee run artifacts.

---

## UBU-Q0047: Minimum committed-contributor onboarding path

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Product Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Contributor recruitment Resolved by: UBU-D0078, UBU-D0086, UBU-D0088 Last scored: Never Scored from commit: None

Resolved. See UBU-D0078, UBU-D0086, UBU-D0088.

---

## UBU-Q0048: Funding offers incompatible with self-governance mission

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Governance Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Prototype-funder discovery Resolved by: UBU-D0079, UBU-D0080, UBU-D0087 Last scored: Never Scored from commit: None

Resolved. See UBU-D0079, UBU-D0080, UBU-D0087.

---

## UBU-Q0049: Release Outreach Pipeline artifact model and implementation boundary

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0030, UBU-Q0036, UBU-Q0038 Blocks: Release Outreach Pipeline implementation, release communication, contributor recruitment Resolved by: UBU-D0094 Last scored: Never Scored from commit: None

### Question

What is the minimum useful artifact model and implementation boundary for the Release Outreach Pipeline?

### Subquestions

1. What files and metadata should a release outreach package contain?
2. How should screenshots, UI-test exports, fixture captures, and demo recordings record provenance?
3. What schema should distinguish implemented behavior, mock behavior, future plans, and speculative goals?
4. What human approval gates are required before video rendering, platform upload, public posting, or external channel mutation?
5. How should Compartment, Identity, public-projection, and export rules prevent private data leakage?
6. Which parts of the pipeline belong in Phase 1 dogfooding and which should remain post-MVP?
7. How should project configurations define audience-specific communication Objectives?
8. Which video-generation tools or local renderers should be treated as Automation Workers versus external publication systems?

### Current direction

`UBU-D0094` accepts the feature bundle and tagline: "UbU should make every release explain itself." The minimum path should start with manually reviewed release outreach packages generated from repo state, release notes, accepted decisions, closed issues, and approved screenshots. Full automated video generation and publication should be deferred until the artifact schema, provenance model, privacy gates, and review workflow are explicit.

### Resolution

Partially established by `UBU-D0094`; detailed artifact schema, implementation boundaries, provenance requirements, and review gates remain open.

---

## UBU-Q0050: Minimum Phase 1 bootstrap interview and next-action focus UX

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 public demo, nontechnical onboarding, mock app prototype, Release Outreach Pipeline demo scripts Resolved by: UBU-D0129 Last scored: Never Scored from commit: None

Resolved. See UBU-D0129.

---

## UBU-Q0051: Minimum preference-calibration examples for Phase 1 onboarding and review

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0050 Blocks: Phase 1 onboarding quality, Preference calibration, Calendar preview, Log review Resolved by: None Last scored: Never Scored from commit: None

### Question

What is the minimum useful set of preference-calibration examples for Phase 1 onboarding, Calendar preview, and Log review?

### Subquestions

1. Which common situations should UbU use to emotionally ground early Preference judgments?
2. How many examples are enough for Phase 1 without making onboarding burdensome?
3. Which answers become Preferences, Snapshots, Logs, Objective annotations, or noncanonical review notes?
4. How should examples avoid leading the user toward an assumed value model?
5. How should calibration examples distinguish urgent value, emotional cost, social pressure, recovery value, and long-term importance?
6. How should UbU revise or retire examples that repeatedly fail to help the user make accurate judgments?

### Current direction

Preference calibration is MVP important but not an MVP blocker. UbU should use examples of common emotional costs, rewards, and tradeoffs to help users make better Preference statements. Examples are grounding aids, not canonical Preferences, unless the user accepts or edits them into canonical entries.

### Resolution

Open.

---

## UBU-Q0052: Discovery-mode action inference and override semantics

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0050 Blocks: Discovery mode, mobile sensor workflow, behavior reconciliation, habit-pattern inference Resolved by: None Last scored: Never Scored from commit: None

### Question

What is the minimum Phase 1 model for discovery mode, actual-action inference, and user override semantics?

### Subquestions

1. How does the user select discovery mode, pause it, exit it, and inspect what it collected?
2. Which mobile sensor, integration, quick-note, or app-state signals are acceptable Phase 1 discovery inputs?
3. How should UbU represent actual user action when it differs from the Calendar?
4. When does an override become a Log entry, Snapshot, Preference update, Task status change, Objective reconsideration, or unresolved review item?
5. What clarification prompt is required before treating repeated behavior as a habit pattern?
6. How should UbU reconcile undetailed or under-specified time periods without overclaiming certainty?
7. How should Discovery mode preserve user sovereignty and avoid covert surveillance semantics?

### Current direction

Discovery mode is a user-selectable workflow state available at any time, especially from the mobile app. It may gather evidence useful for later Log review and UbU-directed reconciliation, but inferred observations are not final truth until accepted, corrected, or otherwise handled according to user-visible rules.

### Resolution

Open.

---

## UBU-Q0053: Calendar preview and Log review psychological annotations

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0050 Blocks: Regular review Tasks, preview UX, Log review UX, psychological annotation boundary Resolved by: None Last scored: Never Scored from commit: None

### Question

What psychological annotations should Phase 1 Calendar preview and Log review collect, store, or report?

### Subquestions

1. What is the minimum recurring Calendar preview Task?
2. What is the minimum recurring Log review Task?
3. How often should these Tasks run by default, and how should the user adjust cadence?
4. Which annotations belong in canonical Logs, Preferences, Snapshots, Objectives, Tasks, or Reports?
5. Which annotations should remain noncanonical review notes?
6. Should autonomy, competence, relatedness, social pressure, attitude, subjective norms, perceived control, and expected execution be modeled directly or only through preview/review comments?
7. How should UbU ask useful consistency questions without becoming therapeutic, accusatory, or paternalistic?

### Current direction

Calendar preview and Log review are notable Tasks that should run regularly to ensure that UbU is correctly modeling the user's intended behavior, actual behavior, affective constraints, and Preference judgments. Self-determination theory and theory of planned behavior mostly inform these interface and reporting layers rather than Phase 1 core ontology.

### Resolution

Open.

---

## UBU-Q0054: Social identity theory impact on Identity, role, group membership, and mode switching

Status: Open Priority: Post-MVP Phase: Post-MVP Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0025 Blocks: Post-MVP multi-user Identity modeling, group-membership modeling, mode-switching policy Resolved by: None Last scored: Never Scored from commit: None

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

Open.

---

## UBU-Q0055: Social choice theory and collective decision legitimacy

Status: Open Priority: Post-MVP Phase: Post-MVP Decision type: Governance Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0025 Blocks: Post-MVP organizational governance, multi-user decision procedures, collective legitimacy Resolved by: None Last scored: Never Scored from commit: None

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

Open.

---

## UBU-Q0056: Game theory, strategic interaction, and counterparty modeling

Status: Open Priority: Post-MVP Phase: Post-MVP Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0025 Blocks: Strategic interaction modeling, counterparty incentives, post-MVP coordination protocols Resolved by: None Last scored: Never Scored from commit: None

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

## UBU-Q0057: Evergreen Dynamic Task and gap-filling semantics

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0016 Blocks: Gap filling before Static Tasks, mobile next-action UX, Calendar preview quality Resolved by: None Last scored: Never Scored from commit: None

### Question

How should UbU represent evergreen Dynamic Tasks or evergreen Objective-maintenance Tasks that can fill spare time when all predetermined Tasks before the next Static Task are complete, blocked, moot, or unsuitable?

### Subquestions

1. Are evergreen gap-fillers a Task subtype, a Task property, an Objective-maintenance pattern, a Technique-instantiation rule, or planner behavior?
2. What fields are required?
   - flexible start time
   - flexible duration
   - minimum useful duration
   - maximum useful duration
   - location invariance or broad location category
   - required materials or device state
   - affect suitability
   - cooldown/frequency limit
   - Objective link
   - recurrence/reactivation rule
3. How should UbU choose among evergreen candidates such as meditation, Calendar preview, Log review, lightweight cleanup, reflection, review queues, or relationship-maintenance prompts?
4. How should evergreen Tasks avoid becoming filler that crowds out recovery, transition time, or user autonomy?
5. How should the reactive branch layer consider evergreen Tasks when early completion creates a gap before the next Static Task?
6. Which evergreen semantics are necessary for Phase 1 and which can wait?

### Current direction

The planner should not leave the user with unexplained idle gaps merely because all predetermined Dynamic Tasks before a Static Task are complete. UbU should be able to suggest or schedule useful evergreen Dynamic Tasks when constraints allow, but the exact representation remains open.

### Resolution

Open.

---

## UBU-Q0058: Adaptive planning granularity and offline precomputation policy

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0016 Blocks: Mobile-only planning, offline mode, low-power mode, Compact Calendar runtime policy Resolved by: None Last scored: Never Scored from commit: None

### Question

How should UbU choose, switch, and explain Compact Calendar time delta, branch horizon, and branch coverage settings across full-detail, mobile, low-power, and offline execution modes?

### Subquestions

1. Should default deltas be exactly one minute, five minutes, and fifteen minutes, or configurable presets?
2. What user-visible explanation is required when UbU switches to a coarser delta?
3. Which triggers cause dynamic execution-mode switching?
   - low battery
   - thermal throttling
   - offline state
   - expected offline window
   - heavy workload
   - user setting
   - missing external worker
4. How much precomputation should UbU run before a known offline window?
5. How should cached plans expire after the user's actual behavior diverges from the precomputed branch?
6. What is the minimum mobile-only guarantee for preserving the core UbU experience?

### Current direction

Provisional defaults are one-minute full-detail delta, five-minute moderate mobile delta, fifteen-minute low-power/offline delta, one-hour reactive horizon, and `0.99` short-horizon branch coverage target. Mobile-only UbU must preserve the core experience, but may use coarser granularity, shallower branch coverage, reduced analysis depth, and fewer LLM-assisted features.

### Resolution

Open.

---

## UBU-Q0059: Execution-provider trust, worker backends, and privacy-preserving compute roadmap

Status: Open Priority: Post-MVP Phase: Phase 2 Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0016 Blocks: Phase 2 personal worker devices, hosted compute policy, third-party provider compatibility Resolved by: None Last scored: Never Scored from commit: None

### Question

How should UbU define the trust, privacy, capability, API, and disclosure boundary for execution backends beyond the current local instance?

### Subquestions

1. What is the minimum Phase 2 personal worker protocol for a user-owned laptop, desktop, or server?
2. How should the planner route work among mobile, local desktop/laptop, personal worker, corporate cloud, and third-party provider backends?
3. What Compartment rules, PII stripping, encryption, redaction, and provenance are required before any external backend receives work?
4. What must be open-core so FOSS contributors can inspect, run, modify, and self-host the planning loop without a hidden service?
5. How should third-party UbU-compatible providers interoperate without being privileged over local execution?
6. Should dedicated personal worker appliances be treated as commercial packaging, reference hardware, or future product work?
7. How should future practical FHE-backed or comparable encrypted compute be prioritized if it becomes viable?
8. Should idle compute monetization be an external API-enabled user choice rather than an UbU corporate marketplace?

### Current direction

Cloud or external compute may improve performance, granularity, and analysis depth, but must not be a hidden mandatory dependency for the open-core planning loop. User-owned worker devices are Phase 2. Dedicated appliances, corporate cloud, third-party providers, compute monetization, and FHE/private encrypted compute are future commercial or strategic-research directions.

### Resolution

Open.


---

## UBU-Q0060: External/cloud LLM provider abstraction and routing policy

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0028 Blocks: LLM provider routing, cloud LLM disclosure, UbUCorp inference boundary, BYOK configuration Resolved by: UBU-D0157 Last scored: Never Scored from commit: None

### Question

What is the minimum provider-neutral LLM routing model that supports local Ollama, user-configured BYOK cloud APIs, optional UbUCorp managed inference, user-owned remote workers, and future compatible providers while preserving Compartment policy and user sovereignty?

### Subquestions

1. What interface should normalize local and cloud model providers?
2. What provider metadata is required for location, cost, context window, capability, retention/disclosure profile, and safety behavior?
3. How should Compartment policy prevent `no_cloud_llm` or `no_external_export` payloads from crossing a provider boundary?
4. What context-minimization, redaction, and provenance fields are required before cloud routing?
5. How should BYOK mode store and scope provider credentials?
6. How should UbUCorp managed inference be represented without becoming a mandatory dependency of the open core?
7. What user-visible disclosure is required before a workflow uses a cloud LLM?

### Current direction

Cloud LLMs are optional execution providers, not the canonical planner. The FOSS core should remain local-capable, provider-neutral, BYOK-capable, and self-hostable. Cloud routing must be policy-governed, explicit, and advisory.

### Resolution

Resolved. See UBU-D0157.

---

## UBU-Q0061: Association object model and lifecycle

Status: Open Priority: Post-MVP Phase: Phase 3 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0054, UBU-Q0055, UBU-Q0056 Blocks: Phase 3 multi-user coordination, Association reconciliation, future skill-barter systems Resolved by: None Last scored: Never Scored from commit: None

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

Open.

---

## UBU-Q0062: AssociationAttestation provenance, trust, and dispute semantics

Status: Open Priority: Post-MVP Phase: Phase 3 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0061 Blocks: Association reconciliation, organizational introspection, pseudonymous reputation, multi-user trust Resolved by: None Last scored: Never Scored from commit: None

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

Open.

---

## UBU-Q0063: Organizational introspection MVP/dogfooding workflow

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0046, UBU-Q0049, UBU-Q0050 Blocks: ETHConf follow-up, public dogfooding artifacts, organizational introspection demo Resolved by: None Last scored: Never Scored from commit: None

### Question

What is the minimum useful Phase 1 workflow for dogfooding organizational introspection on the UbU project itself without implementing full Association automation?

### Subquestions

1. Which UbU artifacts should be used as evidence: design files, open questions, decisions, issue fixtures, outreach notes, follow-up Tasks, and public retrospectives?
2. What structured output should be generated: candidate AssociationAttestations, mission-alignment questions, priority-drift observations, follow-up Tasks, and redacted retrospective notes?
3. How should UbU ask whether its actual outreach behavior proves commitment to its stated goals?
4. Which parts can be manual or fixture-backed in Phase 1?
5. How should public artifacts avoid exposing private notes or contact details?
6. What acceptance criteria show that UbU successfully applied organizational introspection to itself?

### Current direction

Phase 1 should use EthConf outreach and UbU project records as a manually structured dogfooding case. The result should be an evidence-backed retrospective and follow-up plan, not an automated surveillance system.

### Resolution

Open.

---

## UBU-Q0064: SharedAssociationDescriptor model and reconciliation boundary

Status: Open Priority: Post-MVP Phase: Phase 3 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0061, UBU-Q0062 Blocks: Multi-party Association coordination, formal project descriptors, public organizational introspection Resolved by: None Last scored: Never Scored from commit: None

### Question

How should UbU represent a SharedAssociationDescriptor as a signed or reviewable shared artifact without turning it into a false global truth object?

### Subquestions

1. What fields belong in a descriptor: label, purpose, declared mission, public references, governance rules, participation criteria, disclosure policy, contribution paths, and signatures?
2. How should descriptors be versioned, superseded, forked, or disputed?
3. How should multiple Identities indicate acceptance, partial acceptance, rejection, or interpretation of a descriptor?
4. How should organizational introspection compare actual behavior to declared descriptors?
5. How should legal entity records and public registry references be linked as External References?
6. How should descriptors avoid implying that all affected members consented or that membership is objectively settled?

### Current direction

A SharedAssociationDescriptor is a coordination artifact. Multiple Identities may reference or sign it, but each UbU instance still maintains perspective-bound Association models and attestations.

### Resolution

Open.

---

## UBU-Q0065: Skill Barter marketplace and lawful private settlement boundary

Status: Open Priority: Post-MVP Phase: Post-MVP Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0054, UBU-Q0055, UBU-Q0056, UBU-Q0061, UBU-Q0062 Blocks: Future Skill Barter marketplace and delegated skilled-work features Resolved by: None Last scored: Never Scored from commit: None

### Question

How should UbU model the Skill Barter marketplace as a future specialization of Association coordination and the Delegation Substrate without becoming token-first, speculation-first, platform-custodial, exploitative, or illicit-market infrastructure?

### Subquestions

1. What primitives are required: pseudonymous Identity, capability claims, reputation attestations, scoped work agreements, bonds, escrow-like commitments, dispute workflows, and settlement references?
2. Which payment rails can be represented as External References without UbU becoming a custodian or money transmitter?
3. How should privacy-preserving settlement options be described without implying sanctions evasion, tax evasion, illicit services, or unlawful use?
4. How should UbU distinguish skill barter, paid work, grants, bug bounties, volunteer passion-project work, human executors, agentic executors, and General Contractor coordination?
5. What public language must outreach avoid?
6. What lawful-use and safety boundaries are required before implementation?

### Current direction

The Skill Barter marketplace is a future cypherpunk/privacy-builder recruiting hook and product direction. It is a specialization of Association modeling and the Delegation Substrate, not the root concept and not a Phase 1 marketplace commitment. MVP-relevant work should focus on Delegation Substrate primitives that can later support Skill Barter.

### Resolution

Open.

---

## UBU-Q0066: Minimal Phase 3 Message Context Envelope schema

Status: Open Priority: MVP important Phase: Phase 3 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0061, UBU-Q0062 Blocks: Phase 3 cross-user communication, message priority triage, UbU-to-UbU interoperability Resolved by: None Last scored: Never Scored from commit: None

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

Open.

---

## UBU-Q0067: Legacy communication adapter and UbU-to-UbU upgrade protocol

Status: Open Priority: Post-MVP Phase: Phase 3 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0066 Blocks: WhatsApp/SMS/Discord/IRC/Slack/email integration, viral interoperability, native contextual messaging Resolved by: None Last scored: Never Scored from commit: None

### Question

How should UbU integrate with legacy messaging systems so a flat external message can be locally interpreted by one UbU instance, and upgraded into UbU-native contextual messaging when both sender and receiver use UbU?

### Subquestions

1. Which legacy systems should be represented first as examples: WhatsApp, SMS, email, Discord, IRC, Slack, Matrix, or other systems?
2. What transport patterns are acceptable: side channel, attachment, linkable External Reference, embedded metadata, shared relay, or explicit native UbU transport?
3. How should two UbU instances discover that both sides support richer contextual messaging without creating spam, surveillance, or platform-policy violations?
4. How should raw legacy messages remain human-readable while contextual envelopes remain machine-readable?
5. How should UbU handle one-sided use, where only the local user has UbU?
6. What consent, disclosure, and Compartment checks are mandatory before sending structured context to another user?
7. How should invitation flows encourage adoption without coercive or spammy viral loops?

### Current direction

UbU should define a stable internal message envelope first, then bind it to legacy transports through adapters. Legacy integration can become a value-led adoption path, but must preserve user consent, recipient respect, and privacy boundaries.

### Resolution

Open.

---

## UBU-Q0068: Structured message extraction schema, validation, and model strategy

Status: Open Priority: Post-MVP Phase: Phase 3 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0066 Blocks: direct-message ingestion, group-chat ingestion, communication-to-Task conversion, Association evidence extraction Resolved by: None Last scored: Never Scored from commit: None

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

Open.

---

## UBU-Q0069: Personalized TTS, voice-profile descriptors, and anti-impersonation controls

Status: Open Priority: Post-MVP Phase: Post-MVP Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0066 Blocks: expressive asynchronous messaging, accessibility, voice-compressed communication Resolved by: None Last scored: Never Scored from commit: None

### Question

How should UbU represent optional voice or pronunciation metadata so text messages can be rendered as sender-like speech without creating unacceptable biometric, deception, or impersonation risk?

### Subquestions

1. What is the difference between a pronunciation descriptor, a style descriptor, a local voice profile, and a transferable voice model?
2. Which forms should UbU allow to cross Identity, Device, Zone, Compartment, or user boundaries?
3. How should users opt in, revoke, rotate, or restrict voice descriptors?
4. What disclosure, watermarking, provenance, or UI indicators are required when speech is synthesized rather than actually recorded?
5. How should UbU prevent voice synthesis from being treated as authentication or proof that the sender literally spoke the words?
6. Are there narrow accessibility-first versions that could be implemented earlier without creating broad impersonation risk?

### Current direction

Voice descriptors are a promising post-MVP communication and accessibility feature. They should be consent-gated, compartmentalized, separated from authentication, and designed with anti-impersonation controls from the start.

### Resolution

Open.

---


## UBU-Q0070: Skeleton Plan failure diagnostics and user clarification flow

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0014, UBU-Q0015, UBU-Q0016 Blocks: robust planning failure handling Resolved by: None Last scored: Never Scored from commit: None

### Question

When skeleton Plan generation fails, what exact diagnostic payload and user clarification flow should UbU produce?

### Subquestions

1. Which failure classes are required for MVP: missing starting state, impossible dependency, cyclic dependency, Static Task collision, insufficient Calendar window, unavailable resource, blocked External Event, or unknown precondition?
2. How should UbU explain the failed causal chain without overwhelming the user?
3. Which alternatives can UbU safely suggest: relax deadline, remove Task, add prerequisite, mark state already satisfied, choose alternate Technique, extend planning horizon, or ask for manual decision?
4. When should this become an immediate blocking prompt instead of a normal planning warning?

### Current direction

Skeleton Plan failure is a critical model-consistency failure. UbU should not proceed as if the Plan merely needs optimization. It should explain the cause and request clarification or a user choice.

### Resolution

Open.

---

## UBU-Q0071: Legitimization and semi-legitimization cost model

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0016, UBU-Q0053 Blocks: realistic candidate Plan search Resolved by: None Last scored: Never Scored from commit: None

### Question

How expensive is full legitimization, and what semi-legitimization heuristics are needed before full candidate validation?

### Subquestions

1. Which constraints belong to full legitimization versus cheap semi-legitimization?
2. Are affect budget, slack preservation, dependency fragility, user-mode compatibility, local repair, and legitimacy-delta estimates sufficient for MVP?
3. Is legitimacy binary, graded, or both?
4. How does the planner compare a high-value but brittle Plan against a lower-value but more humane Plan?
5. How are recuperative Tasks represented when they are required for Plan legitimacy rather than optional gap-filling?

### Current direction

The legitimized skeleton Plan is the baseline feasible Plan. If full legitimization is cheap, it can be used as a frequent validity oracle. If expensive, UbU needs approximate semi-legitimization before full validation of finalists.

### Resolution

Open.

---

## UBU-Q0072: GPU-aware planner kernels and solver selection

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0016 Blocks: practical planner implementation, mobile/desktop/cloud execution profile Resolved by: None Last scored: Never Scored from commit: None

### Question

Which parts of UbU planning should use CPU-exact logic, and which parts should use GPU-friendly search, simulation, scoring, or learned-model inference?

### Subquestions

1. Which solver/library candidates should be evaluated for skeleton validation, finalist validation, contradiction diagnosis, and candidate optimization?
2. Which candidate expansion, stochastic simulation, affect scoring, and robustness scoring operations can be batched for GPU execution?
3. What are the mobile GPU targets for Android and iOS, and what CPU fallback is required?
4. What desktop/laptop GPU path is appropriate for power users?
5. What cloud GPU path is appropriate for premium wide-horizon planning?
6. How does UbU enforce the rule that GPU search may propose but exact/conservative validation must certify?

### Current direction

GPU suitability is a first-class solver-selection criterion. CPU logic should certify hard constraints and explanations. GPU-capable approaches should handle expensive candidate evaluation, stochastic robustness, affect scoring, and premium cloud planning where practical.

### Resolution

Open.

---

## UBU-Q0073: Mobile stewardship metadata and MVP repair rules

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0016, UBU-Q0058 Blocks: mobile/local planning UX Resolved by: None Last scored: Never Scored from commit: None

### Question

What compact Calendar metadata and local repair rules are required so mobile UbU can preserve legitimacy and next-action clarity without full global optimization?

### Subquestions

1. What is the exact schema for protected, flexible, and disposable Task criticality?
2. What is the MVP schema for decision envelopes?
3. What conflict severity levels are required?
4. What explanation fragments should be cached with each Task or Plan segment?
5. What simple repair recipes are required for late Task, skipped Task, fatigue report, approaching Static Task, and missing prerequisite?
6. How is the last legitimate Plan stored and compared with current reality?
7. When should mobile ask the user, silently repair, or wait for desktop/cloud refinement?

### Current direction

Mobile should be a real-time steward of Plan legitimacy, user agency, and next-action clarity. MVP should include Task criticality, last legitimate Plan storage, simple repair rules, conflict severity, cached explanations, next-best-action mode, and basic decision envelopes.

### Resolution

Open.

---

## UBU-Q0074: Stochastic personality and affect-disruption model

Status: Open Priority: Research Phase: Post-MVP Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0053 Blocks: advanced affect-aware planning, personalized legitimization Resolved by: None Last scored: Never Scored from commit: None

### Question

How should UbU model user personality, current affect, and likely desired planning style as stochastic parameters without claiming false precision or reducing user sovereignty?

### Subquestions

1. What are the minimal user-mode categories: deep work, rest, socialization, autonomy, structure, switching, recovery?
2. How should current affect shift candidate Plan probabilities or legitimacy scores?
3. When should mood/affect changes be treated like External Event-like disruptions?
4. How should ex post Log review update these distributions?
5. How can the user explicitly choose to become more like a different preferred planning/personality pattern?
6. How does UbU avoid manipulative nudging or pseudo-therapeutic overreach?

### Current direction

Stochastic personality modeling is plausible, but not required for MVP. The MVP can use explicit user inputs, conservative defaults, and Log review prompts before learning deeper distributions.

### Resolution

Open.

---

## UBU-Q0075: Optional VoxPopuli EthConf demo boundary

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0050, UBU-Q0068 Blocks: optional EthConf public demo Resolved by: None Last scored: Never Scored from commit: None

### Question

What is the smallest safe optional VoxPopuli demo that lets a user describe an unstructured planning problem in natural language and review the LLM-produced structured UbU candidate output?

### Subquestions

1. What input prompt should invite open-ended user speech without implying therapeutic authority or full life modeling?
2. Which candidate outputs are safe to show: Objectives, Tasks, constraints, Preferences, assumptions, ambiguities, and suggested follow-up questions?
3. How does the UI show that LLM output is candidate structure, not canonical state?
4. What should be excluded from the demo to avoid overclaiming Phase 1 capability?
5. When should this be skipped so it does not displace higher-priority EthConf deliverables?

### Current direction

VoxPopuli is an optional EthConf/public demonstration and populist hook. It is not a replacement for the narrow Phase 1 bootstrap interview and should only be implemented if cheap relative to higher-priority dogfooding, contributor, and funder work.

### Resolution

Open.

---

## UBU-Q0076: Planning horizon, early-preparation bias, and short-horizon time discounting

Status: Open Priority: Research Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0016, UBU-Q0051 Blocks: detailed planning horizon policy, premium planning horizon design Resolved by: None Last scored: Never Scored from commit: None

### Question

How far beyond the visible Calendar window should UbU look, when should prerequisite work be front-loaded, and when does time discounting become relevant to detailed planning?

### Subquestions

1. What is the default internal look-ahead for a one-day visible Calendar?
2. When should a dependency chain be scheduled early rather than just in time?
3. What makes a prerequisite chain fragile enough to justify early preparation?
4. Should one week be the default upper bound for detailed local planning?
5. Which longer-horizon plans belong to premium cloud, user-owned worker, or research modes?
6. When should explicit temporal-discounting Preferences override the short-horizon zero-discount assumption?

### Current direction

The internal planning horizon may exceed the visible Calendar window. Fragile prerequisites should be completed as early as reasonably viable within short detailed horizons to avoid forced stressful choices later. For operational windows of one day to roughly one week, time discounting may usually be treated as negligible unless user Preferences say otherwise.

### Resolution

Open.

---

## UBU-Q0077: Realtime interaction session and candidate update schema

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0025, UBU-Q0031, UBU-Q0068 Blocks: realtime model adapters, discovery mode, meeting capture, interruption handling Resolved by: None Last scored: Never Scored from commit: None

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

### Resolution

Open.

---

## UBU-Q0078: Interruption, escalation, and discovery-mode consent policy

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0052, UBU-Q0077 Blocks: realtime UX, discovery mode, short-horizon reactive repair Resolved by: None Last scored: Never Scored from commit: None

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

## UBU-Q0079: MCP client/server capability and Compartment policy

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0028, UBU-Q0032, UBU-Q0060 Blocks: MCP-style integrations, external agents, Delegation Substrate APIs Resolved by: None Last scored: Never Scored from commit: None

### Question

What is the minimum MCP-style client/server integration boundary that lets UbU use external tools and expose narrow UbU capabilities without giving outside agents broad authority over the user's life model?

### Subquestions

1. Which UbU operations may be exposed as tools in Phase 1 or fixtures?
2. What capability fields are required: object scope, Objective subtree, Task set, operation kind, Compartment, Identity, time window, rate/cost limits, and review requirement?
3. How should external agents submit candidate updates rather than canonical writes?
4. How should tool calls be logged, audited, denied, retried, or rolled back?
5. What is the minimum developer-facing MCP fixture or adapter needed for dogfooding?

### Current direction

UbU should be both an MCP-style client and server, but every tool surface is bounded by user-owned policy, Compartment rules, capability grants, and review semantics.

### Resolution

Open.

---

## UBU-Q0080: Delegation Substrate MVP schema and self-reminder use

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0032, UBU-Q0060, UBU-Q0079 Blocks: Task delegation, Automation Worker assignment, solo Task formalization, future Skill Barter marketplace Resolved by: None Last scored: Never Scored from commit: None

### Question

What is the minimum Delegation Substrate schema that prepares Tasks for delegation while also helping the user perform solo Tasks more clearly?

### Subquestions

1. Which fields belong directly on Task versus a separate `DelegationPacket`?
2. What executor types are needed: self, local agent, remote/cloud agent, tool, human Identity, Association, General Contractor, or external provider?
3. What authority, evidence, privacy, expected-output, review, deadline, and escalation fields are required?
4. How should a solo Task use the same structure as a self-reminder without implying actual handoff?
5. Which subset is required for Phase 1 GitHub dogfooding and Automation Worker assignments?

### Current direction

The MVP should implement Delegation Substrate primitives where they support dogfooding, self-reminder clarity, and worker assignment. The full Skill Barter marketplace is not Phase 1.

### Resolution

Open.

---

## UBU-Q0081: General Contractor role and subdelegation semantics

Status: Open Priority: Post-MVP Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0080, UBU-Q0061 Blocks: multi-executor coordination, Skill Barter marketplace, larger Association workflows Resolved by: None Last scored: Never Scored from commit: None

### Question

How should UbU model a General Contractor who coordinates multiple human, agentic, tool, or Association executors toward an Objective?

### Subquestions

1. What authority can a General Contractor receive and subdelegate?
2. How are subordinate Tasks, executors, evidence, and status reports linked?
3. What review rights does the user or originating Association retain?
4. How are budget, privacy, Compartment, tool, and settlement constraints propagated?
5. How does UbU prevent a General Contractor from becoming an unbounded agentic authority?

### Current direction

General Contractor is a first-class delegated coordination role. It is not an ordinary executor field. Subdelegation requires explicit authority, provenance, evidence, and review semantics.

### Resolution

Open.

---

## UBU-Q0082: Skill Barter marketplace outreach, privacy tech, and MVP boundary

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0065, UBU-Q0080, UBU-Q0081 Blocks: EthConf cypherpunk outreach, Skill Barter future roadmap, privacy-tech positioning Resolved by: None Last scored: Never Scored from commit: None

### Question

How should UbU present the Skill Barter marketplace as an EthConf/cypherpunk outreach hook and future marketplace direction without over-scoping the MVP or implying token-first speculation?

### Subquestions

1. What is the cleanest language for voluntary coordination, sovereign identity, privacy, open markets, FOSS contribution, and pseudonymous capability?
2. How can Skill Barter attract younger developers with drive and time for FOSS contribution?
3. How should the marketplace direction connect to FHE, ZK, private reputation, private commitments, secure compute, and Ethereum-compatible privacy infrastructure?
4. What claims must be avoided in public outreach?
5. What is the smallest credible demo or diagram that shows the future direction while leaving Phase 1 focused on Delegation Substrate primitives?

### Current direction

Skill Barter is a future marketplace direction and “cool factor” outreach lane. It should not become a Phase 1 public marketplace commitment.

### Resolution

Open.

---

## UBU-Q0083: ContextBundle governance and long-context model routing

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0028, UBU-Q0060 Blocks: long-context LLM use, organizational introspection, repository/chat archive review Resolved by: None Last scored: Never Scored from commit: None

### Question

How should UbU represent and govern context assembly for LLMs and agents, especially when long-context models can ingest large archives or repositories?

### Subquestions

1. What fields should a `ContextBundle` contain?
2. How should Compartments, Identities, Associations, retention policy, provider destination, and minimization rules be recorded?
3. When must the user approve a ContextBundle before routing it to a model?
4. How should UbU summarize context exposure after a run?
5. How should ContextBundles link to downstream candidate updates and Logs?

### Current direction

Context assembly is a privacy-relevant act. More context is not automatically better context.

### Resolution

Open.

---

## UBU-Q0084: Computer-use AgentAction and BackgroundProcess model

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0031, UBU-Q0032, UBU-Q0060, UBU-Q0079 Blocks: background agents, scheduled agents, computer-use automation, prompt-injection controls Resolved by: None Last scored: Never Scored from commit: None

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

### Resolution

Open.

---

## UBU-Q0085: State-transition cockpit UX model

Status: Open Priority: Post-MVP Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0050, UBU-Q0077, UBU-Q0080 Blocks: post-MVP UX, generative UI, review workflows, candidate/canonical distinction Resolved by: None Last scored: Never Scored from commit: None

### Question

How should UbU evolve from Phase 1 one-next-Task focus into a state-transition cockpit without losing first-person legibility?

### Subquestions

1. What are the core state-transition screens: next Task, message reply, Log review, Plan repair, Delegation Substrate packet, agent action approval, AssociationAttestation review, organizational introspection, and projection publication?
2. How should candidate state differ visually from canonical state?
3. Where can generative UI be useful without becoming opaque or inconsistent?
4. What minimum cockpit elements should appear in Phase 1 explanations?
5. How should users inspect evidence, constraints, expected effects, authority, and rollback paths?

### Current direction

UbU should become a state-transition cockpit over time. Phase 1 remains the narrow one-next-Task proof.

### Resolution

Open.

---

## UBU-Q0086: Counterparty-perspective hypothesis model for extrospection

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0026, UBU-Q0067 Blocks: extrospection, Relationship review, relationship-scope transitions Resolved by: None Last scored: Never Scored from commit: None

### Question

How should UbU represent domain- and scope-tagged counterparty-perspective hypotheses without converting them into facts about the counterparty?

### Subquestions

1. What minimum fields belong on `counterpartyPerspectiveHypotheses`?
2. Which domains should be first-class: technical collaboration, emotional availability, professional boundary, money/value, authority, reciprocity, trust, intimacy, conflict style, or others?
3. How should hypothesis confidence, evidence refs, counterevidence refs, review status, and last-reviewed time be represented?
4. How should UbU detect internal tension in the user's hypothesis set without making claims about the counterparty's inner state?
5. When should a hypothesis be archived, superseded, or suppressed after rejection?

### Current direction

Use domain- and scope-tagged hypotheses. The attestation target is the user's hypothesis, not the counterparty's actual perspective.

### Resolution

Open.

---

## UBU-Q0087: Relationship declaration versioning and scope drift

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0026 Blocks: Relationship model, extrospection, RelationshipScopeTransition Resolved by: None Last scored: Never Scored from commit: None

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

## UBU-Q0088: Extrospection finding lifecycle and durable rejection semantics

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0062 Blocks: extrospection review, candidate/canonical distinction, state-transition cockpit Resolved by: None Last scored: Never Scored from commit: None

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

### Resolution

Open.

---

## UBU-Q0089: Extrospection safety thresholds and anti-paranoia design

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0086, UBU-Q0088 Blocks: proactive extrospection, scope-drift prompts, rumination detection Resolved by: None Last scored: Never Scored from commit: None

### Question

What evidence thresholds justify candidate findings about scope drift, boundary pressure, reciprocity imbalance, trust miscalibration, or rumination without amplifying paranoia?

### Subquestions

1. What corpus size, evidence count, recency, and salience thresholds are required before proactive prompting?
2. How should UbU search for confirming evidence when surfacing disconfirming evidence?
3. How should evidence decay in salience without deleting legitimate history?
4. When should high-frequency review without model update trigger rumination concerns?
5. How should UbU present uncertainty and false-positive risk in extrospection advisories?

### Current direction

UbU should be humble about interpretation, not timid about evidence. Behavioral-risk findings are advisory by default.

### Resolution

Open.

---

## UBU-Q0090: User-accepted discomfort and value-aligned negative affect

Status: Open Priority: Post-MVP Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0050, UBU-Q0089 Blocks: affective assessment, extrospection, relationship review Resolved by: None Last scored: Never Scored from commit: None

### Question

How should UbU represent reviewed, value-aligned discomfort in a Relationship, and when may new evidence reopen it?

### Subquestions

1. What fields belong in `userAcceptedDiscomfort`?
2. How should UbU distinguish harmful relationship patterns from uncomfortable but value-aligned growth, caregiving, mentorship, or duty?
3. What counts as sufficiently new evidence to reopen a previously accepted discomfort finding?
4. How should accepted discomfort interact with RelationshipScopeTransition Objectives?
5. How can UbU avoid repeatedly warning about discomfort the user has already reviewed?

### Current direction

Negative affect is not equivalent to dysfunction. UbU should separate AffectiveAssessment from normative and functional assessments.

### Resolution

Open.

---

## UBU-Q0091: Relationship, multi-party Relationship, and Association boundary

Status: Open Priority: Post-MVP Phase: Phase 3 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0061, UBU-Q0086 Blocks: multi-party extrospection, Association modeling, group relationship review Resolved by: None Last scored: Never Scored from commit: None

### Question

When should a multi-party interaction be modeled as a Relationship, an Association, or both?

### Subquestions

1. When is the primary object interpersonal interaction with specific Identities?
2. When is the primary object emergent group behavior, shared mission, institutional action, or collective coordination?
3. How should dyadic findings coexist with lower-confidence group-level findings?
4. How should a Relationship and Association over the same people reference each other?
5. What evidence corpus limits apply when the user is not party to all inter-party dynamics?

### Current direction

Use Relationship for interpersonal interaction with specific Identities, Association for emergent coordination, and allow both to coexist over the same people.

### Resolution

Open.

---

## UBU-Q0092: Typed Relationship evidence policies

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0028, UBU-Q0083 Blocks: extrospection evidence, Relationship evidence retention, compartment-aware review Resolved by: None Last scored: Never Scored from commit: None

### Question

What typed retention, redaction, and evidence-use policy enums are required for Relationship evidence?

### Subquestions

1. Which retention policies are needed: retain indefinitely, retain until Relationship closes, session-only, auto-delete after duration, retain summary only, or user review required?
2. Which redaction policies are needed: full text permitted, summary only, selector only, reference only, metadata only, or prohibited?
3. Which EvidenceUsePolicy values govern extrospection, introspection, AssociationAttestation, planning, export, and model training?
4. How should raw excerpts be displayed without becoming durable copied private text?
5. How should EvidenceRefs record Compartment, source, timestamp range, selector, generated summary, permission basis, and provenance?

### Current direction

Evidence policies must be typed and enforceable. Durable Relationship evidence should prefer references and summaries over copied private excerpts.

### Resolution

Open.

---

## UBU-Q0093: Extrospection-to-introspection handoff

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0050, UBU-Q0088 Blocks: introspection UX, extrospection UX, relationship review Resolved by: None Last scored: Never Scored from commit: None

### Question

When should a relational finding be routed into introspection because the evidence primarily concerns the user's own behavior?

### Subquestions

1. How should UbU identify that the user is driving scope drift, boundary violation, reciprocity failure, or trust miscalibration?
2. Which extrospection findings create candidate introspection findings?
3. How should evidence be reused without violating Compartment or EvidenceUsePolicy boundaries?
4. How should UbU avoid turning extrospection into blame-shifting toward the counterparty?
5. How should the user reject or accept the introspection handoff?

### Current direction

Extrospection should remain self-governance. Findings primarily about user behavior should hand off to introspection.

### Resolution

Open.

---

## UBU-Q0094: RelationshipScopeTransition model boundary and reverse references

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0026, UBU-Q0087 Blocks: RelationshipScopeTransition, Objective targeting, Relationship history Resolved by: None Last scored: Never Scored from commit: None

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

## UBU-Q0095: Process success and outcome observations in RelationshipScopeTransition

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0094 Blocks: consent-dependent transitions, Objective status, graceful nonachievement Resolved by: None Last scored: Never Scored from commit: None

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

Open.

---

## UBU-Q0096: RelationshipScopeTransition type taxonomy and review gates

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0094 Blocks: transition planning, review checkpoints, RelationshipScopeTransition UX Resolved by: None Last scored: Never Scored from commit: None

### Question

What transition types are needed, and what default advisory review gates apply to each?

### Subquestions

1. Are `unilateral`, `exploratory`, `mutual`, and `retroactive_scope_clarification` sufficient?
2. Which pre-action reviews should be recommended by default for each transition type?
3. How should romantic, professional, dependency-heavy, or power-asymmetric cases adjust the recommendations?
4. Which advisory checks may the user configure as self-imposed required gates?
5. How should bypass decisions be logged and surfaced for introspection?

### Current direction

Transition gates should be default-on advisory safeguards unless structural boundaries, external constraints, or user-configured required gates apply.

### Resolution

Open.

---

## UBU-Q0097: RelationshipScopeTransition step validation and counterparty autonomy

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0095 Blocks: Technique generation, Step validation, autonomy-preserving relationship planning Resolved by: None Last scored: Never Scored from commit: None

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

## UBU-Q0098: Manipulation-prevention taxonomy and affect-knowledge firewall

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0097 Blocks: RelationshipScopeTransition Technique generation, extrospection safety, relationship safeguards Resolved by: None Last scored: Never Scored from commit: None

### Question

What typed prohibited-strategy taxonomy, affect-knowledge firewall, epistemic-transparency rule, and single-iteration policy are required for RelationshipScopeTransitions?

### Subquestions

1. Which prohibited strategy types are required for MVP hooks and post-MVP validation?
2. How should UbU distinguish authentic self-expression support from optimized disclosure or persuasion scripting?
3. How should extrospection-derived affect knowledge be allowed for harm avoidance but not persuasion timing?
4. What is the epistemic transparency requirement for Techniques?
5. How should advisory warnings express uncertainty and classifier fallibility?

### Current direction

Extrospection-derived affect knowledge may be used for harm avoidance, not persuasion optimization. Behavioral-risk checks are advisory by default.

### Resolution

Open.

---

## UBU-Q0099: Power, vulnerability, pacing, and graceful nonachievement

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0094, UBU-Q0098 Blocks: romantic transitions, professional transitions, dependency-heavy transitions, graceful failure UX Resolved by: None Last scored: Never Scored from commit: None

### Question

How should UbU model power asymmetry, vulnerability asymmetry, pacing constraints, cooling periods, and graceful nonachievement paths?

### Subquestions

1. What values belong in `power_asymmetry` and `power_asymmetry_basis`?
2. How should romantic vulnerability, financial dependence, employment authority, housing dependence, caregiving dependence, emotional dependence, social status, age, and platform power be represented?
3. When should power asymmetry make a transition delayed, require stronger advisory review, or require external review?
4. What pacing constraints are needed: minimum time between Steps, observation periods, cooling periods, evidence requirements, and maximum prompt frequency?
5. What belongs in a graceful nonachievement path: terminal state, model update options, introspection handoff, affective support, and delayed review?

### Current direction

The model should include power/vulnerability fields and pacing constraints. Desired mutual outcome nonachievement is not user failure.

### Resolution

Open.

---

## UBU-Q0100: Structural hard boundaries versus behavioral-risk safeguards

Status: Open Priority: MVP blocker Phase: Phase 1 Decision type: Governance Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0028, UBU-Q0083 Blocks: safeguard policy, relationship safeguards, product integrity, user autonomy Resolved by: None Last scored: Never Scored from commit: None

### Question

How should UbU distinguish structurally enforceable hard boundaries from fallible behavioral-risk safeguards, so that it preserves user autonomy without falsely claiming to prevent misuse or illegal behavior?

### Subquestions

1. Which boundaries are true product invariants: Compartment, privacy, authorization, EvidenceUsePolicy, export, provenance, audit, identity isolation, and integration authorization?
2. Which constraints come from external providers, platforms, app stores, or law rather than UbU's own philosophy?
3. Which behavioral-risk checks are too fallible to act as hard gates by default?
4. How should user-configured required gates be represented without confusing them with product hard boundaries?
5. How should documentation avoid implying that UbU reliably prevents behavioral misuse?

### Current direction

Hard boundaries should be structural or externally required. Behavioral-risk checks should generally be advisory by default.

### Resolution

Open.

---

## UBU-Q0101: Advisory safeguard uncertainty, override, and introspection consequences

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0100 Blocks: safeguard UX, introspection, relationship-transition UX Resolved by: None Last scored: Never Scored from commit: None

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
          "DESIGN.md",
          "DECISIONS.md",
          "OPEN_QUESTIONS.md"
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
- Do not modify `README.md`, `OUTREACH.md`, hidden files, scripts, code files, or generated logs.
- Preserve the single-line metadata format in `OPEN_QUESTIONS.md`.
- The `patch` string must be a raw unified diff as produced by `git diff`; do not wrap it in markdown fences or prose.
- Every file diff must start with `diff --git a/<path> b/<path>`, followed by `--- a/<path>` and `+++ b/<path>`.
- Every hunk must include accurate `@@ -old_start,old_count +new_start,new_count @@` ranges and enough unchanged context for `git apply --check` to apply without `--recount`.
- When editing `OPEN_QUESTIONS.md`, anchor hunks with the selected question heading `## UBU-Q0008: Worker Assignment Model` and its own `### Resolution` section. Do not use a repeated `### Resolution` heading from an earlier or later question as the edit location.
- If resolving the selected question, replace the `Unresolved.` text under that selected question's `### Resolution` section and update that same question's metadata line. Do not insert selected-question resolution text into any other question block.
- Use the existing `UBU-Qxxxx` and `UBU-Dxxxx` numbering conventions.
- Prefer minimal, auditable changesets.
- If the selected question is blocked, propose decomposition only if it produces replacement questions with fewer, simpler, or no dependencies.
- If the selected question is already partially resolved, narrow or clarify it rather than pretending it is fully unresolved.

Return only JSON.
