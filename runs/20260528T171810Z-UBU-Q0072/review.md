# Model-Committee Review

Run: `20260528T171810Z-UBU-Q0072`  
Question: `UBU-Q0072`  
Base commit: `4c32ab30bf18a56438bc152129a905e5f9a419b5`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0072-d0212`

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
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0072-d0212` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0072-d0212` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0072-d0212` | `codex` | `claude` | yes | 87 | Patch applies cleanly and is mechanically valid. It correctly closes the Phase 1 scope of UBU-Q0072 by formalising the CPU-certifies/GPU-proposes authority boundary, adding UBU-D0212 as a closing decision, updating DESIGN.md §16.10.7, and extending PLANNING_KERNEL_CONTRACT.md with the solver-identity deferral note. The tombstone in OPEN_QUESTIONS.md is terse but acceptable. The decision record is well-structured with mandatory vs advisory split and explicit consequences. No new open questions are added and deferred items (mobile, cloud) are correctly pointed to existing questions (UBU-Q0073, UBU-Q0059). Minor concern: the tombstone says 'Solved' where the question had subquestions 1, 3, 5 still open; 'Partially resolved' with explicit forward pointers would be more accurate, but this is editorial rather than a blocking defect. | None | OPEN_QUESTIONS.md tombstone collapses the full question body to a single line, losing subquestion text that could aid future Phase 2 work on Q1/Q3/Q5, Solver evaluation targets (OR-Tools, Z3) listed in DESIGN.md and DECISIONS.md are advisory-only but naming them may create implicit expectation pressure in later phases, UBU-D0212 marks UBU-Q0072 as fully 'Solved' in the tombstone while subquestions 1, 3, and 5 remain open per the question itself; the tombstone should reflect partial resolution or cross-reference those open subquestions explicitly |

## Selected Summary

Resolve UBU-Q0072 for Phase 1 by making CPU certification mandatory, treating GPU/search/solver backends as advisory, naming optional solver-library evaluation targets, and explicitly deferring mobile and cloud GPU targets to existing later-phase questions.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`
- `PLANNING_KERNEL_CONTRACT.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- OPEN_QUESTIONS.md tombstone collapses the full question body to a single line, losing subquestion text that could aid future Phase 2 work on Q1/Q3/Q5
- Solver evaluation targets (OR-Tools, Z3) listed in DESIGN.md and DECISIONS.md are advisory-only but naming them may create implicit expectation pressure in later phases
- UBU-D0212 marks UBU-Q0072 as fully 'Solved' in the tombstone while subquestions 1, 3, and 5 remain open per the question itself; the tombstone should reflect partial resolution or cross-reference those open subquestions explicitly

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260528T171810Z-UBU-Q0072/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260528T171810Z-UBU-Q0072/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260528T171810Z-UBU-Q0072"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
