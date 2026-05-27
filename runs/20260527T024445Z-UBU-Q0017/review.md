# Model-Committee Review

Run: `20260527T024445Z-UBU-Q0017`  
Question: `UBU-Q0017`  
Base commit: `a3a380fcc1c5f4389b554fff287e0c5b9bca674d`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0017-coverage-regeneration-001`

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
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0017-coverage-regeneration-001` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0017-coverage-regeneration-001` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0017-coverage-regeneration-001` | `codex` | `claude` | yes | 88 | The patch applies cleanly and directly resolves all six sub-questions posed by UBU-Q0017. Coverage is correctly characterized as estimated (Q1); the four uncertainty categories are addressed with Objective recurrence excluded from MVP (Q2); the compact Calendar is specified to store both coverage_estimate and uncovered_mass_estimate (Q3); the 0.99 default threshold is confirmed (Q4); the three-level policy resolution order is defined with effective threshold stored for replayability (Q5); and low coverage routes through recalculation trigger plus risk reporting rather than directly creating Tasks or worker assignments (Q6). Changed files are limited to the four documentation files appropriate for this resolution, and the allowlist check passed. The DECISIONS.md entry UBU-D0186 is internally consistent and provides good consequences framing. The PLANNING_KERNEL_CONTRACT.md additions (coverage_scope, uncovered_mass_estimate, coverage_threshold_used, coverage_below_threshold) extend the output schema minimally and coherently. Minor issues: the trigger escalation prose in both DECISIONS.md and DESIGN.md contains a grammatical tangle that should be cleaned up before merge, and the OPEN_QUESTIONS.md tombstone is missing a blank line before the 'Resolved.' sentence. Neither issue is blocking. No new open questions are introduced. Human review is correctly flagged. | None | The DECISIONS.md and DESIGN.md trigger policy contains a grammatical repetition ('can affect the current or next recommended Task, hard feasibility, affect legitimacy') that may cause ambiguity when implementers later read the trigger escalation rule., The tombstone in OPEN_QUESTIONS.md omits a blank line between the status metadata line and 'Resolved. See UBU-D0186.' which may confuse downstream markdown parsers or tooling that expects a blank separator., Excluding Objective recurrence uncertainty from MVP coverage is clearly flagged, but the phrase 'probabilistic recurrence may become a later stochastic input' is left without a tracking question or future-work marker, so it could silently stall., The policy resolution order (Calendar-specific → execution-profile/Device → global) is reasonable but introduces a three-level lookup that must be consistently implemented in the CPU; a mismatch between implementations could make stored coverage_threshold_used unreliable for replay. |

## Selected Summary

Resolve Compact Calendar coverage and regeneration by defining MVP coverage as estimated compact-serialization metadata with stored uncovered mass, effective threshold policy, and low-coverage trigger behavior.

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

- Excluding Objective recurrence uncertainty from MVP coverage is clearly flagged, but the phrase 'probabilistic recurrence may become a later stochastic input' is left without a tracking question or future-work marker, so it could silently stall.
- The DECISIONS.md and DESIGN.md trigger policy contains a grammatical repetition ('can affect the current or next recommended Task, hard feasibility, affect legitimacy') that may cause ambiguity when implementers later read the trigger escalation rule.
- The policy resolution order (Calendar-specific → execution-profile/Device → global) is reasonable but introduces a three-level lookup that must be consistently implemented in the CPU; a mismatch between implementations could make stored coverage_threshold_used unreliable for replay.
- The tombstone in OPEN_QUESTIONS.md omits a blank line between the status metadata line and 'Resolved. See UBU-D0186.' which may confuse downstream markdown parsers or tooling that expects a blank separator.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260527T024445Z-UBU-Q0017/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260527T024445Z-UBU-Q0017/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260527T024445Z-UBU-Q0017"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
