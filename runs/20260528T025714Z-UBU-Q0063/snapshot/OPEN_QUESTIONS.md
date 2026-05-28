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

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0003 Blocks: Phase 1 GitHub dogfooding Resolved by: UBU-D0159 Last scored: Never Scored from commit: None

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

Resolved. See UBU-D0159.

---

## UBU-Q0003: GitHub ↔ UbU External Reference Model

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 GitHub dogfooding Resolved by: UBU-D0098 Last scored: Never Scored from commit: None

Resolved. See UBU-D0098.

---

## UBU-Q0004: Pipeline State

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0003 Blocks: Phase 1 implementation Resolved by: UBU-D0182 Last scored: 2026-05-26 Scored from commit: None

Resolved. See UBU-D0182.

---

## UBU-Q0005: GitHub Event Triage Rules

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0002 Blocks: Phase 1 implementation Resolved by: UBU-D0163 Last scored: Never Scored from commit: None

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

Resolved. See UBU-D0163.

---

## UBU-Q0006: Objective and Task Explosion Control

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0005 Blocks: Phase 1 implementation Resolved by: UBU-D0183 Last scored: 2026-05-26 Scored from commit: None

Resolved. See UBU-D0183.

---

## UBU-Q0007: Automation Worker Identity and Capabilities

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 implementation Resolved by: UBU-D0095 Last scored: Never Scored from commit: None

Resolved. See UBU-D0095.

---

## UBU-Q0008: Worker Assignment Model

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0007 Blocks: Phase 1 implementation Resolved by: UBU-D0158 Last scored: Never Scored from commit: None

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

Resolved. See UBU-D0158.

---

## UBU-Q0009: Worker Mutation Request Schema

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0007 Blocks: Phase 1 implementation Resolved by: UBU-D0164 Last scored: Never Scored from commit: None

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

Resolved. See UBU-D0164.

---

## UBU-Q0010: GitHub Token Custody

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0007 Blocks: Phase 1 implementation Resolved by: UBU-D0184 Last scored: 2026-05-26 Scored from commit: None

Resolved. See UBU-D0184.

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

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0012 Blocks: Phase 1 implementation Resolved by: UBU-D0185 Last scored: 2026-05-26 Scored from commit: None

Resolved. See UBU-D0185.

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

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0016 Blocks: Phase 1 implementation Resolved by: UBU-D0186 Last scored: 2026-05-26 Scored from commit: None

Resolved. See UBU-D0186.

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

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0008 Blocks: Phase 1 implementation Resolved by: UBU-D0187 Last scored: 2026-05-26 Scored from commit: None

Resolved. See UBU-D0187.

---

## UBU-Q0021: Automation Worker Parent / Child Structure

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0008 Blocks: Phase 1 implementation Resolved by: UBU-D0188 Last scored: 2026-05-26 Scored from commit: None

Resolved. See UBU-D0188.

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

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0001 Blocks: README readiness signal Resolved by: UBU-D0189 Last scored: 2026-05-26 Scored from commit: None

Resolved. See UBU-D0189.

---

## UBU-Q0034: Design Automation Stop Rule

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0001 Blocks: Phase 1 scope freeze Resolved by: UBU-D0175 Last scored: Never Scored from commit: None

Broad pre-MVP design automation stops after the Phase 1 planning-kernel blockers are resolved. Future design questions may block Phase 1 only when they are required for a concrete implementation slice, an accepted hard invariant, a needed contract, or avoidance of an irreversible schema contradiction. New MVP blockers require a blocker certificate showing the blocked implementation object, failed acceptance criterion, unsafe fallback, minimum answer needed, and persistence impact.

---

## UBU-Q0035: Automation Coverage Taxonomy

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0032 Blocks: Question-ranking accuracy Resolved by: UBU-D0190 Last scored: 2026-05-27 Scored from commit: None

Resolved. See UBU-D0190.

---

## UBU-Q0036: Committee Log and Provenance Format

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Process Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0032 Blocks: model-committee logging Resolved by: UBU-D0064, UBU-D0160 Last scored: Never Scored from commit: None

### Question

What minimum files and fields must a model-committee run log preserve?

### Current direction

v0.1 uses the provisional filesystem log format defined in `UBU-D0064`. `UBU-D0160` accepts the v0.2 minimum: a manifest-indexed run directory preserving canonical input snapshots, schemas, prompts, raw provider artifacts, parsed structured outputs, candidate patches, validation results, score matrices, disagreement and quorum results, selected artifacts, review notes, commit-message suggestions, and operator-run artifact-publication instructions. The long-term format may evolve through schema migrations and later decisions, but the Phase 1 logging blocker is resolved.

### Resolution

Resolved. See `UBU-D0064` and `UBU-D0160`.

---

## UBU-Q0037: Post-MVP Question Lifecycle

Status: Open Priority: Post-MVP Phase: Post-MVP Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0032 Blocks: None Resolved by: Unresolved Last scored: Never Scored from commit: None

### Question

