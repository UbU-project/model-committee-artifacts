# Model-Committee Review

Run: `20261019T054047Z-UBU-Q0162`  
Question: `UBU-Q0162`  
Base commit: `4198fe6f6dbb4ba668639fcf2ff36c2fd0a71fe1`  
Automated selection: valid
Human review required: no
Selected proposal: `openai-ubu-q0162-planning-quality-20261019`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 90.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-generate` | `claude` | `claude-fable-5-1` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-ubu-q0162-planning-quality-20261019` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-ubu-q0162-planning-quality-20261019` |
| `work-score` | `codex` | `gpt-6.1-sol` | `umc-ubu-q0162-declared-correction-scope-20261019` |
| `work-score` | `claude` | `claude-fable-5-1` | `umc-ubu-q0162-declared-correction-scope-20261019` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-generate` | `claude` | `claude-fable-5-1` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-ubu-q0162-planning-quality-20261019` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-ubu-q0162-planning-quality-20261019` |
| `work-score` | `codex` | `gpt-6.1-sol` | `umc-ubu-q0162-declared-correction-scope-20261019` |
| `work-score` | `claude` | `claude-fable-5-1` | `umc-ubu-q0162-declared-correction-scope-20261019` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `openai-ubu-q0162-planning-quality-20261019` | `openai` | `codex` | yes | 96 | Mechanical validation confirms clean application, allowed-file scope and schema preservation. The patch directly extends the existing documentation handoff with specific planning-quality consumers, correction-aware classification, history-window handling, privacy constraints and concrete behavioral checks. It appropriately keeps UBU-Q0162 open and claims no implementation completion. | None | The added findings remain static-audit evidence; runtime reproduction, a filed remediation ticket and passing behavioral checks remain outstanding. |
| `openai-ubu-q0162-planning-quality-20261019` | `openai` | `claude` | yes | 84 | The patch applies cleanly, touches only OPEN_QUESTIONS.md, and passes schema validation with Status: Open retained, which is correct because closure still requires a revision-pinned reproduction, a filed ticket and passing checks. It follows the question's established pattern exactly: a dated static audit finding of another consumer of uncorrected history, a remediation handoff paragraph consistent with UBU-D0071 (preserve originals, resolve admitted links, no compensation from reads, explicit incomplete outcomes for contested or purged evidence), a required-checks paragraph explicitly marked as not passing results, and matching one-phrase updates to Current direction and Resolution. The finding is directly responsive to the existing instruction to identify callers using raw history as current truth, and the proposal makes no implementation or closure claims it cannot support. The main deduction is that the source references are unverifiable here and the question keeps growing without decomposition. | None | The static audit claims (src/reports/planning_analysis.rs symbols, the 50-row raw history limit, derive_reports, planning_service::generate persistence, tests/planning_o9.rs and the named unit test) cannot be verified from the scoring context; if any reference is wrong, the remediation handoff misdirects implementers., UBU-Q0162 continues to absorb every raw-history consumer as appended paragraphs; the block is already very large, and further growth raises readability and prompt-size concerns that may eventually call for splitting consumer-side handoffs into a separate implementation ticket., The acceptance text binds documentation to implementation details (keyword `overrun`, classification `wrong_estimates`, exact 50-row window) that can drift from source without the question being updated., 'Finite traversal budgets' for cross-window link discovery is left unspecified, so implementers must choose the bound without guidance. |
| `umc-ubu-q0162-declared-correction-scope-20261019` | `anthropic` | `codex` | yes | 70 | The patch provides useful documentation-level progress by declaring correction scope, naming shared-boundary validation and adding required checks. Mechanical validation passes, changes remain in the allowed question block, and the question correctly stays Open without claiming implementation evidence. However, its mandatory encoding requirements exceed the cited existing contract and need reconciliation with privacy, friction and legacy-history rules. | Make old/new value requirements policy-aware. Validate scope using permitted schema or metadata; insufficient retained evidence must produce an incomplete/review outcome without reconstructing removed content., Preserve UBU-D0357 friction annotation/correction semantics and explicitly define field-only withdrawal separately from whole-assertion withdrawal. Add compatible admission and corrected-view fixtures., Define handling of legacy unscoped corrections, imports and exact replays without modifying admitted history or receipts. | Mandatory target old values conflict with DESIGN.md §17.1's allowance to omit sensitive values and could require retaining or reconstructing purged content., The universal replacement format is not reconciled with DESIGN.md §17.8.1's existing event_payload.friction representation, including null for withdrawal. General note-only withdrawal also remains ambiguous., Consumers requiring declared scope have no specified treatment for previously admitted corrections lacking that declaration. |
| `umc-ubu-q0162-declared-correction-scope-20261019` | `anthropic` | `claude` | yes | 76 | Patch applied cleanly under mechanical validation, touches only OPEN_QUESTIONS.md inside the UBU-Q0162 block, keeps Status: Open and all single-line metadata intact, and adds no questions or decisions. It is directly responsive: the existing handoff already demands that duration, completion, reopen and human-complete consumers distinguish note-only, friction-only, duration and whole-outcome corrections, yet the minimum repair gave log_correction_added no scope representation, so each consumer would have to infer scope by payload diffing. Declaring scope in the admitted entry and validating it at the already-targeted append_prepared_log boundary closes that gap using the shared boundary the ticket already names, and the Resolution update is consistent with the body. Points are withheld for unverified citations of UBU-D0071 fields and the UBU-D0248 rule, the Snapshot-scope ambiguity, an unsupported new restriction on annotation payloads, and the density of a single long paragraph in an already oversized block. | Before merge, confirm against DESIGN.md and the decisions log that UBU-D0071 defines optional old/new value fields and that UBU-D0248 defines the non-overlapping field-level rule as described; reword or drop those citations if they do not match., Clarify whether a Snapshot-replacement link is a replacement-scope variant or a distinct third scope so the 'exactly one declared scope' validation rule is unambiguous., Consider splitting the single paragraph into scope definition, writer validation, consumer classification and required fixtures for readability. | The paragraph relies on UBU-D0071 having optional old/new value fields and on UBU-D0248 having a non-overlapping field-level rule with invariant and policy-input conditions. The author had no readable checkout, so both citations are unverified; if either is inaccurate the handoff would specify a payload shape or composition rule that the decisions do not actually define., Internal ambiguity: the text requires exactly one declared scope (withdrawal or named-field replacement) but then says a Snapshot replacement declares scope by linking a new Snapshot, which reads as a third form. Implementers could disagree on how to validate that case at append_prepared_log., The new constraint that log_annotation_added may carry only notes, reason, confidence or evidence references is a fresh restriction on annotation payloads with no source audit behind it and could conflict with existing annotation uses., The addition is one very dense ~2,000-character paragraph appended to an already very long question block, increasing prompt size and reducing auditability of the requirement list., Although framed as a clarification, declaring a mandatory correction-scope payload shape is effectively a data-model rule; recording it only inside the question handoff rather than as a decision may be missed by implementers reading UBU-D0071 alone. |

