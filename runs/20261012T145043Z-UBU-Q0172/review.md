# Model-Committee Review

Run: `20261012T145043Z-UBU-Q0172`  
Question: `UBU-Q0172`  
Base commit: `28dbdcccaa504603fa3a60d2a4210846f5d91666`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0172-openai-gpt-6-20261012`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 89.5
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0172-openai-gpt-6-20261012` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0172-openai-gpt-6-20261012` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0172-openai-gpt-6-20261012` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0172-openai-gpt-6-20261012` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0172-openai-gpt-6-20261012` | `openai` | `codex` | yes | 95 | Mechanical validation passes, changes stay within the three allowed files, and question metadata correctly uses Status: Solved. The contract directly addresses visibility, reliability, independent corroboration, operator knowledge, contradictions and revocation. It distinguishes state, transition and Task attribution; prevents inference laundering; and preserves unconditional whole-list completion and existing provenance. Behavioral fixtures are clearly implementation requirements, not claimed passing results. | None | Host preservation and enforcement of effect-evidence/1 remain unproven and gated by UBU-Q0166., Already-recorded non-idempotent effects may hold completion pending a supported reconciliation path. |
| `UBU-Q0172-openai-gpt-6-20261012` | `openai` | `claude` | yes | 84 | The patch applies cleanly, touches only the three allowlisted files, and changes only Status and Resolved by in UBU-Q0172's metadata with Status: Solved. It directly answers the question: §17.10 separates completion-compatible observations, native success assertions, scoped sufficient evidence and unresolved/contradicted/revoked assessments, and covers every item named in the current direction (visibility limits, source reliability, independent corroboration, operator confirmation, contradictory evidence, revocation, and no self-certification via Plans or summaries). It keeps consequential examples withheld, preserves FactProvenance per UBU-D0293, adds UBU-D0329 in the existing decision format, and fixes the two cross-references in §21.1 and §21.1.1 that would otherwise go stale. Deductions are for prose density, introduction of a new named profile and possibly new vocabulary that need human confirmation, and unverified reliance on §10.2/UBU-D0242 semantics. No decomposition was attempted and none was needed. Human approval is correctly flagged. | None | Section 17.10 is dense and long; many clauses per paragraph reduce auditability and make it harder for the human reviewer to verify each requirement independently., Introduces a new named companion profile `effect-evidence/1` with a required-content table. The text claims no new first-class object or kernel field, but the profile is still new design surface that should be confirmed against existing profile naming and payload conventions., Uses terms such as `admitted-frontier bindings`, `idempotency outcome` and `state_at_time`/`transition`/`task_caused_transition` scopes that could not be verified against existing DESIGN.md vocabulary from the diff alone; if they are new, they are undocumented terminology., Relies on §10.2 unconditional whole-list mutation semantics and UBU-D0242; correctness of the completion gate depends on those references being accurate, which was not independently checked here., Only two stale `UBU-Q0172 retains ...` cross-references in DESIGN.md were updated; other documents (for example MULTIMODAL_*.md) outside the allowlist may still describe the question as open., Required behavioral gates are stated as acceptance requirements; the decision could be misread as implemented unless reviewers keep the 'not passing results' caveat in view. |

## Selected Summary

Define scoped evidence for completion and effects, with separate gates for perceptual success, consequential reuse and later correction.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Already-recorded non-idempotent effects may hold completion pending a supported reconciliation path.
- Host preservation and enforcement of effect-evidence/1 remain unproven and gated by UBU-Q0166.
- Introduces a new named companion profile `effect-evidence/1` with a required-content table. The text claims no new first-class object or kernel field, but the profile is still new design surface that should be confirmed against existing profile naming and payload conventions.
- Only two stale `UBU-Q0172 retains ...` cross-references in DESIGN.md were updated; other documents (for example MULTIMODAL_*.md) outside the allowlist may still describe the question as open.
- Relies on §10.2 unconditional whole-list mutation semantics and UBU-D0242; correctness of the completion gate depends on those references being accurate, which was not independently checked here.
- Required behavioral gates are stated as acceptance requirements; the decision could be misread as implemented unless reviewers keep the 'not passing results' caveat in view.
- Section 17.10 is dense and long; many clauses per paragraph reduce auditability and make it harder for the human reviewer to verify each requirement independently.
- Uses terms such as `admitted-frontier bindings`, `idempotency outcome` and `state_at_time`/`transition`/`task_caused_transition` scopes that could not be verified against existing DESIGN.md vocabulary from the diff alone; if they are new, they are undocumented terminology.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261012T145043Z-UBU-Q0172/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261012T145043Z-UBU-Q0172/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261012T145043Z-UBU-Q0172"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
