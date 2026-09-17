# Model-Committee Review

Run: `20260917T113306Z-UBU-Q0134`  
Question: `UBU-Q0134`  
Base commit: `904b4c563317ce591c718c80ed14c6aa417d9d4a`  
Automated selection: valid
Human review required: no
Selected proposal: `openai-gpt-5-codex-UBU-Q0134-001`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 90.5
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `openai-gpt-5-codex-UBU-Q0134-001` |
| `work-score` | `claude` | `sonnet` | `openai-gpt-5-codex-UBU-Q0134-001` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `openai-gpt-5-codex-UBU-Q0134-001` |
| `work-score` | `claude` | `sonnet` | `openai-gpt-5-codex-UBU-Q0134-001` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `openai-gpt-5-codex-UBU-Q0134-001` | `openai` | `codex` | yes | 93 | The patch cleanly resolves UBU-Q0134 in the expected files, keeps the open-question schema intact, and directly answers all six subquestions. It formalizes the four projection quadrants, distinguishes duration edits from time-pinning, requires review for active external deletions, reuses the UBU-Q0073 severity vocabulary, requires first-class audit events for automatic repairs, and handles pending candidates conservatively. Scope is appropriately limited to the selected question and corresponding design/decision text. | None | The decision is necessarily policy-heavy and may require later implementation work to map `projection_conflict_event` fields to concrete storage/schema names., Some quadrant outcomes are conditional rather than table-like, so implementers must preserve the stated review gates carefully to avoid over-broad auto-resolution. |
| `openai-gpt-5-codex-UBU-Q0134-001` | `openai` | `claude` | yes | 88 | The proposal directly answers all six subquestions: it classifies the four quadrants, distinguishes duration edits from wall-clock time-pinning edits, treats confirmed deletion as review-required (not auto-resolvable) except for already-tombstoned/idempotent cases, adopts the shared advisory/minor/major/blocking severity vocabulary explicitly tied to UBU-D0256, defines an auditable projection_conflict_event record so automatic repairs are never silent, and specifies pending-candidate collision handling (collapse on exact duplicate, quarantine/review on overlap). It correctly respects the 'Current direction' scope note by explicitly excluding Device-to-Device sync conflicts and deferring them to UBU-Q0143. Only allowed files are touched, the OPEN_QUESTIONS.md single-line metadata format is preserved, and mechanical validation confirms the patch applies cleanly and only modifies the allowlisted files. This is a single well-formed proposal rather than a decomposition, so decomposition quality is not applicable. | None | References prior decisions UBU-D0248 and UBU-D0256 as established precedent; if those decisions do not exist or differ from the assumed content, the severity-vocabulary reuse and non-authority boundary claims would be inconsistent with the actual decision history, Bumps Answerability score from 90 to 100 and Status to Solved without an independent scoring pass, which is a self-assessed closure rather than an externally verified one, The projection_conflict_event schema and quadrant defaults introduce moderately detailed new data-model surface in a single decision; future work must not silently diverge from this vocabulary |

## Selected Summary

Resolve UBU-Q0134 by formalizing Phase 1b projection conflict quadrants, their auto-repair versus review gates, shared severities, audit events, and pending-candidate behavior.

## Changed Files

- `OPEN_QUESTIONS.md`
- `DESIGN.md`
- `DECISIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Bumps Answerability score from 90 to 100 and Status to Solved without an independent scoring pass, which is a self-assessed closure rather than an externally verified one
- References prior decisions UBU-D0248 and UBU-D0256 as established precedent; if those decisions do not exist or differ from the assumed content, the severity-vocabulary reuse and non-authority boundary claims would be inconsistent with the actual decision history
- Some quadrant outcomes are conditional rather than table-like, so implementers must preserve the stated review gates carefully to avoid over-broad auto-resolution.
- The decision is necessarily policy-heavy and may require later implementation work to map `projection_conflict_event` fields to concrete storage/schema names.
- The projection_conflict_event schema and quadrant defaults introduce moderately detailed new data-model surface in a single decision; future work must not silently diverge from this vocabulary

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260917T113306Z-UBU-Q0134/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260917T113306Z-UBU-Q0134/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260917T113306Z-UBU-Q0134"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