## Selected Summary

Extend the existing Log-correction remediation handoff to planning-quality advice and its raw 50-row history limit.

## Changed Files

- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- 'Finite traversal budgets' for cross-window link discovery is left unspecified, so implementers must choose the bound without guidance.
- Although framed as a clarification, declaring a mandatory correction-scope payload shape is effectively a data-model rule; recording it only inside the question handoff rather than as a decision may be missed by implementers reading UBU-D0071 alone.
- Consumers requiring declared scope have no specified treatment for previously admitted corrections lacking that declaration.
- Internal ambiguity: the text requires exactly one declared scope (withdrawal or named-field replacement) but then says a Snapshot replacement declares scope by linking a new Snapshot, which reads as a third form. Implementers could disagree on how to validate that case at append_prepared_log.
- Mandatory target old values conflict with DESIGN.md §17.1's allowance to omit sensitive values and could require retaining or reconstructing purged content.
- The acceptance text binds documentation to implementation details (keyword `overrun`, classification `wrong_estimates`, exact 50-row window) that can drift from source without the question being updated.
- The added findings remain static-audit evidence; runtime reproduction, a filed remediation ticket and passing behavioral checks remain outstanding.
- The addition is one very dense ~2,000-character paragraph appended to an already very long question block, increasing prompt size and reducing auditability of the requirement list.
- The new constraint that log_annotation_added may carry only notes, reason, confidence or evidence references is a fresh restriction on annotation payloads with no source audit behind it and could conflict with existing annotation uses.
- The paragraph relies on UBU-D0071 having optional old/new value fields and on UBU-D0248 having a non-overlapping field-level rule with invariant and policy-input conditions. The author had no readable checkout, so both citations are unverified; if either is inaccurate the handoff would specify a payload shape or composition rule that the decisions do not actually define.
- The static audit claims (src/reports/planning_analysis.rs symbols, the 50-row raw history limit, derive_reports, planning_service::generate persistence, tests/planning_o9.rs and the named unit test) cannot be verified from the scoring context; if any reference is wrong, the remediation handoff misdirects implementers.
- The universal replacement format is not reconciled with DESIGN.md §17.8.1's existing event_payload.friction representation, including null for withdrawal. General note-only withdrawal also remains ambiguous.
- UBU-Q0162 continues to absorb every raw-history consumer as appended paragraphs; the block is already very large, and further growth raises readability and prompt-size concerns that may eventually call for splitting consumer-side handoffs into a separate implementation ticket.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261019T054047Z-UBU-Q0162/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261019T054047Z-UBU-Q0162/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261019T054047Z-UBU-Q0162"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
