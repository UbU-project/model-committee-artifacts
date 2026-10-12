# Model-Committee Review

Run: `20261012T010721Z-UBU-Q0128`  
Question: `UBU-Q0128`  
Base commit: `80e2b4ee822b5e1442aa3160bb52b80c6944c62d`  
Automated selection: valid
Human review required: no
Selected proposal: `openai-ubu-q0128-interaction-budget-v1`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 91.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-ubu-q0128-interaction-budget-v1` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-ubu-q0128-interaction-budget-v1` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-ubu-q0128-interaction-budget-v1` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-ubu-q0128-interaction-budget-v1` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `openai-ubu-q0128-interaction-budget-v1` | `openai` | `codex` | yes | 96 | Reported mechanical validation confirms clean application, allowed files and preserved question schema. The patch directly addresses the budget serialization gap with closed shapes, explicit units, controller-owned bindings, expiry and cumulative turn accounting. It preserves candidate admission, scoped media permissions and consequential-action deferral, keeps UBU-Q0128 Open and makes no implementation claim. | None | Quantitative profile ceilings and usability/driving-safety validation remain open; serialization alone does not establish safe operating values., Freshness checks, cumulative counters and immediate local controls require runtime enforcement that this documentation patch does not implement. |
| `openai-ubu-q0128-interaction-budget-v1` | `openai` | `claude` | yes | 86 | The patch applies cleanly, passes the allowlist and question-schema checks, and leaves UBU-Q0128 `Status: Open` with its metadata line untouched. It targets exactly one of the items the Resolution lists as open, the exact serialized budget, and answers it with a closed JSON Schema plus controller-side semantic checks (freshness, revision binding, turn-owned counters, high-risk invariants). The `allowed_operations` enum matches the D0309 initial permissions exactly, so the envelope is not widened. It consistently removes 'the serialized budget' from the open-item lists in DESIGN.md and OPEN_QUESTIONS.md and adds a matching D0311 paragraph and Resolution sentence. Deductions are for density, the local_controls versus manual_availability wording gap, a coarse locomotion-risk enum that leaks into profile design, and the fail-closed 32-item reference caps. No blocking fixes. | Add one sentence clarifying that `local_controls` denotes hands-free or single-action physical/voice stop controls, so `manual_availability = none` plus mandatory `local_controls` is not contradictory., Optionally add a one-line note to the D0310 record (or a 'refined by UBU-D0311' pointer) so DECISIONS.md does not still list the serialized budget as open. | The DESIGN.md block is dense: a 60-line JSON Schema plus seven long semantic bullets in one hunk, which makes human audit of the normative rules harder than the surrounding D0309/D0310 text., Mild semantic tension: high-risk modes require `manual_availability = none`, yet `local_controls` is a mandatory input channel and local pause/stop/silence is required to remain available. The text never says what a local control is when hands are unavailable (steering-wheel button, voice 'stop', watch tap)., `locomotion_risk` has only low/high/unknown and high/unknown collapses to the driving envelope, so walking/running profiles must be classed as `low` to get any menu or clarification allowance; this pre-empts part of the still-open quantitative profile work., `policy_versions`/`grant_versions` are capped at 32 with fail-closed overflow; a session spanning many Compartments or grants could be unable to issue any budget., Byte caps on the complete normalized model invocation including tool declarations and grammar are provider-dependent to measure; the decision disclaims implementation, but the rule reads as if it is mechanically checkable., The historical D0310 record in DECISIONS.md still lists 'the serialized budget' as open; the patch updates DESIGN.md and OPEN_QUESTIONS.md but not that sentence. Acceptable if decision records are treated as immutable, but a reader of DECISIONS.md alone sees a stale open-item list., Fixed `const` values for `confirmation_policy` and `deferral_policy` mean any widening of the envelope requires a schema version bump; intentional, but rigid., I could not inspect the UbU repository at the base commit, so consistency with other DESIGN.md §21.1.2 text outside the shown hunks relies on the mechanical validation and the author's notes. |

## Selected Summary

Specify InteractionBudgetV1 serialization, finite resource units, freshness checks and controller enforcement for the existing restricted-attention envelope.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Byte caps on the complete normalized model invocation including tool declarations and grammar are provider-dependent to measure; the decision disclaims implementation, but the rule reads as if it is mechanically checkable.
- Fixed `const` values for `confirmation_policy` and `deferral_policy` mean any widening of the envelope requires a schema version bump; intentional, but rigid.
- Freshness checks, cumulative counters and immediate local controls require runtime enforcement that this documentation patch does not implement.
- I could not inspect the UbU repository at the base commit, so consistency with other DESIGN.md §21.1.2 text outside the shown hunks relies on the mechanical validation and the author's notes.
- Mild semantic tension: high-risk modes require `manual_availability = none`, yet `local_controls` is a mandatory input channel and local pause/stop/silence is required to remain available. The text never says what a local control is when hands are unavailable (steering-wheel button, voice 'stop', watch tap).
- Quantitative profile ceilings and usability/driving-safety validation remain open; serialization alone does not establish safe operating values.
- The DESIGN.md block is dense: a 60-line JSON Schema plus seven long semantic bullets in one hunk, which makes human audit of the normative rules harder than the surrounding D0309/D0310 text.
- The historical D0310 record in DECISIONS.md still lists 'the serialized budget' as open; the patch updates DESIGN.md and OPEN_QUESTIONS.md but not that sentence. Acceptable if decision records are treated as immutable, but a reader of DECISIONS.md alone sees a stale open-item list.
- `locomotion_risk` has only low/high/unknown and high/unknown collapses to the driving envelope, so walking/running profiles must be classed as `low` to get any menu or clarification allowance; this pre-empts part of the still-open quantitative profile work.
- `policy_versions`/`grant_versions` are capped at 32 with fail-closed overflow; a session spanning many Compartments or grants could be unable to issue any budget.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261012T010721Z-UBU-Q0128/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261012T010721Z-UBU-Q0128/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261012T010721Z-UBU-Q0128"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
