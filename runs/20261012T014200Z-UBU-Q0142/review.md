# Model-Committee Review

Run: `20261012T014200Z-UBU-Q0142`  
Question: `UBU-Q0142`  
Base commit: `e736c1d19e398acdfec94c6a3df32e3896038e55`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0142-openai-gpt-6-disconnect-recovery`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 88.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0142-openai-gpt-6-disconnect-recovery` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0142-openai-gpt-6-disconnect-recovery` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0142-openai-gpt-6-disconnect-recovery` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0142-openai-gpt-6-disconnect-recovery` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0142-openai-gpt-6-disconnect-recovery` | `openai` | `codex` | yes | 96 | Mechanical validation passes, and the checkout matches the stated base commit. The patch directly resolves disconnection handling with durable incremental admission, dependency-safe staging, idempotent recovery, safe discard, and scope-bound completion. It distinguishes lost acknowledgment from failed admission and preserves current authority, Compartment privacy, conflict review, and Plan invalidation rules. Changes stay within allowed files and preserve question metadata while correctly using Status: Solved. | None | Durable admission, recovery metadata, and per-recipient acknowledgments add implementation obligations; the required interruption and recovery certification remains outstanding. |
| `UBU-Q0142-openai-gpt-6-disconnect-recovery` | `openai` | `claude` | yes | 80 | Mechanical validation passes on all counts: patch applies, allowlist holds, and the question block keeps Status: Solved with Resolved by: UBU-D0314 while preserving every ranking and provenance field. The proposal directly answers what happens on mid-session disconnect with a coherent, idempotent recovery model (durable recovery record, slot release, incremental admission of fully verified statements, scope-bound checkpoint completion, lost-ack as unknown delivery, resume/replay/discard, Compartment-safe diagnostics, Plan invalidation) and adds concrete certification cases without claiming they passed. Hunk line counts are internally consistent. It follows the established D0312/D0313 pattern across the three files and sets requires_human_review appropriately. Points are deducted for the oversized DESIGN.md insertion, unverifiable identifier and decision references, and a new sender-side acknowledgment obligation whose interaction with the manual bundle path is left open. No decomposition occurred and none was needed for a single-focus question. | Confirm that incomplete_sync_session, checkpoint, manual_decision, and conflict_resolution match existing identifiers in DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md sections 16, 17, and 22 (and that UBU-D0248 exists with the cited meaning); if any are new, state so explicitly in UBU-D0314 or add a follow-up question to register them in the contract., Reconcile the pending-until-authenticated-acknowledgment rule with UBU-D0313's manual one-way bundle workflow: either scope the rule to live sessions, or state how return-bundle acknowledgments are produced and what happens to pending growth when none arrives. | DESIGN.md section 1 receives seven dense normative paragraphs, far heavier than the sibling UBU-D0312/UBU-D0313 entries; contract-level detail now lives in the design overview and may drift from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md sections 16, 17, and 22, which the proposal could not edit., The text names diagnostic and statement identifiers (incomplete_sync_session, checkpoint, manual_decision, conflict_resolution) and cites UBU-D0248 as if they already exist in the contract vocabulary; this cannot be verified from the prompt, and an unregistered diagnostic code would be an implicit scope expansion., The rule that outgoing statements stay pending until the sender durably records an authenticated acknowledgment of the recipient's checkpoint admission adds a sender-side obligation to the indirect bundle path. UBU-D0313 treats one-way manual export/copy/import as sufficient, so without a return bundle, pending sets may grow indefinitely; this interaction is not addressed., Incremental admission of verified statements before the final checkpoint is a substantive semantic choice (partial sessions produce visible state); the decision does not record the rejected all-or-nothing alternative, which weakens auditability of the choice., Decision is marked Accepted while the question requires human approval; acceptable under the apply-after-approval convention but the human reviewer should confirm the decision text, not just the resolution summary. |

## Selected Summary

Specify durable, idempotent Phase 2 sync recovery: preserve admitted changes, resume or safely discard staging, and distinguish local checkpoint completion from acknowledged delivery.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- DESIGN.md section 1 receives seven dense normative paragraphs, far heavier than the sibling UBU-D0312/UBU-D0313 entries; contract-level detail now lives in the design overview and may drift from DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md sections 16, 17, and 22, which the proposal could not edit.
- Decision is marked Accepted while the question requires human approval; acceptable under the apply-after-approval convention but the human reviewer should confirm the decision text, not just the resolution summary.
- Durable admission, recovery metadata, and per-recipient acknowledgments add implementation obligations; the required interruption and recovery certification remains outstanding.
- Incremental admission of verified statements before the final checkpoint is a substantive semantic choice (partial sessions produce visible state); the decision does not record the rejected all-or-nothing alternative, which weakens auditability of the choice.
- The rule that outgoing statements stay pending until the sender durably records an authenticated acknowledgment of the recipient's checkpoint admission adds a sender-side obligation to the indirect bundle path. UBU-D0313 treats one-way manual export/copy/import as sufficient, so without a return bundle, pending sets may grow indefinitely; this interaction is not addressed.
- The text names diagnostic and statement identifiers (incomplete_sync_session, checkpoint, manual_decision, conflict_resolution) and cites UBU-D0248 as if they already exist in the contract vocabulary; this cannot be verified from the prompt, and an unregistered diagnostic code would be an implicit scope expansion.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261012T014200Z-UBU-Q0142/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261012T014200Z-UBU-Q0142/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261012T014200Z-UBU-Q0142"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
