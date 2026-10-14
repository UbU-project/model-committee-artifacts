# Model-Committee Review

Run: `20261014T075732Z-UBU-Q0101`  
Question: `UBU-Q0101`  
Base commit: `4ceb0ef3797c1cbf3e7a7edd33835404105037e1`  
Automated selection: valid
Human review required: no
Selected proposal: `openai-ubu-q0101-advisory-safeguard-contract`

## Disagreement Flags

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
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-ubu-q0101-advisory-safeguard-contract` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-ubu-q0101-advisory-safeguard-contract` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-ubu-q0101-advisory-safeguard-contract` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-ubu-q0101-advisory-safeguard-contract` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `openai-ubu-q0101-advisory-safeguard-contract` | `openai` | `codex` | yes | 96 | Reported mechanical validation passes, with changes confined to the three allowed documentation files. The proposal addresses all five subquestions through uncertainty disclosure, scoped controls, separately authorized introspection, preservation of declared values, and bounded opt-in reminders. It prevents bypass alone from establishing misconduct or values conflict and preserves independent hard boundaries. The question correctly uses Status: Solved and retains scoring metadata. Implementation validation and broader extrospection thresholds remain explicitly separate. No blocking defect is evident in the supplied patch. | None | The detailed contract requires coordinated policy, notification, audit, and evidence-use enforcement; the specified fixtures are requirements, not demonstrated implementation conformance. |
| `openai-ubu-q0101-advisory-safeguard-contract` | `openai` | `claude` | yes | 84 | Mechanical validation passed: patch applies, allowlist OK, question schema OK. The proposal is directly responsive to all five subquestions: 22.4.1 covers uncertainty and false-positive/false-negative disclosure (subq 1); 22.4.2 defines a concrete control table for review/revise, Continue once, disable, self-imposed required review, and dismiss/snooze (subq 2); 22.4.3 sets evidence, authority, and no-coercion rules for introspection candidates (subq 3) and decouples disablement from declared values (subq 4); 22.4.4 bounds repeated-bypass review to user-requested or opt-in finite reminders (subq 5). OPEN_QUESTIONS.md uses Status: Solved with Resolved by: UBU-D0356 and keeps all other metadata fields intact. Scope is limited to the three expected files, with two small consistency edits to existing §22.4 text and an explicit deferral of broader proactive thresholds to UBU-Q0089. It adds a new accepted decision and sets requires_human_review, consistent with the question's human-approval eligibility. Deductions are for unverifiable cross-references, one residual inconsistent sentence in existing §22.4, and the very dense prose; none of these block adoption. | Align the last paragraph of the pre-existing §22.4 prose ('records bypasses as introspection-relevant evidence') with the new 22.4.3 language, e.g. 'records bypasses in decision Logs that may contribute to optional, authorized introspection review', so the section is internally consistent., Confirm at base commit 4ceb0ef that the cited Log codes/fields and section anchors exist with those exact names; if any do not, either rename to the existing ones or introduce them explicitly in §17.0 rather than referencing them as given. | The new DESIGN.md text cites specific Log codes and fields (`safeguard_advisory_bypassed`, `alert_dismissed_or_ignored`, `authority_source: user_override`, `preference_inference_allowed: false`, `none_given`) and section anchors (§17.0, §21.1.7, §21.2.1, §2.10.1.1, §22.3.5, §22.3.6) as already existing; these cannot be verified from the prompt and would dangle if any name differs at the base commit., The unchanged final paragraph of §22.4 still says UbU 'records bypasses as introspection-relevant evidence', which is slightly out of step with the softened wording the patch introduces two paragraphs earlier ('bypass alone establishes no conflict'); readers could see a residual tension about whether bypass is evidence by default., The decision and design text are extremely compressed, with many capitalized domain nouns and long enumerations; this matches the surrounding repository style but raises the chance that a later reader misreads a clause (for example the distinction between Continue once, required-gate exception, and disablement)., The proposal shifts the question's 'Current direction' ('introspection-relevant when bypassed') toward 'optional, authorized introspection review'; this is responsive to the anti-coercion framing of the question but is a substantive product stance that the human approver should consciously ratify., UBU-D0356 is assumed to be the next free decision ID; the patch applies at end of DECISIONS.md, which supports this, but ID uniqueness is not independently confirmed here. |

## Selected Summary

Define uncertainty disclosures, scoped override controls, optional values-linked introspection, and bounded review of repeated safeguard bypasses.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- The decision and design text are extremely compressed, with many capitalized domain nouns and long enumerations; this matches the surrounding repository style but raises the chance that a later reader misreads a clause (for example the distinction between Continue once, required-gate exception, and disablement).
- The detailed contract requires coordinated policy, notification, audit, and evidence-use enforcement; the specified fixtures are requirements, not demonstrated implementation conformance.
- The new DESIGN.md text cites specific Log codes and fields (`safeguard_advisory_bypassed`, `alert_dismissed_or_ignored`, `authority_source: user_override`, `preference_inference_allowed: false`, `none_given`) and section anchors (§17.0, §21.1.7, §21.2.1, §2.10.1.1, §22.3.5, §22.3.6) as already existing; these cannot be verified from the prompt and would dangle if any name differs at the base commit.
- The proposal shifts the question's 'Current direction' ('introspection-relevant when bypassed') toward 'optional, authorized introspection review'; this is responsive to the anti-coercion framing of the question but is a substantive product stance that the human approver should consciously ratify.
- The unchanged final paragraph of §22.4 still says UbU 'records bypasses as introspection-relevant evidence', which is slightly out of step with the softened wording the patch introduces two paragraphs earlier ('bypass alone establishes no conflict'); readers could see a residual tension about whether bypass is evidence by default.
- UBU-D0356 is assumed to be the next free decision ID; the patch applies at end of DECISIONS.md, which supports this, but ID uniqueness is not independently confirmed here.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261014T075732Z-UBU-Q0101/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261014T075732Z-UBU-Q0101/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261014T075732Z-UBU-Q0101"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
