# Model-Committee Review

Run: `20261014T081852Z-UBU-Q0160`  
Question: `UBU-Q0160`  
Base commit: `c39ea0ad1c7d177fa161ac42318c663545a057fb`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0160-mobile-stewardship-comparison`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 85.5
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0160-mobile-stewardship-comparison` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0160-mobile-stewardship-comparison` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0160-mobile-stewardship-comparison` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0160-mobile-stewardship-comparison` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0160-mobile-stewardship-comparison` | `openai` | `codex` | yes | 95 | Mechanical validation passes, changes stay within the three allowed files, and Q0160 correctly remains Open with Resolved by: None. Repository context confirms D0339's capture allocation. The proposal directly addresses workload framing and defines an auditable comparison covering actual acceleration, exact parity, complete certification costs, concurrent capture load and fallback. It preserves planning schemas and CPU authority without claiming measurements or selecting an unsupported API. | None | Stewardship retention requires human approval; the harness, numerical limits and qualifying device measurements remain outstanding. |
| `UBU-Q0160-mobile-stewardship-comparison` | `openai` | `claude` | yes | 76 | Patch applies, allowlist and question-schema checks pass, and UBU-Q0160 correctly stays Open with Resolved by: None while being marked partially answered. The proposal is directly responsive: it answers subquestions 5 and 6 (stewardship framing survives; interaction surface and planning compute location are separate) and advances subquestions 1 through 4 by specifying a pre-registered same-device comparison with parity, certified-result latency, resource, thermal and fallback evidence, without inferring any hardware result or selecting an API. The pre-registration rule (no thresholds chosen after seeing a winner), the requirement to demonstrate actual accelerator execution rather than library loading, and the explicit 'no accelerator winner required' outcome are sound. Points withheld for unverifiable cross-references the decision depends on, for overwriting rather than preserving the original subquestion text, and for a verbose, implementation-detailed DESIGN.md block that may age poorly. | Confirm UBU-D0339 exists in DECISIONS.md and actually allocates mandatory phone capture/review, permits operator-owned inference workers, and resolves UBU-Q0183 as stated; adjust or drop the citation if not., Keep the original text of subquestions 5 and 6 and append the UBU-D0358 answer beneath each, instead of overwriting them., Obtain operator approval for retaining UBU-D0290's stewardship framing before merging, since the question's Current direction explicitly reserves that call for the operator. | The decision and the rewritten subquestion 6 rest on UBU-D0339 having resolved UBU-Q0183's capture/device allocation and sync sequence. That claim, and the DESIGN.md §4.2, §12.2, §21.1 and UBU-Q0168/UBU-Q0177 cross-references, cannot be verified from the supplied context; a wrong citation would make UBU-D0358's scope claim unsound., Subquestions 5 and 6 are replaced with answer text rather than retained with an appended answer, so the original question wording leaves the live record and auditability depends on git history., UBU-D0358 is logged as Accepted on a question marked Human approval required. The proposal flags requires_human_review, but if merged without operator sign-off it would retire the 'mobile interaction/workload roadmap' Blocks entry on the committee's say-so., The DESIGN.md §16.10.6 addition is long and highly prescriptive (four-row evidence matrix plus three paragraphs). It is on-topic, but it hard-codes protocol details (trial counts, cold/warm procedure, sustained duration) that the eventual harness owner may need to revise, which would then require another decision edit., The 'Shared phone budget' row requires concurrent camera/voice load testing against a route that may not yet exist on device, which could make the comparison gate unpassable until perception work lands, effectively coupling the planning API decision to perception sequencing despite the text saying the gates are separate. |

## Selected Summary

Retain mobile stewardship, acknowledge the settled phone capture sequence, and define a reproducible real-device API comparison while leaving API selection open.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Stewardship retention requires human approval; the harness, numerical limits and qualifying device measurements remain outstanding.
- Subquestions 5 and 6 are replaced with answer text rather than retained with an appended answer, so the original question wording leaves the live record and auditability depends on git history.
- The 'Shared phone budget' row requires concurrent camera/voice load testing against a route that may not yet exist on device, which could make the comparison gate unpassable until perception work lands, effectively coupling the planning API decision to perception sequencing despite the text saying the gates are separate.
- The DESIGN.md §16.10.6 addition is long and highly prescriptive (four-row evidence matrix plus three paragraphs). It is on-topic, but it hard-codes protocol details (trial counts, cold/warm procedure, sustained duration) that the eventual harness owner may need to revise, which would then require another decision edit.
- The decision and the rewritten subquestion 6 rest on UBU-D0339 having resolved UBU-Q0183's capture/device allocation and sync sequence. That claim, and the DESIGN.md §4.2, §12.2, §21.1 and UBU-Q0168/UBU-Q0177 cross-references, cannot be verified from the supplied context; a wrong citation would make UBU-D0358's scope claim unsound.
- UBU-D0358 is logged as Accepted on a question marked Human approval required. The proposal flags requires_human_review, but if merged without operator sign-off it would retire the 'mobile interaction/workload roadmap' Blocks entry on the committee's say-so.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261014T081852Z-UBU-Q0160/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261014T081852Z-UBU-Q0160/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261014T081852Z-UBU-Q0160"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
