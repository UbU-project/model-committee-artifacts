# Model-Committee Review

Run: `20260527T040431Z-UBU-Q0035`  
Question: `UBU-Q0035`  
Base commit: `4ad0f3efaf487fc7771d7ebee52ba6fa5d994fef`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0035-automation-coverage-taxonomy`

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
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0035-automation-coverage-taxonomy` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0035-automation-coverage-taxonomy` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0035-automation-coverage-taxonomy` | `codex` | `claude` | yes | 85 | Patch applies cleanly per mechanical validation. The proposal directly answers the question by defining a three-class taxonomy (Auto eligible / Human approval required / Human only) and correctly propagates that answer across all three documentation layers: DESIGN.md §3.7.1 for normative spec, DECISIONS.md UBU-D0190 for rationale and consequences, and OPEN_QUESTIONS.md for the tombstone. The tombstone is well-formed: Status is updated to Solved, Resolved by is set to UBU-D0190, and the body is reduced to a forwarding pointer. No files outside the standard allowed set are touched, and no new open questions are introduced. The primary concern is the unaddressed UBU-Q0032 dependency — the process normally requires that dependency to be resolved (or co-resolved) before the dependent question can close. This is not a patch defect but is a process gap that reviewers should verify. The UBU-D0175 forward reference is a minor coupling risk. No required fixes are identified because the taxonomy content is substantively correct and the process gap is a reviewer check rather than a patch error. | None | UBU-Q0035 declares 'Depends on: UBU-Q0032' in its metadata header; the proposal resolves it without verifying or documenting whether UBU-Q0032 is already resolved, which could violate the dependency gate., The UBU-D0190 decision text contains a forward reference to 'UBU-D0175 blocker certificate' with a specific classification rule; if UBU-D0175's semantics have changed or are still in flux, this rule may need revisiting., The enumerated 'Human only' and 'Human approval required' category lists are long and prescriptive; future decisions may conflict with or duplicate them, requiring retroactive harmonization. |

## Selected Summary

Resolve UBU-Q0035 by defining a three-class automation eligibility taxonomy for model-committee question work and tombstoning the solved question.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- The UBU-D0190 decision text contains a forward reference to 'UBU-D0175 blocker certificate' with a specific classification rule; if UBU-D0175's semantics have changed or are still in flux, this rule may need revisiting.
- The enumerated 'Human only' and 'Human approval required' category lists are long and prescriptive; future decisions may conflict with or duplicate them, requiring retroactive harmonization.
- UBU-Q0035 declares 'Depends on: UBU-Q0032' in its metadata header; the proposal resolves it without verifying or documenting whether UBU-Q0032 is already resolved, which could violate the dependency gate.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260527T040431Z-UBU-Q0035/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260527T040431Z-UBU-Q0035/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260527T040431Z-UBU-Q0035"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
