# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `claude`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0168`  
Question title: `Snapshot, conversational, streamed and retrospective latency classes`  
Base commit: `a6506e9a699d73b3e8282f74e879c461b90c5f14`

```markdown
## UBU-Q0168: Snapshot, conversational, streamed and retrospective latency classes

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-14 Scored from commit: None

### Question

Which latency classes and degradation behavior are required for snapshots, conversational interaction, streamed supervision and retrospective analysis?

### Current direction

Delta §17.6. `UBU-D0325` defines the four latency classes and deadline, freshness, degradation and interruption semantics in DESIGN.md §21.1.4. Select actual user-visible deadlines, source/context-age limits, stream gap/backlog caps and battery/thermal conditions through revision-pinned local-device evidence. Capture latency remains distinct from planner streaming, admission and proof of execution. No measured performance numbers or supported-hardware claims are established. `UBU-D0335` adds deadline-boundary, stream-invalidation and interruption-race fixtures with separately bounded fallback, output quiescence and resource release; simulated timings do not select numerical profiles.

### Resolution

Open. `UBU-D0325` answers the class/degradation architecture; numerical profiles and measured acceptance remain unresolved. Close only after evidence for each enabled Device/backend/mode pins implementation and policy revisions, selects finite deadlines and freshness/resource ceilings, and demonstrates the specified ordinary, sustained-load, offline, interruption and stale-result behavior. `UBU-Q0177` retains secure hardware selection, `UBU-Q0128` retains quantitative restricted-attention profiles and `UBU-Q0183` retains delivery sequence. `UBU-D0335` makes controller boundary enforcement an explicit qualification gate; its fixtures still need passing implementation and real-device evidence. A parameterized contract alone is insufficient to close this question.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0168-openai-latency-acceptance-accounting",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0168",
    "base_commit": "a6506e9a699d73b3e8282f74e879c461b90c5f14",
    "summary": "Require frozen latency acceptance trials, complete request outcomes and stream coverage accounting; retain UBU-Q0168 as Open.",
    "rationale": "Makes numerical profile selection auditable by separating calibration from acceptance, counting refusals and missing outcomes, and requiring useful-result and stream-coverage thresholds for each operating condition. Bounded fallback and favorable completion-only percentiles cannot substitute for demonstrated ordinary usefulness.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5642,3 +5642,15 @@ The tested use of `field_provenance.preconditions` is a carrier control, not an\n `UBU-D0334`'s inline payload loss remains unrepaired. `UBU-Q0166` stays Open for the full pinned mapping and existing fidelity gates; neither a passing adapter nor a governing schema gap is established. No enum, FactProvenance field, schema, pin, migration or producer authority changes here; `UniverseTarget` remains name-only.\n \n ---\n+\n+## UBU-D0361: Latency profiles require frozen acceptance trials and complete outcome accounting\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a721.1.4. Narrows `UBU-Q0168`; numerical profiles and measured acceptance remain Open.\n+\n+Separate revision-pinned calibration evidence used to propose ceilings from qualification trials of the frozen Device/backend/mode profile. Before acceptance runs, pin the workload, useful-outcome rubric, trial counts and finite limits, including positive minimum timely-useful-outcome and stream-coverage proportions for each applicable ordinary operating condition. Changing limits, workloads or rubrics after observing acceptance results requires a new profile revision and affected qualification runs.\n+\n+Account for every offered workload request, including refusals, timeouts, failures and retries, without counting fallback speed as useful completion. Distinguish successful interpretation, expected consequential clarification/unresolved output and degradation. Report conditional latency distributions with their denominators and missing outcomes; missing results cannot be assigned a successful latency. Stream evidence includes independent sessions, permitted windows and the fraction of declared active time covered by fresh usable guidance, including startup and coverage gaps.\n+\n+DESIGN.md \u00a721.1.4 supplies the accounting and pass/fail requirements alongside `UBU-D0331`/`UBU-D0336`'s endpoint/runtime trials and `UBU-D0335`'s enforcement fixtures. Correct bounded fallback alone cannot qualify an ordinary workload that misses its declared usefulness limit. This selects no numerical ceiling, passing implementation or supported hardware; `UBU-Q0168` stays Open pending operator-reviewed evidence.\n+\n+---\ndiff --git a/DESIGN.md b/DESIGN.md\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -3955,6 +3955,14 @@ Each fixture report binds profile/implementation/policy revisions, minimized job\n \n Exercise cold and warm operation, competing work and sustained streams; offline/backend loss, low battery and thermal/resource exhaustion; interruption during acquisition/inference/output; queue overload and deadline expiry; source/context changes, corrections, revocation, restart/handoff and delayed/duplicate results. Demonstrate bounded waiting/backlog, working manual fallback, truthful saved/coverage/freshness status, no stale admission/dispatch and no hidden rerouting. Publish the selected finite ceilings and demonstrated fallback for each enabled class; an unmeasured class remains unavailable. Hardware selection remains `UBU-Q0177`, restricted-attention numeric profiles remain `UBU-Q0128`, and delivery sequence remains `UBU-Q0183`. `UBU-Q0168` stays Open for these numerical profiles and measured acceptance; this contract asserts no implementation, supported hardware or achieved latency.\n \n+**Profile selection and outcome accounting (`UBU-D0361`).** Keep revision-pinned calibration runs used to propose numeric ceilings separate from acceptance evidence. Before acceptance runs, freeze the candidate profile, scenario/source-size/load envelope, useful-outcome rubric, trial counts and pass/fail rules. Include a positive minimum proportion of timely useful outcomes for each ordinary cold, warm and sustained-load condition and, for streams, a positive minimum fraction of declared active time covered by fresh usable guidance, alongside the finite latency, freshness, resource and degradation limits already required. Retain `UBU-D0331`'s minimum trials/durations and `UBU-D0336`'s deployed isolation costs. Acceptance observations cannot justify widening ceilings, lowering usefulness requirements or removing failed cases; changes require a new profile revision and affected qualification runs.\n+\n+For each predeclared cohort, reconcile all offered workload requests with controller-accepted requests, refusals before dispatch and terminal outcomes. Count an original request once for the timely-useful proportion; retries consume its original elapsed/work budgets and add no extra successes or denominator entries. Keep successful interpretations, corrected results, consequential clarification/explicit unresolved results, expiry, interruption, errors and manual/degraded fallback distinguishable. Clarification or unresolved output counts as useful only when it meets the frozen scenario rubric, and never becomes a successful interpretation. Receipt, progress or a fast unavailable explanation cannot count as useful completion. Fault/interruption cohorts test their predeclared degraded disposition separately from ordinary usefulness; cases cannot move cohorts after their outcomes are known.\n+\n+Report the timely-useful numerator over all offered requests in each ordinary cohort, plus refusal, miss, error and degraded counts. Latency percentiles name their observed-outcome population and denominator; absent useful output has no successful completion time and stays visible as a missing outcome. Report time to fallback separately, rather than substituting it into useful-result timings. For streams, report independent session counts, all permitted observation windows and dropped/missing/stale updates, startup waiting, longest gap, backlog high-water marks and the proportion of declared active-session time with fresh usable guidance. Include startup and delayed/paused coverage in that time denominator; planned inactive periods are declared before trials. Many correlated windows in one session cannot establish independent-session coverage.\n+\n+Publish a per-class, per-condition acceptance table linking the frozen profile and trial plan to counts, distributions, coverage, each limit and pass/fail/unknown. An unknown required metric or missed mandatory limit withholds the affected profile; pooled results cannot hide a failed condition. Passing cancellation/fallback fixtures cannot compensate for missed ordinary usefulness, and rapid ordinary results cannot compensate for failed enforcement, privacy or retention gates. Publish calibration results separately and obtain operator review before enablement. This adds an evidence-selection gate, not measured values or a new wire schema; `UBU-Q0168` remains Open.\n+\n #### 21.1.5 Local AV segmentation and scoped analysis\n \n **Architecture contract (`UBU-D0326`).** Adopt a controller-owned local media broker with the logical adapter profile `local-media/1`. Session consent enables only named sources on eligible execution-enclave Devices on the operator's own hardware; sharing hardware grants no access between enclaves. Capture controls remain visible under \u00a712.2 and \u00a721.1.3. Models and agents can request analysis of supplied media spans, but cannot start/resume capture, enumerate sources, renew consent or enlarge a grant. This profile specifies an interface boundary, not an implemented MCP service, frozen wire-schema change or new planner entity.\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3238,25 +3238,25 @@ Resolved by `UBU-D0324`: DESIGN.md \u00a721.1.1 defines clarification, bounded ordin\n ## UBU-Q0168: Snapshot, conversational, streamed and retrospective latency classes\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-14 Scored from commit: None\n \n ### Question\n \n Which latency classes and degradation behavior are required for snapshots, conversational interaction, streamed supervision and retrospective analysis?\n \n ### Current direction\n \n-Delta \u00a717.6. `UBU-D0325` defines the four latency classes and deadline, freshness, degradation and interruption semantics in DESIGN.md \u00a721.1.4. Select actual user-visible deadlines, source/context-age limits, stream gap/backlog caps and battery/thermal conditions through revision-pinned local-device evidence. Capture latency remains distinct from planner streaming, admission and proof of execution. No measured performance numbers or supported-hardware claims are established. `UBU-D0335` adds deadline-boundary, stream-invalidation and interruption-race fixtures with separately bounded fallback, output quiescence and resource release; simulated timings do not select numerical profiles.\n+Delta \u00a717.6. `UBU-D0325` defines the four latency classes and deadline, freshness, degradation and interruption semantics in DESIGN.md \u00a721.1.4. Select actual user-visible deadlines, source/context-age limits, stream gap/backlog caps and battery/thermal conditions through revision-pinned local-device evidence. Capture latency remains distinct from planner streaming, admission and proof of execution. No measured performance numbers or supported-hardware claims are established. `UBU-D0335` adds deadline-boundary, stream-invalidation and interruption-race fixtures with separately bounded fallback, output quiescence and resource release; simulated timings do not select numerical profiles. `UBU-D0361` separates calibration from frozen acceptance trials and requires complete request/session outcome accounting with declared timely-useful and stream-coverage criteria.\n \n ### Resolution\n \n-Open. `UBU-D0325` answers the class/degradation architecture; numerical profiles and measured acceptance remain unresolved. Close only after evidence for each enabled Device/backend/mode pins implementation and policy revisions, selects finite deadlines and freshness/resource ceilings, and demonstrates the specified ordinary, sustained-load, offline, interruption and stale-result behavior. `UBU-Q0177` retains secure hardware selection, `UBU-Q0128` retains quantitative restricted-attention profiles and `UBU-Q0183` retains delivery sequence. `UBU-D0335` makes controller boundary enforcement an explicit qualification gate; its fixtures still need passing implementation and real-device evidence. A parameterized contract alone is insufficient to close this question.\n+Open. `UBU-D0325` answers the class/degradation architecture; numerical profiles and measured acceptance remain unresolved. Close only after evidence for each enabled Device/backend/mode pins implementation and policy revisions, selects finite deadlines and freshness/resource ceilings, and demonstrates the specified ordinary, sustained-load, offline, interruption and stale-result behavior. `UBU-Q0177` retains secure hardware selection, `UBU-Q0128` retains quantitative restricted-attention profiles and `UBU-Q0183` retains delivery sequence. `UBU-D0335` makes controller boundary enforcement an explicit qualification gate; its fixtures still need passing implementation and real-device evidence. `UBU-D0361` additionally requires frozen per-condition acceptance limits and complete outcome/coverage reports; correct fallback alone cannot qualify ordinary usefulness. A parameterized contract alone is insufficient to close this question.\n \n ---\n \n ## UBU-Q0169: Local AV segmentation and bounded agent/tool interfaces\n \n Status: Solved Priority: MVP important Phase: Phase 2 Decision type: Architecture Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: UBU-D0326 Last scored: 2026-10-12 Scored from commit: None\n \n ### Question\n \n How are local audio/video streams segmented, and what scoped interfaces expose frame extraction, event detection and relevant-section isolation?\n",
    "commit_message": "docs: require frozen latency acceptance and complete outcome accounting",
    "validation_notes": [
      "Passed git apply --check --whitespace=error against the current working tree without --cached or --recount.",
      "Generated unified hunks with git diff --no-index against unchanged working-tree contents; normalized file headers and omitted optional index metadata.",
      "Preserved UBU-Q0168's single-line metadata, Status: Open and Resolved by: None; verified UBU-D0361 is unused and anchored the question hunk with its heading and Resolution section.",
      "Documentation-only proposal: no numerical performance, implementation or hardware results asserted; repository files remain unchanged."
    ],
    "new_questions_added": [],
    "questions_resolved": [],
    "decisions_added": [
      "UBU-D0361"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0168-openai-latency-acceptance-accounting",
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
