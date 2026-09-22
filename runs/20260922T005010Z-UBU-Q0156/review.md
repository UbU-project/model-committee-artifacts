# Model-Committee Review

Run: `20260922T005010Z-UBU-Q0156`  
Question: `UBU-Q0156`  
Base commit: `4f4ef5f80a5900ee4bbcd967a50d94e46d8acd9e`  
Automated selection: valid
Human review required: no
Selected proposal: `openai-gpt-5-codex-UBU-Q0156-001`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 88.5
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `openai-gpt-5-codex-UBU-Q0156-001` |
| `work-score` | `claude` | `sonnet` | `openai-gpt-5-codex-UBU-Q0156-001` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `openai-gpt-5-codex-UBU-Q0156-001` |
| `work-score` | `claude` | `sonnet` | `openai-gpt-5-codex-UBU-Q0156-001` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `openai-gpt-5-codex-UBU-Q0156-001` | `openai` | `codex` | yes | 93 | The proposal directly resolves every subquestion: invocation model, boundary types, timestamp representation, PyTorch confirmation, reproducibility, parity testing, fallback/provenance, chunking, and streaming. It keeps PlanningRequest/PlanningResponse as the semantic boundary while allowing worker envelopes only for transport, aligns with CPU certification and fallback constraints, and modifies only the expected design/contract/question files. The normalized patch applies cleanly and the scope is appropriate for an architecture decision. | None | Chooses a persistent worker process contrary to the prior DESIGN.md text, which is intentional for the decision but will require implementation follow-through in the existing scaffold., Adds new contract fields such as delivery_mode and engine_provenance; these are relevant to the question but may require downstream schema/code updates., Tolerance profiles and length-prefixed frame details are named but not fully specified, leaving some implementation detail for later decisions. |
| `openai-gpt-5-codex-UBU-Q0156-001` | `openai` | `claude` | yes | 84 | The proposal directly answers all eight subquestions in UBU-Q0156 with a single coherent resolution (persistent local Python worker, not in-process call or bare subprocess-as-before), and explicitly preserves the 'current direction' constraints (PlanningRequest/PlanningResponse boundary fixed, CPU certification authoritative, TaskSpec unchanged). It stays within the four allowlisted design files, adds one new decision (UBU-D0283), and correctly flips UBU-Q0156 to Solved/Resolved referencing that decision. Content addresses invocation, boundary types (including timestamp representation and envelope vs semantic types), PyTorch confirmation, reproducibility tolerance with CPU-final-authority language, parity testing (exact vs statistical), fallback/provenance recording, chunked search staying within one invocation, and streaming via PlanningStreamFrame — a genuinely complete decomposition of the question with no unrelated scope creep. The one real defect is mechanical: git apply's ordinary mode fails ('corrupt patch at line 190') and the patch only succeeds after --recount normalization, meaning the diff is not a faithful literal patch against the stated base; validation results otherwise record allowlist_passed and patch_applies as true post-normalization. This is a real but recoverable defect, not a schema or scope violation. | Regenerate the patch as a clean diff against the exact base commit so it applies without --recount normalization. | Patch required --recount normalization to apply (ordinary git apply reported 'corrupt patch at line 190'), so it is not a byte-perfect diff against the stated base commit and may need re-generation for clean history., Introduces a fairly large new subsystem (persistent worker, framed PlanningStreamFrame protocol, engine_provenance schema) in one decision; this is substantively justified by the question's eight subquestions but is a lot of new contract surface to review at once., New PLANNING_KERNEL_CONTRACT.md fields (delivery_mode, engine_provenance, PlanningStreamFrame) are additive but not marked with schema-version bump guidance beyond generic versioning language already in the doc. |

## Selected Summary

Resolve UBU-Q0156 by choosing a persistent local Python/PyTorch worker process over framed local pipes as the Phase 1b GPU invocation boundary, while keeping PlanningRequest/PlanningResponse as the semantic contract and CPU certification as final authority.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`
- `PLANNING_KERNEL_CONTRACT.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Adds new contract fields such as delivery_mode and engine_provenance; these are relevant to the question but may require downstream schema/code updates.
- Chooses a persistent worker process contrary to the prior DESIGN.md text, which is intentional for the decision but will require implementation follow-through in the existing scaffold.
- Introduces a fairly large new subsystem (persistent worker, framed PlanningStreamFrame protocol, engine_provenance schema) in one decision; this is substantively justified by the question's eight subquestions but is a lot of new contract surface to review at once.
- New PLANNING_KERNEL_CONTRACT.md fields (delivery_mode, engine_provenance, PlanningStreamFrame) are additive but not marked with schema-version bump guidance beyond generic versioning language already in the doc.
- Patch required --recount normalization to apply (ordinary git apply reported 'corrupt patch at line 190'), so it is not a byte-perfect diff against the stated base commit and may need re-generation for clean history.
- Tolerance profiles and length-prefixed frame details are named but not fully specified, leaving some implementation detail for later decisions.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260922T005010Z-UBU-Q0156/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260922T005010Z-UBU-Q0156/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260922T005010Z-UBU-Q0156"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
