# Model-Committee Review

Run: `20261014T085913Z-UBU-Q0166`  
Question: `UBU-Q0166`  
Base commit: `37e41e0e0bc060d7b5eaed151d4459c13f57432f`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0166-openai-reference-carriers-20261014`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 80.5
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0166-openai-reference-carriers-20261014` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0166-openai-reference-carriers-20261014` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0166-openai-reference-carriers-20261014` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0166-openai-reference-carriers-20261014` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0166-openai-reference-carriers-20261014` | `openai` | `codex` | yes | 85 | Mechanically valid and confined to allowed documentation files. It advances the selected investigation with bounded normalization and persistence controls, identifies unresolved-reference acceptance, and clearly distinguishes carrier retention from semantic fidelity. Keeping the question Open and preserving existing enablement gates is appropriate. The score reflects useful partial evidence rather than completion of the full host-mapping work. | None | Opaque string retention does not demonstrate preservation of resolved claims, qualifiers, field bindings or evidence revisions., Results rely on a retained harness without a fresh locked build or canonical schema-validator invocation., Rendered review, admission and policy-authorized exchange remain untested; the prior inline qualifier loss remains unrepaired. |
| `UBU-Q0166-openai-reference-carriers-20261014` | `openai` | `claude` | yes | 76 | The patch applies cleanly, touches only the three allowlisted docs, leaves all question metadata intact and keeps UBU-Q0166 at Status: Open with Resolved by: None, which matches the question's own closure condition. It is directly responsive to the current direction, which asks for round-trip evidence through actual normalization, store/replay and review paths: it records a bounded reference-carrier alternative to the inline-payload mapping that UBU-D0334 showed to lose qualifiers, and it adds a useful negative control (an unresolved token also receives native ok), which correctly supports the conclusion that this branch cannot certify adapter resolution. Claims are carefully bounded: no manifest resolution, no qualifier content, no rendered review, no schema or pin change, and no assertion of a passing adapter or a governing schema gap. Deductions are for auditability (reliance on a retained harness with no fresh build and no in-repo fixture), the asymmetric coverage between the two tokens, and the fact that the work advances the inventory without materially narrowing the enablement gate. No schema or scope violations were found. | State where the retained harness source and binary are kept (path or artifact location) so the recorded SHA-256 identities can be verified rather than only asserted., Either run the missing-revision token through the same storage/reload/replay and API controls or make the table explicit that those rows cover the first token only. | Evidence is bound to a retained out-of-tree harness executable identified only by SHA-256; no fresh locked build, in-repo fixture or canonical schema-validator run backs the recorded outcomes, so the results are not independently reproducible from the patch alone., The persistence/replay and review API controls were run only on the first (resolvable-looking) token; the 'missing' token was tested through normalization only, so the negative control is narrower than the table layout suggests., Using field_provenance.preconditions as a string carrier could be misread later as an approved semantic binding despite the explicit disclaimer; the carrier choice is not grounded in any host field contract., Recorded hashes (harness, suppression key) and the exact serialized outcomes cannot be checked by a reviewer without access to the retained artifacts referenced in UBU-D0334., UBU-D0360 occupies a decision number for what is essentially evidence recording; this follows the D0323/D0334 convention but continues to grow DESIGN.md §21.1.1 with probe logs rather than contract text. |

## Selected Summary

Record bounded reference-carrier preservation through normalization, persistence/replay and direct review API reads; keep UBU-Q0166 open.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Evidence is bound to a retained out-of-tree harness executable identified only by SHA-256; no fresh locked build, in-repo fixture or canonical schema-validator run backs the recorded outcomes, so the results are not independently reproducible from the patch alone.
- Opaque string retention does not demonstrate preservation of resolved claims, qualifiers, field bindings or evidence revisions.
- Recorded hashes (harness, suppression key) and the exact serialized outcomes cannot be checked by a reviewer without access to the retained artifacts referenced in UBU-D0334.
- Rendered review, admission and policy-authorized exchange remain untested; the prior inline qualifier loss remains unrepaired.
- Results rely on a retained harness without a fresh locked build or canonical schema-validator invocation.
- The persistence/replay and review API controls were run only on the first (resolvable-looking) token; the 'missing' token was tested through normalization only, so the negative control is narrower than the table layout suggests.
- UBU-D0360 occupies a decision number for what is essentially evidence recording; this follows the D0323/D0334 convention but continues to grow DESIGN.md §21.1.1 with probe logs rather than contract text.
- Using field_provenance.preconditions as a string carrier could be misread later as an approved semantic binding despite the explicit disclaimer; the carrier choice is not grounded in any host field contract.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261014T085913Z-UBU-Q0166/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261014T085913Z-UBU-Q0166/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261014T085913Z-UBU-Q0166"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
