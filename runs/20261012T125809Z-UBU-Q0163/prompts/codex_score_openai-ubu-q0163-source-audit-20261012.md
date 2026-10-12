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
Base commit: `547030976ca3bb2bacdcf57d3420adde781fe951`

```markdown
## UBU-Q0163: Retire the uncaptured recurring-busy capacity workaround

Status: Open Priority: MVP important Phase: Phase 2 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-12 Scored from commit: None

### Question

What verified capture/occupancy behavior allows removal of the temporary manual blocking Static event used for uncaptured recurring busy time during the Phase 1b-to-Phase 2 transition?

### Current direction

`UBU-D0318` specifies the scoped capture, capacity and blocker-handoff gates in DESIGN.md §1. Remaining work is the actual source/series inventory, implementation-path references, revision-pinned passing checks and operator approval for the identified blocker replacements. Unsupported or incomplete coverage retains residual blocking.

Recorded operator-reported limitation: uncaptured busy time is invisible to the planner, so a manual capacity blocker can prevent placement there but can also collide with subsequently captured commitments. Timed expanded recurrence instances already capture as individual Tasks; do not describe all recurrence as unsupported. Series semantics, unsupported inputs and missed/on-demand capture still need review. Remove the workaround only when relevant recurring commitments demonstrably occupy the correct capacity, without duplicates or double counting. No operator event or collision tally is reproduced here.

### Resolution

Open. `UBU-D0318` specifies the retirement process; verified implementation evidence and the scoped operator-approved blocker handoff remain outstanding. This specification removes no blocker and makes no claim that recurring-busy coverage is complete.

---

```

## Candidate proposals

