# Model-Committee Review

Run: `20261014T034948Z-UBU-Q0166`  
Question: `UBU-Q0166`  
Base commit: `7f27411212ca68ff14369579895854bd0d5a3961`  
Automated selection: valid
Human review required: no
Selected proposal: `openai-ubu-q0166-d0334-normalization-evidence`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 80.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-ubu-q0166-d0334-normalization-evidence` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-ubu-q0166-d0334-normalization-evidence` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-ubu-q0166-d0334-normalization-evidence` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-ubu-q0166-d0334-normalization-evidence` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `openai-ubu-q0166-d0334-normalization-evidence` | `openai` | `codex` | yes | 90 | The patch applies cleanly, changes only allowed documentation and preserves question metadata. Inspected pinned source, harness assertions and saved outputs corroborate qualifier loss during normalization and preservation in direct storage/API controls. It appropriately distinguishes an implementation failure from a governing schema gap and keeps Q0166 Open without expanding schema or authority. This substantively advances the selected evidence work, while the complete mapping and downstream fidelity gates remain outstanding. | None | Synthetic unresolved claim links and storage/API controls that bypass normalization do not establish complete qualifier preservation, rendered review, admission or authorized exchange., The executable harness and retrieved outputs remain temporary artifacts, limiting durable reproducibility. |
| `openai-ubu-q0166-d0334-normalization-evidence` | `openai` | `claude` | yes | 70 | Mechanical validation passed: patch applies, allowlist and question schema pass, only DECISIONS.md, DESIGN.md and the UBU-Q0166 block change. The question metadata line is untouched, Status stays Open and Resolved by stays None, which is the honest outcome given the proposal establishes neither a full pinned mapping nor a host-contract gap. The work is directly responsive to the Current direction, which asked for round-trip evidence from actual validators, serializers, store/review paths; it adds a minimal failing specimen through precondition_review::vet_result, direct storage/reload/replay controls and direct API controls, and carefully distinguishes an implementation loss from a schema gap without overclaiming. It makes no schema, pin, enum or migration change and does not enable an adapter. The main weakness is auditability: the harness was temporary and uncommitted, so every runtime result (including an exact sha256 and a specific source line) is unverifiable from the repo and cannot be re-run. The proposal also surfaces a real defect without creating any tracked follow-up, and the DESIGN prose is heavier than needed. Partial progress with good discipline, held back by non-reproducible evidence. | Preserve the probe harness and the four specimen fixtures (plus the two schema controls) at a pinned revision, or at minimum cite the exact commit SHAs of the schema/core/store/orchestrator checkouts inside D0334, so the vet_result loss and store/API controls are reproducible rather than asserted., Route the reproduced normalization loss (ok status with dropped companion member and swallowed unknown profile) to a tracked follow-up: either a new open question or an explicit implementation-defect reference, so the 'repair or validate that branch' directive has an owner., Trim the DESIGN.md addition: keep the minimal specimen, the table and the limitations paragraph, and move build/dependency version detail into D0334 or a fixture README to reduce drift between design prose and code internals. | The central evidence (vet_result dropping the inline qualifier member while returning ok, SQLite close/reopen/replay controls, direct API handler reads) comes from a temporary harness that is not committed anywhere. The record is therefore prose-only and cannot be re-run from the repository, which sits uneasily with the question's own instruction not to infer host behaviour from prose., None of the runtime claims (specific suppression_key sha256, dependency versions, the exact assignment line in src/services/precondition_review.rs) can be verified from the patch itself; if any detail is wrong it becomes a permanent incorrect entry in DECISIONS.md/DESIGN.md., D0334 identifies a concrete implementation defect (normalization silently discards companion content and unknown profiles, with no diagnostic) but only adds a 'repair or validate that branch' directive in prose with no tracked owner, question or ticket, so the finding may stall., The DESIGN.md §21.1.1 addition is long and dense (table plus several paragraphs) and references an implementation code path that can drift from the design text over time., Pinned checkout revisions are referenced only indirectly ('reused the revisions pinned in UBU-D0323') rather than restated, so D0334 is not self-contained for later audit. |

## Selected Summary

Record actual qualifier loss during pinned precondition-review normalization, with successful direct storage/reload/replay and review API controls; keep Q0166 open.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- D0334 identifies a concrete implementation defect (normalization silently discards companion content and unknown profiles, with no diagnostic) but only adds a 'repair or validate that branch' directive in prose with no tracked owner, question or ticket, so the finding may stall.
- None of the runtime claims (specific suppression_key sha256, dependency versions, the exact assignment line in src/services/precondition_review.rs) can be verified from the patch itself; if any detail is wrong it becomes a permanent incorrect entry in DECISIONS.md/DESIGN.md.
- Pinned checkout revisions are referenced only indirectly ('reused the revisions pinned in UBU-D0323') rather than restated, so D0334 is not self-contained for later audit.
- Synthetic unresolved claim links and storage/API controls that bypass normalization do not establish complete qualifier preservation, rendered review, admission or authorized exchange.
- The DESIGN.md §21.1.1 addition is long and dense (table plus several paragraphs) and references an implementation code path that can drift from the design text over time.
- The central evidence (vet_result dropping the inline qualifier member while returning ok, SQLite close/reopen/replay controls, direct API handler reads) comes from a temporary harness that is not committed anywhere. The record is therefore prose-only and cannot be re-run from the repository, which sits uneasily with the question's own instruction not to infer host behaviour from prose.
- The executable harness and retrieved outputs remain temporary artifacts, limiting durable reproducibility.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261014T034948Z-UBU-Q0166/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261014T034948Z-UBU-Q0166/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261014T034948Z-UBU-Q0166"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
