# Model-Committee Review

Run: `20260526T192029Z-UBU-Q0046`  
Question: `UBU-Q0046`  
Base commit: `979db8d46c9f62291795f17fe4a2e4bb83c2df04`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0046-d0179-public-artifacts`

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
| `work-score` | `codex` | `gpt-5.5` | `codex-ubu-q0046-d0179-public-artifacts` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0046-d0179-public-artifacts` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `codex-ubu-q0046-d0179-public-artifacts` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0046-d0179-public-artifacts` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0046-d0179-public-artifacts` | `openai-codex` | `codex` | yes | 93 | The patch cleanly resolves UBU-Q0046 with a focused public artifact policy, mirrors it into DESIGN.md, adds a concrete accepted decision, and tombstones the open question in the expected compact form. It directly addresses committed versus linked or summarized artifacts, selected patch and commit-message presentation, safety boundaries for private reasoning and unsafe outputs, GitHub/PR linkage, and credible handling of failed or partial runs. Scope is limited to the expected design, decisions, and open-question files, with no unnecessary implementation churn. | None | The policy is intentionally high-level and leaves the exact redacted public manifest schema and artifact-safety check for future implementation., The statement that model-committee must not mark canonical questions solved could be read broadly unless understood as prohibiting automatic mutation rather than accepted human-reviewed patches. |
| `codex-ubu-q0046-d0179-public-artifacts` | `openai-codex` | `claude` | yes | 88 | Patch applies cleanly per mechanical validation. The proposal directly and fully addresses all six subquestions in UBU-Q0046: artifact selection, presentation of patches and commit messages, privacy/safety exclusions, credibility evidence, GitHub/PR linkage, and treatment of failed runs. DESIGN.md §3.1.3 and DECISIONS.md UBU-D0179 are consistent with each other and with the existing §3.1.x numbering convention. The tombstone in OPEN_QUESTIONS.md updates status to Solved, updates Resolved by to UBU-D0179, and replaces the body with a compact resolution note, which matches the expected tombstone pattern. No code files, configuration, or unrelated documentation is modified. The only meaningful gap is that the public manifest schema and artifact-safety check are explicitly deferred to future work — the policy is sound but partially unimplemented, which the decision acknowledges. No required fixes. | None | manifest.public.json schema and artifact-safety check are described in the policy but deferred to future implementation; a contributor following this policy has no schema to validate against yet., The tombstone body omits a blank line between the metadata line and 'Resolved. See UBU-D0179.' — minor formatting inconsistency that may not match other tombstone examples in the file., UBU-Q0063 is mentioned as a downstream dependent but not tracked in OPEN_QUESTIONS.md metadata, so the dependency may be invisible to the scoring loop. |

## Selected Summary

Adds UBU-D0179 to define the public model-committee dogfooding artifact policy, mirrors the policy in DESIGN.md, and tombstones UBU-Q0046 as solved.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- The policy is intentionally high-level and leaves the exact redacted public manifest schema and artifact-safety check for future implementation.
- The statement that model-committee must not mark canonical questions solved could be read broadly unless understood as prohibiting automatic mutation rather than accepted human-reviewed patches.
- The tombstone body omits a blank line between the metadata line and 'Resolved. See UBU-D0179.' — minor formatting inconsistency that may not match other tombstone examples in the file.
- UBU-Q0063 is mentioned as a downstream dependent but not tracked in OPEN_QUESTIONS.md metadata, so the dependency may be invisible to the scoring loop.
- manifest.public.json schema and artifact-safety check are described in the policy but deferred to future implementation; a contributor following this policy has no schema to validate against yet.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260526T192029Z-UBU-Q0046/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260526T192029Z-UBU-Q0046/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260526T192029Z-UBU-Q0046"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
