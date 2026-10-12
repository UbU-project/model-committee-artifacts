# Model-Committee Cross-Scoring Request

You are scoring candidate work proposals for the UbU `model-committee` process.

Return exactly one JSON object. Do not return prose outside the JSON object.

This is a v0.2 cross-score. You are not making the final selection; model-committee
will aggregate valid cross-scores locally.

Scoring provider: `codex`
Authoring provider for the candidate proposal(s): `openai`

## Selected question

Question ID: `UBU-Q0163`  
Question title: `Retire the uncaptured recurring-busy capacity workaround`  
Base commit: `934e7fd2f74b06bc847942882ef01fde40938de4`

```markdown
## UBU-Q0163: Retire the uncaptured recurring-busy capacity workaround

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-11 Scored from commit: None

### Question

What verified capture/occupancy behavior allows removal of the temporary manual blocking Static event used for uncaptured recurring busy time during the Phase 1b-to-Phase 2 transition?

### Current direction

Recorded operator-reported limitation: uncaptured busy time is invisible to the planner, so a manual capacity blocker can prevent placement there but can also collide with subsequently captured commitments. Timed expanded recurrence instances already capture as individual Tasks; do not describe all recurrence as unsupported. Series semantics, unsupported inputs and missed/on-demand capture still need review. Remove the workaround only when relevant recurring commitments demonstrably occupy the correct capacity, without duplicates or double counting. No operator event or collision tally is reproduced here.

### Resolution

Open.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "UBU-Q0163-openai-gpt-6-retirement-gates",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0163",
    "base_commit": "934e7fd2f74b06bc847942882ef01fde40938de4",
    "summary": "Define verified capture, occupancy and handoff gates for retiring recurring-busy capacity blockers; keep UBU-Q0163 open pending implementation evidence.",
    "rationale": "Timed expanded recurrence instances already capture as Tasks. A scoped coverage manifest, stable occurrence lineage, certified capacity accounting and reviewed residual-blocker handoff address missing capture and double counting without requiring full series-level import or claiming that retirement has already been verified.",
    "changed_files": [
      "DECISIONS.md",
      "DESIGN.md",
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/DECISIONS.md b/DECISIONS.md\nindex c75469c..e5b91c0 100644\n--- a/DECISIONS.md\n+++ b/DECISIONS.md\n@@ -5051,3 +5051,20 @@ Before enabling this workflow, demonstrate:\n - Long or oversized histories, exhausted storage and queryable archival respect finite limits without silent loss; restricted replicas and policy-directed purge cannot disclose or reconstruct denied source content through prose, coverage or cached context.\n \n These are required implementation checks, not evidence of completed implementation or a claim that arbitrary model paraphrase is provably lossless.\n+\n+---\n+\n+## UBU-D0318: Recurring-busy blockers retire only after scoped capture and occupancy verification\n+\n+**Status:** Accepted \u2192 DESIGN.md \u00a71. Specifies the retirement process for `UBU-Q0163`; implementation evidence and the operator's scoped retirement remain open.\n+\n+Adopt the coverage manifest, stable occurrence lineage, capacity checks and reviewed blocker handoff in DESIGN.md \u00a71. Existing timed expanded-instance capture is the baseline, not evidence that all recurrence is unsupported or that every relevant commitment is covered. Series-level import is optional when expanded instances demonstrably cover the reviewed scope. Keep residual blockers for unsupported or unverified intervals.\n+\n+Before any scoped retirement, attach revision-pinned behavioral evidence demonstrating:\n+\n+- Expanded-instance capture and repeated/on-demand capture preserve one Task lineage per occurrence across retry and restart. Different occurrences remain distinct; single-occurrence and series edits, cancellations and explicit exceptions used by the reviewed scope remove stale future occupancy without rewriting logged history.\n+- Timezone/DST resolution and horizon-crossing intervals match expected UTC occupancy. All-day, floating-time, unexpanded series and other unsupported inputs produce explicit outcomes with retained protection; interrupted or incomplete capture cannot certify coverage. Horizon advancement repeats the coverage check.\n+- The CPU reference path and enabled planner backends agree on busy/free capacity and reject flexible placement inside fixed commitments. Replacement Tasks, their projections and workaround blockers are not counted twice; genuine commitment overlaps remain conflicts.\n+- A mixed covered/uncovered blocker retains exactly the required residual protection. Approved handoff, interrupted handoff, stale-version refusal, restart and replay preserve reservations, invalidate old Plans and prevent reuse/projection until recalculation is certified.\n+\n+These are required implementation and retirement checks, not passing results. Closing `UBU-Q0163` requires the source/path inventory, these results and the recorded operator-approved handoff for the identified workaround.\ndiff --git a/DESIGN.md b/DESIGN.md\nindex 5698f8b..3ab6b20 100644\n--- a/DESIGN.md\n+++ b/DESIGN.md\n@@ -70,6 +70,14 @@ Declare finite storage, prompt, result and job time/call limits. Retain source h\n \n Source records, synthesized prose, coverage maps and subsequent context inherit all source disclosure, routing and retention restrictions; local processing does not waive them. Reject synthesis into a Task whose destination policy cannot protect its sources. Recheck policy on every read, sync and export. Authorized redaction/purge also removes disallowed content from dependent prose, revisions and cached context; prevent reconstruction from retained derivatives. Keep only permitted minimized gap metadata, disclose incompleteness without revealing denied existence, and never claim lossless retention beyond that scope.\n \n+**Recurring-busy workaround retirement (`UBU-D0318`).** Timed expanded recurrence instances already capture as individual Tasks. Retire a temporary manual capacity blocker only for an operator-reviewed source/series and planning scope with verified replacement occupancy. Full series-level import is not a prerequisite when expanded instances cover that scope; unsupported inputs, missed/on-demand capture and incomplete coverage retain explicit residual blocking.\n+\n+Record a policy-permitted coverage manifest binding the implementation revision, source/series references, capture window/cursor and completeness evidence, source and admitted input versions, planning horizon, expected busy UTC intervals, occurrence-to-Task lineage, and blocker references. Every relevant busy occurrence must map to one active capacity-bearing representation; record how any uncovered part of the blocker remains protected. Titles or coincident times do not establish occurrence identity. Repeated capture, restart and replay preserve lineage without duplicate Tasks. Moves, cancellations and series changes supersede prior active reservations through ordinary admission while preserving logged history; absence from an incomplete response cannot establish cancellation.\n+\n+Fixed busy commitments use capacity-occupying Static Tasks at validated UTC start/end times and bound chunks under `UBU-D0279`. Capture counts, Calendar display and Task existence alone do not prove occupancy. Verify the CPU reference path and each enabled planner backend against expected occupied/free intervals: flexible placements cannot enter busy time, and a source occurrence, its projection and its temporary blocker cannot consume capacity twice. Distinct commitments that genuinely overlap retain their conflict rather than being deduplicated by time.\n+\n+Before removal, show the exact blocker replacements and residual intervals and obtain explicit operator approval bound to the manifest and current versions. Apply them through ordinary admission as one batch, or an equivalent serialized handoff that prevents Plan certification/projection of intermediate missing or doubled occupancy. Invalidate affected Plans and certify recalculation before reuse or projection. Failure or stale inputs preserve protection and require fresh review. Retire only the identified workaround, preserving source commitments and history. Advancing the horizon or receiving material source changes requires renewed coverage checks before using affected capacity; missing, stale or policy-withheld data never proves free time. Evidence and diagnostics obey current Compartment policy; this process neither grants capture/export authority nor bypasses existing sync/projection checks.\n+\n ---\n \n ## 2. Core Principles\ndiff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\nindex e7ee071..45a808a 100644\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3074,25 +3074,27 @@ Open. Append-only correction is already decided. The reported offending path, co\n ## UBU-Q0163: Retire the uncaptured recurring-busy capacity workaround\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-11 Scored from commit: None\n \n ### Question\n \n What verified capture/occupancy behavior allows removal of the temporary manual blocking Static event used for uncaptured recurring busy time during the Phase 1b-to-Phase 2 transition?\n \n ### Current direction\n \n+`UBU-D0318` specifies the scoped capture, capacity and blocker-handoff gates in DESIGN.md \u00a71. Remaining work is the actual source/series inventory, implementation-path references, revision-pinned passing checks and operator approval for the identified blocker replacements. Unsupported or incomplete coverage retains residual blocking.\n+\n Recorded operator-reported limitation: uncaptured busy time is invisible to the planner, so a manual capacity blocker can prevent placement there but can also collide with subsequently captured commitments. Timed expanded recurrence instances already capture as individual Tasks; do not describe all recurrence as unsupported. Series semantics, unsupported inputs and missed/on-demand capture still need review. Remove the workaround only when relevant recurring commitments demonstrably occupy the correct capacity, without duplicates or double counting. No operator event or collision tally is reproduced here.\n \n ### Resolution\n \n-Open.\n+Open. `UBU-D0318` specifies the retirement process; verified implementation evidence and the scoped operator-approved blocker handoff remain outstanding. This specification removes no blocker and makes no claim that recurring-busy coverage is complete.\n \n ---\n \n ## UBU-Q0164: Deliver the primary camera-and-voice surface with usable alternatives\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-11 Scored from commit: None\n \n ### Question\n \n How does the MVP realize its already-decided primary camera-and-voice surface while keeping text/forms useful for preference, privacy, precision and context?\n",
    "commit_message": "docs: define recurring-busy blocker retirement gates",
    "validation_notes": [
      "Generated the raw git diff from unmodified current working-tree copies and proposed contents.",
      "git apply --check --whitespace=error passed against the working tree; no repository files were modified.",
      "Verified the OPEN_QUESTIONS.md hunk includes UBU-Q0163's heading and its own Resolution section; its single-line metadata remains unchanged.",
      "Allocated UBU-D0318 once; modified only the three listed allowlisted files.",
      "Capture, occupancy and handoff checks are proposed acceptance gates, not completed implementation evidence. UBU-Q0163 remains Open."
    ],
    "new_questions_added": [],
    "questions_resolved": [],
    "decisions_added": [
      "UBU-D0318"
    ],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "UBU-Q0163-openai-gpt-6-retirement-gates",
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
