# Model-Committee Review

Run: `20261014T080833Z-UBU-Q0113`  
Question: `UBU-Q0113`  
Base commit: `33f0d132cde965e0237f7224edb0bc89c9c95605`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0113-friction-annotation-v1`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 88.5
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0113-friction-annotation-v1` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0113-friction-annotation-v1` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0113-friction-annotation-v1` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0113-friction-annotation-v1` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0113-friction-annotation-v1` | `openai` | `codex` | yes | 95 | Supplied mechanical validation confirms clean application, allowed files and preserved question schema, including Status: Solved. The proposal answers all six subquestions with a bounded optional schema, supported event types, append-only corrections, durable prompt suppression, review presentation and counterfactual links. It preserves duration evidence and clearly distinguishes qualitative friction context from statistical updates. Leaving Bayesian mathematics with existing UBU-Q0116 matches the stated post-MVP boundary. The detailed safeguards remain relevant to the selected work; no blocking fixes are evident. | None | The fixed overrun thresholds and schema limits may need adjustment after practical use., Concurrent offline devices may display duplicate prompts before suppression state synchronizes. |
| `UBU-Q0113-friction-annotation-v1` | `openai` | `claude` | yes | 82 | Patch passed mechanical validation and touches only the three allowlisted docs. OPEN_QUESTIONS.md keeps the full metadata line, sets Status: Solved and Resolved by: UBU-D0357, and leaves other question blocks untouched. All six subquestions get concrete answers: free-text only with taxonomy deferred (matching the stated current direction), supported event types enumerated, a clear non-override boundary for duration learning, a specified review report surface, an explicit and testable overrun threshold (1.5x and +300s with waiting/deferred time excluded), and linkage to decision_recorded for user-choice friction without replacing the Q0107 counterfactual record. Append-only correction semantics and once-per-completion counting are sound. Deductions are for unverifiable cross-references, the partial deferral of Bayesian interaction to UBU-Q0116, and a level of prescriptive detail beyond the minimum the question asked for. | None | Several cross-references cannot be verified from the prompt alone: UBU-D0198, UBU-Q0116 being post-MVP, DESIGN.md §17.0, and the event/field names task_completed, task_failed, plan_realized, log_annotation_added, log_correction_added, annotation_of, correction_of, decision_kind: alert_dismissed_or_ignored, preference_inference_allowed. If any of these do not exist as named, the new section would dangle., The proposal reframes the existing DESIGN.md sentence 'Friction notes feed the Bayesian duration learning system' into 'friction is qualitative review context that cannot alter distributions' and pushes all update mechanics to UBU-Q0116. This is a defensible answer to subquestion 3, but it partially defers the 'how does it interact with Bayesian learning' half of the question rather than answering it; if UBU-Q0116 is actually Phase 2 rather than post-MVP, the phase labelling in D0357 is wrong., Section is considerably more prescriptive than a 'minimum' schema: fixed 2048-char cap, eight-ref cap, sync coalescing, restart persistence, per-skip decision_recorded entries, and a fixtures paragraph. Numbers are arbitrary but bounded; the per-skip Log entry could add noise to the counterfactual log., New heading '### 17.8.1' uses the same level as '### 17.9', which may be inconsistent with how sub-subsections are numbered elsewhere in DESIGN.md., Decision number UBU-D0357 is asserted unique by the author but sequence correctness relative to the latest existing decision is not independently verified here. |

## Selected Summary

Define optional attributed friction notes, non-blocking overrun prompts and an evidence-preserving duration-learning boundary.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Concurrent offline devices may display duplicate prompts before suppression state synchronizes.
- Decision number UBU-D0357 is asserted unique by the author but sequence correctness relative to the latest existing decision is not independently verified here.
- New heading '### 17.8.1' uses the same level as '### 17.9', which may be inconsistent with how sub-subsections are numbered elsewhere in DESIGN.md.
- Section is considerably more prescriptive than a 'minimum' schema: fixed 2048-char cap, eight-ref cap, sync coalescing, restart persistence, per-skip decision_recorded entries, and a fixtures paragraph. Numbers are arbitrary but bounded; the per-skip Log entry could add noise to the counterfactual log.
- Several cross-references cannot be verified from the prompt alone: UBU-D0198, UBU-Q0116 being post-MVP, DESIGN.md §17.0, and the event/field names task_completed, task_failed, plan_realized, log_annotation_added, log_correction_added, annotation_of, correction_of, decision_kind: alert_dismissed_or_ignored, preference_inference_allowed. If any of these do not exist as named, the new section would dangle.
- The fixed overrun thresholds and schema limits may need adjustment after practical use.
- The proposal reframes the existing DESIGN.md sentence 'Friction notes feed the Bayesian duration learning system' into 'friction is qualitative review context that cannot alter distributions' and pushes all update mechanics to UBU-Q0116. This is a defensible answer to subquestion 3, but it partially defers the 'how does it interact with Bayesian learning' half of the question rather than answering it; if UBU-Q0116 is actually Phase 2 rather than post-MVP, the phase labelling in D0357 is wrong.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261014T080833Z-UBU-Q0113/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261014T080833Z-UBU-Q0113/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261014T080833Z-UBU-Q0113"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
