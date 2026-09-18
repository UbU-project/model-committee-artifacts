# Model-Committee Work Proposal Request

You are participating in the UbU `model-committee` process.

Your task is to produce one concrete work proposal as strict JSON.

Do not return prose outside the JSON object.

## Selected question

Question ID: `UBU-Q0137`  
Question title: `Phase 1b BackgroundProcess model for unattended advisory batches`  
Base commit: `21d513d5b9dff6cabf4b8afcc9a0fc1bf7daccdf`

```markdown
## UBU-Q0137: Phase 1b BackgroundProcess model for unattended advisory batches

Status: Open Priority: MVP important Phase: Phase 1b Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0084, UBU-Q0130, UBU-Q0135 Blocks: Phase 1b batch runner, unattended advisory execution Resolved by: None Last scored: 2026-09-18 Scored from commit: None

### Question

What is the minimum `BackgroundProcess` model for Phase 1b's unattended advisory batch runner, which consumes compute and external credentials without occupying Calendar time?

### Subquestions

1. What fields does a Phase 1b `BackgroundProcess` need: operation kind, eligibility filter, pass caps, interval, and interrupt state?
2. How are per-object pass counts represented as per-object state rather than a single aggregate blob, so they can later be versioned and replicated?
3. How are interruption and resumption recorded as events rather than as in-memory state?
4. What notification or escalation policy applies when a batch run fails, produces nothing, or fails for every object?
5. Does a background advisory run occupy Calendar time, and if not, how is its compute cost and duration surfaced to the operator?
6. What prompt-injection exposure does an advisory batch incur when it consumes operator-authored lore and external message content, and how is that scored?
7. What is the contract for an unattended run that completes with no successful work, given that the operator may chain a power-off to a successful exit?

### Current direction

Phase 1b's advisory batch runner is the first real `BackgroundProcess`: unattended, interrupt-safe, pass-capped, and consuming credentials and compute outside Calendar time. The Quick UbU implementation already establishes incremental save per chunk, graceful interrupt, bounded reprocessing, and an exit contract usable for chaining a shutdown. Computer-use `AgentAction` semantics and irreversible-side-effect rollback metadata remain open under UBU-Q0084 and are not answered here.

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
- next decision id: `UBU-D0264`

`DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md` is read-only context and is not in the patch
allowlist: do not propose changes to it.

### OPEN_QUESTIONS.md — related questions

#### UBU-Q0014

```markdown
## UBU-Q0014: UniverseState Mutation Schema

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 implementation Resolved by: UBU-D0130 Last scored: Never Scored from commit: None

Resolved. See UBU-D0130.

---
```

#### UBU-Q0015

```markdown
## UBU-Q0015: Task Precondition Schema

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 implementation Resolved by: UBU-D0131 Last scored: Never Scored from commit: None

Resolved. See UBU-D0131.

---
```

#### UBU-Q0016

```markdown
## UBU-Q0016: Compact Calendar planner grammar and execution profile

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0014, UBU-Q0015 Blocks: Phase 1 implementation Resolved by: UBU-D0151 Last scored: Never Scored from commit: None

Resolved. See UBU-D0151.

---
```

#### UBU-Q0028

```markdown
## UBU-Q0028: Minimum Privacy / Compartment Promise for MVP

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 implementation Resolved by: UBU-D0074 Last scored: Never Scored from commit: None

Resolved. See UBU-D0074.

---
```

#### UBU-Q0031

```markdown
## UBU-Q0031: Log Structure and Fields

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 implementation Resolved by: UBU-D0071 Last scored: Never Scored from commit: None

Resolved. See UBU-D0071.

---
```

#### UBU-Q0032

```markdown
## UBU-Q0032: Model Committee Process and Authority

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Model-committee correctness Resolved by: UBU-D0057, UBU-D0058, UBU-D0059, UBU-D0060, UBU-D0065, UBU-D0069, UBU-D0070, UBU-D0150 Last scored: Never Scored from commit: None

Resolved. See UBU-D0057, UBU-D0058, UBU-D0059, UBU-D0060, UBU-D0065, UBU-D0069, UBU-D0070, UBU-D0150.

---
```

#### UBU-Q0059

```markdown
## UBU-Q0059: Execution-provider trust, worker backends, and privacy-preserving compute roadmap

Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: UBU-Q0016 Blocks: Phase 2 personal worker devices, hosted compute policy, third-party provider compatibility, Phase 1b advisory worker boundary Resolved by: UBU-D0254 Last scored: 2026-09-16 Scored from commit: None

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

Phase 1b re-scope: this question is pulled forward from Phase 2 under the Phase 1b rule, which requires every Quick UbU feature merged into mainline to adopt the restrictions, forms, and functional boundaries of the MVP design it corresponds to, even when that design's implementation phase is beyond Phase 1. Phase 1b emits artifacts whose shape depends on this answer, so it is answered now. Phase 1b scope: the local advisory worker (ollama) must be invoked as a worker behind a scoped `WorkerAuthority` and pass admission, never as a privileged internal service, so that a later remote or rented backend is a transport substitution rather than an advisory-layer rebuild. Subquestions 1, 2, 3, and 5 are Phase 1b-relevant. Subquestions 6, 7, and 8 (appliances, FHE, compute monetization) remain future commercial or strategic-research directions and are not answered by Phase 1b. The minimum local worker protocol is decomposed into UBU-Q0135.

### Resolution

Resolved by `UBU-D0254`: every non-local execution backend is an explicitly authorized, interchangeable advisory worker behind `WorkerAuthority`, Compartment/export admission, payload minimization, provenance, user-visible routing, and CPU certification on return. The open-core planner must remain self-hostable through the FOSS schemas, Compact Calendar representation, planning loop, and CPU admission/certification path; corporate hosted compute, third-party providers, appliances, FHE-like encrypted compute, and idle-compute monetization are optional future backends or packaging choices, not privileged dependencies.

---
```

#### UBU-Q0060

```markdown
## UBU-Q0060: External/cloud LLM provider abstraction and routing policy

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0028 Blocks: LLM provider routing, cloud LLM disclosure, UbUCorp inference boundary, BYOK configuration Resolved by: UBU-D0157 Last scored: Never Scored from commit: None

Resolved. See UBU-D0157.

---
```

#### UBU-Q0079

```markdown
## UBU-Q0079: MCP client/server capability and Compartment policy

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0028, UBU-Q0032, UBU-Q0060 Blocks: MCP-style integrations, external agents, Delegation Substrate APIs Resolved by: UBU-D0177 Last scored: 2026-05-26 Scored from commit: None

Resolved. See UBU-D0177.

---
```

#### UBU-Q0084

```markdown
## UBU-Q0084: Computer-use AgentAction and BackgroundProcess model

Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: UBU-Q0031, UBU-Q0032, UBU-Q0060, UBU-Q0079 Blocks: background agents, scheduled agents, computer-use automation, prompt-injection controls Resolved by: UBU-D0263 Last scored: 2026-09-18 Scored from commit: None

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

Resolved. See UBU-D0263.

---
```

#### UBU-Q0130

```markdown
## UBU-Q0130: Phase 1b mutation envelope causality and idempotency metadata

Status: Solved Priority: MVP blocker Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: UBU-Q0140, UBU-Q0131 Blocks: Phase 1b mutation paths, Phase 2 sync statement compatibility Resolved by: UBU-D0258 Last scored: 2026-09-16 Scored from commit: None

Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §4, §8, §9, §15.

### Question

What causality and idempotency metadata must every Phase 1b canonical mutation carry so that a single-Device Phase 1b mutation can later be wrapped as a Phase 2 `SyncStatement` without reshaping the emitted artifact?

### Subquestions

1. Which of the mandated fields are required on every Phase 1b mutation: idempotency key, `observed_versions`, origin Device, actor Identity, `authority_source`, and created, effective, and recorded timestamps?
2. How is `observed_versions` represented against the existing single integer version counter on canonical objects?
3. What abstract seam defers the Phase 2 ordering mechanism (hybrid logical clocks, Lamport clocks, vector clocks, per-object counters, signed append-only log, or content-addressed bundles) without leaking that choice into Phase 1b call sites?
4. How are `effective_time` and `recorded_time` distinguished for an unattended batch advisory run that proposes overnight and is admitted the following morning?
5. What is the deterministic duplicate-handling rule for a replayed idempotency key within a single Device?
6. Do derived artifacts (Plan, Calendar, reports) carry envelope metadata, or are they excluded as `derived_state`?

### Current direction

Phase 1b carries the frozen field set on every mutation while keeping the Phase 2 hybrid causality stack behind an admission-owned issuer rather than in domain call sites. Bitemporality is load-bearing in Phase 1b rather than theoretical, because unattended advisory batch runs propose at a materially different time than the operator admits, and clarify sessions span days. This is the first Phase 1b ticket's governing constraint: no later mutation path may emit an unshaped mutation.

### Resolution

Resolved by `UBU-D0258`: every Phase 1b canonical mutation carries a sync-ready mutation envelope with idempotency, observed-version preconditions, origin Device, actor Identity, authority source, created/effective/recorded timestamps, and policy-version observations when policy is relied on. Existing integer object versions serialize as per-object version refs inside `observed_versions`; ordering mechanics remain behind the admission-owned issuer seam; `effective_time` and `recorded_time` remain distinct for delayed admission; duplicate idempotency keys collapse only when the canonical payload matches; and derived artifacts remain `derived_state` unless separately admitted as canonical state.

---
```

#### UBU-Q0131

```markdown
## UBU-Q0131: Phase 1b Device and execution-context identity

Status: Solved Priority: MVP blocker Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: UBU-Q0148 Blocks: Phase 1b origin-Device attribution, Phase 2 Device registry Resolved by: UBU-D0257 Last scored: 2026-09-16 Scored from commit: None

### Question

What is a Device in Phase 1b, and how are physical devices, UbU Devices, and execution contexts distinguished so that origin-Device attribution on every mutation is stable and meaningful with a single operator on a single machine?

### Subquestions

1. Is a Phase 1b Device a persisted canonical object, a configuration record, or a derived identity?
2. Are the desktop instance, the CLI process, the unattended batch runner, and the advisory worker one Device or several execution contexts under one Device?
3. What Device identifier remains stable across reinstallation, database reset, and re-authorization of external integrations?
4. Does the Google Calendar surface count as a Device, an external projection surface, or neither?
5. What minimum Device registry must exist in Phase 1b, given that a registry of one must not encode assumptions that break at two or many?

### Current direction

Phase 1b has one Device but must attribute every mutation to it explicitly, because canonical state is derived from valid statements rather than from machine hierarchy and no Device is automatically the canonical source of all state. The Quick UbU assumption that the desktop is authoritative and the phone is a command-only client was a correct single-machine simplification and must not be re-encoded as an invariant. The Google Calendar surface is a projection, not a Device. Any Phase 1b answer must avoid a design that only works for one or two Devices.

### Resolution

Resolved by `UBU-D0257`: Phase 1b uses one persisted registered Device record for the operator-controlled execution enclave, with a stable `device_id` restored from local registration material rather than derived from hardware, app install, database contents, or external integration authorization. The desktop UI, CLI, unattended batch runner, and local advisory worker are execution contexts under that Device unless separately admitted with their own Device authority; Google Calendar is an external projection surface, not a Device. The Phase 1b registry may contain one entry but must use the future multi-Device shape, explicit trust/capability/Zone/Compartment fields, and default-deny semantics.

---
```

#### UBU-Q0135

```markdown
## UBU-Q0135: Phase 1b local advisory worker protocol

Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: UBU-Q0145, UBU-Q0059 Blocks: Phase 1b advisory operations, Phase 3 worker substitution Resolved by: UBU-D0255 Last scored: 2026-09-16 Scored from commit: None

### Question

What is the minimum worker protocol for a Phase 1b local advisory worker invoked through scoped authority and admission, such that a later remote worker is a transport substitution rather than a re-architecture?

### Subquestions

