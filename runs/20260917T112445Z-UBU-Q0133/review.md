# Model-Committee Review

Run: `20260917T112445Z-UBU-Q0133`  
Question: `UBU-Q0133`  
Base commit: `7e750c63e5fb3b38bc9cf2f80632642e95481de5`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0133-P-gpt5-codex-20260917`

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
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `UBU-Q0133-P-gpt5-codex-20260917` |
| `work-score` | `claude` | `sonnet` | `UBU-Q0133-P-gpt5-codex-20260917` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `UBU-Q0133-P-gpt5-codex-20260917` |
| `work-score` | `claude` | `sonnet` | `UBU-Q0133-P-gpt5-codex-20260917` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0133-P-gpt5-codex-20260917` | `openai` | `codex` | yes | 92 | The patch is valid, narrow, and directly resolves UBU-Q0133 in the expected files. It answers all subquestions: bounded Handle stability, allowed disclosures and withheld fields, enforcement at the clearance egress filter, acceptable local-only correlation, and mapping from real CompartmentLabel values to opaque restricted outputs. It preserves the question schema and avoids unrelated scope. Remaining concerns are mostly implementation precision around referenced concepts rather than defects in the decision text. | None | Relies on UBU-D0253 and terms like projection window/policy epoch being sufficiently defined elsewhere; if they are not, implementation may still need precision., Omitting compartment_ref by default is strong and appropriate, but policies permitting restricted grouping will need careful tests to avoid pattern leakage. |
| `UBU-Q0133-P-gpt5-codex-20260917` | `openai` | `claude` | yes | 87 | The patch applies cleanly and touches only the allowlisted files OPEN_QUESTIONS.md and DECISIONS.md, per mechanical validation. It directly resolves UBU-Q0133 by adding a new decision (UBU-D0260) that answers all five subquestions: stability granularity (Device + source-object version + projection window), what a Handle may disclose (opaque placeholder only, with surrounding metadata limited to existence/time/duration/status), how redaction-identity is enforced (trusted clearance egress filter evaluates real CompartmentLabel/policy before choosing replication level and emitting only opaque aliases), correlation acceptability (accepts only local UI continuity within one window, rejects durable cross-window/cross-Device/Compartment-pattern correlation), and the mapping of the old three-tier simplification onto real CompartmentLabel values (labels become enforcement-only inputs, never egress vocabulary). It preserves the existing single-line metadata format in OPEN_QUESTIONS.md and appends a properly formatted new decision block to DECISIONS.md consistent with neighboring entries (e.g. UBU-D0259). Scope is appropriately limited to the one question in the prompt, with no unrelated edits or new open questions introduced. The chief risk is unverifiable reliance on cited prior decisions (UBU-D0253, UBU-Q0150) actually existing with the stated content, but that dependency is declared upfront in the question metadata itself, so it is not scope creep. | None | Relies on prior decisions UBU-D0253 and UBU-Q0150 existing and being consistent with the described rotating-alias policy; if those artifacts diverge from this proposal's characterization, the new decision could misrepresent settled precedent., Sets Answerability score to 100 and Status to Solved without an independent second-pass review step, which may be aggressive given 'Human approval required' auto-choice eligibility — though this is flagged via requires_human_review. |

## Selected Summary

Resolve UBU-Q0133 by adopting the UBU-D0253 rotating-alias policy as the Phase 1b Handle egress rule, with redacted object Handles stable only within receiving Device, source-object version, and projection window; Compartment Handles omitted by default; and all real Compartment identity stripped at the clearance egress filter.

## Changed Files

- `OPEN_QUESTIONS.md`
- `DECISIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Omitting compartment_ref by default is strong and appropriate, but policies permitting restricted grouping will need careful tests to avoid pattern leakage.
- Relies on UBU-D0253 and terms like projection window/policy epoch being sufficiently defined elsewhere; if they are not, implementation may still need precision.
- Relies on prior decisions UBU-D0253 and UBU-Q0150 existing and being consistent with the described rotating-alias policy; if those artifacts diverge from this proposal's characterization, the new decision could misrepresent settled precedent.
- Sets Answerability score to 100 and Status to Solved without an independent second-pass review step, which may be aggressive given 'Human approval required' auto-choice eligibility — though this is flagged via requires_human_review.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260917T112445Z-UBU-Q0133/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260917T112445Z-UBU-Q0133/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260917T112445Z-UBU-Q0133"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
