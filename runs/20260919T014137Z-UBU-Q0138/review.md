# Model-Committee Review

Run: `20260919T014137Z-UBU-Q0138`  
Question: `UBU-Q0138`  
Base commit: `b02f24bac0575de535585432d43ef5f96b6d31c1`  
Automated selection: valid
Human review required: no
Selected proposal: `openai-gpt-5-codex-UBU-Q0138`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 94.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `openai-gpt-5-codex-UBU-Q0138` |
| `work-score` | `claude` | `sonnet` | `openai-gpt-5-codex-UBU-Q0138` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `openai-gpt-5-codex-UBU-Q0138` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `openai-gpt-5-codex-UBU-Q0138` | `openai` | `codex` | yes | 94 | The proposal directly answers all seven subquestions: it defines first-class AdvisoryCandidate objects, explicit lifecycle states and transitions, durable privacy-minimized rejection suppression, pre-admission influence limits with structural enforcement, resurfacing triggers, first-class decision events with Identity and authority_source, and review ordering that preserves candidate or pending state. The patch applies cleanly, modifies only DESIGN.md, DECISIONS.md, and OPEN_QUESTIONS.md, and updates the question resolution without changing the surrounding schema. Scope is appropriate for a data-model decision and does not introduce unrelated new questions. | None | The lifecycle and record schema are intentionally broad, so later implementation work may need to refine exact field names and transition enforcement details., Durable suppression metadata includes normalized proposal shape and fingerprints; care is still needed to ensure these cannot reconstruct sensitive rejected content. |
| `openai-gpt-5-codex-UBU-Q0138` | `openai` | `claude` | no | n/a | claude structured_output failed schema validation: 1 validation error for ScoreResult   Value error, selected_proposal_id must match one scored proposal_id [type=value_error, input_value={'scores': [], 'selected_...quires human approval.'}, input_type=dict]     For further information visit https://errors.pydantic.dev/2.13/v/value_error | None | None |

## Selected Summary

Resolve UBU-Q0138 by introducing first-class Phase 1b AdvisoryCandidate objects with explicit lifecycle states, review-only pre-admission influence, durable privacy-minimized rejection suppression, first-class review decision events, and state-category-safe review ordering.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Durable suppression metadata includes normalized proposal shape and fingerprints; care is still needed to ensure these cannot reconstruct sensitive rejected content.
- The lifecycle and record schema are intentionally broad, so later implementation work may need to refine exact field names and transition enforcement details.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260919T014137Z-UBU-Q0138/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260919T014137Z-UBU-Q0138/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260919T014137Z-UBU-Q0138"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
