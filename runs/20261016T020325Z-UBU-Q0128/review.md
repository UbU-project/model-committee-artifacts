# Model-Committee Review

Run: `20261016T020325Z-UBU-Q0128`  
Question: `UBU-Q0128`  
Base commit: `311745af226ae3fb50a3363ca04525af8f882861`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0128-openai-stt-finalization-20261015`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 95.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0128-openai-stt-finalization-20261015` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0128-openai-stt-finalization-20261015` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0128-openai-stt-finalization-20261015` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0128-openai-stt-finalization-20261015` | `openai` | `codex` | yes | 95 | Mechanical validation confirms clean application, allowed files and schema preservation. The patch directly addresses subquestion 9 through bounded utterance completion, immutable evidence, clarification separation and callback-race handling. It preserves existing permissions, schemas, retention boundaries and turn limits, keeps UBU-Q0128 Open, and clearly limits its claims. No blocking fixes identified. | None | Sealing establishes transcript stability, not recognition correctness; adapter completion and quality policies still require qualification., The controller fixtures are specified requirements, not executed validation. |
| `UBU-Q0128-openai-stt-finalization-20261015` | `openai` | `claude` | no | n/a | claude exited with 1 | None | None |

## Selected Summary

Require sealed speech evidence before restricted-attention interpretation and bind controls to its immutable transcript revision.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Sealing establishes transcript stability, not recognition correctness; adapter completion and quality policies still require qualification.
- The controller fixtures are specified requirements, not executed validation.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261016T020325Z-UBU-Q0128/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261016T020325Z-UBU-Q0128/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261016T020325Z-UBU-Q0128"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
