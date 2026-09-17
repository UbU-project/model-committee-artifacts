# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0136`  
Question title: `Phase 1b single-Device token and worker-credential custody`  
Base commit: `40cf4bb7e8b2f4549debd6c51df6612e111ffe78`

```markdown
## UBU-Q0136: Phase 1b single-Device token and worker-credential custody

Status: Open Priority: MVP important Phase: Phase 1b Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0149, UBU-Q0135 Blocks: Phase 1b external projection adapters, advisory worker configuration Resolved by: None Last scored: 2026-09-17 Scored from commit: None

### Question

How are external integration tokens and local worker credentials stored, scoped, and revoked on a single Phase 1b Device without violating Compartment policy?

### Subquestions

1. Where do external OAuth tokens live relative to the canonical store, and are they canonical objects at all?
2. What Compartment label governs a credential, and may a worker operating in one Compartment read a credential labelled for another?
3. How is a token revoked or rotated, and what happens to queued projection operations that depended on it?
4. What worker configuration counts as a credential: a local endpoint, a model name, a remote provider key?
5. What must never be replicated, projected, or logged, and how is that enforced at egress rather than by convention?
6. How are credentials excluded from store exports, backups, and diagnostic bundles?

### Current direction

Phase 1b holds external OAuth tokens and local worker configuration from its first adapter ticket onward, so custody is decided de facto unless decided deliberately. This entry is scoped to a single Device. Cross-Device replication, rotation propagation, and local encryption-key custody remain open under UBU-Q0149, as does encryption at rest, which is explicitly not claimed before Phase 2.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0136-20260917-openai-gpt5-codex-01",
    "provider_id": "openai",
    "model_name": "gpt-5-codex",
    "question_id": "UBU-Q0136",
    "base_commit": "40cf4bb7e8b2f4549debd6c51df6612e111ffe78",
    "summary": "Resolve UBU-Q0136 by defining Phase 1b single-Device credential custody as Device-local secret capabilities outside the canonical StateStore, with scoped metadata handles, controller-brokered use, revocation/rotation version checks, and egress enforcement for logs, workers, projections, exports, backups, and diagnostics.",
    "rationale": "This follows UBU-D0252 secret-capability semantics while narrowing them to the Phase 1b single-Device scope and preserving UBU-D0255 WorkerAuthority boundaries. It avoids claiming cross-Device replication or encryption-at-rest, blocks workers from receiving credential bytes, and makes credential exclusion an enforced serialization and egress property rather than a convention.",
    "changed_files": [
      "OPEN_QUESTIONS.md",
      "DESIGN.md",
      "DECISIONS.md"
    ],
    "patch": "diff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -2394,7 +2394,7 @@\n ## UBU-Q0136: Phase 1b single-Device token and worker-credential custody\n \n-Status: Open Priority: MVP important Phase: Phase 1b Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 90 Depends on: UBU-Q0149, UBU-Q0135 Blocks: Phase 1b external projection adapters, advisory worker configuration Resolved by: None Last scored: 2026-09-17 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Security Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: UBU-Q0149, UBU-Q0135 Blocks: Phase 1b external projection adapters, advisory worker configuration Resolved by: UBU-D0262 Last scored: 2026-09-17 Scored from commit: None\n \n ### Question\n \n How are external integration tokens and local worker credentials stored, scoped, and revoked on a single Phase 1b Device without violating Compartment policy?\n@@ -2413,9 +2413,9 @@\n Phase 1b holds external OAuth tokens and local worker configuration from its first adapter ticket onward, so custody is decided de facto unless decided deliberately. This entry is scoped to a single Device. Cross-Device replication, rotation propagation, and local encryption-key custody remain open under UBU-Q0149, as does encryption at rest, which is explicitly not claimed before Phase 2.\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0262`: Phase 1b keeps external tokens and worker credential values outside the canonical StateStore as Device-local secret capabilities. Canonical state may hold only scoped metadata and opaque secret handles; use is brokered by the controller under Compartment policy, WorkerAuthority or projection authority, capability version, and revocation state. Credential values are denied to workers and to all projection, worker, log, export, backup, and diagnostic egress; queued work depending on revoked or rotated credentials blocks, cancels, retries after reauthorization, or surfaces review rather than silently reusing or substituting secrets.\n \n ---\n \n ## UBU-Q0137: Phase 1b BackgroundProcess model for unattended advisory batches\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -2351,7 +2351,15 @@\n \n A `LocalAdvisoryResult` echoes the authority, request, policy, input, provider, model, and configuration provenance and returns status `ok`, `partial`, `rejected`, `timeout`, `worker_error`, `malformed_result`, or `cancelled` with schema-valid artifacts, proposed candidates, proposed candidate mutations, diagnostics, safe telemetry, and deletion confirmation when required. Errors, timeouts, cancelled work, and malformed raw output are diagnostic records or quarantined digests, not candidate-state mutations; partial output is usable only artifact by artifact after schema validation.\n \n The worker receives no mutable canonical-state capability. Only the controller/admission path can translate valid advisory artifacts into candidate-queue entries, projection previews, diagnostics, or candidate mutation envelopes, and those proposals remain subject to ordinary schema, policy, provenance, hard-constraint, affect/legitimacy, mutation, and user-review checks. Later remote, rented, or multi-provider workers must preserve the same authority envelope, status semantics, provenance, payload-minimization proof, input digests, retention/deletion semantics, result-size and partial-result rules, and CPU/admission certification; transport may add encryption, signatures, attestations, or sync-carried `worker_request`/`worker_result` statements but not a second mutation channel.\n \n+**Phase 1b single-Device credential custody (`UBU-D0262`).** External OAuth tokens, refresh tokens, provider API keys, client secrets, local endpoint authentication material, and worker credentials are Device-local secret values held outside the canonical StateStore. Canonical state may contain only policy metadata and opaque `SecretCapability` or secret-store handles: kind, owning integration or worker authority, operation scope, allowed `zone_id` and `compartment_ids`, version, status, retention/export class, and audit refs. This does not claim encryption at rest beyond the local platform secret store before Phase 2.\n+\n+The Phase 1b controller labels a credential by the Compartments whose data or external authority it can read, write, or affect; broad provider tokens inherit the broadest applicable label. A worker or projection operation may use a credential only through the controller's credential broker when its authority envelope, purpose, policy versions, and requested operation are within that secret capability. Workers do not receive or read credential material, and a worker scoped to one Compartment cannot use a credential labelled for another.\n+\n+Revocation and rotation are local capability-state changes. Revocation disables new use immediately on the Device; rotation creates a new credential version and disables the old version for new dispatch. Queued projection operations and advisory submissions record the credential capability id and version they expected; dispatch rechecks the current state and cancels, blocks, or asks for reauthorization rather than silently substituting credentials. Completed external side effects remain reconciled through projection records and conflict handling.\n+\n+Credential egress is enforced by typed secret references and central serializers, not by caller convention. Projection records, worker submissions/results, canonical payloads, logs, diagnostics, store exports, backups, and diagnostic bundles must reject reusable bearer tokens, API keys, refresh tokens, client secrets, authentication headers/cookies, local secret-store lookup material, and raw credential values; at most they may include an authorized opaque capability id/version or redacted handle.\n+\n The open-core boundary includes the canonical planning schemas, Compact Calendar representation, deterministic skeletonization and precondition/effect evaluation, conservative CPU hard-constraint validation, admission and provenance checks, payload-safety validation, and enough local execution-profile support to produce a legitimate default Plan or blocking diagnostic without a hidden service. Hosted, rented, GPU, learned-model, or third-party compute may improve speed and depth, but they are replaceable request/result providers.\n \n Routing selects the least-disclosing capable backend under policy, freshness, latency, energy, and compute-budget constraints. Mobile local execution remains the stewardship fallback for current/next Task clarity and cached repair; local desktop/laptop execution is the default high-fidelity self-hosted path when available; Phase 2 user-owned workers use the sync-carried worker protocol; corporate hosted compute and third-party UbU-compatible providers require explicit policy allowance and user-visible routing disclosure. Third-party providers receive no privileged access over local execution and must use the same authority, payload-safety proof, provenance, response validation, and CPU certification boundary as UbU-hosted providers.\ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -4030,3 +4030,23 @@\n Automatic projection reconciliation is never silent. Each automatic action records a first-class `projection_conflict_event` carrying `conflict_id`, projection surface, quadrant, severity, canonical object ref when any, external object ref or redacted handle when policy allows it, observed canonical and policy versions, external delta digest, action taken, resulting mutation or projection-operation refs, candidate refs when relevant, and recorded time. If the action admits canonical state, it uses the ordinary mutation envelope and tombstone rules; if it only retries, ignores, quarantines, recalculates, or repairs projection state, the event still remains auditable as diagnostic/projection state.\n \n When an external edit conflicts with a still-pending candidate for the same object, neither side is admitted by default. Exact duplicate content or idempotency evidence collapses to the pending candidate result. Deterministic non-overlapping edits may annotate or revise the pending candidate proposal while preserving observed-version checks. Overlapping edits quarantine or supersede the candidate and surface a `major` review diagnostic, escalating to `blocking` when no legitimate current or next Task can be identified without resolving the conflict. The operator's decision, when required, enters state only through the normal conflict-resolution or candidate-admission path.\n+\n+---\n+\n+## UBU-D0262: Phase 1b credentials are Device-local secret capabilities\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a716.8. Resolves `UBU-Q0136`.\n+\n+Phase 1b resolves single-Device custody by adopting `UBU-D0252`'s secret-capability semantics without cross-Device replication or an encryption-at-rest claim. External OAuth access tokens, refresh tokens, provider API keys, client secrets, authentication headers or cookies, local endpoint credentials, and remote worker/provider credentials are secret values held outside the canonical StateStore in the Device's local secret store or equivalent operator-controlled local custody. Canonical state may contain only metadata and opaque handles: secret capability id, kind, owning integration or worker authority, provider/surface, allowed operations, `zone_id`, `compartment_ids`, version, status, retention/export class, and audit refs.\n+\n+A credential is labelled by the Compartments whose data or external authority it can read, write, summarize, project, or affect; if a provider token can reach multiple Compartments, it is labelled at the broadest applicable policy scope. A worker or projection operation may use a credential only when the current Device trust state, Compartment policy, purpose, `WorkerAuthority` or projection authority, capability scope, version, and revocation state all allow that use. A worker scoped to one Compartment may not read or use a credential labelled for another; workers receive no credential bytes and can access secrets only through a controller-owned brokered operation that produces the permitted external effect or request.\n+\n+Worker configuration is a credential when it authorizes access, spends a provider account, reveals secret routing, or selects a hidden secret: API keys, client secrets, bearer/refresh tokens, auth headers, cookies, mTLS material, non-public local endpoint tokens, and provider account selectors are credentials. A model name, adapter version, generation parameter, or unauthenticated loopback endpoint is provenance/configuration rather than a credential, though it may still be policy-sensitive metadata if it reveals private routing or provider use. Provenance fields never grant authority.\n+\n+Revocation is an admitted local capability-state change that disables new use immediately on the Device. Rotation creates a new credential version, marks the old version unavailable for new dispatch, and records any reauthorization or cleanup obligation. Queued projection operations, background jobs, and advisory submissions must carry the capability id and version they were authorized against; dispatch rechecks the current capability state and cancels, blocks, retries after reauthorization, or surfaces review if the version is revoked, rotated, expired, or outside policy. Already-completed external side effects are reconciled through projection records, tombstones, and projection-conflict handling; revocation is not represented as proof that an external service erased prior effects.\n+\n+Reusable secret values must never appear in canonical objects, mutation payloads, `projection_record`s, worker submissions/results, redacted replicas, Compact Calendars, Logs, diagnostics, prompts, raw result archives, telemetry, store exports, backups, or diagnostic bundles. Allowed references are opaque capability ids, version ids, revocation/rotation records, and redacted handles when the target authority may know that such a capability exists.\n+\n+Enforcement is at egress and serialization. Secret material is represented inside trusted code as a typed secret reference or secret-store handle whose value cannot be rendered into ordinary JSON payloads. The projection dispatcher, worker submission builder, log/diagnostic writer, export/backup pipeline, and diagnostic-bundle builder share a clearance filter that rejects secret-bearing fields, scans declared payload classes for forbidden secret types, and fails closed when a value is not classified. Diagnostics may record that a credential was missing, revoked, expired, or denied by policy, but not the credential value or enough lookup material to recover it.\n+\n+Phase 1b exports and backups exclude the local secret store by default. They may include redacted secret capability metadata only when policy permits structural disclosure and must mark dependent projection/advisory queues as needing reauthorization on restore. This leaves cross-Device replication, recovery wrapping, rotation propagation, local encryption-key custody, and concrete cryptographic or OS-keychain choices to `UBU-D0252` and later implementation design.\n",
    "commit_message": "Resolve UBU-Q0136 Phase 1b credential custody",
    "validation_notes": [
      "Confirmed repository HEAD matches base commit 40cf4bb7e8b2f4549debd6c51df6612e111ffe78.",
      "git apply --check passed for the proposed patch.",
      "Patch modifies only OPEN_QUESTIONS.md, DESIGN.md, and DECISIONS.md."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0136"
    ],
    "decisions_added": [
      "UBU-D0262"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0136-20260917-openai-gpt5-codex-01",
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
