# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0135`  
Question title: `Phase 1b local advisory worker protocol`  
Base commit: `50bebb3caae7d6a156b9210087ec3a7814eef9ac`

```markdown
## UBU-Q0135: Phase 1b local advisory worker protocol

Status: Open Priority: MVP important Phase: Phase 1b Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0145, UBU-Q0059 Blocks: Phase 1b advisory operations, Phase 3 worker substitution Resolved by: None Last scored: 2026-09-16 Scored from commit: None

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

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "openai-gpt-5-codex-UBU-Q0135-20260917-01",
    "provider_id": "openai",
    "model_name": "gpt-5-codex",
    "question_id": "UBU-Q0135",
    "base_commit": "50bebb3caae7d6a156b9210087ec3a7814eef9ac",
    "summary": "Resolve UBU-Q0135 with a concrete Phase 1b local advisory worker protocol: scoped WorkerAuthority, local submission/result envelopes, failure/status handling, provider/model provenance, enforced no-write boundary, and remote-transport invariants.",
    "rationale": "UBU-D0254 already establishes the advisory backend trust boundary, while UBU-D0250 establishes the later sync-carried worker Device shape. This proposal narrows Phase 1b to the local worker case so implementation can proceed without granting the worker privileged state access: all local advisory output remains proposal material that must pass controller admission. The fields that later remote workers must preserve are explicitly named, keeping remote/rented/multi-provider support a transport substitution rather than a redesign.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\nindex 64e6365..6974d79 100644\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -3904,3 +3904,21 @@ Routing chooses the least-disclosing backend that satisfies the requested capabi\n Before any external backend receives work, the controller must evaluate Compartment policy, export limits, Device or provider trust, `no_cloud_llm`-style constraints, retention rules, and purpose compatibility. The transmitted payload is a minimized `PlanningRequest`, `worker_request`, or derived context bundle over a redacted Replica: direct PII, stable object IDs, Compartment labels, secrets, integration credentials, and unrelated UniverseState are omitted, generalized, or replaced with redacted handles unless policy explicitly authorizes disclosure. Transport must be encrypted; retained payloads must obey the request retention deadline; diagnostics and logs must avoid reintroducing stripped identifiers. The request records a payload-safety proof or equivalent provenance summary, and the result echoes the policy versions, input digests or structural references, provider/model identity, telemetry safe for the authority level, and deletion confirmation when transient payload deletion was required.\n \n Dedicated personal worker appliances are future packaging or reference-hardware work, not a special trust class. They must register and operate as ordinary user-owned worker Devices. Future practical FHE-backed or comparable encrypted compute is a strategic research backend class: it is worth tracking and may reduce disclosure when viable, but it must still use the same authority, capability, provenance, routing, and CPU-certification boundary unless a later accepted design proves stronger semantics. Idle-compute monetization, if ever supported, should be an external API-enabled user choice with explicit opt-in, revocation, policy limits, and auditability rather than an UbU corporate marketplace or a default use of personal compute.\n+\n+---\n+\n+## UBU-D0255: Phase 1b local advisory worker uses scoped request/result envelopes\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a716.8. Resolves `UBU-Q0135`.\n+\n+Phase 1b local advisory workers use the execution-backend boundary from `UBU-D0254` as a concrete local protocol, not as an internal privileged service. The controller creates a `LocalAdvisorySubmission` for each advisory call. The submission contains `request_id`, `operation`, `purpose`, causal parents, observed state or request versions, timeout and compute budget, expected result schema, result size limit, whether partial results are allowed, an authority envelope, a minimized payload, and provenance/configuration metadata. The payload is by value: a `PlanningRequest`, redacted Replica slice, projection-preview bundle, extraction bundle, or other declared context bundle assembled by the CPU/controller after Compartment and export checks.\n+\n+The `WorkerAuthority` grants exactly one or more additive advisory capabilities: candidate planning, simulation, local extraction, summarization, projection-preview preparation, repair suggestion, or candidate-mutation generation. It also records backend kind `local_advisory`, provider/runtime identity, allowed `zone_id` and `compartment_ids`, redaction level, policy-version references, retention deadline, export/network/tool permissions, compute budget, and response schema. Anything not granted is denied. A Phase 1b local advisory worker is explicitly denied canonical-state writes, admission decisions, direct StateStore handles, additional protected-context fetches, authority expansion, Compartment/export policy decisions, stable identifier disclosure beyond the payload, integration credentials, external network/tool use unless specifically granted, and retention after the declared deadline.\n+\n+Model provenance is required when the worker uses an LLM, solver, embedding model, or learned component. The submission and result record `provider_id`, backend adapter name and version, model name, model version or digest when available, runtime or endpoint identity, generation parameters that can affect output, prompt/template or tool schema version refs, RNG seed when applicable, local binary/container/build identifier when available, and safe telemetry such as elapsed time and token or resource counts. These fields are provenance and replay aids, not authority grants.\n+\n+The worker returns a `LocalAdvisoryResult` that echoes `request_id`, authority id or digest, policy versions, input digests or structural refs, provider/model identity, status, diagnostics, safe telemetry, produced artifacts, proposed Plan candidates, proposed candidate mutations, and deletion confirmation when transient payload deletion was required. Status is one of `ok`, `partial`, `rejected`, `timeout`, `worker_error`, `malformed_result`, or `cancelled`. `partial` is valid only when the submission allowed partial results and each included artifact independently passes schema validation. Timeouts, errors, cancellation, and malformed results are represented as diagnostics and optional quarantined raw-result digests; they do not create or mutate candidates.\n+\n+The only path from advisory output to canonical state is the controlling CPU/admission path. The controller may translate schema-valid artifacts into candidate-queue entries, projection previews, diagnostics, or candidate mutation envelopes, each carrying the submission/result provenance. Candidate mutations are proposals only and are admitted, rejected, or routed to user review through ordinary admission, policy, provenance, hard-constraint, affect/legitimacy, and mutation validation. The worker process or function is not passed a mutable canonical-state capability; implementation interfaces expose only the submission and a result return channel, while the StateStore/admission writer remains CPU-owned. This is an enforcement requirement, not a convention.\n+\n+When transport later becomes remote, rented, or multi-provider, the invariant fields remain unchanged: scoped authority, payload minimization proof, request/result ids, capability set, expected schema, status semantics, provenance/model identity, input digests, retention/deletion semantics, result size and partial-result rules, and CPU/admission certification. Remote transport may add encryption, signatures, provider attestations, sync-carried `worker_request`/`worker_result` statements, and stronger deletion evidence, but it must not introduce a second mutation channel or move policy/admission authority to the worker.\ndiff --git a/DESIGN.md b/DESIGN.md\nindex d1aee1f..20edaf0 100644\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -2300,6 +2300,12 @@ The FOSS planner and Compact Calendar representation should be backend-agnostic.\n \n **Backend trust boundary (`UBU-D0254`).** Backends beyond the controlling local instance are advisory workers behind an explicit `WorkerAuthority`; they may propose candidates, simulations, extraction or summary artifacts, projection previews, diagnostics, repair suggestions, and candidate mutations, but they do not mutate canonical state or decide their own scope. Returned artifacts are usable only after schema validation, Compartment/export admission, provenance validation, hard-constraint certification, and ordinary CPU commit or review paths.\n \n+**Phase 1b local advisory worker protocol (`UBU-D0255`).** A local advisory worker is invoked through a `LocalAdvisorySubmission` with a `WorkerAuthority`, minimized by-value payload, expected result schema, timeout, compute budget, result size limit, partial-result allowance, causal parents, policy-version refs, input digests, and provider/model configuration metadata. Its `WorkerAuthority` grants only named advisory capabilities and denies canonical writes, admission decisions, direct StateStore handles, extra context fetches, authority expansion, export-policy decisions, ungranted network/tool use, credential access, and retention past the request deadline.\n+\n+A `LocalAdvisoryResult` echoes the authority, request, policy, input, provider, model, and configuration provenance and returns status `ok`, `partial`, `rejected`, `timeout`, `worker_error`, `malformed_result`, or `cancelled` with schema-valid artifacts, proposed candidates, proposed candidate mutations, diagnostics, safe telemetry, and deletion confirmation when required. Errors, timeouts, cancelled work, and malformed raw output are diagnostic records or quarantined digests, not candidate-state mutations; partial output is usable only artifact by artifact after schema validation.\n+\n+The worker receives no mutable canonical-state capability. Only the controller/admission path can translate valid advisory artifacts into candidate-queue entries, projection previews, diagnostics, or candidate mutation envelopes, and those proposals remain subject to ordinary schema, policy, provenance, hard-constraint, affect/legitimacy, mutation, and user-review checks. Later remote, rented, or multi-provider workers must preserve the same authority envelope, status semantics, provenance, payload-minimization proof, input digests, retention/deletion semantics, result-size and partial-result rules, and CPU/admission certification; transport may add encryption, signatures, attestations, or sync-carried `worker_request`/`worker_result` statements but not a second mutation channel.\n+\n The open-core boundary includes the canonical planning schemas, Compact Calendar representation, deterministic skeletonization and precondition/effect evaluation, conservative CPU hard-constraint validation, admission and provenance checks, payload-safety validation, and enough local execution-profile support to produce a legitimate default Plan or blocking diagnostic without a hidden service. Hosted, rented, GPU, learned-model, or third-party compute may improve speed and depth, but they are replaceable request/result providers.\n \n Routing selects the least-disclosing capable backend under policy, freshness, latency, energy, and compute-budget constraints. Mobile local execution remains the stewardship fallback for current/next Task clarity and cached repair; local desktop/laptop execution is the default high-fidelity self-hosted path when available; Phase 2 user-owned workers use the sync-carried worker protocol; corporate hosted compute and third-party UbU-compatible providers require explicit policy allowance and user-visible routing disclosure. Third-party providers receive no privileged access over local execution and must use the same authority, payload-safety proof, provenance, response validation, and CPU certification boundary as UbU-hosted providers.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\nindex fe79230..0ef3a3a 100644\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -2366,7 +2366,7 @@ Open.\n \n ## UBU-Q0135: Phase 1b local advisory worker protocol\n \n-Status: Open Priority: MVP important Phase: Phase 1b Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0145, UBU-Q0059 Blocks: Phase 1b advisory operations, Phase 3 worker substitution Resolved by: None Last scored: 2026-09-16 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: UBU-Q0145, UBU-Q0059 Blocks: Phase 1b advisory operations, Phase 3 worker substitution Resolved by: UBU-D0255 Last scored: 2026-09-16 Scored from commit: None\n \n ### Question\n \n@@ -2387,7 +2387,7 @@ Phase 1b builds the local advisory worker behind a scoped authority and the admi\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0255`: Phase 1b local advisory workers use a scoped local request/result protocol under `WorkerAuthority`; the worker receives only minimized by-value payloads, returns proposal artifacts or diagnostics, and has no direct canonical-state mutation path. Local output reaches state only through the controller's candidate queue and ordinary admission, preserving the same authority, provenance, status, retention, and CPU-certification boundary required for later remote transport substitution.\n \n ---\n \n",
    "commit_message": "Resolve Phase 1b local advisory worker protocol",
    "validation_notes": [
      "Patch is limited to OPEN_QUESTIONS.md, DECISIONS.md, and DESIGN.md.",
      "OPEN_QUESTIONS.md edit is anchored inside the UBU-Q0135 block and preserves the single-line metadata format.",
      "No changes are proposed to DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md or implementation files.",
      "Local workspace is read-only, so the patch was constructed from inspected line context rather than applied.",
      "Patch required --recount normalization."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0135"
    ],
    "decisions_added": [
      "UBU-D0255"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "openai-gpt-5-codex-UBU-Q0135-20260917-01",
    "patch_applies": true,
    "allowlist_passed": true,
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "error": null,
    "normalized_patch": "diff --git a/DECISIONS.md b/DECISIONS.md\nindex 64e6365..6974d79 100644\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -3904,3 +3904,21 @@ Routing chooses the least-disclosing backend that satisfies the requested capabi\n Before any external backend receives work, the controller must evaluate Compartment policy, export limits, Device or provider trust, `no_cloud_llm`-style constraints, retention rules, and purpose compatibility. The transmitted payload is a minimized `PlanningRequest`, `worker_request`, or derived context bundle over a redacted Replica: direct PII, stable object IDs, Compartment labels, secrets, integration credentials, and unrelated UniverseState are omitted, generalized, or replaced with redacted handles unless policy explicitly authorizes disclosure. Transport must be encrypted; retained payloads must obey the request retention deadline; diagnostics and logs must avoid reintroducing stripped identifiers. The request records a payload-safety proof or equivalent provenance summary, and the result echoes the policy versions, input digests or structural references, provider/model identity, telemetry safe for the authority level, and deletion confirmation when transient payload deletion was required.\n \n Dedicated personal worker appliances are future packaging or reference-hardware work, not a special trust class. They must register and operate as ordinary user-owned worker Devices. Future practical FHE-backed or comparable encrypted compute is a strategic research backend class: it is worth tracking and may reduce disclosure when viable, but it must still use the same authority, capability, provenance, routing, and CPU-certification boundary unless a later accepted design proves stronger semantics. Idle-compute monetization, if ever supported, should be an external API-enabled user choice with explicit opt-in, revocation, policy limits, and auditability rather than an UbU corporate marketplace or a default use of personal compute.\n+\n+---\n+\n+## UBU-D0255: Phase 1b local advisory worker uses scoped request/result envelopes\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a716.8. Resolves `UBU-Q0135`.\n+\n+Phase 1b local advisory workers use the execution-backend boundary from `UBU-D0254` as a concrete local protocol, not as an internal privileged service. The controller creates a `LocalAdvisorySubmission` for each advisory call. The submission contains `request_id`, `operation`, `purpose`, causal parents, observed state or request versions, timeout and compute budget, expected result schema, result size limit, whether partial results are allowed, an authority envelope, a minimized payload, and provenance/configuration metadata. The payload is by value: a `PlanningRequest`, redacted Replica slice, projection-preview bundle, extraction bundle, or other declared context bundle assembled by the CPU/controller after Compartment and export checks.\n+\n+The `WorkerAuthority` grants exactly one or more additive advisory capabilities: candidate planning, simulation, local extraction, summarization, projection-preview preparation, repair suggestion, or candidate-mutation generation. It also records backend kind `local_advisory`, provider/runtime identity, allowed `zone_id` and `compartment_ids`, redaction level, policy-version references, retention deadline, export/network/tool permissions, compute budget, and response schema. Anything not granted is denied. A Phase 1b local advisory worker is explicitly denied canonical-state writes, admission decisions, direct StateStore handles, additional protected-context fetches, authority expansion, Compartment/export policy decisions, stable identifier disclosure beyond the payload, integration credentials, external network/tool use unless specifically granted, and retention after the declared deadline.\n+\n+Model provenance is required when the worker uses an LLM, solver, embedding model, or learned component. The submission and result record `provider_id`, backend adapter name and version, model name, model version or digest when available, runtime or endpoint identity, generation parameters that can affect output, prompt/template or tool schema version refs, RNG seed when applicable, local binary/container/build identifier when available, and safe telemetry such as elapsed time and token or resource counts. These fields are provenance and replay aids, not authority grants.\n+\n+The worker returns a `LocalAdvisoryResult` that echoes `request_id`, authority id or digest, policy versions, input digests or structural refs, provider/model identity, status, diagnostics, safe telemetry, produced artifacts, proposed Plan candidates, proposed candidate mutations, and deletion confirmation when transient payload deletion was required. Status is one of `ok`, `partial`, `rejected`, `timeout`, `worker_error`, `malformed_result`, or `cancelled`. `partial` is valid only when the submission allowed partial results and each included artifact independently passes schema validation. Timeouts, errors, cancellation, and malformed results are represented as diagnostics and optional quarantined raw-result digests; they do not create or mutate candidates.\n+\n+The only path from advisory output to canonical state is the controlling CPU/admission path. The controller may translate schema-valid artifacts into candidate-queue entries, projection previews, diagnostics, or candidate mutation envelopes, each carrying the submission/result provenance. Candidate mutations are proposals only and are admitted, rejected, or routed to user review through ordinary admission, policy, provenance, hard-constraint, affect/legitimacy, and mutation validation. The worker process or function is not passed a mutable canonical-state capability; implementation interfaces expose only the submission and a result return channel, while the StateStore/admission writer remains CPU-owned. This is an enforcement requirement, not a convention.\n+\n+When transport later becomes remote, rented, or multi-provider, the invariant fields remain unchanged: scoped authority, payload minimization proof, request/result ids, capability set, expected schema, status semantics, provenance/model identity, input digests, retention/deletion semantics, result size and partial-result rules, and CPU/admission certification. Remote transport may add encryption, signatures, provider attestations, sync-carried `worker_request`/`worker_result` statements, and stronger deletion evidence, but it must not introduce a second mutation channel or move policy/admission authority to the worker.\ndiff --git a/DESIGN.md b/DESIGN.md\nindex d1aee1f..20edaf0 100644\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -2300,6 +2300,12 @@ The FOSS planner and Compact Calendar representation should be backend-agnostic.\n \n **Backend trust boundary (`UBU-D0254`).** Backends beyond the controlling local instance are advisory workers behind an explicit `WorkerAuthority`; they may propose candidates, simulations, extraction or summary artifacts, projection previews, diagnostics, repair suggestions, and candidate mutations, but they do not mutate canonical state or decide their own scope. Returned artifacts are usable only after schema validation, Compartment/export admission, provenance validation, hard-constraint certification, and ordinary CPU commit or review paths.\n \n+**Phase 1b local advisory worker protocol (`UBU-D0255`).** A local advisory worker is invoked through a `LocalAdvisorySubmission` with a `WorkerAuthority`, minimized by-value payload, expected result schema, timeout, compute budget, result size limit, partial-result allowance, causal parents, policy-version refs, input digests, and provider/model configuration metadata. Its `WorkerAuthority` grants only named advisory capabilities and denies canonical writes, admission decisions, direct StateStore handles, extra context fetches, authority expansion, export-policy decisions, ungranted network/tool use, credential access, and retention past the request deadline.\n+\n+A `LocalAdvisoryResult` echoes the authority, request, policy, input, provider, model, and configuration provenance and returns status `ok`, `partial`, `rejected`, `timeout`, `worker_error`, `malformed_result`, or `cancelled` with schema-valid artifacts, proposed candidates, proposed candidate mutations, diagnostics, safe telemetry, and deletion confirmation when required. Errors, timeouts, cancelled work, and malformed raw output are diagnostic records or quarantined digests, not candidate-state mutations; partial output is usable only artifact by artifact after schema validation.\n+\n+The worker receives no mutable canonical-state capability. Only the controller/admission path can translate valid advisory artifacts into candidate-queue entries, projection previews, diagnostics, or candidate mutation envelopes, and those proposals remain subject to ordinary schema, policy, provenance, hard-constraint, affect/legitimacy, mutation, and user-review checks. Later remote, rented, or multi-provider workers must preserve the same authority envelope, status semantics, provenance, payload-minimization proof, input digests, retention/deletion semantics, result-size and partial-result rules, and CPU/admission certification; transport may add encryption, signatures, attestations, or sync-carried `worker_request`/`worker_result` statements but not a second mutation channel.\n+\n The open-core boundary includes the canonical planning schemas, Compact Calendar representation, deterministic skeletonization and precondition/effect evaluation, conservative CPU hard-constraint validation, admission and provenance checks, payload-safety validation, and enough local execution-profile support to produce a legitimate default Plan or blocking diagnostic without a hidden service. Hosted, rented, GPU, learned-model, or third-party compute may improve speed and depth, but they are replaceable request/result providers.\n \n Routing selects the least-disclosing capable backend under policy, freshness, latency, energy, and compute-budget constraints. Mobile local execution remains the stewardship fallback for current/next Task clarity and cached repair; local desktop/laptop execution is the default high-fidelity self-hosted path when available; Phase 2 user-owned workers use the sync-carried worker protocol; corporate hosted compute and third-party UbU-compatible providers require explicit policy allowance and user-visible routing disclosure. Third-party providers receive no privileged access over local execution and must use the same authority, payload-safety proof, provenance, response validation, and CPU certification boundary as UbU-hosted providers.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\nindex fe79230..0ef3a3a 100644\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -2366,7 +2366,7 @@ Open.\n \n ## UBU-Q0135: Phase 1b local advisory worker protocol\n \n-Status: Open Priority: MVP important Phase: Phase 1b Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0145, UBU-Q0059 Blocks: Phase 1b advisory operations, Phase 3 worker substitution Resolved by: None Last scored: 2026-09-16 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: UBU-Q0145, UBU-Q0059 Blocks: Phase 1b advisory operations, Phase 3 worker substitution Resolved by: UBU-D0255 Last scored: 2026-09-16 Scored from commit: None\n \n ### Question\n \n@@ -2387,7 +2387,7 @@ Phase 1b builds the local advisory worker behind a scoped authority and the admi\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0255`: Phase 1b local advisory workers use a scoped local request/result protocol under `WorkerAuthority`; the worker receives only minimized by-value payloads, returns proposal artifacts or diagnostics, and has no direct canonical-state mutation path. Local output reaches state only through the controller's candidate queue and ordinary admission, preserving the same authority, provenance, status, retention, and CPU-certification boundary required for later remote transport substitution.\n \n ---\n \n",
    "warnings": [
      "Patch required --recount normalization."
    ],
    "ordinary_error": "error: corrupt patch at line 57",
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
