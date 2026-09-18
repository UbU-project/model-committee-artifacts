# Model-Committee Review

Run: `20260918T123925Z-UBU-Q0084`  
Question: `UBU-Q0084`  
Base commit: `b9ea1362d86344b7cb6476c888dbe72171f75f8c`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0084-d0263`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 87.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0084-d0263` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0084-d0263` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0084-d0263` | `codex` | `claude` | yes | 87 | The patch applies cleanly per mechanical validation and touches only DECISIONS.md, DESIGN.md, and OPEN_QUESTIONS.md, matching the allowlist. It directly answers all five subquestions: field split between AgentAction (per-dispatch) and BackgroundProcess (standing policy), trigger/recurrence/budget/notification/escalation representation, a concrete prompt-injection scoring rubric, rollback/mitigation metadata keyed to a side-effect taxonomy, and explicit Calendar-inclusion criteria. It correctly respects the existing Phase 1b split by leaving UBU-Q0137's advisory background-process subset untouched and only resolving the computer-use AgentAction/irreversible-effect/prompt-injection scope described as still open. OPEN_QUESTIONS.md's single-line metadata format is preserved and updated consistently (Status, Answerability, Resolved by, Resolution). No unnecessary files or scope creep. The main soft risk is that many new fields are introduced as prose lists without formal typing, which is consistent with the surrounding DESIGN.md style but adds surface area for future consistency checks. | None | Introduces a large number of new fields on AgentAction/BackgroundProcess without defining their types elsewhere in DESIGN.md, which could create drift if not cross-referenced by later schema work, Calendar-projection boundary ('reserve or require the user's attention... materially unavailable') is a judgment call that may need refinement once concrete background processes (e.g. UBU-Q0137's batch runner) are checked against it, Status change from Open to Solved and Answerability 90->100 assumes 'Solved' is the correct terminal status token used elsewhere in OPEN_QUESTIONS.md; not independently verified here |

## Selected Summary

Resolve UBU-Q0084 by accepting a split model: BackgroundProcess records durable unattended policy and scheduling, while AgentAction records each concrete external dispatch with authority, prompt-injection, budget, side-effect, evidence, and mitigation metadata.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Calendar-projection boundary ('reserve or require the user's attention... materially unavailable') is a judgment call that may need refinement once concrete background processes (e.g. UBU-Q0137's batch runner) are checked against it
- Introduces a large number of new fields on AgentAction/BackgroundProcess without defining their types elsewhere in DESIGN.md, which could create drift if not cross-referenced by later schema work
- Status change from Open to Solved and Answerability 90->100 assumes 'Solved' is the correct terminal status token used elsewhere in OPEN_QUESTIONS.md; not independently verified here

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260918T123925Z-UBU-Q0084/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260918T123925Z-UBU-Q0084/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260918T123925Z-UBU-Q0084"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
