# Model-Committee Review

Run: `20260922T022444Z-UBU-Q0157`  
Question: `UBU-Q0157`  
Base commit: `d2bc730b2920210cc9e774ff7e509541ea451b7a`  
Automated selection: valid
Human review required: no
Selected proposal: `openai-gpt5-codex-UBU-Q0157-001`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 89.5
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `openai-gpt5-codex-UBU-Q0157-001` |
| `work-score` | `claude` | `sonnet` | `openai-gpt5-codex-UBU-Q0157-001` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `openai-gpt5-codex-UBU-Q0157-001` |
| `work-score` | `claude` | `sonnet` | `openai-gpt5-codex-UBU-Q0157-001` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `openai-gpt5-codex-UBU-Q0157-001` | `openai` | `codex` | yes | 91 | The proposal is valid, applies cleanly, stays within the allowed files, and directly resolves all major subquestions: atomic default, explicit splittable fields, constant resume overhead, one piece per chunk, allowed-range enforcement, atomic decomposition segments, shared minor contract version with partial placement, rollout continuation semantics, same-Task progress recording, proportional splittable value, and projection behavior. The only required fix is a local documentation contradiction left behind from the prior decomposition text. | Clarify the remaining DESIGN.md sentence about the kernel contract so it applies only to decomposition segment compilation or otherwise acknowledges the split-policy contract change. | DESIGN.md retains the sentence 'Phase 1b keeps the planning kernel contract unchanged' in the decomposition section, which now conflicts with the accepted split-policy contract changes unless narrowed to decomposition-segment handling., The decision text has a minor wording inconsistency by first calling min_piece_seconds non-negative and then immediately requiring it to be positive. |
| `openai-gpt5-codex-UBU-Q0157-001` | `openai` | `claude` | yes | 88 | The patch applies cleanly and touches only the allowlisted files (DECISIONS.md, DESIGN.md, OPEN_QUESTIONS.md, PLANNING_KERNEL_CONTRACT.md) per mechanical validation. It answers all ten subquestions: policy shape with atomic default and required splittable fields (no hidden category defaults, advisors may propose but admission is explicit), constant resume overhead with no Phase 1b affect cost, at-most-one-piece-per-chunk placement inside the Task's allowed range, segments staying atomic with child Tasks independently splittable only via segment boundaries, a shared minor contract version with UBU-Q0155, rollout semantics for one sampled total duration spent across pieces with continuation-as-feasibility, explicit statement that Phase 1b needs only the split policy as interruption-facing input, Log-evidence-based progress recording without Container replacement, proportional value for completed sampled work while completion/effects wait for the last piece, and piece-based projection titles like '(2/3)' anchored to one canonical Task. OPEN_QUESTIONS.md preserves the single-line metadata format and updates status/Resolution appropriately. The contract additions to PLANNING_KERNEL_CONTRACT.md are proportionate to the decision and don't reach into unrelated sections. No unnecessary scope expansion is evident. | None | Constant resume overhead is a simplifying assumption; later phases may need time-dependent overhead, requiring another contract revision, Proportional value for incomplete splittable Tasks could create scoring incentives that favor starting many splittable Tasks over completing atomic ones, though this is flagged as Phase 1b scope, Sharing the minor contract version with UBU-Q0155 partial placement couples two independent decisions; if UBU-Q0155 resolves differently the two decision records will need reconciliation |

## Selected Summary

Resolve splittable Tasks as explicit per-Task split policies with atomic default, required splittable fields, constant resume overhead, at most one piece per chunk, same-Task progress tracking, proportional scheduled-work value, and split-aware kernel piece metadata/rollout semantics.

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

- Constant resume overhead is a simplifying assumption; later phases may need time-dependent overhead, requiring another contract revision
- DESIGN.md retains the sentence 'Phase 1b keeps the planning kernel contract unchanged' in the decomposition section, which now conflicts with the accepted split-policy contract changes unless narrowed to decomposition-segment handling.
- Proportional value for incomplete splittable Tasks could create scoring incentives that favor starting many splittable Tasks over completing atomic ones, though this is flagged as Phase 1b scope
- Sharing the minor contract version with UBU-Q0155 partial placement couples two independent decisions; if UBU-Q0155 resolves differently the two decision records will need reconciliation
- The decision text has a minor wording inconsistency by first calling min_piece_seconds non-negative and then immediately requiring it to be positive.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260922T022444Z-UBU-Q0157/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260922T022444Z-UBU-Q0157/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260922T022444Z-UBU-Q0157"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
