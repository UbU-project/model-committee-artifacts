# Model-Committee Review

Run: `20261012T021004Z-UBU-Q0147`  
Question: `UBU-Q0147`  
Base commit: `b86dfa27597c9e1e87971c7319880c55207ef6b2`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0147-openai-gpt-6-20261011`

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
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0147-openai-gpt-6-20261011` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0147-openai-gpt-6-20261011` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0147-openai-gpt-6-20261011` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0147-openai-gpt-6-20261011` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0147-openai-gpt-6-20261011` | `openai` | `codex` | yes | 95 | Supplied mechanical validation confirms clean application, allowed files, and preserved question schema. The proposal addresses both cycles through independent scheduling inputs, resolution independent of Task lifecycle, and a terminal diagnostic path for review-workflow conflicts. Authority, privacy, causal-parent requirements, and the diagnostic-only Phase 2 baseline remain protected. The remaining implementation work is explicitly gated. | None | Episode identity, partial-frontier reconciliation, and reference/admission semantics require future contract alignment and behavioral validation before enablement. |
| `UBU-Q0147-openai-gpt-6-20261011` | `openai` | `claude` | yes | 84 | The patch applied cleanly and passed allowlist and question-schema validation. The question metadata uses Status: Solved with Resolved by: UBU-D0316 and leaves ranking fields intact. The decision is directly responsive to the causal regress: rule 1 makes Task admission depend only on uncontested inputs and treats the conflict episode as evidence rather than a prerequisite, rule 3 scopes scheduling to validated uncontested capacity with an unplaced fallback, rule 4 supplies a terminal base case by routing any conflict in review-workflow objects to the existing manual_decision diagnostic instead of a review-of-review Task, and rule 5 ensures resolution never depends on the Task or its Plan. Together these cut both the planner loop and the regress, which is exactly what the question asked for. The identity and privacy rules (2 and 6) address the obvious multi-Device failure modes a syncable Task introduces. Scope is limited to the three files one expects for a question resolution and explicitly preserves the diagnostic-only Phase 2 baseline, so it does not overreach into implementation. Deductions are for unverifiable cross-references, the missing forward pointer from the extended contract section, and the density of the DESIGN.md insertion. | None | Cross-references to UBU-D0247, UBU-D0248, and DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §22/§23 could not be verified from the scoring context; if any of them do not say what the decision assumes, the versioning and invalidation hooks in rules 1 and 3 lose their anchor., DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §17 is not touched, so the contract that the decision claims to extend carries no forward pointer to UBU-D0316; a reader of §17 alone will not discover that a future extension is defined., Rule 2 derives Task identity from a canonical descriptor that includes conflicting statement content addresses; the decision acknowledges partial replicas may produce divergent episode ids, which means a user on several Devices can legitimately see more than one review Task for one root conflict until coalescing under policy occurs. This is accepted rather than solved., The DESIGN.md §1 paragraph is dense for an overview section and partially duplicates the decision text; if DECISIONS.md is later revised, the two could drift., The decision is long (six numbered rules plus three gate groups) and the question requires human approval; reviewers must read the whole text to confirm no rule contradicts existing §17 behavior, which raises review cost. |

## Selected Summary

Define future shared manual-review Tasks with independent scheduling and a terminal diagnostic fallback.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Cross-references to UBU-D0247, UBU-D0248, and DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §22/§23 could not be verified from the scoring context; if any of them do not say what the decision assumes, the versioning and invalidation hooks in rules 1 and 3 lose their anchor.
- DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §17 is not touched, so the contract that the decision claims to extend carries no forward pointer to UBU-D0316; a reader of §17 alone will not discover that a future extension is defined.
- Episode identity, partial-frontier reconciliation, and reference/admission semantics require future contract alignment and behavioral validation before enablement.
- Rule 2 derives Task identity from a canonical descriptor that includes conflicting statement content addresses; the decision acknowledges partial replicas may produce divergent episode ids, which means a user on several Devices can legitimately see more than one review Task for one root conflict until coalescing under policy occurs. This is accepted rather than solved.
- The DESIGN.md §1 paragraph is dense for an overview section and partially duplicates the decision text; if DECISIONS.md is later revised, the two could drift.
- The decision is long (six numbered rules plus three gate groups) and the question requires human approval; reviewers must read the whole text to confirm no rule contradicts existing §17 behavior, which raises review cost.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261012T021004Z-UBU-Q0147/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261012T021004Z-UBU-Q0147/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261012T021004Z-UBU-Q0147"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
