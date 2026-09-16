# Model-Committee Review

Run: `20260916T141203Z-UBU-Q0143`  
Question: `UBU-Q0143`  
Base commit: `78a14eb5860c3595821c6f6f90f73844166592fb`  
Automated selection: valid
Human review required: no
Selected proposal: `ubu-q0143-gpt5-20260916`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 87.5
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `ubu-q0143-gpt5-20260916` |
| `work-score` | `claude` | `sonnet` | `ubu-q0143-gpt5-20260916` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `ubu-q0143-gpt5-20260916` |
| `work-score` | `claude` | `sonnet` | `ubu-q0143-gpt5-20260916` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `ubu-q0143-gpt5-20260916` | `openai` | `codex` | yes | 90 | The patch applies after normalization, stays within the allowed files, preserves the question block format, and directly resolves UBU-Q0143 by drawing a conservative boundary between deterministic auto-resolution, containment/recalculation, and human-review-required cases. The decision is auditable, scoped to the process question, and aligns with the referenced sync-conflict context without modifying the contract itself. The main residual risk is that some conditional auto-resolution classes require careful implementation discipline, but the proposal states those conditions clearly enough for a design decision. | None | The automatic field-level merge allowance for non-overlapping stale_prior_version depends on strong proof of non-interference; implementations may need precise invariant and causal-precondition checks to avoid over-admitting stale writes., The device_revoked_conflict exception for deterministic rejection is reasonable but should be kept narrowly interpreted so it does not bypass review during recovery flows. |
| `ubu-q0143-gpt5-20260916` | `openai` | `claude` | yes | 85 | The patch touches only DECISIONS.md and OPEN_QUESTIONS.md, both allowlisted, and does not modify the read-only device-sync contract it references. OPEN_QUESTIONS.md correctly updates Status to Solved, sets Resolved by to the new decision ID, and writes a resolution paragraph without altering other metadata fields or introducing new open questions. The new DECISIONS.md entry (UBU-D0248) directly answers the question by enumerating concrete conflict classes already named in the contract and sorting them into auto-resolvable, containable/recalculable, and human-review-required buckets with a fail-closed default for authority/intent/privacy conflicts, which is a sound and conservative policy stance. Mechanical validation reports patch_applies true only after --recount normalization was needed (ordinary apply hit 'corrupt patch at line 44'), which is a minor but real fragility flag rather than a clean apply. | None | Patch required --recount normalization to apply (corrupt patch at line 44 under ordinary apply), which is a mechanical fragility even though it ultimately applies cleanly, The three-tier classification (auto-resolvable / containable-recalculable / human-review-required) is a reasonable but somewhat elaborate framework for a question that could have been answered more tersely; some readers may find the containable/recalculable middle tier blurs the auto-vs-review line the question asks for, Edge-case carve-outs (e.g., deterministic rejection of pending statements from a revoked device, non-overlapping stale_prior_version merges) introduce judgment calls that could themselves become sources of future disputes about what counts as 'deterministic' or 'non-overlapping' |

## Selected Summary

Classifies sync conflicts into deterministic auto-resolution, automatic containment/recalculation, and human-review-required classes, resolving UBU-Q0143 with a conservative fail-closed policy for authority, privacy, policy, protected calendar, and irreducible intent conflicts.

## Changed Files

- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Edge-case carve-outs (e.g., deterministic rejection of pending statements from a revoked device, non-overlapping stale_prior_version merges) introduce judgment calls that could themselves become sources of future disputes about what counts as 'deterministic' or 'non-overlapping'
- Patch required --recount normalization to apply (corrupt patch at line 44 under ordinary apply), which is a mechanical fragility even though it ultimately applies cleanly
- The automatic field-level merge allowance for non-overlapping stale_prior_version depends on strong proof of non-interference; implementations may need precise invariant and causal-precondition checks to avoid over-admitting stale writes.
- The device_revoked_conflict exception for deterministic rejection is reasonable but should be kept narrowly interpreted so it does not bypass review during recovery flows.
- The three-tier classification (auto-resolvable / containable-recalculable / human-review-required) is a reasonable but somewhat elaborate framework for a question that could have been answered more tersely; some readers may find the containable/recalculable middle tier blurs the auto-vs-review line the question asks for

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260916T141203Z-UBU-Q0143/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260916T141203Z-UBU-Q0143/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260916T141203Z-UBU-Q0143"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