1. What `WorkerAuthority` scope does an advisory worker require, and what is it explicitly denied?
2. What submission and result envelope does an advisory call use, and by what path does its output reach admission?
3. How are worker failures, timeouts, and partial or malformed results represented without corrupting candidate state?
4. What model and provider identity, version, and configuration metadata must a submission carry for provenance?
5. What prevents an advisory worker from mutating canonical state directly, and how is that enforced rather than merely conventional?
6. Which parts of the protocol must remain unchanged when the transport becomes remote, rented, or multi-provider?

### Current direction

Phase 1b builds the local advisory worker behind a scoped authority and the admission boundary so that the marketplace phase becomes a transport substitution. The invariant that advisory proposals never directly mutate state is non-negotiable and already holds in the Quick UbU implementation, where the advisor only enqueues candidates. The general Phase 2 worker-Device protocol for user-owned remote Devices remains open under UBU-Q0145.

### Resolution

Resolved by `UBU-D0255`: Phase 1b local advisory workers use a scoped local request/result protocol under `WorkerAuthority`; the worker receives only minimized by-value payloads, returns proposal artifacts or diagnostics, and has no direct canonical-state mutation path. Local output reaches state only through the controller's candidate queue and ordinary admission, preserving the same authority, provenance, status, retention, and CPU-certification boundary required for later remote transport substitution.

---
```

#### UBU-Q0140

```markdown
## UBU-Q0140: Phase 2 causality mechanism for sync ordering

Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0130, Phase 2 sync statement compatibility Resolved by: UBU-D0247 Last scored: 2026-09-16 Scored from commit: None

Formerly UBU-QSYNC-002 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §8, §15.

### Question

What exact causality mechanism should Phase 2 use: hybrid logical clocks, Lamport clocks, vector
clocks, per-object version counters, content-addressed logs, or a hybrid?

### Resolution

Resolved by `UBU-D0247`: Phase 2 uses a hybrid causality stack: content-addressed signed sync statements form the durable append-only log; every statement carries a per-origin hybrid logical clock tick for compact total-order tie breaking and queue progress; `causal_parents` preserve the statement DAG; `observed_versions` and `observed_policy_versions` are per-object/version preconditions used for deterministic conflict detection, not the sole clock.

This rejects a pure Lamport clock because wall-time adjacency is useful for operator review, a pure vector clock because N-device and partial replicas make vectors too wide and leaky, and pure per-object counters because cross-object mutations and policy updates must retain causal edges.

---
```

#### UBU-Q0145

```markdown
## UBU-Q0145: Minimum useful worker Device protocol for Phase 2

Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0135, Phase 2 worker Device protocol Resolved by: UBU-D0250 Last scored: 2026-09-16 Scored from commit: None

Formerly UBU-QSYNC-007 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §20.

### Question

What is the minimum useful worker Device protocol for Phase 2?

### Resolution

Resolved by `UBU-D0250`: Phase 2 worker Devices use a scoped request/response protocol over normal sync statements. A controller sends a policy-checked `worker_request` with scope, purpose, allowed operations, input references or redacted context bundle, policy versions, retention deadline, and result schema. The worker may compute only within that scope and returns `worker_result` statements containing derived artifacts, candidate mutations, diagnostics, provenance, and deletion confirmation for transient payloads. Candidate mutations are never admitted state until normal sync-statement admission accepts them.

---
```

#### UBU-Q0148

```markdown
## UBU-Q0148: Distinguishing physical devices, UbU Devices, installs, and workers

Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0131, Phase 2 Device registry Resolved by: UBU-D0251 Last scored: 2026-09-16 Scored from commit: None

Formerly UBU-QSYNC-010 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §4, §5.

### Question

How should UbU distinguish physical devices, UbU Devices/execution enclaves, app installs, browser
sessions, and worker processes? Can one physical machine host multiple UbU Devices/enclaves, and
under what Identity/Compartment conditions?

### Resolution

Resolved by `UBU-D0251`: UbU Device identity is per registered execution enclave, not per physical machine, install, browser session, or process. Those substrates become separate UbU Devices only when intentionally admitted with their own `device_id`, trust state, capability profile, Zone membership, authorized Identity association, and effective Compartment access. One physical machine may host multiple Devices when isolation, Identity authority, and Compartment policy support treating each enclave independently. Projection-only app installs or browser sessions are not Devices, and workers are Devices only when they hold independent policy-scoped execution authority.

---
```

### DECISIONS.md — cited decisions

#### UBU-D0057

```markdown
## UBU-D0057: Model-committee automation is advisory and repo-driven

**Status:** Accepted → DESIGN.md §3

Superseded by `UBU-D0150`. See `UBU-D0150` for the accepted model-committee architecture and constraints.

---
```

#### UBU-D0058

```markdown
## UBU-D0058: Model-committee v0.1 is intentionally constrained

**Status:** Accepted → DESIGN.md §3

Superseded by `UBU-D0150`. See `UBU-D0150` for the accepted model-committee architecture and constraints.

---
```

#### UBU-D0059

```markdown
## UBU-D0059: Model committee outputs are weighted by capability and observed reliability

**Status:** Accepted → DESIGN.md §3

Superseded by `UBU-D0150`. See `UBU-D0150` for the accepted model-committee architecture and constraints.

---
```

#### UBU-D0060

```markdown
## UBU-D0060: Open questions are selected by answerability, automation-likelihood, importance, and risk

**Status:** Accepted → DESIGN.md §3

Superseded by `UBU-D0150`. See `UBU-D0150` for the accepted model-committee architecture and constraints.

---
```

#### UBU-D0065

```markdown
## UBU-D0065: Model-committee v0.1 uses provisional quorum and provider-failure rules

**Status:** Accepted → DESIGN.md §3

Superseded by `UBU-D0150`. See `UBU-D0150` for the accepted model-committee architecture and constraints.

---
```

#### UBU-D0069

```markdown
## UBU-D0069: Model-committee v0.1 uses Codex CLI as the primary model provider

**Status:** Accepted → DESIGN.md §3.1

`model-committee` v0.1 uses Codex CLI as the primary model provider for work proposal generation and work scoring.

The runtime calls `codex exec` with schema-constrained output. Prompts are passed through stdin. Final responses are written to JSON files. JSONL event streams and stderr are preserved in run logs.

Every runtime Codex call must pass `--skip-git-repo-check`.

`model-committee` does not pass deprecated `--disable web_search` flags. If web search must be disabled, that is handled through Codex configuration or profile state outside `model-committee`.

Codex must not directly modify repository files in v0.1. It produces JSON work proposals and score results. Patches are validated and selected by `model-committee`, then written as review artifacts.

---
```

#### UBU-D0070

```markdown
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
```

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

#### UBU-D0074

```markdown
## UBU-D0074: Phase 1 privacy promise is a minimal Compartment guardrail layer

**Status:** Accepted

Resolved question: `UBU-Q0028`.

Phase 1 implements Compartments as metadata-backed classification and routing guardrails, not as the full multi-device containment system intended for later phases. A Phase 1 Compartment can mark content as local-only, disallow cloud LLM routing, disallow external export, declare allowed integrations, and declare allowed Devices within the limits of the single local Device model.

The hard Phase 1 invariants are narrow: `no_cloud_llm` Compartment content must not be sent to cloud LLM routes; `no_external_export` Compartment content must not be exported, projected, or handed to workers except as redacted structural references; Compartment-marked content crosses boundaries only through an allowed route and user-visible action; and boundary-crossing attempts that reach UbU are recorded in Logs as allowed or denied. Sensitive Compartment content is referenced rather than embedded in ordinary WorkItem structure.

Phase 1 does not promise complete privacy isolation, cryptographic isolation, hardware attestation, secure multi-device partial replication, protection from a malicious local administrator, or automated retention deletion/redaction. Retention policies may be recorded, but enforcement beyond append-only Log retention is post-MVP unless a specific implementation later adds and discloses it.

Un-compartmented content is explicitly labeled `security_level: low`. Low-security content is not compartment-protected and may be routed to configured integrations or cloud LLM-backed Automation Workers only through a user-visible integration or worker action.

Phase 1 may claim local-first operation only in the limited sense that canonical planning state, Logs, and source-linked project model data live in the local single-user UbU instance by default. It must not claim local-only operation, Phase 2 sync, conflict handling, partial replication, or secure multi-device Compartment propagation. Phase 1 may claim cloud LLM usage is optional, explicit, integration-scoped, and advisory; cloud LLMs are not the canonical planner and must not receive `no_cloud_llm` Compartment content.

**Consequences:**

- Public messaging must distinguish privacy-first architecture from the narrower implemented Phase 1 privacy baseline.
- Full Compartment enforcement across sync, retention, device eligibility, and cryptographic isolation remains future work unless separately accepted and implemented.
- Implementations must label un-compartmented content as low-security rather than implying default protection.

---
```

#### UBU-D0130

```markdown
## UBU-D0130: UniverseState mutations use dotted targets and envelope-level provenance

**Status:** Accepted → DESIGN.md §11.3

MVP UniverseState mutation items use dotted string targets. The first target segment names the UniverseState collection: `facts`, `numeric_values`, `set_memberships`, or `event_markers`. Remaining segments form the lightweight namespaced key within that collection. Examples include `facts.github.issue.14.pipeline_state`, `numeric_values.affect.energy`, `set_memberships.github.issue.14.labels`, and `event_markers.relationship.rel_123.interactions`.

A mutation item has required `operation` and `target` fields. `payload` is required for all operations except `clear_fact`. `note` is optional for human-readable context. Mutation items do not carry item-level `confidence`, `source`, or `provenance` in MVP; those belong on the containing Task effect, Snapshot, Log entry, worker mutation request, External Reference, import artifact, or projection envelope.

Payload rules are operation-specific. `set_fact` accepts any JSON-compatible value. `clear_fact` has no payload. `increment_numeric` and `decrement_numeric` require numeric delta payloads. `add_membership` and `remove_membership` require JSON scalar member payloads, usually strings. `append_event_marker` requires a JSON object payload representing the lightweight marker.

Mutation lists are unconditional once the containing Task effect succeeds. Per-item conditions are not part of the MVP mutation schema; conditional behavior belongs in Task preconditions, effect success probability, planner branching, or worker/request validation. Implementations should validate the full mutation list before application and apply valid items in list order.

Mutation targets may include affect UniverseState keys in `user_mode`, subject to Snapshot precedence and user sovereignty rules. Organization-mode and worker-mode validators must reject intrinsic-affect mutations. Mutation targets may include Relationship-relevant UniverseState keys, including relationship interaction markers and maintenance facts, but they must respect Compartment policy, mode rules, provenance/logging envelopes, and user-acceptance requirements for private or inferred relationship claims. Task effects, workers, imports, and LLM-assisted flows must not silently overwrite user-declared private affect or Relationship truths.

---
```

#### UBU-D0131

```markdown
## UBU-D0131: Task preconditions use recursive all_of/any_of predicates

**Status:** Accepted → DESIGN.md §10.1

MVP Task preconditions use a recursive boolean object with `all_of` and `any_of` arrays for simple AND/OR composition. A precondition node may be a group node or a leaf predicate. A group node contains `all_of` or `any_of`, each holding one or more precondition nodes. A leaf predicate contains `target`, `predicate`, and optional `expected`.

Precondition targets use the same dotted UniverseState target convention accepted for mutations. The first segment names the UniverseState collection: `facts`, `numeric_values`, `set_memberships`, or `event_markers`. Remaining segments form the lightweight namespaced key inside that collection.

MVP predicates are `equals`, `member_of`, and `absent`. `equals` compares the target value to a JSON-compatible `expected` value. `member_of` checks whether the JSON scalar `expected` value is present in the target set membership. `absent` checks that the target is not present or has been cleared, and does not use `expected`. Numeric comparisons such as greater-than, less-than, ranges, thresholds, and arithmetic expressions are not in MVP.

Preconditions may reference `event_markers` for deterministic marker presence or equality checks. Preconditions may reference affect-related UniverseState keys in `user_mode`, subject to Snapshot precedence and user sovereignty rules; organization-mode and worker-mode validators must reject intrinsic-affect preconditions. Preconditions may reference Relationship-relevant UniverseState keys, including relationship interaction markers and maintenance facts, but private or inferred relationship claims remain subject to Compartment policy, mode rules, provenance/logging envelopes, and user acceptance where required.

