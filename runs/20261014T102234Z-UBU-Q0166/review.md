# Model-Committee Review

Run: `20261014T102234Z-UBU-Q0166`  
Question: `UBU-Q0166`  
Base commit: `966272064f78963fe6d3e9fe89e4c57709789358`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0166-native-admission-20261014`

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
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0166-native-admission-20261014` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0166-native-admission-20261014` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0166-native-admission-20261014` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0166-native-admission-20261014` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0166-native-admission-20261014` | `openai` | `codex` | yes | 89 | Provides useful bounded progress on Q0166's admission evidence. Independent checks confirmed clean patch application, unchanged question metadata, matching host revisions and artifact hashes, and the recorded candidate and decision comparisons. Changes remain within the three allowed documentation files. The proposal accurately distinguishes native admission success from qualifier preservation, retains the enablement gates and makes no unsupported schema-gap or resolution claim. | None | Harness and result artifacts remain temporary; hashes identify them but provide limited durable auditability., Evidence covers synthetic native admission controls. Complete qualifier mappings, evidence resolution, rendered review, policy validation and authorized exchange remain unproven; inline qualifier loss remains unrepaired. |
| `UBU-Q0166-native-admission-20261014` | `openai` | `claude` | yes | 72 | Patch applies cleanly, passes allowlist and question-schema checks, and touches only DECISIONS.md, DESIGN.md and OPEN_QUESTIONS.md. It is directly responsive to the question's stated remaining work (round-trip evidence through actual store/review/admission paths) and follows the established incremental pattern of D0323/D0334/D0360: adds one decision, appends one Current-direction paragraph and one Resolution paragraph, keeps Status: Open and Resolved by: None, and makes no schema, pin, migration or adapter claim. Internal counts are consistent (6 inputs + 6 enqueued + 5 admitted = 17; sixth control is the stale rejection). The new finding, that native enqueue and admission neither restore the lost inline qualifiers nor validate unresolved reference tokens, is genuinely new and correctly scoped as a service-enforcement limitation rather than a governing gap. Score is held below the high range because the evidence is unreproducible from the repo (scratch harness, non-deterministic admission envelopes), the proposal explicitly repairs nothing despite the question's direction to repair or validate the vet_result branch before relying on it, and the series is now accumulating decisions without converging on closure. No fixes are required for acceptance; committing the harness as a test or a regeneration recipe would materially improve auditability and is recommended for the next step in this chain. | None | All runtime evidence rests on an uncommitted scratch harness identified only by SHA-256 hashes; reviewers cannot reproduce the six controls from the repository, and the text itself concedes these are not committed host tests., This is the fourth consecutive evidence-only decision on UBU-Q0166 (D0323, D0334, D0360, now D0366). It repairs nothing and establishes no schema gap, so the question is no closer to its stated close condition; DESIGN.md §21.1.1 is accumulating lab-notebook detail that may belong in a test log rather than the design contract., Controls call advisory_service::run_advisory and admit_candidate directly, bypassing authenticated HTTP and rendered operator review, so the admission-path finding is bounded to the service layer (acknowledged in the text, but easy to over-read)., Native admission uses non-deterministic UTC envelope times and generated actor/Device/key refs, so the recorded results-JSON hash is not a reproducibility anchor., The decision adds normative language ('A future adapter must preserve and validate ... then recheck ... at admission') inside what is framed as an evidence record; this is consistent with existing D0320/D0321 gates but slightly expands the decision's stated purpose., Claims about run outcomes (17 accepted fixtures, two expected rejections, stale-control rejection message) cannot be independently verified from the patch alone. |

## Selected Summary

Record pinned native enqueue/admission controls showing silent inline qualifier loss and admission with unresolved reference carriers.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- All runtime evidence rests on an uncommitted scratch harness identified only by SHA-256 hashes; reviewers cannot reproduce the six controls from the repository, and the text itself concedes these are not committed host tests.
- Claims about run outcomes (17 accepted fixtures, two expected rejections, stale-control rejection message) cannot be independently verified from the patch alone.
- Controls call advisory_service::run_advisory and admit_candidate directly, bypassing authenticated HTTP and rendered operator review, so the admission-path finding is bounded to the service layer (acknowledged in the text, but easy to over-read).
- Evidence covers synthetic native admission controls. Complete qualifier mappings, evidence resolution, rendered review, policy validation and authorized exchange remain unproven; inline qualifier loss remains unrepaired.
- Harness and result artifacts remain temporary; hashes identify them but provide limited durable auditability.
- Native admission uses non-deterministic UTC envelope times and generated actor/Device/key refs, so the recorded results-JSON hash is not a reproducibility anchor.
- The decision adds normative language ('A future adapter must preserve and validate ... then recheck ... at admission') inside what is framed as an evidence record; this is consistent with existing D0320/D0321 gates but slightly expands the decision's stated purpose.
- This is the fourth consecutive evidence-only decision on UBU-Q0166 (D0323, D0334, D0360, now D0366). It repairs nothing and establishes no schema gap, so the question is no closer to its stated close condition; DESIGN.md §21.1.1 is accumulating lab-notebook detail that may belong in a test log rather than the design contract.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261014T102234Z-UBU-Q0166/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261014T102234Z-UBU-Q0166/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261014T102234Z-UBU-Q0166"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
