# Model-Committee Review

Run: `20261014T053030Z-UBU-Q0087`  
Question: `UBU-Q0087`  
Base commit: `3082787323c189d6be5efa9949c6feea38625149`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0087-openai-gpt-6-declaration-history-v1`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 91.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0087-openai-gpt-6-declaration-history-v1` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0087-openai-gpt-6-declaration-history-v1` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0087-openai-gpt-6-declaration-history-v1` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0087-openai-gpt-6-declaration-history-v1` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0087-openai-gpt-6-declaration-history-v1` | `openai` | `codex` | yes | 96 | Mechanical validation confirms clean application, allowed files and preserved question schema. The proposal answers all five subquestions with explicit declaration fields, scoped supersession, temporal evidence comparison, user-reviewed evolution and transition links. It preserves Phase 1 behavior and leaves adjacent thresholds, evidence policies and transition lifecycle questions open. No required documentation fixes are evident. | None | Partial supersession and causal historical views require careful implementation; the specified conformance fixtures have not been executed. |
| `UBU-Q0087-openai-gpt-6-declaration-history-v1` | `openai` | `claude` | yes | 86 | Mechanical validation passed (patch applies, allowlist and question schema OK). The proposal directly answers all five subquestions: an explicit field list for ownDeclaredPerspectiveHistory; clear coexist-versus-supersede rules (explicit user-authorized, scoped to named overlap, desired never replaces current, withdrawal and correction append rather than mutate); drift detection via comparison against the declaration applicable at the behavior's time with affect and counterevidence shown separately and unresolved comparisons made explicit; transition linkage via baseline declaration versions plus a separately admitted resulting declaration, with no automatic scope change on Objective completion or refusal; and a user-review based distinction between reviewed evolution and candidate unnoticed creep. Question metadata stays single-line with Status: Solved and Resolved by: UBU-D0344, scores and dates untouched. Scope is well contained: Phase 1 is explicitly left unchanged and thresholds, typed evidence enums, full transition schema and process/outcome detail are deferred to Q0089, Q0092, Q0094 and Q0095 rather than pulled in. No new questions were added, which is appropriate given answerability 90. Points withheld for auditability of the dense single hunk and for cross-references I cannot verify against the base commit. | None | Cross-references cannot be verified from this prompt: UBU-D0076, UBU-D0153, UBU-D0154, UBU-D0273, DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §§9 and 17, and the claim that UBU-D0344 is the next unused decision id. A wrong or already-allocated id would produce a dangling or colliding reference., DESIGN.md §22.1.1 is a single ~27-line hunk of very dense normative prose. It is correct in substance but hard to audit line by line, and some sentences bundle several distinct rules (e.g., rejection of cyclic refs, staging of unavailable refs, replay dedup) that a reviewer may want split or cross-linked to existing admission rules., Introduces a new named artifact `ScopeAssessment` and the `current_participation`/`desired_participation` distinction inline. These are reasonable but are not anchored to any existing schema section, so later sections may need to reconcile naming., The rewrite of the existing §22.1 sentence drops the phrase 'may legitimize, supersede, or conflict with observed scope drift' in favor of a pointer to §22.1.1. The meaning is preserved in the new section, but the one-line summary in §22.1 is now less self-describing., Decision text marks UBU-D0344 as Accepted while the question is Human-approval-required; acceptable under the normal merge-on-review flow, but the human approver should confirm this is the intended status on merge. |

## Selected Summary

Define immutable Relationship declarations, scoped supersession, temporal drift review and explicit transition links; resolve UBU-Q0087 through UBU-D0344.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Cross-references cannot be verified from this prompt: UBU-D0076, UBU-D0153, UBU-D0154, UBU-D0273, DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §§9 and 17, and the claim that UBU-D0344 is the next unused decision id. A wrong or already-allocated id would produce a dangling or colliding reference.
- DESIGN.md §22.1.1 is a single ~27-line hunk of very dense normative prose. It is correct in substance but hard to audit line by line, and some sentences bundle several distinct rules (e.g., rejection of cyclic refs, staging of unavailable refs, replay dedup) that a reviewer may want split or cross-linked to existing admission rules.
- Decision text marks UBU-D0344 as Accepted while the question is Human-approval-required; acceptable under the normal merge-on-review flow, but the human approver should confirm this is the intended status on merge.
- Introduces a new named artifact `ScopeAssessment` and the `current_participation`/`desired_participation` distinction inline. These are reasonable but are not anchored to any existing schema section, so later sections may need to reconcile naming.
- Partial supersession and causal historical views require careful implementation; the specified conformance fixtures have not been executed.
- The rewrite of the existing §22.1 sentence drops the phrase 'may legitimize, supersede, or conflict with observed scope drift' in favor of a pointer to §22.1.1. The meaning is preserved in the new section, but the one-line summary in §22.1 is now less self-describing.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261014T053030Z-UBU-Q0087/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261014T053030Z-UBU-Q0087/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261014T053030Z-UBU-Q0087"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