A failed precondition makes the Task blocked for planning and execution. It does not make the Task invalid. `unschedulable` is a derived planner result when an otherwise valid Task cannot be placed in the current Calendar scope while satisfying preconditions and Calendar Logic. `invalid` is reserved for malformed Tasks, malformed precondition schema, forbidden targets, or canonical-state contradictions. Unknown, unavailable, or partially modeled preconditions are treated as absent in MVP unless the user or importer explicitly records a deterministic predicate.

---
```

#### UBU-D0150

```markdown
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
```

#### UBU-D0151

```markdown
## UBU-D0151: Compact Calendar MVP grammar uses skeleton, legitimization, bounded candidates, and repair metadata

**Status:** Accepted → DESIGN.md §§15.2.2, 16

See DESIGN.md §§15.2.2, 16.

---
```

#### UBU-D0157

```markdown
## UBU-D0157: LLM provider routing uses provider descriptors and policy-gated route decisions

**Status:** Accepted → DESIGN.md §2.11

See DESIGN.md §2.11.

---
```

#### UBU-D0177

```markdown
## UBU-D0177: Phase 1 MCP tools are capability-scoped candidate surfaces

**Status:** Accepted → DESIGN.md §21.4

See DESIGN.md §21.4.

---
```

#### UBU-D0247

```markdown
## UBU-D0247: Phase 2 sync uses hybrid HLC, DAG, and content-addressed statements

**Status:** Accepted → DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §8, §15. Resolves `UBU-Q0140`.

Phase 2 sync ordering uses a hybrid causality mechanism rather than selecting a single scalar clock.

Canonical components:

- The durable unit is the signed or integrity-protected `SyncStatement`; accepted statements are retained as an append-only, content-addressed log or bundle record.
- Each statement has a deterministic content address computed over its canonical signed payload, excluding replica-local metadata such as `received_time` and `admitted_time`.
- Each origin Device maintains a hybrid logical clock. The HLC tick is included in the statement payload and advances on local statement creation and on admitting remote causal parents.
- `causal_parents` define the statement DAG and are the authoritative happened-before evidence across Devices.
- `observed_versions` and `observed_policy_versions` are per-object preconditions used to detect stale writes, policy races, and deterministic conflicts; they are not a replacement for the statement DAG.
- Admitted-state application order is a deterministic topological traversal of available causal parents, with ties broken by HLC tick, origin Device ID, and sync statement ID.

The HLC tick is an ordering aid and operator-review affordance, not an authority source. It never overrides missing causal parents, observed-version failures, policy failures, or invalid statement integrity. `effective_time`, `recorded_time`, `received_time`, and `admitted_time` retain their separate meanings and must not be collapsed into the causality clock.

Rejected alternatives:

- Pure Lamport clocks lose useful physical-time adjacency for review and diagnostics while still requiring deterministic tie breakers.
- Pure vector clocks are too large and privacy-leaky for N-device sync with partial, redacted, stale, or restricted replicas.
- Pure per-object counters cannot represent cross-object mutations, policy races, worker results, or conflict-resolution statements without an additional statement-level causal graph.
- Pure content-addressed bundles provide integrity and deduplication but do not, by themselves, encode happened-before ordering.

This decision is design-compatible with direct peer, local LAN, removable-file, and encrypted indirect transports because the causality evidence lives in the signed statement payload and its content address rather than in any canonical server.

---
```

#### UBU-D0250

```markdown
## UBU-D0250: Minimum useful worker Device protocol is scoped request/result over sync

**Status:** Accepted → DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §20. Resolves `UBU-Q0145`.

For Phase 2, the minimum useful worker Device protocol is a policy-checked request/result exchange carried as sync statements, not an independent mutation channel. A controlling Device may issue a `worker_request` only after evaluating current Compartment policy and Device authority.

A `worker_request` contains:

- `request_id`, `controller_device_id`, and `target_worker_device_id` or equivalent opaque Device references;
- `zone_id`, allowed `compartment_ids`, redaction level, purpose, and policy-version references used to authorize the work;
- `allowed_operations`, limited in Phase 2 to candidate planning, simulation, local extraction, summarization, projection preview preparation, and candidate-mutation generation;
- input object references and optional scoped context-bundle digests, with payloads redacted or omitted when policy requires;
- retention deadline and required transient-payload deletion confirmation;
- expected `worker_result` schema, result size limits, and whether partial results are allowed;
- causal parents and observed versions needed for deterministic admission and stale-result detection.

A worker Device must reject the request if its current policy view is missing, stale in a way that could expand access, revoked, or incompatible with the requested Compartment, export, retention, or `no_cloud_llm` constraints. The worker may not broaden the scope, fetch extra protected context on its own authority, retain transient payloads beyond the request, or directly mutate admitted state.

A `worker_result` contains `request_id`, worker identity, the policy versions used, input digests or structural references, operation status, diagnostics safe at the request's visibility level, derived artifacts, projection previews, `worker_result` statements, candidate mutations, and transient-payload deletion confirmation. Candidate mutations are proposals only: they become admitted state solely through the normal sync-statement admission path, including conflict detection, policy checks, provenance, and user-review requirements.

This is intentionally narrower than a general distributed worker marketplace protocol. It is sufficient to let user-owned Devices contribute compute while preserving the §20 boundary that workers can prepare and propose but cannot expand authority or bypass admission.

---
```

#### UBU-D0251

```markdown
## UBU-D0251: Device identity is per execution enclave, not per physical machine

**Status:** Accepted → DESIGN.md §23.1. Resolves `UBU-Q0148`.

A UbU Device is a registered execution enclave. A physical machine, app install, browser session, or worker process is not automatically a Device; it is a substrate or runtime context until UbU admits it as a separately identifiable enclave with its own `device_id`, trust state, capability profile, Zone membership, and effective Compartment access.

One physical machine may host multiple UbU Devices when each enclave is intentionally registered, has isolation appropriate to its claimed trust and capabilities, is associated with an authorized Identity, belongs to exactly one Zone, and receives only policy-authorized Compartment knowledge and payload access. Separate OS profiles, containers, VMs, secure enclaves, browser profiles, removable-import environments, and worker runtimes may therefore be separate Devices.

An app install or browser session that only presents another Device's state is a projection surface or execution context, not a Device. A worker process is a Device only when it holds independent execution authority and participates under Device policy; otherwise it is a child process of the controlling Device. Worker candidate mutations still require normal admission and cannot expand Identity, Zone, or Compartment authority.

---
```

#### UBU-D0254

```markdown
## UBU-D0254: Execution backends are interchangeable advisory workers behind authority, minimization, and certification

**Status:** Accepted → DESIGN.md §16.8. Resolves `UBU-Q0059`.

UbU execution providers beyond the currently controlling local instance are advisory workers, not privileged planning authorities. A backend may propose candidate Plans, simulations, summaries, extraction results, projection previews, candidate mutations, diagnostics, or repair suggestions only within an explicit `WorkerAuthority`. It cannot directly mutate canonical state, broaden its own scope, fetch additional protected context, decide Compartment export policy, or bypass admission. Returned artifacts become usable only after the controlling CPU kernel validates schema, policy provenance, hard constraints, skeleton validity, affect or legitimacy claims, and any candidate mutations through the ordinary admission path.

The open-core planning loop must remain self-hostable without a hidden service. The FOSS boundary includes the canonical planning schemas, Compact Calendar representation, deterministic skeletonization and precondition/effect evaluation, conservative CPU hard-constraint validation, admission and provenance checks, payload-safety validation, and enough local execution profile support to produce a legitimate default Plan or a blocking diagnostic. Hosted, rented, GPU, learned-model, or third-party compute may improve speed, branch coverage, candidate diversity, or analysis depth, but they are substitutions behind the same request/result boundary, not requirements for correctness or access to the user's state.

Every worker invocation carries a scoped authority envelope with the backend kind, provider or Device identity, requested capability, purpose, allowed `zone_id` and `compartment_ids`, redaction level, policy-version references, retention deadline, export/network/tool permissions, compute budget, model or solver identity when applicable, request ID, causal parents, and expected response schema. Worker capabilities are additive and narrow: candidate planning, simulation, local extraction, summarization, projection-preview preparation, and candidate-mutation generation are separate grants. Absence of a grant means denial.

Routing chooses the least-disclosing backend that satisfies the requested capability, policy, freshness, latency, energy, and compute-budget requirements. Mobile local execution is the stewardship fallback for current/next Task clarity and cached repair. Local desktop/laptop execution is the default high-fidelity self-hosted path when available. Phase 2 user-owned workers may receive work through the sync-carried worker protocol. Corporate hosted compute and third-party UbU-compatible providers require explicit policy allowance and user-visible routing disclosure. Third-party providers use the same APIs, safety proof, provenance, and return validation as UbU-hosted providers and receive no privileged access over local execution.

Before any external backend receives work, the controller must evaluate Compartment policy, export limits, Device or provider trust, `no_cloud_llm`-style constraints, retention rules, and purpose compatibility. The transmitted payload is a minimized `PlanningRequest`, `worker_request`, or derived context bundle over a redacted Replica: direct PII, stable object IDs, Compartment labels, secrets, integration credentials, and unrelated UniverseState are omitted, generalized, or replaced with redacted handles unless policy explicitly authorizes disclosure. Transport must be encrypted; retained payloads must obey the request retention deadline; diagnostics and logs must avoid reintroducing stripped identifiers. The request records a payload-safety proof or equivalent provenance summary, and the result echoes the policy versions, input digests or structural references, provider/model identity, telemetry safe for the authority level, and deletion confirmation when transient payload deletion was required.

Dedicated personal worker appliances are future packaging or reference-hardware work, not a special trust class. They must register and operate as ordinary user-owned worker Devices. Future practical FHE-backed or comparable encrypted compute is a strategic research backend class: it is worth tracking and may reduce disclosure when viable, but it must still use the same authority, capability, provenance, routing, and CPU-certification boundary unless a later accepted design proves stronger semantics. Idle-compute monetization, if ever supported, should be an external API-enabled user choice with explicit opt-in, revocation, policy limits, and auditability rather than an UbU corporate marketplace or a default use of personal compute.

---
```

#### UBU-D0255

```markdown
## UBU-D0255: Phase 1b local advisory worker uses scoped request/result envelopes

**Status:** Accepted → DESIGN.md §16.8. Resolves `UBU-Q0135`.

Phase 1b local advisory workers use the execution-backend boundary from `UBU-D0254` as a concrete local protocol, not as an internal privileged service. The controller creates a `LocalAdvisorySubmission` for each advisory call. The submission contains `request_id`, `operation`, `purpose`, causal parents, observed state or request versions, timeout and compute budget, expected result schema, result size limit, whether partial results are allowed, an authority envelope, a minimized payload, and provenance/configuration metadata. The payload is by value: a `PlanningRequest`, redacted Replica slice, projection-preview bundle, extraction bundle, or other declared context bundle assembled by the CPU/controller after Compartment and export checks.

The `WorkerAuthority` grants exactly one or more additive advisory capabilities: candidate planning, simulation, local extraction, summarization, projection-preview preparation, repair suggestion, or candidate-mutation generation. It also records backend kind `local_advisory`, provider/runtime identity, allowed `zone_id` and `compartment_ids`, redaction level, policy-version references, retention deadline, export/network/tool permissions, compute budget, and response schema. Anything not granted is denied. A Phase 1b local advisory worker is explicitly denied canonical-state writes, admission decisions, direct StateStore handles, additional protected-context fetches, authority expansion, Compartment/export policy decisions, stable identifier disclosure beyond the payload, integration credentials, external network/tool use unless specifically granted, and retention after the declared deadline.

Model provenance is required when the worker uses an LLM, solver, embedding model, or learned component. The submission and result record `provider_id`, backend adapter name and version, model name, model version or digest when available, runtime or endpoint identity, generation parameters that can affect output, prompt/template or tool schema version refs, RNG seed when applicable, local binary/container/build identifier when available, and safe telemetry such as elapsed time and token or resource counts. These fields are provenance and replay aids, not authority grants.

