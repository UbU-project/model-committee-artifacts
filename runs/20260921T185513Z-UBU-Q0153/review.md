# Model-Committee Review

Run: `20260921T185513Z-UBU-Q0153`  
Question: `UBU-Q0153`  
Base commit: `6db0bed1e0d46f1b148c10c1e2d17592bff39212`  
Automated selection: valid
Human review required: no
Selected proposal: `openai-gpt5-codex-UBU-Q0153-20260921`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 82.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `openai-gpt5-codex-UBU-Q0153-20260921` |
| `work-score` | `claude` | `sonnet` | `openai-gpt5-codex-UBU-Q0153-20260921` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `openai-gpt5-codex-UBU-Q0153-20260921` |
| `work-score` | `claude` | `sonnet` | `openai-gpt5-codex-UBU-Q0153-20260921` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `openai-gpt5-codex-UBU-Q0153-20260921` | `openai` | `codex` | yes | 92 | The proposal directly resolves UBU-Q0153, applies cleanly, stays within the expected design-document scope, preserves the open-question format, and answers all subquestions: canonical explicit operator prioritization, transient per-request bounded numeric values, routine-instance inheritance, scoring_policy interaction, and deferral of revealed-preference learning to UBU-Q0125. The remaining issues are wording clarifications rather than blockers. | None | The normalization formula introduces an indexing ambiguity: bucket position p must be zero-based for the stated highest-bucket value of 1.0 to hold., The routine inheritance wording references evergreen-Objective priority, which is acceptable if that priority is an admitted explicit prioritization record, but could be misread as letting Objective metadata directly define value. |
| `openai-gpt5-codex-UBU-Q0153-20260921` | `openai` | `claude` | yes | 72 | The patch applies and passes the allowlist. It touches only the four appropriate design files. It stays close to the stated current direction: explicit operator prioritization is the canonical source, values are normalized per request, utility is never persisted, and revealed-preference learning stays with UBU-Q0125. It addresses all five subquestions, including routine inheritance, the interaction with scoring_policy, and the affect and hard-constraint gates. The OPEN_QUESTIONS.md metadata stays on a single line and the Resolved by field is filled in. The main weaknesses are the fairly specific numeric choices, the deferral of deadline and routine urgency handling, and the possibility that the question should stay open pending human approval. | Confirm UBU-D0277 is the next free decision ID and that the patch context lines match the repo., Clarify how TaskSpec.priority differs from value, or justify carrying both., Consider distinguishing unranked Tasks from the lowest ranked bucket, or state explicitly that they are treated identically., Have a human approve the Solved status before it is committed. | The question's Human approval required flag means the decision needs human sign-off, so marking it Solved may be premature. The proposal sets requires_human_review: true, which partly covers this., The decision fixes specific numbers: a 0.1 floor, linear bucket spacing, and unranked Tasks at 0.1. These are arbitrary and may be too rigid for Phase 1b. Unranked Tasks tie with the lowest ranked bucket, so unranking is indistinguishable from the worst rank., Ignoring deadlines and routine urgency entirely may weaken planner quality. Subquestion 3, on rising value near a deadline or missed occurrence, is answered only by deferral., The proposal assumes a persisted ordinal priority declaration and a routine-template priority field. Neither may exist in the data model yet., The decision ID UBU-D0277 and the DECISIONS.md insertion point were not verified against the repo. A collision with an existing decision ID is possible., TaskSpec.priority is defined vaguely as a normalized priority rank, so its relationship to value is unclear., The Solved status change may affect the Blocks relationship with UBU-Q0155. No dependent question was updated. |

## Selected Summary

Resolve UBU-Q0153 by making Phase 1b Task value derive from admitted explicit operator prioritization, normalized per PlanningRequest into transient bounded TaskSpec value/priority metadata, with revealed-preference learning deferred to UBU-Q0125.

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

- Ignoring deadlines and routine urgency entirely may weaken planner quality. Subquestion 3, on rising value near a deadline or missed occurrence, is answered only by deferral.
- TaskSpec.priority is defined vaguely as a normalized priority rank, so its relationship to value is unclear.
- The Solved status change may affect the Blocks relationship with UBU-Q0155. No dependent question was updated.
- The decision ID UBU-D0277 and the DECISIONS.md insertion point were not verified against the repo. A collision with an existing decision ID is possible.
- The decision fixes specific numbers: a 0.1 floor, linear bucket spacing, and unranked Tasks at 0.1. These are arbitrary and may be too rigid for Phase 1b. Unranked Tasks tie with the lowest ranked bucket, so unranking is indistinguishable from the worst rank.
- The normalization formula introduces an indexing ambiguity: bucket position p must be zero-based for the stated highest-bucket value of 1.0 to hold.
- The proposal assumes a persisted ordinal priority declaration and a routine-template priority field. Neither may exist in the data model yet.
- The question's Human approval required flag means the decision needs human sign-off, so marking it Solved may be premature. The proposal sets requires_human_review: true, which partly covers this.
- The routine inheritance wording references evergreen-Objective priority, which is acceptable if that priority is an admitted explicit prioritization record, but could be misread as letting Objective metadata directly define value.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260921T185513Z-UBU-Q0153/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260921T185513Z-UBU-Q0153/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260921T185513Z-UBU-Q0153"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
