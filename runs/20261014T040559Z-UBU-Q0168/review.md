# Model-Committee Review

Run: `20261014T040559Z-UBU-Q0168`  
Question: `UBU-Q0168`  
Base commit: `f12f2407c8e4c4a5b77a84a1c6a3b6f0e8727a74`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0168-deadline-boundary-fixtures`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 83.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0168-deadline-boundary-fixtures` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0168-deadline-boundary-fixtures` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0168-deadline-boundary-fixtures` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0168-deadline-boundary-fixtures` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0168-deadline-boundary-fixtures` | `openai` | `codex` | yes | 92 | Mechanical validation confirms clean application, allowed files and preserved question schema. The patch adds precise, auditable deadline-boundary, stream-invalidation and interruption-race qualification requirements without inventing performance claims. It keeps the question Open and preserves related question boundaries. This is useful architectural refinement, with measured acceptance still pending. | None | Controller fixtures are specified but not implemented or executed; enforcement remains unverified., Finite numerical profiles and real-device acceptance evidence remain outstanding, so this patch cannot justify closing UBU-Q0168. |
| `UBU-Q0168-deadline-boundary-fixtures` | `openai` | `claude` | yes | 74 | Patch applies, passes allowlist and schema checks, and edits only DECISIONS.md, DESIGN.md and the UBU-Q0168 entry. It leaves the question Open with metadata intact and explicitly declines to treat the added contract as closure, which matches the question's stated close criteria. The content is directly on-topic: it pins degradation behavior at deadline boundaries, stream freshness invalidation and interruption races, with a per-class fixture table that is auditable and does not invent performance numbers, hardware claims or schema changes. The main limits are that no documentation patch can supply the evidence the question needs, so progress toward closure is small, and that several cross-references and the fit with D0325 cannot be verified from the diff. | Confirm UBU-D0335 is the next unused decision ID at the base commit and that UBU-D0331 is the hardware trial protocol; correct the references if either is wrong., Check the new boundary semantics against the existing UBU-D0325 text in §21.1.4 and cite rather than restate anything D0325 already specifies. | Decision ID UBU-D0335 and the cross-reference to UBU-D0331 as the hardware trial protocol are not verifiable from the diff; a collision or wrong pointer would need a follow-up edit., Adds further unmeasured normative requirements (equality expiry, separate fallback/quiescence/release ceilings, restart rules) to a question whose closure criterion is real-device evidence, so it raises the closure bar without moving toward evidence., Possible overlap or subtle conflict with existing UBU-D0325 text on deadline comparators, first-useful-result deadlines and stream clock basis; the surrounding D0325 wording is not visible in the diff., Introduces an implementation testability constraint (deterministic clock and adapter injection into the actual controller) through documentation while stating no implementation is selected., The new §21.1.4 block is dense and partly restates the adjacent Interruption and Measured enablement gate paragraphs, increasing reading load in an already long section., DECISIONS.md entry header style (Status line with arrow to DESIGN.md section) cannot be checked against sibling entries from the diff context alone. |

## Selected Summary

Add explicit deadline-boundary, stream-invalidation and interruption-race qualification fixtures while keeping numerical latency acceptance open.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Adds further unmeasured normative requirements (equality expiry, separate fallback/quiescence/release ceilings, restart rules) to a question whose closure criterion is real-device evidence, so it raises the closure bar without moving toward evidence.
- Controller fixtures are specified but not implemented or executed; enforcement remains unverified.
- DECISIONS.md entry header style (Status line with arrow to DESIGN.md section) cannot be checked against sibling entries from the diff context alone.
- Decision ID UBU-D0335 and the cross-reference to UBU-D0331 as the hardware trial protocol are not verifiable from the diff; a collision or wrong pointer would need a follow-up edit.
- Finite numerical profiles and real-device acceptance evidence remain outstanding, so this patch cannot justify closing UBU-Q0168.
- Introduces an implementation testability constraint (deterministic clock and adapter injection into the actual controller) through documentation while stating no implementation is selected.
- Possible overlap or subtle conflict with existing UBU-D0325 text on deadline comparators, first-useful-result deadlines and stream clock basis; the surrounding D0325 wording is not visible in the diff.
- The new §21.1.4 block is dense and partly restates the adjacent Interruption and Measured enablement gate paragraphs, increasing reading load in an already long section.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261014T040559Z-UBU-Q0168/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261014T040559Z-UBU-Q0168/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261014T040559Z-UBU-Q0168"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
