# Model-Committee Review

Run: `20261012T151957Z-UBU-Q0177`  
Question: `UBU-Q0177`  
Base commit: `2e0d722f1d6a028bf64ca3ab48cb79aa8f9c25fd`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0177-local-hardware-qualification-2026-10-12`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 81.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0177-local-hardware-qualification-2026-10-12` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0177-local-hardware-qualification-2026-10-12` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0177-local-hardware-qualification-2026-10-12` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0177-local-hardware-qualification-2026-10-12` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0177-local-hardware-qualification-2026-10-12` | `openai` | `codex` | yes | 88 | Mechanical validation passes, edits remain within allowed files, and question metadata is preserved. The protocol meaningfully advances Q0177 with reproducible quality, resource, security, retention and fallback gates while respecting neighboring decisions. Keeping the question Open correctly reflects the missing real-device evidence and required human approval. | None | Provides a qualification protocol, not measured hardware results; hardware selection remains outstanding., Prescribed trial counts require workload-specific justification and do not alone establish reliable tail latency or security. |
| `UBU-Q0177-local-hardware-qualification-2026-10-12` | `openai` | `claude` | yes | 74 | The patch applies cleanly, touches only the three allowlisted files, leaves the Q0177 metadata line intact with Status: Open and Resolved by: None, and limits OPEN_QUESTIONS.md edits to the selected question. It does not answer the literal question (no hardware profile is selected), but it narrows it honestly by defining a revision-pinned qualification protocol instead of fabricating a RAM floor or benchmark results, and it says plainly that the protocol cannot close the question. That is the responsible shape for an Architecture question requiring measured evidence and human approval. Deductions are for unexplained numeric thresholds, unverified cross-references, the sheer length of the DESIGN.md block, and the fact that the question is advanced rather than resolved. | Mark the 30/100/30-minute trial counts as provisional operator-reviewable defaults or give a rationale for them., Confirm every cited section and contract reference (§12.2, §21.1.x, §23.1, DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §26) exists at the base commit., Confirm DESIGN.md §1 is the intended home for accepted deltas, or add a pointer at Delta §17.15 so the question's current-direction reference and the new text agree. | The concrete trial requirements (30 cold-start trials, 100 warm trials, 30 minutes sustained workload) are invented thresholds with no stated derivation; once in DESIGN.md they may harden into arbitrary acceptance gates., The DESIGN.md block cites §12.2, §21.1.3, §21.1.4, §21.1.5, §23.1 and DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §26; the validation notes do not say these targets were verified, so dangling references are possible., The question's current direction points at Delta §17.15, but the new material is inserted in DESIGN.md §1; if §1 is not the delta location, the two pointers diverge., The DESIGN.md addition is six dense paragraphs; the volume makes it hard to audit and increases the chance of contradicting existing retention, Device or Compartment contracts., The question stays Open with unchanged priority and no new sub-question, so the committee may re-select it without any new real-device evidence being available. |

## Selected Summary

Define measured, security-gated qualification for local multimodal hardware; retain Q0177 until real-device evidence supports a reviewed profile.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Prescribed trial counts require workload-specific justification and do not alone establish reliable tail latency or security.
- Provides a qualification protocol, not measured hardware results; hardware selection remains outstanding.
- The DESIGN.md addition is six dense paragraphs; the volume makes it hard to audit and increases the chance of contradicting existing retention, Device or Compartment contracts.
- The DESIGN.md block cites §12.2, §21.1.3, §21.1.4, §21.1.5, §23.1 and DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §26; the validation notes do not say these targets were verified, so dangling references are possible.
- The concrete trial requirements (30 cold-start trials, 100 warm trials, 30 minutes sustained workload) are invented thresholds with no stated derivation; once in DESIGN.md they may harden into arbitrary acceptance gates.
- The question stays Open with unchanged priority and no new sub-question, so the committee may re-select it without any new real-device evidence being available.
- The question's current direction points at Delta §17.15, but the new material is inserted in DESIGN.md §1; if §1 is not the delta location, the two pointers diverge.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261012T151957Z-UBU-Q0177/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261012T151957Z-UBU-Q0177/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261012T151957Z-UBU-Q0177"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