How should post-MVP design questions be preserved, re-ranked, reopened, or retired after the first MVP release?

### Resolution

Unresolved.

---

## UBU-Q0038: Changeset-Based Work Phase

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Process Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0032 Blocks: model-committee work execution Resolved by: UBU-D0063, UBU-D0069, UBU-D0150, UBU-D0161 Last scored: Never Scored from commit: None

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

Resolved. See `UBU-D0063`, `UBU-D0069`, `UBU-D0150`, and `UBU-D0161`.

---

## UBU-Q0039: Prioritized Recursive Loop Semantics

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0032 Blocks: model-committee loop semantics Resolved by: UBU-D0066 Last scored: 2026-05-26 Scored from commit: None

Resolved. See UBU-D0178.

---

## UBU-Q0040: Question Decomposition and Design Burden Scoring

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0039 Blocks: model-committee prioritization accuracy Resolved by: UBU-D0199 Last scored: 2026-05-27 Scored from commit: None

Resolved. See UBU-D0199.

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
Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0036, UBU-Q0038 Blocks: Contributor recruitment Resolved by: UBU-D0179 Last scored: 2026-05-26 Scored from commit: None
Resolved. See UBU-D0179.
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

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0030, UBU-Q0036, UBU-Q0038 Blocks: Release Outreach Pipeline implementation, release communication, contributor recruitment Resolved by: UBU-D0180 Last scored: 2026-05-26 Scored from commit: None
Resolved. See UBU-D0180.

---

## UBU-Q0050: Minimum Phase 1 bootstrap interview and next-action focus UX

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 1 public demo, nontechnical onboarding, mock app prototype, Release Outreach Pipeline demo scripts Resolved by: UBU-D0129 Last scored: Never Scored from commit: None

Resolved. See UBU-D0129.

---

## UBU-Q0051: Minimum preference-calibration examples for Phase 1 onboarding and review

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0050 Blocks: Phase 1 onboarding quality, Preference calibration, Calendar preview, Log review Resolved by: UBU-D0200 Last scored: 2026-05-27 Scored from commit: None

Resolved. See UBU-D0200.

---

## UBU-Q0052: Discovery-mode action inference and override semantics

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0050 Blocks: Discovery mode, mobile sensor workflow, behavior reconciliation, habit-pattern inference Resolved by: UBU-D0201 Last scored: 2026-05-27 Scored from commit: None

Resolved. See UBU-D0201.

---

## UBU-Q0053: Calendar preview and Log review psychological annotations

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0050 Blocks: Regular review Tasks, preview UX, Log review UX, psychological annotation boundary Resolved by: UBU-D0181 Last scored: 2026-05-26 Scored from commit: None

Resolved. See UBU-D0181.

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

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0016 Blocks: Gap filling before Static Tasks, mobile next-action UX, Calendar preview quality Resolved by: UBU-D0202 Last scored: 2026-05-27 Scored from commit: None

Resolved. See UBU-D0202.

---

## UBU-Q0058: Adaptive planning granularity and offline precomputation policy

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0016 Blocks: Mobile-only planning, offline mode, low-power mode, Compact Calendar runtime policy Resolved by: UBU-D0203 Last scored: 2026-05-27 Scored from commit: None

Resolved. See UBU-D0203.

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

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0046, UBU-Q0049, UBU-Q0050 Blocks: ETHConf follow-up, public dogfooding artifacts, organizational introspection demo Resolved by: None Last scored: 2026-05-27 Scored from commit: None

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

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0014, UBU-Q0015, UBU-Q0016 Blocks: robust planning failure handling Resolved by: None Last scored: 2026-05-27 Scored from commit: None

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

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0016, UBU-Q0053 Blocks: realistic candidate Plan search Resolved by: None Last scored: 2026-05-27 Scored from commit: None

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

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0016 Blocks: practical planner implementation, mobile/desktop/cloud execution profile Resolved by: UBU-D0166, UBU-D0167, UBU-D0168, UBU-D0169, UBU-D0170, UBU-D0171, UBU-D0172, UBU-D0173, UBU-D0174 Last scored: 2026-05-27 Scored from commit: None

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

Subquestions 2, 4, and 6 are substantially resolved for Phase 1.

**Subquestion 2 (GPU-batchable operations):** The four Phase 1 GPU pipeline stages are `skeleton_sampling`, `affect_legitimacy_filter`, `value_scoring`, and `monte_carlo_rollout`. Their semantic stage boundaries are specified in `PLANNING_KERNEL_CONTRACT.md`. The `affect_legitimacy_filter` stage implements only sigmoid affect-constraint evaluation, not full UbU legitimization.

**Subquestion 4 (desktop/laptop GPU path):** Resolved for Phase 1. The performance target is a local desktop/laptop GPU backend using PyTorch and a typed pure-function call boundary. `MAX_PLANNING_TASKS = 256` is the Phase 1 planning window ceiling. A CPU reference path or fixture-backed deterministic path is also required for tests, CI, and contributors without GPU hardware. Future premium or wide-horizon tiers may raise the task ceiling by scalar configuration subject to memory, scenario-count, correlation-matrix, validation-cost, and backend performance limits; linear scaling is not assumed.

