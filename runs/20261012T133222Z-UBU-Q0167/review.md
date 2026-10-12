# Model-Committee Review

Run: `20261012T133222Z-UBU-Q0167`  
Question: `UBU-Q0167`  
Base commit: `734e6a1fbb79027e960a2eebc39b96b92b11e61e`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0167-openai-gpt-6-20261012`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 91.5
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0167-openai-gpt-6-20261012` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0167-openai-gpt-6-20261012` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0167-openai-gpt-6-20261012` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0167-openai-gpt-6-20261012` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0167-openai-gpt-6-20261012` | `openai` | `codex` | yes | 97 | Mechanical validation passes, changes stay within the three allowed files, and the question correctly uses Status: Solved. The contract directly covers proportionate review, independent corroboration, unknown and conflicting evidence, operator correction, reassessment before consequential reuse, and separate external-action authorization. Adjacent implementation and proof-of-effect questions remain open, with no schema or kernel changes. | None | Runtime enforcement remains unverified and depends on host mappings and behavioral fixtures; the proposal explicitly gates enablement on those checks. |
| `UBU-Q0167-openai-gpt-6-20261012` | `openai` | `claude` | yes | 86 | Mechanical validation passes: patch applies, only the three allowed files change, and UBU-Q0167 keeps its single-line metadata with Status: Solved and Resolved by: UBU-D0324 populated correctly. The content answers the question as framed by the current direction: a three-tier use classification (clarification, bounded reversible ordinary change, consequential use) keeps ordinary clarification proportionate; a precise definition of independent corroboration explicitly excludes model confidence, repeated frames, multiple models on one input, copied reports and vote counts; unresolved/conflicting-source behavior is specified (no averaging, no inference from absence, no silent source choice, coherent-subset partial progress); operator authoring and correction are preserved as append-only with Plan invalidation and no late-output overwrite; and the boundary between evidence admission and external-action authorization is stated with a pre-execution recheck. No automatic confidence threshold is selected, matching the direction. Cross-references in existing paragraphs are updated consistently so Q0167 no longer appears as retained-open in §21.1.1 while Q0166 and Q0172 do. The resolution text is honest that it settles the process contract, not implementation, host fidelity or proof-of-effect. Deductions reflect the Accepted status ahead of required human approval, the possible hidden schema implication of the retention requirements, and the density/overlap of the new acceptance-case table. | None | DECISIONS.md records UBU-D0324 as Accepted while the question's auto-choice eligibility is Human approval required; the proposal sets requires_human_review, but the recorded status assumes approval that has not yet occurred., The assessment-binding paragraph requires retaining reviewed use/scope, consequence class, independence basis and sufficiency rationale 'with existing candidate/evidence payloads', while the decision asserts no schema change. If the existing companion payload has no slot for these, this silently implies a schema addition that UBU-D0321's schema-gap chain would need to review., The consequential tier requires an independent corroborating source for every decision-critical perceptual claim; combined with the broad consequential category list (any claim that releases capacity or unlocks dependent execution), this could be operationally heavy, though operator direct inspection qualifying as a source keeps it workable., 'Bounded ordinary use by explicit operator cost/reversibility limits' does not say where those limits are declared or how a reviewer learns the current limit, leaving the ordinary/consequential boundary partly unspecified in practice., Section 21.1.1 grows by roughly 20 dense lines plus two tables; the new acceptance-case table partly overlaps the existing fixture table, adding maintenance burden if the two drift. |

## Selected Summary

Define consequence-sensitive corroboration and human review for perceptual planning inputs, resolving UBU-Q0167 through UBU-D0324.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- 'Bounded ordinary use by explicit operator cost/reversibility limits' does not say where those limits are declared or how a reviewer learns the current limit, leaving the ordinary/consequential boundary partly unspecified in practice.
- DECISIONS.md records UBU-D0324 as Accepted while the question's auto-choice eligibility is Human approval required; the proposal sets requires_human_review, but the recorded status assumes approval that has not yet occurred.
- Runtime enforcement remains unverified and depends on host mappings and behavioral fixtures; the proposal explicitly gates enablement on those checks.
- Section 21.1.1 grows by roughly 20 dense lines plus two tables; the new acceptance-case table partly overlaps the existing fixture table, adding maintenance burden if the two drift.
- The assessment-binding paragraph requires retaining reviewed use/scope, consequence class, independence basis and sufficiency rationale 'with existing candidate/evidence payloads', while the decision asserts no schema change. If the existing companion payload has no slot for these, this silently implies a schema addition that UBU-D0321's schema-gap chain would need to review.
- The consequential tier requires an independent corroborating source for every decision-critical perceptual claim; combined with the broad consequential category list (any claim that releases capacity or unlocks dependent execution), this could be operationally heavy, though operator direct inspection qualifying as a source keeps it workable.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261012T133222Z-UBU-Q0167/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261012T133222Z-UBU-Q0167/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261012T133222Z-UBU-Q0167"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
