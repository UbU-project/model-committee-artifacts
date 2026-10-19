# Model-Committee Review

Run: `20261019T041938Z-UBU-Q0162`  
Question: `UBU-Q0162`  
Base commit: `0f700ab4cf63da706da5edeff3f609ab14eafc4b`  
Automated selection: valid
Human review required: no
Selected proposal: `openai-ubu-q0162-human-complete-corrected-history`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 88.5
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-generate` | `claude` | `claude-fable-5-1` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-ubu-q0162-human-complete-corrected-history` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-ubu-q0162-human-complete-corrected-history` |
| `work-score` | `codex` | `gpt-6.1-sol` | `umc-ubu-q0162-correction-scope-contract-2026-10-19` |
| `work-score` | `claude` | `claude-fable-5-1` | `umc-ubu-q0162-correction-scope-contract-2026-10-19` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-generate` | `claude` | `claude-fable-5-1` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `openai-ubu-q0162-human-complete-corrected-history` |
| `work-score` | `claude` | `claude-fable-5-1` | `openai-ubu-q0162-human-complete-corrected-history` |
| `work-score` | `codex` | `gpt-6.1-sol` | `umc-ubu-q0162-correction-scope-contract-2026-10-19` |
| `work-score` | `claude` | `claude-fable-5-1` | `umc-ubu-q0162-correction-scope-contract-2026-10-19` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `openai-ubu-q0162-human-complete-corrected-history` | `openai` | `codex` | yes | 97 | Mechanical validation passes and changes stay within the allowed file and selected question. Inspection of the pinned orchestrator source confirms the independently exposed raw counts and notes. The patch adds precise consumer references and route-level acceptance requirements, preserves correction scope and canonical effects, and keeps the question Open without claiming implementation success. | None | Documentation only: the reproduction, filed remediation ticket and passing implementation evidence remain outstanding. |
| `openai-ubu-q0162-human-complete-corrected-history` | `openai` | `claude` | yes | 80 | Patch applies cleanly, touches only OPEN_QUESTIONS.md within UBU-Q0162, and keeps Status: Open with the closure-evidence and resolution text consistent. It follows the question's established pattern exactly: a dated static audit paragraph naming file/symbol/route references, a remediation-handoff paragraph tied to the existing append-only decision (UBU-D0071) and withdrawal/effect rules, and a required-checks paragraph that is explicitly labeled as requirements rather than passing results. The content is directly responsive to the question's own instruction to identify callers using raw history as current truth, and it correctly notes that repairing `latest_completion` alone would not cover a route that counts raw events and reads raw notes. Semantics are coherent with the rest of the ticket (annotation adds context without changing counts, note-only correction changes only the note, whole-outcome withdrawal does not alter canonical Task status or applied effects, competing corrections require explicit review). The main deduction is that the source claims are unverified here and that the question continues to grow rather than converging on the filed ticket and reproduction the question still lacks. | Before merge, confirm the cited router route, API handler and `report_service` helper names exist at `ubu-orchestrator` 9fddef16990ac89d07a168c75bf04b77c35b9bc3; adjust names if the static audit misnamed any symbol. | The new 2026-10-19 audit cites symbols (`src/router.rs` GET /reports/human-complete, `src/api/reports.rs::human_complete`, `report_service::{human_complete,task_status_counts,notes_from_logs}`, `tests/reports.rs::reports_scaffold_exists`) that this cross-score could not verify against the pinned `ubu-orchestrator` revision 9fddef16; if any name is wrong the remediation handoff points implementers at a non-existent path., UBU-Q0162 is accumulating one static-audit paragraph per consumer and is now very long; each addition increases prompt-size pressure and makes the eventual remediation ticket harder to scope, even though the question's own direction asks to identify every raw-history caller., Two separate paragraphs now instruct extending `tests/reports.rs` (the existing latest_completion paragraph and the new human-complete paragraph), creating mild redundancy that a future consolidation pass should merge. |
| `umc-ubu-q0162-correction-scope-contract-2026-10-19` | `anthropic` | `codex` | yes | 72 | The supplied mechanical validation passes, changes remain confined to the selected question, and Open status and outstanding closure evidence are preserved. The proposal usefully clarifies scope needed by planning, reports and reopen actions without claiming implementation success or operator approval. However, its claimed deterministic contract leaves important withdrawal, chaining and conflict cases underspecified. | Define the concrete scope and replacement-value representation, including whole-assertion withdrawal and the meaning or rejection of path-scoped withdrawal., Specify pointer interpretation for corrections targeting corrections, and exclude immutable links and correction-control metadata from semantic replacement paths., Define overlap and conflict resolution across whole assertions, nested paths and correction chains; add corresponding required fixtures. | Path-scoped withdrawal is permitted but its interpretation and encoding are undefined; only whole-assertion withdrawal receives explicit semantics., Correction-of-correction paths ambiguously address either the correction payload or the underlying assertion. Allowing all event-defined payload paths could also permit reinterpretation of correction links or scope., Conflict rules do not explicitly cover whole-assertion withdrawal versus scoped replacement, ancestor/descendant JSON Pointer overlap, or effective scope across correction chains. |
| `umc-ubu-q0162-correction-scope-contract-2026-10-19` | `anthropic` | `claude` | yes | 70 | The patch applies cleanly, touches only OPEN_QUESTIONS.md, keeps Status Open with all metadata intact, and updates the Resolution in the existing style while marking the contract as awaiting operator approval, which fits the question's human-approval requirement. The gap it identifies is real on the face of the question text: the handoff repeatedly relies on scoped, friction-only, withdrawn, replaced and competing corrections, yet the minimum repair only names `correction_of` and `annotation_of`, so the required planner, report and reopen fixtures have no defined interpretation to assert against. The proposed contract stays within existing event types and the arbitrary JSON payload, keeps legacy unscoped corrections unrewritten, and routes overlapping competing corrections to ordinary review rather than timestamps, all consistent with D0071 and D0248 as the question describes them. Points are withheld because key premises (D0071 content, `event_payload`/`result`/`effective_at` field names, D0248's class name) are asserted without evidence available in the request and partly contradict the recorded `payload` field name, because path-scoped withdraw is left semantically undefined, and because the proposal grows an already oversized question body with a data-model choice that would be more auditable as a separate approval item. | Cite the UBU-D0071 and DESIGN.md text that establishes correction scope is undefined, or soften the claim to what the question already records (the 2026-10-14 audit found no event-specific validation)., Align `corrected_paths` target field names with the recorded Log envelope (`payload` per the NewLogRecord audit) or cite the DESIGN.md section that defines `event_payload`, `result` and `effective_at`., Define corrected-view semantics for a path-scoped `withdraw`, or restrict `withdraw` to the whole assertion so every declared scope has deterministic interpretation., Consider recording the contract as an explicit approval item (candidate decision or dependent question linked under Depends on) rather than only in the minimum-repair prose. | The claim that UBU-D0071 defines only annotation/correction links with optional old/new values is not corroborated by anything in the request; if D0071 or DESIGN.md already defines a scope or kind mechanism, the new contract would conflict with recorded design rather than fill a gap., The contract names target fields `event_payload`, `result` and `effective_at`, but the recorded audits describe the Log request as `id`, `event_type`, `object_refs`, `payload`, `provenance`, `created_at` (NewLogRecord uses `payload`). Field names in the contract may not match the actual envelope, which would make the `corrected_paths` rules unimplementable as written., Path-scoped `withdraw` is permitted (`corrected_paths` may be listed for withdraw) but only whole-assertion withdraw has defined corrected-view semantics; path-scoped withdraw is left undefined, which reintroduces the free-form interpretation the contract is meant to remove., The reference to `UBU-D0248`'s incompatible-intent class asserts specific content of that decision that cannot be verified from the request., A new data-model contract (`correction_kind`, `corrected_paths`) is embedded in an already very long question body instead of a candidate decision or a dependent question, making the approval item easy to lose and further inflating the question text., Author did not run git apply --check and estimated hunk line numbers; mechanical validation reports the patch applies, so this is residual only. |