**Subquestion 6 (CPU certifies, GPU proposes):** Resolved. The GPU engine is advisory and writes no canonical state. Hard constraint certification, provenance validation, final Plan validity, and canonical Plan commit are CPU kernel responsibilities.

Subquestions 1 (solver library evaluation), 3 (mobile GPU targets), and 5 (cloud GPU path) remain open and are deferred beyond Phase 1.

### Resolution

Partially resolved. See `UBU-D0166`, `UBU-D0167`, `UBU-D0168`, `UBU-D0169`, `UBU-D0170`, `UBU-D0171`, `UBU-D0172`, `UBU-D0173`, `UBU-D0174`, and `PLANNING_KERNEL_CONTRACT.md`. Subquestions 1, 3, and 5 remain open.

---

## UBU-Q0073: Mobile stewardship metadata and MVP repair rules

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 0 Depends on: UBU-Q0016, UBU-Q0058 Blocks: mobile/local planning UX Resolved by: None Last scored: 2026-05-26 Scored from commit: None

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

Status: Open Priority: MVP important Phase: Phase 1 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0050 Blocks: optional EthConf public demo Resolved by: None Last scored: 2026-05-27 Scored from commit: None

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

Status: Open Priority: Research Phase: Phase 2 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 0 Depends on: UBU-Q0016, UBU-Q0051 Blocks: detailed planning horizon policy, premium planning horizon design Resolved by: None Last scored: 2026-05-26 Scored from commit: None

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

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 0 Depends on: UBU-Q0025, UBU-Q0031, UBU-Q0068 Blocks: realtime model adapters, discovery mode, meeting capture, interruption handling Resolved by: None Last scored: 2026-05-26 Scored from commit: None

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

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 0 Depends on: UBU-Q0052, UBU-Q0077 Blocks: realtime UX, discovery mode, short-horizon reactive repair Resolved by: None Last scored: 2026-05-26 Scored from commit: None

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

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0028, UBU-Q0032, UBU-Q0060 Blocks: MCP-style integrations, external agents, Delegation Substrate APIs Resolved by: UBU-D0177 Last scored: 2026-05-26 Scored from commit: None

Resolved. See UBU-D0177.

---

## UBU-Q0080: Delegation Substrate MVP schema and self-reminder use

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 0 Depends on: UBU-Q0032, UBU-Q0060, UBU-Q0079 Blocks: Task delegation, Automation Worker assignment, solo Task formalization, future Skill Barter marketplace Resolved by: None Last scored: 2026-05-26 Scored from commit: None

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

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0028, UBU-Q0060 Blocks: long-context LLM use, organizational introspection, repository/chat archive review Resolved by: UBU-D0162 Last scored: Never Scored from commit: None

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

Resolved. See UBU-D0162.

---

## UBU-Q0084: Computer-use AgentAction and BackgroundProcess model

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 0 Depends on: UBU-Q0031, UBU-Q0032, UBU-Q0060, UBU-Q0079 Blocks: background agents, scheduled agents, computer-use automation, prompt-injection controls Resolved by: None Last scored: 2026-05-26 Scored from commit: None

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

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Governance Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0028, UBU-Q0083 Blocks: safeguard policy, relationship safeguards, product integrity, user autonomy Resolved by: UBU-D0165 Last scored: Never Scored from commit: None

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

Resolved. See UBU-D0165.

---

## UBU-Q0101: Advisory safeguard uncertainty, override, and introspection consequences

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0100 Blocks: safeguard UX, introspection, relationship-transition UX Resolved by: None Last scored: 2026-05-26 Scored from commit: None

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

## UBU-Q0102: PlanningRequest and PlanningResponse schema specification

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0016, UBU-Q0072 Blocks: GPU planning kernel implementation, model-committee interface layer generation Resolved by: UBU-D0170 Last scored: Never Scored from commit: None

Resolved for Phase 1. See `UBU-D0170` and `PLANNING_KERNEL_CONTRACT.md §§1-4`.

---

## UBU-Q0103: GPU pipeline stage-boundary schema specification

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0102 Blocks: GPU planning kernel implementation, model-committee stage generation Resolved by: UBU-D0171 Last scored: Never Scored from commit: None

Resolved for Phase 1. See `UBU-D0171` and `PLANNING_KERNEL_CONTRACT.md §5`.

---

## UBU-Q0104: Correlation group matrix construction rules and PSD handling

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0102 Blocks: Monte Carlo rollout implementation Resolved by: UBU-D0172 Last scored: Never Scored from commit: None

Resolved for Phase 1. See `UBU-D0172` and `PLANNING_KERNEL_CONTRACT.md §7`.

---

## UBU-Q0105: Sigmoid affect constraint direction, feasibility threshold, and UX semantics

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0103 Blocks: affect_legitimacy_filter implementation, AffectProfile UX Resolved by: UBU-D0173 Last scored: Never Scored from commit: None

