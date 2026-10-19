# Model-Committee Review

Run: `20261019T022227Z-UBU-Q0162`  
Question: `UBU-Q0162`  
Base commit: `ae9addd02116c76bdc93625c665a6e46a64941e6`  
Automated selection: valid
Human review required: no
Selected proposal: `openai-UBU-Q0162-planning-consumer-20261018`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 84.5
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-UBU-Q0162-planning-consumer-20261018` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-UBU-Q0162-planning-consumer-20261018` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-UBU-Q0162-planning-consumer-20261018` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-UBU-Q0162-planning-consumer-20261018` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `openai-UBU-Q0162-planning-consumer-20261018` | `openai` | `codex` | yes | 95 | Mechanical validation passes, and changes remain confined to UBU-Q0162 in the allowed file. Inspection of the cited orchestrator revision confirms the direct Log query excludes correction events and feeds derived durations into planner requests. The patch adds precise remediation targets and meaningful acceptance checks covering correction chains, conflicts, replay, privacy and observation counts. It preserves Open status, human review requirements and the distinction between static findings and passing behavioral evidence. | None | Documentation only: the operator reproduction, filed remediation ticket and passing implementation checks remain outstanding. |
| `openai-UBU-Q0162-planning-consumer-20261018` | `openai` | `claude` | yes | 74 | Patch applies, touches only OPEN_QUESTIONS.md, stays inside UBU-Q0162 and preserves the single-line metadata including Status: Open. The addition is directly responsive to text already in the question: the read-side paragraph requires identifying callers that use raw history as current truth, and closure evidence requires relevant planning inputs to respect admitted links. Naming a concrete planning consumer with a pinned revision, affected symbols and a matching test-extension requirement follows the established audit pattern and is honest about being static evidence rather than a reproduction or passing result. The Current direction and Resolution summaries are updated consistently. Deductions for unverifiable cross-repo claims, the ambiguous ticket boundary between ubu-store and ubu-orchestrator, and the growing prescriptive test text that does not move the question toward closure. | State whether the ubu-orchestrator consumer work belongs to the ubu-store remediation ticket or to a separate ubu-orchestrator ticket linked to UBU-Q0162, since the existing text names the ticket as 'in ubu-store'. | The orchestrator audit claims (revision 9fddef16990ac89d07a168c75bf04b77c35b9bc3, observed_routine_events, build_request_from_store_with_context, duration_model::by_group/derive, tests/duration_models.rs) could not be independently verified during this cross-score; if any symbol or path is wrong the question would carry a false reference., The remediation ticket is described as living in ubu-store, but the new paragraph folds ubu-orchestrator planning changes into the 'same remediation handoff', creating a cross-repository ticket boundary that the text leaves ambiguous., The new test-requirements paragraph is dense and prescriptive (observation counting, minimum-observation fallback, friction-only corrections, learned-model review rules) and references behaviors ('existing minimum', 'existing review/admission requirements for learned model changes') that are not cited to a decision ID., The proposal adds another static-audit finding without advancing any of the three outstanding closure items (revision-pinned reproduction, filed ticket, passing checks); the question keeps accreting scope each round. |

## Selected Summary

Add the routine-duration planner's direct Log query to the correction-enforcement remediation scope.

## Changed Files

- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Documentation only: the operator reproduction, filed remediation ticket and passing implementation checks remain outstanding.
- The new test-requirements paragraph is dense and prescriptive (observation counting, minimum-observation fallback, friction-only corrections, learned-model review rules) and references behaviors ('existing minimum', 'existing review/admission requirements for learned model changes') that are not cited to a decision ID.
- The orchestrator audit claims (revision 9fddef16990ac89d07a168c75bf04b77c35b9bc3, observed_routine_events, build_request_from_store_with_context, duration_model::by_group/derive, tests/duration_models.rs) could not be independently verified during this cross-score; if any symbol or path is wrong the question would carry a false reference.
- The proposal adds another static-audit finding without advancing any of the three outstanding closure items (revision-pinned reproduction, filed ticket, passing checks); the question keeps accreting scope each round.
- The remediation ticket is described as living in ubu-store, but the new paragraph folds ubu-orchestrator planning changes into the 'same remediation handoff', creating a cross-repository ticket boundary that the text leaves ambiguous.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261019T022227Z-UBU-Q0162/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261019T022227Z-UBU-Q0162/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261019T022227Z-UBU-Q0162"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