```json
[
  {
    "proposal_id": "openai-ubu-q0163-source-audit-20261012",
    "provider_id": "openai",
    "model_name": "gpt-6",
    "question_id": "UBU-Q0163",
    "base_commit": "547030976ca3bb2bacdcf57d3420adde781fe951",
    "summary": "Record revision-pinned capture and occupancy paths, identify the cancellation/retirement gap, and define its verification handoff while keeping Q0163 Open.",
    "rationale": "UBU-D0318 already specifies retirement gates. The inspected implementation supports timed expanded-instance capture but skips cancellation entries and retains missing-source Tasks for review. Recording these paths advances the outstanding implementation inventory without claiming passing behavioral evidence or authorizing blocker removal.",
    "changed_files": [
      "OPEN_QUESTIONS.md"
    ],
    "patch": "diff --git a/OPEN_QUESTIONS.md b/OPEN_QUESTIONS.md\n--- a/OPEN_QUESTIONS.md\n+++ b/OPEN_QUESTIONS.md\n@@ -3087,27 +3087,40 @@\n ## UBU-Q0163: Retire the uncaptured recurring-busy capacity workaround\n \n Status: Open Priority: MVP important Phase: Phase 2 Decision type: Process Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: None Last scored: 2026-10-12 Scored from commit: None\n \n ### Question\n \n What verified capture/occupancy behavior allows removal of the temporary manual blocking Static event used for uncaptured recurring busy time during the Phase 1b-to-Phase 2 transition?\n \n ### Current direction\n \n-`UBU-D0318` specifies the scoped capture, capacity and blocker-handoff gates in DESIGN.md \u00a71. Remaining work is the actual source/series inventory, implementation-path references, revision-pinned passing checks and operator approval for the identified blocker replacements. Unsupported or incomplete coverage retains residual blocking.\n+`UBU-D0318` specifies the scoped capture, capacity and blocker-handoff gates in DESIGN.md \u00a71. The source audit below inventories the expanded-instance implementation paths and identifies a cancellation/retirement gap. Remaining work is the actual operator source/series inventory, confirmation of deployed paths/revisions, revision-pinned passing checks and operator approval for the identified blocker replacements. Unsupported or incomplete coverage retains residual blocking.\n \n Recorded operator-reported limitation: uncaptured busy time is invisible to the planner, so a manual capacity blocker can prevent placement there but can also collide with subsequently captured commitments. Timed expanded recurrence instances already capture as individual Tasks; do not describe all recurrence as unsupported. Series semantics, unsupported inputs and missed/on-demand capture still need review. Remove the workaround only when relevant recurring commitments demonstrably occupy the correct capacity, without duplicates or double counting. No operator event or collision tally is reproduced here.\n \n+### Revision-pinned source audit and implementation handoff\n+\n+A 2026-10-12 read-only source audit of the clean local `ubu-phase2/ubu-orchestrator` checkout at `9fddef16990ac89d07a168c75bf04b77c35b9bc3` identifies these paths. This is static source evidence; the referenced test sources were inspected, not executed, and the operator's actual source/series inventory remains outstanding.\n+\n+- Listing: `src/services/calendar_wire.rs::list_request` requests `singleEvents=true` within the requested time range, and `src/services/calendar_google.rs::GoogleCalendarApi::list_events` follows pagination with a repeated-token guard. `calendar_wire::parse_event_with_diagnostic` requires concrete RFC3339 `dateTime` endpoints and explicitly skips all-day inputs. Listing completion alone does not establish usable coverage of skipped inputs.\n+- Occurrence lineage: `src/services/calendar_capture.rs::{plan_capture,capture}` makes unownable timed event ids, including expanded recurrence-instance ids, Static Tasks at their validated windows, with capacity controlled by transparency. `src/services/calendar_sources.rs::by_source` reconstructs the persisted `provenance.source.source_id` mapping used to reuse Task handles on recapture. These paths record individual events rather than reconstructing a series; restart/replay correctness still needs behavioral evidence.\n+- Occupancy and projection: `src/services/planning_service.rs::build_request_from_store_with_context` includes capacity-bearing Static Tasks as anchored TaskSpecs and separates non-capacity Static Tasks. `src/services/calendar_projection.rs::{external_id_for,desired_events}` excludes captured origins whose ids cannot round-trip. These source paths identify the intended planner/export boundaries; they do not establish CPU/backend parity or a safe blocker handoff.\n+- Cancellation/absence gap: `calendar_wire::parse_event_with_diagnostic` skips entries with `status: cancelled`; `calendar_capture::capture` processes observed candidates without retiring absent captured Tasks. `src/services/calendar_reconciliation_service.rs::reconcile` emits `capture_source_removed` for an active captured Task absent within its observed horizon and retains the Task for operator review. That diagnostic is not an admitted cancellation or reservation retirement. The audited paths therefore do not demonstrate removal of stale future occupancy after an authoritative cancellation or series edit; absence from an incomplete response must continue to preserve protection.\n+\n+Existing assertion sources are `tests/calendar_occupancy.rs` for repeat capture, moves, projection exclusion and a Dynamic-placement control, and `tests/calendar_capture.rs::vanished_source_is_reported_by_reconcile_without_deleting_task` for retained missing-source Tasks. Their presence is not a passing result for `UBU-D0318`.\n+\n+The implementation handoff is a synthetic expanded-instance capture, restart, explicit cancellation and recapture/reconciliation reproduction, with incomplete-list and out-of-horizon controls. Locate or implement the policy-authorized ordinary-admission retirement/supersession path for cancellations and series edits used by the reviewed scope; verify that it removes only stale future reservations, preserves history and invalidates affected Plans while unproven cancellation retains protection. Attach revision-pinned commands, backend/configuration identities and results for that path and the remaining `UBU-D0318` gates to the actual scoped coverage manifest before reviewing the blocker replacement batch. No repair ticket or passing implementation result is recorded here.\n+\n ### Resolution\n \n-Open. `UBU-D0318` specifies the retirement process; verified implementation evidence and the scoped operator-approved blocker handoff remain outstanding. This specification removes no blocker and makes no claim that recurring-busy coverage is complete.\n+Open. `UBU-D0318` remains the retirement specification. The revision-pinned source audit inventories expanded-instance capture and a cancellation/retirement gap; it supplies no passing behavioral results. Actual scoped source/series coverage, confirmation of deployed paths/revisions, the remaining required checks and the operator-approved blocker handoff remain outstanding. This documentation change removes no blocker.\n \n ---\n \n ## UBU-Q0164: Deliver the primary camera-and-voice surface with usable alternatives\n \n Status: Solved Priority: MVP important Phase: Phase 2 Decision type: Product Auto-choice eligibility: Human approval required Importance score: TBD Automation-likelihood score: TBD Risk score: TBD Answerability score: 100 Depends on: None Blocks: None Resolved by: UBU-D0319 Last scored: 2026-10-11 Scored from commit: None\n \n ### Question\n \n How does the MVP realize its already-decided primary camera-and-voice surface while keeping text/forms useful for preference, privacy, precision and context?\n",
    "commit_message": "docs: inventory Q0163 capture paths and cancellation verification handoff",
    "validation_notes": [
      "Generated the diff from current working-tree contents and an in-memory proposal using git diff --no-index; git apply --check passed against the working tree.",
      "git apply --check --whitespace=error passed; hunk counts were independently verified.",
      "Only UBU-Q0163 changes. Its single-line metadata and all other question blocks are preserved.",
      "Confirmed the audited orchestrator checkout is clean at 9fddef16990ac89d07a168c75bf04b77c35b9bc3. Implementation tests were not executed; cited assertions are source evidence only.",
      "Repository files remain unchanged. No operator calendar was inspected and no blocker was retired."
    ],
    "new_questions_added": [],
    "questions_resolved": [],
    "decisions_added": [],
    "requires_human_review": true
  }
]
```

## Mechanical validation results

```json
[
  {
    "proposal_id": "openai-ubu-q0163-source-audit-20261012",
    "patch_applies": true,
    "allowlist_passed": true,
    "changed_files": [
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
