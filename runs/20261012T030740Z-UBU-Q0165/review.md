# Model-Committee Review

Run: `20261012T030740Z-UBU-Q0165`  
Question: `UBU-Q0165`  
Base commit: `2ee88e3dd27155ac8ea0a7323da02c2cdca8fde9`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0165-openai-gpt-6-observation-admission-1`

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
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0165-openai-gpt-6-observation-admission-1` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0165-openai-gpt-6-observation-admission-1` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0165-openai-gpt-6-observation-admission-1` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0165-openai-gpt-6-observation-admission-1` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0165-openai-gpt-6-observation-admission-1` | `openai` | `codex` | yes | 94 | Mechanical validation passes, changes stay within the three allowed files, and Q0165 uses Status: Solved with its metadata preserved. The contract covers source/session/context bindings, ambiguity, candidate dependencies, observed versions, review, replay and corrections through existing Discovery/advisory machinery. It preserves name-only UniverseTarget advice and unchanged FactProvenance fields, requires explicit admission, and keeps adjacent questions open. No blocking architectural defect was identified. | None | Fixtures specify scenarios and expected outcomes but do not supply complete serialized records or demonstrate host-schema compatibility; these remain explicit implementation gates., Future adapters must preserve the evidence-only fallback while qualifier fidelity, corroboration sufficiency and proof-of-effect semantics remain unresolved. |
| `UBU-Q0165-openai-gpt-6-observation-admission-1` | `openai` | `claude` | yes | 84 | Patch applies cleanly, passes allowlist and question-schema checks, and changes only DESIGN.md, DECISIONS.md and OPEN_QUESTIONS.md. Status moves Open → Solved with `Resolved by: UBU-D0320` as a separate field and all other metadata preserved. The content directly answers the question as scoped by Current direction: it specifies source/session refs (`sources`), identity ambiguity (`subjects` with alternatives and explicit operator selection), candidate object/value links (`dependencies`), observed versions and policy versions, evidence binding (`claim_evidence`), idempotency and replay rules, corrections via `predecessors` and append-only §17.3 entries, and review presentation/admission rechecks. It also supplies the previously unresolved concrete fixtures as acceptance gates. It honors the constraint not to invent FactProvenance fields and explicitly keeps UniverseTarget advice name-only, with every path gated through existing Discovery/advisory review and ordinary CPU admission, so no automatic canonical mutation is introduced. Cross-reference hygiene in §21.1.1 and §21.1.3 narrows the retained-question ranges correctly. Deductions are for unverifiable section anchors, dense prose that is hard to audit, possible newly coined terms presented as reuse, and minor bleed into retention/corroboration territory held by other open questions. | Before merge, confirm at base commit 2ee88e3 that the referenced anchors exist and match: §12.2 `DiscoveryEvidenceItem`, §21.2.1 `AdvisoryCandidate`, §11.3 UniverseState operation/target/payload rules, §10.1 preconditions, and `UBU-D0296` name-only UniverseTarget advice., Confirm `UBU-D0320` is the next unused decision id and that the `## UBU-D0320:` heading plus `**Status:**` line match the formatting convention of the preceding DECISIONS.md entries. | Cross-references to §12.2 `DiscoveryEvidenceItem`, §21.2.1 `AdvisoryCandidate`, §11.3 operation/target/payload rules, §10.1 preconditions and `UBU-D0296` cannot be verified from the scoring prompt; if any anchor is absent or numbered differently at the base commit, the contract has dangling references., Introduces terminology (`observation-admission/1`, capture keys, admitted envelope/frontier digest, candidate-creation vs review-operation idempotency keys) that may not correspond to existing named concepts in DESIGN.md; reviewers must confirm these are reuse rather than new first-class machinery despite the text's claim., The DESIGN.md addition is roughly 43 dense lines with long single-paragraph sections and two wide tables, which lowers line-level auditability and raises the chance that a subtle semantic contradiction with existing §21.2 admission text goes unnoticed., Light scope bleed into retention (raw-media purge behavior) and corroboration (repetition cannot satisfy corroboration), which are nominally retained by UBU-Q0167 and UBU-Q0169–Q0171; the text frames these as reuse of existing policy but still states behavior., Question is marked Human approval required; the proposal correctly sets requires_human_review but the decision text uses 'Accepted' status, which should be understood as pending human approval through the committee flow. |

## Selected Summary

Specify observation payload bindings, Discovery/advisory reuse and review-bound admission, with required acceptance fixtures.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Cross-references to §12.2 `DiscoveryEvidenceItem`, §21.2.1 `AdvisoryCandidate`, §11.3 operation/target/payload rules, §10.1 preconditions and `UBU-D0296` cannot be verified from the scoring prompt; if any anchor is absent or numbered differently at the base commit, the contract has dangling references.
- Fixtures specify scenarios and expected outcomes but do not supply complete serialized records or demonstrate host-schema compatibility; these remain explicit implementation gates.
- Future adapters must preserve the evidence-only fallback while qualifier fidelity, corroboration sufficiency and proof-of-effect semantics remain unresolved.
- Introduces terminology (`observation-admission/1`, capture keys, admitted envelope/frontier digest, candidate-creation vs review-operation idempotency keys) that may not correspond to existing named concepts in DESIGN.md; reviewers must confirm these are reuse rather than new first-class machinery despite the text's claim.
- Light scope bleed into retention (raw-media purge behavior) and corroboration (repetition cannot satisfy corroboration), which are nominally retained by UBU-Q0167 and UBU-Q0169–Q0171; the text frames these as reuse of existing policy but still states behavior.
- Question is marked Human approval required; the proposal correctly sets requires_human_review but the decision text uses 'Accepted' status, which should be understood as pending human approval through the committee flow.
- The DESIGN.md addition is roughly 43 dense lines with long single-paragraph sections and two wide tables, which lowers line-level auditability and raises the chance that a subtle semantic contradiction with existing §21.2 admission text goes unnoticed.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261012T030740Z-UBU-Q0165/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261012T030740Z-UBU-Q0165/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261012T030740Z-UBU-Q0165"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
