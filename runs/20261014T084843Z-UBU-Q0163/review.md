# Model-Committee Review

Run: `20261014T084843Z-UBU-Q0163`  
Question: `UBU-Q0163`  
Base commit: `42f89707c3213bb8599ad62dfa9f9a5fd0c6953c`  
Automated selection: valid
Human review required: no
Selected proposal: `openai-UBU-Q0163-cancellation-remediation-2026-10-14`

## Disagreement Flags

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 82.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-UBU-Q0163-cancellation-remediation-2026-10-14` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-UBU-Q0163-cancellation-remediation-2026-10-14` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-UBU-Q0163-cancellation-remediation-2026-10-14` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-UBU-Q0163-cancellation-remediation-2026-10-14` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `openai-UBU-Q0163-cancellation-remediation-2026-10-14` | `openai` | `codex` | yes | 90 | Mechanical validation confirms applicability, allowed-file scope and schema preservation. The documentation provides an actionable cancellation repair handoff covering transport, durable bindings, admission and replay safety. It preserves protection, operator approval and remaining retirement gates. Its contribution is implementation planning; verification remains outstanding. | None | The remediation ticket remains unfiled, and no behavioral evidence advances blocker-retirement eligibility. |
| `openai-UBU-Q0163-cancellation-remediation-2026-10-14` | `openai` | `claude` | yes | 74 | The patch applies cleanly, touches only the UBU-Q0163 section of OPEN_QUESTIONS.md, keeps Status: Open and all metadata intact, adds no decisions or questions, and claims no results it did not obtain. It is directly responsive to the question's own current direction, which asked for the missing production path to be recorded in a remediation ticket; the proposal narrows that ticket to explicit occurrence cancellation, names the transport and binding seams that would need to change, and ties acceptance back to the existing slice. The prose matches the house style and preserves the key safety invariants (absence is not cancellation, no external deletion, no Log rewriting, ordinary admission only). It is docked for relying on new source-path and decision-ID claims that this scorer could not verify, for being specification-only on a question that now needs executed evidence more than more requirements, and for adding length to an already oversized entry. | Confirm against the pinned 9fddef16 checkout that calendar_client.rs::CalendarApi::list_events, calendar_google::GoogleCalendarApi::list_events and calendar_projection::DesiredEvent match the described signatures and fields before merging the citation., Verify that UBU-D0258 and UBU-D0259 exist in DECISIONS.md and actually specify the mutation-envelope and lifecycle paths the section relies on; otherwise replace or drop the reference., Consider folding the added negative controls (duplicate occurrence ids across accounts, unbound legacy Task, missing ordering evidence) into the acceptance table as rows rather than a separate prose paragraph to keep one authoritative checklist. | The new 2026-10-14 static-inspection claims (CalendarApi::list_events returning only Vec<DesiredEvent>, DesiredEvent lacking account/calendar scope and ordering, by_source keyed on bare source_id) could not be independently verified in this scoring pass; if any path or type name is wrong the question would carry an incorrect source citation., The remediation section cross-references UBU-D0258/UBU-D0259 as the mutation-envelope and lifecycle paths; if those decision IDs do not exist or do not cover admission as described, the handoff points implementers at the wrong policy., The proposal adds roughly a dozen dense lines to an already very long question, deepening an unfiled 'proposed ticket' layer rather than executing any check; repeated rounds of specification-only edits risk the question accumulating requirements nobody runs., Minor overlap between the new 'Extend the bounded slice' paragraph and the existing acceptance table (negative controls restated in prose) could drift out of sync on later edits. |

## Selected Summary

Specify a bounded explicit-cancellation repair across wire delivery, durable source bindings, ordinary admission and recapture/projection.

## Changed Files

- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Minor overlap between the new 'Extend the bounded slice' paragraph and the existing acceptance table (negative controls restated in prose) could drift out of sync on later edits.
- The new 2026-10-14 static-inspection claims (CalendarApi::list_events returning only Vec<DesiredEvent>, DesiredEvent lacking account/calendar scope and ordering, by_source keyed on bare source_id) could not be independently verified in this scoring pass; if any path or type name is wrong the question would carry an incorrect source citation.
- The proposal adds roughly a dozen dense lines to an already very long question, deepening an unfiled 'proposed ticket' layer rather than executing any check; repeated rounds of specification-only edits risk the question accumulating requirements nobody runs.
- The remediation section cross-references UBU-D0258/UBU-D0259 as the mutation-envelope and lifecycle paths; if those decision IDs do not exist or do not cover admission as described, the handoff points implementers at the wrong policy.
- The remediation ticket remains unfiled, and no behavioral evidence advances blocker-retirement eligibility.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261014T084843Z-UBU-Q0163/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261014T084843Z-UBU-Q0163/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261014T084843Z-UBU-Q0163"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
