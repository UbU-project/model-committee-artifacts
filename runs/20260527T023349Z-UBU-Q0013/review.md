# Model-Committee Review

Run: `20260527T023349Z-UBU-Q0013`  
Question: `UBU-Q0013`  
Base commit: `c674c45d5bd77afb638efb1dcf7c0e380593a4b5`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0013-authority-source-v1`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 87.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0013-authority-source-v1` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0013-authority-source-v1` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0013-authority-source-v1` | `codex` | `claude` | yes | 87 | The patch applies cleanly and is mechanically valid. It directly resolves all three sub-questions of UBU-Q0013: it enumerates the Phase 1 carrier objects (org-mode Objective/Preference/Task, pipeline_state, worker assignment, worker mutation requests, external projection records, Delegation Substrate packets), it clarifies that authority_source is not required on plain user-mode value objects but is required in any mode for worker/projection/pipeline-state/mutation-request envelopes, and it adopts all seven candidate enum values with precise definitions including the important advisory-only constraint on llm_advisory. UBU-D0185 is a well-formed decision record with explicit consequences and a clear pointer back to the resolved question. The stale references in DECISIONS.md are cleaned up correctly — UBU-Q0013 is removed from the open list, and the organization-mode prose now points to §17.9 rather than the open question. The OPEN_QUESTIONS.md tombstone is appropriately minimal. The only deductions are: the single-line collapsed metadata in the tombstone is a mild format risk; the §25.1 cross-reference in UBU-D0185 is fragile to future renumbering; and no migration guidance exists for pre-existing records. None of these are blocking, and no required fixes are identified. | None | The tombstone in OPEN_QUESTIONS.md collapses the structured metadata block into a single unwrapped line; if other tooling parses those headers as structured YAML-like fields, the format change may break extraction., DECISIONS.md status line references DESIGN.md §§17.9 and 25.1, but §25.1 is not a new section introduced by this patch — only a one-sentence addition. If §25.1 is renumbered by a later change, the cross-reference in UBU-D0185 becomes stale without an obvious audit trail., The decision does not address schema versioning or migration for existing records that were written before this enum was closed; production instances that already have ad-hoc authority_source values will silently violate the new constraint until validation is enforced. |

## Selected Summary

Resolve the authority_source vocabulary by adding UBU-D0185, defining required Phase 1 carriers, clarifying mode requirements, and closing UBU-Q0013 as a tombstone.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- DECISIONS.md status line references DESIGN.md §§17.9 and 25.1, but §25.1 is not a new section introduced by this patch — only a one-sentence addition. If §25.1 is renumbered by a later change, the cross-reference in UBU-D0185 becomes stale without an obvious audit trail.
- The decision does not address schema versioning or migration for existing records that were written before this enum was closed; production instances that already have ad-hoc authority_source values will silently violate the new constraint until validation is enforced.
- The tombstone in OPEN_QUESTIONS.md collapses the structured metadata block into a single unwrapped line; if other tooling parses those headers as structured YAML-like fields, the format change may break extraction.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260527T023349Z-UBU-Q0013/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260527T023349Z-UBU-Q0013/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260527T023349Z-UBU-Q0013"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
