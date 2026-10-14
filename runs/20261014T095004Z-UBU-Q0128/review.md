# Model-Committee Review

Run: `20261014T095004Z-UBU-Q0128`  
Question: `UBU-Q0128`  
Base commit: `8c819c613a58cf6513d2930cc8513be5d9871df7`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0128-profile-qualification-d0365`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

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
| `work-score` | `claude` | `claude-fable-5-1` | `codex-ubu-q0128-profile-qualification-d0365` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `claude` | `claude-fable-5-1` | `codex-ubu-q0128-profile-qualification-d0365` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0128-profile-qualification-d0365` | `codex` | `claude` | yes | 82 | Patch passes mechanical validation, touches only DECISIONS.md, DESIGN.md and OPEN_QUESTIONS.md, and follows the repo's established decision pattern (new D-record, §21.1.2 design text, Current-direction entry, Resolution sentence). It is directly responsive to an explicitly listed open slice of UBU-Q0128 (quantitative mode profiles) and narrows subquestions 2, 12, 16 and 19 without choosing numeric defaults, asserting safety results, altering either v1 schema, or expanding action permissions. Question metadata line is unchanged and Status stays Open with 'Resolved by: None', so schema is preserved. The content is internally consistent with prior direction: comprehension and burden are measured in review rather than diagnosed in-turn, resting carries no productivity floor, and enforcement must hold independently of outcome scores. Deductions are for unverifiable section cross-references, the potentially heavy re-trial burden tied to every revision change, and the slightly odd placement of the block before 'Initial permissions'. No blocking fixes identified. | None | DESIGN.md text cites §§21.1.3–21.1.4 and §4.2 as existing evidence/MVP-minimum sections; these cross-references are not visible in the prompt and should be confirmed to resolve before merge., Binding every profile to STT/TTS/model/template revisions and requiring new affected trials on any cap, template, speech-rate or rubric change creates a heavy re-qualification burden; combined with 'withhold unmeasured combinations', this could in practice gate all restricted-attention profiles behind human-factors trials even though the decision states it adds no MVP-release requirement., The new qualification block is inserted between the serialized-budget paragraph and the 'Initial permissions' paragraph in §21.1.2, so the method for qualifying numerical profiles now precedes the description of the envelope it qualifies; ordering is slightly awkward for readers., Decision ID UBU-D0365 is appended at the end of DECISIONS.md without visible verification of the preceding ID; collision risk if another concurrent proposal claims the same number., The DESIGN.md addition is dense (five paragraphs plus a table) for a method-only decision; consistent with repo style but increases audit effort. |

## Selected Summary

Define evidence-based selection of restricted-attention numerical profiles using comprehension, primary-activity performance and cumulative burden.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Binding every profile to STT/TTS/model/template revisions and requiring new affected trials on any cap, template, speech-rate or rubric change creates a heavy re-qualification burden; combined with 'withhold unmeasured combinations', this could in practice gate all restricted-attention profiles behind human-factors trials even though the decision states it adds no MVP-release requirement.
- DESIGN.md text cites §§21.1.3–21.1.4 and §4.2 as existing evidence/MVP-minimum sections; these cross-references are not visible in the prompt and should be confirmed to resolve before merge.
- Decision ID UBU-D0365 is appended at the end of DECISIONS.md without visible verification of the preceding ID; collision risk if another concurrent proposal claims the same number.
- The DESIGN.md addition is dense (five paragraphs plus a table) for a method-only decision; consistent with repo style but increases audit effort.
- The new qualification block is inserted between the serialized-budget paragraph and the 'Initial permissions' paragraph in §21.1.2, so the method for qualifying numerical profiles now precedes the description of the envelope it qualifies; ordering is slightly awkward for readers.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261014T095004Z-UBU-Q0128/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261014T095004Z-UBU-Q0128/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261014T095004Z-UBU-Q0128"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
