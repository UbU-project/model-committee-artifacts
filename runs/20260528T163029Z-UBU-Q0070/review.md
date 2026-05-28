# Model-Committee Review

Run: `20260528T163029Z-UBU-Q0070`  
Question: `UBU-Q0070`  
Base commit: `91db4ee5ff116480e5d3d0a7bead6314ddc38793`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0070-skeleton-diagnostics-d0210`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 88.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0070-skeleton-diagnostics-d0210` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0070-skeleton-diagnostics-d0210` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0070-skeleton-diagnostics-d0210` | `codex` | `claude` | yes | 88 | The patch cleanly applies and fully addresses all four subquestions of UBU-Q0070: all eight MVP failure classes are enumerated, the explanation budget (three causal-chain steps default, full detail via inspector) is specified, nine safe alternatives are listed with explicit prohibition on inventing canonical resources, and the blocking-vs-warning policy is clearly defined. The SkeletonFailureDiagnostic schema in PLANNING_KERNEL_CONTRACT.md is well-bounded and actionable for implementers. DESIGN.md additions are minimal and well-scoped. The OPEN_QUESTIONS.md tombstone correctly updates status, resolved-by, and collapses the body while preserving the metadata line. DECISIONS.md entry UBU-D0210 is well-structured with consequences noted. The only minor concern is the section reference '§15.2.2' in DECISIONS.md, which is unverifiable without seeing DESIGN.md section numbering. | None | DECISIONS.md references 'DESIGN.md §15.2.2' but the patch does not show section headers being renumbered or confirmed; if DESIGN.md sections shift, this pointer will be stale., SkeletonFailureDiagnostic is defined in PKC with a fixed field list; future questions about optional vs required fields or versioning may conflict with this schema definition. |

## Selected Summary

Resolve UBU-Q0070 by defining MVP skeleton Plan failure classes, the SkeletonFailureDiagnostic payload, safe clarification alternatives, explanation budget, and blocking-vs-warning prompt policy.

## Changed Files

- `DESIGN.md`
- `PLANNING_KERNEL_CONTRACT.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- DECISIONS.md references 'DESIGN.md §15.2.2' but the patch does not show section headers being renumbered or confirmed; if DESIGN.md sections shift, this pointer will be stale.
- SkeletonFailureDiagnostic is defined in PKC with a fixed field list; future questions about optional vs required fields or versioning may conflict with this schema definition.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260528T163029Z-UBU-Q0070/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260528T163029Z-UBU-Q0070/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260528T163029Z-UBU-Q0070"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
