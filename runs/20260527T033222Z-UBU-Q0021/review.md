# Model-Committee Review

Run: `20260527T033222Z-UBU-Q0021`  
Question: `UBU-Q0021`  
Base commit: `d72e4d14e05182aad6f96cec6baa1f15cc6802c6`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0021-d0188`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 89.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0021-d0188` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0021-d0188` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0021-d0188` | `codex` | `claude` | yes | 89 | Patch applies cleanly and mechanical validation is fully green. The proposal directly and completely answers all six sub-questions of UBU-Q0021: (1/2) separate Container, not the outer task reused; (3/4) ordinary Dynamic Tasks, no automation-step subtype; (5) metadata stays in existing envelopes (delegation fields, Delegation Substrate, worker assignment records, capability grants, ContextBundle refs, External References, Logs); (6) retries are sibling Tasks under UBU-D0187 semantics. Decision UBU-D0188 is well-drafted: it recites the rationale, names the consequences clearly, and does not over-specify. Stale 'UBU-Q0021 remains open' references in two earlier decision consequence blocks are correctly cleaned up. OPEN_QUESTIONS.md tombstone updates Status and Resolved-by fields in place and follows the same pattern used for Q0020. No new questions are introduced; the proposal correctly identifies that the existing worker-mutation, retry, and task-to-container decisions already supply the necessary sub-answers. Minor deductions for the section-numbering cross-reference risk and the tombstone blank-line inconsistency, but neither is blocking. | None | DESIGN.md top-of-patch cross-reference mentions §24.1.2 but the patch adds content under a prose subheading ('Automation child Task structure') without an explicit §24.1.2 section-number label; if DESIGN.md uses numbered sections, the cross-ref may be imprecise or drift., Tombstone in OPEN_QUESTIONS.md omits a blank line between the metadata line and 'Resolved. See UBU-D0188.' — minor formatting inconsistency that could affect automated parsers depending on how Q0020's tombstone was actually formatted., Workers-may-propose constraint is stated in both DESIGN.md §9.4 (retained paragraph stub) and new §24.1.2 — slight redundancy that could diverge on future edits. |

## Selected Summary

Resolve Automation Worker parent/child structure by using Task-to-Container restructuring, ordinary child Tasks, existing worker/delegation metadata envelopes, and D0187 retry siblings.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- DESIGN.md top-of-patch cross-reference mentions §24.1.2 but the patch adds content under a prose subheading ('Automation child Task structure') without an explicit §24.1.2 section-number label; if DESIGN.md uses numbered sections, the cross-ref may be imprecise or drift.
- Tombstone in OPEN_QUESTIONS.md omits a blank line between the metadata line and 'Resolved. See UBU-D0188.' — minor formatting inconsistency that could affect automated parsers depending on how Q0020's tombstone was actually formatted.
- Workers-may-propose constraint is stated in both DESIGN.md §9.4 (retained paragraph stub) and new §24.1.2 — slight redundancy that could diverge on future edits.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260527T033222Z-UBU-Q0021/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260527T033222Z-UBU-Q0021/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260527T033222Z-UBU-Q0021"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