The worker returns a `LocalAdvisoryResult` that echoes `request_id`, authority id or digest, policy versions, input digests or structural refs, provider/model identity, status, diagnostics, safe telemetry, produced artifacts, proposed Plan candidates, proposed candidate mutations, and deletion confirmation when transient payload deletion was required. Status is one of `ok`, `partial`, `rejected`, `timeout`, `worker_error`, `malformed_result`, or `cancelled`. `partial` is valid only when the submission allowed partial results and each included artifact independently passes schema validation. Timeouts, errors, cancellation, and malformed results are represented as diagnostics and optional quarantined raw-result digests; they do not create or mutate candidates.

The only path from advisory output to canonical state is the controlling CPU/admission path. The controller may translate schema-valid artifacts into candidate-queue entries, projection previews, diagnostics, or candidate mutation envelopes, each carrying the submission/result provenance. Candidate mutations are proposals only and are admitted, rejected, or routed to user review through ordinary admission, policy, provenance, hard-constraint, affect/legitimacy, and mutation validation. The worker process or function is not passed a mutable canonical-state capability; implementation interfaces expose only the submission and a result return channel, while the StateStore/admission writer remains CPU-owned. This is an enforcement requirement, not a convention.

When transport later becomes remote, rented, or multi-provider, the invariant fields remain unchanged: scoped authority, payload minimization proof, request/result ids, capability set, expected schema, status semantics, provenance/model identity, input digests, retention/deletion semantics, result size and partial-result rules, and CPU/admission certification. Remote transport may add encryption, signatures, provider attestations, sync-carried `worker_request`/`worker_result` statements, and stronger deletion evidence, but it must not introduce a second mutation channel or move policy/admission authority to the worker.

---
```

#### UBU-D0257

```markdown
## UBU-D0257: Phase 1b has one persisted Device and several execution contexts

**Status:** Accepted → DESIGN.md §23.1. Resolves `UBU-Q0131`.

Phase 1b has one UbU Device: the registered operator-controlled execution enclave that owns the local StateStore and admission path. It is a persisted registry record, not a physical-machine identity, not an app-install identity, not a browser/session identity, and not a value derived from the database contents or external integrations.

The Phase 1b `device_id` is assigned at Device registration and restored from operator-controlled local registration material, such as a config or secret-store record outside the ordinary mutable database. Reinstallation, database reset, and external integration reauthorization preserve Device continuity only when that registration material is preserved or restored. If it is lost, the next installation is a new Device and must not claim the old `device_id` merely because it runs on the same physical machine.

Desktop UI sessions, CLI processes, unattended batch runs, and the local advisory worker are execution contexts under the Phase 1b Device unless one is explicitly admitted later as its own Device with independent trust, capability, Zone, Identity, and Compartment policy. Their mutations and candidate mutations use the controlling Device's `origin_device_id`; context ids, process ids, request ids, backend ids, and model provenance may refine auditability but do not confer Device authority.

External projection surfaces, including Google Calendar, are not Devices in Phase 1b. They may receive projected state, provide external events or integration evidence, and appear in provenance as integration or projection surfaces, but they do not receive a UbU `device_id` and cannot be the origin Device for admitted mutations.

The minimum Phase 1b Device registry may have one row, but it must be modeled as the future registry rather than as a singleton constant. It records `device_id`, label/kind, registration metadata, registered Identity association, trust state, sync state, exactly one Zone membership, capability profile, effective Compartment access summary, and last-seen or local-observed timestamp. The single entry is not proof of global authority, canonical-source status, or permission to know every Compartment; unlisted Compartment access remains default-denied.

---
```

#### UBU-D0258

```markdown
## UBU-D0258: Phase 1b canonical mutations carry sync-ready envelopes

**Status:** Accepted → DESIGN.md §23.1. Resolves `UBU-Q0130`.

Every Phase 1b canonical mutation carries a mutation envelope before it enters the StateStore/admission writer. The required Phase 1b fields are `idempotency_key`, `observed_versions`, `origin_device_id`, `actor_identity_id`, `authority_source`, `created_time`, `effective_time`, and `recorded_time`; `observed_policy_versions` is also required whenever the mutation relies on Compartment, Zone, projection, routing, or other policy state. Optional execution-context and backend provenance may refine auditability but cannot replace origin Device, actor Identity, or authority source.

`observed_versions` is a map from canonical object id to the version reference observed for that object. The Phase 1b representation derives that reference from the existing per-object integer counter, serialized canonically as `v<N>` for existing objects and as an explicit absence precondition for creates or operations whose correctness depends on non-existence. It is a precondition set, not the ordering clock and not a global version.

Phase 1b mutation call sites do not name hybrid logical clocks, Lamport clocks, vector clocks, append-only logs, or content-addressed bundles. They ask an admission-owned causality/idempotency issuer for the envelope; that seam returns the frozen provenance, precondition, idempotency, timestamp, and opaque local-causality fields needed by the current single-Device reducer. Phase 2 may implement the same seam with the `UBU-D0247` hybrid stack, including HLC ticks and causal parents, without changing domain mutation APIs or reinterpreting existing Phase 1b fields.

`created_time`, `effective_time`, and `recorded_time` are distinct. `created_time` records when the mutation artifact was first assembled, `effective_time` records the domain time the mutation asserts for the changed fact or decision, and `recorded_time` records when the controlling Device durably records the mutation for admission. If an unattended advisory batch proposes a candidate at 02:00 and the operator admits it at 09:00, the admitted mutation may preserve 02:00 as created/effective time while using 09:00 as recorded time.

Within one origin Device, `(origin_device_id, idempotency_key)` is the duplicate-detection key. Replaying it with the same canonical payload returns the previously recorded result and performs no new write, version increment, side-effect emission, projection push, or derived-state invalidation. Replaying it with a different canonical payload is an `idempotency_key_conflict`; the new payload is not admitted except through an explicit conflict-resolution mutation.

Derived artifacts such as Plans, Calendars, reports, risk summaries, and projection previews are `derived_state`. They carry lineage, provenance, input digests, freshness, and invalidation metadata, but not the canonical mutation envelope unless UbU admits the artifact itself as canonical state. The mutation that records, invalidates, or publishes such an artifact carries the envelope.

---
```

#### UBU-D0263

```markdown
## UBU-D0263: Computer-use agents use separate AgentAction and BackgroundProcess records

**Status:** Accepted → DESIGN.md §21.8. Resolves `UBU-Q0084`.

UbU models external computer-use work with two records rather than treating every autonomous run as Calendar time. A `BackgroundProcess` is durable standing policy for when work may run; an `AgentAction` is one attempted dispatch, candidate dispatch, retry, cancellation, or mitigation step under that policy.

An `AgentAction` records action identity, actor and agent/tool refs, purpose, related work refs, target surface, operation kind, payload summary or hash, expected external version, idempotency key, authority source, capability and credential-handle refs, Compartment and disclosure constraints, route decision refs, prompt-injection assessment, estimated and actual budget use, side-effect class, review/approval state, result refs, completion evidence, downstream candidate refs, Log refs, failure reason when safe, and rollback or mitigation metadata.

A `BackgroundProcess` records process identity, owner, purpose, lifecycle status, trigger and recurrence policy, run windows, event subscriptions, concurrency limits, retry/backoff and cancellation policy, catch-up behavior, allowed action kinds and target surfaces, provider/tool classes, capability grants, credential handles, Compartments, ContextBundle templates, compute/rate/money/privacy budget caps, prompt-injection ceiling, isolation mode, review threshold, output-admission policy, notification policy, escalation path, Calendar projection policy, audit policy, retained evidence, and cleanup obligations.

Prompt-injection exposure is scored before dispatch and revised after execution. The score considers source trust, untrusted webpages/messages/documents/tool outputs, mixed-Compartment context, raw private-context volume, whether untrusted text can influence tool choice or arguments, and the authority at risk: credentials, spending, external mutation, privacy export, or user-facing commitments. The minimum bands are `low`, `medium`, `high`, and `critical`; high exposure requires containment and critical exposure fails closed unless a specific approved policy permits the run inside hard gates.

External side effects are classified as `read_only`, `local_candidate_only`, `externally_reversible`, `compensating_action_possible`, `externally_irreversible`, or `unknown`. Anything beyond read-only or local candidate generation needs expected prior state when available, idempotency, reconciliation evidence, and the review gate that admitted it. Irreversible, unknown, or compensation-only effects require mitigation owner, notification rule, cancellation or revocation path, compensating operation when available, external contact path when relevant, mitigation deadline, and Log or candidate records to write if mitigation is needed.

Background processes enter the Calendar only when they reserve or require user attention, represent a user commitment, or make the user materially unavailable. Compute-only work, polling, sync, advisory analysis, candidate generation, projection reconciliation, and scheduled external checks remain separate process state, though they may create Calendar overlays, deadlines, reminders, or review Tasks.

Dispatch rechecks current Compartment policy, credential version, capability grant, budget, lifecycle status, and expected external version immediately before use. Logs remain append-only; failed, denied, cancelled, mitigated, and superseded actions are recorded rather than rewritten.
```

### DESIGN.md — referenced sections

#### DESIGN.md §2.11 — Cloud LLM provider boundary

```markdown
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
```

#### DESIGN.md §3 — Model-committee dogfooding

```markdown
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

### 3.11 Feature-to-data map

Alongside bootstrap-dependency build ordering, the project maintains a **feature-to-data map** (`UBU-D0224`): for each feature, the data components it actually invokes at runtime versus those merely present in the model. As a worked example, the smart-Focus-mode decision invokes UniverseState, introspected affect, declared rules, divergence detection, dependency resolution (indirectly), and legitimization — and does not invoke extrospection, commitment tracking, or the privacy wire. The map is a legibility instrument: it makes "this feature needs nearly everything" falsifiable, prevents quiet scope creep, and gives reviewers a precise picture of what a slice depends on. It is a planned full-product artifact, not a Phase 1 deliverable.

---
```

#### DESIGN.md §10.1 — Preconditions

```markdown
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

**Phase 1 realization (`UBU-D0241`).** The deterministic evaluator — recursive `all_of`/`any_of`, leaf predicates `equals`/`member_of`/`absent`, unknown or partially modeled targets treated as absent, malformed preconditions surfaced as evaluation errors — is implemented as a pure `ubu-core` function over a UniverseState. Attaching `preconditions` to the Task schema and consuming the evaluator are realized in Wiring-A (`UBU-D0242`); effect application and bootstrap fact-recording remain.

**Phase 1 realization (`UBU-D0242`, Wiring-A).** A Task carries optional `preconditions`, and the orchestrator evaluates them against the current UniverseState via the pure `ubu-core` evaluator during request building, partitioning Tasks: satisfied or absent → eligible, a failed precondition → **blocked** (excluded from planning and surfaced), a malformed precondition → **invalid** (surfaced distinctly). Only eligible Tasks reach the kernel, which is unchanged. This realizes the §10.1 blocked result in the orchestrator; the planner's placement-based `unschedulable` is a separate concern, untouched.
```

#### DESIGN.md §11.3 — Mutation vocabulary

```markdown
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

Targets are dotted strings. The first segment names the UniverseState collection being changed: `facts`, `numeric_values`, `set_memberships`, or `event_markers`. Remaining segments form the lightweight namespaced key inside that collection. Example targets include `facts.github.issue.14.pipeline_state`, `numeric_values.affect.energy`, `set_memberships.github.issue.14.labels`, and `event_markers.relationship.rel_123.interactions`. The first post-collection segment is a controlled-vocabulary `<subject>` governed by `UBU-D0243`, the final segment is a snake_case `<predicate>`, and any segments between form an entity/instance path.

Payload rules by operation:

- `set_fact`: any JSON-compatible value;
- `clear_fact`: no payload;
- `increment_numeric` / `decrement_numeric`: numeric delta payload;
- `add_membership` / `remove_membership`: JSON scalar member payload, usually a string;
- `append_event_marker`: JSON object payload describing the lightweight marker.

A Task effect's mutation list is unconditional once that effect succeeds. Conditional behavior belongs in Task preconditions, effect success probability, or planner branching, not in individual mutation items. Implementations should validate the full mutation list before applying it and apply valid items in list order.

