# Model-Committee Review

Run: `20261014T091320Z-UBU-Q0168`  
Question: `UBU-Q0168`  
Base commit: `a6506e9a699d73b3e8282f74e879c461b90c5f14`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0168-openai-latency-acceptance-accounting`

## Disagreement Flags

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
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0168-openai-latency-acceptance-accounting` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0168-openai-latency-acceptance-accounting` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0168-openai-latency-acceptance-accounting` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0168-openai-latency-acceptance-accounting` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0168-openai-latency-acceptance-accounting` | `openai` | `codex` | yes | 95 | Mechanical validation confirms clean application, allowed files and preserved question schema. The patch directly strengthens latency qualification through frozen trials, complete request accounting, explicit stream coverage denominators and per-condition acceptance gates. It correctly retains Open status without claiming numerical profiles, measured performance or supported hardware. | None | Usefulness rubrics and minimum proportions remain unselected; operator review must prevent trivially permissive acceptance thresholds. |
| `UBU-Q0168-openai-latency-acceptance-accounting` | `openai` | `claude` | yes | 74 | Patch applies cleanly, touches only DECISIONS.md, DESIGN.md and OPEN_QUESTIONS.md, and keeps UBU-Q0168 single-line metadata with Status: Open and Resolved by: None. The content is directly responsive to the question's remaining gap (measured acceptance): it separates calibration from frozen acceptance trials, forbids post-hoc widening of ceilings, requires complete denominators including refusals and timeouts, and requires stream coverage over declared active time rather than cherry-picked windows. Those are genuine auditability improvements that prevent favorable completion-only percentiles from qualifying a profile. The proposal honestly claims no numbers, hardware or implementation and does not attempt to close the question. It loses points because it does not move toward the numerical profiles and real-device evidence the question actually needs, it adds considerable length to an already long §21.1.4, and some of its cross-references cannot be verified here. | Confirm UBU-D0331 and UBU-D0336 exist at the base commit and actually establish minimum trials/durations and deployed isolation costs as paraphrased; adjust wording if not., Confirm UBU-D0361 is unused in DECISIONS.md at the base commit. | Contract accretion: UBU-Q0168 already states that a parameterized contract alone cannot close it, and this adds a fifth methodology layer (after UBU-D0325, UBU-D0331, UBU-D0335, UBU-D0336) without any real-device evidence or numerical profile; repeated proposals of this kind leave the actual blocker untouched., Cross-references to UBU-D0331 ('minimum trials/durations') and UBU-D0336 ('deployed isolation costs') and the UBU-D0361 identifier are asserted by the author's validation notes but not independently verifiable from this prompt; a wrong paraphrase or ID collision would propagate into DESIGN.md., The new DESIGN.md §21.1.4 block is four dense paragraphs of accounting rules (retry budgeting, cohort immutability, independent-session coverage, time-denominator rules) that may be over-specified for a design contract and hard to apply consistently before any profile exists., Adds an operator-review-before-enablement process requirement that is not obviously anchored to an existing operator-review mechanism elsewhere in DESIGN.md. |

## Selected Summary

Require frozen latency acceptance trials, complete request outcomes and stream coverage accounting; retain UBU-Q0168 as Open.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Adds an operator-review-before-enablement process requirement that is not obviously anchored to an existing operator-review mechanism elsewhere in DESIGN.md.
- Contract accretion: UBU-Q0168 already states that a parameterized contract alone cannot close it, and this adds a fifth methodology layer (after UBU-D0325, UBU-D0331, UBU-D0335, UBU-D0336) without any real-device evidence or numerical profile; repeated proposals of this kind leave the actual blocker untouched.
- Cross-references to UBU-D0331 ('minimum trials/durations') and UBU-D0336 ('deployed isolation costs') and the UBU-D0361 identifier are asserted by the author's validation notes but not independently verifiable from this prompt; a wrong paraphrase or ID collision would propagate into DESIGN.md.
- The new DESIGN.md §21.1.4 block is four dense paragraphs of accounting rules (retry budgeting, cohort immutability, independent-session coverage, time-denominator rules) that may be over-specified for a design contract and hard to apply consistently before any profile exists.
- Usefulness rubrics and minimum proportions remain unselected; operator review must prevent trivially permissive acceptance thresholds.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261014T091320Z-UBU-Q0168/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261014T091320Z-UBU-Q0168/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261014T091320Z-UBU-Q0168"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
