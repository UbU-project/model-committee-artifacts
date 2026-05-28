# Model-Committee Review

Run: `20260528T015837Z-UBU-Q0052`  
Question: `UBU-Q0052`  
Base commit: `8e9c1551daab2b74e9e27908360f834512f71a22`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0052-d0201`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 88.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0052-d0201` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0052-d0201` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0052-d0201` | `codex` | `claude` | yes | 88 | The proposal cleanly answers all seven subquestions of UBU-Q0052: it defines session lifecycle states, enumerates allowed and excluded Phase 1 signals, specifies admission rules for each override type, provides the required habit-pattern clarification prompt with explicit valid answers, gives reconciliation options for undetailed periods, and codifies sovereignty/anti-surveillance constraints. The patch applies cleanly, touches only the three allowed canonical files, introduces UBU-D0201 in DECISIONS.md with full provenance, and converts the OPEN_QUESTIONS.md entry to a compact solved tombstone anchored at its heading. Scope is appropriately minimal — it reuses existing Log, Snapshot, Preference, Task, Objective, and recalculation mechanisms rather than inventing a parallel ontology. The main weaknesses are: (1) minor label casing divergence between DESIGN.md and DECISIONS.md that could cause implementation confusion; (2) `decision_kind` values are named but not enumerated in any schema; (3) the tombstone formatting is slightly irregular. None of these block acceptance but should be addressed before the decision is treated as implementation-ready. | Reconcile label casing inconsistency: DESIGN.md §12.2 lists 'planned Task' and 'different Task' while DECISIONS.md UBU-D0201 lists `planned_task` and `different_task`; one form should be canonical., The tombstone in OPEN_QUESTIONS.md omits a blank line between the status line and the 'Resolved.' line, and the separator `---` follows immediately with no blank line before it; formatting should match surrounding question entries. | The OPEN_QUESTIONS.md tombstone strips the question body and subquestions entirely; if a future reader needs to understand what was asked without consulting git history, the compact tombstone may be insufficient., The habit-pattern clarification prompt is a hardcoded string in DESIGN.md and DECISIONS.md with slight wording divergence between the two locations ('more than once' vs same), which could cause inconsistency if either is updated independently., Admission rules for overrides use `decision_recorded` with a specific `decision_kind` value (`system_recommendation_overridden`), but no validation schema or enumeration for `decision_kind` is defined in this patch, leaving the contract partially underspecified., The review UI label list in DESIGN.md uses mixed snake_case and prose ('planned Task', 'different Task') while DECISIONS.md uses consistent snake_case (`planned_task`, `different_task`); this inconsistency should be resolved before implementation. |

## Selected Summary

Resolve UBU-Q0052 by defining Discovery mode as an opt-in, visible workflow state that stores narrow Phase 1 signals as reviewable evidence, admits actual actions and overrides through existing Log/Snapshot/Preference/Task/Objective/recalculation rules, and requires user clarification before habit-pattern inference.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Admission rules for overrides use `decision_recorded` with a specific `decision_kind` value (`system_recommendation_overridden`), but no validation schema or enumeration for `decision_kind` is defined in this patch, leaving the contract partially underspecified.
- The OPEN_QUESTIONS.md tombstone strips the question body and subquestions entirely; if a future reader needs to understand what was asked without consulting git history, the compact tombstone may be insufficient.
- The habit-pattern clarification prompt is a hardcoded string in DESIGN.md and DECISIONS.md with slight wording divergence between the two locations ('more than once' vs same), which could cause inconsistency if either is updated independently.
- The review UI label list in DESIGN.md uses mixed snake_case and prose ('planned Task', 'different Task') while DECISIONS.md uses consistent snake_case (`planned_task`, `different_task`); this inconsistency should be resolved before implementation.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260528T015837Z-UBU-Q0052/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260528T015837Z-UBU-Q0052/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260528T015837Z-UBU-Q0052"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
