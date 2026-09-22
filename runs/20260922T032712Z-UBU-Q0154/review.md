# Model-Committee Review

Run: `20260922T032712Z-UBU-Q0154`  
Question: `UBU-Q0154`  
Base commit: `eff78941c94850ea1f8ec7e3db6dfba1b168477e`  
Automated selection: valid
Human review required: no
Selected proposal: `codex-ubu-q0154-d0286`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 90.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `codex-ubu-q0154-d0286` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0154-d0286` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-5.5` | `n/a` |
| `work-score` | `codex` | `gpt-5.5` | `codex-ubu-q0154-d0286` |
| `work-score` | `claude` | `sonnet` | `codex-ubu-q0154-d0286` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `codex-ubu-q0154-d0286` | `openai` | `codex` | yes | 92 | The proposal directly resolves UBU-Q0154 with a focused decision and updates the relevant design, open-question, and kernel-contract documents. It answers each subquestion: Phase 1b schedule subset, Monday-Friday workday semantics without holiday calendars as a requirement, one evergreen Objective per routine, Objective-local template metadata, direct pre-kernel Task synthesis, deterministic occurrence identity and edit handling, relative `after` lowering, static versus planned placement, completion/miss rollup, and import mapping. The scope is appropriate for a design decision and mechanical validation reports a clean apply with allowed files only. Remaining risks are mostly terminology and implementation-detail tightening rather than blockers. | None | The deterministic occurrence key includes schedule/template/placement versions, so implementations must reliably supersede unstarted cached occurrences on edits to avoid duplicate future instances for the same local anchor., The PLANNING_KERNEL_CONTRACT addition refers to a concrete UTC `window`, while the design language elsewhere uses `allowed_time_range`; if `window` is not the established kernel field name, this should be aligned before implementation., The semantics for reviewed excusals, started-but-not-completed future evidence, and relative occurrence matching are intentionally high level and may need follow-up schema detail during implementation. |
| `codex-ubu-q0154-d0286` | `openai` | `claude` | yes | 88 | The patch applies cleanly per mechanical validation and stays within the allowed-file surface (DECISIONS.md, DESIGN.md, OPEN_QUESTIONS.md, PLANNING_KERNEL_CONTRACT.md). It directly answers all eight subquestions in UBU-Q0154: schedule subset (daily/weekly/day-of-month/first-workday-of-month/first-workday-of-quarter, explicitly deferring holiday calendars), routine-to-Objective mapping with clear field placement, direct instantiation without Technique expansion (deferred to Phase 3 as the question anticipated), instantiation window and deterministic occurrence identity with a defined edit-propagation policy, relative 'after' routines lowered to nominal start/range plus optional dependency edge, static/planned placement with chunk-merge consequences per UBU-D0279, and completion/streak rollup per UBU-D0214. It follows the existing OPEN_QUESTIONS.md status-line schema (Status/Priority/... fields) correctly, updating Status to Solved, Resolved by to UBU-D0286, and Answerability to 100, consistent with the file's conventions. Scope is appropriately bounded to Phase 1b as the question specifies, with explicit non-goals (holiday calendars, minimum-lag kernel edges) called out rather than silently expanded. The PLANNING_KERNEL_CONTRACT.md addition is a small, well-scoped clarification that routine instantiation is pre-kernel, consistent with the existing Phase 3 pre-kernel pattern already documented there. Overall this is a thorough, self-consistent, minimally-scoped resolution. | None | Introduces a fairly large new decision (UBU-D0286) that bundles many sub-decisions (schedule subset, occurrence identity, relative routines, streaks) into one artifact, which could make future amendment harder if any single sub-part needs revision, Deterministic occurrence key formula (Objective id, schedule version, local occurrence anchor, placement mode, template version) is asserted but not cross-referenced against any existing dedup/idempotency decision, so consistency with prior determinism decisions (e.g. UBU-D0213) should be verified by a human reviewer, The 'review candidate' surfacing behavior for conflicting rule edits vs logged history is new terminology not obviously defined elsewhere in DESIGN.md, risking inconsistent usage |

## Selected Summary

Resolve UBU-Q0154 by adding UBU-D0286: Quick UbU routines import as evergreen Objectives with Objective-local routine templates, a narrow Phase 1b schedule subset, direct pre-kernel Task instantiation, deterministic occurrence identity, static/planned placement modes, relative routine lowering, and per-Objective completion/streak rollup.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`
- `PLANNING_KERNEL_CONTRACT.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: review required

## Risks

- Deterministic occurrence key formula (Objective id, schedule version, local occurrence anchor, placement mode, template version) is asserted but not cross-referenced against any existing dedup/idempotency decision, so consistency with prior determinism decisions (e.g. UBU-D0213) should be verified by a human reviewer
- Introduces a fairly large new decision (UBU-D0286) that bundles many sub-decisions (schedule subset, occurrence identity, relative routines, streaks) into one artifact, which could make future amendment harder if any single sub-part needs revision
- The 'review candidate' surfacing behavior for conflicting rule edits vs logged history is new terminology not obviously defined elsewhere in DESIGN.md, risking inconsistent usage
- The PLANNING_KERNEL_CONTRACT addition refers to a concrete UTC `window`, while the design language elsewhere uses `allowed_time_range`; if `window` is not the established kernel field name, this should be aligned before implementation.
- The deterministic occurrence key includes schedule/template/placement versions, so implementations must reliably supersede unstarted cached occurrences on edits to avoid duplicate future instances for the same local anchor.
- The semantics for reviewed excusals, started-but-not-completed future evidence, and relative occurrence matching are intentionally high level and may need follow-up schema detail during implementation.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20260922T032712Z-UBU-Q0154/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20260922T032712Z-UBU-Q0154/commit_message.txt"
```

## Artifact Publication

Operator-run only. Do not execute automatically from model-committee.

```bash
RUN_ID="20260922T032712Z-UBU-Q0154"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
