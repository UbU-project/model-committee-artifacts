# Model-Committee Review

Run: `20260919T012231Z-UBU-Q0088`  
Question: `UBU-Q0088`  
Base commit: `e652dc4fcfb46284cfe5f39da3f38739cf67ad37`  
Automated selection: valid
Human review required: no
Selected proposal: `ubu-q0088-gpt5-codex-d0273`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 92.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `ubu-q0088-gpt5-codex-d0273` |
| `work-score` | `claude` | `sonnet` | `ubu-q0088-gpt5-codex-d0273` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `ubu-q0088-gpt5-codex-d0273` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `ubu-q0088-gpt5-codex-d0273` | `openai` | `codex` | yes | 92 | The proposal applies cleanly, modifies only the expected canonical files, preserves the open-question structure while marking the question resolved, and directly answers all five subquestions. It gives useful durable rejection semantics, prohibits pre-review model influence on durable or external state, defines resurfacing triggers, and constrains accepted findings to ordinary admitted records rather than in-place Relationship mutation. The scope is appropriately limited to design documentation and a decision record; remaining risks are mostly around future implementation precision rather than blocking flaws. | None | The lifecycle is clear at the narrative level but does not enumerate every possible edge as a compact transition table, so implementers may still need to infer some terminal/archive behavior., Resolving the full question while the current direction notes extrospection and the association model are deferred may slightly front-load design commitments ahead of later model work. |
| `ubu-q0088-gpt5-codex-d0273` | `openai` | `claude` | no | n/a | claude structured_output failed schema validation: 1 validation error for ScoreResult   Value error, selected_proposal_id must match one scored proposal_id [type=value_error, input_value={'scores': [], 'selected_...he open Q0138 overlap.'}, input_type=dict]     For further information visit https://errors.pydantic.dev/2.13/v/value_error | None | None |

## Selected Summary

Resolve UBU-Q0088 by defining ExtrospectionFinding as an evidence-backed review candidate with explicit lifecycle states, durable rejection keys, resurfacing rules, and constrained Relationship effects.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Resolving the full question while the current direction notes extrospection and the association model are deferred may slightly front-load design commitments ahead of later model work.
- The lifecycle is clear at the narrative level but does not enumerate every possible edge as a compact transition table, so implementers may still need to infer some terminal/archive behavior.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260919T012231Z-UBU-Q0088/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260919T012231Z-UBU-Q0088/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260919T012231Z-UBU-Q0088"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