Mutation targets may include affect-related UniverseState keys in `user_mode`; organization and worker modes must reject intrinsic-affect mutations. Mutation targets may include Relationship-relevant UniverseState keys, but inferred or worker-generated relationship facts remain subject to Compartment policy, mode rules, provenance/logging envelopes, and user acceptance where required. Task effects should not silently overwrite user-declared private affect or Relationship truths.

**Phase 1 realization (`UBU-D0242`, Wiring-B).** A minimal `InstanceMode` (the §5 modes) and a pure `ubu-core` validation enforce the organization/worker-mode rejection of intrinsic-affect targets for **both** preconditions (§1585) and mutations (this section). A target is intrinsic-affect by namespace — the segment after its collection is `affect` (e.g. `numeric_values.affect.energy`). In `user_mode` such targets are permitted; in organization or worker mode they make the Task `invalid`. The orchestrator passes the instance mode (`user_mode` for the MVP single-user instance), so the rejection is dormant but encoded and tested. The user-declared-private-truths guard above remains deferred (it needs a provenance/acceptance model).

---
```

#### DESIGN.md §15.2.2 — Skeleton Plans and legitimization

```markdown
### 15.2.2 Skeleton Plans and legitimization

The planning pipeline begins with a **skeleton Plan**. A skeleton Plan affixes all Static Tasks and recursively walks backward through dependency DAGs from terminal Static Tasks to dependency roots, then schedules root prerequisites before their dependents. The skeleton Plan is a causal/dependency foundation, not a complete optimized user Plan.

A dependency is a requirement that some state be true before a dependent Task begins. During skeletonization, UbU must check whether that state is already satisfied by the initial UniverseState before inserting prerequisite Tasks. If a viable skeleton Plan cannot be created, the planning model is blocked, contradictory, incomplete, or impossible enough that UbU should stop ordinary planning and ask the user for clarification with a concrete diagnostic explanation.

After skeletonization, UbU performs **legitimization**. Legitimization adds the minimum human-viability constraints and support Tasks needed to make the skeleton Plan plausibly executable by the user. Examples include affect constraints, breaks, recovery Tasks, meals, sleep, transition buffers, setup/teardown time, context-switch limits, and basic sustainability requirements.

The legitimized skeleton Plan is the baseline feasible Plan. Phase 1 treats full legitimization as a finalist oracle: it runs for the baseline and for the small finalist set selected after cheap pruning, not inside every high-fan-out candidate-construction step unless measurements show it is cheap enough. Semi-legitimization is a conservative cheap filter and ranking signal. It may reject obvious legitimacy failures, but uncertain candidates must reach full legitimization before becoming the default Plan.

Minimum skeleton Plan representation records:

- Static Task placements with fixed start and end times;
- the dependency DAG frontier used for the current planning scope;
- prerequisite roots whose required state is not already true in the initial UniverseState;
- ordered prerequisite chains and the dependency or precondition refs that justify them;
- initial UniverseState assumptions, including known true, known false, assumed, and unresolved planner-relevant facts;
- unsatisfied dependency diagnostics with the failing Task, missing state, causal chain, and clarification alternatives when available.

Skeleton failure diagnostics are blocking when no valid skeleton baseline can be produced for the current Calendar scope. A diagnostic records:

- `failure_class`: one of `missing_starting_state`, `impossible_dependency`, `cyclic_dependency`, `static_task_collision`, `insufficient_calendar_window`, `unavailable_resource`, `blocked_external_event`, or `unknown_precondition`;
- severity, affected Task refs, missing or conflicting state, relevant Static Task or time-window refs, initial UniverseState assumption refs, source or External Event refs, and the shortest causal chain needed to explain the failure;
- safe clarification alternatives, each with an action enum, required user input, and the state mutation or Plan repair it would permit.

The user-facing explanation shows the failed Task or state, the immediate cause, and at most three causal-chain steps by default, with full inspector detail available. Wording frames the problem as an incomplete or contradictory model, not user failure.

Safe alternatives are limited to: provide or correct starting state, mark the required state already satisfied, add a prerequisite Task, relax a deadline or Static Task constraint, extend the planning horizon, remove or moot the blocked Task, choose an already-modeled alternate Technique or Task path, wait for or record an External Event, or ask for manual decision. UbU must not invent a new canonical Resource, Skill, Technique, Preference, or external fact merely to repair skeletonization.

A skeleton failure becomes an immediate blocking prompt when it prevents any valid baseline for the current Calendar, current or next recommended Task, a Static Task placement, a hard dependency or precondition, deadline feasibility, or a required Resource or External Event. It may remain a normal planning warning only when a valid skeleton exists and the failed chain is outside the current recommendation path or future horizon, in which case UbU records the diagnostic and marks the relevant Calendar, explanation, or risk report stale.

Full legitimization records the constraints it enforced and any support Tasks or buffers it inserted. Its hard result is `passed`, `failed`, or `needs_clarification`. Graded fields such as `legitimacy_score`, `legitimacy_margin`, and `legitimacy_delta_from_baseline` are advisory comparison signals, not permission to violate hard legitimacy.

In Phase 1, `full_legitimize` implements the **affect-feasibility filter** (`UBU-D0236`). It computes each active dimension's sigmoid satisfaction from the AffectProfile and the affect observation, marks the candidate affect-feasible only when every dimension meets its threshold, and records per-dimension satisfaction, the `violated_dimensions`, and an `affect_margin` that is the legitimacy margin while affect is the only enforced constraint. It runs in `enforce` for user-facing planning and `warn_only` for onboarding, test, and stale or missing affect. Support-task insertion — breaks, recovery, meals, sleep, and transition buffers — and `semi_legitimize` are deferred follow-ons within full legitimization; Phase 1 legitimization is the affect filter only.

The Phase 1 semi-legitimization heuristic set is affect budget, slack preservation, dependency fragility, user-mode compatibility, local repair viability, and legitimacy-delta estimate. Semi-legitimization returns `passes_cheap_checks`, `reject_obvious`, or `needs_full_legitimization`; it is not sufficient to certify a default Plan.

A higher-value brittle Plan may beat a lower-value humane Plan only when it passes full legitimization, preserves minimum slack and repair envelopes, avoids destructive pressure, and does not leave the user in a worse post-plan state. Otherwise the lower-value humane Plan, or the legitimized skeleton baseline, remains the default.

Recuperative support required for legitimacy is not gap-filling. Meals, sleep, rest, breaks, recovery, setup/teardown, transition buffers, affect collection, and checkpoint work inserted or protected by legitimization are ordinary Tasks or buffers with explanation lineage and protected criticality. They are placed before optional Dynamic Tasks and before `gap_fill_policy` suggestions; crowding them out invalidates or degrades the candidate.

After legitimization, the minimum candidate Plan representation includes materialized Task placements, decision envelopes for movable Tasks, Plan probability metadata, value score, semi-legitimization summary when used, full legitimacy result or score, hard-validation status, protected support refs, and explanation lineage back to Objectives, dependencies, preconditions, affect constraints, worker status, risk findings, and probability inputs.
```

#### DESIGN.md §16 — Compact Calendar

```markdown
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

**Phase 1 surfacing (`UBU-D0238`).** The Compact Calendar surfaces the selected finalist's `display_probability` with its Wilson interval, the p10 rollout robustness, and the `probability_quality` (`full`/`degraded_numeric_jitter`/`degraded_independence`/`not_estimated`); non-finalist alternatives are surfaced with `not_estimated`.

**Phase 1 surfacing (`UBU-D0240`).** The Compact Calendar and next-action also surface the risk report — the aggregated `level` and the categorized findings, with blocking findings prominent — and the six `human_complete_plan_quality` signals with their non-blaming `revision_suggestions`.

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
3. Generate richer candidate Plans by adding optional Dynamic Tasks, alternative placements, and alternative Techniques for the same Objectives (§16.3.1).
4. Use semi-legitimization or full legitimization to reject unrealistic candidates.
5. Validate finalists against hard constraints.
6. Select the user-facing default Plan by Plan probability among deterministic candidate Plans.
7. Package compact repair metadata for runtime use.

DFS-like search may still be useful for candidate construction. BFS-like search may still be useful for near-term divergence. Greedy selection may still be useful as a deliberately unintelligent baseline. None of those algorithms is the complete planning architecture by itself.

Execution profiles are additive rather than mutually exclusive. The greedy mean-duration planner is the required MVP benchmark. Local and mobile profiles must support deterministic skeletonization, conservative legitimization, exact hard-constraint checks, local repair recipes, and a short-horizon branch cache. Desktop, worker, and hosted profiles may add DFS-like candidate construction, local search, solver-backed finalist validation, and GPU-friendly scoring or simulation. GPU or learned search may propose and score candidates, but exact or conservative validation certifies the selected Plan.

Plan probability is represented internally as probability metadata rather than only as a display scalar. The minimum record contains a display scalar, a log probability for stable computation, an optional probability interval, provenance over the modeled probabilistic inputs, and correlation-group or scenario references when inputs are not independent. Implementations may multiply probabilities only for inputs declared independent. Correlated or unknown relationships must use joint scenarios, shared random variables, correlation groups, or conservative intervals rather than pretending independence.

### 16.3.1 Multi-Technique candidate generation and outcome comparison (Phase 3)

When an Objective can be satisfied by more than one already-modeled Technique (technicalizing, §15.2.1.1), candidate generation may produce competing candidate Plans that differ by which Technique satisfies which Objective. Comparing those candidates by their predicted real-world outcomes, and letting the user choose, is a premier Phase 3 capability and the demand-side driver for the Technique database (`UBU-Q0119`): the engine consumes Technique variety, so every additional Technique with a distinct cost/time/affect/Resource signature widens the achievable outcome frontier and improves personalization.

**Bounded search, not enumeration.** UbU must not enumerate the full cross-product of Techniques across Objectives; that is `k^N` candidates and contradicts the bounded-search commitment (§16.1, MVP planning does not require exhaustive optimal search). The required shape is **branch-and-bound with semi-legitimization as the cheap pruner**: dominated and obviously-infeasible Technique combinations are pruned during construction using `reject_obvious` / `passes_cheap_checks` (§15.2.2), and only a small Pareto-frontier finalist set reaches full legitimization and full scoring. Dominance pruning runs as a bound during search, not as a filter after enumeration.

**Dominance over the full outcome vector.** A candidate dominates another only if it is at least as good on **every** outcome axis, including robustness, affect-margin, dependency fragility, and Plan probability — not only money and time. A candidate that saves money but is more fragile or has lower completion probability is **not** dominated and must not be pruned. Pruning on a partial vector would discard exactly the robust Plans legitimization exists to protect.

**Surface outcomes, not utils.** Utils are transient internal computational artifacts, not canonical user values (§2.2, §8.5). The user-facing comparison must show concrete predicted terminal UniverseState — money expended, time spent, Resources consumed, affect cost, relaxation, and Plan probability — not a util scalar. Money is shown here as a **generic cost outcome only** (e.g. "this Technique consumes $3.95"); analysis of which account is affected, balances, or overdraft thresholds requires a financial model that UbU does not yet specify and is deferred to Phase 3B/4+. UbU must not imply affordability or account impact without that model.

**User choice and revealed preference.** The user's declared or learned trade-off weights parameterize the existing `scoring_policy` weights (`utility_weight`, `robustness_weight`, `affect_margin_weight`, `schedule_diversity_weight`; `PLANNING_KERNEL_CONTRACT.md` §2). Two modes follow: auto-select applies the weights; ask-the-user presents a small, diverse finalist set and lets the user pick. A revealed choice is a **proposal, not a fact** (`UBU-D0217`): it may propose a weight or Preference update surfaced for explicit user acceptance, and must never silently rewrite the trade-off vector. A single choice is a weak signal (one inequality in weight-space) and must be accumulated conservatively. Because trade-offs are affect- and state-conditioned, learned weights are a function of current state, not a fixed global vector.

**Confidence-gated automation.** Eventual automatic selection requires two gates in series: a confidence threshold **and** a user-granted, revocable auto-choice authority, modeled on `Auto-choice eligibility` as a governance gate separate from automation-likelihood (§3.7.1) and on the trusted-auto-publication pattern (§2.6). Confidence alone never authorizes automatic selection. Auto-selected choices remain logged, inspectable, and overridable.

