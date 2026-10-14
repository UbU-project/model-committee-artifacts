# Model-Committee Review

Run: `20261014T093222Z-UBU-Q0178`  
Question: `UBU-Q0178`  
Base commit: `71ce3e69c21b2d015e8baae67fb825d5103242c6`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0178-first-use-acquisition-recovery`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 81.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0178-first-use-acquisition-recovery` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0178-first-use-acquisition-recovery` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0178-first-use-acquisition-recovery` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0178-first-use-acquisition-recovery` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0178-first-use-acquisition-recovery` | `openai` | `codex` | yes | 88 | Mechanical validation confirms clean application, allowed files and preserved question schema. The patch addresses a concrete affordability barrier: obtaining and installing models within declared budgets. Starting-state distinctions, helper costs, integrity checks and installation storage measurements are relevant and auditable. It consistently retains Open status and human review without inventing results. Its limitation is incremental protocol expansion rather than measured progress toward selecting a lower-cost path. | None | Adds an evidence requirement without supplying comparative real-device results or selecting an accessible multimodal path., One interrupted-acquisition trial provides limited evidence about recovery costs across varying connectivity conditions. |
| `UBU-Q0178-first-use-acquisition-recovery` | `openai` | `claude` | yes | 74 | Patch applies cleanly, touches only DECISIONS.md, DESIGN.md and the selected question block, and keeps Status: Open with the metadata line unchanged. The gap it closes is real for the target population: UBU-D0337's entry-cash limits do not capture data-transfer, temporary storage or helper costs of obtaining packages and weights from an absent starting state, and a prepared inference trial cannot stand in for that. The DESIGN.md text is auditable, ties to existing gates (UBU-D0331 integrity checks, frozen rubric, cost/support matrix) and explicitly refuses to select a configuration, price or architecture. Deductions: the question can only close on measured results, so this is another narrowing step whose marginal value is moderate; the DESIGN.md addition is dense and adds nontrivial trial burden; the recovery-frequency assumption weakens period estimates. No blocking fixes. | None | Third consecutive evidence-contract extension (after UBU-D0332 and UBU-D0337) with no measured result; risk of indefinite deferral by accumulating trial obligations rather than moving toward operator-reviewed real-device data., Per-route interrupted-acquisition trial plus first-use setup trial materially increases measurement burden on the comparison; cost of running the evidence program itself is not bounded., Period estimates now depend on an 'assumed frequency' of recovery events, an unverifiable assumption that can be tuned to make a route pass or fail., Introduces 'helper Device or human assistance' as a frozen trial input without defining how helper availability is scoped for the no-worker ownership scenario., Decision ID UBU-D0363 uniqueness is asserted only by the author's validation notes; not independently verified here. |

## Selected Summary

Require measured first-use installation and interrupted package/model acquisition recovery before selecting a lower-cost route for an explicit starting-state and ownership scenario.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Adds an evidence requirement without supplying comparative real-device results or selecting an accessible multimodal path.
- Decision ID UBU-D0363 uniqueness is asserted only by the author's validation notes; not independently verified here.
- Introduces 'helper Device or human assistance' as a frozen trial input without defining how helper availability is scoped for the no-worker ownership scenario.
- One interrupted-acquisition trial provides limited evidence about recovery costs across varying connectivity conditions.
- Per-route interrupted-acquisition trial plus first-use setup trial materially increases measurement burden on the comparison; cost of running the evidence program itself is not bounded.
- Period estimates now depend on an 'assumed frequency' of recovery events, an unverifiable assumption that can be tuned to make a route pass or fail.
- Third consecutive evidence-contract extension (after UBU-D0332 and UBU-D0337) with no measured result; risk of indefinite deferral by accumulating trial obligations rather than moving toward operator-reviewed real-device data.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261014T093222Z-UBU-Q0178/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261014T093222Z-UBU-Q0178/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261014T093222Z-UBU-Q0178"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
