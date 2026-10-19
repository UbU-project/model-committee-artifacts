# Model-Committee Review

Run: `20261019T025838Z-UBU-Q0162`  
Question: `UBU-Q0162`  
Base commit: `af93fee4c29744d61df4f6c938464db5b80e472e`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0162-completion-consumer-audit`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 86.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-generate` | `claude` | `claude-fable-5-1` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0162-completion-consumer-audit` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0162-completion-consumer-audit` |
| `work-score` | `codex` | `gpt-6.1-sol` | `umc-ubu-q0162-correction-kind-shared-resolver-2026-10-18` |
| `work-score` | `claude` | `claude-fable-5-1` | `umc-ubu-q0162-correction-kind-shared-resolver-2026-10-18` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-generate` | `claude` | `claude-fable-5-1` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0162-completion-consumer-audit` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0162-completion-consumer-audit` |
| `work-score` | `codex` | `gpt-6.1-sol` | `umc-ubu-q0162-correction-kind-shared-resolver-2026-10-18` |
| `work-score` | `claude` | `claude-fable-5-1` | `umc-ubu-q0162-correction-kind-shared-resolver-2026-10-18` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0162-completion-consumer-audit` | `openai` | `codex` | yes | 96 | The documentation patch adds specific completion consumers and actionable regression checks, including the critical distinction between corrected assertions and already admitted Task effects. It stays within UBU-Q0162 and the allowed file, preserves metadata and Open status, and clearly distinguishes required checks from passing evidence. Supplied mechanical validation confirms clean application and schema preservation. | None | Static audit evidence does not establish runtime behavior; reproduction, a filed remediation ticket and passing implementation checks remain outstanding. |
| `UBU-Q0162-completion-consumer-audit` | `openai` | `claude` | yes | 76 | Patch applies, touches only the UBU-Q0162 block in OPEN_QUESTIONS.md, keeps Status: Open and the single-line metadata intact, and passes schema validation. The content is directly responsive to the question's own instruction to identify callers that treat raw history as current truth: it adds a dated static audit of a completion reader and its report/reopen/effect-suppression callers, flags a concrete repair hazard (hiding a withdrawn completion could permit a second effect application), extends the remediation handoff and required checks in the established style, and updates the Current direction and Resolution summaries consistently. It correctly marks everything as static evidence and keeps the question open. Score is held below the high range because the orchestrator-side claims are unverifiable here, the diff provenance looks hand-edited, and the proposal grows an already large ticket rather than moving toward the closure evidence the question actually needs. | None | The cited ubu-orchestrator symbols (calendar_interaction::latest_completion, report_service::time_by_category, log_service::reopen_completion/reopen_calendar_completion/record_task_action_with_calendar) and test files (tests/reports.rs, tests/calendar_interaction.rs, tests/task_reopen.rs fixture name) cannot be verified from the model-committee checkout; the proposal relies on its own validation note for existence at 9fddef16., The question block keeps accumulating consumer-specific audit paragraphs; each addition widens the single proposed remediation ticket and raises closure burden without advancing the still-outstanding reproduction, filed ticket or passing checks., The patch index line uses a 0000000 post-image blob hash, which is unusual for a modification; mechanical validation reports it applies, but it suggests the diff was hand-assembled rather than git-generated., The withdraw-then-recomplete regression requirement makes a normative choice (effects must not re-apply after a withdrawn completion) that is consistent with the existing 'correction does not undo Task effects' rule but is stated as a required check without a linked design decision. |
| `umc-ubu-q0162-correction-kind-shared-resolver-2026-10-18` | `anthropic` | `codex` | yes | 67 | Mechanical validation passes, the patch stays within UBU-Q0162, and it preserves Open status and the distinction between required checks and passing evidence. The payload contract and shared resolver advance the documentation handoff, but their proposed semantics contain ambiguities and a duration-specific contradiction that could produce divergent corrected views. These need correction before adopting the specification. | Define target-sensitive revocation semantics and make duration consumers use the resolved assertion. Revoking an annotation or correction must not automatically withdraw the underlying observation., Specify partial-replacement composition and nested revocation behavior with concrete expected-value fixtures., Include replacement-record links in semantic equivalence and define the admissible representation and resolution behavior for competing branches. | The duration rule says a revocation withdraws the observation, while the general rule says revoking a correction restores the earlier interpretation. These yield different evidence for the same history., Applying the last replacement's named fields over the original does not clearly specify whether earlier partial corrections survive., Correction equivalence compares kind and corrected values without explicitly accounting for linked replacement records, potentially collapsing distinct replacements., Conflict settlement permits targeting competing entries, but the proposed singular correction_of contract does not define how such a resolving correction identifies multiple branches. |
| `umc-ubu-q0162-correction-kind-shared-resolver-2026-10-18` | `anthropic` | `claude` | yes | 74 | Mechanical validation reports the patch applies, touches only OPEN_QUESTIONS.md, and passes the question schema; Status stays Open and the metadata line is untouched. The two inserted paragraphs sit inside the existing '### Source audit and remediation handoff' section and the Resolution line is updated consistently. The content is directly responsive: the accumulated remediation text already uses 'competing', 'uncontested', 'withdrawal', 'replacement' and 'friction-only correction' without defining them, and the ubu-store and ubu-orchestrator handoffs could diverge without a shared resolver contract. The proposal supplies internally consistent semantics (chain resolution, revocation-of-correction reverting to the prior uncontested member, identical competing corrections collapsing, differing ones marked contested and never timestamp-chosen, duration evidence changed only by replacements of duration-bearing fields) and keeps the no-behavior-change, required-checks-not-passing-results framing. It is honest that no source was read and that git apply --check was not run. Points are withheld because the design-authority claims are unverified, a possibly nonexistent `conflict_resolution` record is named, a schema contract is being recorded in question text rather than a decision, and the already oversized question grows further without new audit or reproduction evidence. Author identity was not used as evidence; the prompt lists the scoring provider as claude and the authoring provider as anthropic, so the proposal was scored as instructed. | Verify against DESIGN.md §12.1, UBU-D0071 and UBU-D0248 that the cited permissions and distinctions exist as described, and adjust wording if they do not., Confirm `conflict_resolution` is an existing admitted record type under UBU-D0248, or reword to 'the ordinary UBU-D0248 conflict-resolution record' without naming a specific event type. | The patch was authored without reading OPEN_QUESTIONS.md, DESIGN.md or DECISIONS.md. Its claims that UBU-D0071 leaves event-specific payload schemas to refinement, that DESIGN.md §12.1 already distinguishes replacement from revocation, and that UBU-D0248 forbids timestamp-only choice are plausible but unverified in this scoring session, which also had no file access., The resolver paragraph names a `conflict_resolution` record as a way to settle competing corrections. That identifier does not appear anywhere in the UBU-Q0162 text; if it is not an existing event type under UBU-D0248, the question would now reference a nonexistent mechanism., Introducing `correction_kind` with enumerated values and restricting `log_annotation_added` payloads to notes/reason/confidence/context is a data-model schema decision. Recording it only inside a question's remediation text, with no decision id, creates a shadow specification that DESIGN.md and DECISIONS.md do not carry; the question is marked Human approval required, so a reviewer must decide whether to promote it., The restriction that annotations carry no old/new values may conflict with UBU-D0071 if that decision lists optional old/new values for Log entries generally rather than for corrections only., The UBU-Q0162 block is already very long and this adds roughly two dense paragraphs of further required checks without any new revision-pinned audit finding or reproduction evidence. The outstanding closure items (reproduction, filed ticket, passing checks) are unchanged, so the increment refines the spec rather than advancing closure., 'Names each replaced field' assumes a flat field model for an arbitrary JSON payload; nested or array payloads leave the replacement semantics underspecified. |