**Cognitive load and cadence.** The finalist comparison uses the same UX surface and roughly the same choice counts as Calendar preview (§4.1.2): present a small curated set along the most salient trade-off axis for the current decision, elicit only when the frontier is genuinely ambiguous or the trade-off estimate is stale, and auto-select via learned/declared weights otherwise. Diverging the comparison choice count or weights from Calendar-preview defaults is a user configuration. Within-noise finalists are presented as a tie with a `sensitivity_summary`, not a manufactured ranking.

**Affect-conditioned introspection.** Repeated choices logged under a recognizable affect or time-pressure context may produce an introspection finding routed through habit-pattern reconciliation (§12.2), e.g. surfacing that the user has repeatedly chosen money over work-time when rushed before leaving, and offering to move the decision earlier (the night before, when less rushed). This finding must surface the observed **correlation** and offer a **context** remedy that changes when and how the decision is made; it must not assert a psychological mechanism, blame the user, or nudge toward the option UbU scores higher. Nudging toward a scored-better option would decide values for the user and violate `UBU-D0217`.

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

**Phase 1b mobile stewardship schema (`UBU-D0256`).** A compact Calendar stores `mobile_stewardship_metadata` for each Task or Plan segment that mobile may recommend, move, omit, or explain. The record is compact and by reference where possible; it contains `criticality`, `decision_envelope`, `conflict_severity`, `explanation_fragments`, `repair_recipe_refs`, and optional `recalculation_triggers` and `remote_assist_eligible`. Rich branch packaging, learned local policy, and global re-optimization remain outside MVP.

Task criticality is:

- `protected`: required for hard Calendar Logic, Static Tasks, prerequisites, dependencies, full legitimization support, recovery or safety, or a user-declared must-do commitment. Mobile must not silently drop it, shorten it below its minimum duration, or move it outside its envelope.
- `flexible`: legitimate to move, split, or shorten within its envelope when dependencies, preconditions, affect assumptions, and hard constraints remain satisfied. Mobile may silently repair flexible Tasks only when the resulting conflict severity is `minor`.
- `disposable`: optional or opportunistic work that may be skipped, replaced by an eligible next-best action, or left for desktop/cloud refinement when doing so does not break protected or flexible downstream commitments.

The MVP criticality object is `criticality: { level, reason_refs, recovery_critical, deadline_fragile, min_duration_seconds, user_confirmation_required, disposal_effect }`. `level` is one of `protected`, `flexible`, or `disposable`; `reason_refs` point to Objectives, Static Tasks, dependencies, preconditions, legitimization support, user commitments, or risk findings; `disposal_effect` is a short enum-or-string summary such as `none`, `lost_optional_value`, `missed_deadline_risk`, or `breaks_dependency`.

The MVP decision envelope is `decision_envelope: { earliest_start, latest_start, latest_finish, movable_windows, min_duration_seconds, target_duration_seconds, dependency_refs, precondition_refs, affected_static_refs, affect_assumption_refs, resource_or_location_refs, repair_budget_seconds, stale_after }`. `movable_windows` is an ordered list of `{ start, end }` intervals in which the Task may begin or occupy time, depending on the Task's duration fields. Empty or absent movable windows mean the Task is fixed or must be handled by hard Calendar Logic rather than local motion.

The shared conflict severity vocabulary is:

- `advisory`: stale or degraded metadata exists, but the current and next recommendation remain legitimate.
- `minor`: a local repair inside recorded envelopes preserves all protected commitments and can be undone or inspected later.
- `major`: the current or next recommendation remains explainable, but the user must choose, confirm, or supply missing information before mobile changes a protected commitment or meaningful tradeoff.
- `blocking`: mobile cannot identify a legitimate current or next Task because a hard constraint, Static Task, precondition, dependency, affect legitimacy requirement, or required starting state is violated or unknown.

This severity vocabulary is also the Phase 1b projection-conflict vocabulary: projection-specific classes may map into these severities, but must not invent a parallel severity ladder without a later decision.

**Phase 1b projection conflict policy (`UBU-D0261`).** Projection reconciliation uses the sync conflict class `projection_conflict` with projection-specific quadrants: `known_object_edited`, `known_object_deleted`, `unknown_object_appeared`, and `expected_object_absent`. Automatic handling is allowed only when the result is deterministic, idempotent, within projection policy and decision envelopes, and does not choose between canonical user intent, protected Calendar ownership, or independently changed third-party truth.

The quadrant defaults are:

- `known_object_edited`: auto-resolvable only for non-authority metadata repair or for a permitted edit inside the object's explicit decision envelope. A duration edit to a flexible or disposable Dynamic Task may be `minor` when minimum duration, dependencies, affect assumptions, and Static Tasks remain valid; an edit that pins or moves a Dynamic Task to an external wall-clock time is `major` unless the projection policy already granted that editable window. Protected, Static, dependency-bearing, or precondition-affecting edits are `major` or `blocking`.
- `known_object_deleted`: never silently tombstones or removes the canonical object. If the object is already tombstoned, withdrawn from the projection window, or awaiting idempotent projection deletion, the absence may be `advisory`; otherwise deletion of an active canonical object is `major`, or `blocking` when current/next legitimacy depends on it.
- `unknown_object_appeared`: is contained as external evidence, import candidate, or projection diagnostic rather than admitted canonical state. It remains `advisory` or `minor` when it does not claim a user commitment or protected slot; it becomes `major` when accepting it would create or override a commitment, dependency, Static Task, or third-party-truth assertion.
- `expected_object_absent`: is `advisory` when explained by tombstone, policy/window rotation, or completed projection deletion; it may be `minor` when UbU can retry or recreate the projection idempotently without overriding external truth. It is `major` or `blocking` when the missing external object affects a protected commitment, a current/next recommendation, a required precondition, or Calendar validity.

Every automatic projection repair emits an auditable `projection_conflict_event` record with the conflict id, projection surface, quadrant, severity, canonical object ref when any, external object ref or redacted handle when allowed, observed object and policy versions, external delta digest, action taken, candidate refs when relevant, resulting mutation or projection-operation refs, and recorded time. If the action admits canonical state, it uses the ordinary mutation-envelope or tombstone path; otherwise it remains projection state, diagnostic state, or candidate metadata and is not a silent canonical mutation.

When an external delta touches the same object as a still-pending candidate, admission compares both against the candidate's observed versions before either is accepted. Exact duplicates collapse; deterministic non-overlapping deltas may update the pending candidate's provenance or candidate mutation proposal. Overlapping deltas quarantine or supersede the candidate and surface a `major` review diagnostic, escalating to `blocking` when current/next legitimacy depends on resolving it. External projection evidence cannot force a pending candidate into admitted state.

Cached explanation fragments are short, non-blaming strings with lineage refs. MVP fragments are `why_now`, `matters_because`, `depends_on`, `supports`, `legitimacy_reason`, `if_skipped`, `repair_summary`, and `staleness_summary`. Each fragment may reference Task ids, Objective refs, dependency/precondition refs, Static Task refs, risk finding refs, and compact Calendar provenance, but should avoid copying private details into more-disclosive replicas.

A repair recipe record is `repair_recipe: { trigger, applies_to_refs, eligible_when, steps, ask_policy, fallback, explanation_fragment_ref }`. MVP triggers are:

- `late_task`: condition on elapsed time and accepted Logs; keep protected Tasks and Static Tasks fixed, compress or drop only flexible/disposable work inside envelopes, then offer the next valid Dynamic Task or next-best action. Escalate to `major` or `blocking` if a protected latest finish, hard precondition, or Static Task would be violated.
- `skipped_task`: for disposable Tasks, record omission and continue; for flexible Tasks, reschedule within the envelope; for protected Tasks, ask before omission or create a blocking diagnostic if no legitimate alternate exists.
- `fatigue_report`: record the user report as a Log/Snapshot input, protect recovery-critical Tasks, reduce to protected and low-affect eligible work, and surface rest or clarification when affect assumptions no longer support the Plan.
- `approaching_static_task`: preserve the Static Task, insert required transition/setup/teardown if already modeled, stop or park the current Task when necessary, and ask when the remaining time cannot satisfy a protected current Task.
- `missing_prerequisite`: re-evaluate the deterministic precondition/dependency chain; if a modeled prerequisite Task is available, schedule or pull it forward inside its envelope; otherwise show a clarification Task or blocking prompt. Mobile must not invent a satisfied state, Resource, Skill, Technique, or external fact.

The last legitimate Plan record is `last_legitimate_plan: { plan_id, plan_digest, generated_at, effective_time, horizon, execution_profile, legitimacy_result_ref, hard_validation_ref, default_segment_digest, universe_state_digest, log_cursor, snapshot_refs, external_event_cursor, compact_calendar_digest, protected_task_refs, decision_envelope_digest, explanation_lineage_digest }`. Mobile compares current reality to this record by replaying accepted Logs, Snapshots, External Events, elapsed time, and user actions since the recorded cursors, then checking whether current and next Task placements remain inside decision envelopes with dependencies, preconditions, affect assumptions, Static Tasks, and coverage still valid.

Mobile may silently repair only for `minor` conflicts where the recipe is explicit, all changes stay inside decision envelopes, protected Tasks and support Tasks are preserved, failed preconditions are not hidden, and the explanation cache remains accurate enough for next-action display. Mobile asks the user for `major` conflicts, protected/discretionary tradeoffs, missing starting state, meaningful omission, or any repair that would change a user commitment. Mobile waits for desktop/cloud refinement or marks the Calendar stale when the issue is advisory, outside the reactive horizon, low coverage but not current-action-critical, or requires global optimization. It enters next-best-action mode when there is no safe default but there is at least one legitimate local Task or clarification Task to show.

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

**Backend trust boundary (`UBU-D0254`).** Backends beyond the controlling local instance are advisory workers behind an explicit `WorkerAuthority`; they may propose candidates, simulations, extraction or summary artifacts, projection previews, diagnostics, repair suggestions, and candidate mutations, but they do not mutate canonical state or decide their own scope. Returned artifacts are usable only after schema validation, Compartment/export admission, provenance validation, hard-constraint certification, and ordinary CPU commit or review paths.

**Phase 1b local advisory worker protocol (`UBU-D0255`).** A local advisory worker is invoked through a `LocalAdvisorySubmission` with a `WorkerAuthority`, minimized by-value payload, expected result schema, timeout, compute budget, result size limit, partial-result allowance, causal parents, policy-version refs, input digests, and provider/model configuration metadata. Its `WorkerAuthority` grants only named advisory capabilities and denies canonical writes, admission decisions, direct StateStore handles, extra context fetches, authority expansion, export-policy decisions, ungranted network/tool use, credential access, and retention past the request deadline.

A `LocalAdvisoryResult` echoes the authority, request, policy, input, provider, model, and configuration provenance and returns status `ok`, `partial`, `rejected`, `timeout`, `worker_error`, `malformed_result`, or `cancelled` with schema-valid artifacts, proposed candidates, proposed candidate mutations, diagnostics, safe telemetry, and deletion confirmation when required. Errors, timeouts, cancelled work, and malformed raw output are diagnostic records or quarantined digests, not candidate-state mutations; partial output is usable only artifact by artifact after schema validation.

The worker receives no mutable canonical-state capability. Only the controller/admission path can translate valid advisory artifacts into candidate-queue entries, projection previews, diagnostics, or candidate mutation envelopes, and those proposals remain subject to ordinary schema, policy, provenance, hard-constraint, affect/legitimacy, mutation, and user-review checks. Later remote, rented, or multi-provider workers must preserve the same authority envelope, status semantics, provenance, payload-minimization proof, input digests, retention/deletion semantics, result-size and partial-result rules, and CPU/admission certification; transport may add encryption, signatures, attestations, or sync-carried `worker_request`/`worker_result` statements but not a second mutation channel.

