# Model-Committee Review

Run: `20260528T011808Z-UBU-Q0040`  
Question: `UBU-Q0040`  
Base commit: `2775280826b20e1f0f3da48fb8afc450b76ff159`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0040-d0199`

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
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0040-d0199` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0040-d0199` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0040-d0199` | `codex` | `claude` | yes | 88 | The patch cleanly applies and directly addresses all 12 subquestions in UBU-Q0040. DESIGN.md §3.6 gains a concrete definition of design burden with four named components and distinguishes valid from invalid decomposition. §3.7 gains the hard-gate / decomposition-path distinction that answers subquestion 12. DECISIONS.md adds UBU-D0199 with full consequence enumeration. OPEN_QUESTIONS.md converts Q0040 to a compact solved tombstone pointing to UBU-D0199. The patch is minimal, touches only the three appropriate documentation files, and is directly responsive to the selected question. The main gap is that the relationship between UBU-D0068 and UBU-D0199 is implicit rather than stated, and quantitative burden scoring remains deferred, but neither constitutes a required fix given the question's resolution scope. | None | UBU-D0068 was listed as a prior partial resolution in the original question header; the patch replaces it with UBU-D0199 in the tombstone without explicitly stating D0068 is superseded, which may create ambiguity in the decision log., The burden-component list (dependency, ambiguity, automation, implementation) is defined in prose but has no quantitative weights or normalization rules, leaving scoring open to interpretation until future parser or tooling work lands., Lineage metadata (Decomposes: UBU-Qxxxx) is deferred until parser support exists; in the interim, parent questions remain open, which means the rule has a gap during which decomposition lineage is tracked only informally. |

## Selected Summary

Resolve UBU-Q0040 by defining design-burden scoring, answerability gating, valid decomposition criteria, lineage expectations, and stop-rule interaction for model-committee question decomposition.

## Changed Files

- `DESIGN.md`
- `DECISIONS.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Lineage metadata (Decomposes: UBU-Qxxxx) is deferred until parser support exists; in the interim, parent questions remain open, which means the rule has a gap during which decomposition lineage is tracked only informally.
- The burden-component list (dependency, ambiguity, automation, implementation) is defined in prose but has no quantitative weights or normalization rules, leaving scoring open to interpretation until future parser or tooling work lands.
- UBU-D0068 was listed as a prior partial resolution in the original question header; the patch replaces it with UBU-D0199 in the tombstone without explicitly stating D0068 is superseded, which may create ambiguity in the decision log.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260528T011808Z-UBU-Q0040/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260528T011808Z-UBU-Q0040/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260528T011808Z-UBU-Q0040"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
