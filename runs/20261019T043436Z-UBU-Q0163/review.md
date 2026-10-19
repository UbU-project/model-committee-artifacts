# Model-Committee Review

Run: `20261019T043436Z-UBU-Q0163`  
Question: `UBU-Q0163`  
Base commit: `d11993a598d1fa3fdc636bc3f5bc5032a7b0e354`  
Automated selection: valid
Human review required: no
Selected proposal: `UBU-Q0163-openai-unexpanded-parent-regression-20261019`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 87.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-generate` | `claude` | `claude-fable-5-1` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0163-openai-unexpanded-parent-regression-20261019` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0163-openai-unexpanded-parent-regression-20261019` |
| `work-score` | `codex` | `gpt-6.1-sol` | `umc-ubu-q0163-retirement-closure-ledger-2026-10-19` |
| `work-score` | `claude` | `claude-fable-5-1` | `umc-ubu-q0163-retirement-closure-ledger-2026-10-19` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-generate` | `claude` | `claude-fable-5-1` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0163-openai-unexpanded-parent-regression-20261019` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0163-openai-unexpanded-parent-regression-20261019` |
| `work-score` | `codex` | `gpt-6.1-sol` | `umc-ubu-q0163-retirement-closure-ledger-2026-10-19` |
| `work-score` | `claude` | `claude-fable-5-1` | `umc-ubu-q0163-retirement-closure-ledger-2026-10-19` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0163-openai-unexpanded-parent-regression-20261019` | `openai` | `codex` | yes | 90 | Mechanical validation confirms an applicable, allowlisted patch with preserved question metadata. It advances the existing U3 work with a reproducible, revision-pinned failing parser check and a bounded repair handoff. It accurately keeps the question open and distinguishes passing existing tests from incomplete acceptance gates. The contribution supplies diagnostic evidence rather than the implementation repair. | None | The regression is retained in documentation rather than an executable repository test., Parser evidence does not establish capture, manifest, durable replay, backend occupancy or blocker-retirement qualification. |
| `UBU-Q0163-openai-unexpanded-parent-regression-20261019` | `openai` | `claude` | yes | 84 | Patch applies cleanly, touches only the Q0163 section of OPEN_QUESTIONS.md, and keeps Status: Open with unchanged metadata. It follows the question's own rule to add specification only where an implementation result exposes a gap: it records a revision-pinned, reproducible failing regression showing the production wire parser admits a timed recurrence parent as a live event with no diagnostic, which directly contradicts the existing U3 row, and then specifies a bounded repair inside the already-proposed unsupported-input ticket. The scope limits are stated honestly (no Google delivery, admission, replay, backend or handoff result; no blocker removed). Deductions are for unverifiable execution claims, the slightly redundant three-place edit including an unnecessary Resolution rewording, and the low practical severity of a case that singleEvents=true should already exclude. | None | The executed-test claims (34 passing existing tests, the failing regression, rustc/cargo versions, SHA-256 pins) cannot be verified by the scorer from this prompt; the committee is trusting the author's report that the probe compiled against the pinned revision's parse_event_list signature and DesiredEvent.external_id field., Google expanded reads with singleEvents=true should not deliver recurrence parents, so the regression guards the question's own defensive U3 requirement rather than an observed production delivery; the text could be read as overstating practical severity if that context is not kept in view., The Resolution wording change from 'none supplies passing results' to 'none supplies a completed acceptance slice' is a mild and unneeded softening; the original statement remains true because the only executed check failed., The regression source lives in OPEN_QUESTIONS.md rather than the orchestrator repo, so drift between the documented probe and the pinned revision is possible until the handoff adds it to tests/calendar_wire.rs. |
| `umc-ubu-q0163-retirement-closure-ledger-2026-10-19` | `anthropic` | `codex` | yes | 78 | Mechanical validation confirms clean application, allowed-file compliance and preserved question schema. The ledger usefully connects existing requirements to evidence and accurately keeps the question open. However, its new closure and specification constraints exceed its stated indexing purpose and need correction. | Make closure depend on passing applicable requirements for the approved scope, with explicit non-applicability and residual-protection records for remaining coverage., Preserve the existing implementation-evidence gap rule; remove the stronger failing-result prerequisite and mandatory additional-slice requirement unless separately justified and approved. | The blanket requirement that every row pass does not explicitly accommodate requirements outside the reviewed scope or documented non-applicable cases., Requiring a recorded failing result before any further specification, and a separate bounded slice for every additional series-edit kind, introduces process constraints beyond a neutral evidence index., The ledger provides traceability but supplies no implementation, passing evidence, ticket filing or operator approval. |
| `umc-ubu-q0163-retirement-closure-ledger-2026-10-19` | `anthropic` | `claude` | yes | 72 | Mechanical validation confirms the patch applies, touches only OPEN_QUESTIONS.md, and preserves the single-line metadata; Status stays Open and no ids are allocated. Given that the real work (ubu-orchestrator implementation and operator records) lives outside this repo, a compact closure index that halts slice accretion and makes the closure condition explicit is a reasonable, minimal documentation contribution and is directly responsive to the question's own instruction to add specification only where a result exposes a gap. The main weaknesses are unverified fidelity of the ledger rows to UBU-D0318, a few process assertions that go slightly beyond indexing, an unverified policy term, and further growth of an oversized block. No decomposition is attempted even though the question now bundles four proposed tickets; that is defensible but a missed opportunity. | Verify each ledger row against the actual UBU-D0318 evidence bullets in DESIGN.md §1 at d11993a and adjust wording or add missing rows so the table is a faithful index rather than a paraphrase., Confirm 'Compartment policy' is an established term in the repo; otherwise cite the actual data-handling policy or drop the clause., Either cite the existing rule requiring the approved handoff to be recorded as a decision, or soften that sentence so the ledger does not add a closure requirement beyond UBU-D0318., Consider trimming the ledger prose to limit further growth of the UBU-Q0163 block. | The ledger rows are the author's paraphrase of UBU-D0318 evidence bullets; the author had no file-read access (per validation notes) and did not verify them against DESIGN.md §1 at the base commit, so a row could misstate or omit a real D0318 requirement and give a false sense of complete coverage., Adds roughly 19 lines to an already very large question block, increasing prompt-size pressure for later committee passes., Introduces process prescriptions not plainly sourced from UBU-D0318: that the approved handoff must be recorded as a new decision before closure, that manifest-retained residual blockers are a permitted end state, and that entries obey 'current Compartment policy'. If 'Compartment policy' is not a defined repo term, the reference is dangling., Content is largely a tabular restatement of what the existing Resolution already asserts, so the marginal value depends on whether future passes actually consult and update the ledger rather than appending more slices., Scoring provider 'claude' and authoring provider 'anthropic' are distinct ids in this harness but the same vendor; the aggregator should weigh this score accordingly. |

## Selected Summary

Retain a failing production-parser regression and specify the bounded repair for unexpanded recurrence parents.

## Changed Files

- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- Adds roughly 19 lines to an already very large question block, increasing prompt-size pressure for later committee passes.
- Content is largely a tabular restatement of what the existing Resolution already asserts, so the marginal value depends on whether future passes actually consult and update the ledger rather than appending more slices.
- Google expanded reads with singleEvents=true should not deliver recurrence parents, so the regression guards the question's own defensive U3 requirement rather than an observed production delivery; the text could be read as overstating practical severity if that context is not kept in view.
- Introduces process prescriptions not plainly sourced from UBU-D0318: that the approved handoff must be recorded as a new decision before closure, that manifest-retained residual blockers are a permitted end state, and that entries obey 'current Compartment policy'. If 'Compartment policy' is not a defined repo term, the reference is dangling.
- Parser evidence does not establish capture, manifest, durable replay, backend occupancy or blocker-retirement qualification.
- Requiring a recorded failing result before any further specification, and a separate bounded slice for every additional series-edit kind, introduces process constraints beyond a neutral evidence index.
- Scoring provider 'claude' and authoring provider 'anthropic' are distinct ids in this harness but the same vendor; the aggregator should weigh this score accordingly.
- The Resolution wording change from 'none supplies passing results' to 'none supplies a completed acceptance slice' is a mild and unneeded softening; the original statement remains true because the only executed check failed.
- The blanket requirement that every row pass does not explicitly accommodate requirements outside the reviewed scope or documented non-applicable cases.
- The executed-test claims (34 passing existing tests, the failing regression, rustc/cargo versions, SHA-256 pins) cannot be verified by the scorer from this prompt; the committee is trusting the author's report that the probe compiled against the pinned revision's parse_event_list signature and DesiredEvent.external_id field.
- The ledger provides traceability but supplies no implementation, passing evidence, ticket filing or operator approval.
- The ledger rows are the author's paraphrase of UBU-D0318 evidence bullets; the author had no file-read access (per validation notes) and did not verify them against DESIGN.md §1 at the base commit, so a row could misstate or omit a real D0318 requirement and give a false sense of complete coverage.
- The regression is retained in documentation rather than an executable repository test.
- The regression source lives in OPEN_QUESTIONS.md rather than the orchestrator repo, so drift between the documented probe and the pinned revision is possible until the handoff adds it to tests/calendar_wire.rs.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261019T043436Z-UBU-Q0163/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261019T043436Z-UBU-Q0163/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261019T043436Z-UBU-Q0163"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
