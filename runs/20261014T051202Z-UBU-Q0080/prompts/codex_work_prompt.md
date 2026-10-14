# Model-Committee Work Proposal Request

You are participating in the UbU `model-committee` process.

Your task is to produce one concrete work proposal as strict JSON.

Do not return prose outside the JSON object.

## Selected question

Question ID: `UBU-Q0080`  
Question title: `Delegation Substrate MVP schema and self-reminder use`  
Repository working tree: `/home/sean/ubu-design`

HEAD commit (provenance only): `6a3aae470f2a51591f560d8a0548ef1211d38c49`

```markdown
## UBU-Q0080: Delegation Substrate MVP schema and self-reminder use

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0032, UBU-Q0060, UBU-Q0079 Blocks: Task delegation, Automation Worker assignment, solo Task formalization, future Skill Barter marketplace Resolved by: None Last scored: 2026-10-14 Scored from commit: None

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

```

## Patch baseline

The patch must apply to the **current working-tree files** in the repository above.
The selected question and context excerpts come from that working tree, which may
include uncommitted ranking and metadata updates.

- Set the JSON `base_commit` field to `6a3aae470f2a51591f560d8a0548ef1211d38c49` for provenance. It does not
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
- next decision id: `UBU-D0342`

`DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md` is read-only context and is not in the patch
allowlist: do not propose changes to it.

### OPEN_QUESTIONS.md — related questions

#### UBU-Q0028

```markdown
## UBU-Q0028: Minimum Privacy / Compartment Promise for MVP

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 implementation Resolved by: UBU-D0074 Last scored: Never Scored from commit: None

Resolved. See UBU-D0074.

---
```

#### UBU-Q0032

```markdown
## UBU-Q0032: Model Committee Process and Authority

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Model-committee correctness Resolved by: UBU-D0057, UBU-D0058, UBU-D0059, UBU-D0060, UBU-D0065, UBU-D0069, UBU-D0070, UBU-D0150 Last scored: Never Scored from commit: None

Resolved. See UBU-D0057, UBU-D0058, UBU-D0059, UBU-D0060, UBU-D0065, UBU-D0069, UBU-D0070, UBU-D0150.

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
- When editing `OPEN_QUESTIONS.md`, anchor hunks with the selected question heading `## UBU-Q0080: Delegation Substrate MVP schema and self-reminder use` and its own `### Resolution` section. Do not use a repeated `### Resolution` heading from an earlier or later question as the edit location.
- If resolving the selected question, replace the `Unresolved.` text under that selected question's `### Resolution` section and update that same question's metadata line. Do not insert selected-question resolution text into any other question block.
- Use the existing `UBU-Qxxxx` and `UBU-Dxxxx` numbering conventions.
- Prefer minimal, auditable changesets.
- If the selected question is blocked, propose decomposition only if it produces replacement questions with fewer, simpler, or no dependencies.
- If the selected question is already partially resolved, narrow or clarify it rather than pretending it is fully unresolved.

Return only JSON.
