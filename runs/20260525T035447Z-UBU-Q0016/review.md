# Model-Committee Review

Run: `20260525T035447Z-UBU-Q0016`  
Question: `UBU-Q0016`  
Base commit: `1c387fd9cad42322ba5a51fd04aff6f9430e942f`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0016-20260524-001`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 90.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0016-20260524-001` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0016-20260524-001` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0016-20260524-001` | `codex` | `claude` | yes | 90 | The patch applies cleanly with no mechanical errors. All 11 sub-questions of UBU-Q0016 are addressed: skeleton Plan representation, legitimization contents, candidate Plan representation, execution profiles per search method, probability representation (scalar/log/interval/provenance/correlation groups), independence-assumption safeguards, Compact Calendar encoding checklist, stored vs. reconstructed Plan policy, time-advance coverage recalculation, and the MVP minimum implementation list. DESIGN.md receives appropriately scoped additions to §15.2.2 and §16, and a new §16.9. DECISIONS.md receives a complete UBU-D0151 record with status, consequences, and follow-on question list. OPEN_QUESTIONS.md updates the single-line metadata format correctly (Status: Solved, Resolved by: UBU-D0151) and replaces the resolution text. No new questions are added; pre-existing narrower questions are correctly identified as continuing follow-ups rather than blockers. The proposal explicitly defers exhaustive optimization, GPU/cloud, learned policies, and exact coverage proof, keeping Phase 1 tractable. Minor risks are prescriptive profile naming and the unverified decision number, neither of which requires a fix before acceptance. | None | Execution profile names (baseline, mobile_local, desktop_or_worker, solver_validation, gpu_or_hosted) are prescriptive and may constrain implementation naming conventions that haven't been established yet., Correlation-group requirements for probability provenance add specification depth that may be premature for MVP, since the proposal itself notes MVP may conservatively mark correlation unknown., UBU-D0151 decision number is asserted without verifying it doesn't collide with an existing decision in DECISIONS.md — a merge conflict risk if another branch already used that number. |

## Selected Summary

Resolve UBU-Q0016 by accepting a minimum Compact Calendar planner grammar: skeletonization, legitimization, bounded candidate expansion, probability provenance with correlation safeguards, execution profiles, stored/reconstructed Plan policy, and MVP repair/coverage requirements.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Correlation-group requirements for probability provenance add specification depth that may be premature for MVP, since the proposal itself notes MVP may conservatively mark correlation unknown.
- Execution profile names (baseline, mobile_local, desktop_or_worker, solver_validation, gpu_or_hosted) are prescriptive and may constrain implementation naming conventions that haven't been established yet.
- UBU-D0151 decision number is asserted without verifying it doesn't collide with an existing decision in DECISIONS.md — a merge conflict risk if another branch already used that number.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260525T035447Z-UBU-Q0016/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260525T035447Z-UBU-Q0016/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260525T035447Z-UBU-Q0016"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
