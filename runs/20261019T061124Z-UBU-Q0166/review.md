# Model-Committee Review

Run: `20261019T061124Z-UBU-Q0166`  
Question: `UBU-Q0166`  
Base commit: `cb5b99f5cb73b217e2b1a6620fd51ed2df6f47e6`  
Automated selection: valid
Human review required: no
Selected proposal: `ubu-q0166-openai-local-evidence-slice-20261019`

## Disagreement Flags


## Prompt Size Warning

The rendered work prompt reached or exceeded 90% of the prompt size limit. Consider tombstoning solved questions and compressing source files before the next run.

None.

## Quorum Result

- Valid automated selection: yes
- Selected score: 79.0
- Selected cross-score count: 2
- Blocked reasons: None

## Provider Attempts

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-generate` | `claude` | `claude-fable-5-1` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0166-fixture-prerequisite-ledger-20261019` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0166-fixture-prerequisite-ledger-20261019` |
| `work-score` | `codex` | `gpt-6.1-sol` | `ubu-q0166-openai-local-evidence-slice-20261019` |
| `work-score` | `claude` | `claude-fable-5-1` | `ubu-q0166-openai-local-evidence-slice-20261019` |

## Provider Successes

| Phase | Provider | Model | Target proposal |
| --- | --- | --- | --- |
| `work-generate` | `codex` | `gpt-6.1-sol` | `n/a` |
| `work-generate` | `claude` | `claude-fable-5-1` | `n/a` |
| `work-score` | `codex` | `gpt-6.1-sol` | `UBU-Q0166-fixture-prerequisite-ledger-20261019` |
| `work-score` | `claude` | `claude-fable-5-1` | `UBU-Q0166-fixture-prerequisite-ledger-20261019` |
| `work-score` | `codex` | `gpt-6.1-sol` | `ubu-q0166-openai-local-evidence-slice-20261019` |
| `work-score` | `claude` | `claude-fable-5-1` | `ubu-q0166-openai-local-evidence-slice-20261019` |

## Cross-Score Matrix

| Proposal | Author | Scorer | Valid | Score | Rationale | Required fixes | Risks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `UBU-Q0166-fixture-prerequisite-ledger-20261019` | `anthropic` | `codex` | yes | 62 | Mechanical validation confirms clean application, allowed files and preserved question schema. The ledger usefully distinguishes carrier retention from complete fidelity and keeps Q0166 Open. However, it adds restrictive scheduling and evidence-recording rules unsupported by the bounded findings, while supplying neither the outstanding host mapping nor new fixture evidence. | Describe unresolved paths as not located within the inspected scope, and permit either identifying and validating existing implementations or supplying separately reviewed changes., Remove the prohibition on recording evidence that leaves a row's disposition unchanged; preserve specimen-specific coverage and independent verification., Keep the ledger as an evidence index under D0321's existing gate, and retain identification of concrete evidence write/read/resolution and field-to-claim paths as the next investigative work. | The mandatory requirement to supply absent paths through host changes converts D0386's bounded failure to locate implementations into a stronger conclusion that implementation changes are necessary., Restricting further evidence to changes in ledger disposition can exclude useful additional specimens, independent reproductions and qualifier-fidelity coverage on already-characterized paths., The ledger largely repeats D0385's evidence index without identifying a concrete Discovery evidence path or supplying new mapping evidence. |
| `UBU-Q0166-fixture-prerequisite-ledger-20261019` | `anthropic` | `claude` | yes | 74 | The patch applies cleanly, touches only DECISIONS.md, DESIGN.md and OPEN_QUESTIONS.md, keeps Status: Open with the metadata line untouched, and makes no schema, enum, pin, migration, repair or adapter-enablement claim. It is directly responsive to the question's recorded next step: UBU-D0386 says to identify the concrete evidence write/read/resolution paths before the complete fixture run, and this proposal converts the accumulated findings into an explicit prerequisite gate with a scheduling rule and a churn-stop rule. That is a reasonable narrowing of a question that has produced nine bounded carrier probes without changing closure. It does not advance technical evidence, and its main weakness is that the ledger's cells restate specific host names without the author having been able to read the repository, so the table is not independently auditable from the patch alone. The overlap with UBU-D0385's index and the open identifier-ordering concern lower the score further. Overall a valid, minimal, docs-only proposal that is useful but requires verification of its factual cells before acceptance. | Before merge, verify every pinned path, function and schema name in the ledger table against the cited decisions' recorded text and the five UBU-D0323 revisions; correct or remove any cell that cannot be traced to a recorded result., Confirm UBU-D0388 is the next unallocated decision identifier and that no UBU-D0387 block follows UBU-D0386; reorder if needed., Add one sentence stating how the ledger relates to UBU-D0385's evidence index (complements, supersedes, or indexes into it) to avoid two competing §21.1.1 indices., Split mixed-disposition rows so each row has one disposition, moving per-specimen caveats to the representation column or to separate rows. | The ledger pins concrete file, function and schema names (for example `precondition_review::vet_result`, `advisory_service::run_advisory`, `store_advisory_candidate`, `api::advisory::queue`, `src/api/client.ts::request`, `object_type_from_str`, `tools/validate-fixtures`). The author states no file-read tool was available, so these cells were reconstructed from excerpts of earlier decisions and could misstate what UBU-D0323 through UBU-D0386 actually recorded., Overlaps with UBU-D0385, which already supplies a compact evidence index in §21.1.1. Two adjacent index-style artifacts for the same question invite drift unless their relationship is stated., The decision block is anchored directly after UBU-D0386. If UBU-D0387 exists elsewhere in DECISIONS.md, UBU-D0388 lands out of numerical order; the proposal itself flags this but does not resolve it. Mechanical validation does not confirm UBU-D0388 is the next free identifier., The scheduling rule routes all missing host paths to separately reviewed work outside this question. That is consistent with UBU-D0386, but it leaves UBU-Q0166 with no in-question next action and could stall the question indefinitely., Several table rows carry mixed dispositions in a single cell (for example `present` for string refs and `lossy` for the inline member), which weakens the ledger's intended one-disposition-per-row auditability., The 'do not record further synthetic carrier variants' rule could suppress evidence a later reviewer finds relevant; it is bounded by the 'changes a row' exception but the judgment of what changes a row is left implicit. |
| `ubu-q0166-openai-local-evidence-slice-20261019` | `openai` | `codex` | yes | 88 | Mechanical validation passes and changes remain within the allowed files. The extracted question isolates a useful prerequisite with concrete revision, fidelity, replay and policy controls. It preserves Q0166's metadata, limitations and closure criteria without claiming compatibility or a schema gap. This is sound decomposition, but does not itself deliver the mapping and runtime evidence requested by Q0166. | None | The proposal supplies planning and acceptance criteria, but no concrete host mapping, implementation or executed preservation evidence. |
| `ubu-q0166-openai-local-evidence-slice-20261019` | `openai` | `claude` | yes | 70 | Mechanical validation passes (patch applies, allowlist and question schema OK). The patch is docs-only across DECISIONS.md, DESIGN.md and OPEN_QUESTIONS.md, preserves UBU-Q0166's metadata and Status: Open, keeps Resolved by: None, uses a valid Status for the new question, and makes no schema, enum, pin or migration claim. It is directly responsive to UBU-D0386's recorded next step (identify or separately implement the evidence writer/resolver before the full fixture run) by turning that step into a trackable, bounded question with concrete acceptance evidence: named host paths with pins, a materialized two-revision fixture with candidate field-to-claim links, revision/replay controls, and invalid/unavailable-binding controls, plus explicit prohibitions on inventing kinds, mislabeling native objects, or inferring a schema gap from absent implementation. Boundaries between what UBU-Q0191 can close and what stays under UBU-Q0166 are stated clearly, and the proposal honestly reports that nothing is executed. The decomposition is therefore good. The score is held in the low 70s because the proposal advances no evidence, is partly a restatement of existing direction, understates the coupling between the two questions, and skips the Current direction update that the chain's convention expects. requires_human_review is correctly true for a human-approval-required question. | Non-blocking: add a one-paragraph UBU-D0388 entry to UBU-Q0166's 'Current direction' to match the established per-decision pattern (every prior decision from UBU-D0334 onward appears in both Current direction and Resolution; this patch updates Resolution only)., Non-blocking: state the practical relationship between UBU-Q0191 and UBU-Q0166 in metadata or text (e.g., note that UBU-Q0166's full-conformance route is gated on UBU-Q0191 even though its demonstrated-gap route is not), so the 'Depends on: None' framing is not read as independence. | Produces no new fixture, mapping or host-path evidence for UBU-Q0166; it appends a tenth 'stays Open' decision to an already long chain, so the marginal progress is organizational only., UBU-Q0191 largely restates the direction UBU-D0386 already recorded (identify writer/resolver/policy boundary, supply missing implementation via separate review); risk of duplicate tracking rather than movement., Declaring UBU-Q0191 'Depends on: None / Blocks: None' understates the practical coupling: UBU-Q0166's full-conformance closure route cannot complete without the path UBU-Q0191 is meant to produce. The emphasis on 'dependency-free' reads as tuned for loop prioritization rather than for an accurate dependency graph., Possible topical overlap with existing Discovery evidence storage questions under UBU-D0320; the proposal verifies ID uniqueness but not topic uniqueness., The DESIGN.md table is prescriptive (E1@v1/E2@v1 aliases, pool-reconnect vs crash-recovery labeling, explicit normalization bypass); the disclaimer that these are fixture aliases, not a reference grammar, must be honored by downstream implementers or the slice drifts into de facto schema design., 'Decision type: Data model' on UBU-Q0191 may mislabel what the proposal itself calls an implementation/mapping question. |

## Selected Summary

Extract a dependency-free local Discovery evidence storage/resolution work item with bounded implementation deliverables and fixture acceptance.

## Changed Files

- `DECISIONS.md`
- `DESIGN.md`
- `OPEN_QUESTIONS.md`

## Validation

- Patch applies: yes
- Patch allowlist passed: yes
- Question schema preserved: yes

## Risks

- 'Decision type: Data model' on UBU-Q0191 may mislabel what the proposal itself calls an implementation/mapping question.
- Declaring UBU-Q0191 'Depends on: None / Blocks: None' understates the practical coupling: UBU-Q0166's full-conformance closure route cannot complete without the path UBU-Q0191 is meant to produce. The emphasis on 'dependency-free' reads as tuned for loop prioritization rather than for an accurate dependency graph.
- Overlaps with UBU-D0385, which already supplies a compact evidence index in §21.1.1. Two adjacent index-style artifacts for the same question invite drift unless their relationship is stated.
- Possible topical overlap with existing Discovery evidence storage questions under UBU-D0320; the proposal verifies ID uniqueness but not topic uniqueness.
- Produces no new fixture, mapping or host-path evidence for UBU-Q0166; it appends a tenth 'stays Open' decision to an already long chain, so the marginal progress is organizational only.
- Restricting further evidence to changes in ledger disposition can exclude useful additional specimens, independent reproductions and qualifier-fidelity coverage on already-characterized paths.
- Several table rows carry mixed dispositions in a single cell (for example `present` for string refs and `lossy` for the inline member), which weakens the ledger's intended one-disposition-per-row auditability.
- The 'do not record further synthetic carrier variants' rule could suppress evidence a later reviewer finds relevant; it is bounded by the 'changes a row' exception but the judgment of what changes a row is left implicit.
- The DESIGN.md table is prescriptive (E1@v1/E2@v1 aliases, pool-reconnect vs crash-recovery labeling, explicit normalization bypass); the disclaimer that these are fixture aliases, not a reference grammar, must be honored by downstream implementers or the slice drifts into de facto schema design.
- The decision block is anchored directly after UBU-D0386. If UBU-D0387 exists elsewhere in DECISIONS.md, UBU-D0388 lands out of numerical order; the proposal itself flags this but does not resolve it. Mechanical validation does not confirm UBU-D0388 is the next free identifier.
- The ledger largely repeats D0385's evidence index without identifying a concrete Discovery evidence path or supplying new mapping evidence.
- The ledger pins concrete file, function and schema names (for example `precondition_review::vet_result`, `advisory_service::run_advisory`, `store_advisory_candidate`, `api::advisory::queue`, `src/api/client.ts::request`, `object_type_from_str`, `tools/validate-fixtures`). The author states no file-read tool was available, so these cells were reconstructed from excerpts of earlier decisions and could misstate what UBU-D0323 through UBU-D0386 actually recorded.
- The mandatory requirement to supply absent paths through host changes converts D0386's bounded failure to locate implementations into a stronger conclusion that implementation changes are necessary.
- The proposal supplies planning and acceptance criteria, but no concrete host mapping, implementation or executed preservation evidence.
- The scheduling rule routes all missing host paths to separately reviewed work outside this question. That is consistent with UBU-D0386, but it leaves UBU-Q0166 with no in-question next action and could stall the question indefinitely.
- UBU-Q0191 largely restates the direction UBU-D0386 already recorded (identify writer/resolver/policy boundary, supply missing implementation via separate review); risk of duplicate tracking rather than movement.

## Next Manual Steps

```bash
git -C ../ubu-design apply "$(pwd)/runs/20261019T061124Z-UBU-Q0166/patches/selected.patch"
git -C ../ubu-design commit -S -F "$(pwd)/runs/20261019T061124Z-UBU-Q0166/commit_message.txt"
```

## Artifact Publication

Run these commands manually, or invoke `model-committee uber-loop` to automate
publication after the signed design commit.

```bash
RUN_ID="20261019T061124Z-UBU-Q0166"

mkdir -p ../model-committee-artifacts/runs
cp -r "$(pwd)/runs/${RUN_ID}" ../model-committee-artifacts/runs/

git -C ../model-committee-artifacts add "runs/${RUN_ID}"
git -C ../model-committee-artifacts commit -S -m "UMC artifact ${RUN_ID}"
git -C ../model-committee-artifacts push
```