## Selected Summary

Extend the Log remediation handoff to completion reports and reopen actions while preserving admitted Task effects.

## Changed Files

- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- 'Names each replaced field' assumes a flat field model for an arbitrary JSON payload; nested or array payloads leave the replacement semantics underspecified.
- Applying the last replacement's named fields over the original does not clearly specify whether earlier partial corrections survive.
- Conflict settlement permits targeting competing entries, but the proposed singular correction_of contract does not define how such a resolving correction identifies multiple branches.
- Correction equivalence compares kind and corrected values without explicitly accounting for linked replacement records, potentially collapsing distinct replacements.
- Introducing `correction_kind` with enumerated values and restricting `log_annotation_added` payloads to notes/reason/confidence/context is a data-model schema decision. Recording it only inside a question's remediation text, with no decision id, creates a shadow specification that DESIGN.md and DECISIONS.md do not carry; the question is marked Human approval required, so a reviewer must decide whether to promote it.
- Static audit evidence does not establish runtime behavior; reproduction, a filed remediation ticket and passing implementation checks remain outstanding.
- The UBU-Q0162 block is already very long and this adds roughly two dense paragraphs of further required checks without any new revision-pinned audit finding or reproduction evidence. The outstanding closure items (reproduction, filed ticket, passing checks) are unchanged, so the increment refines the spec rather than advancing closure.
- The cited ubu-orchestrator symbols (calendar_interaction::latest_completion, report_service::time_by_category, log_service::reopen_completion/reopen_calendar_completion/record_task_action_with_calendar) and test files (tests/reports.rs, tests/calendar_interaction.rs, tests/task_reopen.rs fixture name) cannot be verified from the model-committee checkout; the proposal relies on its own validation note for existence at 9fddef16.
- The duration rule says a revocation withdraws the observation, while the general rule says revoking a correction restores the earlier interpretation. These yield different evidence for the same history.
- The patch index line uses a 0000000 post-image blob hash, which is unusual for a modification; mechanical validation reports it applies, but it suggests the diff was hand-assembled rather than git-generated.
- The patch was authored without reading OPEN_QUESTIONS.md, DESIGN.md or DECISIONS.md. Its claims that UBU-D0071 leaves event-specific payload schemas to refinement, that DESIGN.md §12.1 already distinguishes replacement from revocation, and that UBU-D0248 forbids timestamp-only choice are plausible but unverified in this scoring session, which also had no file access.
- The question block keeps accumulating consumer-specific audit paragraphs; each addition widens the single proposed remediation ticket and raises closure burden without advancing the still-outstanding reproduction, filed ticket or passing checks.
- The resolver paragraph names a `conflict_resolution` record as a way to settle competing corrections. That identifier does not appear anywhere in the UBU-Q0162 text; if it is not an existing event type under UBU-D0248, the question would now reference a nonexistent mechanism.
- The restriction that annotations carry no old/new values may conflict with UBU-D0071 if that decision lists optional old/new values for Log entries generally rather than for corrections only.
- The withdraw-then-recomplete regression requirement makes a normative choice (effects must not re-apply after a withdrawn completion) that is consistent with the existing 'correction does not undo Task effects' rule but is stated as a required check without a linked design decision.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261019T025838Z-UBU-Q0162/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261019T025838Z-UBU-Q0162/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261019T025838Z-UBU-Q0162"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
