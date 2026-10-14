# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0185`  
Question title: `Advisory decode controls or operator-owned runtime policy`  
Base commit: `1b55da79c2231ff329f026d81bef54847d45af6c`

```markdown
## UBU-Q0185: Advisory decode controls or operator-owned runtime policy

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-14 Scored from commit: None

### Question

Should the advisory boundary expose decode parameters, or is the model's generation behavior deliberately the operator's to configure in their own runtime?

### Current direction

The advisory Settings expose model, endpoint, timeout and review seed/ceiling, with no decode parameter. P1B-80's live Vocabulary abort prompted an audit that found a separate unbounded SuggestTags grammar (`UBU-D0302`). Correcting that grammar before changing the sampler is evidence on the deliberately-outside side; it does not prove the cause of the already-bounded Vocabulary failure or settle runtime control across models. No repeat penalty, temperature, Setting or other decode control is added.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0185-openai-gpt-6-runtime-policy",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0185",
    "base_commit": "1b55da79c2231ff329f026d81bef54847d45af6c",
    "summary": "Keep advisory sampling configuration in the operator-owned runtime; retain UbU-owned grammar, resource, validation and review contracts.",
    "rationale": "The existing advisory Settings and UBU-D0302 support keeping runtime-specific sampler policy outside the UbU configuration surface. Separating sampling preferences from required grammar and resource limits makes this boundary enforceable across adapters without weakening admission. The proposal preserves existing review seed/ceiling behavior, requires configuration documentation and request/failure fixtures, and makes no causal claim about the Vocabulary abort. Human approval is required to accept this architecture decision.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5351,6 +5351,20 @@ Phone streaming, meeting/multi-party logging, retrospective or cloud-assisted in\n \n An evaluated Stage 1 candidate retaining a dependent Task while omitting any prerequisite required by the request reports `dependency_feasibility = false` and `hard_constraint_feasibility = false`, with a missing-prerequisite rejection reason. This is an infeasible candidate, not an incomplete feasibility result. Dependency feasibility remains boolean.\n \n Dependency feasibility requires presence and timely completion for every request dependency whose dependent is placed. With no such dependencies it is true, while the other hard constraints still apply. This follows the existing plan-validity requirement for dependency presence rather than treating either implementation's behavior as authority. Optional-work omission still defers affected dependents under `UBU-D0289`; an absent placement cannot establish prerequisite completion.\n \n Detect absence before placement or dependency-slack lookups, identify the reason in Stage 1's existing per-candidate rejection/failure output, and preserve evaluation of other candidates. Require shared CPU/GPU goldens covering absence, multiple prerequisites, timing equality and lateness, dependency-free work, permitted omission of an entire optional chain and mixed valid/invalid batches. This resolves the field semantics; implementation conformance and live parity evidence remain outstanding.\n+\n+---\n+\n+## UBU-D0341: Advisory sampling policy belongs to the operator's inference runtime\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a71. Resolves `UBU-Q0185`; preserves `UBU-D0302`'s producer bounds and failure-attribution limits.\n+\n+For the MVP/Phase 2 advisory boundary, the operator configures sampling in the selected runtime. Add no generic decode Settings or UbU-selected temperature, top-p/top-k or repetition penalties, including hidden request overrides or automatic retry tuning. Preserve the existing model, endpoint, timeout and review seed/ceiling behavior; a review seed does not establish reproducibility across models or runtime versions.\n+\n+UbU still owns bounded validator-safe producer grammars, finite request/output and job budgets, cancellation, validation and ordinary candidate review/admission. Required grammar and resource/output-limit request fields remain within this boundary. An incompatible runtime leaves the affected operation unavailable with an explicit limitation and eligible retry/manual route. Abort, timeout and invalid output preserve saved state and cannot admit generated state changes, relax validation or authorize a new route. Retries retain current authority/input bindings and finite call/time budgets.\n+\n+Document adapter requirements and runtime configuration instructions. Record observable sampling configuration or its digest with qualified profiles, mark unknown effective defaults explicitly and repeat affected qualification checks after configuration changes where applicable under `UBU-D0331`/`UBU-D0336`. Require request-construction and failure-path fixtures before claiming conformance, as specified in DESIGN.md \u00a71.\n+\n+This settles runtime-control ownership, not the cause of the already-bounded Vocabulary abort. The separate SuggestTags defect remains evidence for producer bounds, not a sampler diagnosis. No passing adapter fixtures, supported runtime configuration or implementation change is established by this decision.\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -25,12 +25,18 @@ The deeper purpose is to help people build the capabilities, resources, routines\n **Access within ordinary life logistics (`UBU-D0338`).** Present guided camera acquisition and voice/text/form choices as ordinary ways to capture concerns, review proposals and act on Plans. Keep UbU's primary identity as a privacy-first life-logistics system; access features are available without a disability label or special onboarding category. Describe concrete benefits and limitations for tested workflows and Device/assistive-technology combinations. Camera availability alone establishes no accessibility claim.\n \n The first MVP is designed around **dogfooding**: using UbU to coordinate the design, development, release, and maintenance of UbU itself.\n \n Recent LLM and agentic-AI changes reinforce UbU's core boundary: realtime, multimodal, tool-using, memory-bearing models are valuable interaction and extraction backends, but UbU remains the user-sovereign state-transition, planning, logging, privacy, and review layer.\n \n+**Advisory generation policy (`UBU-D0341`).** For the MVP/Phase 2 advisory boundary, the operator configures sampling behavior in the selected inference runtime. UbU adds no generic decode Settings or application-selected temperature, top-p/top-k or repetition penalties, hidden request overrides or automatic retry tuning. Existing model/endpoint selection, timeout and review seed/ceiling behavior remain; the review seed provides no cross-model or cross-runtime reproducibility guarantee.\n+\n+UbU owns bounded validator-safe producer grammars (`UBU-D0302`), finite request/output and job budgets, cancellation, validation and ordinary candidate review/admission. Required grammar and resource/output-limit fields may be sent to the runtime without transferring these contracts to sampler policy. A runtime unable to meet them leaves the affected operation unavailable with an explicit limitation and eligible retry/manual route. Abort, timeout and invalid output preserve saved state and admit no generated state changes. Retries retain current authority/input bindings and finite call/time budgets; failure cannot relax validation or authorize another route.\n+\n+Document each supported adapter's required grammar/limit behavior and runtime sampling configuration instructions. Pin observable sampling configuration or its digest in qualified Device/backend profiles and label unknown effective defaults explicitly. Configuration changes require affected qualification checks under `UBU-D0331`/`UBU-D0336` where applicable. Before claiming conformance, require adapter fixtures that inspect requests for absence of application-selected sampler overrides while preserving existing review seed/ceiling behavior and required grammar/limit fields; exercise abort, timeout and invalid-output paths for preserved saved state, bounded retries and unchanged review/admission requirements. These are required checks, not passing runtime evidence or a diagnosis of the Vocabulary abort.\n+\n For Phase 1, UbU must also be understandable as a first-person user experience. The minimal user-facing loop is: answer a small number of bootstrapping questions, allow UbU to construct an initial context model, receive one recommended next Task, inspect why that Task matters now, act or override, then let UbU learn from the result through Logs, Snapshots, and recalculation.\n \n For Phase 2, a single-live-session sync implementation is sufficient (`UBU-D0312`), with arbitrary N Devices, concurrent offline mutations, partial replicas, and transport-independent integrity-protected statements. Direct exchanges, imported bundles, and user-selected encrypted stores share the same causality, policy, and conflict rules; no Device or transport becomes a permanent canonical authority. Full automatic peer mesh is optional. DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md \u00a726's topology research remains a freeze gate, with at least three Devices demonstrating serialized direct exchange and encrypted store-and-forward, recoverable delivery, deterministic admission/conflict handling, and Compartment-safe partial replication as specified by `UBU-D0312`.\n \n The minimum indirect path for the first Phase 2 sync release is implemented, authenticated encrypted bundle export/import through a user-selected file store, with at least one documented external rclone or rsync copy workflow exercised before freeze (`UBU-D0313`). Manual export/copy/import is sufficient; built-in clients and background delivery are optional. Encryption and recipient authorization protect statements, payloads, and manifests before storage, while ordinary admission and Compartment policy govern every import.\n \ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3523,25 +3523,25 @@ P1B-79 records, but does not resolve, this separate boundary. Rust reports `depe\n Resolved by `UBU-D0340`: an evaluated candidate retaining a dependent while omitting any required prerequisite is infeasible, with both `dependency_feasibility` and `hard_constraint_feasibility` set to `false` and a missing-prerequisite rejection reason. PLANNING_KERNEL_CONTRACT.md \u00a75 requires safe missing-placement handling and exact-parity fixtures. This settles candidate semantics; implementation conformance and live evidence remain outstanding.\n \n ---\n \n ## UBU-Q0185: Advisory decode controls or operator-owned runtime policy\n \n-Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-14 Scored from commit: None\n+Status: Solved Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: UBU-D0341 Last scored: 2026-10-14 Scored from commit: None\n \n ### Question\n \n Should the advisory boundary expose decode parameters, or is the model's generation behavior deliberately the operator's to configure in their own runtime?\n \n ### Current direction\n \n-The advisory Settings expose model, endpoint, timeout and review seed/ceiling, with no decode parameter. P1B-80's live Vocabulary abort prompted an audit that found a separate unbounded SuggestTags grammar (`UBU-D0302`). Correcting that grammar before changing the sampler is evidence on the deliberately-outside side; it does not prove the cause of the already-bounded Vocabulary failure or settle runtime control across models. No repeat penalty, temperature, Setting or other decode control is added.\n+The advisory Settings expose model, endpoint, timeout and review seed/ceiling, with no decode parameter. P1B-80's live Vocabulary abort prompted an audit that found a separate unbounded SuggestTags grammar (`UBU-D0302`). Correcting that grammar before changing the sampler is evidence on the deliberately-outside side; it does not prove the cause of the already-bounded Vocabulary failure or settle runtime control across models. No repeat penalty, temperature, Setting or other decode control is added. `UBU-D0341` subsequently assigns sampling policy to the operator's runtime while retaining UbU's producer, resource and admission contracts.\n \n ### Resolution\n \n-Open.\n+Resolved by `UBU-D0341`: the MVP/Phase 2 advisory boundary leaves sampling configuration to the operator's inference runtime and adds no generic decode Settings, hidden sampler overrides or automatic retry tuning. UbU retains bounded validator-safe grammars, finite resource/output/job limits, existing review seed/ceiling behavior and ordinary validation/review/admission. DESIGN.md \u00a71 requires adapter configuration documentation and request/failure fixtures before claiming conformance. This resolves policy ownership; implementation evidence and the cause of the Vocabulary abort remain outstanding.\n \n ---\n \n ## UBU-Q0186: Device provisioning as the first run of the admission loop\n \n Status: Open Priority: Post-MVP Phase: Post-MVP Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: None Resolved by: None Last scored: Never Scored from commit: None\n",
    "commit_message": "docs: assign advisory sampling policy to operator runtimes",
    "validation_notes": [
      "Generated git diff hunks from current working-tree originals and in-memory proposed contents; normalized destination labels without using HEAD or index contents.",
      "git apply --check --whitespace=error-all passed against the current working tree; no --cached or --recount was used.",
      "UBU-D0341 is unused; only the three allowlisted files are changed by the proposal.",
      "UBU-Q0185 edits are confined to its own block and preserve all single-line metadata fields except Status and Resolved by.",
      "Repository files were not modified; git status --short remained empty. Runtime and adapter fixtures are proposed requirements, not executed tests."
    ],
    "new_questions_added": [],
    "questions_resolved": [
      "UBU-Q0185"
    ],
    "decisions_added": [
      "UBU-D0341"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0185-openai-gpt-6-runtime-policy",
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
