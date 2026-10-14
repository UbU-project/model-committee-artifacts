# Model-Committee Review

Run: `20261014T065808Z-UBU-Q0085`  
Question: `UBU-Q0085`  
Base commit: `71f15e29d6f35d6cc7e8bd23fe23e74580c2ace1`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-UBU-Q0085-cockpit-v1`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 84.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `claude` | `claude-fable-5-1` | `codex-UBU-Q0085-cockpit-v1` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `claude` | `claude-fable-5-1` | `codex-UBU-Q0085-cockpit-v1` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-UBU-Q0085-cockpit-v1` | `codex` | `claude` | yes | 84 | Mechanical validation passed: the patch applies, touches only the three allowlisted files, and the question metadata changes exactly Status (Open → Solved) and Resolved by (None → UBU-D0351) while preserving every other field. All five subquestions are answered in a traceable way: §21.9.3 enumerates the nine requested surfaces with per-surface decision boundaries, §21.9.2 covers candidate vs admitted presentation with non-color-only cues and separated saving/admission/execution outcomes, §21.9.4 bounds generative UI to a closed validated widget vocabulary with controller-owned diffs and controls, §21.9.5 pins the Phase 1 minimum to the frozen §4.1.2 loop without new blockers, and the §21.9.1 inspector table addresses evidence, constraints, expected effects, authority/disclosure and recovery. The DECISIONS.md entry follows the existing format and explicitly disclaims new canonical objects, kernel fields or execution/capture authority, and requires_human_review is set, which fits the question's 'Human approval required' eligibility. Deductions are for unverifiable section and decision anchors, the implicit admitted/canonical terminology mapping, and the sheer density of prescriptive text for a Phase 2 product decision. | None | Dense cross-referencing to DESIGN.md sections (§4.1.2, §19.2, §21.1.1, §21.1.2, §21.2.1, §21.5, §27.3, §1) and decisions (UBU-D0129, UBU-D0142, UBU-D0272, UBU-D0342) that cannot be verified from this prompt; a wrong anchor would leave dangling references in a canonical doc., The proposal uses 'admitted' where the question asks about the candidate/canonical distinction. The intent is clear and deliberately nuanced (Replica-scoped, not global agreement), but the term mapping is never stated explicitly, which could cause terminology drift in later decisions., §21.9 grows by roughly 75 very prescriptive lines for a Post-MVP Phase 2 product question. The level of operational detail (recheck lists, batch rules, widget vocabulary constraints) risks over-constraining implementation before any cockpit slice exists, though the text repeatedly scopes itself as a presentation contract rather than a kernel or schema change., §21.9.6 adds acceptance-evidence requirements beyond the five subquestions. This matches the pattern visible in adjacent DECISIONS.md entries, but it widens what a future 'delivered' claim must satisfy. |

## Selected Summary

Define a first-person state-transition cockpit with a shared inspector, nine workflow surfaces, explicit candidate/admitted labels, bounded generative presentation and operation-specific review controls.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Dense cross-referencing to DESIGN.md sections (§4.1.2, §19.2, §21.1.1, §21.1.2, §21.2.1, §21.5, §27.3, §1) and decisions (UBU-D0129, UBU-D0142, UBU-D0272, UBU-D0342) that cannot be verified from this prompt; a wrong anchor would leave dangling references in a canonical doc.
- The proposal uses 'admitted' where the question asks about the candidate/canonical distinction. The intent is clear and deliberately nuanced (Replica-scoped, not global agreement), but the term mapping is never stated explicitly, which could cause terminology drift in later decisions.
- §21.9 grows by roughly 75 very prescriptive lines for a Post-MVP Phase 2 product question. The level of operational detail (recheck lists, batch rules, widget vocabulary constraints) risks over-constraining implementation before any cockpit slice exists, though the text repeatedly scopes itself as a presentation contract rather than a kernel or schema change.
- §21.9.6 adds acceptance-evidence requirements beyond the five subquestions. This matches the pattern visible in adjacent DECISIONS.md entries, but it widens what a future 'delivered' claim must satisfy.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261014T065808Z-UBU-Q0085/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261014T065808Z-UBU-Q0085/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261014T065808Z-UBU-Q0085"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
