# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0177`  
Question title: `Minimum viable secure local hardware for multimodal interaction`  
Base commit: `2e0d722f1d6a028bf64ca3ab48cb79aa8f9c25fd`

```markdown
## UBU-Q0177: Minimum viable secure local hardware for multimodal interaction

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-12 Scored from commit: None

### Question

What measured local hardware profile can deliver the MVP multimodal experience securely and usefully?

### Current direction

Delta §17.15. Evaluate model quality, memory, latency, energy, sustained thermal behavior, endpoint security and local storage on supported devices. Distinguish primary mobile interaction from mobile planning workload (`UBU-Q0160`). No modern-phone universality or cloud-versus-local quality ranking is assumed.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0177-local-hardware-qualification-2026-10-12",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0177",
    "base_commit": "2e0d722f1d6a028bf64ca3ab48cb79aa8f9c25fd",
    "summary": "Define measured, security-gated qualification for local multimodal hardware; retain Q0177 until real-device evidence supports a reviewed profile.",
    "rationale": "The inspected context supplies interaction and retention contracts but no qualifying hardware measurements. A reproducible profile-specific protocol makes hardware selection reviewable without inventing a RAM floor, assuming user-owned hardware is secure, or changing mobile planning and release-allocation decisions.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5230,3 +5230,15 @@ Require a controller-owned release envelope and step-by-step eligibility checks.\n Stop affected output/acquisition, invalidate pending approvals and reject late or replayed instructions. Explain the permitted reason, unknowns, source state and next route while preserving useful manual planning. Require a fresh explicit request, current evidence/policy bindings and scoped qualified review where applicable before eligible guidance resumes. Dismissal, risk acceptance, rephrasing and professional advice cannot widen release scope. Fallible hazard signals neither prove danger nor control the user's actions.\n \n Keep escalation separate from contact/export/action authority and `UBU-Q0175`'s later expert packets. Preserve source restrictions and existing session/evidence/review/Log contracts. Require \u00a721.1.6's revision-pinned behavioral gates before enablement; this resolves design semantics, not professional replacement, guaranteed safety or completed implementation.\n+\n+---\n+\n+## UBU-D0331: Local multimodal hardware support requires measured Device-profile qualification\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a71. Narrows `UBU-Q0177`; hardware selection and measured acceptance remain Open.\n+\n+Adopt a revision-pinned qualification report for each physical hardware/software configuration and execution-enclave Device role. Freeze useful-outcome criteria and finite limits before real-device trials, then measure quality, cold/warm timings, aggregate memory/storage, energy and sustained thermal behavior alongside the existing interaction, retention and interruption gates. Require endpoint isolation, key/storage protection, persistence-path inspection and denial/cleanup fixtures; user ownership, nominal RAM and model size establish no secure supported minimum.\n+\n+Require DESIGN.md \u00a71's trial counts, sustained sessions, security evidence and fallback demonstrations before operator approval of a scoped support matrix. Missing required evidence or a failed mandatory gate leaves that slice unavailable. Passing grants no Device trust or Compartment authority. Record a tested sufficient profile with limitations, rather than an absolute hardware floor. `UBU-Q0160` retains mobile planning/API selection, `UBU-Q0168` numerical interaction profiles, `UBU-Q0183` release allocation and `UBU-Q0178` lower-cost reach.\n+\n+These are required measurement and implementation checks, not passing results or a selected hardware configuration. `UBU-Q0177` remains Open until the operator reviews actual evidence and selects at least one complete local camera-and-voice configuration for the declared MVP slices.\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -78,6 +78,16 @@ Fixed busy commitments use capacity-occupying Static Tasks at validated UTC star\n \n Before removal, show the exact blocker replacements and residual intervals and obtain explicit operator approval bound to the manifest and current versions. Apply them through ordinary admission as one batch, or an equivalent serialized handoff that prevents Plan certification/projection of intermediate missing or doubled occupancy. Invalidate affected Plans and certify recalculation before reuse or projection. Failure or stale inputs preserve protection and require fresh review. Retire only the identified workaround, preserving source commitments and history. Advancing the horizon or receiving material source changes requires renewed coverage checks before using affected capacity; missing, stale or policy-withheld data never proves free time. Evidence and diagnostics obey current Compartment policy; this process neither grants capture/export authority nor bypasses existing sync/projection checks.\n \n+**Measured local multimodal hardware (`UBU-D0331`).** Qualify an exact physical-hardware/software profile, separately identifying every execution-enclave Device and its capture, review, inference and optional planning roles. Pin hardware/RAM, OS build and security-update posture, drivers/runtime, model and weights digest, quantization, context/media sizes, storage/key configuration, UbU/template and policy revisions. Declare enabled sources, latency classes, concurrency, supported session duration and operating conditions. Primary mobile interaction does not select a full mobile planner or compute API (`UBU-Q0160`); capture-mode/device allocation and delivery sequence remain `UBU-Q0183`.\n+\n+Before testing, freeze the scenario set, useful-outcome rubric and finite acceptance limits for quality, latency/freshness, memory/storage, battery/thermal behavior and cancellation. Use \u00a721.1.3's ordinary interaction and text/form workflows, with synthetic or expressly consented fixtures covering visible details, exact names/values, noisy speech, ambiguity, corrections and unsupported requests. Review grounding, preserved uncertainty, fabricated assertions, correction burden and useful candidate-to-admission/next-action outcomes; transcripts, token rates and plausible conversation alone cannot pass. Count accepted, corrected, unresolved, rejected and failed outcomes separately over all trials. Numeric interaction profiles follow \u00a721.1.4 and remain `UBU-Q0168`'s measured work.\n+\n+For each proposed Device/backend/mode, run at least 30 cold-start and 100 warm trials using the pinned fixtures, plus at least 30 minutes of its proposed concurrent capture/inference/UI workload and the full maximum supported session if longer. Measure the complete capture-to-review route, including any authorized worker transfer. Record ambient conditions, charging/battery state, competing load, instrumentation and repeat procedure. Report \u00a721.1.4's separate timings with sample counts, median, p95, maximum and misses/errors; include peak aggregate CPU/accelerator memory, model loading, buffers/caches and concurrent manual/planning work at the declared state/context limits. Report model/state/approved-artifact storage, peak scratch and required free-space reserve, energy/battery measurements and uncertainty, temperature/throttling and performance over time. Unavailable metrics stay explicit; missing evidence for a required limit prevents qualification. A short successful run cannot establish sustained or longer-session support.\n+\n+Endpoint qualification must demonstrate a maintained security-update posture, installation/model integrity checks, OS access isolation, protected Device keys and access-controlled encrypted persistent storage with documented key custody and recovery. State the threat envelope: locked-device loss, unauthorized unprivileged processes, unintended persistence and unauthorized egress; do not claim protection from a compromised OS or malicious local administrator. Exercise source/Device revocation, lock/restart, denied reads/egress, storage exhaustion and interrupted cleanup. Inventory raw/intermediate/retained copies, OS/runtime swap, hibernation, crash dumps, caches and backups; enforce \u00a712.2 and \u00a721.1.5 on every applicable path or disable the affected mode. Encryption alone cannot satisfy transient-media retention. A rooted/debug planning-parity device supplies no endpoint-security evidence by itself; hardware attestation remains optional under DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a726, and cleanup establishes no forensic-erasure guarantee.\n+\n+Publish a policy-safe support matrix linking the exact profile, enabled workflows/classes, measured usable-memory/free-storage envelope, quality and timing results, sustained operating duration, security/retention outcomes and demonstrated fallback. Test offline/model loss, low battery, thermal/resource pressure, stop/silence, delayed results and manual completion without hidden cloud routing or loss of permitted saved state. A worker on another Device needs its own qualified endpoint/profile and existing route/Compartment checks; a same-Device worker remains an execution context unless separately admitted under \u00a723.1. Ownership grants no disclosure authority. Only profiles passing all declared limits and mandatory security/retention gates may be operator-approved for those slices; material profile changes require affected rechecks. Label the selected configuration a tested sufficient profile among the tested candidates, with failures and limitations visible; infer no universal phone minimum or local-versus-cloud quality ranking. `UBU-Q0177` stays Open until a reviewed evidence-backed profile is selected; a protocol alone cannot close it. Lower-cost reach remains `UBU-Q0178`.\n+\n ---\n \n ## 2. Core Principles\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3354,27 +3354,29 @@ Open.\n ## UBU-Q0177: Minimum viable secure local hardware for multimodal interaction\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-12 Scored from commit: None\n \n ### Question\n \n What measured local hardware profile can deliver the MVP multimodal experience securely and usefully?\n \n ### Current direction\n \n Delta \u00a717.15. Evaluate model quality, memory, latency, energy, sustained thermal behavior, endpoint security and local storage on supported devices. Distinguish primary mobile interaction from mobile planning workload (`UBU-Q0160`). No modern-phone universality or cloud-versus-local quality ranking is assumed.\n \n+`UBU-D0331` specifies revision-pinned Device-role profiles, predeclared usefulness/resource criteria, real-device cold/warm and sustained trials, endpoint/storage gates and a reviewed support matrix in DESIGN.md \u00a71. This narrows the qualification process without selecting hardware, a mobile planner or a compute API.\n+\n ### Resolution\n \n-Open.\n+Open. `UBU-D0331` answers the qualification protocol only. Close after actual real-device evidence demonstrates the declared MVP slices, numeric latency/resource profiles, endpoint and retention controls, sustained behavior and useful manual degradation, and the operator approves a scoped hardware/software support matrix. No measured minimum or supported Device is established by the protocol.\n \n ---\n \n ## UBU-Q0178: Economically accessible local compute paths\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-12 Scored from commit: None\n \n ### Question\n \n Which lower-cost local compute paths prevent economically precarious users from being excluded from the multimodal experience?\n \n ### Current direction\n",
    "commit_message": "docs: define secure local multimodal hardware qualification",
    "validation_notes": [
      "Generated git-diff-style unified hunks with git diff --no-index from current working-tree files versus proposed contents supplied through stdin; normalized stdin file headers and omitted index metadata. No HEAD or Git-index baseline was used.",
      "git apply --check - passed against /home/sean/ubu-design working-tree files without --cached or --recount.",
      "Preserved the selected question's metadata line exactly, including its 2026-10-12 score, Open status and Resolved by: None; changes are confined to its own direction and Resolution.",
      "Verified UBU-D0331 was unused, exact hunk ranges, the allowlisted three-file scope and unchanged nonselected question blocks; repository files remain byte-for-byte unchanged.",
      "No real-device benchmarks or security/retention implementation tests were run. Trial counts and durations are proposed requirements, not observed performance; Q0177 remains Open and human review is required."
    ],
    "new_questions_added": [],
    "questions_resolved": [],
    "decisions_added": [
      "UBU-D0331"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0177-local-hardware-qualification-2026-10-12",
    "patch_applies": true,
    "allowlist_passed": true,
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "question_schema_passed": true,
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
- Allowed question `Status:` values are `Archived`, `Decomposed`, `Deferred`, `Open`, `Solved`, `Superseded`; a resolved question must use `Status: Solved`.
- Reject `Status: Resolved` as a question-schema violation. Set `preserves_question_schema` to false and require `Status: Solved` as the fix; `Resolved by:` is a separate metadata field.
- Do not score your own provider's proposal unless explicitly asked for diagnostic self-score.

Return only JSON.
