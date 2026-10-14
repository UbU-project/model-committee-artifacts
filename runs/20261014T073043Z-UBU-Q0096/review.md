# Model-Committee Review

Run: `20261014T073043Z-UBU-Q0096`  
Question: `UBU-Q0096`  
Base commit: `c626e987550954584a2933ad53a7eb410a6648a9`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0096-openai-gpt-6-20261014`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 91.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0096-openai-gpt-6-20261014` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0096-openai-gpt-6-20261014` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0096-openai-gpt-6-20261014` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0096-openai-gpt-6-20261014` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0096-openai-gpt-6-20261014` | `openai` | `codex` | yes | 96 | Mechanical validation passes against the specified base commit. The proposal answers all five subquestions, preserves advisory defaults and independent authorization boundaries, and defines scoped, auditable exceptions without implying consent or safety. It changes only allowed documentation, correctly uses Status: Solved, and leaves detailed pacing/support and general safeguard UX to their existing questions. No blocking defects found. | None | Version-bound review and required-gate enforcement remain implementation obligations; the specified conformance fixtures have not been executed. |
| `UBU-Q0096-openai-gpt-6-20261014` | `openai` | `claude` | yes | 86 | The patch applies cleanly, touches only the three allowed files, and resolves UBU-Q0096 with Status: Solved and Resolved by: UBU-D0354 while preserving every other metadata field. All five subquestions are answered directly: the four types are retained with contexts as composable overlays (Q1), a per-type default matrix over eight named check codes (Q2), four context overlays with stated emphasis and union composition (Q3), any check configurable as a required gate via a versioned safeguard_policy with inspectable completion and exception authority (Q4), and bypass/disablement logged as counterfactual decision Logs with no preference inference and only authorized introspection use (Q5). It matches the question's 'current direction' of default-on advisory gates with explicit structural/external/user-required exceptions, keeps Phase 1 payload unchanged, and defers pacing/support detail and general safeguard UX to UBU-Q0099 and UBU-Q0101 instead of absorbing them. The four cross-reference edits in existing sections are necessary consistency updates, not scope creep. Deductions are for unverifiable decision/section references, the implicit 'base checks' mapping, and possible stale provisional notes elsewhere in DESIGN.md. | State the 'base' check set explicitly by code (e.g. introspection, extrospection, manipulation_risk, epistemic_transparency, graceful_nonachievement) in the matrix preamble so each row's default set is unambiguous., Before merge, grep DESIGN.md and DECISIONS.md for remaining 'UBU-Q0096' mentions and any 'provisional' annotations on review fields in the §22.3 list, and repoint them to §22.3.5/UBU-D0354. | Cross-references to UBU-D0155, UBU-D0156, UBU-D0165, UBU-D0273, §17.0 and §22.2.1, and the Log kinds `decision_recorded`/`safeguard_advisory_bypassed`, could not be verified from the prompt; if any is absent or named differently, the decision text would dangle., The per-type matrix uses the phrase 'Base checks' without mapping it explicitly to the eight check codes, leaving the exact default set for `unilateral` and `retroactive_scope_clarification` open to interpretation., The §22.3 field list at DESIGN.md line ~1452 is cut off in the patch context; if `review_checkpoints` or `safeguard_policy` are marked 'provisional pending UBU-Q0096' there, or if other sections still say UBU-Q0096 retains gate scope, those notes are now stale and were not updated., The new §22.3.5 is long and very dense; it is consistent with the neighboring 22.3.x sections' style but increases prompt-size and reader burden for later work on UBU-Q0099 and UBU-Q0101., `review_status` and `safeguard_policy` enum values and field sets are introduced here without a corresponding schema or fixture file change, so implementation conformance remains unspecified beyond prose. |

## Selected Summary

Finalize four transition types, per-type advisory reviews, context overlays, configurable required gates and scoped bypass records.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Cross-references to UBU-D0155, UBU-D0156, UBU-D0165, UBU-D0273, §17.0 and §22.2.1, and the Log kinds `decision_recorded`/`safeguard_advisory_bypassed`, could not be verified from the prompt; if any is absent or named differently, the decision text would dangle.
- The new §22.3.5 is long and very dense; it is consistent with the neighboring 22.3.x sections' style but increases prompt-size and reader burden for later work on UBU-Q0099 and UBU-Q0101.
- The per-type matrix uses the phrase 'Base checks' without mapping it explicitly to the eight check codes, leaving the exact default set for `unilateral` and `retroactive_scope_clarification` open to interpretation.
- The §22.3 field list at DESIGN.md line ~1452 is cut off in the patch context; if `review_checkpoints` or `safeguard_policy` are marked 'provisional pending UBU-Q0096' there, or if other sections still say UBU-Q0096 retains gate scope, those notes are now stale and were not updated.
- Version-bound review and required-gate enforcement remain implementation obligations; the specified conformance fixtures have not been executed.
- `review_status` and `safeguard_policy` enum values and field sets are introduced here without a corresponding schema or fixture file change, so implementation conformance remains unspecified beyond prose.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261014T073043Z-UBU-Q0096/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261014T073043Z-UBU-Q0096/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261014T073043Z-UBU-Q0096"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
