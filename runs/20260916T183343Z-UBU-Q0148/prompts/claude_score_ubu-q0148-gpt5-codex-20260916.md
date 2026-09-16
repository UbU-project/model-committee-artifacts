# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0148`  
Question title: `Distinguishing physical devices, UbU Devices, installs, and workers`  
Base commit: `bc52aaa5d6a3bc77abb095616329ebb349c3acad`

```markdown
## UBU-Q0148: Distinguishing physical devices, UbU Devices, installs, and workers

Status: Open Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0131, Phase 2 Device registry Resolved by: None Last scored: 2026-09-16 Scored from commit: None

Formerly UBU-QSYNC-010 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §4, §5.

### Question

How should UbU distinguish physical devices, UbU Devices/execution enclaves, app installs, browser
sessions, and worker processes? Can one physical machine host multiple UbU Devices/enclaves, and
under what Identity/Compartment conditions?

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "ubu-q0148-gpt5-codex-20260916",
    "provider_id": "openai",
    "model_name": "gpt-5-codex",
    "question_id": "UBU-Q0148",
    "base_commit": "bc52aaa5d6a3bc77abb095616329ebb349c3acad",
    "summary": "Resolve UBU-Q0148 by defining UbU Device identity as a registered execution enclave, not a physical machine, install, browser session, or process. Clarify when one physical machine may host multiple Devices and when app/browser/worker contexts remain projection or child execution contexts instead.",
    "rationale": "The existing design already states that a Device is an execution enclave and that one physical machine may host multiple Devices. This proposal makes the boundary operational: separate Device identity requires intentional registration, appropriate isolation, authorized Identity association, exactly one Zone membership, a capability profile, trust state, and policy-authorized Compartment access. It also prevents over-counting ordinary app installs, browser sessions, and child worker processes as Devices unless they hold independent policy-scoped execution authority.",
    "changed_files": [
      "DESIGN.md",
      "DECISIONS.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -3115,7 +3115,7 @@\n \n ### 23.1 Device\n \n-A **Device** is an execution enclave, not necessarily physical hardware.\n+A **Device** is an execution enclave, not necessarily physical hardware. A physical machine, app install, browser session, and worker process are substrates or runtime contexts; they become UbU Devices only when admitted as separately identifiable execution enclaves with their own `device_id`, trust state, capability profile, Zone membership, and effective Compartment access.\n \n Examples:\n \n@@ -3123,8 +3123,10 @@\n - container\n - VM\n - secure enclave\n+- browser profile or session\n+- worker process or worker runtime\n \n-One physical machine may host multiple Devices.\n+One physical machine may host multiple Devices when each enclave is intentionally registered, isolated enough for its trust and capability claims, associated with an authorized Identity, assigned to exactly one Zone, and granted only the Compartments it is allowed to know about. App installs and browser sessions that merely render or project another Device's state are projection surfaces or execution contexts, not Devices. Workers are Devices only when they hold independent execution authority under policy; otherwise they are child processes of the controlling Device.\n \n ### 23.2 Zone\n \ndiff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -3830,3 +3830,15 @@\n A `worker_result` contains `request_id`, worker identity, the policy versions used, input digests or structural references, operation status, diagnostics safe at the request's visibility level, derived artifacts, projection previews, `worker_result` statements, candidate mutations, and transient-payload deletion confirmation. Candidate mutations are proposals only: they become admitted state solely through the normal sync-statement admission path, including conflict detection, policy checks, provenance, and user-review requirements.\n \n This is intentionally narrower than a general distributed worker marketplace protocol. It is sufficient to let user-owned Devices contribute compute while preserving the \u00a720 boundary that workers can prepare and propose but cannot expand authority or bypass admission.\n+\n+---\n+\n+## UBU-D0251: Device identity is per execution enclave, not per physical machine\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a723.1. Resolves `UBU-Q0148`.\n+\n+A UbU Device is a registered execution enclave. A physical machine, app install, browser session, or worker process is not automatically a Device; it is a substrate or runtime context until UbU admits it as a separately identifiable enclave with its own `device_id`, trust state, capability profile, Zone membership, and effective Compartment access.\n+\n+One physical machine may host multiple UbU Devices when each enclave is intentionally registered, has isolation appropriate to its claimed trust and capabilities, is associated with an authorized Identity, belongs to exactly one Zone, and receives only policy-authorized Compartment knowledge and payload access. Separate OS profiles, containers, VMs, secure enclaves, browser profiles, removable-import environments, and worker runtimes may therefore be separate Devices.\n+\n+An app install or browser session that only presents another Device's state is a projection surface or execution context, not a Device. A worker process is a Device only when it holds independent execution authority and participates under Device policy; otherwise it is a child process of the controlling Device. Worker candidate mutations still require normal admission and cannot expand Identity, Zone, or Compartment authority.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -2637,7 +2637,7 @@\n \n ## UBU-Q0148: Distinguishing physical devices, UbU Devices, installs, and workers\n \n-Status: Open Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0131, Phase 2 Device registry Resolved by: None Last scored: 2026-09-16 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 1b Decision type: Data model Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: UBU-Q0131, Phase 2 Device registry Resolved by: UBU-D0251 Last scored: 2026-09-16 Scored from commit: None\n \n Formerly UBU-QSYNC-010 (retired from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md section 28). Defining context: DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a74, \u00a75.\n \n@@ -2649,7 +2649,7 @@\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0251`: UbU Device identity is per registered execution enclave, not per physical machine, install, browser session, or process. Those substrates become separate UbU Devices only when intentionally admitted with their own `device_id`, trust state, capability profile, Zone membership, authorized Identity association, and effective Compartment access. One physical machine may host multiple Devices when isolation, Identity authority, and Compartment policy support treating each enclave independently. Projection-only app installs or browser sessions are not Devices, and workers are Devices only when they hold independent policy-scoped execution authority.\n \n ---\n \n",
    "commit_message": "Resolve Device enclave identity boundary",
    "validation_notes": [
      "Verified patch applies with `git apply --check` against commit bc52aaa5d6a3bc77abb095616329ebb349c3acad.",
      "Patch modifies only DESIGN.md, DECISIONS.md, and OPEN_QUESTIONS.md.",
      "OPEN_QUESTIONS.md preserves the single-line metadata format and anchors the resolution under UBU-Q0148."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0148"
    ],
    "decisions_added": [
      "UBU-D0251"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "ubu-q0148-gpt5-codex-20260916",
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
