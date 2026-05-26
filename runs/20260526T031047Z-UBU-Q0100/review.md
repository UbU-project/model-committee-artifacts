# Model-Committee Review

Run: `20260526T031047Z-UBU-Q0100`  
Question: `UBU-Q0100`  
Base commit: `0c06f1365db83a30cb0382cf159aa8ca93bc255d`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0100-d0165`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 91.0
- Selected cross-score count: 1
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0100-d0165` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0100-d0165` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0100-d0165` | `codex` | `claude` | yes | 91 | The patch applies cleanly per mechanical validation. It directly and completely addresses all five subquestions of UBU-Q0100: the product-invariant set (subquestion 1) maps to 'Product hard boundaries'; external sources (subquestion 2) are captured as 'External hard constraints' with source-labeling requirements; fallible behavioral checks (subquestion 3) are assigned to 'Advisory behavioral safeguards' with explicit rationale for why they must not be unconditional hard gates; user-configured gates (subquestion 4) are given their own category with required record fields; and documentation language rules (subquestion 5) are stated explicitly in both DESIGN.md and DECISIONS.md. The decision text in UBU-D0165 is detailed, internally consistent, and directly aligned with the question's current direction ('structural or externally required hard boundaries; advisory behavioral checks by default'). Scope is tight: only the three directly relevant files are touched, and §2.10.2 and §22.4 are the only DESIGN.md sections modified. Decomposition is good: UBU-Q0101 is left open for advisory-safeguard UX detail, which is the right boundary. No required fixes identified. | None | The four-category taxonomy introduces new governance vocabulary (especially 'user-configured required gates') that will need consistent application across all future documentation and UI copy; drift risk is real., External hard constraints guidance ('if the constraint depends on uncertain interpretation, UbU should disclose uncertainty') sets a behavioral expectation but defers the UX mechanism to no named follow-on question, leaving a potential gap between UBU-Q0100 and UBU-Q0101., The OPEN_QUESTIONS.md status value 'Solved' should be verified against the canonical status vocabulary used elsewhere in the file; other resolved questions may use a different term. |

## Selected Summary

Resolves UBU-Q0100 by adding UBU-D0165 and a four-category safeguard taxonomy: product hard boundaries, external hard constraints, user-configured required gates, and advisory behavioral safeguards.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- External hard constraints guidance ('if the constraint depends on uncertain interpretation, UbU should disclose uncertainty') sets a behavioral expectation but defers the UX mechanism to no named follow-on question, leaving a potential gap between UBU-Q0100 and UBU-Q0101.
- The OPEN_QUESTIONS.md status value 'Solved' should be verified against the canonical status vocabulary used elsewhere in the file; other resolved questions may use a different term.
- The four-category taxonomy introduces new governance vocabulary (especially 'user-configured required gates') that will need consistent application across all future documentation and UI copy; drift risk is real.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260526T031047Z-UBU-Q0100/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260526T031047Z-UBU-Q0100/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260526T031047Z-UBU-Q0100"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
