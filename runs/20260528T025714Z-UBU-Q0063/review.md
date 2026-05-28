# Model-Committee Review

Run: `20260528T025714Z-UBU-Q0063`  
Question: `UBU-Q0063`  
Base commit: `4ba082d282b791e97fd46d6e753e4327e21abe51`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0063-org-introspection-mvp`

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
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0063-org-introspection-mvp` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0063-org-introspection-mvp` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0063-org-introspection-mvp` | `codex` | `claude` | yes | 88 | Patch applies cleanly per mechanical validation. All three changed files are within the allowed set. DESIGN.md §4.1.4 is expanded with a well-structured minimum evidence set, minimum outputs, core review prompt, public-artifact safety constraints, and explicit acceptance criteria that directly answer all six subquestions. DECISIONS.md gains UBU-D0204 with status, resolved question reference, accepted constraints, and consequences — correctly formatted per the existing decision schema. OPEN_QUESTIONS.md converts UBU-Q0063 to a compact tombstone pointing at UBU-D0204. The patch stays strictly within Phase 1 scope, avoids Association automation, avoids contact surveillance, and does not introduce new objects or data types beyond what already exists. Minor nits: the tombstone line lacks a trailing blank line before the separator (cosmetic), and the metadata header does not update Scored from commit to reflect resolution, but neither is a blocking issue. No decomposition was needed for a single-question resolution. Human approval is correctly flagged. | None | OPEN_QUESTIONS.md tombstone collapses the full question block into a single-line status header with no blank line before the closing separator, which may cause formatting inconsistency with other tombstoned questions, Resolved by field in the tombstone header references UBU-D0204 but the question metadata line omits any updated Last scored / Scored from commit values that reflect the resolution commit |

## Selected Summary

Resolve UBU-Q0063 by defining the Phase 1 organizational-introspection dogfooding workflow as a manual, evidence-backed EthConf/outreach retrospective over existing UbU artifacts, with public-safe outputs and acceptance criteria.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- OPEN_QUESTIONS.md tombstone collapses the full question block into a single-line status header with no blank line before the closing separator, which may cause formatting inconsistency with other tombstoned questions
- Resolved by field in the tombstone header references UBU-D0204 but the question metadata line omits any updated Last scored / Scored from commit values that reflect the resolution commit

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260528T025714Z-UBU-Q0063/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260528T025714Z-UBU-Q0063/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260528T025714Z-UBU-Q0063"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
