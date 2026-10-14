# Model-Committee Review

Run: `20261014T070806Z-UBU-Q0092`  
Question: `UBU-Q0092`  
Base commit: `2938bdec36cab6a41917580384c24fe88a7c5cbf`  
Automated selection: valid
Human review required: no
Selected proposal: `openai-ubu-q0092-typed-evidence-policies`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 82.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-ubu-q0092-typed-evidence-policies` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-ubu-q0092-typed-evidence-policies` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-ubu-q0092-typed-evidence-policies` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `openai-ubu-q0092-typed-evidence-policies` | `openai` | `codex` | no | n/a | codex exited with 1 | None | None |
| `openai-ubu-q0092-typed-evidence-policies` | `openai` | `claude` | yes | 82 | The patch applies cleanly, touches only the three allowed files, and the question block uses Status: Solved with Resolved by: UBU-D0352 while preserving every other metadata field and the Last scored / Scored from commit values. It answers all five subquestions directly: retention is split into orthogonal lifetime, retained form and review dimensions, which correctly separates 'summary only' and 'user review required' from deadline semantics; all six redaction values are defined as maximum-representation ceilings with explicit non-interchangeability and intersection rules; EvidenceUsePolicy covers every listed purpose with deny/allow/require_review decisions and non-transitive scoping; transient excerpt review is specified through bounded leases and a durable-copy prohibition; and EvidenceRef fields cover Compartment, source pinning, selector, timestamp range, generated summary, permission basis and provenance. The decision entry is appropriately scoped and does not overclaim implementation. Deductions are for density and auditability, unverifiable cross-references, the empty-compartment-list semantic, and modest expansion into conformance gating. No decomposition was attempted and none was needed for a question with answerability 90. | State explicitly that an absent `compartment_ids` field fails closed and that only an explicit empty list means low-security, so the field cannot be omitted to bypass protection., Confirm during application that every cited section and decision (§§2.11, 21.3, 23, 22.3.1, DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §18, UBU-D0074, UBU-D0162) exists at the base commit and matches the described semantics., Consider splitting §22.2.1 into sub-headings (retention, redaction, use, EvidenceRef, transient review, enforcement) to keep the section auditable without changing content. | DESIGN.md §22.2.1 is extremely dense; a single section carries enum definitions, parameter rules, intersection semantics, lease handling, purge semantics and conformance gates, which lowers auditability and raises the chance of latent internal contradictions., Cross-references to §§2.11, 21.3, 23, 22.3.1, DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §18, UBU-D0074 and UBU-D0162 could not be verified in this cross-score; if any target is absent or says something different, the contract inherits a dangling or inconsistent citation., `compartment_ids` empty is defined as 'explicitly low-security, not protected'. Unless absent and empty are distinguished at the schema level, an omitted field could be read as a fail-open signal, contrary to the stated fail-closed posture., Adds a `review` purpose not enumerated in subquestion 3 and mandates a Phase 2 `model_training` = `deny` policy; both are reasonable but are substantive policy choices that need the flagged human approval., The conformance-fixture paragraph functions as implementation gating language inside a design section; it is framed as a gate rather than a claim, but it expands the decision's footprint beyond the enum contract., Decision ID UBU-D0352 uniqueness is asserted only by the author's validation notes; a collision with another proposal in the same round would require renumbering. |

## Selected Summary

Define typed Phase 2 Relationship evidence retention, redaction and use policies, source-bound EvidenceRefs and transient excerpt review; resolve UBU-Q0092 through UBU-D0352.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Adds a `review` purpose not enumerated in subquestion 3 and mandates a Phase 2 `model_training` = `deny` policy; both are reasonable but are substantive policy choices that need the flagged human approval.
- Cross-references to §§2.11, 21.3, 23, 22.3.1, DEVICE_SYNC_AND_COMPARTMENT_CONTRACT.md §18, UBU-D0074 and UBU-D0162 could not be verified in this cross-score; if any target is absent or says something different, the contract inherits a dangling or inconsistent citation.
- DESIGN.md §22.2.1 is extremely dense; a single section carries enum definitions, parameter rules, intersection semantics, lease handling, purge semantics and conformance gates, which lowers auditability and raises the chance of latent internal contradictions.
- Decision ID UBU-D0352 uniqueness is asserted only by the author's validation notes; a collision with another proposal in the same round would require renumbering.
- The conformance-fixture paragraph functions as implementation gating language inside a design section; it is framed as a gate rather than a claim, but it expands the decision's footprint beyond the enum contract.
- `compartment_ids` empty is defined as 'explicitly low-security, not protected'. Unless absent and empty are distinguished at the schema level, an omitted field could be read as a fail-open signal, contrary to the stated fail-closed posture.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261014T070806Z-UBU-Q0092/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261014T070806Z-UBU-Q0092/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261014T070806Z-UBU-Q0092"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
