# Model-Committee Review

Run: `20260916T191621Z-UBU-Q0149`  
Question: `UBU-Q0149`  
Base commit: `af7efa568ad04b2f5b3139fdf98dc97bbc64d36d`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0149-20260916-001`

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
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `codex-ubu-q0149-20260916-001` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0149-20260916-001` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `codex-ubu-q0149-20260916-001` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0149-20260916-001` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0149-20260916-001` | `openai` | `codex` | yes | 92 | The proposal cleanly resolves UBU-Q0149 with a focused decision that directly answers storage, rotation, revocation, replication, offline-device, recovery, and Compartment-policy constraints. It preserves the OPEN_QUESTIONS metadata shape, marks the question solved, modifies only DECISIONS.md and OPEN_QUESTIONS.md, and avoids unnecessary implementation detail or broad refactoring. The custody model is auditable and conservative, especially around offline revocation and plaintext sync prohibition. | None | Decision text references DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §21 as the target design location but does not update that file, so downstream readers must rely on DECISIONS.md and OPEN_QUESTIONS.md until design integration occurs., The decision intentionally defers concrete cryptographic and platform mechanisms, which is appropriate for semantics but leaves implementation-specific security choices unresolved. |
| `codex-ubu-q0149-20260916-001` | `openai` | `claude` | yes | 88 | The patch applies cleanly per mechanical validation and touches only DECISIONS.md and OPEN_QUESTIONS.md, respecting the read-only constraint on DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md. It directly answers the question's three named custody problems (external tokens, local encryption keys, worker credentials) with concrete, falsifiable custody semantics: Compartment-scoped secret capabilities, explicit per-Device authorization, envelope-encrypted replication only to authorized recipients, versioned rotation with per-Device rewrap obligations, and revocation that honestly distinguishes 'denied for admitted Devices' from 'unconfirmed for offline Devices' rather than claiming guaranteed deletion. This avoids overclaiming security guarantees UbU cannot enforce (e.g., it explicitly refuses to claim an offline Device has destroyed a key). OPEN_QUESTIONS.md update preserves the existing status-line schema and cross-references UBU-D0252 correctly. Scope is appropriately narrow: it doesn't touch the sync contract file, doesn't invent unrelated new questions, and stays within the security/custody topic asked. Minor concern is the density of sub-rules bundled into a single decision, but this is proportionate to the compound nature of the original question. | None | Introduces a fairly dense new normative decision (UBU-D0252) with many sub-rules (rotation, revocation, recovery, envelope encryption) in one shot, which raises the surface area for future inconsistency or reinterpretation, though this matches the breadth of the original question., Leaves cryptographic algorithm choices and OS keychain integration to future implementation design, which is reasonable but means some residual ambiguity remains for later questions. |

## Selected Summary

Resolve UBU-Q0149 by adding UBU-D0252, which defines Phase 3 custody for external tokens, local encryption keys, and worker credentials as Compartment-scoped secret capabilities with explicit per-Device authorization, envelope-encrypted optional replication, auditable rotation, and revocation semantics that do not pretend offline deletion has succeeded.

## Changed Files

- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Decision text references DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §21 as the target design location but does not update that file, so downstream readers must rely on DECISIONS.md and OPEN_QUESTIONS.md until design integration occurs.
- Introduces a fairly dense new normative decision (UBU-D0252) with many sub-rules (rotation, revocation, recovery, envelope encryption) in one shot, which raises the surface area for future inconsistency or reinterpretation, though this matches the breadth of the original question.
- Leaves cryptographic algorithm choices and OS keychain integration to future implementation design, which is reasonable but means some residual ambiguity remains for later questions.
- The decision intentionally defers concrete cryptographic and platform mechanisms, which is appropriate for semantics but leaves implementation-specific security choices unresolved.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260916T191621Z-UBU-Q0149/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260916T191621Z-UBU-Q0149/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260916T191621Z-UBU-Q0149"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
