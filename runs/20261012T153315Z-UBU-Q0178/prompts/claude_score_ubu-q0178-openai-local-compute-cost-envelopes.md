# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0178`  
Question title: `Economically accessible local compute paths`  
Base commit: `bb67912243b1b9f5e1a0f2414488681d2bdb1dee`

```markdown
## UBU-Q0178: Economically accessible local compute paths

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-12 Scored from commit: None

### Question

Which lower-cost local compute paths prevent economically precarious users from being excluded from the multimodal experience?

### Current direction

Delta §17.16. Consider smaller local models, bounded snapshots, CPU fallback, reduced latency classes and operator-controlled trusted hardware without a hidden hosted-model dependency. Measure the useful tradeoffs before claiming reach or costs. Compute accessibility is part of the mission, not a promised device minimum.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0178-openai-local-compute-cost-envelopes",
    "provider_id": "openai",
    "model_name": "GPT-6 (Codex)",
    "question_id": "UBU-Q0178",
    "base_commit": "bb67912243b1b9f5e1a0f2414488681d2bdb1dee",
    "summary": "Define bounded CPU inference and existing personal-worker candidates with measured cost qualification; keep UBU-Q0178 open pending evidence-backed selection.",
    "rationale": "UBU-D0331 defines secure hardware qualification but leaves economic comparison unspecified. Concrete local routes, whole-route cash and effort accounting, and useful multimodal acceptance make lower-cost selection reviewable without assuming that smaller models or manual fallback establish affordability.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\nindex 1ff690b..a053fd9 100644\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5242,3 +5242,15 @@ Adopt a revision-pinned qualification report for each physical hardware/software\n Require DESIGN.md \u00a71's trial counts, sustained sessions, security evidence and fallback demonstrations before operator approval of a scoped support matrix. Missing required evidence or a failed mandatory gate leaves that slice unavailable. Passing grants no Device trust or Compartment authority. Record a tested sufficient profile with limitations, rather than an absolute hardware floor. `UBU-Q0160` retains mobile planning/API selection, `UBU-Q0168` numerical interaction profiles, `UBU-Q0183` release allocation and `UBU-Q0178` lower-cost reach.\n \n These are required measurement and implementation checks, not passing results or a selected hardware configuration. `UBU-Q0177` remains Open until the operator reviews actual evidence and selects at least one complete local camera-and-voice configuration for the declared MVP slices.\n+\n+---\n+\n+## UBU-D0332: Lower-cost multimodal access requires measured local paths and cost envelopes\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a71. Narrows `UBU-Q0178`; measured lower-cost selection remains Open.\n+\n+Compare bounded camera-and-voice inference on an existing supported CPU-capable Device with capture/review plus an existing operator-owned worker. Use smaller/quantized or task-specific local models, bounded inputs and only qualified latency classes. Require useful text/form continuity and no hosted inference, paid API credential or recurring model-service subscription. Preserve current Device, Compartment, source-retention, review and planning gates on both routes.\n+\n+Extend `UBU-D0331`'s real-device qualification with the same useful-workflow fixtures, at least one CPU-only and one worker candidate, and predeclared comparison baselines, workloads and cash/effort/energy/storage budgets. Report whole-route incremental and replacement costs, dated regional prices, measured energy, estimated running costs and operator effort, including unavailable candidates and failed trials. A manual-only fallback or smaller model cannot establish affordable multimodal reach.\n+\n+Require operator-reviewed evidence and a scoped cost/support matrix before selecting a lower-cost path. `UBU-Q0178` stays Open pending that selection; `UBU-Q0177`, `UBU-Q0168`, `UBU-Q0160` and `UBU-Q0183` retain their existing hardware, numeric-profile, mobile-planning and release-allocation scope. No measured configuration or affordability claim is established here.\ndiff --git a/DESIGN.md b/DESIGN.md\nindex 2780add..17b6a19 100644\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -88,6 +88,21 @@ Endpoint qualification must demonstrate a maintained security-update posture, in\n \n Publish a policy-safe support matrix linking the exact profile, enabled workflows/classes, measured usable-memory/free-storage envelope, quality and timing results, sustained operating duration, security/retention outcomes and demonstrated fallback. Test offline/model loss, low battery, thermal/resource pressure, stop/silence, delayed results and manual completion without hidden cloud routing or loss of permitted saved state. A worker on another Device needs its own qualified endpoint/profile and existing route/Compartment checks; a same-Device worker remains an execution context unless separately admitted under \u00a723.1. Ownership grants no disclosure authority. Only profiles passing all declared limits and mandatory security/retention gates may be operator-approved for those slices; material profile changes require affected rechecks. Label the selected configuration a tested sufficient profile among the tested candidates, with failures and limitations visible; infer no universal phone minimum or local-versus-cloud quality ranking. `UBU-Q0177` stays Open until a reviewed evidence-backed profile is selected; a protocol alone cannot close it. Lower-cost reach remains `UBU-Q0178`.\n \n+**Economically accessible local paths (`UBU-D0332`).** Compare two candidate routes for a bounded camera-and-voice slice under `UBU-D0331`:\n+\n+- **Existing single Device:** bounded still-image capture and push-to-talk audio, with smaller/quantized or task-specific local models capable of CPU inference, finite context and serialized inference jobs. Evaluate supported older hardware without assuming that age, nominal RAM or model size establishes eligibility.\n+- **Existing personal worker:** capture and review on one Device, with bounded inference on an existing operator-owned laptop/desktop worker. Qualify both endpoints and the complete authenticated encrypted transfer route. Current Device trust, Compartment/source permissions and original consent, freshness, retention and job limits govern every copy; ownership or physical proximity grants no authority. Worker loss leaves a qualified same-Device route or useful text/forms.\n+\n+Each candidate uses legally locally runnable software/weights with disclosed licenses, installation downloads and fees, and requires no hosted inference, paid API credential or recurring model-service subscription. Dedicated accelerators and continuous streaming are optional for this evaluated slice; `UBU-Q0183` retains overall release scope and sequencing. Demonstrate same-Device operation with networking disabled and worker operation with Internet access blocked. Ordinary manual entry, exact correction, candidate review, admitted-state inspection and action recording remain available with models absent under \u00a721.1.3.\n+\n+Before dispatch, show the selected route, enabled modalities, measured latency class, finite wait/context/media limits and fallback. Reduce resolution, frame count or context only within a qualified profile, retaining transform provenance and explicit missing coverage. Slower processing cannot extend raw-media deadlines or freshness, silently omit necessary detail, or weaken uncertainty, corroboration, review, admission or Plan certification. Unsupported work returns an explicit limitation and an eligible retry or manual route. Switching models, endpoints or sources requires fresh bindings and applicable consent; cost/resource pressure grants no routing authority.\n+\n+Extend `UBU-D0331`'s frozen useful-outcome rubric and real-device trials with a paired comparison of these routes on the same permitted \u00a721.1.3 fixtures and declared workload. Include at least one CPU-only inference candidate and one existing-worker candidate; unavailable or ineligible candidates remain visible. Require a visible camera detail plus speech to produce a grounded reviewable candidate, exact correction/admission and a useful next action or explicit planning limitation. Report successful, corrected, unresolved, rejected and failed trials separately, plus total workflow time and operator setup, maintenance and correction effort. A text-only fallback cannot establish multimodal reach.\n+\n+Declare the comparison baseline, usage period/workload and evaluated cash, effort, energy and storage budgets before trials. Report incremental outlay for already-owned hardware separately from acquisition/refurbishment and replacement assumptions. Include necessary software/model fees, download/connectivity, measured workload energy, storage, secure upkeep and accessories for the whole route, including a worker. Price observations state date, region and currency; running-cost estimates state usage period, workload, rates and uncertainty. Distinguish measured outcomes, quoted prices and estimates; unknown required costs prevent a claim that the budget is met. Use \"lower-cost\" only for a specified comparator and matched useful workload, with accuracy, latency and effort tradeoffs beside cash savings. User-supplied budget scenarios establish an evaluated envelope, without inferring individual affordability or population reach.\n+\n+Publish costs, effort, qualified workflows/classes and failures alongside `UBU-D0331`'s support matrix. Select a path only after the operator reviews evidence that it passes the declared usefulness, resource, cost and mandatory security/retention limits. If no path qualifies, retain useful manual operation and the explicit multimodal gap. `UBU-Q0178` remains Open until an evidence-backed lower-cost path is selected; this protocol establishes no price, device minimum or supported configuration. `UBU-Q0177` and `UBU-Q0168` retain hardware and numeric-profile acceptance; CPU inference here selects no mobile planner or compute API under `UBU-Q0160`.\n+\n ---\n \n ## 2. Core Principles\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\nindex ac3d3c7..4feccee 100644\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3374,27 +3374,29 @@ Open. `UBU-D0331` answers the qualification protocol only. Close after actual re\n ## UBU-Q0178: Economically accessible local compute paths\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-12 Scored from commit: None\n \n ### Question\n \n Which lower-cost local compute paths prevent economically precarious users from being excluded from the multimodal experience?\n \n ### Current direction\n \n Delta \u00a717.16. Consider smaller local models, bounded snapshots, CPU fallback, reduced latency classes and operator-controlled trusted hardware without a hidden hosted-model dependency. Measure the useful tradeoffs before claiming reach or costs. Compute accessibility is part of the mission, not a promised device minimum.\n \n+`UBU-D0332` specifies a comparison of bounded CPU-capable single-Device inference and an existing personal worker, with whole-route cost/effort budgets and the qualification gates in DESIGN.md \u00a71. Actual useful lower-cost profiles and cost envelopes remain unselected.\n+\n ### Resolution\n \n-Open.\n+Open. `UBU-D0332` answers the candidate-path and cost-evidence contract only. Close after operator-reviewed comparative real-device results select at least one qualified lower-cost camera-and-voice path within declared cash, effort, energy and storage budgets, with useful end-to-end outcomes and mandatory security/retention gates met. A protocol, smaller model or text-only fallback cannot establish economically accessible multimodal reach.\n \n ---\n \n ## UBU-Q0179: Audience routing for the social-impact thesis\n \n Status: Open Priority: Post-MVP Phase: Post-MVP Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: TBD Depends on: None Blocks: None Resolved by: None Last scored: Never Scored from commit: None\n \n ### Question\n \n Which additional parts of the social-impact thesis belong in public overview, funder, nonprofit/government outreach and canonical design material?\n \n ### Current direction\n",
    "commit_message": "docs: define measured lower-cost local multimodal paths",
    "validation_notes": [
      "Generated with git diff --no-index from unmodified working-tree copies and proposed contents; repository files remain unchanged.",
      "git apply --check --whitespace=error - passed against the current working tree (exit 0).",
      "git diff --no-index --check reported no whitespace errors for all three document pairs.",
      "Verified UBU-D0332 was unused, Q0178's single-line metadata is preserved, and its hunk includes its heading and its own Resolution section.",
      "No hardware, latency or cost measurements were performed. Q0178 remains Open pending operator-reviewed comparative evidence."
    ],
    "new_questions_added": [],
    "questions_resolved": [],
    "decisions_added": [
      "UBU-D0332"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0178-openai-local-compute-cost-envelopes",
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
