# Model-Committee Review

Run: `20261014T055209Z-UBU-Q0095`  
Question: `UBU-Q0095`  
Base commit: `bf1a30d92564850b0e5759d0d6b2f0b00243cf37`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0095-openai-gpt-6-process-outcome-v1`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 93.5
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0095-openai-gpt-6-process-outcome-v1` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0095-openai-gpt-6-process-outcome-v1` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0095-openai-gpt-6-process-outcome-v1` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0095-openai-gpt-6-process-outcome-v1` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0095-openai-gpt-6-process-outcome-v1` | `openai` | `codex` | yes | 97 | Mechanical validation passes, and changes remain within the three allowed files. The proposal answers all five subquestions with user-controlled criteria, scoped response observations, evidence-bound assessments, existing terminal Objective statuses, and reviewed hypothesis updates. It preserves declaration history, counterparty autonomy, uncertainty, and advisory safeguards. UBU-Q0095 correctly becomes Solved without altering its remaining metadata. Adjacent questions retain their scope, and conformance fixtures are explicitly distinguished from executed implementation checks. | None | Implementation still depends on authentic-refusal validation and typed evidence-policy details reserved for UBU-Q0097 and UBU-Q0092. |
| `UBU-Q0095-openai-gpt-6-process-outcome-v1` | `openai` | `claude` | yes | 90 | Patch passes mechanical validation, touches only the three allowed files, and uses Status: Solved with Resolved by: UBU-D0346 while preserving all other metadata fields. It answers all five subquestions directly: a per-type criteria table for unilateral, exploratory, mutual and retroactive clarification; an explicit observation_kind vocabulary (accepted, declined, withdrawn, ambiguous, insufficient_evidence, no_response) with silence/coverage rules; a met/not_met/undetermined assessment that maps to existing §7.3 statuses so completed can coexist with declined or unresolved outcomes; explicit language that refusal is neither user failure nor a task_failed event; and a scoped, reviewed path from observations into Relationship hypotheses under UBU-D0273 without automatic declaration changes. Edits to existing DESIGN.md paragraphs are limited to replacing 'remain UBU-Q0095' pointers with the new §22.3.2 reference, which is necessary rather than scope creep. The decision correctly frames itself as a data-model contract, not shipped orchestration or passing fixtures. Deductions are for verbosity, unverifiable cross-references, and the possibility of a stale provisional note in the §22.3 field list. | None | The new §22.3.2 is long and dense; several cross-references (§22.3 single-proposal/one-clarification rule, §7.3 one-time statuses, §22.1.1 declaration path, UBU-D0273 review path) are asserted rather than visible in the diff and could not be independently verified from the supplied context., The §22.3 field bullet list for RelationshipScopeTransition (only the first two bullets are visible in the diff) may still carry 'provisional pending UBU-Q0095' style notes on process_success_criteria or outcome_observations that the patch does not update, leaving a stale marker., Introduces new record shapes (process_assessment, criterion_id, action_or_check, applicability_condition/closure_branch, evidence_requirements, observation ids with coverage/uncertainty) and a 'withdrawn' observation kind beyond the question's suggested enumeration; this is within a Data model decision but adds surface area that later taxonomy/validation questions (UBU-Q0096–Q0099) must stay consistent with., Validation notes indicate the diff was generated against a working tree with uncommitted OPEN_QUESTIONS.md edits rather than HEAD; mechanical validation passed, but the base-commit vs working-tree distinction is a provenance caveat., Decision text is at the limit of readability for a human approver; 'Human approval required' review will need to parse multiple nested dependencies to confirm it does not silently widen UBU-D0155/D0345 boundaries. |

## Selected Summary

Define evidence-bound process success, scoped counterparty response observations and terminal closure for RelationshipScopeTransition.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Decision text is at the limit of readability for a human approver; 'Human approval required' review will need to parse multiple nested dependencies to confirm it does not silently widen UBU-D0155/D0345 boundaries.
- Implementation still depends on authentic-refusal validation and typed evidence-policy details reserved for UBU-Q0097 and UBU-Q0092.
- Introduces new record shapes (process_assessment, criterion_id, action_or_check, applicability_condition/closure_branch, evidence_requirements, observation ids with coverage/uncertainty) and a 'withdrawn' observation kind beyond the question's suggested enumeration; this is within a Data model decision but adds surface area that later taxonomy/validation questions (UBU-Q0096–Q0099) must stay consistent with.
- The new §22.3.2 is long and dense; several cross-references (§22.3 single-proposal/one-clarification rule, §7.3 one-time statuses, §22.1.1 declaration path, UBU-D0273 review path) are asserted rather than visible in the diff and could not be independently verified from the supplied context.
- The §22.3 field bullet list for RelationshipScopeTransition (only the first two bullets are visible in the diff) may still carry 'provisional pending UBU-Q0095' style notes on process_success_criteria or outcome_observations that the patch does not update, leaving a stale marker.
- Validation notes indicate the diff was generated against a working tree with uncommitted OPEN_QUESTIONS.md edits rather than HEAD; mechanical validation passed, but the base-commit vs working-tree distinction is a provenance caveat.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261014T055209Z-UBU-Q0095/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261014T055209Z-UBU-Q0095/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261014T055209Z-UBU-Q0095"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
