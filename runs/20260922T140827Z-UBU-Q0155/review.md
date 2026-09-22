# Model-Committee Review

Run: `20260922T140827Z-UBU-Q0155`  
Question: `UBU-Q0155`  
Base commit: `e4aceaf28ce6f8f59e8ff1ae6faa5a8418b32d0b`  
Automated selection: valid
Human review required: no
Selected proposal: `openai-gpt5-codex-UBU-Q0155-D0289`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 91.5
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `openai-gpt5-codex-UBU-Q0155-D0289` |
| `work-score` | `claude` | `sonnet` | `openai-gpt5-codex-UBU-Q0155-D0289` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `openai-gpt5-codex-UBU-Q0155-D0289` |
| `work-score` | `claude` | `sonnet` | `openai-gpt5-codex-UBU-Q0155-D0289` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `openai-gpt5-codex-UBU-Q0155-D0289` | `openai` | `codex` | yes | 93 | The proposal cleanly applies, touches only the expected design/decision/contract/question documents, and directly answers all UBU-Q0155 subquestions. It establishes the local-vs-blocking boundary, deterministic omission order, dependency deferral, additive `PlanningResponse.diagnostics.unplaced_tasks`, shared minor contract version, bounded horizon extension, risk/next-action surfacing, chunk-specific reasons, and splittable carry-forward semantics. Scope is appropriately limited to documentation and contract semantics, with only minor future-integration risks around enum/schema precision. | None | The contract text narrows `status = partial` around unplaced optional Dynamic work even though the enum may eventually need to cover other partial-response modes., The unplaced diagnostic enum and safe-alternative action list are useful but may need later schema alignment if generated schemas enforce the existing `safe_alternatives.action` vocabulary globally., The horizon-extension behavior says the CPU kernel may attempt one retry, which preserves policy discretion but leaves implementation thresholds mostly implicit. |
| `openai-gpt5-codex-UBU-Q0155-D0289` | `openai` | `claude` | yes | 90 | The patch applies cleanly per mechanical validation and touches only the expected canonical decision/design/contract documentation files. It substantively answers all seven subquestions: it draws a clear local/blocking boundary, defines deferral semantics (deferred_dependency, affected_dependent_task_refs), gives a deterministic selection order (value, then deadline/latest_finish, then id) with mandatory work protected, adds an additive PlanningResponse.diagnostics.unplaced_tasks field sharing the Phase 1b minor contract version with the split-policy change, adds a bounded horizon-extension retry step before deferral, specifies risk-report/next-action surfacing, and distinguishes chunk-size vs aggregate-capacity unplaced reasons while supporting splittable carry-forward. The OPEN_QUESTIONS.md resolution text is consistent with the new decision, and status/answerability fields are updated appropriately. It stays within the current question's scope without introducing implementation code, which is proper for a decision-record-only change of this kind. | None | Introduces a new contract minor version (0.2) shared with the UBU-Q0157 split-policy change; if that change lands differently or is rejected, this proposal's contract claim could become inconsistent and need reconciliation., The horizon_extension enum and selection_rank fields add moderate new surface area to UnplacedTaskDiagnostic that downstream implementers must build against before any reference implementation exists., Blocking/local boundary relies on judgment calls (e.g., 'required support Tasks' always blocking) that could be contested by future decisions without an explicit test or example to anchor them. |

## Selected Summary

Resolve partial placement by making optional Dynamic Task capacity failures local unplaced diagnostics while preserving blocking behavior for Static, structural, mandatory routine, support, and required-path failures.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`
- `PLANNING_KERNEL_CONTRACT.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Blocking/local boundary relies on judgment calls (e.g., 'required support Tasks' always blocking) that could be contested by future decisions without an explicit test or example to anchor them.
- Introduces a new contract minor version (0.2) shared with the UBU-Q0157 split-policy change; if that change lands differently or is rejected, this proposal's contract claim could become inconsistent and need reconciliation.
- The contract text narrows `status = partial` around unplaced optional Dynamic work even though the enum may eventually need to cover other partial-response modes.
- The horizon-extension behavior says the CPU kernel may attempt one retry, which preserves policy discretion but leaves implementation thresholds mostly implicit.
- The horizon_extension enum and selection_rank fields add moderate new surface area to UnplacedTaskDiagnostic that downstream implementers must build against before any reference implementation exists.
- The unplaced diagnostic enum and safe-alternative action list are useful but may need later schema alignment if generated schemas enforce the existing `safe_alternatives.action` vocabulary globally.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260922T140827Z-UBU-Q0155/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260922T140827Z-UBU-Q0155/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260922T140827Z-UBU-Q0155"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
