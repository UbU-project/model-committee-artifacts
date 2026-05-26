# Model-Committee Review

Run: `20260526T205621Z-UBU-Q0010`  
Question: `UBU-Q0010`  
Base commit: `c5d808493dfb2d0f4f116a5304d8188055f1ecc0`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0010-d0184-token-custody`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 88.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0010-d0184-token-custody` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0010-d0184-token-custody` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0010-d0184-token-custody` | `codex` | `claude` | yes | 88 | The patch applies cleanly and is confirmed by mechanical validation. All seven sub-questions of UBU-Q0010 are answered: storage location (worker-held by default, canonical only when it is itself the writer), token identity (dedicated GitHub App/bot preferred, maintainer allowed for dogfooding, individual tokens not shared), repository scoping (required where GitHub supports it), Task-level scoping (enforced via UbU capability grants rather than provider-native per-Task tokens), canonical instance token visibility (must never see worker-held raw secrets), read-after-write verification (required; API success alone not sufficient), and MVP security assumption (cooperative operator-administered enclaves, least-privilege, human approval, append-only audit, revocation/rotation). The new DESIGN.md §26.3.1 provides normative prose that is well-structured and directly responsive. UBU-D0184 in DECISIONS.md is complete with consequences and cross-references. The two updates to existing DECISIONS.md entries (replacing 'UBU-Q0010 remains open' with 'governed by UBU-D0184') are correct cleanup. The OPEN_QUESTIONS.md tombstone follows the established 'Status: Solved … Resolved by: … / Resolved. See <decision>.' pattern. The sole identified defect is a missing blank line before the closing '---' separator in the tombstone — cosmetic and non-blocking. Scope is tight: only the three expected documentation files are touched. | Add blank line between 'Resolved. See UBU-D0184.' and the '---' HR in the OPEN_QUESTIONS.md tombstone to match the formatting of other tombstoned entries. | Missing blank line between 'Resolved. See UBU-D0184.' and the '---' separator in OPEN_QUESTIONS.md tombstone — minor formatting inconsistency with other resolved entries., Full question body stripped from OPEN_QUESTIONS.md; traceability depends entirely on DECISIONS.md and DESIGN.md remaining coherent cross-references., MVP security assumption explicitly disclaims protection against malicious local admin or compromised worker — correct scoping but reviewers should confirm this scope exclusion aligns with overall Phase 1 threat model agreed elsewhere. |

## Selected Summary

Resolve GitHub token custody for Phase 1 by making GitHub credentials actor-held scoped secrets, defaulting to worker-held credentials for worker-performed writes while keeping the canonical instance limited to credential refs, projection metadata, results, verification data, and Logs.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Full question body stripped from OPEN_QUESTIONS.md; traceability depends entirely on DECISIONS.md and DESIGN.md remaining coherent cross-references.
- MVP security assumption explicitly disclaims protection against malicious local admin or compromised worker — correct scoping but reviewers should confirm this scope exclusion aligns with overall Phase 1 threat model agreed elsewhere.
- Missing blank line between 'Resolved. See UBU-D0184.' and the '---' separator in OPEN_QUESTIONS.md tombstone — minor formatting inconsistency with other resolved entries.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260526T205621Z-UBU-Q0010/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260526T205621Z-UBU-Q0010/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260526T205621Z-UBU-Q0010"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
