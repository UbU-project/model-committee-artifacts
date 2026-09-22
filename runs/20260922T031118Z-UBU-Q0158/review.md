# Model-Committee Review

Run: `20260922T031118Z-UBU-Q0158`  
Question: `UBU-Q0158`  
Base commit: `52ff8411256450e3c25c3469891cbf46d9880656`  
Automated selection: valid
Human review required: no
Selected proposal: `proposal-ubu-q0158-gpt5-codex`

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
| `work-score` | `codex` | `gpt-5.5` | `proposal-ubu-q0158-gpt5-codex` |
| `work-score` | `claude` | `sonnet` | `proposal-ubu-q0158-gpt5-codex` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `proposal-ubu-q0158-gpt5-codex` |
| `work-score` | `claude` | `sonnet` | `proposal-ubu-q0158-gpt5-codex` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `proposal-ubu-q0158-gpt5-codex` | `openai` | `codex` | yes | 94 | The proposal directly resolves UBU-Q0158 and cleanly distinguishes decision alternatives from stochastic outcome continuations. It answers all subquestions: outcome-state grouping, rollout frequency probabilities and confidence, coverage mapping to branch_coverage_target/reactive_horizon_seconds, shared budget allocation, compact/mobile packaging depth, intra-chunk repair handling, and streaming behavior. It modifies only the expected design and contract files, preserves the open-question metadata shape, and adds a single auditable decision without unnecessary expansion. Mechanical validation reports the patch applies after recount normalization, with no allowlist issue. | None | The added contract diagnostics are intentionally optional but still underspecified as object shapes, so later implementers may need a follow-up schema tightening pass., The budget rule protects requested K decision alternatives before outcome continuations, which is reasonable for interactivity but could make coverage targets harder to satisfy under tight budgets. |
| `proposal-ubu-q0158-gpt5-codex` | `openai` | `claude` | yes | 85 | The proposal directly answers all seven subquestions: it defines outcome states as boundary equivalence classes, ties probability estimation to UBU-D0279's shared-latent rollouts with confidence quality, maps coverage mass to branch_coverage_target/reactive_horizon_seconds, specifies shared compute budget prioritization (current chunk + K alternatives first, then continuations by probability), scopes mobile packaging to compact refs through the reactive horizon, confines intra-chunk deviations to decision envelopes/repair recipes, and allows continuations to stream incrementally in later chunk_result frames. Changes are confined to the four canonical design files (DECISIONS.md, DESIGN.md, OPEN_QUESTIONS.md, PLANNING_KERNEL_CONTRACT.md), the OPEN_QUESTIONS.md status/resolution fields follow the existing single-line metadata and Resolution-paragraph schema, and the new UBU-D0285 decision is properly cross-referenced with Consequences pointing back at the affected sections. The work is well-scoped to the coverage/outcome-branching question without touching unrelated files. The one notable flaw is that the raw patch did not apply cleanly under an ordinary git apply and needed --recount, which the harness's mechanical validation absorbed and marked patch_applies=true; this is a minor mechanical risk rather than a content defect. | None | Patch required --recount normalization to apply (ordinary apply failed with 'corrupt patch at line 57'), indicating the raw diff as submitted is not directly git-apply-able and depends on the harness's normalization step., Introduces several new optional contract fields (coverage_confidence, outcome_continuation_summary, outcome_continuation_refs) that expand the wire schema; reasonable but adds surface area an implementer must eventually build., Merging of near-identical outcome states via 'deterministic digest or quantization rule' is left underspecified beyond 'recorded in diagnostics', leaving some implementation latitude. |

## Selected Summary

Resolve UBU-Q0158 by defining compact Calendar coverage as probability mass over certified chunk-boundary outcome continuations, with rollout-derived probabilities, shared compute budgeting with decision alternatives, reactive-horizon packaging, local-only intra-chunk repair, and incremental interactive streaming.

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

- Introduces several new optional contract fields (coverage_confidence, outcome_continuation_summary, outcome_continuation_refs) that expand the wire schema; reasonable but adds surface area an implementer must eventually build.
- Merging of near-identical outcome states via 'deterministic digest or quantization rule' is left underspecified beyond 'recorded in diagnostics', leaving some implementation latitude.
- Patch required --recount normalization to apply (ordinary apply failed with 'corrupt patch at line 57'), indicating the raw diff as submitted is not directly git-apply-able and depends on the harness's normalization step.
- The added contract diagnostics are intentionally optional but still underspecified as object shapes, so later implementers may need a follow-up schema tightening pass.
- The budget rule protects requested K decision alternatives before outcome continuations, which is reasonable for interactivity but could make coverage targets harder to satisfy under tight budgets.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260922T031118Z-UBU-Q0158/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260922T031118Z-UBU-Q0158/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260922T031118Z-UBU-Q0158"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
