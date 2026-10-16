# Model-Committee Review

Run: `20261016T035222Z-UBU-Q0160`  
Question: `UBU-Q0160`  
Base commit: `8e1d0a7905a856ceddc9ac463ba1fe949275a68c`  
Automated selection: valid
Human review required: no
Selected proposal: `openai-ubu-q0160-structural-pack-v1`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 92.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-ubu-q0160-structural-pack-v1` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-ubu-q0160-structural-pack-v1` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-ubu-q0160-structural-pack-v1` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `openai-ubu-q0160-structural-pack-v1` | `openai` | `codex` | yes | 92 | Mechanical validation passes, and changes stay within the allowed files. The concrete outcomes agree with the existing dependency, containment, Static-anchor, overlap and masking semantics. Independent expected values and comparisons before CPU certification strengthen the first harness slice. The patch preserves Open status, metadata, CPU authority and all remaining qualification gates. It advances fixture specification without supplying API-selection evidence. | None | The pack remains a specification: serialized fixtures, independently checked CPU goldens, harness execution and physical-device measurements are still outstanding. |
| `openai-ubu-q0160-structural-pack-v1` | `openai` | `claude` | no | n/a | claude exited with 1 | None | None |

## Selected Summary

Specify concrete synthetic fixtures for the first mobile skeletonization and hard-check parity slice.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- The pack remains a specification: serialized fixtures, independently checked CPU goldens, harness execution and physical-device measurements are still outstanding.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261016T035222Z-UBU-Q0160/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261016T035222Z-UBU-Q0160/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261016T035222Z-UBU-Q0160"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