Resolved for Phase 1. See `UBU-D0173` and `PLANNING_KERNEL_CONTRACT.md §6`.

---

## UBU-Q0106: Log-normal duration parameter semantics and invalid triple handling

Status: Solved Priority: MVP blocker Phase: Phase 1 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0103 Blocks: skeleton sampling implementation, task duration schema Resolved by: UBU-D0174 Last scored: Never Scored from commit: None

Resolved for Phase 1. See `UBU-D0174` and `PLANNING_KERNEL_CONTRACT.md §3`.

---

## UBU-Q0107: Counterfactual logging and decision completeness

Status: Solved Priority: MVP important Phase: Phase 1 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: preference inference, override review, introspection completeness Resolved by: UBU-D0198 Last scored: 2026-05-27 Scored from commit: None

Resolved. See UBU-D0198.

---

## UBU-Q0108: Attention as a distinct AffectProfile dimension

Status: Open Priority: Post-MVP Phase: Post-MVP Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0105 Blocks: richer affect-aware planning Resolved by: None Last scored: Never Scored from commit: None

### Question

Attention is meaningfully distinct from energy: a user can be high-energy but scattered, or low-energy but deeply focused. Cognitive tasks requiring sustained attention compete differently for planning slots than tasks requiring physical energy or emotional presence. Should `attention` be added as a fourth MVP affect dimension in AffectProfile, and if so what is its direction model and interaction with the existing three dimensions?

### Subquestions

1. Is attention `higher_is_better`, `lower_is_better`, or `bounded_optimum` in the sigmoid model?
2. How is attention affected by task switching versus sustained single-task work?
3. Should TaskSpec carry an `attention_requirement` field analogous to the existing affect dimension fields?
4. How does attention interact with energy — can they be treated as independent sigmoid dimensions or are they coupled?
5. What is the minimum user-facing vocabulary for configuring attention constraints?

### Current direction

Attention as a distinct AffectProfile dimension is confirmed as a post-MVP feature. The `bounded_optimum` direction model introduced for `mood_intensity` in Phase 2 may be the right form for attention as well. To be specified during Phase 2 planning.

### Resolution

Open.

---

## UBU-Q0109: Exit model and data portability

Status: Open Priority: Post-MVP Phase: Post-MVP Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: sovereignty completeness Resolved by: None Last scored: Never Scored from commit: None

### Question

User sovereignty requires that leaving UbU cleanly be a first-class supported operation. What does a complete exit model look like: full model export in a non-proprietary format, Log and history preservation, credential revocation, worker termination, Compartment cleanup, and integration disconnection?

### Subquestions

1. What is the canonical export format for the full UbU model (Objectives, Tasks, Plans, Logs, Preferences, Relationships, Compartments)?
2. What credential and token revocation steps are required when exiting?
3. How are active workers and automation processes terminated safely on exit?
4. What is the minimum export that preserves Log history for personal records outside UbU?
5. How is a partial exit (leaving one Compartment or Identity while retaining others) handled?
6. Should export be a continuous background capability or an on-demand operation?

### Current direction

Exit model is confirmed as a post-MVP sovereignty feature. A system that makes leaving difficult or lossy violates the sovereignty first principle regardless of its behavior while the user is active. Export format should be non-proprietary and human-readable. To be specified during post-MVP planning.

### Resolution

Open.

---

## UBU-Q0110: Async standup artifact as a Log derivative

Status: Open Priority: Post-MVP Phase: Post-MVP Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Association coordination reporting Resolved by: None Last scored: Never Scored from commit: None

### Question

The Log and Plan contain everything needed to generate a structured daily or weekly standup artifact — what was done, what is next, what is blocked — suitable for sharing with an Association under compartment policy. What is the schema for a StandupArtifact derived from Log and Plan data, and how does it interact with the existing compartment and disclosure model?

### Subquestions

1. What fields does a StandupArtifact carry: completed tasks, planned next tasks, blockers, estimated completion changes, affect signals if consented?
2. How is the generation period specified: daily, weekly, sprint-bounded, or custom?
3. How does compartment policy govern which Log entries and Plan items are included or redacted?
4. What is the disclosure model: push to Association, pull by Association members, or user-approved per-instance sharing?
5. How does a StandupArtifact interact with the existing AssociationAttestation model?
6. Can a StandupArtifact be generated for an external audience (funder, client) with a different redaction level than for internal Association members?

### Current direction

Async standup artifact as a Log derivative is confirmed as a post-MVP feature consistent with UbU's reporting and disclosure philosophy. All information is already present in the Log and Plan; the gap is the derived artifact schema and the disclosure pipeline. To be designed during post-MVP planning.

### Resolution

Open.

---

## UBU-Q0111: Streak tracking for recurring commitments

Status: Open Priority: Post-MVP Phase: Post-MVP Decision type: Architecture Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: introspection report completeness Resolved by: None Last scored: Never Scored from commit: None

### Question

