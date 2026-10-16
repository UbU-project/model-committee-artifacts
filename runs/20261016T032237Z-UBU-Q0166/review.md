# Model-Committee Review

Run: `20261016T032237Z-UBU-Q0166`  
Question: `UBU-Q0166`  
Base commit: `3d271b324ecf23946dd559281bc3059eceda6e9b`  
Automated selection: valid
Human review required: no
Selected proposal: `openai-ubu-q0166-client-numeric-fidelity-20261015`

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
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-ubu-q0166-client-numeric-fidelity-20261015` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-ubu-q0166-client-numeric-fidelity-20261015` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-ubu-q0166-client-numeric-fidelity-20261015` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `openai-ubu-q0166-client-numeric-fidelity-20261015` | `openai` | `codex` | yes | 88 | Provides relevant incremental fidelity evidence and identifies how shared decoding can mask value loss. Mechanical validation passes, changes stay within allowed files, and question metadata remains intact. The proposal carefully bounds its claims, preserves the full enablement gate and Open status, and makes no unsupported schema-gap or adapter-approval claim. | None | Run-only harness and result hashes lack accompanying artifacts, limiting independent reproduction., Synthetic carriers with incomplete qualifications do not establish preservation for a permitted mapping or numeric domain. |
| `openai-ubu-q0166-client-numeric-fidelity-20261015` | `openai` | `claude` | no | n/a | claude exited with 1 | None | None |

## Selected Summary

Record pinned client JSON precision loss and require independent exact-value fidelity checks; keep UBU-Q0166 open.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Run-only harness and result hashes lack accompanying artifacts, limiting independent reproduction.
- Synthetic carriers with incomplete qualifications do not establish preservation for a permitted mapping or numeric domain.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261016T032237Z-UBU-Q0166/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261016T032237Z-UBU-Q0166/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261016T032237Z-UBU-Q0166"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
