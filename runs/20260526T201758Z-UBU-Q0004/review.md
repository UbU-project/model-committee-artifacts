# Model-Committee Review

Run: `20260526T201758Z-UBU-Q0004`  
Question: `UBU-Q0004`  
Base commit: `0d363ad36a99f2cbff8c1ef183742a8ea378d8ed`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0004-pipeline-state-001`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 85.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0004-pipeline-state-001` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0004-pipeline-state-001` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0004-pipeline-state-001` | `codex` | `claude` | yes | 85 | The proposal comprehensively answers all six UBU-Q0004 sub-questions: pipeline_state is declared generic and projection-scoped (Q1), one Objective can hold multiple pipeline states across projections (Q2), state is stored in projection metadata not on Objective (Q3), the candidate enum is adopted with per-value semantic definitions (Q4), workers use request-only mutation (Q5), and every accepted transition produces a pipeline_state_transitioned Log entry (Q6). The decision record UBU-D0182 is well-formed, the enum semantics are precise and actionable, and the worker-authority boundary is consistent with the existing mutation-request pattern. The patch is limited to the three expected documentation files, mechanical validation passed, and no new open questions are introduced. Deductions are for the potentially stale §17.2 cross-reference in the decision status line, the missing blank line in the tombstone, and the 'Solved' vs 'Resolved' status-token inconsistency — all fixable before merge without rework of substance. | Verify that DESIGN.md §17.2 already contains pipeline_state content; if not, remove §17.2 from the DECISIONS.md cross-reference or add the missing §17.2 content., Restore the blank line between '## UBU-Q0004: Pipeline State' and the status metadata line in OPEN_QUESTIONS.md to match the format of other tombstones., Align the tombstone status token ('Solved' vs 'Resolved') with whatever value other resolved questions use in OPEN_QUESTIONS.md. | DECISIONS.md UBU-D0182 status line references DESIGN.md §§17.2, 26.2 but the patch only modifies §26.2. If §17.2 does not contain pipeline_state content already established in a prior commit, this is a dangling cross-reference., OPEN_QUESTIONS.md tombstone removes the blank line between the ## heading and the status metadata line, which may break parsers that rely on that formatting., Tombstone uses 'Status: Solved' while other resolved questions in the file appear to use 'Status: Resolved' — inconsistent vocabulary could cause tooling mismatches., pipeline_state_transitioned is added to the MVP event-types list in DESIGN.md without a corresponding definition entry if event types are formally enumerated elsewhere in the spec. |

## Selected Summary

Resolve UBU-Q0004 by defining pipeline_state as generic projection-scoped workflow metadata, stored outside Objective, with a closed Phase 1 enum, request-only worker mutation, and mandatory transition logging.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- DECISIONS.md UBU-D0182 status line references DESIGN.md §§17.2, 26.2 but the patch only modifies §26.2. If §17.2 does not contain pipeline_state content already established in a prior commit, this is a dangling cross-reference.
- OPEN_QUESTIONS.md tombstone removes the blank line between the ## heading and the status metadata line, which may break parsers that rely on that formatting.
- Tombstone uses 'Status: Solved' while other resolved questions in the file appear to use 'Status: Resolved' — inconsistent vocabulary could cause tooling mismatches.
- pipeline_state_transitioned is added to the MVP event-types list in DESIGN.md without a corresponding definition entry if event types are formally enumerated elsewhere in the spec.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260526T201758Z-UBU-Q0004/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260526T201758Z-UBU-Q0004/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260526T201758Z-UBU-Q0004"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
