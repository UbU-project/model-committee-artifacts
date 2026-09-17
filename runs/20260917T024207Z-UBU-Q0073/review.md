# Model-Committee Review

Run: `20260917T024207Z-UBU-Q0073`  
Question: `UBU-Q0073`  
Base commit: `421a51d420aaf2db8da85d71e7769631559ac9b2`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0073-P-gpt5-codex-001`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 89.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `UBU-Q0073-P-gpt5-codex-001` |
| `work-score` | `claude` | `sonnet` | `UBU-Q0073-P-gpt5-codex-001` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `UBU-Q0073-P-gpt5-codex-001` |
| `work-score` | `claude` | `sonnet` | `UBU-Q0073-P-gpt5-codex-001` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0073-P-gpt5-codex-001` | `openai` | `codex` | yes | 92 | The patch applies, stays within the allowed documentation files, resolves the selected question directly, and gives concrete MVP schemas and rules for criticality, decision envelopes, severity, cached explanations, repair recipes, last-legitimate Plan storage, and ask/silent/wait behavior. It is well scoped to Phase 1b mobile stewardship and avoids turning mobile into a global optimizer. Remaining risks are mostly precision issues in a few schema fields rather than blockers. | None | Some schema fields remain semi-open, especially disposal_effect as enum-or-string and repair_recipe.eligible_when/steps/ask_policy/fallback, which may need later tightening before implementation., The decision envelope wording for movable_windows says begin or occupy time depending on duration fields, which is compact but could produce inconsistent implementer interpretations., The conflict severity reconciliation with UBU-Q0134 is stated as a mapping requirement rather than enumerating projection conflict classes, so follow-up may be needed when Q0134 is resolved or edited. |
| `UBU-Q0073-P-gpt5-codex-001` | `openai` | `claude` | yes | 86 | Mechanical validation confirms the patch applies cleanly and touches only OPEN_QUESTIONS.md, DECISIONS.md, and DESIGN.md, all plausible targets for resolving a data-model question. The proposal answers all seven subquestions concretely: three-level criticality, decision envelope fields, four-level conflict severity shared with projection conflicts, cached explanation fragment set, five MVP repair recipes, last-legitimate-Plan storage/comparison, and ask/silent/wait rules. It updates the OPEN_QUESTIONS.md entry status, resolved_by, and answerability score consistently with the new decision, and DESIGN.md §16.5/16.9 references are updated to match. Scope stays bounded to what Phase 1b requires (mobile stewardship metadata) without drifting into unrelated global-optimization or learned-policy work, which it explicitly defers post-MVP. The main soft risk is schema density making future audit harder, but that is proportionate to the question's genuine scope. | None | Very dense, large schema addition in a single decision (UBU-D0256) increases review burden and risk of internal inconsistency slipping through, Introduces many new nested field names across DECISIONS.md and DESIGN.md that future questions/answers must stay consistent with, Conflict severity vocabulary unification with UBU-Q0134 projection conflicts is asserted but UBU-Q0134 itself is not shown as updated/cross-checked in this diff |

## Selected Summary

Resolve UBU-Q0073 with a compact Phase 1b mobile stewardship metadata schema covering criticality, decision envelopes, conflict severity, cached explanations, last-legitimate Plan comparison, required MVP repair recipes, and ask/silent/wait rules.

## Changed Files

- `OPEN_QUESTIONS.md`
- `DECISIONS.md`
- `DESIGN.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Conflict severity vocabulary unification with UBU-Q0134 projection conflicts is asserted but UBU-Q0134 itself is not shown as updated/cross-checked in this diff
- Introduces many new nested field names across DECISIONS.md and DESIGN.md that future questions/answers must stay consistent with
- Some schema fields remain semi-open, especially disposal_effect as enum-or-string and repair_recipe.eligible_when/steps/ask_policy/fallback, which may need later tightening before implementation.
- The conflict severity reconciliation with UBU-Q0134 is stated as a mapping requirement rather than enumerating projection conflict classes, so follow-up may be needed when Q0134 is resolved or edited.
- The decision envelope wording for movable_windows says begin or occupy time depending on duration fields, which is compact but could produce inconsistent implementer interpretations.
- Very dense, large schema addition in a single decision (UBU-D0256) increases review burden and risk of internal inconsistency slipping through

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260917T024207Z-UBU-Q0073/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260917T024207Z-UBU-Q0073/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260917T024207Z-UBU-Q0073"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