Completion streaks for recurring Tasks are a pure read-only computation over existing Log data. Should streak counts be a first-class derived field surfaced during the Log review report, and what is the minimum streak schema?

### Subquestions

1. How is a streak defined: consecutive calendar-day completions, completions within the recurrence window, or completions without gap exceeding a threshold?
2. Should streaks be surfaced per Task, per Objective, or per recurring Task group?
3. What is the introspection trigger for a broken streak: immediate notification, Log review surfacing, or silent record only?
4. Are streaks motivational signals only, or do they feed into the planning model (e.g., high-streak tasks get scheduling preference)?
5. Should streak counts be user-visible as a persistent field or only computed on demand during review?

### Current direction

Streak tracking is confirmed as a post-MVP feature surfaced during the Log review report. It requires no new data model primitives — all required data exists in the Log for recurring Task completions. To be added as a Log review report feature during post-MVP planning.

### Resolution

Open.

---

## UBU-Q0112: Waiting-for as an explicit task state

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: delegation tracking, planning horizon accuracy Resolved by: None Last scored: Never Scored from commit: None

### Question

A Task where the user is personally waiting on a human response — "waiting for Sarah's reply before I can continue" — has no distinct Task state. Currently it is either left open with low priority or tracked informally. A `WAITING_FOR` state with an optional counterparty reference and expected-by date would improve planning horizon accuracy and delegation visibility. What is the complete `WAITING_FOR` state model?

### Subquestions

1. What fields does a `WAITING_FOR` task state carry: counterparty Identity ref, expected-by date, reminder policy, escalation trigger?
2. How does `WAITING_FOR` interact with the Task precondition model — is it a precondition state or a Task status?
3. How does `WAITING_FOR` differ from a Task assigned to an automation worker that is polling for completion?
4. How is a `WAITING_FOR` task surfaced in the planning horizon — as blocked, as a risk, or as a scheduled review trigger?
5. What Log entries are generated when a `WAITING_FOR` task transitions out of the waiting state?
6. Should `WAITING_FOR` tasks appear in the Calendar preview as blocked slots or be suppressed?

### Current direction

`WAITING_FOR` as an explicit task state is a Phase 2 required feature. It should be modeled as a Task status rather than a separate Task type. The counterparty reference should be an optional Identity ref consistent with the existing Relationship and Identity models.

### Resolution

Open.

---

## UBU-Q0113: Friction annotation on Log entries

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0107 Blocks: duration model improvement, introspection quality Resolved by: None Last scored: Never Scored from commit: None

### Question

A lightweight optional friction annotation on Log entries — "took longer because the API was down," "interrupted three times," "underestimated complexity" — would feed the duration model review and the introspection cycle without requiring a separate system. What is the minimum friction annotation schema on Log entries, and how does it interact with the Bayesian duration learning system?

### Subquestions

1. Is friction annotation a free-text field, a structured enum, or both?
2. Which Log entry types support friction annotation: Task completion, Task failure, plan deviation, others?
3. How do friction annotations feed the Bayesian duration model — are they a correction signal, a distribution adjustment trigger, or a manual override?
4. How are friction annotations surfaced during the Log review introspection cycle?
5. Should friction annotation be prompted by the system when actual duration significantly exceeds planned duration?
6. How does friction annotation interact with the counterfactual log (Q0107) when the friction was a user decision rather than an external factor?

### Current direction

Friction annotation on Log entries is a Phase 2 required feature. It should be an optional structured field on Task completion Log entries, prompted by the system when actual duration deviates significantly from planned duration. Free-text is acceptable for Phase 2 MVP; structured taxonomy is post-MVP.

### Resolution

Open.

---

## UBU-Q0114: Resource object model and Technique association

Status: Open Priority: MVP important Phase: Phase 3 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Phase 3 Resource-aware task readiness, rich task precondition modeling, inventory management, financial management Resolved by: None Last scored: Never Scored from commit: None

### Question

Resource is now accepted as a core life-logistics abstraction rather than a merely post-MVP convenience. Resources are physical, digital, legal, financial, informational, location, access-controlled, tool-like, or consumable things whose state affects whether a Task can begin, continue, or complete. A Technique can describe how a Resource is used to accomplish work and how Resource state changes when the work succeeds. What is the Resource and Technique object model, and where is the Phase 3A thin task-readiness boundary relative to Phase 3B/Phase 4+ inventory and financial management?

### Subquestions

