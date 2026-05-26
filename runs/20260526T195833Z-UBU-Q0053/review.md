# Model-Committee Review

Run: `20260526T195833Z-UBU-Q0053`  
Question: `UBU-Q0053`  
Base commit: `1a30743f847b1994c241ad70fc6eb4346b3f93c6`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0053-20260526-001`

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
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0053-20260526-001` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0053-20260526-001` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0053-20260526-001` | `codex` | `claude` | yes | 88 | Mechanical validation passes cleanly. All seven subquestions are addressed: minimum Calendar preview Task trigger and content (before first task in work window, after material regen), minimum Log review Task trigger (end of window / next startup with deviations, weekly catch-up), cadence adjustment via ordinary Task recurrence/snooze, canonical admission rules per object type (Logs, Preferences, Snapshots, Objectives, Tasks, Reports), noncanonical review-note definition, explicit ruling that SDT/TPB constructs (autonomy, competence, relatedness, attitude, subjective norms, perceived control, expected execution) stay as prompt labels and review-note categories rather than canonical fields, and model-repair framing guidance. No new canonical entity types are introduced; the decision reuses existing Phase 1 mechanisms throughout. The OPEN_QUESTIONS.md tombstone preserves all metadata fields in compact form and points to UBU-D0181. The DECISIONS.md entry is well-structured with consequences and cross-references to related open questions (UBU-Q0051, UBU-Q0052, UBU-Q0074). Minor deductions for tombstone loss of subquestion prose and scattered DESIGN.md edits, neither of which is a blocking issue. | None | Tombstone removes the subquestion text, which reduces historical readability for anyone inspecting UBU-Q0053 directly in OPEN_QUESTIONS.md without consulting DECISIONS.md., DESIGN.md edits span three separate hunks (§4 Calendar preview, next-action/log-review inline block, §17.8) introducing psychological annotation rules across multiple sections; a future editor may not know all three need to stay consistent., The closing sentence in §17.8 drops the open-ticket reference to UBU-Q0053 and replaces it with the decision reference UBU-D0181; if the decision is later revised, the DESIGN.md prose may lag. |

## Selected Summary

Resolve Phase 1 Calendar preview and Log review psychological annotation boundaries with a lightweight review-note model, default recurring review cadence, canonical admission rules, and non-blaming prompt framing.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- DESIGN.md edits span three separate hunks (§4 Calendar preview, next-action/log-review inline block, §17.8) introducing psychological annotation rules across multiple sections; a future editor may not know all three need to stay consistent.
- The closing sentence in §17.8 drops the open-ticket reference to UBU-Q0053 and replaces it with the decision reference UBU-D0181; if the decision is later revised, the DESIGN.md prose may lag.
- Tombstone removes the subquestion text, which reduces historical readability for anyone inspecting UBU-Q0053 directly in OPEN_QUESTIONS.md without consulting DECISIONS.md.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260526T195833Z-UBU-Q0053/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260526T195833Z-UBU-Q0053/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260526T195833Z-UBU-Q0053"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
