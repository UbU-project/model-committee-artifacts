# Model-Committee Review

Run: `20261019T055335Z-UBU-Q0163`  
Question: `UBU-Q0163`  
Base commit: `eb861e919fecf938de4defc50d2e208a61610dba`  
Automated selection: valid
Human review required: no
Selected proposal: `openai-ubu-q0163-u3-capture-path-20261019`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 74.5
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-generate` | `claude` | `claude-fable-5-1` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-ubu-q0163-u3-capture-path-20261019` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-ubu-q0163-u3-capture-path-20261019` |
| `work-score` | `codex` | `gpt-6.1-sol` | `ubu-q0163-out-of-window-move-requalification-20261019` |
| `work-score` | `claude` | `claude-fable-5-1` | `ubu-q0163-out-of-window-move-requalification-20261019` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-generate` | `claude` | `claude-fable-5-1` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-ubu-q0163-u3-capture-path-20261019` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-ubu-q0163-u3-capture-path-20261019` |
| `work-score` | `codex` | `gpt-6.1-sol` | `ubu-q0163-out-of-window-move-requalification-20261019` |
| `work-score` | `claude` | `claude-fable-5-1` | `ubu-q0163-out-of-window-move-requalification-20261019` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `openai-ubu-q0163-u3-capture-path-20261019` | `openai` | `codex` | yes | 79 | Implements its narrow documentation refinement accurately. The inspected revision, hashes, ownership predicate and Dynamic/Static branches support the trace. The proposed regression distinguishes parser diagnostics from capture diagnostics and preserves valid expanded instances. Mechanical validation passes, changes remain confined to the allowed question file, and Open status and operator-approval gates remain intact. Credit is limited because the parser repair and behavioral verification remain outstanding. | None | The capture consequences remain static predictions; no new capture, admission, occupancy or coverage result is established., Further handoff elaboration offers limited progress where the question already prioritizes implementation and executed evidence. |
| `openai-ubu-q0163-u3-capture-path-20261019` | `openai` | `claude` | yes | 70 | The patch applies cleanly, touches only the UBU-Q0163 section of OPEN_QUESTIONS.md, keeps the single-line Open metadata and Resolution intact, and passed mechanical validation. It is directly responsive: it extends the executed U3 parser finding one step downstream, predicts that the accepted recurrence parent would become a capacity-bearing Dynamic candidate beside the Static instance, pins the inspected source files, and sharpens the existing handoff with a raw-wire-to-capture regression covering both colour branches. The framing is honest about being static and about not establishing admission, occupancy or projection results, and it removes no blocker. Value is moderate rather than high: nothing was executed, the new regression requirement is added on prediction alone, and two reproducibility details (palette mapping for `colorId: "3"`, test reachability of `plan_capture`) are left unstated. Within the established documentation-only pattern for this question it is a sound, auditable, bounded increment. | State the palette assumption for the `colorId: "3"` case (default palette or an explicit mapping supplied by the test) so the Static-branch regression is reproducible with the stated empty palette., Confirm `plan_capture` is a public entry point reachable from `tests/`, or name the public call the regression should use., Prefer executing the predicted capture-path regression and recording its observed outcome, matching the executed U3 parser regression, rather than adding a prediction-only requirement. | The capture-path trace rests on static inspection only: the ownable-id predicate for `q0163series`, the absent-`colorId` Dynamic branch and the `Dynamic { seconds: 3600 }` candidate are predictions, not executed results. If any is wrong, the canonical question records an incorrect predicted failure under revision pins that make it look authoritative., The handoff now requires a second regression (`parse_event_list` to `plan_capture` in `tests/calendar_occupancy.rs`) justified by static evidence, which sits in tension with the question's own rule to add specification only where an implementation result exposes a gap., The regression spec says to use an empty palette/source map yet relies on `colorId: "3"` to reach the Static branch; unless a default palette maps that color, the Static-branch case may be unreachable as written., It assumes `plan_capture` is callable from an integration test the way `parse_event_list` was; if it is not public, the regression cannot live where the handoff places it., The Q0163 section is already very large; this adds more unexecuted prose without closing any gate, increasing prompt size for later rounds. |
| `ubu-q0163-out-of-window-move-requalification-20261019` | `anthropic` | `codex` | yes | 58 | Mechanical validation confirms clean application, allowed files and preserved question schema. The patch identifies a relevant edge case, retains protection and clearly disclaims executed verification. However, it expands accepted specification and adds an exclusive read requirement instead of advancing the explicitly identified U3 implementation and coverage work. | Prioritize the existing U3 repair and wire-to-capture evidence handoff; justify additional specification with revision-pinned implementation evidence before expanding UBU-D0318., Allow any qualified read supplying scoped occurrence identity, verifiable ordering and ordinary-admission bindings; present targeted reads as one possible implementation., If retaining the move slice, qualify its provider delivery assumptions and update the ticket and acceptance descriptions to distinguish cancellation evidence from move evidence. | Adds specification without the implementation result required by the existing Resolution, while the concrete U3 repair and capture/coverage check remain outstanding., Making targeted per-occurrence reads the only admissible evidence unnecessarily excludes other qualified authoritative read paths, including the existing optional cursor path., The acceptance row requires unverified provider delivery behavior and leaves the cancellation-only description inconsistent with the newly added move coverage. |
| `ubu-q0163-out-of-window-move-requalification-20261019` | `anthropic` | `claude` | yes | 72 | The patch applies cleanly and passes allowlist and question-schema checks; Status stays Open and the metadata line is untouched. It is directly responsive: UBU-D0318 requires moves to supersede prior reservations, the only mapped assertion covers in-window moves, and the recorded facts (timeMin/timeMax-bounded list_request, reconcile emitting capture_source_removed without retirement, no cancellation entry for a moved instance's original slot) do imply that an out-of-window move is indistinguishable from omission. Recording that gap, adding a bounded row to the existing cancellation slice rather than a new ticket, and updating Current direction and Resolution is minimal and auditable, and the text is honest that nothing was executed. Deductions: it adds specification without an implementation result, against the question's own Resolution guidance; it edits adopted DESIGN.md decision text directly; and the 'only admissible source' wording is over-tight and inconsistent with the already-specified optional cursor path, which would deliver the moved instance. Those are fixable wording and review issues rather than structural defects. | Reword 'as the only admissible source' and 'Only a targeted per-occurrence read ... may supersede' to admit any qualified identity-bound read of the occurrence's current state (targeted per-occurrence read or qualified incremental-sync delivery), so the optional cursor path in the Google read-path section is not excluded., Human reviewer must confirm the DESIGN.md UBU-D0318 sentence amendment, or record it per the project's decision-amendment convention, before merge., State in the new row that A's former interval becomes free because the reservation was superseded by a move, and that probe expectations are move-specific rather than inherited from the cancellation row. | Edits adopted UBU-D0318 text in DESIGN.md directly, without an amendment record or new decision id; the proposal itself flags this as needing human confirmation., Adds specification from static deduction rather than from an implementation result, which conflicts with the question's own Resolution guidance ('add specification only where an implementation result exposes a gap'); the move-out-of-window gap is plausible and follows from recorded facts, but was not reproduced., The new paragraph and row make a targeted per-occurrence read 'the only admissible source' of an absent occurrence's current state. That wording conflicts with the question's existing optional cursor/syncToken path, under which a moved instance is delivered regardless of timeMin/timeMax; as written it could forbid an otherwise qualified read from admitting a move., Google per-event delivery for moved or cancelled instance ids was not checked (acknowledged). The row assumes a targeted read can supply verifiable ordering, while the sparse-evidence section already warns that updated/etag fields cannot be assumed present., The row closes with 'the admitted-cancellation probes apply', reusing cancellation probe expectations for a move; acceptable for the in-horizon outcome but could conflate move and cancellation admission outcomes., 'Finite per-capture budget' is left unspecified; a large absent set (for example after a series truncation) would leave scope uncertified, and the interaction with the series-truncation follow-up is only implied., No tools were available to the author; hunk line numbers were estimated. Mechanical validation confirms the patch applies, so this is residual rather than blocking. |

## Selected Summary

Trace the accepted U3 recurrence parent into flexible work during capture and sharpen the existing rejection repair.

## Changed Files

- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- 'Finite per-capture budget' is left unspecified; a large absent set (for example after a series truncation) would leave scope uncertified, and the interaction with the series-truncation follow-up is only implied.
- Adds specification from static deduction rather than from an implementation result, which conflicts with the question's own Resolution guidance ('add specification only where an implementation result exposes a gap'); the move-out-of-window gap is plausible and follows from recorded facts, but was not reproduced.
- Adds specification without the implementation result required by the existing Resolution, while the concrete U3 repair and capture/coverage check remain outstanding.
- Edits adopted UBU-D0318 text in DESIGN.md directly, without an amendment record or new decision id; the proposal itself flags this as needing human confirmation.
- Further handoff elaboration offers limited progress where the question already prioritizes implementation and executed evidence.
- Google per-event delivery for moved or cancelled instance ids was not checked (acknowledged). The row assumes a targeted read can supply verifiable ordering, while the sparse-evidence section already warns that updated/etag fields cannot be assumed present.
- It assumes `plan_capture` is callable from an integration test the way `parse_event_list` was; if it is not public, the regression cannot live where the handoff places it.
- Making targeted per-occurrence reads the only admissible evidence unnecessarily excludes other qualified authoritative read paths, including the existing optional cursor path.
- No tools were available to the author; hunk line numbers were estimated. Mechanical validation confirms the patch applies, so this is residual rather than blocking.
- The Q0163 section is already very large; this adds more unexecuted prose without closing any gate, increasing prompt size for later rounds.
- The acceptance row requires unverified provider delivery behavior and leaves the cancellation-only description inconsistent with the newly added move coverage.
- The capture consequences remain static predictions; no new capture, admission, occupancy or coverage result is established.
- The capture-path trace rests on static inspection only: the ownable-id predicate for `q0163series`, the absent-`colorId` Dynamic branch and the `Dynamic { seconds: 3600 }` candidate are predictions, not executed results. If any is wrong, the canonical question records an incorrect predicted failure under revision pins that make it look authoritative.
- The handoff now requires a second regression (`parse_event_list` to `plan_capture` in `tests/calendar_occupancy.rs`) justified by static evidence, which sits in tension with the question's own rule to add specification only where an implementation result exposes a gap.
- The new paragraph and row make a targeted per-occurrence read 'the only admissible source' of an absent occurrence's current state. That wording conflicts with the question's existing optional cursor/syncToken path, under which a moved instance is delivered regardless of timeMin/timeMax; as written it could forbid an otherwise qualified read from admitting a move.
- The regression spec says to use an empty palette/source map yet relies on `colorId: "3"` to reach the Static branch; unless a default palette maps that color, the Static-branch case may be unreachable as written.
- The row closes with 'the admitted-cancellation probes apply', reusing cancellation probe expectations for a move; acceptable for the in-horizon outcome but could conflate move and cancellation admission outcomes.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261019T055335Z-UBU-Q0163/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261019T055335Z-UBU-Q0163/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261019T055335Z-UBU-Q0163"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