1. What fields does a minimal Phase 3 Resource carry: identifier, name, kind, location or access path, availability state, owner Identity, Compartment, notes?
2. What are the allowed Resource kinds: physical, digital, financial, access, location, information, tool, consumable, legal, other?
3. What are the allowed availability states: available, missing, unavailable, unknown, needs_acquisition, needs_preparation, expired, damaged, depleted, borrowed, reserved?
4. What does a Task resource requirement carry: resource_ref or placeholder, requirement_type, status, acceptable substitute, acquisition/preparation hint?
5. What does a Technique carry: referenced Resource, linked Task or Task template, usage mode, consumption/borrowing/reference/transformation/production semantics, and predicted Resource state after completion?
6. How does Resource availability map to UniverseState: boolean predicate, quantity, location-indexed state, time-windowed reservation, or richer state object?
7. How does UbU generate prerequisite Tasks when a Resource is missing: buy, find, download, charge, renew, prepare, borrow, reserve, or ask?
8. How does the inventory model work: does UbU maintain a full declared inventory, only Resources referenced by active Tasks, or a hybrid of task-relevant inventory plus user-declared persistent Resources?
9. Where does the Resource model end and financial management begin: ownership cost, purchase history, depreciation, warranty, subscription renewal, cash-flow planning, bank import, and Quicken-like accounting?
10. How do digital Resources such as files, queued media, API credentials, software licenses, and cloud access differ from physical Resources?
11. How does the Resource model interact with Compartments: can a Resource be compartment-scoped such that only certain Tasks in certain Compartments can reference it?
12. What safety/risk rules are needed for dangerous, regulated, or legally constrained Resources?

### Current direction

`UBU-D0192` changes the roadmap: Resource should be treated as a core life-logistics abstraction and a thin Phase 3 task-readiness candidate, not merely post-MVP. The Phase 3A feature answers whether a Task is realistically ready to start or complete and may create prerequisite preparation/acquisition Tasks. Full inventory control, procurement automation, subscription management, bank syncing, investment tracking, receipt OCR, tax categorization, and Quicken-like financial management remain Phase 3B/Phase 4+ full-version-1.0 features or later. The Resource model should remain compatible with UniverseState preconditions and Technique state-transform semantics.

### Resolution

Open.

---

## UBU-Q0115: TaskFactory template expansion model

Status: Open Priority: Post-MVP Phase: Post-MVP Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0114, UBU-Q0118 Blocks: recurring project setup friction, Technique database templates Resolved by: None Last scored: Never Scored from commit: None

### Question

A TaskFactory is a template object that expands into a set of Tasks, dependency edges, and an Objective structure when instantiated. What is the TaskFactory model, and how does it interact with the existing Task, Objective, and Precondition systems?

### Subquestions

1. What does a TaskFactory carry: template Task definitions, dependency edges, Objective structure, parameterization hooks (e.g. client name, due date), Resource requirements, Skill requirements?
2. How is a TaskFactory instantiated: manually by the user, triggered by an External Event, or triggered by a Recalculation?
3. How does instantiation interact with the existing Objective and UniverseState model — does instantiation create a new Objective, or expand into an existing one?
4. How are TaskFactory templates versioned and updated without breaking existing instantiations?
5. How do Resource and Skill requirements in a TaskFactory interact with the Resource model (Q0114) and Skill model (Q0118)?
6. Should TaskFactory be a user-authored object or a model-committee-generated artifact?

### Current direction

TaskFactory is confirmed as a post-MVP feature handling recurring project scaffolding. It should be compatible with the existing Task, Objective, Precondition, Resource, Skill, and Technique models. To be designed during post-MVP planning, after the Resource model (Q0114) and Skill model (Q0118) are specified.

### Resolution

Open.

---

## UBU-Q0116: Bayesian duration learning from Log history

Status: Open Priority: Post-MVP Phase: Post-MVP Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0113 Blocks: self-improving planning accuracy Resolved by: None Last scored: Never Scored from commit: None

### Question

All planning uses the user-declared (min_seconds, mode_seconds, p95_seconds) three-point duration estimate. Over time, the Log accumulates actual durations for completed Tasks. A Bayesian update of the prior duration distribution using logged actuals would produce progressively more accurate planning predictions. This is a post-MVP premier feature. What is the Bayesian duration learning model?

### Subquestions

1. What is the update rule: full Bayesian posterior update of log-normal parameters, or a simpler exponential moving average of actual vs planned?
2. How is task-type grouping handled: does learning generalize across similar Tasks, or is it per-Task-identity only?
3. How are friction annotations (Q0113) incorporated: as outlier signals that should be down-weighted, or as explanatory covariates?
4. How is the learned distribution surfaced to the user: as updated suggested (min, mode, p95) values for review, or as a silent override?
5. User sovereignty requires that learned distributions not silently replace user-declared values. How is the update proposed and accepted?
6. How does learning interact with the deferred_intent model (Q0117): tasks that are rarely attempted should not update the distribution from a small biased sample.
7. What minimum Log history depth is required before Bayesian updates are meaningful?

### Current direction

Bayesian duration learning is confirmed as a core planning principle and a post-MVP premier feature. All planning comparisons between predictions and logged results are Bayesian in character. The update model must respect user sovereignty: learned distributions are proposed for user review rather than silently applied. Friction annotations (Q0113) are a key input to the learning model. To be specified during post-MVP planning.

### Resolution

Open.

---

## UBU-Q0117: deferred_intent task status and dismissal review

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Auto eligible Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: planning horizon clarity, introspection completeness Resolved by: None Last scored: Never Scored from commit: None

