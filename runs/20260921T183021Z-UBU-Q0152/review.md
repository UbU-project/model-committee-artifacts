# Model-Committee Review

Run: `20260921T183021Z-UBU-Q0152`  
Question: `UBU-Q0152`  
Base commit: `6cccfc13603d3dd7ec5ae0218cbdfa5a74623082`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0152-P-gpt5-codex-001`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 88.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `UBU-Q0152-P-gpt5-codex-001` |
| `work-score` | `claude` | `sonnet` | `UBU-Q0152-P-gpt5-codex-001` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `UBU-Q0152-P-gpt5-codex-001` |
| `work-score` | `claude` | `sonnet` | `UBU-Q0152-P-gpt5-codex-001` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0152-P-gpt5-codex-001` | `openai` | `codex` | yes | 94 | The proposal cleanly follows the current direction: a single hard absolute UTC allowed_time_range for one-off Dynamic Tasks, CPU mapping to TaskSpec.window by intersection with the planning horizon, routine/local time handling deferred to instantiation, decision envelopes kept derived, Static Tasks kept distinct, and occupies_capacity retirement made evidence-gated. The patch is minimal, applies cleanly, touches appropriate documentation files, and resolves the open question without forcing a kernel multi-window contract change. | None | The decision states that ambiguous or nonexistent local endpoints require recorded disambiguation or review, but does not define the exact disambiguation policy; this is acceptable because recurring local windows are deferred to UBU-Q0154., The PLANNING_KERNEL_CONTRACT wording says each Dynamic TaskSpec carries a window, which is aligned with the resolution but may need later harmonization if the contract also represents non-dynamic or already-fixed work in the same structure. |
| `UBU-Q0152-P-gpt5-codex-001` | `openai` | `claude` | yes | 82 | The patch applies cleanly, touches only allowed docs, and follows the question's current direction: single hard absolute UTC range, mapped to TaskSpec.window by intersection, distinct from the decision envelope and due dates, with multi-window deferred. It answers all seven subquestions concisely and adds no unnecessary scope. Weaknesses are the vague occupies_capacity retirement evidence, limited DST detail, and the Solved status for a human-approval question. | Confirm DECISIONS.md numbering UBU-D0276 is the next free ID and that the placement of the new entry matches file ordering., Consider leaving Status Open pending human approval, or confirm the Solved transition is intended for this workflow., Clarify the specific evidence criteria for retiring occupies_capacity, or track it as a follow-up question. | Marks Q0152 Solved although the question requires human approval; Resolved by/status change is aggressive though requires_human_review is set., occupies_capacity retirement is left as an evidence-gated, loosely defined condition; the 'canonical rule' sentence may conflict with the current orchestrator gate., Static Task vs zero-slack range is stated as mutually exclusive, which slightly hardens a subquestion beyond the current direction., Time zone and DST handling is deferred to UBU-Q0154 with limited specifics (no explicit DST disambiguation policy)., Adds text in DESIGN.md §16.5 area (criticality paragraph) that partially duplicates §9.2 content., Decision text says PLANNING_KERNEL_CONTRACT §3 while question context cites §2; minor reference inconsistency. |

## Selected Summary

Resolve UBU-Q0152 by adding UBU-D0276: one-off Dynamic Tasks use a single hard absolute UTC allowed_time_range that maps to TaskSpec.window, while recurrence resolves local windows into concrete UTC instance ranges before kernel dispatch.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`
- `PLANNING_KERNEL_CONTRACT.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Adds text in DESIGN.md §16.5 area (criticality paragraph) that partially duplicates §9.2 content.
- Decision text says PLANNING_KERNEL_CONTRACT §3 while question context cites §2; minor reference inconsistency.
- Marks Q0152 Solved although the question requires human approval; Resolved by/status change is aggressive though requires_human_review is set.
- Static Task vs zero-slack range is stated as mutually exclusive, which slightly hardens a subquestion beyond the current direction.
- The PLANNING_KERNEL_CONTRACT wording says each Dynamic TaskSpec carries a window, which is aligned with the resolution but may need later harmonization if the contract also represents non-dynamic or already-fixed work in the same structure.
- The decision states that ambiguous or nonexistent local endpoints require recorded disambiguation or review, but does not define the exact disambiguation policy; this is acceptable because recurring local windows are deferred to UBU-Q0154.
- Time zone and DST handling is deferred to UBU-Q0154 with limited specifics (no explicit DST disambiguation policy).
- occupies_capacity retirement is left as an evidence-gated, loosely defined condition; the 'canonical rule' sentence may conflict with the current orchestrator gate.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260921T183021Z-UBU-Q0152/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260921T183021Z-UBU-Q0152/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260921T183021Z-UBU-Q0152"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
