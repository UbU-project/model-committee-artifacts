# Model-Committee Review

Run: `20260526T025116Z-UBU-Q0009`  
Question: `UBU-Q0009`  
Base commit: `1c18876262a0e6d041989a128e36947b2b31f807`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0009-worker-mutation-schema-001`

## Disagreement Flags

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
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0009-worker-mutation-schema-001` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0009-worker-mutation-schema-001` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0009-worker-mutation-schema-001` | `codex` | `claude` | yes | 88 | The patch applies cleanly per mechanical validation. All eight sub-questions in UBU-Q0009 receive direct, explicit answers: submission payload families (all three), full required and optional field enumerations, conditional-not-immediate application rule, rejected-request logging with sanitized reason, atomic batch semantics, create-only request path, append-only reversal, and expected-prior-version plus idempotency-key stale-overwrite prevention. The three changed files (DESIGN.md, DECISIONS.md, OPEN_QUESTIONS.md) are exactly those expected for a question resolution; no extraneous files are touched. DECISIONS.md entry UBU-D0164 is coherent, complete, and correctly cross-references open downstream questions. The section renumbering in DESIGN.md (24.1.3 to 24.1.4) is properly handled. The only notable defect is the 'Solved' vs 'Resolved' status-field vocabulary mismatch, which is a minor schema-consistency issue rather than a substantive gap. The proposal does not open new sub-questions, which is appropriate for an MVP-blocker resolution. | Verify that 'Solved' is the accepted Status vocabulary in OPEN_QUESTIONS.md; if other resolved questions use 'Resolved', change the UBU-Q0009 status line to match. | Status field uses 'Solved' rather than 'Resolved', which may be inconsistent with other resolved questions in OPEN_QUESTIONS.md; the resolution body itself uses 'Resolved' throughout, creating a minor intra-document inconsistency., The required-field list is long and prescriptive for a Phase 1 MVP blocker; implementation teams could struggle to satisfy all mandatory fields simultaneously, and any later relaxation would require another decision., Decision number UBU-D0164 cannot be independently verified as sequential without full DECISIONS.md context; an off-by-one would create a reference mismatch with any existing D0164., The 'independent_items' batch mode is introduced as non-atomic but still permitted, which could allow partial canonical mutations that are hard to reason about without further guidance on reconciliation. |

## Selected Summary

Resolve the Phase 1 worker mutation request schema by defining bounded worker submission payloads, required mutation-request fields, closed operation kinds, review and auto-apply rules, atomic batching, create-request behavior, append-only reversal, and stale-overwrite prevention.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Decision number UBU-D0164 cannot be independently verified as sequential without full DECISIONS.md context; an off-by-one would create a reference mismatch with any existing D0164.
- Status field uses 'Solved' rather than 'Resolved', which may be inconsistent with other resolved questions in OPEN_QUESTIONS.md; the resolution body itself uses 'Resolved' throughout, creating a minor intra-document inconsistency.
- The 'independent_items' batch mode is introduced as non-atomic but still permitted, which could allow partial canonical mutations that are hard to reason about without further guidance on reconciliation.
- The required-field list is long and prescriptive for a Phase 1 MVP blocker; implementation teams could struggle to satisfy all mandatory fields simultaneously, and any later relaxation would require another decision.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260526T025116Z-UBU-Q0009/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260526T025116Z-UBU-Q0009/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260526T025116Z-UBU-Q0009"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