**Phase 1b single-Device credential custody (`UBU-D0262`).** External OAuth tokens, refresh tokens, provider API keys, client secrets, local endpoint authentication material, and worker credentials are Device-local secret values held outside the canonical StateStore. Canonical state may contain only policy metadata and opaque `SecretCapability` or secret-store handles: kind, owning integration or worker authority, operation scope, allowed `zone_id` and `compartment_ids`, version, status, retention/export class, and audit refs. This does not claim encryption at rest beyond the local platform secret store before Phase 2.

The Phase 1b controller labels a credential by the Compartments whose data or external authority it can read, write, or affect; broad provider tokens inherit the broadest applicable label. A worker or projection operation may use a credential only through the controller's credential broker when its authority envelope, purpose, policy versions, and requested operation are within that secret capability. Workers do not receive or read credential material, and a worker scoped to one Compartment cannot use a credential labelled for another.

Revocation and rotation are local capability-state changes. Revocation disables new use immediately on the Device; rotation creates a new credential version and disables the old version for new dispatch. Queued projection operations and advisory submissions record the credential capability id and version they expected; dispatch rechecks the current state and cancels, blocks, or asks for reauthorization rather than silently substituting credentials. Completed external side effects remain reconciled through projection records and conflict handling.

Credential egress is enforced by typed secret references and central serializers, not by caller convention. Projection records, worker submissions/results, canonical payloads, logs, diagnostics, store exports, backups, and diagnostic bundles must reject reusable bearer tokens, API keys, refresh tokens, client secrets, authentication headers/cookies, local secret-store lookup material, and raw credential values; at most they may include an authorized opaque capability id/version or redacted handle.

The open-core boundary includes the canonical planning schemas, Compact Calendar representation, deterministic skeletonization and precondition/effect evaluation, conservative CPU hard-constraint validation, admission and provenance checks, payload-safety validation, and enough local execution-profile support to produce a legitimate default Plan or blocking diagnostic without a hidden service. Hosted, rented, GPU, learned-model, or third-party compute may improve speed and depth, but they are replaceable request/result providers.

Routing selects the least-disclosing capable backend under policy, freshness, latency, energy, and compute-budget constraints. Mobile local execution remains the stewardship fallback for current/next Task clarity and cached repair; local desktop/laptop execution is the default high-fidelity self-hosted path when available; Phase 2 user-owned workers use the sync-carried worker protocol; corporate hosted compute and third-party UbU-compatible providers require explicit policy allowance and user-visible routing disclosure. Third-party providers receive no privileged access over local execution and must use the same authority, payload-safety proof, provenance, response validation, and CPU certification boundary as UbU-hosted providers.

Before external egress, the controller evaluates Compartment policy, export limits, Device or provider trust, `no_cloud_llm`-style constraints, retention rules, and purpose compatibility. Payloads are minimized `PlanningRequest`, `worker_request`, or derived context bundles over redacted Replicas; direct PII, stable object IDs, Compartment labels, secrets, integration credentials, and unrelated UniverseState are omitted, generalized, or replaced with redacted handles unless policy explicitly authorizes disclosure. Transport is encrypted, retention is bounded, diagnostics must not reintroduce stripped identifiers, and results echo policy versions, input digests or structural references, provider/model identity, safe telemetry, and required deletion confirmation.

Dedicated personal worker appliances are future packaging or reference hardware for ordinary user-owned worker Devices, not a separate trust class. Practical FHE-backed or comparable encrypted compute remains strategic research until it can preserve the same authority, provenance, routing, and certification boundary with stronger disclosure properties. Idle-compute monetization, if ever supported, is an external API-enabled user opt-in with revocation, policy limits, and auditability rather than an UbU corporate marketplace or default use of personal compute.

### 16.9 MVP Compact Calendar implementation

The minimum Phase 1 Compact Calendar implementation stores the legitimized skeleton baseline, the default Plan, the active execution profile, coverage estimate, uncovered-mass estimate, effective coverage threshold, decision envelopes, Task criticality, conflict severity, cached explanation fragments, simple repair recipes, probability provenance, and last-legitimate-Plan record. It stores user-previewed, risk-report, and debug/reproducibility Plans only when those artifacts are actually generated or needed for audit.

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

#### 16.10.7 Solver selection and deferred backend targets

Phase 1 does not require OR-Tools, Z3, CP-SAT, SMT, MaxSMT, or another external exact solver as a runtime dependency. The mandatory certification path is the built-in CPU kernel: dependency DAG validation, deterministic precondition and effect evaluation, skeleton validity, bounded contradiction diagnostics, full legitimization, hard Calendar Logic checks, provenance validation, payload-safety validation, and final Plan commit.

Solver and library candidates are evaluation targets:

- built-in CPU graph and precondition validator: required reference path and certification source;
- OR-Tools CP-SAT: optional finalist schedule-feasibility and contradiction-minimization experiment;
- Z3 or comparable SMT/MaxSMT: optional logical contradiction-diagnosis experiment;
- PyTorch or CPU local-search libraries: optional advisory candidate-optimization experiments;
- learned models: post-MVP advisory ranking or parameter estimation unless CPU admission certifies the output.

Phase 1 has no mobile GPU target. The required mobile fallback is CPU stewardship for current or next Task hard checks, cached last-legitimate Plan repair, decision envelopes, and simple repair recipes as defined by `UBU-D0256`.

Premium or cloud GPU planning is governed by the execution-provider boundary in `UBU-D0254`, with concrete products deferred to later provider work. Any future cloud backend must preserve the same typed `PlanningRequest`/`PlanningResponse` boundary, Compartment and export gating, payload minimization, provenance, and CPU certification on return.

See also: `UBU-D0166`, `UBU-D0167`, `UBU-D0168`, `UBU-D0169`, `UBU-D0170`, `UBU-D0171`, `UBU-D0172`, `UBU-D0173`, `UBU-D0174`, `UBU-D0212`, and `PLANNING_KERNEL_CONTRACT.md`.

---
```

#### DESIGN.md §21.4 — MCP-style client and server boundary

```markdown
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
```

#### DESIGN.md §21.8 — AgentAction and BackgroundProcess

```markdown
### 21.8 AgentAction and BackgroundProcess

An `AgentAction` is one bounded attempted action by an agent, model, worker, or tool against an external or privileged surface. A `BackgroundProcess` is the durable controller object that decides when zero or more `AgentAction`s should be proposed, run, retried, cancelled, or escalated without necessarily occupying user Calendar time.

`AgentAction` fields describe a single dispatch or candidate dispatch:

- `agent_action_id`;
- optional `background_process_ref`;
- `actor_identity_ref` and `agent_or_tool_ref`;
- purpose, related Objective/Task/workflow refs, and originating ContextBundle refs;
- target surface, operation kind, payload summary or hash, expected prior external version, and idempotency key;
- authority source, capability grant refs, credential or secret-capability refs by opaque handle and version, Compartment refs, Identity/Association disclosure constraints, and provider/tool route decision refs;
- prompt-injection exposure assessment and containment policy;
- compute, rate, money, and privacy-budget estimate plus actual usage when known;
- side-effect class, reversibility, dry-run/candidate status, review requirement, and approval state;
- result status, external result refs, downstream candidate refs, completion evidence, Log refs, and failure or denial reason when safe;
- rollback or mitigation metadata for any external effect that cannot simply be ignored.

`BackgroundProcess` fields describe standing permission and scheduling policy:

- `background_process_id`, owner Identity, purpose, lifecycle status, created/updated/disabled refs, and policy version;
- trigger spec, recurrence or polling rule, event subscriptions, manual-start affordance, run window, deadline when any, expiry, concurrency limit, retry and backoff policy, cancellation policy, and maximum catch-up behavior after downtime;
- allowed action kinds, target surfaces, provider/tool classes, capability grants, credential handles, Compartments, and ContextBundle templates;
- compute, rate, money, and privacy-budget caps per action, per period, and lifetime;
- prompt-injection risk ceiling, required isolation mode, review threshold, output-admission policy, and candidate-update policy;
- notification policy, quiet-hours behavior, escalation path, failure threshold, and required user-visible summary;
- Calendar projection policy, audit/logging policy, retained evidence refs, and cleanup or mitigation obligations.

Prompt-injection exposure is scored before dispatch and updated after execution. The score combines source trust, whether the agent consumed untrusted webpages, messages, documents, tool outputs, or mixed-Compartment content, the amount of raw private context exposed, whether untrusted text can influence tool choice or arguments, and the authority at risk: credentials, spending, external mutation, privacy export, or user-facing commitments. The minimum qualitative bands are `low`, `medium`, `high`, and `critical`. `High` exposure requires containment such as instruction stripping, source separation, allowlisted tools, candidate-only writes, narrowed ContextBundles, or human review. `Critical` exposure fails closed unless a specific approved policy permits the run and the action remains inside hard Compartment, credential, cost, and review gates.

External side effects carry a side-effect class: `read_only`, `local_candidate_only`, `externally_reversible`, `compensating_action_possible`, `externally_irreversible`, or `unknown`. Anything outside `read_only` or `local_candidate_only` must record expected external prior state when available, idempotency keys, evidence to reconcile the result, and the review gate that admitted the effect. `Compensating_action_possible`, `externally_irreversible`, and `unknown` actions require mitigation metadata before dispatch: mitigation owner, user notification rule, cancellation or revocation path, compensating operation when one exists, external support/contact path when relevant, deadline for mitigation, and the Log or candidate records that should be written if mitigation is needed.

Background processes are Calendar items only when they reserve or require the user's attention, represent a user commitment to another party, or make the user's availability materially unavailable. Compute-only work, polling, sync, advisory analysis, candidate generation, projection reconciliation, and scheduled external checks remain separate process state. They may surface Calendar overlays, deadlines, reminders, or review Tasks, but those projections do not become user time blocks unless the user must attend, decide, travel, meet, or be unavailable.

All AgentAction and BackgroundProcess mutations remain candidate/admission workflows unless an accepted integration policy gives narrower automatic authority. Dispatch rechecks current Compartment policy, credential version, capability grant, budget, lifecycle status, and expected external version immediately before use. Logs are append-only: failed, denied, cancelled, mitigated, and superseded actions are recorded rather than rewritten.
```

#### DESIGN.md §23.1 — Device

```markdown
### 23.1 Device

A **Device** is an execution enclave, not necessarily physical hardware. A physical machine, app install, browser session, and worker process are substrates or runtime contexts; they become UbU Devices only when admitted as separately identifiable execution enclaves with their own `device_id`, trust state, capability profile, Zone membership, and effective Compartment access.

Examples:

- OS user profile
- container
- VM
- secure enclave
- browser profile or session
- worker process or worker runtime

One physical machine may host multiple Devices when each enclave is intentionally registered, isolated enough for its trust and capability claims, associated with an authorized Identity, assigned to exactly one Zone, and granted only the Compartments it is allowed to know about. App installs and browser sessions that merely render or project another Device's state are projection surfaces or execution contexts, not Devices. Workers are Devices only when they hold independent execution authority under policy; otherwise they are child processes of the controlling Device.

In Phase 1b, the dogfooding installation has exactly one registered Device: the operator-controlled execution enclave that owns the local StateStore and admission path. The desktop UI, CLI invocation, unattended batch runner, and local advisory worker are execution contexts under that Device unless a later registration explicitly grants one of them independent Device authority. Every mutation they emit or propose carries the same `origin_device_id`; execution-context identifiers may appear as provenance but do not create authority.

The Phase 1b `device_id` is a stable registered identifier restored from operator-controlled local registration material, such as a config or secret-store record outside the ordinary mutable database. It is not derived from physical hardware, app installation identity, database contents, Google authorization, or other external integration credentials. Reinstalling the app, resetting the database, or reauthorizing integrations therefore preserves the Device only when the registration material is preserved or restored; otherwise the installation must register as a new Device rather than claiming continuity.

The minimum Phase 1b Device registry may contain a single entry, but it uses the same shape required for later registries: `device_id`, label/kind, registration metadata, registered Identity association, trust state, sync state, exactly one Zone membership for the Device, capability profile, effective Compartment access summary, and last-seen or local-observed timestamp. Missing Compartment access is implicit denial, and no code path may special-case the registry cardinality as proof that the Device is canonical, omniscient, or authoritative over projected surfaces such as Google Calendar.

