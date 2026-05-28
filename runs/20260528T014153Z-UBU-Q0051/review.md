# Model-Committee Review

Run: `20260528T014153Z-UBU-Q0051`  
Question: `UBU-Q0051`  
Base commit: `d8f4ea943adb6bb99f9ecdbc8dcb0fb198c5290c`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0051-d0200`

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
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0051-d0200` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0051-d0200` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0051-d0200` | `codex` | `claude` | yes | 88 | Patch applies cleanly per mechanical validation. The proposal directly answers all six subquestions of UBU-Q0051: it specifies six neutral frames (Q1), a strict per-surface presentation budget of 1-2 examples (Q2), explicit admission rules mapping answers to Preferences/Snapshots/Logs/annotations/noncanonical notes (Q3), neutrality rules that prohibit leading language and require escape paths (Q4), a calibration-dimension taxonomy covering all five dimensions named in the question (Q5), and versioned revision/retirement semantics tied to skip signals and user corrections (Q6). Changes are confined to DESIGN.md, DECISIONS.md, and OPEN_QUESTIONS.md — the expected allowlist. No new canonical schema objects are introduced, keeping onboarding lightweight. The decision entry in DECISIONS.md is well-structured with consequences. Minor issue: the `ambiguity_vs_decomposition` frame is not assigned any calibration dimension tag in the dimension list, creating a small inconsistency. Tombstone format is compact but sufficient. Overall a solid, minimal, auditable resolution. | None | Six example frames are a judgment call; the specific set may need revision as Phase 1 testing reveals gaps or redundancy, but the versioning mechanism mitigates this., Calibration dimensions list (`urgent_value`, `emotional_cost`, `social_pressure`, `recovery_value`, `long_term_importance`) omits `ambiguity_vs_decomposition` from the dimension taxonomy even though it is listed as a frame — minor internal inconsistency., OPEN_QUESTIONS.md tombstone drops all subquestion text; future auditors cannot inspect the original question without git history. |

## Selected Summary

Resolve UBU-Q0051 by accepting a small Phase 1 preference-calibration library: six neutral example frames, strict presentation budgets, canonical admission rules, neutrality requirements, and versioned revision/retirement semantics.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Calibration dimensions list (`urgent_value`, `emotional_cost`, `social_pressure`, `recovery_value`, `long_term_importance`) omits `ambiguity_vs_decomposition` from the dimension taxonomy even though it is listed as a frame — minor internal inconsistency.
- OPEN_QUESTIONS.md tombstone drops all subquestion text; future auditors cannot inspect the original question without git history.
- Six example frames are a judgment call; the specific set may need revision as Phase 1 testing reveals gaps or redundancy, but the versioning mechanism mitigates this.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260528T014153Z-UBU-Q0051/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260528T014153Z-UBU-Q0051/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260528T014153Z-UBU-Q0051"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
