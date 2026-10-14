# Model-Committee Review

Run: `20261014T060556Z-UBU-Q0097`  
Question: `UBU-Q0097`  
Base commit: `ea50d7b0f13e20accdb21945b4d1f1e7dc1ca176`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-UBU-Q0097-step-autonomy-20261014`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 86.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `claude` | `claude-fable-5-1` | `codex-UBU-Q0097-step-autonomy-20261014` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `claude` | `claude-fable-5-1` | `codex-UBU-Q0097-step-autonomy-20261014` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-UBU-Q0097-step-autonomy-20261014` | `codex` | `claude` | yes | 86 | Mechanical validation passed (applies cleanly, allowlist OK, question schema OK). The OPEN_QUESTIONS.md hunk correctly sets `Status: Solved`, `Resolved by: UBU-D0347`, keeps metadata on one line and rewrites only the Resolution. All five subquestions are directly answered: a malformed-vs-user-controlled completion-test table; `counterparty_autonomy_acknowledgment` as a versioned, user-confirmed record with invariant codes rather than a Boolean; authentic refusal defined by attribution and scoped meaning with terminal closure via the existing §22.3.2 status mappings; one bounded clarification with cross-Task/Device communication accounting that prevents budget resets; and reopening only through a new transition/Objective with a versioned materially-new-evidence basis and stronger review. The DECISIONS.md entry is consistent with the DESIGN.md section and the preceding UBU-D0345/D0346 pattern, and the two cross-reference edits in §22.3.1 and §22.3.2 are minimal hygiene removing UBU-Q0097 from retained-scope lists. It stays documentation-only, adds no new questions, and explicitly bounds remaining scope to existing questions. Points deducted for density and some orchestration-level detail that goes beyond a data-model contract, which raises maintenance and consistency burden rather than correctness concerns. | None | DESIGN.md §22.3.3 is long and dense; the communication-slot reservation, idempotent transport retry and concurrent-Device dispatch material leans toward orchestration/sync detail for a decision typed as 'Data model', and may overlap with DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md and UBU-Q0099 pacing scope., Introduces many new named fields and enumerations (binding fields, `acknowledgment_id`, six invariant codes, `review_status`, `reopening_basis`, communication attempt purposes) without a consolidated field table; future proposals must keep these names consistent across §22.3.1–22.3.3., Cross-references to UBU-Q0101 and the §22.4 'stronger default advisory safeguards' cannot be verified from this prompt; the author's validation notes claim referenced IDs were checked., Historical decision text in UBU-D0345/UBU-D0346 still says Step/refusal validation 'retains remaining scope'; acceptable as a historical record but a reader following those decisions must also find §22.3.3., Allowing a user-attested report to suffice as authentic refusal (under evidence policy) is appropriate for autonomy but shifts attribution risk onto UBU-Q0092 typed evidence policy, which remains open. |

## Selected Summary

Define user-controlled Step/Task validation, explicit autonomy acknowledgment, scoped refusal closure, bounded clarification and evidenced reopening.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Allowing a user-attested report to suffice as authentic refusal (under evidence policy) is appropriate for autonomy but shifts attribution risk onto UBU-Q0092 typed evidence policy, which remains open.
- Cross-references to UBU-Q0101 and the §22.4 'stronger default advisory safeguards' cannot be verified from this prompt; the author's validation notes claim referenced IDs were checked.
- DESIGN.md §22.3.3 is long and dense; the communication-slot reservation, idempotent transport retry and concurrent-Device dispatch material leans toward orchestration/sync detail for a decision typed as 'Data model', and may overlap with DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md and UBU-Q0099 pacing scope.
- Historical decision text in UBU-D0345/UBU-D0346 still says Step/refusal validation 'retains remaining scope'; acceptable as a historical record but a reader following those decisions must also find §22.3.3.
- Introduces many new named fields and enumerations (binding fields, `acknowledgment_id`, six invariant codes, `review_status`, `reopening_basis`, communication attempt purposes) without a consolidated field table; future proposals must keep these names consistent across §22.3.1–22.3.3.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261014T060556Z-UBU-Q0097/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261014T060556Z-UBU-Q0097/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261014T060556Z-UBU-Q0097"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
