# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0059`  
Question title: `Execution-provider trust, worker backends, and privacy-preserving compute roadmap`  
Base commit: `f0b0f7c05994feb2db3aba5245aef801d2feb751`

```markdown
## UBU-Q0059: Execution-provider trust, worker backends, and privacy-preserving compute roadmap

Status: Open Priority: MVP important Phase: Phase 1b Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0016 Blocks: Phase 2 personal worker devices, hosted compute policy, third-party provider compatibility, Phase 1b advisory worker boundary Resolved by: None Last scored: 2026-09-16 Scored from commit: None

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

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "openai-gpt-5-codex-UBU-Q0059-001",
    "provider_id": "openai",
    "model_name": "gpt-5-codex",
    "question_id": "UBU-Q0059",
    "base_commit": "f0b0f7c05994feb2db3aba5245aef801d2feb751",
    "summary": "Resolve UBU-Q0059 by defining all non-local execution providers as interchangeable advisory workers behind explicit WorkerAuthority, Compartment/export admission, payload minimization, provenance, user-visible routing, and CPU certification, while keeping the open-core planner self-hostable and treating appliances, encrypted compute, and monetization as optional future directions.",
    "rationale": "The proposal answers the Phase 1b-relevant boundary without over-specifying future products: local/mobile execution remains sufficient for the open-core loop, external compute is an optional capability provider, third parties are not privileged over local execution, and any backend receives only minimized, policy-authorized payloads and returns artifacts that must pass admission and CPU validation. It also gives enough direction for Q0135-style local advisory workers to be transport-compatible with later remote or rented backends.",
    "changed_files": [
      "OPEN_QUESTIONS.md",
      "DECISIONS.md",
      "DESIGN.md"
    ],
    "patch": "diff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\nindex 1d6d4d2..e7bdb23 100644\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -532,7 +532,7 @@ Open.\n \n ## UBU-Q0059: Execution-provider trust, worker backends, and privacy-preserving compute roadmap\n \n-Status: Open Priority: MVP important Phase: Phase 1b Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0016 Blocks: Phase 2 personal worker devices, hosted compute policy, third-party provider compatibility, Phase 1b advisory worker boundary Resolved by: None Last scored: 2026-09-16 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: UBU-Q0016 Blocks: Phase 2 personal worker devices, hosted compute policy, third-party provider compatibility, Phase 1b advisory worker boundary Resolved by: UBU-D0254 Last scored: 2026-09-16 Scored from commit: None\n \n ### Question\n \n@@ -557,7 +557,7 @@ Phase 1b re-scope: this question is pulled forward from Phase 2 under the Phase\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0254`: every non-local execution backend is an explicitly authorized, interchangeable advisory worker behind `WorkerAuthority`, Compartment/export admission, payload minimization, provenance, user-visible routing, and CPU certification on return. The open-core planner must remain self-hostable through the FOSS schemas, Compact Calendar representation, planning loop, and CPU admission/certification path; corporate hosted compute, third-party providers, appliances, FHE-like encrypted compute, and idle-compute monetization are optional future backends or packaging choices, not privileged dependencies.\n \n ---\n \ndiff --git a/DECISIONS.md b/DECISIONS.md\nindex 783fb7f..64e6365 100644\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -3886,3 +3886,21 @@ For `compartment_ref`, the default is omission. If policy explicitly permits res\n All redacted handles must be generated as high-entropy random aliases or keyed aliases whose key is unavailable to the restricted Device. Handles must carry no semantic prefix, reason code, sequence, compartment vocabulary, or stable global identifier.\n \n The acceptable correlation risk is limited to local correlation needed for one authorized restricted Device to maintain UI continuity and de-duplication inside one projection window. Durable cross-Device, cross-window, cross-version, or Compartment-pattern correlation is not acceptable for redacted replicas.\n+\n+---\n+\n+## UBU-D0254: Execution backends are interchangeable advisory workers behind authority, minimization, and certification\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a716.8. Resolves `UBU-Q0059`.\n+\n+UbU execution providers beyond the currently controlling local instance are advisory workers, not privileged planning authorities. A backend may propose candidate Plans, simulations, summaries, extraction results, projection previews, candidate mutations, diagnostics, or repair suggestions only within an explicit `WorkerAuthority`. It cannot directly mutate canonical state, broaden its own scope, fetch additional protected context, decide Compartment export policy, or bypass admission. Returned artifacts become usable only after the controlling CPU kernel validates schema, policy provenance, hard constraints, skeleton validity, affect or legitimacy claims, and any candidate mutations through the ordinary admission path.\n+\n+The open-core planning loop must remain self-hostable without a hidden service. The FOSS boundary includes the canonical planning schemas, Compact Calendar representation, deterministic skeletonization and precondition/effect evaluation, conservative CPU hard-constraint validation, admission and provenance checks, payload-safety validation, and enough local execution profile support to produce a legitimate default Plan or a blocking diagnostic. Hosted, rented, GPU, learned-model, or third-party compute may improve speed, branch coverage, candidate diversity, or analysis depth, but they are substitutions behind the same request/result boundary, not requirements for correctness or access to the user's state.\n+\n+Every worker invocation carries a scoped authority envelope with the backend kind, provider or Device identity, requested capability, purpose, allowed `zone_id` and `compartment_ids`, redaction level, policy-version references, retention deadline, export/network/tool permissions, compute budget, model or solver identity when applicable, request ID, causal parents, and expected response schema. Worker capabilities are additive and narrow: candidate planning, simulation, local extraction, summarization, projection-preview preparation, and candidate-mutation generation are separate grants. Absence of a grant means denial.\n+\n+Routing chooses the least-disclosing backend that satisfies the requested capability, policy, freshness, latency, energy, and compute-budget requirements. Mobile local execution is the stewardship fallback for current/next Task clarity and cached repair. Local desktop/laptop execution is the default high-fidelity self-hosted path when available. Phase 2 user-owned workers may receive work through the sync-carried worker protocol. Corporate hosted compute and third-party UbU-compatible providers require explicit policy allowance and user-visible routing disclosure. Third-party providers use the same APIs, safety proof, provenance, and return validation as UbU-hosted providers and receive no privileged access over local execution.\n+\n+Before any external backend receives work, the controller must evaluate Compartment policy, export limits, Device or provider trust, `no_cloud_llm`-style constraints, retention rules, and purpose compatibility. The transmitted payload is a minimized `PlanningRequest`, `worker_request`, or derived context bundle over a redacted Replica: direct PII, stable object IDs, Compartment labels, secrets, integration credentials, and unrelated UniverseState are omitted, generalized, or replaced with redacted handles unless policy explicitly authorizes disclosure. Transport must be encrypted; retained payloads must obey the request retention deadline; diagnostics and logs must avoid reintroducing stripped identifiers. The request records a payload-safety proof or equivalent provenance summary, and the result echoes the policy versions, input digests or structural references, provider/model identity, telemetry safe for the authority level, and deletion confirmation when transient payload deletion was required.\n+\n+Dedicated personal worker appliances are future packaging or reference-hardware work, not a special trust class. They must register and operate as ordinary user-owned worker Devices. Future practical FHE-backed or comparable encrypted compute is a strategic research backend class: it is worth tracking and may reduce disclosure when viable, but it must still use the same authority, capability, provenance, routing, and CPU-certification boundary unless a later accepted design proves stronger semantics. Idle-compute monetization, if ever supported, should be an external API-enabled user choice with explicit opt-in, revocation, policy limits, and auditability rather than an UbU corporate marketplace or a default use of personal compute.\ndiff --git a/DESIGN.md b/DESIGN.md\nindex 78d53f3..d1aee1f 100644\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -2297,6 +2297,16 @@ Possible execution backends include:\n \n The FOSS planner and Compact Calendar representation should be backend-agnostic. Execution providers are interchangeable through explicit APIs, capability grants, Compartment policy, provenance, and user-visible routing decisions. GPU search may propose candidate Plans, but hard constraints and final Plan validity must be certified by exact or conservative validation. Cloud planning payloads should minimize PII and transmit only the structured timing, dependency, value, constraint, criticality, and legitimacy information needed for the selected provider.\n \n+**Backend trust boundary (`UBU-D0254`).** Backends beyond the controlling local instance are advisory workers behind an explicit `WorkerAuthority`; they may propose candidates, simulations, extraction or summary artifacts, projection previews, diagnostics, repair suggestions, and candidate mutations, but they do not mutate canonical state or decide their own scope. Returned artifacts are usable only after schema validation, Compartment/export admission, provenance validation, hard-constraint certification, and ordinary CPU commit or review paths.\n+\n+The open-core boundary includes the canonical planning schemas, Compact Calendar representation, deterministic skeletonization and precondition/effect evaluation, conservative CPU hard-constraint validation, admission and provenance checks, payload-safety validation, and enough local execution-profile support to produce a legitimate default Plan or blocking diagnostic without a hidden service. Hosted, rented, GPU, learned-model, or third-party compute may improve speed and depth, but they are replaceable request/result providers.\n+\n+Routing selects the least-disclosing capable backend under policy, freshness, latency, energy, and compute-budget constraints. Mobile local execution remains the stewardship fallback for current/next Task clarity and cached repair; local desktop/laptop execution is the default high-fidelity self-hosted path when available; Phase 2 user-owned workers use the sync-carried worker protocol; corporate hosted compute and third-party UbU-compatible providers require explicit policy allowance and user-visible routing disclosure. Third-party providers receive no privileged access over local execution and must use the same authority, payload-safety proof, provenance, response validation, and CPU certification boundary as UbU-hosted providers.\n+\n+Before external egress, the controller evaluates Compartment policy, export limits, Device or provider trust, `no_cloud_llm`-style constraints, retention rules, and purpose compatibility. Payloads are minimized `PlanningRequest`, `worker_request`, or derived context bundles over redacted Replicas; direct PII, stable object IDs, Compartment labels, secrets, integration credentials, and unrelated UniverseState are omitted, generalized, or replaced with redacted handles unless policy explicitly authorizes disclosure. Transport is encrypted, retention is bounded, diagnostics must not reintroduce stripped identifiers, and results echo policy versions, input digests or structural references, provider/model identity, safe telemetry, and required deletion confirmation.\n+\n+Dedicated personal worker appliances are future packaging or reference hardware for ordinary user-owned worker Devices, not a separate trust class. Practical FHE-backed or comparable encrypted compute remains strategic research until it can preserve the same authority, provenance, routing, and certification boundary with stronger disclosure properties. Idle-compute monetization, if ever supported, is an external API-enabled user opt-in with revocation, policy limits, and auditability rather than an UbU corporate marketplace or default use of personal compute.\n+\n ### 16.9 MVP Compact Calendar implementation\n \n The minimum Phase 1 Compact Calendar implementation stores the legitimized skeleton baseline, the default Plan, the active execution profile, coverage estimate, uncovered-mass estimate, effective coverage threshold, decision envelopes, Task criticality, cached explanation fragments, probability provenance, and last-legitimate-Plan reference. It stores user-previewed, risk-report, and debug/reproducibility Plans only when those artifacts are actually generated or needed for audit.\n@@ -2390,7 +2400,7 @@ Solver and library candidates are evaluation targets:\n \n Phase 1 has no mobile GPU target. The required mobile fallback is CPU stewardship for current or next Task hard checks, cached last-legitimate Plan repair, decision envelopes, and simple repair recipes; exact mobile metadata remains in `UBU-Q0073`.\n \n-Premium or cloud GPU planning is deferred to `UBU-Q0059` and later provider work. Any future cloud backend must preserve the same typed `PlanningRequest`/`PlanningResponse` boundary, Compartment and export gating, payload minimization, provenance, and CPU certification on return.\n+Premium or cloud GPU planning is governed by the execution-provider boundary in `UBU-D0254`, with concrete products deferred to later provider work. Any future cloud backend must preserve the same typed `PlanningRequest`/`PlanningResponse` boundary, Compartment and export gating, payload minimization, provenance, and CPU certification on return.\n \n See also: `UBU-D0166`, `UBU-D0167`, `UBU-D0168`, `UBU-D0169`, `UBU-D0170`, `UBU-D0171`, `UBU-D0172`, `UBU-D0173`, `UBU-D0174`, `UBU-D0212`, and `PLANNING_KERNEL_CONTRACT.md`.\n \n",
    "commit_message": "Resolve execution provider trust boundary",
    "validation_notes": [
      "Patch hunks were checked with `git apply --check` against the current workspace and applied cleanly.",
      "Patch modifies only allowed files: OPEN_QUESTIONS.md, DECISIONS.md, and DESIGN.md.",
      "Current workspace already had an unrelated OPEN_QUESTIONS.md answerability-score edit for UBU-Q0133; this proposal does not include or depend on that change."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0059"
    ],
    "decisions_added": [
      "UBU-D0254"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "openai-gpt-5-codex-UBU-Q0059-001",
    "patch_applies": true,
    "allowlist_passed": true,
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
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
- Do not score your own provider's proposal unless explicitly asked for diagnostic self-score.

Return only JSON.