The Phase 1b mutation envelope is part of every canonical mutation before the mutation reaches the StateStore writer. The envelope fields are `idempotency_key`, `observed_versions`, `origin_device_id`, `actor_identity_id`, `authority_source`, `created_time`, `effective_time`, and `recorded_time`; it also carries policy-version observations when the mutation relies on Compartment, Zone, projection, or routing policy.

`observed_versions` is an object-id-to-version-reference map over the current canonical objects whose version or absence the mutation relies on. Existing Phase 1b integer object versions are serialized as stable references such as `v17`; object creation records an explicit absence precondition for the newly claimed object id when absence is part of the operation.

Mutation call sites do not choose a clock or sync ordering algorithm. They obtain the envelope from the admission-owned causality/idempotency issuer, which may use a local monotonic stamp in Phase 1b and later supply the Phase 2 hybrid-logical-clock and causal-parent fields without changing domain mutation APIs.

`created_time` records when the mutation artifact was first assembled, `effective_time` records the domain time the mutation asserts for the changed fact or decision, and `recorded_time` records when the controlling Device durably records the mutation for admission. For an unattended advisory run that proposes overnight and is admitted the following morning, created and effective times may remain overnight while recorded time is the morning admission time.

A replayed `(origin_device_id, idempotency_key)` with the same canonical payload returns the already recorded result and does not increment object versions, emit another mutation, or recalculate derived artifacts as a new cause. The same key with a different canonical payload is an `idempotency_key_conflict` and is not admitted without an explicit conflict-resolution mutation.

Derived artifacts such as Plans, Calendars, reports, risk summaries, and projection previews are `derived_state`: they carry lineage, provenance, input digests, and freshness data, but not the canonical mutation envelope unless a later operation admits a derived artifact itself as canonical state. The mutation that records or invalidates derived state carries the envelope.

Phase 1b deletion of canonical objects is lifecycle mutation, not row removal. Retired Tasks, removed Tasks, decomposition-retired parents, and admitted discarded candidates become tombstones with stable object id, object kind, current version reference, lifecycle state/reason code, envelope/provenance reference, created/effective/recorded lifecycle timestamps, policy-safe Compartment refs needed for local enforcement, and any decomposition, supersession, audit, or projection-deletion refs needed for replay. Tombstones do not retain active payload fields, private evidence, cached explanations, human-readable Compartment labels, or sensitive reason text after those fields are redacted or purged.

A decomposition that retires a parent Task records the parent as tombstoned and relies on the decomposition record's parent snapshot as the restore source. Undo is a new admitted mutation that observes the tombstone and snapshot, un-tombstones the parent only when policy permits and the snapshot payload still exists, and refuses to resurrect content that was physically purged or redacted beyond restoration. Projection deletions caused by tombstones are queued idempotent obligations in `projection_state`; their retry or confirmation status never replaces the canonical tombstone.
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

#### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §5 — Device model **(net-new)**

```markdown
## 5. Device model **(net-new)**

ID and ref naming follows the existing schema convention: identities are `*_id`,
references to objects are `*_ref` / `*_refs`. Compartment permissions reuse the
`compartment_ids` noun rather than an invented `allowlist`/`denylist` term.

```json
{
  "device_id": "dev_laptop_001",
  "device_label": "Sean's Laptop",
  "device_kind": "laptop",
  "registered_at": "2026-06-01T10:00:00-04:00",
  "registered_by_identity_id": "identity_user_main",
  "trust_state": "trusted",
  "sync_state": "active",
  "zone_memberships": ["zone_personal", "zone_ubu_project"],
  "capability_profile": {
    "can_store_full_state": true,
    "can_store_partial_state": true,
    "can_run_planner": true,
    "can_run_worker_jobs": true,
    "can_hold_external_tokens": true,
    "can_receive_notifications": true,
    "can_use_cloud_llm": false
  },
  "effective_compartment_access": [
    {
      "compartment_id": "comp_public",
      "max_replication_level": "full",
      "offline_cache_allowed": true,
      "worker_access_allowed": true,
      "diagnostic_visibility": "full"
    },
    {
      "compartment_id": "comp_personal",
      "max_replication_level": "metadata_only",
      "offline_cache_allowed": false,
      "worker_access_allowed": false,
      "diagnostic_visibility": "generic_only"
    }
  ],
  "last_seen_at": "2026-06-01T12:00:00-04:00"
}
```

`effective_compartment_access` is an illustrative cached/effective policy summary, not
the complete source of authority. Final replication eligibility is still evaluated from
the object, Zone, Compartment policy, Device capability profile, policy versions, and
user policy updates.

**A Device's effective-access summary enumerates only Compartments the Device is
authorized to know about.** Compartments the Device is denied are NOT listed — not even
with `max_replication_level: none` — because the summary itself replicates to (or lives
on) the Device it describes, and naming a denied Compartment by its real `compartment_id`
would hand that Device the identity and existence of a Compartment it is not authorized
to know. Denial of any unlisted Compartment is implicit under default-deny. This is a
direct consequence of the redaction-identity invariant (§10) and security invariant #13
(§24): no real Compartment id or label may reach a Device that the Compartment denies,
and a Device's own registry record is not exempt.

Required `trust_state` values: `unregistered`, `trusted`, `limited`, `stale`, `revoked`,
`lost`, `retired`.

Required `sync_state` values: `active`, `offline`, `pending_sync`, `syncing`,
`sync_failed`, `stale`, `revoked`.

---
```

#### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §8 — Sync statement model **(net-new)**

```markdown
## 8. Sync statement model **(net-new)**

The sync statement is the primary transport-independent unit. `actor_identity_id` and
`authority_source` reuse the existing provenance vocabulary (`AuthoritySource` enum):
every statement carries the same provenance backbone already present on `TaskSpec` and
`LogEntry`.

```json
{
  "sync_statement_id": "syncstmt_01JXYZ",
  "origin_device_id": "dev_phone_001",
  "actor_identity_id": "identity_user_main",
  "authority_source": "user",
  "zone_id": "zone_personal",
  "statement_kind": "mutation",
  "object_refs": ["task_123"],
  "compartment_ids": ["comp_personal"],
  "observed_versions": { "task_123": "v17" },
  "observed_policy_versions": { "comp_personal": "v12" },
  "causal_parents": ["syncstmt_01JXYA"],
  "idempotency_key": "dev-phone-task123-update-20260601T120000",
  "payload_ref": "encrypted_blob_ref_456",
  "effective_time": "2026-06-01T12:00:00-04:00",
  "recorded_time": "2026-06-01T12:00:02-04:00",
  "transport": "encrypted_rclone",
  "integrity": { "signature": "..." }
}
```

Note: `effective_time` is the existing schema field (on `PlanningRequest`, `LogEntry`,
`UniverseStateSnapshot`). `recorded_time` **(net-new)** is the Phase 2 companion that
records when the statement entered the local queue; the pair distinguishes when something
happened from when it was logged. `received_time` and `admitted_time` are replica-local
metadata, not necessarily signed statement payload: `received_time` records when a
Device first received the statement, and `admitted_time` records when that Device
admitted it into its local admitted-state view.

Required `statement_kind` values:

```text
mutation
policy_update
tombstone
redaction
projection_record
conflict_resolution
device_registration
device_revocation
checkpoint
worker_result
diagnostic_summary
```

Required `transport` values (sync semantics must be identical across all of them):

```text
direct_peer
local_lan
removable_file
encrypted_rclone
encrypted_rsync
cloud_relay_user_selected
manual_import
```

Phase 2 need not support every listed transport. It must support at least one direct
or local sync path and must be design-compatible with at least one encrypted indirect
transport. Whether encrypted rclone/rsync compatibility is implemented or only
fixture-certified remains `UBU-Q0141`.

`cloud_relay_user_selected` is permitted only as a user-selected transport for
encrypted sync statements or opaque bundles. It must not become a required canonical
server, must not hold plaintext unless explicitly authorized by policy, and must not
become the source of admitted state.

---
```

#### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §9 — Canonical state as admitted event state

```markdown
## 9. Canonical state as admitted event state

Admitted UbU state is the deterministic result of applying all admitted sync statements
**and conflict-resolution statements** according to this contract. No Device is
automatically the canonical source of all state. Devices contribute statements; the
admitted state is derived from valid statements, not from machine hierarchy.

### Determinism is over the resolution-inclusive closure

This is the explicit reconciliation of the determinism claim with the manual-review
reality of §17.

```text
Determinism holds over the closure of statements that INCLUDES conflict_resolution
statements. A conflict_resolution statement is itself a first-class sync statement
(statement_kind: conflict_resolution) carrying origin Device, actor Identity, and
authority_source, exactly like any other statement.

Until the conflict_resolution statement(s) for a contested region have been admitted,
that region is NOT admitted_state. It is pending_state or candidate_state. The word
"deterministic" applies only once the resolution events are part of the closure.
```

Consequences:

* A human merge decision is **not** outside the event model. It is recorded **as an
  event** (`conflict_resolution`) and participates in causality like any mutation.
* Two honest Devices may each hold a *replica* they believe is current, but neither may
  represent a contested region as `admitted_state` before the resolution event lands.
  Pre-resolution, the correct state category is `pending`/`candidate`, never `admitted`.
* This keeps "deterministic" honest: the reducer is a pure function of a statement set
  that includes resolution statements, not a function that secretly depends on
  out-of-band human choices.

State categories:

| State category     | Meaning                                                     |
| ------------------ | ----------------------------------------------------------- |
| `admitted_state`   | Deterministically accepted state (resolution-inclusive).    |
| `replica_state`    | A Device's local view.                                      |
| `pending_state`    | Locally queued changes not yet reconciled.                  |
| `candidate_state`  | Proposed changes not yet accepted; includes contested regions awaiting resolution. |
| `derived_state`    | Computed Plan, Calendar, risk, or summary.                  |
| `projection_state` | External-system representation.                             |
| `redacted_state`   | Restricted representation for unauthorized Devices.         |

---
```

#### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §15 — Versioning, causality, and idempotency

```markdown
## 15. Versioning, causality, and idempotency

The contract requires: stable object IDs; stable Device IDs; stable Zone IDs; stable
Compartment IDs; per-object version references (`observed_versions`); origin Device on
every statement; actor Identity and `authority_source` on every statement; observed prior
versions; causal parents; idempotency keys; created/effective/recorded timestamps;
deterministic duplicate handling; and observed policy versions for any Compartment,
Zone, projection, or routing policy relied on by the statement.

Candidate mechanisms (not yet mandated — see `UBU-Q0140`): hybrid logical clocks,
Lamport clocks, vector clocks, per-object version counters, signed append-only event log,
content-addressed sync bundles. Phase 2 must not adopt a design that only works for two
Devices.

---
```

#### DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §20 — User-owned worker Devices

```markdown
## 20. User-owned worker Devices

Worker Devices **may**: receive scoped context bundles; run planner candidates; run
simulations; perform local LLM extraction if policy allows; generate summaries; return
candidate mutations; return `worker_result` statements; prepare projection previews.

Worker Devices **may not**: directly mutate admitted state without sync-statement
admission; bypass Compartment policy; retain payloads beyond retention policy; route
`no_cloud_llm` data through cloud services; export `no_external_export` content; expand
their own authority; silently keep data after revocation.

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
- When editing `OPEN_QUESTIONS.md`, anchor hunks with the selected question heading `## UBU-Q0137: Phase 1b BackgroundProcess model for unattended advisory batches` and its own `### Resolution` section. Do not use a repeated `### Resolution` heading from an earlier or later question as the edit location.
- If resolving the selected question, replace the `Unresolved.` text under that selected question's `### Resolution` section and update that same question's metadata line. Do not insert selected-question resolution text into any other question block.
- Use the existing `UBU-Qxxxx` and `UBU-Dxxxx` numbering conventions.
- Prefer minimal, auditable changesets.
- If the selected question is blocked, propose decomposition only if it produces replacement questions with fewer, simpler, or no dependencies.
- If the selected question is already partially resolved, narrow or clarify it rather than pretending it is fully unresolved.

Return only JSON.