### Question

UbU needs a named Task status for ideas that are not yet commitments and not cancelled — things the user has explicitly decided not to plan but wants to remember. A `DEFERRED_INTENT` status distinct from open and cancelled would prevent such items from cluttering the planning horizon. The Log review introspection cycle should periodically surface deferred-intent Tasks for review, including prompting the user to dismiss Tasks that are no longer attainable or desirable. What is the complete deferred_intent model?

### Subquestions

1. What fields distinguish a `DEFERRED_INTENT` task from an open low-priority task: explicit user deferral action, optional defer-until date, optional reason?
2. How does `DEFERRED_INTENT` interact with the planner: are these tasks invisible to the skeleton sampler, or available as optional fill tasks with very low priority?
3. What Log entry is created when a Task is moved to `DEFERRED_INTENT`?
4. What is the review cadence for surfacing deferred-intent Tasks: weekly, monthly, on Log review, or triggered by related Objective changes?
5. What dismissal options does the review prompt offer: promote to active, keep deferred, cancel permanently, or escalate to an Objective review?

### Current direction

`DEFERRED_INTENT` is a Phase 2 required status addition to the Task model. It is distinct from low-priority and distinct from cancelled. The periodic Log review report should surface deferred-intent Tasks with a structured dismissal prompt. The preference declaration requirement — that a Task must eventually have an explicit Preference commitment or be dismissed — should be enforced through this review cycle rather than at creation time.

### Resolution

Open.

---

## UBU-Q0118: Skill object model, rust, and evidence

Status: Open Priority: MVP important Phase: Phase 3 Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: Skill-aware task readiness, Technique skill tree, DIY-versus-hire planning, future Skill Barter marketplace Resolved by: None Last scored: Never Scored from commit: None

### Question

Skill is accepted as an Identity-owned, rust-prone capability predicate that can satisfy Task dependencies and unlock Techniques. What is the Skill object model, how are proficiency and rust represented, and what evidence is required for private planning versus delegated, public, or marketplace-visible Skill claims?

### Subquestions

1. What fields does a Skill carry: identifier, name, domain, description, owner Identity, proficiency level, confidence, verification status, last used, last tested, rust model, prerequisite Skills, unlocked Techniques, evidence refs, Compartment?
2. What proficiency scale should UbU use: ordinal levels, task-specific thresholds, domain-specific rubrics, calibrated probabilities, or a hybrid?
3. How does Skill rust work: time decay, practice-frequency model, confidence decay, test-expiration date, or task-risk-specific refresher requirement?
4. How does UbU distinguish self-declared Skill, inferred Skill, tested Skill, credentialed Skill, peer-reviewed Skill, and marketplace-rated Skill?
5. What evidence types are allowed: self declaration, completed Task, failed Task, quiz, supervised completion, credential, portfolio artifact, peer review, marketplace rating, External Reference?
6. How should a Skill requirement attach to a Task: blocks_start, blocks_completion, improves_quality, reduces_risk, reduces_cost, or produces_learning_value?
7. How should UbU represent acceptable Skill substitutes or alternative Techniques when the user lacks the default Skill?
8. How should Skills be compartmented, especially professional, pseudonymous, sensitive, or marketplace-visible Skills?
9. How should high-risk, dangerous, legally regulated, medical, electrical, structural, financial, legal, or security-sensitive Skills be modeled without overclaiming safety?
10. What user-review flow accepts, rejects, corrects, or archives model-suggested Skill updates after completed Tasks?

### Current direction

`UBU-D0193` accepts Skill as a first-class object. Private Skill modeling should precede public Skill Barter. Self-declared Skills may be adequate for private planning, but public or delegated claims require stronger evidence and confidence semantics. Skills must be allowed to rust over time and should influence task readiness, risk, cost, learning paths, and DIY-versus-hire choices.

### Resolution

Open.

---

## UBU-Q0119: Global Technique database and real-life skill tree

Status: Open Priority: Post-MVP Phase: Post-MVP Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0114, UBU-Q0118 Blocks: Technique database, skill-tree UX, real-life capability acquisition, Skill Barter pipeline Resolved by: None Last scored: Never Scored from commit: None

### Question

A large UbU-run or UbU-compatible Technique database could let users select, learn, schedule, and verify Skills that unlock DIY Tasks, maintenance, professional labor skills, and Skill Barter opportunities. What is the Technique database model and how should UbU present the real-life skill tree without turning it into fake gamification?

### Subquestions

