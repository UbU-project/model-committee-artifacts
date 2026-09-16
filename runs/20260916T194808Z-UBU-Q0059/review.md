# Model-Committee Review

Run: `20260916T194808Z-UBU-Q0059`  
Question: `UBU-Q0059`  
Base commit: `f0b0f7c05994feb2db3aba5245aef801d2feb751`  
Automated selection: valid
Human review required: no
Selected proposal: `openai-gpt-5-codex-UBU-Q0059-001`

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
| `work-score` | `codex` | `gpt-5.5` | `openai-gpt-5-codex-UBU-Q0059-001` |
| `work-score` | `claude` | `sonnet` | `openai-gpt-5-codex-UBU-Q0059-001` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `openai-gpt-5-codex-UBU-Q0059-001` |
| `work-score` | `claude` | `sonnet` | `openai-gpt-5-codex-UBU-Q0059-001` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `openai-gpt-5-codex-UBU-Q0059-001` | `openai` | `codex` | yes | 93 | The patch is valid, scoped to the allowed documentation files, and directly resolves UBU-Q0059 with a coherent boundary: external execution is advisory, authority-scoped, least-disclosing, policy-gated, provenance-bearing, and CPU-certified on return. It also preserves the open-core self-hosting requirement and treats third-party providers as interchangeable rather than privileged. The proposal avoids implementation churn and unnecessary new decomposition while giving enough Phase 1b guidance for local advisory workers and future transport substitution. | None | The decision is broad and policy-heavy, so later implementation questions may need sharper protocol details for WorkerAuthority fields, payload-safety proof format, and sync-carried worker behavior., It gives brief directional answers for appliances, encrypted compute, and idle-compute monetization even though those remain future directions; this seems appropriate but should not be mistaken for a full product decision. |
| `openai-gpt-5-codex-UBU-Q0059-001` | `openai` | `claude` | yes | 86 | The proposal directly resolves UBU-Q0059 within its Phase 1b re-scope: it answers subquestions 1, 2, 3, and 5 (worker protocol posture, routing, Compartment/export gating, third-party interop) at the architecture level appropriate to Phase 1b, while correctly deferring subquestions 6, 7, and 8 as future/non-privileged, consistent with the question's stated current direction and re-scope note. It defines a WorkerAuthority envelope, an admission-before-mutation boundary, least-disclosing routing, payload minimization/provenance requirements, and an open-core self-host boundary, giving UBU-Q0135 and later remote-backend work a stable interface without duplicating that decomposed question's scope. The patch touches only OPEN_QUESTIONS.md, DECISIONS.md, and DESIGN.md, matches the existing status-line and decision-entry schema, and passed mechanical validation (patch_applies and allowlist_passed both true). It avoids scope creep by not committing to concrete appliance, FHE, or monetization designs. Minor concerns are the breadth of the new decision text and the unexplained answerability-score bump, but neither undermines correctness or auditability. | None | The new UBU-D0254 decision text is long and touches many subsystems (WorkerAuthority, admission, routing, payload minimization, certification); future decisions must stay consistent with all of these simultaneously or amend this one explicitly., Raising Answerability score from 90 to 100 alongside marking Status Solved is a judgment call not explicitly justified in the diff; reviewers may want to confirm this matches project convention for resolved questions., Subquestions 6, 7, 8 (appliances, FHE, monetization) are addressed only at a high policy level, which is correct per the question's own Phase 1b re-scope but leaves concrete design genuinely open for later work. |

## Selected Summary

Resolve UBU-Q0059 by defining all non-local execution providers as interchangeable advisory workers behind explicit WorkerAuthority, Compartment/export admission, payload minimization, provenance, user-visible routing, and CPU certification, while keeping the open-core planner self-hostable and treating appliances, encrypted compute, and monetization as optional future directions.

## Changed Files

- `OPEN_QUESTIONS.md`
- `DECISIONS.md`
- `DESIGN.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- It gives brief directional answers for appliances, encrypted compute, and idle-compute monetization even though those remain future directions; this seems appropriate but should not be mistaken for a full product decision.
- Raising Answerability score from 90 to 100 alongside marking Status Solved is a judgment call not explicitly justified in the diff; reviewers may want to confirm this matches project convention for resolved questions.
- Subquestions 6, 7, 8 (appliances, FHE, monetization) are addressed only at a high policy level, which is correct per the question's own Phase 1b re-scope but leaves concrete design genuinely open for later work.
- The decision is broad and policy-heavy, so later implementation questions may need sharper protocol details for WorkerAuthority fields, payload-safety proof format, and sync-carried worker behavior.
- The new UBU-D0254 decision text is long and touches many subsystems (WorkerAuthority, admission, routing, payload minimization, certification); future decisions must stay consistent with all of these simultaneously or amend this one explicitly.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260916T194808Z-UBU-Q0059/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260916T194808Z-UBU-Q0059/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260916T194808Z-UBU-Q0059"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
