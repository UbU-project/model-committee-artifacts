# Model-Committee Review

Run: `20261012T022331Z-UBU-Q0161`  
Question: `UBU-Q0161`  
Base commit: `4b0ceaa788cff06361786b685ace45b3f43b6091`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0161-openai-gpt-6-20261011`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 89.5
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0161-openai-gpt-6-20261011` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0161-openai-gpt-6-20261011` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0161-openai-gpt-6-20261011` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0161-openai-gpt-6-20261011` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0161-openai-gpt-6-20261011` | `openai` | `codex` | yes | 95 | Supplied mechanical validation confirms clean application, allowlisted files and preserved question schema, including Status: Solved. The proposal directly resolves retention, synthesis acceptance, corrections and later Clarify context. Exact sources remain independent of derived prose; bidirectional coverage, explicit review and version-bound admission make replacement auditable. Failure handling and finite-budget behavior prevent silent loss, while privacy rules cover retained derivatives. The documentation clearly distinguishes the adopted product contract from outstanding implementation evidence. No blocking fixes are apparent. | None | Semantic fidelity depends on thorough operator review; complete structural coverage cannot detect all omissions or distorted qualifications., Growing source histories increase storage, synthesis and review costs. Concrete limits and derivative-purge behavior still require implementation and validation. |
| `UBU-Q0161-openai-gpt-6-20261011` | `openai` | `claude` | yes | 84 | Patch applies cleanly, touches only the three allowlisted files, and the question metadata line is updated correctly with Status: Solved and Resolved by: UBU-D0317 while preserving all other fields. The decision directly answers the question: it specifies the retention mechanism (durable per-Task source records with provenance and append-only supersession links, answers admitted independently of synthesis), the acceptance criterion (bidirectional source-to-prose coverage map plus explicit version-bound operator approval, with failure preserving prior prose and saved answers), bounding (declared finite budgets, chunking along source records, stop-rather-than-truncate), and later-Clarify context (originals retrieved, narrative not treated as sole memory). It honours the operator's 'lore field, not transcript' direction by keeping the description as prose and the Q&A as separate backing records, and explicitly forbids invented agreement and lossy replacement. The resolution text correctly scopes the outcome as a product contract with implementation evidence outstanding, and requires_human_review is set as the question's auto-choice eligibility demands. Deductions are for verbosity relative to surrounding design entries, an unverifiable cross-reference, and the unaddressed scaling of manual review. | None | DESIGN.md §1 entry is six dense paragraphs where neighbouring entries (e.g. UBU-D0316) are one; readability of the design doc itself degrades and the mechanism detail (hashes, actor/Device/session/round provenance, chunking) is heavier than a Product-type decision typically carries., Cross-reference to UBU-D0274's candidate/admission boundary and the backticked type name `AdvisoryCandidate` could not be verified against the ubu-design tree from this scoring context; if D0274 does not define such a boundary or the identifier is new, the decision introduces an unanchored dependency., Acceptance relies on a full source-by-source operator checklist at every synthesis and after every edit; no incremental-review model is described, so review burden grows with Task history and may push operators toward rubber-stamping, which undermines the losslessness guarantee the decision rests on., Privacy, partial-replica, purge and storage-exhaustion clauses extend beyond the literal question; they are consistent with adjacent design text but widen the surface a reviewer must accept as part of resolving Q0161., No follow-up question is filed for the acknowledged gap that structural coverage cannot prove semantic fidelity; the decision honestly states the limit but leaves no tracked item for how manual review scales. |

## Selected Summary

Adopt reviewed Task-description prose backed by exact Clarify source history, with complete coverage, linked corrections and bounded later-context retrieval.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Acceptance relies on a full source-by-source operator checklist at every synthesis and after every edit; no incremental-review model is described, so review burden grows with Task history and may push operators toward rubber-stamping, which undermines the losslessness guarantee the decision rests on.
- Cross-reference to UBU-D0274's candidate/admission boundary and the backticked type name `AdvisoryCandidate` could not be verified against the ubu-design tree from this scoring context; if D0274 does not define such a boundary or the identifier is new, the decision introduces an unanchored dependency.
- DESIGN.md §1 entry is six dense paragraphs where neighbouring entries (e.g. UBU-D0316) are one; readability of the design doc itself degrades and the mechanism detail (hashes, actor/Device/session/round provenance, chunking) is heavier than a Product-type decision typically carries.
- Growing source histories increase storage, synthesis and review costs. Concrete limits and derivative-purge behavior still require implementation and validation.
- No follow-up question is filed for the acknowledged gap that structural coverage cannot prove semantic fidelity; the decision honestly states the limit but leaves no tracked item for how manual review scales.
- Privacy, partial-replica, purge and storage-exhaustion clauses extend beyond the literal question; they are consistent with adjacent design text but widen the surface a reviewer must accept as part of resolving Q0161.
- Semantic fidelity depends on thorough operator review; complete structural coverage cannot detect all omissions or distorted qualifications.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261012T022331Z-UBU-Q0161/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261012T022331Z-UBU-Q0161/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261012T022331Z-UBU-Q0161"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
