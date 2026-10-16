# Model-Committee Review

Run: `20261016T022606Z-UBU-Q0166`  
Question: `UBU-Q0166`  
Base commit: `c33836bfc4141a45bee394c3f344b8ee9a63c469`  
Automated selection: valid
Human review required: no
Selected proposal: `ubu-q0166-mounted-review-20261015`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 90.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `ubu-q0166-mounted-review-20261015` |
| `work-score` | `claude` | `claude-fable-5-1` | `ubu-q0166-mounted-review-20261015` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `ubu-q0166-mounted-review-20261015` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `ubu-q0166-mounted-review-20261015` | `openai` | `codex` | yes | 90 | Mechanical validation confirms clean application, allowed files and preserved question schema. The patch provides relevant incremental review-path evidence with pinned revisions, explicit controls and carefully bounded conclusions. It correctly keeps Q0166 Open and makes no adapter-qualification or schema-gap claim. Auditability is weakened by omission of the executable harness. | None | The in-memory harness is not included, limiting independent reproduction of the reported DOM and click results., Mocked client calls and unresolved carrier specimens leave full qualifier presentation, native admission and authorized exchange unvalidated. |
| `ubu-q0166-mounted-review-20261015` | `openai` | `claude` | no | n/a | claude exited with 1 | None | None |

## Selected Summary

Record pinned mounted-review controls showing invisible inline qualifier links and literal-only reference presentation; keep UBU-Q0166 Open.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Mocked client calls and unresolved carrier specimens leave full qualifier presentation, native admission and authorized exchange unvalidated.
- The in-memory harness is not included, limiting independent reproduction of the reported DOM and click results.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261016T022606Z-UBU-Q0166/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261016T022606Z-UBU-Q0166/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261016T022606Z-UBU-Q0166"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
