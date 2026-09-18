# Model-Committee Review

Run: `20260918T125514Z-UBU-Q0137`  
Question: `UBU-Q0137`  
Base commit: `21d513d5b9dff6cabf4b8afcc9a0fc1bf7daccdf`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0137-d0264`

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
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0137-d0264` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0137-d0264` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0137-d0264` | `codex` | `claude` | yes | 90 | The patch applies cleanly per mechanical validation and touches only the three allowed files (DESIGN.md, DECISIONS.md, OPEN_QUESTIONS.md). It directly answers all seven subquestions: BackgroundProcess fields (operation kind, eligibility filter, pass caps, interval, interrupt state), per-object state via BackgroundProcessObjectState keyed by process/object/version, event-sourced interruption/resumption (run_interrupted, run_resumed, checkpoint cursors), a clear notification/escalation policy tied to failure/no-output thresholds, an explicit no-Calendar-time policy paired with operator-visible telemetry, a graded prompt-injection exposure model with concrete containment measures, and a precise no-successful-work exit contract distinguishing clean no-op from escalation-required failure. The OPEN_QUESTIONS.md edit correctly preserves the question block structure (title, metadata line updated in place, subquestions untouched, Resolution section filled in) rather than restructuring the schema. Scope is appropriately narrow: it explicitly declines to resolve AgentAction/rollback semantics (UBU-Q0084), staying within Phase 1b. The new decision reuses named concepts from prior decisions (WorkerAuthority, LocalAdvisorySubmission/Result, mutation-envelope/idempotency rules) suggesting internal consistency, though this could not be fully cross-checked against the live repo state in this review. | None | Introduces a new decision UBU-D0264 and cross-references several prior decisions (D0263, D0255, D0258, D0262) whose exact current wording could not be independently verified in this review, so there is some risk of subtle inconsistency with those sections as they exist on the base commit., Sets Answerability score to 100 and Status to Solved in OPEN_QUESTIONS.md, which is a reasonable but somewhat confident self-assessment that a human reviewer should confirm covers all edge cases (e.g., concurrency limit >1 catch-up behavior) before treating the question as fully closed. |

## Selected Summary

Resolve UBU-Q0137 with UBU-D0264, defining the Phase 1b unattended advisory batch runner as a narrow event-sourced BackgroundProcess with per-object pass state, candidate-only output, prompt-injection containment, operator-visible telemetry, and a clean no-work exit contract.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Introduces a new decision UBU-D0264 and cross-references several prior decisions (D0263, D0255, D0258, D0262) whose exact current wording could not be independently verified in this review, so there is some risk of subtle inconsistency with those sections as they exist on the base commit.
- Sets Answerability score to 100 and Status to Solved in OPEN_QUESTIONS.md, which is a reasonable but somewhat confident self-assessment that a human reviewer should confirm covers all edge cases (e.g., concurrency limit >1 catch-up behavior) before treating the question as fully closed.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260918T125514Z-UBU-Q0137/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260918T125514Z-UBU-Q0137/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260918T125514Z-UBU-Q0137"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