## Selected Summary

Extend the Log-correction remediation to human-complete report counts and notes.

## Changed Files

- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- A new data-model contract (`correction_kind`, `corrected_paths`) is embedded in an already very long question body instead of a candidate decision or a dependent question, making the approval item easy to lose and further inflating the question text.
- Author did not run git apply --check and estimated hunk line numbers; mechanical validation reports the patch applies, so this is residual only.
- Conflict rules do not explicitly cover whole-assertion withdrawal versus scoped replacement, ancestor/descendant JSON Pointer overlap, or effective scope across correction chains.
- Correction-of-correction paths ambiguously address either the correction payload or the underlying assertion. Allowing all event-defined payload paths could also permit reinterpretation of correction links or scope.
- Documentation only: the reproduction, filed remediation ticket and passing implementation evidence remain outstanding.
- Path-scoped `withdraw` is permitted (`corrected_paths` may be listed for withdraw) but only whole-assertion withdraw has defined corrected-view semantics; path-scoped withdraw is left undefined, which reintroduces the free-form interpretation the contract is meant to remove.
- Path-scoped withdrawal is permitted but its interpretation and encoding are undefined; only whole-assertion withdrawal receives explicit semantics.
- The claim that UBU-D0071 defines only annotation/correction links with optional old/new values is not corroborated by anything in the request; if D0071 or DESIGN.md already defines a scope or kind mechanism, the new contract would conflict with recorded design rather than fill a gap.
- The contract names target fields `event_payload`, `result` and `effective_at`, but the recorded audits describe the Log request as `id`, `event_type`, `object_refs`, `payload`, `provenance`, `created_at` (NewLogRecord uses `payload`). Field names in the contract may not match the actual envelope, which would make the `corrected_paths` rules unimplementable as written.
- The new 2026-10-19 audit cites symbols (`src/router.rs` GET /reports/human-complete, `src/api/reports.rs::human_complete`, `report_service::{human_complete,task_status_counts,notes_from_logs}`, `tests/reports.rs::reports_scaffold_exists`) that this cross-score could not verify against the pinned `ubu-orchestrator` revision 9fddef16; if any name is wrong the remediation handoff points implementers at a non-existent path.
- The reference to `UBU-D0248`'s incompatible-intent class asserts specific content of that decision that cannot be verified from the request.
- Two separate paragraphs now instruct extending `tests/reports.rs` (the existing latest_completion paragraph and the new human-complete paragraph), creating mild redundancy that a future consolidation pass should merge.
- UBU-Q0162 is accumulating one static-audit paragraph per consumer and is now very long; each addition increases prompt-size pressure and makes the eventual remediation ticket harder to scope, even though the question's own direction asks to identify every raw-history caller.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261019T041938Z-UBU-Q0162/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261019T041938Z-UBU-Q0162/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261019T041938Z-UBU-Q0162"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