1. What fields does a Technique database entry carry: title, Objective class, required Skills, required Resources, expected duration, cost range, risk level, failure consequences, evidence requirements, produced Resources, produced Skills, verification criteria, prerequisites, variants, and citations/External References?
2. How are Techniques curated, versioned, localized, rated, deprecated, or forked?
3. Which Techniques are user-authored, UbU-authored, community-authored, Association-authored, or imported from external sources?
4. How does UbU map a missing Skill to learning Tasks, practice Tasks, prerequisite Techniques, and verification Tasks?
5. How does the planner choose between alternative Techniques for the same Objective when they require different Resources, Skills, costs, risk, or learning value?
6. How should the skill-tree UX show unlocks, rust, confidence, evidence, and practical next learning steps?
7. How does the Technique database avoid dangerous instructions, low-quality procedures, liability traps, hallucinated steps, and stale external references?
8. How can Technique entries remain useful offline or locally while still benefiting from global/community improvements?
9. How should UbU distinguish a fun marketing metaphor from a canonical scoring system so “gamifying real life” remains grounded in real capability?

### Current direction

`UBU-D0194` accepts the Technique database and real-life skill tree as premier future features. The metaphor is valuable only when grounded in real Skills, Resources, evidence, risks, and Objective-serving work. Phase 3B should explore private capability graph usefulness before public marketplace dependence.

### Resolution

Open.

---

## UBU-Q0120: DIY-versus-purchase/hire/barter planning tradeoff

Status: Open Priority: Post-MVP Phase: Post-MVP Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0114, UBU-Q0118, UBU-Q0119 Blocks: economic self-sufficiency planning, Resource/Skill value reports, financial-management extensions Resolved by: None Last scored: Never Scored from commit: None

### Question

Resource and Skill modeling let UbU compare buying, hiring, DIY, learning-then-DIY, bartering, or deferring. What planning model should compare these alternatives without moralizing DIY or overclaiming the safety of user-performed work?

### Subquestions

1. What factors enter the comparison: time cost, money cost, affect cost, Resource availability, Skill level, Skill rust, risk, consequence of failure, learning value, future unlocks, relationship value, and Skill Barter value?
2. How should UbU model the financial value of DIY without becoming a premature accounting package?
3. How should UbU compare a professional service against a DIY Technique when the professional has stronger Skills, credentials, tools, insurance, or warranty coverage?
4. How should UbU account for opportunity cost and affect depletion when DIY saves money but consumes scarce energy or attention?
5. How should completed DIY Tasks update Skill evidence, Resource state, cost history, and future recommendations?
6. How should UbU handle safety-critical, legally regulated, warranty-voiding, or high-failure-cost DIY work?
7. What user-facing explanation should show why UbU recommends buying, hiring, learning, bartering, or deferring?
8. How does this tradeoff interact with Preferences, Objectives, budget constraints, and financial management extensions?

### Current direction

`UBU-D0195` accepts DIY-versus-purchase/hire/barter as a core planning comparison. UbU should promote economic self-sufficiency through transparent tradeoffs, not by pressuring the user toward DIY. The financial-management path should connect spending and ownership to Objectives, Tasks, Techniques, Resources, and Skills.

### Resolution

Open.

---

## UBU-Q0121: Skill Barter evidence, reputation, and sovereignty boundary

Status: Open Priority: Post-MVP Phase: Post-MVP Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: UBU-Q0065, UBU-Q0082, UBU-Q0118, UBU-Q0119, UBU-Q0120 Blocks: public Skill Barter marketplace, pseudonymous capability claims, private reputation, dispute workflows Resolved by: None Last scored: Never Scored from commit: None

### Question

Skill Barter should become an open, user-sovereign skill economy direction built on private Skill modeling, Delegation Substrate, Identity, Resource/Skill readiness, Technique evidence, and Compartments. What evidence, reputation, privacy, dispute, and settlement boundaries are required before any public or federated Skill Barter marketplace can operate responsibly?

### Subquestions

1. Which Skill claims can be self-declared, which require evidence, and which should be disallowed or externally constrained?
2. What is the minimum marketplace-visible Skill profile: Identity, pseudonym, Skill refs, evidence summaries, confidence, availability, price/barter terms, Compartment policy, jurisdictional constraints?
3. How does reputation avoid unnecessary doxxing while resisting fraud, Sybil attacks, collusion, and low-quality work?
4. What evidence can be selectively disclosed without exposing raw private planning data, client data, affect data, or unrelated Identity context?
5. What dispute workflow is needed for failed work, ambiguous completion, unsafe conduct, nonpayment, or broken commitments?
6. What lawful settlement references are allowed, and how does UbU avoid token-first, speculation-first, tax-evasion, sanctions-evasion, or illicit-market framing?
7. How do human executors, agentic executors, Associations, and General Contractor roles appear in the same marketplace without collapsing their different risk profiles?
8. How portable should Skill evidence and reputation be outside UbU so the ecosystem remains user-sovereign rather than closed?
9. Which advanced privacy technologies become useful: ZK claims, selective disclosure, FHE, secure enclaves, private reputation, commitments, or verifiable credentials?

### Current direction

`UBU-D0196` accepts Skill Barter as a future open, user-sovereign skill economy direction, not a Phase 1 marketplace or closed ecosystem. Private Skill and Technique usefulness should come first. Marketplace operation belongs to Phase 4+ unless a narrower prototype can be proven lawful, safe, privacy-preserving, and non-distracting from the personal life-logistics product.

### Resolution

Open.
